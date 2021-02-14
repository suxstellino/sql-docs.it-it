---
title: Risorse di amministrazione per i cluster Big Data (BDC)
titleSuffix: SQL Server
description: Questo articolo illustra come visualizzare lo stato di un cluster Big Data usando Azure Data Studio, i notebook e i comandi di Azure Data CLI (azdata).
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 10/20/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 194f74fa175ad3f1c08ceab542b6db7812987157
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045643"
---
# <a name="administration-resources-for-big-data-clusters-bdc"></a>Risorse di amministrazione per i cluster Big Data (BDC) 

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questo articolo illustra come amministrare un cluster Big Data SQL Server dall'esterno verso l'interno.

## <a name="know-your-architecture"></a>Conoscere l'architettura

A partire da SQL Server 2019 (15.x) i cluster Big Data di SQL Server consentono di distribuire cluster scalabili di contenitori SQL Server, Spark e HDFS in esecuzione in Kubernetes. Gli articoli seguenti contribuiscono a una panoramica dei cluster Big Data:
- [Cluster Big Data di SQL Server](big-data-cluster-overview.md)

I cluster Big Data di SQL Server offrono funzionalità coerenti di autorizzazione e autenticazione. Gli articoli seguenti illustrano una panoramica della sicurezza dei cluster Big Data:
- [Concetti relativi alla sicurezza per i cluster Big Data di SQL Server](concept-security.md)

## <a name="manage-and-operate-with-tools"></a>Gestire e usare gli strumenti

Gli articoli seguenti descrivono come gestire e usare i cluster Big Data nei modi seguenti: 

- [Connettersi a un cluster Big Data di SQL Server con Azure Data Studio](connect-to-big-data-cluster.md)
- [Gestire cluster Big Data con il dashboard del controller di SQL Server](manage-with-controller-dashboard.md)
- [Gestire i cluster Big Data di SQL Server con notebook di Azure Data Studio](notebooks-manage-bdc.md)
- [Gestire i cluster Big Data (BDC) con notebook](cluster-manage-notebooks.md)
- [Eseguire un notebook di esempio con Spark](notebooks-tutorial-spark.md)

## <a name="monitor-with-tools"></a>Eseguire il monitoraggio con strumenti

Gli articoli seguenti descrivono come monitorare i cluster Big Data nei modi seguenti: 

- [Monitorare i cluster BDC con Azure Data Studio](cluster-monitor-ads.md)
- [Monitorare i cluster BDC con l'utilità Azdata](cluster-monitor-cmdlet.md)
- [Monitorare i cluster BDC con Grafana Dashboard](cluster-monitor-grafana.md)
- [Monitorare i cluster con i notebook Jupyter e Azure Data Studio](cluster-monitor-notebooks.md)

## <a name="monitor-and-inspect-logs-with-notebooks"></a>Monitorare ed esaminare i log con i notebook

Gli articoli seguenti elencano molti dei notebook Jupyter disponibili in Azure Data Studio.

- [Monitoraggio del cluster con i notebook](cluster-monitor-notebooks.md)
- [Raccolta e analisi dei log nel cluster con i notebook](cluster-logging-notebooks.md)

## <a name="big-data-clusters-troubleshooting-resources"></a>Risorse per la risoluzione dei problemi dei cluster Big Data

Gli articoli seguenti descrivono come risolvere i problemi relativi a un cluster Big Data:

- [Risolvere i problemi dei cluster BDC con l'utilità kubectl](cluster-troubleshooting-commands.md) 
- [Risolvere i problemi del notebook pyspark](troubleshoot-pyspark-notebook.md)
- [Risolvere i problemi dei cluster BDC con notebook Juypter e Azure Data Studio (ADS)](cluster-troubleshooter-notebooks.md)
- [Ripristinare le autorizzazioni per HDFS](troubleshoot-hdfs-restore-admin.md)

Gli articoli seguenti descrivono come risolvere i problemi dei cluster Big Data distribuiti in modalità Active Directory:
- [Risolvere i problemi dei cluster BDC in modalità Active Directory](troubleshoot-active-directory.md) 
- [Risolvere i problemi di AD - Errore di accesso in modalità AD](troubleshoot-ad-login-failed-untrusted-domain.md)
- [Risolvere i problemi di AD - Distribuzione in modalità Active Directory arrestata](troubleshoot-ad-reverse-lookup-zone.md)

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sui cluster Big Data, vedere [Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]?](big-data-cluster-overview.md)