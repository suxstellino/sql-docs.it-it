---
description: IN (Transact-SQL)
title: IN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IN_TSQL
- IN
dev_langs:
- TSQL
helpviewer_keywords:
- values [SQL Server], matching
- NOT IN keyword
- 8623 (Database Engine error)
- matching values in subquery or list [SQL Server]
- IN keyword
- 8632 (Database Engine error)
ms.assetid: 4419de73-96b1-4dfe-8500-f4507915db04
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4e4f50467ecee94acb09fc1bb07685daca9a1285
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99113019"
---
# <a name="in-transact-sql"></a>IN (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Determina se il valore specificato corrisponde a un valore in una sottoquery o in un elenco.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
test_expression [ NOT ] IN   
    ( subquery | expression [ ,...n ]  
    )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *test_expression*  
 Qualsiasi [espressione](../../t-sql/language-elements/expressions-transact-sql.md) valida.  
  
 *subquery*  
 Sottoquery che include un set di risultati di una colonna. Il tipo di dati di questa colonna deve corrispondere a quello di *test_expression*.  
  
 *expression*[ **,**... *n* ]  
 Elenco di espressioni in cui individuare una corrispondenza. Tutte le espressioni devono essere dello stesso tipo di *test_expression*.  
  
## <a name="result-types"></a>Tipi restituiti  
 **Boolean**  
  
## <a name="result-value"></a>Valore restituito  
 Se il valore di *test_expression* è uguale a uno dei valori restituiti da *subquery* o a qualsiasi elemento *expression* dell'elenco delimitato da virgole, il valore restituito è TRUE. In caso contrario, il valore restituito è FALSE.  
  
 L'uso di NOT IN nega il valore di *subquery* o l'elemento *expression*.  
  
> [!CAUTION]  
>  Qualsiasi valore Null restituito da *subquery* o *expression* che viene confrontato con *test_expression* usando IN o NOT IN restituisce UNKNOWN. L'utilizzo di valori Null insieme a IN o NOT IN può generare risultati imprevisti.  
  
## <a name="remarks"></a>Osservazioni  
 L'inclusione esplicita di un numero estremamente elevato di valori (molte migliaia di valori delimitati da virgole) all'interno di parentesi, in una clausola IN può determinare un uso elevato delle risorse e restituire gli errori 8623 o 8632. Per risolvere questo problema, archiviare gli elementi dell'elenco IN in una tabella e usare una sottoquery SELECT all'interno della clausola IN.  
  
 Errore 8623:  
  
 `The query processor ran out of internal resources and could not produce a query plan. This is a rare event and only expected for extremely complex queries or queries that reference a very large number of tables or partitions. Please simplify the query. If you believe you have received this message in error, contact Customer Support Services for more information.`  
  
 Errore 8632:  
  
 `Internal error: An expression services limit has been reached. Please look for potentially complex expressions in your query, and try to simplify them.`  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-comparing-or-and-in"></a>R. Confronto di OR e IN  
 Nell'esempio seguente viene selezionato un elenco di nomi dei dipendenti con il titolo di Design Engineer, Tool Designer o Marketing Assistant.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT p.FirstName, p.LastName, e.JobTitle  
FROM Person.Person AS p  
JOIN HumanResources.Employee AS e  
    ON p.BusinessEntityID = e.BusinessEntityID  
WHERE e.JobTitle = 'Design Engineer'   
   OR e.JobTitle = 'Tool Designer'   
   OR e.JobTitle = 'Marketing Assistant';  
GO  
```  
  
 Gli stessi risultati vengono tuttavia recuperati tramite IN.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT p.FirstName, p.LastName, e.JobTitle  
FROM Person.Person AS p  
JOIN HumanResources.Employee AS e  
    ON p.BusinessEntityID = e.BusinessEntityID  
WHERE e.JobTitle IN ('Design Engineer', 'Tool Designer', 'Marketing Assistant');  
GO  
```  
  
 Set di risultati per entrambe le query.  
  
```  
FirstName   LastName      Title  
---------   ---------   ---------------------  
Sharon      Salavaria   Design Engineer                                     
Gail        Erickson    Design Engineer                                     
Jossef      Goldberg    Design Engineer                                     
Janice      Galvin      Tool Designer                                       
Thierry     D'Hers      Tool Designer                                       
Wanida      Benshoof    Marketing Assistant                                 
Kevin       Brown       Marketing Assistant                                 
Mary        Dempsey     Marketing Assistant                                 
  
(8 row(s) affected)  
```  
  
### <a name="b-using-in-with-a-subquery"></a>B. Utilizzo di IN con una sottoquery  
 Nell'esempio seguente vengono recuperati tutti gli ID dei venditori nella tabella `SalesPerson` per i dipendenti la cui quota di vendita per l'anno corrente supera € 250.000 e vengono quindi selezionati dalla tabella `Employee` i nomi di tutti i dipendenti il cui `EmployeeID` corrisponde ai risultati della sottoquery `SELECT`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT p.FirstName, p.LastName  
FROM Person.Person AS p  
    JOIN Sales.SalesPerson AS sp  
    ON p.BusinessEntityID = sp.BusinessEntityID  
WHERE p.BusinessEntityID IN  
   (SELECT BusinessEntityID  
   FROM Sales.SalesPerson  
   WHERE SalesQuota > 250000);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
FirstName   LastName                                             
---------   --------   
Tsvi         Reiter                                              
Michael      Blythe                                              
Tete         Mensa-Annan                                         
  
(3 row(s) affected)  
```  
  
### <a name="c-using-not-in-with-a-subquery"></a>C. Utilizzo di NOT IN con una sottoquery  
 Nell'esempio seguente vengono recuperati tutti i venditori la cui quota di vendita è inferiore o uguale a €250.000. `NOT IN` restituisce i venditori che non corrispondono agli elementi nell'elenco dei valori.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT p.FirstName, p.LastName  
FROM Person.Person AS p  
    JOIN Sales.SalesPerson AS sp  
    ON p.BusinessEntityID = sp.BusinessEntityID  
WHERE p.BusinessEntityID NOT IN  
   (SELECT BusinessEntityID  
   FROM Sales.SalesPerson  
   WHERE SalesQuota > 250000);  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-using-in-and-not-in"></a>D. Uso di IN e NOT IN  
 Nell'esempio seguente vengono recuperate tutte le voci nella tabella `FactInternetSales` che corrispondono ai valori `SalesReasonKey` nella tabella `DimSalesReason`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT * FROM FactInternetSalesReason   
WHERE SalesReasonKey   
IN (SELECT SalesReasonKey FROM DimSalesReason);   
```  
  
 Nell'esempio seguente vengono recuperate tutte le voci nella tabella `FactInternetSalesReason` che non corrispondono ai valori `SalesReasonKey` nella tabella `DimSalesReason`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT * FROM FactInternetSalesReason   
WHERE SalesReasonKey   
NOT IN (SELECT SalesReasonKey FROM DimSalesReason);  
```  
  
### <a name="e-using-in-with-an-expression-list"></a>E. Uso di IN con un elenco di espressioni  
 Nell'esempio seguente vengono recuperati tutti gli ID dei venditori nella tabella `DimEmployee` per i dipendenti di nome `Mike` o `Michael`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT FirstName, LastName  
FROM DimEmployee  
WHERE FirstName IN ('Mike', 'Michael');  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)   
 [Espressioni &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Funzioni predefinite &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [Operatori &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)   
 [ALL &#40;Transact-SQL&#41;](../../t-sql/language-elements/all-transact-sql.md)   
 [SOME &#124; ANY &#40;Transact-SQL&#41;](../../t-sql/language-elements/some-any-transact-sql.md)  
  
  



