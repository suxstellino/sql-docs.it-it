---
description: STRING_AGG (Transact-SQL)
title: STRING_AGG (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/19/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: conceptual
f1_keywords:
- STRING_AGG
- STRING_AGG_TSQL
helpviewer_keywords:
- STRING_AGG function
ms.assetid: 8860ef3f-142f-4cca-aa64-87a123e91206
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6c0eaef945ae4234987e6849f305d7b26cbb9374
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346602"
---
# <a name="string_agg-transact-sql"></a>STRING_AGG (Transact-SQL)

<!--[!INCLUDE [sqlserver2017-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi-asa.md)]-->

[!INCLUDE [sqlserver2017-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi-asa.md)]

Concatena i valori delle espressioni della stringa e inserisce i valori dei separatori tra di essi. Il separatore non viene aggiunto alla fine della stringa. 
 
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
STRING_AGG ( expression, separator ) [ <order_clause> ]

<order_clause> ::=   
    WITHIN GROUP ( ORDER BY <order_by_expression_list> [ ASC | DESC ] )   
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

*expression*  
[Espressione](../../t-sql/language-elements/expressions-transact-sql.md) di qualsiasi tipo. Le espressioni vengono convertite in tipi `NVARCHAR` o `VARCHAR` durante la concatenazione. I tipi non stringa vengono convertiti nel tipo `NVARCHAR`.

*separator*  
È un'[espressione](../../t-sql/language-elements/expressions-transact-sql.md) di tipo `NVARCHAR` o `VARCHAR` usata come separatore per stringhe concatenate. Può essere un valore letterale o una variabile. 

<order_clause>   
Facoltativamente è possibile specificare l'ordine dei risultati concatenati mediante la clausola `WITHIN GROUP`:

```syntaxsql
WITHIN GROUP ( ORDER BY <order_by_expression_list> [ ASC | DESC ] )
```   
<order_by_expression_list>   
 
  Elenco di [espressioni](../../t-sql/language-elements/expressions-transact-sql.md) non costanti che può essere usato per l'ordinamento dei risultati. È consentito un solo valore `order_by_expression` per query. Per impostazione predefinita, l'ordinamento è crescente.   
  
## <a name="return-types"></a>Tipi restituiti

Il tipo restituito dipende dal primo argomento (espressione). Se l'argomento di input è di tipo stringa (`NVARCHAR`, `VARCHAR`), il tipo di risultato sarà uguale al tipo dell'input. Nella tabella seguente sono elencate le conversioni automatiche:  

|Tipo di espressione di input |Risultato | 
|-------|-------|
|NVARCHAR(MAX) |NVARCHAR(MAX) |
|VARCHAR(MAX) |VARCHAR(MAX) |
|NVARCHAR(1...4000) |NVARCHAR(4000) |
|VARCHAR(1...8000) |VARCHAR(8000) |
|int, bigint, smallint, tinyint, numeric, float, real, bit, decimal, smallmoney, money, datetime, datetime2, |NVARCHAR(4000) |

## <a name="remarks"></a>Commenti

`STRING_AGG` è una funzione di aggregazione che accetta tutte le espressioni dalle righe e le concatena in una stringa singola. I valori delle espressioni vengono convertiti in modo implicito nei tipi di stringa e successivamente concatenati. Per la conversione implicita in stringhe vengono seguite le regole esistenti per le conversioni dei tipi di dati. Per altre informazioni sulle conversioni dei tipi di dati, vedere [CAST e CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md). 

Se l'espressione di input è di tipo `VARCHAR`, il separatore non può essere di tipo `NVARCHAR`. 

I valori Null vengono ignorati e il separatore corrispondente non viene aggiunto. Per restituire un segnaposto per i valori Null, usare la funzione `ISNULL` come illustrato nell'esempio B.

`STRING_AGG` è disponibile in qualsiasi livello di compatibilità.

## <a name="examples"></a>Esempi

### <a name="a-generate-list-of-names-separated-in-new-lines"></a>R. Generare l'elenco di nomi separati in nuove righe

Nell'esempio seguente viene generato un elenco di nomi in una cella di risultato singola, separati con ritorno a capo.
```sql
USE AdventureWorks2016
GO
SELECT STRING_AGG (CONVERT(NVARCHAR(max),FirstName), CHAR(13)) AS csv 
FROM Person.Person;  
```
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

|csv | 
|--- |
|Syed <br />Catherine <br />Kim <br />Kim <br />Kim <br />Hazem <br />... | 

I valori `NULL` presenti nelle celle `name` non vengono restituiti nel risultato.   

> [!NOTE]  
> Se si usa l'editor di query di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], l'opzione **Risultati in formato griglia** non può implementare il ritorno a capo. Passare a **Risultati in formato testo** per visualizzare il risultato impostato correttamente.       
> Per impostazione predefinita, i risultati in formato testo vengono troncati a 256 caratteri. Per aumentare questo limite, modificare l'opzione **Numero massimo di caratteri visualizzati in ogni colonna**.

### <a name="b-generate-list-of-names-separated-with-comma-without-null-values"></a>B. Generare l'elenco di nomi delimitati da virgola senza valori NULL

Nell'esempio seguente i valori Null vengono sostituiti con "N/A" e i nomi delimitati da virgole vengono restituiti in una cella di risultato singola.  
```sql
USE AdventureWorks2016
GO
SELECT STRING_AGG(CONVERT(NVARCHAR(max), ISNULL(FirstName,'N/A')), ',') AS csv 
FROM Person.Person; 
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

> [!NOTE]
> I risultati vengono visualizzati tagliati.

|csv | 
|--- |
|Syed,Catherine,Kim,Kim,Kim,Hazem,Sam,Humberto,Gustavo,Pilar,Pilar, ...|  

### <a name="c-generate-comma-separated-values"></a>C. Generare valori delimitati da virgole

```sql
USE AdventureWorks2016
GO
SELECT STRING_AGG(CONVERT(NVARCHAR(max), CONCAT(FirstName, ' ', LastName, '(', ModifiedDate, ')')), CHAR(13)) AS names 
FROM Person.Person; 
```
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

> [!NOTE]
> I risultati vengono visualizzati tagliati.

|nomi |
|--- |
|Davide Sánchez (8 febbraio 2003 12:00) <br />Terri Duffy (24 febbraio 2002 12:00) <br />Roberto Tamburello (5 dicembre 2001 12:00) <br />Rob Walters (29 dicembre 2001 12:00) <br />... |

> [!NOTE]  
> Se si usa l'editor di query di Management Studio, l'opzione **Risultati in formato griglia** non può implementare il ritorno a capo. Passare a **Risultati in formato testo** per visualizzare il risultato impostato correttamente.

### <a name="d-return-news-articles-with-related-tags"></a>D. Restituire articoli di giornale con tag correlati

Gli articoli e i tag correlati vengono suddivisi in tabelle diverse. Lo sviluppatore vuole che venga restituita una riga per ogni articolo con tutti i tag associati. Usare la query seguente:

```sql
SELECT a.articleId, title, STRING_AGG (tag, ',') as tags
FROM dbo.Article AS a
LEFT JOIN dbo.ArticleTag AS t
    ON a.ArticleId = t.ArticleId
GROUP BY a.articleId, title;
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

|articleId |title |tags |
|--- |--- |--- |
|172 |I sondaggi indicano risultati delle elezioni molto vicini |politica, sondaggi, municipio |
|176 |La nuova autostrada dovrebbe ridurre il traffico |NULL |
|177 |I cani continuano a essere preferiti ai gatti |sondaggi, animali|

> [!NOTE]
> La clausola `GROUP BY` è obbligatoria se la funzione `STRING_AGG` non è l'unico elemento nell'elenco `SELECT`.

### <a name="e-generate-list-of-emails-per-towns"></a>E. Generare un elenco di messaggi di posta elettronica per città

La query seguente consente di trovare gli indirizzi di posta elettronica dei dipendenti e di raggrupparli in base alla città:

```sql
USE AdventureWorks2016
GO

SELECT TOP 10 City, STRING_AGG(CONVERT(NVARCHAR(max), EmailAddress), ';') AS emails 
FROM Person.BusinessEntityAddress AS BEA  
INNER JOIN Person.Address AS A ON BEA.AddressID = A.AddressID
INNER JOIN Person.EmailAddress AS EA ON BEA.BusinessEntityID = EA.BusinessEntityID 
GROUP BY City;
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

> [!NOTE]
> I risultati vengono visualizzati tagliati.

|City |emails |
|--- |--- |
|Ballard|paige28@adventure-works.com;joshua24@adventure-works.com;javier12@adventure-works.com;...|
|Baltimore|gilbert9@adventure-works.com|
|Barstow|kristen4@adventure-works.com|
|Basingstoke Hants|dale10@adventure-works.com;heidi9@adventure-works.com|
|Baytown|kelvin15@adventure-works.com|
|Beaverton|billy6@adventure-works.com;dalton35@adventure-works.com;lawrence1@adventure-works.com;...|
|Bell Gardens|christy8@adventure-works.com
|Bellevue|min0@adventure-works.com;gigi0@adventure-works.com;terry18@adventure-works.com;...|
|Bellflower|philip0@adventure-works.com;emma34@adventure-works.com;jorge8@adventure-works.com;...|
|Bellingham|christopher23@adventure-works.com;frederick7@adventure-works.com;omar0@adventure-works.com;...|

I messaggi di posta elettronica restituiti nella colonna relativa possono essere usati per inviare messaggi di posta elettronica a un gruppo di persone che lavorano tutte nella stessa città. 

### <a name="f-generate-a-sorted-list-of-emails-per-towns"></a>F. Generare un elenco di messaggi di posta elettronica ordinato per città   
La query seguente è simile alla precedente e consente di trovare gli indirizzi di posta elettronica dei dipendenti, di raggrupparli in base alla città e di metterli in ordine alfabetico:   

```sql
USE AdventureWorks2016
GO

SELECT TOP 10 City, STRING_AGG(CONVERT(NVARCHAR(max), EmailAddress), ';') WITHIN GROUP (ORDER BY EmailAddress ASC) AS emails 
FROM Person.BusinessEntityAddress AS BEA  
INNER JOIN Person.Address AS A ON BEA.AddressID = A.AddressID
INNER JOIN Person.EmailAddress AS EA ON BEA.BusinessEntityID = EA.BusinessEntityID 
GROUP BY City;
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

> [!NOTE]
> I risultati vengono visualizzati tagliati.

|City |emails |
|--- |--- |
|Barstow|kristen4@adventure-works.com
|Basingstoke Hants|dale10@adventure-works.com;heidi9@adventure-works.com
|Braintree|mindy20@adventure-works.com
|Bell Gardens|christy8@adventure-works.com
|Byron|louis37@adventure-works.com
|Bordeaux|ranjit0@adventure-works.com
|Carnation|don0@adventure-works.com;douglas0@adventure-works.com;george0@adventure-works.com;...|
|Boulogne-Billancourt|allen12@adventure-works.com;bethany15@adventure-works.com;carl5@adventure-works.com;...|
|Berkshire|barbara41@adventure-works.com;brenda4@adventure-works.com;carrie14@adventure-works.com;...|
|Berks|adriana6@adventure-works.com;alisha13@adventure-works.com;arthur19@adventure-works.com;...|

## <a name="see-also"></a>Vedere anche
 
 [CONCAT &#40;Transact-SQL&#41;](../../t-sql/functions/concat-transact-sql.md)  
 [CONCAT_WS &#40;Transact-SQL&#41;](../../t-sql/functions/concat-ws-transact-sql.md)  
 [FORMATMESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/formatmessage-transact-sql.md)  
 [QUOTENAME &#40;Transact-SQL&#41;](../../t-sql/functions/quotename-transact-sql.md)  
 [REPLACE &#40;Transact-SQL&#41;](../../t-sql/functions/replace-transact-sql.md)  
 [REVERSE &#40;Transact-SQL&#41;](../../t-sql/functions/reverse-transact-sql.md)  
 [STRING_ESCAPE &#40;Transact-SQL&#41;](../../t-sql/functions/string-escape-transact-sql.md)  
 [STUFF &#40;Transact-SQL&#41;](../../t-sql/functions/stuff-transact-sql.md)  
 [TRANSLATE &#40;Transact-SQL&#41;](../../t-sql/functions/translate-transact-sql.md)  
 [Funzioni di aggregazione &#40;Transact-SQL&#41;](../../t-sql/functions/aggregate-functions-transact-sql.md)  
 [Funzioni per i valori stringa &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  

