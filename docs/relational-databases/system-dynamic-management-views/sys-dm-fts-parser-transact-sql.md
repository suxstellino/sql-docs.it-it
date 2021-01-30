---
description: sys.dm_fts_parser (Transact-SQL)
title: sys.dm_fts_parser (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_fts_parser_TSQL
- dm_fts_parser
- dm_fts_parser_TSQL
- sys.dm_fts_parser
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_parser dynamic management function
- troubleshooting [SQL Server], full-text search
ms.assetid: 2736d376-fb9d-4b28-93ef-472b7a27623a
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
ms.openlocfilehash: ff5dac01f1fa48db60a384b8cc8bfb2216a5bab5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184323"
---
# <a name="sysdm_fts_parser-transact-sql"></a>sys.dm_fts_parser (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce il risultato della suddivisione in token finale dopo aver applicato una determinata combinazione di [Word breaker](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md), [Thesaurus](../../relational-databases/search/configure-and-manage-thesaurus-files-for-full-text-search.md)ed elementi di parole non [significative](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md) all'input di una stringa di query. Il risultato della tokenizzazione è equivalente all'output del motore di ricerca full-text per la stringa di query specificata.  
  
 sys.dm_fts_parser è una funzione a gestione dinamica.  
  
## <a name="syntax"></a>Sintassi  
  
```  
sys.dm_fts_parser('query_string', lcid, stoplist_id, accent_sensitivity)  
```  
  
## <a name="arguments"></a>Argomenti  
 *query_string*  
 Query da analizzare. *QUERY_STRING* può essere una catena di stringhe che [contiene](../../t-sql/queries/contains-transact-sql.md) il supporto della sintassi. È possibile ad esempio includere forme flessive, un thesaurus e operatori logici.  
  
 *lcid*  
 Identificatore delle impostazioni locali (LCID) del Word breaker da utilizzare per l'analisi *QUERY_STRING*.  
  
 *stoplist_id*  
 ID dell'oggetto di parole non significative, se presente, che verrà utilizzato dal Word breaker identificato da *LCID*. *stoplist_id* è di **tipo int**. Se si specifica ' NULL ', non viene utilizzato alcun numero di parole non significative. Se si specifica 0, viene utilizzato l'elemento STOPLIST di sistema.  
  
 L'ID di un elenco di parole non significative è univoco all'interno di un database. Per ottenere l'ID di elenco di parole non significative per un indice full-text in una tabella specificata, utilizzare la vista del catalogo [sys.fulltext_indexes](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md) .  
  
 *accent_sensitivity*  
 Valore booleano che controlla se la ricerca full-text supporta o meno la distinzione relativa ai segni diacritici. *accent_sensitivity* è di **bit**, con uno dei valori seguenti:  
  
|Valore|Distinzione tra caratteri accentati...|  
|-----------|----------------------------|  
|0|Non attiva<br /><br /> Parole del tipo "café" e "cafe" vengono considerate in modo identico.|  
|1|Sensibili<br /><br /> Parole del tipo "café" e "cafe" vengono considerate in modo diverso.|  
  
> [!NOTE]  
>  Per visualizzare l'impostazione corrente di questo valore per un catalogo full-text, eseguire l' [!INCLUDE[tsql](../../includes/tsql-md.md)] istruzione seguente: `SELECT fulltextcatalogproperty('` *catalog_name* `', 'AccentSensitivity');` .  
  
## <a name="table-returned"></a>Tabella restituita  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|parola chiave|**varbinary(128)**|Rappresentazione esadecimale di una parola chiave specificata restituita da un word breaker. Tale rappresentazione viene utilizzata per archiviare la parola chiave nell'indice full-text. Questo valore non è leggibile, ma è utile per correlare una parola chiave specificata all'output restituito da altre viste a gestione dinamica che restituiscono il contenuto di un indice full-text, ad esempio [sys.dm_fts_index_keywords](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-transact-sql.md) e [sys.dm_fts_index_keywords_by_document](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md).<br /><br /> **Nota:** OxFF rappresenta il carattere speciale che indica la fine di un file o di un set di dati.|  
|group_id|**int**|Contiene un valore integer utile per differenziare il gruppo logico dal quale è stato generato un termine specifico. '`Server AND DB OR FORMSOF(THESAURUS, DB)"`' produce ad esempio i seguenti valori di group_id in inglese:<br /><br /> 1: Server<br />2: DATABASE<br />3: DATABASE|  
|phrase_id|**int**|Contiene un valore integer utile per differenziare i casi in cui il word breaker invia forme alternative di parole composte, come full-text. In presenza di parole composte, ad esempio "multi-million", a volte il word breaker invia forme alternative. In alcuni casi tali forme alternative (frasi) devono essere differenziate.<br /><br /> '`multi-million`' produce ad esempio i seguenti valori di phrase_id in inglese:<br /><br /> 1 per `multi`<br />1 per `million`<br />2 per `multimillion`|  
|occurrence|**int**|Indica l'ordine di ogni termine nel risultato dell'analisi. Per la frase "`SQL Server query processor`", ad esempio, in occurrence sarebbero presenti i seguenti valori di occorrenza per i termini, in inglese:<br /><br /> 1 per `SQL`<br />2 per `Server`<br />3 per `query`<br />4 per `processor`|  
|special_term|**nvarchar(4000)**|Contiene informazioni sulle caratteristiche del termine inviato dal word breaker, tra cui:<br /><br /> Corrispondenza esatta<br /><br /> Parola non significativa<br /><br /> Fine di frase<br /><br /> Fine di paragrafo<br /><br /> Fine di capitolo|  
|display_term|**nvarchar(4000)**|Contiene la forma leggibile della parola chiave. Analogamente alle funzioni progettate per accedere al contenuto dell'indice full-text, il termine visualizzato potrebbe non essere identico al termine originale a causa della limitazione della denormalizzazione, ma dovrebbe essere tuttavia abbastanza preciso per consentire di identificarlo partendo dall'input originale.|  
|expansion_type|**int**|Contiene informazioni sulla natura dell'espansione di un termine specificato. I valori possono essere i seguenti:<br /><br /> 0 = singola parola<br /><br /> 2 = espansione flessiva<br /><br /> 4 = espansione/sostituzione del thesaurus<br /><br /> Si consideri ad esempio un caso in cui il thesaurus definisce il termine "run" come un'espansione di `jog`:<br /><br /> `<expansion>`<br /><br /> `<sub>run</sub>`<br /><br /> `<sub>jog</sub>`<br /><br /> `</expansion>`<br /><br /> Il termine `FORMSOF (FREETEXT, run)` genera l'output seguente:<br /><br /> `run` con expansion_type = 0<br /><br /> `runs` con expansion_type = 2<br /><br /> `running` con expansion_type = 2<br /><br /> `ran` con expansion_type = 2<br /><br /> `jog` con expansion_type = 4|  
|source_term|**nvarchar(4000)**|Termine o frase da cui viene generato o analizzato un termine specifico. Una query eseguita su '"`word breakers" AND stemmers'` produce ad esempio i seguenti valori di source_term in inglese:<br /><br /> `word breakers` per la display_term`word`<br />`word breakers` per la display_term`breakers`<br />`stemmers` per la display_term`stemmers`|  
  
## <a name="remarks"></a>Commenti  
 **sys.dm_fts_parser** supporta la sintassi e le funzionalità dei predicati full-text, ad esempio [Contains](../../t-sql/queries/contains-transact-sql.md) e [FREETEXT](../../t-sql/queries/freetext-transact-sql.md), e funzioni, ad esempio [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md) e [FREETEXTTABLE](../../relational-databases/system-functions/freetexttable-transact-sql.md).  
  
## <a name="using-unicode-for-parsing-special-characters"></a>Utilizzo di Unicode per l'analisi di caratteri speciali  
 Quando si analizza una stringa di query, **sys.dm_fts_parser** utilizza le regole di confronto del database a cui si è connessi, a meno che non si specifichi la stringa di query come Unicode. Pertanto, per una stringa non Unicode che contiene caratteri speciali, ad esempio ü o ç, l'output potrebbe essere imprevisto, a seconda delle regole di confronto del database. Per elaborare una stringa di query indipendentemente dalle regole di confronto del database, anteporre alla stringa il prefisso `N` , ovvero `N'` *QUERY_STRING* `'` .  
  
 Per ulteriori informazioni, vedere "C. Visualizzazione dell'output di una stringa che contiene caratteri speciali" più avanti in questo argomento.  
  
## <a name="when-to-use-sysdm_fts_parser"></a>Utilizzo di sys.dm_fts_parser  
 **sys.dm_fts_parser** possono essere molto potenti a scopo di debug. Di seguito vengono riportati alcuni dei principali scenari di utilizzo:  
  
-   Comprensione del modo in cui un word breaker considera un input specificato  
  
     La restituzione di risultati imprevisti da parte di una query può essere probabilmente provocata dal modo in cui il word breaker analizza e divide i dati. Se si utilizza sys.dm_fts_parser è possibile individuare il risultato che un word breaker passa all'indice full-text. È inoltre possibile visualizzare quali termini sono parole non significative che non vengono ricercate nell'indice full-text. Se un termine è un parola non significativa per una determinata lingua varia a seconda che si trovi nell'oggetto di parole non significative specificato dal valore *stoplist_id* dichiarato nella funzione.  
  
     Si noti inoltre il flag relativo alla distinzione tra caratteri accentati e non accentati, che consentirà all'utente di visualizzare il modo in cui il word breaker analizzerà l'input in base alle informazioni relative alla distinzione tra caratteri accentati e non accentati di cui dispone.  
  
-   Comprensione del funzionamento dello stemmer su un input specificato  
  
     Per individuare il modo in cui il word breaker e lo stemmer analizzano un termine della query e le relative forme di stemming, è possibile specificare una query CONTAINS o CONTAINSTABLE che contiene la clausola FORMSOF seguente:  
  
    ```  
    FORMSOF( INFLECTIONAL, query_term )  
    ```  
  
     I risultati indicano i termini che vengono passati all'indice full-text.  
  
-   Comprensione del modo in cui il thesaurus espande o sostituisce tutto o parte dell'input  
  
     È inoltre possibile specificare:  
  
    ```  
    FORMSOF( THESAURUS, query_term )  
    ```  
  
     I risultati di questa query indicano il modo in cui il word breaker e il thesaurus interagiscono per il termine della query. È possibile visualizzare l'espansione o le sostituzioni eseguite dal thesaurus e identificare la query risultante effettivamente inviata all'indice full-text.  
  
     Si noti che se l'utente invia:  
  
    ```  
    FORMSOF( FREETEXT, query_term )  
    ```  
  
     Le funzionalità flessive e del thesaurus verranno eseguite automaticamente.  
  
 Oltre agli scenari di utilizzo precedenti, sys.dm_fts_parser può risultare estremamente utile nella comprensione e nella risoluzione di molti altri problemi relativi alla query full-text.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server **sysadmin** e i diritti di accesso all'oggetto specificato.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-displaying-the-output-of-a-given-word-breaker-for-a-keyword-or-phrase"></a>R. Visualizzazione dell'output di un word breaker specifico per una parola chiave o una frase  
 Nell'esempio seguente viene restituito l'output relativo all'utilizzo del word breaker inglese, il cui LCID è 1033, e di nessun elenco di parole non significative sulla stringa di query seguente:  
  
 `The Microsoft business analysis`  
  
 La distinzione tra caratteri accentati e non accentati è disabilitata.  
  
```sql
SELECT * FROM sys.dm_fts_parser (' "The Microsoft business analysis" ', 1033, 0, 0);  
```  
  
### <a name="b-displaying-the-output-of-a-given-word-breaker-in-the-context-of-stoplist-filtering"></a>B. Visualizzazione dell'output di un word breaker specifico nel contesto di applicazione di filtri dell'elenco di parole non significative  
 Nell'esempio seguente viene restituito l'output relativo all'utilizzo del word breaker inglese, il cui LCID è 1033, e di un elenco di parole non significative, il cui ID è 77, sulla stringa di query seguente:  
  
 `"The Microsoft business analysis" OR "MS revenue"`  
  
 La distinzione tra caratteri accentati e non accentati è disabilitata.  
  
```sql
SELECT * FROM sys.dm_fts_parser (' "The Microsoft business analysis"  OR " MS revenue" ', 1033, 77, 0);  
```  
  
### <a name="c-displaying-the-output-of-a-string-that-contains-special-characters"></a>C. Visualizzazione dell'output di una stringa che contiene caratteri speciali  
 Nell'esempio seguente viene utilizzato Unicode per analizzare la stringa in lingua francese seguente:  
  
 `français`  
  
 Nell'esempio viene specificato l'identificatore LCID per la lingua francese, `1036`, e l'ID di un elenco di parole non significative definito dall'utente, `5` La distinzione tra caratteri accentati e non accentati è abilitata.  
  
```sql
SELECT * FROM sys.dm_fts_parser(N'français', 1036, 5, 1);  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica per la ricerca full-text e la ricerca semantica &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)   
 [Ricerca full-text](../../relational-databases/search/full-text-search.md)   
 [Configurazione e gestione di word breaker e stemmer per la ricerca](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md)   
 [Configurare e gestire i file del thesaurus per la ricerca Full-Text](../../relational-databases/search/configure-and-manage-thesaurus-files-for-full-text-search.md)   
 [Configurare e gestire parole non significative ed elenchi di parole non significative per la ricerca full-text](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)   
 [Eseguire query con ricerca full-text](../../relational-databases/search/query-with-full-text-search.md)   
 [Eseguire query con ricerca full-text](../../relational-databases/search/query-with-full-text-search.md)   
 [Entità a protezione diretta](../../relational-databases/security/securables.md)  
  
  
