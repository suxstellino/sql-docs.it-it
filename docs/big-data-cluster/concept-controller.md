---
title: Che cos'è il controller?
titleSuffix: SQL Server big data clusters
description: Questo articolo descrive il controller di un cluster Big Data di SQL Server.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 9e2a7413578a8625231ffa0dad1750e98445f339
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039441"
---
# <a name="what-is-the-controller-on-a-sql-server-big-data-cluster"></a>Che cos'è il controller in un cluster Big Data di SQL Server?

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Il controller ospita la logica di base per la distribuzione e la gestione di un cluster Big Data di SQL Server. Si occupa di tutte le interazioni con Kubernetes, le istanze di SQL Server che fanno parte del cluster e altri componenti come HDFS e Spark.

Il servizio controller fornisce le funzionalità di base seguenti:

- Gestione del ciclo di vita del cluster: bootstrap del cluster ed eliminazione e aggiornamento delle configurazioni
- Gestione delle istanze master di SQL Server
- Gestione dei pool di calcolo, dati e archiviazione
- Esposizione degli strumenti di monitoraggio per osservare lo stato del cluster
- Esposizione degli strumenti di risoluzione dei problemi per rilevare e correggere problemi imprevisti
- Gestione della sicurezza del cluster:
  - Protezione degli endpoint del cluster
  - Gestione di utenti e ruoli
  - Configurazione delle credenziali per la comunicazione all'interno del cluster

## <a name="deploying-the-controller-service"></a>Distribuzione del servizio controller

Il controller viene distribuito e ospitato nello stesso spazio dei nomi Kubernetes in cui il cliente vuole compilare un cluster Big Data. Questo servizio viene installato da un amministratore di Kubernetes durante il bootstrap del cluster, usando l'utilità della riga di comando **azdata**. Per altre informazioni, vedere [Introduzione ai [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deploy-get-started.md).

Il flusso di lavoro di implementazione disporrà su Kubernetes un cluster Big Data di SQL Server completamente funzionale che include tutti i componenti descritti nell'articolo [Panoramica](big-data-cluster-overview.md). Il flusso di lavoro di bootstrap crea innanzitutto il servizio controller che, una volta distribuito, coordina l'installazione e la configurazione della restante parte dei servizi dei pool master, di calcolo, di dati e di archiviazione.

## <a name="managing-the-cluster-through-the-controller-service"></a>Gestione del cluster tramite il servizio controller

È possibile gestire il cluster tramite il servizio controller usando uno dei comandi **azdata**. Se si distribuiscono oggetti Kubernetes aggiuntivi, come i pod, nello stesso spazio dei nomi, questi non vengono gestiti o monitorati dal servizio controller. È anche possibile usare i comandi **kubectl** per gestire il cluster a livello di Kubernetes. Per altre informazioni, vedere [Monitoraggio e risoluzione dei problemi dei [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](cluster-troubleshooting-commands.md).

Il controller e gli oggetti Kubernetes (set con stato, pod, segreti e così via) creati per un cluster Big Data si trovano in uno spazio dei nomi Kubernetes dedicato. L'amministratore del cluster concederà al servizio controller l'autorizzazione Kubernetes per gestire tutte le risorse all'interno di questo spazio dei nomi.  Il criterio di controllo degli accessi in base al ruolo per questo scenario viene configurato automaticamente come parte della distribuzione iniziale del cluster tramite **azdata**.

### <a name="azdata"></a>azdata

**azdata** è un'utilità della riga di comando scritta in Python che permette agli amministratori del cluster di eseguire il bootstrap e la gestione di cluster Big Data tramite API REST esposte dal servizio controller.

## <a name="controller-service-security"></a>Sicurezza del servizio controller

Tutte le comunicazioni con il servizio controller avvengono tramite un'API REST su HTTPS. Un certificato autofirmato verrà generato automaticamente in fase di bootstrap. 

L'autenticazione all'endpoint del servizio controller usa un'identità di Active Directory o si basa su nome utente e password. Queste credenziali vengono sottoposte a provisioning in fase di bootstrap del cluster usando l'input per le variabili di ambiente `AZDATA_USERNAME` e `AZDATA_PASSWORD`.

> [!NOTE]
> È necessario fornire una password conforme ai [requisiti di complessità delle password di SQL Server](../relational-databases/security/password-policy.md).

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sui [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vedere le risorse seguenti:

- [Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)
- [Workshop: Architettura Microsoft dei [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](https://github.com/microsoft/sqlworkshops-bdc)