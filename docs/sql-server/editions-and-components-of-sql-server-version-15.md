---
description: Edizioni e funzionalità supportate di [!INCLUDE[sssql19-md](../includes/sssql19-md.md)]
title: Edizioni e funzionalità supportate di SQL Server 2019 | Microsoft Docs
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- Enterprise Edition [SQL Server]
- Developer Edition [SQL Server]
- 32-bit vs. 64-bit editions [SQL Server]
- default components
- Workgroup Edition [SQL Server]
- Internet servers [SQL Server]
- installing SQL Server, components
- Setup [SQL Server], components
- SQL Server, editions
- SQL Server, components
- client/server applications [SQL Server]
- editions [SQL Server]
- versions [SQL Server]
- Setup [SQL Server], editions
- SQL Server Installation Wizard
- components [SQL Server]
- Standard Edition [SQL Server]
- 64-bit edition [SQL Server]
- IIS [SQL Server]
- installing SQL Server, editions
- editions [SQL Server], about edition options
- Setup [SQL Server]
ms.assetid: ''
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 98b630f4471d35259db609a5bf1740ff96b8d604
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336174"
---
# <a name="editions-and-supported-features-of-sssql19-md"></a>Edizioni e funzionalità supportate di [!INCLUDE[sssql19-md](../includes/sssql19-md.md)]

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx](../includes/applies-to-version/sqlserver.md)]

Questo argomento offre informazioni dettagliate sulle funzionalità supportate dalle diverse edizioni di [!INCLUDE[sssql19-md](../includes/sssql19-md.md)].

Per informazioni sulle versioni precedenti, vedere:

* [SQL Server 2017](editions-and-components-of-sql-server-2017.md)
* [SQL Server 2016](editions-and-components-of-sql-server-2016.md)

I requisiti di installazione variano in base alle esigenze dell'applicazione. Le diverse edizioni di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] consentono di soddisfare le esigenze specifiche di utenti e organizzazioni in termini di prezzo, esecuzione e prestazioni. I componenti di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] installati dipendono inoltre dai requisiti specifici. Nelle sezioni seguenti vengono fornite tutte le informazioni necessarie per adottare la scelta migliore tra le edizioni e i componenti disponibili in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] è disponibile per un periodo di valutazione di 180 giorni.

Per le note sulla versione più recenti e informazioni sulle novità, vedere quanto segue:

* [Note sulla versione di [!INCLUDE[sssql19-md](../includes/sssql19-md.md)]](../sql-server/sql-server-version-15-release-notes.md)
* [Novità di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2019](../sql-server/what-s-new-in-sql-server-ver15.md)

**Provare [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]: [scaricare [!INCLUDE[sssql19-md](../includes/sssql19-md.md)] da Evaluation Center](https://www.microsoft.com//evalcenter/evaluate-sql-server)**

## <a name="ssnoversion-editions"></a>Edizioni di[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)]

La tabella seguente descrive tali edizioni di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

|Edizione di[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|Definizione|
|---------------------------------------|----------------|
|Enterprise|L'offerta premium, [!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Enterprise Edition, offre le funzionalità complete dei data center di fascia alta con prestazioni velocissime, virtualizzazione illimitata<sup>1</sup> e business Intelligence end-to-end, abilitando livelli di servizio elevati per carichi di lavoro cruciali e l'accesso dell'utente finale agli approfondimenti sui dati.|
|Standard|[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Standard offre un database di gestione dati e di Business Intelligence di base per l'esecuzione di applicazioni per reparti e piccole organizzazioni e supporta gli strumenti di sviluppo comuni locali e per cloud, abilitando una gestione efficace del database con risorse IT minime.|
|Web|[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Web Edition costituisce un'opzione con un costo totale di proprietà ridotto per provider di servizi di hosting Web e VAP Web, offrendo funzionalità di scalabilità, convenienza e facilità di gestione per proprietà Web di ogni dimensione.|
|Developer|L'edizione[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Developer consente agli sviluppatori di compilare qualsiasi tipo di applicazione in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Benché includa tutte le funzionalità dell'edizione Enterprise, ne è consentito l'utilizzo solo come sistema di sviluppo e di prova e non come server di produzione. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Developer rappresenta la scelta ideale per chi desidera compilare e testare applicazioni.|
|Edizioni Express|L'edizione Express è un database di base gratuito, ideale per l'apprendimento e la compilazione di applicazioni basate sui dati desktop e server di piccole dimensioni. Questa edizione costituisce la scelta ottimale per fornitori di software indipendenti, sviluppatori e sviluppatori amatoriali di applicazioni client. Se sono necessarie funzionalità di database più avanzate, è possibile aggiornare facilmente [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express a versioni di fascia superiore di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express LocalDB è una versione semplificata di Express che, pur includendone tutte le funzionalità di programmazione, viene eseguita in modalità utente e prevede un'installazione veloce senza operazioni di configurazione, nonché un elenco ridotto di prerequisiti.|

<sup>1</sup> La virtualizzazione illimitata è disponibile nella versione Enterprise Edition per i clienti con [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default). Le distribuzioni devono essere conformi alla guida alle licenze. Per altre informazioni, vedere la pagina dei prezzi e delle licenze.

## <a name="using-ssnoversion-with-clientserver-applications"></a>Uso di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] con applicazioni client/server

È possibile installare solo i componenti client di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] in un computer in cui vengono eseguite applicazioni client/server connesse direttamente a un'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. L'installazione di componenti client rappresenta una scelta ottimale anche se si amministra un'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] in un server di database o se si prevede di sviluppare applicazioni basate su [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .

La scelta degli strumenti client comporta l'installazione delle seguenti funzionalità [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] : componenti per la compatibilità con le versioni precedenti, [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)], componenti di connettività, strumenti di gestione, Software Development Kit e componenti della documentazione online di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Per altre informazioni, vedere [Installare [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../database-engine/install-windows/install-sql-server.md).

### <a name="running-with-iis"></a>Esecuzione con IIS

Gli strumenti client di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] vengono in genere installati in un server Internet, ad esempio un server che esegue Internet Information Services (IIS). Gli strumenti client includono i componenti di connettività client utilizzati dalle applicazioni per la connessione a un'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

>[!NOTE]
>Benché sia possibile installare un'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] nello stesso computer in cui è in esecuzione IIS, si tratta in genere di una configurazione utilizzata solo per siti Web di piccole dimensioni che dispongono di un singolo computer server. Nella maggior parte dei siti Web, i sistemi IIS di livello intermedio risiedono in un server o in un cluster di server, mentre i database corrispondenti si trovano in un server separato o in una federazione di server.

## <a name="deciding-among-ssnoversion-components"></a>Scelta dei componenti di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]

Per selezionare i componenti da includere in un'installazione di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , utilizzare la pagina di selezione delle funzionalità dell'Installazione guidata di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Per impostazione predefinita, non è selezionata alcuna funzionalità inclusa nell'albero.

Utilizzare le informazioni incluse nelle tabelle seguenti per determinare il set di funzionalità più adatto per le proprie esigenze.

|Componenti server|Descrizione|
|-----------------------|-----------------|
|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] include [!INCLUDE[ssDE](../includes/ssde-md.md)], il servizio di base per l'archiviazione, l'elaborazione e la sicurezza dei dati, la replica, la ricerca full-text, gli strumenti per la gestione di dati XML e relazionali, l'integrazione analitica nel database e l'integrazione PolyBase per l'accesso a Hadoop e ad altre origini di dati eterogenee, nonché Machine Learning Services per l'esecuzione di script Python e R con i dati relazionali.|
|[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]|[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] include gli strumenti per la creazione e la gestione delle applicazioni di data mining e Online Analytical Processing (OLAP).|
|[!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|In[!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] sono inclusi componenti client e server per la creazione, la gestione e la distribuzione di report tabulari, matrice, grafici e in formato libero. [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] è inoltre una piattaforma estendibile che consente di sviluppare applicazioni di creazione di report.|
|[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]|[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] è un set di strumenti grafici e oggetti programmabili per lo spostamento, la copia e la trasformazione di dati. È incluso, inoltre, il componente [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS) per [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].|
|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] (MDS) è la soluzione [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] per la gestione dei dati master. MDS può essere configurato per gestire qualsiasi dominio (prodotti, clienti, account) e può includere gerarchie, sicurezza granulare, transazioni, controllo delle versioni dei dati e regole business e un [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)] che può essere utilizzato per gestire dati.|
|Machine Learning Services (In-Database)|Machine Learning Services (In-Database) supporta soluzioni di Machine Learning distribuite e scalabili che usano origini dati aziendali. In [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2016 era supportato il linguaggio R. [!INCLUDE[sssql19-md](../includes/sssql19-md.md)] supporta R e Python.|
|Machine Learning Server (Standalone)|Machine Learning Server (Standalone) supporta la distribuzione di soluzioni di Machine Learning distribuite e scalabili su più piattaforme che usano più origini dati aziendali, tra cui Linux e Hadoop. In [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2016 era supportato il linguaggio R. [!INCLUDE[sssql19-md](../includes/sssql19-md.md)] supporta R e Python.|

|Strumenti di gestione|Descrizione|
|----------------------|-----------------|
|[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]|[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] (SSMS) è un ambiente integrato per l'accesso, la configurazione, la gestione, l'amministrazione e lo sviluppo di componenti di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. SSMS consente l'uso di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] a sviluppatori e amministratori con qualsiasi livello di esperienza. La versione più recente di SSMS Aggiorna SMO, che include l' [API valutazione SQL](../tools/sql-assessment-api/sql-assessment-api-overview.md).<br /><br/> Scaricare e installare <br />[!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] da [Scaricare [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Gestione configurazione|Gestione configurazione[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] offre funzionalità di base per la gestione della configurazione dei servizi di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , dei protocolli server, dei protocolli client e degli alias per i client.|
|[!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)]|[!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] offre un'interfaccia utente grafica per il monitoraggio di un'istanza del [!INCLUDE[ssDE](../includes/ssde-md.md)] o di [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)].|
|Ottimizzazione guidata[!INCLUDE[ssDE](../includes/ssde-md.md)]|Ottimizzazione guidata[!INCLUDE[ssDE](../includes/ssde-md.md)] consente di creare set di indici, viste indicizzate e partizioni ottimali.|
|Client Data Quality|Fornisce un'interfaccia utente grafica estremamente intuitiva e semplice per la connessione al server DQS e per eseguire operazioni di pulizia dei dati. Consente inoltre di monitorare centralmente le varie attività eseguite durante l'operazione di pulizia dei dati.|
|[!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)]|In[!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)] è disponibile un IDE per la compilazione di soluzioni per i componenti di Business Intelligence: [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)], [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]e [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].<br /><br /> (in precedenza denominato Business Intelligence Development Studio).<br /><br /> In[!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)] è inoltre incluso un ambiente integrato che consente agli sviluppatori di database di svolgere tutte le attività di progettazione di database per qualsiasi piattaforma [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , locale e non, all'interno di Visual Studio. Gli sviluppatori di database possono utilizzare le funzionalità avanzate di Esplora server in Visual Studio per creare o modificare facilmente dati e oggetti di database oppure eseguire query.|
|Componenti di connettività|Consente di installare i componenti per la comunicazione tra client e server nonché le librerie di rete per DB-Library, ODBC e OLE DB.|

|Documentazione|Descrizione|
|-------------------|-----------------|
|Documentazione online di[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|Documentazione principale di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].|

**Developer Edition ed Evaluation Edition** Per le funzionalità supportate da Developer Edition ed Evaluation Edition, vedere le funzionalità elencate per [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Enterprise Edition nelle tabelle seguenti.

Developer Edition continua a supportare un solo client per [Riesecuzione distribuita di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../tools/distributed-replay/sql-server-distributed-replay.md).

## <a name="scale-limits"></a><a name="Cross-BoxScaleLimits"></a> Limiti di scalabilità

|Funzionalità|Enterprise|Standard|Web|Express with<br/>Advanced Services|Express|
|-------|--------:|------:|---:|-------------------------------:|-----:|
|Capacità di calcolo massima usata da una singola istanza - [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]<sup>1</sup>|Valore massimo del sistema operativo|Limitato a meno di 4 socket o 24 core|Limitato a meno di 4 socket o 16 core|Limitato a meno di 1 socket o 4 core|Limitato a meno di 1 socket o 4 core|
|Capacità di calcolo massima usata da una singola istanza - [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] oppure [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|Valore massimo del sistema operativo|Limitato a meno di 4 socket o 24 core|Limitato a meno di 4 socket o 16 core|Limitato a meno di 1 socket o 4 core|Limitato a meno di 1 socket o 4 core|
|Memoria massima per il pool di buffer per ogni istanza di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|valore massimo del sistema operativo|128 <nobr/>GB|64 <nobr/>GB|1410 <nobr/>MB|1410<nobr/> MB|
|Memoria massima per la cache dei segmenti Columnstore per ogni istanza di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Memoria illimitata| 32 <nobr/>GB| 16 <nobr/>GB| 352 <nobr/>MB| 352 <nobr/>MB|
|Dimensione massima dati ottimizzati per la memoria per ogni database in [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Memoria illimitata| 32 <nobr/>GB| 16 <nobr/>GB| 352 <nobr/>MB| 352 <nobr/>MB|
|Memoria massima usata per ogni istanza di [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]|valore massimo del sistema operativo|16 <nobr/>GB<sup>2</sup><br /><br /> 64 <nobr/>GB<sup>3</sup>|N/D|N/D|N/D|
|Memoria massima usata per ogni istanza di [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|valore massimo del sistema operativo|64 <nobr/>GB|64 <nobr/>GB|4 <nobr/>GB|N/D|
|Dimensione massima del database relazionale|524 <nobr/> PB|524 <nobr/> PB|524 <nobr/> PB|10 <nobr/>GB|10 <nobr/>GB|

<sup>1</sup> La licenza basata su Enterprise Edition con Server + Licenza CAL (Client Access License) (non disponibile per i nuovi contratti) è limitata a un massimo di 20 core per istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Non sono previsti limiti nel modello di licenza server basato su core. Per altre informazioni, vedere [Limiti della capacità di calcolo per edizione di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../sql-server/compute-capacity-limits-by-edition-of-sql-server.md).

<sup>2</sup> Tabulare

<sup>3</sup> MOLAP

## <a name="rdbms-high-availability"></a><a name="RDBMSHA"></a> Disponibilità elevata RDBMS

|Funzionalità|Enterprise|Standard|Web|Express with<br/>Advanced Services|Rapida|
|-------|:--------:|:------:|:-:|--------------------------------:|:-----:|
|Supporto di Server Core<sup>1</sup>|Sì|Sì|Sì|Sì|Sì|
|Log shipping|Sì|Sì|Sì|No|No|
|Mirroring del database|Sì|Sì<sup>2</sup>|Sì<sup>3</sup>|Sì<sup>3</sup>|Sì<sup>3</sup>|
|Compressione backup|Sì|Sì|No|No|No|
|Snapshot del database|Sì|Sì|Sì|Sì|Sì|
|Istanze del cluster di failover AlwaysOn<sup>4</sup>|Sì|Sì|No|No|No|
|Gruppi di disponibilità Always On<sup>5</sup>|Sì|No|No|No|No|
|Gruppi di disponibilità di base<sup>6</sup>|No|Sì|No|No|No|
|Reindirizzamento automatico della connessione di lettura/scrittura |Sì|No|No|No|No|
|Ripristino di pagine e file online|Sì|No|No|No|No|
|Creazione e ricompilazione di indici online|Sì|No|No|No|No|
|Ricompilazioni degli indici online ripristinabili|Sì|No|No|No|No|
|Modifica dello schema online|Sì|No|No|No|No|
|Recupero rapido|Sì|No|No|No|No|
|Recupero del database accelerato|Sì|Sì|Sì|No|No|
|Backup con mirroring|Sì|No|No|No|No|
|Aggiunta di memoria a caldo e CPU|Sì|No|No|No|No|
|Database Recovery Advisor|Sì|Sì|Sì|Sì|Sì|
|Backup crittografato|Sì|Sì|No|No|No|
|Backup ibrido in Microsoft Azure (backup nell'URL)|Sì|Sì|Sì|No|No|
|Gruppo di disponibilità senza cluster <sup>5, 6</sup>|Sì|Sì|No|No|No|
|Server di failover per il ripristino di emergenza<sup>7</sup>|Sì|Sì|No|No|No|
|Server di failover per la disponibilità elevata<sup>7</sup>|Sì|Sì|No|No|No|
|Server di failover per il ripristino di emergenza in Azure<sup>7</sup>|Sì|Sì|No|No|No|

<sup>1</sup> Per altre informazioni sull'installazione di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] in Server Core, vedere [Installare [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] in Server Core](../database-engine/install-windows/install-sql-server-on-server-core.md).

<sup>2</sup> Solo modalità di protezione Full

<sup>3</sup> Solo server di controllo

<sup>4</sup> In Enterprise Edition il numero di nodi corrisponde al valore massimo del sistema operativo. In Standard Edition è presente il supporto per due nodi.

<sup>5</sup> In Enterprise Edition è disponibile il supporto fino a 8 repliche secondarie, incluse 5 repliche secondarie sincrone.

<sup>6</sup> Standard Edition supporta i gruppi di disponibilità di base. Un gruppo di disponibilità di base supporta due repliche, con un database. Per altre informazioni sui gruppi di disponibilità di base, vedere [Gruppi di disponibilità di base](../database-engine/availability-groups/windows/basic-availability-groups-always-on-availability-groups.md).

<sup>7</sup> [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default) obbligatorio.

## <a name="rdbms-scalability-and-performance"></a><a name="RDBMSSP"></a> Scalabilità e prestazioni RDBMS

|Funzionalità|Enterprise|Standard|Web|Express with<br/>Advanced Services|Rapida|
|------:|:--------:|:------:|:-:|:-------------------------------:|:------:|
|Columnstore<sup>1</sup> <sup>2</sup>|Sì|Sì|Sì|Sì|Sì|
|File binari di oggetti di grandi dimensioni in indici columnstore cluster|Sì|Sì|Sì|Sì|Sì|
|Ricompilazione degli indici columnstore non cluster online|Sì|No|No|No|No|
|Database in memoria: OLTP in memoria<sup>1</sup>|Sì|Sì|Sì|Sì<sup>3</sup>|Sì|
|Database in memoria: pool di buffer ibrido|Sì|Sì|No|No|No|
|Database in memoria: metadati tempdb ottimizzati per la memoria|Sì|No|No|No|No|
|Database in memoria: supporto memoria persistente|Sì|Sì|Sì|Sì|Sì|
|Stretch database|Sì|Sì|Sì|Sì|Sì|
|Supporto di più istanze|50|50|50|50|50|
|Partizionamento di tabelle e indici|Sì|Sì|Sì|Sì|Sì|
|Compressione dei dati|Sì|Sì|Sì|Sì|Sì|
|Resource Governor|Sì|No|No|No|No|
|Parallelismo della tabella partizionata|Sì|No|No|No|No|
|Più contenitori FILESTREAM|Sì|Sì|Sì|Sì|Sì|
|Allocazione di una matrice di buffer e di memoria in pagine grandi con supporto NUMA|Sì|No|No|No|No|
|Estensione del pool di buffer|Sì|Sì|No|No|No|
|Governance delle risorse di I/O|Sì|No|No|No|No|
|Read-Ahead|Sì|No|No|No|No|
|Analisi avanzata|Sì|No|No|No|No|
|Durabilità posticipata|Sì|Sì|Sì|Sì|Sì|
|Intelligent Database: ottimizzazione automatica|Sì|No|No|No|No|
|Intelligent Database: modalità batch per rowstore <sup>1</sup>|Sì|No|No|No|No|
|Intelligent Database: feedback delle concessioni di memoria in modalità riga|Sì|No|No|No|No|
|Intelligent Database: conteggio distinti approssimato|Sì|Sì|Sì|Sì|Sì|
|Intelligent Database: compilazione posticipata delle variabili di tabella|Sì|Sì|Sì|Sì|Sì|
|Intelligent Database: inlining della funzione definita dall'utente scalare|Sì|Sì|Sì|Sì|Sì|
|Join adattivi in modalità batch|Sì|No|No|No|No|
|Feedback delle concessioni di memoria in modalità batch|Sì|No|No|No|No|
|Esecuzione interleaved per funzioni con valori di tabella a più istruzioni|Sì|Sì|Sì|Sì|Sì|
|Miglioramenti dell'inserimento bulk|Sì|Sì|Sì|Sì|Sì|

<sup>1</sup> Le dimensioni dati OLTP in memoria e la cache dei segmenti Columnstore sono limitate alla quantità di memoria specificata dall'edizione nella sezione [Limiti di scalabilità](#Cross-BoxScaleLimits). Il grado di parallelismo per le operazioni in [modalità batch](../relational-databases/query-processing-architecture-guide.md#batch-mode-execution) è limitato a 2 per [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Standard Edition e 1 per [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Web Edition ed Express Edition. Questo si riferisce agli indici columnstore creati tramite le tabelle basate su disco e le tabelle ottimizzate per la memoria.

<sup>2</sup> La distribuzione dell'aggregazione, la distribuzione dei predicati di stringa e le ottimizzazioni SIMD sono miglioramenti alla scalabilità di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Enterprise Edition. Per altri dettagli, vedere [Indici columnstore - Novità](../relational-databases/indexes/columnstore-indexes-what-s-new.md).

<sup>3</sup> Questa funzionalità non è inclusa nell'opzione di installazione LocalDB.

## <a name="rdbms-security"></a><a name="RDBMSS"></a> Sicurezza RDBMS

|Funzionalità|Enterprise|Standard|Web|Express with<br/>Advanced Services|Express|
|-------|:--------:|:------:|:-:|:-----:|:-----------------------:|:-----:|
|Sicurezza a livello di riga|Sì|Sì|Sì|Sì|Sì|
|Always Encrypted|Sì|Sì|Sì|Sì|Sì|
|Always Encrypted con enclave sicuri|Sì|Sì|Sì|Sì|Sì|
|Maschera dati dinamica|Sì|Sì|Sì|Sì|Sì|
|Controllo server|Sì|Sì|Sì|Sì|Sì|
|Controllo database|Sì|Sì|Sì|Sì|Sì|
|Crittografia trasparente del database|Sì|Sì|Sì|No|No|
|Extensible Key Management|Sì|Sì|No|No|No|
|Ruoli definiti dall'utente|Sì|Sì|Sì|Sì|Sì|
|Database indipendenti|Sì|Sì|Sì|Sì|Sì|
|Crittografia per backup|Sì|Sì|No|No|No|
|Classificazione e controllo dei dati|Sì|Sì|Sì|Sì|Sì|

## <a name="replication"></a><a name="Replication"></a> Replica

|Funzionalità|Enterprise|Standard|Web|Express with<br/>Advanced Services|Rapida|
|-------|:---------:|:-------:|:--:|:--------------------------------:|:------:|
|Sottoscrittori eterogenei|Sì|Sì|No|No|No|
|Replica di tipo merge|Sì|Sì|Sì<sup>1</sup>|Sì<sup>1</sup>|Sì<sup>1</sup>|
|Pubblicazione Oracle|Sì|No|No|No|No|
|Replica transazionale peer-to-peer|Sì|No|No|No|No|
|Replica snapshot|Sì|Sì|Sì<sup>1</sup>|Sì<sup>1</sup>|Sì<sup>1</sup>|
|Rilevamento modifiche di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|Sì|Sì|Sì|Sì|Sì|
|Replica transazionale|Sì|Sì|Sì<sup>1</sup>|Sì<sup>1</sup>|Sì<sup>1</sup>|
|Replica transazionale in Azure|Sì|Sì|No|No|No|
|Sottoscrizione aggiornabile con replica transazionale|Sì|Sì|No|No|No|

<sup>1</sup> Solo Sottoscrittore

## <a name="management-tools"></a><a name="SSMS"></a> Strumenti di gestione

|Funzionalità|Enterprise|Standard|Web|Express with Advanced Services|Express|
|-------|:--------:|:------:|:-:|:----------------------------:|:-----:|
|SMO (SQL Management Objects)|Sì|Sì|Sì|Sì|Sì|
|API Valutazione SQL|Sì|Sì|Sì|Sì|Sì|
|Valutazione della vulnerabilità SQL |Sì|Sì|Sì|Sì|Sì|
|Gestione configurazione SQL Server|Sì|Sì|Sì|Sì|Sì|
|CMD SQL (strumento da riga di comando)|Sì|Sì|Sì|Sì|Sì|
|Riesecuzione distribuita - Strumento di amministrazione|Sì|Sì|Sì|Sì|No|
|Riesecuzione distribuita - Client|Sì|Sì|Sì|No|No|
|Distributed Replay - Controller|Sì<sup>1</sup>|Sì<sup>2</sup>|Sì<sup>2</sup>|No|No|
|SQL Profiler|Sì|Sì|N.<sup>3</sup>|N.<sup>3</sup>|N.<sup>3</sup>|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Agent|Sì|Sì|Sì|No|No|
|Management Pack di Microsoft System Center Operations Manager|Sì|Sì|Sì|No|No|
|Ottimizzazione guidata motore di database (DTA)|Sì|Sì<sup>4</sup>|Sì<sup>4</sup>|No|No|

<sup>1</sup> Fino a 16 client

<sup>2</sup> 1 client

<sup>3</sup> Il profiling di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Web, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express with Tools e [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express with Advanced Services può essere eseguito usando le edizioni [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Standard e [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Enterprise.

<sup>4</sup> Ottimizzazione abilitata solo sulle funzionalità dell'edizione Standard

## <a name="rdbms-manageability"></a><a name="RDBMSM"></a> Gestione RDBMS

|Funzionalità|Enterprise|Standard|Web|Express with<br/>Advanced Services|Rapida|
|-------|:--------:|:------:|:-:|:-------------------------------:|:-----:|
|Istanze utente|No|No|No|Sì|Sì|
|DB locale|No|No|No|Sì|No|
|Connessione amministrativa dedicata|Sì|Sì|Sì|Sì<sup>1</sup>|Sì<sup>1</sup>|
|Supporto SysPrep <sup>2</sup>|Sì|Sì|Sì|Sì|Sì|
|Supporto per script di PowerShell<sup>3</sup>|Sì|Sì|Sì|Sì|Sì|
|Supporto per le operazioni del componente dell'applicazione livello dati (DAC) - estrazione, distribuzione, aggiornamento, eliminazione|Sì|Sì|Sì|Sì|Sì|
|Automazione dei criteri (controllo pianificato e modifica)|Sì|Sì|Sì|No|No|
|Agente di raccolta dati relativi alle prestazioni|Sì|Sì|Sì|No|No|
|Possibilità di registrarsi come un'istanza gestita in una gestione a più istanze|Sì|Sì|Sì|No|No|
|Report di prestazioni standard|Sì|Sì|Sì|No|No|
|Guide di piano e blocco del piano per le guide di piano|Sì|Sì|Sì|No|No|
|Query diretta di viste indicizzate (tramite hint NOEXPAND)|Sì|Sì|Sì|Sì|Sì|
|Query diretta di SQL Server Analysis Services|Sì|Sì|No|No|Sì|
|Gestione automatica viste indicizzate|Sì|Sì|Sì|No|No|
|Viste partizionate distribuite|Sì|No|No|No|No|
|Operazioni indicizzate parallele|Sì|No|No|No|No|
|Utilizzo automatico di viste indicizzate da Query Optimizer|Sì|No|No|No|No|
|Verifica di coerenza parallela|Sì|No|No|No|No|
|Punto di controllo dell'utilità [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|Sì|No|No|No|No|
|Estensione del pool di buffer|Sì|Sì|No|No|No|
|Istanza master per cluster Big Data|Sì|Sì|No|No|No|
|Certificazione di compatibilità|Sì|Sì|Sì|Sì|Sì|

<sup>1</sup> Con flag di traccia

<sup>2</sup> Per altre informazioni, vedere [Considerazioni sull'installazione di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] tramite SysPrep](../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md).

<sup>3</sup> In Linux gli script di PowerShell sono supportati dai computer Windows con destinazione [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] su Linux.

## <a name="development-tools"></a><a name="DevTools"></a> Strumenti di sviluppo

|Funzionalità|Enterprise|Standard|Web|Express with<br/>Advanced Services|Rapida|
|-------|:--------:|:------:|:-:|:-------------------------------:|:-----:|
|Integrazione con Microsoft Visual Studio|Sì|Sì|Sì|Sì|Sì|
|Intellisense (Transact-SQL e MDX)|Sì|Sì|Sì|Sì|Sì|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Data Tools (SSDT)|Sì|Sì|Sì|Sì|No|
|Strumenti di progettazione, debug e modifica MDX|Sì|Sì|No|No|No|

## <a name="programmability"></a><a name="Programmability"></a> Programmability

|Funzionalità|Enterprise|Standard|Web|Express with<br/>Advanced Services|Rapida
|-------|:--------:|:------:|:-:|:-------------------------------:|:-----:|
|Integrazione di R di base<sup>1</sup>|Sì|Sì|Sì|Sì|No|
|Integrazione di R avanzata<sup>2</sup>|Sì|No|No|No|No|
|Integrazione di Python di base|Sì|Sì|Sì|Sì|No|
|Integrazione di Python avanzata|Sì|No|No|No|No|
|Machine Learning Server (Standalone)|Sì|No|No|No|No|
|Nodo di calcolo PolyBase|Sì|Sì<sup>3</sup>|Sì<sup>3</sup>|Sì<sup>3</sup>|Sì<sup>3</sup> |
|Nodo head PolyBase<sup>4</sup>|Sì|Sì|No|No|No|
|JSON|Sì|Sì|Sì|Sì|Sì|
|Archivio query|Sì|Sì|Sì|Sì|Sì|
|Temporale|Sì|Sì|Sì|Sì|Sì|
|Integrazione con Common Language Runtime (CLR)|Sì|Sì|Sì|Sì|Sì|
|Integrazione di Java Language Runtime|Sì|Sì|Sì|Sì|Sì|
|Supporto XML nativo|Sì|Sì|Sì|Sì|Sì|
|Indicizzazione XML|Sì|Sì|Sì|Sì|Sì|
|Funzionalità MERGE e UPSERT|Sì|Sì|Sì|Sì|Sì|
|Supporto FILESTREAM|Sì|Sì|Sì|Sì|Sì|
|FileTable|Sì|Sì|Sì|Sì|Sì|
|Tipi di dati data e ora|Sì|Sì|Sì|Sì|Sì|
|Supporto di internazionalizzazione|Sì|Sì|Sì|Sì|Sì|
|Ricerca full-text e semantica|Sì|Sì|Sì|Sì|No|
|Impostazione della lingua nelle query|Sì|Sì|Sì|Sì|No|
|Service Broker (messaggistica)|Sì|Sì|N.<sup>5</sup>|N.<sup>5</sup>|N.<sup>5</sup>|
|Transact-SQL - endpoint|Sì|Sì|Sì|No|No|
|Grafico|Sì|Sì|Sì|Sì|Sì|
|Supporto UTF-8|Sì|Sì|Sì|Sì|Sì|

<sup>1</sup> L'integrazione di base è limitata a 2 core e ai set di dati in memoria.

<sup>2</sup> L'integrazione avanzata può usare tutti i core disponibili per l'elaborazione parallela dei set di dati con qualsiasi dimensione, nei limiti imposti dall'hardware.

<sup>3</sup> La scalabilità orizzontale con più nodi di calcolo richiede un nodo head.

<sup>4</sup> Per versioni precedenti a SQL Server 2019 il nodo PolyBase richiede l'edizione Enterprise.

<sup>5</sup> Solo per client

## <a name="integration-services"></a><a name="IS"></a> Integration Services

Per informazioni sulle funzionalità di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Integration Services (SSIS) supportate dalle edizioni di [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], vedere [Funzionalità di Integration Services supportate dalle edizioni di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../integration-services/integration-services-features-supported-by-the-editions-of-sql-server.md).

## <a name="master-data-services"></a><a name="MDS"></a> Master Data Services

Per informazioni sulle funzionalità di [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] e Data Quality Services supportate dalle edizioni di [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], vedere [Funzionalità di Master Data Services e Data Quality Services supportate dalle edizioni di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../master-data-services/master-data-services-and-data-quality-services-features-support.md).

## <a name="data-warehouse"></a><a name="DW"></a> Data warehouse

|Funzionalità|Enterprise|Standard|Web|Express with<br/>Advanced Services|Rapida|
|-------|:--------:|:------:|:-:|:-------------------------------:|:-----:|
|Generazione automatica dello schema della gestione temporanea e del data warehouse|Sì|Sì|No|No|No|
|Change Data Capture|Sì|Sì|No|No|No|
|Ottimizzazione query join a stella|Sì|No|No|No|No|
|Elaborazione di query parallela su tabelle e indici partizionati|Sì|No|No|No|No|
|Aggregazione batch globale|Sì|No|No|No|No|

## <a name="analysis-services"></a><a name="SSAS"></a> Analysis Services

Per informazioni sulle funzionalità di Analysis Services supportate dalle edizioni di [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], vedere [Funzionalità di Analysis Services supportate dalle edizioni di SQL Server[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016.md).

## <a name="reporting-services"></a><a name="SSRS"></a> Reporting Services

Per informazioni sulle funzionalità di Reporting Services supportate dalle edizioni di [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], vedere [Funzionalità di Reporting Services supportate dalle edizioni di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md).

## <a name="business-intelligence-clients"></a><a name="BIC"></a> Client di business intelligence

Per informazioni sulle funzionalità del client Business Intelligence supportate dalle edizioni di [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], vedere [Funzionalità di Analysis Services supportate dalle edizioni di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016.md) o [Funzionalità di Reporting Services supportate dalle edizioni di SQL Server[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md).

## <a name="spatial-and-location-services"></a><a name="SLS"></a> Servizi spaziali e basati sulla posizione

|Nome funzionalità|Enterprise|Standard|Web|Express with<br/>Advanced Services|Express|
|-------------|:-------:|:------:|:-:|:-------------------------------:|:-----:|
|Indici spaziali|Sì|Sì|Sì|Sì|Sì|
|Tipi di dati planari e geodetici|Sì|Sì|Sì|Sì|Sì|
|Librerie spaziali avanzate|Sì|Sì|Sì|Sì|Sì|
|Importazione/esportazione di formati di dati spaziali standard del settore|Sì|Sì|Sì|Sì|Sì|

## <a name="additional-database-services"></a><a name="ADS"></a> Servizi di database aggiuntivi

|Nome funzionalità|Enterprise|Standard|Web|Express with<br/>Advanced Services|Rapida|
|------------|:--------:|:------:|:-:|:-------------------------------:|:-----:|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Migration Assistant|Sì|Sì|Sì|Sì|Sì|
|Posta elettronica database|Sì|Sì|Sì|No|No|

## <a name="other-components"></a><a name="Other"></a> Altri componenti

|Nome funzionalità|Enterprise|Standard|Web|Express with<br/>Advanced Services|Rapida|
|------------|:--------:|:------:|:-:|:-------------------------------:|:-----:|
|StreamInsight|StreamInsight Premium Edition|StreamInsight Standard Edition|StreamInsight Standard Edition|No|No|
|StreamInsight HA|StreamInsight Premium Edition|No|No|No|No|

## <a name="next-steps"></a>Passaggi successivi

[Specifiche del prodotto per [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](https://msdn.microsoft.com/library/6445fd53-6844-4170-a86b-7fe76a9f64cb)

[Installazione per [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../database-engine/install-windows/installation-for-sql-server-2016.md)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]
