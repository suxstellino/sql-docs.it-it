---
title: 'Accedere ai dati esterni: SQL Server - PolyBase'
description: Informazioni su come usare PolyBase in un'istanza di SQL Server per eseguire query sui dati esterni in un'altra istanza di SQL Server. Creare tabelle esterne per fare riferimento ai dati esterni.
ms.date: 10/06/2020
ms.custom: seo-lt-2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
monikerRange: '>= sql-server-linux-ver15 || >= sql-server-ver15'
ms.openlocfilehash: dbd62f695128965e1b60f9b027ddc06c003a8b71
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351768"
---
# <a name="configure-polybase-to-access-external-data-in-sql-server"></a>Configurare PolyBase per l'accesso a dati esterni in SQL Server

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Questo articolo illustra come usare PolyBase in un'istanza di SQL Server per eseguire query sui dati esterni in un'altra istanza di SQL Server.

## <a name="prerequisites"></a>Prerequisiti

Se PolyBase non è stato installato, vedere [Installazione di PolyBase](polybase-installation.md). Nell'articolo sull'installazione vengono illustrati i prerequisiti. Dopo l'installazione, assicurarsi anche di [abilitare PolyBase](polybase-installation.md#enable).

L'origine dati esterna di SQL Server usa l'autenticazione SQL.

Prima di creare credenziali con ambito database, è necessario creare una [chiave master](../../t-sql/statements/create-master-key-transact-sql.md). 

## <a name="configure-a-sql-server-external-data-source"></a>Configurare un'origine dati esterna di SQL Server

Per eseguire query sui dati da un'origine dati SQL Server, è necessario creare tabelle esterne per fare riferimento ai dati esterni. In questa sezione è disponibile codice di esempio per creare queste tabelle esterne.
 
Per prestazioni ottimali delle query, creare le statistiche sulle colonne delle tabelle esterne, in particolare quelle usate per join, filtri e aggregazioni.

In questa sezione vengono usati i comandi Transact-SQL seguenti:

- [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)
- [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md) 
- [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md)

1. Creare le credenziali con ambito database per l'accesso all'origine SQL Server. Nell'esempio seguente vengono create le credenziali per l'origine dati esterna con `IDENTITY = 'username'` e `SECRET = 'password'`.

    ```sql
    CREATE DATABASE SCOPED CREDENTIAL SqlServerCredentials
    WITH IDENTITY = 'username', SECRET = 'password';
    ```
   >[!IMPORTANT]
   >Il connettore ODBC SQL per PolyBase supporta solo l'autenticazione di base e non l'autenticazione Kerberos.

1. Creare un'origine dati esterna con [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md). L'esempio seguente:

   - Crea un'origine dati esterna denominata `SQLServerInstance`.
   - Identifica l'origine dati esterna (`LOCATION = '<vendor>://<server>[:<port>]'`). Nell'esempio punta a un'istanza predefinita di SQL Server.
   - Indica se eseguire il push del calcolo nell'origine (`PUSHDOWN`). `PUSHDOWN` è `ON` per impostazione predefinita.

   Infine, l'esempio usa le credenziali create in precedenza.

    ```sql
    CREATE EXTERNAL DATA SOURCE SQLServerInstance
        WITH ( LOCATION = 'sqlserver://SqlServer',
        PUSHDOWN = ON,
        CREDENTIAL = SQLServerCredentials);
    ```

1. Facoltativamente, creare statistiche per una tabella esterna.

  Per prestazioni ottimali delle query, creare le statistiche sulle colonne delle tabelle esterne, in particolare quelle usate per filtri di join e aggregazioni.

  ```sql
    CREATE STATISTICS statistics_name ON customer (C_CUSTKEY)
    WITH FULLSCAN;
  ```

>[!IMPORTANT]
>Dopo aver creato un'origine dati esterna, è possibile usare il comando [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md) per creare una tabella disponibile per query su tale origine.

## <a name="sql-server-connector-compatible-types"></a>Tipi compatibili con il connettore SQL Server

È possibile stabilire una connessione ad altre origini dati che riconosca una connessione di SQL Server. Usare il connettore PolyBase di SQL Server per creare una tabella esterna sia di Azure Synapse Analytics che del database SQL di Azure. A questo scopo, seguire gli stessi passaggi elencati in precedenza. Assicurarsi che le credenziali dell'ambito database, l'indirizzo del server, la porta e la stringa del percorso siano correlati a quelli dell'origine dati compatibile a cui si desidera connettersi.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su PolyBase, vedere [Che cos'è PolyBase?](polybase-guide.md).
