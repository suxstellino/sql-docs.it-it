---
title: Monitorare gli script con eventi estesi
description: Informazioni su come usare gli eventi estesi per monitorare e risolvere i problemi delle operazioni correlate agli script esterni di processi di Machine Learning Services per SQL Server, Launchpad di SQL Server e Python o R.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/04/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: d8324e49ce4de61096880275a07df5a20e288a93
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337159"
---
# <a name="monitor-python-and-r-scripts-with-extended-events-in-sql-server-machine-learning-services"></a>Monitorare gli script Python e R con eventi estesi in Machine Learning Services per SQL Server
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Informazioni su come usare gli eventi estesi per monitorare e risolvere i problemi delle operazioni correlate agli script esterni di processi di Machine Learning Services per SQL Server, Launchpad di SQL Server e Python o R.

## <a name="extended-events-for-sql-server-machine-learning-services"></a>Eventi estesi per Machine Learning Services per SQL Server

Per visualizzare un elenco di eventi correlati a Machine Learning Services per SQL Server, eseguire la query seguente da Azure Data Studio o da SQL Server Management Studio.

```sql
SELECT o.name AS event_name, o.description
FROM sys.dm_xe_objects o
JOIN sys.dm_xe_packages p
ON o.package_guid = p.guid
WHERE o.object_type = 'event'
AND p.name = 'SQLSatellite';
```

Per altre informazioni su come usare gli eventi estesi, vedere [Strumenti degli eventi estesi](../../relational-databases/extended-events/extended-events-tools.md).

## <a name="additional-events-specific-to-machine-learning-services"></a>Eventi aggiuntivi specifici di Machine Learning Services

Sono disponibili altri eventi estesi per i componenti correlati a Machine Learning Services per SQL Server e usati da tale funzionalità, ad esempio [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)] e BXLServer, il processo satellite che avvia il runtime di Python o R. Questi eventi estesi aggiuntivi vengono attivati dai processi esterni e devono quindi essere acquisiti usando un'utilità esterna.

Per altre informazioni su come eseguire questa operazione, vedere [Raccolta di eventi da processi esterni](#bkmk_externalevents).

<a name="bkmk_xeventtable"></a> 

## <a name="table-of-extended-events"></a>Tabella degli eventi estesi

|Event|Descrizione|Note|  
|-----------|-----------------|---------|  
|connection_accept|Si verifica quando viene accettata una nuova connessione. Questo evento viene usato per registrare tutti i tentativi di connessione.||  
|failed_launching|Avvio non riuscito.|Indica un errore.|  
|satellite_abort_connection|Record di interruzione della connessione.||  
|satellite_abort_received|Generato quando viene ricevuto un messaggio di interruzione su una connessione satellite.||  
|satellite_abort_sent|Generato quando viene inviato un messaggio di interruzione su una connessione satellite.||  
|satellite_authentication_completion|Generato quando viene completata l'autenticazione per una connessione su TCP o Named pipe.||  
|satellite_authorization_completion|Generato quando viene completata l'autorizzazione per una connessione su TCP o Named pipe.||  
|satellite_cleanup|Generato quando il satellite chiama la pulizia.|Generato solo da processi esterni. Vedere le istruzioni sulla raccolta di eventi da processi esterni.|  
|satellite_data_chunk_sent|Generato quando la connessione satellite completa l'invio di un singolo blocco di dati.|L'evento indica il numero di righe inviate, il numero di colonne, il numero di pacchetti SNI usati e il tempo trascorso in millisecondi per l'invio del blocco. Le informazioni possono aiutare a determinare il tempo necessario per passare tipi di dati diversi e il numero di pacchetti usati.|  
|satellite_data_receive_completion|Generato quando vengono ricevuti tutti i dati richiesti da una query sulla connessione satellite.|Generato solo da processi esterni. Vedere le istruzioni sulla raccolta di eventi da processi esterni.|  
|satellite_data_send_completion|Generato quando vengono ricevuti tutti i dati richiesti per una sessione sulla connessione satellite.||  
|satellite_data_send_start|Generato quando si avvia la trasmissione dei dati.| La trasmissione dei dati si avvia poco prima dell'invio del primo blocco di dati.|  
|satellite_error|Usato per tenere traccia degli errori del satellite di sql.||  
|satellite_invalid_sized_message|La dimensione del messaggio non è valida.||  
|satellite_message_coalesced|Usato per tenere traccia dei messaggi che si uniscono a livello di rete.||  
|satellite_message_ring_buffer_record|Record del buffer circolare del messaggio.||  
|satellite_message_summary|Informazioni di riepilogo sui messaggi.||  
|satellite_message_version_mismatch|Il campo della versione del messaggio non corrisponde.||  
|satellite_messaging|Usato per tenere traccia degli eventi di messaggistica (associazione, annullamento dell'associazione e così via)||  
|satellite_partial_message|Usato per tenere traccia dei messaggi parziali a livello di rete.||  
|satellite_schema_received|Generato quando il messaggio dello schema viene ricevuto e letto da SQL.||  
|satellite_schema_sent|Generato quando il messaggio dello schema viene inviato tramite il satellite.|Generato solo da processi esterni. Vedere le istruzioni sulla raccolta di eventi da processi esterni.|  
|satellite_service_start_posted|Generato quando avviare il servizio messaggio viene registrato nel Launchpad.|questo evento indica a Launchpad di avviare il processo esterno e contiene un ID per la nuova sessione.|  
|satellite_unexpected_message_received|Generato quando viene ricevuto un messaggio imprevisto.|Indica un errore.|  
|stack_trace|Si verifica alla richiesta di un dump di memoria del processo.|Indica un errore.|  
|trace_event|Usato a scopo di monitoraggio|Questi eventi possono contenere SQL Server, Launchpad e i messaggi di traccia dei processi esterni. Questi includono l'output in stdout e stderr da R.|  
|launchpad_launch_start|Generato quando Launchpad avvia un satellite.|Generato solo da Launchpad Vedere le istruzioni sulla raccolta di eventi da launchpad.exe.|  
|launchpad_resume_sent|Generato quando Launchpad ha avviato il satellite e inviato un messaggio di ripristino a SQL Server.|Generato solo da Launchpad Vedere le istruzioni sulla raccolta di eventi da launchpad.exe.|  
|satellite_data_chunk_sent|Generato quando la connessione satellite completa l'invio di un singolo blocco di dati.|Contiene informazioni sul numero di colonne, numero di righe, numero di pacchetti e sul tempo impiegato per l'invio del blocco.|  
|satellite_sessionId_mismatch|L'ID della sessione del messaggio non è previsto||  

<a name="bkmk_externalevents"></a>

### <a name="collecting-events-from-external-processes"></a>Raccolta di eventi da processi esterni

Machine Learning Services per SQL Server avvia alcuni servizi che vengono eseguiti all'esterno del processo di SQL Server. Per acquisire gli eventi correlati a questi processi esterni, è necessario creare un file di configurazione per la traccia degli eventi e inserire il file nella stessa directory del file eseguibile per il processo.  

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!IMPORTANT]
> A partire da SQL Server 2019, il meccanismo di isolamento è stato modificato. È quindi necessario assegnare le autorizzazioni appropriate alla directory in cui è archiviato il file di configurazione del tracciato. Per altre informazioni su come impostare queste autorizzazioni, vedere [la sezione Autorizzazioni per i file in SQL Server 2019 in Windows: Modifiche al meccanismo di isolamento per Machine Learning Services](../install/sql-server-machine-learning-services-2019.md#file-permissions).
::: moniker-end

+ **[!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)]**   
  
    Per acquisire gli eventi correlati a Launchpad, posizionare il file con estensione *xml* nella directory Binn per l'istanza di SQL Server. In un'installazione predefinita la directory è:

    `C:\Program Files\Microsoft SQL Server\MSSQL_version_number.MSSQLSERVER\MSSQL\Binn`.  
  
+ **BXLServer** è il processo satellite che supporta l'estendibilità di SQL con linguaggi di script esterni, come R o Python. Un'istanza separata di BxlServer viene avviata per ogni istanza del linguaggio esterno.
  
    Per acquisire gli eventi correlati a BXLServer, posizionare il file con estensione *xml* nella directory di installazione di R o Python. In un'installazione predefinita la directory è:
     
    **R:** `C:\Program Files\Microsoft SQL Server\MSSQL_version_number.MSSQLSERVER\R_SERVICES\library\RevoScaleR\rxLibs\x64`.  

    **Python:** `C:\Program Files\Microsoft SQL Server\MSSQL_version_number.MSSQLSERVER\PYTHON_SERVICES\Lib\site-packages\revoscalepy\rxLibs`.

Il file di configurazione deve avere lo stesso nome del file eseguibile, con il formato "[nome].xeventi.xml". In altre parole, i file devono essere denominati come segue:

+ `Launchpad.xevents.xml`
+ `bxlserver.xevents.xml`

Il file di configurazione presenta il formato seguente:

```xml
<?xml version="1.0" encoding="utf-8"?>  
<event_sessions>  
<event_session name="[session name]" maxMemory="1" dispatchLatency="1" MaxDispatchLatency="2 SECONDS">  
    <description owner="you">Xevent for launchpad or bxl server.</description>  
    <event package="SQLSatellite" name="[XEvent Name 1]" />  
    <event package="SQLSatellite" name="[XEvent Name 2]" />  
    <target package="package0" name="event_file">  
      <parameter name="filename" value="[SessionName].xel" />  
      <parameter name="max_file_size" value="10" />  
      <parameter name="max_rollover_files" value="10" />  
    </target>  
  </event_session>  
</event_sessions>  
```

+ Per configurare la traccia, modificare il segnaposto *session name*, il segnaposto per il nome del file (`[SessionName].xel`) e i nomi degli eventi da acquisire, ad esempio `[XEvent Name 1]` o `[XEvent Name 1]`.  
+ Verrà raccolto qualsiasi numero di tag event package visualizzati, a condizione che l'attributo name sia corretto.

### <a name="example-capturing-launchpad-events"></a>Esempio: Acquisizione degli eventi di Launchpad

L'esempio seguente illustra la definizione della traccia degli eventi per il servizio Launchpad:

```xml
<?xml version="1.0" encoding="utf-8"?>  
<event_sessions>  
<event_session name="sqlsatelliteut" maxMemory="1" dispatchLatency="1" MaxDispatchLatency="2 SECONDS">  
    <description owner="hay">Xevent for sql tdd runner.</description>  
    <event package="SQLSatellite" name="launchpad_launch_start" />  
    <event package="SQLSatellite" name="launchpad_resume_sent" />  
    <target package="package0" name="event_file">  
      <parameter name="filename" value="launchpad_session.xel" />  
      <parameter name="max_file_size" value="10" />  
      <parameter name="max_rollover_files" value="10" />  
    </target>  
  </event_session>  
</event_sessions>  
```

+ Posizionare il file con estensione *xml* nella directory Binn per l'istanza di SQL Server.
+ Il nome del file deve essere `Launchpad.xevents.xml`.

### <a name="example-capturing-bxlserver-events"></a>Esempio: Acquisizione degli eventi di BXLServer  

L'esempio seguente illustra la definizione di una traccia di eventi per l'eseguibile di BXLServer.
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<event_sessions>  
 <event_session name="sqlsatelliteut" maxMemory="1" dispatchLatency="1" MaxDispatchLatency="2 SECONDS">  
    <description owner="hay">Xevent for sql tdd runner.</description>  
    <event package="SQLSatellite" name="satellite_abort_received" />  
    <event package="SQLSatellite" name="satellite_authentication_completion" />  
    <event package="SQLSatellite" name="satellite_cleanup" />  
    <event package="SQLSatellite" name="satellite_data_receive_completion" />  
    <event package="SQLSatellite" name="satellite_data_send_completion" />  
    <event package="SQLSatellite" name="satellite_data_send_start" />  
    <event package="SQLSatellite" name="satellite_schema_sent" />   
    <event package="SQLSatellite" name="satellite_unexpected_message_received" />    
    <event package="SQLSatellite" name="satellite_data_chunk_sent" />   
    <target package="package0" name="event_file">  
      <parameter name="filename" value="satellite_session.xel" />  
      <parameter name="max_file_size" value="10" />  
      <parameter name="max_rollover_files" value="10" />  
    </target>  
  </event_session>  
</event_sessions>  
```

+ Posizionare il file con estensione *xml* nella stessa directory dell'eseguibile di BXLServer.
+ Il nome del file deve essere `bxlserver.xevents.xml`.

## <a name="next-steps"></a>Passaggi successivi

- [Monitorare l'esecuzione di script Python ed R tramite report personalizzati in SQL Server Management Studio](../../machine-learning/administration/monitor-sql-server-machine-learning-services-using-custom-reports-management-studio.md)
- [Monitorare Machine Learning Services per SQL Server tramite DMV](../../machine-learning/administration/monitor-sql-server-machine-learning-services-using-dynamic-management-views.md)