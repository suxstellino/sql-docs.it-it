---
description: Funzioni scalari ODBC (Transact-SQL)
title: Funzioni scalari ODBC (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- CURDATE ODBC function
- DAYOFMONTH ODBC function
- WEEK ODBC function
- functions, ODBC CONCAT
- SECOND ODBC function
- functions, ODBC CURRENT_TIME
- functions, ODBC HOUR
- functions, ODBC QUARTER
- TRUNCATE ODBC function
- functions, ODBC SECOND
- DAYOFWEEK ODBC function
- CURRENT_DATE ODBC function
- DAYNAME ODBC function
- CURTIME ODBC function
- functions, ODBC CURRENT_DATE
- MINUTE ODBC function
- functions, ODBC BIT_LENGTH
- functions, ODBC OCTET_LENGTH
- CONCAT ODBC function
- MONTHNAME ODBC function
- functions, ODBC DAYOFYEAR
- OCTET_LENGTH ODBC function
- functions [SQL Server], ODBC scalar functions
- BIT_LENGTH ODBC function
- functions, ODBC DAYOFMONTH
- CURRENT_TIME ODBC function
- functions, ODBC CURDATE
- functions, ODBC CURTIME
- functions, ODBC TRUNCATE
- functions, ODBC MONTHNAME
- functions, ODBC DAYNAME
- ODBC scalar functions
- functions, ODBC MINUTE
- functions, ODBC DAYOFWEEK
- QUARTER ODBC function
- DAYOFYEAR ODBC function
- functions, ODBC WEEK
- HOUR ODBC function
ms.assetid: a0df1ac2-6699-4ac0-8f79-f362f23496f1
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0fbc4c09e7d00c8836f7c3efd17dbfe660cd669a
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597121"
---
# <a name="odbc-scalar-functions-transact-sql"></a>Funzioni scalari ODBC (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  È possibile usare le [funzioni scalari ODBC](../../odbc/reference/appendixes/appendix-e-scalar-functions.md) all'interno di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)]. Queste istruzioni sono interpretate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ed è possibile utilizzarle in stored procedure e funzioni definite dall'utente. Queste includono funzioni per i valori stringa, numerici, di ora, di data, di intervallo e di sistema.  
  
## <a name="usage"></a>Utilizzo  
 ```syntaxsql
 SELECT {fn <function_name> [ (<argument>,....n) ] }
 ```
  
## <a name="functions"></a>Funzioni  
 Nelle tabelle seguenti sono elencate le funzioni scalari ODBC non duplicate in [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
### <a name="string-functions"></a>Funzioni di stringa  
  
|Funzione|Descrizione|  
|--------------|-----------------|  
|BIT_LENGTH(string_exp) (ODBC 3.0)|Restituisce la lunghezza in bit dell'espressione stringa.<br /><br /> Restituisce le dimensioni interne del tipo di dati specificato, senza convertire string_exp in stringa.|  
|CONCAT( string_exp1,string_exp2) (ODBC 1.0)|Restituisce la stringa di caratteri che risulta dalla concatenazione di string_exp2 e string_exp1. La stringa risultante dipende da DBMS. Ad esempio, se la colonna rappresentata string_exp1 contiene un valore NULL, DB2 restituisce NULL ma [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce una stringa non Null.|  
|OCTET_LENGTH( string_exp ) (ODBC 3.0)|Restituisce la lunghezza in byte dell'espressione stringa. Il risultato è il più piccolo numero integer non inferiore al numero di bit diviso per 8.<br /><br /> Restituisce le dimensioni interne del tipo di dati specificato, senza convertire string_exp in stringa.|  
  
### <a name="numeric-function"></a>Funzione numerica  
  
|Funzione|Descrizione|  
|--------------|-----------------|  
|TRUNCATE( numeric_exp, integer_exp) (ODBC 2.0)|Restituisce numeric_exp troncato in corrispondenza di un numero di posizioni pari a integer_exp a destra del separatore decimale. Se integer_exp è negativo, numeric_exp viene troncato in corrispondenza di un numero di posizioni pari a &#124;integer_exp&#124; a sinistra del separatore decimale.|  
  
### <a name="time-date-and-interval-functions"></a>Funzioni di data, ora e intervallo  
  
|Funzione|Descrizione|  
|--------------|-----------------|  
|CURRENT_DATE( ) (ODBC 3.0)|Restituisce la data corrente.|  
|CURDATE( ) (ODBC 3.0)|Restituisce la data corrente.|  
|CURRENT_TIME`[( time-precision )]` (ODBC 3.0)|Restituisce l'ora locale corrente. L'argomento time-precision determina la precisione a livello di secondi del valore restituito.|  
|CURTIME() (ODBC 3.0)|Restituisce l'ora locale corrente.|  
|DAYNAME( date_exp ) (ODBC 2.0)|Restituisce una stringa di caratteri che contiene il nome specifico dell'origine dati del giorno per la parte del giorno di date_exp. Ad esempio, il nome è da Sunday a Saturday o da Sun a Sat per un'origine dati che usa l'inglese. Il nome è da Sonntag a Samstag per un'origine dati che usa il tedesco.|
|DAYOFMONTH( date_exp ) (ODBC 1.0)|Restituisce il giorno del mese in base al campo del mese in date_exp come valore intero. Il valore restituito è compreso nell'intervallo da 1 a 31.|  
|DAYOFWEEK( date_exp ) (ODBC 1.0)|Restituisce il giorno della settimana in base al campo della settimana in date_exp come valore intero. Il valore restituito è compreso nell'intervallo da 1 a 7, dove 1 corrisponde a domenica.|  
|HOUR( time_exp ) (ODBC 1.0)|Restituisce l'ora in base al campo dell'ora in time_exp come valore intero nell'intervallo da 0 a 23.|  
|MINUTE( time_exp ) (ODBC 1.0)|Restituisce i minuti in base al campo dei minuti in time_exp come valore intero nell'intervallo da 0 a 59.|  
|SECOND( time_exp ) (ODBC 1.0)|Restituisce i secondi in base al campo dei secondi in time_exp come valore intero nell'intervallo da 0 a 59.|  
|MONTHNAME( date_exp ) (ODBC 2.0)|Restituisce una stringa di caratteri che contiene il nome specifico dell'origine dati del mese per la parte del mese di date_exp. Ad esempio, il nome è da January a December o da Jan a Dec per un'origine dati che usa l'inglese. Il nome è da Januar a Dezember per un'origine dati che usa il tedesco.|  
|QUARTER( date_exp ) (ODBC 1.0)|Restituisce il trimestre in date_exp come valore intero nell'intervallo da 1 a 4, dove 1 rappresenta il periodo dal 1º gennaio al 31 marzo.|  
|WEEK( date_exp ) (ODBC 1.0)|Restituisce la settimana dell'anno in base al campo della settimana in date_exp come valore intero nell'intervallo da 1 a 53.|  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-an-odbc-function-in-a-stored-procedure"></a>R. Utilizzo di una funzione ODBC in una stored procedure  
 Nell'esempio seguente viene utilizzata una funzione ODBC in una stored procedure:  
  

```sql 
CREATE PROCEDURE dbo.ODBCprocedure  
(  
    @string_exp NVARCHAR(4000)  
)  
AS  
SELECT {fn OCTET_LENGTH( @string_exp )};  
```  
  
### <a name="b-using-an-odbc-function-in-a-user-defined-function"></a>B. Utilizzo di una funzione ODBC in una funzione definita dall'utente  
 Nell'esempio seguente viene utilizzata una funzione ODBC in una funzione definita dall'utente:  
  
```sql  
CREATE FUNCTION dbo.ODBCudf  
(  
    @string_exp NVARCHAR(4000)  
)  
RETURNS INT  
AS  
BEGIN  
DECLARE @len INT  
SET @len = (SELECT {fn OCTET_LENGTH( @string_exp )})  
RETURN(@len)  
END ;  
  
SELECT dbo.ODBCudf('Returns the length.');  
--Returns 38  
```  
  
### <a name="c-using-an-odbc-functions-in-select-statements"></a>C. Utilizzo di funzioni ODBC nelle istruzioni SELECT  
 Nella seguente istruzione SELECT vengono utilizzate funzioni ODBC:  
  
```sql 
DECLARE @string_exp NVARCHAR(4000) = 'Returns the length.';  
SELECT {fn BIT_LENGTH( @string_exp )};  
-- Returns 304  
SELECT {fn OCTET_LENGTH( @string_exp )};  
-- Returns 38  
  
SELECT {fn CONCAT( 'CONCAT ','returns a character string')};  
-- Returns CONCAT returns a character string  
SELECT {fn TRUNCATE( 100.123456, 4)};  
-- Returns 100.123400  
SELECT {fn CURRENT_DATE( )};  
-- Returns 2007-04-20  
SELECT {fn CURRENT_TIME(6)};  
-- Returns 10:27:11.973000  
  
DECLARE @date_exp NVARCHAR(30) = '2007-04-21 01:01:01.1234567';  
SELECT {fn DAYNAME( @date_exp )};  
-- Returns Saturday  
SELECT {fn DAYOFMONTH( @date_exp )};  
-- Returns 21  
SELECT {fn DAYOFWEEK( @date_exp )};  
-- Returns 7  
SELECT {fn HOUR( @date_exp)};  
-- Returns 1   
SELECT {fn MINUTE( @date_exp )};  
-- Returns 1  
SELECT {fn SECOND( @date_exp )};  
-- Returns 1  
SELECT {fn MONTHNAME( @date_exp )};  
-- Returns April  
SELECT {fn QUARTER( @date_exp )};  
-- Returns 2  
SELECT {fn WEEK( @date_exp )};  
-- Returns 16  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-using-an-odbc-function-in-a-stored-procedure"></a>D. Utilizzo di una funzione ODBC in una stored procedure  
 Nell'esempio seguente viene utilizzata una funzione ODBC in una stored procedure:  
  
```sql  
CREATE PROCEDURE dbo.ODBCprocedure  
(  
    @string_exp NVARCHAR(4000)  
)  
AS  
SELECT {fn BIT_LENGTH( @string_exp )};  
```  
  
### <a name="e-using-an-odbc-function-in-a-user-defined-function"></a>E. Utilizzo di una funzione ODBC in una funzione definita dall'utente  
 Nell'esempio seguente viene utilizzata una funzione ODBC in una funzione definita dall'utente:  
  
```sql  
CREATE FUNCTION dbo.ODBCudf  
(  
    @string_exp NVARCHAR(4000)  
)  
RETURNS INT  
AS  
BEGIN  
DECLARE @len INT  
SET @len = (SELECT {fn BIT_LENGTH( @string_exp )})  
RETURN(@len)  
END ;  
  
SELECT dbo.ODBCudf('Returns the length in bits.');  
--Returns 432  
```  
  
### <a name="f-using-an-odbc-functions-in-select-statements"></a>F. Utilizzo di funzioni ODBC nelle istruzioni SELECT  
 Nella seguente istruzione SELECT vengono utilizzate funzioni ODBC:  
  
```sql  
DECLARE @string_exp NVARCHAR(4000) = 'Returns the length.';  
SELECT {fn BIT_LENGTH( @string_exp )};  
-- Returns 304  
  
SELECT {fn CONCAT( 'CONCAT ','returns a character string')};  
-- Returns CONCAT returns a character string  
SELECT {fn CURRENT_DATE( )};  
-- Returns today's date  
SELECT {fn CURRENT_TIME(6)};  
-- Returns the time  
  
DECLARE @date_exp NVARCHAR(30) = '2007-04-21 01:01:01.1234567';  
SELECT {fn DAYNAME( @date_exp )};  
-- Returns Saturday  
SELECT {fn DAYOFMONTH( @date_exp )};  
-- Returns 21  
SELECT {fn DAYOFWEEK( @date_exp )};  
-- Returns 7  
SELECT {fn HOUR( @date_exp)};  
-- Returns 1   
SELECT {fn MINUTE( @date_exp )};  
-- Returns 1  
SELECT {fn SECOND( @date_exp )};  
-- Returns 1  
SELECT {fn MONTHNAME( @date_exp )};  
-- Returns April  
SELECT {fn QUARTER( @date_exp )};  
-- Returns 2  
SELECT {fn WEEK( @date_exp )};  
-- Returns 16  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni predefinite &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)