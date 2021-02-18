---
title: Guida all'uso della crittografia dei dati inattivi TDE di cluster Big Data di SQL Server
titleSuffix: SQL Server Big Data Clusters
description: Questo articolo illustra come usare la funzionalità di crittografia dei dati inattivi TDE di cluster Big Data di SQL Server
author: DaniBunny
ms.author: dacoelho
ms.reviewer: mihaelab
ms.date: 10/19/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 7d249f9b70479603ad66e6b68cec784b1476324a
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489315"
---
# <a name="sql-server-big-data-clusters-transparent-data-encryption-tde-at-rest-usage-guide"></a>Guida all'uso della crittografia dei dati inattivi TDE di cluster Big Data di SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questa guida illustra come usare le funzionalità di crittografia dei dati inattivi di cluster Big Data di SQL Server per crittografare i database.

L'esperienza è in genere identica a SQL Server in Linux. È valida la [documentazione standard TDE](../relational-databases/security/encryption/transparent-data-encryption.md) ad eccezione dei casi indicati. Per monitorare lo stato della crittografia nell'istanza master, seguire i modelli di query DMV standard nella parte superiore di [`sys.dm_database_encryption_keys`](../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md) e [`sys.certificates`](../relational-databases/system-catalog-views/sys-certificates-transact-sql.md).

__Funzionalità non supportate:__
* Crittografia dei pool di dati

## <a name="prerequisites"></a><a id="prereqs"></a> Prerequisiti

- [Cluster Big Data di SQL Server CU8+](release-notes-big-data-cluster.md)
- [Strumenti per Big Data](deploy-big-data-tools.md)
   - **Azure Data Studio**
- Utente SQL Server con privilegi amministrativi.

## <a name="query-the-installed-certificates"></a>Eseguire una query sui certificati installati

1. In Azure Data Studio connettersi all'istanza master di SQL Server del cluster Big Data. Per altre informazioni, vedere [Connettersi all'istanza master di SQL Server](connect-to-big-data-cluster.md#master).

1. Fare doppio clic sulla connessione nella finestra **Server** per visualizzare il dashboard del server per l'istanza master di SQL Server. Selezionare **Nuova query**.

   ![Query dell'istanza master di SQL Server](./media/tutorial-data-pool-ingest-sql/sql-server-master-instance-query.png)

1. Eseguire il comando Transact-SQL seguente per modificare il contesto nel database **master** dell'istanza master.

   ```sql
   USE master
   GO
   ```

1. Eseguire una query sui certificati gestiti dal sistema installati. 

   ```sql
    SELECT TOP 1 name FROM sys.certificates WHERE name LIKE 'TDECertificate%' ORDER BY name DESC
   ```

    Usare criteri di query diversi in base alle esigenze.

    Il nome del certificato verrà elencato come "TDECertificate{timestamp}". Il prefisso TDECertificate seguito da timestamp che viene visualizzato corrisponde al certificato fornito dalla funzionalità gestita dal sistema.

## <a name="encrypt-a-database-using-the-system-provided-certificate"></a>Crittografare un database usando il certificato fornito dal sistema

Negli esempi seguenti prendere in considerazione un database denominato __userdb__ come destinazione della crittografia e un certificato fornito dal sistema denominato __TDECertificate2020_09_15_22_46_27__ per output della sezione precedente.

1. Usare il modello seguente per generare una chiave di crittografia del database usando il certificato fornito dal sistema.

   ```sql
    USE userdb; 
    GO
    CREATE DATABASE ENCRYPTION KEY WITH ALGORITHM = AES_256 ENCRYPTION BY SERVER CERTIFICATE TDECertificate2020_09_15_22_46_27;
    GO
   ```

1. Crittografare il database __userdb__ con il comando seguente.

   ```sql
    ALTER DATABASE userdb SET ENCRYPTION ON;
    GO
   ```

## <a name="next-steps"></a>Passaggi successivi

Informazioni sulla crittografia dei dati inattivi per HDFS:
> [!div class="nextstepaction"]
> [Zone di crittografia HDFS](encryption-at-rest-hdfs-encryption-zones.md)
