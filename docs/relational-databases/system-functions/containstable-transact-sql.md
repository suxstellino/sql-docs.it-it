---
description: CONTAINSTABLE (Transact-SQL)
title: CONTAINSTABLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- CONTAINSTABLE
- CONTAINSTABLE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- precise or fuzzy (less precise) matches [full-text search]
- fuzzy (less precise) word or phrase search [full-text search]
- word searches [full-text search]
- weighted values [full-text search]
- values [SQL Server], ranked
- LANGUAGE option
- NEAR option [full-text search]
- RANK column
- phrase searches [full-text search]
- conditions [SQL Server], CONTAINSTABLE
- relevance ranking values [full-text search]
- proximity searches [full-text search]
- CONTAINSTABLE function (Transact-SQL)
- ranked results [full-text search]
- rankings [full-text search]
- less precise (fuzzy) searches [full-text search]
ms.assetid: e580c210-cf57-419d-9544-7f650f2ab814
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0884b34e2da3eaa6a081da8bebaae06f4d16ddc7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196143"
---
# <a name="containstable-transact-sql"></a>CONTAINSTABLE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce una tabella contenente una o più righe o nessuna riga per le colonne che includono corrispondenze più o meno esatte di singole parole e frasi, della prossimità delle parole a una certa distanza l'una dall'altra o di corrispondenze ponderate. CONTAINSTABLE viene utilizzato nella [clausola from](../../t-sql/queries/from-transact-sql.md) di un' [!INCLUDE[tsql](../../includes/tsql-md.md)] istruzione SELECT e viene fatto riferimento a come se fosse un normale nome di tabella. Esegue una ricerca full-text di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in colonne indicizzate full-text che contengono tipi di dati basati su caratteri.  
  
 CONTAINSTABLE è utile per gli stessi tipi di corrispondenze del [predicato CONTAINS](../../t-sql/queries/contains-transact-sql.md) e utilizza le stesse condizioni di ricerca di Contains.  
  
 Tuttavia, a differenza di CONTAINS, le query che utilizzano la funzione CONTAINSTABLE restituiscono un valore di classificazione della pertinenza (RANK) e una chiave full-text (KEY) per ogni riga.  Per informazioni sulle forme di ricerca full-text supportate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Eseguire query con ricerca full-text](../../relational-databases/search/query-with-full-text-search.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
CONTAINSTABLE   
( table , { column_name | ( column_list ) | * } , ' <contains_search_condition> '   
     [ , LANGUAGE language_term]   
  [ , top_n_by_rank ]   
)   
  
<contains_search_condition> ::=   
    { <simple_term>   
    | <prefix_term>   
    | <generation_term>   
    | <generic_proximity_term>   
    | <custom_proximity_term>   
    |  <weighted_term>   
    }   
    | { ( <contains_search_condition> )   
    { { AND | & } | { AND NOT | &! } | { OR | | } }   
     <contains_search_condition> [ ...n ]   
    }  
  
<simple_term> ::=   
     { word | "phrase" }  
<prefix term> ::=   
     { "word*" | "phrase*" }   
<generation_term> ::=   
     FORMSOF ( { INFLECTIONAL | THESAURUS } , <simple_term> [ ,...n ] )   
  
<generic_proximity_term> ::=   
     { <simple_term> | <prefix_term> } { { { NEAR | ~ }   
     { <simple_term> | <prefix_term> } } [ ...n ] }  
  
<custom_proximity_term> ::=   
  NEAR (   
     {  
        { <simple_term> | <prefix_term> } [ ,...n ]  
     |  
        ( { <simple_term> | <prefix_term> } [ ,...n ] )   
      [, <maximum_distance> [, <match_order> ] ]  
     }  
       )   
  
      <maximum_distance> ::= { integer | MAX }  
      <match_order> ::= { TRUE | FALSE }   
  
<weighted_term> ::=   
     ISABOUT  
    ( { {   
  <simple_term>   
  | <prefix_term>   
  | <generation_term>   
  | <proximity_term>   
  }   
   [ WEIGHT ( weight_value ) ]   
   } [ ,...n ]   
    )  
  
```  
  
## <a name="arguments"></a>Argomenti  
 *tabella*  
 Nome di una tabella indicizzata full-text. *Table* può essere un nome di oggetto di database costituito da una, due, tre o quattro parti. L'esecuzione della ricerca in una vista può interessare solo una tabella di base con indicizzazione full-text.  
  
 la *tabella* non può specificare un nome di server e non può essere utilizzata nelle query sui server collegati.  
  
 *column_name*  
 Nome di una o più colonne indicizzate per la ricerca full-text. La colonna o le colonne possono essere di tipo **char**, **varchar**, **nchar**, **nvarchar**, **text**, **ntext**, **image**, **xml**, **varbinary** r **varbinary(max)** .  
  
 *column_list*  
 Viene indicato che è possibile specificare più colonne, separate da virgola. *column_list* deve essere racchiuso tra parentesi. La lingua di tutte le colonne di *column_list* deve essere la stessa, a meno che non sia specificato *language_term*.  
  
 \*  
 Specifica che tutte le colonne con indicizzazione full-text nella *tabella* devono essere utilizzate per cercare la condizione di ricerca specificata. La lingua di tutte le colonne della tabella deve essere la stessa, a meno che non sia specificato *language_term*.  
  
 LANGUAGE *language_term*  
 Lingua le cui risorse verranno utilizzate per la rimozione di parole, stemming e thesaurus e la rimozione di parole non significative (o [parola non significativa](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)) nell'ambito della query. Questo parametro è facoltativo e può essere specificato come valore stringa, intero o esadecimale corrispondente all'identificatore delle impostazioni locali (LCID) di una lingua. Se si specifica *language_term*, la lingua rappresentata dall'argomento verrà applicata a tutti gli elementi della condizione di ricerca. Se non si specifica alcun valore, verrà utilizzata la lingua full-text della colonna.  
  
 Se documenti di lingue diverse vengono archiviati insieme come oggetti BLOB in una singola colonna, l'identificatore delle impostazioni locali (LCID) di un documento specifico determina la lingua da utilizzare per indicizzarne il contenuto. Se quando si esegue una query su una colonna di questo tipo si specifica *LANGUAGE**language_term*, è possibile aumentare la probabilità di una corrispondenza soddisfacente.  
  
 Quando viene specificato come stringa, *language_term* corrisponde al valore della colonna **alias** nella vista di compatibilità dei [ linguaggisys.sys](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md) .  La stringa deve essere racchiusa tra virgolette singole chiuse, come in '*language_term*'. Se l'argomento *language_term* viene specificato come valore intero, corrisponde all'LCID effettivo che identifica la lingua. Se si specifica un valore esadecimale, *language_term* è 0x seguito dal valore esadecimale di LCID. Il valore esadecimale non deve superare le otto cifre, inclusi gli zeri iniziali.  
  
 Se il valore è in formato DBCS (Double-Byte Character Set), [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lo convertirà in Unicode.  
  
 Se la lingua specificata non è valida o non vi sono risorse installate corrispondenti a tale lingua, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce un errore. Per usare le risorse della lingua neutra, specificare 0x0 per *language_term*.  
  
 *top_n_by_rank*  
 Specifica che vengono restituite solo le *n* corrispondenze con classificazione più alta, in ordine decrescente. Si applica solo quando viene specificato un valore integer, *n*. Se il parametro *top_n_by_rank* viene combinato con altri parametri, la query potrebbe restituire un numero inferiore di righe rispetto al numero di righe effettivamente corrispondenti a tutti i predicati. *top_n_by_rank* consente di aumentare le prestazioni delle query richiamando solo i riscontri più rilevanti.  
  
 <contains_search_condition>  
 Specifica il testo da cercare in *column_name* e le condizioni della ricerca. Per informazioni sulle condizioni di ricerca, vedere [contains &#40;&#41;Transact-SQL ](../../t-sql/queries/contains-transact-sql.md).  
  
## <a name="remarks"></a>Commenti  
 I predicati e le funzioni full-text possono essere utilizzati in una singola tabella, specificata in modo implicito nel predicato FROM. Per cercare in più tabelle, utilizzare una tabella unita in join nella clausola FROM, che consente di eseguire una ricerca in un set di risultati prodotto da due o più tabelle.  
  
 La tabella restituita include una colonna denominata **Key** che contiene i valori della chiave full-text. Ogni tabella con indicizzazione full-text include una colonna i cui valori sono garantiti univoci e i valori restituiti nella colonna **chiave** sono i valori chiave full-text delle righe che corrispondono ai criteri di selezione specificati nella condizione di ricerca Contains. La proprietà **TableFulltextKeyColumn** , ottenuta dalla funzione OBJECTPROPERTYEX, fornisce l'identità della colonna chiave univoca. Per ottenere l'ID della colonna associata alla chiave full-text dell'indice full-text, utilizzare **sys.fulltext_indexes**. Per ulteriori informazioni, vedere [sys.fulltext_indexes &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md).  
  
 Per ottenere le righe desiderate dalla tabella originale, specificare un join per le righe CONTAINSTABLE. La forma tipica della clausola FROM per un'istruzione SELECT che utilizza la funzione CONTAINSTABLE è la seguente:  
  
```  
SELECT select_list  
FROM table AS FT_TBL INNER JOIN  
   CONTAINSTABLE(table, column, contains_search_condition) AS KEY_TBL  
   ON FT_TBL.unique_key_column = KEY_TBL.[KEY];  
```  
  
 La tabella prodotta da CONTAINSTABLE include una colonna denominata **Rank**. La colonna Rank è un valore (compreso tra 0 e 1000) per ogni riga che indica il **livello** di corrispondenza tra una riga e i criteri di selezione. Questo valore di pertinenza viene normalmente utilizzato nell'istruzione SELECT in uno dei modi seguenti:  
  
-   Nella clausola ORDER BY per ottenere le righe a cui sono stati assegnati i valori di pertinenza massimi nelle prime posizioni della tabella.  
  
-   Nell'elenco di selezione per visualizzare il valore di pertinenza assegnato a ogni riga.  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni di esecuzione sono disponibili solo per gli utenti che dispongono dei privilegi appropriati per l'istruzione SELECT nella tabella o nelle colonne della tabella a cui viene fatto riferimento.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-simple-example"></a>R. Esempio semplice  
 Nell'esempio seguente viene creata e popolata una semplice tabella di due colonne, che elenca 3 contee e i colori nei rispettivi flag. Consente di creare e popolare un indice e un catalogo full-text nella tabella. Viene quindi illustrata la sintassi **CONTAINSTABLE** . In questo esempio viene illustrata la crescita del valore di rango superiore quando il valore di ricerca viene soddisfatto più volte. Nell'ultima query, Tanzania che contiene sia il verde che il nero hanno un rango superiore rispetto all'Italia, che contiene solo uno dei colori sottoposti a query.  
  
```  
CREATE TABLE Flags (Country nvarchar(30) NOT NULL, FlagColors varchar(200));  
CREATE UNIQUE CLUSTERED INDEX FlagKey ON Flags(Country);  
INSERT Flags VALUES ('France', 'Blue and White and Red');  
INSERT Flags VALUES ('Italy', 'Green and White and Red');  
INSERT Flags VALUES ('Tanzania', 'Green and Yellow and Black and Yellow and Blue');  
SELECT * FROM Flags;  
GO  
  
CREATE FULLTEXT CATALOG TestFTCat;  
CREATE FULLTEXT INDEX ON Flags(FlagColors) KEY INDEX FlagKey ON TestFTCat;  
GO   
  
SELECT * FROM Flags;  
SELECT * FROM CONTAINSTABLE (Flags, FlagColors, 'Green') ORDER BY RANK DESC;  
SELECT * FROM CONTAINSTABLE (Flags, FlagColors, 'Green or Black') ORDER BY RANK DESC;  
```  
  
### <a name="b-returning-rank-values"></a>B. Restituzione di valori di rango  
 Nell'esempio seguente viene eseguita una ricerca dei nomi di prodotto che includono le parole "frame", "wheel" o "tire". Ogni parola viene ponderata in modo diverso. Per ogni riga restituita corrispondente ai criteri di ricerca, viene illustrata la prossimità relativa (valore di rango assegnato) della corrispondenza. Le righe di rango superiore vengono restituite per prime.  
  
```  
USE AdventureWorks2012;  
GO  
  
SELECT FT_TBL.Name, KEY_TBL.RANK  
    FROM Production.Product AS FT_TBL   
        INNER JOIN CONTAINSTABLE(Production.Product, Name,   
        'ISABOUT (frame WEIGHT (.8),   
        wheel WEIGHT (.4), tire WEIGHT (.2) )' ) AS KEY_TBL  
            ON FT_TBL.ProductID = KEY_TBL.[KEY]  
ORDER BY KEY_TBL.RANK DESC;  
GO  
```  
  
### <a name="c-returning-rank-values-greater-than-a-specified-value"></a>C. Restituzione di valori di rango superiori a un valore specificato  
  
||  
|-|  
|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.|  
  
 Nell'esempio seguente viene utilizzato NEAR per cercare "`bracket`" e "`reflector`" vicini nella tabella `Production.Document`. Vengono restituite solo le righe con valore di pertinenza pari a 50 o superiore.  
  
```  
USE AdventureWorks2012  
GO  
  
SELECT DocumentNode, Title, DocumentSummary  
FROM Production.Document AS DocTable   
INNER JOIN CONTAINSTABLE(Production.Document, Document,  
  'NEAR(bracket, reflector)' ) AS KEY_TBL  
  ON DocTable.DocumentNode = KEY_TBL.[KEY]  
WHERE KEY_TBL.RANK > 50  
ORDER BY KEY_TBL.RANK DESC;  
GO  
```  
  
> [!NOTE]  
>  Se una query full-text non specifica un valore intero come distanza massima, un documento che contiene solo riscontri il cui gap è maggiore di 100 termini logici non soddisferà i requisiti NEAR e il relativo rango sarà pari a 0.  
  
### <a name="d-returning-top-5-ranked-results-using-top_n_by_rank"></a>D. Restituzione dei primi 5 valori con pertinenza maggiore utilizzando top_n_by_rank  
 Nell'esempio seguente viene restituita la descrizione dei primi 5 prodotti la cui colonna `Description` include la parola "aluminum" accanto alla parola "light" o "lightweight".  
  
```  
USE AdventureWorks2012;  
GO  
  
SELECT FT_TBL.ProductDescriptionID,  
   FT_TBL.Description,   
   KEY_TBL.RANK  
FROM Production.ProductDescription AS FT_TBL INNER JOIN  
   CONTAINSTABLE (Production.ProductDescription,  
      Description,   
      '(light NEAR aluminum) OR  
      (lightweight NEAR aluminum)',  
      5  
   ) AS KEY_TBL  
   ON FT_TBL.ProductDescriptionID = KEY_TBL.[KEY];  
GO  
```  
  
 `GO`  
  
### <a name="e-specifying-the-language-argument"></a>E. Utilizzo dell'argomento LANGUAGE  
 Nell'esempio seguente viene illustrato l'utilizzo dell'argomento `LANGUAGE`.  
  
```  
USE AdventureWorks2012;  
GO  
  
SELECT FT_TBL.ProductDescriptionID,  
   FT_TBL.Description,   
   KEY_TBL.RANK  
FROM Production.ProductDescription AS FT_TBL INNER JOIN  
   CONTAINSTABLE (Production.ProductDescription,  
      Description,   
      '(light NEAR aluminum) OR  
      (lightweight NEAR aluminum)',  
      LANGUAGE N'English',  
      5  
   ) AS KEY_TBL  
   ON FT_TBL.ProductDescriptionID = KEY_TBL.[KEY];  
GO  
```  
  
> [!NOTE]  
>  Il linguaggio *language_term* argomento non è obbligatorio per l'utilizzo di *top_n_by_rank.*  
  
## <a name="see-also"></a>Vedere anche  
 [Limitare i risultati della ricerca con RANK](../../relational-databases/search/limit-search-results-with-rank.md)   
 [Eseguire query con ricerca full-text](../../relational-databases/search/query-with-full-text-search.md)   
 [Creare query di ricerca full-text &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/create-full-text-search-queries-visual-database-tools.md)   
 [CONTAINS &#40;Transact-SQL&#41;](../../t-sql/queries/contains-transact-sql.md)   
 [Eseguire query con ricerca full-text](../../relational-databases/search/query-with-full-text-search.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)  
  
