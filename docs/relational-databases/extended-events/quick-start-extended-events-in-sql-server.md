---
title: 'Guida introduttiva: Eventi estesi in SQL Server'
description: Questo argomento di avvio rapido consente di usare gli eventi estesi, un sistema di monitoraggio delle prestazioni leggero, per raccogliere i dati per il monitoraggio e la risoluzione dei problemi in SQL Server.
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xevents
ms.topic: quickstart
f1_keywords:
- sql11.ssms.XeNewEventSession.General.f1
- sql11.ssms.XeNewEventSession.Events.f1
- sql11.ssms.XeNewEventSession.Targets.f1
- sql11.ssms.XeNewEventSession.Advanced.f1
ms.assetid: 7bb78b25-3433-4edb-a2ec-c8b2fa58dea1
author: rothja
ms.author: jroth
ms.reviewer: maghan
ms.date: 04/16/2020
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9459b2a2b01670a0c6201586597ba02eb1218853
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492397"
---
# <a name="quickstart-extended-events-in-sql-server"></a>Guida introduttiva: Eventi estesi in SQL Server

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Gli eventi estesi costituiscono un sistema di monitoraggio delle prestazioni leggero che consente di raccogliere i dati necessari per il monitoraggio e la risoluzione dei problemi in SQL Server. Per altre informazioni sull'architettura degli eventi estesi, vedere [Panoramica degli eventi estesi](extended-events.md).  Questo articolo è destinato agli sviluppatori SQL che non hanno familiarità con gli eventi estesi e vogliono creare una sessione eventi in pochi minuti. Usando gli eventi estesi, è possibile visualizzare i dettagli sulle operazioni interne del sistema e dell'applicazione SQL. Quando si crea una sessione di eventi estesi, si indica al sistema:

- A quali occorrenze si è interessati.
- La modalità desiderata di ricezione delle segnalazioni di dati.

Questo articolo:

- Usa schermate per illustrare i clic eseguiti in SSMS.exe per creare una sessione dell'evento.
- Mette in correlazione le schermate alle istruzioni Transact-SQL equivalenti.
- Illustra in dettaglio i termini e concetti di base delle operazioni eseguite e di T-SQL per le sessioni eventi.
- Illustra come testare la sessione eventi.
- Descrive le alternative relative ai risultati:
  - Acquisizione dei risultati per l'archiviazione.
  - Risultati elaborati rispetto a risultati non elaborati.
  - Strumenti per visualizzare i risultati in modi diversi e su diverse scale cronologiche.
- Illustra come è possibile cercare e individuare tutti gli eventi disponibili.
- Fornisce la chiave primaria e relazioni di chiave esterna che sono implicite tra le viste a gestione dinamica (DMV) per gli eventi estesi.
- Descrive altri contenuti da apprendere in articoli correlati.

I blog e altri contenuti informali talvolta fanno riferimento agli eventi estesi usando l'abbreviazione *XEvent*.

> [!NOTE]
> Per informazioni sugli eventi estesi per il database SQL di Azure, tra cui anche codici di esempio, vedere [Eventi estesi nel database SQL](/azure/azure-sql/database/xevent-db-diff-from-svr).

## <a name="preparations-before-demo"></a>Operazioni preliminari alla demo

Le operazioni preliminari seguenti sono necessarie per eseguire realmente la dimostrazione fornita di seguito.

1. [Scaricare SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md)

   Ogni mese è necessario installare l'aggiornamento mensile più recente di SSMS.
2. Accedere a Microsoft SQL Server 2014 o versione successiva.
3. Assicurarsi che l'account abbia [autorizzazione server](../../t-sql/statements/grant-server-permissions-transact-sql.md) di **ALTER ANY EVENT SESSION**.

  Per gli utenti interessati, altre informazioni sulla sicurezza e le autorizzazioni correlate agli eventi estesi sono disponibili nell'[Appendice](#appendix1)alla fine di questo articolo.

## <a name="demo-of-ssms-integration"></a>Demo di integrazione di SSMS

SSMS.exe fornisce un'eccellente interfaccia utente (UI) per gli eventi estesi. L'interfaccia utente è di qualità tale che in molti casi non occorre usare gli eventi estesi con Transact-SQL o le viste a gestione dinamica (DMV) che hanno come destinazione gli eventi estesi.

Questa sezione illustra i passaggi dell'interfaccia utente necessari per creare un evento esteso e visualizzare i dati segnalati. Successivamente vengono fornite informazioni di approfondimento sui concetti relativi a tali passaggi.

### <a name="steps-of-demo"></a>Passaggi della demo

I passaggi sono comprensibili anche se si decide di non eseguirli. La dimostrazione ha inizio con l'avvio della finestra di dialogo **Nuova sessione** , di cui verranno trattate le quattro pagine denominate:

- Generale
- Eventi
- Archiviazione dati
- Avanzate

Il testo e le schermate di supporto possono presentare leggere imprecisioni a causa delle modifiche apportate all'interfaccia utente nel corso di mesi o anni. Le schermate restano tuttavia valide ai fini della spiegazione se le discrepanze sono di minore entità.

1. Connettersi a SSMS.

2. In Esplora oggetti, fare clic su **Gestione** > **Eventi estesi** > **Nuova sessione**. La finestra di dialogo **Nuova sessione** è preferibile rispetto alla finestra di dialogo di **Creazione guidata nuova sessione** anche se le due finestre sono simili tra loro.

3. In alto a sinistra fare clic sulla pagina **Generale** . Quindi digitare *SessioneUtente*, o qualsiasi nome desiderato, nella casella di testo **Nome sessione** . *Non* premere il pulsante **OK**; questa operazione verrà eseguita solo alla fine della demo.

   ![Nuova sessione > Generale > Nome sessione](../../relational-databases/extended-events/media/xevents-session-newsessions-10-general-ssms-yoursessionnode.png)

4. In alto a sinistra fare clic sulla pagina **Eventi** e quindi fare clic sul pulsante **Seleziona** .

   ![Nuova sessione > Eventi > Seleziona > Libreria di eventi, Eventi selezionati](../../relational-databases/extended-events/media/xevents-session-newsessions-14-events-ssms-rightclick-not-wizard.png)

5. Nell'elenco a discesa dell'area **Libreria di eventi** scegliere **Solo nomi evento**.
    - Nella casella di testo digitare **sql**, che consente di filtrare e ridurre il lungo elenco di eventi disponibili usando un operatore *contains* .
    - Scorrere e fare clic sull'evento denominato **sql_statement_completed**.
    - Fare clic sul pulsante freccia destra **>** per spostare l'evento nella casella **Eventi selezionati** .

6. Sempre nella pagina **Eventi** fare clic sul pulsante **Configura** all'estrema destra.

   Nella schermata seguente, dalla quale è stato eliminato il lato sinistro per una migliore visualizzazione, è possibile vedere l'area **Opzioni di configurazione eventi**.

    ![Nuova sessione > Eventi > Configura > Filtro (predicato) > Campo](../../relational-databases/extended-events/media/xevents-session-newsessions-20b-events-ssms-yoursessionnode.png)

7. Fare clic sulla scheda **Filtro (predicato)** . Successivamente, fare clic su **Fare clic qui per aggiungere una clausola** se si intende acquisire tutte le istruzioni SQL SELECT che hanno una clausola HAVING.

8. Scegliere **sqlserver.sql_text** dall'elenco a discesa **Campo**.
   - Per **Operatore** scegliere un operatore LIKE.
   - Per **Valore** digitare **%SELECT%HAVING%** .

   > [!NOTE]
   > In questo nome in due parti, *sqlserver* è il nome del pacchetto e *sql_text* è il nome del campo. L'evento scelto in precedenza, *sql_statement_completed*, deve essere nello stesso pacchetto del campo scelto.

9. In alto a sinistra fare clic sulla pagina **Archiviazione dati** .

10. Nell'area **Destinazioni** fare clic su **Fare clic qui per aggiungere una destinazione**.
    - Nell'elenco a discesa **Tipo** scegliere **event_file**.
    - Selezionando questa opzione i dati degli eventi verranno archiviati in un file che è possibile visualizzare.
    
    > [!NOTE]
    > Non è possibile usare Archiviazione BLOB di Azure come destinazione di archiviazione dei dati in un'istanza locale di SQL Server.

    ![Nuova sessione > Archiviazione dati > Destinazioni > Tipo > event_file](../../relational-databases/extended-events/media/xevents-session-newsessions-30-datastorage-ssms-yoursessionnode.png)

11. Nell'area **Proprietà** digitare un nome di percorso e file completo nella casella di testo **Nome file sul server** .
    - L'estensione del nome file deve essere *XEL*.
    - Questo semplice test richiederà meno di 1 MB delle dimensioni del file.

    ![Nuova sessione > Avanzate > Latenza di recapito massima > OK](../../relational-databases/extended-events/media/xevents-session-newsessions-40-advanced-ssms-yoursessionnode.png)

12. In alto a sinistra fare clic sulla pagina **Avanzate**.
    - Ridurre la **Latenza di recapito massima** a 3 secondi.
    - Infine, fare clic su **OK** nella parte inferiore.

13. In **Esplora oggetti** espandere **Gestione** > **Sessioni** e visualizzare il nuovo nodo per **SessioneUtente**.

    ![Nodo per la nuova *sessione eventi* denominata SessioneUtente, in Esplora oggetti, in Gestione> Eventi estesi > Sessioni](../../relational-databases/extended-events/media/xevents-session-newsessions-50-objectexplorer-ssms-yoursessionnode.png)

#### <a name="edit-your-event-session"></a>Modificare la sessione eventi

In **Esplora oggetti** di SSMS è possibile modificare la sessione eventi facendo clic con il pulsante destro del mouse sul relativo nodo e quindi selezionando **Proprietà**. Viene visualizzata la stessa finestra di dialogo a più pagine.

### <a name="corresponding-t-sql-for-your-event-session"></a>T-SQL corrispondente per la sessione eventi

È stata usata l'interfaccia utente di SSMS per generare uno script T-SQL che ha creato la sessione eventi. Per visualizzare lo script generato seguire questa procedura:

- Fare clic con il pulsante destro sul nodo della sessione, quindi selezionare **Crea script per sessione** > **CREATE in** > **Appunti**.
- Incollare in qualsiasi editor di testo.

Di seguito è riportata l'istruzione T-SQL CREATE EVENT SESSION per *SessioneUtente*, che è stata generata dai clic eseguiti nell'interfaccia utente:

```sql
CREATE EVENT SESSION [YourSession]
    ON SERVER
    ADD EVENT sqlserver.sql_statement_completed
    (
        ACTION(sqlserver.sql_text)
        WHERE
        ( [sqlserver].[like_i_sql_unicode_string]([sqlserver].[sql_text], N'%SELECT%HAVING%')
        )
    )
    ADD TARGET package0.event_file
    (SET
        filename = N'C:\Junk\YourSession_Target.xel',
        max_file_size = (2),
        max_rollover_files = (2)
    )
    WITH (
        MAX_MEMORY = 2048 KB,
        EVENT_RETENTION_MODE = ALLOW_MULTIPLE_EVENT_LOSS,
        MAX_DISPATCH_LATENCY = 3 SECONDS,
        MAX_EVENT_SIZE = 0 KB,
        MEMORY_PARTITION_MODE = NONE,
        TRACK_CAUSALITY = OFF,
        STARTUP_STATE = OFF
    );
GO
```

#### <a name="pre-drop-of-the-event-session"></a>Pre-DROP della sessione eventi

Prima dell'istruzione CREATE EVENT SESSION, potrebbe essere necessario eseguire un'istruzione DROP EVENT SESSION nel caso in cui il nome esiste già.

```sql
IF EXISTS (SELECT *
      FROM sys.server_event_sessions
      WHERE name = 'YourSession')
BEGIN
    DROP EVENT SESSION YourSession
          ON SERVER;
END
go
```

#### <a name="alter-to-start-and-stop-the-event-session"></a>ALTER per avviare o arrestare la sessione eventi

Quando si crea una sessione eventi, il valore predefinito è l'avvio non automatico. È possibile avviare o arrestare la sessione eventi in qualsiasi momento usando l'istruzione T-SQL ALTER EVENT SESSION seguente.

```sql
ALTER EVENT SESSION [YourSession]
      ON SERVER
    --ON DATABASE
    STATE = START;   -- STOP;
```

È possibile indicare l'avvio automatico della sessione eventi all'avvio dell'istanza di SQL Server. Vedere la parola chiave **STARTUP STATE = ON** in CREATE EVENT SESSION.

- L'interfaccia utente di SSMS offre una casella di controllo corrispondente nella pagina **Nuova sessione** > **Generale** .

## <a name="test-your-event-session"></a>Testare la sessione eventi

Testare la sessione eventi con questi semplici passaggi:

1. In **Esplora oggetti** di SSMS fare clic con il pulsante destro del mouse sul nodo della sessione, quindi fare clic su **Avvia sessione**.
2. Eseguire alcune volte l'istruzione `SELECT...HAVING` seguente.
    - In teoria è possibile modificare il valore `HAVING Count` tra le due esecuzioni, passando tra 2 e 3. Ciò consente di visualizzare le differenze nei risultati.
3. Fare clic con il pulsante destro del nodo della sessione, quindi fare clic su **Arresta sessione**.
4. Leggere la sottosezione successiva su [come SELEZIONARE e visualizzare i risultati](#select-the-full-results-xml-37).

```sql
SELECT
        c.name,
        Count(*)  AS [Count-Per-Column-Repeated-Name]
    FROM
             sys.syscolumns  AS c
        JOIN sys.sysobjects  AS o
            ON o.id = c.id
    WHERE
        o.type = 'V'
        AND
        c.name like '%event%'
    GROUP BY
        c.name
    HAVING
        Count(*) >= 3   --2     -- Try both values during session.
    ORDER BY
        c.name;
```

Per completezza di informazioni, di seguito è riportato l'output approssimativo della precedente istruzione SELECT...HAVING.

```
/*** Approximate output, 6 rows, all HAVING Count >= 3:
name                   Count-Per-Column-Repeated-Name
---------------------  ------------------------------
event_group_type       4
event_group_type_desc  4
event_session_address  5
event_session_id       5
is_trigger_event       4
trace_event_id         3
***/
```

<a name="select-the-full-results-xml-37"/>

### <a name="select-the-full-results-as-xml"></a>SELEZIONARE i risultati completi in formato XML

In SSML eseguire l'istruzione T-SQL SELECT seguente per restituire i risultati in cui ogni riga include i dati sull'occorrenza di un evento. CAST AS XML consente di visualizzare facilmente i risultati.

> [!NOTE]
> Il sistema di eventi aggiunge sempre un numero lungo al nome file event_file *xel* specificato. Prima di poter eseguire l'istruzione SELECT dal file, è necessario copiare il nome completo specificato dal sistema e incollarlo nell'istruzione SELECT.

```sql
SELECT
        object_name,
        file_name,
        file_offset,
        event_data,
        'CLICK_NEXT_CELL_TO_BROWSE_XML RESULTS!'
                AS [CLICK_NEXT_CELL_TO_BROWSE_XML_RESULTS],
        CAST(event_data AS XML) AS [event_data_XML]
                -- TODO: In ssms.exe results grid, double-click this xml cell!
    FROM
        sys.fn_xe_file_target_read_file(
            'C:\Junk\YourSession_Target_0_131085363367310000.xel',
            null, null, null
        );
```

L'istruzione SELECT precedente offre due modi per visualizzare i risultati completi di una riga di evento specifica:

- Eseguire l'istruzione SELECT in SSMS, quindi fare clic su una cella della colonna **event_data_XML** . Si tratta di un metodo molto pratico.
- Copiare la lunga stringa XML da una cella nella colonna **event_data** . Incollare in qualsiasi editor di testo semplice come Notepad.exe e salvare la stringa in un file con estensione XML. Aprire quindi il file XML con un browser.

#### <a name="display-of-results-for-one-event"></a>Visualizzare i risultati per un evento

Ora verrà esaminata una parte dei risultati, che sono in formato XML. In questo caso il codice XML viene modificato per renderlo più breve ai fini della visualizzazione. Si noti che `<data name="row_count">` visualizza un valore di `6`, che corrisponde alle 6 righe di risultati visualizzate in precedenza. Inoltre, è visibile l'istruzione SELECT completa.

```xml
<event name="sql_statement_completed" package="sqlserver" timestamp="2016-05-24T04:06:08.997Z">
  <data name="duration">
    <value>111021</value>
  </data>
  <data name="cpu_time">
    <value>109000</value>
  </data>
  <data name="physical_reads">
    <value>0</value>
  </data>
  <data name="last_row_count">
    <value>6</value>
  </data>
  <data name="offset">
    <value>0</value>
  </data>
  <data name="offset_end">
    <value>584</value>
  </data>
  <data name="statement">
    <value>SELECT
        c.name,
        Count(*)  AS [Count-Per-Column-Repeated-Name]
    FROM
             sys.syscolumns  AS c
        JOIN sys.sysobjects  AS o

            ON o.id = c.id
    WHERE
        o.type = 'V'
        AND
        c.name like '%event%'
    GROUP BY
        c.name
    HAVING
        Count(*) &gt;= 3   --2     -- Try both values during session.
    ORDER BY
        c.name</value>
  </data>
</event>
```

## <a name="ssms-to-display-results"></a>SSMS per visualizzare i risultati

Nell'interfaccia utente di SSMS sono disponibili numerose funzionalità avanzate che è possibile usare per visualizzare i dati acquisiti da un evento esteso. I dettagli sono disponibili in:

- [Visualizzazione avanzata dei dati di destinazione da eventi estesi in SQL Server](../../relational-databases/extended-events/advanced-viewing-of-target-data-from-extended-events-in-sql-server.md)

Le nozioni di base iniziano con le opzioni del menu di scelta rapida **Visualizza dati di destinazione** e **Controlla i dati dinamici**.

### <a name="view-target-data"></a>Visualizza dati di destinazione

In **Esplora oggetti** di SSMS è possibile fare clic con il pulsante destro del mouse sul nodo di destinazione sotto il nodo della sessione eventi. Nel menu di scelta rapida fare clic su **Visualizza dati di destinazione**. SSMS visualizza i dati.

La visualizzazione non viene aggiornata man mano che nuovi dati vengono segnalati dall'evento. È possibile, tuttavia, fare clic di nuovo clic su **Visualizza dati di destinazione** .

![Visualizzare i dati di destinazione, in SSMS > Gestione > Eventi estesi > Sessioni > SessioneUtente > package0.event_file, fare doppio clic su](../../relational-databases/extended-events/media/xevents-viewtargetdata-ssms-targetnode-61.png)

### <a name="watch-live-data"></a>Controlla i dati dinamici

In **Esplora oggetti** di SSMS è possibile fare clic con il pulsante destro del mouse sul nodo della sessione eventi. Nel menu di scelta rapida fare clic su **Controlla i dati dinamici**. SSMS visualizza i dati in ingresso man mano che continuano ad arrivare in tempo reale.

![Controlla i dati dinamici, in SSMS, Gestione > Eventi estesi > Sessioni > SessioneUtente, fare clic con il pulsante destro del mouse](../../relational-databases/extended-events/media/xevents-watchlivedata-ssms-yoursessionnode-63.png)

## <a name="scenarios"></a>Scenari

Esistono innumerevoli scenari in cui è possibile usare in maniera efficace gli eventi estesi. Gli articoli seguenti forniscono scenari di esempio che comportano l'uso dei blocchi acquisiti durante le query.

Alcuni scenari specifici per le sessioni eventi finalizzate alla valutazione dei blocchi sono descritti negli articoli seguenti. Gli articoli illustrano anche alcune tecniche avanzate, ad esempio l'uso di **\@dbid** e l'uso di `EXECUTE (@YourSqlString)` dinamico:

- [Cercare gli oggetti con il maggior numero di blocchi acquisiti](../../relational-databases/extended-events/find-the-objects-that-have-the-most-locks-taken-on-them.md)
  - Questo scenario usa l'elemento package0.histogram di destinazione, che elabora i dati dell'evento non elaborati prima di visualizzarli all'utente.
- [Individuare le query che mantengono attivi i blocchi](../../relational-databases/extended-events/determine-which-queries-are-holding-locks.md)
  - Questo scenario usa l'elemento [package0.pair_matching di destinazione](/previous-versions/sql/sql-server-2016/ff878062(v=sql.130)), dove la coppia di eventi è sqlserver.lock_acquire e lock_release.

## <a name="terms-and-concepts-in-extended-events"></a>Termini e concetti negli eventi estesi

La tabella seguente elenca i termini usati per gli eventi estesi e ne descrive i significati.

| Termine | Descrizione |
| :--- | :---------- |
| sessione eventi | Un costrutto incentrato su uno o più eventi, oltre a elementi di supporto quali azioni e destinazioni. L'istruzione CREATE EVENT SESSION viene costruita a ogni sessione eventi. È possibile modificare una sessione eventi in modo da avviarla e arrestarla in base alle proprie esigenze. <br/> <br/> Una sessione eventi viene a volte indicata semplicemente come *sessione*, quando il dal contesto si evince che indica la *sessione eventi*. <br/> <br/> Altre informazioni sulle sessioni eventi sono disponibili in: [Sessioni Eventi estesi di SQL Server](../../relational-databases/extended-events/sql-server-extended-events-sessions.md). |
| evento | Un'occorrenza specifica del sistema che viene controllata da una sessione eventi attiva. <br/> <br/> Ad esempio, l'evento *sql_statement_completed* rappresenta il momento in cui viene completata un'istruzione T-SQL. L'evento può segnalare la durata e altri dati. |
| target | Un elemento che riceve i dati di output da un evento acquisito. La destinazione visualizza i dati all'utente. <br/> <br/> Gli esempi includono *event_file* e la memoria *ring_buffer*, che rappresenta una versione più pratica e leggera della destinazione event_file. La destinazione *histogram* esegue alcune operazioni di elaborazione dei dati prima di visualizzarli. <br/> <br/> Qualsiasi destinazione può essere usata per qualsiasi sessione eventi. Per informazioni dettagliate, vedere [Destinazioni per gli eventi estesi in SQL Server](../../relational-databases/extended-events/targets-for-extended-events-in-sql-server.md). |
| action | Un campo noto all'evento. I dati del campo vengono inviati alla destinazione. Il campo azione è strettamente correlato al *filtro predicato*. |
| filtro predicato | Test di dati in un campo evento, usato in modo che solo un subset interessante di occorrenze dell'evento venga inviato alla destinazione. <br/> <br/> Ad esempio, un filtro può includere solo le occorrenze dell'evento *sql_statement_completed* in cui l'istruzione T-SQL conteneva la stringa *HAVING*. |
| Pacchetto | Un qualificatore del nome associato a ogni elemento in un set di elementi incentrato su un core di eventi. <br/> <br/> Ad esempio, un pacchetto potrebbe avere eventi sul testo T-SQL, un evento potrebbe riguardare T-SQL in un batch delimitato da GO, un evento più circoscritto potrebbe riguardare le singole istruzioni T-SQL. Inoltre, per qualsiasi istruzione T-SQL sono disponibili gli eventi iniziali e completati. <br/> <br/> Anche i campi appropriati per gli eventi sono inclusi nel pacchetto con gli eventi. La maggior parte delle destinazioni si trova in *package0* e viene usata con gli eventi di molti altri pacchetti. |

## <a name="how-to-discover-the-available-events-in-packages"></a>Come individuare gli eventi disponibili nei pacchetti

L'istruzione T-SQL SELECT seguente restituisce una riga per ogni evento disponibile il cui nome contiene la stringa di tre caratteri 'sql'. Naturalmente, è possibile modificare il valore LIKE per cercare nomi di eventi diversi. Le righe mostrano anche il nome del pacchetto che contiene l'evento.

```sql
SELECT   -- Find an event you want.
        p.name         AS [Package-Name],
        o.object_type,
        o.name         AS [Object-Name],
        o.description  AS [Object-Descr],
        p.guid         AS [Package-Guid]
    FROM
              sys.dm_xe_packages  AS p
        JOIN  sys.dm_xe_objects   AS o
                ON  p.guid = o.package_guid
    WHERE
        o.object_type = 'event'   --'action'  --'target'
        AND
        p.name LIKE '%'
        AND
        o.name LIKE '%sql%'
    ORDER BY
        p.name, o.object_type, o.name;
```

La visualizzazione seguente mostra la riga restituita, modificata in questo caso nel formato di colonna nome = valore. I dati provengono dall'evento *sql statement_completed* usato nei passaggi di esempio precedenti. La frase per la colonna oggetto Object-Descr è particolarmente utile.

```
Package-Name = sqlserver
object_type  = event
Object-Name  = sql_statement_completed
Object-Descr = Occurs when a Transact-SQL statement has completed.
Package-Guid = 655FD93F-3364-40D5-B2BA-330F7FFB6491
```

### <a name="ssms-ui-for-search"></a>Interfaccia utente di SSMS per la ricerca

Un'altra opzione di ricerca consiste nell'usare l'interfaccia utente di SSMS per la finestra di dialogo **Nuova sessione** > **Eventi** > **Libreria di eventi** visualizzata in una schermata precedente.

### <a name="sql-trace-event-classes-with-extended-events"></a>Classi di evento di Traccia SQL, con eventi estesi

Una descrizione dell'uso degli eventi estesi con colonne e classi di evento di Traccia SQL è disponibile in: [Visualizzare gli eventi estesi equivalenti alle classi di eventi di Traccia SQL](../../relational-databases/extended-events/view-the-extended-events-equivalents-to-sql-trace-event-classes.md)

### <a name="event-tracing-for-windows-etw-with-extended-events"></a>Event Tracing for Windows (ETW), con eventi estesi

Le descrizioni dell'uso degli eventi estesi con Event Tracing for Windows (ETW) sono disponibili in:

- [Destinazione di Event Tracing for Windows](../../relational-databases/extended-events/event-tracing-for-windows-target.md)
- [Monitorare l'attività del sistema mediante gli eventi estesi](../../relational-databases/extended-events/monitor-system-activity-using-extended-events.md)

## <a name="additional-items"></a>Elementi aggiuntivi

Questa sezione tratta brevemente alcuni argomenti di contenuto vario.

### <a name="event-sessions-installed-with-sql-server"></a>Sessioni eventi installate con SQL Server

SQL Server viene fornito con alcuni eventi estesi già creati. Tutti sono configurati per essere avviati automaticamente all'avvio del sistema SQL. Le sessioni eventi raccolgono i dati che potrebbero risultare utili in caso di errore di sistema. Analogamente a tutti gli eventi estesi, utilizzano solo una piccola quantità di risorse e Microsoft consiglia di eseguirle da sole.

È possibile visualizzare le sessioni eventi in **Esplora oggetti** di SSMS in **Gestione** > **Eventi estesi** > **Sessioni**.  Al mese di giugno 2016 l'elenco di queste sessioni eventi installate è il seguente:

- AlwaysOn_health
- system_health
- telemetry_events

### <a name="powershell-provider-for-extended-events"></a>Provider PowerShell per eventi estesi

È possibile gestire gli eventi estesi di SQL Server usando il provider PowerShell per SQL Server. I dettagli sono disponibili in: [Usare il provider PowerShell per eventi estesi](../../relational-databases/extended-events/use-the-powershell-provider-for-extended-events.md)

### <a name="system-views-for-extended-events"></a>Viste di sistema per gli eventi estesi

Le viste di sistema per gli eventi estesi includono:

- *Viste del catalogo:* per informazioni sulle sessioni eventi che sono stati definiti da CREATE EVENT SESSION.

- *Viste a gestione dinamica (DMV):* per informazioni sulle sessioni eventi in esecuzione.

[Istruzioni SELECT e JOIN da viste di sistema per eventi estesi in SQL Server](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md) : forniscono informazioni su:

- Come creare un join delle viste.
- Diverse istruzioni SELECT utili delle viste.
- La correlazione tra:
  - Colonne delle viste.
  - Clausole CREATE EVENT SESSION.
  - Controlli dell'interfaccia utente di SSMS.

## <a name="appendix-selects-to-ascertain-permission-owner-in-advance"></a><a name="appendix1"></a> Appendice: Istruzioni SELECT per verificare in anticipo il proprietario delle autorizzazioni

Le autorizzazioni indicate in questo articolo sono:

- ALTER ANY EVENT SESSION
- VIEW SERVER STATE
- CONTROL SERVER

Le istruzioni Transact-SQL SELECT seguenti possono segnalare l'identità del proprietario di tali autorizzazioni.

### <a name="union-direct-permissions-plus-role-derived-permissions"></a>Autorizzazioni dirette UNION più autorizzazioni derivate dal ruolo

L'istruzione SELECT...UNION ALL seguente restituisce le righe che mostrano l'identità di chi ha le autorizzazioni necessarie per la creazione di sessioni eventi e l'esecuzione di query nelle viste del catalogo di sistema per gli eventi estesi.

```sql
-- Ascertain who has the permissions listed in the ON clause.
-- 'CONTROL SERVER' permission includes the permissions
-- 'ALTER ANY EVENT SESSION' and 'VIEW SERVER STATE'.
SELECT
        'Owner-is-Principal'  AS [Type-That-Owns-Permission],
        NULL                  AS [Role-Name],
        prin.name             AS [Owner-Name],

        perm.permission_name
            COLLATE Latin1_General_CI_AS_KS_WS
            AS [Permission-Name]
    FROM
             sys.server_permissions  AS perm
        JOIN sys.server_principals   AS prin

            ON prin.principal_id = perm.grantee_principal_id
    WHERE
        perm.permission_name IN
            ('ALTER ANY EVENT SESSION',
            'VIEW SERVER STATE',
            'CONTROL SERVER')
UNION ALL

-- Plus check for members of the 'sysadmin' fixed server role,
-- because 'sysadmin' includes the 'CONTROL SERVER' permission.
SELECT
        'Owner-is-Role'
        , prin.name  -- [Role-Name]
        , CAST( (IsNull(pri2.name, N'No members'))
            AS nvarchar(128))
        , NULL
    FROM
        sys.server_role_members  AS rolm
        RIGHT OUTER JOIN sys.server_principals    AS prin
            ON prin.principal_id = rolm.role_principal_id
        LEFT OUTER JOIN sys.server_principals     AS pri2
            ON rolm.member_principal_id = pri2.principal_id
    WHERE
        prin.name = 'sysadmin'
    ORDER BY
        1,2,3,4;
```

### <a name="has_perms_by_name-function"></a>HAS_PERMS_BY_NAME - funzione

L'istruzione SELECT seguente segnala le autorizzazioni. Si basa sulla funzione predefinita [HAS_PERMS_BY_NAME](../../t-sql/functions/has-perms-by-name-transact-sql.md).

Inoltre, se si ha l'autorità di *rappresentare* temporaneamente altri account, è possibile rimuovere il commento dalle istruzioni [EXECUTE AS LOGIN](../../t-sql/statements/execute-as-transact-sql.md) e REVERT per richiedere informazioni sugli altri account.

```sql
--EXECUTE AS LOGIN = 'AccountNameHere';
SELECT HAS_PERMS_BY_NAME
    (
       null
       , null
       , 'ALTER ANY EVENT SESSION'
    );
--REVERT;
```

### <a name="security-links"></a>Collegamenti di sicurezza

Di seguito sono forniti collegamenti alla documentazione relativa a queste istruzioni SELECT e alle autorizzazioni:

- Informazioni dettagliate sulla funzione predefinita [HAS_PERMS_BY_NAME (Transact-SQL)](../../t-sql/functions/has-perms-by-name-transact-sql.md)
- [sys.fn_my_permissions (Transact-SQL)](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)
- [GRANT - autorizzazioni per server (Transact-SQL)](../../t-sql/statements/grant-server-permissions-transact-sql.md)
- [sys.server_principals (Transact-SQL)](../system-catalog-views/sys-server-principals-transact-sql.md)
- Blog: [Effective Database Engine Permissions](https://social.technet.microsoft.com/wiki/contents/articles/15180.effective-database-engine-permissions.aspx) (Autorizzazioni del motore di database valide)
- [poster](https://aka.ms/sql-permissions-poster)ingrandibile, in formato PDF, che visualizza la gerarchia di tutte le autorizzazioni di SQL Server.

## <a name="links-to-supporting-information"></a>Collegamenti a informazioni di supporto

- [sys.fn_xe_file_target_read_file (Transact-SQL)](../../relational-databases/system-functions/sys-fn-xe-file-target-read-file-transact-sql.md)