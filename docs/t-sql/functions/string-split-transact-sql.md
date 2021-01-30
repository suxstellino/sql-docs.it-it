---
title: STRING_SPLIT (Transact-SQL) | Microsoft Docs
description: Informazioni di riferimento Transact-SQL per la funzione STRING_SPLIT. Questa funzione con valori di tabella suddivide una stringa in sottostringhe in base a un delimitatore di caratteri.
ms.date: 11/28/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: jrasnick
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STRING_SPLIT
- STRING_SPLIT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STRING_SPLIT function
ms.assetid: 3273dbf3-0b4f-41e1-b97e-b4f67ad370b9
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current||=azure-sqldw-latest||>= sql-server-2016 || >= sql-server-linux-2017
ms.openlocfilehash: 472da04cc9a93a8f7846927ad9ae4595766e9116
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207887"
---
# <a name="string_split-transact-sql"></a>STRING_SPLIT (Transact-SQL)

[!INCLUDE[tsql-appliesto-ss2016-asdb-asdw-xxx-md.md](../../includes/tsql-appliesto-ss2016-asdb-asdw-xxx-md.md)]

Funzione con valori di tabella che divide una stringa in righe di sottostringhe in base a un carattere separatore specificato.

#### <a name="compatibility-level-130"></a>Livello di compatibilità 130

STRING_SPLIT richiede un livello di compatibilità minimo di 130. Quando il livello è inferiore a 130, SQL Server non riesce a trovare la funzione STRING_SPLIT.

Per modificare il livello di compatibilità di un database, fare riferimento a [Visualizzare o modificare il livello di compatibilità di un database](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md).

> [!NOTE]
> La configurazione della compatibilità non è necessaria per STRING_SPLIT in Azure Synapse Analytics.

![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  

```syntaxsql
STRING_SPLIT ( string , separator )  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

 *string*  
 [Espressione](../../t-sql/language-elements/expressions-transact-sql.md) di qualsiasi tipo carattere, ad esempio **nvarchar**, **varchar**, **nchar** o **char**.  
  
 *separator*  
 È un'[espressione](../../t-sql/language-elements/expressions-transact-sql.md) a carattere singolo di qualsiasi tipo, ad esempio **nvarchar(1)** , **varchar (1)** , **nchar (1)** o **char (1)** , usata come separatore per sottostringhe concatenate.  
  
## <a name="return-types"></a>Tipi restituiti  

Restituisce una tabella a colonna singola le cui righe sono sottostringhe. Il nome della colonna è **value**. Restituisce **nvarchar** se uno qualsiasi degli argomenti di input è **nvarchar** o **nchar**. In caso contrario, restituisce **varchar**. La lunghezza del tipo restituito è uguale a quella dell'argomento di stringa.  
  
## <a name="remarks"></a>Osservazioni  

**STRING_SPLIT** inserisce una stringa con sottostringhe delimitate e inserisce un carattere da usare come delimitatore o separatore. STRING_SPLIT restituisce una tabella a colonna singola le cui righe contengono le sottostringhe. Il nome della colonna di output è **value**.

Le righe di output potrebbero essere in qualsiasi ordine. L'ordine _non_ corrisponde necessariamente all'ordine delle sottostringhe nella stringa di input. È possibile ignorare l'ordinamento finale usando una clausola ORDER BY nell'istruzione SELECT (`ORDER BY value`).

0x0000 (**char(0)** ) è un carattere non definito nelle regole di confronto di Windows e non può essere incluso in STRING_SPLIT.

Le sottostringhe vuote di lunghezza zero sono presenti quando la stringa di input contiene due o più occorrenze consecutive del carattere delimitatore. Le sottostringhe vuote vengono trattate allo stesso modo delle sottostringhe semplici. È possibile escludere le righe che contengono la sottostringa vuota usando la clausola WHERE (`WHERE value <> ''`). Se la stringa di input è NULL, la funzione con valori di tabella STRING_SPLIT restituisce una tabella vuota.  

Ad esempio, l'istruzione SELECT seguente usa il carattere spazio come separatore:

```sql
SELECT value FROM STRING_SPLIT('Lorem ipsum dolor sit amet.', ' ');
```

In un esempio pratico, l'istruzione SELECT precedente ha restituito la tabella dei risultati seguente:  
  
|Valore|  
| :-- |  
|Lorem|  
|ipsum|  
|dolor|  
|sit|  
|amet.|  
| &nbsp; |

## <a name="examples"></a>Esempi  
  
### <a name="a-split-comma-separated-value-string"></a>R. Dividere una stringa di valori separati da virgola

Analizzare un elenco di valori separati da virgole e restituire tutti i token non vuoti:  

```sql
DECLARE @tags NVARCHAR(400) = 'clothing,road,,touring,bike'  
  
SELECT value  
FROM STRING_SPLIT(@tags, ',')  
WHERE RTRIM(value) <> '';
```

STRING_SPLIT restituirà una stringa vuota se non c'è niente tra i separatori. La condizione RTRIM(value) <> " rimuoverà i token vuoti.  
  
### <a name="b-split-comma-separated-value-string-in-a-column"></a>B. Dividere una stringa di valori delimitati da virgola in una colonna

La tabella Product ha una colonna con un elenco di tag delimitati da virgole illustrato nell'esempio seguente:  
  
|ProductId|Nome|Tag|  
|---------------|----------|----------|  
|1|Full-Finger Gloves|clothing,road,touring,bike|  
|2|LL Headset|bike|  
|3|HL Mountain Frame|bike, mountain|  
  
La query seguente trasforma gli elenchi di tag e li unisce alla riga originale:  

```sql  
SELECT ProductId, Name, value  
FROM Product  
    CROSS APPLY STRING_SPLIT(Tags, ',');  
```

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|ProductId|Nome|Valore|  
|---------------|----------|-----------|  
|1|Full-Finger Gloves|clothing|  
|1|Full-Finger Gloves|strada|  
|1|Full-Finger Gloves|touring|  
|1|Full-Finger Gloves|bike|  
|2|LL Headset|bike|  
|3|HL Mountain Frame|bike|  
|3|HL Mountain Frame|mountain|  

  >[!NOTE]
  > L'ordine dell'output può variare poiché _non_ corrisponde necessariamente all'ordine delle sottostringhe nella stringa di input.
  
### <a name="c-aggregation-by-values"></a>C. Aggregazione per valori

Gli utenti devono creare un report che visualizzi il numero di prodotti per ogni tag, ordinati in base al numero di prodotti, e che sia possibile filtrare in base ai tag con più di due prodotti.  

```sql  
SELECT value as tag, COUNT(*) AS [Number of articles]  
FROM Product  
    CROSS APPLY STRING_SPLIT(Tags, ',')  
GROUP BY value  
HAVING COUNT(*) > 2  
ORDER BY COUNT(*) DESC;  
```

### <a name="d-search-by-tag-value"></a>D. Ricerca in base al valore del tag

Gli sviluppatori devono creare query per trovare articoli in base a parole chiave. Possono usare le query seguenti:  
  
Per trovare i prodotti con un singolo tag (clothing):  

```sql
SELECT ProductId, Name, Tags  
FROM Product  
WHERE 'clothing' IN (SELECT value FROM STRING_SPLIT(Tags, ','));  
```

Per trovare i prodotti con due tag specificati (clothing e road):  

```sql  
SELECT ProductId, Name, Tags  
FROM Product  
WHERE EXISTS (SELECT *  
    FROM STRING_SPLIT(Tags, ',')  
    WHERE value IN ('clothing', 'road'));  
```

### <a name="e-find-rows-by-list-of-values"></a>E. Trovare le righe in base all'elenco di valori

Gli sviluppatori devono creare una query che consenta di trovare gli articoli in base a un elenco di ID. Possono usare la query seguente:  

```sql  
SELECT ProductId, Name, Tags  
FROM Product  
JOIN STRING_SPLIT('1,2,3',',')
    ON value = ProductId;  
```  

L'uso precedente di STRING_SPLIT è una sostituzione di un anti-pattern comune. Questo anti-pattern può comportare la creazione di una stringa SQL dinamica nel livello dell'applicazione o in Transact-SQL. Oppure, è possibile ottenere un anti-pattern usando l'operatore LIKE. Vedere l'esempio di istruzione SELECT seguente:

```sql  
SELECT ProductId, Name, Tags  
FROM Product  
WHERE ',1,2,3,' LIKE '%,' + CAST(ProductId AS VARCHAR(20)) + ',%';  
```

## <a name="see-also"></a>Vedere anche

- [LEFT &#40;Transact-SQL&#41;](../../t-sql/functions/left-transact-sql.md)
- [LTRIM &#40;Transact-SQL&#41;](../../t-sql/functions/ltrim-transact-sql.md)
- [RIGHT &#40;Transact-SQL&#41;](../../t-sql/functions/right-transact-sql.md)
- [RTRIM &#40;Transact-SQL&#41;](../../t-sql/functions/rtrim-transact-sql.md)
- [SUBSTRING &#40;Transact-SQL&#41;](../../t-sql/functions/substring-transact-sql.md)
- [TRIM &#40;Transact-SQL&#41;](../../t-sql/functions/trim-transact-sql.md)
- [Funzioni per i valori stringa &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)
