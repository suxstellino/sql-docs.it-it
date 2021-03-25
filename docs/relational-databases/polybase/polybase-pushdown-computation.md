---
description: Calcoli con distribuzione in PolyBase
title: Calcoli con distribuzione in PolyBase
dexcription: Enable pushdown computation to improve performance of queries on your Hadoop cluster. You can select a subset of rows/columns in an external table for pushdown.
ms.date: 03/09/2021
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 872d23ca6c908bae5e238a50aad76c52ff4bca26
ms.sourcegitcommit: 17f05be5c08cf9a503a72b739da5ad8be15baea5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/25/2021
ms.locfileid: "105103888"
---
# <a name="pushdown-computations-in-polybase"></a>Calcoli con distribuzione in PolyBase

[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

Il calcolo con distribuzione migliora le prestazioni delle query su origini dati esterne. A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] , sono disponibili calcoli distribuzione per le origini dati esterne di Hadoop. [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] sono stati introdotti calcoli distribuzione per altri tipi di origini dati esterne.  

## <a name="enable-pushdown-computation"></a> Abilitare il calcolo con distribuzione

Gli articoli seguenti includono informazioni sulla configurazione del calcolo con distribuzione per tipi specifici di origini dati esterne:

- [Abilitare il calcolo con distribuzione in Hadoop](polybase-configure-hadoop.md#pushdown)
- [Configurare PolyBase per l'accesso a dati esterni in Oracle](polybase-configure-oracle.md)
- [Configurare PolyBase per l'accesso a dati esterni in Teradata](polybase-configure-teradata.md)
- [Configurare PolyBase per l'accesso a dati esterni in MongoDB](polybase-configure-mongodb.md)
- [Configurare PolyBase per l'accesso a dati esterni con i tipi generici ODBC](polybase-configure-odbc-generic.md)
- [Configurare PolyBase per l'accesso a dati esterni in SQL Server](polybase-configure-sql-server.md)

Questa tabella riepiloga il supporto per il calcolo di distribuzione in origini dati esterne diverse:

| origine dati      | Join  | Proiezioni | Aggregations | Filtri   | Statistiche |
|------------------|--------|-------------|--------------|-----------|------------|
| **ODBC generico** | Sì    | Sì         | Sì          | Sì       | Sì        |  
| **Oracle**       | Sì    | Sì         | Sì          | Sì       | Sì        |
| **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**   | Sì    | Sì         | Sì          | Sì       | Sì        |
| **Teradata**     | Sì    | Sì         | Sì          | Sì       | Sì        |  
| **MongoDB**      | **No** | Sì         | Sì          | Sì       | Sì        |
| **Hadoop \** _     | _ *No** | Sì         | Alcuni\*\*     | Alcuni\*\*  | Sì        |  
|                  |

\* La polibase supporta attualmente due provider Hadoop: Hortonworks Data Platform (HDP) e Cloudera Distributed Hadoop (CDH). Non esistono differenze tra le due caratteristiche in termini di calcolo distribuzione.

\*\* Per altre informazioni sul supporto della funzionalità distribuzione di Hadoop, vedere [calcolo distribuzione supportato dagli operatori T-SQL](polybase-versioned-feature-summary.md#pushdown-computation-supported-by-t-sql-operators).

> [!NOTE]
> Il calcolo distribuzione può essere bloccato da una sintassi T-SQL. Per altre informazioni, vedere [sintassi che impedisce distribuzione](polybase-versioned-feature-summary.md#syntax-that-prevents-pushdown).

## <a name="key-beneficial-scenarios-of-pushdown-computation"></a>Scenari principali utili per il calcolo di distribuzione

Con il calcolo di distribuzione di base, è possibile delegare le attività di calcolo a origini dati esterne. In questo modo si riduce il carico di lavoro sull' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] istanza e si può migliorare significativamente le prestazioni. 

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consente di eseguire il push di join, proiezioni, aggregazioni e filtri a origini dati esterne per sfruttare i vantaggi del calcolo remoto e limitare i dati inviati in rete. 

### <a name="pushdown-of-joins"></a>Distribuzione di join

In molti casi, la funzione polibase può semplificare il distribuzione dell'operatore di join che migliorerà significativamente le prestazioni.  

Se il join può essere eseguito nell'origine dati esterna, questo riduce la quantità di spostamento dei dati e migliora le prestazioni della query. Senza join distribuzione, i dati delle tabelle da unire in join devono essere portati localmente in tempdb, quindi Uniti in join.

### <a name="select-a-subset-of-rows"></a>Selezionare un subset di righe

Usare la distribuzione del predicato per migliorare le prestazioni se la query seleziona un subset di righe da una tabella esterna.

In questo esempio, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avvia un processo di riduzione della mappa per recuperare le righe che corrispondono al predicato `customer.account_balance < 200000` in Hadoop. Poiché la query può essere completata correttamente senza analizzare tutte le righe della tabella, vengono copiate solo le righe che soddisfano i criteri del predicato [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . In questo modo si risparmia molto tempo ed è necessario meno spazio per l'archiviazione temporanea quando il numero di saldi dei clienti inferiori a 200000 è ridotto rispetto al numero di clienti con saldi superiori o uguali a 200000.

```sql
SELECT * FROM customer WHERE customer.account_balance < 200000;
SELECT * FROM SensorData WHERE Speed > 65;  
```

### <a name="select-a-subset-of-columns"></a>Selezionare un subset di colonne

Usare la distribuzione del predicato per migliorare le prestazioni se la query seleziona un subset di colonne da una tabella esterna.

In questa query, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avvia un processo di riduzione della mappa per pre-elaborare il file di testo delimitato da Hadoop in modo che vengano copiati solo i dati per le due colonne, Customer.Name e customer.zip_code [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

```sql
SELECT customer.name, customer.zip_code
FROM customer
WHERE customer.account_balance < 200000;
```

### <a name="pushdown-for-basic-expressions-and-operators"></a>Distribuzione per operatori ed espressioni di base

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consente le espressioni e gli operatori di base seguenti per il predicato distribuzione.

- Operatori di confronto binari (`<`, `>`, `=`, `!=`, `<>`, `>=`, `<=`) per valori numerici, di data e di ora.
- Operatori aritmetici (`+`, `-`, `*`, `/`, `%`).
- Operatori logici (`AND`, `OR`).
- Operatori unari (`NOT`, `IS NULL`, `IS NOT NULL`).

Gli operatori `BETWEEN`, `NOT`, `IN` e `LIKE` potrebbero essere distribuiti. Il comportamento effettivo dipende dal modo in cui Query Optimizer riscrive le espressioni degli operatori come serie di istruzioni che usano operatori relazionali di base.

La query in questo esempio include più predicati di cui è possibile eseguire il push in Hadoop. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consente di eseguire il push dei processi Map-reduce a Hadoop per eseguire il predicato `customer.account_balance <= 200000` . Anche l'espressione `BETWEEN 92656 AND 92677` è costituita da operazioni binarie e logiche di cui è possibile eseguire il push in Hadoop. L'operatore **AND** logico in `customer.account_balance AND customer.zipcode` è un'espressione finale.

Con questa combinazione di predicati, i processi MapReduce possono eseguire interamente la clausola WHERE. Solo i dati che soddisfano i `SELECT` criteri vengono copiati di nuovo in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

```sql
SELECT * FROM customer 
WHERE customer.account_balance <= 200000 
    AND customer.zipcode BETWEEN 92656 AND 92677;
```

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

Per altre informazioni su polibase, vedere [che cos'è la polibase?](polybase-guide.md)
