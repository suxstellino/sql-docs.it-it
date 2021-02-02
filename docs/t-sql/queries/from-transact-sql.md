---
title: Clausola FROM con JOIN, APPLY, PIVOT (T-SQL)
description: Clausola FROM con JOIN, APPLY, PIVOT (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- JOIN
- FROM_TSQL
- FROM
- JOIN_TSQL
- OUTER_JOIN_TSQL
- INNER_JOIN_TSQL
- CROSS_TSQL
- CROSS_APPLY_TSQL
- APPLY_TSQL
- CROSS_JOIN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- OUTER APPLY operator
- hints [SQL Server], FROM clause
- SELECT statement [SQL Server], FROM clause
- ISO syntax
- DELETE statement [SQL Server], FROM clause
- CROSS APPLY operator
- FROM clause
- APPLY operator
- joins [SQL Server], FROM clause
- UPDATE statement [SQL Server], FROM clause
- derived tables
ms.assetid: 36b19e68-94f6-4539-aeb1-79f5312e4263
author: VanMSFT
ms.author: vanto
ms.custom: ''
ms.date: 06/01/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c13c477c5042658edd47f5f9be53a707e47628d2
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99235966"
---
# <a name="from-clause-plus-join-apply-pivot-transact-sql"></a>Clausola FROM con JOIN, APPLY, PIVOT (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

In Transact-SQL, la clausola FROM è disponibile per le istruzioni seguenti:

- [DELETE](../statements/delete-transact-sql.md)
- [UPDATE](update-transact-sql.md)
- [SELECT](select-transact-sql.md)

La clausola FROM è in genere obbligatoria per l'istruzione SELECT. L'eccezione è quando non viene elencata alcuna colonna di tabella e gli unici elementi elencati sono valori letterali o variabili o espressioni aritmetiche.

Questo articolo illustra anche le parole chiave seguenti che possono essere usate nella clausola FROM:

- [JOIN](../../relational-databases/performance/joins.md)
- APPLY
- [PIVOT](from-using-pivot-and-unpivot.md)

![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
[ FROM { <table_source> } [ ,...n ] ]   
<table_source> ::=   
{  
    table_or_view_name [ FOR SYSTEM_TIME <system_time> ] [ AS ] table_alias ]   
        [ <tablesample_clause> ]   
        [ WITH ( < table_hint > [ [ , ]...n ] ) ]   
    | rowset_function [ [ AS ] table_alias ]   
        [ ( bulk_column_alias [ ,...n ] ) ]   
    | user_defined_function [ [ AS ] table_alias ]  
    | OPENXML <openxml_clause>   
    | derived_table [ [ AS ] table_alias ] [ ( column_alias [ ,...n ] ) ]   
    | <joined_table>   
    | <pivoted_table>   
    | <unpivoted_table>  
    | @variable [ [ AS ] table_alias ]  
    | @variable.function_call ( expression [ ,...n ] )   
        [ [ AS ] table_alias ] [ (column_alias [ ,...n ] ) ]     
}  
<tablesample_clause> ::=  
    TABLESAMPLE [SYSTEM] ( sample_number [ PERCENT | ROWS ] )   
        [ REPEATABLE ( repeat_seed ) ]   
  
<joined_table> ::=   
{  
    <table_source> <join_type> <table_source> ON <search_condition>   
    | <table_source> CROSS JOIN <table_source>   
    | left_table_source { CROSS | OUTER } APPLY right_table_source   
    | [ ( ] <joined_table> [ ) ]   
}  
<join_type> ::=   
    [ { INNER | { { LEFT | RIGHT | FULL } [ OUTER ] } } [ <join_hint> ] ]  
    JOIN  
  
<pivoted_table> ::=  
    table_source PIVOT <pivot_clause> [ [ AS ] table_alias ]  
  
<pivot_clause> ::=  
        ( aggregate_function ( value_column [ [ , ]...n ])   
        FOR pivot_column   
        IN ( <column_list> )   
    )   
  
<unpivoted_table> ::=  
    table_source UNPIVOT <unpivot_clause> [ [ AS ] table_alias ]  
  
<unpivot_clause> ::=  
    ( value_column FOR pivot_column IN ( <column_list> ) )   
  
<column_list> ::=  
    column_name [ ,...n ]   
  
<system_time> ::=  
{  
       AS OF <date_time>  
    |  FROM <start_date_time> TO <end_date_time>  
    |  BETWEEN <start_date_time> AND <end_date_time>  
    |  CONTAINED IN (<start_date_time> , <end_date_time>)   
    |  ALL  
}  
  
    <date_time>::=  
        <date_time_literal> | @date_time_variable  
  
    <start_date_time>::=  
        <date_time_literal> | @date_time_variable  
  
    <end_date_time>::=  
        <date_time_literal> | @date_time_variable  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
FROM { <table_source> [ ,...n ] }  
  
<table_source> ::=   
{  
    [ database_name . [ schema_name ] . | schema_name . ] table_or_view_name [ AS ] table_or_view_alias 
    [<tablesample_clause>]  
    | derived_table [ AS ] table_alias [ ( column_alias [ ,...n ] ) ]  
    | <joined_table>  
}  
  
<tablesample_clause> ::=
    TABLESAMPLE ( sample_number [ PERCENT ] ) -- Azure Synapse Analytics only  
 
<joined_table> ::=   
{  
    <table_source> <join_type> <table_source> ON search_condition   
    | <table_source> CROSS JOIN <table_source> 
    | left_table_source { CROSS | OUTER } APPLY right_table_source   
    | [ ( ] <joined_table> [ ) ]   
}  
  
<join_type> ::=   
    [ INNER ] [ <join hint> ] JOIN  
    | LEFT  [ OUTER ] JOIN  
    | RIGHT [ OUTER ] JOIN  
    | FULL  [ OUTER ] JOIN  
  
<join_hint> ::=   
    REDUCE  
    | REPLICATE  
    | REDISTRIBUTE  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
\<table_source>  
 Specifica una tabella, vista, variabile di tabella o origine di tabella derivata con o senza un alias, da utilizzare nell'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)]. In un'istruzione sono consentite fino a 256 origini di tabella. Il limite varia tuttavia in base alla memoria disponibile e alla complessità delle altre espressioni nella query, ovvero alcune query specifiche potrebbero non supportare 256 origini di tabella.  
  
> [!NOTE]  
>  Nelle query con riferimenti a molte tabelle le prestazioni di esecuzione potrebbero essere ridotte. I tempi di compilazione e ottimizzazione vengono influenzati anche da fattori aggiuntivi, ad esempio la presenza di indici e visualizzazioni indicizzate in ogni \<table_source> e le dimensioni di \<select_list> nell'istruzione SELECT.  
  
 L'ordine delle origini di tabella dopo la parola chiave FROM non influisce sul set di risultati restituito. Quando la clausola FROM include nomi duplicati, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce un errore.  
  
 *table_or_view_name*  
 Nome di una tabella o di una vista.  
  
 Se la tabella o la visualizzazione è presente in un altro database della stessa istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], specificare un nome completo nel formato *database*.*schema*.*object_name*.  
  
 Se la tabella o la visualizzazione è presente all'esterno dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]l, specificare un nome composto da quattro parti nel formato *linked_server*.*catalog*.*schema*.*object*. Per altre informazioni, vedere [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md). Per specificare l'origine della tabella remota è anche possibile usare un nome composto da quattro parti formulato tramite la funzione [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md) come componente server del nome. Quando viene specificato OPENDATASOURCE, *database_name* e *schema_name* possono non essere validi per tutte le origini dati e non essere soggetti alle funzionalità del provider OLE DB che accede all'oggetto remoto.  
  
 [AS] *table_alias*  
 Alias per *table_source* che può essere usato per convenienza o per contraddistinguere una tabella o una visualizzazione in un self-join o in una sottoquery. Un alias è spesso un nome di tabella abbreviato utilizzato per fare riferimento a colonne specifiche delle tabelle di un join. Se lo stesso nome di colonna è presente in più di una tabella del join, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è necessario qualificarlo con un nome di tabella o di vista oppure con un alias. Se viene definito un alias, non è possibile utilizzare il nome di tabella.  
  
 Quando viene usata una tabella derivata, una funzione con valori di tabella o per i set di righe oppure una clausola con operatori (ad esempio PIVOT o UNPIVOT), il parametro *table_alias* necessario alla fine della clausola è il nome della tabella associata per tutte le colonne restituite, comprese le colonne di raggruppamento.  
  
 WITH (\<table_hint> )  
 Specifica che Query Optimizer utilizza una strategia di ottimizzazione o di blocco con questa tabella e per questa istruzione. Per altre informazioni, vedere [Hint di tabella &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
*rowset_function*  
**SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive e [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
Specifica una delle funzioni per i set di righe, ad esempio OPENROWSET, che restituisce un oggetto che è possibile utilizzare in sostituzione di un riferimento alla tabella. Per altre informazioni su un elenco delle funzioni per i set di righe, vedere [Funzioni per i set di righe &#40;Transact-SQL&#41;](../functions/opendatasource-transact-sql.md).  
  
 L'utilizzo delle funzioni OPENROWSET e OPENQUERY per specificare un oggetto remoto dipende dalle funzionalità del provider OLE DB che accede all'oggetto.  
  
 *bulk_column_alias*  
**SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive e [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
 Alias facoltativo da utilizzare in sostituzione del nome di colonna nel set di risultati. Gli alias di colonna sono consentiti solo nelle istruzioni SELECT che utilizzano la funzione OPENROWSET con l'opzione BULK. Quando si usa *bulk_column_alias*, specificare un alias per ogni colonna di tabella nello stesso ordine delle colonne nel file.  
  
> [!NOTE]  
>  Questo alias esegue l'override dell'attributo NAME negli elementi COLUMN di un file in formato XML, se presente.  
  
 *user_defined_function*  
 Specifica una funzione con valori di tabella.  
  
OPENXML \<openxml_clause>  
**SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive e [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
Consente di visualizzare un documento XML come set di righe. Per altre informazioni, vedere [OPENXML &#40;Transact-SQL&#41;](../../t-sql/functions/openxml-transact-sql.md).  
  
 *derived_table*  
 Sottoquery che recupera le righe dal database. *derived_table* viene usato come input per la query esterna.  
  
 *derived_table* può usare la funzione di costruttore di valori di tabella [!INCLUDE[tsql](../../includes/tsql-md.md)] per specificare più righe. Ad esempio: `SELECT * FROM (VALUES (1, 2), (3, 4), (5, 6), (7, 8), (9, 10) ) AS MyTable(a, b);`. Per altre informazioni, vedere [Costruttore di valori di tabella &#40;Transact-SQL&#41;](../../t-sql/queries/table-value-constructor-transact-sql.md).  
  
 *column_alias*  
 Alias facoltativo da utilizzare in sostituzione del nome di colonna nel set di risultati della tabella derivata. Includere un alias di colonna per ogni colonna nell'elenco di selezione e racchiudere tra parentesi l'intero elenco di alias di colonna.  
  
 *table_or_view_name* FOR SYSTEM_TIME \<system_time> Si applica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive e [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
Specifica che venga restituita una determinata versione dei dati della tabella temporale indicata e della relativa tabella di cronologia con controllo delle versioni di sistema collegata  
  
### <a name="tablesample-clause"></a>Clausola TABLESAMPLE
**Si applica a:** SQL Server, database SQL Specifica che vengono restituiti dati di esempio dalla tabella. I dati di esempio possono essere approssimativi. Questa clausola può essere usata in ogni tabella primaria o unita in join in un'istruzione SELECT o UPDATE. Non è possibile specificare TABLESAMPLE con le viste.  
  
> [!NOTE]  
>  Quando si utilizza TABLESAMPLE sui database aggiornati a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il livello di compatibilità del database viene impostato su 110 o su un valore maggiore. L'operatore PIVOT non è consentito in una query ricorsiva dell'espressione di tabella comune. Per altre informazioni, vedere [Livello di compatibilità ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
  
 SYSTEM  
 Metodo di campionamento dipendente dall'implementazione specificato dagli standard ISO. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è l'unico metodo di campionamento disponibile e viene applicato per impostazione predefinita. SYSTEM applica un metodo di campionamento basato su pagine in cui come campione viene scelto un set di pagine casuale dalla tabella e tutte le righe di tali pagine vengono restituite come subset campione.  
  
 *sample_number*  
 Espressione numerica costante esatta o approssimativa che rappresenta la percentuale o il numero delle righe. Quando viene specificato con PERCENT, *sample_number* viene implicitamente convertito in valore di tipo **float**. In caso contrario, viene convertito in **bigint**. PERCENT è l'impostazione predefinita.  
  
 PERCENT  
 Specifica che una percentuale *sample_number* di righe della tabella deve essere recuperata dalla tabella. Quando viene specificato PERCENT, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce un valore approssimativo della percentuale specificata. Quando viene specificato PERCENT l'espressione *sample_number* deve restituire un valore compreso tra 0 e 100.  
  
 ROWS  
 Specifica che verrà recuperato un numero di righe approssimativamente pari a *sample_number*. Quando viene specificato ROWS, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce un'approssimazione del numero di righe specificato. Quando viene specificato ROWS, l'espressione *sample_number* deve restituire un valore integer maggiore di zero.  
  
 REPEATABLE  
 Indica che il campione selezionato può essere restituito nuovamente. Se specificato con lo stesso valore *repeat_seed*, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce lo stesso subset di righe a condizione che non siano state apportate modifiche alle righe della tabella. Se specificato con un valore *repeat_seed* diverso, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituirà probabilmente un campione diverso delle righe nella tabella. Le azioni seguenti nella tabella vengono considerate modifiche: inserimento, aggiornamento, eliminazione, ricompilazione o deframmentazione dell'indice e ripristino o collegamento del database.  
  
 *repeat_seed*  
 Espressione di tipo integer costante utilizzata da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per generare un numero casuale. *repeat_seed* è **bigint**. Se *repeat_seed* non viene specificato, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] assegna un valore in modo casuale. Per un valore *repeat_seed* specifico, il risultato del campionamento è sempre lo stesso se non sono state applicate modifiche alla tabella. L'espressione *repeat_seed* deve restituire un valore integer maggiore di zero.  
  
### <a name="tablesample-clause"></a>Clausola TABLESAMPLE
**Si applica a:** [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]

 Specifica che vengono restituiti dati di esempio dalla tabella. I dati di esempio possono essere approssimativi. Questa clausola può essere usata in ogni tabella primaria o unita in join in un'istruzione SELECT o UPDATE. Non è possibile specificare TABLESAMPLE con le viste. 

 PERCENT  
 Specifica che una percentuale *sample_number* di righe della tabella deve essere recuperata dalla tabella. Quando viene specificato PERCENT, [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] restituisce un valore approssimativo della percentuale specificata. Quando viene specificato PERCENT, l'espressione *sample_number* deve restituire un valore compreso tra 0 e 100.  


### <a name="joined-table"></a>Tabella unita in join 
Un tabella unita in join è un set di risultati che rappresenta il prodotto di due o più tabelle. In caso di più join, utilizzare le parentesi per modificarne l'ordine standard.  
  
### <a name="join-type"></a>Tipo di join
Specifica il tipo di operazione di join.  
  
 INNER  
 Specifica che vengono restituite tutte le coppie di righe corrispondenti. Le righe senza corrispondenza vengono eliminate da entrambe le tabelle. Corrisponde al valore predefinito se non viene specificato alcun tipo di join.  
  
 FULL [OUTER]  
 Specifica che una riga della tabella di destra o di sinistra che non rispetta la condizione di join è inclusa nel set di risultati e le colonne di output che corrispondono all'altra tabella sono impostate su NULL. Questa si aggiunge a tutte le righe normalmente restituite dall'INNER JOIN.  
  
 LEFT [OUTER]  
 Specifica che, oltre alle righe restituite dall'inner join, vengono incluse nel set di risultati tutte le righe della tabella sinistra che non rispettano la condizione di join e le colonne di output dell'altra tabella sono impostate su NULL.  
  
 RIGHT [OUTER]  
 Specifica che, oltre alle righe restituite dall'inner join, vengono incluse nel set di risultati tutte le righe della tabella destra che non rispettano le condizioni di join e le colonne di output che corrispondono all'altra tabella sono impostate su NULL.  
  
### <a name="join-hint"></a>Hint per il join  
Per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e [!INCLUDE[ssSDS](../../includes/sssds-md.md)], specifica che Query Optimizer di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa un hint di join o un algoritmo di esecuzione per ogni join specificato nella clausola FROM della query. Per altre informazioni, vedere [Hint &#40;Transact-SQL&#41; - Join](../../t-sql/queries/hints-transact-sql-join.md).  
  
 Per [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], questi hint di join si applicano ai join INNER in due colonne non compatibili di distribuzione. Possono migliorare le prestazioni delle query limitando lo spostamento dei dati che si verifica durante l'elaborazione delle query. Gli hint di join consentiti per [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] sono i seguenti:  
  
 REDUCE  
 Riduce il numero di righe da spostare per la tabella sul lato destro del join per rendere compatibili due tabelle non compatibili di distribuzione. L'hint REDUCE è chiamato anche hint di semi-join.  
  
 REPLICATE  
 Fa in modo che i valori nella colonna di join della tabella sul lato sinistro del join vengano replicati in tutti i nodi. La tabella di destra viene unita alla versione replicata delle colonne.  
  
 REDISTRIBUTE  
 Forza la distribuzione di due origini dati nelle colonne specificate nella clausola JOIN. Per una tabella distribuita, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] esegue uno spostamento casuale. Per una tabella replicata, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] esegue uno spostamento di taglio. Per comprendere questi tipi di spostamento, vedere la sezione "DMS Query Plan Operations" (Operazioni del piano di query del Servizio Migrazione del database) in "Understanding Query Plans" (Informazioni sui piani di query) in [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)]. Questo hint può migliorare le prestazioni quando il piano di query usa uno spostamento dei dati trasmessi per risolvere un join non compatibile di distribuzione.  
  
 JOIN  
 Indica che l'operazione di join specificata deve essere eseguita tra le viste o le origini di tabella specificate.  
  
 ON \<search_condition>  
 Specifica la condizione su cui è basato il join. La condizione può includere qualsiasi predicato, ma vengono in genere utilizzati nomi di colonne e operatori di confronto, ad esempio:  
  
```sql
SELECT p.ProductID, v.BusinessEntityID  
FROM Production.Product AS p   
JOIN Purchasing.ProductVendor AS v  
ON (p.ProductID = v.ProductID);  
  
```  
  
 Quando nella condizione sono specificate le colonne, non è necessario che queste abbiano lo stesso nome o lo stesso tipo di dati. Se tuttavia i tipi di dati sono diversi, è necessario che siano compatibili o supportino la conversione implicita in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se non è possibile eseguire una conversione implicita dei tipi di dati, la condizione deve convertire in modo esplicito il tipo di dati tramite la funzione CONVERT.  
  
 È possibile che alcuni predicati includano una sola delle tabelle unite in join nella clausola ON. Tali predicati potrebbero essere presenti inoltre nella clausola WHERE della query. La posizione di tali predicati non è rilevante nel caso di INNER join, ma potrebbe generare risultati diversi se vengono utilizzati OUTER join. I predicati inclusi nella clausola ON infatti vengono applicati alla tabella prima dell'esecuzione del join, mentre la clausola WHERE viene applicata semanticamente ai risultati del join.  
  
 Per altre informazioni sulle condizioni di ricerca e i predicati, vedere [Condizioni di ricerca &#40;Transact-SQL&#41;](../../t-sql/queries/search-condition-transact-sql.md).  
  
 CROSS JOIN  
 Specifica il prodotto incrociato di due tabelle. Restituisce le righe che verrebbero restituite se non fosse specificata alcuna clausola WHERE in un join obsoleto, non SQL-92.  
  
 *left_table_source* { CROSS | OUTER } APPLY *right_table_source*  
 Specifica che *right_table_source* dell'operatore APPLY viene valutato rispetto a ogni riga di *left_table_source*. Questa funzionalità risulta utile quando *right_table_source* include una funzione con valori di tabella che accetta i valori di colonna da *left_table_source* come uno dei relativi argomenti.  
  
 È necessario specificare CROSS o OUTER con APPLY. Se si specifica CROSS, non vengono restituite righe quando *right_table_source* viene valutato rispetto a ogni riga specificata di *left_table_source* e viene restituito un set di risultati vuoto.  
  
 Se si specifica OUTER, viene restituita una riga per ogni riga di *left_table_source* anche quando *right_table_source* viene valutato rispetto a tale riga e viene restituito un set di risultati vuoto.  
  
 Per altre informazioni, vedere la sezione Osservazioni.  
  
 *left_table_source*  
 Origine di tabella definita nell'argomento precedente. Per altre informazioni, vedere la sezione Osservazioni.  
  
 *right_table_source*  
 Origine di tabella definita nell'argomento precedente. Per altre informazioni, vedere la sezione Osservazioni.  
  
### <a name="pivot-clause"></a>Clausola PIVOT

 *table_source* PIVOT \<pivot_clause>  
 Specifica che *table_source* venga trasformato tramite Pivot in base a *pivot_column*. *table_source* è una tabella o un'espressione di tabella. L'output è una tabella che contiene tutte le colonne di *table_source* ad eccezione di *pivot_column* e *value_column*. Le colonne di *table_source*, eccetto *pivot_column* e *value_column*, vengono definite colonne di raggruppamento dell'operatore PIVOT. Per altre informazioni su PIVOT e UNPIVOT, vedere [Uso di PIVOT e UNPIVOT](../../t-sql/queries/from-using-pivot-and-unpivot.md).  
  
 PIVOT esegue un'operazione di raggruppamento sulla tabella di input relativamente alle colonne di raggruppamento e restituisce una riga per ogni gruppo. L'output contiene anche una colonna per ogni valore specificato in *column_list* visualizzato in *pivot_column* in *input_table*.  
  
 Per ulteriori informazioni, vedere la sezione Osservazioni riportata di seguito.  
  
 *aggregate_function*  
 Funzione di aggregazione di sistema o definita dall'utente che accetta uno o più input. La funzione di aggregazione deve essere invariante rispetto ai valori Null. Una funzione di aggregazione invariante rispetto ai valori Null non considera i valori Null nel gruppo mentre valuta il valore di aggregazione.  
  
 La funzione di aggregazione di sistema COUNT(*) non è consentita.  
  
 *value_column*  
 Indica la colonna dei valori dell'operatore PIVOT. Se usato con UNPIVOT, *value_column* non può corrispondere al nome di una colonna esistente in *table_source* nell'input.  
  
 FOR *pivot_column*  
 Colonna pivot dell'operatore PIVOT. *pivot_column* deve essere di un tipo di dati che è possibile convertire in modo implicito o esplicito nel tipo **nvarchar()** . Questa colonna non può essere **image** o **rowversion**.  
  
 Quando viene usato UNPIVOT, *pivot_column* indica il nome della colonna di output risultante dal raggruppamento delle colonne di *table_source*. In *table_source* non può esistere già una colonna con questo nome.  
  
 IN (*column_list* )  
 Nella clausola PIVOT elenca i valori della colonna *pivot_column* che diventeranno i nomi di colonna della tabella di output. L'elenco non può includere nomi di colonna già presenti nell'origine di tabella di input, specificata in *table_source*, che viene trasformata tramite Pivot.  
  
 Nella clausola UNPIVOT elenca le colonne in *table_source* che verranno raggruppate in un'unica colonna *pivot_column*.  
  
 *table_alias*  
 Nome dell'alias della tabella di output. È necessario specificare *pivot_table_alias*.  
  
 UNPIVOT \<unpivot_clause>  
 Indica che la tabella di input viene ridotta da più colonne specificate in *column_list* a una singola colonna denominata *pivot_column*. Per altre informazioni su PIVOT e UNPIVOT, vedere [Uso di PIVOT e UNPIVOT](../../t-sql/queries/from-using-pivot-and-unpivot.md).  
  
AS OF \<date_time>  
**SI APPLICA A**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive e [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
Restituisce una tabella con un singolo record per ogni riga contenente i valori che erano effettivi (correnti) in un momento specificato nel passato. Internamente, viene eseguita un'unione tra la tabella temporale e la relativa tabella di cronologia e i risultati vengono filtrati in modo da restituire i valori nella riga che era valida nel momento specificato dal parametro *\<date_time>* . Il valore per una riga viene considerato valido se il valore *system_start_time_column_name* è minore o uguale al valore del parametro *\<date_time>* e il valore *system_end_time_column_name* è maggiore del valore del parametro *\<date_time>* .   
  
FROM \<start_date_time> TO \<end_date_time>
**Si applica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive e [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].
Restituisce una tabella con i valori per tutte le versioni di record che erano attive nell'intervallo di tempo specificato, indipendentemente dal fatto che abbiano iniziato a essere attive prima del valore del parametro *\<start_date_time>* per l'argomento FROM o non siano più state attive dopo il valore del parametro *\<end_date_time>* per l'argomento TO. Internamente, viene eseguita un'unione tra la tabella temporale e la relativa tabella di cronologia e i risultati vengono filtrati in modo da restituire i valori per tutte le versioni di riga che erano attive in qualsiasi momento durante l'intervallo di tempo specificato. Le righe diventate attive esattamente in corrispondenza del limite inferiore definito dall'endpoint FROM sono incluse e le righe diventate attive esattamente in corrispondenza del limite superiore definito dall'endpoint TO non sono incluse.  
  
BETWEEN \<start_date_time> AND \<end_date_time>  
**SI APPLICA A**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive e [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
Come sopra per la descrizione di **FROM \<start_date_time> TO \<end_date_time>** , tranne per il fatto che include le righe diventate attive in corrispondenza del limite superiore definito dall'endpoint \<end_date_time>.  
  
CONTAINED IN (\<start_date_time> , \<end_date_time>)  
**SI APPLICA A**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive e [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
Restituisce una tabella con i valori per tutte le versioni di record che sono state aperte e chiuse nell'intervallo di tempo specificato, definito dai due valori datetime per l'argomento CONTAINED IN. Sono incluse le righe diventate attive esattamente in corrispondenza del limite inferiore o che non sono più state attive esattamente in corrispondenza del limite superiore.  
  
ALL  
Restituisce una tabella con i valori di tutte le righe della tabella corrente e della tabella di cronologia.  
  
## <a name="remarks"></a>Osservazioni  
 La clausola FROM supporta la sintassi SQL-92 per le tabelle unite in join e per le tabelle derivate. Nella sintassi SQL-92 sono disponibili gli operatori di join INNER, LEFT OUTER, RIGHT OUTER, FULL OUTER e CROSS.  
  
 In una clausola FROM le istruzioni UNION e JOIN sono supportate sia nelle viste, sia nelle tabelle derivate e nelle sottoquery.  
  
 Un self join è una tabella unita in join con se stessa. Nelle operazioni di inserimento e aggiornamento basate su un self join viene seguito l'ordine indicato nella clausola FROM.  
  
 Dato che in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono prese in considerazione le statistiche di distribuzione e cardinalità dei server collegati che forniscono statistiche sulla distribuzione delle colonne, non è necessario ricorrere all'hint di join REMOTE per imporre la valutazione di un join in remoto. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Query Processor analizza le statistiche remote e determina se è appropriato adottare una strategia di join remoto. L'hint di join REMOTE risulta utile per i provider che non forniscono statistiche sulla distribuzione delle colonne.  
  
## <a name="using-apply"></a>Utilizzo di APPLY  
 Entrambi gli operandi sinistro e destro dell'operatore APPLY sono espressioni di tabella. La differenza principale tra questi operandi è rappresentata dal fatto che *right_table_source* può usare una funzione con valori di tabella in cui una colonna di *left_table_source* è considerata uno degli argomenti. *left_table_source* può includere funzioni con valori di tabella, ma non può contenere come argomenti colonne di *right_table_source*.  
  
L'operatore APPLY funziona nel modo seguente per restituire l'origine di tabella per la clausola FROM:  
  
1.  Valuta *right_table_source* rispetto a ogni riga di *left_table_source* per produrre set di righe.  
  
    I valori di *right_table_source* dipendono da *left_table_source*. *right_table_source* può essere rappresentato approssimativamente in questo modo: `TVF(left_table_source.row)`, dove `TVF` è una funzione con valori di tabella.  
  
2.  Combina i set di risultati restituiti per ogni riga nella valutazione di *right_table_source* con *left_table_source* tramite un'operazione UNION ALL.  
  
    L'elenco di colonne restituito dal risultato dell'operatore APPLY corrisponde al set di colonne di *left_table_source* combinato con l'elenco di colonne di *right_table_source*.  
  
## <a name="using-pivot-and-unpivot"></a>Utilizzo di PIVOT e UNPIVOT  
 *pivot_column* e *value_column* sono colonne di raggruppamento usate dall'operatore PIVOT. PIVOT segue il processo seguente per ottenere il set di risultati di output:  
  
1.  Esegue GROUP BY in *input_table* sulle colonne di raggruppamento e restituisce una riga di output per ogni gruppo.  
  
     Le colonne di raggruppamento nella riga di output ottengono i valori delle colonne corrispondenti per tale gruppo in *input_table*.  
  
2.  Genera valori per le colonne nell'elenco delle colonne per ogni riga di output eseguendo le operazioni seguenti:  
  
    1.  Raggruppando le righe restituite in GROUP BY nel passaggio precedente rispetto a *pivot_column*.  
  
         Selezionando per ogni colonna di output in *column_list* un sottogruppo che soddisfa la condizione:  
  
         `pivot_column = CONVERT(<data type of pivot_column>, 'output_column')`  
  
    2.  *aggregate_function* viene valutata rispetto a *value_column* in questo sottogruppo e il risultato viene restituito come valore per la colonna *output_column* corrispondente. Se il sottogruppo è vuoto, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un valore Null per *output_column*. Se la funzione di aggregazione è COUNT e il sottogruppo è vuoto, viene restituito zero (0).  

> [!NOTE]
> Gli identificatori di colonna nella clausola `UNPIVOT` seguono le regole di confronto dei cataloghi. Per il [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], le regole di confronto sono sempre `SQL_Latin1_General_CP1_CI_AS`. Per i database parzialmente indipendenti di [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], le regole di confronto sono sempre `Latin1_General_100_CI_AS_KS_WS_SC`. Se la colonna è combinata con altre colonne, sarà necessaria una clausola COLLATE, ovvero `COLLATE DATABASE_DEFAULT`, per evitare conflitti.   
  
 Per altre informazioni su PIVOT e UNPIVOT ed esempi, vedere [Uso di PIVOT e UNPIVOT](../../t-sql/queries/from-using-pivot-and-unpivot.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 Sono richieste le autorizzazioni per l'istruzione DELETE, SELECT o UPDATE.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-a-simple-from-clause"></a>R. Utilizzo di una clausola FROM semplice  
 Nell'esempio seguente vengono recuperate le colonne `TerritoryID` e `Name` dalla tabella `SalesTerritory` nel database di esempio [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql    
SELECT TerritoryID, Name  
FROM Sales.SalesTerritory  
ORDER BY TerritoryID ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
TerritoryID Name                            
----------- ------------------------------  
1           Northwest                       
2           Northeast                       
3           Central                         
4           Southwest                       
5           Southeast                       
6           Canada                          
7           France                          
8           Germany                         
9           Australia                       
10          United Kingdom                  
(10 row(s) affected)  
```  
  
### <a name="b-using-the-tablock-and-holdlock-optimizer-hints"></a>B. Utilizzo degli hint di ottimizzazione TABLOCK e HOLDLOCK  
 Nella transazione parziale seguente viene illustrato come impostare un blocco di tabella condiviso esplicito in `Employee` e come leggere l'indice. Il blocco viene mantenuto attivo fino al termine della transazione.  
  
```sql    
BEGIN TRAN  
SELECT COUNT(*)   
FROM HumanResources.Employee WITH (TABLOCK, HOLDLOCK) ;  
```  
  
### <a name="c-using-the-sql-92-cross-join-syntax"></a>C. Utilizzo della sintassi CROSS JOIN di SQL-92  
 Nell'esempio seguente viene restituito il prodotto incrociato delle due tabelle `Employee` e `Department` nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Vengono restituiti inoltre un elenco di tutte le possibili combinazioni delle righe di `BusinessEntityID` e tutte le righe di nome `Department`.  
  
```sql    
SELECT e.BusinessEntityID, d.Name AS Department  
FROM HumanResources.Employee AS e  
CROSS JOIN HumanResources.Department AS d  
ORDER BY e.BusinessEntityID, d.Name ;  
```  
  
### <a name="d-using-the-sql-92-full-outer-join-syntax"></a>D. Utilizzo della sintassi FULL OUTER JOIN di SQL-92  
 Nell'esempio seguente vengono restituiti il nome del prodotto ed eventuali ordini di vendita corrispondenti nella tabella `SalesOrderDetail` del database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Vengono inoltre restituiti gli ordini di vendita per cui non è elencato alcun prodotto nella tabella `Product` e tutti i prodotti con un ordine di vendita diverso da quello elencato nella tabella `Product`.  
  
```sql  
-- The OUTER keyword following the FULL keyword is optional.  
SELECT p.Name, sod.SalesOrderID  
FROM Production.Product AS p  
FULL OUTER JOIN Sales.SalesOrderDetail AS sod  
ON p.ProductID = sod.ProductID  
ORDER BY p.Name ;  
```  
  
### <a name="e-using-the-sql-92-left-outer-join-syntax"></a>E. Utilizzo della sintassi LEFT OUTER JOIN di SQL-92  
 Nell'esempio seguente vengono unite in join due tabelle tramite la colonna `ProductID`. Le righe della tabella sinistra prive di corrispondenza vengono mantenute. La tabella `Product` viene confrontata con la tabella `SalesOrderDetail` nelle colonne `ProductID` in ogni tabella. Tutti i prodotti, ordinati e non ordinati, vengono visualizzati nel set dei risultati.  
  
```sql    
SELECT p.Name, sod.SalesOrderID  
FROM Production.Product AS p  
LEFT OUTER JOIN Sales.SalesOrderDetail AS sod  
ON p.ProductID = sod.ProductID  
ORDER BY p.Name ;  
```  
  
### <a name="f-using-the-sql-92-inner-join-syntax"></a>F. Utilizzo della sintassi INNER JOIN di SQL-92  
 Nell'esempio seguente vengono restituiti tutti i nomi di prodotti e gli ID degli ordini di vendita.  
  
```sql    
-- By default, SQL Server performs an INNER JOIN if only the JOIN   
-- keyword is specified.  
SELECT p.Name, sod.SalesOrderID  
FROM Production.Product AS p  
INNER JOIN Sales.SalesOrderDetail AS sod  
ON p.ProductID = sod.ProductID  
ORDER BY p.Name ;  
```  
  
### <a name="g-using-the-sql-92-right-outer-join-syntax"></a>G. Utilizzo della sintassi RIGHT OUTER JOIN di SQL-92  
 Nell'esempio seguente vengono unite in join due tabelle tramite la colonna `TerritoryID`. Le righe della tabella destra prive di corrispondenza vengono mantenute. La tabella `SalesTerritory` viene confrontata con la tabella `SalesPerson` nella colonna `TerritoryID` in ogni tabella. Tutti i venditori vengono visualizzati nel set dei risultati, a prescindere dal fatto che siano assegnati a un'area o meno.  
  
```sql    
SELECT st.Name AS Territory, sp.BusinessEntityID  
FROM Sales.SalesTerritory AS st   
RIGHT OUTER JOIN Sales.SalesPerson AS sp  
ON st.TerritoryID = sp.TerritoryID ;  
```  
  
### <a name="h-using-hash-and-merge-join-hints"></a>H. Utilizzo degli hint di join HASH e MERGE  
 Nell'esempio seguente viene eseguito un join delle tre tabelle `Product`, `ProductVendor` e `Vendor` per ottenere un elenco di prodotti e dei relativi fornitori. Query Optimizer unisce in join le tabelle `Product` e `ProductVendor` (`p` e `pv`) tramite un join di tipo MERGE. I risultati del join di tipo MERGE delle tabelle `Product` e `ProductVendor` (`p` e `pv`) vengono quindi uniti tramite join di tipo HASH alla tabella `Vendor` per ottenere (`p` e `pv`) e `v`.  
  
> [!IMPORTANT]  
>  Dopo avere specificato un hint di join, la parola chiave INNER non è più facoltativa e deve essere indicata in modo esplicito per l'esecuzione di un INNER JOIN.  
  
```sql    
SELECT p.Name AS ProductName, v.Name AS VendorName  
FROM Production.Product AS p   
INNER MERGE JOIN Purchasing.ProductVendor AS pv   
ON p.ProductID = pv.ProductID  
INNER HASH JOIN Purchasing.Vendor AS v  
ON pv.BusinessEntityID = v.BusinessEntityID  
ORDER BY p.Name, v.Name ;  
```  
  
### <a name="i-using-a-derived-table"></a>I. Utilizzo di una tabella derivata  
 Nell'esempio seguente viene utilizzata una tabella derivata, con un'istruzione `SELECT` dopo la clausola `FROM`, per restituire nome e cognome di tutti i dipendenti e le città in cui abitano.  
  
```sql    
SELECT RTRIM(p.FirstName) + ' ' + LTRIM(p.LastName) AS Name, d.City  
FROM Person.Person AS p  
INNER JOIN HumanResources.Employee e ON p.BusinessEntityID = e.BusinessEntityID   
INNER JOIN  
   (SELECT bea.BusinessEntityID, a.City   
    FROM Person.Address AS a  
    INNER JOIN Person.BusinessEntityAddress AS bea  
    ON a.AddressID = bea.AddressID) AS d  
ON p.BusinessEntityID = d.BusinessEntityID  
ORDER BY p.LastName, p.FirstName;  
```  
  
### <a name="j-using-tablesample-to-read-data-from-a-sample-of-rows-in-a-table"></a>J. Utilizzo di TABLESAMPLE per leggere i dati di un campione di righe in una tabella  
 Nell'esempio seguente viene utilizzata l'opzione `TABLESAMPLE` nella clausola `FROM` per restituire approssimativamente il `10` percento di tutte le righe della tabella `Customer`.  
  
```sql    
SELECT *  
FROM Sales.Customer TABLESAMPLE SYSTEM (10 PERCENT) ;  
```  
  
### <a name="k-using-apply"></a>K. Utilizzo di APPLY  
Nell'esempio seguente si presuppone che nel database siano presenti le tabelle e la funzione con valori di tabella seguenti:  

|Nome oggetto|Nomi di colonna|      
|---|---|   
|Departments|DeptID, DivisionID, DeptName, DeptMgrID|      
|EmpMgr|MgrID, EmpID|     
|Employees|EmpID, EmpSalary EmpLastName, EmpFirstName|  
|GetReports(MgrID)|EmpSalary EmpID, EmpLastName|     
  
La funzione con valori di tabella `GetReports` restituisce un elenco di tutti i dipendenti che sono subordinati direttamente o indirettamente all'elemento `MgrID` specificato.  
  
In questo esempio viene utilizzato `APPLY` per restituire tutti i reparti e tutti i dipendenti in ogni reparto. Se uno specifico reparto non ha dipendenti, non verranno restituite righe per tale reparto.  
  
```sql
SELECT DeptID, DeptName, DeptMgrID, EmpID, EmpLastName, EmpSalary  
FROM Departments d    
CROSS APPLY dbo.GetReports(d.DeptMgrID) ;  
```  
  
Se si desidera che la query restituisca righe per i reparti senza dipendenti, restituendo valori Null per le colonne `EmpID`, `EmpLastName` e `EmpSalary`, utilizzare invece `OUTER APPLY`.  
  
```sql
SELECT DeptID, DeptName, DeptMgrID, EmpID, EmpLastName, EmpSalary  
FROM Departments d   
OUTER APPLY dbo.GetReports(d.DeptMgrID) ;  
```  
  
### <a name="l-using-cross-apply"></a>L. Utilizzo di CROSS APPLY  
Nell'esempio seguente viene recuperato uno snapshot di tutti i piani di query disponibili nella cache dei piani, eseguendo una query sulla DMV `sys.dm_exec_cached_plans` per recuperare gli handle per tutti i piani di query nella cache. Successivamente viene specificato l'operatore `CROSS APPLY` per passare gli handle del piano a `sys.dm_exec_query_plan`. L'output Showplan XML per ogni piano disponibile nella cache dei piani viene indicato nella colonna `query_plan` della tabella restituita.  
  
```sql
USE master;  
GO  
SELECT dbid, object_id, query_plan   
FROM sys.dm_exec_cached_plans AS cp   
CROSS APPLY sys.dm_exec_query_plan(cp.plan_handle);   
GO  
```  
  
### <a name="m-using-for-system_time"></a>M. Uso di FOR SYSTEM_TIME  
  
**SI APPLICA A**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive e [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
L'esempio seguente usa l'argomento FOR SYSTEM_TIME AS OF date_time_literal_or_variable per restituire le righe di tabella correnti in data 1 gennaio 2014.  
  
```sql
SELECT DepartmentNumber,   
    DepartmentName,   
    ManagerID,   
    ParentDepartmentNumber   
FROM DEPARTMENT  
FOR SYSTEM_TIME AS OF '2014-01-01'  
WHERE ManagerID = 5;
```  
  
L'esempio seguente usa l'argomento FOR SYSTEM_TIME FROM date_time_literal_or_variable TO date_time_literal_or_variable per restituire tutte le righe attive durante il periodo definito che inizia l'1 gennaio 2013 e termina l'1 gennaio 2014, escluso il limite superiore.  
  
```sql
SELECT DepartmentNumber,   
    DepartmentName,   
    ManagerID,   
    ParentDepartmentNumber   
FROM DEPARTMENT  
FOR SYSTEM_TIME FROM '2013-01-01' TO '2014-01-01'  
WHERE ManagerID = 5;
```  
  
 L'esempio seguente usa l'argomento FOR SYSTEM_TIME BETWEEN date_time_literal_or_variable AND date_time_literal_or_variable per restituire tutte le righe attive durante il periodo definito che inizia l'1 gennaio 2013 e termina l'1 gennaio 2014, incluso il limite superiore.  
  
```sql
SELECT DepartmentNumber,   
    DepartmentName,   
    ManagerID,   
    ParentDepartmentNumber   
FROM DEPARTMENT  
FOR SYSTEM_TIME BETWEEN '2013-01-01' AND '2014-01-01'  
WHERE ManagerID = 5;
```  
  
 L'esempio seguente usa l'argomento FOR SYSTEM_TIME CONTAINED IN ( date_time_literal_or_variable, date_time_literal_or_variable ) per restituire tutte le righe aperte e chiuse durante il periodo definito che inizia l'1 gennaio 2013 e termina l'1 gennaio 2014.  
  
```sql
SELECT DepartmentNumber,   
    DepartmentName,   
    ManagerID,   
    ParentDepartmentNumber   
FROM DEPARTMENT  
FOR SYSTEM_TIME CONTAINED IN ( '2013-01-01', '2014-01-01' )  
WHERE ManagerID = 5;
```  
  
 L'esempio seguente usa una variabile anziché un letterale per specificare i valori limite di data per la query.  
  
```sql
DECLARE @AsOfFrom datetime2 = dateadd(month,-12, sysutcdatetime());
DECLARE @AsOfTo datetime2 = dateadd(month,-6, sysutcdatetime());
  
SELECT DepartmentNumber,   
    DepartmentName,   
    ManagerID,   
    ParentDepartmentNumber   
FROM DEPARTMENT  
FOR SYSTEM_TIME FROM @AsOfFrom TO @AsOfTo  
WHERE ManagerID = 5;
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="n-using-the-inner-join-syntax"></a>N. Uso della sintassi INNER JOIN  
 L'esempio seguente restituisce le colonne `SalesOrderNumber`, `ProductKey` e `EnglishProductName` delle tabelle `FactInternetSales` e `DimProduct` in cui la chiave di join `ProductKey` corrisponde in entrambe le tabelle. Poiché ogni colonna `SalesOrderNumber` e `EnglishProductName` è presente solo in una delle tabelle, non è necessario specificare l'alias di tabella con queste colonne; in questo caso gli alias sono inclusi per favorire la leggibilità. La parola **AS** prima di un nome di alias non è obbligatoria ma è consigliabile specificarla per migliorare la leggibilità e per conformità allo standard ANSI.  
  
```sql
-- Uses AdventureWorks  
  
SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
FROM FactInternetSales AS fis 
INNER JOIN DimProduct AS dp  
    ON dp.ProductKey = fis.ProductKey;  
```  
  
 Poiché la parola chiave `INNER` non è obbligatoria per gli inner join, la stessa query può essere scritta come segue:  
  
```sql
-- Uses AdventureWorks  
  
SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
FROM FactInternetSales AS fis 
JOIN DimProduct AS dp  
ON dp.ProductKey = fis.ProductKey;  
```  
  
 È anche possibile usare una clausola `WHERE` con questa query per limitare i risultati. Questo esempio limita i risultati ai valori `SalesOrderNumber` maggiori di 'SO5000':  
  
```sql
-- Uses AdventureWorks  
  
SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
FROM FactInternetSales AS fis 
JOIN DimProduct AS dp  
    ON dp.ProductKey = fis.ProductKey  
WHERE fis.SalesOrderNumber > 'SO50000'  
ORDER BY fis.SalesOrderNumber;  
```  
  
### <a name="o-using-the-left-outer-join-and-right-outer-join-syntax"></a>O. Uso della sintassi LEFT OUTER JOIN e RIGHT OUTER JOIN  
 L'esempio seguente unisce le tabelle `FactInternetSales` e `DimProduct` nelle colonne `ProductKey`. La sintassi LEFT OUTER JOIN mantiene le righe senza corrispondenza della tabella di sinistra (`FactInternetSales`). Poiché la tabella `FactInternetSales` non contiene valori `ProductKey` che non corrispondono alla tabella `DimProduct`, la query restituisce le stesse righe dell'esempio del primo inner join precedente.  
  
```sql
-- Uses AdventureWorks  
  
SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
FROM FactInternetSales AS fis 
LEFT OUTER JOIN DimProduct AS dp  
    ON dp.ProductKey = fis.ProductKey;  
```  
  
 Questa query può essere scritta anche senza la parola chiave `OUTER`.  
  
 Nei right outer join le righe senza corrispondenza della tabella di destra vengono mantenute. L'esempio seguente restituisce le stesse righe dell'esempio di left outer join precedente.  
  
```sql
-- Uses AdventureWorks  
  
SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
FROM DimProduct AS dp 
RIGHT OUTER JOIN FactInternetSales AS fis  
    ON dp.ProductKey = fis.ProductKey;  
```  
  
 La query seguente usa la tabella `DimSalesTerritory` come tabella di sinistra in un left outer join. Recupera i valori `SalesOrderNumber` della tabella `FactInternetSales`. Se non sono presenti ordini per un particolare `SalesTerritoryKey`, la query restituisce NULL per `SalesOrderNumber` per la riga. Poiché questa query viene ordinata dalla colonna `SalesOrderNumber`, tutti i valori NULL in questa colonna verranno visualizzati per primi nei risultati.  
  
```sql
-- Uses AdventureWorks  
  
SELECT dst.SalesTerritoryKey, dst.SalesTerritoryRegion, fis.SalesOrderNumber  
FROM DimSalesTerritory AS dst 
LEFT OUTER JOIN FactInternetSales AS fis  
    ON dst.SalesTerritoryKey = fis.SalesTerritoryKey  
ORDER BY fis.SalesOrderNumber;  
```  
  
 Questa query potrebbe essere riscritta con un right outer join per recuperare gli stessi risultati:  
  
```sql
-- Uses AdventureWorks  
  
SELECT dst.SalesTerritoryKey, dst.SalesTerritoryRegion, fis.SalesOrderNumber  
FROM FactInternetSales AS fis 
RIGHT OUTER JOIN DimSalesTerritory AS dst  
    ON fis.SalesTerritoryKey = dst.SalesTerritoryKey  
ORDER BY fis.SalesOrderNumber;  
```  
  
### <a name="p-using-the-full-outer-join-syntax"></a>P. Uso della sintassi FULL OUTER JOIN  
 L'esempio seguente illustra un full outer join che restituisce tutte le righe di entrambe le tabelle unite ma restituisce NULL per i valori che non corrispondono dell'altra tabella.  
  
```sql
-- Uses AdventureWorks  
  
SELECT dst.SalesTerritoryKey, dst.SalesTerritoryRegion, fis.SalesOrderNumber  
FROM DimSalesTerritory AS dst 
FULL OUTER JOIN FactInternetSales AS fis  
    ON dst.SalesTerritoryKey = fis.SalesTerritoryKey  
ORDER BY fis.SalesOrderNumber;  
```  
  
 Questa query può essere scritta anche senza la parola chiave `OUTER`.  
  
```sql
-- Uses AdventureWorks  
  
SELECT dst.SalesTerritoryKey, dst.SalesTerritoryRegion, fis.SalesOrderNumber  
FROM DimSalesTerritory AS dst 
FULL JOIN FactInternetSales AS fis  
    ON dst.SalesTerritoryKey = fis.SalesTerritoryKey  
ORDER BY fis.SalesOrderNumber;  
```  
  
### <a name="q-using-the-cross-join-syntax"></a>Q. Uso della sintassi CROSS JOIN  
 L'esempio seguente restituisce il prodotto presente in entrambe le tabelle `FactInternetSales` e `DimSalesTerritory`. Viene restituito un elenco delle combinazioni possibili di `SalesOrderNumber` e `SalesTerritoryKey`. Si noti l'assenza della clausola `ON` della query di cross join.  
  
```sql
-- Uses AdventureWorks  
  
SELECT dst.SalesTerritoryKey, fis.SalesOrderNumber  
FROM DimSalesTerritory AS dst 
CROSS JOIN FactInternetSales AS fis  
ORDER BY fis.SalesOrderNumber;  
```  
  
### <a name="r-using-a-derived-table"></a>R. Utilizzo di una tabella derivata  
 L'esempio seguente usa una tabella derivata (un'istruzione `SELECT` dopo la clausola `FROM`) per restituire le colonne `CustomerKey` e `LastName` di tutti i clienti della tabella `DimCustomer` con valori `BirthDate` successivi all'1 gennaio 1970 e il cognome 'Smith'.  
  
```sql
-- Uses AdventureWorks  
  
SELECT CustomerKey, LastName  
FROM  
   (SELECT * FROM DimCustomer  
    WHERE BirthDate > '01/01/1970') AS DimCustomerDerivedTable  
WHERE LastName = 'Smith'  
ORDER BY LastName;  
```  
  
### <a name="s-reduce-join-hint-example"></a>S. Esempio di hint di join REDUCE  
 L'esempio seguente usa l'hint di join `REDUCE` per modificare l'elaborazione della tabella derivata all'interno della query. Quando si usa l'hint di join `REDUCE` in questa query, `fis.ProductKey` viene proiettato, replicato e differenziato e quindi unito a `DimProduct` durante lo spostamento casuale di `DimProduct` in `ProductKey`. La tabella derivata risultante viene distribuita in `fis.ProductKey`.  
  
```sql
-- Uses AdventureWorks  
  
EXPLAIN SELECT SalesOrderNumber  
FROM  
   (SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
    FROM DimProduct AS dp   
      INNER REDUCE JOIN FactInternetSales AS fis   
          ON dp.ProductKey = fis.ProductKey  
   ) AS dTable  
ORDER BY SalesOrderNumber;  
```  
  
### <a name="t-replicate-join-hint-example"></a>T. Esempio di hint di join REPLICATE  
 L'esempio successivo illustra la stessa query dell'esempio precedente in cui viene usato un hint di join `REPLICATE` anziché l'hint di join `REDUCE`. L'uso dell'hint `REPLICATE` causa la replica dei valori della colonna `ProductKey` (di join) della tabella `FactInternetSales` in tutti i nodi. La tabella `DimProduct` viene unita alla versione replicata dei valori.  
  
```sql
-- Uses AdventureWorks  
  
EXPLAIN SELECT SalesOrderNumber  
FROM  
   (SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
    FROM DimProduct AS dp   
      INNER REPLICATE JOIN FactInternetSales AS fis  
          ON dp.ProductKey = fis.ProductKey  
   ) AS dTable  
ORDER BY SalesOrderNumber;  
```  
  
### <a name="u-using-the-redistribute-hint-to-guarantee-a-shuffle-move-for-a-distribution-incompatible-join"></a>U. Uso dell'hint REDISTRIBUTE per garantire uno spostamento casuale per un join non compatibile di distribuzione  
 La query seguente usa l'hint di query REDISTRIBUTE in un join non compatibile di distribuzione. In questo modo si garantisce che Query Optimizer userà uno spostamento casuale nel piano di query. Si garantisce anche che il piano di query non userà uno spostamento di trasmissione che sposta una tabella distribuita in una tabella replicata.  
  
 Nell'esempio seguente l'hint REDISTRIBUTE impone uno spostamento casuale nella tabella FactInternetSales poiché ProductKey è la colonna di distribuzione di DimProduct e non è la colonna di distribuzione di FactInternetSales.  
  
```sql
-- Uses AdventureWorks  
  
EXPLAIN  
SELECT dp.ProductKey, fis.SalesOrderNumber, fis.TotalProductCost  
FROM DimProduct AS dp 
INNER REDISTRIBUTE JOIN FactInternetSales AS fis  
    ON dp.ProductKey = fis.ProductKey;  
```  

### <a name="v-using-tablesample-to-read-data-from-a-sample-of-rows-in-a-table"></a>V. Utilizzo di TABLESAMPLE per leggere i dati di un campione di righe in una tabella  
 Nell'esempio seguente viene utilizzata l'opzione `TABLESAMPLE` nella clausola `FROM` per restituire approssimativamente il `10` percento di tutte le righe della tabella `Customer`.  
  
```sql    
SELECT *  
FROM Sales.Customer TABLESAMPLE SYSTEM (10 PERCENT) ;
```
  
## <a name="see-also"></a>Vedere anche  
 [CONTAINSTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/containstable-transact-sql.md)   
 [FREETEXTTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/freetexttable-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [OPENQUERY &#40;Transact-SQL&#41;](../../t-sql/functions/openquery-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Operatori &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)
