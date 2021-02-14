---
description: SOME | ANY (Transact-SQL)
title: SOME | ANY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SOME
- SOME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- scalar values
- comparing scalar with single-column set
- ANY operator
- SOME | ANY keyword
- single-column set of values [SQL Server]
ms.assetid: 1f717ad6-f67b-4980-9397-577ecb0e5789
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5f25709500f370b6083584b1321937b0484310bf
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342585"
---
# <a name="some--any-transact-sql"></a>SOME | ANY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Confronta un valore scalare con un set di valori a colonna singola. SOME e ANY sono equivalenti.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
scalar_expression { = | < > | ! = | > | > = | ! > | < | < = | ! < }   
     { SOME | ANY } ( subquery )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *scalar_expression*  
 Qualsiasi [espressione](../../t-sql/language-elements/expressions-transact-sql.md) valida.  
  
 { = \| <> \| != \| > \| >= \| !> \| < \| <= \| !< }  
 Qualsiasi operatore di confronto valido.  
  
 SOME | ANY  
 Specifica che deve essere eseguito un confronto.  
  
 *subquery*  
 Sottoquery che include un set di risultati di una colonna. Il tipo di dati della colonna restituita deve corrispondere a quello di *scalar_expression*.  
  
## <a name="result-types"></a>Tipi restituiti  
 **Boolean**  
  
## <a name="result-value"></a>Valore restituito  
 SOME o ANY restituisce **TRUE** se il confronto specificato è TRUE per qualsiasi coppia (_scalar_expression_ **,** _x_) dove *x* è un valore del set di valori a colonna singola. In caso contrario restituisce **FALSE**.  
  
## <a name="remarks"></a>Osservazioni  
 SOME specifica che l'argomento *scalar_expression* deve essere confrontato in modo univoco con almeno un valore restituito dalla sottoquery. Per le istruzioni che specificano che l'argomento *scalar_expression* deve essere confrontato in modo univoco con ogni valore restituito dalla sottoquery, vedere [ALL &#40;Transact-SQL&#41;](../../t-sql/language-elements/all-transact-sql.md). Se ad esempio la sottoquery restituisce i valori 2 e 3, *scalar_expression* = SOME (subquery) restituisce TRUE se il valore di *scalar_expression* è 2. Se la sottoquery restituisce i valori 2 e 3, *scalar_expression* = ALL (subquery) restituisce FALSE, perché alcuni valori della sottoquery (il valore 3) non soddisfano i criteri dell'espressione.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-running-a-simple-example"></a>R. Esecuzione di un esempio semplice  
 Le istruzioni seguenti consentono di creare una tabella semplice e di aggiungere i valori di `1`, `2`, `3` e `4` alla colonna `ID`.  
  
```sql  
CREATE TABLE T1  
(ID INT) ;  
GO  
INSERT T1 VALUES (1) ;  
INSERT T1 VALUES (2) ;  
INSERT T1 VALUES (3) ;  
INSERT T1 VALUES (4) ;  
```  
  
 Nella query seguente viene restituito `TRUE` perché `3` è inferiore ad alcuni valori della tabella.  
  
```sql  
IF 3 < SOME (SELECT ID FROM T1)  
PRINT 'TRUE'   
ELSE  
PRINT 'FALSE' ;  
```  
  
 La query seguente restituisce `FALSE` perché `3` non è inferiore a tutti i valori nella tabella.  
  
```sql  
IF 3 < ALL (SELECT ID FROM T1)  
PRINT 'TRUE'   
ELSE  
PRINT 'FALSE' ;  
```  
  
### <a name="b-running-a-practical-example"></a>B. Esecuzione di un esempio pratico  
 Nell'esempio seguente viene creata una stored procedure che determina se tutti i componenti di una colonna `SalesOrderID` specificata nel database `AdventureWorks2012` possono essere prodotti nel numero di giorni indicato. Nell'esempio viene utilizzata una sottoquery per creare un elenco contenente i numeri corrispondenti ai valori di `DaysToManufacture` per tutti i componenti della colonna `SalesOrderID` specifica e viene quindi verificato se alcuni dei valori restituiti dalla sottoquery sono maggiori del numero di giorni specificato. Se ogni valore di `DaysToManufacture` restituito è minore del numero specificato, la condizione è TRUE e viene stampato il primo messaggio.  
  
```sql  
-- Uses AdventureWorks  
  
CREATE PROCEDURE ManyDaysToComplete @OrderID INT, @NumberOfDays INT  
AS  
IF   
@NumberOfDays < SOME  
   (  
    SELECT DaysToManufacture  
    FROM Sales.SalesOrderDetail  
    JOIN Production.Product   
    ON Sales.SalesOrderDetail.ProductID = Production.Product.ProductID   
    WHERE SalesOrderID = @OrderID  
   )  
PRINT 'At least one item for this order can''t be manufactured in specified number of days.'
ELSE   
PRINT 'All items for this order can be manufactured in the specified number of days or less.' ;  
  
```  
  
 Per testare la procedura, eseguirla usando il valore `SalesOrderID``49080`, a cui sono associati un componente che richiede `2` giorni e due componenti che richiedono 0 giorni. La prima istruzione soddisfa i criteri, la seconda query no.  
  
```sql  
EXECUTE ManyDaysToComplete 49080, 2 ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `All items for this order can be manufactured in the specified number of days or less.`  
  
```  
EXECUTE ManyDaysToComplete 49080, 1 ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `At least one item for this order can't be manufactured in specified number of days.`  
  
## <a name="see-also"></a>Vedere anche  
 [ALL &#40;Transact-SQL&#41;](../../t-sql/language-elements/all-transact-sql.md)   
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)   
 [Funzioni predefinite &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [Operatori &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)   
 [IN &#40;Transact-SQL&#41;](../../t-sql/language-elements/in-transact-sql.md)  
  
