---
title: 'Accedere ai dati esterni: Tipi ODBC generici - PolyBase'
description: PolyBase in SQL Server consente di connettersi alle origini dati compatibili tramite il connettore ODBC. Installare il driver ODBC e creare le tabelle esterne.
ms.date: 07/16/2020
ms.custom: seo-lt-2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
monikerRange: '>= sql-server-linux-ver15 || >= sql-server-ver15'
ms.openlocfilehash: e9cdd3692c32b57917e6db0e358c22f9c0dcd15a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351846"
---
# <a name="configure-polybase-to-access-external-data-with-odbc-generic-types"></a>Configurare PolyBase per l'accesso a dati esterni con i tipi generici ODBC

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

PolyBase in SQL Server 2019 consente di connettersi alle origini dati compatibili ODBC tramite il connettore ODBC.

Questo articolo illustra come creare la connettività per la configurazione usando un'origine dati ODBC. Le indicazioni fornite usano un driver ODBC specifico come esempio. Per esempi specifici, rivolgersi al provider ODBC. Per determinare le opzioni appropriate per la stringa di connessione, vedere la documentazione del driver ODBC per l'origine dati. Gli esempi in questo articolo potrebbero non essere applicabili a un driver ODBC specifico.

## <a name="prerequisites"></a>Prerequisiti

>[!NOTE]
>Questa funzionalità richiede SQL Server in Windows.

* È necessario installare e abilitare PolyBase per l'istanza [installazione di PolyBase](polybase-installation.md) di SQL Server.

* Prima di creare credenziali con ambito database, è necessario creare una [chiave master](../../t-sql/statements/create-master-key-transact-sql.md).

## <a name="install-the-odbc-driver"></a>Installare il driver ODBC

Scaricare e installare il driver ODBC dell'origine dati a cui ci si vuole connettere in ogni nodo PolyBase. Una volta installato correttamente il driver, è possibile visualizzare e testare il driver da **Amministratore origine dati ODBC**.

![Gruppi con scalabilità orizzontale di PolyBase](../../relational-databases/polybase/media/polybase-odbc-admin.png) 

Nell'esempio precedente il nome del driver è racchiuso in un cerchio rosso. Usare questo nome quando si crea l'origine dati esterna.

> [!IMPORTANT]
> Per migliorare le prestazioni delle query, abilitare il pool di connessioni. Questa operazione può essere eseguita da **Amministratore origine dati ODBC**.

## <a name="create-dependent-objects-in-sql-server"></a>Creare oggetti dipendenti in SQL Server

Per usare l'origine dati ODBC, è prima di tutto necessario creare alcuni oggetti per completare la configurazione.

In questa sezione vengono usati i comandi Transact-SQL seguenti:

* [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)
* [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md) 

1. Creare le credenziali con ambito database per l'accesso all'origine ODBC.

    ```sql
    CREATE DATABASE SCOPED CREDENTIAL <credential_name> WITH IDENTITY = '<username>', Secret = '<password>';
    ```

    L'esempio seguente crea una credenziale denominata `credential_name`, con l'identità `username` e una password complessa.

    ```sql
    CREATE DATABASE SCOPED CREDENTIAL credential_name WITH IDENTITY = 'username', Secret = 'BycA4ZjrE#*2W%!';
    ```

1. Creare un'origine dati esterna con [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

    ```sql
    CREATE EXTERNAL DATA SOURCE <external_data_source_name>
    WITH ( LOCATION = 'odbc://<ODBC server address>[:<port>]',
    CONNECTION_OPTIONS = 'Driver={<Name of Installed Driver>};
    ServerNode = <name of server  address>:<Port>',
    -- PUSHDOWN = [ON] | OFF,
    CREDENTIAL = <credential_name> );
    ```

    L'esempio seguente crea un'origine dati esterna:
    * Denominata `external_data_source_name`
    * Contenuta in `SERVERNAME` e sulla porta `4444` ODBC
    * Connessa con `CData ODBC Driver For SAP 2015`, ovvero il driver creato in [Installare il driver ODBC](#install-the-odbc-driver)
    * Sulla porta `5555` di `ServerNode` `sap_server_node`
    * Configurata per l'elaborazione propagata al server (`PUSHDOWN = ON`)
    * Che usa la credenziale `credential_name`

    ```sql
    CREATE EXTERNAL DATA SOURCE external_data_source_name
    WITH ( LOCATION = 'odbc://SERVERNAME:4444',
    CONNECTION_OPTIONS = 'Driver={CData ODBC Driver For SAP 2015};
    ServerNode = sap_server_node:5555',
    PUSHDOWN = ON,
    CREDENTIAL = credential_name );
    ```
    
## <a name="create-an-external-table"></a>Creare una tabella esterna

Dopo avere creato gli oggetti dipendenti, è possibile creare tabelle esterne con T-SQL. 

In questa sezione vengono usati i comandi Transact-SQL seguenti:
* [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md)
* [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md)

1. Creare una o più tabelle esterne.

   Creare una tabella esterna. Sarà necessario fare riferimento all'origine dati esterna creata in precedenza usando l'argomento `DATA_SOURCE` e specificare la tabella di origine come `LOCATION`. Non è necessario fare riferimento a tutte le colonne, ma sarà necessario assicurare che i tipi siano mappati correttamente.  

   ```sql
     CREATE EXTERNAL TABLE <your_table_name>
     (
     <col1_name>     DECIMAL(38) NOT NULL,
     <col2_name>     DECIMAL(38) NOT NULL,
     <col3_name>     CHAR COLLATE Latin1_General_BIN NOT NULL
     )
     WITH (
     LOCATION='<sap_table_name>',
     DATA_SOURCE= <external_data_source_name>
     )
     ;
   ```

   > [!NOTE]
   > Si noti che è possibile riutilizzare gli oggetti dipendenti per tutte le tabelle esterne usando questa origine dati esterna.

1. **Facoltativo:** Creare statistiche per una tabella esterna.

    Per garantire prestazioni ottimali delle query, è consigliabile creare le statistiche sulle colonne delle tabelle esterne, in particolare quelle usate per join, filtri e aggregazioni.

    ```sql
    CREATE STATISTICS statistics_name ON contact (FirstName) WITH FULLSCAN; 
    ```
    
## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su PolyBase, vedere [Che cos'è PolyBase?](polybase-guide.md).
