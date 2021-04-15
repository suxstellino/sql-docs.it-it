---
description: Funzioni logiche - GREATEST (Transact-SQL)
title: GREATEST (Transact-SQL)
ms.custom: ''
ms.date: 04/14/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GREATEST
- GREATEST_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- GREATEST function
author: jmsteen
ms.author: josteen
ms.reviewer: wiassaf
ms.openlocfilehash: 37f7ce71fc6ea9aedd2b8d92e9a19c6f844a02d6
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492463"
---
# <a name="logical-functions---greatest-transact-sql"></a>Funzioni logiche - GREATEST (Transact-SQL)
[!INCLUDE [asdb-asdbmi](../../includes/applies-to-version/asdb-asdbmi.md)]

 Questa funzione restituisce il valore massimo da un elenco di una o più espressioni. 
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
GREATEST ( expression1 [ ,...expressionN ] ) 
```  

## <a name="arguments"></a>Argomenti
 *expression1, expressionN*  
 Elenco di espressioni delimitati da virgole di qualsiasi tipo di dati confrontabile. La  `GREATEST`   funzione richiede almeno un argomento e non supporta più di 254 argomenti.  
 
 Ogni espressione può essere una costante, una variabile, un nome di colonna o una funzione e qualsiasi combinazione di operatori aritmetici, bit per bit e stringa. Sono consentite funzioni di aggregazione e sottoquery scalari.  
  
## <a name="return-types"></a>Tipi restituiti  
 Restituisce il tipo di dati con precedenza maggiore nel set di tipi passato alla funzione. Per altre informazioni, vedere [Precedenza dei tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md).  

 Se tutti gli argomenti hanno lo stesso tipo di dati e il tipo è supportato per il confronto,  `GREATEST` restituirà tale tipo. 

 In caso contrario, la funzione convertirà in modo implicito tutti gli argomenti nel tipo di dati con la precedenza più alta prima del confronto e userà questo tipo come tipo restituito. 

 Per i tipi numerici, la scala del tipo restituito sarà la stessa dell'argomento con precedenza più alta o la scala più grande se più di un argomento è del tipo di dati con precedenza più elevata.
  
## <a name="remarks"></a>Commenti  
 Tutte le espressioni nell'elenco di argomenti devono essere di un tipo di dati confrontabile e che può essere convertito in modo implicito nel tipo di dati dell'argomento con la precedenza più elevata. 

 La conversione implicita di tutti gli argomenti nel tipo di dati con precedenza più elevata viene eseguita prima del confronto. 

 Se la conversione implicita dei tipi tra gli argomenti non è supportata, la funzione avrà esito negativo e restituirà un errore. 

 Per altre informazioni sulla conversione implicita ed esplicita, vedere Conversione di tipi [di dati &#40;motore di database&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md). 

 Se uno o più argomenti non sono `NULL` , gli argomenti verranno `NULL` ignorati durante il confronto. Se tutti gli argomenti `NULL` sono , `GREATEST` restituirà `NULL` . 

 Il confronto degli argomenti di tipo carattere segue le regole di precedenza delle [regole di &#40;transact-SQL&#41;](../../t-sql/statements/collation-precedence-transact-sql.md). 

 I tipi  seguenti non sono supportati per il confronto in `GREATEST` : **varchar(max), varbinary(max) o nvarchar(max) superiore a 8.000 byte, cursor, geometry, geography, image, tipi definiti** dall'utente non ordinati in byte, ntext, table, text e **xml**. 

 I tipi di dati varchar(max), varbinary(max) e nvarchar(max) sono supportati rispettivamente per gli argomenti di 8.000 byte o inferiori e verranno convertiti in modo implicito in varchar(n), varbinary(n) e nvarchar(n), rispettivamente, prima del confronto. 

 Ad esempio, varchar(max) può supportare fino a 8.000 caratteri se si usa un set di caratteri di codifica a byte singolo e nvarchar(max) può supportare fino a 4.000 coppie di byte (presupponendo la codifica di caratteri UTF-16). 
  
## <a name="examples"></a>Esempi  

### <a name="a-simple-example"></a>R. Esempio semplice

 Nell'esempio seguente viene restituito il valore massimo dall'elenco di costanti fornito. 

 La scala del tipo restituito è determinata dalla scala dell'argomento con il tipo di dati con precedenza più elevata. 
 
```sql 
SELECT GREATEST ( '6.62', 3.1415, N'7' ) AS GreatestVal; 
GO 
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
GreatestVal 
-------- 
  7.0000 

(1 rows affected)  
```  

### <a name="b-simple-example-with-character-types"></a>B. Esempio semplice con tipi di carattere

 Nell'esempio seguente viene restituito il valore massimo dall'elenco di costanti carattere fornito.  
  
```sql  
SELECT GREATEST ('Glacier', N'Joshua Tree', 'Mount Rainier') AS GreatestString;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
GreatestString 
------------- 
Mount Rainier 

(1 rows affected)  
```  

### <a name="c-simple-example-with-table"></a>C. Esempio semplice con la tabella
  
 Questo esempio restituisce il valore massimo da un elenco di argomenti di colonna e ignora `NULL` i valori durante il confronto. 
  
```sql  
USE AdventureWorks2019; 
GO 

SELECT sp.SalesQuota, sp.SalesYTD, sp.SalesLastYear 
      , GREATEST(sp.SalesQuota, sp.SalesYTD, sp.SalesLastYear) AS Sales 
FROM Sales.SalesPerson AS sp 
WHERE sp.SalesYTD < 3000000; 
GO  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
SalesQuota            SalesYTD              SalesLastYear         Sales 

--------------------- --------------------- --------------------- --------------------- 
                 NULL           559697.5639                 .0000           559697.5639 
          250000.0000          1453719.4653          1620276.8966          1620276.8966 
          300000.0000          2315185.6110          1849640.9418          2315185.6110 
          250000.0000          1352577.1325          1927059.1780          1927059.1780 
          250000.0000          2458535.6169          2073505.9999          2458535.6169 
          250000.0000          2604540.7172          2038234.6549          2604540.7172 
          250000.0000          1573012.9383          1371635.3158          1573012.9383 
          300000.0000          1576562.1966                 .0000          1576562.1966 
                 NULL           172524.4512                 .0000           172524.4512 
          250000.0000          1421810.9242          2278548.9776          2278548.9776 
                 NULL           519905.9320                 .0000           519905.9320 
          250000.0000          1827066.7118          1307949.7917          1827066.7118 

(12 rows affected)
  
```  
### <a name="d-using-greatest-with-local-variables"></a>D. Uso `GREATEST` con variabili locali

 In questo esempio `GREATEST` viene utilizzato per determinare il valore massimo di un elenco di variabili locali all'interno del predicato di una `WHERE` clausola . 
  
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
WHERE Correlation > GREATEST(@PredictionA, @PredictionB); 
GO 
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
VarX   Correlation 
---------- ----------- 
Var2              .825 

(1 rows affected)  
```  

### <a name="e-using-greatest-with-columns-constants-and-variables"></a>E. Uso `GREATEST` di con colonne, costanti e variabili

 In questo esempio `GREATEST` viene utilizzato per determinare il valore massimo di un elenco che include colonne, costanti e variabili. 
  
```sql  
CREATE TABLE dbo.products (    
    prod_id int IDENTITY(1,1),    
    listprice smallmoney NULL 
); 

INSERT INTO dbo.products VALUES (14.99), (49.99), (24.99); 
GO 

DECLARE @PriceX smallmoney = 19.99;  

SELECT GREATEST(listprice, 0, @PriceX) as GreatestPrice  
FROM dbo.products;
GO 
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
GreatestPrice
-------------
      19.9900
      49.9900
      24.9900

(3 rows affected)  
```  

  
## <a name="see-also"></a>Vedi anche  
 [LEAST &#40;Transact-SQL&#41;](../../t-sql/functions/logical-functions-least-transact-sql.md)  
 [MAX &#40;Transact-SQL&#41;](../../t-sql/functions/max-transact-sql.md)  
 [MIN &#40;Transact-SQL&#41;](../../t-sql/functions/min-transact-sql.md)  
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)  
 [CHOOSE &#40;Transact-SQL&#41;](../../t-sql/functions/logical-functions-choose-transact-sql.md)  
  
  
