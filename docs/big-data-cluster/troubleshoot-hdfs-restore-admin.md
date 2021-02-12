---
title: Ripristinare le autorizzazioni per HDFS
titleSuffix: SQL Server Big Data Cluster
description: Ripristinare i diritti di amministratore di HDFS.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
ms.date: 04/21/2020
ms.topic: how-to
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d914b2006656162b233351815e238f282d73e913
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045531"
---
# <a name="restore-hdfs-permissions"></a>Ripristinare le autorizzazioni per HDFS

Le modifiche degli elenchi di controllo di accesso (ACL) di HDFS potrebbero aver modificato le cartelle `/system` e `/tmp` in HDFS. La causa più probabile della modifica degli ACL è la manipolazione manuale degli ACL delle cartelle da parte di un utente. La modifica diretta delle autorizzazioni nella cartella /system e nella cartella /tmp/logs non è supportata.

## <a name="symptom"></a>Sintomo

Un processo Spark viene inviato tramite ADS e ha esito negativo con errore di inizializzazione SparkContext e AccessControlException.

```
583 ERROR spark.SparkContext: Error initializing SparkContext.
org.apache.hadoop.security.AccessControlException: Permission denied: user=<UserAccount>, access=WRITE, inode="/system/spark-events":sph:BDCAdmin:drwxr-xr-x
```

L'interfaccia utente di Yarn mostra l'ID applicazione con stato KILLED.

Si verifica un errore anche quando si tenta di scrivere nella cartella come utente di dominio. È possibile eseguire il test con l'esempio seguente:

```bash
kinit <UserAccount>
hdfs dfs -touch /system/spark-events/test
hdfs dfs -rm /system/spark-events/test
```

## <a name="cause"></a>Causa

Gli ACL di HDFS sono stati modificati per il gruppo di sicurezza del dominio utente BDC. Le modifiche possibili includono gli ACL per le cartelle /system e /tmp. Le modifiche di queste cartelle non sono supportate.

Verificare l'effetto nei log Livy:

```
INFO utils.LineBufferedStream: YYYY-MM-DD-HH:MM:SS,858 INFO yarn.Client: Application report for application_1580771254352_0041 (state: ACCEPTED)
...
WARN rsc.RSCClient: Client RPC channel closed unexpectedly.
INFO interactive.InteractiveSession: Failed to ping RSC driver for session <ID>. Killing application
```

L'interfaccia utente di YARN mostra le applicazioni con stato KILLED per l'ID applicazione.

Per individuare la causa radice della chiusura della connessione RPC, controllare il registro applicazioni YARN per l'app corrispondente all'applicazione. Nell'esempio precedente viene fatto riferimento a `application_1580771254352_0041`. Usare `kubectl` per connettersi al pod sparkhead-0 ed eseguire il comando seguente:

Il comando seguente esegue una query nel log YARN per l'applicazione.

```bash
yarn logs -applicationId application_1580771254352_0041
```

Nei risultati seguenti viene negata l'autorizzazione per l'utente. 

```
YYYY-MM-DD-HH:MM:SS,583 ERROR spark.SparkContext: Error initializing SparkContext.
org.apache.hadoop.security.AccessControlException: Permission denied: user=user1, access=WRITE, inode="/system/spark-events":sph:BDCAdmin:drwxr-xr-x
```

La causa potrebbe essere che l'utente BDC è stato aggiunto in modo ricorsivo alla cartella radice di HDFS. Questa operazione può avere avuto effetto sulle autorizzazioni predefinite.

## <a name="resolution"></a>Risoluzione

Ripristinare le autorizzazioni con lo script seguente: Usare `kinit` con admin:

```bash
hdfs dfs -chmod 733 /system/spark-events
hdfs dfs -setfacl --set default:user:sph:rwx,default:other::--- /system/spark-events
hdfs dfs -setfacl --set default:user:app-setup:r-x,default:other::--- /system/appdeploy
hadoop fs -chmod 733 /tmp/logs
hdfs dfs -setfacl --set default:user:yarn:rwx,default:other::--- /tmp/logs
```
