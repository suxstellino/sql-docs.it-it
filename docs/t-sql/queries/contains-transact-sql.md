---
title: CONTAINS (Transact-SQL) | Microsoft Docs
description: Informazioni di riferimento Transact-SQL per l'elemento del linguaggio CONTAINS. Usato per cercare parole o frasi all'interno di un'altra espressione.
ms.date: 08/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CONTAINS_TSQL
- CONTAINS
dev_langs:
- TSQL
helpviewer_keywords:
- precise or fuzzy (less precise) matches [full-text search]
- CONTAINS predicate (Transact-SQL)
- conditions [SQL Server], CONTAINS
- fuzzy (less precise) word or phrase search [full-text search]
- word weighting values [full-text search]
- word searches [full-text search]
- weighted values [full-text search]
- LANGUAGE option
- word inflectionally generated from another [full-text search]
- NEAR option [full-text search]
- phrase searches [full-text search]
- word near another word search [full-text search]
- full-text search [SQL Server], searching on a property
- proximity searches [full-text search]
- less precise (fuzzy) searches [full-text search]
- property searching [SQL Server], searching on a property
- inflectional forms [full-text search]
- prefix searches [full-text search]
ms.assetid: 996c72fc-b1ab-4c96-bd12-946be9c18f84
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 680cbfab28064f92d446705de0ca2b2f778735df
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207785"
---
# <a name="contains-transact-sql"></a>CONTAINS (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Cerca corrispondenze precise o fuzzy (meno precise) a singole parole e frasi, parole a una certa distanza una dall'altra o corrispondenze ponderate in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. CONTAINS è un predicato usato nella [clausola WHERE](../../t-sql/queries/where-transact-sql.md) di un'istruzione SELECT [!INCLUDE[tsql](../../includes/tsql-md.md)] per eseguire una ricerca full-text [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in colonne indicizzate full-text che contengono tipi di dati basati su caratteri.  
  
 Il predicato CONTAINS consente di cercare:  
  
-   Una parola o una frase.  
  
-   Il prefisso di una parola o di una frase.  
  
-   Una parola accanto a una parola specifica.  
  
-   Una parola generata da un'altra per flessione, ad esempio le parole guida, guidare, guidando e guidare derivanti dalla radice guida.  
  
-   Una parola sinonimo di un'altra parola utilizzando il thesaurus, ad esempio le parole alluminio e acciaio possono essere sinonimi della parola metallo.  
  
 Per informazioni sulle forme di ricerca full-text supportate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Eseguire query con ricerca full-text](../../relational-databases/search/query-with-full-text-search.md).  
 
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CONTAINS (   
     {   
        column_name | ( column_list )   
      | *   
      | PROPERTY ( { column_name }, 'property_name' )    
     }   
     , '<contains_search_condition>'  
     [ , LANGUAGE language_term ]  
   )   
  
<contains_search_condition> ::=   
  {   
      <simple_term>   
    | <prefix_term>   
    | <generation_term>   
    | <generic_proximity_term>   
    | <custom_proximity_term>   
    | <weighted_term>   
    }   
  |   
    { ( <contains_search_condition> )   
        [ { <AND> | <AND NOT> | <OR> } ]   
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
   ( {   
        {   
          <simple_term>   
        | <prefix_term>   
        | <generation_term>   
        | <proximity_term>   
        }   
      [ WEIGHT ( weight_value ) ]   
      } [ ,...n ]   
   )   
  
<AND> ::=   
  { AND | & }  
  
<AND NOT> ::=   
  { AND NOT | &! }  
  
<OR> ::=   
  { OR | | }  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *column_name*  
 Nome di una colonna con indicizzazione full-text della tabella specificata nella clausola FROM. La colonna o le colonne possono essere di tipo **char**, **varchar**, **nchar**, **nvarchar**, **text**, **ntext**, **image**, **xml**, **varbinary** r **varbinary(max)** .  
  
 *column_list*  
 Specifica due o più colonne, separate da virgole. *column_list* deve essere racchiuso tra parentesi. La lingua di tutte le colonne di *column_list* deve essere la stessa, a meno che non sia specificato *language_term*.  
  
 \*  
 Specifica che la query esegue ricerche in tutte le colonne con indicizzazione full-text nella tabella specificata nella clausola FROM per la condizione di ricerca indicata. Le colonne nella clausola CONTAINS devono appartenere a una singola tabella che include un indice full-text. La lingua di tutte le colonne della tabella deve essere la stessa, a meno che non sia specificato *language_term*.  
  
 PROPERTY (*column_name*, '*property_name*')  
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive. 
  
 Specifica una proprietà di documento in cui cercare la condizione di ricerca specificata.  
  
> [!IMPORTANT]  
>  Perché la query restituisca delle righe, è necessario specificare *property_name* nell'elenco delle proprietà di ricerca dell'indice full-text e l'indice full-text deve contenere voci specifiche per la proprietà *property_name*. Per altre informazioni, vedere [Eseguire ricerche nelle proprietà dei documenti con elenchi delle proprietà di ricerca](../../relational-databases/search/search-document-properties-with-search-property-lists.md).  
  
 LANGUAGE *language_term*  
 Lingua da usare per word breaking, stemming, espansioni e sostituzioni del thesaurus e rimozione di [parole non significative](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md) come parte della query. Questo parametro è facoltativo e,  
  
 Se documenti di lingue diverse vengono archiviati insieme come oggetti BLOB in una singola colonna, l'identificatore delle impostazioni locali (LCID) di un documento specifico determina la lingua da utilizzare per indicizzarne il contenuto. Se quando si esegue una query su una colonna di questo tipo si specifica LANGUAGE *language_term*, è possibile aumentare la probabilità di una corrispondenza soddisfacente.  
  
 È possibile specificare *language_term* come valore stringa, intero o esadecimale corrispondente all'identificatore LCID di una lingua. Se si specifica *language_term*, la lingua rappresentata dall'argomento viene applicata a tutti gli elementi della condizione di ricerca. Se non si specifica alcun valore, verrà utilizzata la lingua full-text della colonna.  
  
 Se l'argomento *language_term* viene specificato come stringa, corrisponde al valore della colonna **alias** nella vista di compatibilità [sys.syslanguages &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md). La stringa deve essere racchiusa tra virgolette singole chiuse, come in '*language_term*'. Se l'argomento *language_term* viene specificato come valore intero, corrisponde all'LCID effettivo che identifica la lingua. Se si specifica un valore esadecimale, *language_term* è 0x seguito dal valore esadecimale di LCID. Il valore esadecimale non deve superare le otto cifre, inclusi gli zeri iniziali.  
  
 Se il valore è in formato DBCS (Double-Byte Character Set), [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lo converte in Unicode.  
  
 Se la lingua specificata non è valida o non vi sono risorse installate corrispondenti a tale lingua, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce un errore. Per usare le risorse della lingua neutra, specificare 0x0 per *language_term*.  
  
 \<*contains_search_condition*>  
 Specifica il testo da cercare in *column_name* e le condizioni della ricerca.  
  
*\<contains_search_condition>* è **nvarchar**. Viene eseguita una conversione implicita quando si utilizza come input un tipo di dati character diverso. Non è possibile usare varchar (max) e nvarchar (max) per tipi di dati di stringa di grandi dimensioni. Nell'esempio seguente la variabile `@SearchWord`, definita come `varchar(30)`, causa una conversione implicita nel predicato `CONTAINS`.
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @SearchWord VARCHAR(30)  
SET @SearchWord ='performance'  
SELECT Description   
FROM Production.ProductDescription   
WHERE CONTAINS(Description, @SearchWord);  
```  
  
 Poiché non è possibile usare l'analisi dei parametri nella conversione, usare **nvarchar** per migliorare le prestazioni. Nell'esempio dichiarare `@SearchWord` come `nvarchar(30)`.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @SearchWord NVARCHAR(30)  
SET @SearchWord = N'performance'  
SELECT Description   
FROM Production.ProductDescription   
WHERE CONTAINS(Description, @SearchWord);  
```  
  
 È inoltre possibile utilizzare l'hint per la query OPTIMIZE FOR per i casi in cui viene generato un piano non ottimale.  
  
 *word*  
 Stringa di caratteri senza spazi o punteggiatura.  
  
 *phrase*  
 Una o più parole separate da uno spazio.  
  
> [!NOTE]  
>  In alcune lingue, ad esempio quelle asiatiche, sono possibili frasi composte da una o più parole non separate da alcuno spazio.  
  
\<simple_term>  
Specifica che deve essere trovata l'esatta corrispondenza di una parola o di una frase. Esempi di termini semplici validi sono "copri capo", copricapo e "Microsoft SQL Server". Le frasi devono essere racchiuse tra virgolette doppie (""). Le parole di una frase devono essere specificate in *\<contains_search_condition>* nello stesso ordine usato nella colonna del database. La distinzione tra maiuscole e minuscole non è rilevante per la ricerca di caratteri nella parola o nella frase. Le [parole non significative](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md), quali preposizioni, congiunzioni e articoli, nelle colonne con indicizzazione full-text non vengono archiviate nell'indice full-text. Se una parola non significativa viene utilizzata nella ricerca di una singola parola, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce un messaggio di errore che indica che la query contiene solo parole non significative. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] include un elenco standard di parole non significative, disponibile nella directory \Mssql\Binn\FTERef di ogni istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 La punteggiatura viene ignorata. `CONTAINS(testing, "computer failure")`, pertanto, consente di trovare una riga contenente il valore "Where is my computer? Failure to find it would be expensive". Per altre informazioni sul comportamento word breaker, vedere [Configurazione e gestione di word breaker e stemmer per la ricerca](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md).  
  
 \<prefix_term>  
 Specifica che devono essere trovate parole o frasi che iniziano con il testo specificato. Racchiudere un prefisso tra virgolette doppie ("") e aggiungere un asterisco (\*) prima delle virgolette di chiusura, in modo tale da eseguire la ricerca di tutto il testo che inizia con il termine specificato prima dell'asterisco. La clausola deve essere specificata nel seguente modo: `CONTAINS (column, '"text*"')` L'asterisco corrisponde a zero, uno o più caratteri della parola o delle parole radice della parola o della frase. Se il testo e l'asterisco non sono racchiusi tra virgolette doppie, per cui vengono letti dal predicato come `CONTAINS (column, 'text*')`, nella ricerca full-text l'asterisco viene interpretato come carattere e viene eseguita la ricerca esatta di `text*`. In questo caso, non sarà possibile trovare parole con il carattere asterisco (\*), in quanto i word breaker ignorano in genere tali caratteri.  
  
 Se *\<prefix_term>* è una frase, ogni parola della frase verrà considerata un prefisso distinto. Una query che specifica il prefisso "sviluppo foto*", pertanto, consente di individuare tutte le righe che includono il testo "sviluppo fotografie", "sviluppo fotografie e ritocco" e così via.  
  
 \<generation_term>  
 Specifica che devono essere trovate le parole i cui termini semplici includono varianti della parola originale da cercare.  
  
 INFLECTIONAL  
 Specifica che deve essere utilizzato lo stemmer specifico della lingua per il termine semplice specificato. Il comportamento dello stemmer dipende dalle regole di stemming di ogni lingua specifica. Alla lingua neutra non è associato alcuno stemmer. Per individuare lo stemmer corretto, viene utilizzata la lingua delle colonne in cui viene eseguita la ricerca. Se si specifica *language_term*, viene usato lo stemmer corrispondente a tale lingua.  
  
 Un valore *\<simple_term>* specificato incluso in un valore *\<generation_term>* non potrà restituire sia sostantivi che verbi.  
  
 THESAURUS  
 Specifica che verrà utilizzato il thesaurus corrispondente alla lingua full-text per le colonne oppure alla lingua specificata nella query. Il derivato o i derivati più lunghi di *\<simple_term>* vengono confrontati con il thesaurus e vengono generati termini aggiuntivi per espandere o sostituire il modello originale. Se non viene trovata alcuna corrispondenza per il valore *\<simple_term>* o parte di esso, la parte per cui non è stata trovata alcuna corrispondenza viene considerata *simple_term*. Per altre informazioni sul thesaurus di ricerca full-text, vedere [Configurare e gestire i file del thesaurus per la ricerca full-text](../../relational-databases/search/configure-and-manage-thesaurus-files-for-full-text-search.md).  
  
 \<generic_proximity_term>  
 Specifica che devono essere trovate parole o frasi incluse nel documento in cui viene eseguita la ricerca.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]È consigliabile usare \<custom_proximity_term>.  
  
 NEAR | ~  
 Indica che le parole o le frasi a sinistra e a destra dell'operatore NEAR o ~ devono essere incluse in un documento in modo che venga restituita una corrispondenza. È necessario specificare due termini di ricerca. Un termine di ricerca può essere una frase o una singola parola delimitata da virgolette doppie ("*phrase*").  
  
 È possibile concatenare più termini vicini, come in `a NEAR b NEAR c` o `a ~ b ~ c`. I termini vicini concatenati devono essere tutti presenti nel documento affinché venga restituita una corrispondenza.  
  
 `CONTAINS(*column_name*, 'fox NEAR chicken')` e `CONTAINSTABLE(*table_name*, *column_name*, 'fox ~ chicken')`, ad esempio restituiscono entrambi tutti i documenti nella colonna specificata che contengono sia "fox" che "chicken". CONTAINSTABLE, inoltre, restituisce una pertinenza per ogni documento in base alla prossimità delle parole "volpe" e "pollo". Ad esempio, se un documento contiene la frase "La volpe mangia il pollo", il relativo rango è elevato perché i termini sono più vicini uno all'altro che negli altri documenti.  
  
 Per altre informazioni sui termini di prossimità generici, vedere [Ricerca di parole vicine a un'altra parola con NEAR](../../relational-databases/search/search-for-words-close-to-another-word-with-near.md).  
  
 \<custom_proximity_term>  
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.
  
 Specifica una corrispondenza di parole o frasi e facoltativamente la distanza massima consentita tra termini di ricerca. È anche possibile specificare che i termini di ricerca devono essere trovati nell'ordine esatto in cui vengono specificati (\<match_order>).  
  
 Un termine di ricerca può essere una frase o una singola parola delimitata da virgolette doppie ("*phrase*"). Ogni termine specificato deve trovarsi nel documento affinché venga restituita una corrispondenza. È necessario specificare almeno due termini di ricerca. Il numero massimo di termini di ricerca è 64.  
  
 Per impostazione predefinita, il termine vicino personalizzato restituisce qualsiasi riga contenente i termini specificati indipendentemente dalla distanza e dall'ordine. Ad esempio, per trovare una corrispondenza con la query seguente, un documento dovrebbe semplicemente contenere `term1` e "`term3 term4`" in qualsiasi punto e in qualsiasi ordine:  
  
```sql  
CONTAINS(column_name, 'NEAR(term1,"term3 term4")')  
```  
  
 I parametri facoltativi sono i seguenti:  
  
 \<maximum_distance>  
 Specifica la distanza massima consentita tra i termini di ricerca all'inizio e alla fine di una stringa affinché tale stringa possa essere ritenuta una corrispondenza.  
  
 *integer*  
 Specifica un intero positivo compreso tra 0 e 4294967295. Questo valore determina il numero di termini non di ricerca che possono esistere tra il primo e l'ultimo termine di ricerca, esclusi eventuali termini di ricerca specificati aggiuntivi.  
  
 La query seguente, ad esempio, cerca "`AA`" e "`BB`", in qualsiasi ordine, entro una distanza massima di cinque.  
  
```sql  
CONTAINS(column_name, 'NEAR((AA,BB),5)')  
```  
  
 La stringa `AA one two three four five BB` consente di trovare corrispondenze. Nell'esempio seguente, la query specifica tre termini di ricerca, `AA`, `BB` e `CC` entro una distanza massima di cinque:  
  
```sql  
CONTAINS(column_name, 'NEAR((AA,BB,CC),5)')  
```  
  
 La query troverebbe una corrispondenza per la stringa seguente in cui la distanza totale è cinque:  
  
 `BB   one two   CC   three four five A  A`  
  
 Si noti che il termine di ricerca interno, `CC`, non viene preso in considerazione.  
  
 **MAX**  
 Restituisce le righe che contengono i termini specificati indipendentemente dalla distanza esistente tra di essi. Questa è la modalità predefinita.  
  
 \<match_order>  
 Specifica se i termini devono trovarsi nell'ordine specificato che deve essere restituito da una query di ricerca. Se si specifica \<match_order>, è necessario specificare anche \<maximum_distance>.  
  
 \<match_order> passa uno dei valori seguenti:  
  
 **TRUE**  
 Applica l'ordine specificato all'interno dei termini. Ad esempio, `NEAR(A,B)` corrisponderebbe solo a `A ... B`.  
  
 **FALSE**  
 Ignora l'ordine specificato. Ad esempio, `NEAR(A,B)` corrisponderebbe sia a `A ... B` sia a `B ... A`.  
  
 Questa è la modalità predefinita.  
  
 Ad esempio, il termine vicino seguente cerca le parole "`Monday`", "`Tuesday`" e "`Wednesday`" nell'ordine specificato indipendentemente dalla distanza esistente tra di essi:  
  
```sql  
CONTAINS(column_name, 'NEAR ((Monday, Tuesday, Wednesday), MAX, TRUE)')  
```  
  
 Per altre informazioni sull'uso di termini di prossimità personalizzati, vedere [Ricerca di parole vicine a un'altra parola con NEAR](../../relational-databases/search/search-for-words-close-to-another-word-with-near.md).  
  
 \<weighted_term>  
 Specifica che le righe restituite dalla query corrispondono a un elenco di parole e di frasi, a ognuna delle quali può essere associato un valore di ponderazione facoltativo.  
  
 ISABOUT  
 Specifica la parola chiave *\<weighted_term>* .  
  
 WEIGHT(*weight_value*)  
 Specifica un valore di ponderazione compreso tra 0.0 e 1.0. Ogni componente di *\<weighted_term>* può includere un valore *weight_value*. *weight_value* consente di modificare l'effetto delle varie parti di una query sul valore di pertinenza assegnato a ogni riga che soddisfa la query. WEIGHT non ha alcun effetto sui risultati delle query CONTAINS, ma ha effetto sul valore di pertinenza nelle query [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md).  
  
> [!NOTE]  
> Il separatore decimale è sempre un punto, indipendentemente dalle impostazioni locali del sistema operativo.  
  
 { AND \| & } \| { AND NOT \| &! } \| { OR \| \| }  
 Specifica un'operazione logica tra due condizioni di ricerca del predicato CONTAINS.  
  
 { AND \| & }  
 Indica che devono essere soddisfatte entrambe le condizioni di ricerca del predicato CONTAINS. Per rappresentare l'operatore AND, è possibile usare il carattere e commerciale (&) anziché la parola chiave AND.  
  
 { AND NOT \| &! }  
 Indica che la seconda condizione di ricerca non deve essere soddisfatta. Per rappresentare l'operatore AND NOT, è possibile usare il carattere e commerciale seguito dal punto esclamativo (&!) anziché la parola chiave AND NOT.  
  
 { OR \| \| }  
 Indica che deve essere soddisfatta una delle due condizioni di ricerca del predicato CONTAINS. Per rappresentare l'operatore OR, è possibile utilizzare il carattere barra (|) anziché la parola chiave OR.  
  
 Quando *\<contains_search_condition>* include gruppi tra parentesi, tali gruppi vengono valutati per primi. In seguito alla valutazione dei gruppi tra parentesi, gli operatori logici che includono condizioni di ricerca vengono valutati in base alle regole seguenti:  
  
-   L'operatore NOT viene applicato prima dell'operatore AND.  
  
-   L'operatore NOT può essere utilizzato solo dopo l'operatore AND, ad esempio in AND NOT. L'operatore OR NOT non è consentito. Non è possibile specificare l'operatore NOT prima del primo termine. `CONTAINS (mycolumn, 'NOT "phrase_to_search_for" ' )`, ad esempio, non è valido:  
  
-   L'operatore AND viene applicato prima dell'operatore OR.  
  
-   Gli operatori booleani dello stesso tipo (AND, OR) sono associativi e pertanto possono essere applicati in qualsiasi ordine.  
  
 *n*  
 Segnaposto che indica la possibilità di specificare più termini e più condizioni di ricerca del predicato CONTAINS.  
  
## <a name="general-remarks"></a>Osservazioni generali  
 I predicati e le funzioni full-text possono essere utilizzati in una singola tabella, specificata in modo implicito nel predicato FROM. Per cercare in più tabelle, utilizzare una tabella unita in join nella clausola FROM, che consente di eseguire una ricerca in un set di risultati prodotto da due o più tabelle.  
  
 I predicati full-text non sono consentiti nella [clausola OUTPUT](../../t-sql/queries/output-clause-transact-sql.md) quando il livello di compatibilità del database è impostato su 100.  
  
## <a name="querying-remote-servers"></a>Esecuzione di query in server remoti  
 È possibile usare un nome in quattro parti nel predicato CONTAINS o [FREETEXT](../../t-sql/queries/freetext-transact-sql.md) per eseguire query di colonne con indicizzazione full-text delle tabelle di destinazione in un server collegato. Per preparare un server remoto alla ricezione di query full-text, creare un indice full-text delle tabelle di destinazione e delle colonne nel server remoto, quindi aggiungere il server remoto come server collegato.  
  
## <a name="comparison-of-like-to-full-text-search"></a>Confronto tra LIKE e la ricerca full-text  
 Contrariamente alla ricerca full-text, il predicato [LIKE](../../t-sql/language-elements/like-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)] funziona unicamente con i modelli di caratteri. Non è inoltre possibile utilizzare il predicato LIKE per eseguire query su dati binari formattati. Inoltre, l'esecuzione di una query LIKE su una grande quantità di dati di testo non strutturati è molto più lenta dell'esecuzione di una query full-text equivalente sugli stessi dati. Una query LIKE eseguita su milioni di righe di dati di testo può richiedere diversi minuti, mentre per una query full-text sugli stessi dati possono essere necessari al massimo pochi secondi, a seconda del numero di righe restituite e delle relative dimensioni. Si tenga inoltre presente che LIKE esegue unicamente un'analisi modello semplice di un'intera tabella. Una query full-text tiene invece conto della lingua, applicando trasformazioni specifiche durante l'esecuzione della query e dell'indice, ad esempio il filtro delle parole non significative ed espansioni flessive e del thesaurus. Tali trasformazioni consentono alle query full-text di migliorare la chiamata e il rango finale dei risultati.  
  
## <a name="querying-multiple-columns-full-text-search"></a>Esecuzione di query su più colonne (ricerca full-text)  
 È possibile eseguire una query su più colonne specificando un elenco di colonne in cui eseguire ricerche. Le colonne devono appartenere alla stessa tabella.  
  
 La query CONTAINS seguente, ad esempio, esegue la ricerca del termine `Red` nelle colonne `Name` e `Color` della tabella `Production.Product` del database di esempio [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
Use AdventureWorks2012;  
GO  
SELECT Name, Color   
FROM Production.Product  
WHERE CONTAINS((Name, Color), 'Red');  
```  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-contains-with-simple_term"></a>R. Uso di CONTAINS con \<simple_term>  
 Nell'esempio seguente vengono trovati tutti i prodotti il cui prezzo è `$80.99` e contenenti la parola `Mountain`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Name, ListPrice  
FROM Production.Product  
WHERE ListPrice = 80.99  
   AND CONTAINS(Name, 'Mountain');  
GO  
```  
  
### <a name="b-using-contains-and-phrase-with-simple_term"></a>B. Uso di CONTAINS e di una frase con \<simple_term>  
 Nell'esempio seguente vengono restituiti tutti i prodotti che contengono la frase `Mountain` o `Road`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Name  
FROM Production.Product  
WHERE CONTAINS(Name, ' Mountain OR Road ')  
GO  
```  
  
### <a name="c-using-contains-with-prefix_term"></a>C. Uso di CONTAINS con \<prefix_term>  
 Nell'esempio seguente vengono restituiti tutti i nomi di prodotto della colonna `Name` contenenti almeno una parola che inizia con il prefisso chain.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Name  
FROM Production.Product  
WHERE CONTAINS(Name, ' "Chain*" ');  
GO  
```  
  
### <a name="d-using-contains-and-or-with-prefix_term"></a>D. Uso di CONTAINS e OR con \<prefix_term>  
 Nell'esempio seguente vengono restituite tutte le descrizioni di categoria contenenti stringhe con il prefisso `chain` o `full`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Name  
FROM Production.Product  
WHERE CONTAINS(Name, '"chain*" OR "full*"');  
GO  
```  
  
### <a name="e-using-contains-with-proximity_term"></a>E. Uso di CONTAINS con \<proximity_term>  
  
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive. 
  
 L'esempio seguente cerca nella tabella `Production.ProductReview` tutti i commenti che contengono la parola `bike` all'interno di 10 termini della parola `control` e nell'ordine specificato (ovvero dove "`bike`" precede "`control`").  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Comments  
FROM Production.ProductReview  
WHERE CONTAINS(Comments , 'NEAR((bike,control), 10, TRUE)');  
GO  
```  
  
### <a name="f-using-contains-with-generation_term"></a>F. Uso di CONTAINS con \<generation_term>  
 Nell'esempio seguente viene eseguita la ricerca di tutti i prodotti con parole derivate da `ride`, ad esempio riding, ridden e così via.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Description  
FROM Production.ProductDescription  
WHERE CONTAINS(Description, ' FORMSOF (INFLECTIONAL, ride) ');  
GO  
```  
  
### <a name="g-using-contains-with-weighted_term"></a>G. Uso di CONTAINS con \<weighted_term>  
 Nell'esempio seguente viene eseguita la ricerca di tutti i nomi di prodotto contenenti la parola `performance`, `comfortable` o `smooth`. Ogni parola viene ponderata in modo diverso.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Description  
FROM Production.ProductDescription  
WHERE CONTAINS(Description, 'ISABOUT (performance weight (.8),   
comfortable weight (.4), smooth weight (.2) )' );  
GO  
```  
  
### <a name="h-using-contains-with-variables"></a>H. Utilizzo di CONTAINS con variabili  
 Nell'esempio seguente viene utilizzata una variabile anziché un termine di ricerca specifico.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @SearchWord NVARCHAR(30)  
SET @SearchWord = N'Performance'  
SELECT Description   
FROM Production.ProductDescription   
WHERE CONTAINS(Description, @SearchWord);  
GO  
```  
  
### <a name="i-using-contains-with-a-logical-operator-and"></a>I. Utilizzo di CONTAINS con un operatore logico (AND)  
 Nell'esempio seguente viene utilizzata la tabella ProductDescription del database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . La query usato il predicato CONTAINS per cercare le descrizioni il cui ID sia diverso da 5 e che contengano le parole `Aluminum` e `spindle`. Nella condizione di ricerca viene utilizzato l'operatore booleano AND.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Description  
FROM Production.ProductDescription  
WHERE ProductDescriptionID <> 5 AND  
   CONTAINS(Description, 'Aluminum AND spindle');  
GO  
```  
  
### <a name="j-using-contains-to-verify-a-row-insertion"></a>J. Utilizzo di CONTAINS per verificare l'inserimento di una riga  
 Nell'esempio seguente viene utilizzato il predicato CONTAINS in una sottoquery SELECT. Se si utilizza il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], la query consente di ottenere il valore di commento per tutti i commenti della tabella ProductReview per un ciclo specifico. Nella condizione di ricerca viene utilizzato l'operatore booleano AND.  
  
```sql  
USE AdventureWorks2012;  
GO  
INSERT INTO Production.ProductReview   
  (ProductID, ReviewerName, EmailAddress, Rating, Comments)   
VALUES  
  (780, 'John Smith', 'john@fourthcoffee.com', 5,   
'The Mountain-200 Silver from AdventureWorks2008 Cycles meets and exceeds expectations. I enjoyed the smooth ride down the roads of Redmond');  
  
-- Given the full-text catalog for these tables is Adv_ft_ctlg,   
-- with change_tracking on so that the full-text indexes are updated automatically.  
WAITFOR DELAY '00:00:30';     
-- Wait 30 seconds to make sure that the full-text index gets updated.  
  
SELECT r.Comments, p.Name  
FROM Production.ProductReview AS r  
JOIN Production.Product AS p   
    ON r.ProductID = p.ProductID  
    AND r.ProductID = (SELECT ProductID  
FROM Production.ProductReview  
WHERE CONTAINS (Comments,   
    ' AdventureWorks2008 AND   
    Redmond AND   
    "Mountain-200 Silver" '));  
GO  
```  
  
### <a name="k-querying-on-a-document-property"></a>K. Query su una proprietà di documento  
  
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive. 
  
 Tramite la query seguente viene eseguita una ricerca su una proprietà indicizzata, `Title`, nella colonna `Document` della tabella `Production.Document`. La query restituisce solo documenti la cui proprietà `Title` contiene la stringa `Maintenance` o `Repair`.  
  
> [!NOTE]  
>  Affinché una ricerca basata su proprietà restituisca righe, il filtro o i filtri che analizzano la colonna durante l'indicizzazione devono estrarre la proprietà specificata. L'indice full-text della tabella specificata deve inoltre essere stato configurato per includere la proprietà. Per altre informazioni, vedere [Eseguire ricerche nelle proprietà dei documenti con elenchi delle proprietà di ricerca](../../relational-databases/search/search-document-properties-with-search-property-lists.md).  
  
```sql  
Use AdventureWorks2012;  
GO  
SELECT Document 
FROM Production.Document  
WHERE CONTAINS(PROPERTY(Document,'Title'), 'Maintenance OR Repair');  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Introduzione alla ricerca full-text](../../relational-databases/search/get-started-with-full-text-search.md)   
 [Creare e gestire cataloghi full-text](../../relational-databases/search/create-and-manage-full-text-catalogs.md)   
 [CREATE FULLTEXT CATALOG &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-catalog-transact-sql.md)   
 [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [Creazione e gestione di indici full-text](../../relational-databases/search/create-and-manage-full-text-indexes.md)   
 [Eseguire query con ricerca full-text](../../relational-databases/search/query-with-full-text-search.md)   
 [CONTAINSTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/containstable-transact-sql.md)   
 [FREETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/freetext-transact-sql.md)   
 [FREETEXTTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/freetexttable-transact-sql.md)   
 [Eseguire query con ricerca full-text](../../relational-databases/search/query-with-full-text-search.md)   
 [Ricerca full-text](../../relational-databases/search/full-text-search.md)   
 [Creare query di ricerca full-text &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/create-full-text-search-queries-visual-database-tools.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)   
 [Eseguire ricerche nelle proprietà dei documenti con elenchi delle proprietà di ricerca](../../relational-databases/search/search-document-properties-with-search-property-lists.md)  
  
