---
title: SQL Server configurazione dei cluster di Big Data CU9
titleSuffix: SQL Server big data clusters
description: Configurazione di Big Data cluster CU9 pre-installazione
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b00ed57288d19f08555a00eec8c9e62edc0f8cf6
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343970"
---
# <a name="configure-a-sql-server-big-data-cluster---pre-cu9-release"></a>Configurare un cluster SQL Server Big Data-versione pre-CU9

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

> [!NOTE]
> Prima della versione di CU9 e del supporto per i cluster abilitati per la configurazione, i cluster di Big Data potevano essere configurati solo in fase di distribuzione, fatta eccezione per l'istanza di SQL Server Master, che può essere configurata dopo la distribuzione solo usando MSSQL-conf. Le istruzioni per configurare un CU9 e una versione successiva di BDC sono disponibili [qui](configure-bdc-overview.md).


Nelle versioni BDC CU8 e versioni precedenti, è possibile configurare le impostazioni di integrazione applicativa dei dati in fase di distribuzione tramite il file di distribuzione `bdc.json` . L'istanza master di SQL Server può essere configurata post-distribuzione solo usando MSSQL-conf.

## <a name="configuration-scopes"></a>Ambiti di configurazione
La configurazione di cluster di Big data pre-CU9 ha due livelli `service` di ambito:, e `resource` . Anche la gerarchia delle impostazioni segue in questo ordine, dal più alto al più basso. I componenti BDC accetteranno il valore dell'impostazione definito nell'ambito più basso. Se l'impostazione non è definita in un ambito specificato, erediterà il valore dall'ambito padre superiore.

Ad esempio, è possibile definire il numero predefinito di core che il driver Spark userà nel pool di archiviazione e nelle `Sparkhead` risorse. Questa operazione può essere eseguita in due modi:

* Specificare un valore di core predefinito nell'ambito del servizio `Spark` 
* Specificare un valore di core predefinito nell'ambito della risorsa `storage-0` e `sparkhead`

Nel primo scenario tutte le risorse con ambito inferiore del servizio Spark (pool di archiviazione e `Sparkhead` ) *erediteranno* il numero predefinito di core dal valore predefinito del servizio Spark.

Nel secondo scenario ogni risorsa userà il valore definito nel rispettivo ambito.

Se il numero predefinito di core viene configurato sia nell'ambito del servizio sia in quello delle risorse, il valore con ambito risorsa eseguirà l'override del valore con ambito servizio, poiché si tratta dell'ambito **configurato dell'utente** più basso per l'impostazione specificata.

Per informazioni specifiche sulla configurazione, vedere gli articoli appropriati:

## <a name="configure-the-sql-server-master-instance"></a>Configurare l'istanza master di SQL Server
Configurare l'istanza master di [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)].

Non è possibile configurare le impostazioni di configurazione del server per l'istanza master di SQL Server in fase di distribuzione. Questo articolo descrive una soluzione alternativa temporanea per configurare impostazioni quali l'edizione di SQL Server, l'abilitazione o la disabilitazione di SQL Server Agent, l'abilitazione di flag di traccia specifici o l'abilitazione o la disabilitazione del feedback dei clienti.

Per cambiare una qualsiasi di queste impostazioni, eseguire la procedura seguente:

1. Creare un file `mssql-custom.conf` personalizzato che includa le impostazioni interessate. L'esempio seguente abilita SQL Server Agent e la telemetria, imposta un PID per l'edizione Enterprise e abilita il flag di traccia 1204.

   ```
   [sqlagent]
   enabled=true
   
   [telemetry]
   customerfeedback=true
   userRequestedLocalAuditDirectory = /tmp/audit

   [DEFAULT]
   pid = Enterprise

   [traceflag]
   traceflag0 = 1204
   ```

1. Copiare il file `mssql-custom.conf` in `/var/opt/mssql` nel contenitore `mssql-server` nel pod `master-0`. Sostituire `<namespaceName>` con il nome del cluster Big Data.

   ```bash
   kubectl cp mssql-custom.conf master-0:/var/opt/mssql/mssql-custom.conf -c mssql-server -n <namespaceName>
   ```

1. Riavviare l'istanza di SQL Server.  Sostituire `<namespaceName>` con il nome del cluster Big Data.

   ```bash
   kubectl exec -it master-0  -c mssql-server -n <namespaceName> -- /bin/bash
   supervisorctl restart mssql-server
   exit
   ```

> [!IMPORTANT]
> Se l'istanza master di SQL Server si trova in una configurazione di gruppi di disponibilità, copiare il file di `mssql-custom.conf` in tutti i pod `master`. Si noti che ogni riavvio provocherà un failover. È quindi necessario assicurarsi di pianificare l'esecuzione di questa attività in periodi di inattività.

### <a name="known-limitations"></a>Limitazioni note

- La procedura precedente richiede autorizzazioni di amministratore del cluster Kubernetes
- Non è possibile modificare le regole di confronto del server per l'istanza master di SQL Server del cluster Big Data dopo la distribuzione.

## <a name="configure-apache-spark-and-apache-hadoop"></a>Configurare Apache Spark e Apache Hadoop
Per configurare Apache Spark e Apache Hadoop nei cluster Big Data, è necessario modificare il profilo del cluster al momento della distribuzione.

Un cluster Big Data ha quattro categorie di configurazione: 

- `sql` 
- `hdfs` 
- `spark` 
- `gateway` 

`sql`, `hdfs`, `spark`, `sql` sono servizi. Ogni servizio è mappato alla categoria di configurazione con lo stesso nome. Tutte le configurazioni del gateway sono associate alla categoria `gateway`. 

Ad esempio, tutte le configurazioni nel servizio `hdfs` appartengono alla categoria `hdfs`. Si noti che tutte le configurazioni di Hadoop (core-site), HDFS e Zookeeper appartengono alla categoria `hdfs`. Tutte le configurazioni di Livy, Spark, Yarn, Hive e Metastore appartengono alla categoria `spark`. 

[Configurazioni supportate](reference-config-spark-hadoop.md#supported-configurations) elenca le proprietà di Apache Spark e Hadoop che è possibile configurare quando si distribuisce un cluster Big Data di SQL Server.

Le sezioni seguenti elencano le proprietà **non è possibile** modificare in un cluster:

- [Configurazioni `spark` non supportate](reference-config-spark-hadoop.md#unsupported-spark-configurations)
- [Configurazioni `hdfs` non supportate](reference-config-spark-hadoop.md#unsupported-hdfs-configurations)
- [Configurazioni `gateway` non supportate](reference-config-spark-hadoop.md#unsupported-gateway-configurations)

## <a name="next-steps"></a>Passaggi successivi

[Configurare un cluster Big Data di SQL Server](configure-bdc-overview.md)