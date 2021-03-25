---
title: Configurazione di PolyBase e sicurezza per Hadoop | Microsoft Docs
description: Usare queste impostazioni per la connettività PolyBase a Hadoop, tra cui Hadoop.RPC.Protection, i file XML di esempio per il cluster CDH 5.X e la configurazione Kerberos.
ms.date: 04/23/2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016 '
ms.openlocfilehash: 140c5d20c18fc7dae3710ae91c37f5ee1bb8d242
ms.sourcegitcommit: 17f05be5c08cf9a503a72b739da5ad8be15baea5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/25/2021
ms.locfileid: "105103878"
---
# <a name="polybase-configuration-and-security-for-hadoop"></a>Configurazione di PolyBase e sicurezza per Hadoop

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

Questo articolo include informazioni di riferimento per varie impostazioni di configurazione che influiscono sulla connettività di PolyBase con Hadoop. Per una procedura dettagliata su come usare PolyBase con Hadoop, vedere [Configurare PolyBase per l'accesso a dati esterni in Hadoop](polybase-configure-hadoop.md).

## <a name="hadooprpcprotection-setting"></a><a id="rpcprotection"></a> Impostazione Hadoop.RPC.Protection

Un modo comune per proteggere la comunicazione in un cluster Hadoop è modificare la configurazione di hadoop.rpc.protection impostandola su "Privacy" o "Integrity". Per impostazione predefinita, PolyBase presuppone che la configurazione sia impostata su "Authenticate". Per eseguire l'override di questa impostazione predefinita, aggiungere la proprietà seguente al file core-site.xml. La modifica di questa configurazione consente il trasferimento sicuro dei dati tra i nodi Hadoop nonché la connessione TLS a SQL Server.

```xml
<!-- RPC Encryption information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG -->
   <property>
     <name>hadoop.rpc.protection</name>
     <value></value>
   </property> 
```

Per usare 'Privacy' o 'Integrity' per hadoop.rpc.protection, è necessario usare almeno SQL Server 2016 SP1 CU7, SQL Server 2016 SP2 o SQL Server 2017 CU3.

## <a name="example-xml-files-for-cdh-5x-cluster"></a>File XML di esempio per cluster CDH 5.X

File yarn-site.xml con i parametri di configurazione yarn.application.classpath e mapreduce.application.classpath.

```xml
<?xml version="1.0" encoding="utf-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!-- Put site-specific property overrides in this file. -->
 <configuration>
   <property>
      <name>yarn.resourcemanager.connect.max-wait.ms</name>
      <value>40000</value>
   </property>
   <property>
      <name>yarn.resourcemanager.connect.retry-interval.ms</name>
      <value>30000</value>
   </property>
<!-- Applications' Configuration-->
   <property>
     <description>CLASSPATH for YARN applications. A comma-separated list of CLASSPATH entries</description>
      <!-- Please set this value to the correct yarn.application.classpath that matches your server side configuration -->
      <!-- For example: $HADOOP_CONF_DIR,$HADOOP_COMMON_HOME/share/hadoop/common/*,$HADOOP_COMMON_HOME/share/hadoop/common/lib/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/lib/*,$HADOOP_YARN_HOME/share/hadoop/yarn/*,$HADOOP_YARN_HOME/share/hadoop/yarn/lib/* -->
      <name>yarn.application.classpath</name>
      <value>$HADOOP_CLIENT_CONF_DIR,$HADOOP_CONF_DIR,$HADOOP_COMMON_HOME/*,$HADOOP_COMMON_HOME/lib/*,$HADOOP_HDFS_HOME/*,$HADOOP_HDFS_HOME/lib/*,$HADOOP_YARN_HOME/*,$HADOOP_YARN_HOME/lib/,$HADOOP_MAPRED_HOME/*,$HADOOP_MAPRED_HOME/lib/*,$MR2_CLASSPATH*</value>
   </property>

<!-- kerberos security information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG
   <property>
      <name>yarn.resourcemanager.principal</name>
      <value></value>
   </property>
-->
</configuration>
```

Se si sceglie di suddividere le due impostazioni di configurazione nei file mapred-site.xml e yarn-site.xml, i file saranno come indicato di seguito:

**yarn-site.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!-- Put site-specific property overrides in this file. -->
 <configuration>
   <property>
      <name>yarn.resourcemanager.connect.max-wait.ms</name>
      <value>40000</value>
   </property>
   <property>
      <name>yarn.resourcemanager.connect.retry-interval.ms</name>
      <value>30000</value>
   </property>
<!-- Applications' Configuration-->
   <property>
     <description>CLASSPATH for YARN applications. A comma-separated list of CLASSPATH entries</description>
      <!-- Please set this value to the correct yarn.application.classpath that matches your server side configuration -->
      <!-- For example: $HADOOP_CONF_DIR,$HADOOP_COMMON_HOME/share/hadoop/common/*,$HADOOP_COMMON_HOME/share/hadoop/common/lib/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/lib/*,$HADOOP_YARN_HOME/share/hadoop/yarn/*,$HADOOP_YARN_HOME/share/hadoop/yarn/lib/* -->
      <name>yarn.application.classpath</name>
      <value>$HADOOP_CLIENT_CONF_DIR,$HADOOP_CONF_DIR,$HADOOP_COMMON_HOME/*,$HADOOP_COMMON_HOME/lib/*,$HADOOP_HDFS_HOME/*,$HADOOP_HDFS_HOME/lib/*,$HADOOP_YARN_HOME/*,$HADOOP_YARN_HOME/lib/*</value>
   </property>

<!-- kerberos security information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG
   <property>
      <name>yarn.resourcemanager.principal</name>
      <value></value>
   </property>
-->
</configuration>
```

**mapred-site.xml**

Si noti che è stata aggiunta la proprietà mapreduce.application.classpath. In CDH 5.x i valori di configurazione usano la stessa convenzione di denominazione di Ambari.

```xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!-- Put site-specific property overrides in this file. -->
<configuration xmlns:xi="http://www.w3.org/2001/XInclude">
   <property>
     <name>mapred.min.split.size</name>
       <value>1073741824</value>
   </property>
   <property>
     <name>mapreduce.app-submission.cross-platform</name>
     <value>true</value>
   </property>
<property>
     <name>mapreduce.application.classpath</name>
     <value>$HADOOP_MAPRED_HOME/*,$HADOOP_MAPRED_HOME/lib/*,$MR2_CLASSPATH</value>
   </property>


<!--kerberos security information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG
   <property>
     <name>mapreduce.jobhistory.principal</name>
     <value></value>
   </property>
   <property>
     <name>mapreduce.jobhistory.address</name>
     <value></value>
   </property>
-->
</configuration>
```

## <a name="kerberos-configuration"></a>Configurazione di Kerberos  

Si noti che se PolyBase esegue l'autenticazione in un cluster protetto con Kerberos, il parametro hadoop.rpc.protection deve essere impostato su 'Authenticate' per impostazione predefinita. La comunicazione dei dati tra i nodi Hadoop rimane non crittografata. Per usare le impostazioni 'Privacy' o 'Integrity' per hadoop.rpc.protection, aggiornare il file core-site.xml sul server PolyBase. Per altre informazioni, vedere la sezione precedente [Connessione a un cluster Hadoop con l'impostazione hadoop.rpc.protection](#rpcprotection).

Per connettersi a un cluster Hadoop protetto con Kerberos tramite MIT KDC:

1. Trovare la directory di configurazione Hadoop nel percorso di installazione di SQL Server. In genere il percorso è:  

   ```  
   C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\PolyBase\Hadoop\conf  
   ```  

2. Trovare il valore di configurazione lato Hadoop delle chiavi di configurazione elencate nella tabella. Nel computer Hadoop trovare i file nella directory di configurazione Hadoop.  
   
3. Copiare i valori di configurazione nella proprietà del valore nei file corrispondenti sul computer SQL Server.  
   
   |**#**|**File di configurazione**|**Chiave di configurazione**|**Azione**|  
   |------------|----------------|---------------------|----------|   
   |1|core-site.xml|polybase.kerberos.kdchost|Specificare il nome host KDC. Ad esempio: kerberos.area-di-autenticazione.com.|  
   |2|core-site.xml|polybase.kerberos.realm|Specificare l'area di autenticazione Kerberos. Ad esempio: AREA-DI-AUTENTICAZIONE.COM <br><br>**Nota sulla configurazione**: il nome dell'area di autenticazione deve essere scritto in maiuscolo.|  
   |3|core-site.xml|hadoop.security.authentication|Trovare la configurazione lato Hadoop e copiarla nel computer SQL Server. Ad esempio: KERBEROS<br></br>**Nota sulla sicurezza:** è necessario scrivere KERBEROS in maiuscolo. Se si usano lettere minuscole, potrebbe non essere disponibile.|   
   |4|hdfs-site.xml|dfs.namenode.kerberos.principal|Trovare la configurazione lato Hadoop e copiarla nel computer SQL Server. Ad esempio: hdfs/_HOST@YOUR-REALM.COM|  
   |5|mapred-site.xml|mapreduce.jobhistory.principal|Trovare la configurazione lato Hadoop e copiarla nel computer SQL Server. Ad esempio: mapred/_HOST@YOUR-REALM.COM|  
   |6|mapred-site.xml|mapreduce.jobhistory.address|Trovare la configurazione lato Hadoop e copiarla nel computer SQL Server. Ad esempio: 10.193.26.174:10020|  
   |7|yarn-site.xml yarn.|yarn.resourcemanager.principal|Trovare la configurazione lato Hadoop e copiarla nel computer SQL Server. Ad esempio: yarn/_HOST@YOUR-REALM.COM|  

4. Creare un oggetto credenziali con ambito database per specificare le informazioni di autenticazione per ogni utente di Hadoop. Vedere [Oggetti T-SQL PolyBase](../../relational-databases/polybase/polybase-t-sql-objects.md).  

## <a name="next-steps"></a>Passaggi successivi  

Per altre informazioni, vedere gli articoli seguenti:

[Configurare PolyBase per l'accesso a dati esterni in Hadoop](polybase-configure-hadoop.md)
[Panoramica di PolyBase](../../relational-databases/polybase/polybase-guide.md)
