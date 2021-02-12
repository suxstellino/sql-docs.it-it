---
title: Che cos'è la distribuzione di applicazioni?
titleSuffix: SQL Server Big Data Clusters
description: Informazioni su come la distribuzione delle applicazioni offre le interfacce necessarie per creare, gestire ed eseguire le applicazioni in un cluster Big Data di SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 8c1f6b4895e872289095411cfd8ecdaf1e2eb087
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047651"
---
# <a name="what-is-application-deployment-on-a-sql-server-big-data-cluster"></a>Che cos'è la distribuzione di applicazioni in un cluster Big Data di SQL Server?

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

La distribuzione di applicazioni consente di distribuire applicazioni in un cluster Big Data di SQL Server fornendo interfacce per la creazione, la gestione e l'esecuzione di applicazioni. Le applicazioni distribuite nel cluster Big Data di SQL Server traggono vantaggio dalla potenza di calcolo del cluster e possono accedere ai dati disponibili nel cluster. In questo modo, viene aumentata la scalabilità e le prestazioni delle applicazioni nel caso in cui le applicazioni gestite si trovino nella posizione in cui risiedono i dati. I runtime dell'applicazione supportati in [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] sono R, Python, SSIS e MLeap.

Le sezioni seguenti descrivono l'architettura e le funzionalità della distribuzione di applicazioni.

## <a name="application-deployment-architecture"></a>Architettura della distribuzione di applicazioni

La distribuzione di applicazioni è costituita da un controller e da gestori di runtime delle app. Quando si crea un'applicazione, viene fornito un file di specifica (`spec.yaml`). Questo file `spec.yaml` contiene tutte le informazioni necessarie al controller per distribuire correttamente l'applicazione. Di seguito è riportato un esempio di contenuti per `spec.yaml`:

```yaml
#spec.yaml
name: add-app #name of your python script
version: v1  #version of the app
runtime: Python #the language this app uses (R or Python)
src: ./add.py #full path to the location of the app
entrypoint: add #the function that will be called upon execution
replicas: 1  #number of replicas needed
poolsize: 1  #the pool size that you need your app to scale
inputs:  #input parameters that the app expects and the type
  x: int
  y: int
output: #output parameter the app expects and the type
  result: int
```

Il controller esamina il `runtime` specificato nel file `spec.yaml` e chiama il gestore di runtime corrispondente. Il gestore di runtime crea quindi l'applicazione. Per prima cosa, viene creato un set di repliche Kubernetes contenente uno o più POD, ognuno dei quali contiene l'applicazione da distribuire. Il numero di POD è definito dal set di parametri `replicas` nel file `spec.yaml` per l'applicazione. Ogni pod può includere uno o più pool. Il numero di pool è definito dal set di parametri `poolsize` nel file `spec.yaml`.

Queste impostazioni influiscono sulla quantità di richieste che la distribuzione è in grado di gestire in parallelo. Il numero massimo di richieste contemporanee è uguale alla `replicas` per il numero di `poolsize`. Se si hanno 5 repliche e 2 pool per replica, quindi, la distribuzione potrà gestire 10 richieste in parallelo. Vedere l'immagine seguente per una rappresentazione grafica di `replicas` e `poolsize`:

![Dimensione del pool e repliche](media/big-data-cluster-create-apps/poolsize-vs-replicas.png)

Dopo la creazione del set di repliche e l'avvio dei pod, viene creato un processo cron se nel file `schedule` è stato impostato un oggetto `spec.yaml`. Viene creato infine un servizio Kubernetes che può essere usato per gestire ed eseguire l'applicazione (vedere di seguito).

Quando viene eseguita un'applicazione, il servizio Kubernetes dell'applicazione inoltra le richieste a una replica e restituisce i risultati.

## <a name="security-considerations-for-applications-deployments-on-openshift"></a><a id="app-deploy-security"></a> Considerazioni sulla sicurezza per la distribuzione di applicazioni

SQL Server 2019 CU5 abilita il supporto per la distribuzione di cluster Big Data in Red Hat OpenShift, oltre a un modello di sicurezza aggiornato per cluster BDC, in modo che i contenitori con privilegi non siano più necessari. Oltre che senza privilegi, per impostazione predefinita i contenitori vengono eseguiti come utente non ROOT per tutte le nuove distribuzioni con SQL Server 2019 CU5.

Al momento della versione CU5, il passaggio di installazione delle applicazioni distribuite con le interfacce [app deploy]() verrà comunque eseguito come utente *ROOT*. Questo è necessario perché durante questo passaggio vengono installati pacchetti aggiuntivi che saranno usati dall'applicazione. Altro codice utente distribuito come parte dell'applicazione verrà eseguito come utente con privilegi limitati. 

È inoltre disponibile la funzionalità facoltativa **CAP_AUDIT_WRITE** che può essere usata per consentire la pianificazione di applicazioni SSIS con processi cron. Quando il file di specifiche YAML dell'applicazione definisce una pianificazione, l'applicazione viene attivata tramite un processo cron, il che richiede funzionalità aggiuntive.  In alternativa, l'applicazione può essere attivata su richiesta con *azdata app run* tramite una chiamata al servizio Web, senza che sia necessaria la funzionalità CAP_AUDIT_WRITE. 

> [!NOTE]
> I vincoli del contesto di sicurezza (SCC) personalizzati nell'[articolo relativo alla distribuzione di OpenShift](deploy-openshift.md) non include questa funzionalità perché non è necessaria per una distribuzione predefinita di un cluster Big Data. Per abilitare questa funzionalità, è necessario prima aggiornare il file YAML di SCC personalizzato per includere CAP_AUDIT_WRITE nel modo seguente: 

```yml
...
allowedCapabilities:
- SETUID
- SETGID
- CHOWN
- SYS_PTRACE
- AUDIT_WRITE
...
```

## <a name="how-to-work-with-application-deployment"></a>Come usare la distribuzione di applicazioni

Le due interfacce principali per la distribuzione di applicazioni sono: 
- [Interfaccia della riga di comando [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](app-create.md)
- [Estensione di Visual Studio Code e Azure Data Studio](app-deployment-extension.md)

Un'applicazione può essere eseguita anche mediante un servizio Web RESTful. Per altre informazioni, vedere [Utilizzare applicazioni in cluster Big Data](app-consume.md).

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su come creare ed eseguire applicazioni in [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vedere gli argomenti seguenti:

- [Distribuire applicazioni con azdata](app-create.md)
- [Distribuire applicazioni usando l'estensione per la distribuzione di app](app-deployment-extension.md)
- [Utilizzare applicazioni in cluster Big Data](app-consume.md)

Per altre informazioni su [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vedere la panoramica seguente:

- [Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)