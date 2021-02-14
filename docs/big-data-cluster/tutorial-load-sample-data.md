---
title: Caricare dati di esempio
titleSuffix: SQL Server big data clusters
description: Questa esercitazione illustra come caricare dati di esempio in un cluster Big Data di SQL Server. I dati di esempio includono dati relazionali presenti nell'istanza master di SQL Server e dati HDFS presenti nel pool di archiviazione. Questi dati supportano altre esercitazioni disponibili in questa sezione.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 08/21/2019
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 9b4e7667a3f109a0b5cca2f0af60af38613c5ce8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045687"
---
# <a name="tutorial-load-sample-data-into-a-sql-server-big-data-cluster"></a>Esercitazione: Caricare dati di esempio in un cluster Big Data di SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questa esercitazione illustra come usare uno script per caricare dati di esempio in un [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]. Questi dati di esempio vengono usati anche da molte altre esercitazioni disponibili nella documentazione.

> [!TIP]
> È possibile trovare esempi aggiuntivi per [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] nel repository GitHub [sql-server-samples](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster). Si trovano nel percorso **sql-server-samples/samples/features/sql-big-data-cluster/** .

## <a name="prerequisites"></a>Prerequisiti

- [Un cluster Big Data distribuito](deployment-guidance.md)
- [Strumenti per Big Data](deploy-big-data-tools.md)
   - **azdata**
   - **kubectl**
   - **sqlcmd**
   - **curl**
 
## <a name="load-sample-data"></a><a id="sampledata"></a> Caricare dati di esempio

Questa procedura usa uno script di bootstrap per scaricare un backup del database di SQL Server e caricare i dati nel cluster Big Data. Per semplicità d'uso, questa procedura è stata suddivisa nelle sezioni [Windows](#windows) e [Linux](#linux). Se si vuole usare la combinazione nome utente/password di base come meccanismo di autenticazione, impostare le variabili di ambiente AZDATA_USERNAME e AZDATA_PASSWORD prima di eseguire lo script. In caso contrario, lo script userà l'autenticazione integrata per connettersi all'istanza master di SQL Server e al gateway Knox. È anche necessario specificare il nome DNS per gli endpoint per poter usare l'autenticazione integrata.

## <a name="windows"></a><a id="windows"></a> Windows

Questa procedura descrive come usare un client Windows per caricare i dati di esempio nel cluster Big Data.

1. Aprire un nuovo prompt dei comandi di Windows.

   > [!IMPORTANT]
   > Non usare Windows PowerShell per questa procedura. In PowerShell, lo script avrà esito negativo perché userà la versione PowerShell di **curl**.

1. Usare **curl** per scaricare lo script di bootstrap per i dati di esempio.

   ```cmd
   curl -o bootstrap-sample-db.cmd "https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/bootstrap-sample-db.cmd"
   ```

1. Scaricare lo script Transact-SQL **bootstrap-sample-db.sql**, che viene chiamato dallo script di bootstrap.

   ```cmd
   curl -o bootstrap-sample-db.sql "https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/bootstrap-sample-db.sql"
   ```

1. Lo script di bootstrap richiede i seguenti parametri posizionali per il cluster Big Data:

   | Parametro | Descrizione |
   |---|---|
   | <CLUSTER_NAMESPACE> | Nome assegnato al cluster Big Data. |
   | <SQL_MASTER_ENDPOINT> | Nome DNS o indirizzo IP dell'istanza master. |
   | <KNOX_ENDPOINT> | Nome DNS o indirizzo IP del gateway HDFS/Spark. |
   
   > [!TIP]
   > Usare [kubectl](cluster-troubleshooting-commands.md) per trovare gli indirizzi IP per l'istanza master di SQL Server e Knox. Eseguire `kubectl get svc -n <your-big-data-cluster-name>` e osservare gli indirizzi EXTERNAL-IP per l'istanza master (**master-svc-external**) e Knox (**gateway-svc-external**). Il nome predefinito di un cluster è **mssql-cluster**.

1. Eseguire lo script di bootstrap.

   ```cmd
   .\bootstrap-sample-db.cmd <CLUSTER_NAMESPACE> <SQL_MASTER_ENDPOINT> <KNOX_ENDPOINT>
   ```

## <a name="linux"></a><a id="linux"></a> Linux

Questa procedura descrive come usare un client Linux per caricare i dati di esempio nel cluster Big Data.

1. Scaricare lo script di bootstrap e assegnare ad esso le autorizzazioni eseguibili.

   ```bash
   curl -o bootstrap-sample-db.sh "https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/bootstrap-sample-db.sh"
   chmod +x bootstrap-sample-db.sh
   ```

1. Scaricare lo script Transact-SQL **bootstrap-sample-db.sql**, che viene chiamato dallo script di bootstrap.

   ```bash
   curl -o bootstrap-sample-db.sql "https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/bootstrap-sample-db.sql"
   ```

1. Lo script di bootstrap richiede i seguenti parametri posizionali per il cluster Big Data:

   | Parametro | Descrizione |
   |---|---|
   | <CLUSTER_NAMESPACE> | Nome assegnato al cluster Big Data. |
   | <SQL_MASTER_ENDPOINT> | Nome DNS o indirizzo IP dell'istanza master. |
   | <KNOX_ENDPOINT> | Nome DNS o indirizzo IP del gateway HDFS/Spark. |

   > [!TIP]
   > Usare [kubectl](cluster-troubleshooting-commands.md) per trovare gli indirizzi IP per l'istanza master di SQL Server e Knox. Eseguire `kubectl get svc -n <your-big-data-cluster-name>` e osservare gli indirizzi EXTERNAL-IP per l'istanza master (**master-svc-external**) e Knox (**gateway-svc-external**). Il nome predefinito di un cluster è **mssql-cluster**.

1. Eseguire lo script di bootstrap.

   ```bash
   ./bootstrap-sample-db.sh <CLUSTER_NAMESPACE> <SQL_MASTER_ENDPOINT> <KNOX_ENDPOINT>
   ```

## <a name="next-steps"></a>Passaggi successivi

Al termine dell'esecuzione dello script di bootstrap, nel cluster Big Data sono contenuti i database di esempio e i dati HDFS. Le esercitazioni seguenti usano i dati di esempio per illustrare le funzionalità dei cluster Big Data:

Virtualizzazione dei dati:

- [Esercitazione: Eseguire query su dati HDFS in un cluster Big Data di SQL Server ](tutorial-query-hdfs-storage-pool.md)
- [Esercitazione: Eseguire query su dati Oracle da un cluster Big Data di SQL Server ](tutorial-query-oracle.md)

Inserimento dati:

- [Esercitazione: Inserire dati in un pool di dati di SQL Server con Transact-SQL](tutorial-data-pool-ingest-sql.md)
- [Esercitazione: Inserire dati in un pool di dati di SQL Server con processi Spark](tutorial-data-pool-ingest-spark.md)

Notebook:

- [Esercitazione: Eseguire un notebook di esempio in un cluster Big Data di SQL Server 2019](notebooks-tutorial-spark.md)
