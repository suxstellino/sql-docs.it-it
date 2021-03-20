---
title: WITH common_table_expression (Transact-SQL) | Microsoft Docs
description: Informazioni di riferimento Transact-SQL per l'uso delle espressioni di tabella comuni (CTE) nelle query.
ms.date: 08/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- WITH common_table_expression
- WITH_TSQL
- WITH
- common table expression
dev_langs:
- TSQL
helpviewer_keywords:
- WITH common_table_expression clause
- CTEs
- hierarchical queries [SQL Server], WITH common_table_expression
- recursive CTEs [SQL Server]
- recursive queries [SQL Server]
- common table expressions
- MAXRECURSION hint
- clauses [SQL Server], WITH common_table_expression
ms.assetid: 27cfb819-3e8d-4274-8bbe-cbbe4d9c2e23
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 44ebd366a78ed0f0da4da6a5769ed31e4481e0e8
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753671"
---
# <a name="with-common_table_expression-transact-sql"></a>WITH common_table_expression (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Indica un set di risultati denominato temporaneo, noto come espressione di tabella comune (CTE). Questa deriva da una query semplice e viene definita all'interno dell'ambito di esecuzione di una singola istruzione SELECT, INSERT, UPDATE, DELETE o MERGE. Questa clausola può anche essere utilizzata in un'istruzione CREATE VIEW come parte dell'istruzione di definizione SELECT. Un'espressione di tabella comune può includere riferimenti a se stessa. In questo caso viene indicata con il nome di espressione di tabella comune ricorsiva.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
[ WITH <common_table_expression> [ ,...n ] ]  
  
<common_table_expression>::=  
    expression_name [ ( column_name [ ,...n ] ) ]  
    AS  
    ( CTE_query_definition )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *expression_name*  
Identificatore valido per l'espressione di tabella comune. *expression_name* deve essere diverso dal nome di qualsiasi altra espressione di tabella comune definita nella stessa clausola WITH \<common_table_expression>, ma *expression_name* può corrispondere al nome di una vista o tabella di base. Tutti i riferimenti a *expression_name* nella query usano l'espressione di tabella comune e non l'oggetto di base.
  
 *column_name*  
 Specifica un nome di colonna nell'espressione di tabella comune. Non sono consentiti nomi duplicati all'interno di una singola definizione CTE. Il numero dei nomi di colonna specificato deve corrispondere al numero delle colonne nel set di risultati di *CTE_query_definition*. L'elenco dei nomi di colonna è facoltativo solo se i nomi distinti di tutte le colonne risultanti sono specificati nella definizione della query.  
  
 *CTE_query_definition*  
 Specifica un'istruzione SELECT il cui set di risultati popola l'espressione di tabella comune. L'istruzione SELECT per *CTE_query_definition* deve soddisfare gli stessi requisiti necessari per creare una vista, con la differenza che una CTE non può definire un'altra CTE. Per altre informazioni, vedere la sezione Osservazioni e [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md).  
  
 Se si definiscono più elementi *CTE_query_definition*, è necessario creare un join delle definizioni di query in base a uno dei seguenti operatori sui set: UNION ALL, UNION, EXCEPT o INTERSECT.  
  
## <a name="remarks"></a>Osservazioni  
  
## <a name="guidelines-for-creating-and-using-common-table-expressions"></a>Linee guida per la creazione e l'utilizzo delle espressioni di tabella comuni  
Le linee guida seguenti sono valide per le espressioni di tabella comuni non ricorsive. Per le linee guida relative alle espressioni di tabella comuni ricorsive, vedere [Linee guida per la definizione e l'utilizzo delle espressioni di tabella comuni ricorsive](#guidelines-for-defining-and-using-recursive-common-table-expressions) più avanti.  
  
-   Una CTE deve essere seguita da un'istruzione `SELECT`, `INSERT`, `UPDATE` o `DELETE` singola che faccia riferimento ad alcune o a tutte le colonne della CTE. Una CTE può anche essere specificata in un'istruzione `CREATE VIEW` come parte dell'istruzione di definizione `SELECT` della vista.  
  
-   In una CTE non ricorsiva è possibile definire più query CTE. Le definizioni devono essere combinate da uno degli operatori sui set seguenti: `UNION ALL`, `UNION`, `INTERSECT`, o `EXCEPT`.  
  
-   Una CTE può far riferimento a se stessa e alle CTE definite in precedenza nella stessa clausola `WITH`. Il riferimento in avanti non è consentito.  
  
-   Non è consentito specificare più di una clausola WITH in una CTE. Se ad esempio una definizione *CTE_query_definition* include una sottoquery, tale sottoquery non può includere una clausola WITH annidata che definisce un'altra CTE.  
  
-   Le clausole seguenti non possono essere usate in *CTE_query_definition*:  
  
    -   `ORDER BY` (tranne quando si specifica una clausola `TOP`)  
  
    -   `INTO`  
  
    -   Clausola `OPTION` con hint per la query  
  
    -   `FOR BROWSE`  
  
-   Quando un'espressione CTE viene utilizzata in un'istruzione che fa parte di un batch, l'istruzione precedente deve essere seguita da un punto e virgola.  
  
-   Una query che fa riferimento a un'espressione CTE può essere utilizzata per definire un cursore.  
  
-   L'espressione CTE può fare riferimento alle tabelle nei server remoti.  
  
-   Durante l'esecuzione di una CTE, tutti gli hint che fanno riferimento a una CTE possono entrare in conflitto con altri hint individuati quando la CTE accede alle tabelle sottostanti, allo stesso modo degli hint che fanno riferimento alle viste nelle query. In questo caso, la query restituisce un errore.  
  
## <a name="guidelines-for-defining-and-using-recursive-common-table-expressions"></a>Linee guida per la definizione e l'utilizzo delle espressioni di tabella comuni ricorsive  
 Le linee guida seguenti sono valide per la definizione delle espressioni di tabella comuni ricorsive.  
  
-   La definizione CTE ricorsiva deve contenere almeno due definizioni di query CTE, un membro non ricorsivo e un membro ricorsivo. È possibile definire più membri ricorsivi e non ricorsivi, ma tutte le definizioni delle query dei membri non ricorsivi devono precedere la definizione del primo membro ricorsivo. Tutte le definizioni delle query CTE sono membri non ricorsivi tranne nei casi un cui fanno riferimento all'espressione CTE stessa.  
  
-   I membri non ricorsivi devono essere combinati da uno degli operatori sui set seguenti: UNION ALL, UNION, INTERSECT o EXCEPT. UNION ALL è l'unico operatore sui set consentito tra l'ultimo membro non ricorsivo e il primo membro ricorsivo, nonché durante la combinazione di più membri ricorsivi.  
  
-   Il numero delle colonne nei membri ricorsivi e non ricorsivi deve essere lo stesso.  
  
-   Il tipo di dati di una colonna nel membro ricorsivo deve essere lo stesso del tipo di dati della colonna corrispondente nel membro non ricorsivo.  
  
-   La clausola FROM di un membro ricorsivo deve fare riferimento solo una volta all'espressione CTE *expression_name*.  
  
-   Gli elementi seguenti non sono consentiti nella definizione *CTE_query_definition* di un membro ricorsivo:  
  
    -   `SELECT DISTINCT`  
  
    -   `GROUP BY`  
  
    -   `PIVOT` quando il livello di compatibilità del database è 110 o superiore. Vedere [Modifiche di rilievo apportate alle funzionalità del motore di database in SQL Server 2016](../../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md).  
  
    -   `HAVING`  
  
    -   Aggregazione scalare  
  
    -   `TOP`  
  
    -   `LEFT`, `RIGHT`, `OUTER JOIN` (`INNER JOIN` è consentita)  
  
    -   Sottoquery:  
  
    -   Hint applicato a un riferimento ricorsivo a un'espressione CTE all'interno di una definizione *CTE_query_definition*.  
  
 Le linee guida seguenti sono valide per l'utilizzo delle espressioni di tabella comuni ricorsive.  
  
-   Tutte le colonne restituite dalla CTE ricorsiva ammettono valori Null a prescindere dal supporto dei valori Null delle colonne restituite dalle istruzioni `SELECT` coinvolte.  
  
-   Una CTE ricorsiva formulata in modo non corretto può provocare un ciclo infinito. Ad esempio, se la definizione della query del membro ricorsivo restituisce gli stessi valori per entrambe le colonne padre e figlio, si crea un ciclo infinito. Per evitare un ciclo infinito, è possibile limitare il numero di livelli di ricorsione consentito per una particolare espressione usando l'hint `MAXRECURSION` e un valore compreso tra 0 e 32.767 nella clausola OPTION dell'istruzione `INSERT`, `UPDATE`, `DELETE`, o `SELECT`. Ciò consente di controllare l'esecuzione dell'istruzione fino a quando non viene risolto il problema relativo al codice che sta creando il ciclo. Il valore predefinito per l'intero server è 100. Se è specificato 0, non viene applicato alcun limite. È possibile specificare solo un valore `MAXRECURSION` per istruzione. Per altre informazioni, vedere [Hint per la query &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
  
-   Non è possibile utilizzare una vista che contiene un'espressione di tabella comune ricorsiva per aggiornare i dati.  
  
-   I cursori possono essere definiti sulle query tramite CTE. La CTE corrisponde all'argomento *select_statement* che definisce il set di risultati del cursore. Sono consentiti solo i cursori fast forward only e statici (snapshot) per le CTE ricorsive. Se viene specificato un altro tipo di cursore in una CTE ricorsiva, il tipo di cursore viene convertito in statico.  
  
-   Nelle CTE è possibile far riferimento alle tabelle nei server remoti. Se nel membro ricorsivo della CTE si fa riferimento al server remoto, viene creato uno spool per ogni tabella remota in maniera che si possa accedere alle tabelle in modo locale ripetutamente. Se la query è di tipo CTE, Index Spool o Lazy Spool viene visualizzato nel piano di query con il predicato aggiuntivo `WITH STACK` associato. Questo è uno dei modi utilizzati per confermare una ricorsione appropriata.  
  
-   Le funzioni analitiche e di aggregazione nella parte ricorsiva dell'espressione CTE vengono applicate al set per il livello di ricorsione corrente, non al set per l'espressione CTE. Le funzioni come `ROW_NUMBER` funzionano solo nel subset di dati passato dal livello di ricorsione corrente e non nell'intero set di dati passato alla parte ricorsiva della CTE. Per altre informazioni, vedere l'esempio K, Utilizzo di funzioni analitiche in un'espressione CTE ricorsiva, più avanti.  
  
## <a name="features-and-limitations-of-common-table-expressions-in-sssdw-and-sspdw"></a>Funzionalità e limitazioni delle espressioni di tabella comuni in [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 L'implementazione corrente delle CTE in [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] presenta le caratteristiche e le limitazioni seguenti:  
  
-   Una CTE può essere specificata in un'istruzione `SELECT`.  
  
-   Una CTE può essere specificata in un'istruzione `CREATE VIEW`.  
  
-   Una CTE può essere specificata in un'istruzione `CREATE TABLE AS SELECT` (CTAS).  
  
-   Una CTE può essere specificata in un'istruzione `CREATE REMOTE TABLE AS SELECT` (CRTAS).  
  
-   Una CTE può essere specificata in un'istruzione `CREATE EXTERNAL TABLE AS SELECT` (CETAS).  
  
-   Una CTE può fare riferimento a una tabella remota.  
  
-   Una CTE può fare riferimento a una tabella esterna.  
  
-   In una CTE è possibile definire più query CTE.  
  
-   Una CTE deve essere seguita da un'unica istruzione `SELECT`. Le istruzioni `INSERT`, `UPDATE`, `DELETE` e `MERGE` non sono supportate.  
  
-   Le espressioni di tabella comuni che includono riferimenti a se stesse (espressioni di tabella comuni ricorsive) non sono supportate.  
  
-   Non è consentito specificare più di una clausola `WITH` in una CTE. Se ad esempio la definizione di una query CTE include una sottoquery, tale sottoquery non può includere una clausola `WITH` annidata che definisce un'altra CTE.  
  
-   Non è possibile usare una clausola `ORDER BY` in una definizione CTE_query_definition, tranne quando è specificata una clausola `TOP`.  
  
-   Quando un'espressione CTE viene utilizzata in un'istruzione che fa parte di un batch, l'istruzione precedente deve essere seguita da un punto e virgola.  
  
-   Se usate all'interno di istruzioni preparate da `sp_prepare`, le CTE si comportano allo stesso modo delle altre istruzioni `SELECT` in PDW. Se tuttavia le CTE vengono usate all'interno di CETAS preparate da `sp_prepare`, il comportamento può differire da quello di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e di altre istruzioni PDW a causa della modalità di implementazione dell'associazione per `sp_prepare`. Se l'istruzione `SELECT` che fa riferimento a una CTE usa una colonna non corretta che non esiste nella CTE, l'errore non viene rilevato durante l'esecuzione di `sp_prepare`, ma viene generato durante l'esecuzione di `sp_execute`.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-creating-a-simple-common-table-expression"></a>R. Creazione di un'espressione di tabella comune semplice  
 Nell'esempio seguente viene illustrato il numero totale di ordini di vendita all'anno per tutti i venditori di [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)].  
  
```sql   
-- Define the CTE expression name and column list.  
WITH Sales_CTE (SalesPersonID, SalesOrderID, SalesYear)  
AS  
-- Define the CTE query.  
(  
    SELECT SalesPersonID, SalesOrderID, YEAR(OrderDate) AS SalesYear  
    FROM Sales.SalesOrderHeader  
    WHERE SalesPersonID IS NOT NULL  
)  
-- Define the outer query referencing the CTE name.  
SELECT SalesPersonID, COUNT(SalesOrderID) AS TotalSales, SalesYear  
FROM Sales_CTE  
GROUP BY SalesYear, SalesPersonID  
ORDER BY SalesPersonID, SalesYear;  
```  
  
### <a name="b-using-a-common-table-expression-to-limit-counts-and-report-averages"></a>B. Utilizzo di un'espressione di tabella comune per limitare il numero medio di ordini  
 Nell'esempio seguente viene illustrato il numero medio di ordini di vendita all'anno per i venditori.  
  
```sql  
WITH Sales_CTE (SalesPersonID, NumberOfOrders)  
AS  
(  
    SELECT SalesPersonID, COUNT(*)  
    FROM Sales.SalesOrderHeader  
    WHERE SalesPersonID IS NOT NULL  
    GROUP BY SalesPersonID  
)  
SELECT AVG(NumberOfOrders) AS "Average Sales Per Person"  
FROM Sales_CTE;  
```  
  
### <a name="c-using-multiple-cte-definitions-in-a-single-query"></a>C. Utilizzo di più definizioni CTE in una singola query  
 Nell'esempio seguente viene illustrato come definire più di una CTE in una singola query. Si noti che per separare le definizioni di query CTE è utilizzata una virgola. La funzione FORMAT, utilizzata per visualizzare gli importi monetari in un formato di valuta, è disponibile in SQL Server 2012 e versioni successive.  
  
```sql  
WITH Sales_CTE (SalesPersonID, TotalSales, SalesYear)  
AS  
-- Define the first CTE query.  
(  
    SELECT SalesPersonID, SUM(TotalDue) AS TotalSales, YEAR(OrderDate) AS SalesYear  
    FROM Sales.SalesOrderHeader  
    WHERE SalesPersonID IS NOT NULL  
       GROUP BY SalesPersonID, YEAR(OrderDate)  
  
)  
,   -- Use a comma to separate multiple CTE definitions.  
  
-- Define the second CTE query, which returns sales quota data by year for each sales person.  
Sales_Quota_CTE (BusinessEntityID, SalesQuota, SalesQuotaYear)  
AS  
(  
       SELECT BusinessEntityID, SUM(SalesQuota)AS SalesQuota, YEAR(QuotaDate) AS SalesQuotaYear  
       FROM Sales.SalesPersonQuotaHistory  
       GROUP BY BusinessEntityID, YEAR(QuotaDate)  
)  
  
-- Define the outer query by referencing columns from both CTEs.  
SELECT SalesPersonID  
  , SalesYear  
  , FORMAT(TotalSales,'C','en-us') AS TotalSales  
  , SalesQuotaYear  
  , FORMAT (SalesQuota,'C','en-us') AS SalesQuota  
  , FORMAT (TotalSales -SalesQuota, 'C','en-us') AS Amt_Above_or_Below_Quota  
FROM Sales_CTE  
JOIN Sales_Quota_CTE ON Sales_Quota_CTE.BusinessEntityID = Sales_CTE.SalesPersonID  
                    AND Sales_CTE.SalesYear = Sales_Quota_CTE.SalesQuotaYear  
ORDER BY SalesPersonID, SalesYear;    
```  
  
Set di risultati parziale:  
  
```  
SalesPersonID SalesYear   TotalSales    SalesQuotaYear SalesQuota  Amt_Above_or_Below_Quota  
------------- ---------   -----------   -------------- ---------- ----------------------------------   
274           2005        $32,567.92    2005           $35,000.00  ($2,432.08)  
274           2006        $406,620.07   2006           $455,000.00 ($48,379.93)  
274           2007        $515,622.91   2007           $544,000.00 ($28,377.09)  
274           2008        $281,123.55   2008           $271,000.00  $10,123.55  
```  
  
### <a name="d-using-a-recursive-common-table-expression-to-display-multiple-levels-of-recursion"></a>D. Utilizzo di un'espressione di tabella comune ricorsiva per visualizzare più livelli di ricorsione  
 Nell'esempio seguente viene illustrato l'elenco gerarchico dei responsabili e dei dipendenti a loro subordinati. L'esempio inizia con la creazione e il popolamento della tabella `dbo.MyEmployees`.  
  
```sql  
-- Create an Employee table.  
CREATE TABLE dbo.MyEmployees  
(  
EmployeeID SMALLINT NOT NULL,  
FirstName NVARCHAR(30)  NOT NULL,  
LastName  NVARCHAR(40) NOT NULL,  
Title NVARCHAR(50) NOT NULL,  
DeptID SMALLINT NOT NULL,  
ManagerID INT NULL,  
 CONSTRAINT PK_EmployeeID PRIMARY KEY CLUSTERED (EmployeeID ASC)   
);  
-- Populate the table with values.  
INSERT INTO dbo.MyEmployees VALUES   
 (1, N'Ken', N'Sánchez', N'Chief Executive Officer',16,NULL)  
,(273, N'Brian', N'Welcker', N'Vice President of Sales',3,1)  
,(274, N'Stephen', N'Jiang', N'North American Sales Manager',3,273)  
,(275, N'Michael', N'Blythe', N'Sales Representative',3,274)  
,(276, N'Linda', N'Mitchell', N'Sales Representative',3,274)  
,(285, N'Syed', N'Abbas', N'Pacific Sales Manager',3,273)  
,(286, N'Lynn', N'Tsoflias', N'Sales Representative',3,285)  
,(16,  N'David',N'Bradley', N'Marketing Manager', 4, 273)  
,(23,  N'Mary', N'Gibson', N'Marketing Specialist', 4, 16);  
```  
  
```sql
WITH DirectReports(ManagerID, EmployeeID, Title, EmployeeLevel) AS   
(  
    SELECT ManagerID, EmployeeID, Title, 0 AS EmployeeLevel  
    FROM dbo.MyEmployees   
    WHERE ManagerID IS NULL  
    UNION ALL  
    SELECT e.ManagerID, e.EmployeeID, e.Title, EmployeeLevel + 1  
    FROM dbo.MyEmployees AS e  
        INNER JOIN DirectReports AS d  
        ON e.ManagerID = d.EmployeeID   
)  
SELECT ManagerID, EmployeeID, Title, EmployeeLevel   
FROM DirectReports  
ORDER BY ManagerID;   
```  
  
#### <a name="using-a-recursive-common-table-expression-to-display-two-levels-of-recursion"></a>Utilizzo di un'espressione di tabella comune ricorsiva per visualizzare due livelli di ricorsione  
 Nell'esempio seguente vengono illustrati i responsabili e i dipendenti che sono loro subordinati. Il numero di livelli restituiti è limitato a due.  
  
```sql
WITH DirectReports(ManagerID, EmployeeID, Title, EmployeeLevel) AS   
(  
    SELECT ManagerID, EmployeeID, Title, 0 AS EmployeeLevel  
    FROM dbo.MyEmployees   
    WHERE ManagerID IS NULL  
    UNION ALL  
    SELECT e.ManagerID, e.EmployeeID, e.Title, EmployeeLevel + 1  
    FROM dbo.MyEmployees AS e  
        INNER JOIN DirectReports AS d  
        ON e.ManagerID = d.EmployeeID   
)  
SELECT ManagerID, EmployeeID, Title, EmployeeLevel   
FROM DirectReports  
WHERE EmployeeLevel <= 2 ;  
```  
  
#### <a name="using-a-recursive-common-table-expression-to-display-a-hierarchical-list"></a>Utilizzo di un'espressione di tabella comune ricorsiva per visualizzare un elenco gerarchico  
 Nell'esempio seguente vengono aggiunti i nomi del responsabile e dei dipendenti e i loro rispettivi titoli. La gerarchia dei responsabili e dei dipendenti viene inoltre evidenziata rientrando ogni livello.  
  
```sql
WITH DirectReports(Name, Title, EmployeeID, EmployeeLevel, Sort)  
AS (SELECT CONVERT(VARCHAR(255), e.FirstName + ' ' + e.LastName),  
        e.Title,  
        e.EmployeeID,  
        1,  
        CONVERT(VARCHAR(255), e.FirstName + ' ' + e.LastName)  
    FROM dbo.MyEmployees AS e  
    WHERE e.ManagerID IS NULL  
    UNION ALL  
    SELECT CONVERT(VARCHAR(255), REPLICATE ('|    ' , EmployeeLevel) +  
        e.FirstName + ' ' + e.LastName),  
        e.Title,  
        e.EmployeeID,  
        EmployeeLevel + 1,  
        CONVERT (VARCHAR(255), RTRIM(Sort) + '|    ' + FirstName + ' ' +   
                 LastName)  
    FROM dbo.MyEmployees AS e  
    JOIN DirectReports AS d ON e.ManagerID = d.EmployeeID  
    )  
SELECT EmployeeID, Name, Title, EmployeeLevel  
FROM DirectReports   
ORDER BY Sort;  
```  
  
#### <a name="using-maxrecursion-to-cancel-a-statement"></a>Utilizzo di MAXRECURSION per annullare un'istruzione  
 È possibile utilizzare `MAXRECURSION` per impedire che una CTE ricorsiva non corretta provochi un ciclo infinito. Nell'esempio seguente viene creato intenzionalmente un ciclo infinito e viene utilizzato l'hint `MAXRECURSION` per limitare a due il numero di livelli di ricorsione.  
  
```sql
--Creates an infinite loop  
WITH cte (EmployeeID, ManagerID, Title) AS  
(  
    SELECT EmployeeID, ManagerID, Title  
    FROM dbo.MyEmployees  
    WHERE ManagerID IS NOT NULL  
  UNION ALL  
    SELECT cte.EmployeeID, cte.ManagerID, cte.Title  
    FROM cte   
    JOIN  dbo.MyEmployees AS e   
        ON cte.ManagerID = e.EmployeeID  
)  
--Uses MAXRECURSION to limit the recursive levels to 2  
SELECT EmployeeID, ManagerID, Title  
FROM cte  
OPTION (MAXRECURSION 2);  
```  
  
 Dopo la correzione dell'errore del codice, MAXRECURSION non è più necessario. Nell'esempio seguente viene illustrato il codice corretto.  
  
```sql
WITH cte (EmployeeID, ManagerID, Title)  
AS  
(  
    SELECT EmployeeID, ManagerID, Title  
    FROM dbo.MyEmployees  
    WHERE ManagerID IS NOT NULL  
  UNION ALL  
    SELECT  e.EmployeeID, e.ManagerID, e.Title  
    FROM dbo.MyEmployees AS e  
    JOIN cte ON e.ManagerID = cte.EmployeeID  
)  
SELECT EmployeeID, ManagerID, Title  
FROM cte;  
```  
  
### <a name="e-using-a-common-table-expression-to-selectively-step-through-a-recursive-relationship-in-a-select-statement"></a>E. Utilizzo di un'espressione di tabella comune per analizzare in maniera selettiva una relazione ricorsiva in un'istruzione SELECT  
 Nell'esempio seguente viene illustrata la gerarchia di assembly e componenti del prodotto che sono necessari per costruire la bicicletta per `ProductAssemblyID = 800`.  
  
```sql  
USE AdventureWorks2012;  
GO  
WITH Parts(AssemblyID, ComponentID, PerAssemblyQty, EndDate, ComponentLevel) AS  
(  
    SELECT b.ProductAssemblyID, b.ComponentID, b.PerAssemblyQty,  
        b.EndDate, 0 AS ComponentLevel  
    FROM Production.BillOfMaterials AS b  
    WHERE b.ProductAssemblyID = 800  
          AND b.EndDate IS NULL  
    UNION ALL  
    SELECT bom.ProductAssemblyID, bom.ComponentID, p.PerAssemblyQty,  
        bom.EndDate, ComponentLevel + 1  
    FROM Production.BillOfMaterials AS bom   
        INNER JOIN Parts AS p  
        ON bom.ProductAssemblyID = p.ComponentID  
        AND bom.EndDate IS NULL  
)  
SELECT AssemblyID, ComponentID, Name, PerAssemblyQty, EndDate,  
        ComponentLevel   
FROM Parts AS p  
    INNER JOIN Production.Product AS pr  
    ON p.ComponentID = pr.ProductID  
ORDER BY ComponentLevel, AssemblyID, ComponentID;  
```  
  
### <a name="f-using-a-recursive-cte-in-an-update-statement"></a>F. Utilizzo di una CTE ricorsiva in un'istruzione UPDATE  
 L'esempio seguente aggiorna il valore `PerAssemblyQty` per tutte le parti usate per costruire il prodotto 'Road-550-W Yellow, 44' `(ProductAssemblyID``800`). L'espressione di tabella comune restituisce un elenco gerarchico di parti utilizzate per compilare `ProductAssemblyID 800`, i componenti utilizzati per creare tali parti e così via. Vengono modificate solo le righe restituite dall'espressione di tabella comune.  
  
```sql  
USE AdventureWorks2012;  
GO  
WITH Parts(AssemblyID, ComponentID, PerAssemblyQty, EndDate, ComponentLevel) AS  
(  
    SELECT b.ProductAssemblyID, b.ComponentID, b.PerAssemblyQty,  
        b.EndDate, 0 AS ComponentLevel  
    FROM Production.BillOfMaterials AS b  
    WHERE b.ProductAssemblyID = 800  
          AND b.EndDate IS NULL  
    UNION ALL  
    SELECT bom.ProductAssemblyID, bom.ComponentID, p.PerAssemblyQty,  
        bom.EndDate, ComponentLevel + 1  
    FROM Production.BillOfMaterials AS bom   
        INNER JOIN Parts AS p  
        ON bom.ProductAssemblyID = p.ComponentID  
        AND bom.EndDate IS NULL  
)  
UPDATE Production.BillOfMaterials  
SET PerAssemblyQty = c.PerAssemblyQty * 2  
FROM Production.BillOfMaterials AS c  
JOIN Parts AS d ON c.ProductAssemblyID = d.AssemblyID  
WHERE d.ComponentLevel = 0;  
```  
  
### <a name="h-using-multiple-anchor-and-recursive-members"></a>H. Utilizzo di più membri ricorsivi e non ricorsivi  
 Nell'esempio seguente vengono utilizzati più membri ricorsivi e non ricorsivi per restituire tutti gli antenati di una specifica persona. Viene creata una tabella e vengono inseriti i valori per stabilire l'albero genealogico restituito dalla CTE ricorsiva.  
  
```sql  
-- Genealogy table  
IF OBJECT_ID('dbo.Person','U') IS NOT NULL DROP TABLE dbo.Person;  
GO  
CREATE TABLE dbo.Person(ID int, Name VARCHAR(30), Mother INT, Father INT);  
GO  
INSERT dbo.Person   
VALUES(1, 'Sue', NULL, NULL)  
      ,(2, 'Ed', NULL, NULL)  
      ,(3, 'Emma', 1, 2)  
      ,(4, 'Jack', 1, 2)  
      ,(5, 'Jane', NULL, NULL)  
      ,(6, 'Bonnie', 5, 4)  
      ,(7, 'Bill', 5, 4);  
GO  
-- Create the recursive CTE to find all of Bonnie's ancestors.  
WITH Generation (ID) AS  
(  
-- First anchor member returns Bonnie's mother.  
    SELECT Mother   
    FROM dbo.Person  
    WHERE Name = 'Bonnie'  
UNION  
-- Second anchor member returns Bonnie's father.  
    SELECT Father   
    FROM dbo.Person  
    WHERE Name = 'Bonnie'  
UNION ALL  
-- First recursive member returns male ancestors of the previous generation.  
    SELECT Person.Father  
    FROM Generation, Person  
    WHERE Generation.ID=Person.ID  
UNION ALL  
-- Second recursive member returns female ancestors of the previous generation.  
    SELECT Person.Mother  
    FROM Generation, dbo.Person  
    WHERE Generation.ID=Person.ID  
)  
SELECT Person.ID, Person.Name, Person.Mother, Person.Father  
FROM Generation, dbo.Person  
WHERE Generation.ID = Person.ID;  
GO  
```  
  
###  <a name="i-using-analytical-functions-in-a-recursive-cte"></a><a name="bkmkUsingAnalyticalFunctionsInARecursiveCTE"></a> I. Utilizzo di funzioni analitiche in un'espressione CTE ricorsiva  
 Nell'esempio seguente viene illustrata una trappola in cui si può cadere quando si utilizza una funzione analitica o di aggregazione nella parte ricorsiva di un'espressione CTE.  
  
```sql  
DECLARE @t1 TABLE (itmID INT, itmIDComp INT);  
INSERT @t1 VALUES (1,10), (2,10);   
  
DECLARE @t2 TABLE (itmID INT, itmIDComp INT);   
INSERT @t2 VALUES (3,10), (4,10);   
  
WITH vw AS  
 (  
    SELECT itmIDComp, itmID  
    FROM @t1  
  
    UNION ALL  
  
    SELECT itmIDComp, itmID  
    FROM @t2  
)   
,r AS  
 (  
    SELECT t.itmID AS itmIDComp  
           , NULL AS itmID  
           ,CAST(0 AS BITING) AS N  
           ,1 AS Lvl  
    FROM (SELECT 1 UNION ALL SELECT 2 UNION ALL SELECT 3 UNION ALL SELECT 4) AS t (itmID)   
  
UNION ALL  
  
SELECT t.itmIDComp  
    , t.itmID  
    , ROW_NUMBER() OVER(PARTITION BY t.itmIDComp ORDER BY t.itmIDComp, t.itmID) AS N  
    , Lvl + 1  
FROM r   
    JOIN vw AS t ON t.itmID = r.itmIDComp  
)   
  
SELECT Lvl, N FROM r;  
```  
  
Di seguito vengono riportati i risultati previsti per la query.  
  
```  
Lvl  N  
1    0  
1    0  
1    0  
1    0  
2    4  
2    3  
2    2  
2    1  
```  
  
Di seguito vengono riportati i risultati effettivi per la query.  
  
```  
Lvl  N  
1    0  
1    0  
1    0  
1    0  
2    1  
2    1  
2    1  
2    1  
```  
  
 `N` restituisce 1 per ogni sessione della parte ricorsiva dell'espressione CTE perché solo il subset di dati per tale livello di ricorsione viene passato a `ROWNUMBER`. Per ognuna delle iterazioni della parte ricorsiva della query, viene passata solo una riga a `ROWNUMBER`.  
  
## <a name="examples-sssdw-and-sspdw"></a>Esempi: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="j-using-a-common-table-expression-within-a-ctas-statement"></a>J. Uso di un'espressione di tabella comune all'interno di un'istruzione CTAS  
 L'esempio seguente crea una nuova tabella contenente il numero totale di ordini di vendita all'anno per tutti i venditori di [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)].  
  
```sql  
USE AdventureWorks2012;  
GO   
CREATE TABLE SalesOrdersPerYear  
WITH  
(  
    DISTRIBUTION = HASH(SalesPersonID)  
)  
AS  
    -- Define the CTE expression name and column list.  
    WITH Sales_CTE (SalesPersonID, SalesOrderID, SalesYear)  
    AS  
    -- Define the CTE query.  
    (  
        SELECT SalesPersonID, SalesOrderID, YEAR(OrderDate) AS SalesYear  
        FROM Sales.SalesOrderHeader  
        WHERE SalesPersonID IS NOT NULL  
    )  
    -- Define the outer query referencing the CTE name.  
    SELECT SalesPersonID, COUNT(SalesOrderID) AS TotalSales, SalesYear  
    FROM Sales_CTE  
    GROUP BY SalesYear, SalesPersonID  
    ORDER BY SalesPersonID, SalesYear;  
GO  
```  
  
### <a name="k-using-a-common-table-expression-within-a-cetas-statement"></a>K. Uso di un'espressione di tabella comune all'interno di un'istruzione CETAS  
 L'esempio seguente crea una nuova tabella esterna contenente il numero totale di ordini di vendita all'anno per tutti i venditori di [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)].  
  
```sql  
USE AdventureWorks2012;  
GO    
CREATE EXTERNAL TABLE SalesOrdersPerYear  
WITH  
(  
    LOCATION = 'hdfs://xxx.xxx.xxx.xxx:5000/files/Customer',  
    FORMAT_OPTIONS ( FIELD_TERMINATOR = '|' )   
)  
AS  
    -- Define the CTE expression name and column list.  
    WITH Sales_CTE (SalesPersonID, SalesOrderID, SalesYear)  
    AS  
    -- Define the CTE query.  
    (  
        SELECT SalesPersonID, SalesOrderID, YEAR(OrderDate) AS SalesYear  
        FROM Sales.SalesOrderHeader  
        WHERE SalesPersonID IS NOT NULL  
    )  
    -- Define the outer query referencing the CTE name.  
    SELECT SalesPersonID, COUNT(SalesOrderID) AS TotalSales, SalesYear  
    FROM Sales_CTE  
    GROUP BY SalesYear, SalesPersonID  
    ORDER BY SalesPersonID, SalesYear;  
GO  
```  
  
### <a name="l-using-multiple-comma-separated-ctes-in-a-statement"></a>L. Uso di più CTE delimitate da virgole in un'istruzione  
 L'esempio seguente illustra come includere due CTE all'interno di un'unica istruzione. Le CTE non possono essere annidate (la ricorsione non è consentita).  
  
```sql  
WITH   
 CountDate (TotalCount, TableName) AS  
    (  
     SELECT COUNT(datekey), 'DimDate' FROM DimDate  
    ) ,  
 CountCustomer (TotalAvg, TableName) AS  
    (  
     SELECT COUNT(CustomerKey), 'DimCustomer' FROM DimCustomer  
    )  
SELECT TableName, TotalCount FROM CountDate  
UNION ALL  
SELECT TableName, TotalAvg FROM CountCustomer;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [EXCEPT e INTERSECT &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-operators-except-and-intersect-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)  
  
 
