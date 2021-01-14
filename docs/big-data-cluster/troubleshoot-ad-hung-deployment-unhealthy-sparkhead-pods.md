---
title: Blocco della distribuzione in modalità AD - Pod `sparkhead` non integri
titleSuffix: SQL Server Big Data Cluster
description: Risoluzione dei problemi relativi al blocco della distribuzione di un cluster Big Data di SQL Server in un dominio di Active Directory con pod `sparkhead` non integri.
author: macarv-ms
ms.author: macarv
ms.reviewer: mikeray
ms.date: 08/20/2020
ms.topic: how-to
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: ded5030adeff1b9ccccf3f6683d5dfb04e7b8a07
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091674"
---
# <a name="ad-mode-deployment-hangs---unhealthy-sparkhead-pods"></a>Blocco della distribuzione in modalità AD - Pod `sparkhead` non integri

La distribuzione in modalità Active Directory (AD) si blocca. Controllare i sintomi per determinare se il problema è dovuto a una voce di zona di ricerca inversa mancante per il controller di dominio nelle diverse reti dei nodi del cluster.

## <a name="symptom"></a>Sintomo

È stata avviata la distribuzione di BDC con la modalità AD, tuttavia la distribuzione è bloccata e non va avanti.

L'esempio seguente illustra i risultati della distribuzione in una shell Bash.

```output
Starting cluster deployment.
Waiting for cluster controller to start.
Waiting for cluster controller to start.
Waiting for cluster controller to start.
Waiting for cluster controller to start.
Waiting for cluster controller to start.
Cluster controller endpoint is available at bdc-control.corpnet.contoso.com:30080, 10.166.6.77:30080.
Waiting for control plane to be ready after 5 minutes.
Cluster control plane is ready.
Cluster is not ready after 15 minutes. Check controller logs for more details.
Data pool is ready.
Storage pool is ready.
Compute pool is ready.
Master pool is ready.
Cluster is not ready after 30 minutes. Check controller logs for more details.
...
...
```

Controllare i pod distribuiti correnti.

```bash
kubectl get pods -n mssql-cluster
```

I risultati indicano che tutti i pod sono stati distribuiti, ma la distribuzione non risulta completata con esito positivo.

```output
NAME              READY   STATUS    RESTARTS   AGE 
appproxy-c7f2l    2/2     Running   0          3d13h 
compute-0-0       3/3     Running   0          3d13h 
control-88dgt     3/3     Running   0          3d13h 
controldb-0       2/2     Running   0          3d13h 
controlwd-zzkxz   1/1     Running   0          3d13h 
data-0-0          3/3     Running   0          3d13h 
data-0-1          3/3     Running   0          3d13h 
dns-xkdhh         2/2     Running   0          3d13h 
gateway-0         2/2     Running   0          3d13h 
logsdb-0          1/1     Running   0          3d13h 
logsui-qz8qq      1/1     Running   0          3d13h 
master-0          4/4     Running   0          3d13h 
master-1          4/4     Running   0          3d13h 
master-2          4/4     Running   0          3d13h 
metricsdb-0       1/1     Running   0          3d13h 
metricsdc-xezf7   1/1     Running   0          3d13h 
metricsdc-qdjkh   1/1     Running   0          3d13h 
metricsui-mr34w   1/1     Running   0          3d13h 
mgmtproxy-kz5gg   2/2     Running   0          3d13h 
nmnode-0-0        2/2     Running   1          3d13h 
nmnode-0-1        2/2     Running   0          3d13h 
operator-42ffv    1/1     Running   0          3d13h 
sparkhead-0       4/4     Running   0          3d13h 
sparkhead-1       4/4     Running   0          3d13h 
storage-0-0       4/4     Running   0          3d13h 
storage-0-1       4/4     Running   0          3d13h 
storage-0-2       4/4     Running   0          3d13h 
zookeeper-0       2/2     Running   0          3d13h 
zookeeper-1       2/2     Running   0          3d13h 
zookeeper-2       2/2     Running   0          3d13h 
```

Controllare l'integrità dei servizi HDFS e Spark. Individuare gli errori dei pod `sparkhead`.

## <a name="check-the-hdfs-and-spark-services"></a>Controllare i servizi HDFS e Spark 

Da Azure Data Studio (ADS), connettersi al controller e visualizzare il dashboard dei cluster Big Data. Verificare se entrambi i servizi HDFS e Spark presentano pod `sparkhead` non integri.

![Pod sparkhead non integri nei servizi HDFS Spark](./media/troubleshoot-ad-hung-deployment-unhealthy-sparkhead-pods/hdfs_spark_unhealthy_sparkhead_pods.png)

Estrarre i log e individuare gli errori.

`\mssql-cluster\control-<identifier>\controller\control-<identifier>-controller-stdout.log`.

> [!TIP]
> Esistono diversi modi per raccogliere i log. Anziché copiare i log con [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)], è possibile usare un notebook in Azure Data Studio.
> In Azure Data Studio connettersi al cluster Kubernetes ed eseguire un notebook appropriato per la risoluzione dei problemi. Di seguito sono elencati alcuni esempi di notebook.
>
> - TSG027 - Osservare la distribuzione del cluster
> - TSG061 - Ottenere la parte finale di tutti i log dei contenitori per i pod nello spazio dei nomi BDC
> - TSG001 - Eseguire `azdata copy-logs`
>
  
## <a name="inspect-the-logs"></a>Esaminare i log

Individuare il log. L'esempio seguente fa riferimento a un log di distribuzione del controller.

`<folderOfDebugCopyLog>\debuglogs-mssql-cluster-YYYYMMDD-HHMMSS\<namespace>\control-<identifier>\controller\control-<identifier>-controller-stdout.log`

```output
StatefulSet sparkhead is not healthy: 
{{Pod sparkhead-0 is unhealthy: 
{Container hadoop-yarn-jobhistory is unhealthy: 
{Found error properties: 
{Property: jobhistoryserver.readiness, Details: 'Health module returned error state. error: Head https://sparkhead-0.corpnet.contoso.com:19888/ws/v1/history: dial tcp 10.244.2.33:19888: connect: connection refused'}}} 
{Container hadoop-livy-sparkhistory is unhealthy: 
{Found error properties: 
{Property: sparkhistory.readiness, Details: 'Health module returned error state. error: Head https://sparkhead-0.corpnet.contoso.com:18480: dial tcp 10.244.2.33:18480: connect: connection refused'}}}, 
{Container hadoop-hivemetastore is unhealthy: 
{Found error properties: 
{Property: hivemetastorehttp.readiness, Details: 'Health module returned error state. error: Post https://sparkhead-0.corpnet.contoso.com:9084/api/hms: dial tcp 10.244.2.33:9084: connect: connection refused'}}}}}, 
  
{{Pod sparkhead-1 is unhealthy: 
{Container hadoop-yarn-jobhistory is unhealthy: 
{Found error properties: 
{Property: jobhistoryserver.readiness, Details: 'Health module returned error state. error: Head https://sparkhead-1.corpnet.contoso.com:19888/ws/v1/history: dial tcp 10.244.1.24:19888: connect: connection refused'}}}, 
{Container hadoop-livy-sparkhistory is unhealthy: 
{Found error properties: 
{Property: sparkhistory.readiness, Details: 'Health module returned error state. error: Head https://sparkhead-1.corpnet.contoso.com:18480: dial tcp 10.244.1.24:18480: connect: connection refused'}}}, 
{Container hadoop-hivemetastore is unhealthy: 
{Found error properties: 
{Property: hivemetastorehttp.readiness, Details: 'Health module returned error state. error: Post https://sparkhead-1.corpnet.contoso.com:9084/api/hms: dial tcp 10.244.1.24:9084: connect: connection refused'}}}}} 
```

Esaminare i pod `sparkhead` facendo attenzione ai log dei contenitori. Questo esempio esamina `sparkhead-0`.

```output
sparkhead-0\hadoop-hivemetastore\supervisor\log\hivemetastorehttp-stderr---supervisor-pZ1gdb 
  
YYYY-MM-DD HH:MM:SS.ms INFO retry.RetryInvocationHandler: org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.ipc.StandbyException): Operation category READ is not supported in state standby. Visit https://s.apache.org/sbnn-error 
at org.apache.hadoop.hdfs.server.namenode.ha.StandbyState.checkOperation(StandbyState.java:98) 
at org.apache.hadoop.hdfs.server.namenode.NameNode$NameNodeHAContext.checkOperation(NameNode.java:1998) 
at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.checkOperation(FSNamesystem.java:1502) 
at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.getFileInfo(FSNamesystem.java:3227) 
at org.apache.hadoop.hdfs.server.namenode.NameNodeRpcServer.getFileInfo(NameNodeRpcServer.java:1158) 
at org.apache.hadoop.hdfs.protocolPB.ClientNamenodeProtocolServerSideTranslatorPB.getFileInfo(ClientNamenodeProtocolServerSideTranslatorPB.java:983) 
at org.apache.hadoop.hdfs.protocol.proto.ClientNamenodeProtocolProtos$ClientNamenodeProtocol$2.callBlockingMethod(ClientNamenodeProtocolProtos.java) 
at org.apache.hadoop.ipc.ProtobufRpcEngine$Server$ProtoBufRpcInvoker.call(ProtobufRpcEngine.java:527) 
at org.apache.hadoop.ipc.RPC$Server.call(RPC.java:1036) 
at org.apache.hadoop.ipc.Server$RpcCall.run(Server.java:978) 
at org.apache.hadoop.ipc.Server$RpcCall.run(Server.java:906) 
at java.security.AccessController.doPrivileged(Native Method) 
at javax.security.auth.Subject.doAs(Subject.java:422) 
at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1729) 
at org.apache.hadoop.ipc.Server$Handler.run(Server.java:2876) 
, while invoking ClientNamenodeProtocolTranslatorPB.getFileInfo over nmnode-0-0.corpnet.contoso.com/10.244.2.36:9000 after 8 failover attempts. Trying to failover after sleeping for 13518ms. 
  
sparkhead-0\hadoop-yarn-jobhistory\supervisor\log\jobhistoryserver-stderr---supervisor-GvebR8 
  
YYYY-MM-DD HH:MM:SS.ms INFO retry.RetryInvocationHandler: org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.ipc.StandbyException): Operation category READ is not supported in state standby. Visit https://s.apache.org/sbnn-error 
at org.apache.hadoop.hdfs.server.namenode.ha.StandbyState.checkOperation(StandbyState.java:98) 
at org.apache.hadoop.hdfs.server.namenode.NameNode$NameNodeHAContext.checkOperation(NameNode.java:1998) 
at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.checkOperation(FSNamesystem.java:1502) 
at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.getFileInfo(FSNamesystem.java:3227) 
at org.apache.hadoop.hdfs.server.namenode.NameNodeRpcServer.getFileInfo(NameNodeRpcServer.java:1158) 
at org.apache.hadoop.hdfs.protocolPB.ClientNamenodeProtocolServerSideTranslatorPB.getFileInfo(ClientNamenodeProtocolServerSideTranslatorPB.java:983) 
at org.apache.hadoop.hdfs.protocol.proto.ClientNamenodeProtocolProtos$ClientNamenodeProtocol$2.callBlockingMethod(ClientNamenodeProtocolProtos.java) 
at org.apache.hadoop.ipc.ProtobufRpcEngine$Server$ProtoBufRpcInvoker.call(ProtobufRpcEngine.java:527) 
at org.apache.hadoop.ipc.RPC$Server.call(RPC.java:1036) 
at org.apache.hadoop.ipc.Server$RpcCall.run(Server.java:978) 
at org.apache.hadoop.ipc.Server$RpcCall.run(Server.java:906) 
at java.security.AccessController.doPrivileged(Native Method) 
at javax.security.auth.Subject.doAs(Subject.java:422) 
at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1729) 
at org.apache.hadoop.ipc.Server$Handler.run(Server.java:2876) 
, while invoking ClientNamenodeProtocolTranslatorPB.getFileInfo over nmnode-0-1.corpnet.contoso.com/10.244.1.30:9000 after 5 failover attempts. Trying to failover after sleeping for 11416ms. 
  
sparkhead-0\hadoop-livy-sparkhistory\supervisor\log\livy-stderr---supervisor-XiHB1w 
  
YYYY-MM-DD HH:MM:SS.ms INFO retry.RetryInvocationHandler: org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.ipc.StandbyException): Operation category READ is not supported in state standby. Visit https://s.apache.org/sbnn-error 
at org.apache.hadoop.hdfs.server.namenode.ha.StandbyState.checkOperation(StandbyState.java:98) 
at org.apache.hadoop.hdfs.server.namenode.NameNode$NameNodeHAContext.checkOperation(NameNode.java:1998) 
at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.checkOperation(FSNamesystem.java:1502) 
at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.getFileInfo(FSNamesystem.java:3227) 
at org.apache.hadoop.hdfs.server.namenode.NameNodeRpcServer.getFileInfo(NameNodeRpcServer.java:1158) 
at org.apache.hadoop.hdfs.protocolPB.ClientNamenodeProtocolServerSideTranslatorPB.getFileInfo(ClientNamenodeProtocolServerSideTranslatorPB.java:983) 
at org.apache.hadoop.hdfs.protocol.proto.ClientNamenodeProtocolProtos$ClientNamenodeProtocol$2.callBlockingMethod(ClientNamenodeProtocolProtos.java) 
at org.apache.hadoop.ipc.ProtobufRpcEngine$Server$ProtoBufRpcInvoker.call(ProtobufRpcEngine.java:527) 
at org.apache.hadoop.ipc.RPC$Server.call(RPC.java:1036) 
at org.apache.hadoop.ipc.Server$RpcCall.run(Server.java:978) 
at org.apache.hadoop.ipc.Server$RpcCall.run(Server.java:906) 
at java.security.AccessController.doPrivileged(Native Method) 
at javax.security.auth.Subject.doAs(Subject.java:422) 
at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1729) 
at org.apache.hadoop.ipc.Server$Handler.run(Server.java:2876) 
, while invoking ClientNamenodeProtocolTranslatorPB.getFileInfo over nmnode-0-1.corpnet.contoso.com/10.244.1.30:9000 after 1 failover attempts. Trying to failover after sleeping for 1401ms. 
```

## <a name="cause"></a>Causa

La voce della zona di ricerca inversa per il controller di dominio nel server DNS del controller di dominio per la rete Kubernetes è mancante. Per questo esempio, la voce mancante era `cni0 10.244`. I contenitori dei pod `sparkhead` stavano provando a usare l'indirizzo IP 10.244.1.30:9000 per raggiungere nnnode-0-1, ma il DNS non era in grado di risolverlo.

:::image type="content" source="media/troubleshoot-ad-hung-deployment-unhealthy-sparkhead-pods/missing_reverse_lookup_zone_entry_for_domain_controller.png" alt-text="Voce della zona di ricerca inversa mancante per il controller di dominio":::

## <a name="resolution"></a>Soluzione

Aggiungere la voce DNS inversa mancante (record PTR) per la zona indicata nei log. Per questo esempio è stata aggiunta la voce 244.10.

:::image type="content" source="media/troubleshoot-ad-hung-deployment-unhealthy-sparkhead-pods/missing_reverse_lookup_zone_entry_for_domain_controller_add.png" alt-text="Aggiungere la voce della zona di ricerca inversa mancante per il controller di dominio":::

> [!NOTE]
> Verificare che sia presente una voce DNS inversa (record PTR) per il controller di dominio stesso, registrata nel server DNS per tutte le diverse reti dei nodi del cluster.

## <a name="next-steps"></a>Passaggi successivi

[Verificare la voce DNS inversa (record PTR) per il controller di dominio](active-directory-deploy.md#verify-reverse-dns-entry-for-domain-controller).
