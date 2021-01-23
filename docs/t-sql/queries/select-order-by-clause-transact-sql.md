---
description: Clausola SELECT - ORDER BY (Transact-SQL)
title: Clausola ORDER BY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/24/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ORDER_TSQL
- BY
- ORDER_BY_TSQL
- BY_TSQL
- ORDER
- ORDER BY
dev_langs:
- TSQL
helpviewer_keywords:
- ad-hoc query paging
- OFFSET clause
- SELECT statement [SQL Server], FETCH clause
- clauses [SQL Server], ORDER BY
- SELECT statement [SQL Server], limiting the rows returned
- data [SQL Server], limiting the rows returned
- data [SQL Server], ad-hoc query paging
- sort orders [SQL Server]
- SELECT statement [SQL Server], OFFSET clause
- ORDER BY clause [Transact-SQL]
- LIMIT
- limiting result sets
- SELECT statement [SQL Server], ORDER BY clause
- query paging
- data [SQL Server], sorting
- limiting data returned in a query
- sort orders [SQL Server], ORDER BY clause
- FETCH clause
ms.assetid: bb394abe-cae6-4905-b5c6-8daaded77742
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a7903d3a208bd08c9ba22d251a0e4124e81725ea
ms.sourcegitcommit: 2bdf1f1ee88f4fe3e872227d025e965e95d1b2b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2021
ms.locfileid: "98711987"
---
# <a name="select---order-by-clause-transact-sql"></a>Clausola SELECT - ORDER BY (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Ordina i dati restituiti da una query in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Utilizzare questa clausola per eseguire le operazioni seguenti:  
  
-   Ordinare il set di risultati di una query in base all'elenco di colonne specificato e, facoltativamente, limitare le righe restituite a un intervallo specificato. L'ordine in cui le righe vengono restituite in un set di risultati non è garantito, a meno che non venga specificata una clausola ORDER BY.  
  
-   Determinare l'ordine in cui i valori della [funzione di rango](../../t-sql/functions/ranking-functions-transact-sql.md) vengono applicati al set di risultati.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
> [!NOTE]  
>  ORDER BY non è supportata nelle istruzioni SELECT/INTO o CREATE TABLE AS SELECT (CTAS) in [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].

## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
ORDER BY order_by_expression  
    [ COLLATE collation_name ]   
    [ ASC | DESC ]   
    [ ,...n ]   
[ <offset_fetch> ]  
  
<offset_fetch> ::=  
{   
    OFFSET { integer_constant | offset_row_count_expression } { ROW | ROWS }  
    [  
      FETCH { FIRST | NEXT } {integer_constant | fetch_row_count_expression } { ROW | ROWS } ONLY  
    ]  
}  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
[ ORDER BY   
    {  
    order_by_expression   
    [ ASC | DESC ]   
    } [ ,...n ]   
]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *order_by_expression*  
 Specifica una colonna o un'espressione sulla quale ordinare il set di risultati della query. Per specificare una colonna di ordinamento è possibile utilizzare il nome, l'alias di colonna o un intero non negativo che rappresenta la posizione della colonna nell'elenco di selezione.  
  
 È possibile specificare più colonne di ordinamento. I nomi delle colonne devono essere univoci. La sequenza delle colonne di ordinamento nella clausola ORDER BY definisce l'organizzazione del set di risultati ordinato. Ciò significa che il set di risultati viene ordinato in base alla prima colonna e che l'elenco ordinato viene ordinato in base alla seconda colonna e così via.  
  
 I nomi delle colonne a cui viene fatto riferimento nella clausola ORDER BY devono corrispondere senza ambiguità a una colonna o a un alias di colonna nell'elenco di selezione o a una colonna definita in una tabella specificata nella clausola FROM. Se la clausola ORDER BY fa riferimento a un alias di colonna nell'elenco di selezione, l'alias di colonna deve essere usato autonomamente e non come parte di un'espressione nella clausola ORDER BY, ad esempio:
 
```sql
SELECT SCHEMA_NAME(schema_id) AS SchemaName FROM sys.objects 
ORDER BY SchemaName; -- correct 
SELECT SCHEMA_NAME(schema_id) AS SchemaName FROM sys.objects 
ORDER BY SchemaName + ''; -- wrong
```
  
 COLLATE *collation_name*  
 Specifica che l'operazione ORDER BY deve essere eseguita in base alle regole di confronto indicate in *collation_name* e non in base alle regole della colonna definite nella tabella o nella vista. In *collation_name* è possibile usare nomi di regole di confronto di Windows o SQL. Per altre informazioni, vedere [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md). COLLATE può essere applicato solo alle colonne di tipo **char**, **varchar**, **nchar** e **nvarchar**.  
  
 **ASC** | DESC  
 Specifica che i valori nella colonna specificata devono essere ordinati in ordine crescente o decrescente. ASC consente di ordinare i valori dal più piccolo al più grande. DESC consente di ordinare i valori dal più grande al più piccolo. ASC è l'ordinamento predefinito. I valori Null vengono considerati i valori in assoluto più piccoli.  
  
 OFFSET { *integer_constant* | *offset_row_count_expression* } { ROW | ROWS }  
 Specifica il numero di righe da ignorare prima della restituzione delle righe dall'espressione di query. Il valore può essere un valore costante intero o un'espressione maggiore o uguale a zero.  
  
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 *offset_row_count_expression* può essere una variabile, un parametro o una sottoquery scalare costante. Quando viene utilizzata una sottoquery, non è possibile fare riferimento alle colonne definite nell'ambito di query esterno. Ciò significa che non è possibile metterla in correlazione con la query esterna.  
  
 ROW e ROWS sono sinonimi e vengono specificati per garantire la compatibilità ANSI.  
  
 Nei piani di esecuzione della query, il valore del conteggio delle righe di offset viene visualizzato nell'attributo **Offset** dell'operatore di query TOP.  
  
 FETCH { FIRST \| NEXT } { *integer_constant* \| *fetch_row_count_expression* } { ROW \| ROWS } ONLY  
 Specifica il numero di righe da restituire dopo l'elaborazione della clausola OFFSET. Il valore può essere un valore costante intero o un'espressione maggiore o uguale a uno.  
  
**SI APPLICA A**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 *fetch_row_count_expression* può essere una variabile, un parametro o una sottoquery scalare costante. Quando viene utilizzata una sottoquery, non è possibile fare riferimento alle colonne definite nell'ambito di query esterno. Ciò significa che non è possibile metterla in correlazione con la query esterna.  
  
 FIRST e NEXT sono sinonimi e vengono specificati per garantire la compatibilità ANSI.  
  
 ROW e ROWS sono sinonimi e vengono specificati per garantire la compatibilità ANSI.  
  
 Nei piani di esecuzione della query, il valore del conteggio delle righe di offset viene visualizzato nell'attributo **Rows** o **Top** dell'operatore di query TOP.  
  
## <a name="best-practices"></a>Procedure consigliate  
 Evitare di specificare numeri interi nella clausola ORDER BY come rappresentazioni posizionali delle colonne nell'elenco di selezione. Ad esempio, anche se un'istruzione quale `SELECT ProductID, Name FROM Production.Production ORDER BY 2` è valida, l'istruzione non risulta comprensibile come la specifica del nome della colonna effettivo. Inoltre, le modifiche all'elenco di selezione, ad esempio la modifica dell'ordine delle colonne o l'aggiunta di nuove colonne, richiedono la modifica della clausola ORDER BY per evitare risultati imprevisti.  
  
 In un'istruzione SELECT TOP ((*N*) usare sempre una clausola ORDER BY. È l'unico modo per indicare prevedibilmente quali righe sono interessate dalla clausola TOP. Per altre informazioni, vedere [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md).  
  
## <a name="interoperability"></a>Interoperabilità  
 Quando viene usato con un oggetto SELECT... INTO o ins... Istruzione SELECT per inserire righe da un'altra origine, la clausola ORDER BY non garantisce che le righe vengano inserite nell'ordine specificato.  
  
 L'utilizzo di OFFSET e FETCH in una vista non modifica la proprietà updateability della vista.  
  
## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni  
 Non vi è limite al numero di colonne nella clausola ORDER BY. Tuttavia, la dimensione totale delle colonne specificata in una clausola ORDER BY non può superare 8.060 byte.  
  
 Le colonne di tipo **ntesto**, **testo**, **immagine**, **geografia**, **geometria** e **xml** non possono essere usate in una clausola ORDER BY.  
  
 Non è possibile specificare un valore intero o una costante quando *order_by_expression* è incluso in una funzione di rango. Per altre informazioni, vedere [Clausola OVER - &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
 Se per un nome di tabella sono stati creati alias nella clausola FROM, è possibile utilizzare solo il nome dell'alias per qualificare le relative colonne nella clausola ORDER BY.  
  
 I nomi e gli alias delle colonne specificati nella clausola ORDER BY devono essere definiti nell'elenco di selezione se l'istruzione SELECT contiene una delle clausole o operatori seguenti:  
  
-   operatore UNION  
  
-   operatore EXCEPT  
  
-   operatore INTERSECT  
  
-   SELECT DISTINCT  
  
 Inoltre, quando l'istruzione include un operatore UNION, EXCEPT o INTERSECT, i nomi o gli alias delle colonne devono essere specificati nell'elenco di selezione della prima query (lato sinistro).  
  
 In una query che utilizza l'operatore UNION, EXCEPT o INTERSECT, la clausola ORDER BY è consentita solo alla fine dell'istruzione. Questa restrizione si applica unicamente ai casi in cui UNION, EXCEPT e INTERSECT vengono specificati in una query di livello principale e non in una sottoquery. Vedere la sezione Esempi riportata di seguito.  
  
 La clausola ORDER BY non è valida nelle viste, nelle funzioni inline, nelle tabelle derivate e nelle sottoquery, a meno che non vengano specificate anche la clausola TOP o le clausole OFFSET e FETCH. Quando si utilizza ORDER BY in questi oggetti, la clausola viene utilizzata esclusivamente per determinare le righe restituite dalla clausola TOP o dalle clausole OFFSET e FETCH. La clausola ORDER BY non garantisce risultati ordinati in caso di query su questi costrutti, a meno che tale clausola non venga specificata anche nella query.  
  
 OFFSET e FETCH non sono supportate nelle viste indicizzate o in una vista definita tramite la clausola CHECK OPTION.  
  
 OFFSET e FETCH possono essere utilizzate in qualsiasi query in cui le clausole TOP e ORDER BY siano consentite con le limitazioni seguenti:  
  
-   La clausola OVER non supporta OFFSET e FETCH.  
  
-   Non è possibile specificare OFFSET e FETCH direttamente nelle istruzioni INSERT, UPDATE, MERGE e DELETE, ma possono essere specificate in una sottoquery definita in tali istruzioni. Ad esempio, nell'istruzione INSERT INTO SELECT, è possibile specificare OFFSET e FETCH nell'istruzione SELECT.  
  
-   In una query che utilizza l'operatore UNION, EXCEPT o INTERSECT, è possibile specificare OFFSET e FETCH solo nella query finale che specifica l'ordine dei risultati della query.  
  
-   Non è possibile combinare TOP con OFFSET e FETCH nella stessa espressione di query (nello stesso ambito della query).  
  
## <a name="using-offset-and-fetch-to-limit-the-rows-returned"></a>Utilizzo di OFFSET e FETCH per limitare le righe restituite  
 È consigliabile utilizzare le clausole OFFSET e FETCH anziché la clausola TOP per implementare una soluzione di paging di query e limitare il numero di righe inviate a un'applicazione client.  
  
 L'utilizzo di OFFSET e FETCH come soluzione di paging richiede di eseguire la query una volta per ogni "pagina" di dati restituita all'applicazione client. Ad esempio, per restituire i risultati di una query in incrementi di 10 righe, è necessario eseguire la query una volta per restituire le righe da 1 a 10 e quindi eseguirla nuovamente per restituire le righe da 11 a 20 e così via. Ogni query è indipendente e non correlata in alcun modo all'altra. Ciò significa che, contrariamente all'utilizzo di un cursore in cui viene eseguita la query e lo stato viene gestito nel server, l'applicazione client è responsabile del rilevamento dello stato. Per ottenere risultati stabili tra richieste di query utilizzando OFFSET e FETCH, è necessario soddisfare le condizioni seguenti:  
  
1.  I dati sottostanti utilizzati dalla query non devono cambiare. Ciò significa che le righe interessate dalla query non vengono aggiornate o che tutte le richieste di pagine dalla query vengono eseguite in una singola transazione utilizzando l'isolamento dello snapshot o della transazione serializzabile. Per altre informazioni sui livelli di isolamento di questa transazione, vedere [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md).  
  
2.  La clausola ORDER BY contiene una colonna o una combinazione di colonne di cui è garantita l'univocità.  
  
 Vedere l'esempio "Esecuzione di più query in una singola transazione" nella sezione Esempi più avanti in questo argomento.  
  
 Se piani di esecuzione coerenti sono importanti nella soluzione di paging, si consideri l'utilizzo dell'hint per la query OPTIMIZE FOR per i parametri OFFSET e FETCH. Vedere "Specifica di espressioni per valori OFFSET e FETCH" nella sezione Esempi più avanti in questo argomento. Per altre informazioni su OPTIMZE FOR, vedere [Hint per la query &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
  
## <a name="examples"></a>Esempi  
  
|Category|Elementi di sintassi inclusi|  
|--------------|------------------------------|  
|[Sintassi di base](#BasicSyntax)|ORDER BY|  
|[Specifica dell'ordine crescente e decrescente](#SortOrder)|DESC • ASC|  
|[Specifica di regole di confronto](#Collation)|COLLATE|  
|[Specifica di un ordine condizionale](#Case)|Espressione CASE|  
|[Utilizzo di ORDER BY in una funzione di rango](#Rank)|Funzioni di rango|  
|[Limitazione del numero di righe restituite](#Offset)|OFFSET • FETCH|  
|[Utilizzo di ORDER BY con UNION, EXCEPT e INTERSECT](#Union)|UNION|  
  
###  <a name="basic-syntax"></a><a name="BasicSyntax"></a> Sintassi di base  
 Negli esempi contenuti in questa sezione vengono illustrate le funzionalità di base della clausola ORDER BY utilizzando la sintassi minima necessaria.  
  
#### <a name="a-specifying-a-single-column-defined-in-the-select-list"></a>R. Specifica di una singola colonna definita nell'elenco di selezione  
 Nell'esempio seguente il set di risultati viene ordinato in base alla colonna `ProductID` numerica. Poiché non viene specificato un ordinamento specifico, viene utilizzata l'impostazione predefinita (ordine crescente).  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ProductID, Name FROM Production.Product  
WHERE Name LIKE 'Lock Washer%'  
ORDER BY ProductID;  
```  
  
#### <a name="b-specifying-a-column-that-is-not-defined-in-the-select-list"></a>B. Specifica di una colonna non definita nell'elenco di selezione  
 Nell'esempio seguente il set di risultati viene ordinato in base a una colonna non inclusa nell'elenco di selezione, ma viene definito nella tabella specificata nella clausola FROM.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ProductID, Name, Color  
FROM Production.Product  
ORDER BY ListPrice;  
```  
  
#### <a name="c-specifying-an-alias-as-the-sort-column"></a>C. Specifica di un alias come colonna di ordinamento  
 Nell'esempio seguente l'alias della colonna `SchemaName` viene specificato come colonna di ordinamento.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT name, SCHEMA_NAME(schema_id) AS SchemaName  
FROM sys.objects  
WHERE type = 'U'  
ORDER BY SchemaName;  
```  
  
#### <a name="d-specifying-an-expression-as-the-sort-column"></a>D. Specifica di un'espressione come colonna di ordinamento  
 Nell'esempio seguente viene utilizzata un'espressione come colonna di ordinamento. L'espressione viene definita tramite la funzione DATEPART per ordinare il set di risultati in base all'anno in cui sono stati assunti i dipendenti.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT BusinessEntityID, JobTitle, HireDate  
FROM HumanResources.Employee  
ORDER BY DATEPART(year, HireDate);  
```  
  
###  <a name="specifying-ascending-and-descending-sort-order"></a><a name="SortOrder"></a> Specifica dell'ordinamento crescente e decrescente  
  
#### <a name="a-specifying-a-descending-order"></a>R. Specifica di un ordine decrescente  
 Nell'esempio seguente il set di risultati viene ordinato in base alla colonna numerica `ProductID` in ordine decrescente.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ProductID, Name FROM Production.Product  
WHERE Name LIKE 'Lock Washer%'  
ORDER BY ProductID DESC;  
```  
  
#### <a name="b-specifying-an-ascending-order"></a>B. Specifica di un ordine crescente  
 Nell'esempio seguente il set di risultati viene ordinato in base alla colonna `Name` in ordine crescente. I caratteri sono ordinati alfabeticamente, non numericamente. Di conseguenza, 10 precede 2.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ProductID, Name FROM Production.Product  
WHERE Name LIKE 'Lock Washer%'  
ORDER BY Name ASC ;  
```  
  
#### <a name="c-specifying-both-ascending-and-descending-order"></a>C. Specifica di entrambi gli ordini, crescente e decrescente  
 Nell'esempio seguente il set di risultati viene ordinato in base a due colonne. Il set di risultati della query viene ordinato prima in ordine crescente in base alla colonna `FirstName` e quindi in ordine decrescente in base alla colonna `LastName`.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT LastName, FirstName FROM Person.Person  
WHERE LastName LIKE 'R%'  
ORDER BY FirstName ASC, LastName DESC ;  
```  
  
###  <a name="specifying-a-collation"></a><a name="Collation"></a> Specifica di regole di confronto  
 Nell'esempio seguente viene illustrato come la specifica di regole di confronto nella clausola ORDER BY possa modificare l'ordine in cui vengono restituiti i risultati della query. Viene creata una tabella contenente una colonna definita tramite regole di confronto che non fanno distinzione tra maiuscole e minuscole e tra i vari accenti. I valori vengono inseriti con una varietà di differenze di caso e accento. Poiché nella clausola ORDER BY non vengono specificate regole di confronto, la prima query utilizza le regole di confronto della colonna quando si ordinano i valori. Nella seconda query, nella clausola ORDER BY, vengono specificate regole di confronto che fanno distinzione tra maiuscole e minuscole e tra gli accenti che comportano la modifica dell'ordine in cui vengono restituite le righe.  
  
```sql
USE tempdb;  
GO  
CREATE TABLE #t1 (name NVARCHAR(15) COLLATE Latin1_General_CI_AI)  
GO  
INSERT INTO #t1 VALUES(N'Sánchez'),(N'Sanchez'),(N'sánchez'),(N'sanchez');  
  
-- This query uses the collation specified for the column 'name' for sorting.  
SELECT name  
FROM #t1  
ORDER BY name;  
-- This query uses the collation specified in the ORDER BY clause for sorting.  
SELECT name  
FROM #t1  
ORDER BY name COLLATE Latin1_General_CS_AS;  
```  
  
###  <a name="specifying-a-conditional-order"></a><a name="Case"></a> Specifica di un ordine condizionale  
 Negli esempi seguenti viene usata l'espressione CASE in una clausola ORDER BY per determinare in modo condizionale l'ordinamento delle righe in base a un valore di colonna specificato. Nel primo esempio, viene calcolato il valore nella colonna `SalariedFlag` della tabella `HumanResources.Employee`. I dipendenti per cui `SalariedFlag` è impostato su 1 vengono restituiti in ordine decrescente in base a `BusinessEntityID`. I dipendenti per cui `SalariedFlag` è impostato su 0 vengono restituiti in ordine crescente in base a `BusinessEntityID`. Nel secondo esempio il set di risultati viene ordinato in base alla colonna `TerritoryName` quando la colonna `CountryRegionName` è uguale a 'Stati Uniti' e in base a `CountryRegionName` per tutte le altre righe.  
  
```sql
SELECT BusinessEntityID, SalariedFlag  
FROM HumanResources.Employee  
ORDER BY CASE SalariedFlag WHEN 1 THEN BusinessEntityID END DESC  
        ,CASE WHEN SalariedFlag = 0 THEN BusinessEntityID END;  
GO  
```  
  
```sql
SELECT BusinessEntityID, LastName, TerritoryName, CountryRegionName  
FROM Sales.vSalesPerson  
WHERE TerritoryName IS NOT NULL  
ORDER BY CASE CountryRegionName WHEN 'United States' THEN TerritoryName  
         ELSE CountryRegionName END;  
```  
  
###  <a name="using-order-by-in-a-ranking-function"></a><a name="Rank"></a> Utilizzo di ORDER BY in una funzione di rango  
 Nell'esempio seguente la clausola ORDER BY viene utilizzata nelle funzioni di rango ROW_NUMBER, RANK, DENSE_RANK e NTILE.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT p.FirstName, p.LastName  
    ,ROW_NUMBER() OVER (ORDER BY a.PostalCode) AS "Row Number"  
    ,RANK() OVER (ORDER BY a.PostalCode) AS "Rank"  
    ,DENSE_RANK() OVER (ORDER BY a.PostalCode) AS "Dense Rank"  
    ,NTILE(4) OVER (ORDER BY a.PostalCode) AS "Quartile"  
    ,s.SalesYTD, a.PostalCode  
FROM Sales.SalesPerson AS s   
    INNER JOIN Person.Person AS p   
        ON s.BusinessEntityID = p.BusinessEntityID  
    INNER JOIN Person.Address AS a   
        ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL AND SalesYTD <> 0;  
```  
  
###  <a name="limiting-the-number-of-rows-returned"></a><a name="Offset"></a> Limitazione del numero di righe restituite  
 Negli esempi seguenti vengono utilizzate le clausole OFFSET e FETCH per limitare il numero di righe restituite da una query.  
  
**SI APPLICA A**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
#### <a name="a-specifying-integer-constants-for-offset-and-fetch-values"></a>R. Specifica di valori costanti interi per OFFSET e FETCH  
 Nell'esempio seguente viene specificata una costante intera come valore per le clausole OFFSET e FETCH. La prima query restituisce tutte le righe ordinate in base alla colonna `DepartmentID`. Confrontare i risultati restituiti da questa query ai risultati delle due query che la seguono. La query successiva utilizza la clausola `OFFSET 5 ROWS` per ignorare le prime 5 righe e restituire tutte le righe rimanenti. Nella query finale viene utilizzata la clausola `OFFSET 0 ROWS` per iniziare con la prima riga, quindi `FETCH NEXT 10 ROWS ONLY` per limitare le righe restituite a 10 righe dal set di risultati ordinato.  
  
```sql
USE AdventureWorks2012;  
GO  
-- Return all rows sorted by the column DepartmentID.  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID;  
  
-- Skip the first 5 rows from the sorted result set and return all remaining rows.  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID OFFSET 5 ROWS;  
  
-- Skip 0 rows and return only the first 10 rows from the sorted result set.  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID   
    OFFSET 0 ROWS  
    FETCH NEXT 10 ROWS ONLY;  
```  
  
#### <a name="b-specifying-variables-for-offset-and-fetch-values"></a>B. Specifica di variabili per i valori OFFSET e FETCH  
 Nell'esempio seguente vengono dichiarate le variabili `@RowsToSkip` e `@FetchRows` che vengono in seguito specificate nelle clausole OFFSET e FETCH.  
  
```sql
USE AdventureWorks2012;  
GO  
-- Specifying variables for OFFSET and FETCH values    
DECLARE @RowsToSkip TINYINT = 2
      , @FetchRows TINYINT = 8;  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID ASC   
    OFFSET @RowsToSkip ROWS   
    FETCH NEXT @FetchRows ROWS ONLY;  
```  
  
#### <a name="c-specifying-expressions-for-offset-and-fetch-values"></a>C. Specifica di espressioni per i valori OFFSET e FETCH  
 Nell'esempio seguente vengono utilizzate l'espressione `@StartingRowNumber - 1` per specificare il valore OFFSET e l'espressione `@EndingRowNumber - @StartingRowNumber + 1` per specificare il valore FETCH. Viene inoltre specificato l'hint per la query OPTIMIZE FOR. Questo hint può essere utilizzato per specificare un valore per una variabile locale quando la query viene compilata e ottimizzata. Il valore viene utilizzato solo durante l'ottimizzazione della query e non durante l'esecuzione. Per altre informazioni, vedere [Hint per la query &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
  
```sql
USE AdventureWorks2012;  
GO  
  
-- Specifying expressions for OFFSET and FETCH values      
DECLARE @StartingRowNumber TINYINT = 1  
      , @EndingRowNumber TINYINT = 8;  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID ASC   
    OFFSET @StartingRowNumber - 1 ROWS   
    FETCH NEXT @EndingRowNumber - @StartingRowNumber + 1 ROWS ONLY  
OPTION ( OPTIMIZE FOR (@StartingRowNumber = 1, @EndingRowNumber = 20) );  
```  
  
#### <a name="d-specifying-a-constant-scalar-subquery-for-offset-and-fetch-values"></a>D. Specifica di una sottoquery scalare costante per valori OFFSET e FETCH  
 Nell'esempio seguente viene utilizzata una sottoquery scalare costante per definire il valore per la clausola FETCH. La sottoquery restituisce un singolo valore dalla colonna `PageSize` nella tabella `dbo.AppSettings`.  
  
```sql
-- Specifying a constant scalar subquery  
USE AdventureWorks2012;  
GO  
CREATE TABLE dbo.AppSettings (AppSettingID INT NOT NULL, PageSize INT NOT NULL);  
GO  
INSERT INTO dbo.AppSettings VALUES(1, 10);  
GO  
DECLARE @StartingRowNumber TINYINT = 1;  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID ASC   
    OFFSET @StartingRowNumber ROWS   
    FETCH NEXT (SELECT PageSize FROM dbo.AppSettings WHERE AppSettingID = 1) ROWS ONLY;  
```  
  
#### <a name="e-running-multiple-queries-in-a-single-transaction"></a>E. Esecuzione di più query in una singola transazione  
 Nell'esempio seguente viene illustrato un metodo per l'implementazione di una soluzione di paging che garantisca la restituzione di risultati stabili in tutte le richieste dalla query. La query viene eseguita in una singola transazione utilizzando il livello di isolamento dello snapshot e la colonna specificata nella clausola ORDER BY garantisce l'univocità della colonna.  
  
```sql
USE AdventureWorks2012;  
GO  
  
-- Ensure the database can support the snapshot isolation level set for the query.  
IF (SELECT snapshot_isolation_state FROM sys.databases WHERE name = N'AdventureWorks2012') = 0  
    ALTER DATABASE AdventureWorks2012 SET ALLOW_SNAPSHOT_ISOLATION ON;  
GO  
  
-- Set the transaction isolation level  to SNAPSHOT for this query.  
SET TRANSACTION ISOLATION LEVEL SNAPSHOT;  
GO  
  
-- Beginning the transaction.
BEGIN TRANSACTION;  
GO  
-- Declare and set the variables for the OFFSET and FETCH values.  
DECLARE @StartingRowNumber INT = 1  
      , @RowCountPerPage INT = 3;  
  
-- Create the condition to stop the transaction after all rows have been returned.  
WHILE (SELECT COUNT(*) FROM HumanResources.Department) >= @StartingRowNumber  
BEGIN  
  
-- Run the query until the stop condition is met.  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID ASC   
    OFFSET @StartingRowNumber - 1 ROWS   
    FETCH NEXT @RowCountPerPage ROWS ONLY;  
  
-- Increment @StartingRowNumber value.  
SET @StartingRowNumber = @StartingRowNumber + @RowCountPerPage;  
CONTINUE  
END;  
GO  
COMMIT TRANSACTION;  
GO  
```  
  
###  <a name="using-order-by-with-union-except-and-intersect"></a><a name="Union"></a> Utilizzo di ORDER BY con UNION, EXCEPT e INTERSECT  
 Quando una query utilizza l'operatore UNION, EXCEPT o INTERSECT, è necessario specificare la clausola ORDER BY alla fine dell'istruzione e i risultati delle query combinate vengono ordinati. Nell'esempio seguente vengono restituiti tutti i prodotti rossi o gialli e questo elenco combinato viene ordinato in base alla colonna `ListPrice`.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT Name, Color, ListPrice  
FROM Production.Product  
WHERE Color = 'Red'  
-- ORDER BY cannot be specified here.  
UNION ALL  
SELECT Name, Color, ListPrice  
FROM Production.Product  
WHERE Color = 'Yellow'  
ORDER BY ListPrice ASC;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 Nell'esempio seguente viene illustrato l'ordinamento di un set di risultati in base alla colonna `EmployeeKey` numerica in ordine crescente.  
  
```sql
-- Uses AdventureWorks  
  
SELECT EmployeeKey, FirstName, LastName FROM DimEmployee  
WHERE LastName LIKE 'A%'  
ORDER BY EmployeeKey;  
```  
  
 Nell'esempio seguente un set di risultati viene ordinato in base alla colonna numerica `EmployeeKey` in ordine decrescente.  
  
```sql
-- Uses AdventureWorks  
  
SELECT EmployeeKey, FirstName, LastName FROM DimEmployee  
WHERE LastName LIKE 'A%'  
ORDER BY EmployeeKey DESC;  
```  
  
 Nell'esempio seguente il set di risultati viene ordinato in base alla colonna `LastName`.  
  
```sql
-- Uses AdventureWorks  
  
SELECT EmployeeKey, FirstName, LastName FROM DimEmployee  
WHERE LastName LIKE 'A%'  
ORDER BY LastName;  
```  
  
 Nell'esempio seguente l'ordinamento viene fatto in base a due colonne. Questa query esegue l'ordinamento crescente in base alla colonna `FirstName`, quindi esegue un ordinamento decrescente dei valori `FirstName` comuni in base alla colonna `LastName`.  
  
```sql
-- Uses AdventureWorks  
  
SELECT EmployeeKey, FirstName, LastName FROM DimEmployee  
WHERE LastName LIKE 'A%'  
ORDER BY LastName, FirstName;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Espressioni &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)   
 [Funzioni di rango &#40;Transact-SQL&#41;](../../t-sql/functions/ranking-functions-transact-sql.md)   
 [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md)   
 [Hint di query &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md)   
 [EXCEPT e INTERSECT &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-operators-except-and-intersect-transact-sql.md)   
 [UNION &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-operators-union-transact-sql.md)   
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)  
  
  

