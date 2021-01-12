---
description: ELSE (IF...ELSE) (Transact-SQL)
title: ELSE (IF...ELSE) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ELSE
- ELSE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ELSE (IF...ELSE) keyword
- ELSE keyword
- IF keyword
ms.assetid: 6f2b4278-0dea-4603-bbd3-7cbad602a645
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1217953d76c37fd4e03e41336ec2f6846fdd255b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102490"
---
# <a name="else-ifelse-transact-sql"></a>ELSE (IF...ELSE) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Impone le condizioni per l'esecuzione di un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)]. L'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] (*sql_statement*) che segue *Boolean_expression* viene eseguita se *Boolean_expression* restituisce TRUE. La parola chiave facoltativa ELSE rappresenta un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] alternativa che viene eseguita quando l'argomento *Boolean_expression* restituisce FALSE o NULL.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
IF Boolean_expression   
     { sql_statement | statement_block }   
[ ELSE   
     { sql_statement | statement_block } ]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *Boolean_expression*  
 Espressione che restituisce TRUE o FALSE. Se *Boolean_expression* contiene un'istruzione SELECT, tale istruzione deve essere racchiusa tra parentesi.  
  
 { *sql_statement* | *statement_block* }  
 Qualsiasi istruzione o gruppo di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] valido definito con un blocco di istruzioni. Per definire un blocco di istruzioni (batch), utilizzare le parole chiave del linguaggio per il controllo di flusso BEGIN ed END. Sebbene tutte le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] siano valide nell'ambito di un blocco BEGIN...END, alcune istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] non devono essere raggruppate nello stesso batch (blocco di istruzioni).  
  
## <a name="result-types"></a>Tipi restituiti  
 **Boolean**  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-a-simple-boolean-expression"></a>R. Esecuzione di un'espressione booleana  
 Nell'esempio seguente è presente un'espressione booleana semplice (`1=1`) con valore true e di conseguenza viene stampata la prima istruzione.  
  
```sql
IF 1 = 1 PRINT 'Boolean_expression is true.'  
ELSE PRINT 'Boolean_expression is false.' ;  
```  
  
 Nell'esempio seguente è presente un'espressione booleana semplice (`1=2`) con valore false e di conseguenza viene stampata la seconda istruzione.  
  
```sql
IF 1 = 2 PRINT 'Boolean_expression is true.'  
ELSE PRINT 'Boolean_expression is false.' ;  
GO  
```  
  
### <a name="b-using-a-query-as-part-of-a-boolean-expression"></a>B. Utilizzo di una query come parte di un'espressione booleana  
 Nell'esempio seguente viene eseguita una query come parte dell'espressione booleana. Poiché nella tabella `Product` sono presenti 10 biciclette che soddisfano la clausola `WHERE`, verrà eseguita la prima istruzione di stampa. Modificare `> 5` in `> 15` per ottenere il modo in cui potrebbe essere eseguita la seconda parte dell'istruzione.  
  
```sql
USE AdventureWorks2012;  
GO  
IF   
(SELECT COUNT(*) FROM Production.Product WHERE Name LIKE 'Touring-3000%' ) > 5  
PRINT 'There are more than 5 Touring-3000 bicycles.'  
ELSE PRINT 'There are 5 or less Touring-3000 bicycles.' ;  
GO  
```  
  
### <a name="c-using-a-statement-block"></a>C. Utilizzo di un blocco di istruzioni  
 Nell'esempio seguente viene eseguita una query come parte dell'espressione booleana e successivamente vengono eseguiti blocchi di istruzioni leggermente diversi in base al risultato dell'espressione booleana. Ogni blocco di istruzioni inizia con `BEGIN` e termina con `END`.  
  
```sql
USE AdventureWorks2012;  
GO  
DECLARE @AvgWeight DECIMAL(8,2), @BikeCount INT  
IF   
(SELECT COUNT(*) FROM Production.Product WHERE Name LIKE 'Touring-3000%' ) > 5  
BEGIN  
   SET @BikeCount =   
        (SELECT COUNT(*)   
         FROM Production.Product   
         WHERE Name LIKE 'Touring-3000%');  
   SET @AvgWeight =   
        (SELECT AVG(Weight)   
         FROM Production.Product   
         WHERE Name LIKE 'Touring-3000%');  
   PRINT 'There are ' + CAST(@BikeCount AS VARCHAR(3)) + ' Touring-3000 bikes.'  
   PRINT 'The average weight of the top 5 Touring-3000 bikes is ' + CAST(@AvgWeight AS VARCHAR(8)) + '.';  
END  
ELSE   
BEGIN  
SET @AvgWeight =   
        (SELECT AVG(Weight)  
         FROM Production.Product   
         WHERE Name LIKE 'Touring-3000%' );  
   PRINT 'Average weight of the Touring-3000 bikes is ' + CAST(@AvgWeight AS VARCHAR(8)) + '.' ;  
END ;  
GO  
```  
  
### <a name="d-using-nested-ifelse-statements"></a>D. Utilizzo di istruzioni IF...ELSE nidificate  
 L'esempio seguente mostra come è possibile annidare più istruzioni IF ... ELSE. Impostare la variabile `@Number` su `5`, `50` e `500` per eseguire il test di ogni istruzione.  
  
```sql
DECLARE @Number INT;  
SET @Number = 50;  
IF @Number > 100  
   PRINT 'The number is large.';  
ELSE   
   BEGIN  
      IF @Number < 10  
      PRINT 'The number is small.';  
   ELSE  
      PRINT 'The number is medium.';  
   END ;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-a-query-as-part-of-a-boolean-expression"></a>B. Uso di una query come parte di un'espressione booleana  
 L'esempio seguente usa `IF...ELSE` per determinare quale delle due risposte visualizzare all'utente, in base al peso di un elemento nella tabella `DimProduct`.  
  
```sql
-- Uses AdventureWorks  
  
DECLARE @maxWeight FLOAT, @productKey INTEGER  
SET @maxWeight = 100.00  
SET @productKey = 424  
IF @maxWeight <= (SELECT Weight FROM DimProduct WHERE ProductKey=@productKey)   
    (SELECT @productKey, EnglishDescription, Weight, 'This product is too heavy to ship and is only available for pickup.' FROM DimProduct WHERE ProductKey=@productKey)  
ELSE  
    (SELECT @productKey, EnglishDescription, Weight, 'This product is available for shipping or pickup.' FROM DimProduct WHERE ProductKey=@productKey)  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [Elementi del linguaggio per il controllo di flusso &#40;Transact-SQL&#41;](~/t-sql/language-elements/control-of-flow.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [IF...ELSE &#40;Transact-SQL&#41;](../../t-sql/language-elements/if-else-transact-sql.md)  
  
  


