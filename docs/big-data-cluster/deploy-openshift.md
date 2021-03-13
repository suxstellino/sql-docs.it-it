---
title: Distribuire in OpenShift
titleSuffix: SQL Server Big Data Cluster
description: Informazioni su come aggiornare cluster Big Data di SQL Server in OpenShift.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d78e229bcbf2a088d42431abdf02bec3f9e51eab
ms.sourcegitcommit: 62c7b972db0ac28e3ae457ce44a4566ebd3bbdee
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/12/2021
ms.locfileid: "103231499"
---
# <a name="deploy-big-data-clusters-2019-on-openshift-on-premises-and-azure-red-hat-openshift"></a>Distribuire [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in OpenShift in locale e in Azure Red Hat OpenShift

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questo articolo illustra come distribuire un cluster Big Data (BDC) di SQL Server in ambienti OpenShift, in locale o in Azure Red Hat OpenShift (ARO).

> [!TIP]
> Per un bootstrap rapido di un ambiente di esempio usando ARO e quindi il cluster Big Data distribuito in questa piattaforma, è possibile usare lo script Python disponibile [qui](quickstart-big-data-cluster-deploy-aro.md).


SQL Server 2019 CU5 introduce il supporto per i cluster Big Data di SQL Server in OpenShift. È possibile distribuire cluster Big Data in OpenShift in locale o in Azure Red Hat OpenShift (ARO). Per la distribuzione è necessaria la versione minima 4.3 del cluster OpenShift. Anche se il flusso di lavoro di distribuzione è simile alla distribuzione in altre piattaforme basate su Kubernetes ([kubeadm](deploy-with-kubeadm.md) e [AKS](deploy-on-aks.md)), esistono alcune differenze. La differenza riguarda principalmente l'esecuzione delle applicazioni come utente non ROOT e il contesto di protezione usato per lo spazio dei nomi in cui viene distribuito il cluster Big Data.

Per la distribuzione del cluster OpenShift in locale, vedere la [documentazione di Red Hat OpenShift](https://docs.openshift.com/container-platform/4.3/release_notes/ocp-4-3-release-notes.html#ocp-4-3-installation-and-upgrade). Per le distribuzioni di ARO, vedere [Azure Red Hat OpenShift](/azure/openshift/intro-openshift).

Questo articolo illustra i passaggi di distribuzione specifici della piattaforma OpenShift, indica le opzioni disponibili per accedere all'ambiente di destinazione e lo spazio dei nomi usato per distribuire il cluster Big Data.

## <a name="pre-requisites"></a>Prerequisiti

> [!IMPORTANT]
> Le operazioni seguenti devono essere eseguite da un amministratore del cluster OpenShift (ruolo di amministratore del cluster) con autorizzazioni sufficienti per creare questi oggetti a livello di cluster. Per altre informazioni sui ruoli del cluster in OpenShift, vedere [Uso del controllo degli accessi in base al ruolo per definire e applicare le autorizzazioni](https://docs.openshift.com/container-platform/4.4/authentication/using-rbac.html).

1. Assicurarsi che l'impostazione `pidsLimit` in OpenShift sia aggiornata per gestire i carichi di lavoro di SQL Server. Il valore predefinito in OpenShift è troppo basso per i carichi di lavoro di produzione. Iniziare con almeno `4096`, ma il valore ottimale dipende dall'impostazione `max worker threads` in SQL Server e dal numero di processori CPU nel nodo host OpenShift. 
    - Per informazioni su come aggiornare `pidsLimit` per il cluster OpenShift usare [queste istruzioni]( https://github.com/openshift/machine-config-operator/blob/master/docs/ContainerRuntimeConfigDesign.md). Si noti che le versioni di OpenShift precedenti alla `4.3.5` presentano un problema che fa sì che il valore aggiornato non abbia effetto. Assicurarsi di aggiornare OpenShift alla versione più recente. 
    - Per semplificare il calcolo del valore ottimale a seconda dell'ambiente e dei carichi di lavoro di SQL Server pianificati, è possibile usare la stima e gli esempi seguenti:

    |Numero di processori|Valore predefinito max worker threads|Ruoli di lavoro predefiniti per processore|Valore minimo pidsLimit|
    |--------------------|--------------------------|-----------------------------|-----------------------|
    |          64        |           512            |             16              | 512 + (64 *16) = 1536 |
    |         128        |           512            |             32              | 512 + (128*32) = 4608 |

    > [!NOTE]
    > Anche altri processi, come backup, CLR, Fulltext, SQLAgent, comportano un overhead, quindi aggiungono un buffer al valore stimato.

1. Scaricare il vincolo del contesto di protezione personalizzato [`bdc-scc.yaml`](#bdc-sccyaml-file):

    ```console
    curl https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/deployment/openshift/bdc-scc.yaml -o bdc-scc.yaml
    ```

1. Applicare il vincolo del contesto di protezione al cluster.

    ```console
    oc apply -f bdc-scc.yaml
    ```

    > [!NOTE]
    > Il contesto di protezione personalizzato per il cluster Big Data è basato sul contesto di protezione personalizzato `nonroot` predefinito di OpenShift, con autorizzazioni aggiuntive. Per altre informazioni sui vincoli del contesto di protezione in OpenShift, vedere [Gestione dei vincoli del contesto di protezione](https://docs.openshift.com/container-platform/4.3/authentication/managing-security-context-constraints.html). Per informazioni dettagliate sulle autorizzazioni aggiuntive richieste per i cluster Big Data oltre al vincolo del contesto di protezione `nonroot`, scaricare il white paper [qui](https://aka.ms/sql-bdc-openshift-security).

3. Creare uno spazio dei nomi/progetto:

   ```console
   oc new-project <namespaceName>
   ```

4. Assegnare il vincolo del contesto di protezione personalizzato agli account del servizio per gli utenti all'interno dello spazio dei nomi in cui viene distribuito il cluster Big Data:

   ```console
   oc create rolebinding bdc-rbac --clusterrole=system:scc:bdc-scc --group=system:serviceaccounts:<namespace>
   ```

5. Assegnare le autorizzazioni appropriate all'utente che distribuisce il cluster Big Data. riportate di seguito. 

   - Se l'utente che distribuisce il cluster Big Data ha un ruolo di amministratore del cluster, procedere con la [distribuzione del cluster Big Data](#deploy-big-data-cluster).

   - Se l'utente che distribuisce il cluster Big Data è un amministratore dello spazio dei nomi, assegnare all'utente il ruolo locale di amministratore del cluster per lo spazio dei nomi creato. Questa è l'opzione preferita per l'utente che distribuisce e gestisce il cluster Big Data per avere autorizzazioni di amministratore a livello dello spazio dei nomi.

   ```console
   oc adm policy add-role-to-user cluster-admin <deployingUser> -n <namespaceName>
   ```

   L'utente che distribuisce il cluster Big Data deve quindi accedere alla console OpenShift:

   ```console
   oc login -u <deployingUser> -p <password>
   ```

## <a name="deploy-big-data-cluster"></a>Distribuire il cluster Big Data

1. Installare la versione più recente di [azdata](../azdata/install/deploy-install-azdata.md).

1. Clonare uno dei file di configurazione predefiniti per OpenShift, a seconda dell'ambiente di destinazione (OpenShift in locale o ARO) e dello scenario di distribuzione. Vedere la sezione *Impostazioni specifiche di OpenShift nei file di configurazione della distribuzione* di seguito per le impostazioni specifiche di OpenShift nei file di configurazione predefiniti. Per altre informazioni sui file di configurazione disponibili, vedere le [linee guida per la distribuzione](deployment-guidance.md).

   Elencare tutti i file di configurazione predefiniti disponibili.

   ```console
   azdata bdc config list
   ```

   Per clonare uno dei file di configurazione predefiniti, eseguire il comando seguente (facoltativamente, è possibile sostituire il profilo in base alla piattaforma/scenario di destinazione):

   ```console
   azdata bdc config init --source openshift-dev-test --target custom-openshift
   ```

   Per una distribuzione in ARO, iniziare con uno dei profili `aro-` che include valori predefiniti per `serviceType` e`storageClass` appropriati per questo ambiente. Ad esempio:

   ```console
   azdata bdc config init --source aro-dev-test --target custom-openshift
   ```

1. Personalizzare i file di configurazione control.json e bdc.json. Di seguito sono riportate alcune risorse aggiuntive che illustrano le personalizzazioni supportate per diversi casi d'uso:

   - [Storage](concept-data-persistence.md)
   - [Impostazioni relative ad AD](active-directory-deploy.md)
   - [Altre personalizzazioni](deployment-custom-configuration.md)

   > [!NOTE]
   > L'integrazione con Azure Active Directory per il cluster Big Data non è supportata, di conseguenza non è possibile usare questo metodo di autenticazione per la distribuzione in ARO.

1. Impostare le [variabili di ambiente](deployment-guidance.md#env)

1. Distribuire il cluster Big Data

   ```console
   azdata bdc create --config custom-openshift --accept-eula yes
   ```

1. Al termine della distribuzione, è possibile accedere ed elencare gli endpoint del cluster esterni:

   ```console
      azdata login -n mssql-cluster
      azdata bdc endpoint list
   ```

## <a name="openshift-specific-settings-in-the-deployment-configuration-files"></a>Impostazioni specifiche di OpenShift nei file di configurazione della distribuzione

SQL Server 2019 CU5 ha introdotto due opzioni di funzionalità per controllare la raccolta di metriche di pod e nodi. Questi parametri sono impostati su `false` per impostazione predefinita nei profili predefiniti per OpenShift perché i contenitori di monitoraggio richiedono un [contesto di protezione con privilegi](https://www.openshift.com/blog/managing-sccs-in-openshift), cosa che consentirà di ridurre alcuni dei vincoli di protezione per lo spazio dei nomi in cui viene distribuito il cluster Big Data.

```json
    "security": {
      "allowNodeMetricsCollection": false,
      "allowPodMetricsCollection": false
}
```

Il nome della classe di archiviazione predefinita in ARO è managed-premium (a differenza del servizio Azure Kubernetes in cui la classe di archiviazione predefinita è denominata default). Si trova nel file `control.json` corrispondente a `aro-dev-test` e `aro-dev-test-ha`:

```json
    },
    "storage": {
      "data": {
        "className": "managed-premium",
        "accessMode": "ReadWriteOnce",
        "size": "15Gi"
      },
      "logs": {
        "className": "managed-premium",
        "accessMode": "ReadWriteOnce",
        "size": "10Gi"
      }
```

## <a name="bdc-sccyaml-file"></a>File `bdc-scc.yaml`

Il file del vincolo del contesto di protezione per la distribuzione è:

:::code language="yaml" source="../../sql-server-samples/samples/features/sql-big-data-cluster/deployment/openshift/bdc-scc.yaml":::

## <a name="next-steps"></a>Passaggi successivi

[Esercitazione: Caricare dati di esempio in un cluster Big Data di SQL Server](tutorial-load-sample-data.md)
