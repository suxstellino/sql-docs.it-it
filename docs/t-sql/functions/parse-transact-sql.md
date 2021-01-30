---
description: PARSE (Transact-SQL)
title: PARSE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/05/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- PARSE
- PARSE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- PARSE function
ms.assetid: 6a2dbf10-f692-471b-9458-24d246963049
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: daa87ffc035733c415d1c5a4c66bfc7a4e1278f3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99100602"
---
# <a name="parse-transact-sql"></a>PARSE (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce il risultato di un'espressione, convertito nel tipo di dati richiesto in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
PARSE ( string_value AS data_type [ USING culture ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *string_value*  
 Valore **nvarchar**(4000) che rappresenta il valore formattato da analizzare nel tipo di dati specificato.  
  
 *string_value* deve essere una rappresentazione valida del tipo di dati richiesto, altrimenti PARSE genera un errore.  
  
 *data_type*  
 Valore letterale che rappresenta il tipo di dati richiesto per il risultato.  
  
 *Impostazioni cultura*  
 Stringa facoltativa che identifica le impostazioni cultura in cui viene formattato *string_value*.  
  
 Se l'argomento *culture* non è specificato, viene usata la lingua della sessione corrente. Tale lingua viene impostata in modo implicito o in modo esplicito tramite l'istruzione SET LANGUAGE. *culture* accetta qualsiasi impostazione cultura supportata da .NET Framework e non è limitato alle lingue supportate in modo esplicito da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se l'argomento *culture* non è valido, PARSE genera un errore.  
  
## <a name="return-types"></a>Tipi restituiti  
 Restituisce il risultato dell'espressione, convertito nel tipo di dati richiesto.  
  
## <a name="remarks"></a>Osservazioni  
 Eventuali valori Null passati come argomenti a PARSE vengono trattati in due modalità:  
  
1.  Se viene passata una costante Null, viene generato un errore. Un valore Null non può essere analizzato in un tipo di dati diverso in modo consapevole.  
  
2.  Se viene passato un parametro con valore Null in fase di esecuzione, viene restituito un valore Null per evitare di annullare l'intero batch.  
  
 Utilizzare PARSE solo per la conversione da stringa a data/ora e tipi di numero. Per le conversioni di tipi generali, continuare a utilizzare CAST o CONVERT. È opportuno ricordare che si verifica un sovraccarico nelle prestazioni durante l'analisi del valore stringa.  
  
 PARSE è basato sulla presenza di CLR (Common Language Runtime) di .NET Framework.  
  
 Questa funzione non sarà eseguita in modalità remota poiché dipende dalla presenza di CLR. L'esecuzione in modalità remota di una funzione che richiede CLR provocherebbe un errore sul server remoto.  
  
 **Ulteriori informazioni sul parametro data_type**  
  
 I valori per il parametro *data_type* sono limitati ai tipi visualizzati nella tabella seguente, insieme con gli stili. Le informazioni sugli stili vengono fornite per determinare i tipi di modelli consentiti. Per altre informazioni sugli stili, vedere la documentazione di .NET Framework relativa alle enumerazioni **System.Globalization.NumberStyles** e **DateTimeStyles**.  
  
|Category|Type|Tipo .NET Framework|Stili utilizzati|  
|--------------|----------|-------------------------|-----------------|  
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
  
### <a name="a-parse-into-datetime2"></a>R. PARSE in datetime2  
  
```sql  
SELECT PARSE('Monday, 13 December 2010' AS datetime2 USING 'en-US') AS Result;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
---------------  
2010-12-13 00:00:00.0000000  
  
(1 row(s) affected)  
```  
  
### <a name="b-parse-with-currency-symbol"></a>B. PARSE con simbolo di valuta  
  
```sql  
SELECT PARSE('€345,98' AS money USING 'de-DE') AS Result;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
---------------  
345.98  
  
(1 row(s) affected)  
```  
  
### <a name="c-parse-with-implicit-setting-of-language"></a>C. PARSE con impostazione implicita di linguaggio  
  
```sql  
-- The English language is mapped to en-US specific culture  
SET LANGUAGE 'English';  
SELECT PARSE('12/16/2010' AS datetime2) AS Result;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
---------------  
2010-12-16 00:00:00.0000000  
  
(1 row(s) affected)  
```  
  
  
