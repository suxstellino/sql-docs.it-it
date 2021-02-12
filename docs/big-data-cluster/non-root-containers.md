---
title: Contenitori non ROOT di cluster Big Data
titleSuffix: SQL Server big data clusters
description: Questo articolo descrive come distribuire contenitori non ROOT in cluster Big Data di SQL Server
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 52a24c1dbe2a239450b440c07de1a7852f9c627a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046041"
---
# <a name="non-root-big-data-clusters-containers"></a>Contenitori non ROOT di cluster Big Data

In SQL Server 2019 CU5 è stato introdotto il supporto per i contenitori non ROOT. L'implementazione della piattaforma è più sicura poiché garantisce che tutte le applicazioni contenitore eseguite nel cluster BDC vengano avviate per impostazione predefinita come utenti non ROOT, su tutte le piattaforme supportate. Queste funzionalità sono disponibili per tutte le nuove distribuzioni che usano il tag di immagine corrispondente di SQL Server 2019 CU5. Le distribuzioni di cluster BDC precedenti alla versione CU5 non saranno interessate da questa modifica e le applicazioni in questi cluster continueranno a essere eseguite come utente ROOT. 

## <a name="technical-background"></a>Informazioni tecniche di base

Leggere [questo white paper tecnico](https://aka.ms/sql-bdc-openshift-security) in cui sono riportati i dettagli relativi alla progettazione per la sicurezza nel caso di distribuzioni che usano utenti non ROOT, evidenziando gli aspetti e i motivi per cui i cluster Big Data elevano temporaneamente le autorizzazioni in determinati casi. Il contenuto del white paper è stato sviluppato in collaborazione con gli esperti di sicurezza di SQL Server e Red Hat e si concentra sui contesti di sicurezza e sulle funzionalità di OpenShift, ma i concetti e la progettazione della sicurezza per i cluster BDC sono applicabili a tutte le piattaforme supportate.

> [!NOTE]
> Al momento della versione CU5, il passaggio di installazione delle applicazioni distribuite con le interfacce [app deploy](concept-application-deployment.md) verrà comunque eseguito come utente *ROOT*. Questo è necessario perché durante questo passaggio vengono installati pacchetti aggiuntivi che saranno usati dall'applicazione. Altro codice utente distribuito come parte dell'applicazione verrà eseguito come utente con privilegi limitati. 

> [!NOTE]
> È consigliabile eseguire il cluster con l'impostazione non ROOT predefinita. Se si vuole ripristinare il comportamento precedente alla versione CU5 e all'interno del cluster BDC sono presenti contenitori eseguiti come utente `root`, è possibile usare il nuovo commutatore di funzionalità `allowRunAsRoot` e disattivare il comportamento predefinito. È possibile specificare questa impostazione solo in fase di distribuzione. A tale scopo, specificare l'impostazione nella sezione `security` del file di configurazione della distribuzione `control.json`:

```json
 "security": {
  …
    "allowRunAsRoot": true,
  …
}
```

> [!IMPORTANT]
> La modifica di questa impostazione negli ambienti OpenShift non è supportata.

In seguito all'esecuzione di servizi nel cluster BDC come utenti non ROOT, le credenziali usate per la connessione ai servizi tramite l'endpoint del gateway non usano il nome utente `root`. Se l'applicazione usata per la connessione all'endpoint gateway usa credenziali errate, verrà visualizzato un errore di autenticazione. Il nuovo nome utente per l'endpoint del gateway si basa sul valore passato tramite la variabile di ambiente `AZDATA_USERNAME`. Si tratta dello stesso nome utente usato per il controller e gli endpoint SQL Server. Questa modifica ha impatto solo sulle nuove distribuzioni, mentre i cluster Big Data esistenti distribuiti con qualunque versione precedente continuano a usare `root`. Non vi è alcun impatto per le credenziali quando il cluster viene distribuito in modo da usare l'autenticazione di Active Directory. 

## <a name="use-the-latest-azure-data-studio"></a>Usare la versione più recente di Azure Data Studio

Azure Data Studio gestisce la modifica delle credenziali in modo trasparente per la connessione effettuata tramite il gateway, in modo da consentire l'esperienza di esplorazione di Hadoop Distributed File System in Esplora oggetti o l'invio di processi Spark mediante notebook. Installare la [build Insider più recente per Azure Data Studio](../azure-data-studio/download-azure-data-studio.md#download-insiders-build-of-azure-data-studio). La build include le modifiche necessarie per questo caso d'uso.

Per altri scenari in cui è necessario fornire credenziali per l'accesso al servizio tramite il gateway, ad esempio l'accesso con [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] o l'accesso a dashboard Web per Spark, assicurarsi che vengano usate le credenziali corrette. Se la destinazione è un cluster esistente distribuito prima della versione CU5, si continuerà a usare il nome utente `root` per la connessione al gateway, anche dopo l'aggiornamento del cluster a CU5. Se si distribuisce un nuovo cluster tramite la build CU5, accedere specificando il nome utente corrispondente alla variabile di ambiente `AZDATA_USERNAME`.

## <a name="configuration-file-switches"></a>Opzioni nel file di configurazione

A partire dalla versione CU5, sono state aggiunte due opzioni di funzionalità per controllare la raccolta delle metriche di pod e nodi. Se si usano soluzioni diverse per il monitoraggio dell'infrastruttura Kubernetes, è possibile disattivare la raccolta predefinita di metriche per pod e nodi host impostando `allowNodeMetricsCollection` e `allowPodMetricsCollection`* su `false` nel file di configurazione della distribuzione `control.json`. 

Ad esempio: 

```json
"security": {
  ...
  "allowPodMetricsCollection": true,
  "allowNodeMetricsCollection": true,
  ...
}
```

> [!NOTE]
> Per gli ambienti OpenShift, queste opzioni sono impostate su false per impostazione predefinita nei profili di distribuzione incorporati. La raccolta delle metriche di pod e nodi richiede funzionalità con privilegi e il contesto di sicurezza consigliato per OpenShift è basato sui vincoli *limitati*.

Oltre ai contenitori senza privilegi, a partire dalla versione CU5, per tutte le nuove distribuzioni per cluster BDC, i contenitori vengono eseguiti per impostazione predefinita come utente non ROOT su tutte le piattaforme supportate. Queste funzionalità sono disponibili per tutte le nuove distribuzioni che usano il tag di immagine corrispondente di SQL Server 2019 CU5. Le distribuzioni di cluster BDC precedenti alla versione CU5 non saranno interessate e le applicazioni in questi cluster continueranno a essere eseguite come utente ROOT.

## <a name="next-steps"></a>Passaggi successivi
[Come distribuire[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in Kubernetes](deployment-guidance.md)

[Distribuire [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in OpenShift](deploy-openshift.md)

[White paper sulla sicurezza](https://aka.ms/sql-bdc-openshift-security)
