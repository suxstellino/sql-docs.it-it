---
title: Statistiche per tabelle con ottimizzazione per la memoria | Microsoft Docs
description: Informazioni sull'uso da parte di Query Optimizer delle statistiche sulle colonne nelle tabelle ottimizzate per la memoria per creare piani di query che consentono di migliorare le prestazioni delle query per OLTP in memoria.
ms.custom: ''
ms.date: 10/23/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: e644766d-1d1c-43d7-83ff-8ccfe4f3af9f
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0839c2845df3c47af084be7553d071eac9f0b10a
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492487"
---
# <a name="statistics-for-memory-optimized-tables"></a>Statistiche per tabelle con ottimizzazione per la memoria
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  In Query Optimizer vengono utilizzate le statistiche sulle colonne per creare piani di query che consentono di migliorare le prestazioni di esecuzione delle query. Le statistiche vengono raccolte dalle tabelle del database e archiviate nei metadati del database.  
  
 Le statistiche vengono create automaticamente, ma possono essere create anche manualmente. Ad esempio, le statistiche vengono create automaticamente per le colonne chiave dell'indice alla creazione dell'indice. Per altre informazioni sulla creazione di statistiche, vedere [Statistiche](../../relational-databases/statistics/statistics.md).  
  
 I dati della tabella in genere cambiano nel corso del tempo quando le righe vengono inserite, aggiornate ed eliminate. Ciò significa che le statistiche devono essere periodicamente aggiornate. Per impostazione predefinita, le statistiche sulle tabelle vengono aggiornate automaticamente quando Query Optimizer determina che potrebbero non essere aggiornate.  
  
 Considerazioni sulle statistiche delle tabelle ottimizzate per la memoria:  
  
-   A partire da SQL Server 2016 e in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], è supportato l'aggiornamento automatico delle statistiche per le tabelle ottimizzate per la memoria, quando si usa un livello di compatibilità del database di almeno 130. Vedere [Livello di compatibilità ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md). Se un database include tabelle create in precedenza con un livello di compatibilità inferiore, è necessario aggiornare manualmente le statistiche una volta, per consentirne l'aggiornamento automatico per il futuro.
  
-   Per le stored procedure compilate in modo nativo, i piani di esecuzione delle query nella procedura vengono ottimizzati quando la procedura viene compilata, ovvero al momento della creazione. Le procedure non vengono automaticamente ricompilate quando si aggiornano le statistiche. Pertanto, le tabelle devono contenere un set di dati rappresentativo prima della creazione delle procedure.  
  
-   Le stored procedure compilate in modo nativo possono essere ricompilate manualmente tramite [sp_recompile (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-recompile-transact-sql.md)e vengono ricompilate automaticamente se il database viene portato offline e poi online, se si verifica un failover del database o se il server viene riavviato.  
  
## <a name="enabling-automatic-update-of-statistics-in-existing-tables"></a>Abilitazione dell'aggiornamento automatico delle statistiche nelle tabelle esistenti

Quando le tabelle vengono create in un database con livello di compatibilità di almeno 130, si abilita l'aggiornamento automatico di tutte le statistiche su quella tabella e non è richiesta alcuna azione aggiuntiva.

Se un database include tabelle ottimizzate per la memoria create in una versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o con livello di compatibilità inferiore a 130, è necessario aggiornare manualmente le statistiche una volta per consentirne l'aggiornamento automatico per il futuro.

Per abilitare l'aggiornamento automatico delle statistiche per le tabelle ottimizzate per la memoria create con un livello di compatibilità meno recente, seguire questa procedura:

1. Aggiornare il livello di compatibilità del database: `ALTER DATABASE CURRENT SET COMPATIBILITY_LEVEL=130`

2. Aggiornare manualmente le statistiche per le tabelle ottimizzate per la memoria. Di seguito è riportato un esempio di script per tale operazione.

3. Ricompilare manualmente le stored procedure compilate in modo nativo per trarre vantaggio dalle statistiche aggiornate.

*Script a singola occorrenza per le statistiche:* per le tabelle ottimizzate per la memoria create con un livello di compatibilità inferiore, è possibile eseguire lo script Transact-SQL seguente una volta per aggiornare le statistiche delle tabelle ottimizzate per la memoria e abilitare l'aggiornamento automatico delle statistiche per il futuro (presupponendo che AUTO_UPDATE_STATISTICS sia abilitato per il database):

```
-- Assuming AUTO_UPDATE_STATISTICS is already ON for your database:
-- ALTER DATABASE CURRENT SET AUTO_UPDATE_STATISTICS ON;

ALTER DATABASE CURRENT SET COMPATIBILITY_LEVEL = 130;
GO
DECLARE @sql NVARCHAR(MAX) = N'';
SELECT
      @sql += N'UPDATE STATISTICS '
         + quotename(schema_name(t.schema_id))
         + N'.'
         + quotename(t.name)
         + ';' + CHAR(13) + CHAR(10)
   FROM sys.tables AS t
   WHERE t.is_memory_optimized = 1 AND 
        t.object_id IN (SELECT object_id FROM sys.stats WHERE no_recompute=1)
;
EXECUTE sp_executesql @sql;
GO
-- Each row appended to @sql looks roughly like:
-- UPDATE STATISTICS [dbo].[MyMemoryOptimizedTable];
```

*Verificare che l'aggiornamento automatico sia abilitato:* lo script seguente consente di verificare se l'aggiornamento automatico è abilitato per le statistiche sulle tabelle ottimizzate per la memoria. Dopo aver eseguito lo script precedente restituirà `1` nella colonna `auto-update enabled` per tutti gli oggetti statistiche.

```
SELECT 
    quotename(schema_name(o.schema_id)) + N'.' + quotename(o.name) AS [table],
    s.name AS [statistics object],
    1-s.no_recompute AS [auto-update enabled]
FROM sys.stats s JOIN sys.tables o ON s.object_id=o.object_id
WHERE o.is_memory_optimized=1
```

## <a name="guidelines-for-deploying-tables-and-procedures"></a>Linee guida per la distribuzione di tabelle e procedure  
 Per garantire che le statistiche in Query Optimizer risultino aggiornate durante la creazione dei piani di query, distribuire tabelle ottimizzate per la memoria e stored procedure compilate in modo nativo che accedono a tali tabelle usando i seguenti quattro passaggi:  
  
1.  Verificare che il livello di compatibilità del database sia almeno 130. Vedere [Livello di compatibilità ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).

2.  Creare tabelle e indici. Gli indici devono essere specificati inline nelle istruzioni **CREATE TABLE** .  
  
3.  Caricare i dati nelle tabelle.  
  
4.  Creare stored procedure che accedono alle tabelle.  
  
 La creazione delle stored procedure compilate in modo nativo dopo il caricamento dei dati garantisce la presenza delle statistiche disponibili per le tabelle ottimizzate per la memoria in Query Optimizer. In questo modo si ottengono piani di query efficienti quando la procedura viene compilata.  

## <a name="see-also"></a>Vedere anche  
 [Tabelle ottimizzate per la memoria](./sample-database-for-in-memory-oltp.md)  
  
