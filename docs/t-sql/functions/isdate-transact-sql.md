---
description: ISDATE (Transact-SQL)
title: ISDATE (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ISDATETIME
- ISDATE_TSQL
- ISDATE
- ISDATETIME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- date and time [SQL Server], ISDATE
- validate dates times [SQL Server]
- formats [SQL Server], time
- dates [SQL Server], validate
- verify dates times [SQL Server]
- functions [SQL Server], time
- formats [SQL Server], dates
- functions [SQL Server], date and time
- time [SQL Server], functions
- time [SQL Server], validate
- ISDATE function [SQL Server]
ms.assetid: 8e2c9ee7-388a-432f-b2c9-7b398f26bf85
author: cawrites
ms.author: chadam
ms.reviewer: ''
ms.custom: ''
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b4e5166cba81e1fd1b7d08c9de742e5c7088a7c3
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104746811"
---
# <a name="isdate-transact-sql"></a>ISDATE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce 1 se *expression* è un valore **date**, **time** o **datetime** valido; in caso contrario, 0.  
  
 ISDATE restituisce 0 se *expression* è un valore **datetime2**.  
  
 Per una panoramica di tutti i tipi di dati e delle funzioni di data e ora [!INCLUDE[tsql](../../includes/tsql-md.md)], vedere [Funzioni e tipi di dati di data e ora &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md). Si noti che l'intervallo di dati datetime è compreso tra 1753-01-01 e 9999-12-31, mentre l'intervallo di dati relativi alla data è compreso tra 0001-01-01 e 9999-12-31.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
ISDATE ( expression )
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *expression*  
 Stringa di caratteri o [espressione](../../t-sql/language-elements/expressions-transact-sql.md) che può essere convertita in una stringa di caratteri. L'espressione deve essere di lunghezza inferiore a 4.000 caratteri. I tipi di dati relativi a data e ora, ad eccezione di datetime e smalldatetime, non sono consentiti come argomento di ISDATE.  
  
## <a name="return-type"></a>Tipo restituito  
 **int**  
  
## <a name="remarks"></a>Osservazioni  
 La funzione ISDATE è deterministica solo quando viene usata con la funzione [CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md), se il parametro style della funzione CONVERT è specificato e se lo stile è diverso da 0, 100, 9 o 109.  
  
 Il valore restituito di ISDATE dipende dalle impostazioni definite in [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md), [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md) e [Configurare l'opzione di configurazione del server default language](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md).  
  
## <a name="isdate-expression-formats"></a>Formati di espressione ISDATE  
 Per alcuni esempi di formati validi per i quali ISDATE restituisce 1, vedere la sezione relativa ai formati di valore letterale stringa supportati per datetime negli argomenti [datetime](../../t-sql/data-types/datetime-transact-sql.md) e [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md). Per esempi aggiuntivi, vedere anche la colonna Input/output della sezione "Argomenti" di [CAST e CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md).  
  
 Nella tabella seguente sono riepilogati i formati di espressione non validi e che restituiscono 0 o un errore.  
  
|Espressione ISDATE|Valore restituito ISDATE|  
|-----------------------|-------------------------|  
|NULL|0|  
|Valori dei tipi di dati elencati in [Tipi di dati](../../t-sql/data-types/data-types-transact-sql.md) in qualsiasi categoria diversa da stringhe di caratteri, stringhe di caratteri Unicode o data e ora.|0|  
|Valori dei tipi di dati **text**, **ntext** o **image**.|0|  
|Qualsiasi valore con una scala di precisione in secondi frazionari maggiore di 3 (da 0,0000 a 0,0000000... n) ISDATE restituisce 0 se *expression* è un valore **datetime2** ma restituisce 1 se *expression* è un valore **datetime** valido.|0|  
|Qualsiasi valore che combina una data valida con un valore non valido, ad esempio 1995-10-1a.|0|  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-isdate-to-test-for-a-valid-datetime-expression"></a>R. Utilizzo di ISDATE per il test di un'espressione datetime valida  
 Nell'esempio seguente viene illustrato l'uso di `ISDATE` per testare se una stringa di caratteri è un valore **datetime** valido.  
  
```sql  
IF ISDATE('2009-05-12 10:19:41.177') = 1  
    PRINT 'VALID'  
ELSE  
    PRINT 'INVALID';  
```  
  
### <a name="b-showing-the-effects-of-the-set-dateformat-and-set-language-settings-on-return-values"></a>B. Effetti delle impostazioni di SET DATEFORMAT e SET LANGUAGE sui valori restituiti  
 Nelle istruzioni seguenti sono mostrati i valori restituiti come risultato delle impostazioni di `SET DATEFORMAT` e `SET LANGUAGE`.  
  
```sql  
/* Use these sessions settings. */  
SET LANGUAGE us_english;  
SET DATEFORMAT mdy;  
/* Expression in mdy dateformat */  
SELECT ISDATE('04/15/2008'); --Returns 1.  
/* Expression in mdy dateformat */  
SELECT ISDATE('04-15-2008'); --Returns 1.   
/* Expression in mdy dateformat */  
SELECT ISDATE('04.15.2008'); --Returns 1.   
/* Expression in myd  dateformat */  
SELECT ISDATE('04/2008/15'); --Returns 1.  
  
SET DATEFORMAT mdy;  
SELECT ISDATE('15/04/2008'); --Returns 0.  
SET DATEFORMAT mdy;  
SELECT ISDATE('15/2008/04'); --Returns 0.  
SET DATEFORMAT mdy;  
SELECT ISDATE('2008/15/04'); --Returns 0.  
SET DATEFORMAT mdy;  
SELECT ISDATE('2008/04/15'); --Returns 1.  
  
SET DATEFORMAT dmy;  
SELECT ISDATE('15/04/2008'); --Returns 1.  
SET DATEFORMAT dym;  
SELECT ISDATE('15/2008/04'); --Returns 1.  
SET DATEFORMAT ydm;  
SELECT ISDATE('2008/15/04'); --Returns 1.  
SET DATEFORMAT ymd;  
SELECT ISDATE('2008/04/15'); --Returns 1.  
  
SET LANGUAGE English;  
SELECT ISDATE('15/04/2008'); --Returns 0.  
SET LANGUAGE Hungarian;  
SELECT ISDATE('15/2008/04'); --Returns 0.  
SET LANGUAGE Swedish;  
SELECT ISDATE('2008/15/04'); --Returns 0.  
SET LANGUAGE Italian;  
SELECT ISDATE('2008/04/15'); --Returns 1.  
  
/* Return to these sessions settings. */  
SET LANGUAGE us_english;  
SET DATEFORMAT mdy;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-isdate-to-test-for-a-valid-datetime-expression"></a>C. Utilizzo di ISDATE per il test di un'espressione datetime valida  
 Nell'esempio seguente viene illustrato l'uso di `ISDATE` per testare se una stringa di caratteri è un valore **datetime** valido.  
  
```sql  
IF ISDATE('2009-05-12 10:19:41.177') = 1  
    SELECT 'VALID';  
ELSE  
    SELECT 'INVALID';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
