---
description: STDEVP (Transact-SQL)
title: STDEVP (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STDEVP
- STDEVP_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STDEVP function [Transact-SQL]
- expressions [SQL Server], statistical standard deviation
- statistical standard deviation
ms.assetid: 29f2a906-d084-4464-abc3-4b275ed19442
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 72e175dfab472a27269ace46feaa42cd182af3cd
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753731"
---
# <a name="stdevp-transact-sql"></a>STDEVP (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce la deviazione statistica standard relativa al popolamento di tutti i valori dell'espressione specificata.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Aggregate Function Syntax   
STDEVP ( [ ALL | DISTINCT ] expression )  
  
-- Analytic Function Syntax   
STDEVP ([ ALL ] expression) OVER ( [ partition_by_clause ] order_by_clause)  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 **ALL**  
 Applica la funzione a tutti i valori. Il valore predefinito è ALL.  
  
 DISTINCT  
 Consente di considerare ogni valore univoco.  
  
 *expression*  
 [Espressione](../../t-sql/language-elements/expressions-transact-sql.md) numerica. Non è possibile utilizzare funzioni di aggregazione e sottoquery. *expression* è un'espressione della categoria dei tipi di dati numerici esatti o numerici approssimativi, ad eccezione del tipo di dati **bit**.  
  
 OVER **(** [ _partition\_by\_clause_ ] _order\_by\_clause_ **)**  
 *partition_by_clause* suddivide il set di risultati generato dalla clausola FROM in partizioni alle quali viene applicata la funzione. Se non specificato, la funzione tratta tutte le righe del set di risultati della query come un unico gruppo. *order_by_clause* determina l'ordine logico in cui viene eseguita l'operazione. *order_by_clause* è obbligatorio. Per altre informazioni, vedere [Clausola OVER - &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
## <a name="return-types"></a>Tipi restituiti  
 **float**  
  
## <a name="remarks"></a>Commenti  
 Se la funzione STDEVP viene utilizzata per tutti gli elementi di un'istruzione SELECT, ogni valore del set di risultati viene incluso nel calcolo. STDEVP può essere utilizzata solo con colonne numeriche. I valori Null vengono ignorati.  
  
 STDEVP è una funzione deterministica quando viene utilizzata senza le clausole ORDER BY e OVER. Non è deterministica quando viene specificata con le clausole ORDER BY e OVER. Per altre informazioni, vedere [Funzioni deterministiche e non deterministiche](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-stdevp"></a>A: uso di STDEVP  
 Nell'esempio seguente viene restituita la deviazione standard per il popolamento per tutti i valori dei premi di produttività nella tabella `SalesPerson` del database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
SELECT STDEVP(Bonus)  
FROM Sales.SalesPerson;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="b-using-stdevp"></a>B: uso di STDEVP  
 Nell'esempio seguente viene restituita la `STDEVP` dei valori quota di vendite nella tabella `dbo.FactSalesQuota`. La prima colonna contiene la deviazione standard di tutti i valori distinct e la seconda colonna contiene la deviazione standard di tutti i valori, compresi eventuali valori duplicati.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT STDEVP(DISTINCT SalesAmountQuota)AS Distinct_Values, STDEVP(SalesAmountQuota) AS All_Values  
FROM dbo.FactSalesQuota;SELECT STDEVP(DISTINCT Quantity)AS Distinct_Values, STDEVP(Quantity) AS All_Values  
FROM ProductInventory;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
Distinct_Values   All_Values  
----------------  ----------------  
397676.79         397226.44
```  
  
### <a name="c-using-stdevp-with-over"></a>C. Uso di STDEVP con OVER  
 L'esempio seguente restituisce l'elemento `STDEVP` dei valori degli obiettivi di vendita per ogni trimestre in un anno di calendario. Si noti che `ORDER BY` nella clausola `OVER` ordina `STDEVP` e che `ORDER BY` dell'istruzione `SELECT` ordina il set di risultati.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT CalendarYear AS Year, CalendarQuarter AS Quarter, SalesAmountQuota AS SalesQuota,  
       STDEVP(SalesAmountQuota) OVER (ORDER BY CalendarYear, CalendarQuarter) AS StdDeviation  
FROM dbo.FactSalesQuota  
WHERE EmployeeKey = 272 AND CalendarYear = 2002  
ORDER BY CalendarQuarter;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
Year  Quarter  SalesQuota              StdDeviation  
----  -------  ----------------------  -------------------  
2002  1         91000.0000             0.00  
2002  2        140000.0000             24500.00  
2002  3         70000.0000             29329.55  
2002  4        154000.0000             34426.55
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni di aggregazione &#40;Transact-SQL&#41;](../../t-sql/functions/aggregate-functions-transact-sql.md)   
 [Clausola OVER &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md)  
  
  

