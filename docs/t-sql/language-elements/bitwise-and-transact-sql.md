---
description: '&amp; (AND bit per bit) (Transact-SQL)'
title: '&amp; (AND bit per bit) (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 01/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- bitwise
- '&'
- '&_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- AND, bitwise AND
- '& (bitwise AND)'
- bitwise AND (&)
ms.assetid: 20275755-4fa7-47b1-a9be-ac85606d63b0
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9fced4f3698e07950c20ef1fa16d0e70e4040348
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104733671"
---
# <a name="amp-bitwise-and-transact-sql"></a>&amp; (AND bit per bit) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Esegue un'operazione con AND logico bit per bit tra due valori integer.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
expression & expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *expression*  
 Qualsiasi [espressione](../../t-sql/language-elements/expressions-transact-sql.md) valida di qualsiasi tipo di dati della categoria Integer o del tipo di dati **bit**, **binary** o **varbinary**. *expression* viene considerato un numero binario per l'operazione bit per bit.  
  
> [!NOTE]  
>  In un'operazione bit per bit un solo valore *expression* può avere il tipo di dati **binary** o **varbinary**.  
  
## <a name="result-types"></a>Tipi restituiti  
 **int** se i valori di input sono **int**.  
  
 **smallint** se i valori di input sono **smallint**.  
  
 **tinyint** se i valori di input sono **tinyint** o **bit**.  
  
## <a name="remarks"></a>Osservazioni  
 L'operatore **&** bit per bit esegue un'operazione con AND logico bit per bit tra due espressioni, considerando tutti i bit corrispondenti in entrambe le espressioni. I bit del risultato vengono impostati su 1 se, e solo se, il valore del bit in fase di risoluzione di entrambe le espressioni di input è uguale a 1. In caso contrario, il bit del risultato viene impostato su 0.  
  
 Se alle due espressioni viene applicato un tipo di dati Integer diverso, ad esempio se il tipo *expression* a sinistra è **smallint** e il tipo *expression* a destra è **int**, l'argomento del tipo di dati di livello inferiore viene convertito nel tipo di dati di livello superiore. In questo caso il tipo _expression_ **smallint** viene convertito in **int**.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene creata una tabella con il tipo di dati **int** per l'archiviazione dei valori e i due valori vengono inseriti in un'unica riga.  
  
```sql
CREATE TABLE bitwise (   
  a_int_value INT NOT NULL,  
  b_int_value INT NOT NULL);  
GO  
INSERT bitwise VALUES (170, 75);  
GO  
```  
  
 La query esegue l'operazione con AND bit per bit tra le colonne `a_int_value` e `b_int_value`.  
  
```sql  
SELECT a_int_value & b_int_value  
FROM bitwise;  
GO  
```  
  
 Set di risultati:  
  
```  
-----------   
10            
  
(1 row(s) affected)  
```  
  
 La rappresentazione binaria di 170 (`a_int_value` o `A`) è `0000 0000 1010 1010`. La rappresentazione binaria di 75 (`b_int_value` o `B`) è `0000 0000 0100 1011`. L'esecuzione dell'operazione con AND bit per bit su questi valori genera il risultato binario `0000 0000 0000 1010`, corrispondente al valore decimale 10.  
  
```  
(A & B)  
0000 0000 1010 1010  
0000 0000 0100 1011  
-------------------  
0000 0000 0000 1010  
```  
  
  
## <a name="see-also"></a>Vedere anche  
 [Espressioni &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Operatori &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [Operatori bit per bit &#40;Transact-SQL&#41;](../../t-sql/language-elements/bitwise-operators-transact-sql.md)   
 [&= &#40;assegnazione AND bit per bit&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/bitwise-and-equals-transact-sql.md)   
 [Operatori composti &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)  
  
  


