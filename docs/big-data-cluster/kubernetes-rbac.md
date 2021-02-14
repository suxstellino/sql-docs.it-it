---
title: Controllo degli accessi in base al ruolo di Kubernetes
titleSuffix: SQL Server big data clusters
description: Questo articolo descrive come i cluster Big Data di SQL Server usano il controllo degli accessi in base al ruolo con Kubernetes.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 08/04/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a379b2bd1bb0e1992d70e1c86ae93163f6c02201
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046441"
---
# <a name="kubernetes-rbac-model--impact-on-users-and-service-accounts-managing-bdc"></a>Modello di controllo degli accessi in base al ruolo di Kubernetes e impatto sugli utenti e sugli account del servizio che gestiscono i cluster Big Data

Questo articolo descrive i requisiti di autorizzazione per gli utenti che gestiscono i cluster Big Data e la semantica per l'account del servizio predefinito e l'accesso a Kubernetes dall'interno del cluster Big Data.

> [!NOTE]
> Per altre risorse sul modello di controllo degli accessi in base al ruolo di Kubernetes, vedere [Using RBAC Authorization - Kubernetes](https://kubernetes.io/docs/reference/access-authn-authz/rbac/) (Uso dell'autorizzazione di controllo degli accessi in base al ruolo - Kubernetes) e [Using RBAC to define and apply permissions - OpenShift](https://docs.openshift.com/container-platform/4.4/authentication/using-rbac.html) (Uso del controllo degli accessi in base al ruolo per definire e applicare le autorizzazioni - OpenShift).

## <a name="role-required-for-deployment"></a>Ruolo necessario per la distribuzione

Il cluster BDC usa gli account del servizio (ad esempio `sa-mssql-controller` o `master`) per orchestrare il provisioning dei pod, dei servizi, della disponibilità elevata, del monitoraggio del cluster e così via. Quando viene avviata la distribuzione del cluster BDC (ad esempio, `azdata bdc create`), [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] esegue le operazioni seguenti:

1. Verifica se lo spazio dei nomi specificato esiste.
2. Se non esiste, ne crea uno e applica l'etichetta `MSSQL_CLUSTER`.
3. Crea l'account del servizio `sa-mssql-controller`.
4. Crea un ruolo `<namespaced>-admin` con autorizzazioni complete per lo spazio dei nomi o il progetto ma non con autorizzazioni a livello di cluster.
5. Crea un'assegnazione di tale account del servizio al ruolo.

Una volta completati questi passaggi, viene effettuato il provisioning dei pod del piano di controllo e l'account del servizio distribuisce la parte restante del cluster Big Data.  

Di conseguenza, l'utente che esegue la distribuzione deve avere le autorizzazioni per:

- Elencare gli spazi dei nomi nel cluster (1).
- Applicare una patch allo spazio dei nomi nuovo o esistente con l'etichetta (2).
- Creare l'account del servizio `sa-mssql-controller`, il ruolo `<namespaced>-admin` e l'associazione di ruolo (3-5).

Il ruolo `admin` predefinito non dispone di queste autorizzazioni, pertanto l'utente che distribuisce il cluster Big Data deve disporre almeno delle autorizzazioni di amministratore a livello di spazio dei nomi.

## <a name="cluster-role-required-for-pods-and-nodes-metrics-collection"></a>Ruolo del cluster necessario per la raccolta di metriche di pod e nodi

A partire da SQL Server 2019 CU5, Telegraf richiede un account del servizio con autorizzazioni di ruolo a livello di cluster per raccogliere le metriche relative a pod e nodi. Durante la distribuzione (o l'aggiornamento delle distribuzioni esistenti), viene effettuato un tentativo di creare l'account del servizio e il ruolo del cluster necessari. Se tuttavia l'utente che distribuisce il cluster o esegue l'aggiornamento non dispone di autorizzazioni sufficienti, la distribuzione (o l'aggiornamento) verrà comunque completata con un avviso. In questo caso, le metriche di pod e nodi non verranno raccolte. L'utente che distribuisce il cluster deve rivolgersi a un amministratore del cluster per creare il ruolo e l'account del servizio (prima o dopo la distribuzione o l'aggiornamento). Una volta creati, vengono usati dal cluster BDC. 

Di seguito sono descritti i passaggi per creare gli artefatti necessari:

1. Creare un file *metrics-role.yaml* con il contenuto seguente. Assicurarsi di sostituire i segnaposto *<clusterName>* con il nome del cluster Big Data.

   ```yaml
   apiVersion: rbac.authorization.k8s.io/v1
   kind: ClusterRole
   metadata:
     name: <clusterName>:cr-mssql-metricsdc-reader
   rules:
   - apiGroups:
     - '*'
     resources:
     - pods
     - nodes/stats
     verbs:
     - get
   ---
   apiVersion: rbac.authorization.k8s.io/v1
   kind: ClusterRoleBinding
   metadata:
     name: <clusterName>:crb-mssql-metricsdc-reader
   roleRef:
     apiGroup: rbac.authorization.k8s.io
     kind: ClusterRole
     name: <clusterName>:cr-mssql-metricsdc-reader
   subjects:
   - kind: ServiceAccount
     name: sa-mssql-metricsdc-reader
     namespace: <clusterName>
   ```

2. Creare il ruolo del cluster e l'associazione del ruolo del cluster:

   ```bash
   kubectl create -f metrics-role.yaml
   ```

L'account del servizio, il ruolo del cluster e l'associazione del ruolo del cluster possono essere creati prima o dopo la distribuzione del cluster BDC. Kubernetes aggiorna automaticamente l'autorizzazione per l'account del servizio Telegraf. Se questi artefatti vengono creati come una distribuzione di pod, verrà visualizzato un ritardo di pochi minuti nelle metriche di pod e nodi raccolte.

> [!NOTE]
> SQL Server 2019 CU5 ha introdotto due opzioni di funzionalità per controllare la raccolta di metriche di pod e nodi. Per impostazione predefinita, questi parametri sono impostati su true in tutte le destinazioni dell'ambiente, ad eccezione di OpenShift, in cui viene eseguito l'override del valore predefinito. 

È possibile personalizzare queste impostazioni nella sezione relativa alla sicurezza del file di configurazione della distribuzione `control.json`:

```json
  "security": {
    …
    "allowNodeMetricsCollection": false,
    "allowPodMetricsCollection": false
  }
```

Se queste impostazioni sono impostate su `false`, il flusso di lavoro di distribuzione del cluster BDC non tenterà di creare l'account del servizio, il ruolo del cluster e l'associazione per Telegraf.

## <a name="default-service-account-usage-from-within-a-bdc-pod"></a>Uso dell'account del servizio predefinito dall'interno di un pod BDC

Per un modello di sicurezza più rigoroso, SQL Server 2019 CU5 ha disabilitato il montaggio tramite le credenziali predefinite per l'account del servizio Kubernetes predefinito nei pod BDC. Questo vale per le distribuzioni nuove e aggiornate in CU5 o versioni successive.
Il token delle credenziali all'interno dei pod può essere usato per accedere al server API Kubernetes e il livello di autorizzazioni dipende dalle impostazioni dei criteri di autorizzazione Kubernetes. Se sono presenti casi d'uso specifici che richiedono il ripristino del comportamento precedente di CU5, in CU6 verrà introdotta una nuova opzione della funzionalità in modo che sia possibile attivare il montaggio automatico solo in fase di distribuzione. L'operazione può essere eseguita usando il file di distribuzione della configurazione control.json e impostando *automountServiceAccountToken* su *true*. Eseguire questo comando per aggiornare questa impostazione nel file di configurazione personalizzato *control.json* usando [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]: 

``` bash
azdata bdc config replace -c custom-bdc/control.json -j "$.security.automountServiceAccountToken=true"
```
