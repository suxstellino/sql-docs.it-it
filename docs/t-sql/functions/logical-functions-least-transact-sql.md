---
description: Funzioni logiche - LEAST (Transact-SQL)
title: LEAST (Transact-SQL)
ms.custom: ''
ms.date: 04/14/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- LEAST
- LEAST_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- LEAST function
author: jmsteen
ms.author: josteen
ms.reviewer: wiassaf
ms.openlocfilehash: ec8ac611a7d2903a47dc9a800843c5f6f97b6ec8
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492445"
---
# <a name="logical-functions---least-transact-sql"></a>Funzioni logiche - LEAST (Transact-SQL)
[!INCLUDE [asdb-asdbmi](../../includes/applies-to-version/asdb-asdbmi.md)]

 Questa funzione restituisce il valore minimo da un elenco di una o più espressioni. 

 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
LEAST ( expression1 [ ,...expressionN ] ) 
```  

## <a name="arguments"></a>Argomenti
 *expression1, expressionN*  
 Elenco di espressioni delimitati da virgole di qualsiasi tipo di dati confrontabile. La  `LEAST`   funzione richiede almeno un argomento e non supporta più di 254 argomenti.  
 
 Ogni espressione può essere una costante, una variabile, un nome di colonna o una funzione e qualsiasi combinazione di operatori aritmetici, bit per bit e stringa. Sono consentite funzioni di aggregazione e sottoquery scalari.  
  
## <a name="return-types"></a>Tipi restituiti  
 Restituisce il tipo di dati con precedenza maggiore nel set di tipi passato alla funzione. Per altre informazioni, vedere [Precedenza dei tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md).  

 Se tutti gli argomenti hanno lo stesso tipo di dati e il tipo è supportato per il confronto,  `LEAST` restituirà tale tipo. 

 In caso contrario, la funzione convertirà in modo implicito tutti gli argomenti nel tipo di dati con la precedenza più alta prima del confronto e userà questo tipo come tipo restituito. 

 Per i tipi numerici, la scala del tipo restituito sarà la stessa dell'argomento con precedenza più alta o la scala più grande se più di un argomento è del tipo di dati con precedenza più elevata.
  
## <a name="remarks"></a>Commenti  
 Tutte le espressioni nell'elenco di argomenti devono essere di un tipo di dati confrontabile e che può essere convertito in modo implicito nel tipo di dati dell'argomento con la precedenza più elevata. 

 La conversione implicita di tutti gli argomenti nel tipo di dati con precedenza più elevata viene eseguita prima del confronto. 

 Se la conversione implicita dei tipi tra gli argomenti non è supportata, la funzione avrà esito negativo e restituirà un errore. 

 Per altre informazioni sulla conversione implicita ed esplicita, vedere Conversione di tipi [di dati &#40;motore di database&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md). 

 Se uno o più argomenti non sono `NULL` , gli argomenti verranno `NULL` ignorati durante il confronto. Se tutti gli argomenti `NULL` sono , `LEAST` restituirà `NULL` . 

 Il confronto degli argomenti di tipo carattere segue le regole di precedenza delle [regole &#40;Transact-SQL&#41;](../../t-sql/statements/collation-precedence-transact-sql.md). 

 I tipi  seguenti non sono supportati per il confronto in `LEAST` : **varchar(max), varbinary(max) o nvarchar(max) superiore a 8.000 byte, cursor, geometry, geography, image, tipi definiti** dall'utente non ordinati in byte, ntext, table, text e **xml**. 

 I tipi di dati varchar(max), varbinary(max) e nvarchar(max) sono supportati rispettivamente per gli argomenti di 8.000 byte o inferiori e verranno convertiti in modo implicito in varchar(n), varbinary(n) e nvarchar(n), rispettivamente, prima del confronto. 

 Ad esempio, varchar(max) può supportare fino a 8.000 caratteri se si usa un set di caratteri di codifica a byte singolo e nvarchar(max) può supportare fino a 4.000 coppie di byte (presupponendo la codifica di caratteri UTF-16). 
  
## <a name="examples"></a>Esempi  

### <a name="a-simple-example"></a>R. Esempio semplice

 Nell'esempio seguente viene restituito il valore minimo dall'elenco di costanti specificato. 

 La scala del tipo restituito è determinata dalla scala dell'argomento con il tipo di dati con precedenza più elevata. 
 
```sql 
SELECT LEAST ( '6.62', 3.1415, N'7' ) AS LeastVal; 
GO 
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
LeastVal 
------- 
 3.1415 

(1 rows affected)  
```  

### <a name="b-simple-example-with-character-types"></a>B. Esempio semplice con tipi di carattere

 Nell'esempio seguente viene restituito il valore minimo dall'elenco di costanti carattere fornito.  
  
```sql  
SELECT LEAST ('Glacier', N'Joshua Tree', 'Mount Rainier') AS LeastString;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
LeastString 
------------- 
Glacier 

(1 rows affected)  
```  

### <a name="c-simple-example-with-table"></a>C. Esempio semplice con la tabella
  
 Questo esempio restituisce il valore minimo da un elenco di argomenti di colonna e ignora `NULL` i valori durante il confronto. 
  
```sql  
USE AdventureWorks2019; 
GO 

SELECT sp.SalesQuota, sp.SalesYTD, sp.SalesLastYear 
      , LEAST(sp.SalesQuota, sp.SalesYTD, sp.SalesLastYear) AS Sales 
FROM Sales.SalesPerson AS sp 
WHERE sp.SalesYTD < 3000000; 
GO  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
SalesQuota            SalesYTD              SalesLastYear         Sales 
--------------------- --------------------- --------------------- --------------------- 
                 NULL           559697.5639                 .0000                 .0000 
          250000.0000          1453719.4653          1620276.8966           250000.0000 
          300000.0000          2315185.6110          1849640.9418           300000.0000 
          250000.0000          1352577.1325          1927059.1780           250000.0000 
          250000.0000          2458535.6169          2073505.9999           250000.0000 
          250000.0000          2604540.7172          2038234.6549           250000.0000 
          250000.0000          1573012.9383          1371635.3158           250000.0000 
          300000.0000          1576562.1966                 .0000                 .0000 
                 NULL           172524.4512                 .0000                 .0000 
          250000.0000          1421810.9242          2278548.9776           250000.0000 
                 NULL           519905.9320                 .0000                 .0000 
          250000.0000          1827066.7118          1307949.7917           250000.0000 

(12 rows affected) 
  
```  
### <a name="d-using-least-with-local-variables"></a>D. Uso `LEAST` con variabili locali

 In questo esempio `LEAST` viene utilizzato per determinare il valore minimo di un elenco di variabili locali all'interno del predicato di una `WHERE` clausola . 
  
```sql  
CREATE TABLE dbo.studies (    
    VarX varchar(10) NOT NULL,    
    Correlation decimal(4, 3) NULL 
); 

INSERT INTO dbo.studies VALUES ('Var1', 0.2), ('Var2', 0.825), ('Var3', 0.61); 
GO 

DECLARE @PredictionA DECIMAL(2,1) = 0.7;  
DECLARE @PredictionB DECIMAL(3,1) = 0.65;  

SELECT VarX, Correlation  
FROM dbo.studies 
WHERE Correlation < LEAST(@PredictionA, @PredictionB); 
GO 
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
VarX   Correlation 
---------- ----------- 
Var1              .200 
Var3              .610 

(2 rows affected)
```  

### <a name="e-using-least-with-columns-constants-and-variables"></a>E. Uso `LEAST` di con colonne, costanti e variabili

 In questo esempio `LEAST` viene utilizzato per determinare il valore minimo di un elenco che include colonne, costanti e variabili. 
  
```sql  
CREATE TABLE dbo.products (    
    prod_id INT IDENTITY(1,1),    
    listprice smallmoney NULL 
); 

INSERT INTO dbo.products VALUES (14.99), (49.99), (24.99); 
GO 

DECLARE @PriceX smallmoney = 19.99;  

SELECT LEAST(listprice, 40, @PriceX) as LeastPrice  
FROM dbo.products;
GO 
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
LeastPrice
------------
     14.9900
     19.9900
     19.9900

(3 rows affected) 
```  

  
## <a name="see-also"></a>Vedi anche  
 [GREATEST &#40;Transact-SQL&#41;](../../t-sql/functions/logical-functions-greatest-transact-sql.md)  
 [MAX &#40;Transact-SQL&#41;](../../t-sql/functions/max-transact-sql.md)  
 [MIN &#40;Transact-SQL&#41;](../../t-sql/functions/min-transact-sql.md)  
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)  
 [CHOOSE &#40;Transact-SQL&#41;](../../t-sql/functions/logical-functions-choose-transact-sql.md)  
  
  
