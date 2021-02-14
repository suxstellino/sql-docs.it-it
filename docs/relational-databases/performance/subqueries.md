---
title: Sottoquery (SQL Server) | Microsoft Docs
description: Esaminare un esempio di sottoquery, ovvero una query annidata in un'istruzione SELECT, INSERT, UPDATE o DELETE oppure in un'altra sottoquery in SQL Server.
ms.custom: ''
ms.date: 02/18/2018
ms.prod: sql
ms.technology: performance
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Subquery
- Subqueries
- subqueries [SQL Server], fundamentals
- subqueries [SQL Server], correlated
- subqueries [SQL Server], types
ms.assetid: bfc97432-c14c-4768-9dc5-a9c512f6b2bd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6ce8c74858311d125d8053272f0cff3e14917cf6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345529"
---
# <a name="subqueries-sql-server"></a>Sottoquery (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
 
Una sottoquery è una query nidificata all'interno di un'istruzione `SELECT`, `INSERT`, `UPDATE` o `DELETE` o all'interno di un'altra sottoquery. È possibile utilizzare una sottoquery in qualsiasi posizione in cui è consentito inserire un'espressione. In questo esempio viene usata una sottoquery come espressione di colonna denominata MaxUnitPrice in un'istruzione SELECT.

```sql
USE AdventureWorks2016;
GO
SELECT Ord.SalesOrderID, Ord.OrderDate,
    (SELECT MAX(OrdDet.UnitPrice)
     FROM Sales.SalesOrderDetail AS OrdDet
     WHERE Ord.SalesOrderID = OrdDet.SalesOrderID) AS MaxUnitPrice
FROM Sales.SalesOrderHeader AS Ord;
GO
```

## <a name="subquery-fundamentals"></a><a name="fundamentals"></a> Nozioni fondamentali sulle sottoquery
Le sottoquery sono dette anche query interne o istruzioni SELECT interne. L'istruzione che include una sottoquery è detta anche query esterna o istruzione SELECT esterna.   

Molte istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] che includono sottoquery possono essere formulate anche come join. Altre domande possono essere poste solo in forma di sottoquery. In [!INCLUDE[tsql](../../includes/tsql-md.md)] non si rileva in genere alcuna differenza nelle prestazioni tra un'istruzione che include una sottoquery e una versione equivalente dal punto di vista semantico ma priva di sottoquery. In alcuni casi in cui è necessario verificare l'esistenza di dati specifici, tuttavia, l'utilizzo di un join consente di ottenere prestazioni migliori. Se non si utilizza un join, è necessario assicurarsi che i duplicati vengano eliminati elaborando la query nidificata per ogni risultato della query esterna. In questi casi, l'utilizzo del join consente di ottenere risultati migliori. L'esempio seguente illustra un'istruzione `SELECT` con una sottoquery e un'istruzione `SELECT` con un join che restituiscono lo stesso set di risultati:

```sql
USE AdventureWorks2016;
GO

/* SELECT statement built using a subquery. */
SELECT Name
FROM Production.Product
WHERE ListPrice =
    (SELECT ListPrice
     FROM Production.Product
     WHERE Name = 'Chainring Bolts' );
GO

/* SELECT statement built using a join that returns
   the same result set. */
SELECT Prd1. Name
FROM Production.Product AS Prd1
     JOIN Production.Product AS Prd2
       ON (Prd1.ListPrice = Prd2.ListPrice)
WHERE Prd2. Name = 'Chainring Bolts';
GO
```

Una sottoquery nidificata nell'istruzione SELECT esterna include i componenti seguenti:    
-   Una normale query `SELECT` che include i componenti normalmente specificati in un elenco di selezione.   
-   Una normale clausola `FROM` che include uno o più nomi di tabelle o visualizzazioni.   
-   Una clausola `WHERE` facoltativa.   
-   Una clausola `GROUP BY` facoltativa.   
-   Una clausola `HAVING` facoltativa.   

La query SELECT di una sottoquery è sempre racchiusa tra parentesi. Non può includere una clausola `COMPUTE` o `FOR BROWSE` e può includere una clausola `ORDER BY` solo quando è specificata anche una clausola TOP.   

Una sottoquery può essere nidificata nella clausola `WHERE` o `HAVING` di un'istruzione `SELECT`, `INSERT`, `UPDATE` o `DELETE` esterna oppure in un'altra sottoquery. Sono consentiti fino a 32 livelli di nidificazione. Il limite massimo tuttavia varia in base alla memoria disponibile e alla complessità delle altre espressioni della query, ovvero alcune query specifiche potrebbero non supportare 32 livelli di nidificazione. Una sottoquery può essere specificata in qualsiasi posizione in cui è consentito inserire un'espressione, a condizione che venga restituito un solo valore.   

Se una tabella viene specificata in una sottoquery e non nella query esterna, le colonne di tale tabella non vengono inserite nell'output, ovvero nell'elenco di selezione della query esterna.   

Le istruzioni che includono una sottoquery vengono in genere formulate in uno dei formati seguenti:   
-   WHERE espressione \[NOT] IN (sottoquery)
-   WHERE espressione operatore_confronto \[ANY | ALL] (sottoquery)
-   WHERE \[NOT] EXISTS (sottoquery)   

In alcune istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] la sottoquery può essere valutata come se si trattasse di una query indipendente. Concettualmente i risultati della sottoquery vengono sostituiti nella query esterna, anche se ciò non corrisponde al modo in cui [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] elabora le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] con sottoquery.    

Sono disponibili tre tipi di sottoquery di base, ovvero: 
-   Sottoquery applicate agli elenchi introdotte da `IN` o da un operatore di confronto modificato da `ANY` o `ALL`.
-   Sottoquery introdotte da un operatore di confronto non modificato le quali devono restituire un solo valore.
-   Sottoquery che corrispondono a test di esistenza introdotte da `EXISTS`.

## <a name="subquery-rules"></a><a name="rules"></a> Regole delle sottoquery
Le sottoquery sono soggette alle seguenti restrizioni: 
-   L'elenco di selezione di una sottoquery introdotta da un operatore di confronto può includere una sola espressione o un solo nome di colonna, ad eccezione dei casi in cui `EXISTS` e `IN` vengono usati rispettivamente in `SELECT *` o in un elenco.   
-   Se la clausola `WHERE` di una query esterna include un nome di colonna, questo deve essere compatibile a livello di join con la colonna dell'elenco di selezione della sottoquery.   
-   I tipi di dati **ntext**, **text** e **image** non possono essere usati nell'elenco di selezione delle sottoquery.   
-   Poiché le sottoquery introdotte da un operatore di confronto non modificato (ovvero non seguito dalla parola chiave ANY o ALL) devono restituire un solo valore, non possono includere le clausole `GROUP BY` e `HAVING`.   
-   La parola chiave `DISTINCT` non può essere usata nelle sottoquery che includono la clausola GROUP BY.
-   Le clausole `COMPUTE` e `INTO` non possono essere specificate.   
-   È possibile specificare `ORDER BY` solo quando viene specificata anche la clausola `TOP`.   
-   Le viste create utilizzando una sottoquery non possono essere aggiornate.   
-   Per convenzione, l'elenco di selezione di una sottoquery introdotta da `EXISTS` include un asterisco (\*) anziché un singolo nome di colonna. Le regole valide per una sottoquery introdotta da `EXISTS` corrispondono a quelle seguite per un elenco di selezione standard poiché una sottoquery introdotta da `EXISTS` crea un test di esistenza e non restituisce dati, ma TRUE o FALSE.   

## <a name="qualifying-column-names-in-subqueries"></a><a name="qualifying"></a> Qualificazione dei nomi delle colonne nelle sottoquery
Nell'esempio seguente la colonna *BusinessEntityID* specificata nella clausola `WHERE` della query esterna viene qualificata in modo implicito con il nome della tabella indicato nella clausola `FROM` della query esterna, ovvero *Sales.Store*. Il riferimento a *CustomerID* nell'elenco di selezione della sottoquery viene qualificato dalla clausola `FROM` della sottoquery, ovvero dalla tabella *Sales.Customer*.

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Sales.Store
WHERE BusinessEntityID NOT IN
    (SELECT CustomerID
     FROM Sales.Customer
     WHERE TerritoryID = 5);
GO
```

Come regola generale, i nomi delle colonne inclusi in un'istruzione vengono qualificati in modo implicito dalla tabella cui viene fatto riferimento nella clausola `FROM` allo stesso livello. Se non è disponibile una colonna nella tabella cui viene fatto riferimento nella clausola `FROM` di una sottoquery, la colonna viene qualificata in modo implicito dalla tabella cui viene fatto riferimento nella clausola `FROM` della query esterna.   

Di seguito viene riportata una query che include le qualificazioni implicite specificate:

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Sales.Store
WHERE Sales.Store.BusinessEntityID NOT IN
    (SELECT Sales.Customer.CustomerID
     FROM Sales.Customer
     WHERE TerritoryID = 5);
GO
```

È comunque consigliabile specificare sempre il nome della tabella in modo esplicito. È inoltre sempre possibile sostituire i presupposti impliciti relativi ai nomi delle tabelle con qualificazioni esplicite.   

> [!IMPORTANT]
> Se viene fatto riferimento a una colonna in una sottoquery che non è inclusa nella tabella cui viene fatto riferimento nella clausola `FROM` della sottoquery, ma che è invece inclusa in una tabella cui viene fatto riferimento nella clausola `FROM` della query esterna, la query viene eseguita senza errori. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qualifica in modo implicito la colonna nella sottoquery con il nome della tabella indicato nella query esterna.   

## <a name="multiple-levels-of-nesting"></a><a name="nesting"></a> Più livelli di nidificazione
Una sottoquery può includere una o più sottoquery. Un'istruzione supporta qualsiasi numero di livelli di nidificazione di sottoquery.   

La query seguente consente di trovare i nomi dei dipendenti che operano anche come venditori.   

```sql
USE AdventureWorks2016;
GO
SELECT LastName, FirstName
FROM Person.Person
WHERE BusinessEntityID IN
    (SELECT BusinessEntityID
     FROM HumanResources.Employee
     WHERE BusinessEntityID IN
        (SELECT BusinessEntityID
         FROM Sales.SalesPerson)
    );
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
LastName                                           FirstName
-------------------------------------------------- -----------------------
Jiang                                              Stephen
Abbas                                              Syed
Alberts                                            Amy
Ansman-Wolfe                                       Pamela
Campbell                                           David
Carson                                             Jillian
Ito                                                Shu
Mitchell                                           Linda
Reiter                                             Tsvi
Saraiva                                            Jos
Vargas                                             Garrett
Varkey Chudukatil                                  Ranjit
Valdez                                             Rachel
Tsoflias                                           Lynn
Pak                                                Jae
Blythe                                             Michael
Mensa-Annan                                        Tete

(17 row(s) affected)
```

La query più interna restituisce gli ID dei venditori. La query di livello immediatamente superiore viene valutata in base a tali ID e restituisce gli ID contatti dei dipendenti. La query più esterna, infine, utilizza gli ID contatti per trovare i nomi dei dipendenti.   

Questa query può inoltre essere formulata come join:

```sql
USE AdventureWorks2016;
GO
SELECT LastName, FirstName
FROM Person.Person c
INNER JOIN HumanResources.Employee e
ON c.BusinessEntityID = e.BusinessEntityID
JOIN Sales.SalesPerson s 
ON e.BusinessEntityID = s.BusinessEntityID;
GO
```

## <a name="correlated-subqueries"></a><a name="correlated"></a> Sottoquery correlate
In molti casi per valutare le query è possibile eseguire una volta la sottoquery e sostituire quindi i valori ottenuti a quelli specificati nella clausola `WHERE` della query esterna. Nelle query che includono una sottoquery correlata (nota anche come sottoquery ripetuta), i valori della sottoquery variano in base ai valori della query esterna. Ciò significa che la sottoquery viene eseguita ripetutamente, una volta per ogni riga selezionata dalla query esterna.
La query seguente recupera un'istanza di ogni nome e cognome di dipendente per il quale il premio di produzione nella tabella *SalesPerson* è pari a 5000 e per il quale sono presenti numeri di matricola corrispondenti nelle tabelle *Employee* e *SalesPerson*.

```sql
USE AdventureWorks2016;
GO
SELECT DISTINCT c.LastName, c.FirstName, e.BusinessEntityID 
FROM Person.Person AS c JOIN HumanResources.Employee AS e
ON e.BusinessEntityID = c.BusinessEntityID 
WHERE 5000.00 IN
    (SELECT Bonus
    FROM Sales.SalesPerson sp
    WHERE e.BusinessEntityID = sp.BusinessEntityID) ;
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
LastName FirstName BusinessEntityID
-------------------------- ---------- ------------
Ansman-Wolfe Pamela 280
Saraiva José 282

(2 row(s) affected)
```

La sottoquery precedente non può essere valutata indipendentemente dalla query esterna. Richiede infatti un valore per *Employee.BusinessEntityID*, ma questo valore cambia quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esamina righe diverse della tabella *Employee*.   
La query viene valutata nel modo seguente: in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene esaminata ogni riga della tabella Employee per determinare se verrà inclusa nei risultati tramite la sostituzione del valore di ogni riga nella query interna.
Ad esempio, se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esamina per prima la riga relativa a `Syed Abbas`, la variabile *Employee.BusinessEntityID* assumerà il valore 285, che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sostituisce al valore nella query interna.

```sql
USE AdventureWorks2016;
GO
SELECT Bonus
FROM Sales.SalesPerson
WHERE BusinessEntityID = 285;
GO
```

Poiché il risultato è 0 (`Syed Abbas` non ha ricevuto un premio di produzione perché non è un venditore), la query esterna viene valutata nel modo seguente:

```sql
USE AdventureWorks2016;
GO
SELECT LastName, FirstName
FROM Person.Person AS c JOIN HumanResources.Employee AS e
ON e.BusinessEntityID = c.BusinessEntityID 
WHERE 5000 IN (0.00);
GO
```

La riga relativa a `Syed Abbas` non rispetta la condizione specificata e non viene pertanto inclusa nel set di risultati. Eseguire la stessa procedura per la riga relativa a `Pamela Ansman-Wolfe`. La riga verrà inclusa nei risultati.     

Le sottoquery correlate possono anche includere funzioni con valori di tabella nella clausola `FROM` se un argomento di tali funzioni fa riferimento a colonne di una tabella della query esterna. In questo caso, per ogni riga della query esterna, la funzione viene valutata in base alla sottoquery.    
  
## <a name="subquery-types"></a><a name="types"></a> Tipi di sottoquery
È possibile specificare sottoquery in numerose posizioni: 
-   Con alias. Per altre informazioni, vedere [Sottoquery con alias](#aliases).
-   Con `IN` o `NOT IN`. Per altre informazioni, vedere [Sottoquery con IN](#in) e [Sottoquery con NOT IN](#notin).
-   Nelle istruzioni `UPDATE`, `DELETE` e `INSERT`. Per altre informazioni, vedere [Sottoquery in istruzioni UPDATE, DELETE e INSERT](#upsert).
-   Con operatori di confronto. Per altre informazioni, vedere [Sottoquery con operatori di confronto](#comparison).
-   Con `ANY`, `SOME` o `ALL`. Per altre informazioni, vedere [Operatori di confronto modificati da ANY, SOME o ALL](#comparison_modified).
-   Con `EXISTS` o `NOT EXISTS`. Per altre informazioni, vedere [Sottoquery con EXISTS](#exists) e [Sottoquery con NOT EXISTS](#notexists).
-   In sostituzione di un'espressione. Per altre informazioni, vedere [Sottoquery usate in sostituzione di un'espressione](#expression).

### <a name="subqueries-with-aliases"></a><a name="aliases"></a> Sottoquery con alias
Molte istruzioni in cui la sottoquery e la query esterna fanno riferimento alla stessa tabella possono essere formulate come self-join, ovvero join che uniscono una tabella a se stessa. Ad esempio, è possibile trovare indirizzi di dipendenti di un particolare stato utilizzando una sottoquery:   

```sql
USE AdventureWorks2016;
GO
SELECT StateProvinceID, AddressID
FROM Person.Address
WHERE AddressID IN
    (SELECT AddressID
     FROM Person.Address
     WHERE StateProvinceID = 39);
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
StateProvinceID AddressID
----------- -----------
39 942
39 955
39 972
39 22660

(4 row(s) affected)
```   

In alternativa, è possibile utilizzare un self-join:   

```sql
USE AdventureWorks2016;
GO
SELECT e1.StateProvinceID, e1.AddressID
FROM Person.Address AS e1
INNER JOIN Person.Address AS e2
ON e1.AddressID = e2.AddressID
AND e2.StateProvinceID = 39;
GO
```

È necessario specificare gli alias delle tabelle in quanto la tabella unita in join assume due ruoli diversi. È inoltre possibile utilizzare gli alias nelle query nidificate in cui in una query interna e in una esterna viene fatto riferimento alla stessa tabella.    

```sql
USE AdventureWorks2016;
GO
SELECT e1.StateProvinceID, e1.AddressID
FROM Person.Address AS e1
WHERE e1.AddressID IN
    (SELECT e2.AddressID
     FROM Person.Address AS e2
     WHERE e2.StateProvinceID = 39);
GO
```

Gli alias espliciti evidenziano in modo chiaro che un riferimento alla tabella *Person.Address* nella sottoquery non corrisponde al riferimento incluso nella query esterna.   

### <a name="subqueries-with-in"></a><a name="in"></a> Sottoquery con IN
Il risultato di una sottoquery introdotta da `IN` (o da `NOT IN`) è un elenco di zero o più valori. I risultati restituiti dalla sottoquery vengono utilizzati dalla query esterna.    
La query seguente trova i nomi di tutti i prodotti con nome "Wheels" creati da Adventure Works Cycles.     

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ProductSubcategoryID IN
    (SELECT ProductSubcategoryID
     FROM Production.ProductSubcategory
     WHERE Name = 'Wheels');
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]     

```
Name
----------------------------
LL Mountain Front Wheel
ML Mountain Front Wheel
HL Mountain Front Wheel
LL Road Front Wheel
ML Road Front Wheel
HL Road Front Wheel
Touring Front Wheel
LL Mountain Rear Wheel
ML Mountain Rear Wheel
HL Mountain Rear Wheel
LL Road Rear Wheel
ML Road Rear Wheel
HL Road Rear Wheel
Touring Rear Wheel

(14 row(s) affected)
```

Questa istruzione viene valutata in due fasi: la query interna restituisce il numero di identificazione della sottocategoria corrispondente al nome Wheel (17), dopodiché tale valore viene sostituito nella query esterna, che trova i nomi dei prodotti corrispondenti ai numeri di identificazione di sottocategoria in Product.     

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ProductSubcategoryID IN ('17');
GO
```

L'utilizzo di un join anziché di una sottoquery per la risoluzione di questo e altri problemi analoghi consente di visualizzare nel risultato colonne che derivano da più tabelle. Se, ad esempio, si desidera includere nel risultato il nome della sottocategoria del prodotto, è necessario utilizzare un join.    

```sql
USE AdventureWorks2016;
GO
SELECT p.Name, s.Name
FROM Production.Product p
INNER JOIN Production.ProductSubcategory s
ON p.ProductSubcategoryID = s.ProductSubcategoryID
AND s.Name = 'Wheels';
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]    

```
Name
LL Mountain Front Wheel Wheels
ML Mountain Front Wheel Wheels
HL Mountain Front Wheel Wheels
LL Road Front Wheel Wheels
ML Road Front Wheel Wheels
HL Road Front Wheel Wheels
Touring Front Wheel Wheels
LL Mountain Rear Wheel Wheels
ML Mountain Rear Wheel Wheels
HL Mountain Rear Wheel Wheels
LL Road Rear Wheel Wheels
ML Road Rear Wheel Wheels
HL Road Rear Wheel Wheels
Touring Rear Wheel Wheels

(14 row(s) affected)
```    

La query seguente trova i nomi di tutti i fornitori con un buon rating creditizio da cui Adventure Works Cycles ordina almeno 20 prodotti e il cui tempo medio di consegna è inferiore a 16 giorni.    

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Purchasing.Vendor
WHERE CreditRating = 1
AND BusinessEntityID IN
    (SELECT BusinessEntityID
     FROM Purchasing.ProductVendor
     WHERE MinOrderQty >= 20
     AND AverageLeadTime < 16);
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
Name
--------------------------------------------------
Compete Enterprises, Inc
International Trek Center
First National Sport Co.
Comfort Road Bicycles
Circuit Cycles
First Rate Bicycles
Jeff's Sporting Goods
Competition Bike Training Systems
Electronic Bike Repair & Supplies
Crowley Sport
Expert Bike Co
Team Athletic Co.
Compete, Inc.   

(13 row(s) affected)
```   

Viene innanzitutto valutata la query interna, la quale restituisce i numeri di identificazione dei fornitori che soddisfano le qualificazioni della sottoquery. Dopodiché viene valutata la query esterna. Si noti che nella clausola WHERE sia della query interna che di quella esterna è possibile includere più condizioni.   

Se si utilizza un join, la stessa query viene formulata nel modo seguente:

```sql
USE AdventureWorks2016;
GO
SELECT DISTINCT Name
FROM Purchasing.Vendor v
INNER JOIN Purchasing.ProductVendor p
ON v.BusinessEntityID = p.BusinessEntityID
WHERE CreditRating = 1
  AND MinOrderQty >= 20
  AND AverageLeadTime < 16;
GO
```

Un join può sempre essere espresso come sottoquery. Spesso, ma non sempre, una sottoquery può essere espressa come join. Ciò è possibile perché i join sono simmetrici, ovvero l'unione in join delle tabelle A e B restituisce sempre gli stessi risultati indipendentemente dall'ordine delle tabelle nel join. Questo non avviene invece quando si utilizza una sottoquery.    

### <a name="subqueries-with-not-in"></a><a name="notin"></a> Sottoquery con NOT IN
Le sottoquery introdotte dalla parola chiave NOT IN restituiscono un elenco di zero o più valori.   
La query seguente trova i nomi dei prodotti che non sono biciclette finite.   

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ProductSubcategoryID NOT IN
    (SELECT ProductSubcategoryID
     FROM Production.ProductSubcategory
     WHERE Name = 'Mountain Bikes' 
        OR Name = 'Road Bikes'
        OR Name = 'Touring Bikes');
GO
```

Non è possibile convertire questa istruzione in un join. Il join di disuguaglianza corrispondente ha infatti un significato diverso perché trova i nomi dei prodotti inclusi in una sottocategoria che non corrisponde a una bicicletta finita.      

### <a name="subqueries-in-update-delete-and-insert-statements"></a><a name="upsert"></a> Sottoquery in istruzioni UPDATE, DELETE e INSERT
Le sottoquery possono essere nidificate nelle istruzioni DML `UPDATE`, `DELETE`, `INSERT` e `SELECT`.    

L'esempio seguente raddoppia il valore della colonna *ListPrice* della tabella *Production.Product*. La sottoquery nella clausola `WHERE` fa riferimento alla tabella *Purchasing.ProductVendor* per limitare le righe aggiornate nella tabella *Product* a quelle fornite da *BusinessEntity* 1540.

```sql
USE AdventureWorks2016;
GO 
UPDATE Production.Product
SET ListPrice = ListPrice * 2
WHERE ProductID IN
    (SELECT ProductID 
     FROM Purchasing.ProductVendor
     WHERE BusinessEntityID = 1540);
GO
```

Di seguito è riportata un'istruzione `UPDATE` equivalente che usa un join:

```sql
USE AdventureWorks2016;
GO 
UPDATE Production.Product
SET ListPrice = ListPrice * 2
FROM Production.Product AS p
INNER JOIN Purchasing.ProductVendor AS pv
    ON p.ProductID = pv.ProductID AND BusinessEntityID = 1540;
GO   
```

### <a name="subqueries-with-comparison-operators"></a><a name="comparison"></a> Sottoquery con operatori di confronto
Le sottoquery possono essere introdotte da un operatore di confronto, ovvero =, < >, >, > =, <, ! >, ! < o < =).   

Analogamente alle sottoquery introdotte da `IN`, le sottoquery introdotte da un operatore di confronto non modificato, ovvero non seguito dalla parola chiave `ANY` o `ALL`, devono restituire un solo valore anziché un elenco di valori. Se una sottoquery di questo tipo restituisce più valori, SQL Server visualizza un messaggio di errore.    

Per utilizzare una sottoquery introdotta da un operatore di confronto non modificato, è necessario aver acquisito una certa familiarità con i dati e la natura del problema in modo da sapere che la sottoquery restituirà un solo valore.     

Si supponga, ad esempio, che ogni venditore copra una sola zona di vendita e che si desideri trovare i clienti che risiedono nella zona coperta da `Linda Mitchell`. In questo caso è possibile scrivere un'istruzione con una sottoquery introdotta dall'operatore di confronto =.    

```sql
USE AdventureWorks2016;
GO
SELECT CustomerID
FROM Sales.Customer
WHERE TerritoryID =
    (SELECT TerritoryID
     FROM Sales.SalesPerson
     WHERE BusinessEntityID = 276);
GO
```

Se, tuttavia, `Linda Mitchell` copre più zone di vendita, viene restituito un messaggio di errore. In questo caso, è possibile usare `IN` (oppure = ANY) anziché l'operatore di confronto =.    

Le sottoquery introdotte da un operatore di confronto non modificato spesso includono funzioni di aggregazione, in quanto tali funzioni restituiscono un solo valore. L'istruzione seguente consente ad esempio di individuare i nomi di tutti i prodotti il cui prezzo di listino è superiore a quello medio.     

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ListPrice >
    (SELECT AVG (ListPrice)
     FROM Production.Product);
GO
```     

Poiché le sottoquery introdotte da un operatore di confronto non modificato devono restituire un solo valore, non è possibile includervi clausole `GROUP BY` o `HAVING`, a meno che tali clausole non restituiscano un solo valore. La query seguente consente, ad esempio, di individuare i prodotti il cui prezzo è maggiore rispetto al prodotto di prezzo più basso nella sottocategoria 14.     

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ListPrice >
    (SELECT MIN (ListPrice)
     FROM Production.Product
     GROUP BY ProductSubcategoryID
     HAVING ProductSubcategoryID = 14);
GO
```

### <a name="comparison-operators-modified-by-any-some-or-all"></a><a name="comparison_modified"></a> Operatori di confronto modificati da ANY, SOME o ALL
Gli operatori di confronto che introducono una sottoquery possono essere modificati tramite le parole chiave ALL o ANY. SOME è un equivalente dello standard ISO per `ANY`.     

Le sottoquery introdotte da un operatore di confronto modificato restituiscono un elenco di zero o più valori e possono includere una clausola `GROUP BY` o `HAVING`. Queste sottoquery possono essere riformulate con `EXISTS`.     

Usando l'operatore di confronto > come esempio, `>ALL` indica maggiore di qualsiasi valore, ovvero maggiore del valore massimo. Ad esempio, `>ALL (1, 2, 3)` significa maggiore di 3. `>ANY` significa maggiore di almeno un valore, ovvero maggiore del valore minimo. Di conseguenza, `>ANY (1, 2, 3)` significa maggiore di 1.
Una riga di una sottoquery che include `>ALL` rispetta la condizione specificata nella query esterna se il valore della colonna che introduce la sottoquery è maggiore di tutti i valori dell'elenco restituito dalla sottoquery.    

In modo analogo, quando si usa `>ANY`, una riga rispetta la condizione specificata nella query esterna se il valore della colonna che introduce la sottoquery è maggiore di almeno uno dei valori dell'elenco restituito dalla sottoquery.    

La query seguente, in cui viene illustrata una sottoquery introdotta da un operatore di confronto modificato da ANY, trova i prodotti con un prezzo di listino maggiore o uguale al prezzo di listino massimo di tutte le sottocategorie di prodotto.    

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ListPrice >= ANY
    (SELECT MAX (ListPrice)
     FROM Production.Product
     GROUP BY ProductSubcategoryID);
GO
```    

Per  ogni sottocategoria di prodotto, la query interna trova il prezzo di listino massimo. La query esterna esamina tutti i valori e determina quali prezzi di listino di singoli prodotti sono maggiori o uguali al prezzo di listino massimo delle sottocategorie di prodotto. Se `ANY` viene sostituito da `ALL`, la query restituirà unicamente i prodotti con un prezzo di listino maggiore o uguale a tutti i prezzi di listino restituiti dalla query interna.    

Se la sottoquery non restituisce valori, anche la query non restituisce alcun valore.    

L'operatore `=ANY` equivale a `IN`. Ad esempio, per trovare i nomi di tutti i prodotti di tipo wheel creati da Adventure Works Cycles, è possibile usare `IN` o `=ANY`.

```sql
--Using =ANY
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ProductSubcategoryID =ANY
    (SELECT ProductSubcategoryID
     FROM Production.ProductSubcategory
     WHERE Name = 'Wheels');
GO

--Using IN
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ProductSubcategoryID IN
    (SELECT ProductSubcategoryID
     FROM Production.ProductSubcategory
     WHERE Name = 'Wheels');
GO
```

Di seguito è riportato il set di risultati per entrambe le query:

```
Name
--------------------------------------------------
LL Mountain Front Wheel
ML Mountain Front Wheel
HL Mountain Front Wheel
LL Road Front Wheel
ML Road Front Wheel
HL Road Front Wheel
Touring Front Wheel
LL Mountain Rear Wheel
ML Mountain Rear Wheel
HL Mountain Rear Wheel
LL Road Rear Wheel
ML Road Rear Wheel
HL Road Rear Wheel
Touring Rear Wheel

(14 row(s) affected)
```    

L'operatore `<>ANY`, tuttavia, è diverso da `NOT IN`: `<>ANY` indica non = a o non = b oppure non = c. `NOT IN` indica non = a e non = b e non = c. `<>ALL` indica uguale a `NOT IN`.     

Ad esempio, la query seguente trova i clienti di un'area in cui non sono disponibili venditori.     

```sql
USE AdventureWorks2016;
GO
SELECT CustomerID
FROM Sales.Customer
WHERE TerritoryID <> ANY
    (SELECT TerritoryID
     FROM Sales.SalesPerson);
GO
```    

I risultati includono tutti i clienti, a eccezione di quelli per i quali l'area di vendita corrisponde a NULL, perché per ogni area assegnata a un cliente è disponibile un venditore. La query interna trova tutte le aree di vendita per le quali sono disponibili venditori e quindi, per ogni area, la query esterna trova i clienti che non sono associati all'area.    

Per lo stesso motivo, se si usa `NOT IN` nella query, nei risultati non sarà incluso alcun cliente.      

È possibile ottenere gli stessi risultati usando l'operatore `<>ALL`, che equivale a `NOT IN`.   

### <a name="subqueries-with-exists"></a><a name="exists"></a> Sottoquery con EXISTS
Le sottoquery introdotte dalla parola chiave `EXISTS` fungono da test di esistenza dei dati. La clausola `WHERE` della query esterna verifica l'esistenza delle righe restituite dalla sottoquery. La sottoquery non restituisce dati, ma TRUE o FALSE.   

Per specificare una sottoquery introdotta da EXISTS, usare la sintassi seguente:   

`WHERE [NOT] EXISTS (subquery)`    

La query seguente individua i nomi di tutti i prodotti nella sottocategoria Wheels:    

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE EXISTS
    (SELECT * 
     FROM Production.ProductSubcategory
     WHERE ProductSubcategoryID = 
            Production.Product.ProductSubcategoryID
        AND Name = 'Wheels');
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]    

```
Name
--------------------------------------------------
LL Mountain Front Wheel
ML Mountain Front Wheel
HL Mountain Front Wheel
LL Road Front Wheel
ML Road Front Wheel
HL Road Front Wheel
Touring Front Wheel
LL Mountain Rear Wheel
ML Mountain Rear Wheel
HL Mountain Rear Wheel
LL Road Rear Wheel
ML Road Rear Wheel
HL Road Rear Wheel
Touring Rear Wheel

(14 row(s) affected)
```    

Per determinare i risultati di questa query, è necessario considerare di volta in volta se nel nome di ogni prodotto la sottoquery restituisce almeno una riga, in altri termini, se il test di esistenza restituisce TRUE.   

Si noti che le sottoquery introdotte da EXISTS sono leggermente diverse rispetto alle altre sottoquery, e precisamente: 
-   La parola chiave `EXISTS` non è preceduta dal nome di una colonna, da una costante o da altre espressioni.     
-   Nella maggior parte dei casi l'elenco di selezione di una sottoquery introdotta da `EXISTS` è costituito da un asterisco (*). Non è necessario infatti specificare i nomi di colonna, in quanto viene semplicemente verificata l'esistenza di righe che soddisfano le condizioni specificate nella sottoquery.    

La parola chiave `EXISTS` è importante in quanto spesso non esiste una formulazione alternativa senza sottoquery. Mentre per alcune query introdotte dalla parola chiave EXISTS non è disponibile alcuna formulazione alternativa, tutte le query che usano IN o un operatore di confronto modificato da `ANY` o `ALL` consentono di ottenere risultati simili.     

Ad esempio, la query precedente può essere formulata usando IN:   

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ProductSubcategoryID IN
    (SELECT ProductSubcategoryID
     FROM Production.ProductSubcategory
     WHERE Name = 'Wheels');
GO
```   

### <a name="subqueries-with-not-exists"></a><a name="notexists"></a> Sottoquery con NOT EXISTS
La parola chiave `NOT EXISTS` funziona in modo analogo a `EXISTS`, con la differenza che la clausola `WHERE` in cui è specificata viene soddisfatta se la sottoquery non restituisce alcuna riga.    

Per trovare, ad esempio, i nomi dei prodotti che non fanno parte della sottocategoria wheels:   

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE NOT EXISTS
    (SELECT * 
     FROM Production.ProductSubcategory
     WHERE ProductSubcategoryID = 
            Production.Product.ProductSubcategoryID
        AND Name = 'Wheels');
GO
```   

### <a name="subqueries-used-in-place-of-an-expression"></a><a name="expression"></a> Sottoquery usate in sostituzione di un'espressione
In [!INCLUDE[tsql](../../includes/tsql-md.md)] è possibile usare una sottoquery in qualsiasi punto in cui è possibile inserire un'espressione nelle istruzioni `SELECT`, `UPDATE`, `INSERT` e `DELETE`, ad eccezione di un elenco `ORDER BY`.    

Nell'esempio seguente viene illustrata la modalità di utilizzo di questa funzione. La query trova i prezzi di tutte le mountain bike, il prezzo medio e la differenza di prezzo delle singole biciclette rispetto al prezzo medio.    

```sql
USE AdventureWorks2016;
GO
SELECT Name, ListPrice, 
(SELECT AVG(ListPrice) FROM Production.Product) AS Average, 
    ListPrice - (SELECT AVG(ListPrice) FROM Production.Product)
    AS Difference
FROM Production.Product
WHERE ProductSubcategoryID = 1;
GO
```   

## <a name="see-also"></a>Vedere anche  
[IN &#40;Transact-SQL&#41;](../../t-sql/language-elements/in-transact-sql.md)       
[EXISTS &#40;Transact-SQL&#41;](../../t-sql/language-elements/exists-transact-sql.md)     
[ALL &#40;Transact-SQL&#41;](../../t-sql/language-elements/all-transact-sql.md)     
[SOME | ANY &#40;Transact-SQL&#41;](../../t-sql/language-elements/some-any-transact-sql.md)     
[Join](../../relational-databases/performance/joins.md)    
[Operatori di confronto &#40;Transact-SQL&#41;](../../t-sql/language-elements/comparison-operators-transact-sql.md)       
  
