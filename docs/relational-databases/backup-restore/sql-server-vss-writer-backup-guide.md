---
title: Applicazioni di backup SQL Server - Servizio Copia Shadow del volume e writer SQL
description: Descrive il componente writer SQL e il ruolo che svolge nel processo di creazione e ripristino di snapshot Servizio Copia Shadow del volume per i database SQL Server.
ms.custom: ''
ms.date: 07/24/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
author: cawrites
ms.author: chadam
ms.openlocfilehash: 36fbf9ac4010797561e0852b96b6df6fd85b640d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347302"
---
# <a name="sql-server-back-up-applications---volume-shadow-copy-service-vss-and-sql-writer"></a>Applicazioni di backup SQL Server - Servizio Copia Shadow del volume e writer SQL
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

SQL Server offre supporto per il Servizio Copia Shadow del volume fornendo un writer (il writer SQL) che consente a un'applicazione di backup di terze parti di usare il framework Servizio Copia Shadow del volume per eseguire il backup dei file di database. Questo documento descrive il componente writer SQL e il ruolo che svolge nel processo di creazione e ripristino di snapshot Servizio Copia Shadow del volume per i database SQL Server. Acquisisce anche informazioni dettagliate su come configurare e usare il writer SQL per lavorare con le applicazioni di backup nel framework Servizio Copia Shadow del volume.


## <a name="introduction"></a>Introduzione

SQL Server fornisce supporto per la creazione di snapshot dai dati di SQL Server usando il Servizio Copia Shadow del volume. A tale scopo viene fornito un writer compatibile con il Servizio Copia Shadow del volume (il writer SQL) che consente a un'applicazione di backup di terze parti di usare il framework Servizio Copia Shadow del volume per eseguire il backup dei file di database. Questo documento descrive il componente writer SQL e il ruolo che svolge nel processo di creazione e ripristino di snapshot Servizio Copia Shadow del volume per i database SQL Server. Acquisisce anche informazioni dettagliate su come configurare e usare il writer SQL per lavorare con le applicazioni di backup nel contesto del framework Servizio Copia Shadow del volume.

## <a name="definition-of-terms"></a>Definizione dei termini

- **Virtual Device Interface**: SQL Server fornisce un'API denominata VDI (Virtual Device Interface) che consente ai fornitori di software indipendenti di integrare SQL Server nei propri prodotti offrendo supporto per operazioni di backup e di ripristino. Queste API sono state progettate per offrire affidabilità e prestazioni ottimali e per supportare la gamma completa di funzionalità di backup e di ripristino di SQL Server, incluse tutte le capacità di backup a caldo e di snapshot. Per altre informazioni, vedere [Specifica di SQL Server 2005 Virtual Backup Device Interface](https://www.microsoft.com/download/details.aspx?id=17282). 

- **Richiedente**: un processo (automatizzato o GUI) che richiede che uno o più set di snapshot vengano acquisiti da uno o più volumi originali. In questo documento il termine richiedente indica anche un'applicazione di backup che crea uno snapshot dei database di SQL Server.

## <a name="about-vss"></a>Informazioni sul Servizio Copia Shadow del volume

Il Servizio Copia Shadow del volume fornisce l'infrastruttura di sistema per l'esecuzione di applicazioni del Servizio Copia Shadow del volume nei sistemi Windows. Nonostante sia per lo più trasparente sia per l'utente che per lo sviluppatore, il Servizio Copia Shadow del volume esegue le operazioni seguenti:

- Coordina le attività di provider, writer e richiedenti durante la creazione e l'uso di copie shadow.
- Fornisce il provider di sistema predefinito.
- Implementa le funzionalità dei driver di base necessarie per il funzionamento di qualsiasi provider.

Il Servizio viene avviato on demand, quindi, perché le operazioni del Servizio Copia Shadow del volume abbiano esito positivo, questo servizio deve essere abilitato.

## <a name="vss-components"></a>Componenti del Servizio Copia Shadow del volume

Il Servizio Copia Shadow del volume coordina le attività dei componenti interoperativi seguenti: 

- I provider sono proprietari dei dati delle copie shadow e creano istanze delle copie shadow.
- I writer sono applicazioni che modificano i dati e partecipano al processo di sincronizzazione delle copie shadow.
- I richiedenti avviano la creazione e l'eliminazione delle copie shadow. La progettazione è basata sullo scenario in cui il richiedente è un'applicazione di backup.

Il Servizio Copia Shadow del volume fornisce il coordinamento tra le parti seguenti: 

![Diagramma che mostra come il Servizio Copia Shadow del volume fornisce il coordinamento tra queste parti.](media/sql-server-vss-writer-backup-guide/coordinations.png)

Questo diagramma mostra tutti i componenti che partecipano a una tipica attività di snapshot del Servizio Copia Shadow del volume. In uno scenario di questo tipo SQL Server (incluso il writer SQL) funge da writer in una delle caselle Writer.  Altri writer di questo tipo potrebbero essere Exchange Server e così via.

Nella parte restante di questo documento si presuppone che il lettore abbia familiarità con i termini del framework Servizio Copia Shadow del volume.  Per informazioni dettagliate, vedere la documentazione sul [Servizio Copia Shadow del volume](/windows/win32/vss/volume-shadow-copy-service-overview).

## <a name="about-sql-writer"></a>Informazioni sul writer SQL

Il writer SQL è un VSS writer fornito da SQL Server. Gestisce l'interazione del Servizio Copia Shadow del volume con SQL Server. Il writer SQL viene fornito con SQL Server come servizio autonomo e viene installato durante l'installazione di SQL Server. Per impostazione predefinita, il writer SQL non è abilitato. È necessario abilitarlo in modo esplicito per eseguirlo nel computer server.

Ruolo del writer SQL in un'operazione di backup di snapshot del Servizio Copia Shadow del volume: 

![Snapshot del Servizio Copia Shadow del volume](media/sql-server-vss-writer-backup-guide/sql-vss-snapshot.png)



## <a name="configuring-the-sql-writer"></a>Configurazione del writer SQL

Il servizio writer SQL viene installato nel sistema durante l'installazione di SQL Server ed è configurato per l'avvio automatico all'avvio di Windows. 

### <a name="sql-writer-service-account"></a>Account del servizio writer SQL

Durante l'installazione, verrà installato l'account del writer SQL per usare l'account di sistema locale. Poiché è necessario che il writer SQL comunichi con SQL Server usando API VDI esclusive, l'account del writer SQL deve avere diritti di accesso sufficienti sia per SQL Server che per il Servizio Copia Shadow del volume.  La configurazione del servizio come account di sistema locale fornisce diritti sufficienti per la corretta esecuzione del servizio.

  > [!NOTE]
  > Per il corretto funzionamento del servizio writer SQL, è importante assicurarsi che l'account di sistema locale non venga rimosso dal ruolo "sa" dell'istanza di SQL Server.

### <a name="enabling-and-starting-sql-writer"></a>Abilitazione e avvio del writer SQL

Per avviare e usare il writer SQL, è necessario seguire questa procedura:

Il servizio writer SQL può essere abilitato contrassegnandolo come "Automatico". Per aprire i servizi tramite il Pannello di controllo, fare clic sul pulsante Start, scegliere Pannello di controllo, fare doppio clic su Strumenti di amministrazione, quindi su Servizi. Nel riquadro Servizi fare doppio clic sul servizio writer SQL e impostare la proprietà "Tipo di avvio" su "Automatico".

Il servizio dovrebbe venire avviato anche selezionando il pulsante "Avvia" sotto la proprietà "Stato del servizio" nella schermata delle proprietà del servizio indicata in precedenza.

Per praticità, il servizio writer SQL apporterà automaticamente queste modifiche al primo avvio.

  > [!NOTE]
  > Nei casi in cui è installata un'istanza di SQL Server Express e un'applicazione usa la funzionalità Istanze utente, il writer SQL potrebbe essere avviato automaticamente da SQL Server allo scopo di facilitare l'enumerazione di queste istanze utente durante un'operazione di backup del Servizio Copia Shadow del volume. 

## <a name="backuprestore-operations-and-supported-versions"></a>Operazioni di backup/ripristino e versioni supportate

### <a name="version-support"></a>Supporto versione

Il writer SQL viene fornito con SQL Server e supporta solo istanze di SQL Server. 

Il writer SQL enumera anche le istanze di SQL Server Express. Le istanze utente avviate da SQL Server Express verranno enumerate anche dal writer SQL.


## <a name="supported-backuprestore-operations"></a>Operazioni di backup/ripristino supportate

SQL Server (tramite il writer SQL) supporterà le modalità seguenti delle operazioni di backup basate sul Servizio Copia Shadow del volume:

- Non basate su componenti
- Basate su componenti

### <a name="noncomponent-based-backup-operations"></a>Operazioni di backup non basate su componenti

I backup non basati su componenti selezionano in modo implicito i database usando l'elenco di volumi nel set di snapshot. Il writer SQL controlla la presenza di database incompleti e, se ne trova, genera un errore. Un database incompleto è uno in cui un subset di file viene selezionato dall'elenco di volumi.

Nel modello non basato su componenti, sono supportati solo i database con il modello di recupero con registrazione minima. Il roll forward in seguito a un ripristino non è supportato.

### <a name="component-based-backup-operations"></a>Operazioni di backup basate su componenti

I backup basati su componenti sono preferibili e consigliati con il writer SQL, perché l'applicazione (applicazione di backup Servizio Copia Shadow del volume) selezionerà in modo esplicito i database dai metadati restituiti dal writer SQL. Il set di snapshot deve includere tutti i volumi necessari per eseguire il backup di tali database. L'infrastruttura del Servizio Copia Shadow del volume non aggiunge automaticamente i volumi necessari per il set di database selezionato. Tutti i volumi di backup devono essere inclusi nel set di snapshot dei volumi. È responsabilità dell'applicazione di backup verificare che tutti i volumi di backup siano inclusi nel set di snapshot.  Il writer SQL rileverà i database incompleti (con volumi di backup non compresi nel set di snapshot) e il backup avrà esito negativo.

Nella parte restante di questo argomento si presuppone che i backup basati su componenti vengano usati durante il processo di creazione di snapshot del Servizio Copia Shadow del volume per SQL Server.

## <a name="features-supported-by-sql-writer"></a>Funzionalità supportate dal writer SQL


- **Full-text**: il writer SQL segnala i contenitori di catalogo full-text con specifiche di file ricorsive sotto i componenti del database nel documento di metadati del writer.  Vengono inclusi automaticamente nel backup quando viene selezionato il componente del database
- **Backup/Ripristino differenziale**: il writer SQL supporta il backup/ripristino differenziale tramite due meccanismi differenziali del Servizio Copia Shadow del volume: file parziale e file differenziato in base all'ora dell'ultima modifica.
- **File parziale**:   il writer SQL usa il meccanismo di file parziale del Servizio Copia Shadow del volume per segnalare gli intervalli di byte modificati all'interno dei file di database.  
- **File differenziato in base all'ora dell'ultima modifica**: il writer SQL usa il meccanismo di file differenziato in base all'ora dell'ultima modifica del Servizio Copia Shadow del volume per segnalare i file modificati nei cataloghi full-text.
- **Ripristino con spostamento**: il writer SQL supporta la specifica di una nuova destinazione del Servizio Copia Shadow del volume durante il ripristino.  La specifica di una nuova destinazione del Servizio Copia Shadow del volume consente di rilocare un database/file di log o un contenitore di cataloghi full-text durante l'operazione di ripristino.
- **Ridenominazione del database**: un richiedente potrebbe dover ripristinare un database SQL con un nuovo nome, soprattutto se il database deve essere ripristinato side-by-side con il database originale. Il writer SQL supporta la ridenominazione di un database durante l'operazione di ripristino, purché il database rimanga all'interno dell'istanza di SQL originale.
- **Backup solo copia**: a volte è necessario eseguire un backup destinato a uno scopo specifico, ad esempio quando è necessario creare una copia di un database a scopo di test.  Questo backup non avrà alcun effetto sulle procedure di backup e ripristino generali per il database. L'uso dell'opzione COPY_ONLY specifica che il backup viene eseguito "fuori banda" e non influirà sulla normale sequenza di backup. Il writer SQL supporta il tipo di backup "solo copia" con le istanze SQL Server.
- **Recupero automatico dello snapshot del database**:   in genere uno snapshot di un database di SQL Server ottenuto usando il framework del Servizio Copia Shadow del volume è in uno stato non recuperato. Non è possibile accedere in modo sicuro ai dati dello snapshot prima della fase di recupero per eseguire il rollback delle transazioni in corso e porre il database in uno stato coerente. È possibile per un'applicazione di backup Servizio Copia Shadow del volume richiedere il recupero automatico degli snapshot, durante il processo di creazione degli snapshot.

Queste nuove funzionalità e il relativo utilizzo sono descritte in modo più dettagliato in Dettagli delle opzioni di backup e ripristino in questo argomento.

### <a name="what-is-not-supported"></a>Funzionalità non supportate

- I backup del log non sono supportati dal writer SQL.
- Il backup di file/filegroup non è supportato.
- Il ripristino della pagina non è supportato.
- Gli snapshot del database non sono supportati e vengono ignorati durante la creazione di snapshot del Servizio Copia Shadow del volume sia di componenti che di non componenti. 
- Database con chiusura automatica o database con arresto abilitato.
- Linux non fornisce un framework Servizio Copia Shadow del volume e quindi il writer SQL non è disponibile in Linux

La tabella seguente elenca i tipi di backup di snapshot supportati dal writer SQL o da SQL Server che usa il framework Servizio Copia Shadow del volume per tutte le edizioni di SQL Server.

| **Operazione di backup/ripristino**                 | **Basata su componenti**           | **Non basata su componenti** |
|:-------------------------------------------- | :---------------------------- | :--------------------- |
|Backup completo dei dati </br> (incluso il catalogo full-text)| Sì                     | Sì                    |
|Ripristino completo                                  | Sì                           | Sì                    |
|Ripristino completo (nessun recupero)                    | Sì                           | No                     |
|Backup differenziale                           | Sì                           | No                     |
|Ripristino differenziale                          | Sì                           | No                     |
|Ripristino con spostamento                             | Sì                           | No                     |
|Ridenominazione del database                               | Sì                           | No                     |
|Backup di sola copia                              | Sì                           | No                     |
|Snapshot recuperati automaticamente                       | Sì                           | No                     |
|Backup del log                                    | No                            | No                     |
|Snapshot del database                            | No                            | No                     |
|Database con chiusura automatica</br> Database con arresto | Sì                        | No                     |
|Database di gruppi di disponibilità                  | Sì                           | Non su componenti secondari        | 




## <a name="snapshot-creation-process"></a>Processo di creazione degli snapshot

Il framework Servizio Copia Shadow del volume coordina le attività di un richiedente (un'applicazione di backup) e del writer SQL durante la creazione degli snapshot di SQL Server. Per abilitare questo coordinamento, il framework Servizio Copia Shadow del volume definisce le interfacce del richiedente e del writer.  Queste interfacce devono essere implementate dalle applicazioni richiedenti e dai writer partecipanti. Il writer SQL implementa le interfacce del writer necessarie. Durante il processo di creazione degli snapshot, le interfacce del writer SQL vengono chiamate dal framework Servizio Copia Shadow del volume. Il writer SQL interagisce con le istanze di SQL Server nel sistema per facilitare la creazione degli snapshot.

Il framework Servizio Copia Shadow del volume definisce un set di API che verrà utilizzato da un'applicazione richiedente o di backup. È necessario che lo sviluppatore di un'applicazione di backup segua questi modelli di chiamata API per interagire con il processo di creazione degli snapshot del framework Servizio Copia Shadow del volume. Le sezioni successive descrivono il processo di creazione degli snapshot dal punto di vista del writer SQL e illustrano in dettaglio alcune interazioni interne tra il richiedente, il framework Servizio Copia Shadow del volume, il writer SQL e le istanze di SQL Server. Per altre informazioni su questi passaggi e per informazioni dettagliate sulle interfacce del framework Servizio Copia Shadow del volume, vedere la documentazione del Servizio Copia Shadow del volume.    

  > [!NOTE]
  > Si presuppone che il lettore abbia familiarità con il framework del Servizio Copia Shadow del volume e in generale con il processo di creazione di backup. Queste sezioni contengono informazioni aggiuntive su come il writer SQL partecipa al processo di creazione di backup del Servizio Copia Shadow del volume.

## <a name="snapshot-creation-workflow"></a>Flusso di lavoro di creazione degli snapshot

![Flusso di dati](media/sql-server-vss-writer-backup-guide/dataflow-chart.png)

L'immagine mostra il diagramma del flusso di dati durante un'operazione di creazione/backup di snapshot basata su componenti. Per comprendere appieno le attività di base dell'esecuzione di un backup, è utile suddividere questa panoramica nelle fasi seguenti:

- Inizializzazione del backup
- Fase di individuazione del backup
- Attività di pre-backup
- Backup effettivo dei file
- Terminazione del backup



### <a name="backup-initialization"></a>Inizializzazione del backup

Durante questa fase del backup il richiedente (l'applicazione di backup) esegue l'associazione all'interfaccia dello snapshot **IvssBackupComponents** e la inizializza in preparazione al backup. Chiama anche l'API del Servizio Copia Shadow del volume **IVssGatherWriterMetadata** per indicare al framework Servizio Copia Shadow del volume di raccogliere i metadati da tutti i writer.

Il framework Servizio Copia Shadow del volume chiamerà ogni writer registrato, incluso il writer SQL, per i metadati dei writer usando l'evento **OnIdentify**. Il writer SQL eseguirà una query sulle istanze di SQL Server per ottenere le informazioni sui metadati di backup per ogni database e creare il documento di metadati del writer. Questa fase è detta anche enumerazione dei metadati.

Il documento di metadati del writer è un documento che contiene le informazioni passate dal writer al richiedente (applicazione di backup). Il documento di metadati del writer contiene le informazioni seguenti:

- ID e nome descrittivo dell'applicazione.
- Posizione dei file e dei componenti.
- File da includere o escludere in un backup.
- Opzioni da usare in fase di ripristino.
- Queste informazioni vengono passate al richiedente tramite il framework Servizio Copia Shadow del volume.

### <a name="sql-writer-metadata-document"></a>Documento di metadati del writer SQL

Si tratta di un documento XML creato da un writer (in questo caso, il writer SQL) usando l'interfaccia **IVssCreateWriterMetadata** e contenente informazioni sullo stato e sui componenti del writer. I dettagli strutturali di un documento di metadati del writer sono descritti nella documentazione dell'API del Servizio Copia Shadow del volume. Di seguito sono riportati alcuni dettagli del documento di metadati del writer SQL.

- Informazioni di identificazione del writer
    - **Nome del writer**: L"SqlServerWriter"
    - **ID classe del writer**: 0xa65faa63, 0x5ea8, 0x4ebc, 0x9d, 0xbd, 0xa0, 0xc4, 0xdb, 0x26, 0x91, 0x2a 
    - **ID istanza del writer**: L"SQL Server:SQLWriter" 
    - **VSSUsageType**: VSS_UT_USERDATA 
    - **VSSSourceType**: VSS_ST_TRANSACTEDDB 
- Informazioni a livello di writer: VSS_APP_BACK_END 
- Specifica del metodo di ripristino: VSS_RME_RESTORE_IF_CAN_REPLACE.
- Schema di backup supportato (API IVssCreateWriterMetadata::SetBackupSchema)
    - VSS_BS_DIFFERENTIAL: backup differenziale
    - VSS_BS_TIMESTAMPED: basato su timestamp, per i file di catalogo full-text.
    - VSS_BS_LAST_MODIFY: backup differenziale basato sull'ora dell'ultima modifica.
    - VSS_BS_WRITER_SUPPORTS_NEW_TARGET: supporta l'opzione della nuova posizione di destinazione.
    - VSS_BS_WRITER_SUPPORTS_RESTORE_WITH_MOVE: supporta il ripristino "con spostamento"
    - VSS_BS_COPY: supporta l'opzione di backup "solo copia".
- Informazioni a livello di componente: contengono informazioni specifiche a livello di componente fornite dal writer SQL.
   - **Tipo**: VSS_CT_FILEGROUP
   - **Nome**: nome del componente (nome del database)
   - **Percorso logico** dell'istanza del server (in formato "server\nome-istanza" per le istanze denominate e "server" per l'istanza predefinita).
   - **Flag dei componenti**
   - **VSS_CF_APP_ROLLBACK_RECOVERY**: indica che gli snapshot di SQL Server richiedono sempre una fase di "ripristino" per rendere i file coerenti e utilizzabili per gli scenari non di backup (ovvero per il rollback delle app).
   - Selectable - True
   - Selectable for Restore - True 
   - Metodi di ripristino supportati: VSS_RME_RESTORE_IF_CAN_REPLACE

L'unica estensione della struttura del set di componenti in SQL Server è l'introduzione di cataloghi full-text.  I cataloghi full-text sono directory contenitore, che non possono essere espresse come database o file di log del Servizio Copia Shadow del volume, dato che il database e i file di log del Servizio Copia Shadow del volume non hanno una specifica ricorsiva.  Il writer SQL userà quindi un componente del filegroup del Servizio Copia Shadow del volume (VSS_CT_FILEGROUP) per rappresentare il componente a livello di database e i file filegroup per rappresentare i file di database, di log e di catalogo full-text.

Alla fine di questo documento viene fornito un documento di metadati del writer di esempio.

### <a name="backup-discovery"></a>Individuazione del backup
In questa fase, un richiedente esamina il documento di metadati del writer e crea e compila un **documento dei componenti di backup** che elenca ogni componente di cui è necessario eseguire il backup. Specifica anche le opzioni e i parametri di backup necessari come parte di questo documento. Per il writer SQL, ogni istanza di database di cui è necessario eseguire il backup è un componente separato.

#### <a name="backup-components-document"></a>Documento dei componenti di backup
Si tratta di un documento XML creato da un richiedente (tramite l'interfaccia **IVssBackupComponents**) durante la configurazione di un'operazione di ripristino o di backup. Il documento dei componenti di backup contiene un elenco dei componenti inclusi in modo esplicito, da uno o più writer, che partecipano a un'operazione di backup o di ripristino. Non contiene informazioni sui componenti inclusi in modo implicito. Al contrario, un documento di metadati del writer contiene solo i componenti del writer che possono partecipare a un backup. I dettagli strutturali di un documento dei componenti di backup sono descritti nella documentazione dell'API del Servizio Copia Shadow del volume.

#### <a name="prebackup-tasks"></a>Attività di pre-backup
Le attività di pre-backup nel Servizio Copia Shadow del volume sono relative alla creazione di una copia shadow dei volumi contenenti i dati per il backup. L'applicazione di backup salverà i dati della copia shadow, non del volume effettivo.

I richiedenti in genere attendono i writer durante la preparazione del backup e durante la creazione della copia shadow. Se il writer SQL partecipa all'operazione di backup, deve configurare i file e se stesso per prepararsi al backup e alla copia shadow.

#### <a name="prepare-for-backup"></a>Preparare il backup
Il richiedente dovrà impostare il tipo di operazione di backup da eseguire (**IVssBackupComponents::SetBackupState**) e quindi inviare una notifica ai writer tramite il Servizio Copia Shadow del volume, per preparare un'operazione di backup usando **IVssBackupComponents::PrepareForBackup**.

Al writer SQL viene concesso l'accesso al documento dei componenti di backup, che elenca dettagliatamente i database di cui è necessario eseguire il backup. Tutti i volumi di backup devono essere inclusi nel set di snapshot dei volumi. Il writer SQL rileverà i database incompleti (con volumi di backup non compresi nel set di snapshot) e il backup avrà esito negativo durante l'evento PostSnapshot.

**Avviare lo snapshot** Il richiedente avvierà il processo di snapshot chiamando l'interfaccia DoSnapshotSet del framework Servizio Copia Shadow del volume.

**Creare lo snapshot** Questa fase prevede una serie di interazioni tra il framework Servizio Copia Shadow del volume e il writer SQL.

1. _Preparare lo snapshot_. Il writer SQL chiamerà SQL Server per preparare la creazione dello snapshot.
1. _Bloccare_. Il writer SQL chiamerà SQL Server per bloccare tutte gli I/O del database per ogni database di cui viene eseguito il backup nello snapshot. Una volta che l'evento di blocco verrà restituito al framework Servizio Copia Shadow del volume, Servizio Copia Shadow del volume creerà lo snapshot.
1. _Sbloccare_. In questo evento il writer SQL chiamerà le istanze di SQL Server per sbloccare o riprendere le normali operazioni di I/O.


La fase di creazione dello snapshot è veloce (meno di 60 secondi), per impedire il blocco di tutte le operazioni di scrittura nel database.

**Post-snapshot** Se è necessario il recupero automatico per lo snapshot, il writer SQL lo eseguirà per ogni database selezionato per lo snapshot. Per una spiegazione dettagliata, vedere Snapshot recuperati automaticamente.

#### <a name="actual-backup-of-files"></a>Backup effettivo dei file

In questa fase, il richiedente può spostare i dati in un supporto di backup, se necessario. Le interazioni in questa fase avvengono tra il richiedente e il framework Servizio Copia Shadow del volume. Il writer SQL non è coinvolto.

1. Ottenere lo stato del writer. Restituisce lo stato dei writer. Il richiedente potrebbe dover gestire eventuali errori.
1. Eseguire il backup.

In questa fase, il richiedente può spostare i dati in un supporto di backup, se necessario.

#### <a name="backup-complete"></a>Backup completato

Questo evento indicherà che il backup è stato completato correttamente.

È anche il momento in cui il writer SQL può eseguire il commit del backup come base differenziale, se il backup corrente è un backup completo del database (e non un backup di solo copia).


  > [!NOTE]
  > Il richiedente deve inviare in modo esplicito questo evento (evento Backup completo) per consentire al writer SQL di eseguire il commit dei backup di base differenziale. Se questo evento non viene ricevuto, il backup creato non sarà un backup di "base differenziale" idoneo.

#### <a name="save-writer-metadata"></a>Salvare i metadati del writer

Il richiedente salverà il documento dei componenti di backup e i metadati di backup di ogni componente insieme ai dati di cui è stato eseguito il backup dallo snapshot. Questi metadati del writer sono necessari al writer SQL o a SQL Server per le operazioni di ripristino.

#### <a name="backup-termination"></a>Terminazione del backup

Il richiedente termina la copia shadow rilasciando l'interfaccia **IVssBackupComponents** o chiamando **IVssBackupComponents::DeleteSnapshots**.

## <a name="restore-process"></a>Processo di ripristino

Questa sezione descrive il flusso di lavoro dell'operazione di ripristino e i passaggi necessari.

### <a name="restore-operation-workflow"></a>Flusso di lavoro dell'operazione di ripristino

La figura seguente illustra il diagramma del flusso dei flussi di lavoro durante un'operazione di ripristino del Servizio Copia Shadow del volume. Per comprendere appieno le attività di base dell'esecuzione di un ripristino, è utile suddividere questa panoramica negli argomenti seguenti:

- Inizializzazione del ripristino
- Preparazione del ripristino
- Ripristino dei file effettivo
- Pulizia e terminazione del ripristino

![Flusso del processo di ripristino](media/sql-server-vss-writer-backup-guide/restore-process-flow.png)

In tutti gli scenari di ripristino basati su componenti del Servizio Copia Shadow del volume, il ripristino del database viene gestito dal writer SQL in due fasi distinte.

- **Pre-ripristino**:  il writer SQL gestisce la convalida, la chiusura degli handle di file e così via.
- **Post-ripristino**:  il writer SQL collega il database ed esegue il ripristino a seguito dell'arresto anomalo del sistema, se necessario.

Tra queste due fasi, l'applicazione di backup è responsabile dello spostamento dei dati pertinenti nell'istanza di SQL sottostante.

### <a name="restore-initialization"></a>Inizializzazione del ripristino

Durante la fase di inizializzazione di un ripristino, il richiedente deve avere accesso ai documenti dei componenti di backup archiviati.

Il documento dei componenti di backup generato durante l'operazione di backup viene archiviato come parte dei dati di backup. L'applicazione di backup deve passare questi dati al framework Servizio Copia Shadow del volume. Il writer SQL ottiene l'accesso a questi dati all'inizio del processo di ripristino.

#### <a name="prepare-for-restore"></a>Preparare il ripristino

Per la preparazione di un ripristino, un richiedente usa il documento dei componenti di backup archiviato per determinare quali elementi ripristinare e come.  Il richiedente selezionerà i componenti da ripristinare e imposterà le opzioni di ripristino appropriate in base alle esigenze.

Se un'applicazione di backup intende applicare i backup differenziali o del log all'inizio dell'operazione di ripristino corrente (ad esempio, se è necessario "Ripristino con NORECOVERY"), è consigliabile impostare l'opzione seguente durante la creazione dei componenti per ogni database in fase di ripristino.

```
IVssBackupComponents::SetAdditionalRestores(true)
```

Dopo l'impostazione di tutti i dettagli necessari nel documento dei componenti di backup, il richiedente esegue la chiamata a **IVssBackupComponents::PreRestore** per generare un evento PreRestore tramite il Servizio Copia Shadow del volume che verrà gestito dai writer.

Il writer SQL analizzerà il documento dei componenti di backup fornito per identificare i database appropriati, eliminando eventuali file aggiuntivi creati dopo il backup. Controlla anche gli spazi su disco e chiude eventuali handle di file di database aperti, in modo che il richiedente possa copiare i dati necessari durante la fase di ripristino. Questa fase consente di rilevare eventuali condizioni di errore iniziali prima che il richiedente esegua la copia del file effettivo. SQL Server porrà inoltre il database in uno stato di ripristino.  Da questo momento il database potrà essere avviato solo dopo un ripristino riuscito.

#### <a name="restore-files"></a>Ripristinare i file

Si tratta di un'azione specifica del richiedente. È responsabilità del richiedente (applicazione di backup) copiare i file di database necessari (o copiare gli intervalli di dati pertinenti per i ripristini differenziali) nelle posizioni appropriate. Il writer SQL non è coinvolto in questa operazione.

#### <a name="cleanup-and-termination"></a>Pulizia e terminazione

Dopo il ripristino di tutti i dati nelle posizioni corrette, una chiamata da un richiedente per notificare che l'operazione di ripristino è stata completata (**IvssBackupComponents::PostRestore**) comunicherà al writer SQL che le azioni post-ripristino possono essere avviate.  A questo punto, il writer SQL eseguirà la fase di roll forward del ripristino a seguito dell'arresto anomalo del sistema. Se il recupero non è richiesto (ovvero SetAdditionalRestores(true) non viene specificato dal richiedente), durante questa fase viene eseguita anche la fase di rollback del passaggio di recupero.

## <a name="backup-and-restore-option-details"></a>Dettagli delle opzioni di backup e ripristino

Questa sezione descrive in dettaglio tutte le opzioni di backup e ripristino supportate dal writer SQL.

### <a name="requestor-creates-a-volume-shadow-copy"></a>Il richiedente crea una copia shadow del volume

Il writer SQL potrebbe essere coinvolto nel processo di creazione della copia shadow del volume (al di fuori del contesto di backup/ripristino) perché i volumi di backup dei file di database sono stati aggiunti nel set di snapshot dei volumi.  In questo caso, il writer SQL partecipa solo al coordinamento di enumerazione dei metadati, blocco, sblocco, preparazione dello snapshot e post-snapshot. Per informazioni dettagliate, vedere il diagramma del flusso di dati.

### <a name="full-backuprestore"></a>Backup/Ripristino completo

Il writer SQL supporta le operazioni di backup/ripristino completo sia in modalità non basata su componenti che in modalità basata su componenti.

### <a name="noncomponent-based-backup-and-restore"></a>Backup e ripristino non basati su componenti

In un backup/ripristino non basato su componenti, il richiedente specifica un volume o un albero di cartelle di cui eseguire il backup/ripristino. Viene eseguito il backup/ripristino di tutti i dati nel volume o nella cartella specificata.

#### <a name="backup"></a>Backup

In un backup non basato su componenti il writer SQL seleziona in modo implicito i database usando l'elenco di volumi nel set di snapshot.  Il writer controlla la presenza di database incompleti e, se ne trova, genera un errore. Un database incompleto è uno in cui un subset di file viene selezionato dall'elenco di volumi.  Il roll forward (ripristini differenziali o di log) dopo un ripristino non è supportato tramite il writer SQL.

#### <a name="restore"></a>Restore

Il richiedente ripristina i database di cui è stato eseguito il backup in modalità non basata su componenti.  Tali ripristini non possono essere completati da un ripristino di roll forward, ad esempio un ripristino di log o un ripristino differenziale.

Per le operazioni di ripristino non basate su componenti, il ripristino deve essere eseguito con l'istanza di SQL Server offline, altrimenti i database di destinazione vengono eliminati/scollegati per garantire che i file siano offline.  I file vengono copiati sul posto e quindi i database vengono collegati. Tutte queste operazioni vengono eseguite al di fuori dell'ambito del writer SQL.

### <a name="component-based-backup-and-restore"></a>Backup e ripristino basati su componenti

In un backup basato su componenti, il richiedente seleziona in modo esplicito i componenti del database (dai metadati che il writer SQL restituisce al client) per eseguire il backup/ripristino.

#### <a name="backup"></a>Backup

In un backup basato su componenti, tutti i volumi di backup per i database selezionati devono essere inclusi nel set di snapshot dei volumi.  In caso contrario, il writer SQL rileverà i database incompleti (con volumi di backup non compresi nel set di snapshot) e il backup avrà esito negativo.  Un backup completo esegue il backup dei dati del database e di tutti i file di log necessari perché lo stato del database al momento del ripristino sia coerente a livello di transazione.

#### <a name="full-restore-without-rollforward"></a>Ripristino completo senza roll forward

Un ripristino completo del backup del database viene a volte effettuato senza eseguire operazioni di roll forward aggiuntive perché non sono presenti metadati per facilitare il roll forward o, in alcuni casi, non è necessario eseguire il roll forward. Questa sezione descrive brevemente le due situazioni.  

#### <a name="no-metadatano-rollforward"></a>Nessun metadato/nessun roll forward

Se non vengono salvati metadati del writer (metadati di backup basati su componenti) durante l'operazione di backup, il ripristino deve essere eseguito con l'istanza di SQL Server offline, altrimenti i database di destinazione vengono eliminati/scollegati per garantire che i file siano offline.  I file vengono copiati sul posto e quindi i database vengono collegati. Tutte queste operazioni vengono eseguite al di fuori dell'ambito del writer SQL.

#### <a name="metadata-exist-but-no-additional-rollforward-is-needed"></a>I metadati esistono, ma non sono necessarie operazioni di roll forward aggiuntive

Il richiedente ripristina i database di cui è stato eseguito il backup in modalità basata su componenti, ma non sono richieste operazioni di roll forward. In questo caso SQL Server eseguirà il ripristino a seguito dell'arresto anomalo del sistema sul database durante il ripristino.

**Ripristino completo con operazioni di roll forward aggiuntive** Il richiedente può eseguire un ripristino specificando l'opzione SetAdditionalRestores(true).  Questa opzione indica che il richiedente farà seguire altri ripristini di roll forward (ad esempio, ripristino del log, ripristino differenziale e così via) e indica a SQL Server di non eseguire il passaggio di recupero al termine dell'operazione di ripristino.

Ciò è possibile solo se i metadati del writer sono stati salvati durante il backup e sono disponibili per il writer SQL al momento del ripristino. Il servizio SQL Server deve essere in esecuzione prima che il richiedente indichi al Servizio Copia Shadow del volume di ripristinare l'attività.

Il writer SQL prevede la sequenza seguente:

1. Preparazione del ripristino di ogni database. Questa attività comporta la chiusura di tutti gli handle di file in modo che i file di database possano essere copiati/montati dall'applicazione richiedente.
1. I file vengono copiati o montati dall'applicazione richiedente.
1. Finalizzare il ripristino (con NORECOVERY).  I database vengono portati online, ma in uno stato di "ripristino".

È quindi possibile usare i convenzionali backup di SQL, differenziali o di log, per eseguire il roll forward del database tramite VDI/T-SQL oppure applicando il ripristino differenziale tramite il framework Servizio Copia Shadow del volume.

### <a name="full-text-support"></a>Supporto full-text

Il writer SQL segnala i contenitori di catalogo full-text con specifiche di file ricorsive sotto i componenti del database nel documento di metadati del writer.  Vengono inclusi automaticamente nel backup quando viene selezionato il componente del database

### <a name="differential-backuprestore"></a>Backup/Ripristino differenziale

Un'operazione di backup differenziale esegue un backup solo dei dati che sono stati modificati dopo il backup completo di base più recente. Un backup differenziale contiene solo le parti dei file di database che sono state modificate. Per eseguire tale backup, l'applicazione di backup, ovvero il richiedente, necessita di informazioni sulla posizione delle modifiche nei file di database, in modo che sia possibile eseguire il backup delle sezioni appropriate dei file. Durante un'operazione di backup differenziale, il writer SQL fornisce queste informazioni nel formato specificato dalle informazioni sul file parziale del Servizio Copia Shadow del volume. Queste informazioni possono essere usate per eseguire il backup solo della parte modificata dei file di database.

### <a name="backup"></a>Backup

Il richiedente può eseguire un backup differenziale impostando l'opzione DIFFERENTIAL (**VSS_BT_DIFFERENTIAL**) nel documento dei componenti di backup (**IVssBackupComponents::SetBackupState**) durante l'avvio di un'operazione di backup con il Servizio Copia Shadow del volume.  Il writer SQL passerà le informazioni sul file parziale (restituite da SQL Server) al Servizio Copia Shadow del volume.  Il richiedente può ottenere queste informazioni chiamando le API del Servizio Copia Shadow del volume (**IVssComponent::GetPartialFile**). Queste informazioni sul file parziale consentono al richiedente di scegliere solo gli intervalli di byte modificati per eseguire il backup dei file di database.

Durante la fase delle attività di pre-backup, il writer SQL verificherà che esista una singola base differenziale per ogni database selezionato.

Durante l'evento PostSnapshot, il writer SQL otterrà le informazioni sul file parziale da SQL Server e le aggiungerà al documento dei componenti di backup usando la chiamata a **IVssComponent::AddPartialFile**.  

  > [!NOTE]
  > Il writer SQL supporta una sola baseline differenziale per i backup differenziali. Non sono supportate baseline multiple.

### <a name="partial-file-information-format"></a>Formato delle informazioni sul file parziale

Per ogni database di cui viene eseguito il backup durante un backup differenziale, il writer SQL archivia le informazioni sul file parziale per ogni file di database. Queste informazioni vengono usate dal richiedente o dall'applicazione di backup per copiare solo le parti pertinenti del file nel supporto di backup durante il backup effettivo dei file. Per altre informazioni sul formato di queste informazioni sul file parziale, vedere la documentazione del Servizio Copia Shadow del volume.

Un richiedente può determinare questi file chiamando **IVssComponent::GetPartialFileCount** e **IVssComponent::GetPartialFile**.  **IVssComponent::GetPartialFile** restituirà un percorso e un nome che puntano al file e una stringa di intervalli indicante di che cosa è necessario eseguire un backup nel file.

Per altre informazioni sul recupero delle informazioni sul file parziale, vedere la [documentazione del Servizio Copia Shadow del volume](/windows/win32/vss/volume-shadow-copy-service-overview).

### <a name="back-up-files"></a>Eseguire un backup dei file

Durante questa fase, l'applicazione di backup esaminerà i metadati del writer archiviati nel documento dei componenti di backup ed eseguirà il backup solo delle parti pertinenti dei file. Per i file di catalogo full-text, questo backup dovrà essere eseguito in base ai timestamp dei file, come descritto più avanti in questo documento.

Un backup differenziale sarà sempre relativo al backup di base più recente esistente per il database.  In fase di ripristino SQL Server rileverà i backup di base e differenziali non corrispondenti. È quindi responsabilità dell'applicazione di backup o dell'amministratore di sistema verificare che il differenziale sia relativo al backup completo previsto.  Se alcune procedure fuori banda hanno eseguito un altro backup completo, l'applicazione di backup potrebbe non riuscire a ripristinare il differenziale, dal momento che non è "proprietario" del backup di base.

Attualmente, se le informazioni sugli intervalli di byte (informazioni sul file parziale) sono troppo grandi (dimensioni del buffer superiori a 64 KB), SQL Server genererà un errore che indica all'utente di eseguire un backup completo.

### <a name="interesting-cases-during-backup"></a>Casi interessanti durante il backup

L'aggiunta, l'eliminazione, la compattazione, l'aumento delle dimensioni, la ridenominazione logica e la ridenominazione fisica dei file sono casi interessanti del backup.

**File aggiunti subito dopo l'acquisizione della base**

Questi file sono inclusi nella specifica parziale perché è necessario che ogni intestazione del file di database SQL sia inclusa nella specifica parziale.  Oltre alla pagina di intestazione, è necessario includere nella specifica parziale tutte le pagine allocate.

**File eliminati dopo l'acquisizione della base**

Una volta acquisita la base, è possibile eliminare i file di dati.  Tali file non vengono inclusi nel documento di metadati del writer durante il backup differenziale.  Non saranno presenti nemmeno informazioni parziali associate al file eliminato.

**File compattati dopo l'acquisizione della base**

Le informazioni parziali non vengono raccolte dai file finché le operazioni di compattazione dei file non sono state disabilitate nel server.  In questo modo si garantisce che non vengano mai incluse informazioni parziali corrispondenti all'area compattata di un file di dati.  

**Aumento delle dimensioni dei file dopo l'acquisizione della base**

Se si verifica un aumento delle dimensioni prima della raccolta delle informazioni parziali, tali informazioni dovrebbero includere le pagine allocate nell'area con dimensioni aumentate.  Se si verifica un aumento delle dimensioni prima della raccolta delle informazioni parziali, tali informazioni dovrebbero includere le pagine allocate nell'area con dimensioni aumentate.  Nelle sezioni seguenti si noterà che tali modifiche verranno ripristinate dal roll forward del log.

**File rinominato logicamente dopo l'acquisizione della base**

Una ridenominazione logica del file non influisce sul backup o sul ripristino, perché in nessun punto del documento di metadati del writer o del documento dei componenti di backup si fa riferimento al nome logico del file.  Per informazioni più dettagliate, vedere Documento di metadati del writer: un esempio.

**File rinominato fisicamente dopo l'acquisizione della base**

La ridenominazione fisica di un file di database diventa effettiva solo dopo il riavvio del database,  quindi le informazioni di configurazione del database o le informazioni sul percorso del file nel buffer delle informazioni parziali sono ancora basate sui percorsi fisici precedenti, che sono gli unici percorsi validi di tali file di database nello snapshot.

### <a name="restore"></a>Restore

Durante un ripristino differenziale, i metadati di backup restituiti dal richiedente al writer SQL contengono le informazioni sul tipo di backup.  Non è quindi necessario alcun trattamento speciale da parte del writer SQL.  SQL Server determinerà automaticamente che si tratta di un ripristino differenziale.  SQL Server gestisce tale ripristino differenziale esattamente come avviene per un ripristino differenziale nativo che non viene eseguito tramite il Servizio Copia Shadow del volume.

### <a name="prerestore-phase"></a>Fase di pre-ripristino

Durante questa fase SQL Server ridimensiona tutti i file alle dimensioni appropriate in base ai metadati del file del backup differenziale.  Se le dimensioni del file aumentano, SQL Server azzera la parte aumentata.  Se è necessario creare un nuovo file (creato dopo l'acquisizione della base), SQL Server azzera il nuovo file. Chiude anche tutti gli handle di file in modo che l'applicazione di backup possa sovrascrivere i file con i dati ripristinati dai supporti di backup.

### <a name="restore-files"></a>Ripristinare i file

Il client deve ripristinare i file in base alla specifica del file parziale.  I dati devono essere ripristinati nello stesso offset/intervallo del file di database, come indicato nella specifica del file parziale archiviata nei metadati del writer.

L'aggiunta, l'eliminazione, l'aumento delle dimensioni, la compattazione, la ridenominazione logica e la ridenominazione fisica dei file di database costituiscono casi interessanti anche in fase di ripristino.

**Se un file di database era stato aggiunto dopo l'acquisizione della base completa**

È necessario che tali file siano stati creati in precedenza da SQL Server durante la fase di preparazione del ripristino.  Devono essere stati estesi fino alle dimensioni corrette e azzerati.  Il client deve solo riposizionare i dati in base alla specifica parziale, che include tutti gli extent allocati.

**Se un file di database era stato eliminato dopo l'acquisizione della base completa**

Le informazioni parziali fornite da SQL Server non includono informazioni di rilevamento per tali eliminazioni di file.  SQL Server ha la responsabilità di rilevare i file da eliminare, confrontando i metadati dei file ripristinati con i contenitori esistenti, e di eliminarli effettivamente.  Questa operazione viene eseguita prima del ripristino come passaggio di preparazione.

**Se le dimensioni di un file di database erano aumentate dopo l'acquisizione della base completa**

È necessario che tali file siano stati estesi fino alle dimensioni corrette da SQL Server durante la fase di preparazione del ripristino.  È anche necessario che l'area estesa sia stata azzerata da SQL Server.  Il client può quindi riposizionare in modo sicuro i dati anche nell'area con dimensioni aumentate in base alla specifica parziale.  Se le dimensioni del file sono aumentate dopo l'acquisizione delle informazioni parziali, le modifiche nell'area con dimensioni aumentate verranno ripristinate riproducendo il log di cui è stato eseguito il backup insieme al backup differenziale.

**Se un file di database era stato compattato dopo l'acquisizione della base completa**

SQL Server è responsabile del troncamento del file fino alle dimensioni necessarie in base ai metadati.  Questa operazione viene eseguita prima del ripristino come passaggio di preparazione.

**Se un file di database era stato rinominato logicamente dopo l'acquisizione della base completa**

Questa operazione non influirà sul ripristino perché il nome logico non viene visualizzato nel documento di metadati del writer né nel documento dei componenti di backup.  La modifica del nome logico verrà ripristinata quando il client applicherà la modifica al file di database primario, che contiene le informazioni del catalogo di sistema.

**Se un file di database era stato rinominato fisicamente dopo l'acquisizione della base completa**

Se al momento del backup differenziale, la ridenominazione non era stata applicata, il client ripristina comunque i dati nella posizione precedente.  Un riavvio del database dopo il ripristino provocherà l'applicazione della ridenominazione fisica.  Se al momento del backup differenziale, la ridenominazione fisica del file era già stata applicata, il backup dei dati parziali, se presenti, dal nuovo percorso fisico è stato eseguito.  

### <a name="post-restore"></a>Post-ripristino

Durante gli eventi di post-ripristino, il writer SQL eseguirà la normale operazione di roll forward e il ripristino (se SetAdditionalRestores() è impostato su False) del database.

## <a name="differential-backuprestore-for-full-text-catalogs"></a>Backup/Ripristino differenziale per i cataloghi full-text

I cataloghi full-text di SQL Server fanno parte delle risorse del database di cui è necessario eseguire un backup o un ripristino con gli altri file di database.  Un backup differenziale è basato su timestamp per il catalogo full-text.  Il backup/ripristino differenziale del Servizio Copia Shadow del volume di SQL Server ha un singolo backup di base.  In altre parole, non saranno presenti basi diverse a seconda dei contenitori.  Per il backup del catalogo full-text del Servizio Copia Shadow del volume, ovvero per tutti i contenitori di cataloghi full-text, il backup differenziale sarà basato su un singolo timestamp, diversamente dal backup differenziale di SQL nativo, in cui è presente una base di timestamp per ogni contenitore di catalogo full-text.

Nel Servizio Copia Shadow del volume questo timestamp è espresso come proprietà a livello di componente impostata durante il backup completo e usata durante un backup differenziale successivo.  

### <a name="onidentify"></a>OnIdentify

In OnIdentify il writer SQL chiama IVssCreateWriterMetadata::SetBackupSchema() per impostare il valore VSS_BS_TIMESTAMPED,  che indica all'applicazione di backup che il writer SQL è proprietario della gestione della base differenziale.

### <a name="setting-the-base-timestamp"></a>Impostazione del timestamp di base

Il timestamp di base viene impostato durante un backup completo.  In **OnPostSnapshot()** il writer richiama **IVssComponent::SetBackupStamp()** per archiviare il timestamp con il componente nel documento di backup.

### <a name="differential-backup"></a>Backup differenziale

L'applicazione di backup recupererà questo timestamp dal backup completo di base e lo renderà disponibile per il writer chiamando **IVssComponent::GetBackupStamp()** per recuperare il timestamp di base dal backup di base precedente.  Lo rende quindi disponibile per il writer chiamando **IVssBackupComponent::SetPreviousBackupStamp()** .  Il writer recupera quindi il timestamp chiamando **IVssComponent::GetPreviousBackupStamp()** e lo converte in un timestamp usato per **IVssComponent::AddDifferencedFilesByLastModifyTime()** .  

**Responsabilità dell'applicazione di backup durante il backup differenziale** Durante un backup differenziale, l'applicazione di backup è responsabile di:

- Backup di eventuali file (gli interi file) il cui timestamp dell'ultima modifica è più recente del timestamp specificato dall'ora dell'ultima modifica per il set di file nel componente.
- Verifica e rilevamento dei file eliminati.  

**Responsabilità dell'applicazione di backup durante un ripristino differenziale** Durante un ripristino differenziale, l'applicazione di backup è responsabile di:

- Ripristino di tutti i file di cui è stato eseguito il backup, creando un nuovo file, se non esiste già, oppure sovrascrivendo un file esistente, se esiste già.
- Espansione del file prima di riposizionare il contenuto se il file ripristinato è più grande del file esistente.
- Troncamento del file alle stesse dimensioni del file ripristinato se il file ripristinato è più piccolo del file esistente.
- Eliminazione di tutti i file da eliminare, ovvero i file che non devono esistere al momento del backup differenziale.

### <a name="copy-only-backup"></a>Backup di sola copia

A volte è necessario eseguire un backup destinato a uno scopo specifico. Ad esempio, potrebbe essere necessario creare una copia di un database a scopo di test.  Un backup di sola copia non ha alcun effetto sulle procedure di backup e ripristino generali per il database. L'uso dell'opzione COPY_ONLY specifica che il backup viene eseguito "fuori banda" e non influirà sulla normale sequenza di backup. Il writer SQL supporta il tipo di backup "solo copia" con le istanze SQL Server.

Durante la fase di individuazione del backup, il writer SQL indicherà la capacità di eseguire un backup di sola copia impostando l'opzione dello schema di backup supportata VSS_BS_COPY usando la chiamata a **IVssCreateWriterMetadata::SetBackupSchema**. Il richiedente può impostare il tipo di backup come backup di sola copia impostando l'opzione VSS_BACKUP_TYPE come VSS_BT_COPY con la chiamata a **IVssBackupComponents::SetBackupState**.

Quando viene selezionato un backup di sola copia, si presuppone che i file su disco vengano copiati in un supporto di backup (dal richiedente) indipendentemente dallo stato della cronologia di backup di ogni file. SQL Server non aggiornerà la cronologia di backup. Questo tipo di backup non costituirà un backup di base per ulteriori operazioni di backup differenziale e non influirà sulla cronologia dei backup differenziali precedenti.

### <a name="restore-with-move"></a>Ripristino con spostamento

Il Servizio Copia Shadow del volume consente all'applicazione di backup (richiedente) di specificare una nuova destinazione di ripristino usando la chiamata a **IVssComponent::SetNewTarget**.  Sia in PreRestore() che in PostRestore() il writer SQL controlla se è stata specificata almeno una nuova destinazione. È responsabilità dell'applicazione di backup copiare fisicamente i file nella nuova posizione durante la fase di ripristino o di copia dei file effettivi.

L'applicazione di backup può specificare solo nuove destinazioni per il percorso fisico, ma non la specifica del file.  Ad esempio, per un file di database che si trova in c:\data\test.mdf, il nome file effettivo, test.mdf, non può essere modificato.  Solo il percorso c:\data può essere modificato.  Per un contenitore di cataloghi full-text che si trova in c:\ftdata\foo, poiché la specifica del file nel Servizio Copia Shadow del volume è "*" e la specifica del percorso nel Servizio Copia Shadow del volume è c:\ftdata\foo, l'intero percorso può essere modificato.  

### <a name="database-rename"></a>Ridenominazione del database

Un richiedente potrebbe dover ripristinare un database SQL con un nuovo nome, soprattutto se il database deve essere ripristinato side-by-side con il database originale.  Questa opzione può essere specificata dal richiedente durante l'operazione di ripristino impostando un'operazione di ripristino personalizzata come "Nuovo nome componente" = <"Nuovo nome"> usando la chiamata del Servizio Copia Shadow del volume a **IVssBackupComponents::SetRestoreOptions()** (nel parametro wszRestoreOptions).

Il writer SQL acquisirà l'intero contenuto del valore Nuovo nome componente e lo userà come nuovo nome per il database ripristinato. Se non si specifica alcuna opzione, SQL ripristinerà il database con il nome originale (il nome del componente).

  > [!NOTE]
  > Il writer SQL attualmente non supporta la ridenominazione tra istanze per spostare un database in una nuova istanza.

### <a name="auto-recovered-snapshots"></a>Snapshot recuperati automaticamente

In genere uno snapshot del database di SQL Server ottenuto usando il framework del Servizio Copia Shadow del volume è in uno stato non recuperato. Non è possibile accedere in modo sicuro ai dati dello snapshot prima della fase di recupero per eseguire il rollback delle transazioni in corso e porre il database in uno stato coerente. Poiché lo snapshot è in stato di sola lettura, non può essere recuperato dal normale processo di collegamento del database.

È possibile eseguire il ripristino automatico degli snapshot durante il processo di creazione dello snapshot. Nell'ambito del documento di metadati del writer, il writer SQL specifica il flag di componente "VSS_CF_APP_ROLLBACK_RECOVERY" per indicare che è necessario eseguire il recupero per il database nello snapshot prima che il database sia accessibile. Quando si specifica il set di snapshot, il richiedente può indicare che lo snapshot deve essere uno snapshot di ripristino dello stato precedente dell'app (ovvero, tutti i file di database in uno snapshot devono essere in uno stato coerente per l'utilizzo dell'applicazione) o uno snapshot di backup (uno snapshot usato per il backup dei dati da ripristinare in un secondo momento in caso di errore di sistema).

Il richiedente deve impostare VSS_VOLSNAP_ATTR_ROLLBACK_RECOVERY per indicare che viene eseguito il backup di questo componente per uno scopo non di backup.  Il Servizio Copia Shadow del volume correla quindi VSS_CF_APP_ROLLBACK_RECOVERY specificato dal writer SQL nel componente selezionato con VSS_VOLSNAP_ATTR_ROLLBACK_RECOVERY e determina che il ripristino automatico è in corso.  Il Servizio Copia Shadow del volume renderà scrivibile lo snapshot per un periodo di tempo limitato e aggiungerà automaticamente il bit VSS_VOLSNAP_ATTR_AUTORECOVERY al contesto dello snapshot.

Nel caso di SQL Server il recupero automatico deve essere applicato solo agli snapshot di ripristino dello stato precedente dell'app, ma non per gli snapshot di backup. Per gli snapshot di ripristino dello stato precedente dell'app, un processo di recupero automatico viene avviato dal writer SQL durante l'evento PostSnapShot. Questo processo effettuerà le operazioni seguenti per ogni database di SQL Server selezionato (dal richiedente) in modo esplicito nel set di snapshot:

- Collegare il database di snapshot all'istanza di SQL Server originale, ovvero l'istanza a cui è collegato il database originale.
- Recuperare il database. Questa operazione viene eseguita durante l'operazione di "collegamento".
- Compattare i file di log.

    In questo modo si riduce la quantità di operazioni di "copia su scrittura" superflue che devono essere eseguite dal framework Servizio Copia Shadow del volume, se il provider del Servizio Copia Shadow del volume è un "provider di software". La compattazione dei file di log è il comportamento predefinito, che può essere disabilitato impostando il valore della chiave del Registro di sistema seguente su 1.
    
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SQLWriter\
    Settings\DisableLogShrink
    
    Ciò può essere utile negli scenari in cui lo snapshot può essere usato per esportare i dati da una pagina specifica (in un momento specifico) del log per risolvere un problema nel database online.

- Scollegare il database.

A questo punto è stato recuperato uno snapshot coerente che può essere collegato per l'esecuzione di query.

### <a name="multi-database-transactions"></a>Transazioni tra più database

In alcuni casi, i database di snapshot possono contenere alcune transazioni tra più database in corso. Durante l'operazione di recupero, il writer SQL collegherà il database negli snapshot con l'opzione Presumed Abort (Interruzione presunta). Verrà eseguito il rollback di qualsiasi transazione tra più database di cui non è ancora stato eseguito il commit, incluse le transazioni con stato Prepared to Commit (Pronta per il commit). Questo può causare alcune incoerenze tra i database nel set di snapshot. Si considerino, ad esempio, i due database A e B. Esiste una transazione distribuita tra questi due database e questa transazione si trova nello stato Commit eseguito nel database A e nello stato Prepared to Commit (Pronta per il commit) nel database B. Durante il processo di recupero automatico, viene eseguito il commit di questa transazione nel database A e il rollback nel database B. Questo può causare alcune incoerenze nel set di snapshot.

Il componente Microsoft Distributed Transaction Coordinator (MS DTC) che deve essere rilasciato nell'intervallo di tempo di Longhorn dal framework Servizio Copia Shadow del volume risolverà questo problema di incoerenza per le transazioni che si estendono sui database tra le istanze di SQL Server. La prossima versione di SQL Server correggerà queste incoerenze per le transazioni che si estendono su database all'interno di un'istanza di SQL Server.

### <a name="security-implications-for-autorecovered-snapshots"></a>Implicazioni per la sicurezza degli snapshot recuperati automaticamente

Per gli snapshot del Servizio Copia Shadow del volume, dopo il recupero automatico, i file verranno protetti con elenchi di controllo di accesso (ACL) per consentire l'accesso solo al gruppo predefinito speciale a cui appartiene l'account SQL Server.  Ciò implica che il membro di un gruppo di amministratori o del gruppo speciale potrà collegare il database. Il client che richiede un collegamento dei file di database in uno snapshot deve essere un membro di Builtin/Administrators o dell'account SQL Server.

### <a name="special-cases"></a>Casi speciali

Questa sezione descrive alcuni dei casi speciali rilevati durante le operazioni di backup e ripristino basate sul writer SQL.

#### <a name="autoclose-databases"></a>Database con chiusura automatica

Per i backup non basati su componenti, viene eseguita la chiusura automatica dei database, quando si verifica la presenza di condizioni incomplete, ma i database chiusi automaticamente non vengono bloccati in modo esplicito durante le operazioni di backup.

Lo scenario previsto in questo caso è che possano esistere più database chiusi e che si voglia ridurre al minimo il costo dello snapshot.  I database chiusi automaticamente vengono in genere usati nelle configurazioni di base in cui le risorse sono limitate.

#### <a name="file-list"></a>Elenco di file

L'elenco di file per ogni database viene determinato durante un passaggio di enumerazione prima dell'evento di preparazione del backup.  Se l'elenco dei file di database cambia tra l'enumerazione e il blocco, il database potrebbe essere incompleto, a meno che l'applicazione non ricontrolli l'elenco dei file. Questo non dovrebbe essere un problema, ma è necessario che i fornitori ne siano consapevoli.

#### <a name="stopped-instances"></a>Istanze arrestate

Se un'istanza di SQL Server non è in esecuzione nel momento in cui si verifica il passaggio di enumerazione, non è possibile selezionare nessuno dei database per tali istanze.

Se un'istanza si arresta nell'intervallo tra l'enumerazione e l'evento di preparazione del backup, i database nell'istanza arrestata verranno ignorati.

#### <a name="system-and-user-databases"></a>Database di sistema e utente

I database di sistema in SQL Server includono i database master, modello e msdb che vengono forniti e installati come parte di SQL Server. Questa sezione descrive come questi database vengono gestiti in un processo di backup di snapshot del Servizio Copia Shadow del volume.

### <a name="system-databases"></a>Database di sistema

È possibile ripristinare il database master solo arrestando l'istanza, sostituendo i file di database (creati dall'applicazione di backup), quindi riavviando l'istanza. Non è possibile eseguire il roll forward.

Il writer SQL supporta il ripristino online dei database modello e msdb, senza arrestare l'istanza.


#### <a name="simple-recovery-model-user-databases"></a>Database utente con modello di recupero con registrazione minima

Se il database master viene ripristinato insieme ai database utente che usano il modello di recupero con registrazione minima, è possibile ripristinare i database utente con la stessa tecnica del database master: quando l'istanza viene arrestata, è sufficiente copiare o montare i volumi.  Quando viene avviata l'istanza di SQL, viene eseguito il recupero di tutti gli elementi.

#### <a name="rolling-forward-user-databases"></a>Roll forward dei database utente

Se è necessario eseguire il recupero e il roll forward dei database utente insieme al recupero del database master, l'istanza non deve avviare e recuperare i database master e utente insieme.

Di seguito è riportata la procedura:

1. Verificare che l'istanza di SQL Server sia stata arrestata.
1. Eseguire il ripristino in due fasi.
    1. Ripristinare i database di sistema e i database utente che devono essere recuperati nello stesso momento (ovvero i database utente in modalità di recupero con registrazione minima) tramite la copia dei file o il montaggio dei volumi con il Servizio Copia Shadow del volume.
        1. Se i database utente di cui eseguire il roll forward non si trovano nello stesso volume dei database di sistema, tale volume non tornerà disponibile in questo momento. Questo scenario richiede la pianificazione prima del backup.
        1. Se i database utente si trovano nello stesso volume dei database di sistema, è necessario che i database utente siano nascosti a SQL Server.
    1. Avviare l'istanza di SQL Server usando il parametro -f.  Quando si usa l'opzione di avvio -f, è possibile ripristinare solo il database master.
        1. Eseguire ALTER DATABASE \<x> SET OFFLINE per ogni database di cui eseguire il roll forward.  In alternativa, scollegare il database.
        1. Arrestare l'istanza di SQL Server.
        1. Avviare l'istanza di SQL Server. I file per i database utente di cui eseguire il roll forward non sono visibili a SQL Server.

Usare il Servizio Copia Shadow del volume per ripristinare i database utente con NORECOVERY, come descritto in "Ripristino completo con roll forward".

## <a name="appendix"></a>Appendice

### <a name="writer-metadata-document--an-example"></a>Documento di metadati del writer:  un esempio

Con un database di esempio denominato DB1, che appartiene all'istanza di SQL Server Instance1 nel computer Server1 e che contiene i file di database/log seguenti:

- File di database denominato "primary" archiviato in c:\db\DB1.mdf
- File di database denominato "secondary" archiviato in c:\db\DB1.ndf
- File di log denominato "log" archiviato in c:\db\DB1.ldf
- Catalogo full-text denominato "foo" archiviato nella directory c:\db\ftdata\foo
- Catalogo full-text denominato "bar" archiviato nella directory c:\db\ftdata\bar

I seguenti sono i metadati del writer del database:

**Componente filegroup a livello di database**

- ComponentType: VSS_CT_FILEGROUP
- LogicalPath: "Server1\Instance1"
- ComponentName: "DB1"
- Caption: NULL
- pbIcon: NULL
- cbIcon: 0
- bRestoreMetadata: FALSE
- NotifyOnBackupComplete: TRUE
- Selectable: TRUE
- SelectableForRestore: TRUE
- ComponentFlags: VSS_CF_APP_ROLLBACK_RECOVERY

**File filegroup**

- LogicalPath: "Server1\Instance1"
- GroupName: "DB1"
- Path: "c:\db"
- FileSpec: "DB1.mdf"
- Recursive: FALSE
- AlternatePath: NULL
- BackupTypeMask: VSS_FSBT_ALL_BACKUP_REQUIRED | VSS_FSBT_ALL_SNAPSHOT_REQUIRED
- File filegroup
- LogicalPath: "Server1\Instance1"
- GroupName: "DB1"
- Path: "c:\db"
- FileSpec: "DB1.ndf"
- Recursive: FALSE
- AlternatePath: NULL
- BackupTypeMask: VSS_FSBT_ALL_BACKUP_REQUIRED | VSS_FSBT_ALL_SNAPSHOT_REQUIRED

**File filegroup**

- LogicalPath: "Server1\Instance1"
- GroupName: "DB1"
- Path: "c:\db"
- FileSpec: "DB1.ldf"
- Recursive: FALSE
- AlternatePath: NULL
- BackupTypeMask: VSS_FSBT_ALL_BACKUP_REQUIRED | VSS_FSBT_ALL_SNAPSHOT_REQUIRED

**File filegroup:**

- LogicalPath: "Server1\Instance1"
- GroupName: "DB1"
- Path: "c:\db\ftdata\foo"
- FileSpec: "*"
- Recursive: TRUE
- AlternatePath: NULL
- BackupTypeMask: VSS_FSBT_ALL_BACKUP_REQUIRED | VSS_FSBT_ALL_SNAPSHOT_REQUIRED

**File filegroup:**

- LogicalPath: "Server1\Instance1"
- GroupName: "DB1"
- Path: "c:\db\ftdata\bar"
- FileSpec: "*"
- Recursive: TRUE
- AlternatePath: NULL
- BackupTypeMask: VSS_FSBT_ALL_BACKUP_REQUIRED | VSS_FSBT_ALL_SNAPSHOT_REQUIRED

Se l'istanza del server è l'istanza predefinita nel computer, il percorso logico include una sola parte, "Server1".


    
## <a name="see-also"></a>Vedere anche  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Backup e ripristino di database SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [Backup di sola copia &#40;SQL Server&#41;](../../relational-databases/backup-restore/copy-only-backups-sql-server.md)   
 [Backup di log delle transazioni &#40;SQL Server&#41;](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md)   
 [Applicare backup del log delle transazioni &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)    
 [Architettura e gestione del log delle transazioni di SQL Server](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md)
  
