---
title: Risorse di amministrazione per i cluster Big Data (BDC)
titleSuffix: SQL Server
description: Questo articolo illustra come visualizzare lo stato di un cluster Big Data usando Azure Data Studio, i notebook e i comandi di Azure Data CLI (azdata).
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 04/15/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b41666e4f0474d0e0843e8a1b78d83fcfab474be
ms.sourcegitcommit: a177a1e17200400a70f1d61b737481c83249e9a3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2021
ms.locfileid: "107584009"
---
# <a name="administration-resources-for-big-data-clusters-bdc"></a>Risorse di amministrazione per i cluster Big Data (BDC)

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questo articolo descrive come amministrare un oggetto [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] dall'esterno di .

## <a name="know-your-architecture"></a>Conoscere l'architettura

A partire da SQL Server 2019 (15.x), è possibile distribuire cluster scalabili di contenitori SQL Server, Spark e HDFS in esecuzione in [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] Kubernetes. Per una panoramica di [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] , vedere Informazioni [SQL Server cluster Big Data](big-data-cluster-overview.md).

I [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] forniscono autorizzazione e autenticazione coerenti. Per una panoramica della sicurezza dei cluster Big Data, vedere [Concetti relativi alla sicurezza per [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] ](concept-security.md).

## <a name="manage-and-operate-with-tools"></a>Gestire e usare gli strumenti

Gli articoli seguenti descrivono come gestire e usare il cluster Big Data nei modi seguenti: 

- [Connettersi a un cluster Big Data di SQL Server con Azure Data Studio](connect-to-big-data-cluster.md)
- [Gestire cluster Big Data con il dashboard del controller di SQL Server](manage-with-controller-dashboard.md)
- [Gestire i cluster Big Data di SQL Server con notebook di Azure Data Studio](notebooks-manage-bdc.md)
- [Gestire i cluster Big Data (BDC) con notebook](cluster-manage-notebooks.md)
- [Eseguire un notebook di esempio con Spark](notebooks-tutorial-spark.md)

## <a name="monitor-with-tools"></a>Eseguire il monitoraggio con strumenti

Gli articoli seguenti descrivono come monitorare il cluster Big Data nei modi seguenti: 

- [Monitorare i cluster BDC con Azure Data Studio](cluster-monitor-ads.md)
- [Monitorare i cluster BDC con l'utilità Azdata](cluster-monitor-cmdlet.md)
- [Monitorare i cluster BDC con Grafana Dashboard](cluster-monitor-grafana.md)
- [Monitorare i cluster con i notebook Jupyter e Azure Data Studio](cluster-monitor-notebooks.md)

## <a name="monitor-and-inspect-logs-with-notebooks"></a>Monitorare ed esaminare i log con i notebook

Gli articoli seguenti elencano molti dei notebook Jupyter disponibili in Azure Data Studio.

- [Monitoraggio del cluster con i notebook](cluster-monitor-notebooks.md)
- [Raccolta e analisi dei log nel cluster con i notebook](cluster-logging-notebooks.md)

## <a name="big-data-clusters-troubleshooting-resources"></a>Risorse per la risoluzione dei problemi dei cluster Big Data

Gli articoli seguenti descrivono come risolvere i problemi del cluster Big Data:

- [Risolvere i problemi dei cluster BDC con l'utilità kubectl](cluster-troubleshooting-commands.md) 
- [Risolvere i problemi del notebook pyspark](troubleshoot-pyspark-notebook.md)
- [Risolvere i problemi dei cluster BDC con notebook Juypter e Azure Data Studio (ADS)](cluster-troubleshooter-notebooks.md)
- [Ripristinare le autorizzazioni per HDFS](troubleshoot-hdfs-restore-admin.md)

Gli articoli seguenti descrivono come risolvere i problemi del cluster Big Data distribuito in modalità Active Directory:
- [Risolvere i problemi dei cluster BDC in modalità Active Directory](troubleshoot-active-directory.md) 
- [Risolvere i problemi di AD - Errore di accesso in modalità AD](troubleshoot-ad-login-failed-untrusted-domain.md)
- [Risolvere i problemi di AD - Distribuzione in modalità Active Directory arrestata](troubleshoot-ad-reverse-lookup-zone.md)


## <a name="where-to-find-big-data-clusters-2019-administration-notebooks"></a>Dove trovare i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] notebook di amministrazione 

[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] offre un'esperienza di amministrazione completa con i notebook di Jupyter. I notebook forniti riguardano le operazioni del cluster, la gestione, il monitoraggio, la registrazione e la risoluzione dei problemi. 


### <a name="access-to-local-notebooks"></a>Accesso ai notebook locali 

Dopo aver gestito la connessione a un cluster BDC con Azure Data Studio (ADS), è possibile passare alla scheda ' ' e trovare il collegamento diretto a tutti i notebook locali, come illustrato di [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] seguito: 

![Guide locali di BDC](media/view-cluster-status/bdc-local-guides.png)

È anche possibile accedere ai notebook direttamente da Azure Data Studio (ADS). Usare la scelta rapida da tastiera 'CTRL+MAIUSC+P' o 'Visualizza' e selezionare 'Riquadro comandi' per trovare l'opzione 'Jupyter Books: Get localized SQL Server 2019 guide'. 


### <a name="add-remote-notebooks"></a>Aggiungere notebook remoti

È possibile ottenere la versione preferita dei notebook, poiché i notebook sono di origine remota. Per aggiungere un notebook remoto da Azure Data Studio (ADS), è possibile usare i tasti di scelta rapida 'CTRL+MAIUSC+P' o da 'Visualizza' e selezionare 'Riquadro comandi', come illustrato di seguito:

![Riquadro di ADS](media/view-cluster-status/bdc-ads-palette.png)

È disponibile l'opzione "Jupyter Books: Add Remote Book" (Jupyter Books: Aggiungi libro remoto) e verrà visualizzato un pannello che consente di selezionare i notebook per la versione scelta.

![Guide remote del BDC](media/view-cluster-status/bdc-remote-guides.png)

Dopo aver fatto clic su "Aggiungi", sarà possibile accedere a tutti i notebook della versione scelta nella scheda "Notebook" di Azure Data Studio, come illustrato di seguito: 

![Guide di ADS BDC](media/view-cluster-status/bdc-ads-guides.png)


### <a name="how-to-use-these-notebooks"></a>Come usare questi notebook 

È possibile trovare la guida sull'uso di questi notebook dagli articoli seguenti:

- [Monitorare i cluster con i notebook Jupyter e Azure Data Studio](cluster-monitor-notebooks.md)
- [Raccolta e analisi dei log nel cluster con i notebook](cluster-logging-notebooks.md)
- [Risolvere i problemi del notebook pyspark](troubleshoot-pyspark-notebook.md)
- [Risolvere i problemi dei cluster BDC con notebook Juypter e Azure Data Studio (ADS)](cluster-troubleshooter-notebooks.md)

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sui cluster Big Data, vedere [Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]?](big-data-cluster-overview.md)