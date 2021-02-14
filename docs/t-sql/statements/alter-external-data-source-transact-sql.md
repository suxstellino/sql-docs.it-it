---
description: ALTER EXTERNAL DATA SOURCE (Transact-SQL)
title: ALTER EXTERNAL DATA SOURCE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER EXTERNAL DATA SOURCE
- ALTER_EXTERNAL_DATA_SOURCE
dev_langs:
- TSQL
helpviewer_keywords:
- polybase, alter external data source statement
- ALTER EXTERNAL DATA SOURCE statement
ms.assetid: a34b9e90-199d-46d0-817a-a7e69387bf5f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 28b287969222e47501fd9e4fd18fef091a02a683
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337975"
---
# <a name="alter-external-data-source-transact-sql"></a>ALTER EXTERNAL DATA SOURCE (Transact-SQL)
[!INCLUDE [sqlserver2016-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdbmi-asa-pdw.md)]

  Modifica un'origine dati esterna usata per creare una tabella esterna. L'origine dati esterna può essere Hadoop o Archiviazione BLOB di Azure (WASBS) per SQL Server e Archiviazione BLOB di Azure (WASBS) o Azure Data Lake Storage (ABFSS/ADL) per [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]. 

## <a name="syntax"></a>Sintassi  

```syntaxsql
-- Modify an external data source
-- Applies to: SQL Server (2016 or later) and APS
ALTER EXTERNAL DATA SOURCE data_source_name SET
    {   
        LOCATION = '<prefix>://<path>[:<port>]' [,] |
        RESOURCE_MANAGER_LOCATION = <'IP address;Port'> [,] |
        CREDENTIAL = credential_name
    }  
    [;]  

-- Modify an external data source pointing to Azure Blob storage
-- Applies to: SQL Server (starting with 2017)
ALTER EXTERNAL DATA SOURCE data_source_name
    SET
        LOCATION = 'https://storage_account_name.blob.core.windows.net'
        [, CREDENTIAL = credential_name ] 

-- Modify an external data source pointing to Azure Blob storage or Azure Data Lake storage
-- Applies to: Azure Synapse Analytics
ALTER EXTERNAL DATA SOURCE data_source_name
    SET
        [LOCATION = '<location prefix>://<location path>']
        [, CREDENTIAL = credential_name ] 
```

## <a name="arguments"></a>Argomenti  
 data_source_name Specifica il nome definito dall'utente per l'origine dati. Il nome deve essere univoco.

 LOCATION = '<prefix>://<path>[:<port>]' Fornisce il protocollo di connettività e il percorso dell'origine dati esterna. Per un elenco delle opzioni di percorso valide, vedere [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](create-external-data-source-transact-sql.md#location--prefixpathport).

 RESOURCE_MANAGER_LOCATION = '\<IP address;Port>' (non si applica a [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]) Specifica il percorso di Gestione risorse Hadoop. Quando specificato, Query Optimizer può scegliere di pre-elaborare i dati di una query PolyBase usando le funzionalità di calcolo di Hadoop. Questa decisione si basa sui costi. Il pushdown dei predicati può ridurre notevolmente il volume dei dati trasferiti tra Hadoop e SQL e pertanto migliorare le prestazioni delle query.

 CREDENTIAL = Credential_Name Specifica la credenziale denominata. Vedere [CREATE DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-scoped-credential-transact-sql.md).

TYPE = [HADOOP | BLOB_STORAGE]   
**Si applica a:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)].
Solo per le operazioni bulk, `LOCATION` deve essere l'URL valido dell'archiviazione BLOB di Azure. Non inserire **/** , nome file o parametri di firma per l'accesso condiviso alla fine dell'URL `LOCATION`.
La credenziale usata deve essere creata usando `SHARED ACCESS SIGNATURE` come identità. Per altre informazioni sulle firme di accesso condiviso, vedere [Uso delle firme di accesso condiviso](/azure/storage/storage-dotnet-shared-access-signature-part-1).

  

## <a name="remarks"></a>Osservazioni
 È possibile modificare una sola origine per volta. Le richieste simultanee di modifica della stessa origine mettono in attesa un'istruzione. È invece possibile modificare origini diverse nello stesso momento. Questa istruzione può essere eseguita contemporaneamente ad altre istruzioni.

## <a name="permissions"></a>Autorizzazioni  
 È necessaria l'autorizzazione ALTER ANY EXTERNAL DATA SOURCE.
 > [!IMPORTANT]  
 > L'autorizzazione ALTER ANY EXTERNAL DATA SOURCE concede a qualsiasi entità di sicurezza la possibilità di creare e modificare qualsiasi oggetto origine dati esterna e, di conseguenza, la possibilità di accedere a tutte le credenziali con ambito database per il database. Questa autorizzazione deve essere considerata con privilegi elevati e quindi essere concessa solo a entità attendibili nel sistema.


## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene modificato il percorso e il percorso di Gestione risorse di un'origine dati esistente.

```sql  
ALTER EXTERNAL DATA SOURCE hadoop_eds SET
     LOCATION = 'hdfs://10.10.10.10:8020',
     RESOURCE_MANAGER_LOCATION = '10.10.10.10:8032'
    ;
```

 Nell'esempio seguente viene modificata la credenziale per connettersi a un'origine dati esistente.

```sql 
ALTER EXTERNAL DATA SOURCE hadoop_eds SET
   CREDENTIAL = new_hadoop_user
    ;
```
 Nell'esempio seguente le credenziali vengono modificate con un nuovo percorso. Questo esempio è un'origine dati esterna creata per [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]. 

```sql  
ALTER EXTERNAL DATA SOURCE AzureStorage_west SET
   LOCATION = 'wasbs://loadingdemodataset@updatedproductioncontainer.blob.core.windows.net',
   CREDENTIAL = AzureStorageCredential
```