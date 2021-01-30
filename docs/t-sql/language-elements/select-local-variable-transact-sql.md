---
description: SELECT @local_variable (Transact-SQL)
title: SELECT @local_variable (Transact-SQL)
ms.custom: ''
ms.date: 09/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- variable_TSQL
- '@loca_variable'
- '@local'
- variable
- '@loca_variable_TSQL'
- '@local_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- variables [SQL Server], assigning
- SELECT statement [SQL Server], @local_variable
- '@local_variable'
- local variables [SQL Server]
ms.assetid: 8e1a9387-2c5d-4e51-a1fd-a2a95f026d6f
author: cawrites
ms.author: chadam
monikerRange: = azuresqldb-current ||>= sql-server-2016 ||= azure-sqldw-latest||>= sql-server-linux-2017
ms.openlocfilehash: 69e2495cb78896378196b163c868e7a2d069f1b3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194280"
---
# <a name="select-local_variable-transact-sql"></a>SELECT @local_variable (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Imposta una variabile locale sul valore di un'espressione.  
  
 Per l'assegnazione delle variabili è consigliabile usare [SET @local_variable](../../t-sql/language-elements/set-local-variable-transact-sql.md) anziché SELECT @*local_variable*.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
SELECT { @local_variable { = | += | -= | *= | /= | %= | &= | ^= | |= } expression } 
    [ ,...n ] [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

@*local_variable*  
 Variabile dichiarata a cui deve essere assegnato un valore.  
  
{= \| += \| -= \| \*= \| /= \| %= \| &= \| ^= \| \|= }  
Assegna il valore a destra alla variabile a sinistra.  
  
Operatore di assegnazione composto:  

| operator | action |  
| -------- | ------ |  
| = | Assegna l'espressione seguente alla variabile |  
| += | Aggiunta e assegnazione |  
| -= | Sottrazione e assegnazione |  
| \*= | Moltiplicazione e assegnazione |  
| /= | Divisione e assegnazione |  
| %= | Modulo e assegnazione |  
| &= | AND bit per bit e assegnazione |  
| ^= | XOR bit per bit e assegnazione |  
| \|= | OR bit per bit e assegnazione |  

*expression*  
Qualsiasi [espressione](../../t-sql/language-elements/expressions-transact-sql.md) valida. Include una sottoquery scalare.  

## <a name="remarks"></a>Osservazioni

L'istruzione SELECT @*local_variable* viene in genere usata per restituire un valore singolo nella variabile. Se tuttavia *expression* corrisponde al nome di una colonna, è possibile che restituisca più valori. Se l'istruzione SELECT restituisce più valori, alla variabile viene assegnato l'ultimo valore restituito.  

Se l'istruzione SELECT non restituisce righe, la variabile mantiene il valore corrente. Se *expression* è una sottoquery scalare che non restituisce valori, la variabile viene impostata su NULL.  

Un'istruzione SELECT può inizializzare più variabili locali.  

> [!NOTE]
> Un'istruzione SELECT che include l'assegnazione di un valore a una variabile non può essere utilizzata anche per eseguire normali operazioni di recupero del set di risultati.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-use-select-local_variable-to-return-a-single-value"></a>R. Usare SELECT @local_variable per restituire un valore singolo  
 Nell'esempio seguente alla variabile `@var1` viene assegnato `Generic Name` come valore. La query eseguita nella tabella `Store` non restituisce righe perché il valore specificato per `CustomerID` non esiste nella tabella. La variabile mantiene il valore `Generic Name`.  
  
```sql  
-- Uses AdventureWorks    
  
DECLARE @var1 VARCHAR(30);         
SELECT @var1 = 'Generic Name';         
SELECT @var1 = Name         
FROM Sales.Store         
WHERE CustomerID = 1000 ;        
SELECT @var1 AS 'Company Name';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Company Name  
 ------------------------------  
 Generic Name  
 ```  
  
### <a name="b-use-select-local_variable-to-return-null"></a>B. Usare SELECT @local_variable per restituire Null  
 Nell'esempio seguente viene utilizzata una sottoquery per assegnare un valore a `@var1`. Poiché il valore richiesto da `CustomerID` non esiste, la sottoquery non restituisce valori e la variabile viene impostata su `NULL`.  
  
```sql  
-- Uses AdventureWorks  
  
DECLARE @var1 VARCHAR(30)   
SELECT @var1 = 'Generic Name'   
SELECT @var1 = (SELECT Name   
FROM Sales.Store   
WHERE CustomerID = 1000)   
SELECT @var1 AS 'Company Name' ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
Company Name  
----------------------------  
NULL  
```  
  
## <a name="see-also"></a>Vedere anche  
 [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [Espressioni &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Operatori composti &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)  
