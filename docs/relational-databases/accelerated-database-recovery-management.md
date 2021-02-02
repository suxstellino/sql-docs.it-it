---
description: Gestire il ripristino accelerato del database
title: Gestire il ripristino accelerato del database | Microsoft Docs
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- accelerated database recovery [SQL Server], recovery-only
- database recovery [SQL Server]
author: mashamsft
ms.author: mathoma
ms.reviewer: kfarlee
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: cfd5a901f38dacf9e17baff4d65363796ab3cd73
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236105"
---
# <a name="manage-accelerated-database-recovery"></a>Gestire il ripristino accelerato del database

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver2019.md)]

## <a name="enabling-and-controlling-adr"></a>Abilitazione e controllo del ripristino accelerato del database

La funzionalità di ripristino accelerato del database (ADR, Accelerated Database Recovery) è disattivata per impostazione predefinita in [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] e può essere controllata usando la sintassi DDL:
```sql
ALTER DATABASE [DB] SET ACCELERATED_DATABASE_RECOVERY = {ON | OFF}
[(PERSISTENT_VERSION_STORE_FILEGROUP = { filegroup name }) ];

```

Usare questa sintassi per controllare se la funzionalità è attivata o disattivata e designare un filegroup specifico per i dati dell'*archivio versioni permanente* (PSV, Persistent Version Store). Se non è specificato alcun filegroup, per l'archivio versioni permanente viene usato il filegroup PRIMARY.

## <a name="managing-the-persistent-version-store-filegroup"></a>Gestione del filegroup dell'archivio versioni permanente
La funzionalità di ripristino accelerato del database si basa sull'assegnazione di versioni alle modifiche, con versioni diverse di un elemento dati conservate nell'archivio versioni permanente.
Ci sono alcune considerazioni in merito all'individuazione della posizione dell'archivio versioni permanente e a come gestire le dimensioni dei dati in tale archivio.

### <a name="to-enable-adr-without-specifying-a-filegroup"></a>Per abilitare il ripristino accelerato del database senza specificare un filegroup

```sql
ALTER DATABASE [MyDatabase] SET ACCELERATED_DATABASE_RECOVERY = ON;
GO
```

In questo caso, se il filegroup dell'archivio versioni permanente non viene specificato, i dati dell'archivio vengono inseriti nel filegroup `PRIMARY`.

### <a name="to-enable-adr-and-specify-that-the-pvs-should-be-stored-in-the-versionstorefg-filegroup"></a>Per abilitare il ripristino accelerato del database e specificare che l'archivio versioni permanente deve essere archiviato nel filegroup [VersionStoreFG]

Prima di eseguire lo script, creare il filegroup.

```sql
ALTER DATABASE [MyDatabase] SET ACCELERATED_DATABASE_RECOVERY = ON
(PERSISTENT_VERSION_STORE_FILEGROUP = [VersionStoreFG])
```

### <a name="to-disable-the-adr-feature"></a>Per disabilitare la funzionalità di ripristino accelerato del database

```sql
ALTER DATABASE [MyDatabase] SET ACCELERATED_DATABASE_RECOVERY = OFF;
GO
```

Anche dopo aver disabilitato la funzionalità di ripristino accelerato del database, saranno presenti versioni archiviate nell'archivio versioni permanente ancora necessarie al sistema per il ripristino logico.

### <a name="change-the-location-of-the-pvs-to-a-different-filegroup"></a>Modificare la posizione dell'archivio versioni permanente in un filegroup diverso

Può essere necessario spostare l'archivio versioni permanente in un filegroup diverso per vari motivi. Un archivio versioni permanente potrebbe ad esempio richiedere più spazio o una maggiore velocità di archiviazione.

Il processo di modifica della posizione dell'archivio versioni permanente prevede tre passaggi.

1. Disattivare la funzionalità di ripristino accelerato del database.

   ```sql
   ALTER DATABASE [MyDatabase] SET ACCELERATED_DATABASE_RECOVERY = OFF;
   GO
   ```

2. Attendere che tutte le versioni archiviate nell'archivio versioni permanente possano essere liberate

   Per poter attivare il ripristino accelerato del database con una nuova posizione per l'archivio versioni permanente, è necessario prima di tutto assicurarsi che tutte le informazioni sulle versioni siano state eliminate dalla posizione dell'archivio versioni permanente precedente. Per forzare tale pulizia, eseguire il comando:

   ```sql
   EXEC sys.sp_persistent_version_cleanup [database name]
   ```

   La stored procedure `sys.sp_persistent_version_cleanup` è sincrona, ovvero non verrà completata fino a quando non vengono eliminate tutte le informazioni sulle versioni dall'archivio versioni permanente corrente.  Dopo il completamento, è possibile verificare che le informazioni sulle versioni siano state effettivamente rimosse eseguendo una query sulla DMV `sys.dm_persistent_version_store_stats` ed esaminando il valore di `persistent_version_store_size_kb`.

   ```sql
   SELECT DB_Name(database_id), persistent_version_store_size_kb 
   FROM sys.dm_tran_persistent_version_store_stats where database_id = [MyDatabaseID]
   ```

   Quando il valore di persistent_version_store_size_kb è 0, è possibile riabilitare la funzionalità di ripristino accelerato del database, configurando l'archivio versioni permanente nel nuovo filegroup.

1. Attivare il ripristino accelerato del database specificando la nuova posizione per l'archivio versioni permanente

   ```sql
   ALTER DATABASE [MyDatabase] SET ACCELERATED_DATABASE_RECOVERY = ON
   (PERSISTENT_VERSION_STORE_FILEGROUP = [VersionStoreFG])
   ```

## <a name="troubleshooting"></a>Risoluzione dei problemi

> [!NOTE]
> Questa sezione si applica anche al Database SQL di Azure.

Eseguire una query su `sys.dm_tran_persistent_version_store_stats` per controllare le dimensioni dell'archivio versioni permanente.

Controllare le dimensioni di `% of DB`. Osservare anche la differenza rispetto alle dimensioni tipiche.

L'archivio versioni permanente è considerato grande se le sue dimensioni sono significativamente maggiori rispetto a quelle di base o se è prossimo al 50% delle dimensioni del database. 

1. Recuperare `oldest_active_transaction_id` e verificare se la transazione è stata attiva per un tempo molto lungo eseguendo una query su `sys.dm_tran_database_transactions` in base all'ID della transazione.

   Le transazioni attive impediscono la pulizia dell'archivio versioni permanente.

1. Se il database fa parte di un gruppo di disponibilità, controllare `secondary_low_water_mark`. Questo valore corrisponde a quello di `low_water_mark_for_ghosts` restituito da `sys.dm_hadr_database_replica_states`. Eseguire una query su `sys.dm_hadr_database_replica_states` per verificare se una delle repliche conserva questo valore perché anche in questo caso viene impedita la pulizia dell'archivio versioni permanente.
1. Controllare `min_transaction_timestamp` (o `online_index_min_transaction_timestamp` se l'archivio versioni permanente online impedisce l'operazione) e quindi controllare in `sys.dm_tran_active_snapshot_database_transactions` la colonna `transaction_sequence_num` per individuare la sessione con la transazione snapshot precedente che impedisce la pulizia dell'archivio versioni permanente.
1. Se non si applica alcuna delle opzioni precedenti, significa che la pulizia viene impedita da transazioni interrotte. Controllare gli orari di `aborted_version_cleaner_last_start_time` e `aborted_version_cleaner_last_end_time` più recenti per verificare se la pulizia della transazione interrotta è stata completata. Il valore di `oldest_aborted_transaction_id` aumenta dopo il completamento della pulizia della transazione interrotta.
1. Se la transazione interrotta non è stata completata di recente, controllare se nel log degli errori sono presenti messaggi che segnalano problemi di tipo `VersionCleaner`.
