---
title: Che cosa sono i pool di dati?
titleSuffix: SQL Server big data clusters
description: Informazioni sul ruolo del pool di dati di SQL Server in un cluster Big Data di SQL Server e sull'architettura e sulle funzionalità di un pool di dati SQL.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 08/21/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 69ef4f739b35c55a31cb19fd8cb317d7750f8639
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047479"
---
# <a name="what-are-data-pools-in-a-sql-server-big-data-cluster"></a>Che cosa sono i pool di dati in un cluster Big Data di SQL Server?

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questo articolo descrive il ruolo dei *pool di dati di SQL Server* in un cluster Big Data di SQL Server. Le sezioni seguenti descrivono l'architettura, le funzionalità e gli scenari di utilizzo di un pool di dati.

Questo video di 5 minuti introduce i pool di dati e mostra come eseguire query sui dati dai pool di dati:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Querying-Data-from-Big-Data-Cluster-Data-Pool/player?WT.mc_id=dataexposed-c9-niner]

## <a name="data-pool-architecture"></a>Architettura dei pool di dati

Un pool di dati è costituito da una o più istanze di pool di dati di SQL Server che forniscono risorse di archiviazione permanenti di SQL Server per il cluster. Consente di eseguire query sulle prestazioni dei dati memorizzati nella cache su origini dati esterne e l'offload del lavoro. I dati vengono inseriti nel pool di dati usando query T-SQL o processi Spark. Per migliorare le prestazioni tra set di dati di grandi dimensioni, i dati inseriti vengono distribuiti in partizioni e archiviati in tutte le istanze di SQL Server nel pool. I metodi di distribuzione supportati sono round robin e con replica. Per l'ottimizzazione dell'accesso in lettura, viene creato un indice columnstore cluster in ogni tabella in ogni istanza del pool di dati. Un pool di dati funge da data mart con scalabilità orizzontale per [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)].

![Data mart con scalabilità orizzontale](media/concept-data-pool/data-virtualization-improvements.png)

L'accesso alle istanze di SQL Server nel pool di dati viene gestito dall'istanza master di SQL Server. Viene creata un'origine dati esterna per il pool di dati, insieme alle tabelle esterne di PolyBase per archiviare la cache di dati. In background, il controller crea un database nel pool di dati con tabelle corrispondenti alle tabelle esterne. Dall'istanza master di SQL Server il flusso di lavoro è trasparente. Il controller reindirizza le richieste specifiche di tabelle esterne alle istanze di SQL Server nel pool di dati, eventualmente tramite il pool di calcolo, esegue le query e restituisce il set di risultati. I dati nel pool di dati possono essere solo inseriti o sottoposti a query e non possono essere modificati. Eventuali aggiornamenti dei dati richiedono pertanto l'eliminazione della tabella, seguita dalla ricreazione della tabella e dal successivo ripopolamento dei dati.

## <a name="data-pool-scenarios"></a>Scenari di pool di dati

 La creazione di report è uno scenario comune per i pool di dati. Ad esempio, per una query complessa per il join di più origini dati di PolyBase, usata per un report settimanale, potrebbe essere eseguito l'offload al pool di dati. I dati memorizzati nella cache supportano il calcolo rapido in locale ed evitano di dover tornare ai set di dati originali. Analogamente, i dati di dashboard che richiedono un aggiornamento periodico possono essere memorizzati nella cache nel pool di dati per ottimizzare la creazione di report. Anche l'esplorazione ripetuta di Machine Learning può trarre vantaggio dalla memorizzazione nella cache dei set di dati nel pool di dati.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sui [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vedere le risorse seguenti:

- [Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)
- [Workshop: Architettura Microsoft dei [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](https://github.com/Microsoft/sqlworkshops/tree/master/sqlserver2019bigdataclusters)
