---
title: Destinazioni per gli eventi estesi in SQL Server
description: Questo articolo illustra le destinazioni package0 per gli eventi estesi in SQL Server. Vengono fornite informazioni sulle funzionalità delle destinazioni per la raccolta e la creazione di report sui dati e sui parametri di destinazione.
ms.date: 09/07/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: conceptual
ms.assetid: 47c64144-4432-4778-93b5-00496749665b
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||=azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: bb2ea11d395ab996adcd892a366cbf41575a78d4
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107489820"
---
# <a name="targets-for-extended-events-in-sql-server"></a>Destinazioni per gli eventi estesi in SQL Server

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]


Questo articolo illustra quando e come usare le destinazioni package0 per gli eventi estesi in SQL Server. Per ogni destinazione, l'articolo presenta le informazioni seguenti:

- Le funzionalità di raccolta e creazione di report dei dati inviati dagli eventi.
- I parametri, tranne nei casi in cui il parametro è facilmente comprensibile.


#### <a name="xquery-example"></a>Esempio di XQuery


La [sezione ring_buffer](#h2_target_ring_buffer) include un esempio d'uso di [XQuery in Transact-SQL](../../xquery/xquery-language-reference-sql-server.md) per copiare una stringa XML in un set di righe relazionale.


### <a name="prerequisites"></a>Prerequisiti


- Avere una certa familiarità con le nozioni di base relative agli eventi estesi, come descritto in [Avvio rapido: Eventi estesi in SQL Server](../../relational-databases/extended-events/quick-start-extended-events-in-sql-server.md).


- Avere installato una versione recente dell'utilità SQL Server Management Studio (SSMS.exe) aggiornata di frequente. Per informazioni dettagliate, vedere:
    - [Scaricare SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md)


- In SSMS.exe sapere come usare **Esplora oggetti** per fare clic con il pulsante destro del mouse sul nodo di destinazione nella sessione eventi per [semplificare la visualizzazione dei dati di output](../../relational-databases/extended-events/advanced-viewing-of-target-data-from-extended-events-in-sql-server.md).
    - I dati dell'evento vengono acquisiti come stringa XML, ma in questo articolo sono visualizzati in righe relazionali. Per visualizzare i dati è stato usato SSMS e quindi i dati sono stati copiati e incollati in questo articolo.
    - La tecnica T-SQL alternativa per la generazione di set di righe da XML viene spiegata nella [sezione ring_buffer](#h2_target_ring_buffer). Questa operazione implica l'uso di XQuery.



## <a name="parameters-actions-and-fields"></a>Parametri, azioni e campi


In Transact-SQL l'istruzione [CREATE EVENT SESSION](~/t-sql/statements/create-event-session-transact-sql.md) è fondamentale per gli eventi estesi. Per scrivere l'istruzione sono spesso necessari un elenco e una descrizione dei dati seguenti:

- I campi associati all'evento selezionato.
- I parametri associati alla destinazione selezionata.

Le istruzioni SELECT che restituiscono tali elenchi dalle visualizzazioni di sistema possono essere copiate dalla sezione C dell'articolo seguente:

- [Istruzioni SELECT e JOIN da viste di sistema per eventi estesi in SQL Server](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md)
    - [C.4](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_C_4_data_fields) Campi SELECT per un evento.
    - [C.6](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_C_6_parameters_targets) Parametri SELECT per una destinazione.
    - [C.3](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_C_3_select_all_available_objects) Azioni SELECT.


È possibile visualizzare i parametri, le azioni e i campi usati nel contesto di un'istruzione CREATE EVENT SESSION effettiva, in [questo collegamento](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_B_2_TSQL_perspective).



<a name="h2_target_etw_classic_sync_target"></a>

## <a name="etw_classic_sync_target-target"></a>Destinazione etw_classic_sync_target


Gli eventi estesi di SQL Server possono interagire con Event Tracing for Windows (ETW) per monitorare l'attività del sistema. Per altre informazioni, vedere:

- [Destinazione di Event Tracing for Windows](../../relational-databases/extended-events/event-tracing-for-windows-target.md)
- [Monitorare l'attività del sistema mediante gli eventi estesi](../../relational-databases/extended-events/monitor-system-activity-using-extended-events.md)


Questa destinazione di ETW elabora *in modalità sincrona* i dati ricevuti, mentre la maggior parte delle destinazioni esegue l'elaborazione *in modalità asincrona*.

> [!NOTE]
> Il database SQL di Azure non supporta `etw_classic_sync_target target`.

<!-- After OPS Versioning is live, the above !NOTE could be converted into a "3colon ZONE".  GeneMi = MightyPen. -->

<a name="h2_target_event_counter"></a>

## <a name="event_counter-target"></a>Destinazione event_counter


La destinazione event_counter calcola semplicemente la frequenza con cui si verifica ogni evento specificato.


A differenza di quasi tutte le altre destinazioni:

- La destinazione event_counter non ha parametri.


- A differenza di quasi tutte le destinazioni, event_counter elabora *in modalità sincrona* i dati ricevuti.
    - La modalità sincrona è accettabile per la destinazione event_counter semplice perché questa destinazione richiede poche operazioni di elaborazione.
    - Il motore di database viene disconnesso da qualsiasi destinazione che risulta troppo lenta e che pertanto rischia di rallentare le prestazioni del motore. Questo è uno dei motivi per cui la maggior parte delle destinazioni elabora i dati *in modalità asincrona*.


#### <a name="example-output-captured-by-event_counter"></a>Output di esempio acquisito da event_counter


```
package_name   event_name         count
------------   ----------         -----
sqlserver      checkpoint_begin   4
```


Di seguito è riportata l'istruzione CREATE EVENT SESSION che ha generato i risultati precedenti. Per questo test, nella clausola EVENT...WHERE è stato usato il campo **package0.counter** per arrestare il conteggio dopo che è stato raggiunto 4.


```sql
CREATE EVENT SESSION [event_counter_1]
    ON SERVER 
    ADD EVENT sqlserver.checkpoint_begin   -- Test by issuing CHECKPOINT; statements.
    (
        WHERE ([package0].[counter] <= (4))   -- A predicate filter.
    )
    ADD TARGET package0.event_counter
    WITH
    (
        MAX_MEMORY = 4096 KB,
        MAX_DISPATCH_LATENCY = 3 SECONDS
    );
```



<a name="h2_target_event_file"></a>

## <a name="event_file-target"></a>Destinazione event_file


La destinazione **event_file** scrive l'output della sessione eventi dal buffer in un file su disco:


- Specificare il parametro *filename=* nella clausola ADD TARGET.
    - L'estensione del file deve essere **xel** .


- Il nome di file selezionato viene usato dal sistema come prefisso a cui viene aggiunto un valore long integer basato su data-ora, seguito dall'estensione xel.

::: moniker range="= azuresqldb-current || = azuresqldb-mi-current "

> [!NOTE]
> database SQL di Azure supporta solo l'archiviazione `xel` di file Archiviazione BLOB di Azure. 
>
> Per un esempio di codice **event_file** specifico per il database SQL (e Istanza gestita di SQL), vedere [Codice di destinazione del file evento per eventi estesi nel database SQL](/azure/sql-database/sql-database-xevent-code-event-file).

::: moniker-end


#### <a name="create-event-session-with-event_file-target"></a>CREATE EVENT SESSION con destinazione **event_file**


Di seguito è riportata l'estensione CREATE EVENT SESSION usata per eseguire il test. Una delle clausole ADD TARGET specifica una destinazione event_file.


```sql
CREATE EVENT SESSION [locks_acq_rel_eventfile_22]
    ON SERVER 
    ADD EVENT sqlserver.lock_acquired
    (
        SET
            collect_database_name=(1),
            collect_resource_description=(1)

        ACTION (sqlserver.sql_text,sqlserver.transaction_id)

        WHERE
        (
            [database_name]=N'InMemTest2'
            AND
            [object_id]=(370100359)
        )
    ),
    ADD EVENT sqlserver.lock_released
    (
        SET
            collect_database_name=(1),
            collect_resource_description=(1)

        ACTION(sqlserver.sql_text,sqlserver.transaction_id)

        WHERE
        (
            [database_name]=N'InMemTest2'
            AND
            [object_id]=(370100359)
        )
    )
    ADD TARGET package0.event_counter,
    ADD TARGET package0.event_file
    (
        SET     filename=N'C:\Junk\locks_acq_rel_eventfile_22-.xel'
    )
    WITH
    (
        MAX_MEMORY=4096 KB,
        MAX_DISPATCH_LATENCY=10 SECONDS
    );
```


#### <a name="sysfn_xe_file_target_read_file-function"></a>Funzione sys.fn_xe_file_target_read_file


La destinazione event_file archivia i dati ricevuti in un formato binario che non è leggibile. Transact-SQL può riportare il contenuto del file con estensione xel eseguendo un'istruzione SELECT FROM sui dati restituiti dalla funzione [**sys.fn_xe_file_target_read_file**](../../relational-databases/system-functions/sys-fn-xe-file-target-read-file-transact-sql.md) .


Per SQL Server **2016** e versioni successive, i dati sono stati riportati dall'istruzione T-SQL SELECT seguente. Nel codice viene usato il suffisso *.xel. 


```
SELECT f.*
        --,CAST(f.event_data AS XML)  AS [Event-Data-Cast-To-XML]  -- Optional
    FROM
        sys.fn_xe_file_target_read_file(
            'C:\junk\locks_acq_rel_eventfile_22-*.xel',
            null, null, null)  AS f;
```


Per SQL Server **2014**, i dati verrebbero riportati da un'istruzione SELECT simile alla seguente. Dopo SQL Server 2014, i file con estensione xem non sono stati più usati.


```
SELECT f.*
        --,CAST(f.event_data AS XML)  AS [Event-Data-Cast-To-XML]  -- Optional
    FROM
        sys.fn_xe_file_target_read_file(
            'C:\junk\locks_acq_rel_eventfile_22-*.xel',
            'C:\junk\metafile.xem',
            null, null)  AS f;
```


Naturalmente, per vedere i dati del file con estensione xel è possibile anche usare manualmente l'interfaccia utente di SSMS:


#### <a name="data-stored-in-the-event_file-target"></a>Dati archiviati nella destinazione event_file


Di seguito è illustrato il report dell'esecuzione di SELECT FROM sui dati restituiti da **sys.fn_xe_file_target_read_file**, in SQL Server 2016.


```
module_guid                            package_guid                           object_name     event_data                                                                                                                                                                                                                                                                                          file_name                                                      file_offset
-----------                            ------------                           -----------     ----------                                                                                                                                                                                                                                                                                          ---------                                                      -----------
D5149520-6282-11DE-8A39-0800200C9A66   03FDA7D0-91BA-45F8-9875-8B6DD0B8E9F2   lock_acquired   <event name="lock_acquired" package="sqlserver" timestamp="2016-08-07T20:13:35.827Z"><action name="transaction_id" package="sqlserver"><value>39194</value></action><action name="sql_text" package="sqlserver"><value><![CDATA[  select top 1 * from dbo.T_Target;  ]]></value></action></event>   C:\junk\locks_acq_rel_eventfile_22-_0_131150744126230000.xel   11776
D5149520-6282-11DE-8A39-0800200C9A66   03FDA7D0-91BA-45F8-9875-8B6DD0B8E9F2   lock_released   <event name="lock_released" package="sqlserver" timestamp="2016-08-07T20:13:35.832Z"><action name="transaction_id" package="sqlserver"><value>39194</value></action><action name="sql_text" package="sqlserver"><value><![CDATA[  select top 1 * from dbo.T_Target;  ]]></value></action></event>   C:\junk\locks_acq_rel_eventfile_22-_0_131150744126230000.xel   11776
```



<a name="h2_target_histogram"></a>

## <a name="histogram-target"></a>Destinazione histogram


La destinazione **histogram** è più complessa rispetto a event_counter. Questa destinazione può eseguire le operazioni seguenti:

- Contare le occorrenze per più elementi separatamente.
- Contare le occorrenze di diversi tipi di elementi:
    - Campi di evento.
    - Azioni.


Il parametro **source_type** è l'elemento chiave per il controllo della destinazione histogram:

- **source_type=0** indica la raccolta di dati per i *campi di evento*.
- **source_type=1** indica la raccolta di dati per le *azioni*.
    - Il valore predefinito è 1.


Il valore predefinito del parametro 'slots' è 256. Se si assegna un altro valore, il valore viene arrotondato alla successiva potenza di 2.

- Ad esempio, slots=59 viene arrotondato a =64.


### <a name="action-example-for-histogram"></a>Esempio di *azione* per histogram


Nella clausola TARGET...SET, l'istruzione Transact-SQL CREATE EVENT SESSION seguente specifica l'assegnazione del parametro di destinazione **source_type=1**. Il valore 1 significa che la destinazione histogram tiene traccia di un'azione.

In questo esempio, la clausola EVENT...ACTION consente di selezionare una sola azione per la destinazione, ovvero **sqlos.system_thread_id**. Nella clausola TARGET...SET viene visualizzata l'assegnazione **source=N'sqlos.system_thread_id'** .

- Per tener traccia di più azioni di origine, è possibile aggiungere una seconda destinazione histogram all'istruzione CREATE EVENT SESSION.


```sql
CREATE EVENT SESSION [histogram_lockacquired]
    ON SERVER 
    ADD EVENT sqlserver.lock_acquired
        (
        ACTION
            (
            sqlos.system_thread_id
            )
        )
    ADD TARGET package0.histogram
        (
        SET
            filtering_event_name=N'sqlserver.lock_acquired',
            slots=(16),
            source=N'sqlos.system_thread_id',
            source_type=1
        )
    WITH
        (
        <.... (For brevity, numerous parameter assignments generated by SSMS.exe are not shown here.) ....>
        );
```


 Sono stati acquisiti i dati seguenti. I valori della colonna **value** sono valori di system_thread_id. Ad esempio, sono stati acquisiti 236 blocchi nell'ambito del thread 6540.


```
value   count
-----   -----
 6540     236
 9308      91
 9668      74
10144      49
 5244      44
 2396      28
```


#### <a name="select-to-discover-available-actions"></a>Istruzione SELECT per individuare le azioni disponibili


L'istruzione SELECT [C.3](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_C_3_select_all_available_objects) può trovare le azioni che il sistema consente di specificare nell'istruzione CREATE EVENT SESSION. Nella clausola WHERE modificare prima di tutto il filtro **o.name LIKE** in modo da filtrare le azioni a cui si è interessati.


Di seguito è riportato un set di righe di esempio restituito da C.3 SELECT. L'azione **system_thread_id** è visualizzata nella seconda riga.


```
Package-Name   Action-Name                 Action-Description
------------   -----------                 ------------------
package0       collect_current_thread_id   Collect the current Windows thread ID
sqlos          system_thread_id            Collect current system thread ID
sqlserver      create_dump_all_threads     Create mini dump including all threads
sqlserver      create_dump_single_thread   Create mini dump for the current thread
```


### <a name="event-field-example-for-histogram"></a>Esempio di *campo* di evento per histogram


L'esempio seguente imposta **source_type=0**. Il valore assegnato a **source=** è un campo di evento (non un'azione).



```sql
CREATE EVENT SESSION [histogram_checkpoint_dbid]
    ON SERVER 
    ADD EVENT  sqlserver.checkpoint_begin
    ADD TARGET package0.histogram
    (
    SET
        filtering_event_name = N'sqlserver.checkpoint_begin',
        source               = N'database_id',
        source_type          = (0)
    )
    WITH
    ( <....> );
```


I dati seguenti sono stati acquisiti dalla destinazione histogram. I dati mostrano che nel database ID=5 si sono verificati 7 eventi checkpoint_begin.


```
value   count
-----   -----
5       7
7       4
6       3
```


#### <a name="select-to-discover-available-fields-on-your-chosen-event"></a>Istruzione SELECT per individuare i campi disponibili nell'evento selezionato


L'istruzione [C.4](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_C_4_data_fields) SELECT mostra i campi di evento che è possibile selezionare. Modificare prima di tutto il filtro **o.name LIKE** in modo da restituire il nome dell'evento selezionato.


C.4 SELECT ha restituito il set di righe seguente. Il set di righe mostra che database_id è l'unico campo dell'evento checkpoint_begin che può fornire valori per la destinazione histogram.


```
Package-Name   Event-Name         Field-Name   Field-Description
------------   ----------         ----------   -----------------
sqlserver      checkpoint_begin   database_id  NULL
sqlserver      checkpoint_end     database_id  NULL
```


<a name="h2_target_pair_matching"></a>

## <a name="pair_matching-target"></a>Destinazione pair_matching


La destinazione pair_matching consente di individuare gli eventi iniziali che si verificano senza un evento finale corrispondente. Ad esempio, se si verifica un evento lock_acquired che non è seguito tempestivamente da un evento lock_released, questo può costituire un problema.


Il sistema non stabilisce automaticamente la corrispondenza tra eventi iniziali ed eventi finali. Al contrario, la corrispondenza viene definita nell'istruzione CREATE EVENT SESSION. Quando viene stabilita una corrispondenza tra un evento iniziale e uno finale, la coppia viene eliminata per consentire di concentrarsi sugli eventi iniziali senza eventi finali corrispondenti.


#### <a name="finding-matchable-fields-for-the-start-and-end-event-pair"></a>Ricerca di campi associabili per la coppia di eventi iniziale e finale


Usando [C.4 SELECT](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_C_4_data_fields), è possibile notare che nel set di righe seguente sono presenti circa 16 campi per l'evento lock_acquired. Il set di righe visualizzato qui è stato suddiviso manualmente per mostrare i campi in base ai quali è stata stabilita la corrispondenza. Per alcuni campi sarebbe inutile provare a stabilire la corrispondenza, come nel caso del campo **duration** di entrambi gli eventi.


```
Package-Name   Event-Name   Field-Name               Field-Description
------------   ----------   ----------               -----------------
sqlserver   lock_acquired   database_name            NULL
sqlserver   lock_acquired   mode                     NULL
sqlserver   lock_acquired   resource_0               The ID of the locked object, when lock_resource_type is OBJECT.
sqlserver   lock_acquired   resource_1               NULL
sqlserver   lock_acquired   resource_2               The ID of the lock partition, when lock_resource_type is OBJECT, and resource_1 is 0.
sqlserver   lock_acquired   transaction_id           NULL

sqlserver   lock_acquired   associated_object_id     The ID of the object that requested the lock that was acquired.
sqlserver   lock_acquired   database_id              NULL
sqlserver   lock_acquired   duration                 The time (in microseconds) between when the lock was requested and when it was canceled.
sqlserver   lock_acquired   lockspace_nest_id        NULL
sqlserver   lock_acquired   lockspace_sub_id         NULL
sqlserver   lock_acquired   lockspace_workspace_id   NULL
sqlserver   lock_acquired   object_id                The ID of the locked object, when lock_resource_type is OBJECT. For other lock resource types it will be 0
sqlserver   lock_acquired   owner_type               NULL
sqlserver   lock_acquired   resource_description     The description of the lock resource. The description depends on the type of lock. This is the same value as the resource_description column in the sys.dm_tran_locks view.
sqlserver   lock_acquired   resource_type            NULL
```


### <a name="example-of-pair_matching"></a>Esempio di pair_matching


L'istruzione CREATE EVENT SESSION seguente specifica due eventi e due destinazioni. La destinazione pair_matching specifica due set di campi in modo da stabilire la corrispondenza tra gli eventi. La sequenza di campi delimitati da virgole assegnati a **begin_matching_columns=** e **end_matching_columns=** deve essere identica. Tra i campi indicati nel valore delimitato da virgole non sono consentiti caratteri di tabulazione o di nuova riga, ma possono essere usati gli spazi.

Per limitare i risultati, si è prima eseguita l'istruzione SELECT su sys.objects per trovare il valore di object_id della tabella di test. Per lo specifico ID si è aggiunto un filtro alla clausola EVENT...WHERE.


```sql
CREATE EVENT SESSION [pair_matching_lock_a_r_33]
    ON SERVER 
    ADD EVENT sqlserver.lock_acquired
    (
        SET
            collect_database_name = (1),
            collect_resource_description = (1)

        ACTION (sqlserver.transaction_id)

        WHERE
        (
            [database_name] = 'InMemTest2'
            AND
            [object_id] = 370100359
        )
    ),
    ADD EVENT sqlserver.lock_released
    (
        SET
            collect_database_name = (1),
            collect_resource_description = (1)

        ACTION (sqlserver.transaction_id)

        WHERE
        (
            [database_name] = 'InMemTest2'
            AND
            [object_id] = 370100359
        )
    )
    ADD TARGET package0.event_counter,
    ADD TARGET package0.pair_matching
    (
        SET
            begin_event = N'sqlserver.lock_acquired',
            begin_matching_columns =
                N'resource_0, resource_1, resource_2, transaction_id, database_id',

            end_event = N'sqlserver.lock_released',
            end_matching_columns =
                N'resource_0, resource_1, resource_2, transaction_id, database_id',

            respond_to_memory_pressure = (1)
    )
    WITH
    (
        MAX_MEMORY = 8192 KB,
        MAX_DISPATCH_LATENCY = 15 SECONDS
    );
```


Per testare la sessione eventi, si è intenzionalmente impedito il rilascio di blocchi acquisiti. A tal fine, si sono eseguiti i passaggi T-SQL seguenti:

1. BEGIN TRANSACTION.
2. UPDATE MyTable....
3. Non si è intenzionalmente eseguita un'istruzione COMMIT TRANSACTION finché non si sono esaminate le destinazioni.
4. Successivamente, dopo il testing, si è eseguita un'istruzione COMMIT TRANSACTION.

La semplice destinazione **event_counter** ha fornito le righe di output seguenti. Poiché 52-50=2, l'output indica che, quando si esamina l'output della destinazione pair-matching, dovrebbero essere visibili due eventi lock_acquired non abbinati.


```
package_name   event_name      count
------------   ----------      -----
sqlserver      lock_acquired   52
sqlserver      lock_released   50
```


La destinazione **pair_matching** ha fornito l'output seguente. Come indicato dall'output event_counter, sono effettivamente visibili due righe lock_acquired. Ciò significa che questi due eventi lock_acquired non sono abbinati.


```
package_name   event_name      timestamp                     database_name   duration   mode   object_id   owner_type   resource_0   resource_1   resource_2   resource_description   resource_type   transaction_id
------------   ----------      ---------                     -------------   --------   ----   ---------   ----------   ----------   ----------   ----------   --------------------   -------------   --------------
sqlserver      lock_acquired   2016-08-05 12:45:47.9980000   InMemTest2      0          S      370100359   Transaction  370100359    3            0            [INDEX_OPERATION]      OBJECT          34126
sqlserver      lock_acquired   2016-08-05 12:45:47.9980000   InMemTest2      0          IX     370100359   Transaction  370100359    0            0                                   OBJECT          34126
```


Le righe relative agli eventi lock_acquired non abbinati possono includere il testo T-SQL, o **sqlserver.sql_text**, che ha acquisito i blocchi, ma in questo caso il testo è stato omesso per limitare le dimensioni dell'immagine.


<a name="h2_target_ring_buffer"></a>

## <a name="ring_buffer-target"></a>Destinazione ring_buffer


La destinazione ring_buffer è utile per eseguire in modo semplice e rapido il test degli eventi. Quando si arresta la sessione eventi, l'output archiviato viene rimosso.

In questa sezione ring_buffer viene inoltre illustrato come usare l'implementazione Transact-SQL di XQuery per copiare il contenuto XML di ring_buffer in un set di righe relazionale più leggibile.


#### <a name="create-event-session-with-ring_buffer"></a>CREATE EVENT SESSION con ring_buffer


Questa istruzione CREATE EVENT SESSION che usa la destinazione ring_buffer non presenta caratteristiche particolari.


```sql
CREATE EVENT SESSION [ring_buffer_lock_acquired_4]
    ON SERVER 
    ADD EVENT sqlserver.lock_acquired
    (
        SET collect_resource_description=(1)

        ACTION(sqlserver.database_name)

        WHERE
        (
            [object_id]=(370100359)  -- ID of MyTable
            AND
            sqlserver.database_name='InMemTest2'
        )
    )
    ADD TARGET package0.ring_buffer
    (
        SET max_events_limit=(98)
    )
    WITH
    (
        MAX_MEMORY=4096 KB,
        MAX_DISPATCH_LATENCY=3 SECONDS
    );
```


### <a name="xml-output-received-for-lock_acquired-by-ring_buffer"></a>Output XML ricevuto per lock_acquired da ring_buffer


Quando è recuperato da un'istruzione SELECT, il contenuto è riportato in forma di stringa XML. Di seguito è riportata la stringa XML archiviata dalla destinazione ring_buffer durante il testing. Tuttavia, per limitare la quantità di codice XML visualizzata, sono stati cancellati tutti gli elementi tranne due &#x3c;event&#x3e;. Inoltre, per ogni elemento &#x3c;event&#x3e; sono stati eliminati diversi elementi &#x3c;data&#x3e; estranei.


```xml
<RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="6" eventCount="6" droppedCount="0" memoryUsed="1032">
  <event name="lock_acquired" package="sqlserver" timestamp="2016-08-05T23:59:53.987Z">
    <data name="mode">
      <type name="lock_mode" package="sqlserver"></type>
      <value>1</value>
      <text><![CDATA[SCH_S]]></text>
    </data>
    <data name="transaction_id">
      <type name="int64" package="package0"></type>
      <value>111030</value>
    </data>
    <data name="database_id">
      <type name="uint32" package="package0"></type>
      <value>5</value>
    </data>
    <data name="resource_0">
      <type name="uint32" package="package0"></type>
      <value>370100359</value>
    </data>
    <data name="resource_1">
      <type name="uint32" package="package0"></type>
      <value>0</value>
    </data>
    <data name="resource_2">
      <type name="uint32" package="package0"></type>
      <value>0</value>
    </data>
    <data name="database_name">
      <type name="unicode_string" package="package0"></type>
      <value><![CDATA[]]></value>
    </data>
    <action name="database_name" package="sqlserver">
      <type name="unicode_string" package="package0"></type>
      <value><![CDATA[InMemTest2]]></value>
    </action>
  </event>
  <event name="lock_acquired" package="sqlserver" timestamp="2016-08-05T23:59:56.012Z">
    <data name="mode">
      <type name="lock_mode" package="sqlserver"></type>
      <value>1</value>
      <text><![CDATA[SCH_S]]></text>
    </data>
    <data name="transaction_id">
      <type name="int64" package="package0"></type>
      <value>111039</value>
    </data>
    <data name="database_id">
      <type name="uint32" package="package0"></type>
      <value>5</value>
    </data>
    <data name="resource_0">
      <type name="uint32" package="package0"></type>
      <value>370100359</value>
    </data>
    <data name="resource_1">
      <type name="uint32" package="package0"></type>
      <value>0</value>
    </data>
    <data name="resource_2">
      <type name="uint32" package="package0"></type>
      <value>0</value>
    </data>
    <data name="database_name">
      <type name="unicode_string" package="package0"></type>
      <value><![CDATA[]]></value>
    </data>
    <action name="database_name" package="sqlserver">
      <type name="unicode_string" package="package0"></type>
      <value><![CDATA[InMemTest2]]></value>
    </action>
  </event>
</RingBufferTarget>
```


Per visualizzare il codice XML precedente, è possibile eseguire l'istruzione SELECT seguente mentre è attiva la sessione eventi. I dati XML attivi vengono recuperati dalla visualizzazione di sistema **sys.dm_xe_session_targets**.


```sql
SELECT
        CAST(LocksAcquired.TargetXml AS XML)  AS RBufXml
    INTO
        #XmlAsTable
    FROM
        (
        SELECT
                CAST(t.target_data AS XML)  AS TargetXml
            FROM
                     sys.dm_xe_session_targets  AS t
                JOIN sys.dm_xe_sessions         AS s

                    ON s.address = t.event_session_address
            WHERE
                t.target_name = 'ring_buffer'
                AND
                s.name        = 'ring_buffer_lock_acquired_4'
        )
            AS LocksAcquired;


SELECT * FROM #XmlAsTable;
```


### <a name="xquery-to-see-the-xml-as-a-rowset"></a>XQuery per visualizzare il codice XML come set di righe


Per visualizzare il codice XML precedente come set di righe relazionale, continuare dall'istruzione SELECT precedente ed eseguire il codice T-SQL seguente. Le righe commentate spiegano ogni uso di XQuery.


```sql
SELECT
         -- (A)
         ObjectLocks.value('(@timestamp)[1]',
            'datetime'     )  AS [OccurredDtTm]

        -- (B)
        ,ObjectLocks.value('(data[@name="mode"]/text)[1]',
            'nvarchar(32)' )  AS [Mode]

        -- (C)
        ,ObjectLocks.value('(data[@name="transaction_id"]/value)[1]',
            'bigint' )  AS [TxnId]

        -- (D)
        ,ObjectLocks.value('(action[@name="database_name" and @package="sqlserver"]/value)[1]',
            'nvarchar(128)')  AS [DatabaseName]
    FROM
        #TableXmlCell
    CROSS APPLY
        -- (E)
        TargetDateAsXml.nodes('/RingBufferTarget/event[@name="lock_acquired"]')  AS T(ObjectLocks);
```


#### <a name="xquery-notes-from-preceding-select"></a>Note di XQuery dall'istruzione SELECT precedente


(A)
- Valore dell'attributo timestamp= sull'elemento &#x3c;event&#x3e;.
- Il costrutto '(...)[1]' garantisce la restituzione di un solo valore per iterazione, poiché questa è una limitazione obbligatoria del metodo .value() di XQuery per variabili e colonne di tipo XML.


(B)
- Valore interno dell'elemento &#x3c;text&#x3e;, in un elemento &#x3c;data&#x3e; con l'attributo name= impostato su "mode".


(C)
- Valore interno dell'elemento &#x3c;value&#x3e;, in un elemento &#x3c;data&#x3e; con l'attributo name= impostato su "transaction_id".


(D)
- L'elemento &#x3c;event&#x3e; contiene l'elemento &#x3c;action&#x3e;.
- L'elemento &#x3c;action&#x3e; con l'attributo name= impostato su "database_name" e l'attributo package= impostato su "sqlserver" (non su "package0"), ottiene il valore interno dell'elemento &#x3c;value&#x3e;.


(E)
- C.A. determina la ripetizione dell'elaborazione per ogni singolo elemento &#x3c;event&#x3e; con l'attributo name= impostato su "lock_acquired".
- Questo vale per il codice XML restituito dalla clausola FROM precedente.


#### <a name="output-from-xquery-select"></a>Output di XQuery SELECT


Di seguito è riportato il set di righe generato dal codice T-SQL precedente che include XQuery.


```
OccurredDtTm              Mode    DatabaseName
------------              ----    ------------
2016-08-05 23:59:53.987   SCH_S   InMemTest2
2016-08-05 23:59:56.013   SCH_S   InMemTest2
```



## <a name="xevent-net-namespaces-and-cx23"></a>Spazi dei nomi XEvent .NET e C&#x23;


Package0 ha altre due destinazioni che però non possono essere usate in Transact-SQL:

- compressed_history
- event_stream


Un motivo noto per cui queste due destinazioni non possono essere usate in T-SQL consiste nel fatto che i relativi valori non null nella colonna *sys.dm_xe_objects.capabilities* non includono il bit 0x1.


La destinazione event_stream può essere usata nei programmi .NET scritti in linguaggi come C#. Gli sviluppatori C# e altri sviluppatori .NET possono accedere a un flusso di eventi tramite una classe .NET Framework, ad esempio nello spazio dei nomi Microsoft.SqlServer.XEvents.Linq.

Se rilevato, l'errore **25726** indica che il flusso di eventi riempito più velocemente rispetto al client può avere determinato il consumo dei dati. Di conseguenza, il motore di database si è disconnesso dal flusso di eventi per evitare il rallentamento delle prestazioni del server.


### <a name="xevent-namespaces"></a>Spazi dei nomi XEvent


- [Spazio dei nomi Microsoft.SqlServer.Management.XEvent](/dotnet/api/microsoft.sqlserver.management.xevent)

- [Spazio dei nomi Microsoft.SqlServer.XEvent.Linq](/dotnet/api/microsoft.sqlserver.xevent.linq)
