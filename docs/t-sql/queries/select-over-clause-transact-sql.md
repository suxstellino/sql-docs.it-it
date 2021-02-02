---
title: Clausola OVER (Transact-SQL) | Microsoft Docs
description: Informazioni di riferimento Transact-SQL per la clausola OVER, che definisce un set di righe specificato dall'utente all'interno di un set di risultati della query.
ms.date: 08/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OVER_TSQL
- OVER
dev_langs:
- TSQL
helpviewer_keywords:
- order of rowsets [SQL Server]
- rowsets [SQL Server], windowing
- window function
- partitions [SQL Server], rowsets
- clauses [SQL Server], OVER
- rowsets [SQL Server], partitioning
- rowsets [SQL Server], ordering
- OVER clause
ms.assetid: ddcef3a6-0341-43e0-ae73-630484b7b398
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f1caba28e45e1eea3217f41e0dc37789f4e3054e
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250756"
---
# <a name="select---over-clause-transact-sql"></a>Clausola SELECT - OVER (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Determina il partizionamento e l'ordinamento di un set di righe prima dell'applicazione della funzione finestra associata. In altre parole, la clausola OVER definisce una finestra o un set di righe specificato dall'utente all'interno di un set di risultati della query. Una funzione finestra calcola quindi un valore per ogni riga della finestra. È possibile utilizzare la clausola OVER con le funzioni per calcolare i valori aggregati, ad esempio medie mobili, aggregazioni cumulative, totali parziali o i primi N risultati per gruppo.  
  
-   [Funzioni di rango](../../t-sql/functions/ranking-functions-transact-sql.md)  
  
-   [Funzioni di aggregazione](../../t-sql/functions/aggregate-functions-transact-sql.md)  
  
-   [Funzioni analitiche](../../t-sql/functions/analytic-functions-transact-sql.md)  
  
-   [Funzione NEXT VALUE FOR](../../t-sql/functions/next-value-for-transact-sql.md)  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Syntax for SQL Server, Azure SQL Database, and Azure Synapse Analytics  
  
OVER (   
       [ <PARTITION BY clause> ]  
       [ <ORDER BY clause> ]   
       [ <ROW or RANGE clause> ]  
      )  
  
<PARTITION BY clause> ::=  
PARTITION BY value_expression , ... [ n ]  
  
<ORDER BY clause> ::=  
ORDER BY order_by_expression  
    [ COLLATE collation_name ]   
    [ ASC | DESC ]   
    [ ,...n ]  
  
<ROW or RANGE clause> ::=  
{ ROWS | RANGE } <window frame extent>  
  
<window frame extent> ::=   
{   <window frame preceding>  
  | <window frame between>  
}  
<window frame between> ::=   
  BETWEEN <window frame bound> AND <window frame bound>  
  
<window frame bound> ::=   
{   <window frame preceding>  
  | <window frame following>  
}  
  
<window frame preceding> ::=   
{  
    UNBOUNDED PRECEDING  
  | <unsigned_value_specification> PRECEDING  
  | CURRENT ROW  
}  
  
<window frame following> ::=   
{  
    UNBOUNDED FOLLOWING  
  | <unsigned_value_specification> FOLLOWING  
  | CURRENT ROW  
}  
  
<unsigned value specification> ::=   
{  <unsigned integer literal> }  
  
```  
  
```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
OVER ( [ PARTITION BY value_expression ] [ order_by_clause ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

Nella clausola `OVER` delle funzioni finestra possono essere presenti gli argomenti seguenti:
- [PARTITION BY](#partition-by), che suddivide il set dei risultati della query in partizioni.
- [ORDER BY](#order-by), che definisce l'ordine logico delle righe all'interno di ogni partizione del set di risultati. 
- [ROWS/RANGE](#rows-or-range), che limita le righe all'interno della partizione specificando i punti iniziali e finali. Richiede l’uso dell’argomento `ORDER BY`e il valore predefinito è dall'inizio della partizione fino all'elemento corrente se viene specificato l'argomento `ORDER BY`.

Se non si specifica alcun argomento, le funzioni finestra vengono applicate all'intero set di risultati.
```sql
select 
      object_id
    , [min] = min(object_id) over()
    , [max] = max(object_id) over()
from sys.objects
```
 
|object_id | min | max |
|---|---|---|
| 3 | 3 | 2139154666 |
| 5 | 3 | 2139154666 |
| ... | ... | ... |
| 2123154609 |  3 | 2139154666 |
| 2139154666 |  3 | 2139154666 |

### <a name="partition-by"></a>PARTITION BY  
 Suddivide il set di risultati della query in partizioni. La funzione finestra viene applicata a ogni singola partizione e il calcolo viene riavviato per ogni partizione.  

```sqlsyntax
PARTITION BY *value_expression* 
```
 
 Se PARTITION BY non viene specificato, la funzione considera tutte le righe del set di risultati della query come un’unica partizione.
Se non si specifica la clausola `ORDER BY`, la funzione viene applicata a tutte le righe della partizione.
  
#### <a name="partition-by-value_expression"></a>PARTITION BY *value_expression*  
 Specifica la colonna in base alla quale viene partizionato il set di righe. *value_expression* può fare riferimento solo alle colonne rese disponibili dalla clausola FROM. *value_expression* non può fare riferimento a espressioni o alias nell'elenco di selezione. *value_expression* può essere un'espressione di colonna, una sottoquery scalare, una funzione scalare o una variabile definita dall'utente. 
 
 ```sql
 select 
      object_id, type
    , [min] = min(object_id) over(partition by type)
    , [max] = max(object_id) over(partition by type)
from sys.objects
```

|object_id | tipo | min | max |
|---|---|---|---|
| 68195293  | PK    | 68195293  | 711673583 |
| 631673298 | PK    | 68195293  | 711673583 |
| 711673583 | PK    | 68195293  | 711673583 |
| ... | ... | ... |
| 3 | S | 3 | 98 |
| 5 | S |   3   | 98 |
| ... | ... | ... |
| 98    | S |   3   | 98 |
| ... | ... | ... |
  
### <a name="order-by"></a>ORDER BY  

```sqlsyntax
ORDER BY *order_by_expression* [COLLATE *collation_name*] [ASC|DESC]  
```

 Definisce l'ordine logico delle righe all'interno di ogni partizione del set di risultati. In altre parole, specifica l'ordine logico in cui viene eseguito il calcolo della funzione finestra. 
 - Se non viene specificato, l'ordine predefinito è `ASC` e la funzione finestra utilizza tutte le righe della partizione.
 - Se viene specificato e non viene specificato un intervallo di righe/intervallo, per impostazione predefinita `RANGE UNBOUNDED PRECEDING AND CURRENT ROW` viene usato il valore predefinito per la cornice della finestra dalle funzioni che possono accettare una specifica facoltativa di righe/intervalli, ad esempio `min` o `max` . 
 
```sql
select 
      object_id, type
    , [min] = min(object_id) over(partition by type order by object_id)
    , [max] = max(object_id) over(partition by type order by object_id)
from sys.objects
```

|object_id | tipo | min | max |
|---|---|---|---|
| 68195293  | PK    | 68195293  | 68195293 |
| 631673298 | PK    | 68195293  | 631673298 |
| 711673583 | PK    | 68195293  | 711673583 |
| ... | ... | ... |
| 3 | S | 3 | 3 |
| 5 | S |   3 | 5 |
| 6 | S |   3 | 6 |
| ... | ... | ... |
| 97    | S |   3 | 97 |
| 98    | S |   3 | 98 |
| ... | ... | ... |


 *order_by_expression*  
 Specifica una colonna o un'espressione in base alla quale eseguire l'ordinamento. *order_by_expression* può fare riferimento solo alle colonne rese disponibili dalla clausola FROM. Non è possibile specificare un numero intero per rappresentare un alias o un nome di colonna.  
  
 COLLATE *collation_name*  
 Specifica che l'operazione ORDER BY deve essere eseguita in base alle regole di confronto definite in *collation_name*. In *collation_name* è possibile usare nomi di regole di confronto di Windows o SQL. Per altre informazioni, vedere [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md). COLLATE può essere applicato solo alle colonne di tipo **char**, **varchar**, **nchar** e **nvarchar**.  
  
 **ASC** | DESC  
 Specifica che i valori nella colonna specificata devono essere ordinati in ordine crescente o decrescente. ASC è l'ordinamento predefinito. I valori Null vengono considerati i valori in assoluto più piccoli.  
  
### <a name="rows-or-range"></a>ROWS o RANGE  
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive. 
  
 Limita ulteriormente le righe all'interno della partizione specificando i punti iniziali e finali. A tale scopo, è necessario specificare un intervallo di righe rispetto alla riga corrente in base all'associazione logica o all'associazione fisica. L'associazione fisica viene eseguita mediante la clausola ROWS.  
  
 La clausola ROWS limita le righe all'interno di una partizione specificando un numero fisso di righe precedenti o successive alla riga corrente. In alternativa, la clausola RANGE limita logicamente le righe all'interno di una partizione specificando un intervallo di valori rispetto al valore nella riga corrente. Le righe precedenti e successive vengono definite in base all'ordinamento nella clausola ORDER BY. La cornice di finestra "RANGE ... CURRENT ROW ..." include tutte le righe che contengono gli stessi valori nell'espressione ORDER BY della riga corrente. Ad esempio, ROWS BETWEEN 2 PRECEDING AND CURRENT ROW significa che la finestra di righe a cui viene applicata la funzione è di tre righe, a partire dalle 2 righe precedenti fino alla riga corrente inclusa.  
 
```sql
select
      object_id
    , [preceding]   = count(*) over(order by object_id ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW )
    , [central] = count(*) over(order by object_id ROWS BETWEEN 2 PRECEDING AND 2 FOLLOWING )
    , [following]   = count(*) over(order by object_id ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING)
from sys.objects
order by object_id asc
```

|object_id | preceding | central | following |
|---|---|---|---|
| 3 | 1 | 3 | 156 |
| 5 | 2 | 4 | 155 |
| 6 | 3 | 5 | 154 |
| 7 | 4 | 5 | 153 |
| 8 | 5 | 5 | 152 |
| ...   | ...   | ...   | ... |
| 2112726579    | 153   | 5 | 4 |
| 2119678599    | 154   | 5 | 3 |
| 2123154609    | 155   | 4 | 2 |
| 2139154666    | 156   | 3 | 1 | 
  
> [!NOTE]  
>  ROWS o RANGE richiede che venga specificata la clausola ORDER BY. Se ORDER BY contiene più espressioni di ordinamento, CURRENT ROW FOR RANGE considera tutte le colonne nell'elenco ORDER BY per la determinazione della riga corrente.  
  
#### <a name="unbounded-preceding"></a>UNBOUNDED PRECEDING  
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.  
  
 Specifica che la finestra inizia in corrispondenza della prima riga della partizione. UNBOUNDED PRECEDING può essere specificato solo come punto iniziale della finestra.  
  
 \<unsigned value specification> PRECEDING  
 Specificato con \<unsigned value specification> per indicare il numero di righe o valori che deve precedere la riga corrente. Questa specifica non è consentita per RANGE.  
  
#### <a name="current-row"></a>CURRENT ROW  
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive. 
  
 Specifica che la finestra inizia o termina in corrispondenza della riga corrente quando viene utilizzato con ROWS o in corrispondenza del valore corrente quando viene utilizzato con RANGE. CURRENT ROW può essere specificato sia come punto iniziale che come punto finale.  
  
#### <a name="between-and"></a>BETWEEN AND  
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive. 
  
```sqlsyntax
BETWEEN <window frame bound > AND <window frame bound >  
```
 Utilizzato con ROWS o RANGE per specificare i punti limite inferiore (punto iniziale) e superiore (punto finale) della finestra. Il valore \<window frame bound> definisce il punto limite iniziale e il valore \<window frame bound> definisce il punto limite finale. Il limite superiore non può essere minore del limite inferiore.  
  
#### <a name="unbounded-following"></a>UNBOUNDED FOLLOWING  
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive. 
  
 Specifica che la finestra termina in corrispondenza dell'ultima riga della partizione. UNBOUNDED FOLLOWING può essere specificato solo come punto finale della finestra. Ad esempio, RANGE BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING definisce una finestra che inizia in corrispondenza della riga corrente e termina in corrispondenza dell'ultima riga della partizione.  
  
 \<unsigned value specification> FOLLOWING  
 Specificato con \<unsigned value specification> per indicare il numero di righe o valori da seguire per la riga corrente. Quando \<unsigned value specification> FOLLOWING viene specificato come punto iniziale della finestra, il punto finale deve essere \<unsigned value specification>FOLLOWING. Ad esempio, ROWS BETWEEN 2 FOLLOWING AND 10 FOLLOWING definisce una finestra che inizia in corrispondenza della seconda riga successiva alla riga corrente e termina in corrispondenza della decima riga successiva alla riga corrente. Questa specifica non è consentita per RANGE.  
  
 valore letterale integer senza segno  
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.  
  
 Valore letterale integer positivo (incluso 0) che specifica il numero di righe o valori che deve precedere o seguire la riga o il valore corrente. Questa specifica è valida solo per ROWS.  
  
## <a name="general-remarks"></a>Osservazioni generali  
 È possibile utilizzare più funzioni finestra in una singola query con una singola clausola FROM. La clausola OVER di ogni funzione può specificare un partizionamento e un ordinamento diversi.  
  
 Se PARTITION BY non viene specificato, la funzione considera tutte le righe del set di risultati della query come un unico gruppo. 
 
### <a name="important"></a>Importante!

Se viene specificato ROWS/RANGE e \<window frame preceding> viene usato come \<window frame extent> (sintassi breve), questa specifica verrà usata per il punto limite iniziale della cornice della finestra e CURRENT ROW verrà usato per il punto limite finale. Ad esempio, "ROWS 5 PRECEDING" corrisponde a "ROWS BETWEEN 5 PRECEDING AND CURRENT ROW".  
  
> [!NOTE]
> Se ORDER BY non viene specificato, viene utilizzata l'intera partizione per una cornice di finestra. Si applica solo alle funzioni che non richiedono la clausola ORDER BY. Se ROWS/RANGE non viene specificato, ma si specifica ORDER BY, come valore predefinito per la cornice della finestra viene utilizzato RANGE UNBOUNDED PRECEDING AND CURRENT ROW. Si applica solo alle funzioni che possono accettare la specifica facoltativa di ROWS/RANGE. Le funzioni di rango, ad esempio, non possono accettare ROWS/RANGE, pertanto questa cornice della finestra non viene applicata anche se ORDER BY è presente e ROWS/RANGE non lo è.  
    
## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni  
 Non è possibile utilizzare la clausola OVER con la funzione di aggregazione CHECKSUM.  
  
 RANGE non può essere usato con \<unsigned value specification> PRECEDING o \<unsigned value specification> FOLLOWING.  
  
 A seconda della funzione analitica, di aggregazione o di rango usata con la clausola OVER, è possibile che la clausola \<ORDER BY clause> e/o la clausola \<ROWS and RANGE clause> non siano supportate.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-the-over-clause-with-the-row_number-function"></a>R. Utilizzo della clausola OVER con la funzione ROW_NUMBER  
 Nell'esempio seguente viene illustrato l'utilizzo della clausola OVER con la funzione ROW_NUMBER per visualizzare un numero di riga per ogni riga all'interno di una partizione. La clausola ORDER BY specificata nella clausola OVER ordina le righe in ogni partizione in base alla colonna `SalesYTD`. La clausola ORDER BY nell'istruzione SELECT determina l'ordine in cui viene restituito l'intero set di risultati della query.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT ROW_NUMBER() OVER(PARTITION BY PostalCode ORDER BY SalesYTD DESC) AS "Row Number",   
    p.LastName, s.SalesYTD, a.PostalCode  
FROM Sales.SalesPerson AS s   
    INNER JOIN Person.Person AS p   
        ON s.BusinessEntityID = p.BusinessEntityID  
    INNER JOIN Person.Address AS a   
        ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL   
    AND SalesYTD <> 0  
ORDER BY PostalCode;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Row Number      LastName                SalesYTD              PostalCode 
 --------------- ----------------------- --------------------- ---------- 
 1               Mitchell                4251368.5497          98027 
 2               Blythe                  3763178.1787          98027 
 3               Carson                  3189418.3662          98027 
 4               Reiter                  2315185.611           98027 
 5               Vargas                  1453719.4653          98027  
 6               Ansman-Wolfe            1352577.1325          98027  
 1               Pak                     4116871.2277          98055  
 2               Varkey Chudukatil       3121616.3202          98055  
 3               Saraiva                 2604540.7172          98055  
 4               Ito                     2458535.6169          98055  
 5               Valdez                  1827066.7118          98055  
 6               Mensa-Annan             1576562.1966          98055  
 7               Campbell                1573012.9383          98055  
 8               Tsoflias                1421810.9242          98055
 ```  
  
### <a name="b-using-the-over-clause-with-aggregate-functions"></a>B. Utilizzo della clausola OVER con funzioni di aggregazione  
 Nell'esempio seguente viene utilizzata la clausola `OVER` con funzioni di aggregazione su tutte le righe restituite dalla query. Nell'esempio l'utilizzo della clausola `OVER` è più efficace rispetto all'utilizzo di sottoquery per derivare i valori di aggregazione.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT SalesOrderID, ProductID, OrderQty  
    ,SUM(OrderQty) OVER(PARTITION BY SalesOrderID) AS Total  
    ,AVG(OrderQty) OVER(PARTITION BY SalesOrderID) AS "Avg"  
    ,COUNT(OrderQty) OVER(PARTITION BY SalesOrderID) AS "Count"  
    ,MIN(OrderQty) OVER(PARTITION BY SalesOrderID) AS "Min"  
    ,MAX(OrderQty) OVER(PARTITION BY SalesOrderID) AS "Max"  
FROM Sales.SalesOrderDetail   
WHERE SalesOrderID IN(43659,43664);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
SalesOrderID ProductID   OrderQty Total       Avg         Count       Min    Max  
------------ ----------- -------- ----------- ----------- ----------- ------ ------  
43659        776         1        26          2           12          1      6  
43659        777         3        26          2           12          1      6  
43659        778         1        26          2           12          1      6  
43659        771         1        26          2           12          1      6  
43659        772         1        26          2           12          1      6  
43659        773         2        26          2           12          1      6  
43659        774         1        26          2           12          1      6  
43659        714         3        26          2           12          1      6  
43659        716         1        26          2           12          1      6  
43659        709         6        26          2           12          1      6  
43659        712         2        26          2           12          1      6  
43659        711         4        26          2           12          1      6  
43664        772         1        14          1           8           1      4  
43664        775         4        14          1           8           1      4  
43664        714         1        14          1           8           1      4  
43664        716         1        14          1           8           1      4  
43664        777         2        14          1           8           1      4  
43664        771         3        14          1           8           1      4  
43664        773         1        14          1           8           1      4  
43664        778         1        14          1           8           1      4  
```  
  
 Nell'esempio seguente viene illustrato l'utilizzo della clausola `OVER` con una funzione di aggregazione in un valore calcolato.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT SalesOrderID, ProductID, OrderQty  
    ,SUM(OrderQty) OVER(PARTITION BY SalesOrderID) AS Total  
    ,CAST(1. * OrderQty / SUM(OrderQty) OVER(PARTITION BY SalesOrderID)   
        *100 AS DECIMAL(5,2))AS "Percent by ProductID"  
FROM Sales.SalesOrderDetail   
WHERE SalesOrderID IN(43659,43664);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)] Si noti che le aggregazioni vengono calcolate in base a `SalesOrderID`, mentre `Percent by ProductID` viene calcolato per ogni riga di ogni `SalesOrderID`.  
  
```  
SalesOrderID ProductID   OrderQty Total       Percent by ProductID  
------------ ----------- -------- ----------- ---------------------------------------  
43659        776         1        26          3.85  
43659        777         3        26          11.54  
43659        778         1        26          3.85  
43659        771         1        26          3.85  
43659        772         1        26          3.85  
43659        773         2        26          7.69  
43659        774         1        26          3.85  
43659        714         3        26          11.54  
43659        716         1        26          3.85  
43659        709         6        26          23.08  
43659        712         2        26          7.69  
43659        711         4        26          15.38  
43664        772         1        14          7.14  
43664        775         4        14          28.57  
43664        714         1        14          7.14  
43664        716         1        14          7.14  
43664        777         2        14          14.29  
43664        771         3        14          21.4  
43664        773         1        14          7.14  
43664        778         1        14          7.14  
  
 (20 row(s) affected)  
```  
  
### <a name="c-producing-a-moving-average-and-cumulative-total"></a>C. Generazione di una media mobile e di un totale cumulativo  
 Nell'esempio seguente vengono utilizzate le funzioni AVG e SUM con la clausola OVER per fornire una media mobile e un totale cumulativo delle vendite annuali per ogni area presente nella tabella `Sales.SalesPerson`. I dati vengono partizionati in base a `TerritoryID` e ordinati logicamente in base a `SalesYTD`. La funzione AVG viene pertanto calcolata per ogni area in base all'anno di vendita. Si noti che per `TerritoryID` 1, sono presenti due righe per l'anno di vendita 2005, a indicare due venditori con vendite in tale anno. Viene calcolata la media delle vendite delle due righe e nel calcolo viene quindi inclusa la terza riga che rappresenta le vendite per l'anno 2006.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT BusinessEntityID, TerritoryID   
   ,DATEPART(yy,ModifiedDate) AS SalesYear  
   ,CONVERT(VARCHAR(20),SalesYTD,1) AS  SalesYTD  
   ,CONVERT(VARCHAR(20),AVG(SalesYTD) OVER (PARTITION BY TerritoryID   
                                            ORDER BY DATEPART(yy,ModifiedDate)   
                                           ),1) AS MovingAvg  
   ,CONVERT(VARCHAR(20),SUM(SalesYTD) OVER (PARTITION BY TerritoryID   
                                            ORDER BY DATEPART(yy,ModifiedDate)   
                                            ),1) AS CumulativeTotal  
FROM Sales.SalesPerson  
WHERE TerritoryID IS NULL OR TerritoryID < 5  
ORDER BY TerritoryID,SalesYear;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
BusinessEntityID TerritoryID SalesYear   SalesYTD             MovingAvg            CumulativeTotal  
---------------- ----------- ----------- -------------------- -------------------- --------------------  
274              NULL        2005        559,697.56           559,697.56           559,697.56  
287              NULL        2006        519,905.93           539,801.75           1,079,603.50  
285              NULL        2007        172,524.45           417,375.98           1,252,127.95  
283              1           2005        1,573,012.94         1,462,795.04         2,925,590.07  
280              1           2005        1,352,577.13         1,462,795.04         2,925,590.07  
284              1           2006        1,576,562.20         1,500,717.42         4,502,152.27  
275              2           2005        3,763,178.18         3,763,178.18         3,763,178.18  
277              3           2005        3,189,418.37         3,189,418.37         3,189,418.37  
276              4           2005        4,251,368.55         3,354,952.08         6,709,904.17  
281              4           2005        2,458,535.62         3,354,952.08         6,709,904.17  
  
(10 row(s) affected)  
  
```  
  
 In questo esempio la clausola OVER non include PARTITION BY. La funzione verrà pertanto applicata a tutte le righe restituite dalla query. La clausola ORDER BY specificata nella clausola OVER determina l'ordine logico in base al quale viene applicata la funzione AVG. La query restituisce una media mobile delle vendite annuali per tutte le aree di vendita specificate nella clausola WHERE. La clausola ORDER BY specificata nell'istruzione SELECT determina l'ordine in cui vengono visualizzate le righe della query.  
  
```sql  
SELECT BusinessEntityID, TerritoryID   
   ,DATEPART(yy,ModifiedDate) AS SalesYear  
   ,CONVERT(VARCHAR(20),SalesYTD,1) AS SalesYTD  
   ,CONVERT(VARCHAR(20),AVG(SalesYTD) OVER (ORDER BY DATEPART(yy,ModifiedDate)   
                                            ),1) AS MovingAvg  
   ,CONVERT(VARCHAR(20),SUM(SalesYTD) OVER (ORDER BY DATEPART(yy,ModifiedDate)   
                                            ),1) AS CumulativeTotal  
FROM Sales.SalesPerson  
WHERE TerritoryID IS NULL OR TerritoryID < 5  
ORDER BY SalesYear;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
BusinessEntityID TerritoryID SalesYear   SalesYTD             MovingAvg            CumulativeTotal  
---------------- ----------- ----------- -------------------- -------------------- --------------------  
274              NULL        2005        559,697.56           2,449,684.05         17,147,788.35  
275              2           2005        3,763,178.18         2,449,684.05         17,147,788.35  
276              4           2005        4,251,368.55         2,449,684.05         17,147,788.35  
277              3           2005        3,189,418.37         2,449,684.05         17,147,788.35  
280              1           2005        1,352,577.13         2,449,684.05         17,147,788.35  
281              4           2005        2,458,535.62         2,449,684.05         17,147,788.35  
283              1           2005        1,573,012.94         2,449,684.05         17,147,788.35  
284              1           2006        1,576,562.20         2,138,250.72         19,244,256.47  
287              NULL        2006        519,905.93           2,138,250.72         19,244,256.47  
285              NULL        2007        172,524.45           1,941,678.09         19,416,780.93  
(10 row(s) affected)  
```  
  
### <a name="d-specifying-the-rows-clause"></a>D. Specifica della clausola ROWS  
  
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.  
  
 Nell'esempio seguente viene usata la clausola ROWS per definire una finestra su cui vengono calcolate le righe come la riga corrente e il numero *N* di righe che seguono (in questo esempio 1 riga).  
  
```sql  
SELECT BusinessEntityID, TerritoryID   
    ,CONVERT(VARCHAR(20),SalesYTD,1) AS SalesYTD  
    ,DATEPART(yy,ModifiedDate) AS SalesYear  
    ,CONVERT(VARCHAR(20),SUM(SalesYTD) OVER (PARTITION BY TerritoryID   
                                             ORDER BY DATEPART(yy,ModifiedDate)   
                                             ROWS BETWEEN CURRENT ROW AND 1 FOLLOWING ),1) AS CumulativeTotal  
FROM Sales.SalesPerson  
WHERE TerritoryID IS NULL OR TerritoryID < 5;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
BusinessEntityID TerritoryID SalesYTD             SalesYear   CumulativeTotal  
---------------- ----------- -------------------- ----------- --------------------  
274              NULL        559,697.56           2005        1,079,603.50  
287              NULL        519,905.93           2006        692,430.38  
285              NULL        172,524.45           2007        172,524.45  
283              1           1,573,012.94         2005        2,925,590.07  
280              1           1,352,577.13         2005        2,929,139.33  
284              1           1,576,562.20         2006        1,576,562.20  
275              2           3,763,178.18         2005        3,763,178.18  
277              3           3,189,418.37         2005        3,189,418.37  
276              4           4,251,368.55         2005        6,709,904.17  
281              4           2,458,535.62         2005        2,458,535.62  
```  
  
 Nell'esempio seguente la clausola ROWS viene specificata con UNBOUNDED PRECEDING. Come risultato si ottiene che la finestra inizia in corrispondenza della prima riga della partizione.  
  
```sql  
SELECT BusinessEntityID, TerritoryID   
    ,CONVERT(VARCHAR(20),SalesYTD,1) AS SalesYTD  
    ,DATEPART(yy,ModifiedDate) AS SalesYear  
    ,CONVERT(VARCHAR(20),SUM(SalesYTD) OVER (PARTITION BY TerritoryID   
                                             ORDER BY DATEPART(yy,ModifiedDate)   
                                             ROWS UNBOUNDED PRECEDING),1) AS CumulativeTotal  
FROM Sales.SalesPerson  
WHERE TerritoryID IS NULL OR TerritoryID < 5;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
BusinessEntityID TerritoryID SalesYTD             SalesYear   CumulativeTotal  
---------------- ----------- -------------------- ----------- --------------------  
274              NULL        559,697.56           2005        559,697.56  
287              NULL        519,905.93           2006        1,079,603.50  
285              NULL        172,524.45           2007        1,252,127.95  
283              1           1,573,012.94         2005        1,573,012.94  
280              1           1,352,577.13         2005        2,925,590.07  
284              1           1,576,562.20         2006        4,502,152.27  
275              2           3,763,178.18         2005        3,763,178.18  
277              3           3,189,418.37         2005        3,189,418.37  
276              4           4,251,368.55         2005        4,251,368.55  
281              4           2,458,535.62         2005        6,709,904.17  
  
```  
  
## <a name="examples-sspdw"></a>Esempi: [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-the-over-clause-with-the-row_number-function"></a>E. Utilizzo della clausola OVER con la funzione ROW_NUMBER  
 L'esempio seguente restituisce ROW_NUMBER per i venditori in base alle rispettive quote di vendite assegnata.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ROW_NUMBER() OVER(ORDER BY SUM(SalesAmountQuota) DESC) AS RowNumber,  
    FirstName, LastName,   
CONVERT(VARCHAR(13), SUM(SalesAmountQuota),1) AS SalesQuota   
FROM dbo.DimEmployee AS e  
INNER JOIN dbo.FactSalesQuota AS sq  
    ON e.EmployeeKey = sq.EmployeeKey  
WHERE e.SalesPersonFlag = 1  
GROUP BY LastName, FirstName;  
```  
  
 Set di risultati parziale:  
  
 ```
 RowNumber  FirstName  LastName            SalesQuota  
 ---------  ---------  ------------------  -------------  
 1          Jillian    Carson              12,198,000.00  
 2          Linda      Mitchell            11,786,000.00  
 3          Michael    Blythe              11,162,000.00  
 4          Jae        Pak                 10,514,000.00  
 ```
 
### <a name="f-using-the-over-clause-with-aggregate-functions"></a>F. Utilizzo della clausola OVER con funzioni di aggregazione  
 Nell'esempio seguente viene illustrato l'uso della clausola OVER con funzioni di aggregazione. Nell'esempio l'uso della clausola OVER è più efficace rispetto all'uso di sottoquery.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT SalesOrderNumber AS OrderNumber, ProductKey,   
       OrderQuantity AS Qty,   
       SUM(OrderQuantity) OVER(PARTITION BY SalesOrderNumber) AS Total,  
       AVG(OrderQuantity) OVER(PARTITION BY SalesOrderNumber) AS Avg,  
       COUNT(OrderQuantity) OVER(PARTITION BY SalesOrderNumber) AS Count,  
       MIN(OrderQuantity) OVER(PARTITION BY SalesOrderNumber) AS Min,  
       MAX(OrderQuantity) OVER(PARTITION BY SalesOrderNumber) AS Max  
FROM dbo.FactResellerSales   
WHERE SalesOrderNumber IN(N'SO43659',N'SO43664') AND  
      ProductKey LIKE '2%'  
ORDER BY SalesOrderNumber,ProductKey;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 OrderNumber  Product  Qty  Total  Avg  Count  Min  Max  
 -----------  -------  ---  -----  ---  -----  ---  ---  
 SO43659      218      6    16     3    5      1    6  
 SO43659      220      4    16     3    5      1    6  
 SO43659      223      2    16     3    5      1    6  
 SO43659      229      3    16     3    5      1    6  
 SO43659      235      1    16     3    5      1    6  
 SO43664      229      1     2     1    2      1    1  
 SO43664      235      1     2     1    2      1    1  
 ```
 
 Nell'esempio seguente viene illustrato l'uso della clausola OVER con una funzione di aggregazione in un valore calcolato. Si noti che le aggregazioni vengono calcolate in base a `SalesOrderNumber`, mentre la percentuale dell'ordine di vendita totale viene calcolata per ogni riga di ogni `SalesOrderNumber`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT SalesOrderNumber AS OrderNumber, ProductKey AS Product,   
       OrderQuantity AS Qty,   
       SUM(OrderQuantity) OVER(PARTITION BY SalesOrderNumber) AS Total,  
       CAST(1. * OrderQuantity / SUM(OrderQuantity)   
        OVER(PARTITION BY SalesOrderNumber)   
            *100 AS DECIMAL(5,2)) AS PctByProduct  
FROM dbo.FactResellerSales   
WHERE SalesOrderNumber IN(N'SO43659',N'SO43664') AND  
      ProductKey LIKE '2%'  
ORDER BY SalesOrderNumber,ProductKey;  
```  
  
 La prima parte di questo set di risultati è:  
  
 ```
 OrderNumber  Product  Qty  Total  PctByProduct  
 -----------  -------  ---  -----  ------------  
 SO43659      218      6    16     37.50  
 SO43659      220      4    16     25.00  
 SO43659      223      2    16     12.50  
 SO43659      229      2    16     18.75  
 ```
 
## <a name="see-also"></a>Vedere anche  
 [Funzioni di aggregazione &#40;Transact-SQL&#41;](../../t-sql/functions/aggregate-functions-transact-sql.md)   
 [Funzioni analitiche &#40;Transact-SQL&#41;](../../t-sql/functions/analytic-functions-transact-sql.md)   
 [Post di blog sulle funzioni finestra e OVER, in sqlmag.com, di Itzik Ben-Gan](https://www.itprotoday.com/sql-server/how-use-microsoft-sql-server-2012s-window-functions-part-1)  
  
  
