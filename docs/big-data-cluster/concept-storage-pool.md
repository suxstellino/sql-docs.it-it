---
title: Che cos'è il pool di archiviazione?
titleSuffix: SQL Server big data clusters
description: Informazioni sul ruolo del pool di archiviazione di SQL Server in un cluster Big Data di SQL Server 2019 e sull'architettura e le funzionalità di un pool di archiviazione SQL.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 10/01/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 25877bc5f69af865a32389e5c293ace7cab09c10
ms.sourcegitcommit: c242f423cc3b776c20268483cfab0f4be54460d4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/25/2021
ms.locfileid: "105551531"
---
# <a name="what-is-the-storage-pool-in-a-sql-server-big-data-cluster"></a>Che cos'è il pool di archiviazione in un cluster Big Data di SQL Server?

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questo articolo descrive il ruolo del *pool di archiviazione di SQL Server* in un cluster Big Data di SQL Server. Le sezioni seguenti descrivono l'architettura e le funzionalità di un pool di archiviazione.

## <a name="storage-pool-architecture"></a>Architettura dei pool di archiviazione

Il pool di archiviazione è il cluster HDFS (Hadoop) locale in un cluster Big Data di SQL Server. Fornisce l'archiviazione permanente per i dati non strutturati e semistrutturati. I file di dati, ad esempio Parquet o testo delimitato, possono essere archiviati nel pool di archiviazione. Per ottenere l'archiviazione permanente, a ogni pod nel pool è associato un volume permanente. I file del pool di archiviazione sono accessibili con [PolyBase](../relational-databases/polybase/polybase-guide.md) tramite SQL Server o direttamente usando Apache Knox Gateway.

Una configurazione di HDFS classica è costituita da un set di computer con hardware commerciale e archiviazione collegata. I dati vengono distribuiti in blocchi tra i nodi per la tolleranza di errore e per sfruttare l'elaborazione parallela. Uno dei nodi del cluster funge da nodo dei nomi e contiene le informazioni dei metadati relative ai file presenti nei nodi dei dati.

![Configurazione di HDFS classica](media/concept-storage-pool/classic-hdfs-setup.png)

Il pool di archiviazione è costituito da nodi di archiviazione membri di un cluster HDFS. Esegue uno o più pod Kubernetes con ogni pod che ospita i contenitori seguenti:

- Un contenitore Hadoop collegato a un volume permanente (archiviazione). Tutti i contenitori di questo tipo nel loro insieme formano il cluster Hadoop. All'interno del contenitore Hadoop è disponibile un processo di gestione dei nodi YARN che può creare processi di lavoro Apache Spark su richiesta. Il nodo head Spark ospita il metastore hive, la cronologia di Spark e i contenitori di cronologia dei processi YARN.
- Un'istanza di SQL Server per leggere i dati da HDFS usando la tecnologia OpenRowSet.
- `collectd` per la raccolta dei dati di metrica.
- `fluentbit` per la raccolta dei dati di log.

![Architettura dei pool di archiviazione](media/concept-storage-pool/scale-big-data-on-demand.png)

## <a name="responsibilities"></a>Responsabilità

I nodi di archiviazione sono responsabili delle attività seguenti:

- Inserimento di dati tramite Apache Spark.
- Archiviazione dei dati in HDFS (formato parquet e testo delimitato). HDFS fornisce anche la persistenza dei dati, tenendo conto che i dati HDFS vengono distribuiti tra tutti i nodi di archiviazione del cluster Big Data di SQL Server.
- Accesso ai dati tramite gateway HDFS ed endpoint SQL Server.

## <a name="accessing-data"></a>Accesso ai dati

I metodi principali per accedere ai dati nel pool di archiviazione sono:

- Processi Spark.
- Utilizzo di tabelle esterne di SQL Server per consentire l'esecuzione di query sui dati tramite i nodi di calcolo PolyBase e le istanze di SQL Server in esecuzione sui nodi HDFS.

È anche possibile interagire con HDFS usando:

- Azure Data Studio.
- [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)].
- kubectl per eseguire comandi nel contenitore Hadoop.
- Gateway HTTP HDFS.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sui [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vedere le risorse seguenti:

- [Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)
- [Workshop: Architettura Microsoft dei [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](https://github.com/microsoft/sqlworkshops-bdc)
