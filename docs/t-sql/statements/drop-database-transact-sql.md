---
description: DROP DATABASE (Transact-SQL)
title: DROP DATABASE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/21/2019
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP DATABASE
- DROP_DATABASE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- snapshots [SQL Server database snapshots], deleting
- removing databases
- database snapshots [SQL Server], removing
- deleting databases
- dropping databases
- databases [SQL Server], dropping
- DROP DATABASE statement
- database removal [SQL Server], DROP DATABASE statement
ms.assetid: 477396a9-92dc-43c9-9b97-42c8728ede8e
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 25fb14a69ee138195ae163f9ee90a82b0e90e3dd
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095809"
---
# <a name="drop-database-transact-sql"></a>DROP DATABASE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Rimuove uno o più database utente o snapshot di database da un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintassi

```syntaxsql
-- SQL Server Syntax
DROP DATABASE [ IF EXISTS ] { database_name | database_snapshot_name } [ ,...n ] [;]
```

```syntaxsql
-- Azure SQL Database, Azure Synapse Analytics and Analytics Platform System Syntax
DROP DATABASE database_name [;]
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

*IF EXISTS*
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] fino alla [versione corrente](https://go.microsoft.com/fwlink/p/?LinkId=299658)).

Rimuove in modo condizionale il database solo se esiste già.

*database_name* specifica il nome del database da rimuovere. Per visualizzare un elenco di database, usare la vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

*database_snapshot_name*
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.

Specifica il nome di uno snapshot del database da rimuovere.

## <a name="general-remarks"></a>Osservazioni generali

Un database può essere rimosso indipendentemente dallo stato: offline, di sola lettura, sospetto e così via. Per visualizzare lo stato corrente di un database, usare la vista del catalogo **sys.databases**.

È possibile ricreare un database rimosso solo tramite il ripristino di un backup. Non è possibile eseguire il backup degli snapshot di un database e, di conseguenza, non è possibile ripristinarli.

Dopo la rimozione di un database, è necessario eseguire il backup del [database master](../../relational-databases/databases/master-database.md).

La rimozione di un database comporta l'eliminazione del database da un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e dei relativi file su disco utilizzati. Se il database o uno dei relativi file è offline quando viene rimosso, i file su disco non vengono eliminati. Questi file possono essere eliminati manualmente tramite Esplora risorse. Per rimuovere un database dal server corrente senza eliminare i file dal file system, usare [sp_detach_db](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md).

> [!WARNING]
> Sarà possibile rimuovere un database con backup FILE_SNAPSHOT associati, ma i file del database con snapshot associati non verranno eliminati per evitare di invalidare i backup che fanno riferimento a tali file del database. Il file verrà troncato, ma non sarà eliminato fisicamente in modo da mantenere inalterati i backup FILE_SNAPSHOT. Per altre informazioni, vedere [Backup e ripristino di SQL Server con il servizio di archiviazione BLOB di Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md). **Si applica a**: da [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] alla [versione corrente](https://go.microsoft.com/fwlink/p/?LinkId=299658).

### [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

La rimozione di uno snapshot di database comporta l'eliminazione dello snapshot del database in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e dei file sparse fisici del file system NTFS utilizzati dallo snapshot. Per informazioni sull'uso di file sparse con gli snapshot del database, vedere [Snapshot del database](../../relational-databases/databases/database-snapshots-sql-server.md). La rimozione di uno snapshot del database comporta la cancellazione della cache dei piani per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La cancellazione della cache dei piani comporta la ricompilazione di tutti i piani di esecuzione successivi e può causare un peggioramento improvviso e temporaneo delle prestazioni di esecuzione delle query. Il log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contiene il messaggio informativo seguente per ogni archivio cache cancellato nella cache dei piani: "[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha rilevato %d occorrenza/e di scaricamento dell'archivio cache '%s' (parte della cache dei piani) a causa di operazioni di manutenzione o riconfigurazione del database". Questo messaggio viene registrato ogni cinque minuti per tutta la durata dello scaricamento della cache.

## <a name="interoperability"></a>Interoperabilità

### <a name="sql-server"></a>SQL Server

Per rimuovere un database pubblicato per la replica transazionale, o pubblicato o sottoscritto per la replica di tipo merge, è necessario innanzitutto rimuovere la replica dal database. Se un database è danneggiato, se non è possibile rimuovere prima la replica o se si verificano entrambe le situazioni, nella maggior parte dei casi è comunque possibile eliminare il database tramite ALTER DATABASE per impostarlo offline e, successivamente, rimuoverlo.

Se il database è coinvolto nel log shipping, rimuovere quest'ultimo prima di eliminare il database. Per altre informazioni, vedere [Informazioni sul log shipping](../../database-engine/log-shipping/about-log-shipping-sql-server.md).

## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni

I [database di sistema](../../relational-databases/databases/system-databases.md) non possono essere eliminati.

L'istruzione DROP DATABASE deve essere eseguita in modalità autocommit e non è consentita in una transazione esplicita o implicita. La modalità autocommit è la modalità predefinita per la gestione delle transazioni.

Non è possibile rimuovere un database in uso, ovvero aperto per la lettura o la scrittura da parte di un utente. Per rimuovere gli utenti dal database, usare ALTER DATABASE per impostare il database su SINGLE_USER.

> [!WARNING]
> Questo non è un approccio a prova di errore, poiché la prima connessione consecutiva eseguita da qualsiasi thread riceverà il thread SINGLE_USER, causando un errore di connessione. SQL Server non offre un metodo incorporato per eliminare i database sotto carico.

### [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Prima di poter rimuovere un database, è necessario eliminare tutti gli snapshot del database.

L'eliminazione di un database abilitato per Stretch Database non determina la rimozione dei dati remoti. Per eliminare i dati remoti, è necessario rimuoverli manualmente.

### [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

È necessario essere connessi al database master per eliminare un database.

 L'istruzione DROP DATABASE deve essere l'unica istruzione in un batch SQL ed è possibile eliminare un solo database alla volta.

### [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]

È necessario essere connessi al database master per eliminare un database.

L'istruzione DROP DATABASE deve essere l'unica istruzione in un batch SQL ed è possibile eliminare un solo database alla volta.

## <a name="permissions"></a>Autorizzazioni

### [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

È richiesta l'autorizzazione **CONTROL** per il database, l'autorizzazione **ALTER ANY DATABASE** o l'appartenenza al ruolo predefinito del database **db_owner**.

### [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Solo l'account di accesso dell'entità di livello server (creato dal processo di provisioning) o i membri del ruolo del database **dbmanager** possono eliminare un database.

### [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

È richiesta l'autorizzazione **CONTROL** per il database, l'autorizzazione **ALTER ANY DATABASE** o l'appartenenza al ruolo predefinito del database **db_owner**.

## <a name="examples"></a>Esempi

### <a name="a-dropping-a-single-database"></a>R. Rimozione di un singolo database

Nell'esempio seguente viene rimosso il database `Sales`.

```sql
DROP DATABASE Sales;
```

### <a name="b-dropping-multiple-databases"></a>B. Rimozione di più database

**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.

Nell'esempio seguente vengono rimossi tutti i database elencati.

```sql
DROP DATABASE Sales, NewSales;
```

### <a name="c-dropping-a-database-snapshot"></a>C. Eliminazione di uno snapshot del database

**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.

Nell'esempio seguente viene rimosso uno snapshot di database, denominato `sales_snapshot0600`, senza influire sul database di origine.

```sql
DROP DATABASE sales_snapshot0600;
```

## <a name="see-also"></a>Vedere anche

- [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
