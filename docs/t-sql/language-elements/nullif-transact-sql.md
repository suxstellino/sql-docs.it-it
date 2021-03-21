---
description: NULLIF (Transact-SQL)
title: NULLIF (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/08/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- NULLIF
- NULLIF_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- null values [SQL Server], equivalent expressions
- expressions [SQL Server], null values
- NULLIF function
- equivalent expressions [SQL Server]
ms.assetid: 44c7b67e-74c7-4bb9-93a4-7a3016bd2feb
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a54ea5ad6e7c66bc983e9d8101b7af235cb090cd
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752351"
---
# <a name="nullif-transact-sql"></a>NULLIF (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce un valore Null se le due espressioni specificate sono uguali. Ad esempio, `SELECT NULLIF(4,4) AS Same, NULLIF(5,7) AS Different;` restituisce NULL per la prima colonna (4 e 4) perché i due valori di input sono uguali. La seconda colonna restituisce il primo valore (5) perché i due valori di input sono diversi. 
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
NULLIF ( expression , expression )  
```  
  
## <a name="arguments"></a>Argomenti  
 *expression*  
 Qualsiasi [espressione](../../t-sql/language-elements/expressions-transact-sql.md) scalare valida.  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
 Restituisce lo stesso tipo di dati del primo argomento *expression*.  
  
 Se le due espressioni non sono uguali, la funzione NULLIF restituisce il primo argomento *expression*. Se le espressioni sono uguali, NULLIF restituisce un valore Null del tipo del primo argomento *expression*.  
  
## <a name="remarks"></a>Commenti  
 NULLIF è equivalente a un'espressione CASE avanzata in cui le due espressioni sono uguali e l'espressione risultante è Null.  
  
 È consigliabile non utilizzare funzioni dipendenti dal tempo, ad esempio RAND(), in una funzione NULLIF. La funzione potrebbe essere valutata due volte e restituire risultati diversi nelle due chiamate.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-returning-budget-amounts-that-have-not-changed"></a>R. Restituzione degli importi del budget non modificati  
 Nell'esempio seguente viene creata una tabella `budgets` per visualizzare un reparto (`dept`), il relativo budget corrente (`current_year`) e il relativo budget precedente (`previous_year`). Per l'anno in corso, viene utilizzato `NULL` per i reparti in cui il budget dell'anno corrente è uguale a quello dell'anno precedente e `0` per i budget non ancora definiti. Per trovare la media solo dei reparti che ricevono un budget e includere inoltre il valore del budget dell'anno precedente (utilizzare il valore `previous_year` se `current_year` è `NULL`), vengono combinate le funzioni `NULLIF` e `COALESCE`.  
  
```sql  
CREATE TABLE dbo.budgets  
(  
   dept            TINYINT   IDENTITY,  
   current_year    DECIMAL   NULL,  
   previous_year   DECIMAL   NULL  
);  
INSERT budgets VALUES(100000, 150000);  
INSERT budgets VALUES(NULL, 300000);  
INSERT budgets VALUES(0, 100000);  
INSERT budgets VALUES(NULL, 150000);  
INSERT budgets VALUES(300000, 250000);  
GO    
SET NOCOUNT OFF;  
SELECT AVG(NULLIF(COALESCE(current_year,  
   previous_year), 0.00)) AS [Average Budget]  
FROM budgets;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Average Budget  
 --------------  
 212500.000000  
 (1 row(s) affected)
 ```  
  
### <a name="b-comparing-nullif-and-case"></a>B. Confronto tra NULLIF e CASE  
 Per illustrare la somiglianza tra `NULLIF` e `CASE`, nelle query seguenti viene determinato se i valori nelle colonne `MakeFlag` e `FinishedGoodsFlag` corrispondono. La prima query utilizza `NULLIF`. La seconda query utilizza l'espressione `CASE`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT ProductID, MakeFlag, FinishedGoodsFlag,   
   NULLIF(MakeFlag,FinishedGoodsFlag) AS [Null if Equal]  
FROM Production.Product  
WHERE ProductID < 10;  
GO  
  
SELECT ProductID, MakeFlag, FinishedGoodsFlag, [Null if Equal] =  
   CASE  
       WHEN MakeFlag = FinishedGoodsFlag THEN NULL  
       ELSE MakeFlag  
   END  
FROM Production.Product  
WHERE ProductID < 10;  
GO  
```  

### <a name="c-returning-budget-amounts-that-contain-no-data"></a>C. Restituzione degli importi del budget che non contengono dati  
 Nell'esempio seguente viene creata una tabella `budgets`, vengono caricati i dati e viene usato `NULLIF` per restituire un valore Null se né `current_year` né `previous_year` contengono dati.  
  
```sql  
CREATE TABLE budgets (  
   dept           TINYINT,  
   current_year   DECIMAL(10,2),  
   previous_year  DECIMAL(10,2)  
);  
  
INSERT INTO budgets VALUES(1, 100000, 150000);  
INSERT INTO budgets VALUES(2, NULL, 300000);  
INSERT INTO budgets VALUES(3, 0, 100000);  
INSERT INTO budgets VALUES(4, NULL, 150000);  
INSERT INTO budgets VALUES(5, 300000, 300000);  
  
SELECT dept, NULLIF(current_year,  
   previous_year) AS LastBudget  
FROM budgets;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 dept   LastBudget  
 ----   -----------  
 1      100000.00  
 2      null 
 3      0.00  
 4      null  
 5      null
 ```  
  
## <a name="see-also"></a>Vedere anche  
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)   
 [decimal e numeric &#40;Transact-SQL&#41;](../../t-sql/data-types/decimal-and-numeric-transact-sql.md)   
 [Funzioni di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)  
  
  

