---
title: IF...ELSE (Transact-SQL) | Microsoft Docs
description: Informazioni di riferimento sul linguaggio Transact-SQl per le istruzioni IF-ELSE per fornire il flusso di controllo nelle istruzioni Transact-SQL.
ms.date: 07/11/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IF_TSQL
- IF
dev_langs:
- TSQL
helpviewer_keywords:
- IF...ELSE keyword
- ELSE (IF...ELSE) keyword
- ELSE keyword
- IF keyword
ms.assetid: 676c881f-dee1-417a-bc51-55da62398e81
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 27444d6caae36530c18907bf128b5ff255df0852
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749861"
---
# <a name="ifelse-transact-sql"></a>IF...ELSE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Impone le condizioni per l'esecuzione di un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)]. L'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] che segue una parola chiave IF e le relative condizioni viene eseguita se le condizioni vengono soddisfatte, ovvero quando l'espressione booleana restituisce TRUE. La parola chiave facoltativa ELSE introduce un'altra istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] eseguita quando non viene soddisfatta la condizione IF, ovvero quando l'espressione booleana restituisce FALSE.  
  
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
 Espressione che restituisce TRUE o FALSE. Se l'espressione booleana include un'istruzione SELECT, tale istruzione deve essere racchiusa tra parentesi.  
  
 { *sql_statement*| *statement_block* }  
 Qualsiasi istruzione o gruppo di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] valido definito tramite un blocco di istruzioni. Le condizioni IF o ELSE possono influire sulle prestazioni di una sola istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)], a meno che non venga utilizzato un blocco di istruzioni.  
  
 Per definire un blocco di istruzioni, utilizzare le parole chiave per il controllo di flusso BEGIN ed END.  
  
## <a name="remarks"></a>Osservazioni  
 È possibile utilizzare un costrutto IF...ELSE in batch, stored procedure e query ad hoc. In caso di utilizzo in una stored procedure, questo costrutto viene in genere utilizzato per verificare l'esistenza di parametri.  
  
 È possibile nidificare condizioni IF dopo un'altra condizione IF o una parola chiave ELSE. Il limite del numero di livelli di nidificazione dipende dalla memoria disponibile.  
  
## <a name="example"></a>Esempio  
  
```sql
IF DATENAME(weekday, GETDATE()) IN (N'Saturday', N'Sunday')
       SELECT 'Weekend';
ELSE 
       SELECT 'Weekday';
```  
  
 Per altri esempi, vedere [ELSE &#40;IF...ELSE&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/else-if-else-transact-sql.md).  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 L'esempio seguente usa `IF...ELSE` per determinare quale delle due risposte visualizzare all'utente, in base al peso di un elemento nella tabella `DimProduct`.  
  
```sql
-- Uses AdventureWorksDW  

DECLARE @maxWeight FLOAT, @productKey INTEGER  
SET @maxWeight = 100.00  
SET @productKey = 424  
IF @maxWeight <= (SELECT Weight from DimProduct WHERE ProductKey = @productKey)   
    SELECT @productKey AS ProductKey, EnglishDescription, Weight, 'This product is too heavy to ship and is only available for pickup.' 
        AS ShippingStatus
    FROM DimProduct WHERE ProductKey = @productKey
ELSE  
    SELECT @productKey AS ProductKey, EnglishDescription, Weight, 'This product is available for shipping or pickup.' 
        AS ShippingStatus
    FROM DimProduct WHERE ProductKey = @productKey
```  
  
## <a name="see-also"></a>Vedere anche  
 [BEGIN...END &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-end-transact-sql.md)   
 [END &#40;BEGIN...END&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/end-begin-end-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [WHILE &#40;Transact-SQL&#41;](../../t-sql/language-elements/while-transact-sql.md)   
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)   
 [Control-of-Flow Language &#40;Transact-SQL&#41;](~/t-sql/language-elements/control-of-flow.md) [ELSE &#40;IF...ELSE&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/else-if-else-transact-sql.md) 
