---
title: Salvataggio permanente dei dati in Kubernetes
titleSuffix: SQL Server big data clusters
description: Informazioni su come i volumi permanenti offrono un modello plug-in per l'archiviazione in Kubernetes. Informazioni anche sul funzionamento del salvataggio permanente dei dati in un cluster Big Data di SQL Server 2019.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a2836a6966727c0c2d27b363048f2e1dcd6afa49
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052101"
---
# <a name="data-persistence-with-sql-server-big-data-cluster-in-kubernetes"></a>Salvataggio permanente dei dati con un cluster Big Data di SQL Server in Kubernetes

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

I [volumi permanenti](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) offrono un modello plug-in per l'archiviazione in Kubernetes. In questo modello il modo in cui viene resa disponibile l'archiviazione è astratto rispetto al modo in cui viene usata. Di conseguenza, è possibile usare la propria risorsa di archiviazione a disponibilità elevata e collegarla al cluster Big Data di SQL Server. In questo modo, si ottiene il controllo completo sul tipo di archiviazione, sulla disponibilità e sulle prestazioni necessari. Kubernetes supporta [vari tipi di soluzioni di archiviazione](https://kubernetes.io/docs/concepts/storage/storage-classes/#provisioner), tra cui dischi e file di Azure, NFS (Network File System) e archiviazione locale.

## <a name="configure-persistent-volumes"></a>Configurare volumi permanenti

Un cluster Big Data di SQL Server consuma questi volumi permanenti usando le [classi di archiviazione](https://kubernetes.io/docs/concepts/storage/storage-classes/). È possibile creare classi di archiviazione diverse per diversi tipi di archiviazione e specificarle al momento della distribuzione del cluster Big Data. È possibile configurare la classe di archiviazione e la dimensione dell'attestazione di volume permanente da usare per uno scopo specifico a livello di pool. Un cluster Big Data di SQL Server crea [attestazioni di volume permanente](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistentvolumeclaims) usando il nome della classe di archiviazione specificato per ogni componente che richiede volumi permanenti. Monta quindi il volume o i volumi permanenti corrispondenti nel pod. 

Ecco alcuni aspetti importanti di cui tenere conto quando si pianifica la configurazione dell'archiviazione per il cluster Big Data:

- Per una corretta distribuzione del cluster Big Data, verificare che sia disponibile la quantità necessaria di volumi permanenti. Se si esegue la distribuzione in un cluster del servizio Azure Kubernetes (AKS) e si usa una classe di archiviazione predefinita (`default` o `managed-premium`), la classe supporta il provisioning dinamico per i volumi permanenti. Di conseguenza, non è necessario creare in anticipo i volumi permanenti, ma è necessario assicurarsi che i nodi di lavoro disponibili nel cluster del servizio Azure Kubernetes siano in grado di collegare tanti dischi quanti sono i volumi permanenti richiesti per la distribuzione. A seconda delle [dimensioni delle macchine virtuali](/azure/virtual-machines/linux/sizes) specificate per i nodi di lavoro, ogni nodo può collegare un determinato numero di dischi. Per un cluster con dimensioni predefinite (senza disponibilità elevata), sono necessari almeno 24 dischi. Se si abilita la disponibilità elevata o si aumentano i pool, assicurarsi di avere almeno due volumi permanenti per ogni replica aggiuntiva, indipendentemente dalla risorsa che si intende aumentare.

- Se lo strumento di provisioning dell'archiviazione per la classe di archiviazione specificata nella configurazione non supporta il provisioning dinamico, è necessario creare in anticipo i volumi permanenti. Ad esempio, lo strumento di provisioning `local-storage` non consente il provisioning dinamico. Vedere questo [script di esempio](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/deployment/kubeadm/ubuntu) per informazioni su come eseguire questa operazione in un cluster Kubernetes distribuito con `kubeadm`.

- Quando si distribuisce un cluster Big Data, è possibile configurare la stessa classe di archiviazione perché venga usata da tutti i componenti del cluster. Tuttavia, come procedura consigliata per una distribuzione di produzione, diversi componenti richiederanno configurazioni di archiviazione diverse per gestire i vari carichi di lavoro in termini di dimensioni o velocità effettiva. È possibile sovrascrivere la configurazione dell'archiviazione predefinita specificata nel controller per ognuna delle istanze master di SQL Server, per i set di dati e per i pool di archiviazione. Questo articolo contiene alcuni esempi su come eseguire questa operazione.

- Quando si calcolano i requisiti di dimensionamento del pool di archiviazione, è necessario prendere in considerazione il fattore di replica con cui è configurato HDFS.  Il fattore di replica può essere configurato in fase di distribuzione nel file di configurazione della distribuzione del cluster. Il valore predefinito per i profili dev.test, (ovvero `aks-dev-test` o `kubeadm-dev-test`), è 2 e per i profili consigliati per le distribuzioni di produzione (ovvero `kubeadm-prod`), il valore predefinito è 3. La procedura consigliata prevede la configurazione della distribuzione di produzione del cluster Big Data con un fattore di replica per HDFS di almeno 3. Il valore del fattore di replica influirà sul numero di istanze nel pool di archiviazione. Come minimo, è necessario distribuire un numero di istanze del pool di archiviazione pari almeno al valore del fattore di replica. Inoltre, è necessario ridimensionare la risorsa di archiviazione di conseguenza e prevedere che i dati vengano replicati in HDFS lo stesso numero di volte del valore del fattore di replica. Altre informazioni sulla replica dei dati in HDFS sono disponibili [qui](https://hadoop.apache.org/docs/r3.2.1/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Data_Replication). 

- A partire dalla versione SQL Server 2019 CU1, il cluster Big Data non consente di aggiornare la configurazione dell'archiviazione dopo la distribuzione. Questo vincolo impedisce di eseguire operazioni sul cluster Big Data per modificare l'attestazione di volume permanente per ogni istanza o ridimensionare i pool dopo la distribuzione. È quindi molto importante pianificare il layout di archiviazione prima di distribuire un cluster Big Data. È tuttavia possibile espandere la dimensione del volume permanente usando direttamente le API Kubernetes. In questo caso, i metadati del cluster Big Data non verranno aggiornati e verranno visualizzate incoerenze relative alle dimensioni dei volumi nei metadati del cluster di configurazione.

- Con la distribuzione in Kubernetes come applicazioni in contenitori e l'uso di funzionalità come set con stato e archiviazione permanente, Kubernetes garantisce che i pod vengano riavviati in caso di problemi di integrità e collegati alla stessa risorsa di archiviazione permanente. Tuttavia, in caso di errori del nodo e se un pod deve essere riavviato in un altro nodo, aumenta il rischio di non disponibilità se l'archiviazione è locale per il nodo in errore. Per ridurre questo rischio, è necessario configurare ridondanza aggiuntiva e abilitare [funzionalità di disponibilità elevata](deployment-high-availability.md) oppure usare l'archiviazione con ridondanza remota. Ecco una panoramica delle opzioni di archiviazione per i diversi componenti nei cluster Big Data:

| Risorse | Tipo di archiviazione per i dati | Tipo di archiviazione per i log |  Note |
|---|---|---|--|
| Istanza master di SQL Server | Archiviazione con ridondanza locale (repliche >= 3) o remota (replica = 1) | Archiviazione locale | Un'implementazione basata su set con stato in cui i pod che restano nei nodi assicurano il riavvio e gli errori temporanei non causano la perdita di dati. |
| Pool di calcolo | Archiviazione locale | Archiviazione locale | Dati utente non archiviati. |
| Pool di dati | Archiviazione con ridondanza locale/remota | Archiviazione locale | Usare l'archiviazione con ridondanza remota se il pool di dati non può essere ricreato da altre origini dati.  |
| Pool di archiviazione (HDFS) | Archiviazione con ridondanza locale/remota | Archiviazione locale | La replica HDFS è abilitata. |
| Pool Spark | Archiviazione locale | Archiviazione locale | Dati utente non archiviati. |
| Controllo (controldb, metricsdb, logsdb)| Archiviazione con ridondanza remota | Archiviazione locale | Essenziale per l'uso di ridondanza a livello di archiviazione per i metadati del cluster Big Data. |

> [!IMPORTANT]
> Verificare che i componenti correlati al controllo siano in un dispositivo con archiviazione con ridondanza remota. Un pod `controldb-0` ospita un'istanza di SQL Server con database che archiviano tutti i metadati correlati agli stati del servizio cluster, diverse impostazioni e altre informazioni rilevanti. Se questa istanza non è più disponibile, si verificheranno problemi di integrità in altri servizi dipendenti nel cluster.

## <a name="configure-big-data-cluster-storage-settings"></a>Configurare le impostazioni di archiviazione del cluster Big Data

Come in altre personalizzazioni, è possibile specificare le impostazioni di archiviazione nei file di configurazione del cluster in fase di distribuzione per ogni pool nel file di configurazione `bdc.json` e per i servizi di controllo nel file `control.json`. Se le specifiche del pool non includono impostazioni di configurazione dell'archiviazione, verranno usate le impostazioni di archiviazione di controllo per tutti gli altri componenti, tra cui l'istanza master di SQL Server (risorsa `master`), HDFS (risorsa `storage-0`) e i componenti del pool di dati. L'esempio di codice seguente rappresenta la sezione di configurazione dell'archiviazione che è possibile includere nella specifica:

```json
    "storage": 
    {
      "data": {
        "className": "default",
        "accessMode": "ReadWriteOnce",
        "size": "15Gi"
      },
      "logs": {
        "className": "default",
        "accessMode": "ReadWriteOnce",
        "size": "10Gi"
    }
```

La distribuzione del cluster Big Data usa l'archiviazione permanente per archiviare i dati, i metadati e i log per diversi componenti. È possibile personalizzare le dimensioni delle attestazioni di volumi permanenti create come parte della distribuzione. La procedura consigliata è usare le classi di archiviazione con un *criterio di recupero* [Retain](https://kubernetes.io/docs/concepts/storage/storage-classes/#reclaim-policy).

> [!WARNING]
> L'esecuzione senza archiviazione permanente può funzionare in un ambiente di test, ma può causare il mancato funzionamento del cluster. Al riavvio del pod, i metadati del cluster e/o i dati utente vanno persi in modo permanente. L'esecuzione in questa configurazione non è consigliabile.

La sezione [Configurare l'archiviazione](#config-samples) contiene altri esempi su come configurare le impostazioni di archiviazione per la distribuzione del cluster Big Data di SQL Server.

## <a name="aks-storage-classes"></a>Classi di archiviazione del servizio Azure Kubernetes

Il servizio Azure Kubernetes include [due classi di archiviazione predefinite](/azure/aks/azure-disks-dynamic-pv/), `default` e `managed-premium` insieme allo strumento di provisioning dinamico per entrambe. È possibile specificare una delle due per creare una classe di archiviazione personalizzata per distribuire il cluster Big Data con archiviazione permanente abilitata. Per impostazione predefinita, il file di configurazione predefinito del cluster per il servizio Azure Kubernetes, `aks-dev-test`, include configurazioni dell'archiviazione permanente per l'uso della classe di archiviazione `default`.

> [!WARNING]
> I volumi permanenti creati con le classi di archiviazione predefinite `default` e `managed-premium` usano il criterio di recupero *Delete*. Di conseguenza, l'eliminazione del cluster Big Data di SQL Server implica l'eliminazione delle attestazioni di volumi permanenti così come dei volumi permanenti. È possibile creare classi di archiviazione personalizzate usando lo strumento di provisioning `azure-disk` con un criterio di recupero `Retain`, come illustrato nei [concetti relativi all'archiviazione](/azure/aks/concepts-storage/#storage-classes).

## <a name="storage-classes-for-kubeadm-clusters"></a>Classi di archiviazione per cluster `kubeadm` 

I cluster Kubernetes distribuiti con `kubeadm` non includono una classe di archiviazione predefinita. È necessario creare classi di archiviazione personalizzate e volumi permanenti usando l'archiviazione locale o lo [strumento di provisioning di archiviazione preferito](https://kubernetes.io/docs/concepts/storage/storage-classes/#provisioner). In questa situazione si imposta `className` sulla classe di archiviazione configurata.

> [!IMPORTANT]
>  Nei file di configurazione della distribuzione predefiniti per kubeadm (`kubeadm-dev-test` e `kubeadm-prod`) non è specificato alcun nome di classe di archiviazione per l'archiviazione di dati e log. Prima della distribuzione, è necessario personalizzare il file di configurazione e impostare il valore per `className`. In caso contrario, le convalide precedenti alla distribuzione avranno esito negativo. La distribuzione prevede anche un passaggio di convalida che controlla l'esistenza della classe di archiviazione, ma non i volumi permanenti necessari. Assicurarsi di creare volumi sufficienti in base alla scala del cluster. Per le dimensioni minime predefinite del cluster (scalabilità predefinita, nessuna disponibilità elevata), è necessario creare almeno 24 volumi permanenti. Per un esempio di creazione di volumi permanenti con uno strumento di provisioning locale, vedere [Creare un cluster Kubernetes usando Kubeadm](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/deployment/kubeadm/ubuntu).

## <a name="customize-storage-configurations-for-each-pool"></a>Personalizzare le configurazioni di archiviazione per ogni pool

Per tutte le personalizzazioni, è necessario per prima cosa creare una copia del file di configurazione predefinito che si vuole usare. Ad esempio, il comando seguente crea una copia dei file di configurazione della distribuzione `aks-dev-test` in una sottodirectory denominata `custom`:

```bash
azdata bdc config init --source aks-dev-test --target custom
```

Questo processo crea due file, `bdc.json` e `control.json`, che si possono personalizzare modificandoli manualmente o usando il comando `azdata bdc config`. È possibile usare una combinazione di librerie jsonpath e jsonpatch per fornire modi per modificare i file di configurazione.


### <a name="configure-storage-class-name-andor-claims-size"></a><a id="config-samples"></a> Configurare il nome della classe di archiviazione e/o le dimensioni delle attestazioni

Per impostazione predefinita, la dimensione delle attestazioni di volume permanente di cui è stato effettuato il provisioning per ognuno dei pod di cui è stato effettuato il provisioning nel cluster è 10 gigabyte (GB). È possibile aggiornare questo valore in base ai carichi di lavoro in esecuzione in un file di configurazione personalizzato prima della distribuzione del cluster.

Nell'esempio seguente la dimensione delle attestazioni di volume permanente viene aggiornata a 32 GB nel file `control.json`. Se non viene sostituita a livello di pool, questa impostazione verrà applicata a tutti i pool.

```bash
azdata bdc config replace --config-file custom/control.json --json-values "$.spec.storage.data.size=100Gi"
```

L'esempio seguente illustra come modificare la classe di archiviazione per il file `control.json`:

```bash
azdata bdc config replace --config-file custom/control.json --json-values "$.spec.storage.data.className=<yourStorageClassName>"
```

Un'altra opzione consiste nel modificare manualmente il file di configurazione personalizzato o usare un file di patch JSON che modifica la classe di archiviazione per il pool di archiviazione, come nell'esempio che segue:

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "$.spec.resources.storage-0.spec",
      "value": {
        "type":"Storage",
        "replicas":2,
        "storage":{
            "data":{
                    "size": "100Gi",
                    "className": "myStorageClass",
                    "accessMode":"ReadWriteOnce"
                    },
            "logs":{
                    "size":"32Gi",
                    "className":"myStorageClass",
                    "accessMode":"ReadWriteOnce"
                    }
                }
            }
        }
    ]
}
```

Applicare il file di patch. Usare il comando `azdata bdc config patch` per applicare le modifiche nel file di patch JSON. L'esempio seguente applica il file `patch.json` a un file di configurazione della distribuzione di destinazione, `custom.json`:

```bash
azdata bdc config patch --config-file custom/bdc.json --patch-file ./patch.json
```

## <a name="next-steps"></a>Passaggi successivi

- Per la documentazione completa sui volumi in Kubernetes, vedere la [documentazione di Kubernetes sui volumi](https://kubernetes.io/docs/concepts/storage/volumes/).

- Per altre informazioni sulle distribuzioni di un cluster Big Data di SQL Server, vedere [Come distribuire cluster Big Data di SQL Server in Kubernetes](deployment-guidance.md).
