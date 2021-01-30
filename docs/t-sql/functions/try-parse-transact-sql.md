---
description: TRY_PARSE (Transact-SQL)
title: TRY_PARSE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TRY_PARSE_TSQL
- TRY_PARSE
dev_langs:
- TSQL
helpviewer_keywords:
- TRY_PARSE function
ms.assetid: 292bac1d-edd8-468c-8ff1-8c7de625bc55
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current||>= sql-server-2016||>= sql-server-linux-2017||=azure-sqldw-latest
ms.openlocfilehash: dfd39a530ca9bac61d2a4ac6036eaae7d5c37152
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183329"
---
# <a name="try_parse-transact-sql"></a>TRY_PARSE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Viene restituito il risultato di un'espressione convertito nel tipo di dati richiesto. Se il cast non viene eseguito in modo corretto in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene restituito Null. Utilizzare TRY_PARSE solo per la conversione da stringa a data/ora e tipi di numero.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
TRY_PARSE ( string_value AS data_type [ USING culture ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *string_value*  
 Valore **nvarchar(4000)** che rappresenta il valore formattato da analizzare nel tipo di dati specificato.  
  
 *string_value* deve essere una rappresentazione valida del tipo di dati richiesto. In caso contrario, TRY_PARSE restituisce Null.  
  
 *data_type*  
 Valore letterale che rappresenta il tipo di dati richiesto per il risultato.  
  
 *Impostazioni cultura*  
 Stringa facoltativa che identifica le impostazioni cultura in cui viene formattato *string_value*.  
  
 Se l'argomento *culture* non è specificato, viene usata la lingua della sessione corrente. La lingua in questione viene impostata in modo implicito o esplicito tramite l'istruzione SET LANGUAGE. *culture* accetta qualsiasi impostazione cultura supportata da .NET Framework e non è limitato alle lingue supportate in modo esplicito da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se l'argomento *culture* non è valido, PARSE genera un errore.  
  
## <a name="return-types"></a>Tipi restituiti  
 Restituisce il risultato dell'espressione convertito nel tipo di dati richiesto. Se il cast non viene eseguito in modo corretto, restituisce Null.  
  
## <a name="remarks"></a>Osservazioni  
 Utilizzare TRY_PARSE solo per la conversione da stringa a data/ora e tipi di numero. Per le conversioni di tipi generali, continuare a utilizzare CAST o CONVERT. È opportuno ricordare che si verifica un sovraccarico nelle prestazioni durante l'analisi del valore stringa.  
  
 TRU_PARSE è basato sulla presenza di CLR (Common Language Runtime) di .NET Framework.  
  
 Questa funzione non sarà eseguita in modalità remota poiché dipende dalla presenza di CLR. L'esecuzione in modalità remota di una funzione che richiede CLR provocherebbe un errore sul server remoto.  
  
 **Ulteriori informazioni sul parametro data_type**  
  
 I valori per il parametro *data_type* sono limitati ai tipi visualizzati nella tabella seguente, insieme con gli stili. Le informazioni sugli stili vengono fornite per determinare i tipi di modelli consentiti. Per altre informazioni sugli stili, vedere la documentazione di .NET Framework relativa alle enumerazioni **System.Globalization.NumberStyles** e **DateTimeStyles**.  
  
|Category|Type|Tipo .NET|Stili utilizzati|  
|--------------|----------|---------------|-----------------|  
|Numeric|bigint|Int64|NumberStyles.Number|  
|Numeric|INT|Int32|NumberStyles.Number|  
|Numeric|SMALLINT|Int16|NumberStyles.Number|  
|Numeric|TINYINT|Byte|NumberStyles.Number|  
|Numeric|decimal|Decimal|NumberStyles.Number|  
|Numeric|NUMERIC|Decimal|NumberStyles.Number|  
|Numeric|float|Double|NumberStyles.Float|  
|Numeric|real|Single|NumberStyles.Float|  
|Numeric|SMALLMONEY|Decimal|NumberStyles.Currency|  
|Numeric|money|Decimal|NumberStyles.Currency|  
|Data e ora|Data|Datetime|DateTimeStyles.AllowWhiteSpaces &#124; DateTimeStyles.AssumeUniversal|  
|Data e ora|time|TimeSpan|DateTimeStyles.AllowWhiteSpaces &#124; DateTimeStyles.AssumeUniversal|  
|Data e ora|Datetime|Datetime|DateTimeStyles.AllowWhiteSpaces &#124; DateTimeStyles.AssumeUniversal|  
|Data e ora|smalldatetime|Datetime|DateTimeStyles.AllowWhiteSpaces &#124; DateTimeStyles.AssumeUniversal|  
|Data e ora|datetime2|Datetime|DateTimeStyles.AllowWhiteSpaces &#124; DateTimeStyles.AssumeUniversal|  
|Data e ora|datetimeoffset|DateTimeOffset|DateTimeStyles.AllowWhiteSpaces &#124; DateTimeStyles.AssumeUniversal|  
  
 **Ulteriori informazioni sul parametro relativo alle impostazioni cultura**  
  
 Nella tabella seguente vengono mostrati i mapping dai linguaggi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] alle impostazioni cultura di .NET Framework.  
  
|Nome completo|Alias|LCID|Impostazioni cultura specifiche|  
|---------------|-----------|----------|----------------------|  
|us_english|Inglese|1033|it-IT|  
|Deutsch|Tedesco|1031|de-DE|  
|Français|Francese|1036|fr-FR|  
|日本語|Giapponese|1041|ja-JP|  
|Dansk|Danese|1030|da-DK|  
|Español|Spagnolo|3082|es-ES|  
|Italiano|Italiano|1040|it-IT|  
|Nederlands|Olandese|1043|nl-NL|  
|Norsk|Norvegese|2068|nn-NO|  
|Português|Portoghese|2070|pt-PT|  
|Suomi|Finlandese|1035|fi-FI|  
|Svenska|Svedese|1053|sv-SE|  
|čeština|Ceco|1029|Cs-CZ|  
|magyar|Ungherese|1038|Hu-HU|  
|polski|Polacco|1045|Pl-PL|  
|română|Romeno|1048|Ro-RO|  
|hrvatski|Croato|1050|hr-HR|  
|slovenčina|Slovacco|1051|Sk-SK|  
|slovenski|Sloveno|1060|Sl-SI|  
|ελληνικά|Greco|1032|El-GR|  
|български|Bulgaro|1026|bg-BG|  
|русский|Russo|1049|Ru-RU|  
|Türkçe|Turco|1055|Tr-TR|  
|British|Inglese (Regno Unito)|2057|en-GB|  
|eesti|Estone|1061|Et-EE|  
|latviešu|Lettone|1062|lv-LV|  
|lietuvių|Lituano|1063|lt-LT|  
|Português (Brasile)|Portoghese (Brasile)|1046|pt-BR|  
|繁體中文|Cinese tradizionale|1028|zh-TW|  
|한국어|Coreano|1042|Ko-KR|  
|简体中文|Cinese semplificato|2052|zh-CN|  
|Arabo|Arabo|1025|ar-SA|  
|ไทย|Thai|1054|Th-TH|  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-simple-example-of-try_parse"></a>R. Esempio semplice di TRY_PARSE  
  
```sql
SELECT TRY_PARSE('Jabberwokkie' AS datetime2 USING 'en-US') AS Result;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
---------------  
NULL  
  
(1 row(s) affected)  
```  
  
### <a name="b-detecting-nulls-with-try_parse"></a>B. Rilevamento di valori Null con TRY_PARSE  
  
```sql
SELECT  
    CASE WHEN TRY_PARSE('Aragorn' AS decimal USING 'sr-Latn-CS') IS NULL  
        THEN 'True'  
        ELSE 'False'  
END  
AS Result;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
---------------  
True  
  
(1 row(s) affected)  
```  
  
### <a name="c-using-iif-with-try_parse-and-implicit-culture-setting"></a>C. Utilizzo di IIF con TRY_PARSE e impostazioni cultura implicite  
  
```sql
SET LANGUAGE English;  
SELECT IIF(TRY_PARSE('01/01/2011' AS datetime2) IS NULL, 'True', 'False') AS Result;
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
---------------  
False  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Vedere anche  
 [PARSE &#40;Transact-SQL&#41;](../../t-sql/functions/parse-transact-sql.md)   
 [Funzioni di conversione &#40;Transact-SQL&#41;](../../t-sql/functions/conversion-functions-transact-sql.md)   
 [TRY_CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/try-convert-transact-sql.md)   
 [CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
  
  
