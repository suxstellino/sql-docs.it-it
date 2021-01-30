---
description: '| (OR bit per bit) (Transact-SQL)'
title: '| (OR bit per bit) (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 01/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '|'
- '|_TSQL'
- Bitwise OR
- bitwise
- OR
dev_langs:
- TSQL
helpviewer_keywords:
- OR operator
- bitwise OR (|)
- '| (bitwise OR operator)'
ms.assetid: 86a3b87f-9688-4eaf-a552-29f1b01d880a
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ff76c7ce5b98084aaf6f833a3e1b85d9fa8f68be
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193390"
---
# <a name="-bitwise-or-transact-sql"></a>| (OR bit per bit) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Esegue un'operazione con OR logico bit per bit tra i due valori interi specificati convertiti in espressioni binarie in istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql   
expression | expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *expression*  
 Qualsiasi valore [expression](../../t-sql/language-elements/expressions-transact-sql.md) valido della categoria dei tipi di dati Integer oppure dei tipi di dati **bit**, **binary** o **varbinary**. *expression* viene considerato un numero binario per l'operazione bit per bit.  
  
> [!NOTE]  
>  In un'operazione bit per bit un solo valore *expression* può avere il tipo di dati **binary** o **varbinary**.  
  
## <a name="result-types"></a>Tipi restituiti  
 Restituisce un tipo **int** se i valori di input sono **int**, un tipo **smallint** se i valori di input sono **smallint** o un valore **tinyint** se i valori di input sono **tinyint**.  
  
## <a name="remarks"></a>Osservazioni  
 L'operatore | esegue un'operazione con OR logico bit per bit tra due espressioni, considerando tutti i bit corrispondenti in entrambe le espressioni. I bit nel risultato sono impostati su 1 se il valore di uno o di entrambi i bit (per il bit in fase di risoluzione) delle espressioni di input è uguale a 1. Se nessuno dei bit è uguale a 1, il bit del risultato è impostato su 0.  
  
 Se alle due espressioni viene applicato un tipo di dati Integer diverso, ad esempio se il tipo *expression* a sinistra è **smallint** e il tipo *expression* a destra è **int**, l'argomento del tipo di dati di livello inferiore viene convertito nel tipo di dati di livello superiore. In questo esempio **smallint**_expression_ viene convertito in **int**.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene creata una tabella con tipi di dati **int** per la visualizzazione dei valori originali. La tabella viene quindi inserita in un'unica riga.  
  
```sql  
CREATE TABLE bitwise (  
  a_int_value INT NOT NULL,  
  b_int_value INT NOT NULL);  
GO  
INSERT bitwise VALUES (170, 75);  
GO  
```  
  
 La query seguente esegue l'operazione con OR logico bit per bit nelle colonne **a_int_value** e **b_int_value**.  
  
```sql  
SELECT a_int_value | b_int_value  
FROM bitwise;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
-----------   
235           
  
(1 row(s) affected)  
```  
  
 La rappresentazione binaria di 170 (**a_int_value** o `A` nell'esempio seguente) è `0000 0000 1010 1010`. La rappresentazione binaria di 75 (**b_int_value** o `B` nell'esempio seguente) è `0000 0000 0100 1011`. L'operazione con OR logico bit per bit eseguita su questi due valori restituisce il risultato binario `0000 0000 1110 1011`, ovvero 235 decimale.  
  
```  
(A | B)  
0000 0000 1010 1010  
0000 0000 0100 1011  
-------------------  
0000 0000 1110 1011  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Operatori &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [Operatori bit per bit &#40;Transact-SQL&#41;](../../t-sql/language-elements/bitwise-operators-transact-sql.md)   
 [&#124;= &#40;Assegnazione OR bit per bit&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/bitwise-or-equals-transact-sql.md)   
 [Operatori composti &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)  
  
  


