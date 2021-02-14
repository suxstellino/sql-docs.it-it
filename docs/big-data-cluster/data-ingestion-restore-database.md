---
title: Ripristinare un database
titleSuffix: SQL Server big data clusters
description: Questo articolo illustra come ripristinare un database nell'istanza master di un cluster Big Data di SQL Server 2019.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 08/21/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3082c970c648fddf67b635ba7cbae5b19bfc0170
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049091"
---
# <a name="restore-a-database-into-the-sql-server-big-data-cluster-master-instance"></a>Ripristinare un database nell'istanza master di un cluster Big Data di SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questo articolo descrive come ripristinare un database esistente nell'istanza master di un [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]. Il metodo consigliato prevede l'uso di un approccio di tipo: backup, copia e ripristino.

## <a name="backup-your-existing-database"></a>Eseguire il backup di un database esistente

Come prima operazione, eseguire il backup di un database SQL Server esistente da SQL Server in Windows o in Linux. Usare tecniche di backup standard con Transact-SQL o con uno strumento come SQL Server Management Studio (SSMS).

Questo articolo descrive come ripristinare il database AdventureWorks, ma è possibile usare qualsiasi backup di database. 

> [!TIP]
> Scaricare il [backup di AdventureWorks](../samples/adventureworks-install-configure.md).

## <a name="copy-the-backup-file"></a>Copiare il file di backup

Copiare il file di backup nel contenitore SQL Server disponibile nel pod dell'istanza master del cluster Kubernetes.

```bash
kubectl cp <path to .bak file> master-0:/var/tmp/<.bak filename> -c mssql-server -n <name of your big data cluster>
```

Esempio:

```bash
kubectl cp ~/Downloads/AdventureWorks2016CTP3.bak master-0:/var/tmp/AdventureWorks2016CTP3.bak -c mssql-server -n clustertest
```

Verificare quindi che il file di backup sia stato copiato nel contenitore pod.

```bash
kubectl exec -it master-0 -n <name of your big data cluster> -c mssql-server -- bin/bash
cd /var/
ls /tmp
exit
```

Esempio:

```bash
kubectl exec -it master-0 -n clustertest -c mssql-server -- bin/bash
cd /var/
ls /tmp
exit
```

## <a name="restore-the-backup-file"></a>Ripristinare il file di backup

Ripristinare quindi il backup del database in un'istanza master di SQL Server.  Se si ripristina un backup di database creato in Windows, sarà necessario ottenere i nomi dei file.  In Azure Data Studio connettersi all'istanza master ed eseguire lo script SQL seguente:

```sql
RESTORE FILELISTONLY FROM DISK='/tmp/<db file name>.bak'
```

Esempio:

```sql
RESTORE FILELISTONLY FROM DISK='/tmp/AdventureWorks2016CTP3.bak'
```

![Elenco dei file di backup](media/restore-database/database-restore-file-list.png)

Ripristinare ora il database. Di seguito è riportato uno script di esempio. Sostituire i nomi o i percorsi in base alle esigenze, a seconda del backup di database selezionato.

```sql
RESTORE DATABASE AdventureWorks2016CTP3
FROM DISK='/tmp/AdventureWorks2016CTP3.bak'
WITH MOVE 'AdventureWorks2016CTP3_Data' TO '/var/opt/mssql/data/AdventureWorks2016CTP3_Data.mdf',
        MOVE 'AdventureWorks2016CTP3_Log' TO '/var/opt/mssql/data/AdventureWorks2016CTP3_Log.ldf',
        MOVE 'AdventureWorks2016CTP3_mod' TO '/var/opt/mssql/data/AdventureWorks2016CTP3_mod'
```

## <a name="configure-data-pool-and-hdfs-access"></a>Configurare il pool di dati e l'accesso HDFS

Per consentire all'istanza master di SQL Server di accedere ai pool di dati e al gateway HDFS, eseguire ora le stored procedure del pool di dati e del pool di archiviazione. Eseguire gli script Transact-SQL seguenti sul database appena ripristinato:

```sql
USE AdventureWorks2016CTP3
GO
-- Create the SqlDataPool data source:
IF NOT EXISTS(SELECT * FROM sys.external_data_sources WHERE name = 'SqlDataPool')
  CREATE EXTERNAL DATA SOURCE SqlDataPool
  WITH (LOCATION = 'sqldatapool://controller-svc/default');

-- Create the SqlStoragePool data source:
IF NOT EXISTS(SELECT * FROM sys.external_data_sources WHERE name = 'SqlStoragePool')
   CREATE EXTERNAL DATA SOURCE SqlStoragePool
   WITH (LOCATION = 'sqlhdfs://controller-svc/default');
GO
```

> [!NOTE]
> Sarà necessario eseguire questi script di installazione solo sui database ripristinati da versioni precedenti di SQL Server. Se si crea un nuovo database nell'istanza master di SQL Server, le stored procedure del pool di dati e del pool di archiviazione sono già configurate.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vedere la panoramica seguente:

- [Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)
