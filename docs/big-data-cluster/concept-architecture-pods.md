---
title: Risorse distribuite
titleSuffix: SQL Server Big Data Clusters
description: Descrizione dei pod generalmente distribuiti in un cluster Big Data di SQL Server.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 02/11/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a3428763808e4bf2928c66ff42911b44cc74361d
ms.sourcegitcommit: c242f423cc3b776c20268483cfab0f4be54460d4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/25/2021
ms.locfileid: "105551591"
---
# <a name="resources-deployed-with-big-data-cluster"></a>Risorse distribuite in un cluster Big Data

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questo articolo descrive le risorse distribuite da un cluster Big Data di SQL Server.

Un cluster Big Data distribuisce i pod in base al profilo di distribuzione. Per informazioni dettagliate, vedere [Configurazioni predefinite](deployment-guidance.md#configfile). 

Questo articolo descrive i pod distribuiti con il profilo `aks-dev-test-ha` e include un pool Spark. Per visualizzare i pod distribuiti nel cluster, è possibile eseguire una query in Kubernetes. Nell'esempio seguente viene restituito un elenco di pod in uno spazio dei nomi specifico.

```bash
kubectl get pods -n <namespace>
```

Sostituire `<namespace>` con il nome del cluster Big Data. 

Per altre informazioni, vedere [Come distribuire [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in Kubernetes](deployment-guidance.md#configfile).

Il diagramma seguente mostra i componenti distribuiti in un cluster Big Data:

:::image type="content" source="media/big-data-cluster-overview/architecture-diagram-overview.png" alt-text="big-data-cluster-diagram":::

Per altre informazioni sull'architettura, vedere [Cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]?](big-data-cluster-overview.md).

## <a name="deployed-pods"></a>Pod distribuiti

La tabella seguente elenca i pod distribuiti in un cluster Big Data.

|Nome  |Area|
|---------|---------|
|`control-<nnnn>`        |[Controllo](#control)|
|`controldb-<#>`         |[Controllo](#control)|
|`controlwd-<nnnn>`      |[Controllo](#control)|
|`logsdb-<#>`            |[Controllo](#control)|
|`logsui-<nnnn>`         |[Controllo](#control)|
|`metricsdb-<#>`         |[Controllo](#control)|
|`metricsdc-<nnnn>`      |[Controllo](#control)|
|`metricsui-<nnnn>`      |[Controllo](#control)|
|`mgmtproxy-<nnnn>`      |[Controllo](#control)|
|`zookeeper-<#>`         |[Controllo](#control)|
|`dns-<nnnn>`            |[Controllo](#control)|
|`master-<#n>`           |[Istanza master](#master-instance)|
|`operator-<nnnn>`       |[Istanza master](#master-instance)
|`compute-<#n>-<#m>`     |[Pool di calcolo](#compute-pool)|
|`data-<#>-<#>`          |[Pool di dati](#data-pool) |
|`storage-<#>-<#>`       |[Pool di archiviazione](#storage-pool)|
|`nmnode-<#>-<#>`        |[Pool di archiviazione](#storage-pool)|
|`sparkhead-<#>`         |[Pool di archiviazione](#storage-pool)|
|`appproxy-<#m>`         |[Pool di applicazioni](#application-pool)|
|`gateway-<#>`           |[Servizio gateway](#gateway-service)|

Non tutti i pod sono inclusi in ogni cluster BDC. Le distribuzioni con disponibilità elevata o integrazione di Active Directory includono POD specifici. 

### <a name="high-availability-specific-pods"></a>Pod specifici in caso di disponibilità elevata:

- `operator-<nnnn>`
- `zookeeper-<#>`

### <a name="active-directory-specific-pods"></a>Pod specifici di Active Directory:

- `dns-<nnnn>`

Le sezioni seguenti descrivono i pod ed elencano i contenitori presenti in ogni pod.

## <a name="control"></a>Controllo

I pod di controllo forniscono il servizio di controllo.

|Nome pod |Conteggio| Tipo di controller Kubernetes | Contenitori |
|--------|----|------|--------|-------|
|`control-#`|1| ReplicaSet |- `controller`<br><br>- `security-support`<br><br>- `fluentbit`
|`controldb`|1| StatefulSet |- `mssql-server`<br><br>- `fluentbit`
|`controlwd`|1|  ReplicaSet |- `controlwatchdog`
|`logsdb-#` |1| StatefulSet |- `elasticsearch`
|`logsui`   |1| ReplicaSet |- `kibana`
|`metricsdb-#`|1| StatefulSet |- `influxdb`
|`metricsdc`|1 per nodo Kubernetes. | DaemonSet |- `telegraf` |
|`metricsui-nnnn`|1| ReplicaSet |- `grafana` |
|`mgmtproxy-nnnn`|1| ReplicaSet |- `service-proxy`<br><br>- `fluentbit`|
|`dns-nnnn`|0 o 1 per l'integrazione di Active Directory| ReplicaSet |- `dns`<br><br>- `fluentbit`|

## <a name="master-instance"></a>Istanza master

`master-<#n>` è l'istanza master di SQL Server.

- Gestisce il pool di dati tramite DDL
- Modifica i dati nel pool di dati tramite DML
- Disattiva l'esecuzione di query analitiche nel pool di dati

|Nome pod |Conteggio| Tipo di controller Kubernetes | Contenitori |
|--------|----|------|--------|
|`master-<#n>`|1 o più in caso di disponibilità elevata.| StatefulSet|- `mssql-server`<br><br>- `fluentbit`<br><br>- `collectd`<br><br>- `mssql-ha-supervisor` <sup>*</sup>|
|`operator`<sup>*</sup>| 0 o 1 in caso di disponibilità elevata | ReplicaSet |- `mssql-ha-operator`

<sup>*</sup> Solo distribuzioni con disponibilità elevata. L'operatore implementa e registra la definizione di risorsa personalizzata per SQL Server e le risorse del gruppo di disponibilità. Quando l'operatore viene distribuito, viene registrato come listener delle notifiche relative alle risorse SQL Server distribuite nel cluster Kubernetes. `mssql-ha-supervisor` supporta il gruppo di disponibilità.

Ogni pod `master` contiene un'istanza di SQL Server. Una distribuzione a disponibilità elevata include tre pod, ciascuno dei quali contiene un'istanza di SQL Server con database in un gruppo di disponibilità Always On SQL Server.

È possibile includere altri pod in fase di distribuzione, in base al carico di lavoro. 

## <a name="compute-pool"></a>Pool di calcolo

Il pool di calcolo fornisce un'istanza di SQL Server per il calcolo.

|Nome pod |Conteggio| Tipo di controller Kubernetes | Contenitori |
|--------|----|------|--------|
|`compute-<#n>-<#m>`|1 o più.| StatefulSet |- `mssql-server`<br><br>- `fluentbit`<br><br>- `collectd`

- `#n` identifica il pool di calcolo.
- `#m` identifica l'ID istanza nel pool.

Le istanze del pool di calcolo SQL Server sono senza stato. Richiedono solo spazio di archiviazione per `tempdb`.

È possibile includere altri pod in fase di distribuzione, in base al carico di lavoro. 

## <a name="data-pool"></a>Pool di dati

Il pool di dati fornisce istanze di SQL Server per l'archiviazione e il calcolo.

|Nome pod |Conteggio| Tipo di controller Kubernetes | Contenitori |
|--------|----|------|--------|-------|
|`data-<#n>-<#m>` | 0 o più | StatefulSet | -` mssql-server` <br><br>- `fluentbit`<br><br>- `collectd`|

- `#n` identifica il pool di dati.
- `#m` identifica l'ID istanza nel pool.

È possibile includere altri pod in fase di distribuzione, in base al carico di lavoro.

## <a name="storage-pool"></a>Pool di archiviazione

Il pool di archiviazione consente l'inserimento di dati tramite Spark, l'archiviazione in HDFS e l'accesso ai dati tramite HDFS, oltre a fornire endpoint SQL Server.

|Nome pod |Conteggio| Tipo di controller Kubernetes | Contenitori |
|--------|----|------|--------|
|`storage-0-#`|1 o più. È possibile includere altri pod in fase di distribuzione, in base al carico di lavoro. | StatefulSet |- `hadoop`<br><br>- `mssql-server`<br><br>- `fluentbit`<br><br>
|`nmnode-0-#`|1 o più in caso di disponibilità elevata| StatefulSet |- `hadoop`<br><br>- `fluentbit`
|`sparkehead-#`|1 o più in caso di disponibilità elevata| StatefulSet |- `hadoop-yarn-jobhistory`<br><br>- `hadoop-livy-sparkhistory`<br><br>- `hadoop-hivemetastore`<br><br>-- `fluentbit`
|`zookeeper`|0 o 3 in caso di disponibilità elevata. | StatefulSet |- `zookeeper`<br><br>- `fluentbit`

## <a name="application-pool"></a>Pool di applicazioni

Il pool di applicazioni è incluso in alcuni profili di configurazione di test. Esegue l'hosting dei proxy del servizio dell'applicazione definiti quando si distribuiscono le applicazioni per i cluster Big Data. 

`appproxy` è un'API Web che si trova davanti alle applicazioni del pool di applicazioni. Autentica gli utenti e quindi instrada le richieste alle applicazioni.

|Nome pod | Tipo di controller Kubernetes | Contenitori  |
|--------|----|------|
|`appproxy`| ReplicaSet |- `app-service-proxy`<br><br>- `fluentbit`

Per altre informazioni, vedere [Che cos'è la distribuzione di applicazioni in un cluster Big Data?](concept-application-deployment.md).

È possibile includere altri pod in fase di distribuzione, in base al carico di lavoro. 

## <a name="gateway-service"></a>Servizio gateway

I servizi gateway forniscono il gateway Knox a Spark, HDFS, [Yarn](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html), l'interfaccia utente di Yarn e l'interfaccia utente di Spark.

|Nome pod | Tipo di controller Kubernetes | Contenitori |
|--------|-----|-----|
|`gateway-<#>`| StatefulSet |- `knox`<br><br>- `fluentbit`

È supportato un solo gateway.

## <a name="open-source-container-references"></a>Riferimenti a contenitori open source

Per per progetti e versioni Open Source specifici, vedere informazioni di [riferimento sul software open source](reference-open-source-software.md).

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sui [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vedere le risorse seguenti:

- [Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)
- [Workshop: Architettura Microsoft dei [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](https://github.com/microsoft/sqlworkshops-bdc)
- [Come distribuire[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in Kubernetes](deployment-guidance.md#configfile)
