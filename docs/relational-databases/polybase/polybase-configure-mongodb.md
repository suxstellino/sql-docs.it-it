---
title: 'Accedere ai dati esterni: MongoDB - PolyBase'
description: L'articolo illustra come usare PolyBase in un'istanza di SQL Server per eseguire query sui dati esterni in MongoDB. Creare tabelle esterne per fare riferimento ai dati esterni.
ms.date: 03/05/2021
ms.metadata: seo-lt-2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
monikerRange: '>= sql-server-linux-ver15 || >= sql-server-ver15'
ms.openlocfilehash: a9d975bf5a65ec8ece1aa2f3b1e957007046f4c8
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247520"
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

   Lo script seguente crea una credenziale con ambito database. Prima di eseguire lo script, aggiornarlo per l'ambiente:

    - Sostituire `<credential_name>` con un nome per la credenziale.
    - Sostituire `<username>` con il nome utente per l'origine esterna.
    - Sostituire `<password>` con la password appropriata. 

    ```sql
    CREATE DATABASE SCOPED CREDENTIAL <credential_name> WITH IDENTITY = '<username>', Secret = '<password>';
    ```

   > [!IMPORTANT]
   > Il connettore ODBC MongoDB per PolyBase supporta solo l'autenticazione di base e non l'autenticazione Kerberos.

1. Creare un'origine dati esterna.

    Lo script seguente crea l'origine dati esterna. Per riferimento, vedere [creare un'origine dati esterna](../../t-sql/statements/create-external-data-source-transact-sql.md). Prima di eseguire lo script, aggiornarlo per l'ambiente:

    - Aggiornare il percorso. Impostare `<server>` e `<port>` per l'ambiente.
    - Sostituire `<credential_name>` con il nome delle credenziali create nel passaggio precedente.
    - Facoltativamente, è possibile specificare `PUSHDOWN = ON` o `PUSHDOWN = OFF` se si vuole specificare il calcolo di distribuzione per l'origine esterna.

    ```sql
    CREATE EXTERNAL DATA SOURCE external_data_source_name
    WITH (LOCATION = '<mongodb://<server>[:<port>]>',
    -- PUSHDOWN = ON | OFF,
    CREDENTIAL = <credential_name>);
    ```

1. **Facoltativo:** Creare statistiche per una tabella esterna.

    È consigliabile creare le statistiche sulle colonne delle tabelle esterne, in particolare quelle usate per join, filtri e aggregazioni, per prestazioni ottimali delle query.

    ```sql
    CREATE STATISTICS statistics_name ON customer (C_CUSTKEY) WITH FULLSCAN; 
    ```

>[!IMPORTANT]
>Dopo aver creato un'origine dati esterna, è possibile usare il comando [Create external Table](../../t-sql/statements/create-external-table-transact-sql.md) per creare una tabella in grado di eseguire query su tale origine.
>
>Per un esempio, vedere [Creare una tabella esterna per MongoDB](../../t-sql/statements/create-external-table-transact-sql.md#k-create-an-external-table-for-mongodb).

## <a name="mongodb-connection-options"></a>Opzioni di connessione MongoDB

Per informazioni sulle opzioni di connessione di MongoDB, vedere la [documentazione di MongoDB: formato URI della stringa di connessione](https://docs.mongodb.com/manual/reference/connection-string/#connection-string-options).

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

- Il campo nidificato `restaurant.address.building` diventa `restaurant.address_building`
- Il campo nidificato `restaurant.address.coord` diventa `restaurant.address_coord`
- Il campo nidificato `restaurant.address.street` diventa `restaurant.address_street`
- Il campo nidificato `restaurant.address.zipcode` diventa `restaurant.address_zipcode`

Le valutazioni della matrice vengono appiattite come indicato di seguito:

| grades_date | grades_grade  | games_score | 
| ------------- | ------------------------- | -------------- |
|1393804800000 |Una |2|
|1378857600000|Una |6|
|135898560000 |Una |10|
|1322006400000|Una |9|
|1299715200000 |b |14|

## <a name="cosmos-db-connection"></a>Connessione Cosmos DB

Usando l'API Mongo Cosmos DB e il connettore di base del database Mongo DB è possibile creare una tabella esterna di un' **istanza di Cosmos DB**. Questa operazione viene eseguita seguendo gli stessi passaggi elencati in precedenza. Assicurarsi che la credenziale con ambito database, l'indirizzo server, la porta e la stringa di posizione riflettano quelli del server di Cosmos DB.

## <a name="examples"></a>Esempio

Nell'esempio seguente viene creata un'origine dati esterna con i parametri seguenti:

| Parametro | Valore|
|---|---|
| Nome | `external_data_source_name`|
| Servizio | `mongodb0.example.com`|
| Istanza | `27017`|
| Set di repliche | `myRepl`|
| TLS | `true`|
| Calcolo con distribuzione | `On`|

```sql
CREATE EXTERNAL DATA SOURCE external_data_source_name
    WITH (LOCATION = 'mongodb://mongodb0.example.com:27017',
    CONNECTION_OPTION = 'replicaSet=myRepl','tls=true',
    PUSHDOWN = ON ,
    CREDENTIAL = credential_name);
```

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su PolyBase, vedere [Che cos'è PolyBase?](polybase-guide.md).
