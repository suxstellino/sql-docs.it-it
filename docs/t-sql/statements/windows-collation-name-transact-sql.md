---
description: Windows_collation_name (Transact-SQL)
title: Nome delle regole di confronto di Windows (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- Windows collations [SQL Server]
- names [SQL Server], collations
- collations [SQL Server], Windows collations
- Collation Designator
ms.assetid: acceef84-2c68-46e2-a021-be019b7ab14e
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ebe10e5195a934ae7921619c5914d65dfbb50eaa
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104733433"
---
# <a name="windows-collation-name-transact-sql"></a>Windows_collation_name (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Specifica il nome delle regole di confronto Windows nella clausola COLLATE in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il nome delle regole di confronto Windows è composto dalla designazione delle regole di confronto e dagli stili di confronto.

![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintassi

```syntaxsql
<Windows_collation_name> :: =
<CollationDesignator>_<ComparisonStyle>

<ComparisonStyle> :: =
{ <CaseSensitivity>_<AccentSensitivity> [ _<KanatypeSensitive> ] [ _<WidthSensitive> ] [ _<VariationSelectorSensitive> ] 
}
| { _UTF8 }
| { _BIN | _BIN2 }
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

*CollationDesignator*   
Specifica le regole alla base delle regole di confronto Windows, ovvero:

- Le regole di ordinamento e confronto applicate quando si specifica l'ordinamento del dizionario. Le regole di ordinamento si basano sull'alfabeto o sulla lingua.
- La tabella codici utilizzata per memorizzare i dati **varchar**.

Ad esempio:

- Latin1\_General o francese: per entrambe viene utilizzata la tabella codici 1252.
- Turco: viene utilizzata la tabella codici 1254.

*CaseSensitivity*  
**CI** specifica che la distinzione tra maiuscole e minuscole non è rilevante, mentre **CS** indica che la differenza tra maiuscole e minuscole è rilevante.

*AccentSensitivity*  
**AI** specifica che la distinzione tra caratteri accentati e non accentati non è rilevante, mentre **AS** indica che la distinzione tra caratteri accentati e non accentati è rilevante.

*KanatypeSensitive*  
Omettendo questa opzione si specifica che la distinzione Kana non è rilevante. **KS** specifica che la distinzione Kana è rilevante.

*WidthSensitivity*  
Omettendo questa opzione si specifica che la distinzione di larghezza non è rilevante. **WS** specifica che la distinzione di larghezza è rilevante.

*VariationSelectorSensitivity*  
- **Si applica a**: A partire da [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)] 

- Omettendo questa opzione si specifica che il selettore di variazione non è rilevante, **VSS** specifica che il selettore di variazione è rilevante.

**UTF8**  
- **Si applica a**: A partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]   

- Specifica la codifica UTF-8 da usare per i tipi di dati idonei. Per altre informazioni, vedere [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md).

**BIN**  
Specifica che deve essere usato il tipo di ordinamento binario compatibile con le versioni precedenti.

**BIN2**  
Specifica l'ordinamento binario che utilizza la semantica del confronto dei punti di codice.

## <a name="remarks"></a>Commenti
A seconda della versione delle regole di confronto, per alcuni punti di codice potrebbero non essere stati definiti pesi di ordinamento e/o il mapping tra caratteri maiuscoli e minuscoli. Ad esempio, confrontare l'output della funzione `LOWER` quando viene assegnato lo stesso carattere, ma con versioni diverse delle stesse regole di confronto:

```sql
SELECT NCHAR(504) COLLATE Latin1_General_CI_AS AS [Uppercase],
       NCHAR(505) COLLATE Latin1_General_CI_AS AS [Lowercase];
-- Ǹ    ǹ


SELECT LOWER(NCHAR(504) COLLATE Latin1_General_CI_AS) AS [Version80Collation],
       LOWER(NCHAR(504) COLLATE Latin1_General_100_CI_AS) AS [Version100Collation];
-- Ǹ    ǹ
```

La prima istruzione mostra sia la forma maiuscola che minuscola di questo carattere nelle regole di confronto precedenti perché le regole di confronto non influenzano la disponibilità dei caratteri quando usano dati Unicode. La seconda istruzione, invece, mostra che viene restituito un carattere maiuscolo quando le regole di confronto sono Latin1\_General\_CI\_AS perché questo punto di codice non presenta un mapping dei caratteri minuscoli definito nelle regole di confronto.

Con alcune lingue può essere fondamentale evitare le regole di confronto precedenti. Ad esempio, questo vale per il telugu.

In alcuni casi, le regole di confronto di Windows e le regole di confronto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] possono generare piani di query diversi per la stessa query.

## <a name="examples"></a>Esempi

Di seguito sono riportati alcuni esempi di nomi delle regole di confronto di Windows:

- **Latin1\_General\_100\_CI\_AS**

  Le regole di confronto utilizzano le regole di ordinamento del dizionario Latin1 General ed eseguono il mapping alla tabella codici 1252. Sono regole di confronto con versione \_100 per cui la distinzione tra maiuscole e minuscole non è rilevante mentre è rilevante la distinzione tra caratteri accentati e non accentati.

- **Estonian\_CS\_AS**

  Le regole di confronto utilizzano le regole di ordinamento del dizionario estone ed eseguono il mapping alla tabella codici 1257. Sono regole di confronto con versione \_80 (indicata implicitamente dalla mancanza del numero di versione nel nome) per cui la distinzione tra maiuscole e minuscole è rilevante la distinzione tra caratteri accentati e non accentati è rilevante.

- **Japanese\_Bushu\_Kakusu\_140\_BIN2**

  Le regole di confronto utilizzano le regole di ordinamento del punto di codice binario ed eseguono il mapping alla tabella codici 932. Sono regole di confronto con versione \_140 e le regole di ordinamento del dizionario Bushu Kakusu giapponese vengono ignorate.

## <a name="windows-collations"></a>Regole di confronto di Windows

Per elencare le regole di confronto di Windows supportate dall'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], eseguire la query seguente.

```sql
SELECT * FROM sys.fn_helpcollations() WHERE [name] NOT LIKE N'SQL%';
```

Nella tabella seguente vengono elencate tutte le regole di confronto di Windows supportate in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].

|Impostazioni locali di Windows|Regole di confronto versione 100|Regole di confronto 90|
|--------------------|---------------------------|--------------------------|
|Alsaziano (Francia)|Latin1_General_100_|Non disponibile|
|Amarico (Etiopia)|Latin1_General_100_|Non disponibile|
|Armeno (Armenia)|Cyrillic_General_100_|Non disponibile|
|Assamese (India)|Assamese_100_ <sup>1</sup>|Non disponibile|
|Bengalese (Bangladesh)|Bengali_100_<sup>1</sup>|Non disponibile|
|Baschiro (Russia)|Bashkir_100_|Non disponibile|
|Basco (Basco)|Latin1_General_100_|Non disponibile|
|Bengalese (India)|Bengali_100_<sup>1</sup>|Non disponibile|
|Bosniaco (Bosnia ed Erzegovina, alfabeto cirillico)|Bosnian_Cyrillic_100_|Non disponibile|
|Bosniaco (Bosnia ed Erzegovina, alfabeto latino)|Bosnian_Latin_100_|Non disponibile|
|Bretone (Francia)|Breton_100_|Non disponibile|
|Cinese (RAS di Macao)|Chinese_Traditional_Pinyin_100_|Non disponibile|
|Cinese (RAS di Macao)|Chinese_Traditional_Stroke_Order_100_|Non disponibile|
|Cinese (Singapore)|Chinese_Simplified_Stroke_Order_100_|Non disponibile|
|Corso (Francia)|Corsican_100_|Non disponibile|
|Croato (Bosnia ed Erzegovina, alfabeto latino)|Croatian_100_|Non disponibile|
|Dari (Afghanistan)|Dari_100_|Non disponibile|
|Inglese (India)|Latin1_General_100_|Non disponibile|
|Inglese (Malaysia)|Latin1_General_100_|Non disponibile|
|Inglese (Singapore)|Latin1_General_100_|Non disponibile|
|Filippino (Filippine)|Latin1_General_100_|Non disponibile|
|Frisone (Paesi Bassi)|Frisian_100_|Non disponibile|
|Georgiano (Georgia)|Cyrillic_General_100_|Non disponibile|
|Groenlandese (Groenlandia)|Danish_Greenlandic_100_|Non disponibile|
|Gujarati (India)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Hausa (Nigeria, alfabeto latino)|Latin1_General_100_|Non disponibile|
|Hindi (India)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Igbo (Nigeria)|Latin1_General_100_|Non disponibile|
|Inuktitut (Canada, alfabeto latino)|Latin1_General_100_|Non disponibile|
|Inuktitut (alfabeto sillabico) Canada|Latin1_General_100_|Non disponibile|
|Irlandese (Irlanda)|Latin1_General_100_|Non disponibile|
|Giapponese (Giappone XJIS)|Japanese_XJIS_100_|Japanese_90_, Japanese_|
|Giapponese (Giappone)|Japanese_Bushu_Kakusu_100_|Non disponibile|
|Kannada (India)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Khmer (Cambogia)|Khmer_100_<sup>1</sup>|Non disponibile|
|Quiché (Guatemala)|Modern_Spanish_100_|Non disponibile|
|Kinyarwanda (Ruanda)|Latin1_General_100_|Non disponibile|
|Konkani (India)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Lao (Repubblica popolare democratica del Laos)|Lao_100_<sup>1</sup>|Non disponibile|
|Basso sorabo (Germania)|Latin1_General_100_|Non disponibile|
|Lussemburghese (Lussemburgo)|Latin1_General_100_|Non disponibile|
|Malayalam (India)|Indic_General_100_<sup>1</sup>|Non disponibile|
|Maltese (Malta)|Maltese_100_|Non disponibile|
|Maori (Nuova Zelanda)|Maori_100_|Non disponibile|
|Mapudungun (Cile)|Mapudungan_100_|Non disponibile|
|Marathi (India)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Mohawk (Canada)|Mohawk_100_|Non disponibile|
|Mongolo (RPC)|Cyrillic_General_100_|Non disponibile|
|Nepalese (Nepal)|Nepali_100_<sup>1</sup>|Non disponibile|
|Norvegese (Bokmål, Norvegia)|Norwegian_100_|Non disponibile|
|Norvegese (Nynorsk, Norvegia)|Norwegian_100_|Non disponibile|
|Occitano (Francia)|French_100_|Non disponibile|
|Odia (India)|Indic_General_100_<sup>1</sup>|Non disponibile|
|Pashto (Afghanistan)|Pashto_100_<sup>1</sup>|Non disponibile|
|Persiano (Iran)|Persian_100_|Non disponibile|
|Punjabi (India)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Quechua (Bolivia)|Latin1_General_100_|Non disponibile|
|Quechua (Ecuador)|Latin1_General_100_|Non disponibile|
|Quechua (Perù)|Latin1_General_100_|Non disponibile|
|Romancio (Svizzera)|Romansh_100_|Non disponibile|
|Sami (Inari, Finlandia)|Sami_Sweden_Finland_100_|Non disponibile|
|Sami (Lule, Norvegia)|Sami_Norway_100_|Non disponibile|
|Sami (Lule, Svezia)|Sami_Sweden_Finland_100_|Non disponibile|
|Sami (settentrionale, Finlandia)|Sami_Sweden_Finland_100_|Non disponibile|
|Sami (settentrionale, Norvegia)|Sami_Norway_100_|Non disponibile|
|Sami (settentrionale, Svezia)|Sami_Sweden_Finland_100_|Non disponibile|
|Sami (Skolt, Finlandia)|Sami_Sweden_Finland_100_|Non disponibile|
|Sami (meridionale, Norvegia)|Sami_Norway_100_|Non disponibile|
|Sami (meridionale, Svezia)|Sami_Sweden_Finland_100_|Non disponibile|
|Sanscrito (India)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Serbo (Bosnia ed Erzegovina, alfabeto cirillico)|Serbian_Cyrillic_100_|Non disponibile|
|Serbo (Bosnia ed Erzegovina, alfabeto latino)|Serbian_Latin_100_|Non disponibile|
|Serbo (Serbia, alfabeto cirillico)|Serbian_Cyrillic_100_|Non disponibile|
|Serbo (Serbia, alfabeto latino)|Serbian_Latin_100_|Non disponibile|
|Sotho del nord/Sotho settentrionale (Sudafrica)|Latin1_General_100_|Non disponibile|
|SeTswana/Tswana (Sudafrica)|Latin1_General_100_|Non disponibile|
|Singalese (Sri Lanka)|Indic_General_100_<sup>1</sup>|Non disponibile|
|Swahili (Kenya)|Latin1_General_100_|Non disponibile|
|Siriano (Siria)|Syriac_100_<sup>1</sup>|Syriac_90_|
|Tagico (Tajikistan)|Cyrillic_General_100_|Non disponibile|
|Tamazight (Algeria, alfabeto latino)|Tamazight_100_|Non disponibile|
|Tamil (India)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Telugu (India)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Tibetano (RPC)|Tibetan_100_<sup>1</sup>|Non disponibile|
|Turcomanno (Turkmenistan)|Turkmen_100_|Non disponibile|
|Uiguro (RPC)|Uighur_100_|Non disponibile|
|Alto sorabo (Germania)|Upper_Sorbian_100_|Non disponibile|
|Urdu (Pakistan)|Urdu_100_|Non disponibile|
|Gallese (Regno Unito)|Welsh_100_|Non disponibile|
|Wolof (Senegal)|French_100_|Non disponibile|
|Xhosa/isiXhosa (Sudafrica)|Latin1_General_100_|Non disponibile|
|Sacha (Russia)|Yakut_100_|Non disponibile|
|Yi (RPC)|Latin1_General_100_|Non disponibile|
|Yoruba (Nigeria)|Latin1_General_100_|Non disponibile|
|Zulu/isiZulu (Sudafrica)|Latin1_General_100_|Non disponibile|
|Deprecato, non disponibile a livello di server in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] o versioni successive|Hindi|Hindi|
|Deprecato, non disponibile a livello di server in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] o versioni successive|Korean_Wansung_Unicode|Korean_Wansung_Unicode|
|Deprecato, non disponibile a livello di server in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] o versioni successive|Lithuanian_Classic|Lithuanian_Classic|
|Deprecato, non disponibile a livello di server in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] o versioni successive|Macedone|Macedone|
||||

<sup>1</sup> Le regole di confronto solo Unicode di Windows possono essere applicate solo a dati a livello di colonna o a livello di espressione. Tali regole non possono essere usate come regole di confronto del server o del database.

<sup>2</sup> Come le regole di confronto per il cinese (Taiwan), il cinese (Macao) usa le regole del cinese semplificato; a differenza del cinese (Taiwan), usa la tabella codici 950.

## <a name="see-also"></a>Vedere anche

- [Regole di confronto e supporto Unicode](../../relational-databases/collations/collation-and-unicode-support.md)
- [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)
- [Costanti](../../t-sql/data-types/constants-transact-sql.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)
- [DECLARE @local_variable](../../t-sql/language-elements/declare-local-variable-transact-sql.md)
- [tabella](../../t-sql/data-types/table-transact-sql.md)
- [sys.fn_helpcollations](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md)
