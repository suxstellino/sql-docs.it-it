---
title: Che cos'è l'istanza master?
titleSuffix: SQL Server big data clusters
description: Questo articolo descrive l'istanza master di SQL Server in un cluster Big Data di SQL Server 2019.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 08/21/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: bd97d5b0da5d88974dcd56aa0f32cbc3243532f0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052126"
---
# <a name="what-is-the-master-instance-in-a-sql-server-big-data-cluster"></a>Che cos'è l'istanza master in un cluster Big Data di SQL Server?

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questo articolo descrive il ruolo dell'*istanza master di SQL Server* in un cluster Big Data di SQL Server. L'istanza master è un'istanza di SQL Server in esecuzione in un cluster Big Data di SQL Server per gestire connettività, query con scalabilità orizzontale, database utente e metadati e Machine Learning Services.

L'istanza master di SQL Server offre le funzionalità seguenti:

## <a name="connectivity"></a>Connettività

L'istanza master di SQL Server fornisce un endpoint TDS accessibile esternamente per il cluster. È possibile connettere applicazioni o strumenti di SQL Server come Azure Data Studio o SQL Server Management Studio a questo endpoint allo stesso modo di qualsiasi altra istanza di SQL Server.

## <a name="scale-out-query-management"></a>Gestione delle query con scalabilità orizzontale

L'istanza master di SQL Server contiene il motore di query con scalabilità orizzontale usato per distribuire query tra istanze di SQL Server nei nodi del [pool di calcolo](concept-compute-pool.md). Il motore di query con scalabilità orizzontale fornisce anche l'accesso tramite Transact-SQL a tutte le tabelle Hive nel cluster senza configurazioni aggiuntive.

## <a name="metadata-and-user-databases"></a>Database di metadati e utente

Oltre ai database di sistema di SQL Server standard, l'istanza master di SQL Server contiene anche gli elementi seguenti:

- Un database di metadati che contiene i metadati della tabella HDFS.
- Una mappa partizioni del piano dati.
- Informazioni dettagliate sulle tabelle esterne che permettono di accedere al piano dati del cluster.
- Origini dati esterne e tabelle esterne PolyBase definite nei database utente.

È anche possibile scegliere di aggiungere i propri database utente all'istanza master di SQL Server.

## <a name="machine-learning-services"></a>Machine Learning Services

Machine Learning Services per SQL Server è una funzionalità aggiuntiva per il motore di database, usata per l'esecuzione di codice Java, R e Python in SQL Server. Questa funzionalità è basata sul framework di estendibilità di SQL Server, che isola i processi esterni dai processi del motore di base, ma si integra completamente con i dati relazionali come stored procedure, come script T-SQL contenenti istruzioni R o Python o come codice Java, R o Python contenente T-SQL.

Nell'ambito di un cluster Big Data di SQL Server, Machine Learning Services sarà disponibile nell'istanza master di SQL Server per impostazione predefinita. Questo significa che una volta abilitata l'esecuzione di script esterni nell'istanza master di SQL Server, sarà possibile eseguire script Java, R e Python usando sp_execute_external_script.

### <a name="advantages-of-machine-learning-services-in-a-big-data-cluster"></a>Vantaggi di Machine Learning Services in un cluster Big Data

[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] semplifica l'unione di Big Data ai dati dimensionali generalmente archiviati nel database aziendale. Il valore dei Big Data aumenta notevolmente se questi non sono disponibili solo parzialmente in un'organizzazione, ma sono inclusi anche in report, dashboard e applicazioni. Allo stesso tempo, i data scientist possono continuare a usare gli strumenti dell'ecosistema Spark/HDFS e hanno semplice accesso in tempo reale ai dati nell'istanza master di SQL Server e nelle origini dati esterne accessibili _tramite_ l'istanza master di SQL Server.

Con [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] è possibile eseguire molte più operazioni con i data lake aziendali. Sviluppatori e analisti di SQL Server possono eseguire queste operazioni:

* Compilare applicazioni che utilizzano dati di data lake aziendali.
* Riflettere su tutti i dati con query Transact-SQL.
* Usare l'ecosistema esistente di strumenti e applicazioni di SQL Server per accedere ai dati aziendali e analizzarli.
* Ridurre la necessità di spostamento dei dati tramite la virtualizzazione dei dati e i data mart.
* Continuare a usare Spark per scenari di Big Data.
* Compilare applicazioni aziendali intelligenti con Spark o SQL Server per eseguire il training di modelli sui data lake.
* Rendere operativi i modelli nei database di produzione per ottenere prestazioni ottimali.
* Trasmettere i dati direttamente nei data mart aziendali per l'analisi in tempo reale.
* Esplorare visivamente i dati usando analisi interattive e strumenti di business intelligence.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sui [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vedere le risorse seguenti:

- [Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)
- [Workshop: Architettura Microsoft dei [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](https://github.com/Microsoft/sqlworkshops/tree/master/sqlserver2019bigdataclusters)
