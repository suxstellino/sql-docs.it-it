---
description: Conversione non deterministica di stringhe di valori letterali data in valori DATE
title: Conversione non deterministica di valori letterali data | Microsoft Docs
ms.custom: ''
ms.date: 11/19/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 74e4df5fc33b90ae9e455d566b41e6eae71408ac
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187785"
---
# <a name="nondeterministic-conversion-of-literal-date-strings-into-date-values"></a>Conversione non deterministica di stringhe di valori letterali data in valori DATE

Quando si consente la conversione di stringhe di caratteri in tipi di dati DATE, è necessario prestare attenzione. Tali conversioni, infatti, sono spesso _non deterministiche_.

Per controllare le conversioni non deterministiche è necessario tenere conto delle impostazioni di [SET LANGUAGE](../statements/set-language-transact-sql.md) e [SET DATEFORMAT](../statements/set-dateformat-transact-sql.md).



## <a name="set-language-example-month-name-in-polish"></a>Esempio di SET LANGUAGE: nome del mese in polacco

- `SET LANGUAGE Polish;`

Una stringa di caratteri può contenere il nome di un mese. Ma questo nome è in croato, in inglese, in polacco o in un'altra lingua? E la sessione dell'utente sarà impostata nella lingua corretta?

Si consideri ad esempio la parola _listopad_, che corrisponde al nome di un mese. A quale mese corrisponda, però, dipende dalla lingua che il sistema SQL ritiene che sia usata:
- In polacco, _listopad_ corrisponde al mese 11 (_novembre_ in italiano).
- In croato, _listopad_ corrisponde al mese 10 (_ottobre_ in italiano).

#### <a name="code-example-of-set-language"></a>Esempio di codice per SET LANGUAGE

```sql
--SELECT alias FROM sys.syslanguages ORDER BY alias;

DECLARE @yourInputDate  NVARCHAR(32) = '28 listopad 2018';

SET LANGUAGE Polish;
SELECT CONVERT(DATE, @yourInputDate) AS [SL_Polish];

SET LANGUAGE Croatian;
SELECT CONVERT(DATE, @yourInputDate) AS [SL_Croatian];

SET LANGUAGE English;


/***  Actual output:  For the two months, note the 11 versus the 10.
SL_Polish
2018-11-28

SL_Croatian
2018-10-28
**_/
```



## <a name="set-dateformat-example"></a>Esempio di SET DATEFORMAT

- `SET DATEFORMAT dmy;`

Il formato _ *dmy** qui sopra indica che la stringa della data di esempio '01-03-2018' viene interpretata come _il primo giorno del mese di marzo nell'anno 2018_.

Se invece si specifica **mdy**, la stessa stringa '01-03-2018' significa _il terzo giorno del mese di gennaio 2018_.

E se si specifica **ymd**, non è possibile sapere con certezza quale potrebbe essere l'output. Il valore numerico '2018' è troppo grande per corrispondere a un giorno.
<!--
The preceding claim of "no guarantee" might be incorrect, in the minds of the SQL query engine Developer team?
-->

#### <a name="specific-countries"></a>Paesi specifici

In Cina e in Giappone, il formato DATEFORMAT usato è **ymd**. Le parti del formato sono in una sequenza ragionevole, dalla più grande alla più piccola. Di conseguenza, l'ordinamento di questo formato è efficiente. Questo formato viene considerato il formato _internazionale_. Le quattro cifre dell'anno, infatti, non lasciano spazio ad ambiguità e nessun paese al mondo usa il formato desueto **ydm**.

In altri paesi, ad esempio Germania e Francia, DATEFORMAT è **dmy**, ovvero **'gg-mm-aaaa'**. Il formato **dmy** non consente un ordinamento efficiente, ma rappresenta una sequenza ragionevole, dall'unità più piccola alla più grande.

Stati Uniti e Stati Federati di Micronesia sono gli unici paesi che usano il formato **mdy**, che non consente l'ordinamento. La sequenza mista del formato corrisponde al modello di pronuncia colloquiale delle date.

#### <a name="code-example-of-set-dateformat-mdy-versus-dmy"></a>Esempio di codice per SET DATEFORMAT: confronto tra *mdy* e *dmy*

L'esempio di codice Transact-SQL seguente usa la stessa stringa di caratteri della data con tre diverse impostazioni di DATEFORMAT. Se si esegue il codice, viene generato l'output illustrato nel commento:

```sql
DECLARE @yourDateString NVARCHAR(10) = '12-09-2018';
PRINT @yourDateString + '  = the input.';

SET DATEFORMAT dmy;
SELECT CONVERT(DATE, @yourDateString) AS [DMY-Interpretation-of-input-format];

SET DATEFORMAT mdy;
SELECT CONVERT(DATE, @yourDateString) AS [MDY-Interpretation-of-input-format];

SET DATEFORMAT ymd;
SELECT CONVERT(DATE, @yourDateString) AS [YMD-Interpretation--?--NotGuaranteed];


/***  Actual output:
12-09-2018  = the input.

DMY-Interpretation-of-input-format
2018-09-12

MDY-Interpretation-of-input-format
2018-12-09

YMD-Interpretation--?--NotGuaranteed
2018-12-09
**_/
```

Nell'esempio di codice precedente, l'esempio finale presenta una mancata corrispondenza tra il formato _ *ymd** e la stringa di input. Il terzo nodo della stringa di input rappresenta un valore numerico troppo grande per corrispondere a un giorno. Microsoft non garantisce la correttezza del valore di output con mancate corrispondenze di questo tipo.

#### <a name="convert-offers-explicit-codes-for-_deterministic_-control-of-date-formats"></a>CONVERT rende disponibili codici espliciti per il controllo _deterministico_ dei formati di data

L'articolo della documentazione relativo a CAST e CONVERT elenca i codici espliciti che è possibile usare con la funzione CONVERT per controllare _in modo deterministico_ le conversioni delle date. Ogni mese l'articolo presenta uno dei conteggi di visualizzazione della pagina più alti.

- [CAST e CONVERT (Transact-SQL): stili date e time](../functions/cast-and-convert-transact-sql.md#date-and-time-styles)
- [CAST e CONVERT (Transact-SQL): alcune conversioni di data/ora sono non deterministiche](../functions/cast-and-convert-transact-sql.md#certain-datetime-conversions-are-nondeterministic)



## <a name="compatibility-level-90-and-above"></a>Livello di compatibilità 90 e superiore

In SQL Server 2000, il livello di compatibilità era pari a 80. Per le impostazioni di livello pari a 80 o inferiori, le conversioni di data implicite erano deterministiche.

A partire da SQL Server 2005, grazie al livello di compatibilità pari a 90, le conversioni di data implicite sono ora non deterministiche. A partire dal livello 90, le conversioni di data sono dipendenti da SET LANGUAGE e SET DATEFORMAT.

#### <a name="unicode"></a>Unicode

<!-- The next live sentence needs an explanatory example!  N'somethingHere?'.
-->
Anche la conversione di dati di tipo carattere non Unicode tra regole di confronto viene considerata non deterministica.



## <a name="see-also"></a>Vedere anche

- [Impostazione di una lingua di sessione](../../relational-databases/collations/set-a-session-language.md)
- [Funzioni e tipi di dati di data e ora (Transact-SQL)](../functions/date-and-time-data-types-and-functions-transact-sql.md)
- [FORMAT (Transact-SQL)](../functions/format-transact-sql.md)
- [ISDATE (Transact-SQL)](../functions/isdate-transact-sql.md)



<!--
This new article is linked-to by the following articles (at least initially on 2018/11/19).....
...
* docs/relational-databases/views/create-indexed-views.md
* docs/relational-databases/indexes/indexes-on-computed-columns.md
* docs/t-sql/functions/cast-and-convert-transact-sql.md
...
As a reaction to public PR 1279, this approach of creating a new article to link to is a better alternative than a docs/includes/ approach.
GeneMi (MightyPen), 2018/11/19
-->

