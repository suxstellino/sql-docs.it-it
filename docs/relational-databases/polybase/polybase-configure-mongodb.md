---
title: 'Accedere ai dati esterni: MongoDB - PolyBase'
description: L'articolo illustra come usare PolyBase in un'istanza di SQL Server per eseguire query sui dati esterni in MongoDB. Creare tabelle esterne per fare riferimento ai dati esterni.
ms.date: 12/13/2019
ms.metadata: seo-lt-2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
monikerRange: '>= sql-server-linux-ver15 || >= sql-server-ver15'
ms.openlocfilehash: 306feebece733cf382f486dc686117016800f4d1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351795"
---
# <a name="configure-polybase-to-access-external-data-in-mongodb"></a>Configurare PolyBase per l'accesso a dati esterni in MongoDB

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

L'articolo illustra come usare PolyBase in un'istanza di SQL Server per eseguire query sui dati esterni in MongoDB.

## <a name="prerequisites"></a>Prerequisiti

Se PolyBase non è stato installato, vedere [Installazione di PolyBase](polybase-installation.md).

Prima di creare credenziali con ambito database, è necessario creare una [chiave master](../../t-sql/statements/create-master-key-transact-sql.md). 
    

## <a name="configure-a-mongodb-external-data-source"></a>Configurare un'origine dati MongoDB esterna

Per eseguire query sui dati da un'origine dati MongoDB, è necessario creare tabelle esterne per fare riferimento ai dati esterni. In questa sezione è disponibile codice di esempio per creare queste tabelle esterne.

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
   > Il connettore ODBC MongoDB per PolyBase supporta solo l'autenticazione di base e non l'autenticazione Kerberos.    
    
1. Creare un'origine dati esterna con [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

    ```sql
    /*  LOCATION: Location string should be of format '<type>://<server>[:<port>]'.
    *  PUSHDOWN: specify whether computation should be pushed down to the source. ON by default.
    *CONNECTION_OPTIONS: Specify driver location
    *  CREDENTIAL: the database scoped credential, created above.
    */
    CREATE EXTERNAL DATA SOURCE external_data_source_name
    WITH (LOCATION = 'mongodb://<server>[:<port>]',
    -- PUSHDOWN = ON | OFF,
    CREDENTIAL = credential_name);
    ```

1. **Facoltativo:** Creare statistiche per una tabella esterna.

    È consigliabile creare le statistiche sulle colonne delle tabelle esterne, in particolare quelle usate per join, filtri e aggregazioni, per prestazioni ottimali delle query.

    ```sql
    CREATE STATISTICS statistics_name ON customer (C_CUSTKEY) WITH FULLSCAN; 
    ```

>[!IMPORTANT] 
>Dopo aver creato un'origine dati esterna, è possibile usare il comando [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md) per creare una tabella disponibile per query su tale origine.
>
>Per un esempio, vedere [Creare una tabella esterna per MongoDB](../../t-sql/statements/create-external-table-transact-sql.md#k-create-an-external-table-for-mongodb).

## <a name="flattening"></a>Rendere flat
L'impostazione per l'appiattimento è abilitata per i dati nidificati e ripetuti delle raccolte di documenti MongoDB. L'utente deve abilitare `create an external table` e specificare in modo esplicito uno schema relazionale per le raccolte di documenti MongoDB che possono avere dati nidificati e/o ripetuti. I tipi di dati JSON nidificati/ripetuti verranno appiattiti come indicato di seguito

* Oggetto: raccolta chiave/valore non ordinata racchiusa tra parentesi graffe (nidificata)

   - SQL Server crea una colonna della tabella per ogni chiave oggetto

     * Nome colonna: objectname_keyname

* Matrice: valori ordinati, separati da virgole, racchiusi tra parentesi quadre (ripetute)

   - SQL Server aggiunge una nuova riga della tabella per ogni elemento della matrice

   - SQL Server crea una colonna per ogni matrice per archiviare l'indice dell'elemento matrice

     * Nome colonna: arrayname_index

     * Tipo di dati: bigint

Questa tecnica può originare vari problemi, tra cui i due seguenti:

* Un campo ripetuto vuoto maschererà i dati contenuti nei campi flat dello stesso record

* La presenza di più campi ripetuti può comportare una crescita esponenziale del numero di righe prodotte

A titolo di esempio SQL Server valuta la raccolta di ristoranti del dataset di esempio MongoDB archiviata in formato JSON non relazionale. Ogni ristorante dispone di un campo indirizzo nidificato e di una matrice di valutazioni che sono state assegnate in giorni diversi. La figura seguente illustra un ristorante con l'indirizzo nidificato e le valutazioni nidificate ripetute.

![Appiattimento MongoDB](../../relational-databases/polybase/media/mongo-flattening.png "Appiattimento ristoranti MongoDB")

L'indirizzo dell'oggetto verrà appiattito (reso flat) come indicato di seguito:

* Il campo nidificato restaurant.address.building diventa restaurant.address_building
* Il campo nidificato restaurant.address.coord diventa restaurant.address_coord
* Il campo nidificato restaurant.address.street diventa restaurant.address_street
* Il campo nidificato restaurant.address.zipcode diventa restaurant.address_zipcode

Le valutazioni della matrice vengono appiattite come indicato di seguito:

| grades_date | grades_grade  | games_score | 
| ------------- | ------------------------- | -------------- |
|1393804800000 |Una |2|
|1378857600000|Una |6|
|135898560000 |Una |10|
|1322006400000|Una |9|
|1299715200000 |b |14|

## <a name="cosmos-db-connection"></a>Connessione Cosmos DB

Usando l'api di Mongo Cosmos DB e il connettore di Mongo DB PolyBase è possibile creare una tabella esterna di un'**istanza di Cosmos DB**. Questa operazione viene eseguita seguendo gli stessi passaggi elencati in precedenza. Assicurarsi che la credenziale con ambito database, l'indirizzo server, la porta e la stringa di posizione riflettano quelli del server di Cosmos DB. 

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su PolyBase, vedere [Che cos'è PolyBase?](polybase-guide.md).
