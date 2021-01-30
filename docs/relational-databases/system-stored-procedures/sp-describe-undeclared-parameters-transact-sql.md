---
description: sp_describe_undeclared_parameters (Transact-SQL)
title: sp_describe_undeclared_parameters (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/24/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_describe_undeclared_parameters
- sp_describe_undeclared_parameters_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_describe_undeclared_parameters
ms.assetid: 6f016da6-dfee-4228-8b0d-7cd8e7d5a354
author: markingmyname
ms.author: maghan
monikerRange: = azuresqldb-current||= azure-sqldw-latest||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: 46dd9d66c73337920300162f331ee7a59b1e559f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200770"
---
# <a name="sp_describe_undeclared_parameters-transact-sql"></a>sp_describe_undeclared_parameters (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)] 

  Restituisce un set di risultati che contiene metadati sui parametri non dichiarati in un [!INCLUDE[tsql](../../includes/tsql-md.md)] batch. Considera ogni parametro utilizzato nel batch **\@ TSQL** , ma non dichiarato in **\@ params**. Viene restituito un set di risultati che contiene una riga per ognuno di questi parametri, con le informazioni sul tipo dedotte per quel parametro. La procedura restituisce un set di risultati vuoto se il batch di input **\@ TSQL** non contiene parametri, ad eccezione di quelli dichiarati in **\@ params**.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```sql
  
sp_describe_undeclared_parameters   
    [ @tsql = ] 'Transact-SQL_batch'   
    [ , [ @params = ] N'parameters' data type ] [, ...n]  
```  

> [!Note] 
> Per usare questa stored procedure in Azure sinapsi Analytics nel pool SQL dedicato, impostare il livello di compatibilità del database su 20 o su un valore superiore.   Per rifiutare esplicitamente, impostare il livello di compatibilità del database su 10.

## <a name="arguments"></a>Argomenti  
`[ \@tsql = ] 'Transact-SQL\_batch'` Una o più [!INCLUDE[tsql](../../includes/tsql-md.md)] istruzioni. *Transact-SQL_batch* può essere di **tipo nvarchar (**_n_**)** o **nvarchar (max)**.  
  
`[ \@params = ] N'parameters'`\@params fornisce una stringa di dichiarazione per i parametri del [!INCLUDE[tsql](../../includes/tsql-md.md)] batch, analogamente al modo in cui sp_executesql funziona. I *parametri* possono essere di **tipo nvarchar (**_n_**)** o **nvarchar (max)**.  
  
 È una stringa che contiene le definizioni di tutti i parametri incorporati in *Transact-SQL_batch*. La stringa deve essere una costante o una variabile Unicode. Ogni definizione di parametro è costituita da un nome del parametro e da un tipo di dati. n è un segnaposto che indica definizioni di parametro aggiuntive. Se l'istruzione Transact-SQL o il batch nell'istruzione non contiene parametri, \@ params non è necessario. Il valore predefinito per questo parametro è NULL.  
  
 Datatype  
 Tipo di dati del parametro.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **sp_describe_undeclared_parameters** restituisce sempre lo stato restituito zero in seguito all'esito positivo. Se la procedura genera un errore e la procedura viene chiamata come RPC, lo stato restituito viene popolato dal tipo di errore, come descritto nella colonna error_type di sys.dm_exec_describe_first_result_set. Se la procedura viene chiamata da [!INCLUDE[tsql](../../includes/tsql-md.md)], il valore restituito è sempre zero, anche in caso di errore.  
  
## <a name="result-sets"></a>Set di risultati  
 **sp_describe_undeclared_parameters** restituisce il set di risultati seguente.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**parameter_ordinal**|**int NOT NULL**|Contiene la posizione ordinale del parametro nel set di risultati. La posizione del primo parametro viene specificata come 1.|  
|**nome**|**sysname non NULL**|Contiene il nome del parametro.|  
|**suggested_system_type_id**|**int NOT NULL**|Contiene la **system_type_id** del tipo di dati del parametro come specificato in sys. Types.<br /><br /> Per i tipi CLR, anche se la colonna **system_type_name** restituirà null, questa colonna restituirà il valore 240.|  
|**suggested_system_type_name**|**nvarchar (256) NULL**|Contiene il nome del tipo di dati. Include gli argomenti, quali lunghezza, precisione e scala, specificati per il tipo di dati del parametro. Se il tipo di dati è un tipo di alias definito dall'utente, il tipo di sistema sottostante viene specificato qui. Se il tipo di dati è un tipo CLR definito dall'utente, in questa colonna viene restituito NULL. Se non è possibile dedurre il tipo del parametro, viene restituito NULL.|  
|**suggested_max_length**|**smallint non NULL**|Vedere sys. Columns. per **max_length** Descrizione della colonna.|  
|**suggested_precision**|**tinyint non NULL**|Vedere sys. Columns. per la descrizione della colonna PRECISION.|  
|**suggested_scale**|**tinyint non NULL**|Vedere sys. Columns. per la descrizione della colonna SCALE.|  
|**suggested_user_type_id**|**int NULL**|Per i tipi di alias e CLR, contiene il valore user_type_id del tipo di dati della colonna come specificato in sys.types. In caso contrario, è NULL.|  
|**suggested_user_type_database**|**sysname NULL**|Per i tipi di alias e CLR, contiene il nome del database in cui è definito il tipo. In caso contrario, è NULL.|  
|**suggested_user_type_schema**|**sysname NULL**|Per i tipi di alias e CLR, contiene il nome dello schema in cui è definito il tipo. In caso contrario, è NULL.|  
|**suggested_user_type_name**|**sysname NULL**|Per i tipi di alias e CLR, contiene il nome del tipo. In caso contrario, è NULL.|  
|**suggested_assembly_qualified_type_name**|**nvarchar (4000) NULL**|Per i tipi CLR, restituisce il nome dell'assembly e la classe che definisce il tipo. In caso contrario, è NULL.|  
|**suggested_xml_collection_id**|**int NULL**|Contiene la xml_collection_id del tipo di dati del parametro come specificato in sys. Columns. Questa colonna restituisce NULL se il tipo restituito non è associato a una raccolta XML Schema.|  
|**suggested_xml_collection_database**|**sysname NULL**|Contiene il database in cui viene definita la raccolta XML Schema associata a questo tipo. Questa colonna restituisce NULL se il tipo restituito non è associato a una raccolta XML Schema.|  
|**suggested_xml_collection_schema**|**sysname NULL**|Contiene lo schema in cui viene definita la raccolta XML Schema associata a questo tipo. Questa colonna restituisce NULL se il tipo restituito non è associato a una raccolta XML Schema.|  
|**suggested_xml_collection_name**|**sysname NULL**|Contiene il nome della raccolta XML Schema associata a questo tipo. Questa colonna restituisce NULL se il tipo restituito non è associato a una raccolta XML Schema.|  
|**suggested_is_xml_document**|**bit NOT NULL**|Restituisce 1 se il tipo restituito è XML ed è garantito che il tipo sia un documento XML. In caso contrario, restituisce 0.|  
|**suggested_is_case_sensitive**|**bit NOT NULL**|Restituisce 1 se la colonna è di un tipo string che fa distinzione tra maiuscole e minuscole e 0 in caso contrario.|  
|**suggested_is_fixed_length_clr_type**|**bit NOT NULL**|Restituisce 1 se la colonna è di un tipo CLR a lunghezza fissa e 0 in caso contrario.|  
|**suggested_is_input**|**bit NOT NULL**|Restituisce 1 se il parametro viene utilizzato in qualsiasi posizione, a eccezione del lato sinistro di un'assegnazione. In caso contrario, restituisce 0.|  
|**suggested_is_output**|**bit NOT NULL**|Restituisce 1 se il parametro viene utilizzato sul lato sinistro di un'assegnazione o se viene passato a un parametro di output di una stored procedure. In caso contrario, restituisce 0.|  
|**formal_parameter_name**|**sysname NULL**|Se il parametro è un argomento per una stored procedure o una funzione definita dall'utente, restituisce il nome del parametro formale corrispondente. In caso contrario, viene restituito NULL.|  
|**suggested_tds_type_id**|**int NOT NULL**|Per uso interno.|  
|**suggested_tds_length**|**int NOT NULL**|Per uso interno.|  
  
## <a name="remarks"></a>Commenti  
 **sp_describe_undeclared_parameters** restituisce sempre lo stato restituito zero.  
  
 Lo scenario più comune si verifica quando a un'applicazione viene fornita un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] che potrebbe contenere parametri e li deve elaborare in qualche modo. Un esempio è dato da un'interfaccia utente quale ODBCTest o RowsetViewer in cui l'utente fornisce una query con la sintassi del parametro ODBC. L'applicazione deve individuare in modo dinamico il numero di parametri che verranno richiesti singolarmente all'utente.  
  
 Un altro esempio è dato dalla situazione in cui, senza l'input dell'utente, un'applicazione deve eseguire in ciclo i parametri e ottenere i relativi dati da altri percorsi, ad esempio una tabella. In questo caso non è necessario passare immediatamente tutte le informazioni sui parametri, ma è possibile ottenerle dal provider e acquisire i dati dalla tabella. Il codice che usa **sp_describe_undeclared_parameters** è più generico ed è meno probabile che sia necessario modificarlo se la struttura dei dati viene modificata in un secondo momento.  
  
 **sp_describe_undeclared_parameters** restituisce un errore nei casi seguenti.  
  
-   Se l'input \@ TSQL non è un [!INCLUDE[tsql](../../includes/tsql-md.md)] batch valido. La validità è determinata dall'analisi e dall'analisi del [!INCLUDE[tsql](../../includes/tsql-md.md)] batch. Eventuali errori causati dal batch durante l'ottimizzazione della query o durante l'esecuzione non vengono considerati quando si determina se il [!INCLUDE[tsql](../../includes/tsql-md.md)] batch è valido.  
  
-   Se \@ params non è null e contiene una stringa che non è una stringa di dichiarazione sintatticamente valida per i parametri, o se contiene una stringa che dichiara qualsiasi parametro più di una volta.  
  
-   Se il [!INCLUDE[tsql](../../includes/tsql-md.md)] batch di input dichiara una variabile locale con lo stesso nome di un parametro dichiarato in \@ params.  
  
- Se l'istruzione fa riferimento a tabelle temporanee.

- La query include la creazione di una tabella permanente sulla quale viene eseguita una query.
  
 Se \@ TSQL non dispone di parametri diversi da quelli dichiarati in \@ params, la procedura restituisce un set di risultati vuoto.  
  
## <a name="parameter-selection-algorithm"></a>Algoritmo di selezione dei parametri  
 Per una query con parametri non dichiarati, la deduzione del tipo di dati per i parametri non dichiarati si svolge in tre passaggi.  
  
 **Passaggio 1**  
  
 Il primo passaggio della deduzione del tipo di dati per una query con parametri non dichiarati consiste nel trovare i tipi di dati di tutte le sottoespressioni i cui tipi di dati non dipendono dai parametri non dichiarati. È possibile determinare il tipo per le espressioni seguenti:  
  
-   Colonne, costanti, variabili e parametri dichiarati.  
  
-   Risultati di una chiamata a una funzione definita dall'utente.  
  
-   Espressione con tipi di dati che non dipendono dai parametri non dichiarati per tutti gli input.  
  
 Si consideri, ad esempio, la query `SELECT dbo.tbl(@p1) + c1 FROM t1 WHERE c2 = @p2 + 2`. Le espressioni dbo. tbl ( \@ P1) + C1 e C2 hanno tipi di dati, mentre Expression \@ P1 e \@ P2 + 2 non lo sono.  
  
 Dopo questo passaggio, se un'espressione, che non sia una chiamata a una funzione definita dall'utente, dispone di due argomenti senza tipi di dati, si verifica un errore durante la deduzione dei tipi. In tutti gli esempi seguenti si verificano errori:  
  
```sql
SELECT * FROM t1 WHERE @p1 = @p2  
SELECT * FROM t1 WHERE c1 = @p1 + @p2  
SELECT * FROM t1 WHERE @p1 = SUBSTRING(@p2, 2, 3)  
```  
  
 Nell'esempio seguente non viene generato alcun errore:  
  
```sql
SELECT * FROM t1 WHERE @p1 = dbo.tbl(c1, @p2, @p3)  
```
  
 **Passaggio 2**  
  
 Per un determinato parametro non dichiarato \@ p, l'algoritmo di deduzione dei tipi trova l'espressione più interna e ( \@ p) che contiene \@ p ed è uno dei seguenti:  
  
-   Argomento per un operatore di confronto o assegnazione.  
  
-   Argomento per una funzione definita dall'utente, incluse funzioni definite dall'utente con valori di tabella, procedura o metodo.  
  
-   Argomento di una clausola **values** di un'istruzione **Insert** .  
  
-   Argomento per un **cast** o una **conversione**.  
  
 L'algoritmo di deduzione dei tipi trova un tipo di dati di destinazione TT ( \@ p) per E ( \@ p). I tipi di dati di destinazione per gli esempi precedenti sono i seguenti:  
  
-   Tipo di dati dell'altro lato dell'operatore di confronto o assegnazione.  
  
-   Tipo di dati dichiarato del parametro a cui viene passato questo argomento.  
  
-   Tipo di dati della colonna in cui viene inserito questo valore.  
  
-   Tipo di dati nel quale l'istruzione esegue il cast o la conversione.  
  
 Si consideri, ad esempio, la query `SELECT * FROM t WHERE @p1 = dbo.tbl(@p2 + c1)`. Quindi E ( \@ P1) = \@ P1, e ( \@ P2) = \@ P2 + C1, TT ( \@ P1) è il tipo di dati restituito dichiarato di dbo. tbl e TT ( \@ P2) è il tipo di dati del parametro dichiarato per dbo. tbl.  
  
 Se \@ p non è incluso in alcuna espressione elencata all'inizio del passaggio 2, l'algoritmo di deduzione dei tipi determina che E ( \@ p) è l'espressione scalare più grande che contiene \@ p e l'algoritmo di deduzione del tipo non calcola un tipo di dati di destinazione TT ( \@ p) per E ( \@ p). Ad esempio, se la query è SELECT `@p + 2` , e ( \@ p) = \@ p + 2 e non è presente TT ( \@ p).  
  
 **Passaggio 3**  
  
 Ora che \@ sono stati identificati e (p) e TT ( \@ p), l'algoritmo di deduzione del tipo deduce un tipo di dati per \@ p in uno dei due modi seguenti:  
  
-   Deduzione semplice  
  
     Se E ( \@ p) = \@ p e TT ( \@ p) esiste, ad esempio se \@ p è direttamente un argomento di una delle espressioni elencate all'inizio del passaggio 2, l'algoritmo di deduzione dei tipi deduce il tipo di dati \@ p come TT ( \@ p). Ad esempio:  
  
    ```sql
    SELECT * FROM t WHERE c1 = @p1 AND @p2 = dbo.tbl(@p3)  
    ```  
  
     Il tipo di dati per \@ P1, \@ P2 e \@ P3 sarà il tipo di dati C1, il tipo di dati restituito di dbo. tbl e il tipo di dati Parameter per dbo. tbl rispettivamente.  
  
     Come caso speciale, se \@ p è un argomento di un \<, > operatore, \<=, or > =, le regole di deduzione semplice non si applicano. L'algoritmo di deduzione dei tipi utilizzerà le regole della deduzione generale illustrate nella sezione successiva. Se ad esempio c1 è una colonna del tipo di dati char(30), si considerino le due query seguenti:  
  
    ```sql
    SELECT * FROM t WHERE c1 = @p  
    SELECT * FROM t WHERE c1 > @p  
    ```  
  
     Nel primo caso, l'algoritmo di deduzione dei tipi deduce **char (30)** come tipo di dati per \@ p come per le regole precedenti in questo argomento. Nel secondo caso, l'algoritmo di deduzione dei tipi deduce **varchar (8000)** in base alle regole di deduzione generale nella sezione successiva.  
  
-   Deduzione generale  
  
     Se la deduzione semplice non è applicabile, si considerino i tipi di dati seguenti per i parametri non dichiarati:  
  
    -   Tipi di dati Integer (**bit**, **tinyint**, **smallint**, **int**, **bigint**)  
  
    -   Tipi di dati Money (**smallmoney**, **Money**)  
  
    -   Tipi di dati a virgola mobile (**float**, **Real**)  
  
    -   **numeric (38, 19)** : non vengono considerati altri tipi di dati numerici o decimali.  
  
    -   **varchar (8000)**, **varchar (max)**, **nvarchar (4000)** e **nvarchar (max)** . altri tipi di dati stringa, ad esempio **Text**, **char (8000)**, **nvarchar (30)** e così via, non vengono considerati.  
  
    -   **varbinary (8000)** e **varbinary (max)** : gli altri tipi di dati binari non vengono considerati, ad esempio **Image**, **Binary (8000)**, **varbinary (30)** e così via).  
  
    -   **date**, **time (7)**, **smalldatetime**, **DateTime**, **datetime2 (7)**, **DateTimeOffset (7)** . altri tipi di data e ora, ad esempio **time (4)**, non vengono considerati.  
  
    -   **sql_variant**  
  
    -   **xml**  
  
    -   Tipi CLR definiti dal sistema (**hierarchyid**, **Geometry**, **geography**)  
  
    -   Tipi CLR definiti dall'utente  
  
### <a name="selection-criteria"></a>Criteri di selezione  
 Di tutti i tipi di dati candidati, qualsiasi tipo di dati che renderebbe non valida la query viene rifiutato. Dei tipi di dati candidati rimanenti, l'algoritmo di deduzione dei tipi ne seleziona uno in base alle regole seguenti.  
  
1.  Viene selezionato il tipo di dati che produce il minor numero di conversioni implicite in E ( \@ p). Se un tipo di dati specifico produce un tipo di dati per E ( \@ p) diverso da TT ( \@ p), l'algoritmo di deduzione dei tipi considera che questo sia una conversione implicita aggiuntiva dal tipo di dati di E ( \@ p) a TT ( \@ p).  
  
     Ad esempio:  
  
    ```sql
    SELECT * FROM t WHERE Col_Int = Col_Int + @p  
    ```  
  
     In questo caso, E ( \@ p) è Col_Int + \@ p e TT ( \@ p) è **int**. viene scelto **int** per \@ p perché non produce conversioni implicite. Qualsiasi altra scelta del tipo di dati produce almeno una conversione implicita.  
  
2.  Se più tipi di dati hanno un valore equivalente per il numero più piccolo di conversioni, viene utilizzato il tipo di dati con la precedenza maggiore. Ad esempio:  
  
    ```sql
    SELECT * FROM t WHERE Col_Int = Col_smallint + @p  
    ```  
  
     In questo caso, **int** e **smallint** producono una conversione. Ogni altro tipo di dati produce più di una conversione. Poiché **int** ha la precedenza su **smallint**, viene usato **int** per \@ p. Per ulteriori informazioni sulla precedenza dei tipi di dati, vedere la pagina relativa alla [precedenza dei tipi di dati &#40;&#41;Transact-SQL ](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
  
     Questa regola è applicabile solo in presenza di una conversione implicita tra ogni tipo di dati con valori equivalenti in base alla regola 1 e il tipo di dati con la precedenza maggiore. In assenza di una conversione implicita, la deduzione dei tipi di dati genera un errore. Nella query, ad esempio `SELECT @p FROM t` , la deduzione del tipo di dati non riesce perché qualsiasi tipo di dati per \@ p sarebbe ugualmente valido. Ad esempio, non esiste alcuna conversione implicita da **int** a **XML**.  
  
3.  Se due tipi di dati simili sono collegati alla regola 1, ad esempio **varchar (8000)** e **varchar (max)**, viene scelto il tipo di dati più piccolo (**varchar (8000)**). Lo stesso principio si applica ai tipi di dati **nvarchar** e **varbinary** .  
  
4.  Ai fini della regola 1, l'algoritmo di deduzione dei tipi preferisce alcune conversioni ad altre. Le conversioni dalla migliore alla peggiore sono:  

    1.  Conversione tra lo stesso tipo di dati di base di lunghezza diversa.  
  
    2.  Conversione tra le versioni a lunghezza fissa e a lunghezza variabile degli stessi tipi di dati (ad esempio, **char** a **varchar**).  
  
    3.  Conversione tra **valori null** e **int**.  
  
    4.  Qualsiasi altra conversione.  
  
 Per la query, ad esempio `SELECT * FROM t WHERE [Col_varchar(30)] > @p` , viene scelto **varchar (8000)** perché la conversione (a) è migliore. Per la query `SELECT * FROM t WHERE [Col_char(30)] > @p` , **varchar (8000)** viene ancora scelto perché causa una conversione di tipo (b) e perché un'altra scelta (ad esempio **varchar (4000)**) provocherebbe una conversione di tipo (d).  
  
 Come esempio finale, data una query `SELECT NULL + @p` , viene scelto **int** per \@ p perché comporta una conversione di tipo (c).  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione per eseguire l' \@ argomento TSQL.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituite informazioni quali il tipo di dati previsto per i parametri `@id` e `@name` non dichiarati.  
  
```sql
sp_describe_undeclared_parameters @tsql =   
N'SELECT object_id, name, type_desc   
FROM sys.indexes  
WHERE object_id = @id OR name = @name'  
  
```  
  
 Quando il parametro `@id` viene specificato come un riferimento `@params`, il parametro `@id` viene omesso dal set di risultati e solo il parametro `@name` viene descritto.  
  
```sql
sp_describe_undeclared_parameters @tsql =   
N'SELECT object_id, name, type_desc   
FROM sys.indexes  
WHERE object_id = @id OR NAME = @name',  
@params = N'@id int'  
  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_describe_first_result_set &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md)   
 [sys.dm_exec_describe_first_result_set &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-transact-sql.md)   
 [sys.dm_exec_describe_first_result_set_for_object &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-for-object-transact-sql.md)
