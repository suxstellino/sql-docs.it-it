---
description: SELECT - GROUP BY- Transact-SQL
title: GROUP BY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GROUP
- CUBE
- ROLLUP
- GROUPING_SETS_TSQL
- GROUP BY
- GROUPING SETS
- GROUP_BY_TSQL
- GROUP_TSQL
- CUBE_TSQL
- ROLLUP_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- GROUP BY clause, about GROUP BY clause
- dividing tables into groups
- GROUPING SETS
- GROUP BY clause
- table groups [SQL Server]
- groups [SQL Server], tables divided into groups
- summary values [SQL Server]
ms.assetid: 40075914-6385-4692-b4a5-62fe44ae6cb6
author: shkale-msft
ms.author: shkale
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d1081121051d3e033bbeec724fa3f5bb6ab4ba58
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751551"
---
# <a name="select---group-by--transact-sql"></a>SELECT - GROUP BY- Transact-SQL
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Una clausola dell'istruzione SELECT che divide il risultato della query in gruppi di righe solitamente per eseguire una o più aggregazioni in ogni gruppo. L'istruzione SELECT restituisce una riga per ogni gruppo.  
  
## <a name="syntax"></a>Sintassi  

 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database   
-- ISO-Compliant Syntax  
  
GROUP BY {
      column-expression  
    | ROLLUP ( <group_by_expression> [ ,...n ] )  
    | CUBE ( <group_by_expression> [ ,...n ] )  
    | GROUPING SETS ( <grouping_set> [ ,...n ]  )  
    | () --calculates the grand total 
} [ ,...n ] 
 
<group_by_expression> ::=  
      column-expression  
    | ( column-expression [ ,...n ] )    
   
<grouping_set> ::=  
      () --calculates the grand total  
    | <grouping_set_item>  
    | ( <grouping_set_item> [ ,...n ] )  
  
<grouping_set_item> ::=  
      <group_by_expression>  
    | ROLLUP ( <group_by_expression> [ ,...n ] )  
    | CUBE ( <group_by_expression> [ ,...n ] )  
  

-- For backward compatibility only.
-- Non-ISO-Compliant Syntax for SQL Server and Azure SQL Database 
  
GROUP BY 
      [ ALL ] column-expression [ ,...n ] 
    | column-expression [ ,...n ] [ WITH { CUBE | ROLLUP } ]   

```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics 
  
GROUP BY {
      column-name [ WITH (DISTRIBUTED_AGG) ]  
    | column-expression
    | ROLLUP ( <group_by_expression> [ ,...n ] ) 
} [ ,...n ]

```

```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
GROUP BY {
      column-name [ WITH (DISTRIBUTED_AGG) ]  
    | column-expression
} [ ,...n ]

```  
    
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 
### <a name="column-expression"></a>*column-expression*  
Specifica una colonna o un calcolo di non aggregazione in una colonna. Questa colonna può appartenere a una tabella, una tabella derivata o una visualizzazione. La colonna deve essere presente nella clausola FROM dell'istruzione SELECT, ma non è necessario che sia inclusa nell'elenco SELECT. 

Per le espressioni valide, vedere [Espressioni](~/t-sql/language-elements/expressions-transact-sql.md).    

La colonna deve essere presente nella clausola FROM dell'istruzione SELECT, ma non è necessario che sia inclusa nell'elenco SELECT. Tuttavia, ogni colonna della tabella o della visualizzazione di qualsiasi espressione di non aggregazione presente nell'elenco \<select> deve essere inclusa nell'elenco GROUP BY:  
  
Le istruzioni seguenti sono consentite:  
  
```sql  
SELECT ColumnA, ColumnB FROM T GROUP BY ColumnA, ColumnB;  
SELECT ColumnA + ColumnB FROM T GROUP BY ColumnA, ColumnB;  
SELECT ColumnA + ColumnB FROM T GROUP BY ColumnA + ColumnB;  
SELECT ColumnA + ColumnB + constant FROM T GROUP BY ColumnA, ColumnB;  
```  
  
Le istruzioni seguenti non sono consentite:  
  
```sql  
SELECT ColumnA, ColumnB FROM T GROUP BY ColumnA + ColumnB;  
SELECT ColumnA + constant + ColumnB FROM T GROUP BY ColumnA + ColumnB;  
```  

L'espressione della colonna non può contenere:

- L'alias di una colonna definita nell'elenco SELECT. Può usare l'alias di una colonna per una tabella derivata definita nella clausola FROM.
- Una colonna di tipo **text**, **ntext** o **image**. È possibile tuttavia usare una colonna text, ntext o image come argomento di una funzione che restituisce un valore di un tipo di dati valido. Ad esempio, l'espressione può usare SUBSTRING() e CAST(). Ciò si applica anche alle espressioni nella clausola HAVING.
- metodi con tipo di dati xml. Può includere una funzione definita dall'utente che usa metodi con tipo di dati xml. Può includere una colonna calcolata che usa metodi con tipo di dati xml. 
- Sottoquery. Viene restituito l'errore 144. 
- Una colonna di una visualizzazione indicizzata. 
 
### <a name="group-by-column-expression--n-"></a>GROUP BY *column-expression* [ ,...n ] 

Raggruppa i risultati dell'istruzione SELECT in base ai valori in un elenco di una o più espressioni di colonna. 

Ad esempio, questa query crea una tabella Sales con colonne per Country, Region e Sales. Inserisce quattro righe, due delle quali con valori corrispondenti per Country e Region.  

```sql
CREATE TABLE Sales ( Country VARCHAR(50), Region VARCHAR(50), Sales INT );

INSERT INTO sales VALUES (N'Canada', N'Alberta', 100);
INSERT INTO sales VALUES (N'Canada', N'British Columbia', 200);
INSERT INTO sales VALUES (N'Canada', N'British Columbia', 300);
INSERT INTO sales VALUES (N'United States', N'Montana', 100);
```
La tabella Sales contiene le righe seguenti:

| Country | Region | Sales |
|---------|--------|-------|
| Canada | Alberta | 100 |
| Canada | British Columbia | 200 |
| Canada | British Columbia | 300 |
| Stati Uniti | Montana | 100 |

La query successiva raggruppa Country e Region e restituisce la somma aggregata per ogni combinazione di valori.  
 
```sql 
SELECT Country, Region, SUM(sales) AS TotalSales
FROM Sales
GROUP BY Country, Region;
```
Il risultato della query ha 3 righe poiché sono presenti 3  combinazioni di valori per Country e Region. Il valore TotalSales per Canada e British Columbia è la somma di due righe. 

| Country | Region | TotalSales |
|---------|--------|-------|
| Canada | Alberta | 100 |
| Canada | British Columbia | 500 |
| Stati Uniti | Montana | 100 |

### <a name="group-by-rollup"></a>GROUP BY ROLLUP

Crea un gruppo per ogni combinazione di espressioni di colonna. Esegue anche il rollup dei risultati in subtotali e totali complessivi. Per eseguire questa operazione, esegue uno spostamento da destra a sinistra riducendo il numero di espressioni di colonna su cui crea gruppi e aggregazioni. 

L'ordine delle colonne ha effetto sull'output di ROLLUP e può avere effetto sul numero di righe nel set di risultati.  

Ad esempio, `GROUP BY ROLLUP (col1, col2, col3, col4)` crea gruppi per ogni combinazione di espressioni di colonna negli elenchi seguenti.  

- col1, col2, col3, col4 
- col1, col2, col3, NULL
- col1, col2, NULL, NULL
- col1, NULL, NULL, NULL
- NULL, NULL, NULL, NULL --Totale complessivo

Usando la tabella dell'esempio precedente, questo codice esegue un'operazione GROUP BY ROLLUP anziché una semplice operazione GROUP BY.

```sql
SELECT Country, Region, SUM(Sales) AS TotalSales
FROM Sales
GROUP BY ROLLUP (Country, Region);
```

Il risultato della query ha le stesse aggregazioni di GROUP BY senza ROLLUP. Inoltre, crea i subtotali per ogni valore di Country. Infine, visualizza un totale complessivo per tutte le righe. Il risultato è simile al seguente:

| Country | Region | TotalSales |
| :------ | :----- | ---------: |
| Canada | Alberta | 100 |
| Canada | British Columbia | 500 |
| Canada | NULL | 600 |
| Stati Uniti | Montana | 100 |
| Stati Uniti | NULL | 100 |
| NULL | NULL | 700 |

### <a name="group-by-cube--"></a>GROUP BY CUBE ( )  

GROUP BY CUBE crea gruppi per tutte le possibili combinazioni di colonne. Per GROUP BY CUBE (a, b) il risultato include gruppi per i valori univoci di (a, b), (NULL, b), (a, NULL) e (NULL, NULL).

Usando la tabella degli esempi precedenti, questo codice esegue un'operazione GROUP BY CUBE in Country e Region. 

```sql
SELECT Country, Region, SUM(Sales) AS TotalSales
FROM Sales
GROUP BY CUBE (Country, Region);
```

Il risultato della query include gruppi per i valori univoci di (Country, Region), (NULL, Region), (Country, NULL) e (NULL, NULL). I risultati sono simili ai seguenti:

| Country | Region | TotalSales |
|---------|--------|-------|
| Canada | Alberta | 100 |
| NULL | Alberta | 100 |
| Canada | British Columbia | 500 |
| NULL | British Columbia | 500 |
| Stati Uniti | Montana | 100 |
| NULL | Montana | 100 |
| NULL | NULL | 700
| Canada | NULL | 600 |
| Stati Uniti | NULL | 100 |
   
 ### <a name="group-by-grouping-sets--"></a>GROUP BY GROUPING SETS ( )  
 
L'opzione GROUPING SETS consente di combinare più clausole GROUP BY in un'unica clausola GROUP BY. I risultati rappresentano l'equivalente di quelli della clausola UNION ALL dei gruppi specificati. 

Ad esempio, `GROUP BY ROLLUP (Country, Region)` e `GROUP BY GROUPING SETS ( ROLLUP (Country, Region) )` restituiscono gli stessi risultati. 

Quando GROUPING SETS ha due o più elementi, i risultati sono un'unione degli elementi. Questo esempio restituisce l'unione dei risultati di ROLLUP e CUBE per Country e Region.

```sql
SELECT Country, Region, SUM(Sales) AS TotalSales
FROM Sales
GROUP BY GROUPING SETS ( ROLLUP (Country, Region), CUBE (Country, Region) );
```

I risultati sono gli stessi di questa query che restituisce un'unione delle due istruzioni GROUP BY.

```sql
SELECT Country, Region, SUM(Sales) AS TotalSales
FROM Sales
GROUP BY ROLLUP (Country, Region)
UNION ALL
SELECT Country, Region, SUM(Sales) AS TotalSales
FROM Sales
GROUP BY CUBE (Country, Region);
```

SQL non consolida i gruppi duplicati generati per un elenco GROUPING SETS. Ad esempio, in `GROUP BY ( (), CUBE (Country, Region) )` entrambi gli elementi restituiscono una riga per il totale complessivo ed entrambe le righe saranno elencate nei risultati. 

 ### <a name="group-by-"></a>GROUP BY ()  
Specifica il gruppo vuoto che genera il totale complessivo. Ha la stessa funzione di uno degli elementi di GROUPING SET. Ad esempio, questa istruzione restituisce le vendite totali per ogni paese e quindi restituisce il totale complessivo per tutti i paesi.

```sql
SELECT Country, SUM(Sales) AS TotalSales
FROM Sales
GROUP BY GROUPING SETS ( Country, () );
```

### <a name="group-by--all--column-expression--n-"></a>GROUP BY [ ALL ] column-expression [ ,...n ] 

Si applica a: SQL Server e database SQL di Azure

NOTA:  questa sintassi è disponibile solo per la compatibilità con le versioni precedenti. Verrà rimossa in una versione futura. Evitare pertanto di usarla in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni che attualmente usano questa sintassi.

Specifica di includere tutti i gruppi nei risultati indipendentemente dal fatto che soddisfino i criteri di ricerca nella clausola WHERE. I gruppi che non soddisfano i criteri di ricerca hanno valore NULL per l'aggregazione. 

GROUP BY ALL:
- Questa operazione non è supportata in query che accedono a tabelle remote se la query include anche una clausola WHERE.
- L'operazione avrà esito negativo nelle colonne con l'attributo FILESTREAM.
  
### <a name="with-distributed_agg"></a>WITH (DISTRIBUTED_AGG)
Si applica a: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

L'hint per la query DISTRIBUTED_AGG forza nel sistema di elaborazione parallela elevata (Massively Parallel Processing, MPP) la ridistribuzione di una tabella in una colonna specifica prima dell'esecuzione di un'aggregazione. Solo una colonna della clausola GROUP BY può avere un hint per la query DISTRIBUTED_AGG. Al termine dell'esecuzione della query, la tabella ridistribuita viene eliminata. La tabella originale non viene modificata.  

NOTA:  l'hint per la query DISTRIBUTED_AGG è disponibile per compatibilità con le versioni precedenti di [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] e non migliora la prestazioni per la maggior parte delle query. Per impostazione predefinita, MPP distribuisce già i dati in base alle esigenze per migliorare le prestazioni per le aggregazioni. 
  
## <a name="general-remarks"></a>Osservazioni generali

### <a name="how-group-by-interacts-with-the-select-statement"></a>Interazione di GROUP BY con l'istruzione SELECT
Elenco SELECT:
- Aggregazione di vettori. Se l'elenco SELECT include funzioni di aggregazione, la clausola GROUP BY calcola un valore di riepilogo per ogni gruppo. Tali funzioni sono denominate aggregazioni vettoriali. 
- Aggregazioni DISTINCT. Le aggregazioni AVG (DISTINCT *column_name*), COUNT (DISTINCT *column_name*) e SUM (DISTINCT *column_name*) sono supportate con ROLLUP, CUBE e GROUPING SETS.
  
Clausola WHERE:
- SQL rimuove le righe che non soddisfano le condizioni presenti nella clausola WHERE prima che venga eseguita qualsiasi operazione di raggruppamento.  
  
Clausola HAVING:
- SQL usa la clausola HAVING per filtrare i gruppi nel set di risultati. 
  
Clausola ORDER BY:
- Per eseguire questa operazione, utilizzare la clausola ORDER BY. La clausola GROUP BY non ordina il set di risultati. 
  
Valori NULL:
- Se una colonna di raggruppamento contiene valori NULL, tutti i valori NULL sono considerati uguali e raccolti in un unico gruppo.   
  
## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni

Si applica a: SQL Server (a partire dalla versione 2008) e [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]

### <a name="maximum-capacity"></a>Capacità massima

Per una clausola GROUP BY che usa ROLLUP, CUBE o GROUPING SETS, il numero massimo di espressioni è 32. Il numero massimo di gruppi è 4096 (2<sup>12</sup>). Gli esempi seguenti non possono essere eseguiti correttamente perché la clausola GROUP BY ha più di 4096 gruppi.  
 
-   L'esempio seguente genera 4097 (2<sup>12</sup> + 1) set di raggruppamenti e ha esito negativo.  
  
    ```sql
    GROUP BY GROUPING SETS( CUBE(a1, ..., a12), b )  
    ```  
  
-   L'esempio seguente genera 4097 (2<sup>12</sup> + 1) gruppi e ha esito negativo. Sia `CUBE ()` sia il set di raggruppamenti `()` consentono di generare una riga del totale complessivo e i set di raggruppamenti duplicati non vengono eliminati.  
  
    ```sql
    GROUP BY GROUPING SETS( CUBE(a1, ..., a12), ())  
    ```  

-   Questo esempio usa la sintassi compatibile con le versioni precedenti. Genera 8192 (2<sup>13</sup>) set di raggruppamenti e ha esito negativo.  
  
    ```sql
    GROUP BY CUBE (a1, ..., a13)   
    GROUP BY a1, ..., a13 WITH CUBE   
    ```    
    Per clausole GROUP BY compatibili con le versioni precedenti che non includono CUBE o ROLLUP, il numero di elementi GROUP BY è limitato dalle dimensioni delle colonne GROUP BY, dalle colonne di aggregazione e dai valori di aggregazione interessati dalla query. Tale limite è correlato al limite di 8.060 byte della tabella di lavoro intermedia necessaria per mantenere i risultati intermedi delle query. Quando viene specificato l'operatore CUBE o ROLLUP, sono consentite al massimo 12 espressioni di raggruppamento.

### <a name="support-for-iso-and-ansi-sql-2006-group-by-features"></a>Supporto per le funzionalità GROUP BY di SQL-2006 ISO e ANSI

La clausola GROPU BY supporta tutte le funzionalità GROUP BY incluse nello standard SQL-2006 con le eccezioni di sintassi seguenti:  
  
-   Nella clausola GROUP BY non è consentito utilizzare set di raggruppamenti a meno che non appartengano a un elenco GROUPING SETS esplicito. Ad esempio, `GROUP BY Column1, (Column2, ...ColumnN`) è consentito nello standard ma non in Transact-SQL.  Transact-SQL supporta `GROUP BY C1, GROUPING SETS ((Column2, ...ColumnN))` e `GROUP BY Column1, Column2, ... ColumnN` che sono semanticamente equivalenti. Queste espressioni sono semanticamente equivalenti a quelle dell'esempio relativo a `GROUP BY` precedente. Ciò consente di evitare che `GROUP BY Column1, (Column2, ...ColumnN`) possa essere erroneamente interpretato come `GROUP BY C1, GROUPING SETS ((Column2, ...ColumnN))`, che non è semanticamente equivalente.  
  
-   L'utilizzo di set di raggruppamenti non è consentito all'interno di GROUPING SETS. Ad esempio, `GROUP BY GROUPING SETS (A1, A2,...An, GROUPING SETS (C1, C2, ...Cn))` è consentito nello standard SQL-2006 ma non in Transact-SQL. `GROUP BY GROUPING SETS( A1, A2,...An, C1, C2, ...Cn )` o `GROUP BY GROUPING SETS( (A1), (A2), ... (An), (C1), (C2), ... (Cn) )` sono consentiti in Transact-SQL e sono semanticamente equivalenti al primo esempio di GROUP BY ma hanno una sintassi più chiara.  
  
-   GROUP BY [ALL/DISTINCT] è consentito solo in una clausola GROUP BY semplice che contiene espressioni di colonna. Non è consentito con i costrutti GROUPING SETS, ROLLUP, CUBE, WITH CUBE o WITH ROLLUP. ALL è il valore predefinito ed è implicito. È anche consentito solo nella sintassi compatibile con le versioni precedenti.
  
### <a name="comparison-of-supported-group-by-features"></a>Confronto tra le funzionalità GROUP BY supportate  
 La tabella seguente descrive le funzionalità GROUP BY supportate in base alle versioni SQL e al livello di compatibilità del database.  
  
|Funzionalità|SQL Server Integration Services|Livello di compatibilità di SQL Server 100 o superiore|Livello di compatibilità di SQL Server 2008 o versioni successive pari a 90.|  
|-------------|-------------------------------------|--------------------------------------------------|-----------------------------------------------------------|  
|Aggregazioni DISTINCT|Non supportate per WITH CUBE o WITH ROLLUP.|Supportate per WITH CUBE, WITH ROLLUP, GROUPING SETS, CUBE o ROLLUP.|Comportamento analogo al livello di compatibilità pari a 100.|  
|Funzione definita dall'utente denominata CUBE o ROLLUP nella clausola GROUP BY|La funzione definita dall'utente **dbo.cube(** _arg1_ **,** _...argN_ **)** o **dbo.rollup(** _arg1_ **,** ..._argN_ **)** nella clausola GROUP BY è consentita.<br /><br /> Ad esempio: `SELECT SUM (x) FROM T  GROUP BY dbo.cube(y);`|La funzione definita dall'utente **dbo.cube (** _arg1_ **,** ...argN **)** o **dbo.rollup(** arg1 **,** _...argN_ **)** nella clausola GROUP BY non è consentita.<br /><br /> Ad esempio: `SELECT SUM (x) FROM T  GROUP BY dbo.cube(y);`<br /><br /> Viene restituito il messaggio di errore seguente: "Sintassi non corretta in prossimità della parola chiave 'cube'&#124;'rollup'."<br /><br /> Per evitare questo problema, sostituire `dbo.cube` con `[dbo].[cube]` oppure `dbo.rollup` con `[dbo].[rollup]`.<br /><br /> L'esempio seguente è consentito: `SELECT SUM (x) FROM T  GROUP BY [dbo].[cube](y);`|La funzione definita dall'utente **dbo.cube (** _arg1_ **,** _...argN_) o **dbo.rollup(** _arg1_ **,** _...argN_ **)** nella clausola GROUP BY è consentita<br /><br /> Ad esempio: `SELECT SUM (x) FROM T  GROUP BY dbo.cube(y);`|  
|GROUPING SETS|Non supportate|Supportato|Supportato|  
|CUBE|Non supportate|Supportato|Non supportate|  
|ROLLUP|Non supportate|Supportato|Non supportate|  
|Totale complessivo, ad esempio GROUP BY ()|Non supportate|Supportato|Supportato|  
|GROUPING_ID - funzione|Non supportate|Supportato|Supportato|  
|GROUPING - funzione|Supportato|Supportato|Supportato|  
|WITH CUBE|Supportato|Supportato|Supportato|  
|WITH ROLLUP|Supportato|Supportato|Supportato|  
|Rimozione di raggruppamenti duplicati relativa a WITH CUBE o WITH ROLLUP|Supportato|Supportato|Supportato| 
 
  
## <a name="examples"></a>Esempi  
  
### <a name="a-use-a-simple-group-by-clause"></a>R. Uso di una clausola GROUP BY semplice  
 Nell'esempio seguente viene recuperato il totale per ogni `SalesOrderID` dalla tabella `SalesOrderDetail`. Questo esempio usa AdventureWorks.  
  
```sql  
SELECT SalesOrderID, SUM(LineTotal) AS SubTotal  
FROM Sales.SalesOrderDetail AS sod  
GROUP BY SalesOrderID  
ORDER BY SalesOrderID;  
```  
  
### <a name="b-use-a-group-by-clause-with-multiple-tables"></a>B. Uso di una clausola GROUP BY con più tabelle  
 Nell'esempio seguente viene recuperato il numero di dipendenti per ogni `City` dalla tabella `Address` unita in join alla tabella `EmployeeAddress`. Questo esempio usa AdventureWorks. 
  
```sql  
SELECT a.City, COUNT(bea.AddressID) EmployeeCount  
FROM Person.BusinessEntityAddress AS bea   
    INNER JOIN Person.Address AS a  
        ON bea.AddressID = a.AddressID  
GROUP BY a.City  
ORDER BY a.City;  
```  
  
### <a name="c-use-a-group-by-clause-with-an-expression"></a>C. Uso di una clausola GROUP BY con un'espressione  
 Nell'esempio seguente vengono recuperate le vendite totali per ogni anno tramite la funzione `DATEPART`. La stessa espressione deve essere presente sia nell'elenco `SELECT` che nella clausola `GROUP BY`.  
  
```sql  
SELECT DATEPART(yyyy,OrderDate) AS N'Year'  
    ,SUM(TotalDue) AS N'Total Order Amount'  
FROM Sales.SalesOrderHeader  
GROUP BY DATEPART(yyyy,OrderDate)  
ORDER BY DATEPART(yyyy,OrderDate);  
```  
  
### <a name="d-use-a-group-by-clause-with-a-having-clause"></a>D. Uso di una clausola GROUP BY con una clausola HAVING  
 Nell'esempio seguente viene utilizzata la clausola `HAVING` per specificare quale gruppo generato nella clausola `GROUP BY` deve essere incluso nel set di risultati.  
  
```sql  
SELECT DATEPART(yyyy,OrderDate) AS N'Year'  
    ,SUM(TotalDue) AS N'Total Order Amount'  
FROM Sales.SalesOrderHeader  
GROUP BY DATEPART(yyyy,OrderDate)  
HAVING DATEPART(yyyy,OrderDate) >= N'2003'  
ORDER BY DATEPART(yyyy,OrderDate);  
```  
  
## <a name="examples-azure-synapse-analytics-and-parallel-data-warehouse"></a>Esempi: Azure Synapse Analytics e Parallel Data Warehouse  
  
### <a name="e-basic-use-of-the-group-by-clause"></a>E. Uso di base della clausola GROUP BY  
 L'esempio seguente trova l'importo totale di tutte le vendite in ogni giornata. Viene restituita una sola riga contenente il totale di tutte le vendite per ogni giornata.  
  
```sql  
-- Uses AdventureWorksDW  
  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales FROM FactInternetSales  
GROUP BY OrderDateKey ORDER BY OrderDateKey;  
```  
  
### <a name="f-basic-use-of-the-distributed_agg-hint"></a>F. Uso di base dell'hint DISTRIBUTED_AGG  
 Questo esempio usa l'hint per la query DISTRIBUTED_AGG per forzare nell'appliance la distribuzione casuale della tabella nella colonna `CustomerKey` prima di eseguire l'aggregazione.  
  
```sql  
-- Uses AdventureWorksDW  
  
SELECT CustomerKey, SUM(SalesAmount) AS sas  
FROM FactInternetSales  
GROUP BY CustomerKey WITH (DISTRIBUTED_AGG)  
ORDER BY CustomerKey DESC;  
```  
  
### <a name="g-syntax-variations-for-group-by"></a>G. Variazioni della sintassi per GROUP BY  
 Quando l'elenco SELECT non include aggregazioni, ogni colonna dell'elenco SELECT deve essere inclusa nell'elenco GROUP BY. Le colonne calcolate dell'elenco SELECT possono essere incluse, ma non sono richieste, nell'elenco GROUP BY. Di seguito sono riportati esempi di istruzioni SELECT sintatticamente valide:  
  
```sql  
-- Uses AdventureWorks  
  
SELECT LastName, FirstName FROM DimCustomer GROUP BY LastName, FirstName;  
SELECT NumberCarsOwned FROM DimCustomer GROUP BY YearlyIncome, NumberCarsOwned;  
SELECT (SalesAmount + TaxAmt + Freight) AS TotalCost FROM FactInternetSales GROUP BY SalesAmount, TaxAmt, Freight;  
SELECT SalesAmount, SalesAmount*1.10 SalesTax FROM FactInternetSales GROUP BY SalesAmount;  
SELECT SalesAmount FROM FactInternetSales GROUP BY SalesAmount, SalesAmount*1.10;  
```  
  
### <a name="h-using-a-group-by-with-multiple-group-by-expressions"></a>H. Uso di GROUP BY con più espressioni GROUP BY  
 L'esempio seguente raggruppa i risultati usando più criteri `GROUP BY`. Se, all'interno di ogni gruppo `OrderDateKey`, sono presenti sottogruppi che possono essere differenziati da `DueDateKey`, verrà definito un nuovo raggruppamento per il set di risultati.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT OrderDateKey, DueDateKey, SUM(SalesAmount) AS TotalSales   
FROM FactInternetSales
GROUP BY OrderDateKey, DueDateKey   
ORDER BY OrderDateKey;  
```  
  
### <a name="i-using-a-group-by-clause-with-a-having-clause"></a>I. Utilizzo di una clausola GROUP BY con una clausola HAVING  
 L'esempio seguente usa la clausola `HAVING` per specificare i gruppi generati nella clausola `GROUP BY` che devono essere inclusi nel set di risultati. Nei risultati verranno inclusi solo i gruppi con date dell'ordine del 2004 o successive.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales   
FROM FactInternetSales  
GROUP BY OrderDateKey   
HAVING OrderDateKey > 20040000   
ORDER BY OrderDateKey;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [GROUPING_ID &#40;Transact-SQL&#41;](~/t-sql/functions/grouping-id-transact-sql.md)   
 [GROUPING &#40;Transact-SQL&#41;](~/t-sql/functions/grouping-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](~/t-sql/queries/select-transact-sql.md)   
 [Clausola SELECT &#40;Transact-SQL&#41;](~/t-sql/queries/select-clause-transact-sql.md)  
  
  




