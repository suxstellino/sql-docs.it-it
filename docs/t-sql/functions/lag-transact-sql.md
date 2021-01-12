---
description: LAG (Transact-SQL)
title: LAG (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- LAG_TSQL
- LAG
dev_langs:
- TSQL
helpviewer_keywords:
- LAG function
- analytic functions, LAG
ms.assetid: a9a90bdb-3f80-4c97-baca-b7407bcdc7f0
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1390066fbbef2cf772c8410e9ae1b237e6ad5491
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98087009"
---
# <a name="lag-transact-sql"></a>LAG (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Accede ai dati da una riga precedente nello stesso set di risultati senza l'utilizzo di un self-join che inizia con [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]. LAG fornisce l'accesso a una riga situata a una distanza fisica specificata e precedente rispetto alla riga corrente. Utilizzare questa funzione analitica in un'istruzione SELECT per confrontare valori nella riga corrente con i valori in una riga precedente.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
LAG (scalar_expression [,offset] [,default])  
    OVER ( [ partition_by_clause ] order_by_clause )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *scalar_expression*  
 Valore da restituire basato sull'offset specificato. Tale valore può essere un'espressione di ogni tipo che restituisce un singolo valore scalare. *scalar_expression* non può essere una funzione analitica.  
  
 *offset*  
 Numero di righe in posizioni precedenti rispetto alla riga corrente dalla quale ottenere un valore. Se non specificato, il valore predefinito è 1. *offset* può essere una colonna, una sottoquery o un'altra espressione che restituisce un valore intero positivo o che può essere convertita in modo implicito in un tipo di dati **bigint**. *offset* non può essere un valore negativo o una funzione analitica.  
  
 *default*  
 Il valore da restituire quando *offset* non rientra nell'ambito della partizione. Se non viene specificato un valore predefinito, viene restituito NULL. *default* può essere una colonna, una sottoquery o un'altra espressione, ma non può essere una funzione analitica. *default* deve essere compatibile a livello di tipo con *scalar_expression*.  
  
 OVER **(** [ _partition\_by\_clause_ ] _order\_by\_clause_ **)**  
 *partition_by_clause* suddivide il set di risultati generato dalla clausola FROM in partizioni alle quali viene applicata la funzione. Se non specificato, la funzione tratta tutte le righe del set di risultati della query come un unico gruppo. *order_by_clause* determina l'ordine dei dati prima che venga applicata la funzione. Se specificato, *partition_by_clause* determina l'ordine dei dati nella partizione. *order_by_clause* è obbligatorio. Per altre informazioni, vedere [Clausola OVER - &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
## <a name="return-types"></a>Tipi restituiti  
 Tipo dei dati dell'elemento *scalar_expression* specificato. Se *scalar_expression* ammette valori Null o se *default* è impostato su NULL, viene restituito NULL.  
  
## <a name="general-remarks"></a>Osservazioni generali  
 LAG è non deterministico. Per altre informazioni, vedere [Funzioni deterministiche e non deterministiche](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-compare-values-between-years"></a>R. Confronto di valori tra anni  
 Nell'esempio seguente viene utilizzata la funzione LAG per restituire la differenza nelle quote vendite per un dipendente specifico negli anni precedenti. Si noti che poiché non è presente alcun valore di ritardo disponibile per la prima riga, viene restituita l'impostazione predefinita zero (0).  
  
```sql   
USE AdventureWorks2012;  
GO  
SELECT BusinessEntityID, YEAR(QuotaDate) AS SalesYear, SalesQuota AS CurrentQuota,   
       LAG(SalesQuota, 1,0) OVER (ORDER BY YEAR(QuotaDate)) AS PreviousQuota  
FROM Sales.SalesPersonQuotaHistory  
WHERE BusinessEntityID = 275 AND YEAR(QuotaDate) IN ('2005','2006');  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```    
BusinessEntityID SalesYear   CurrentQuota          PreviousQuota  
---------------- ----------- --------------------- ---------------------  
275              2005        367000.00             0.00  
275              2005        556000.00             367000.00  
275              2006        502000.00             556000.00  
275              2006        550000.00             502000.00  
275              2006        1429000.00            550000.00  
275              2006        1324000.00            1429000.00  
  
```  
  
### <a name="b-compare-values-within-partitions"></a>B. Confronto di valori all'interno di partizioni  
 Nell'esempio seguente viene utilizzata la funzione LAG per confrontare le vendite dei dipendenti a partire dall'inizio dell'anno. La clausola PARTITION BY è specificata per dividere le righe nel set di risultati in base al territorio di vendita. La funzione LAG viene applicata a ogni singola partizione e il calcolo viene riavviato per ogni partizione. La clausola ORDER BY specificata nella clausola OVER ordina le righe in ogni partizione. La clausola ORDER BY nell'istruzione SELECT ordina le righe nell'intero set di risultati. Si noti che poiché non è presente alcun valore di ritardo per la prima riga di ogni partizione, viene restituita l'impostazione predefinita zero (0).  
  
```sql   
USE AdventureWorks2012;  
GO  
SELECT TerritoryName, BusinessEntityID, SalesYTD,   
       LAG (SalesYTD, 1, 0) OVER (PARTITION BY TerritoryName ORDER BY SalesYTD DESC) AS PrevRepSales  
FROM Sales.vSalesPerson  
WHERE TerritoryName IN (N'Northwest', N'Canada')   
ORDER BY TerritoryName;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```    
TerritoryName            BusinessEntityID SalesYTD              PrevRepSales  
-----------------------  ---------------- --------------------- ---------------------  
Canada                   282              2604540.7172          0.00  
Canada                   278              1453719.4653          2604540.7172  
Northwest                284              1576562.1966          0.00  
Northwest                283              1573012.9383          1576562.1966  
Northwest                280              1352577.1325          1573012.9383  
  
```  
  
### <a name="c-specifying-arbitrary-expressions"></a>C. Specifica di espressioni arbitrarie  
 Nell'esempio seguente viene illustrata la specifica di una varietà di espressioni arbitrarie nella sintassi della funzione LAG.  
  
```sql   
CREATE TABLE T (a INT, b INT, c INT);   
GO  
INSERT INTO T VALUES (1, 1, -3), (2, 2, 4), (3, 1, NULL), (4, 3, 1), (5, 2, NULL), (6, 1, 5);   
  
SELECT b, c,   
    LAG(2*c, b*(SELECT MIN(b) FROM T), -c/2.0) OVER (ORDER BY a) AS i  
FROM T;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
b           c           i  
----------- ----------- -----------  
1           -3          1  
2           4           -2  
1           NULL        8  
3           1           -6  
2           NULL        NULL  
1           5           NULL  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-compare-values-between-quarters"></a>D. Confronto di valori fra trimestri  
 L'esempio seguente illustra l'utilizzo della funzione LAG. La query usa la funzione LAG per restituire la differenza nelle quote vendite per un dipendente specifico nei trimestri del calendario precedenti. Si noti che poiché non è presente alcun valore di ritardo disponibile per la prima riga, viene restituita l'impostazione predefinita zero (0).  
  
```sql   
-- Uses AdventureWorks  
  
SELECT CalendarYear, CalendarQuarter, SalesAmountQuota AS SalesQuota,  
       LAG(SalesAmountQuota,1,0) OVER (ORDER BY CalendarYear, CalendarQuarter) AS PrevQuota,  
       SalesAmountQuota - LAG(SalesAmountQuota,1,0) OVER (ORDER BY CalendarYear, CalendarQuarter) AS Diff  
FROM dbo.FactSalesQuota  
WHERE EmployeeKey = 272 AND CalendarYear IN (2001, 2002)  
ORDER BY CalendarYear, CalendarQuarter;   
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
Year Quarter  SalesQuota  PrevQuota  Diff  
---- -------  ----------  ---------  -------------  
2001 3        28000.0000      0.0000   28000.0000  
2001 4         7000.0000  28000.0000  -21000.0000  
2001 1        91000.0000   7000.0000   84000.0000  
2002 2       140000.0000  91000.0000   49000.0000  
2002 3         7000.0000 140000.0000  -70000.0000  
2002 4       154000.0000   7000.0000   84000.0000
```  
  
## <a name="see-also"></a>Vedere anche  
 [LEAD &#40;Transact-SQL&#41;](../../t-sql/functions/lead-transact-sql.md)  
  
  


