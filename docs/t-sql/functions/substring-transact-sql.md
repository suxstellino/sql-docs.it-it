---
title: SUBSTRING (Transact-SQL) | Microsoft Docs
description: Informazioni di riferimento Transact-SQL per la funzione SUBSTRING. Questa funzione restituisce una parte di un'espressione di tipo carattere, binario, testo o immagine specificata.
ms.date: 10/21/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SUBSTRING
- SUBSTRING_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- portion of expression returned [SQL Server]
- part of expression returned [SQL Server]
- SUBSTRING function
- offsets [SQL Server]
- binary [SQL Server], returning part of
- expressions [SQL Server], part returned
- characters [SQL Server], returning part of
ms.assetid: a19c808f-aaf9-4a69-af59-b1a5fc3e5c4c
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2a220d976b1e85dc8559b34d74e8280847afccce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207959"
---
# <a name="substring-transact-sql"></a>SUBSTRING (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Restituisce parte di un'espressione di tipo carattere, binario, testo o immagine in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
SUBSTRING ( expression ,start , length )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *expression*  
 È un'[espressione](../../t-sql/language-elements/expressions-transact-sql.md) di tipo **character**, **binary**, **text**, **ntext** o **image**.  
  
 *start*  
 Valore intero o espressione **bigint** che specifica l'inizio dei caratteri restituiti. (La numerazione è in base 1, ovvero il primo carattere dell'espressione è 1). Se *start* è minore di 1, l'espressione restituita inizierà con il primo carattere specificato in *expression*. In questo caso, il numero di caratteri restituito è il valore maggiore tra la somma di *start* + *length*- 1 oppure 0. Se *start* è maggiore del numero di caratteri nell'espressione valore, viene restituita un'espressione di lunghezza zero.  
  
 *length*  
 Valore integer positivo o espressione **bigint** che specifica quanti caratteri di *expression* verranno restituiti. Se *length* è negativo, viene generato un errore e l'istruzione viene terminata. Se la somma di *start* e *length* è maggiore del numero di caratteri di *expression*, viene restituita l'intera espressione del valore che inizia con *start*.  
  
## <a name="return-types"></a>Tipi restituiti  
 Restituisce dati di tipo carattere se *expression* è un tipo di dati carattere supportato. Restituisce dati binari se *expression* è un tipo di dati **binary** supportato. Il tipo di dati della stringa restituita corrisponde a quello dell'espressione specificata, con le eccezioni descritte nella tabella seguente.  
  
|Espressione specificata|Tipo restituito|  
|--------------------------|-----------------|  
|**char**/**varchar**/**text**|**varchar**|  
|**nchar**/**nvarchar**/**ntext**|**nvarchar**|  
|**binary**/**varbinary**/**image**|**varbinary**|  
  
## <a name="remarks"></a>Osservazioni  
 I valori per *start* e *lenght* devono essere specificati nel numero di caratteri per i tipi di dati **ntext**, **char**, o **varchar**  e nei byte per i tipi di dati **text**, **image**, **binary**, o **varbinary**.  
  
 *Expression* deve essere **varchar (max)** o **varbinary (max)** quando *start* o *lenght* contiene un valore maggiore di 2.147.483.647.  
  
## <a name="supplementary-characters-surrogate-pairs"></a>Caratteri supplementari (coppie di surrogati)  
 Quando si usano le regole di confronto SC (caratteri supplementari), ciascuna coppia di surrogati in *expression* viene considerata come un singolo carattere sia da *start* sia da *length*. Per altre informazioni, vedere [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-substring-with-a-character-string"></a>R. Utilizzo della funzione SUBSTRING con una stringa di caratteri  
 In questo esempio viene illustrato come restituire solo una parte di una stringa di caratteri. Dalla tabella `sys.databases`, questa query restituisce i nomi di database di sistema nella prima colonna, la prima lettera del database nella seconda colonna e il terzo e quarto carattere nella colonna finale.  
  
```sql
SELECT name, SUBSTRING(name, 1, 1) AS Initial ,
SUBSTRING(name, 3, 2) AS ThirdAndFourthCharacters
FROM sys.databases  
WHERE database_id < 5;   
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

|name |Initial |ThirdAndFourthCharacters|
|---|--|--|
|master    |m    |st |
|tempdb    |t    |mp |
|model    |m    |de |
|msdb    |m    |db |


  
 Di seguito viene illustrato come visualizzare il secondo, il terzo e il quarto carattere della costante stringa `abcdef`.  
  
```sql
SELECT x = SUBSTRING('abcdef', 2, 3);  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
x  
----------  
bcd  
  
(1 row(s) affected)
```  
  
### <a name="b-using-substring-with-text-ntext-and-image-data"></a>B. Utilizzo della funzione SUBSTRING con dati di tipo text, ntext e image  
  
> [!NOTE]  
>  Per eseguire gli esempi seguenti è necessario installare il database **pubs**.  
  
 Nell'esempio seguente viene illustrato come restituire i primi 10 caratteri di ogni colonna di dati **testo** e **immagine** nella tabella `pub_info` del database `pubs`. I dati di tipo **testo** vengono restituiti come **varchar**, e i dati di tipo **immagine** vengono restituiti come **varbinary**.  
  
```sql
USE pubs;  
SELECT pub_id, SUBSTRING(logo, 1, 10) AS logo,   
   SUBSTRING(pr_info, 1, 10) AS pr_info  
FROM pub_info  
WHERE pub_id = '1756';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 pub_id logo    pr_info
------ ---------------------- ----------
1756   0x474946383961E3002500 This is sa

(1 row(s) affected)
```  
  
 Nell'esempio seguente vengono illustrati gli effetti della funzione SUBSTRING sui dati di tipo **testo** e **ntesto**. In primo luogo nel database `pubs` viene creata una nuova tabella denominata `npub_info`. In secondo luogo viene creata la colonna `pr_info` nella tabella `npub_info` dai primi 80 caratteri della colonna `pub_info.pr_info` e come primo carattere viene aggiunto `ü`. Infine, tramite un `INNER JOIN` vengono recuperati i numeri di identificazione di tutti i server di pubblicazione e il valore di `SUBSTRING` di entrambe le colonne di tipo **testo** e **ntesto** contenenti informazioni sui server di pubblicazione.  
  
```sql
IF EXISTS (SELECT table_name FROM INFORMATION_SCHEMA.TABLES   
      WHERE table_name = 'npub_info')  
   DROP TABLE npub_info;  
GO  
-- Create npub_info table in pubs database. Borrowed from instpubs.sql.  
USE pubs;  
GO  
CREATE TABLE npub_info  
(  
 pub_id CHAR(4) NOT NULL  
    REFERENCES publishers(pub_id)  
    CONSTRAINT UPKCL_npubinfo PRIMARY KEY CLUSTERED,  
pr_info ntext NULL  
);  
  
GO  
  
-- Fill the pr_info column in npub_info with international data.  
RAISERROR('Now at the inserts to pub_info...',0,1);  
  
GO  
  
INSERT npub_info VALUES('0736', N'üThis is sample text data for New Moon Books, publisher 0736 in the pubs database')  
,('0877', N'üThis is sample text data for Binnet & Hardley, publisher 0877 in the pubs databa')  
,('1389', N'üThis is sample text data for Algodata Infosystems, publisher 1389 in the pubs da')  
,('9952', N'üThis is sample text data for Scootney Books, publisher 9952 in the pubs database')  
,('1622', N'üThis is sample text data for Five Lakes Publishing, publisher 1622 in the pubs d')  
,('1756', N'üThis is sample text data for Ramona Publishers, publisher 1756 in the pubs datab')  
,('9901', N'üThis is sample text data for GGG&G, publisher 9901 in the pubs database. GGG&G i')  
,('9999', N'üThis is sample text data for Lucerne Publishing, publisher 9999 in the pubs data');  
GO  
-- Join between npub_info and pub_info on pub_id.  
SELECT pr.pub_id, SUBSTRING(pr.pr_info, 1, 35) AS pr_info,  
   SUBSTRING(npr.pr_info, 1, 35) AS npr_info  
FROM pub_info pr INNER JOIN npub_info npr  
   ON pr.pub_id = npr.pub_id  
ORDER BY pr.pub_id ASC;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-substring-with-a-character-string"></a>C. Utilizzo della funzione SUBSTRING con una stringa di caratteri  
 In questo esempio viene illustrato come restituire solo una parte di una stringa di caratteri. Questa query eseguita nella tabella `dbo.DimEmployee` restituisce il cognome in una colonna e l'iniziale del nome nella seconda colonna.  
  
```sql
-- Uses AdventureWorks  
  
SELECT LastName, SUBSTRING(FirstName, 1, 1) AS Initial  
FROM dbo.DimEmployee  
WHERE LastName LIKE 'Bar%'  
ORDER BY LastName;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
LastName             Initial
-------------------- -------
Barbariol            A
Barber               D
Barreto de Mattos    P
```  
  
 L'esempio seguente mostra come restituire il secondo, il terzo e il quarto carattere della costante stringa `abcdef`.  
  
```sql
USE ssawPDW;  
  
SELECT TOP 1 SUBSTRING('abcdef', 2, 3) AS x FROM dbo.DimCustomer;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
x
-----
bcd
```  
  
## <a name="see-also"></a>Vedere anche  
 [LEFT &#40;Transact-SQL&#41;](../../t-sql/functions/left-transact-sql.md)  
 [LTRIM &#40;Transact-SQL&#41;](../../t-sql/functions/ltrim-transact-sql.md)  
 [RIGHT &#40;Transact-SQL&#41;](../../t-sql/functions/right-transact-sql.md)  
 [RTRIM &#40;Transact-SQL&#41;](../../t-sql/functions/rtrim-transact-sql.md)  
 [STRING_SPLIT &#40;Transact-SQL&#41;](../../t-sql/functions/string-split-transact-sql.md)  
 [TRIM &#40;Transact-SQL&#41;](../../t-sql/functions/trim-transact-sql.md)  
 [Funzioni per i valori stringa &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  
  
  


