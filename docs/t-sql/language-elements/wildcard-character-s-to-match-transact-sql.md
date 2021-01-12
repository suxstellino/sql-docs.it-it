---
title: '[ ] Carattere jolly per la corrispondenza dei caratteri'
description: Usare un carattere jolly per trovare una corrispondenza con uno o più caratteri.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 12/06/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- Match
- wildcard
- '[ ]'
- '[_]_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- wildcard characters [SQL Server]
- '[ ] (wildcard - character(s) to match)'
ms.assetid: 57817576-0bf1-49ed-b05d-fac27e8fed7a
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b0cc55631431246f87a64c9a17fe953e34c454f3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095881"
---
# <a name="--wildcard---characters-to-match-transact-sql"></a>\[ \] (Caratteri jolly per la corrispondenza) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Rileva la corrispondenza con uno dei caratteri inclusi nell'intervallo o set racchiuso tra parentesi quadre `[ ]`. È possibile usare i caratteri jolly nei confronti di stringhe che prevedono l'uso di criteri di ricerca, ad esempio `LIKE` e `PATINDEX`.  

 
## <a name="examples"></a>Esempi  
### <a name="a-simple-example"></a>A: Esempio semplice   
L'esempio seguente restituisce i nomi che iniziano con la lettera `m`. `[n-z]` specifica che la seconda lettera deve essere inclusa nell'intervallo tra `n` e `z`. Il carattere jolly percentuale `%` indica che il terzo carattere può essere qualsiasi o nessuno. I database `model` e `msdb` soddisfano questi criteri. Il database `master` non soddisfa i criteri e viene escluso dal set di risultati.
 
```sql
SELECT name FROM sys.databases
WHERE name LIKE 'm[n-z]%';
```
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]  

```
name
-----
model
msdb
```   
 È possibile che siano installati altri database che soddisfano i criteri.


### <a name="b-more-complex-example"></a>B: Esempio più complesso   
 Nell'esempio seguente viene utilizzato l'operatore [] per trovare gli ID e i nomi di tutti i dipendenti inclusi in [!INCLUDE[ssSampleDBCoShort](../../includes/sssampledbcoshort-md.md)] il cui indirizzo è associato a un C.A.P. a quattro cifre.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT e.BusinessEntityID, p.FirstName, p.LastName, a.PostalCode  
FROM HumanResources.Employee AS e  
INNER JOIN Person.Person AS p ON e.BusinessEntityID = p.BusinessEntityID  
INNER JOIN Person.BusinessEntityAddress AS ea ON e.BusinessEntityID = ea.BusinessEntityID  
INNER JOIN Person.Address AS a ON a.AddressID = ea.AddressID  
WHERE a.PostalCode LIKE '[0-9][0-9][0-9][0-9]';  
```  
  
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]  
  
```  
EmployeeID      FirstName      LastName      PostalCode  
----------      ---------      ---------     ----------  
290             Lynn           Tsoflias      3000  
```  

### <a name="c-using-a-set-that-combines-ranges-and-single-characters"></a>C: Uso di un set che combina intervalli e caratteri singoli

Un set di caratteri jolly può includere sia caratteri singoli sia intervalli. Nell'esempio seguente viene usato l'operatore [] per trovare una stringa che inizia con un numero o una serie di caratteri speciali.

```sql
SELECT [object_id], OBJECT_NAME(object_id) AS [object_name], name, column_id 
FROM sys.columns 
WHERE name LIKE '[0-9!@#$.,;_]%';
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]  

```
object_id     object_name                         name  column_id
---------     -----------                         ----  ---------
615673241     vSalesPersonSalesByFiscalYears      2002  5
615673241     vSalesPersonSalesByFiscalYears      2003  6
615673241     vSalesPersonSalesByFiscalYears      2004  7
1591676718    JunkTable                           _xyz  1
```
  
## <a name="see-also"></a>Vedere anche  
 [LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)   
 [PATINDEX &#40;Transact-SQL&#41;](../../t-sql/functions/patindex-transact-sql.md)   
  [% &#40;caratteri jolly per la corrispondenza&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/percent-character-wildcard-character-s-to-match-transact-sql.md)   
 [&#91;^&#93; &#40;Wildcard - Character&#40;s&#41; Not to Match&#41; (Carattere jolly - Non corrispondente/i a) &#40;Transact-SQL&#41;](../../t-sql/language-elements/wildcard-character-s-not-to-match-transact-sql.md)     
 [\_ &#40;carattere jolly per corrispondenze di singoli caratteri&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/wildcard-match-one-character-transact-sql.md)  
    
  
