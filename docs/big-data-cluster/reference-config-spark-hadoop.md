---
title: Proprietà di configurazione di Apache Spark e Apache Hadoop
titleSuffix: SQL Server big data clusters
description: Articolo di riferimento per le proprietà di configurazione di Apache Spark e Apache Hadoop (HDFS).
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 03/23/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 231381eccc46430b31b1af7d3001f4fd6134363b
ms.sourcegitcommit: c242f423cc3b776c20268483cfab0f4be54460d4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/25/2021
ms.locfileid: "105551246"
---
# <a name="apache-spark--apache-hadoop-hdfs-configuration-properties"></a>Proprietà di configurazione di Apache Spark e Apache Hadoop (HDFS)

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

I cluster di Big Data supportano la configurazione dell'ora di distribuzione e di post-distribuzione dei componenti Apache Spark e Hadoop nel servizio e negli ambiti delle risorse. I cluster di Big Data usano gli stessi valori di configurazione predefiniti del rispettivo progetto open source per la maggior parte delle impostazioni. Le impostazioni modificate sono elencate di seguito insieme a una descrizione e al relativo valore predefinito. Oltre alla risorsa gateway, non esiste alcuna differenza tra le impostazioni configurabili nell'ambito del servizio e nell'ambito della risorsa.

È possibile trovare tutte le configurazioni possibili e le impostazioni predefinite nel sito della documentazione di Apache associato:
- Apache Spark: https://spark.apache.org/docs/latest/configuration.html
- Apache Hadoop:
  - HDFS HDFS-Site: https://hadoop.apache.org/docs/r2.7.1/hadoop-project-dist/hadoop-hdfs/hdfs-default.xml
  - HDFS Core-Site: https://hadoop.apache.org/docs/r2.8.0/hadoop-project-dist/hadoop-common/core-default.xml  
  - Yarn: https://hadoop.apache.org/docs/r3.1.1/hadoop-yarn/hadoop-yarn-site/ResourceModel.html
- Hive: https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties#ConfigurationProperties-MetaStore
- Livy: https://github.com/cloudera/livy/blob/master/conf/livy.conf.template
- Apache Knox Gateway: https://knox.apache.org/books/knox-0-14-0/user-guide.html#Gateway+Details

Di seguito sono elencate anche le impostazioni che non supportano la configurazione.

> [!NOTE]
> Per includere Spark nel pool di archiviazione, impostare il valore booleano `includeSpark` nel file di configurazione `bdc.json` in `spec.resources.storage-0.spec.settings.spark`. Vedere [Configurare Apache Spark e Apache Hadoop nei cluster Big Data](configure-spark-hdfs.md) per istruzioni.


##  <a name="big-data-clusters-specific-default-spark-settings"></a>Cluster di Big Data: impostazioni Spark predefinite specifiche
Le impostazioni di Spark seguenti sono quelle con impostazioni predefinite specifiche del catalogo dati business, ma che possono essere configurate dall'utente. Non sono incluse le impostazioni gestite dal sistema.

| Nome dell'impostazione                                                                                 | Descrizione                                                                                                                                                           | Tipo   | Valore predefinito                                                                                                                              |
| ------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------ |
| Capacity-Scheduler. Yarn. Scheduler. Capacity. Maximum-applicazioni                      | Numero massimo di applicazioni nel sistema che possono essere contemporaneamente attive sia in esecuzione che in sospeso.                                                               | INT    | 10000                                                                                                                                      |
| Capacity-Scheduler. Yarn. Scheduler. Capacity. Resource-Calculator                       | Implementazione di ResourceCalculator da utilizzare per confrontare le risorse nell'utilità di pianificazione.                                                                               | string | org. Apache. Hadoop. Yarn. util. Resource. DominantResourceCalculator                                                                            |
| Capacity-Scheduler. Yarn. Scheduler. Capacity. root. Queues                               | Utilità di pianificazione della capacità con coda predefinita denominata radice.                                                                                                              | string | default                                                                                                                                    |
| Capacity-Scheduler. Yarn. Scheduler. Capacity. root. default. Capacity                     | Capacità coda in percentuale (%) come capacità minima della coda di risorse assoluta per la coda radice.                                                                          | INT    | 100                                                                                                                                        |
| Spark-valori predefiniti-conf. Spark. driver. Cores                                               | Numero di core da usare per il processo del driver, solo in modalità cluster.                                                                                                  | INT    | 1                                                                                                                                          |
| Spark-valori predefiniti-conf. Spark. driver. memoryOverhead                                      | Quantità di memoria non heap da allocare per ogni driver in modalità cluster.                                                                                             | INT    | 384                                                                                                                                        |
| spark-defaults-conf.spark.exeCutor. Instances                                         | Numero di esecutori per l'allocazione statica.                                                                                                                        | INT    | 1                                                                                                                                          |
| spark-defaults-conf.spark.exeCutor. Cores                                             | Il numero di core da usare per ogni Executor.                                                                                                                          | INT    | 1                                                                                                                                          |
| Spark-impostazioni predefinite-conf. Spark. driver. memory                                              | Quantità di memoria da utilizzare per il processo del driver.                                                                                                                       | string | 1g                                                                                                                                         |
| spark-defaults-conf.spark.exeCutor. memory                                            | Quantità di memoria da utilizzare per ogni processo Executor.                                                                                                                         | string | 1g                                                                                                                                         |
| spark-defaults-conf.spark.exeCutor. memoryOverhead                                    | Quantità di memoria non heap da allocare per ogni Executor.                                                                                                           | INT    | 384                                                                                                                                        |
| Yarn-site. Yarn. nodemanager. Resource. memory-MB                                        | Quantità di memoria fisica, in MB, che può essere allocata per i contenitori.                                                                                               | INT    | 8192                                                                                                                                       |
| Yarn-site. Yarn. Scheduler. Maximum-allocation-MB                                       | L'allocazione massima per ogni richiesta del contenitore in Resource Manager.                                                                                           | INT    | 8192                                                                                                                                       |
| Yarn-site. Yarn. nodemanager. Resource. CPU-vcore                                       | Numero di core CPU che possono essere allocati per i contenitori.                                                                                                             | INT    | 32                                                                                                                                         |
| Yarn-site. Yarn. Scheduler. Maximum-allocation-vcore                                   | L'allocazione massima per ogni richiesta di contenitore a Resource Manager, in termini di core CPU virtuali.                                                            | INT    | 8                                                                                                                                          |
| Yarn-site. Yarn. nodemanager. Linux-container-Executor. Secure-Mode. pool-User-Count      | Il numero di utenti del pool per l'executor del contenitore Linux in modalità protetta.                                                                                             | INT    | 6                                                                                                                                          |
| Yarn-site. Yarn. Scheduler. Capacity. Maximum-am-Resource-percent                        | Percentuale massima di risorse nel cluster che è possibile usare per eseguire i master applicazioni.                                                                              | float  | 0,1                                                                                                                                        |
| Yarn-site. Yarn. nodemanager. container-Executor. Class                                  | Esecutori di contenitori per uno o più sistemi operativi specifici.                                                                                                               | string | org. Apache. Hadoop. Yarn. Server. nodemanager. LinuxContainerExecutor                                                                           |
| Capacity-Scheduler. Yarn. Scheduler. Capacity. root. default. user-limit-Factor            | Il multiplo della capacità della coda che può essere configurata in modo da consentire a un singolo utente di acquisire più risorse.                                                          | INT    | 1                                                                                                                                          |
| Capacity-Scheduler. Yarn. Scheduler. Capacity. root. default. capacità massima             | Capacità massima della coda in percentuale (%) come una capacità massima della coda di risorse float o Absolute. L'impostazione di questo valore su-1 imposta la capacità massima su 100%.           | INT    | 100                                                                                                                                        |
| Capacity-Scheduler. Yarn. Scheduler. Capacity. root. default. state                        | Lo stato della coda può essere in esecuzione o arrestato.                                                                                                                      | string | RUNNING                                                                                                                                    |
| Capacity-Scheduler. Yarn. Scheduler. Capacity. root. default. Max-Application-Lifetime | Durata massima di un'applicazione inviata a una coda in pochi secondi. Qualsiasi valore minore o uguale a zero verrà considerato disabilitato.                     | INT    | -1                                                                                                                                         |
| Capacity-Scheduler. Yarn. Scheduler. Capacity. root. default. Default-Application-Lifetime | Durata predefinita di un'applicazione inviata a una coda in pochi secondi. Qualsiasi valore minore o uguale a zero verrà considerato disabilitato.                     | INT    | -1                                                                                                                                         |
| Capacity-Scheduler. Yarn. Scheduler. Capacity. Node-locale-ritardo                       | Numero di opportunità di pianificazione mancanti dopo le quali il CapacityScheduler tenta di pianificare i contenitori di rack locali.                                               | INT    | 40                                                                                                                                         |
| Capacity-Scheduler. Yarn. Scheduler. Capacity. rack-locality-additional-Delay            | Numero di altre opportunità di pianificazione mancanti rispetto al ritardo dei nodi di località, dopo il quale il CapacityScheduler tenta di pianificare i contenitori fuori dal cambio. | INT    | -1                                                                                                                                         |
| Hadoop-ENV. HADOOP_HEAPSIZE_MAX                                                       | Dimensioni massime heap predefinite di tutti i processi JVM Hadoop.                                                                                                                 | INT    | 2048                                                                                                                                       |
| Yarn-ENV. YARN_RESOURCEMANAGER_HEAPSIZE                                               | Dimensioni heap del ResourceManager Yarn.                                                                                                                                     | INT    | 2048                                                                                                                                       |
| Yarn-ENV. YARN_NODEMANAGER_HEAPSIZE                                                   | Dimensioni heap di Yarn NodeManager.                                                                                                                                         | INT    | 2048                                                                                                                                       |
| mapred-ENV. HADOOP_JOB_HISTORYSERVER_HEAPSIZE                                         | Dimensioni heap del processo di Hadoop HistoryServer.                                                                                                                                 | INT    | 2048                                                                                                                                       |
| hive-ENV. HADOOP_HEAPSIZE                                                             | Dimensioni heap di Hadoop per hive.                                                                                                                                          | INT    | 2048                                                                                                                                       |
| Livio-conf. Livio. Server. Session. timeout-check                                          | Verificare il timeout della sessione del server Livio.                                                                                                                                | bool   | true                                                                                                                                       |
| Livio-conf. Livio. Server. Session. timeout-check. Skip-occupato                                | Skip-busy per verificare il timeout della sessione del server Livio.                                                                                                                  | bool   | true                                                                                                                                       |
| Livio-conf. Livio. Server. Session. timeout                                                | Timeout per la sessione del server Livio in (MS/s/m \| min/h/d/y).                                                                                                              | string | 2h                                                                                                                                         |
| Livio-conf. Livio. Server. Yarn. Poll-Interval                                             | Intervallo di polling per Yarn nel server Livio in (MS/s/m \| min/h/d/y).                                                                                                     | string | 500 ms                                                                                                                                      |
| Livy-conf. Livio. rsc. jar                                                              | Jar di Livio RSC.                                                                                                                                                        | string | local:/opt/Livio/RSC-Jars/Livy-API. jar, local:/opt/Livio/RSC-Jars/Livy-RSC. jar, local:/opt/Livio/RSC-Jars/Netty-all. jar                         |
| Livio-conf. Livio. repl. jar                                                             | REPL di Livio.                                                                                                                                                       | string | local:/opt/Livio/repl_2 .11-Jars/Livy-core. jar, local:/opt/Livio/repl_2 .11-Jars/Livy-repl. jar, local:/opt/Livio/repl_2.11-Jars/Commons-codec. jar |
| Livy-conf. Livio. rsc. sparkr. Package                                                    | Pacchetto di Livio Sparkr RSC.                                                                                                                                              | string | sparkr.zip hdfs:///system/livy/                                                                                                             |
| Livio-ENV. LIVY_SERVER_JAVA_OPTS                                                       | Opzioni Java del server Livio.                                                                                                                                             | string | -Xmx2g                                                                                                                                     |
| Spark-valori predefiniti-conf. Spark. r. backendConnectionTimeout                                 | Timeout di connessione impostato dal processo R sulla connessione a RBackend in secondi.                                                                                         | INT    | 86400                                                                                                                                      |
| Spark-valori predefiniti-conf. Spark. pyspark. Python                                             | Opzione Python per Spark.                                                                                                                                              | string | /opt/bin/python3                                                                                                                           |
| Spark-valori predefiniti-conf. Spark. Yarn. jar                                                  | Jar Yarn.                                                                                                                                                            | string | locale:/opt/Spark/jar/*                                                                                                                    |
| Spark-History-server-conf. Spark. History. FS. Cleaner. maxAge                            | Validità massima dei file di cronologia processo prima che vengano eliminati dalla pulizia della cronologia file System in (MS/s/m \| min/h/d/y).                                                   | string | 7D                                                                                                                                         |
| Spark-History-server-conf. Spark. History. FS. Cleaner. Interval                          | Intervallo di pulizia per la cronologia Spark in (MS/s/m \| min/h/d/y).                                                                                                        | string | 12 ore                                                                                                                                        |
| Hadoop-ENV. HADOOP_CLASSPATH                                                          | Imposta il classpath Hadoop aggiuntivo.                                                                                                                                 | string |                                                                                                                                            |
| Spark-ENV. SPARK_DAEMON_MEMORY                                                        | Memoria del daemon Spark.                                                                                                                                                  | string | 2G                                                                                                                                         |
| Yarn-site. Yarn. log-aggregazione. conservazione-secondi                                        | Quando l'aggregazione dei log è abilitata, questa proprietà determina il numero di secondi di conservazione dei log.                                                                       | INT    | 604800                                                                                                                                     |
| Yarn-site. Yarn. nodemanager. log-Aggregation. Compression-Type                          | Tipo di compressione per l'aggregazione dei log per Yarn NodeManager.                                                                                                            | string | GZ                                                                                                                                         |
| Yarn-site. Yarn. nodemanager. log-Aggregation. Roll-Monitoring-Interval-seconds          | Intervallo di secondi per il monitoraggio del roll nell'aggregazione dei log NodeManager.                                                                                                  | INT    | 3600                                                                                                                                       |
| Yarn-site. Yarn. Scheduler. Minimum-allocation-MB                                       | L'allocazione minima per ogni richiesta del contenitore nel Gestione risorse, in MB.                                                                                   | INT    | 512                                                                                                                                        |
| Yarn-site. Yarn. Scheduler. Minimum-allocation-vcore                                   | L'allocazione minima per ogni richiesta del contenitore nel Gestione risorse in termini di core CPU virtuali.                                                             | INT    | 1                                                                                                                                          |
| Yarn-site. Yarn. nm. liveity-monitor. scadenza-intervallo-MS                                | Tempo di attesa prima che un gestore di nodi venga considerato inattivo.                                                                                                             | INT    | 180000                                                                                                                                     |
| Yarn-site. Yarn. ResourceManager. ZK-timeout-MS                                         | Timeout della sessione ' ZooKeeper ' in millisecondi.                                                                                                                          | INT    | 40000                                                                                                                                      |
| capacità-scheduler.yarn.scheduler.capacity.root.default.acl_application_max_priority | ACL di chi può inviare applicazioni con la priorità configurata. Ad esempio, [user = {name} Group = {Name} max_priority = {Priority} default_priority = {Priority}].             | string | *                                                                                                                                          |
| includeSpark                                                                         | Valore booleano per specificare se i processi Spark possono essere eseguiti o meno nel pool di archiviazione.                                                                                           | bool   | true                                                                                                                                       |
| enableSparkOnK8s                                                                     | Valore booleano per specificare se abilitare Spark in K8s, che aggiunge i contenitori per K8s in Spark Head.                                                               | bool   | false                                                                                                                                      |
| sparkVersion                                                                         | Versione di Spark                                                                                                                                                  | string | 2.4                                                                                                                                        |
| Spark-ENV. PYSPARK_ARCHIVES_PATH                                                      | Percorso dei file jar di archivio pyspark usati nei processi Spark.                                                                                                                      | string | locale:/opt/Spark/Python/lib/pyspark.zip, local:/opt/Spark/Python/lib/py4j-0.10.7-src.zip                                                    |

Nelle sezioni seguenti sono elencate le configurazioni non supportate.

##  <a name="big-data-clusters-specific-default-hdfs-settings"></a>Cluster di Big Data-impostazioni HDFS predefinite specifiche
Le impostazioni di HDFS di seguito sono quelle con impostazioni predefinite specifiche del catalogo dati business, ma che possono essere configurate dall'utente. Non sono incluse le impostazioni gestite dal sistema.

| Nome dell'impostazione                                                                       | Descrizione                                                                                         | Tipo   | Valore predefinito                                                                    |
| -------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- | ------ | -------------------------------------------------------------------------------------- |
| HDFS-site. DFS. Replication                                                  | Replica a blocchi predefinita.                                                                          | INT    | 2                                                                                      |
| HDFS-site. DFS. NameNode. fornito. Enabled                                    | Consente al nodo del nome di gestire le archiviazioni fornite.                                                   | bool   | true                                                                                   |
| HDFS. site. DFS. NameNode. Mount. ACL. Enabled |  Impostare su true per ereditare gli ACL (elenchi di controllo di accesso) dagli archivi remoti durante il montaggio.| bool | false |
| HDFS-site. DFS. dataNode. fornito. Enabled                                    | Consente al nodo dati di gestire le archiviazioni fornite.                                                   | bool   | true                                                                                   |
| HDFS-site. DFS. dataNode. fornito. volume. Lazy. Load                           | Abilitare il caricamento lazy nel nodo dati per le archiviazioni fornite.                                                 | bool   | true                                                                                   |
| HDFS-site. DFS. fornito. aliasmap. InMemory. Enabled                           | Abilitare la mappa degli alias in memoria per le archiviazioni fornite.                                                     | bool   | true                                                                                   |
| HDFS-site. DFS. fornito. aliasmap. Class                                      | Classe utilizzata per specificare il formato di input dei blocchi nelle archiviazioni fornite.              | string | org. Apache. Hadoop. HDFS. Server. Common. blockaliasmap. impl. InMemoryLevelDBAliasMapClient  |
| HDFS-site. DFS. NameNode. fornito. aliasmap. Class                             | Classe utilizzata per specificare il formato di input dei blocchi nelle archiviazioni fornite per NameNode. | string | org. Apache. Hadoop. HDFS. Server. Common. blockaliasmap. impl. NamenodeInMemoryAliasMapClient |
| HDFS-site. DFS. fornito. aliasmap. Load. Retries                               | Numero di tentativi sul nodo dataNode per caricare il aliasmap specificato.                                    | INT    | 0                                                                                      |
| Dimensioni hdfs-site.dfs.provided.aliasmap.inmemory.batch                        | Dimensioni del batch durante lo scorrimento del database che esegue il aliasmap.                               | INT    | 500                                                                                    |
| HDFS-site. DFS. dataNode. fornito. volume. readthrough                         | Abilitare readthrough per le archiviazioni fornite in dataNode.                                               | bool   | true                                                                                   |
| HDFS-site. DFS. fornito. cache. Capacity. Mount                                | Abilitare il montaggio della capacità della cache per le archiviazioni fornite.                                                  | bool   | true                                                                                   |
| HDFS-site. DFS. fornito. overreplication. Factor                              | Fattore di sovrareplica per le archiviazioni fornite.  Numero di blocchi di cache in BDC creati per blocco HDFS remoto.                                                     | float  | 1                                                                                      |
| HDFS-site. DFS. fornito. cache. Capacity. fraction                             | Frazione della capacità della cache per l'archiviazione specificata. Frazione della capacità totale del cluster che può essere utilizzata per memorizzare nella cache i dati dagli archivi forniti.                                                      | float  | 0.01                                                                                   |
| HDFS-site. DFS. fornito. cache. Capacity. bytes | Capacità del cluster da utilizzare come spazio della cache per i blocchi specificati, in byte. | INT | -1 | 
| HDFS-site. DFS. ls. Limit                                                     | Limitare il numero di file stampati da ls.                                                            | INT    | 500                                                                                    |
| HDFS-ENV. HDFS_NAMENODE_OPTS                                                | Opzioni di NameNode HDFS.                                                                              | string | -Dhadoop. Security. logger = INFO, RFAS-Xmx2g                                              |
| HDFS-ENV. HDFS_DATANODE_OPTS                                                | Opzioni dataNode HDFS.                                                                              | string | -Dhadoop. Security. logger = ERROR, RFAS-Xmx2g                                             |
| HDFS-ENV. HDFS_ZKFC_OPTS                                                    | Opzioni di ZKFC HDFS.                                                                                  | string | -Xmx1g                                                                                 |
| HDFS-ENV. HDFS_JOURNALNODE_OPTS                                             | Opzioni di JournalNode HDFS.                                                                           | string | -Xmx2g                                                                                 |
| HDFS-ENV. HDFS_AUDIT_LOGGER                                                 | Opzioni del logger di controllo HDFS.                                                                          | string | INFO, RFAAUDIT                                                                          |
| Core-site. Hadoop. Security. Group. mapping. LDAP. search. Group. Hierarchy. Levels | Livelli di gerarchia per il gruppo di ricerca LDAP Hadoop del sito principale.                                            | INT    | 10                                                                                     |
| Core-site. FS. Permissions. umask-Mode                                        | Modalità umask di autorizzazione.                                                                              | string | 077                                                                                    |
| Core-site. Hadoop. Security. KMS. client. failover. max. Retries                  | Numero massimo di tentativi per il failover del client.                                                                    | INT    | 20                                                                                     |
| Zoo-cfg. tickTime                                       | Tempo di ciclo per la configurazione di ' ZooKeeper '.                         | INT    | 2000                |
| zoo-cfg.initLimit                                      | Tempo di inizializzazione per la configurazione di ' ZooKeeper '.                         | INT    | 10                  |
| Zoo-cfg. syncLimit                                      | Tempo di sincronizzazione per la configurazione di ' ZooKeeper '.                         | INT    | 5                   |
| Zoo-cfg. maxClientCnxns                                 | Numero massimo di connessioni client per la configurazione di ' ZooKeeper '.            | INT    | 60                  |
| Zoo-cfg. minSessionTimeout                              | Timeout della sessione minima per la configurazione di ' ZooKeeper '.           | INT    | 4000                |
| Zoo-cfg. maxSessionTimeout                              | Timeout massimo della sessione per la configurazione di ' ZooKeeper '.           | INT    | 40000               |
| Zoo-cfg. AutoPurge. snapRetainCount                      | Conteggio degli snap-in per la configurazione automatica di ' ZooKeeper '.       | INT    | 3                   |
| Zoo-cfg. AutoPurge. purgeInterval                        | Intervallo di ripulitura per la configurazione automatica di ' ZooKeeper '.          | INT    | 0                   |
| Zookeeper-Java-ENV. JVMFLAGS                            | Flag JVM per l'ambiente Java in ' ZooKeeper '.            | string | -Xmx1G -Xms1G       |
| Zookeeper-log4j-Properties. Zookeeper. Console. Threshold | Soglia per la console Log4j in ' ZooKeeper '.               | string | INFO                |
| Zoo-cfg. Zookeeper. Request. timeout                      | Controlla il timeout della richiesta ' ZooKeeper ' in millisecondi. | INT    | 40000               |
| KMS-site. Hadoop. Security. KMS. Encrypted. Key. cache. size | Dimensioni della cache per la chiave crittografata nel servizio di gestione delle chiavi Hadoop. | INT  | 500 |
    

##  <a name="big-data-clusters-specific-default-gateway-settings"></a>Cluster di Big Data: impostazioni del gateway predefinite specifiche
Le impostazioni del gateway seguenti sono quelle con impostazioni predefinite specifiche del catalogo dati business, ma che possono essere configurate dall'utente. Non sono incluse le impostazioni gestite dal sistema. Le impostazioni del gateway possono essere configurate solo nell'ambito della **risorsa** .

| Nome dell'impostazione                                          | Descrizione                                            | Tipo   | Valore predefinito |
| --------------------------------------------- | ------------------------------------------------------ | ------ | ------------------- |
| Gateway-site. gateway. HttpClient. socketTimeout | Timeout del socket per il client HTTP nel gateway in (MS/s/m). | string | anni '90                 |
| Gateway-site. Sun. Security. krb5. debug          | Debug per la sicurezza Kerberos.                           | bool   | true                |
| Knox-ENV. KNOX_GATEWAY_MEM_OPTS                | Opzioni di memoria del gateway Knox.                           | string | -Xmx2g              |

## <a name="unsupported-spark-configurations"></a>Configurazioni di Spark non supportate

Le configurazioni `spark` seguenti non sono supportate e non possono essere modificate nel contesto del cluster Big Data.

| Category  | Sottocategoria               | File                       | Configurazioni non supportate                                              |
|-----------|----------------------------|----------------------------|-------------------------------------------------------------------------|
|           | yarn-site                  | yarn-site.xml              | yarn.log-aggregation-enable                                             |
|           |                            |                            | yarn.log.server.url                                                     |
|           |                            |                            | yarn.nodemanager.pmem-check-enabled                                     |
|           |                            |                            | yarn.nodemanager.vmem-check-enabled                                     |
|           |                            |                            | yarn.nodemanager.aux-services                                           |
|           |                            |                            | yarn.resourcemanager.address                                            |
|           |                            |                            | yarn.nodemanager.address                                                |
|           |                            |                            | yarn.client.failover-no-ha-proxy-provider                               |
|           |                            |                            | yarn.client.failover-proxy-provider                                     |
|           |                            |                            | yarn.http.policy                                                        |
|           |                            |                            | yarn.nodemanager.linux-container-executor.secure-mode.use-pool-user     |
|           |                            |                            | yarn.nodemanager.linux-container-executor.secure-mode.pool-user-prefix  |
|           |                            |                            | yarn.nodemanager.linux-container-executor.nonsecure-mode.local-user     |
|           |                            |                            | yarn.acl.enable                                                         |
|           |                            |                            | yarn.admin.acl                                                          |
|           |                            |                            | yarn.resourcemanager.hostname                                           |
|           |                            |                            | yarn.resourcemanager.principal                                          |
|           |                            |                            | yarn.resourcemanager.keytab                                             |
|           |                            |                            | yarn.resourcemanager.webapp.spnego-keytab-file                          |
|           |                            |                            | yarn.resourcemanager.webapp.spnego-principal                            |
|           |                            |                            | yarn.nodemanager.principal                                              |
|           |                            |                            | yarn.nodemanager.keytab                                                 |
|           |                            |                            | yarn.nodemanager.webapp.spnego-keytab-file                              |
|           |                            |                            | yarn.nodemanager.webapp.spnego-principal                                |
|           |                            |                            | yarn.resourcemanager.ha.enabled                                         |
|           |                            |                            | yarn.resourcemanager.cluster-id                                         |
|           |                            |                            | yarn.resourcemanager.zk-address                                         |
|           |                            |                            | yarn.resourcemanager.ha.rm-ids                                          |
|           |                            |                            | yarn.resourcemanager.hostname.*                                         |
|           | capacity-scheduler         | capacity-scheduler.xml     | yarn.scheduler.capacity.root.acl_submit_applications                    |
|           |                            |                            | yarn.scheduler.capacity.root.acl_administer_queue                       |
|           |                            |                            | yarn.scheduler.capacity.root.default.acl_application_max_priority       |
|           | yarn-env                   | yarn-env.sh                |                                                                         |
|           | spark-defaults-conf        | spark-defaults.conf        | spark.yarn.archive                                                      |
|           |                            |                            | spark.yarn.historyServer.address                                        |
|           |                            |                            | spark.eventLog.enabled                                                  |
|           |                            |                            | spark.eventLog.dir                                                      |
|           |                            |                            | spark.sql.warehouse.dir                                                 |
|           |                            |                            | spark.sql.hive.metastore.version                                        |
|           |                            |                            | spark.sql.hive.metastore.jars                                           |
|           |                            |                            | spark.extraListeners                                                    |
|           |                            |                            | spark.metrics.conf                                                      |
|           |                            |                            | spark.ssl.enabled                                                       |
|           |                            |                            | spark.authenticate                                                      |
|           |                            |                            | spark.network.crypto.enabled                                            |
|           |                            |                            | spark.ssl.keyStore                                                      |
|           |                            |                            | spark.ssl.keyStorePassword  
|           |                            |                            | spark.ui.enabled                                                        |
|           | spark-env                  | spark-env.sh               | SPARK_NO_DAEMONIZE                                                      |
|           |                            |                            | SPARK_DIST_CLASSPATH                                                    |
|           | spark-history-server-conf  | spark-history-server.conf  | spark.history.fs.logDirectory                                           |
|           |                            |                            | spark.ui.proxyBase                                                      |
|           |                            |                            | spark.history.fs.cleaner.enabled                                        |
|           |                            |                            | spark.ssl.enabled                                                       |
|           |                            |                            | spark.authenticate                                                      |
|           |                            |                            | spark.network.crypto.enabled                                            |
|           |                            |                            | spark.ssl.keyStore                                                      |
|           |                            |                            | spark.ssl.keyStorePassword                                              |
|           |                            |                            | spark.history.kerberos.enabled                                          |
|           |                            |                            | spark.history.kerberos.principal                                        |
|           |                            |                            | spark.history.kerberos.keytab                                           |
|           |                            |                            | spark.ui.filters                                                        |
|           |                            |                            | spark.acls.enable                                                       |
|           |                            |                            | spark.history.ui.acls.enable                                            |
|           |                            |                            | spark.history.ui.admin.acls                                             |
|           |                            |                            | spark.history.ui.admin.acls.groups                                      |
|           | livy-conf                  | livy.conf                  | livy.keystore                                                           |
|           |                            |                            | livy.keystore.password                                                  |
|           |                            |                            | livy.spark.master                                                       |
|           |                            |                            | livy.spark.deploy-mode                                                  |
|           |                            |                            | livy.rsc.jars                                                           |
|           |                            |                            | livy.repl.jars                                                          |
|           |                            |                            | livy.rsc.pyspark.archives                                               |
|           |                            |                            | livy.rsc.sparkr.package                                                 |
|           |                            |                            | livy.repl.enable-hive-context                                           |
|           |                            |                            | livy.superusers                                                         |
|           |                            |                            | livy.server.auth.type                                                   |
|           |                            |                            | livy.server.launch.kerberos.keytab                                      |
|           |                            |                            | livy.server.launch.kerberos.principal                                   |
|           |                            |                            | livy.server.auth.kerberos.principal                                     |
|           |                            |                            | livy.server.auth.kerberos.keytab                                        |
|           |                            |                            | livy.impersonation.enabled                                              |
|           |                            |                            | livy.server.access-control.enabled                                      |
|           |                            |                            | livy.server.access-control.*                                            |
|           | livy-env                   | livy-env.sh                |                                                                         |
|           | hive-site                  | hive-site.xml              | javax.jdo.option.ConnectionURL                                          |
|           |                            |                            | javax.jdo.option.ConnectionDriverName                                   |
|           |                            |                            | javax.jdo.option.ConnectionUserName                                     |
|           |                            |                            | javax.jdo.option.ConnectionPassword                                     |
|           |                            |                            | hive.metastore.uris                                                     |
|           |                            |                            | hive.metastore.pre.event.listeners                                      |
|           |                            |                            | hive.security.authorization.enabled                                     |
|           |                            |                            | hive.security.metastore.authenticator.manager                           |
|           |                            |                            | hive.security.metastore.authorization.manager                           |
|           |                            |                            | hive.metastore.use.SSL                                                  |
|           |                            |                            | hive.metastore.keystore.path                                            |
|           |                            |                            | hive.metastore.keystore.password                                        |
|           |                            |                            | hive.metastore.truststore.path                                          |
|           |                            |                            | hive.metastore.truststore.password                                      |
|           |                            |                            | hive.metastore.kerberos.keytab.file                                     |
|           |                            |                            | hive.metastore.kerberos.principal                                       |
|           |                            |                            | hive.metastore.sasl.enabled                                             |
|           |                            |                            | hive.metastore.execute.setugi                                           |
|           |                            |                            | hive.cluster.delegation.token.store.class                               |
|           | hive-env                   | hive-env.sh                | |

## <a name="unsupported-hdfs-configurations"></a>Configurazioni HDFS non supportate

Le configurazioni `hdfs` seguenti non sono supportate e non possono essere modificate nel contesto del cluster Big Data.

| Category  | Sottocategoria               | File                       | Configurazioni non supportate                                              |
|-----------|----------------------------|----------------------------|-------------------------------------------------------------------------|
|          | core-site                   | core-site.xml                 | fs.defaultFS                                          |
|          |                             |                               | ha.zookeeper.quorum                                   |
|          |                             |                               | hadoop.tmp.dir                                        |
|          |                             |                               | hadoop.rpc.protection                                 |
|          |                             |                               | hadoop.security.auth_to_local                         |
|          |                             |                               | hadoop.security.authentication                        |
|          |                             |                               | hadoop.security.authorization                         |
|          |                             |                               | hadoop.http.authentication.simple.anonymous.allowed   |
|          |                             |                               | hadoop.http.authentication.type                       |
|          |                             |                               | hadoop.http.authentication.kerberos.principal         |
|          |                             |                               | hadoop.http.authentication.kerberos.keytab            |
|          |                             |                               | hadoop.http.filter.initializers                       |
|          |                             |                               | hadoop.security.group.mapping.*                       |                               
|          |                             |                               | hadoop.security.key.provider.path                     |                               
|          | mapred-env                  | mapred-env.sh                 |                                                       |
|          | hdfs-site                   | hdfs-site.xml                 | dfs.namenode.name.dir                                 |
|          |                             |                               | dfs.datanode.data.dir                                 |
|          |                             |                               | dfs.namenode.acls.enabled                             |
|          |                             |                               | dfs.namenode.datanode.registration.ip-hostname-check  |
|          |                             |                               | dfs.client.retry.policy.enabled                       |
|          |                             |                               | dfs.permissions.enabled                               |
|          |                             |                               | dfs.nameservices                                      |
|          |                             |                               | dfs.ha.namenodes.nmnode-0                             |
|          |                             |                               | dfs.namenode.rpc-address.nmnode-0.*                   |
|          |                             |                               | dfs.namenode.shared.edits.dir                         |
|          |                             |                               | dfs.ha.automatic-failover.enabled                     |
|          |                             |                               | dfs.ha.fencing.methods                                |
|          |                             |                               | dfs.journalnode.edits.dir                             |
|          |                             |                               | dfs.client.failover.proxy.provider.nmnode-0           |
|          |                             |                               | dfs.namenode.http-address                             |
|          |                             |                               | dfs.namenode.httpS-address                            |
|          |                             |                               | dfs.http.policy                                       |
|          |                             |                               | dfs.encrypt.data.transfer                             |
|          |                             |                               | dfs.block.access.token.enable                         |
|          |                             |                               | dfs.data.transfer.protection                          |
|          |                             |                               | dfs.encrypt.data.transfer.cipher.suites               |
|          |                             |                               | dfs.https.port                                        |
|          |                             |                               | dfs.namenode.keytab.file                              |
|          |                             |                               | dfs.namenode.kerberos.principal                       |
|          |                             |                               | dfs.namenode.kerberos.internal.spnego.principal       |
|          |                             |                               | dfs.datanode.data.dir.perm                            |
|          |                             |                               | dfs.datanode.address                                  |
|          |                             |                               | dfs.datanode.http.address                             |
|          |                             |                               | dfs.datanode.ipc.address                              |
|          |                             |                               | dfs.datanode.https.address                            |
|          |                             |                               | dfs.datanode.keytab.file                              |
|          |                             |                               | dfs.datanode.kerberos.principal                       |
|          |                             |                               | dfs.journalnode.keytab.file                           |
|          |                             |                               | dfs.journalnode.kerberos.principal                    |
|          |                             |                               | dfs.journalnode.kerberos.internal.spnego.principal    |
|          |                             |                               | dfs.web.authentication.kerberos.keytab                |
|          |                             |                               | dfs.web.authentication.kerberos.principal             |
|          |                             |                               | dfs.webhdfs.enabled                                   |
|          |                             |                               | dfs.permissions.superusergroup                        |
|          | hdfs-env                    | hdfs-env.sh                   | HADOOP_HEAPSIZE_MAX                                   |
|          | zoo-cfg                     | zoo.cfg                       | secureClientPort                                      |
|          |                             |                               | clientPort                                            |
|          |                             |                               | dataDir                                               |
|          |                             |                               | dataLogDir                                            |
|          |                             |                               | 4lw.commands.whitelist                                |
|          | zookeeper-java-env          | java.env                      | ZK_LOG_DIR                                            |
|          |                             |                               | SERVER_JVMFLAGS                                       |
|          | zookeeper-log4j-properties  | log4j.properties (zookeeper)  | log4j.rootLogger                                      |
|          |                             |                               | log4j.appender.CONSOLE.*                              |

> [!NOTE]
> Questo articolo contiene il termine elenco elementi consentiti *, un termine che* Microsoft considera insensitive in questo contesto. Il termine viene visualizzato in questo articolo perché è attualmente incluso nel software. Quando il termine verrà rimosso dal software, verrà rimosso dall'articolo.

## <a name="unsupported-gateway-configurations"></a>Configurazioni `gateway` non supportate

Le configurazioni `gateway` seguenti non sono supportate e non possono essere modificate nel contesto del cluster Big Data.

| Category  | Sottocategoria               | File                       | Configurazioni non supportate                                              |
|-----------|----------------------------|----------------------------|-------------------------------------------------------------------------|
|          | gateway-site                | gateway-site.xml              | gateway.port                                          |
|          |                             |                               | gateway.path                                          |
|          |                             |                               | gateway.gateway.conf.dir                              |
|          |                             |                               | gateway.hadoop.kerberos.secured                       |
|          |                             |                               | java.security.krb5.conf                               |
|          |                             |                               | java.security.auth.login.config                       |
|          |                             |                               | gateway.websocket.feature.enabled                     |
|          |                             |                               | gateway.scope.cookies.feature.enabled                 |
|          |                             |                               | ssl.exclude.protocols                                 |
|          |                             |                               | ssl.include.ciphers                                   |

## <a name="next-steps"></a>Passaggi successivi

[Configurare SQL Server cluster di Big Data](configure-bdc-overview.md)
