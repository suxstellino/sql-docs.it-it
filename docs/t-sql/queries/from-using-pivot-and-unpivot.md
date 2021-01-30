---
title: Uso di PIVOT e UNPIVOT | Microsoft Docs
description: Informazioni di riferimento Transact-SQL per gli operatori relazionali PIVOT e UNPIVOT. Usare questi operatori nelle istruzioni SELECT per modificare un'espressione con valori di tabella in un'altra tabella.
ms.date: 10/14/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- PIVOT_TSQL
helpviewer_keywords:
- FROM clause, UNPIVOT operator
- unpivoting tables
- table pivoting [SQL Server]
- UNPIVOT operator
- crosstab query
- PIVOT operator
- rotating table-valued expressions
- pivoting tables
- FROM clause, PIVOT operator
- rotating columns
ms.assetid: 24ba54fc-98f7-4d35-8881-b5158aac1d66
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 21df23cf904c561d295553fb5fb79ebe2cd7702a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207742"
---
# <a name="from---using-pivot-and-unpivot"></a>FROM - Uso di PIVOT e UNPIVOT

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

È possibile usare gli operatori relazionali `PIVOT` e `UNPIVOT` per modificare un'espressione con valori di tabella in un'altra tabella. `PIVOT` ruota un'espressione con valori di tabella convertendo i valori univoci di una colonna nell'espressione in più colonne nell'output. E `PIVOT` esegue le aggregazioni richieste sugli eventuali valori di colonna restanti da includere nell'output finale. `UNPIVOT` esegue l'operazione opposta rispetto a PIVOT, ruotando le colonne di un'espressione con valori di tabella in valori di colonna.  
  
La sintassi di `PIVOT` è più semplice e leggibile di quella che sarebbe altrimenti possibile specificare in una serie complessa di istruzioni `SELECT...CASE`. Per una descrizione completa della sintassi per `PIVOT`, vedere [FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md).  
  
## <a name="syntax"></a>Sintassi  
La sintassi seguente offre un riepilogo dell'uso dell'operatore `PIVOT`.  
  
```syntaxsql  
SELECT <non-pivoted column>,  
    [first pivoted column] AS <column name>,  
    [second pivoted column] AS <column name>,  
    ...  
    [last pivoted column] AS <column name>  
FROM  
    (<SELECT query that produces the data>)   
    AS <alias for the source query>  
PIVOT  
(  
    <aggregation function>(<column being aggregated>)  
FOR   
[<column that contains the values that will become column headers>]   
    IN ( [first pivoted column], [second pivoted column],  
    ... [last pivoted column])  
) AS <alias for the pivot table>  
<optional ORDER BY clause>;  
```  

## <a name="remarks"></a>Osservazioni  
Gli identificatori di colonna nella clausola `UNPIVOT` seguono le regole di confronto dei cataloghi. Per il [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], le regole di confronto sono sempre `SQL_Latin1_General_CP1_CI_AS`. Per i database parzialmente indipendenti di [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], le regole di confronto sono sempre `Latin1_General_100_CI_AS_KS_WS_SC`. Se la colonna è combinata con altre colonne, sarà necessaria una clausola COLLATE, ovvero `COLLATE DATABASE_DEFAULT`, per evitare conflitti.  

  
## <a name="basic-pivot-example"></a>Esempio di PIVOT di base  
Nell'esempio di codice seguente viene generata una tabella a due colonne che include quattro righe.  
  
```sql
USE AdventureWorks2014 ;  
GO  
SELECT DaysToManufacture, AVG(StandardCost) AS AverageCost   
FROM Production.Product  
GROUP BY DaysToManufacture;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
DaysToManufacture AverageCost
----------------- -----------
0                 5.0885
1                 223.88
2                 359.1082
4                 949.4105
```
  
Nessun prodotto viene definito con tre `DaysToManufacture`.  
  
Il codice seguente consente di visualizzare lo stesso risultato, trasformato tramite Pivot in modo che i valori di `DaysToManufacture` diventino le intestazioni di colonna. Una colonna è disponibile per tre `[3]` giorni, anche se i risultati sono `NULL`.  
  
```sql
-- Pivot table with one row and five columns  
SELECT 'AverageCost' AS Cost_Sorted_By_Production_Days,   
[0], [1], [2], [3], [4]  
FROM  
(SELECT DaysToManufacture, StandardCost   
    FROM Production.Product) AS SourceTable  
PIVOT  
(  
AVG(StandardCost)  
FOR DaysToManufacture IN ([0], [1], [2], [3], [4])  
) AS PivotTable;  
  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
Cost_Sorted_By_Production_Days 0           1           2           3           4         
------------------------------ ----------- ----------- ----------- ----------- -----------
AverageCost                    5.0885      223.88      359.1082    NULL        949.4105
```
  
## <a name="complex-pivot-example"></a>Esempio di PIVOT complesso  
Uno scenario comune in cui `PIVOT` può essere utile è il caso in cui si vuole generare report a tabulazione incrociata per fornire un riepilogo dei dati. Si supponga, ad esempio, di voler eseguire una query sulla tabella `PurchaseOrderHeader` nel database di esempio `AdventureWorks2014` per determinare il numero di ordini di acquisto effettuati da dipendenti specifici. La query seguente fornisce questo report, ordinato per fornitore.  
  
```sql
USE AdventureWorks2014;  
GO  
SELECT VendorID, [250] AS Emp1, [251] AS Emp2, [256] AS Emp3, [257] AS Emp4, [260] AS Emp5  
FROM   
(SELECT PurchaseOrderID, EmployeeID, VendorID  
FROM Purchasing.PurchaseOrderHeader) p  
PIVOT  
(  
COUNT (PurchaseOrderID)  
FOR EmployeeID IN  
( [250], [251], [256], [257], [260] )  
) AS pvt  
ORDER BY pvt.VendorID;  
```  
  
Set di risultati parziale:  
  
```
VendorID    Emp1        Emp2        Emp3        Emp4        Emp5  
----------- ----------- ----------- ----------- ----------- -----------
1492        2           5           4           4           4
1494        2           5           4           5           4
1496        2           4           4           5           5
1498        2           5           4           4           4
1500        3           4           4           5           4
```
  
I risultati restituiti dall'istruzione di selezione secondaria vengono trasformati tramite Pivot nella colonna `EmployeeID`.  
  
```sql
SELECT PurchaseOrderID, EmployeeID, VendorID  
FROM PurchaseOrderHeader;  
```  
  
I valori univoci restituiti dalla colonna `EmployeeID` diventano campi nel set di risultati finale. Pertanto è presente una colonna per ogni numero `EmployeeID` specificato nella clausola Pivot. In questo caso i dipendenti `250`, `251`, `256`, `257` e `260`. La colonna `PurchaseOrderID` funge da colonna dei valori, rispetto alla quale vengono raggruppate le colonne restituite nell'output finale, dette colonne di raggruppamento. In questo caso, le colonne di raggruppamento vengono aggregate dalla funzione `COUNT`. Si noti che viene visualizzato un messaggio di avviso che indica che eventuali valori Null visualizzati nella colonna `PurchaseOrderID` non sono stati considerati nel calcolo di `COUNT` per ogni dipendente.  
  
> [!IMPORTANT]  
>  Quando le funzioni di aggregazione sono usate con `PIVOT`, gli eventuali valori Null presenti nella colonna dei valori non vengono considerati nel calcolo di un'aggregazione.  

## <a name="unpivot-example"></a>Esempio di UNPIVOT
  
`UNPIVOT` esegue l'operazione quasi opposta rispetto a `PIVOT`, ruotando le colonne in righe. Si supponga che la tabella generata nell'esempio precedente venga archiviata nel database come `pvt` e che si desideri ruotare gli identificatori di colonna `Emp1`, `Emp2`, `Emp3`, `Emp4` e `Emp5` in valori di riga corrispondenti a un particolare fornitore. È quindi necessario identificare altre due colonne. La colonna che conterrà i valori di colonna da ruotare (`Emp1`, `Emp2` e così via) sarà denominata `Employee` e la colonna che conterrà i valori che attualmente si trovano nelle colonne ruotate sarà denominata `Orders`. Queste colonne corrispondono rispettivamente a *pivot_column* e *value_column* nella definizione [!INCLUDE[tsql](../../includes/tsql-md.md)]. La query è la seguente.  
  
```sql
-- Create the table and insert values as portrayed in the previous example.  
CREATE TABLE pvt (VendorID INT, Emp1 INT, Emp2 INT,  
    Emp3 INT, Emp4 INT, Emp5 INT);  
GO  
INSERT INTO pvt VALUES (1,4,3,5,4,4);  
INSERT INTO pvt VALUES (2,4,1,5,5,5);  
INSERT INTO pvt VALUES (3,4,3,5,4,4);  
INSERT INTO pvt VALUES (4,4,2,5,5,4);  
INSERT INTO pvt VALUES (5,5,1,5,5,5);  
GO  
-- Unpivot the table.  
SELECT VendorID, Employee, Orders  
FROM   
   (SELECT VendorID, Emp1, Emp2, Emp3, Emp4, Emp5  
   FROM pvt) p  
UNPIVOT  
   (Orders FOR Employee IN   
      (Emp1, Emp2, Emp3, Emp4, Emp5)  
)AS unpvt;  
GO  
```  
  
Set di risultati parziale:  
  
```
VendorID    Employee    Orders
----------- ----------- ------
1            Emp1       4
1            Emp2       3 
1            Emp3       5
1            Emp4       4
1            Emp5       4
2            Emp1       4
2            Emp2       1
2            Emp3       5
2            Emp4       5
2            Emp5       5
...
```
  
Si noti che `UNPIVOT` non è l'esatto opposto di `PIVOT`. `PIVOT` esegue un'aggregazione e unisce più righe in una singola riga nell'output. `UNPIVOT` non riproduce il risultato dell'espressione con valori di tabella originale perché le righe sono state unite. Inoltre, i valori Null nell'input di `UNPIVOT` non sono più presenti nell'output. Quando i valori non sono più presenti, ciò indica che è possibile che fossero presenti valori Null nell'input prima dell'operazione `PIVOT`.  
  
Nella vista `Sales.vSalesPersonSalesByFiscalYears` nel database di esempio [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] viene usato `PIVOT` per restituire le vendite totali per ogni venditore, per ogni anno fiscale. Per creare uno script per la vista in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], in **Esplora oggetti** individuare la vista nella cartella **Viste** per il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Fare clic con il pulsante destro del mouse sul nome della vista e quindi selezionare **Crea script per vista**.  
  
## <a name="see-also"></a>Vedere anche  
[FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md)   
[CASE (Transact-SQL)](../../t-sql/language-elements/case-transact-sql.md)  
  
