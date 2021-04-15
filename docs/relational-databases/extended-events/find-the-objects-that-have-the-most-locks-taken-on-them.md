---
title: Cercare gli oggetti con il maggior numero di blocchi usando gli eventi estesi
description: Questo articolo illustra come trovare gli oggetti con il maggior numero di blocchi. Gli amministratori di database potrebbero dover trovare la maggior parte degli oggetti bloccati per migliorare le prestazioni del database.
ms.date: 10/18/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: tutorial
helpviewer_keywords:
- objects [SQL Server], extended events
- xe
- extended events [SQL Server], locks
- objects [SQL Server], locks
ms.assetid: fcbadbda-c91c-43f0-a1b5-601e40110e07
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ec4ecbd7df48a0f182559cbd6e7a0538c302d865
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490040"
---
# <a name="find-the-objects-that-have-the-most-locks-taken-on-them"></a>Cercare gli oggetti con il maggior numero di blocchi acquisiti

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Gli amministratori di database hanno spesso la necessità di individuare l'origine dei blocchi che hanno effetti negativi sulle prestazioni del database.  
  
Si supponga ad esempio di eseguire il monitoraggio del server di produzione per individuare eventuali colli di bottiglia. Si ritiene che potrebbero essere presenti risorse contese e si desidera conoscere il numero di blocchi acquisiti su tali oggetti. Una volta identificati gli oggetti bloccati con maggiore frequenza, è possibile effettuare i passaggi che consentono di ottimizzare l'accesso agli oggetti contesi.  
  
A questo scopo, utilizzare l'editor di query in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
### <a name="to-find-the-objects-that-have-the-most-locks"></a>Per cercare gli oggetti con il maggior numero di blocchi  
  
1. Nell'editor di query eseguire le istruzioni indicate di seguito.

    ```sql
    -- Find objects in a particular database that have the most
    -- lock acquired. This sample uses AdventureWorksDW2012.
    -- Create the session and add an event and target.
    
    IF EXISTS(SELECT * FROM sys.server_event_sessions WHERE name='LockCounts')
        DROP EVENT session LockCounts ON SERVER;
    GO
    DECLARE @dbid int;
  
    SELECT @dbid = db_id('AdventureWorksDW2012');
  
    DECLARE @sql nvarchar(1024);
    SET @sql = '
        CREATE event session LockCounts ON SERVER
            ADD EVENT sqlserver.lock_acquired (WHERE database_id ='
                + CAST(@dbid AS nvarchar) +')
            ADD TARGET package0.histogram(
                SET filtering_event_name=''sqlserver.lock_acquired'',
                    source_type=0, source=''resource_0'')';
  
    EXEC (@sql);
    GO
    ALTER EVENT session LockCounts ON SERVER
        STATE=start;
    GO
    -- Create a simple workload that takes locks.
    
    USE AdventureWorksDW2012;
    GO
    SELECT TOP 1 * FROM dbo.vAssocSeqLineItems;
    GO
    -- The histogram target output is available from the
    -- sys.dm_xe_session_targets dynamic management view in
    -- XML format.
    -- The following query joins the bucketizing target output with
    -- sys.objects to obtain the object names.
    
    SELECT name, object_id, lock_count
        FROM
        (
        SELECT objstats.value('.','bigint') AS lobject_id,
            objstats.value('@count', 'bigint') AS lock_count
            FROM (
                SELECT CAST(xest.target_data AS XML)
                    LockData
                FROM     sys.dm_xe_session_targets xest
                    JOIN sys.dm_xe_sessions        xes  ON xes.address = xest.event_session_address
                    JOIN sys.server_event_sessions ses  ON xes.name    = ses.name
                WHERE xest.target_name = 'histogram' AND xes.name = 'LockCounts'
                 ) Locks
            CROSS APPLY LockData.nodes('//HistogramTarget/Slot') AS T(objstats)
        ) LockedObjects
        INNER JOIN sys.objects o  ON LockedObjects.lobject_id = o.object_id
        WHERE o.type != 'S' AND o.type = 'U'
        ORDER BY lock_count desc;
    GO
    
    -- Stop the event session.
    
    ALTER EVENT SESSION LockCounts ON SERVER
        state=stop;
    GO
    ```

> [!NOTE]
> L'esempio di codice Transact-SQL precedente viene eseguito in SQL Server locale, ma potrebbe _non essere eseguito in un database SQL di Azure_. Le parti principali dell'esempio che coinvolgono direttamente gli eventi, ad esempio `ADD EVENT sqlserver.lock_acquired`, funzionano anche nel database SQL di Azure. Tuttavia, gli elementi preliminari, ad esempio `sys.server_event_sessions`, devono essere modificati nelle relative controparti del database SQL di Azure, ad esempio `sys.database_event_sessions` per l'esecuzione dell'esempio.
> Per altre informazioni su queste differenze minime tra SQL Server locale e il database SQL di Azure, vedere gli articoli seguenti:
> - [Eventi estesi nel database SQL di Azure](/azure/sql-database/sql-database-xevent-db-diff-from-svr#transact-sql-differences)
> - [Oggetti di sistema che supportano eventi estesi](xevents-references-system-objects.md)

Al termine delle istruzioni nello script Transact-SQL precedente, nella scheda **Risultati** dell'editor di query vengono visualizzate le colonne seguenti:
  
- name
- object_id
- lock_count
  
## <a name="see-also"></a>Vedere anche

[CREATE EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/create-event-session-transact-sql.md)  
[ALTER EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-event-session-transact-sql.md)  
[sys.dm_xe_session_targets &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-session-targets-transact-sql.md)  
[sys.dm_xe_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-sessions-transact-sql.md)  
[sys.server_event_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-event-sessions-transact-sql.md)  
