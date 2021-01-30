---
title: SELECT (Transact-SQL)
description: SELECT (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SELECT_TSQL
- SELECT
dev_langs:
- TSQL
helpviewer_keywords:
- retrieving rows
- SELECT statement [SQL Server]
- SELECT statement [SQL Server], about SELECT statement
- row retrieval [SQL Server], SELECT statement
- DML [SQL Server], SELECT statement
- data manipulation language [SQL Server], SELECT statement
- row retrieval [SQL Server]
- queries [SQL Server], results
author: VanMSFT
ms.author: vanto
ms.custom: ''
ms.date: 10/24/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5bc947add5b9413d4935cd3e6ddc517dfbc30cd5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204344"
---
# <a name="select-transact-sql"></a>SELECT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Recupera righe dal database e consente la selezione di una o più righe o colonne da una o più tabelle in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La sintassi completa dell'istruzione SELECT è complessa, ma le clausole principali sono le seguenti:  
  
[ WITH { [[ XMLNAMESPACES ,]](../../t-sql/xml/with-xmlnamespaces.md) [[ \<common_table_expression> ]](../../t-sql/queries/with-common-table-expression-transact-sql.md) } ]
  
 [SELECT *select_list*](../../t-sql/queries/select-clause-transact-sql.md) [ [INTO *new_table*](../../t-sql/queries/select-into-clause-transact-sql.md) ]  
  
 [ [FROM *table_source*](../../t-sql/queries/from-transact-sql.md) ] [ [WHERE *search_condition*](../../t-sql/queries/where-transact-sql.md) ]  
  
 [ [GROUP BY *group_by_expression*](../../t-sql/queries/select-group-by-transact-sql.md) ]  
  
 [ [HAVING *search_condition*](../../t-sql/queries/select-having-transact-sql.md) ]  
  
 [ [ORDER BY *order_expression* [ ASC | DESC ]](../../t-sql/queries/select-order-by-clause-transact-sql.md)]  
  
 È possibile usare gli operatori [UNION](../../t-sql/language-elements/set-operators-union-transact-sql.md), [EXCEPT e INTERSECT](../../t-sql/language-elements/set-operators-except-and-intersect-transact-sql.md) tra le query per combinare o confrontare i risultati di più query in un unico set di risultati.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
<SELECT statement> ::=    
    [ WITH { [ XMLNAMESPACES ,] [ <common_table_expression> [,...n] ] } ]  
    <query_expression>   
    [ ORDER BY <order_by_expression> ] 
    [ <FOR Clause>]   
    [ OPTION ( <query_hint> [ ,...n ] ) ]   
<query_expression> ::=   
    { <query_specification> | ( <query_expression> ) }   
    [  { UNION [ ALL ] | EXCEPT | INTERSECT }  
        <query_specification> | ( <query_expression> ) [...n ] ]   
<query_specification> ::=   
SELECT [ ALL | DISTINCT ]   
    [TOP ( expression ) [PERCENT] [ WITH TIES ] ]   
    < select_list >   
    [ INTO new_table ]   
    [ FROM { <table_source> } [ ,...n ] ]   
    [ WHERE <search_condition> ]   
    [ <GROUP BY> ]   
    [ HAVING < search_condition > ]   
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
[ WITH <common_table_expression> [ ,...n ] ]  
SELECT <select_criteria>  
[;]  
  
<select_criteria> ::=  
    [ TOP ( top_expression ) ]   
    [ ALL | DISTINCT ]   
    { * | column_name | expression } [ ,...n ]   
    [ FROM { table_source } [ ,...n ] ]  
    [ WHERE <search_condition> ]   
    [ GROUP BY <group_by_clause> ]   
    [ HAVING <search_condition> ]   
    [ ORDER BY <order_by_expression> ]  
    [ OPTION ( <query_option> [ ,...n ] ) ]  
  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Osservazioni  
 A causa della complessità dell'istruzione SELECT, gli elementi della sintassi e gli argomenti dettagliati sono stati raggruppati e descritti in base alla clausola:  

:::row:::
    :::column:::
        [WITH XMLNAMESPACES](../../t-sql/xml/with-xmlnamespaces.md)
    :::column-end:::
    :::column:::
        [HAVING](../../t-sql/queries/select-having-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [WITH common_table_expression](../../t-sql/queries/with-common-table-expression-transact-sql.md)
    :::column-end:::
    :::column:::
        [UNION](../../t-sql/language-elements/set-operators-union-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [Clausola SELECT](../../t-sql/queries/select-clause-transact-sql.md)
    :::column-end:::
    :::column:::
        [EXCEPT e INTERSECT](../../t-sql/language-elements/set-operators-except-and-intersect-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [Clausola INTO](../../t-sql/queries/select-into-clause-transact-sql.md)
    :::column-end:::
    :::column:::
        [ORDER BY](../../t-sql/queries/select-order-by-clause-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [FROM](../../t-sql/queries/from-transact-sql.md)
    :::column-end:::
    :::column:::
        [Clausola FOR](../../t-sql/queries/select-for-clause-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [WHERE](../../t-sql/queries/where-transact-sql.md)
    :::column-end:::
    :::column:::
        [Clausola OPTION](../../t-sql/queries/option-clause-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [GROUP BY](../../t-sql/queries/select-group-by-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

 L'ordine delle clausole nell'istruzione SELECT è significativo. È possibile omettere qualsiasi clausola facoltativa, ma se tali clausole vengono utilizzate, è necessario specificarle nell'ordine corretto.  
  
 Le istruzioni SELECT sono consentite in funzioni definite dall'utente solo se gli elenchi di selezione di tali istruzioni includono espressioni per l'assegnazione di valori a variabili che sono locali rispetto alle funzioni.  
  
 Un nome composto da quattro parti costruito con la funzione OPENDATASOURCE come parte del nome di server può essere utilizzato come origine di tabella in qualsiasi punto di istruzioni SELECT in cui sono consentiti i nomi di tabella. Non è possibile specificare un nome in quattro parti per il [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Per le istruzioni SELECT in cui sono coinvolte tabelle remote sono previste alcune limitazioni della sintassi.  
  
## <a name="logical-processing-order-of-the-select-statement"></a>Ordine di elaborazione logico dell'istruzione SELECT  
 Nei passaggi seguenti viene mostrato l'ordine di elaborazione logica, o ordine di associazione, per un'istruzione SELECT. Questo ordine consente di determinare il momento in cui gli oggetti definiti in un passaggio vengono resi disponibili per le clausole nei passaggi successivi. Ad esempio, se Query Processor può essere associato alle tabelle o alle viste definite nella clausola FROM, ovvero gli viene consentito l'accesso, questi oggetti e le relative colonne vengono resi disponibili in tutti i passaggi successivi. Invece, poiché la clausola SELECT si trova al passaggio 8, tramite le clausole precedenti non è possibile fare riferimento a qualsiasi alias di colonna o colonna derivata definito in tale clausola. Tuttavia, è possibile farvi riferimento tramite clausole successive, ad esempio ORDER BY. L'esecuzione fisica effettiva dell'istruzione viene determinata da Query Processor e l'ordine potrebbe essere diverso rispetto a questo elenco.  
  
1.  FROM  
2.  ON  
3.  JOIN  
4.  WHERE  
5.  GROUP BY  
6.  WITH CUBE o WITH ROLLUP  
7.  HAVING  
8.  SELECT  
9. DISTINCT  
10. ORDER BY  
11. TOP  

> [!WARNING]
> La sequenza precedente si rivela in genere esatta. Tuttavia, esistono casi non comuni in cui la sequenza può variare.
>
> Si supponga, ad esempio, di avere un indice cluster in una vista, che la vista escluda alcune righe di tabella e che l'elenco della colonna SELECT della vista usi una funzione CONVERT che modifica un tipo di dati da *varchar* a *integer*. In questo caso la funzione CONVERT può essere eseguita prima della clausola WHERE. Si tratta di una situazione molto insolita. Spesso è possibile modificare la vista per evitare una sequenza diversa, se è importante nel caso specifico. 

## <a name="permissions"></a>Autorizzazioni  
 La selezione di dati richiede l'autorizzazione **SELECT** per la tabella o la vista che potrebbe essere ereditata da un ambito più elevato, ad esempio l'autorizzazione **SELECT** per lo schema o l'autorizzazione **CONTROL** per la tabella. Oppure richiede l'appartenenza al ruolo predefinito del database **db_datareader** o **db_owner** o il ruolo predefinito del server **sysadmin**. La creazione di una nuova tabella tramite **SELECTINTO** richiede anche le autorizzazioni **CREATE TABLE** e **ALTER SCHEMA** per lo schema proprietario della nuova tabella.  
  
## <a name="examples"></a>Esempi:   
Nell'esempio seguente viene utilizzato il database [!INCLUDE[ssawPDW](../../includes/ssawpdw-md.md)].
  
### <a name="a-using-select-to-retrieve-rows-and-columns"></a>R. Utilizzo dell'istruzione SELECT per il recupero di righe e colonne  
 Questa sezione illustra i tre esempi di codice. Nel primo esempio di codice vengono restituite tutte le righe (clausola WHERE omessa) e tutte le colonne, usando `*`, della tabella `DimEmployee`.  
  
```sql  
SELECT *  
FROM DimEmployee  
ORDER BY LastName;  
```  
  
 Nell'esempio successivo vengono usati gli alias di tabella per ottenere lo stesso risultato.  
  
```sql  
SELECT e.*  
FROM DimEmployee AS e  
ORDER BY LastName;  
```  
  
 Nell'esempio vengono restituite tutte le righe (clausola WHERE omessa) e un subset delle colonne (`FirstName`, `LastName`, `StartDate`) della tabella `DimEmployee` del database `AdventureWorksPDW2012`. L'intestazione della terza colonna è stata rinominata `FirstDay`.  
  
```sql  
SELECT FirstName, LastName, StartDate AS FirstDay  
FROM DimEmployee   
ORDER BY LastName;  
```  
  
 In questo esempio vengono restituite solo le righe per `DimEmployee` che hanno un valore di `EndDate` che non è NULL e un valore di `MaritalStatus` pari a "M" (sposato).  
  
```sql  
SELECT FirstName, LastName, StartDate AS FirstDay  
FROM DimEmployee   
WHERE EndDate IS NOT NULL   
AND MaritalStatus = 'M'  
ORDER BY LastName;  
```  
  
### <a name="b-using-select-with-column-headings-and-calculations"></a>B. Utilizzo dell'istruzione SELECT con intestazioni e calcoli di colonna  
 Nell'esempio seguente vengono restituite tutte le righe della tabella `DimEmployee` e viene calcolata la retribuzione lorda per ogni dipendente in base al valore di `BaseRate` e considerando 40 ore di lavoro alla settimana.  
  
```sql  
SELECT FirstName, LastName, BaseRate, BaseRate * 40 AS GrossPay  
FROM DimEmployee  
ORDER BY LastName;  
```  
  
### <a name="c-using-distinct-with-select"></a>C. Utilizzo della clausola DISTINCT con l'istruzione SELECT  
 L'esempio seguente usa `DISTINCT` per generare un elenco di tutti i titoli univoci nella tabella `DimEmployee`.  
  
```sql  
SELECT DISTINCT Title  
FROM DimEmployee  
ORDER BY Title;  
```  
  
### <a name="d-using-group-by"></a>D. Utilizzo della clausola GROUP BY  
 L'esempio seguente trova l'importo totale di tutte le vendite in ogni giornata.  
  
```sql  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales  
FROM FactInternetSales  
GROUP BY OrderDateKey  
ORDER BY OrderDateKey;  
```  
  
 La presenza della clausola `GROUP BY` comporta la restituzione di una sola riga contenente il totale di tutte le vendite per ogni giornata.  
  
### <a name="e-using-group-by-with-multiple-groups"></a>E. Utilizzo della clausola GROUP BY con più gruppi  
 Nell'esempio seguente viene individuato il prezzo medio e la somma delle vendite su Internet per ogni giorno, raggruppati per data dell'ordine e per chiave di innalzamento di livello.  
  
```sql  

SELECT OrderDateKey, PromotionKey, AVG(SalesAmount) AS AvgSales, SUM(SalesAmount) AS TotalSales  
FROM FactInternetSales  
GROUP BY OrderDateKey, PromotionKey  
ORDER BY OrderDateKey;   
```  
  
### <a name="f-using-group-by-and-where"></a>F. Utilizzo delle clausole GROUP BY e WHERE  
 Nell'esempio seguente i risultati vengono suddivisi in gruppi dopo che sono state recuperate solo le righe con date successive al 1 agosto 2002.  
  
```sql  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales  
FROM FactInternetSales  
WHERE OrderDateKey > '20020801'  
GROUP BY OrderDateKey  
ORDER BY OrderDateKey;  
```  
  
### <a name="g-using-group-by-with-an-expression"></a>G. Utilizzo della clausola GROUP BY con un'espressione  
 Nell'esempio seguente vengono creati gruppi in base a un'espressione. È possibile creare gruppi in base a un'espressione se tale espressione non include funzioni di aggregazione.  
  
```sql  
SELECT SUM(SalesAmount) AS TotalSales  
FROM FactInternetSales  
GROUP BY (OrderDateKey * 10);  
```  
  
### <a name="h-using-group-by-with-order-by"></a>H. Utilizzo della clausola GROUP BY con la clausola ORDER BY  
 Nell'esempio seguente le vendite vengono sommate e ordinate per giorno.  
  
```sql  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales  
FROM FactInternetSales  
GROUP BY OrderDateKey  
ORDER BY OrderDateKey;  
```  
  
### <a name="i-using-the-having-clause"></a>I. Utilizzo della clausola HAVING  
 Questa query usa la clausola `HAVING` per limitare i risultati.  
  
```sql  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales  
FROM FactInternetSales  
GROUP BY OrderDateKey  
HAVING OrderDateKey > 20010000  
ORDER BY OrderDateKey;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Esempi di istruzioni SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-examples-transact-sql.md)  
 [Hint &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql.md)
  

