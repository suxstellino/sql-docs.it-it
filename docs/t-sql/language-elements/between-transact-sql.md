---
description: BETWEEN (Transact-SQL)
title: BETWEEN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/28/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- BETWEEN
- BETWEEN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- inclusive ranges
- testing range
- exclusive ranges
- NOT BETWEEN operator
- BETWEEN operator
- range to test [SQL Server]
ms.assetid: a5d5b050-203e-4355-ac85-e08ef5ca7823
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8f8df816ba1885e6dd713d8b77affcd4b1175c3c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104747861"
---
# <a name="between-transact-sql"></a>BETWEEN (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Specifica un intervallo da testare.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
test_expression [ NOT ] BETWEEN begin_expression AND end_expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *test_expression*  
 [Espressione](../../t-sql/language-elements/expressions-transact-sql.md) da testare nell'intervallo definito da *begin_expression* e *end_expression*. *test_expression* deve essere dello stesso tipo di dati di *begin_expression* e *end_expression*.  
  
 NOT  
 Specifica che il risultato del predicato viene negato.  
  
 *begin_expression*  
 È qualsiasi espressione valida. *begin_expression* deve essere dello stesso tipo di dati di *test_expression* e *end_expression*.  
  
 *end_expression*  
 È qualsiasi espressione valida. *end_expression* deve essere dello stesso tipo di dati di *test_expression* e *begin_expression*.  
  
 AND  
 Fa da segnaposto e indica che *test_expression* deve essere compreso nell'intervallo delimitato da *begin_expression* e *end_expression*.  
  
## <a name="result-types"></a>Tipi restituiti  
 **Boolean**  
  
## <a name="result-value"></a>Valore restituito  
 BETWEEN restituisce **TRUE** se il valore di *test_expression* è maggiore o uguale al valore di *begin_expression* e minore o uguale al valore di *end_expression*.  
  
 NOT BETWEEN restituisce **TRUE** se il valore di *test_expression* è minore del valore di *begin_expression* o maggiore del valore di *end_expression*.  
  
## <a name="remarks"></a>Osservazioni  
 Per specificare un intervallo esclusivo, usare gli operatori Maggiore di (>) e Minore di (<). Se l'input per il predicato BETWEEN o NOT BETWEEN è NULL, il risultato è UNKNOWN.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-between"></a>R. Utilizzo di BETWEEN  
 Nell'esempio seguente vengono restituite informazioni sui ruoli di database in un database. La prima query restituisce tutti i ruoli. Il secondo esempio usa la clausola `BETWEEN` per limitare i ruoli ai valori `database_id` specificati.  
  
```sql  
SELECT principal_id, name 
FROM sys.database_principals
WHERE type = 'R';

SELECT principal_id, name 
FROM sys.database_principals
WHERE type = 'R'
AND principal_id BETWEEN 16385 AND 16390;
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]   
```  
principal_id    name
------------  ---- 
0               public
16384           db_owner
16385           db_accessadmin
16386           db_securityadmin
16387           db_ddladmin
16389           db_backupoperator
16390           db_datareader
16391           db_datawriter
16392           db_denydatareader
16393           db_denydatawriter
```  
```  
principal_id    name
------------  ---- 
16385           db_accessadmin
16386           db_securityadmin
16387           db_ddladmin
16389           db_backupoperator
16390           db_datareader
```  
  
### <a name="b-using--and--instead-of-between"></a>B. Uso di > e < al posto di BETWEEN  
 Nell'esempio seguente vengono utilizzati gli operatori maggiore di (`>`) e minore di (`<`) e, poiché non sono operatori inclusivi, vengono restituite nove righe anziché le dieci restituite nell'esempio precedente.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT e.FirstName, e.LastName, ep.Rate  
FROM HumanResources.vEmployee e   
JOIN HumanResources.EmployeePayHistory ep   
    ON e.BusinessEntityID = ep.BusinessEntityID  
WHERE ep.Rate > 27 AND ep.Rate < 30  
ORDER BY ep.Rate;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```  
 FirstName   LastName             Rate  
 ---------   -------------------  ---------  
 Paula       Barreto de Mattos    27.1394  
 Janaina     Bueno                27.4038  
 Dan         Bacon                27.4038  
 Ramesh      Meyyappan            27.4038  
 Karen       Berg                 27.4038  
 David       Bradley              28.7500  
 Hazem       Abolrous             28.8462  
 Ovidiu      Cracium              28.8462  
 Rob         Walters              29.8462  
 ```    
  
### <a name="c-using-not-between"></a>C. Utilizzo di NOT BETWEEN  
 Nell'esempio seguente vengono trovate tutte le righe che non rientrano nell'intervallo da `27` a `30`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT e.FirstName, e.LastName, ep.Rate  
FROM HumanResources.vEmployee e   
JOIN HumanResources.EmployeePayHistory ep   
    ON e.BusinessEntityID = ep.BusinessEntityID  
WHERE ep.Rate NOT BETWEEN 27 AND 30  
ORDER BY ep.Rate;  
GO  
```  
  
### <a name="d-using-between-with-datetime-values"></a>D. Utilizzo di BETWEEN tra valori datetime  
 Nell'esempio seguente vengono recuperate le righe con valori **datetime** compresi tra `'20011212'` e `'20020105'`, entrambi inclusi.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT BusinessEntityID, RateChangeDate  
FROM HumanResources.EmployeePayHistory  
WHERE RateChangeDate BETWEEN '20011212' AND '20020105';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```  
 BusinessEntityID RateChangeDate  
 ----------- -----------------------  
 3           2001-12-12 00:00:00.000  
 4           2002-01-05 00:00:00.000  
 ```  
 
 La query recupera le righe previste poiché i valori di data nella query e i valori **datetime** archiviati nella colonna `RateChangeDate` sono stati specificati senza la parte della data relativa all'ora. Quando la frazione di ora non è specificata, il valore predefinito viene impostato su 00.00. Si noti che una riga contenente una frazione dell'ora successiva all'ora 0.00 del 5 gennaio 2002 non verrà restituita dalla query, poiché non inclusa nell'intervallo.  
  
  
## <a name="see-also"></a>Vedere anche  
 [&#62; &#40;Greater Than&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/greater-than-transact-sql.md)   
 [&#60; &#40;Less Than&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/less-than-transact-sql.md)   
 [Espressioni &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Funzioni predefinite &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [Operatori &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)  
  
  


