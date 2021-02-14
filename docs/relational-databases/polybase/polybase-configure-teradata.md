---
title: 'Accedere ai dati esterni: Teradata - PolyBase'
description: Informazioni su come usare PolyBase in un'istanza di SQL Server per eseguire query sui dati esterni in Teradata. Creare tabelle esterne per fare riferimento ai dati esterni.
ms.date: 12/13/2019
ms.custom: seo-lt-2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
monikerRange: '>= sql-server-linux-ver15 || >= sql-server-ver15'
ms.openlocfilehash: d4ce2a5dba42325a6520c73120426e3f06c6e121
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351763"
---
# <a name="configure-polybase-to-access-external-data-in-teradata"></a>Configurare PolyBase per l'accesso a dati esterni in Teradata

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Questo articolo illustra come usare PolyBase in un'istanza di SQL Server per eseguire query sui dati esterni in Teradata.

## <a name="prerequisites"></a>Prerequisiti

Se PolyBase non è stato installato, vedere [Installazione di PolyBase](polybase-installation.md). Nell'articolo sull'installazione vengono illustrati i prerequisiti.

Prima di creare credenziali con ambito database, è necessario creare una [chiave master](../../t-sql/statements/create-master-key-transact-sql.md). 

Per usare PolyBase in Teradata, è necessario il ridistribuibile di VC++.
 
## <a name="configure-a-teradata-external-data-source"></a>Configurare un'origine dati Teradata esterna

Per eseguire query sui dati da un'origine dati Teradata, è necessario creare tabelle esterne per fare riferimento ai dati esterni. In questa sezione è disponibile codice di esempio per creare queste tabelle esterne. 



In questa sezione vengono usati i comandi Transact-SQL seguenti:

- [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)
- [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md) 
- [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md)

1. Creare una credenziale con ambito database per l'accesso all'origine di MongoDB.

    ```sql
    /*  specify credentials to external data source
    *  IDENTITY: user name for external source. 
    *  SECRET: password for external source.
    */
    CREATE DATABASE SCOPED CREDENTIAL credential_name WITH IDENTITY = 'username', Secret = 'password';
    ```

   > [!IMPORTANT] 
   > Il connettore ODBC Teradata per PolyBase supporta solo l'autenticazione di base e non l'autenticazione Kerberos.

1. Creare un'origine dati esterna con [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

    ```sql
    /*  LOCATION: Location string should be of format '<vendor>://<server>[:<port>]'.
    *  PUSHDOWN: specify whether computation should be pushed down to the source. ON by default.
    * CONNECTION_OPTIONS: Specify driver location
    *  CREDENTIAL: the database scoped credential, created above.
    */  
    CREATE EXTERNAL DATA SOURCE external_data_source_name
    WITH (LOCATION = teradata://<server address>[:<port>],
    -- PUSHDOWN = ON | OFF,
    CREDENTIAL =credential_name);
    ```

1. **Facoltativo:** Creare statistiche per una tabella esterna.

    È consigliabile creare le statistiche sulle colonne delle tabelle esterne, in particolare quelle usate per join, filtri e aggregazioni, per prestazioni ottimali delle query.

    ```sql
    CREATE STATISTICS statistics_name ON customer (C_CUSTKEY) WITH FULLSCAN; 
    ```

>[!IMPORTANT] 
>Dopo aver creato un'origine dati esterna, è possibile usare il comando [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md) per creare una tabella disponibile per query su tale origine.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su PolyBase, vedere [Che cos'è PolyBase?](polybase-guide.md).
