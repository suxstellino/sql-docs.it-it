---
description: Funzioni per i valori text e image - TEXTPTR (Transact-SQL)
title: TEXTPTR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/23/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TEXTPTR_TSQL
- TEXTPTR
dev_langs:
- TSQL
helpviewer_keywords:
- TEXTPTR function
- viewing text pointer values
- text-pointer values
- displaying text pointer values
ms.assetid: 2672b8cb-f747-46f3-9358-9b49b3583b8e
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 39e1c646755c4ea2907b328938cac0f30f6f47d9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181445"
---
# <a name="text-and-image-functions---textptr-transact-sql"></a>Funzioni per i valori text e image - TEXTPTR (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce il valore del puntatore di testo corrispondente a una colonna di tipo **text**, **ntext** o **image** in formato **varbinary**. È possibile utilizzare il valore del puntatore di testo recuperato nelle istruzioni READTEXT, WRITETEXT e UPDATETEXT.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Non è disponibile una funzionalità alternativa.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
TEXTPTR ( column )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *column*  
 Colonna **text**, **ntext** o **image** che verrà usata.  
  
## <a name="return-types"></a>Tipi restituiti  
 **varbinary**  
  
## <a name="remarks"></a>Osservazioni  
 Per le tabelle contenenti testo nelle righe, l'istruzione TEXTPTR restituisce un handle per il testo da elaborare. È possibile ottenere un puntatore di testo valido anche se il valore di testo è Null.  
  
 Non è possibile utilizzare la funzione TEXTPTR sulle colonne di viste, ma solo sulle colonne di tabelle. Per usare la funzione TEXTPTR su una colonna di una vista, è necessario impostare il livello di compatibilità su 80 tramite il [livello di compatibilità di ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md). Se una tabella non contiene testo all'interno di righe e se una colonna di tipo **text**, **ntext** o **image** non è stata inizializzata tramite un'istruzione UPDATETEXT, TEXTPTR restituisce un puntatore Null.  
  
 Utilizzare TEXTVALID per verificare la presenza di un puntatore di testo. Non è possibile utilizzare UPDATETEXT, WRITETEXT o READTEXT senza un puntatore di testo valido.  
  
 Queste funzioni e istruzioni sono utili anche quando si usano dati di tipo **text**, **ntext** e **image**.  
  
|Funzione o istruzione|Descrizione|  
|---------------------------|-----------------|  
|PATINDEX <b>('</b> _%pattern%_ **' ,** _expression_ **)**|Restituisce la posizione dei caratteri di una determinata stringa di caratteri nelle colonne di tipo **text** o **ntext**.|  
|DATALENGTH <b>(</b>_expression_ **)**|Restituisce la lunghezza dei dati nelle colonne **text**, **ntext** e **image**.|  
|SET TEXTSIZE|Restituisce il limite in byte dei dati di tipo **text**, **ntext** o **image** da restituire con un'istruzione SELECT.|  
|SUBSTRING <b>(</b>_text_column_, _start_, _length_ **)**|Restituisce una stringa **varchar** determinata dall'offset *start* specificato e da *length*. La lunghezza massima è di 8 KB.|  
  
## <a name="examples"></a>Esempi  
  
> [!NOTE]  
>  Per eseguire gli esempi seguenti è necessario installare il database **pubs**.  
  
### <a name="a-using-textptr"></a>R. Utilizzo di TEXTPTR  
 Nell'esempio seguente viene usata la funzione `TEXTPTR` per trovare la colonna **image**`logo` associata a `New Moon Books` nella tabella `pub_info` del database `pubs`. Il puntatore di testo è inserito in una variabile locale `@ptrval.`  
  
```sql
USE pubs;  
GO  
DECLARE @ptrval VARBINARY(16);  
SELECT @ptrval = TEXTPTR(logo)  
FROM pub_info pr, publishers p  
WHERE p.pub_id = pr.pub_id   
   AND p.pub_name = 'New Moon Books';  
GO  
```  
  
### <a name="b-using-textptr-with-in-row-text"></a>B. Utilizzo di TEXTPTR con testo all'interno di righe  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] il puntatore di testo all'interno di righe deve essere utilizzato all'interno di una transazione, come illustrato nell'esempio seguente.  
  
```sql
CREATE TABLE t1 (c1 INT, c2 TEXT);  
EXEC sp_tableoption 't1', 'text in row', 'on';  
INSERT t1 VALUES ('1', 'This is text.');  
GO  
BEGIN TRAN;  
   DECLARE @ptrval VARBINARY(16);  
   SELECT @ptrval = TEXTPTR(c2)  
   FROM t1  
   WHERE c1 = 1;  
   READTEXT t1.c2 @ptrval 0 1;  
COMMIT;  
```  
  
### <a name="c-returning-text-data"></a>C. Restituzione di dati in formato testo  
 Nell'esempio seguente la colonna `pub_id` e il puntatore di testo a 16 byte della colonna `pr_info` vengono selezionati nella tabella `pub_info`.  
  
```sql
USE pubs;  
GO  
SELECT pub_id, TEXTPTR(pr_info)  
FROM pub_info  
ORDER BY pub_id;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
pub_id                                      
------ ----------------------------------   
0736   0x6c0000000000feffb801000001000100   
0877   0x6d0000000000feffb801000001000300   
1389   0x6e0000000000feffb801000001000500   
1622   0x700000000000feffb801000001000900   
1756   0x710000000000feffb801000001000b00   
9901   0x720000000000feffb801000001000d00   
9952   0x6f0000000000feffb801000001000700   
9999   0x730000000000feffb801000001000f00   
  
(8 row(s) affected)  
```  
  
 Nell'esempio seguente viene illustrato come restituire i primi `8000` byte di testo senza utilizzare TEXTPTR.  
  
```sql
USE pubs;  
GO  
SET TEXTSIZE 8000;  
SELECT pub_id, pr_info  
FROM pub_info  
ORDER BY pub_id;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
pub_id pr_info                                                                                                                                                                                                                                                           
------ -----------------------------------------------------------------  
0736   New Moon Books (NMB) has just released another top ten publication. With the latest publication this makes NMB the hottest new publisher of the year!                                                                                                             
0877   This is sample text data for Binnet & Hardley, publisher 0877 in the pubs database. Binnet & Hardley is located in Washington, D.C.  
  
This is sample text data for Binnet & Hardley, publisher 0877 in the pubs database. Binnet & Hardley is located in Washi   
1389   This is sample text data for Algodata Infosystems, publisher 1389 in the pubs database. Algodata Infosystems is located in Berkeley, California.  
  
9999   This is sample text data for Lucerne Publishing, publisher 9999 in the pubs database. Lucerne publishing is located in Paris, France.  
  
This is sample text data for Lucerne Publishing, publisher 9999 in the pubs database. Lucerne publishing is located in   
  
(8 row(s) affected)  
```  
  
### <a name="d-returning-specific-text-data"></a>D. Restituzione di dati specifici in formato testo  
 Nell'esempio seguente viene trovata la colonna `text` (`pr_info`) associata a `pub_id``0736` nella tabella `pub_info` del database `pubs`. Viene innanzitutto dichiarata la variabile locale `@val`. Il puntatore di testo, ovvero una stringa di dati binari di tipo Long, viene quindi inserito in `@val` e passato come parametro all'istruzione `READTEXT`, che restituisce 10 byte a partire dal quinto byte (offset pari a 4).  
  
```sql
USE pubs;  
GO  
DECLARE @val VARBINARY(16);  
SELECT @val = TEXTPTR(pr_info)   
FROM pub_info  
WHERE pub_id = '0736';  
READTEXT pub_info.pr_info @val 4 10;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
pr_info                                                                                                                                                                                                                                                           
-----------------------------------------------------------------------  
 is sample  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Vedere anche  
 [DATALENGTH &#40;Transact-SQL&#41;](../../t-sql/functions/datalength-transact-sql.md)   
 [PATINDEX &#40;Transact-SQL&#41;](../../t-sql/functions/patindex-transact-sql.md)   
 [READTEXT &#40;Transact-SQL&#41;](../../t-sql/queries/readtext-transact-sql.md)   
 [SET TEXTSIZE &#40;Transact-SQL&#41;](../../t-sql/statements/set-textsize-transact-sql.md)   
 [Funzioni per i valori text e image &#40;Transact-SQL&#41;]()   
 [UPDATETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/updatetext-transact-sql.md)   
 [WRITETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/writetext-transact-sql.md)  
  
