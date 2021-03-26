---
title: Che cosa sono i cluster Big Data?
titleSuffix: SQL Server Big Data Clusters
description: Informazioni sui cluster Big Data di SQL Server che vengono eseguiti in Kubernetes e offrono opzioni di scale-out per dati relazionali e HDFS.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 01/07/2020
ms.topic: overview
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 5e3b94698712f8359c51f925b70e148917610360
ms.sourcegitcommit: c242f423cc3b776c20268483cfab0f4be54460d4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/25/2021
ms.locfileid: "105551270"
---
# <a name="what-are-big-data-clusters-2019"></a>Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]?

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

A partire da [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)], i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] consentono di distribuire cluster scalabili di contenitori SQL Server, Spark e HDFS in esecuzione in Kubernetes. Questi componenti vengono eseguiti in modo affiancato per permettere la lettura, la scrittura e l'elaborazione di Big Data da Transact-SQL o Spark, in modo da combinare e analizzare facilmente i dati relazionali di alto valore con volumi elevati di Big Data.

Utilizzare [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] per:

- [Distribuire cluster scalabili](./deploy-get-started.md) di contenitori SQL Server, Spark e HDFS in esecuzione in Kubernetes. 
- Leggere, scrivere ed elaborare Big Data da Transact-SQL o Spark.
- Combinare e analizzare con facilità dati relazionali di alto valore con volumi elevati di Big Data.
- Eseguire query su origini dati esterne.
- Archiviare Big Data in HDFS gestito da SQL Server.
- Eseguire query sui dati da più origini dati esterne tramite il cluster.
- Usare i dati per intelligenza artificiale, Machine Learning e altre attività di analisi.
- [Distribuire ed eseguire applicazioni](./concept-application-deployment.md) in [!INCLUDE[big-data-clusters](../includes/ssbigdataclusters-nover.md)].
- Virtualizzare i dati con [PolyBase](../relational-databases/polybase/polybase-guide.md). Eseguire query sui dati da origini dati esterne SQL Server, Oracle, Teradata, MongoDB e ODBC con tabelle esterne.
- Fornire disponibilità elevata per l'istanza master di SQL Server e tutti i database tramite una tecnologia basata su gruppi di disponibilità Always On.

Per altre informazioni sulle nuove funzionalità e sui problemi noti per l'ultima versione, vedere le [note sulla versione](release-notes-big-data-cluster.md).   

Per altre informazioni sui cluster di Big data, vedere [domande frequenti sui cluster di Big Data](big-data-cluster-faq.yml).

## <a name="scenarios"></a>Scenari

I [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] offrono la flessibilità necessaria per interagire con i Big Data. È possibile eseguire query su origini dati esterne, archiviare Big Data in HDFS gestito da SQL Server o eseguire query sui dati da più origini dati esterne tramite il cluster. È quindi possibile usare i dati per intelligenza artificiale, Machine Learning e altre attività di analisi. Le sezioni seguenti forniscono altre informazioni su questi scenari.

### <a name="data-virtualization"></a>Virtualizzazione dei dati

Grazie alla possibilità di sfruttare [PolyBase](../relational-databases/polybase/polybase-guide.md), i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] sono in grado di eseguire query su origini dati esterne senza dover spostare o copiare i dati. [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] introduce nuovi connettori alle origini dati.

![Virtualizzazione dei dati](media/big-data-cluster-overview/data-virtualization.png)

### <a name="data-lake"></a>Data lake

Un cluster Big Data di SQL Server include un *pool di archiviazione* HDFS scalabile. Questo pool può essere usato per archiviare Big Data, potenzialmente inseriti da più origini esterne. Una volta archiviati i Big Data in HDFS nel cluster Big Data, è possibile analizzare ed eseguire query sui dati e combinarli con i dati relazionali.

![Data lake](media/big-data-cluster-overview/data-lake.png)

### <a name="scale-out-data-mart"></a>Data mart con scalabilità orizzontale

I [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] offrono risorse di calcolo e archiviazione con scalabilità orizzontale per migliorare le prestazioni di analisi dei dati. È possibile inserire e distribuire dati provenienti da diverse origini tra più nodi di *pool dati* come cache per ulteriori analisi.

![Data mart](media/big-data-cluster-overview/data-mart.png)

### <a name="integrated-ai-and-machine-learning"></a>Intelligenza artificiale e Machine Learning integrati

I [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] consentono l'esecuzione di attività di intelligenza artificiale e Machine Learning sui dati archiviati in pool di archiviazione HDFS e sui pool di dati. È anche possibile usare Spark nonché strumenti di intelligenza artificiale predefiniti in SQL Server, tramite R, Python, Scala o Java.

![Intelligenza artificiale e Machine Learning](media/big-data-cluster-overview/ai-ml-spark.png)

### <a name="management-and-monitoring"></a>Gestione e monitoraggio

La gestione e il monitoraggio vengono forniti tramite una combinazione di strumenti da riga di comando, API, portali e viste a gestione dinamica.

È possibile usare [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md) per eseguire un'ampia gamma di attività sul cluster Big Data:
- Frammenti predefiniti per attività di gestione comuni.
- Possibilità di esplorare HDFS, caricare file, visualizzare file in anteprima e creare directory.
- Possibilità di creare, aprire ed eseguire notebook compatibili con Jupyter.
- Procedura guidata di virtualizzazione dei dati per semplificare la creazione di origini dati esterne (funzionalità abilitata dall'**estensione di virtualizzazione dei dati**).

## <a name="architecture"></a><a id="architecture"></a> Architettura

Un cluster Big Data di SQL Server è un cluster di contenitori Linux orchestrati da [Kubernetes](https://kubernetes.io/docs/concepts/).

### <a name="kubernetes-concepts"></a>Concetti relativi a Kubernetes

Kubernetes è un agente di orchestrazione di contenitori open source, che può ridimensionare le distribuzioni di contenitori in base alle esigenze. La tabella seguente definisce alcuni importanti termini di Kubernetes:

|Termine|Descrizione|
|:--|:--|
| **Cluster** | Un cluster Kubernetes è un set di computer, noti come nodi. Un nodo controlla il cluster e viene designato come nodo master, mentre i nodi rimanenti sono nodi di lavoro. Il nodo master Kubernetes è responsabile della distribuzione del lavoro tra i nodi di lavoro e del monitoraggio dell'integrità del cluster. |
| **Node** | Un nodo esegue applicazioni in contenitori. Può essere un computer fisico o una macchina virtuale. Un cluster Kubernetes può contenere una combinazione di nodi computer fisico e macchina virtuale. |
| **Pod** | Un pod è l'unità di distribuzione atomica di Kubernetes. Un pod è un gruppo logico di uno o più contenitori, nonché delle risorse associate, necessari per eseguire un'applicazione. Ogni pod viene eseguito in un nodo e un nodo può eseguire uno o più pod. Il nodo master Kubernetes assegna automaticamente pod a nodi nel cluster. |
| &nbsp; ||

Nei [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] Kubernetes è responsabile dello stato dei [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] e compila e configura i nodi del cluster, assegna pod ai nodi ed esegue il monitoraggio dell'integrità del cluster.

### <a name="big-data-clusters-architecture"></a>Architettura dei cluster Big Data

Il diagramma seguente mostra i componenti di un cluster Big Data di SQL Server:

![Panoramica dell'architettura](media/big-data-cluster-overview/architecture-diagram-overview.png)

### <a name="controller"></a><a id="controlplane"></a> Controller

Il controller fornisce la gestione e la sicurezza per il cluster. Contiene il servizio di controllo, l'archivio di configurazione e altri servizi a livello di cluster, tra cui Kibana, Grafana ed ElasticSearch.

### <a name="compute-pool"></a><a id="computeplane"></a> Pool di calcolo

Il pool di calcolo fornisce risorse di calcolo al cluster. Contiene nodi che eseguono SQL Server in pod Linux. I Pod nel pool di calcolo sono divisi in *istanze di calcolo SQL* per attività di elaborazione specifiche. 

### <a name="data-pool"></a><a id="dataplane"></a> Pool di dati

Il pool di dati viene usato per il salvataggio permanente e la memorizzazione nella cache dei dati. Il pool di dati è costituito da uno o più pod che eseguono SQL Server in Linux. Viene usato per inserire dati da query SQL o processi Spark. I data mart del cluster Big Data di SQL Server vengono salvati in modo permanente nel pool di dati. 

### <a name="storage-pool"></a>Pool di archiviazione

Il pool di archiviazione è formato da pod del pool di archiviazione costituiti da SQL Server in Linux, Spark, and HDFS. Tutti i nodi di archiviazione in un cluster Big Data di SQL Server sono membri di un cluster HDFS.

> [!TIP]
> Per informazioni dettagliate sull'architettura e sull'installazione dei cluster Big Data, vedere [Workshop: Architettura Microsoft dei [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](https://github.com/microsoft/sqlworkshops-bdc).

## <a name="next-steps"></a>Passaggi successivi

* Per altre informazioni sulla distribuzione di [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vedere [Introduzione ai [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deploy-get-started.md).    
* Vedere le [domande frequenti sui cluster di Big Data](big-data-cluster-faq.yml).  