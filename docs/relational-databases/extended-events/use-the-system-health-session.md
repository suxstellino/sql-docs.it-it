---
title: Utilizzare la sessione system_health
description: La sessione di eventi estesi system_health è inclusa in SQL Server. Questa sessione raccoglie i dati di sistema per risolvere i problemi di prestazioni del motore di database.
ms.date: 11/27/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: tutorial
helpviewer_keywords:
- extended events [SQL Server], system health session
- extended events [SQL Server], system_health session
- system_health session [SQL Server extended events]
- system health session [SQL Server extended events]
ms.assetid: 1e1fad43-d747-4775-ac0d-c50648e56d78
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f157320744b80593264225be72d42d6f08ba398f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481322"
---
# <a name="use-the-system_health-session"></a>Utilizzare la sessione system_health

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

La sessione system_health è una sessione di eventi estesi inclusa per impostazione predefinita in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questa sessione viene avviata automaticamente all'avvio del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] e viene eseguita senza effetti rilevanti sulle prestazioni. La sessione raccoglie dati del sistema che consentono di semplificare la risoluzione dei problemi relativi alle prestazioni nel [!INCLUDE[ssDE](../../includes/ssde-md.md)]. 

> [!IMPORTANT]
> È consigliabile non arrestare, modificare o eliminare la sessione system_health. Le eventuali modifiche apportate alle impostazioni della sessione system_health potrebbero essere sovrascritte da un aggiornamento futuro del prodotto.
  
La sessione raccoglie le informazioni seguenti:  
  
-   *Sql_text* e *session_id* per qualsiasi sessione in cui si verifica un errore con gravità >=20.  
  
-   *Sql_text* e *session_id* per qualsiasi sessione in cui si verifica un errore relativo alla memoria. ad esempio gli errori 17803, 701, 802, 8645, 8651, 8657 e 8902.  
  
-   Un record di qualsiasi problema dell'utilità di pianificazione relativo alla mancata cessione del controllo. Questi problemi sono inclusi come errori 17883 nel log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Qualsiasi deadlock rilevato, tra cui l'evento Deadlock Graph.  
  
-   *Callstack*, *sql_text* e *session_id* per qualsiasi sessione in attesa su latch (o altre risorse interessanti) per un tempo maggiore di 15 secondi.  
  
-   *Callstack*, *sql_text* e *session_id* per qualsiasi sessione in attesa su blocchi per un tempo maggiore di 30 secondi.  
  
-   *Callstack*, *sql_text* e *session_id* per qualsiasi sessione rimasta in attesa per molto tempo a causa di attese preemptive. La durata varia in base al tipo di attesa. In un'attesa preemptive [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in attesa di chiamate API esterne.  
  
-   *Callstack* e *session_id* per l'allocazione CLR e per errori di allocazione virtuali.  
  
-   Eventi del buffer circolare per il broker di memoria, il monitoraggio dell'utilità di pianificazione, la memoria insufficiente del nodo di memoria, la sicurezza e la connettività.  
  
-   Risultati del componente di sistema da `sp_server_diagnostics`.  
  
-   Integrità dell'istanza raccolta da *scheduler_monitor_system_health_ring_buffer_recorded*.  
  
-   Errori di allocazione CLR.  
  
-   Errori di connettività usando *connectivity_ring_buffer_recorded*.  
  
-   Errori di sicurezza usando *security_error_ring_buffer_recorded*.  

> [!NOTE]
> Per altre informazioni, vedere [Uso di deadlock in Guida per il controllo delle versioni delle righe e il blocco della transazione](../../relational-databases/sql-server-transaction-locking-and-row-versioning-guide.md#deadlocks).   
> Per altre informazioni sui messaggi di errore SQL, vedere [Errori del motore di database](../../relational-databases/errors-events/database-engine-events-and-errors.md).

## <a name="viewing-the-session-data"></a>Visualizzazione dei dati della sessione  
Nella sessione viene usata la destinazione del buffer circolare e la destinazione del file degli eventi per archiviare i dati. Per la destinazione del file degli eventi vengono configurati 5 MB di dimensioni massime e 4 file come criteri di conservazione dei file. 

Per visualizzare i dati della sessione dalla destinazione del buffer circolare usando l'interfaccia utente degli eventi estesi disponibile in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], vedere [Visualizzazione avanzata dei dati di destinazione da eventi estesi in SQL Server (casella Controlla dati dinamici)](../../relational-databases/extended-events/advanced-viewing-of-target-data-from-extended-events-in-sql-server.md#b3-watch-live-data).

Per visualizzare i dati della sessione dalla destinazione del buffer circolare con [!INCLUDE[tsql](../../includes/tsql-md.md)], usare la query seguente:  
  
```sql  
SELECT CAST(xet.target_data as xml) 
FROM sys.dm_xe_session_targets xet  
JOIN sys.dm_xe_sessions xe ON (xe.address = xet.event_session_address)  
WHERE xe.name = 'system_health'  
```  
  
Per visualizzare i dati della sessione dal file degli eventi, usare l'interfaccia utente degli eventi estesi disponibile in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Per altre informazioni, vedere [Visualizzazione avanzata dei dati di destinazione da eventi estesi in SQL Server](../../relational-databases/extended-events/advanced-viewing-of-target-data-from-extended-events-in-sql-server.md).
  
## <a name="restoring-the-system_health-session"></a>Ripristino della sessione system_health  
Se si elimina la sessione system_health, è possibile ripristinarla eseguendo il file **u_tables.sql** nell'editor di query. Questo file si trova nella cartella seguente, dove **C:** rappresenta l'unità in cui sono stati installati i file di programma di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e **MSSQL1x** è la versione principale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
 `C:\Program Files\Microsoft SQL Server\MSSQL1x.\<*instanceid*>\MSSQL\Install`  
  
Tenere presente che dopo aver ripristinato la sessione, è necessario avviarla tramite l'istruzione `ALTER EVENT SESSION` o tramite il nodo **Eventi estesi** in Esplora oggetti. In caso contrario, la sessione verrà avviata al successivo riavvio del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="see-also"></a>Vedere anche  
 [Eventi estesi](../../relational-databases/extended-events/extended-events.md)    
 [Strumenti degli eventi estesi](../../relational-databases/extended-events/extended-events-tools.md)    
 [Errori del motore di database](../../relational-databases/errors-events/database-engine-events-and-errors.md)    
 [Viste del catalogo per i messaggi (di errore) - Sys.messages](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md) 
