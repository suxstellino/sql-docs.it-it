---
title: Oggetti di Active Directory
titleSuffix: SQL Server Big Data Cluster
description: Descrive gli oggetti di Active Directory creati per i cluster Big Data di SQL Server.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 09/30/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 347129c6627bce865ffd4962a9cf2cb3b0beb862
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039782"
---
# <a name="auto-generated-active-directory-objects"></a>Oggetti di Active Directory generati automaticamente

Questo articolo descrive gli account e i gruppi di Active Directory (AD) creati da SQL Server durante la distribuzione di un cluster Big Data (BDC).

## <a name="accounts--groups"></a>Account e gruppi

Gli account utente e i gruppi vengono generati nell'[unità organizzativa (OU)](/windows-server/identity/ad-ds/plan/reviewing-ou-design-concepts) specificata durante la distribuzione del cluster.

Ogni account rappresenta un servizio nel cluster BDC. Gli account sono proprietari dei nomi di entità servizio (SPN) richiesti da ogni servizio. Gli SPN di proprietà di ogni account possono essere elencati tramite il comando [setspn](https://social.technet.microsoft.com/wiki/contents/articles/717.service-principal-names-spn-setspn-syntax.aspx).

La distribuzione genera automaticamente i nomi di account e di gruppo. A partire da SQL Server 2019 CU5, il prefisso dei nomi di account o di gruppo corrisponde al nome dello spazio dei nomi della distribuzione (nome del cluster BDC). Se per gli elementi riportati in questo articolo il nome del cluster è `bdc`, sostituire `<prefix>` con `bdc` per identificare gli account.

Di seguito, il suffisso pod (-x) denota un ID di pod variabile. I nomi seguenti non includono un prefisso variabile che viene specificato dall'utente durante la distribuzione.

Il nome dell'account classico si applica alle distribuzioni che usano versioni precedenti a SQL Server 2019 CU5 e alle distribuzioni eseguite con l'opzione "useSubdomain" impostata su false nella configurazione di sicurezza.

| Nome di account o di gruppo|Ulteriori informazioni|
|----|---|
|`<prefix>-ctrl`|[Account del servizio controller](#controller-service-account)|
|`<prefix>-ngxm`|[Account del servizio proxy di monitoraggio del servizio](#monitoring-service-proxy-service-account)|
|`<prefix>-ldap`|[Utente del servizio di ricerca LDAP](#ldap-lookup-user)|
|`<prefix>-sqc0-x/<prefix>-sqc0`|[Utente di SQL Server nel pool di calcolo](#compute-pool-sql-server-user)|
|`<prefix>-dmc0-x`|[Utente del Servizio Migrazione del database del data warehouse nel pool di calcolo](#compute-pool-data-warehouse-dms-user)|
|`<prefix>-dec0-x`|[Utente del motore del data warehouse nel pool di calcolo](#compute-pool-data-warehouse-engine-user)|
|`<prefix>-sqd0`|[Utente di SQL Server nel pool di dati](#data-pool-sql-server-user)|
|`<prefix>-sqs0`|[Utente di SQL Server nel pool di archiviazione](#storage-pool-sql-server-user)|
|`<prefix>-ynt0-x`|[Utente del servizio YARN Node Manager nel pool di archiviazione](#storage-pool-yarn-node-manager-service-user)|
|`<prefix>-htt0`|[Utente del servizio HTTP nel pool di archiviazione](#storage-pool-http-service-user)|
|`<prefix>-hdt0`|[Utente del servizio DataNode di HDFS nel pool di archiviazione](#storage-pool-hdfs-datanode-service-user)|
|`<prefix>-hdnn`|[Utente del servizio NameNode di HDFS](#hdfs-name-node-service-user)|
|`<prefix>-htnn`|[Utente del servizio HTTP del NameNode di HDFS](#hdfs-name-node-http-service-user)|
|`<prefix>-kmnn-x`|[Utente del servizio di gestione delle chiavi del NameNode](#name-node-kms-service-user)|
|`<prefix>-jnzk-x`|[Utenti del servizio JournalNode di Zookeeper](#zookeeper-journalnode-service-users)|
|`<prefix>-htzk`|[Utente del servizio HTTP di Zookeeper](#zookeeper-http-service-user)|
|`<prefix>-yrsh-x`|[Utente del servizio YARN Resource Manager di Sparkhead](#sparkhead-yarn-resource-manager-service-user)|
|`<prefix>-htsh`|[Utente HTTP di Sparkhead](#sparkhead-http-user)|
|`<prefix>-shsh-x`|[Utente del servizio cronologia Spark di Sparkhead](#sparkhead-spark-history-service-user)|
|`<prefix>-lvsh-x`|[Utente del servizio Livy di Sparkhead](#sparkhead-livy-service-user)|
|`<prefix>-hvsh-x`|[Utente del servizio Hive di Sparkhead](#sparkhead-hive-service-user)|
|`<prefix>-yns0-x`|[Utente del servizio YARN Node Manager nel pool Spark](#spark-pool-yarn-node-manager-service-user)|
|`<prefix>-hts0`|[Utente del servizio HTTP di YARN Node Manager nel pool Spark](#spark-pool-yarn-node-manager-http-user)|
|`<prefix>-knox-x`|[Utente del gateway Knox](#knox-gateway-user)|
|`<prefix>-htgw`|[Utente del servizio HTTP del gateway Knox](#knox-gateway-http-user)|
|`<prefix>-apst`|[Utente per l'installazione dell'app](#app-setup-user)|
|`<prefix>-dmsvc`|[Gruppo del Servizio Migrazione del database del data warehouse](#data-warehouse-dms-service-group)|
|`<prefix>-desvc`|[Gruppo del servizio del motore del data warehouse](#data-warehouse-engine-service-group)|

La sezione seguente include maggiori dettagli su ogni account. Per informazioni sui gruppi, passare a [Gruppi](#groups).

## <a name="controller-management--ldap-related-accounts"></a>Account correlati a controller, gestione e LDAP

### <a name="controller-service-account"></a>Account del servizio controller

|Oggetto|Nome account|
|---|---|
|Nome set di scalabilità|`control`|
|Nome pod|`control-x`|
|Nome contenitore|`controller`|
|Nome servizio|`controller`|
|Nome account (senza prefisso)| `ctrl`|
|Account (con prefisso dello spazio dei nomi)|`<prefix>-ctrl`|
|Nome account classico|`ctrl-controller`|

### <a name="monitoring-service-proxy-service-account"></a>Account del servizio proxy di monitoraggio del servizio

|Oggetto|Nome account|
|---|---|
|Nome set di scalabilità|`mgmtproxy`|
|Nome pod|`mgmtproxy-x`|
|Nome contenitore|`service-proxy`|
|Nome servizio|`nginx`|
|Account (senza prefisso)| `ngxm`|
|Account (con prefisso dello spazio dei nomi)|`<prefix>-ngxm`|
|Nome account classico|`nginx-mgmtproxy`|

### <a name="ldap-lookup-user"></a>Utente del servizio di ricerca LDAP

Usato dai servizi grafana e hadoop per cercare gli utenti tramite LDAP.

|Oggetto|Nome account|
|---|---|
|Nome set di scalabilità|`metricsui`|
|Nome pod|`metricsui-x`|
|Nome contenitore|`grafana`|
|Nome servizio|`grafana`|
|Nome account (senza prefisso)| `ldap`|
|Nome account (con prefisso dello spazio dei nomi)|`<prefix>-ldap`|
|Nome account classico|`ldap-user`|

## <a name="master-pool-accounts"></a>Account del pool master

### <a name="master-pool-sql-server-user"></a>Utente di SQL Server nel pool master

|Oggetto|Nome account|
|---|---|
|Nome set di scalabilità|`master`|
|Nome pod|`master-x`|
|Nome contenitore|`mssql-server`|
|Nome servizio|`mssql`|
|Nome account (senza prefisso)| `sqmp-x/sqmp`|
|Nome account (con prefisso dello spazio dei nomi)|`<prefix>-sqmp-x/<prefix>-sqmp`|
|Nome account classico|`mssql-master-x`|

### <a name="master-pool-data-warehouse-dms-user"></a>Utente del Servizio Migrazione del database del data warehouse nel pool master

|Oggetto|Nome account|
|---|---|
|Nome set di scalabilità|`master`|
|Nome pod|`master-x`|
|Nome contenitore|`mssql-server`|
|Nome servizio|`dwdms`|
|Account (senza prefisso)| `dmmp-x`|
|Account (con prefisso dello spazio dei nomi)|`<prefix>-dmmp-x`|
|Nome account classico|`dwdms-master-x`|

### <a name="master-pool-data-warehouse-engine-user"></a>Utente del motore del data warehouse nel pool master

|Oggetto|Nome account|
|---|---|
|Nome set di scalabilità|`master`|
|Nome pod|`master-x`|
|Nome contenitore|`mssql-server`|
|Nome servizio|`dweng`|
|Account (senza prefisso)| `demp`|
|Account (con prefisso dello spazio dei nomi)|`<prefix>-demp-x`|
|Nome account classico|`dweng-master-x`|

## <a name="compute-pool-accounts"></a>Account del pool di calcolo

### <a name="compute-pool-sql-server-user"></a>Utente di SQL Server nel pool di calcolo

|Oggetto|Nome account|
|---|---|
|Nome set di scalabilità|`compute-0`|
|Nome pod|`compute-0-x`|
|Nome contenitore|`mssql-server`|
|Nome servizio|`mssql`|
|Account (senza prefisso)| `sqc0-x/sqlc0`|
|Account (con prefisso dello spazio dei nomi)|`<prefix>-sqc0-x/<prefix>-sqc0`|
|Nome account classico|`mssql-compute-0-x`|

### <a name="compute-pool-data-warehouse-dms-user"></a>Utente del Servizio Migrazione del database del data warehouse nel pool di calcolo

|Oggetto|Nome account|
|---|---|
|Nome set di scalabilità|`compute-0`|
|Nome pod|`compute-0-x`|
|Nome contenitore|`mssql-server`|
|Nome servizio|`dwdms`|
|Account (senza prefisso)| `dmc0-x`|
|Account (con prefisso dello spazio dei nomi)|`<prefix>-dmc0-x`|
|Nome account classico|`dwdms-compute-0-x`|

### <a name="compute-pool-data-warehouse-engine-user"></a>Utente del motore del data warehouse nel pool di calcolo

|Oggetto|Nome account|
|---|---|
|Nome set di scalabilità|`compute-0`|
|Nome pod|`compute-0-x`|
|Nome contenitore|`mssql-server`|
|Nome servizio|`dweng`|
|Account (senza prefisso)| `dec0-x`|
|Account (con prefisso dello spazio dei nomi)|`<prefix>-dec0-x`|
|Nome account classico|`dweng-compute-0-x`|

## <a name="data-pool-accounts"></a>Account del pool di dati

### <a name="data-pool-sql-server-user"></a>Utente di SQL Server nel pool di dati

|Oggetto|Nome account|
|---|---|
|Nome set di scalabilità|`data-0`|
|Nome pod|`data-0-x`|
|Nome contenitore|`mssql-server`|
|Nome servizio|`mssql`|
|Account (senza prefisso)| `sqd0`|
|Account (con prefisso dello spazio dei nomi)|`<prefix>-sqd0`|
|Nome account classico|`mssql-data-0`|

## <a name="storage-pool-accounts"></a>Account del pool di archiviazione

### <a name="storage-pool-sql-server-user"></a>Utente di SQL Server nel pool di archiviazione

|Oggetto|Nome account|
|---|---|
|Nome set di scalabilità|`storage-0`|
|Nome pod|`storage-0-x`|
|Nome contenitore|`mssql-server`|
|Nome servizio|`mssql`|
|Account (senza prefisso)| `sqs0`|
|Account (con prefisso dello spazio dei nomi)|`<prefix>-sqs0`|
|Nome account classico|`mssql-storage-0`|

### <a name="storage-pool-yarn-node-manager-service-user"></a>Utente del servizio YARN Node Manager nel pool di archiviazione

|Oggetto|Nome account|
|---|---|
|Nome set di scalabilità|`storage-0`|
|Nome pod|`storage-0-x`|
|Nome contenitore|`hadoop`|
|Nome servizio|`Yarn Node Manager`|
|Account (senza prefisso)| `ynt0-x`|
|Account (con prefisso dello spazio dei nomi)|`<prefix>-ynt0-x`|
|Nome account classico|`yarnnm-storage-0-x`|

### <a name="storage-pool-http-service-user"></a>Utente del servizio HTTP nel pool di archiviazione

|Oggetto|Nome account|
|---|---|
|Nome set di scalabilità|`storage-0`|
|Nome pod|`storage-0-x`|
|Nome contenitore|`hadoop`|
|Nome servizio|`HDFS Datanode`|
|Account (senza prefisso)| `hdt0`|
|Account (con prefisso dello spazio dei nomi)|`<prefix>-hdt0`|
|Nome account classico|`http-storage-0`|

### <a name="storage-pool-hdfs-datanode-service-user"></a>Utente del servizio DataNode di HDFS nel pool di archiviazione

|Oggetto|Nome account|
|---|---|
|Nome set di scalabilità|`storage-0`|
|Nome pod|`storage-0-x`|
|Nome contenitore|`hadoop`|
|Nome servizio|`HDFS Datanode`|
|Account (senza prefisso)| `hdt0`|
|Account (con prefisso dello spazio dei nomi)|`<prefix>-hdt0`|
|Nome account classico|`hdfsdn-storage-0`|

## <a name="hdfs-accounts"></a>Account HDFS

### <a name="hdfs-name-node-service-user"></a>Utente del servizio NameNode di HDFS

|Oggetto|Nome account|
|---|---|
|Nome set di scalabilità|`nmnode-0`|
|Nome pod|`nmnode-0-x`|
|Nome contenitore|`hadoop`|
|Nome servizio|`HDFS Namenode`|
|Account (senza prefisso)| `hdnn`|
|Account (con prefisso dello spazio dei nomi)|`<prefix>-hdnn`|
|Nome account classico|`hdfsnn-nmnode`|

### <a name="hdfs-name-node-http-service-user"></a>Utente del servizio HTTP del NameNode di HDFS

|Oggetto|Nome account|
|---|---|
|Nome set di scalabilità|`nmnode-0`|
|Nome pod|`nmnode-0-x`|
|Nome contenitore|`hadoop`|
|Nome servizio|`HDFS Namenode`|
|Account (senza prefisso)| `htnn`|
|Account (con prefisso dello spazio dei nomi)|`<prefix>-htnn`|
|Nome account classico|`http-nmnode`|

## <a name="kms-accounts"></a>Account del servizio di gestione delle chiavi

### <a name="name-node-kms-service-user"></a>Utente del servizio di gestione delle chiavi del NameNode

|Oggetto|Nome account|
|---|---|
|Nome set di scalabilità|`nmnode-0`|
|Nome pod|`nmnode-0-x`|
|Nome contenitore|`hadoop`|
|Nome servizio|`KMS`|
|Account (senza prefisso)| `kmnn-x`|
|Account (con prefisso dello spazio dei nomi)|`<prefix>-kmnn-x`|
|Nome account classico|`kms-nmnode-x`|

## <a name="zookeeper-accounts"></a>Account di Zookeeper

### <a name="zookeeper-journalnode-service-users"></a>Utenti del servizio JournalNode di Zookeeper

|Oggetto|Nome account|
|---|---|
|Nome set di scalabilità|`zookeeper`|
|Nome pod|`zookeeper-x`|
|Nome contenitore|`zookeeper`|
|Nome servizio|`Journal node`|
|Account (senza prefisso)| `jnzk-x`|
|Account (con prefisso dello spazio dei nomi)|`<prefix>-jnzk-x`|
|Nome account classico|`jn-zookeeper-x`|

### <a name="zookeeper-http-service-user"></a>Utente del servizio HTTP di Zookeeper

|Oggetto|Nome account|
|---|---|
|Nome set di scalabilità|`zookeeper`|
|Nome pod|`zookeeper-x`|
|Nome contenitore|`zookeeper`|
|Nome servizio|`Zookeeper`|
|Account (senza prefisso)| `htzk`|
|Account (con prefisso dello spazio dei nomi)|`<prefix>-htzk`|
|Nome account classico|`http-zookeeper`|

## <a name="spark-related-accounts"></a>Account correlati a Spark

### <a name="sparkhead-yarn-resource-manager-service-user"></a>Utente del servizio YARN Resource Manager di Sparkhead

|Oggetto|Nome account|
|---|---|
|Nome set di scalabilità|`sparkhead`|
|Nome pod|`sparkhead-x`|
|Nome contenitore|`hadoop-yarn-jobhistory`|
|Nome servizio|`Yarn Resource Manager`|
|Account (senza prefisso)| `yrsh-x`|
|Account (con prefisso dello spazio dei nomi)|`<prefix>-yrsh-x`|
|Nome account classico|`yarnrm-sparkhead-x`|

### <a name="sparkhead-http-user"></a>Utente HTTP di Sparkhead

|Oggetto|Nome account|
|---|---|
|Nome set di scalabilità|`sparkhead`|
|Nome pod|`sparkhead-x`|
|Nome contenitore|`*`|
|Nome servizio|`*`|
|Account (senza prefisso)| `htsh`|
|Account (con prefisso dello spazio dei nomi)|`<prefix>-htsh`|
|Nome account classico|`http-sparkhead`|

### <a name="sparkhead-spark-history-service-user"></a>Utente del servizio cronologia Spark di Sparkhead

|Oggetto|Nome account|
|---|---|
|Nome set di scalabilità|`sparkhead`|
|Nome pod|`sparkhead-x`|
|Nome contenitore|`hadoop-livy-sparkhistory`|
|Nome servizio|`Spark History Server`|
|Account (senza prefisso)| `shsh-x`|
|Account (con prefisso dello spazio dei nomi)|`<prefix>-shsh-x`|
|Nome account classico|`sph-sparkhead-x`|

### <a name="sparkhead-livy-service-user"></a>Utente del servizio Livy di Sparkhead

|Oggetto|Nome account|
|---|---|
|Nome set di scalabilità|`sparkhead`|
|Nome pod|`sparkhead-x`|
|Nome contenitore|`hadoop-livy-sparkhistory`|
|Nome servizio|`Livy`|
|Account (senza prefisso)| `lvsh-x`|
|Account (con prefisso dello spazio dei nomi)|`<prefix>-lvsh-x`|
|Nome account classico|`livy-sparkhead-x`|

### <a name="sparkhead-hive-service-user"></a>Utente del servizio Hive di Sparkhead

|Oggetto|Nome account|
|---|---|
|Nome set di scalabilità|`sparkhead`|
|Nome pod|`sparkhead-x`|
|Nome contenitore|`hadoop-hivemetastore`|
|Nome servizio|`Hive Metastore`|
|Account (senza prefisso)| `hvsh-x`|
|Account (con prefisso dello spazio dei nomi)|`<prefix>-hvsh-x`|
|Nome account classico|`hive-sparkhead-x`|

### <a name="spark-pool-yarn-node-manager-service-user"></a>Utente del servizio YARN Node Manager nel pool Spark

|Oggetto|Nome account|
|---|---|
|Nome set di scalabilità|`spark-0`|
|Nome pod|`spark-0-x`|
|Nome contenitore|`hadoop`|
|Nome servizio|`Yarn Node Manager`|
|Account (senza prefisso)| `yns0-x`|
|Account (con prefisso dello spazio dei nomi)|`<prefix>-yns0-x`|
|Nome account classico|`yarnnm-spark-0-x`|

### <a name="spark-pool-yarn-node-manager-http-user"></a>Utente del servizio HTTP di YARN Node Manager nel pool Spark

|Oggetto|Nome account|
|---|---|
|Nome set di scalabilità|`spark-0`|
|Nome pod|`spark-0-x`|
|Nome contenitore|`hadoop`|
|Nome servizio|`Yarn Node Manager`|
|Account (senza prefisso)| `hts0`|
|Account (con prefisso dello spazio dei nomi)|`<prefix>-hts0`|
|Nome account classico|`http-spark-0`|

## <a name="knox-accounts"></a>Account Knox

### <a name="knox-gateway-user"></a>Utente del gateway Knox

|Oggetto|Nome account|
|---|---|
|Nome set di scalabilità|`gateway`|
|Nome pod|`gateway-x`|
|Nome contenitore|`knox`|
|Nome servizio|`Knox`|
|Account (senza prefisso)| `knox-x`|
|Account (con prefisso dello spazio dei nomi)|`<prefix>-knox-x`|
|Nome account classico|`knox-gateway-x`|

### <a name="knox-gateway-http-user"></a>Utente del servizio HTTP del gateway Knox

|Oggetto|Nome account|
|---|---|
|Nome set di scalabilità|`gateway`|
|Nome pod|`gateway-x`|
|Nome contenitore|`knox`|
|Nome servizio|`Knox`|
|Account (senza prefisso)| `htgw`|
|Account (con prefisso dello spazio dei nomi)|`<prefix>-htgw`|
|Nome account classico|`http-gateway`|

## <a name="app-accounts"></a>Account dell'app

### <a name="app-setup-user"></a>Utente per l'installazione dell'app

|Oggetto|Nome account|
|---|---|
|Nome set di scalabilità|`appproxy`|
|Nome pod|`appproxy-x`|
|Nome contenitore|`App Service Proxy`|
|Nome servizio|`nginx`|
|Account (senza prefisso)| `apst`|
|Account (con prefisso dello spazio dei nomi)|`<prefix>-apst`|
|Nome account classico|`app-setup`|

## <a name="groups"></a>Gruppi

I gruppi seguenti vengono creati nell'unità organizzativa specificata dall'utente. I membri dei gruppi sono gli utenti creati in precedenza per i servizi corrispondenti.

### <a name="data-warehouse-dms-service-group"></a>Gruppo del Servizio Migrazione del database del data warehouse

|Oggetto|Nome gruppo|
|---|---|
|Nome set di scalabilità|`master/compute-0`|
|Nome pod|`master-x/compute-0-x`|
|Nome contenitore|`mssql-server`|
|Nome servizio|`dwdms`|
|Gruppo (senza prefisso)| `dmsvc`|
|Account (con prefisso dello spazio dei nomi)|`<prefix>-dmsvc`|
|Nome account classico|`dwdms-service`|

### <a name="data-warehouse-engine-service-group"></a>Gruppo del servizio del motore del data warehouse

|Oggetto|Nome gruppo|
|---|---|
|Nome set di scalabilità|`master/compute-0`|
|Nome pod|`master-x/compute-0-x`|
|Nome contenitore|`mssql-server`|
|Nome servizio|`dweng`|
|Gruppo (senza prefisso)| `desvc`|
|Account (con prefisso dello spazio dei nomi)|`<prefix>-desvc`|
|Nome account classico|`desvc`|

## <a name="next-steps"></a>Passaggi successivi

[Distribuire [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in modalità Active Directory](active-directory-deploy.md)

[Distribuire più [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] nello stesso dominio di Active Directory](active-directory-deployment-background.md)
