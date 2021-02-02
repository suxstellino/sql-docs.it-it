---
title: CAST e CONVERT (Transact-SQL) | Microsoft Docs
description: Informazioni di riferimento per le funzioni Transact-SQL CAST e CONVERT. Queste funzioni convertono un'espressione da un tipo di dati a un altro.
ms.date: 08/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CAST_TSQL
- CONVERT_TSQL
- CAST
- CONVERT
dev_langs:
- TSQL
helpviewer_keywords:
- CAST function
- automatic data type conversion
- varbinary data type
- CONVERT function
- data types [SQL Server], converting
- large value data types
- implicit data type conversions
- image data type, converting
- explicit data type conversions [SQL Server]
- binary [SQL Server], converting
- text data type, converting
- date and time [SQL Server], cast and convert
- truncating conversions
- converting data types [SQL Server], conversion functions
- time zones [SQL Server]
- roundtrip conversions
ms.assetid: a87d0850-c670-4720-9ad5-6f5a22343ea8
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1572028c2aa575078392c34fca37fa8105e25661
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236379"
---
# <a name="cast-and-convert-transact-sql"></a>CAST e CONVERT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Queste funzioni convertono un'espressione da un tipo di dati a un altro.  

## <a name="syntax"></a>Sintassi  
  
```sqlsyntax 
-- CAST Syntax:  
CAST ( expression AS data_type [ ( length ) ] )  
  
-- CONVERT Syntax:  
CONVERT ( data_type [ ( length ) ] , expression [ , style ] )  
```  

![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*expression*  
Qualsiasi [espressione](../../t-sql/language-elements/expressions-transact-sql.md) valida.
  
*data_type*  
Tipo di dati di destinazione. Include **xml**, **bigint** e **sql_variant**. Non è possibile usare i tipi di dati alias.
  
*length*  
Intero facoltativo che specifica la lunghezza del tipo di dati di destinazione per i tipi di dati che consentono una lunghezza specificata dall'utente. Il valore predefinito è 30.
  
*style*  
Espressione integer che specifica il modo in cui la funzione CONVERT converte *expression*. Per un valore di stile NULL, viene restituito NULL. *data_type* determina l'intervallo. 
  
## <a name="return-types"></a>Tipi restituiti
Restituisce *expression* convertito in *data_type*.
  
## <a name="date-and-time-styles"></a>Stili di data e ora  
Se il tipo di dati di *expression* è date o time, *style* può avere uno dei valori indicati nella tabella seguente. Gli altri valori vengono elaborati come 0. A partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], gli unici stili supportati nella conversione dai tipi date e time in **datetimeoffset** sono 0 o 1. Tutti gli altri stili di conversione restituiscono l'errore 9809.
  
> [!NOTE]
> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta il formato di data in stile arabo con l'algoritmo kuwaitiano.
  
|Senza il secolo (aa) (<sup>1</sup>)|Con il secolo (aaaa)|Standard|Input/Output (<sup>3</sup>)|  
|---|---|--|---|
|-|**0** o **100** (<sup>1,</sup><sup>2</sup>)|Predefinito per datetime e smalldatetime|mes gg aaaa hh:miAM (o PM)|  
|**1**|**101**|U.S.|  1 = mm/gg/aa<br /> 101 = mm/gg/aaaa|  
|**2**|**102**|ANSI|  2 = aa.mm.gg<br /> 102 = aaaa.mm.gg|  
|**3**|**103**|Inglese Regno Unito/Francese|  3 = gg/mm/aa<br /> 103 = gg/mm/aaaa|  
|**4**|**104**|Tedesco|  4 = gg.mm.aa<br /> 104 = gg.mm.aaaa|  
|**5**|**105**|Italiano|  5 = gg-mm-aa<br /> 105 = gg-mm-aaaa|  
|**6**|**106** <sup>(1)</sup>|-|  6 = gg mes aa<br /> 106 = gg mes aaaa|  
|**7**|**107** <sup>(1)</sup>|-|  7 = Mes gg, aa<br /> 107 = Mes gg, aaaa|  
|**8** o **24**|**108**|-|hh:mi:ss|  
|-|**9** o **109** (<sup>1,</sup><sup>2</sup>)|Valore predefinito + millisecondi|mes gg aaaa hh:mi:ss:mmmAM (o PM)|  
|**10**|**110**|USA| 10 = mm-gg-aa<br /> 110 = mm-gg-aaaa|  
|**11**|**111**|Giappone| 11 = aa/mm/gg<br /> 111 = aaaa/mm/gg|  
|**12**|**112**|ISO| 12 = aammgg<br /> 112 = aaaammgg|  
|-|**13** o **113** (<sup>1,</sup><sup>2</sup>)|Valore predefinito Europa + millisecondi|gg mes aaaa hh:mi:ss:mmm (24h)|  
|**14**|**114**|-|hh:mi:ss:mmm (24h)|  
|-|**20** o **120** (<sup>2</sup>)|ODBC canonico|aaaa-mm-gg hh:mi:ss (24h)|  
|-|**21** o **25** o **121** (<sup>2</sup>)|ODBC canonico (con millisecondi) predefinito per time, date, datetime2 e datetimeoffset|aaaa-mm-gg hh:mi:ss.mmm (24h)|  
|**22**|-|U.S.| mm/gg/aa hh:mi:ss AM (o PM)|
|-|**23**|ISO8601|aaaa-mm-gg|
|-|**126** (<sup>4</sup>)|ISO8601|aaaa-mm-ggThh:mi:ss.mmm (senza spazi)<br /><br /> **Nota:** per un valore in millisecondi (mmm) pari a 0, il valore della frazione decimale in millisecondi non verrà visualizzato. Ad esempio, il valore "2012-11-07T18:26:20.000" viene visualizzato come "2012-11-07T18:26:20".| 
|-|**127**(<sup>6, 7</sup>)|ISO8601 con fuso orario Z.|aaaa-MM-ggThh:mm:ss.fffZ (senza spazi)<br /><br /> **Nota:** per un valore in millisecondi (mmm) pari a 0, il valore decimale in millisecondi non verrà visualizzato. Ad esempio, il valore "2012-11-07T18:26:20.000" viene visualizzato come "2012-11-07T18:26:20".|  
|-|**130** (<sup>1,</sup><sup>2</sup>)|Hijri (<sup>5</sup>)|gg mes aaaa hh:mi:ss:mmmAM<br /><br /> In questo stile, **mes** è una rappresentazione unicode multi token Hijri del nome completo del mese. Questo valore non è reso correttamente in un'installazione US predefinita di SSMS.|  
|-|**131** (<sup>2</sup>)|Hijri (<sup>5</sup>)|gg/mm/aaaa hh:mi:ss:mmmAM|  
  
<sup>1</sup> Questi valori di stile restituiscono risultati non deterministici. Include tutti gli stili (aa) (senza secolo) e un subset di stili (aaaa) (con il secolo).
  
<sup>2</sup> I valori predefiniti (**0** o **100**, **9** o **109**, **13** o **113**, **20** o **120**, **23** e **21** o **25** o **121**) restituiscono sempre il secolo (aaaa).

<sup>3</sup> Input quando viene eseguita la conversione nel tipo di dati **datetime**, output quando viene eseguita la conversione in dati di tipo carattere.

<sup>4</sup> Progettato per l'uso in XML. Per le conversioni di dati di tipo **datetime** o **smalldatetime** in dati di tipo carattere, vedere la tabella precedente per il formato di output.

<sup>5</sup> Hijri è un sistema di calendario con diverse variazioni. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene usato l'algoritmo kuwaitiano.

> [!IMPORTANT]
>  Per impostazione predefinita, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] interpreta anni a due cifre in base a un anno di cambio data pari a 2049, Ciò significa che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] interpreta l'anno a due cifre 49 come 2049 e l'anno a due cifre 50 come 1950. Numerose applicazioni client, incluse quelle basate su oggetti di automazione, usano il 2030 come anno di cambio data. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] specifica l'opzione di configurazione del cambio data per anno a due cifre per modificare l'anno di cambio data usato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ciò consente una gestione coerente delle date. È consigliabile specificare l'anno nel formato a quattro cifre.

<sup>6</sup> Supportato solo per il cast di dati dal tipo carattere al tipo di dati **datetime** o **smalldatetime**. Quando si esegue il cast dei dati di tipo carattere che rappresentano solo componenti di data o di ora al tipo di dati **datetime** o **smalldatetime**, il componente di ora non specificato viene impostato su 00:00:00.000 e il componente di data non specificato viene impostato su 01-01-1900.
  
<sup>7</sup> Usare l'indicatore di fuso orario facoltativo **Z** per semplificare il mapping tra i valori **datetime** XML con informazioni sul fuso orario e i valori **datetime** di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] senza fuso orario. Z indica il fuso orario UTC-0. Il valore di offset HH:MM nella direzione + o - indica altri fusi orari. Ad esempio: `2006-12-12T23:45:12-08:00`.
  
Quando si convertono dati di tipo **smalldatetime** in dati di tipo carattere, gli stili che includono secondi o millisecondi visualizzano una serie di zeri. Quando si esegue la conversione da valori **datetime** o **smalldatetime**, usare la lunghezza appropriata per il tipo di dati **char** o **varchar** per troncare le parti della data superflue.
  
Quando si esegue la conversione di dati di tipo carattere in **datetimeoffset** usando uno stile che include un'ora, al risultato viene aggiunta una differenza di fuso orario.
  
## <a name="float-and-real-styles"></a>Stili float e real
Se il tipo di dati di *expression* è **float** o **real**, *style* può avere uno dei valori indicati nella tabella seguente. Gli altri valori vengono elaborati come 0.
  
|valore|Output|  
|---|---|
|**0** (predefinito)|Al massimo 6 cifre. Usare questo valore nella notazione scientifica, quando è appropriato.|  
|**1**|Sempre 8 cifre. Usare questo valore nella notazione scientifica.|  
|**2**|Sempre 16 cifre. Usare questo valore nella notazione scientifica.|  
|**3**|Sempre 17 cifre. Usare per la conversione senza perdita di dati. Con questo stile, è garantita la conversione di ogni float Distinct in una stringa di caratteri Distinct.<br /><br /> **Si applica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|  
|**126, 128, 129**|Incluso per motivi di compatibilità con le versioni precedenti. Questi valori potrebbero essere deprecati in una versione futura.|  
  
## <a name="money-and-smallmoney-styles"></a>Stili money e smallmoney
Se il tipo di dati di *expression* è **money** o **smallmoney**, *style* può avere uno dei valori indicati nella tabella seguente. Gli altri valori vengono elaborati come 0.
  
|valore|Output|  
|---|---|
|**0** (predefinito)|Nessun separatore delle migliaia a sinistra del separatore decimale e due cifre a destra del separatore decimale<br /><br />Esempio: 4235.98.|  
|**1**|Separatore delle migliaia tre cifre a sinistra del separatore decimale e due cifre a destra del separatore decimale<br /><br />Esempio: 3,510.92.|  
|**2**|Nessun separatore delle migliaia a sinistra del separatore decimale e quattro cifre a destra del separatore decimale<br /><br />Esempio: 4235.9819.|  
|**126**|Equivalente allo stile 2 in caso di conversione in char(n) o varchar(n)|  
  
## <a name="xml-styles"></a>Stili xml
Se il tipo di dati di *expression* è **xml**, *style* può avere uno dei valori indicati nella tabella seguente. Gli altri valori vengono elaborati come 0.
  
|valore|Output|  
|---|---|
|**0** (predefinito)|Viene usato il comportamento di analisi predefinito, ovvero vengono eliminati gli spazi vuoti non significativi e non vengono consentiti subset DTD interni.<br /><br />**Nota:** quando si esegue la conversione nel tipo di dati **xml**, gli spazi vuoti non significativi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono gestiti diversamente rispetto a XML 1.0. Per altre informazioni, vedere [Creare istanze di dati XML](../../relational-databases/xml/create-instances-of-xml-data.md).|  
|**1**|Mantiene gli spazi vuoti non significativi. Con questa impostazione dello stile, il comportamento predefinito di **xml:space** è equivalente a **xml:space="preserve"** .|  
|**2**|Abilita l'elaborazione limitata di subset DTD interni.<br /><br /> Se abilitata, il server può usare le informazioni seguenti disponibili in un subset DTD intero per eseguire operazioni di analisi senza convalida.<br /><br />   - Vengono applicati i valori predefiniti per gli attributi<br />   - I riferimenti alle entità interne vengono risolti ed espansi<br />   - Viene controllata la correttezza della sintassi del modello di contenuti DTD<br /><br /> Il parser ignora i subset DTD esterni. Inoltre, non valuta la dichiarazione XML per verificare se l'attributo **standalone** ha un valore **yes** o **no**. Analizza invece l'istanza XML come documento autonomo.|  
|**3**|Mantiene gli spazi non significativi e consente l'elaborazione limitata di subset DTD interni.|  
  
## <a name="binary-styles"></a>Stili binary
Se il tipo di dati di *expression* è **binary(n)** , **char(n)** , **varbinary(n)** o **varchar(n)** , *style* può avere uno dei valori indicati nella tabella seguente. I valori dello stile non indicati nella tabella restituiscono un errore.
  
|valore|Output|  
|---|---|
|**0** (predefinito)|Converte caratteri ASCII in byte binari e viceversa. Ogni carattere o byte viene convertito in base allo schema 1:1.<br /><br /> Se *data-type* è binario, a sinistra del risultato vengono aggiunti i caratteri 0x.|  
|**1**, **2**|Se *data_tye* è binario, l'espressione deve essere un'espressione di caratteri. *expression* deve includere un numero **pari** di cifre esadecimali (0, 1, 2, 3, 4, 5, 6, 7, 8, 9, A, B, C, D, E, F, a, b, c, d, e, f). Se *style* è impostato su 1, i primi due caratteri dell'espressione devono essere 0x. Se l'espressione contiene un numero di caratteri dispari o se un carattere qualsiasi non è valido, viene generato un errore.<br /><br /> Se la lunghezza dell'espressione convertita supera la lunghezza di *data_type*, il risultato viene troncato a destra.<br /><br /> A *data_type* a lunghezza fissa maggiori del risultato convertito vengono aggiunti zeri a destra del risultato.<br /><br /> Un *data_type* di tipo carattere richiede un'espressione binaria. Ogni carattere binario viene convertito in due caratteri esadecimali. Si supponga che la lunghezza dell'espressione convertita superi la lunghezza di *data_type*. In questo caso verrà troncata.<br /><br /> Se *data_type* è un tipo di carattere di dimensioni fisse e la lunghezza del risultato convertito è inferiore a quella di *data_type*, a destra dell'espressione convertita vengono aggiunti spazi per mantenere un numero pari di cifre esadecimali.<br /><br /> Se *style* è uguale a 2, a sinistra del risultato convertito non vengono aggiunti caratteri 0x.|  
  
## <a name="implicit-conversions"></a>Conversioni implicite
Per le conversioni implicite non è necessario specificare la funzione CAST o la funzione CONVERT. Per le conversioni esplicite è necessario specificare la funzione CAST o la funzione CONVERT. Nella figura seguente vengono illustrate le conversioni di tipi di dati esplicite e implicite consentite per i tipi di dati di sistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questi includono **bigint**, **sql_variant** e **xml**. Non è possibile eseguire una conversione implicita in un'assegnazione dal tipo di dati **sql_variant**, ma è possibile eseguire una conversione implicita verso il tipo di dati **sql_variant**.
  
> [!TIP]  
> Questo grafico è disponibile nell'[Area download Microsoft](https://www.microsoft.com/download/details.aspx?id=35834) come file PNG scaricabile.  
  
![Tabella di conversione dei tipi di dati](../../t-sql/data-types/media/lrdatahd.png "Tabella di conversione dei tipi di dati")
  
Il grafico sopra riportato illustra tutte le conversioni esplicite e implicite consentite in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ma il tipo di dati risultante della conversione dipende dall'operazione eseguita:

-  Per le conversioni esplicite, l'istruzione stessa determina il tipo di dati risultante.    
-  Per le conversioni implicite, le istruzioni di assegnazione, ad esempio l'impostazione del valore di una variabile o l'inserimento di un valore in una colonna, hanno come risultato il tipo di dati definito dalla dichiarazione di variabile o dalla definizione di colonna.    
-  Per gli operatori di confronto o altre espressioni, il tipo di dati risultante dipenderà dalle regole di [precedenza dei tipi di dati](../../t-sql/data-types/data-type-precedence-transact-sql.md).

> [!TIP]
> Un esempio pratico degli [effetti della precedenza dei tipi di dati nelle conversioni](#precedence-example) è disponibile più avanti in questa sezione.

Quando si esegue la conversione tra **datetimeoffset** e i tipi di dati di carattere **char**, **nchar**, **nvarchar** e **varchar**, la parte relativa alla differenza di fuso orario convertita deve essere sempre costituita da cifre doppie sia per HH che per MM. Ad esempio, -08:00.
  
> [!NOTE]   
> Poiché i dati Unicode usano sempre un numero pari di byte, prestare attenzione nella conversione tra dati di tipo **binary** o **varbinary** e tipi di dati supportati da Unicode. La conversione seguente, ad esempio, non restituisce il valore esadecimale 41. Restituisce un valore esadecimale di 4100: `SELECT CAST(CAST(0x41 AS nvarchar) AS varbinary)`. Per altre informazioni, vedere [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md). 
  
## <a name="large-value-data-types"></a>Tipi di dati per valori di grandi dimensioni
Per quanto riguarda le conversioni implicite ed esplicite, i tipi di dati per valori di grandi dimensioni si comportano come i tipi di dati per valori di dimensioni minori, in particolare i tipi di dati **nvarchar**, **varbinary** e **varchar**. Considerare tuttavia queste indicazioni:
-   La conversione di **image** in **varbinary(max)** e viceversa funziona come conversione implicita, come le conversioni tra **text** e **varchar(max)** e **ntext** e **nvarchar(max)** .  
-   La conversione di tipi di dati per valori di grandi dimensioni, ad esempio **varchar(max)** , in tipi di dati per valori di dimensioni minori, ad esempio **varchar**, è una conversione implicita, ma si verifica un troncamento se la dimensione del valore più grande supera la lunghezza specificata per il tipo di dati più piccolo.  
-   La conversione di **varchar**, **nvarchar** o **varbinary** nei corrispondenti tipi di dati per valori di grandi dimensioni avviene in modo implicito.  
-   La conversione del tipo di dati **sql_variant** in tipi di dati per valori di grandi dimensioni è una conversione esplicita.  
-   Non è possibile convertire i tipi di dati per valori di grandi dimensioni nel tipo di dati **sql_variant**.  
  
Per altre informazioni sulla conversione del tipo di dati **xml**, vedere [Creare istanze di dati XML](../../relational-databases/xml/create-instances-of-xml-data.md).
  
## <a name="xml-data-type"></a>Tipo di dati XML.
Quando si esegue il cast esplicito o implicito del tipo di dati **xml** a un tipo string o binary, il contenuto del tipo di dati **xml** viene serializzato in base a un set di regole. Per informazioni su queste regole, vedere [Definire la serializzazione di dati XML](../../relational-databases/xml/define-the-serialization-of-xml-data.md). Per informazioni sulla conversione di altri tipi di dati nel tipo di dati **xml**, vedere [Creare istanze di dati XML](../../relational-databases/xml/create-instances-of-xml-data.md).
  
## <a name="text-and-image-data-types"></a>Tipi di dati text e image
I tipi di dati **text** e **image** non supportano la conversione automatica del tipo di dati. È possibile convertire in modo esplicito dati di tipo **text** in dati di tipo carattere e dati di tipo **image** in dati di tipo **binary** o **varbinary**, ma la lunghezza massima è di 8000 byte. Se si tenta di eseguire una conversione non corretta, ad esempio convertire un'espressione di tipo carattere contenente lettere in un tipo **int**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce un messaggio di errore.
  
## <a name="output-collation"></a>Regole di confronto per l'output  
Quando l'output delle funzioni CAST o CONVERT è una stringa di caratteri e le funzioni ricevono come input una stringa di caratteri, l'output e l'input hanno le stesse regole di confronto e le stesse etichette delle regole di confronto. Se l'input non è una stringa di caratteri, all'output sono associate le regole di confronto predefinite del database e un'etichetta delle regole di confronto a cui possono essere assegnati valori predefiniti. Per altre informazioni, vedere [Precedenza delle regole di confronto &#40;Transact-SQL&#41;](../../t-sql/statements/collation-precedence-transact-sql.md).
  
Per assegnare all'output regole di confronto diverse, applicare la clausola COLLATE all'espressione risultante della funzione CAST o CONVERT. Ad esempio:
  
`SELECT CAST('abc' AS varchar(5)) COLLATE French_CS_AS`
  
## <a name="truncating-and-rounding-results"></a>Troncamento e arrotondamento dei risultati
Durante la conversione di espressioni di caratteri o binarie (**binary**, **char**, **nchar**, **nvarchar**, **varbinary** o **varchar**) in un'espressione con tipo di dati differente, l'operazione di conversione può troncare i dati di output, visualizzare solo parzialmente i dati di output o restituire un errore. Queste situazioni si verificano se il risultato è troppo breve per essere visualizzato. Le conversioni in dati di tipo **binary**, **char**, **nchar**, **nvarchar**, **varbinary** o **varchar** vengono troncate, ad eccezione delle conversioni riportate nella tabella seguente.
  
|Tipo di dati di origine|Tipo di dati di destinazione|Risultato|  
|---|---|---|
|**int**, **smallint** o **tinyint**|**char**|*|  
||**varchar**|*|  
||**nchar**|E|  
||**nvarchar**|E|  
|**money**, **smallmoney**, **numeric**, **decimal**, **float** o **real**|**char**|E|  
||**varchar**|E|  
||**nchar**|E|  
||**nvarchar**|E|  
  
\* = Risultato di lunghezza insufficiente per essere visualizzato<br /><br />E = Viene restituito un errore perché la lunghezza del risultato è insufficiente per la visualizzazione.
  
In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la restituzione degli stessi valori in versioni diverse è garantita solo per le conversioni rount trip, ovvero le conversioni in cui un tipo di dati viene convertito in un altro tipo di dati e quindi riconvertito nel tipo di dati iniziale. Nell'esempio seguente viene illustrata una conversione di questo tipo:
  
```sql
DECLARE @myval DECIMAL (5, 2);  
SET @myval = 193.57;  
SELECT CAST(CAST(@myval AS VARBINARY(20)) AS DECIMAL(10,5));  
-- Or, using CONVERT  
SELECT CONVERT(DECIMAL(10,5), CONVERT(VARBINARY(20), @myval));  
```  
  
> [!WARNING]  
> Evitare di costruire valori di tipo **binary** e di convertirli in un tipo di dati della categoria dei tipi numerici. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non garantisce che il risultato della conversione di un tipo di dati **decimal** o **numeric** nel tipo **binary** sia uguale in versioni diverse di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
Nell'esempio seguente viene illustrata un'espressione troppo breve per essere visualizzata.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT p.FirstName, p.LastName, SUBSTRING(p.Title, 1, 25) AS Title,
    CAST(e.SickLeaveHours AS char(1)) AS [Sick Leave]  
FROM HumanResources.Employee e JOIN Person.Person p 
    ON e.BusinessEntityID = p.BusinessEntityID  
WHERE NOT e.BusinessEntityID > 5;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```   
FirstName   LastName      Title   Sick Leave
---------   ------------- ------- --------`
Ken         Sanchez       NULL   *
Terri       Duffy         NULL   *
Roberto     Tamburello    NULL   *
Rob         Walters       NULL   *
Gail        Erickson      Ms.    *

(5 row(s) affected)  
```
  
Quando si convertono tipi di dati con un numero di cifre decimali diverso, in alcuni casi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce un valore troncato e in altri restituisce un valore arrotondato. La tabella che segue illustra questo comportamento.
  
|From|A|Comportamento|  
|---|---|---|
|**numeric**|**numeric**|Round|  
|**numeric**|**int**|Truncate|  
|**numeric**|**money**|Round|  
|**money**|**int**|Round|  
|**money**|**numeric**|Round|  
|**float**|**int**|Truncate|  
|**float**|**numeric**|Round<br /><br /> La conversione dei valori **float** che usano come notazione scientifica **decimal** o **numeric** è limitata ai soli valori con precisione a 17 cifre. Tutti i valori con precisione maggiore di 17 vengono arrotondati a zero.|  
|**float**|**datetime**|Round|  
|**datetime**|**int**|Round|  
  
Ad esempio, i valori 10.6496 e -10.6496 potrebbero essere troncati o arrotondati durante la conversione nei tipi **int** o **numeric**:
  
```sql
SELECT  CAST(10.6496 AS INT) as trunc1,
        CAST(-10.6496 AS INT) as trunc2,
        CAST(10.6496 AS NUMERIC) as round1,
        CAST(-10.6496 AS NUMERIC) as round2;
 ```
I risultati della query sono riportati nella tabella seguente:
 
|trunc1|trunc2|round1|round2|
|---|---|---|---|
|10|-10|11|-11|
 
Se si esegue una conversione di tipi di dati in cui il tipo di destinazione ha un numero di decimali inferiore rispetto al tipo di origine, il valore viene arrotondato. Ad esempio, questa conversione restituisce `$10.3497`:
  
`SELECT CAST(10.3496847 AS money);`
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce un messaggio di errore quando si convertono dati **char**, **nchar**, **varchar** o **nvarchar** non numerici in **int**, **float**, **numeric** o **decimal**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce un errore anche quando una stringa vuota (" ") viene convertita in **numeric** o **decimal**.
  
## <a name="certain-datetime-conversions-are-nondeterministic"></a>Alcune conversioni di data/ora sono non deterministiche

Gli stili per cui la conversione da stringa al tipo datetime è di tipo non deterministico sono i seguenti:
  
- Tutti gli stili inferiori a 100<sup>1</sup>
- 106  
- 107
- 109
- 113
- 130  
  
<sup>1</sup> Ad eccezione degli stili 20 e 21

Per altre informazioni, vedere [Conversione non deterministica di stringhe di valori letterali in valori DATE](../data-types/nondeterministic-convert-date-literals.md).

## <a name="supplementary-characters-surrogate-pairs"></a>Caratteri supplementari (coppie di surrogati)
A partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], quando si usano le regole di confronto per caratteri supplementari (SC), un'operazione CAST da **nchar** o **nvarchar** a un tipo **nchar** o **nvarchar** di lunghezza minore non verrà troncato in una coppia di surrogati. Verrà invece troncato prima del carattere supplementare. Ad esempio, nel frammento di codice seguente `@x` mantiene solo `'ab'`. Lo spazio non è sufficiente per mantenere il carattere supplementare.
  
```sql
DECLARE @x NVARCHAR(10) = 'ab' + NCHAR(0x10000);  
SELECT CAST (@x AS NVARCHAR(3));  
```  
  
Quando si usano le regole di confronto SC, il comportamento di `CONVERT` è analogo a quello di `CAST`. Per altre informazioni, vedere [Regole di confronto e supporto Unicode - Caratteri supplementari](../../relational-databases/collations/collation-and-unicode-support.md#Supplementary_Characters).
  
## <a name="compatibility-support"></a>Informazioni sulla compatibilità
Nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lo stile predefinito per le operazioni CAST e CONVERT nei tipi di dati **time** e **datetime2** è 121, tranne quando uno dei due tipi viene usato in un'espressione di colonna calcolata. Per le colonne calcolate, lo stile predefinito è 0. Questo comportamento influisce sulle colonne calcolate quando vengono create o usate nelle query con parametrizzazione automatica o nelle definizioni dei vincoli.
  
Con il [livello di compatibilità](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#compatibility-levels-and-database-engine-upgrades) 110 e superiore, lo stile predefinito per le operazioni CAST e CONVERT sui tipi di dati **time** e **datetime2** è sempre 121. Se una query si basa sul comportamento obsoleto, usare un livello di compatibilità inferiore a 110 oppure specificare in modo esplicito lo stile 0 nella query interessata.

|Valore di [Livello di compatibilità](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#compatibility-levels-and-database-engine-upgrades)|Stile predefinito per CAST e CONVERT<sup>1</sup>|Stile predefinito per la colonna calcolata|  
|------------|------------|------------|
|< **110**|121|0|  
|> = **110**|121|121|  

<sup>1</sup> Ad eccezione delle colonne calcolate

L'aggiornamento del database al livello di compatibilità 110 e superiore non comporta la modifica dei dati utente archiviati su disco. È necessario correggere manualmente questi dati nel modo opportuno. Se ad esempio si usa SELECT INTO per creare una tabella da un'origine che contiene un'espressione di colonna calcolata descritta in precedenza, vengono archiviati i dati (con stile 0), non la definizione della colonna calcolata. Sarà necessario aggiornare manualmente questi dati in base allo stile 121.
  
## <a name="examples"></a><a name="BKMK_examples"></a> Esempi  
  
### <a name="a-using-both-cast-and-convert"></a>R. Utilizzo delle funzioni CAST e CONVERT  
In questi esempi vengono recuperati i nomi dei prodotti il cui prezzo contiene un `3` come prima cifra e i relativi valori di `ListPrice` vengono convertiti nel tipo `int`.
  
```sql
-- Use CAST  
USE AdventureWorks2012;  
GO  
SELECT SUBSTRING(Name, 1, 30) AS ProductName, ListPrice  
FROM Production.Product  
WHERE CAST(ListPrice AS int) LIKE '33%';  
GO  
  
-- Use CONVERT.  
USE AdventureWorks2012;  
GO  
SELECT SUBSTRING(Name, 1, 30) AS ProductName, ListPrice  
FROM Production.Product  
WHERE CONVERT(int, ListPrice) LIKE '33%';  
GO  
```  

[!INCLUDE[ssResult](../../includes/ssresult-md.md)] Il set di risultati di esempio è lo stesso per CAST e CONVERT. 

```
ProductName                    ListPrice
------------------------------ ---------------------
LL Road Frame - Black, 58      337.22
LL Road Frame - Black, 60      337.22
LL Road Frame - Black, 62      337.22
LL Road Frame - Red, 44        337.22
LL Road Frame - Red, 48        337.22
LL Road Frame - Red, 52        337.22
LL Road Frame - Red, 58        337.22
LL Road Frame - Red, 60        337.22
LL Road Frame - Red, 62        337.22
LL Road Frame - Black, 44      337.22
LL Road Frame - Black, 48      337.22
LL Road Frame - Black, 52      337.22
Mountain-100 Black, 38         3374.99
Mountain-100 Black, 42         3374.99
Mountain-100 Black, 44         3374.99
Mountain-100 Black, 48         3374.99
HL Road Front Wheel            330.06
LL Touring Frame - Yellow, 62  333.42
LL Touring Frame - Blue, 50    333.42
LL Touring Frame - Blue, 54    333.42
LL Touring Frame - Blue, 58    333.42
LL Touring Frame - Blue, 62    333.42
LL Touring Frame - Yellow, 44  333.42
LL Touring Frame - Yellow, 50  333.42
LL Touring Frame - Yellow, 54  333.42
LL Touring Frame - Yellow, 58  333.42
LL Touring Frame - Blue, 44    333.42
HL Road Tire                   32.60

(28 rows affected)
```
  
### <a name="b-using-cast-with-arithmetic-operators"></a>B. Utilizzo della funzione CAST con operatori aritmetici  
In questo esempio viene eseguito il calcolo di una sola colonna (`Computed`) dividendo il totale delle vendite dell'anno in corso (`SalesYTD`) per la percentuale di commissione (`CommissionPCT`). Questo valore viene arrotondato al numero intero più vicino e viene quindi eseguito il CAST del valore a un tipo di dati `int`.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT CAST(ROUND(SalesYTD/CommissionPCT, 0) AS INT) AS Computed  
FROM Sales.SalesPerson   
WHERE CommissionPCT != 0;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
```  
Computed
------
379753754
346698349
257144242
176493899
281101272
0  
301872549
212623750
298948202
250784119
239246890
101664220
124511336
97688107

(14 row(s) affected)  
```  
  
### <a name="c-using-cast-to-concatenate"></a>C. Utilizzo della funzione CAST per la concatenazione  
In questo esempio vengono concatenate espressioni non di tipo carattere usando CAST. Viene usato il database AdventureWorksDW.
  
```sql
SELECT 'The list price is ' + CAST(ListPrice AS VARCHAR(12)) AS ListPrice  
FROM dbo.DimProduct  
WHERE ListPrice BETWEEN 350.00 AND 400.00;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
ListPrice
------------------------
The list price is 357.06
The list price is 364.09
The list price is 364.09
The list price is 364.09
The list price is 364.09  
```  
  
### <a name="d-using-cast-to-produce-more-readable-text"></a>D. Utilizzo di CAST per migliorare la leggibilità del testo  
Questo esempio usa CAST nell'elenco SELECT per convertire la colonna `Name` in una colonna **char(10)** . Viene usato il database AdventureWorksDW.
  
```sql
SELECT DISTINCT CAST(EnglishProductName AS CHAR(10)) AS Name, ListPrice  
FROM dbo.DimProduct  
WHERE EnglishProductName LIKE 'Long-Sleeve Logo Jersey, M';  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Name        ListPrice
----------  ---------
Long-Sleev  31.2437
Long-Sleev  32.4935
Long-Sleev  49.99  
```  
  
### <a name="e-using-cast-with-the-like-clause"></a>E. Utilizzo di CAST con la clausola LIKE  
Questo esempio converte nella colonna `money` i valori `SalesYTD` nel tipo di dati `int`e quindi nel tipo di dati`char(20)`, in modo che possano essere usati dalla clausola `LIKE`.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT p.FirstName, p.LastName, s.SalesYTD, s.BusinessEntityID  
FROM Person.Person AS p   
JOIN Sales.SalesPerson AS s   
    ON p.BusinessEntityID = s.BusinessEntityID  
WHERE CAST(CAST(s.SalesYTD AS INT) AS char(20)) LIKE '2%';  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
FirstName        LastName            SalesYTD         BusinessEntityID
---------------- ------------------- ---------------- -------------
Tsvi             Reiter              2811012.7151      279
Syed             Abbas               219088.8836       288
Rachel           Valdez              2241204.0424      289

(3 row(s) affected)  
```
  
### <a name="f-using-convert-or-cast-with-typed-xml"></a>F. Utilizzo di CONVERT o CAST con XML tipizzato  
Questi esempi spiegano come usare CONVERT per convertire i dati in XML tipizzato usando [Colonne e tipo di dati XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-type-and-columns-sql-server.md).
  
In questo esempio una stringa con spazi vuoti, testo e markup viene convertita in XML tipizzato e vengono rimossi tutti gli spazi non significativi (spazi vuoti limite tra i nodi):
  
```sql
SELECT CONVERT(XML, '<root><child/></root>')  
```  
  
In questo esempio una stringa simile con spazi vuoti, testo e markup viene convertita in XML tipizzato e vengono mantenuti gli spazi vuoti non significativi (spazi vuoti limite tra i nodi):
  
```sql
SELECT CONVERT(XML, '<root>          <child/>         </root>', 1)  
```  
  
In questo esempio viene eseguito il cast di una stringa con spazi vuoti, testo e markup in XML tipizzato:
  
```sql
SELECT CAST('<Name><FName>Carol</FName><LName>Elliot</LName></Name>'  AS XML)  
```  
  
Per altri esempi, vedere [Creare istanze di dati XML](../../relational-databases/xml/create-instances-of-xml-data.md).
  
### <a name="g-using-cast-and-convert-with-datetime-data"></a>G. Utilizzo di CAST e CONVERT con dati datetime  
Iniziando con i valori `GETDATE()`, questo esempio visualizza la data e l'ora correnti, usa `CAST` per modificarle in dati di tipo carattere e quindi usa `CONVERT` per visualizzare data e ora nel formato `ISO 8601`.
  
```sql
SELECT   
   GETDATE() AS UnconvertedDateTime,  
   CAST(GETDATE() AS NVARCHAR(30)) AS UsingCast,  
   CONVERT(nvarchar(30), GETDATE(), 126) AS UsingConvertTo_ISO8601  ;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
UnconvertedDateTime     UsingCast              UsingConvertTo_ISO8601
----------------------- ---------------------- ------------------------------
2006-04-18 09:58:04.570 Apr 18 2006  9:58AM    2006-04-18T09:58:04.570

(1 row(s) affected)  
```
  
Questo esempio rappresenta all'incirca l'opposto dell'esempio precedente. Nell'esempio vengono visualizzate una data e un'ora come dati di tipo carattere, viene usato `CAST` per modificare i dati di tipo carattere nel tipo di dati `datetime` e quindi viene usato `CONVERT` per modificare i dati nel tipo di dati `datetime`.
  
```sql
SELECT   
   '2006-04-25T15:50:59.997' AS UnconvertedText,  
   CAST('2006-04-25T15:50:59.997' AS datetime) AS UsingCast,  
   CONVERT(datetime, '2006-04-25T15:50:59.997', 126) AS UsingConvertFrom_ISO8601 ;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
UnconvertedText         UsingCast               UsingConvertFrom_ISO8601
----------------------- ----------------------- ------------------------
2006-04-25T15:50:59.997 2006-04-25 15:50:59.997 2006-04-25 15:50:59.997

(1 row(s) affected)  
```
  
### <a name="h-using-convert-with-binary-and-character-data"></a>H. Utilizzo di CONVERT con dati binari e di tipo carattere  
In questi esempi vengono illustrati i risultati della conversione di dati binari e di tipo carattere utilizzando stili diversi.
  
```sql
--Convert the binary value 0x4E616d65 to a character value.  
SELECT CONVERT(CHAR(8), 0x4E616d65, 0) AS [Style 0, binary to character];  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Style 0, binary to character
----------------------------
Name  

(1 row(s) affected)  
```
 
Questo esempio indica che Style 1 è in grado di forzare il troncamento del risultato. I caratteri 0x nel set di risultati forzano il troncamento.  
```sql  
SELECT CONVERT(CHAR(8), 0x4E616d65, 1) AS [Style 1, binary to character];  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Style 1, binary to character
------------------------------
0x4E616D

(1 row(s) affected)  
```  
 
L'esempio seguente indica che Style 2 non tronca il risultato perché i caratteri 0x non sono inclusi nel risultato.  
```sql  
SELECT CONVERT(CHAR(8), 0x4E616d65, 2) AS [Style 2, binary to character];  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Style 2, binary to character
------------------------------
4E616D65

(1 row(s) affected)  
```
  
Convertire il valore di carattere 'Name' in valore binario.  
```sql
SELECT CONVERT(BINARY(8), 'Name', 0) AS [Style 0, character to binary];  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Style 0, character to binary
----------------------------
0x4E616D6500000000

(1 row(s) affected)  
```
  
```sql
SELECT CONVERT(BINARY(4), '0x4E616D65', 1) AS [Style 1, character to binary];  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Style 1, character to binary
---------------------------- 
0x4E616D65

(1 row(s) affected)  
```  

```sql
SELECT CONVERT(BINARY(4), '4E616D65', 2) AS [Style 2, character to binary];  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Style 2, character to binary  
----------------------------------  
0x4E616D65

(1 row(s) affected)  
```  
  
### <a name="i-converting-date-and-time-data-types"></a>I. Conversione dei tipi di dati date e time  
Questo esempio illustra la conversione dei tipi di dati date, time e datetime.
  
```sql
DECLARE @d1 DATE, @t1 TIME, @dt1 DATETIME;  
SET @d1 = GETDATE();  
SET @t1 = GETDATE();  
SET @dt1 = GETDATE();  
SET @d1 = GETDATE();  
-- When converting date to datetime the minutes portion becomes zero.  
SELECT @d1 AS [DATE], CAST (@d1 AS DATETIME) AS [date as datetime];  
-- When converting time to datetime the date portion becomes zero   
-- which converts to January 1, 1900.  
SELECT @t1 AS [TIME], CAST (@t1 AS DATETIME) AS [time as datetime];  
-- When converting datetime to date or time non-applicable portion is dropped.  
SELECT @dt1 AS [DATETIME], CAST (@dt1 AS DATE) AS [datetime as date], 
   CAST (@dt1 AS TIME) AS [datetime as time];  
```  

### <a name="j-using-convert-with-datetime-data-in-different-formats"></a>J. Uso di CONVERT con dati datetime in formati diversi  
A partire da valori `GETDATE()`, questo esempio usa `CONVERT` per visualizzare tutti gli stili di data e ora nella sezione [Stili di data e ora](#date-and-time-styles) di questo articolo.

|N. di formato|Query di esempio|Risultato di esempio|
|--------|------------------------------------|------------------|
|0|`SELECT CONVERT(NVARCHAR, GETDATE(), 0)`|23 ago 2019  1:39PM|
|1|`SELECT CONVERT(NVARCHAR, GETDATE(), 1)`|08/23/19|
|2|`SELECT CONVERT(NVARCHAR, GETDATE(), 2)`|19.08.23|
|3|`SELECT CONVERT(NVARCHAR, GETDATE(), 3)`|23/08/19|
|4|`SELECT CONVERT(NVARCHAR, GETDATE(), 4)`|23.08.19|
|5|`SELECT CONVERT(NVARCHAR, GETDATE(), 5)`|23-08-19|
|6|`SELECT CONVERT(NVARCHAR, GETDATE(), 6)`|23 ago 19|
|7|`SELECT CONVERT(NVARCHAR, GETDATE(), 7)`|Ago 23, 19|
|8 o 24 o 108|`SELECT CONVERT(NVARCHAR, GETDATE(), 8)`|13:39:17|
|9 o 109|`SELECT CONVERT(NVARCHAR, GETDATE(), 9)`|Ago 23 2019  1:39:17:090PM|
|10|`SELECT CONVERT(NVARCHAR, GETDATE(), 10)`|08-23-19|
|11|`SELECT CONVERT(NVARCHAR, GETDATE(), 11)`|19/08/23|
|12|`SELECT CONVERT(NVARCHAR, GETDATE(), 12)`|190823|
|13 o 113|`SELECT CONVERT(NVARCHAR, GETDATE(), 13)`|23 ago 2019 13:39:17:090|
|14 o 114|`SELECT CONVERT(NVARCHAR, GETDATE(), 14)`|13:39:17:090|
|20 o 120|`SELECT CONVERT(NVARCHAR, GETDATE(), 20)`|2019-08-23 13:39:17|
|21 o 25 o 121|`SELECT CONVERT(NVARCHAR, GETDATE(), 21)`|2019-08-23 13:39:17.090|
|22|`SELECT CONVERT(NVARCHAR, GETDATE(), 22)`|08/23/19  1:39:17 PM|
|23|`SELECT CONVERT(NVARCHAR, GETDATE(), 23)`|2019-08-23|
|101|`SELECT CONVERT(NVARCHAR, GETDATE(), 101)`|08/23/2019|
|102|`SELECT CONVERT(NVARCHAR, GETDATE(), 102)`|2019.08.23|
|103|`SELECT CONVERT(NVARCHAR, GETDATE(), 103)`|23/08/2019|
|104|`SELECT CONVERT(NVARCHAR, GETDATE(), 104)`|23.08.2019|
|105|`SELECT CONVERT(NVARCHAR, GETDATE(), 105)`|23-08-2019|
|106|`SELECT CONVERT(NVARCHAR, GETDATE(), 106)`|23 ago 2019|
|107|`SELECT CONVERT(NVARCHAR, GETDATE(), 107)`|Ago 23, 2019|
|110|`SELECT CONVERT(NVARCHAR, GETDATE(), 110)`|08-23-2019|
|111|`SELECT CONVERT(NVARCHAR, GETDATE(), 111)`|2019/08/23|
|112|`SELECT CONVERT(NVARCHAR, GETDATE(), 112)`|20190823|
|113|`SELECT CONVERT(NVARCHAR, GETDATE(), 113)`|23 ago 2019 13:39:17.090|
|120|`SELECT CONVERT(NVARCHAR, GETDATE(), 120)`|2019-08-23 13:39:17|
|121|`SELECT CONVERT(NVARCHAR, GETDATE(), 121)`|2019-08-23 13:39:17.090|
|126|`SELECT CONVERT(NVARCHAR, GETDATE(), 126)`|2019-08-23T13:39:17.090|
|127|`SELECT CONVERT(NVARCHAR, GETDATE(), 127)`|2019-08-23T13:39:17.090|
|130|`SELECT CONVERT(NVARCHAR, GETDATE(), 130)`|22 ذو الحجة 1440  1:39:17.090P|
|131|`SELECT CONVERT(NVARCHAR, GETDATE(), 131)`|22/12/1440  1:39:17.090PM|

### <a name="k-effects-of-data-type-precedence-in-allowed-conversions"></a><a name="precedence-example"></a> K. Effetti della precedenza dei tipi di dati nelle conversioni consentite  
L'esempio seguente definisce una variabile di tipo VARCHAR, assegna un valore integer alla variabile e quindi seleziona una concatenazione della variabile con una stringa.

```sql
DECLARE @string VARCHAR(10);
SET @string = 1;
SELECT @string + ' is a string.' AS Result
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Result
-----------------------
1 is a string.
```  

Il valore int di 1 è stato convertito in VARCHAR.

In questo esempio viene illustrata una query simile, usando invece una variabile int:

```sql
DECLARE @notastring INT;
SET @notastring = '1';
SELECT @notastring + ' is not a string.' AS Result
```

In questo caso, l'istruzione SELECT genererà l'errore seguente:

```
Msg 245, Level 16, State 1, Line 3
Conversion failed when converting the varchar value ' is not a string.' to data type int.
```

Per valutare l'espressione `@notastring + ' is not a string.'`, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deve seguire le regole di precedenza dei tipi di dati per completare la conversione implicita prima che il risultato dell'espressione possa essere calcolato. Dato che int ha una precedenza maggiore di VARCHAR, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tenta di convertire la stringa in un intero e non riesce perché la stringa non può essere convertita in un intero. 

Se si specifica una stringa che può essere convertita, l'istruzione ha esito positivo, come nell'esempio seguente:

```SQL
DECLARE @notastring INT;
SET @notastring = '1';
SELECT @notastring + '1'
```

In questo caso, la stringa `'1'` può essere convertita nel valore intero 1, quindi questa istruzione SELECT restituirà il valore 2. Quando i tipi di dati forniti sono integer, l'operatore + diventa operatore matematico di addizione, anziché una concatenazione di stringhe.

## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="l-using-cast-and-convert"></a>L. Uso di CAST e CONVERT  
Questo esempio recupera il nome dei prodotti il cui prezzo contiene un `3` come prima cifra e converte il valore `ListPrice` di questi prodotti nel tipo **int**. Usa il database `AdventureWorksDW2016`.
  
```sql
SELECT EnglishProductName AS ProductName, ListPrice  
FROM dbo.DimProduct  
WHERE CAST(ListPrice AS int) LIKE '3%';  
```  
  
Questo esempio illustra la stessa query in cui viene usato CONVERT anziché CAST. Usa il database `AdventureWorksDW2016`.
  
```sql
SELECT EnglishProductName AS ProductName, ListPrice  
FROM dbo.DimProduct  
WHERE CONVERT(INT, ListPrice) LIKE '3%';  
```  
  
### <a name="m-using-cast-with-arithmetic-operators"></a>M. Utilizzo della funzione CAST con operatori aritmetici  
Questo esempio calcola un unico valore di colonna dividendo il prezzo unitario del prodotto (`UnitPrice`) per la percentuale di sconto (`UnitPriceDiscountPct`). Il risultato viene arrotondato al numero intero più prossimo e infine convertito in un tipo di dati `int`. In questo esempio viene utilizzato il database `AdventureWorksDW2016`.
  
```sql
SELECT ProductKey, UnitPrice,UnitPriceDiscountPct,  
       CAST(ROUND (UnitPrice*UnitPriceDiscountPct,0) AS int) AS DiscountPrice  
FROM dbo.FactResellerSales  
WHERE SalesOrderNumber = 'SO47355'   
      AND UnitPriceDiscountPct > .02;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
ProductKey  UnitPrice  UnitPriceDiscountPct  DiscountPrice
----------  ---------  --------------------  -------------
323         430.6445   0.05                  22
213         18.5043    0.05                  1
456         37.4950    0.10                  4
456         37.4950    0.10                  4
216         18.5043    0.05                  1  
```  
  
### <a name="n-using-cast-with-the-like-clause"></a>N. Utilizzo di CAST con la clausola LIKE  
Questo esempio converte nella colonna **money** il valore `ListPrice` in un tipo **int** e quindi in un tipo **char(20)** in modo che possa essere usato dalla clausola LIKE. In questo esempio viene utilizzato il database `AdventureWorksDW2016`.  
  
```sql
SELECT EnglishProductName AS Name, ListPrice  
FROM dbo.DimProduct  
WHERE CAST(CAST(ListPrice AS INT) AS CHAR(20)) LIKE '2%';  
```  
  
### <a name="o-using-cast-and-convert-with-datetime-data"></a>O. Utilizzo di CAST e CONVERT con dati datetime  
Questo esempio visualizza la data e l'ora correnti, usa CAST per modificarle in dati di tipo carattere e quindi usa CONVERT per visualizzarle nel formato ISO 8601. In questo esempio viene utilizzato il database `AdventureWorksDW2016`.
  
```sql
SELECT TOP(1)  
   SYSDATETIME() AS UnconvertedDateTime,  
   CAST(SYSDATETIME() AS NVARCHAR(30)) AS UsingCast,  
   CONVERT(NVARCHAR(30), SYSDATETIME(), 126) AS UsingConvertTo_ISO8601  
FROM dbo.DimCustomer;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
UnconvertedDateTime     UsingCast                     UsingConvertTo_ISO8601  
---------------------   ---------------------------   ---------------------------  
07/20/2010 1:44:31 PM   2010-07-20 13:44:31.5879025   2010-07-20T13:44:31.5879025  
```  
  
Questo esempio rappresenta all'incirca l'opposto dell'esempio precedente. L'esempio visualizza una data e un'ora come dati di tipo carattere, usa CAST per modificare i dati di tipo carattere nel tipo di dati **datetime** e quindi usa CONVERT per modificare i dati di tipo carattere nel tipo di dati **datetime**. In questo esempio viene utilizzato il database `AdventureWorksDW2016`.
  
```sql
SELECT TOP(1)   
   '2010-07-25T13:50:38.544' AS UnconvertedText,  
CAST('2010-07-25T13:50:38.544' AS DATETIME) AS UsingCast,  
   CONVERT(DATETIME, '2010-07-25T13:50:38.544', 126) AS UsingConvertFrom_ISO8601  
FROM dbo.DimCustomer;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
UnconvertedText         UsingCast               UsingConvertFrom_ISO8601
----------------------- ----------------------- ------------------------
2010-07-25T13:50:38.544 07/25/2010 1:50:38 PM   07/25/2010 1:50:38 PM  
```  

## <a name="see-also"></a>Vedere anche
[Precedenza dei tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md)       
[Conversione di tipi di dati &#40;motore di database&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)     
[FORMAT &#40;Transact-SQL&#41;](../../t-sql/functions/format-transact-sql.md)      
[STR &#40;Transact-SQL&#41;](../../t-sql/functions/str-transact-sql.md)     
[SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)      
[Funzioni di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)      
[Regole di confronto e supporto Unicode](../../relational-databases/collations/collation-and-unicode-support.md)      
[Scrittura di istruzioni Transact-SQL internazionali](../../relational-databases/collations/write-international-transact-sql-statements.md)       
  
