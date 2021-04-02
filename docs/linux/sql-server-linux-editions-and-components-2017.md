---
title: Edizioni e funzionalità supportate di SQL Server 2017 ~ Linux
description: Questo articolo descrive le funzionalità supportate dalle diverse edizioni di SQL Server 2017 in Linux. Consente di scegliere tra le edizioni e i componenti disponibili.
ms.date: 01/14/2020
ms.prod: sql
ms.technology: linux
ms.topic: conceptual
helpviewer_keywords:
- Enterprise Edition [SQL Server]
- Developer Edition [SQL Server]
- default components
- installing SQL Server, components
- Setup [SQL Server], components
- SQL Server, editions
- SQL Server, components
- editions [SQL Server]
- versions [SQL Server]
- Setup [SQL Server], editions
- SQL Server Installation Wizard
- components [SQL Server]
- Standard Edition [SQL Server]
- installing SQL Server, editions
- editions [SQL Server], about edition options
- Setup [SQL Server]
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.openlocfilehash: fe15c32e5536aee8135e0a32e9365ed01dc40c8e
ms.sourcegitcommit: 2db7412d30722f198cbafcd683bd4da206b33996
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/31/2021
ms.locfileid: "106099955"
---
# <a name="editions-and-supported-features-of-sql-server-2017-on-linux"></a>Edizioni e funzionalità supportate di SQL Server 2017 in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo articolo offre informazioni dettagliate sulle funzionalità supportate dalle diverse edizioni di SQL Server 2017 in Linux. Per le edizioni e le funzionalità supportate di SQL Server in Windows, vedere [ SQL Server 2017 - Windows](../sql-server/editions-and-components-of-sql-server-2017.md).  
  
I requisiti di installazione variano in base alle esigenze dell'applicazione. Le diverse edizioni di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] consentono di soddisfare le esigenze specifiche di utenti e organizzazioni in termini di prezzo, esecuzione e prestazioni. I componenti di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] installati dipendono inoltre dai requisiti specifici. Nelle sezioni seguenti vengono fornite tutte le informazioni necessarie per adottare la scelta migliore tra le edizioni e i componenti disponibili in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  

Per le note sulla versione più recenti e informazioni sulle novità, vedere quanto segue:
- [Note sulla versione di SQL Server 2017 in Linux](sql-server-linux-release-notes.md)
- [Novità di SQL Server 2017 in Linux](sql-server-linux-whats-new.md)

Per l'elenco delle funzionalità di SQL Server non disponibili in Linux, vedere [Funzionalità e servizi non supportati](#Unsupported).

### <a name="try-sql-server"></a>Per provare SQL Server    
    
[Download di SQL Server 2017](https://www.microsoft.com/sql-server/sql-server-2017)

## <a name="ssnoversion-editions"></a>Edizioni di[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)]  
 La tabella seguente descrive tali edizioni di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. 
  
|Edizione di[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|Definizione|  
|---------------------------------------|----------------|  
|Enterprise|L'offerta Premium, [!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Enterprise Edition, offre le funzionalità complete dei centri dati di fascia alta con prestazioni velocissime, abilitando livelli di servizio elevati per carichi di lavoro di importanza critica.|  
|Standard|[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] L'edizione standard offre una gestione dei dati di base per i reparti e le organizzazioni di piccole dimensioni per eseguire le applicazioni e supporta gli strumenti di sviluppo comuni per la gestione di database in locale e abilitata al cloud con risorse IT minime.|  
|Web|[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Web Edition costituisce un'opzione con un costo totale di proprietà ridotto per provider di servizi di hosting Web e VAP Web, offrendo funzionalità di scalabilità, convenienza e facilità di gestione per proprietà Web di ogni dimensione.|  
|Developer|L'edizione[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Developer consente agli sviluppatori di compilare qualsiasi tipo di applicazione in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Benché includa tutte le funzionalità dell'edizione Enterprise, ne è consentito l'utilizzo solo come sistema di sviluppo e di prova e non come server di produzione. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Developer rappresenta la scelta ideale per chi desidera compilare e testare applicazioni.|  
|Express edition|L'edizione Express è un database di base gratuito, ideale per l'apprendimento e la compilazione di applicazioni basate sui dati desktop e server di piccole dimensioni. Questa edizione costituisce la scelta ottimale per fornitori di software indipendenti, sviluppatori e sviluppatori amatoriali di applicazioni client. Se sono necessarie funzionalità di database più avanzate, è possibile aggiornare facilmente [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express a versioni di fascia superiore di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].|  
  
## <a name="using-ssnoversion-with-clientserver-applications"></a>Uso di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] con applicazioni client/server  

È possibile installare solo i componenti client di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] in un computer in cui vengono eseguite applicazioni client/server connesse direttamente a un'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. L'installazione di componenti client rappresenta una scelta ottimale anche se si amministra un'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] in un server di database o se si prevede di sviluppare applicazioni basate su [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
## <a name="ssnoversion-components"></a>Componenti di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]  

SQL Server 2017 in Linux supporta il motore di database di SQL Server. La tabella seguente descrive le funzionalità del motore di database.   
  
|Componenti server|Descrizione|  
|-----------------------|-----------------|  
|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] include il [!INCLUDE[ssDE](../includes/ssde-md.md)], il servizio principale per l'archiviazione, l'elaborazione e la protezione dei dati, la replica, la ricerca full-text, gli strumenti per la gestione di dati XML e relazionali e l'integrazione dell'analisi dei dati.|  

**Developer Edition, Enterprise Core Edition ed Evaluation Edition**  
Per le funzionalità supportate dalle edizioni Developer Edition, Enterprise Core Edition ed Evaluation Edition, vedere le funzionalità elencate per SQL Server Enterprise Edition nelle tabelle seguenti.

La versione Developer Edition continua a supportare un solo client per la [Riesecuzione distribuita di Microsoft SQL Server](../tools/distributed-replay/sql-server-distributed-replay.md). 
  
##  <a name="scale-limits"></a><a name="Cross-BoxScaleLimits"></a> Limiti di scalabilità  
  
|Funzionalità|Enterprise|Standard|Web|Express| 
|-------------|----------------|--------------|---------|------------------------|
|Capacità di calcolo massima usata da una singola istanza - [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]<sup>1</sup>|Valore massimo del sistema operativo|Limitato a meno di 4 socket o 24 core|Limitato a meno di 4 socket o 16 core|Limitato a meno di 1 socket o 4 core| 
|Capacità di calcolo massima usata da una singola istanza - [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] oppure [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|Valore massimo del sistema operativo|Limitato a meno di 4 socket o 24 core|Limitato a meno di 4 socket o 16 core|Limitato a meno di 1 socket o 4 core|
|Memoria massima per il pool di buffer per ogni istanza di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|valore massimo del sistema operativo|128 GB|64 GB|1410 MB|
|Memoria massima per la cache dei segmenti Columnstore per ogni istanza di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Memoria illimitata| 32 GB| 16 GB| 352 MB|  
|Dimensione massima dati ottimizzati per la memoria per ogni database in [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Memoria illimitata| 32 GB| 16 GB| 352 MB|
|Dimensione massima del database relazionale|524 PB|524 PB|524 PB|10 GB|  
  
<sup>1</sup> La licenza basata su Enterprise Edition con Server + Licenza CAL (Client Access License), non disponibile per nuovi contratti, è limitata a un massimo di 20 core per istanza di SQL Server. Non sono previsti limiti nel modello di licenza server basato su core. Per altre informazioni, vedere [Limiti della capacità di calcolo per edizione di SQL Server](../sql-server/compute-capacity-limits-by-edition-of-sql-server.md).  
 
##  <a name="rdbms-high-availability"></a><a name="RDBMSHA"></a> Disponibilità elevata RDBMS  
  
|Funzionalità|Enterprise|Standard|Web|Express|  
|-------------|----------------|--------------|---------|------------------------|  
|Log shipping|Sì|Sì|Sì|No|  
|Compressione backup|Sì|Sì|No|No| 
|Snapshot del database|Sì|No|No|No|
|Istanza del cluster di failover Always On<sup>1</sup>|Sì|Sì|No|No| 
|Gruppi di disponibilità AlwaysOn<sup>2</sup>|Sì|No|No|No|
|Gruppi di disponibilità di base<sup>3</sup>|No|Sì|No|No|
|Gruppo di disponibilità con commit di un numero minimo di repliche|Sì|Sì|No|No|
|Gruppo di disponibilità senza cluster|Sì|Sì|No|No|
|Ripristino di pagine e file online|Sì|No|No|No|
|Indicizzazione online|Sì|No|No|No|
|Ricompilazioni degli indici online ripristinabili|Sì|No|No|No|
|Modifica dello schema online|Sì|No|No|No|
|Recupero rapido|Sì|No|No|No|
|Backup con mirroring|Sì|No|No|No|
|Aggiunta di memoria a caldo e CPU|Sì|No|No|No|
|Backup crittografato|Sì|Sì|No|No|
|Backup ibrido in Microsoft Azure (backup nell'URL)|Sì|Sì|No|No|
  
<sup>1</sup> In Enterprise Edition il numero di nodi corrisponde al valore massimo del sistema operativo. Nella versione Standard Edition è disponibile il supporto per due nodi. 

<sup>2</sup> In Enterprise Edition è disponibile il supporto fino a 8 repliche secondarie, incluse 2 repliche secondarie sincrone. 

<sup>3</sup> Standard Edition supporta i gruppi di disponibilità di base. Un gruppo di disponibilità di base supporta due repliche, con un database. Per altre informazioni sui gruppi di disponibilità di base, vedere [Gruppi di disponibilità di base](../database-engine/availability-groups/windows/basic-availability-groups-always-on-availability-groups.md).    

##  <a name="rdbms-scalability-and-performance"></a><a name="RDBMSSP"></a> Scalabilità e prestazioni RDBMS  
  
|Funzionalità|Enterprise|Standard|Web|Express|  
|-------------|----------------|--------------|---------|------------------------| 
|Columnstore <sup>1</sup>|Sì|Sì|Sì|Sì|  
|File binari di oggetti di grandi dimensioni in indici columnstore cluster|Sì|Sì|Sì|Sì|  
|Ricompilazione degli indici columnstore non cluster online|Sì|No|No|No|
|OLTP in memoria <sup>1</sup>|Sì|Sì|Sì|Sì|
|Memoria principale persistente|Sì|Sì|Sì|Sì|
|Partizionamento di tabelle e indici|Sì|Sì|Sì|Sì|  
|Compressione dei dati|Sì|Sì|Sì|Sì|
|Resource Governor|Sì|No|No|No|  
|Parallelismo della tabella partizionata|Sì|No|No|No|
|Allocazione di una matrice di buffer e di memoria in pagine grandi con supporto NUMA|Sì|No|No|No|
|Governance delle risorse di I/O|Sì|No|No|No|  
|Durabilità posticipata|Sì|Sì|Sì|Sì|
|Ottimizzazione automatica|Sì|No|No|No|
|Join adattivi in modalità batch|Sì|No|No|No|
|Feedback delle concessioni di memoria in modalità batch|Sì|No|No|No|
|Esecuzione interleaved per funzioni con valori di tabella a più istruzioni|Sì|Sì|Sì|Sì|
|Miglioramenti dell'inserimento bulk|Sì|Sì|Sì|Sì|


<sup>1</sup> Le dimensioni dati OLTP in memoria e la cache dei segmenti Columnstore sono limitate alla quantità di memoria specificata dall'edizione nella sezione Limiti di scalabilità. I gradi di parallelismo (DOP) massimi sono limitati. I gradi di parallelismo del processo (DOP) per le compilazioni di indici sono limitati a 2 per Standard Edition e a 1 per le Web Edition ed Express Edition. Questo si riferisce agli indici columnstore creati tramite le tabelle basate su disco e le tabelle ottimizzate per la memoria.

##  <a name="rdbms-security"></a><a name="RDBMSS"></a> Sicurezza RDBMS  
  
|Funzionalità|Enterprise|Standard|Web|Express|
|-------------|----------------|--------------|---------|------------------------------------| 
|Sicurezza a livello di riga|Sì|Sì|Sì|Sì|  
|Always Encrypted|Sì|Sì|Sì|Sì| 
|Maschera dati dinamica|Sì|Sì|Sì|Sì|   
|Controllo di base|Sì|Sì|Sì|Sì| 
|Controllo con granularità fine|Sì|Sì|Sì|Sì| 
|Crittografia trasparente del database|Sì|No|No|No|   
|Ruoli definiti dall'utente|Sì|Sì|Sì|Sì| 
|Database indipendenti|Sì|Sì|Sì|Sì| 
|Crittografia per backup|Sì|Sì|No|No|  

##  <a name="rdbms-manageability"></a><a name="RDBMSM"></a> Gestione RDBMS  
  
|Funzionalità|Enterprise|Standard|Web|Express|   
|-------------|----------------|--------------|---------|------------------------|  
|Connessione amministrativa dedicata|Sì|Sì|Sì|Sì, con flag di traccia|   
|Supporto per script di PowerShell|Sì|Sì|Sì|Sì| 
|Supporto per le operazioni del componente dell'applicazione livello dati (DAC) - estrazione, distribuzione, aggiornamento, eliminazione|Sì|Sì|Sì|Sì| 
|Automazione dei criteri (controllo pianificato e modifica)|Sì|Sì|Sì|No|  
|Agente di raccolta dati relativi alle prestazioni|Sì|Sì|Sì|No|
|Report di prestazioni standard|Sì|Sì|Sì|No|
|Guide di piano e blocco del piano per le guide di piano|Sì|Sì|Sì|No| 
|Query diretta di viste indicizzate (tramite hint NOEXPAND)|Sì|Sì|Sì|Sì| 
|Gestione automatica viste indicizzate|Sì|Sì|Sì|No|
|Viste partizionate distribuite|Sì|No|No|No| 
|Operazioni indicizzate parallele|Sì|No|No|No|  
|Utilizzo automatico di viste indicizzate da Query Optimizer|Sì|No|No|No| 
|Verifica di coerenza parallela|Sì|No|No|No| 
|Punto di controllo dell'Utilità SQL Server|Sì|No|No|No|    

##  <a name="programmability"></a><a name="Programmability"></a> Programmability  
  
|Funzionalità|Enterprise|Standard|Web|Express 
|-------------|----------------|--------------|---------|------------------------|  
|JSON|Sì|Sì|Sì|Sì|   
|Archivio query|Sì|Sì|Sì|Sì|   
|Temporale|Sì|Sì|Sì|Sì|   
|Supporto XML nativo|Sì|Sì|Sì|Sì| 
|Indicizzazione XML|Sì|Sì|Sì|Sì| 
|Funzionalità MERGE e UPSERT|Sì|Sì|Sì|Sì|   
|Tipi di dati data e ora|Sì|Sì|Sì|Sì|  
|Supporto di internazionalizzazione|Sì|Sì|Sì|Sì| 
|Ricerca full-text e semantica|Sì|Sì|Sì|Sì|
|Impostazione della lingua nelle query|Sì|Sì|Sì|Sì|
|Service Broker (messaggistica)|Sì|Sì|No (solo client)|No (solo client)|
|Transact-SQL - endpoint|Sì|Sì|Sì|No|
|Grafico|Sì|Sì|Sì|Sì|  


<sup>1</sup> La scalabilità orizzontale con più nodi di calcolo richiede un nodo head.

## <a name="integration-services"></a><a name="IS"></a> Integration Services

Per informazioni sulle funzionalità di Integration Services (SSIS) supportate dalle edizioni di [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], vedere [Funzionalità di Integration Services supportate dalle edizioni di SQL Server](../integration-services/integration-services-features-supported-by-the-editions-of-sql-server.md).

##  <a name="spatial-and-location-services"></a><a name="SLS"></a> Servizi spaziali e basati sulla posizione  
  
|Nome funzionalità|Enterprise|Standard|Web|Express|  
|------------------|----------------|--------------|---------|------------------------------------|------------------------|
|Indici spaziali|Sì|Sì|Sì|Sì|   
|Tipi di dati planari e geodetici|Sì|Sì|Sì|Sì| 
|Librerie spaziali avanzate|Sì|Sì|Sì|Sì|   
|Importazione/esportazione di formati di dati spaziali standard del settore|Sì|Sì|Sì|Sì|   

## <a name="unsupported-features--services"></a><a name="Unsupported"></a> Funzionalità e servizi non supportati

Le funzionalità e i servizi seguenti non sono disponibili in SQL Server 2017 in Linux. Il supporto di queste funzionalità aumenterà nel corso del tempo.

| Area | Funzionalità o servizio non supportato |
|-----|-----|
| **Motore di database** | Replica di tipo merge |
| &nbsp; | Stretch DB |
| &nbsp; | PolyBase |
| &nbsp; | Query distribuita con connessioni di terze parti |
| &nbsp; | Server collegati a origini dati diverse da [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]  |
| &nbsp; | Stored procedure estese di sistema (XP_CMDSHELL e così via) |
| &nbsp; | Filetable, FILESTREAM |
| &nbsp; | Assembly CLR con il set di autorizzazioni EXTERNAL_ACCESS o UNSAFE |
| &nbsp; | Estensione pool di buffer |
| &nbsp; | Backup su URL - BLOB di pagine<sup>1</sup>|
| **SQL Server Agent** |  Sottosistemi: CmdExec, PowerShell, Agente di lettura coda, SSIS, SSAS, SSRS |
| &nbsp; | Avvisi |
| &nbsp; | Agente di lettura log |
| &nbsp; | Backup gestito |
| **Disponibilità elevata** | Mirroring del database  |
| **Sicurezza** | Extensible Key Management |
| &nbsp; | Autenticazione AD per i server collegati |
| &nbsp; | Autenticazione AD per gli endpoint del gruppo di disponibilità (AG) |
| **Services** | SQL Server Browser |
| &nbsp; | R Services per SQL Server |
| &nbsp; | StreamInsight |
| &nbsp; | Analysis Services |
| &nbsp; | Reporting Services |
| &nbsp; | Data Quality Services |
| &nbsp; | Master Data Services |
  
<sup>1</sup> Il backup su URL è supportato per i BLOB in blocchi, usando la [firma di accesso condiviso](../relational-databases/backup-restore/sql-server-backup-to-url.md#SAS).

## <a name="next-steps"></a>Passaggi successivi
 [Edizioni e funzionalità supportate per SQL Server 2019 - Windows](../sql-server/editions-and-components-of-sql-server-version-15.md)  
 [Edizioni e funzionalità supportate per SQL Server 2017 - Windows](../sql-server/editions-and-components-of-sql-server-2017.md)  
 [Edizioni e funzionalità supportate per SQL Server 2016 - Windows](../sql-server/editions-and-components-of-sql-server-2016.md)  
 [Installation for SQL Server](../database-engine/install-windows/install-sql-server.md) (Installazione per SQL Server)  
 [Documentazione di SQL Server](../sql-server/index.yml)