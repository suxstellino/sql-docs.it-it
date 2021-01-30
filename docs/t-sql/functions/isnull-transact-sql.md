---
description: ISNULL (Transact-SQL)
title: ISNULL (Transact-SQL)
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ISNULL
- ISNULL_TSQL
- IFNULL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- replacing null values
- null values [SQL Server], ISNULL
- null values [SQL Server], replacement values
- ISNULL function
ms.assetid: 6f3e5802-864b-4e77-9862-657bb5430b68
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e7579c4f0d5c310bf8837169948ced17ee1f64a6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186486"
---
# <a name="isnull-transact-sql"></a>ISNULL (Transact-SQL) 

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Sostituisce il valore NULL con il valore di sostituzione specificato.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
ISNULL ( check_expression , replacement_value )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *check_expression*  
 [Espressione](../../t-sql/language-elements/expressions-transact-sql.md) da verificare per determinare se è NULL. *check_expression* può essere di qualsiasi tipo.  
  
 *replacement_value*  
 Espressione da restituire se *check_expression* è NULL. *replacement_value* deve essere di un tipo convertibile in modo implicito nel tipo di *check_expression*.  
  
## <a name="return-types"></a>Tipi restituiti  
 Restituisce lo stesso tipo di *check_expression*. Se come *check_expression* viene specificato un valore letterale NULL, restituisce il tipo di dati di *replacement_value*. Se come *check_expression* viene specificato un valore letterale NULL e non viene specificato alcun *replacement_value*, restituisce un valore **int**.  
  
## <a name="remarks"></a>Osservazioni  
 Il valore di *check_expression* viene restituito se non è NULL. Altrimenti viene restituito *replacement_value* dopo che è stato convertito in modo implicito nel tipo di *check_expression*, se i tipi sono diversi. È possibile che *replacement_value* venga troncato se la lunghezza di *replacement_value* è maggiore di quella di *check_expression*.  
  
> [!NOTE]  
>  Usare [COALESCE &#40;Transact-SQL&#41;](../../t-sql/language-elements/coalesce-transact-sql.md) per restituire il primo valore non NULL.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-isnull-with-avg"></a>R. Utilizzo di ISNULL con AVG  
 Nell'esempio seguente viene calcolato il valore medio del peso di tutti i prodotti. Viene inoltre sostituito il valore `50` per tutte le voci NULL nella colonna `Weight` della tabella `Product`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT AVG(ISNULL(Weight, 50))  
FROM Production.Product;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 -------------------------- 
59.79  
  
 (1 row(s) affected)
 ```  
  
### <a name="b-using-isnull"></a>B. Utilizzo di ISNULL  
 Nell'esempio seguente vengono selezionate la descrizione, la percentuale di sconto, la quantità minima e la quantità massima per tutte le offerte speciali in `AdventureWorks2012`. Se la quantità massima per una determinata offerta speciale è NULL, il valore di `MaxQty` visualizzato nel set di risultati è `0.00`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Description, DiscountPct, MinQty, ISNULL(MaxQty, 0.00) AS 'Max Quantity'  
FROM Sales.SpecialOffer;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|  Descrizione       |  DiscountPct    |   MinQty    |   Max Quantity       |
|  ---------------   |  -------------  |   --------  |   ---------------    |
|  No Discount       |  0,00           |   0         |   0                  |
|  Volume Discount   |  0,02           |   11        |   14                 |
|  Volume Discount   |  0.05           |   15        |   4                  |
|  Volume Discount   |  0,10           |   25        |   0                  |
|  Volume Discount   |  0,15           |   41        |   0                  |
|  Volume Discount   |  0,20           |   61        |   0                  |
|  Mountain-100 Cl   |  0,35           |   0         |   0                  |
|  Sport Helmet Di   |  0,10           |   0         |   0                  |
|  Road-650 Overst   |  0,30           |   0         |   0                  |
|  Mountain Tire S   |  0.50           |   0         |   0                  |
|  Sport Helmet Di   |  0,15           |   0         |   0                  |
|  LL Road Frame S   |  0,35           |   0         |   0                  |
|  Touring-3000 Pr   |  0,15           |   0         |   0                  |
|  Touring-1000 Pr   |  0,20           |   0         |   0                  |
|  Half-Price Peda   |  0.50           |   0         |   0                  |
|  Mountain-500 Si   |  0,40           |   0         |   0                  |

 `(16 row(s) affected)`  
  
### <a name="c-testing-for-null-in-a-where-clause"></a>C. Verifica dei valori NULL in una clausola WHERE  
 Per trovare i valori NULL, non utilizzare ISNULL, bensì IS NULL. Nell'esempio seguente vengono trovati tutti i prodotti con valore `NULL` nella colonna relativa al peso. Si noti lo spazio tra `IS` e `NULL`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Name, Weight  
FROM Production.Product  
WHERE Weight IS NULL;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-using-isnull-with-avg"></a>D. Utilizzo di ISNULL con AVG  
 Nell'esempio seguente viene calcolato il valore medio del peso di tutti i prodotti in una tabella di esempio. Viene inoltre sostituito il valore `50` per tutte le voci NULL nella colonna `Weight` della tabella `Product`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT AVG(ISNULL(Weight, 50))  
FROM dbo.DimProduct;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
--------------------------   
52.88   
```  
  
### <a name="e-using-isnull"></a>E. Utilizzo di ISNULL  
 Nell'esempio seguente viene usato ISNULL per verificare i valori NULL nella colonna `MinPaymentAmount` e visualizzare il valore `0.00` per le righe.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ResellerName,   
       ISNULL(MinPaymentAmount,0) AS MinimumPayment  
FROM dbo.DimReseller  
ORDER BY ResellerName;  
  
```  
  
 Set di risultati parziale:  
  
|  ResellerName                |  MinimumPayment    |
|  -------------------------   |  --------------    |
|  A Bicycle Association       |     0,0000         |
|  A Bike Store                |     0,0000         |
|  A Cycle Shop                |     0,0000         |
|  A Great Bicycle Company     |     0,0000         |
|  A Typical Bike Shop         |   200,0000         |
|  Acceptable Sales & Service  |     0,0000         |
  
### <a name="f-using-is-null-to-test-for-null-in-a-where-clause"></a>F. Uso di IS NULL per verificare i valori NULL in una clausola WHERE  
 Nell'esempio seguente vengono trovati tutti i prodotti con valore `NULL` nella colonna `Weight`. Si noti lo spazio tra `IS` e `NULL`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT EnglishProductName, Weight  
FROM dbo.DimProduct  
WHERE Weight IS NULL;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Espressioni &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [IS NULL &#40;Transact-SQL&#41;](../../t-sql/queries/is-null-transact-sql.md)   
 [Funzioni di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)   
 [COALESCE &#40;Transact-SQL&#41;](../../t-sql/language-elements/coalesce-transact-sql.md)  
  
  

