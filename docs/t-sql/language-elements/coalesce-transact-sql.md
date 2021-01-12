---
title: COALESCE (Transact-SQL) | Microsoft Docs
description: Informazioni di riferimento Transact-SQL per COALESCE, che restituisce il valore della prima espressione che non restituisce NULL.
ms.date: 08/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- COALESCE
- COALESCE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- expressions [SQL Server], nonnull
- COALESCE function
- first nonnull expressions [SQL Server]
- nonnull expressions
ms.assetid: fafc0dba-f8a8-4aad-9b7f-908e34b74d88
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 041f5b1b87a5d6ee92cd67fe17c6fd45f682dd95
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093591"
---
# <a name="coalesce-transact-sql"></a>COALESCE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Valuta gli argomenti seguendo l'ordine e restituisce il valore corrente della prima espressione che inizialmente non restituisce `NULL`. Ad esempio, `SELECT COALESCE(NULL, NULL, 'third_value', 'fourth_value');` restituisce il terzo valore perché il terzo valore è il primo non Null. 
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
COALESCE ( expression [ ,...n ] )   
```  
  
## <a name="arguments"></a>Argomenti  
_expression_  
[Espressione](../../t-sql/language-elements/expressions-transact-sql.md) di qualsiasi tipo.  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
Restituisce il tipo di dati dell'_espressione_ con la precedenza del tipo di dati più alta. Se tutte le espressioni non ammettono valori Null, il risultato non ammetterà valori Null.  
  
## <a name="remarks"></a>Osservazioni  
Quando tutti gli argomenti sono `NULL`, `COALESCE`restituisce `NULL`. Almeno uno dei valori Null deve essere un valore `NULL` tipizzato.  
  
## <a name="comparing-coalesce-and-case"></a>Confronto tra COALESCE e CASE  
L'espressione `COALESCE` è una scorciatoia sintattica dell'espressione `CASE`.  Il codice `COALESCE`(_expression1_, _...n_) viene quindi riscritto da Query Optimizer come la seguente espressione `CASE`:  
  
```sql  
CASE  
WHEN (expression1 IS NOT NULL) THEN expression1  
WHEN (expression2 IS NOT NULL) THEN expression2  
...  
ELSE expressionN  
END  
```  
  
In questo modo, i valori di input (_expression1_, _expression2_, _expressionN_ e così via) vengono valutati più volte. Un'espressione valore contenente una sottoquery viene considerata non deterministica e la sottoquery viene valutata due volte. Questo risultato è conforme allo standard SQL. In entrambi i casi, tra la prima valutazione e le successive possono essere restituiti risultati diversi.  
  
Ad esempio, quando viene eseguito il codice `COALESCE((subquery), 1)`, la sottoquery viene valutata due volte. Di conseguenza, è possibile che si ottengano risultati differenti a seconda del livello di isolamento della query. Ad esempio, il codice può restituire `NULL` con il livello di isolamento `READ COMMITTED` in un ambiente multiutente. Per assicurare risultati costanti, utilizzare il livello di isolamento `SNAPSHOT ISOLATION` oppure sostituire `COALESCE` con la funzione `ISNULL`. In alternativa, è possibile riscrivere la query in modo da inserire la sottoquery in una sub-SELECT, come illustrato nell'esempio seguente:  
  
```sql  
SELECT CASE WHEN x IS NOT NULL THEN x ELSE 1 END  
FROM  
(  
SELECT (SELECT Nullable FROM Demo WHERE SomeCol = 1) AS x  
) AS T;  
  
```  
  
## <a name="comparing-coalesce-and-isnull"></a>Confronto tra COALESCE e ISNULL  
Le finalità della funzione `ISNULL` e dell'espressione `COALESCE` sono simili, ma i comportamenti differiscono.  
  
1.  Dato che `ISNULL` è una funzione, la valutazione viene eseguita una sola volta.  Come descritto in precedenza, i valori di input per l'espressione `COALESCE` possono essere valutati più volte.  
  
2.  La determinazione dei tipi di dati dell'espressione risultante è differente. `ISNULL` utilizza il tipo di dati del primo parametro, `COALESCE` segue le regole dell'espressione `CASE` e restituisce il tipo di dati del valore con la precedenza più alta.  
  
3.  Il supporto dei valori NULL dell'espressione risultante è differente per `ISNULL` e `COALESCE`. Il valore restituito da `ISNULL` viene sempre considerato come non nullable, supponendo che il valore restituito non ammetta valori Null. Al contrario, l'espressione `COALESCE` con parametri non Null viene considerata `NULL`. Nonostante siano uguali, le espressioni `ISNULL(NULL, 1)` e `COALESCE(NULL, 1)` hanno quindi valori diversi in termini di supporto dei valori Null. Questi valori fanno la differenza se si usano queste espressioni in colonne calcolate, creando vincoli di chiave o rendendo deterministico il valore restituito di una funzione definita dall'utente scalare in modo che possa essere indicizzato, come illustrato nell'esempio seguente:  
  
    ```sql  
    USE tempdb;  
    GO  
    -- This statement fails because the PRIMARY KEY cannot accept NULL values  
    -- and the nullability of the COALESCE expression for col2   
    -- evaluates to NULL.  
    CREATE TABLE #Demo   
    (   
      col1 INTEGER NULL,   
      col2 AS COALESCE(col1, 0) PRIMARY KEY,   
      col3 AS ISNULL(col1, 0)   
    );   
  
    -- This statement succeeds because the nullability of the   
    -- ISNULL function evaluates AS NOT NULL.  
  
    CREATE TABLE #Demo   
    (   
      col1 INTEGER NULL,   
      col2 AS COALESCE(col1, 0),   
      col3 AS ISNULL(col1, 0) PRIMARY KEY   
    );  
    ```  
  
4.  Anche le convalide per `ISNULL` e `COALESCE` sono diverse. Ad esempio, un valore `NULL` per `ISNULL` viene convertito in **int**, mentre per `COALESCE` è necessario specificare un tipo di dati.  
  
5.  `ISNULL` accetta solo due parametri. `COALESCE` accetta invece un numero variabile di parametri.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-running-a-simple-example"></a>R. Esecuzione di un esempio semplice  
Nell'esempio seguente viene illustrato il modo in cui `COALESCE` seleziona i dati dalla prima colonna in cui è presente un valore non Null. In questo esempio viene utilizzato il database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
SELECT Name, Class, Color, ProductNumber,  
COALESCE(Class, Color, ProductNumber) AS FirstNotNull  
FROM Production.Product;  
```  
  
### <a name="b-running-a-complex-example"></a>B. Esecuzione di un esempio complesso  
Nell'esempio seguente viene illustrata una tabella `wages` che include tre colonne con informazioni sulla retribuzione annua dei dipendenti, ovvero retribuzione oraria, stipendio e commissione. Un dipendente tuttavia riceve un solo tipo di paga. Per determinare l'importo totale pagato a tutti i dipendenti, utilizzare la funzione `COALESCE` per ottenere solo i valori non Null delle colonne `hourly_wage`, `salary` e `commission`.  
  
```sql  
SET NOCOUNT ON;  
GO  
USE tempdb;  
IF OBJECT_ID('dbo.wages') IS NOT NULL  
    DROP TABLE wages;  
GO  
CREATE TABLE dbo.wages  
(  
    emp_id        TINYINT   IDENTITY,  
    hourly_wage   DECIMAL   NULL,  
    salary        DECIMAL   NULL,  
    commission    DECIMAL   NULL,  
    num_sales     TINYINT   NULL  
);  
GO  
INSERT dbo.wages (hourly_wage, salary, commission, num_sales)  
VALUES  
    (10.00, NULL, NULL, NULL),  
    (20.00, NULL, NULL, NULL),  
    (30.00, NULL, NULL, NULL),  
    (40.00, NULL, NULL, NULL),  
    (NULL, 10000.00, NULL, NULL),  
    (NULL, 20000.00, NULL, NULL),  
    (NULL, 30000.00, NULL, NULL),  
    (NULL, 40000.00, NULL, NULL),  
    (NULL, NULL, 15000, 3),  
    (NULL, NULL, 25000, 2),  
    (NULL, NULL, 20000, 6),  
    (NULL, NULL, 14000, 4);  
GO  
SET NOCOUNT OFF;  
GO  
SELECT CAST(COALESCE(hourly_wage * 40 * 52,   
   salary,   
   commission * num_sales) AS money) AS 'Total Salary'   
FROM dbo.wages  
ORDER BY 'Total Salary';  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```   
Total Salary  
------------  
10000.00  
20000.00  
20800.00  
30000.00  
40000.00  
41600.00  
45000.00  
50000.00  
56000.00  
62400.00  
83200.00  
120000.00  
  
(12 row(s) affected)
```  
  
### <a name="c-simple-example"></a>C: Esempio semplice  
Nell'esempio seguente viene illustrato come `COALESCE` seleziona i dati dalla prima colonna in cui è presente un valore non Null. Si supponga per questo esempio che la tabella `Products` contenga i dati seguenti:  
  
```  
Name         Color      ProductNumber  
------------ ---------- -------------  
Socks, Mens  NULL       PN1278  
Socks, Mens  Blue       PN1965  
NULL         White      PN9876
```  
 
è quindi possibile eseguire la seguente query COALESCE:  
  
```sql  
SELECT Name, Color, ProductNumber, COALESCE(Color, ProductNumber) AS FirstNotNull   
FROM Products ;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Name         Color      ProductNumber  FirstNotNull  
------------ ---------- -------------  ------------  
Socks, Mens  NULL       PN1278         PN1278  
Socks, Mens  Blue       PN1965         Blue  
NULL         White      PN9876         White
```  
  
Si noti che nella prima riga, il valore `FirstNotNull` è `PN1278`, non `Socks, Mens`. Questo valore è determinato dal fatto che la colonna `Name`non è stata specificata come parametro per `COALESCE` nell'esempio.  
  
### <a name="d-complex-example"></a>D: Esempio complesso  
L'esempio seguente usa `COALESCE` per confrontare i valori in tre colonne e restituire solo il valore non Null trovato nelle colonne.  
  
```sql  
CREATE TABLE dbo.wages  
(  
    emp_id        TINYINT   NULL,  
    hourly_wage   DECIMAL   NULL,  
    salary        DECIMAL   NULL,  
    commission    DECIMAL   NULL,  
    num_sales     TINYINT   NULL  
);  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (1, 10.00, NULL, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (2, 20.00, NULL, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (3, 30.00, NULL, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (4, 40.00, NULL, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (5, NULL, 10000.00, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (6, NULL, 20000.00, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (7, NULL, 30000.00, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (8, NULL, 40000.00, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (9, NULL, NULL, 15000, 3);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (10,NULL, NULL, 25000, 2);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (11, NULL, NULL, 20000, 6);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (12, NULL, NULL, 14000, 4);  
  
SELECT CAST(COALESCE(hourly_wage * 40 * 52,   
   salary,   
   commission * num_sales) AS DECIMAL(10,2)) AS TotalSalary   
FROM dbo.wages  
ORDER BY TotalSalary;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
Total Salary  
------------  
10000.00  
20000.00  
20800.00  
30000.00  
40000.00  
41600.00  
45000.00  
50000.00  
56000.00  
62400.00  
83200.00  
120000.00
```  
  
## <a name="see-also"></a>Vedere anche  
[ISNULL &#40;Transact-SQL&#41;](../../t-sql/functions/isnull-transact-sql.md)   
[CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)  
  
  
