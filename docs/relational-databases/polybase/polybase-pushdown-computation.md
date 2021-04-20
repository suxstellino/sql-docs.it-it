---
description: Calcoli con distribuzione in PolyBase
title: Calcoli con distribuzione in PolyBase
dexcription: Enable pushdown computation to improve performance of queries on your Hadoop cluster. You can select a subset of rows/columns in an external table for pushdown.
ms.date: 04/19/2021
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: ea91e9968f4e47ea394f6bb02d362d93da82ab94
ms.sourcegitcommit: 708b3131db2e542b1d461d17dec30d673fd5f0fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/19/2021
ms.locfileid: "107729184"
---
# <a name="pushdown-computations-in-polybase"></a>Calcoli con distribuzione in PolyBase

[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

Il calcolo con distribuzione migliora le prestazioni delle query su origini dati esterne. A partire [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] da , i calcoli di pushdown erano disponibili per le origini dati esterne Hadoop. [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] ha introdotto calcoli di pushdown per altri tipi di origini dati esterne.  

## <a name="enable-pushdown-computation"></a> Abilitare il calcolo con distribuzione

Gli articoli seguenti includono informazioni sulla configurazione del calcolo con distribuzione per tipi specifici di origini dati esterne:

- [Abilitare il calcolo con distribuzione in Hadoop](polybase-configure-hadoop.md#pushdown)
- [Configurare PolyBase per l'accesso a dati esterni in Oracle](polybase-configure-oracle.md)
- [Configurare PolyBase per l'accesso a dati esterni in Teradata](polybase-configure-teradata.md)
- [Configurare PolyBase per l'accesso a dati esterni in MongoDB](polybase-configure-mongodb.md)
- [Configurare PolyBase per l'accesso a dati esterni con i tipi generici ODBC](polybase-configure-odbc-generic.md)
- [Configurare PolyBase per l'accesso a dati esterni in SQL Server](polybase-configure-sql-server.md)

Questa tabella riepiloga il supporto del calcolo pushdown in origini dati esterne diverse:

| origine dati      | Join  | Proiezioni | Aggregations | Filtri   | Statistiche |
|------------------|--------|-------------|--------------|-----------|------------|
| **ODBC generico** | Sì    | Sì         | Sì          | Sì       | Sì        |  
| **Oracle**       | Sì    | Sì         | Sì          | Sì       | Sì        |
| **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**   | Sì    | Sì         | Sì          | Sì       | Sì        |
| **Teradata**     | Sì    | Sì         | Sì          | Sì       | Sì        |  
| **MongoDB**      | **No** | Sì         | Sì          | Sì       | Sì        |
| **Hadoop \** _     | _ *No** | Sì         | Alcuni\*\*     | Alcuni\*\*  | Sì        |  
| **Archiviazione BLOB di Azure** | No | No | No | No | Sì |
|                  |

\* PolyBase supporta attualmente due provider Hadoop: Hortonworks Data Platform (HDP) e Cloudera Distributed Hadoop (CDH). Non esistono differenze tra le due funzionalità in termini di calcolo della pushdown.

**I provider Hadoop supportano quanto segue:

| **Aggregazioni**                  | **Filtri (confronto binario)** | 
|-----------------------------------|---------------------------------| 
| Count_Big                         | NotEqual                        | 
| Somma                               | LessThan                        | 
| Media                               | LessOrEqual                     | 
| Max                               | GreaterOrEqual                  | 
| Min                               | GreaterThan                     | 
| Approx_Count_Distinct             | È                              | 
|                                   | IsNot                           | 
|                                   |                                 | 

Alcune aggregazioni devono verificarsi dopo che i dati raggiungono [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Tuttavia, una parte dell'aggregazione viene eseguita in Hadoop. Si tratta di un metodo comune di calcolo delle aggregazioni nei sistemi con la funzionalità di elaborazione parallela massiva.  


> [!NOTE]
> Il calcolo della pushdown può essere bloccato da una sintassi T-SQL. Per altre informazioni, vedere [Sintassi che impedisce la pushdown.](polybase-pushdown-computation.md#syntax-that-prevents-pushdown)

## <a name="key-beneficial-scenarios-of-pushdown-computation"></a>Scenari utili principali di calcolo della distribuzione

Con il calcolo pushdown di PolyBase, è possibile delegare le attività di calcolo a origini dati esterne. Questo riduce il carico di lavoro [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nell'istanza e può migliorare significativamente le prestazioni. 

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può eseguire il push di join, proiezioni, aggregazioni e filtri a origini dati esterne per sfruttare il calcolo remoto e limitare i dati inviati in rete. 

### <a name="pushdown-of-joins"></a>Pushdown dei join

In molti casi, PolyBase può facilitare il pushdown dell'operatore di join che migliorerà notevolmente le prestazioni.  

Se il join può essere eseguito nell'origine dati esterna, questo riduce la quantità di spostamento dei dati e migliora le prestazioni della query. Senza pushdown di join, i dati delle tabelle da unire in join devono essere aggiunti localmente in tempdb e quindi uniti in join.

### <a name="select-a-subset-of-rows"></a>Selezionare un subset di righe

Usare la distribuzione del predicato per migliorare le prestazioni se la query seleziona un subset di righe da una tabella esterna.

In questo esempio avvia un processo map-reduce per recuperare le righe [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che corrispondono al predicato `customer.account_balance < 200000` in Hadoop. Poiché la query può essere completata correttamente senza analizzare tutte le righe della tabella, solo le righe che soddisfano i criteri del predicato vengono copiate in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . In questo modo si risparmia molto tempo ed è necessario meno spazio per l'archiviazione temporanea quando il numero di saldi dei clienti inferiori a 200000 è ridotto rispetto al numero di clienti con saldi superiori o uguali a 200000.

```sql
SELECT * FROM customer WHERE customer.account_balance < 200000;
SELECT * FROM SensorData WHERE Speed > 65;  
```

### <a name="select-a-subset-of-columns"></a>Selezionare un subset di colonne

Usare la distribuzione del predicato per migliorare le prestazioni se la query seleziona un subset di colonne da una tabella esterna.

In questa query viene avviato un processo map-reduce per pre-elaborare il file di testo delimitato hadoop in modo che solo i dati per le due [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] colonne, customer.name e customer.zip_code, verranno copiati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

```sql
SELECT customer.name, customer.zip_code
FROM customer
WHERE customer.account_balance < 200000;
```

### <a name="pushdown-for-basic-expressions-and-operators"></a>Distribuzione per operatori ed espressioni di base

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consente le espressioni e gli operatori di base seguenti per il pushdown del predicato.

- Operatori di confronto binari (`<`, `>`, `=`, `!=`, `<>`, `>=`, `<=`) per valori numerici, di data e di ora.
- Operatori aritmetici (`+`, `-`, `*`, `/`, `%`).
- Operatori logici (`AND`, `OR`).
- Operatori unari (`NOT`, `IS NULL`, `IS NOT NULL`).

Gli operatori `BETWEEN`, `NOT`, `IN` e `LIKE` potrebbero essere distribuiti. Il comportamento effettivo dipende dal modo in cui Query Optimizer riscrive le espressioni degli operatori come serie di istruzioni che usano operatori relazionali di base.

La query in questo esempio include più predicati di cui è possibile eseguire il push in Hadoop. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può eseguire il push di processi map-reduce in Hadoop per eseguire il predicato `customer.account_balance <= 200000` . Anche l'espressione `BETWEEN 92656 AND 92677` è costituita da operazioni binarie e logiche di cui è possibile eseguire il push in Hadoop. L'operatore **AND** logico in `customer.account_balance AND customer.zipcode` è un'espressione finale.

Con questa combinazione di predicati, i processi MapReduce possono eseguire interamente la clausola WHERE. Solo i dati che soddisfano i `SELECT` criteri vengono copiati di nuovo in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

```sql
SELECT * FROM customer 
WHERE customer.account_balance <= 200000 
    AND customer.zipcode BETWEEN 92656 AND 92677;
```
### <a name="supported-functions-for-pushdown"></a>Funzioni supportate per il pushdown

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consente le funzioni seguenti per il pushdown del predicato.

Funzioni per i valori stringa
- `CONCAT`
- `DATALENGTH`
- `LEN`
- `LIKE`
- `LOWER`
- `LTRIM`
- `RTRIM`
- `SUBSTRING`
- `UPPER`

Funzioni matematiche
- `ABS`
- `ACOS`
- `ASIN`
- `ATAN`
- `CEILING`
- `COS`
- `EXP`
- `FLOOR`
- `POWER`
- `SIGN`
- `SIN`
- `SQRT`
- `TAN`

Funzioni generali
- `COALESCE`
- `NULLIF`

Funzioni & di data e ora
- `DATEADD`
- `DATEDIFF`
- `DATEPART`

## <a name="syntax-that-prevents-pushdown"></a>Sintassi che impedisce il pushdown

La sintassi o le funzioni T-SQL seguenti impediranno il calcolo pushdown:

- `AT TIME ZONE`
- `CONCAT_WS`
- `TRANSLATE`
- `RAND`
- `CHECKSUM`
- `BINARY_CHECKSUM`
- `ISJSON`
- `JSON_VALUE`
- `JSON_QUERY`
- `JSON_MODIFY`
- `NEWID`
- `STRING_ESCAPE`
- `COMPRESS`
- `DECOMPRESS`
- `GREATEST`
- `LEAST`
- `PARSE`

Il supporto pushdown per la `FORMAT` `TRIM` sintassi e è stato introdotto in [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] CU10.

## <a name="examples"></a>Esempio

### <a name="force-pushdown"></a>Forzare la distribuzione

```sql
SELECT * FROM [dbo].[SensorData]
WHERE Speed > 65
OPTION (FORCE EXTERNALPUSHDOWN);
```

### <a name="disable-pushdown"></a>Disabilitare la distribuzione

```sql
SELECT * FROM [dbo].[SensorData]
WHERE Speed > 65
OPTION (DISABLE EXTERNALPUSHDOWN);
```

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su PolyBase, vedere [Introduzione alla virtualizzazione dei dati con PolyBase](polybase-guide.md)
