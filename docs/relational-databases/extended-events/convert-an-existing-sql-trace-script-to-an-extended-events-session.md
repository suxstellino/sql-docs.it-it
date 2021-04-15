---
title: Convertire uno script di Traccia SQL in una sessione Eventi estesi
description: Utilizzare queste procedure per creare una sessione Eventi estesi equivalente a uno script di Traccia SQL esistente che si desidera convertire.
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: tutorial
helpviewer_keywords:
- SQL Trace, convert script to extended events
- extended events [SQL Server], convert SQL Trace script
ms.assetid: 4c8f29e6-0a37-490f-88b3-33493871b3f9
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4549a86841410a4aa5c68c80cd9480abf858d1d8
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492439"
---
# <a name="convert-an-existing-sql-trace-script-to-an-extended-events-session"></a>Convertire uno script di Traccia SQL esistente in una sessione Eventi estesi

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Se si dispone di uno script di Traccia SQL esistente che si desidera convertire in una sessione Eventi estesi, è possibile utilizzare le procedure descritte in questo argomento per creare una sessione Eventi estesi equivalente. Usando le informazioni contenute nelle tabelle di sistema trace_xe_action_map e trace_xe_event_map, è possibile raccogliere le informazioni necessarie per la conversione.  
  
 I passaggi necessari sono i seguenti:  
  
1.  Eseguire lo script esistente per creare una sessione di Traccia SQL, quindi ottenere l'ID della traccia.  
  
2.  Eseguire una query che usi la funzione fn_trace_geteventinfo per trovare gli eventi e le azioni equivalenti degli eventi estesi per ogni classe di evento di Traccia SQL e per le colonne associate.  
  
3.  Usare la funzione fn_trace_getfilterinfo per elencare i filtri e le azioni equivalenti degli eventi estesi da usare.  
  
4.  Creare manualmente una sessione Eventi estesi, utilizzando gli eventi, le azioni e i predicati (filtri) equivalenti degli eventi estesi.  

## <a name="to-obtain-the-trace-id"></a>Per ottenere l'ID della traccia  
  
1.  Aprire lo script di Traccia SQL nell'editor di query e quindi eseguire lo script per creare la sessione di traccia. Si noti che non è necessario che la sessione di traccia sia in esecuzione per completare questa procedura.  
  
2.  Ottenere l'ID della traccia. A tale scopo, utilizzare la query seguente:  
  
    ```sql
    SELECT * FROM sys.traces;  
    GO  
    ```  
  
    > [!NOTE]  
    >  L'ID di traccia 1 indica in genere la traccia predefinita.  
  
## <a name="to-determine-the-extended-events-equivalents"></a>Per determinare gli equivalenti degli eventi estesi  
  
1.  Per determinare gli eventi equivalenti degli eventi estesi, eseguire la query seguente, dove *trace_id* è impostato sul valore dell'ID di traccia ottenuto nella procedura descritta in precedenza.  
  
    > [!NOTE]  
    >  In questo esempio, viene utilizzato l'ID della traccia predefinita (1).  
  
    ```sql
    USE MASTER;  
    GO  
    DECLARE @trace_id int;  
    SET @trace_id = 1;  
    SELECT DISTINCT el.eventid, em.package_name, em.xe_event_name AS 'event'  
       , el.columnid, ec.xe_action_name AS 'action'  
    FROM (sys.fn_trace_geteventinfo(@trace_id) AS el  
       LEFT OUTER JOIN sys.trace_xe_event_map AS em  
          ON el.eventid = em.trace_event_id)  
    LEFT OUTER JOIN sys.trace_xe_action_map AS ec  
       ON el.columnid = ec.trace_column_id  
    WHERE em.xe_event_name IS NOT NULL AND ec.xe_action_name IS NOT NULL;  
    ```  
  
     Vengono restituiti l'ID di evento, il nome di pacchetto, il nome di evento, l'ID di colonna e il nome di azione equivalenti degli eventi estesi. Questo output verrà utilizzato nella procedura "Per creare la sessione Eventi estesi", più avanti in questo argomento.  
  
     In alcuni casi, per la colonna filtrata viene eseguito il mapping a un campo dati di evento incluso per impostazione predefinita nell'evento di eventi estesi. La colonna "Extended_Events_action_name" sarà pertanto NULL. Se questo si verifica, è necessario effettuare le operazioni seguenti per determinare il campo dati equivalente alla colonna filtrata:  
  
    1.  Per le azioni che restituiscono NULL, identificare le classi di eventi di Traccia SQL nello script che contengono la colonna filtrata.  
  
         È possibile ad esempio che sia stata usata la classe di evento SP:StmtCompleted e sia stato specificato un filtro sul nome della colonna di traccia Duration (ID classe di evento di Traccia SQL 45 e ID colonna di Traccia SQL 13). In questo caso, il nome dell'azione verrà visualizzato come NULL nei risultati della query.  
  
    2.  Per ogni classe di evento di Traccia SQL identificata nel passaggio precedente, trovare il nome dell'evento equivalente degli eventi estesi. Se non si conosce il nome dell'evento equivalente, usare la query nell'argomento [Visualizzare gli eventi estesi equivalenti alle classi di evento di traccia SQL](../../relational-databases/extended-events/view-the-extended-events-equivalents-to-sql-trace-event-classes.md).  
  
    3.  Utilizzare la query seguente per identificare i campi dati corretti da utilizzare per gli eventi identificati nel passaggio precedente. Nella query i campi dati degli eventi estesi sono riportati nella colonna "event_field". Nella query sostituire *<event_name>* con il nome di un evento specificato nel passaggio precedente.  
  
        ```sql
        SELECT xp.name package_name, xe.name event_name  
           ,xc.name event_field, xc.description  
        FROM sys.trace_xe_event_map AS em  
        INNER JOIN sys.dm_xe_objects AS xe  
           ON em.xe_event_name = xe.name  
        INNER JOIN sys.dm_xe_packages AS xp  
           ON xe.package_guid = xp.guid AND em.package_name = xp.name  
        INNER JOIN sys.dm_xe_object_columns AS xc  
           ON xe.name = xc.object_name  
        WHERE xe.object_type = 'event' AND xc.column_type <> 'readonly'  
           AND em.xe_event_name = '<event_name>';  
        ```  
  
         Ad esempio, la classe di evento SP:StmtCompleted esegue il mapping all'evento sp_statement_completed degli eventi estesi. Se si specifica sp_statement_completed come nome di evento nella query, nella colonna "event_field" vengono visualizzati i campi inclusi per impostazione predefinita nell'evento. Analizzando i campi, è possibile notare un campo relativo alla durata ("duration"). Per creare il filtro nella sessione Eventi estesi equivalente, è necessario aggiungere un predicato, ad esempio "WHERE duration > 0". Per un esempio, vedere la procedura "Per creare la sessione Eventi estesi", più avanti in questo argomento.  
  
## <a name="to-create-the-extended-events-session"></a>Per creare la sessione Eventi estesi  
 Utilizzare l'editor di query per creare la sessione Eventi estesi e scrivere l'output in una destinazione file. Nei passaggi seguenti viene descritta una singola query e viene illustrato come compilarla. Per l'esempio di query completo, vedere la sezione "Esempio" di questo argomento.  
  
1.  Aggiungere istruzioni per creare la sessione eventi, sostituendo *session_name* con il nome che si desidera utilizzare per la sessione eventi estesi.  
  
    ```sql
    IF EXISTS(SELECT * FROM sys.server_event_sessions WHERE name='session_name')  
       DROP EVENT SESSION [Session_Name] ON SERVER;  
    CREATE EVENT SESSION [Session_Name]  
    ON SERVER;  
    ```  
  
2.  Aggiungere gli eventi e le azioni degli eventi estesi restituiti come output nella procedura "Per determinare gli equivalenti degli eventi estesi" e aggiungere i predicati (filtri) identificati nella procedura "Per determinare i filtri utilizzati nello script".  
  
     Nell'esempio seguente viene usato uno script di Traccia SQL che include le classi di eventi StmtStarting e SP:StmtCompleted, con i filtri per ID sessione e durata. L'output di esempio per la query nella procedura "Per determinare gli equivalenti degli eventi estesi" ha restituito il set di risultati seguente:  
  
    ```  
    Eventid  package_name  event                   columnid  action  
    44       sqlserver     sp_statement_starting   6         nt_username  
    44       sqlserver     sp_statement_starting   9         client_pid  
    44       sqlserver     sp_statement_starting   10        client_app_name  
    44       sqlserver     sp_statement_starting   11        server_principal_name  
    44       sqlserver     sp_statement_starting   12        session_id  
    45       sqlserver     sp_statement_completed  6         nt_username  
    45       sqlserver     sp_statement_completed  9         client_pid  
    45       sqlserver     sp_statement_completed  10        client_app_name  
    45       sqlserver     sp_statement_completed  11        server_principal_name  
    45       sqlserver     sp_statement_completed  12        session_id  
    ```  
  
     Per eseguire la conversione dell'elemento nell'equivalente degli eventi estesi, vengono aggiunti gli eventi sqlserver.sp_statement_starting e sqlserver.sp_statement_completed, con un elenco di azioni. Le istruzioni dei predicati sono incluse come clausole WHERE.  
  
    ```sql
    ADD EVENT sqlserver.sp_statement_starting  
       (ACTION  
          (  
          sqlserver.nt_username,  
          sqlserver.client_pid,  
          sqlserver.client_app_name,  
          sqlserver.server_principal_name,  
          sqlserver.session_id  
          )  
       WHERE sqlserver.session_id = 59   
       ),  
  
    ADD EVENT sqlserver.sp_statement_completed  
       (ACTION  
          (  
          sqlserver.nt_username,  
          sqlserver.client_pid,  
          sqlserver.client_app_name,  
          sqlserver.server_principal_name,  
          sqlserver.session_id  
          )  
       WHERE sqlserver.session_id = 59 AND duration > 0  
       )  
    ```  
  
3.  Aggiungere la destinazione file asincrona, sostituendo i percorsi di file con il percorso in cui si desidera salvare l'output. Quando si specifica la destinazione file, è necessario includere un file di percorso del file di log e del file di metadati.  
  
    ```sql
    ADD TARGET package0.asynchronous_file_target(  
       SET filename='c:\temp\ExtendedEventsStoredProcs.xel', metadatafile='c:\temp\ExtendedEventsStoredProcs.xem');  
    ```  
  
## <a name="to-view-the-results"></a>Per visualizzare i risultati  
  
1.  È possibile usare la funzione sys.fn_xe_file_target_read_file per visualizzare l'output. A tale scopo, eseguire la query seguente, sostituendo i percorsi di file con i percorsi specificati:  
  
    ```sql
    SELECT *, CAST(event_data as XML) AS 'event_data_XML'  
    FROM sys.fn_xe_file_target_read_file('c:\temp\ExtendedEventsStoredProcs*.xel', 'c:\temp\ExtendedEventsStoredProcs*.xem', NULL, NULL);  
  
    ```  
  
    > [!NOTE]  
    >  L'esecuzione del cast dei dati degli eventi in formato XML è facoltativa.  
  
     Per altre informazioni sulla funzione sys.fn_xe_file_target_read_file, vedere [sys.fn_xe_file_target_read_file &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-xe-file-target-read-file-transact-sql.md).  
  
    ```sql
    IF EXISTS(SELECT * FROM sys.server_event_sessions WHERE name='session_name')  
       DROP EVENT SESSION [session_name] ON SERVER;  
    CREATE EVENT SESSION [session_name]  
    ON SERVER  
  
    ADD EVENT sqlserver.sp_statement_starting  
       (ACTION  
       (  
          sqlserver.nt_username,  
          sqlserver.client_pid,  
          sqlserver.client_app_name,  
          sqlserver.server_principal_name,  
          sqlserver.session_id  
       )  
       WHERE sqlserver.session_id = 59   
       ),  
  
    ADD EVENT sqlserver.sp_statement_completed  
       (ACTION  
       (  
          sqlserver.nt_username,  
          sqlserver.client_pid,  
          sqlserver.client_app_name,  
          sqlserver.server_principal_name,  
          sqlserver.session_id  
       )  
       WHERE sqlserver.session_id = 59 AND duration > 0  
       );  
  
    ADD TARGET package0.asynchronous_file_target  
       (SET filename='c:\temp\ExtendedEventsStoredProcs.xel', metadatafile='c:\temp\ExtendedEventsStoredProcs.xem');  
    ```  
  
## <a name="example"></a>Esempio  
  
```sql
IF EXISTS(SELECT * FROM sys.server_event_sessions WHERE name='session_name')  
   DROP EVENT SESSION [session_name] ON SERVER;  
CREATE EVENT SESSION [session_name]  
ON SERVER  
  
ADD EVENT sqlserver.sp_statement_starting  
   (ACTION  
   (  
      sqlserver.nt_username,  
      sqlserver.client_pid,  
      sqlserver.client_app_name,  
      sqlserver.server_principal_name,  
      sqlserver.session_id  
   )  
   WHERE sqlserver.session_id = 59   
   ),  
  
ADD EVENT sqlserver.sp_statement_completed  
   (ACTION  
   (  
      sqlserver.nt_username,  
      sqlserver.client_pid,  
      sqlserver.client_app_name,  
      sqlserver.server_principal_name,  
      sqlserver.session_id  
   )  
   WHERE sqlserver.session_id = 59 AND duration > 0  
   )  
  
ADD TARGET package0.asynchronous_file_target  
   (SET filename='c:\temp\ExtendedEventsStoredProcs.xel', metadatafile='c:\temp\ExtendedEventsStoredProcs.xem');  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Visualizzare gli eventi estesi equivalenti alle classi di eventi di Traccia SQL](../../relational-databases/extended-events/view-the-extended-events-equivalents-to-sql-trace-event-classes.md)  
  
  
