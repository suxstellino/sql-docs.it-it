---
description: EXECUTE (Transact-SQL)
title: EXECUTE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- EXEC
- EXECUTE_TSQL
- EXECUTE
- EXEC_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- remote stored procedures [SQL Server]
- command strings [SQL Server]
- extended stored procedures [SQL Server], executing
- stored procedures [SQL Server], executing
- Transact-SQL statements, executing
- strings [SQL Server], executing
- statements [SQL Server], executing
- context switching [SQL Server], execution context
- user-defined functions [SQL Server], executing
- character strings [SQL Server], executing
- switching execution context
- EXECUTE statement
ms.assetid: bc806b71-cc55-470a-913e-c5f761d5c4b7
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 37acb46f1e8fd238703d5d6af1d7fd84881f032f
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749891"
---
# <a name="execute-transact-sql"></a>EXECUTE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Esegue una stringa di comando o una stringa di caratteri all'interno di un batch [!INCLUDE[tsql](../../includes/tsql-md.md)] oppure uno dei moduli seguenti: stored procedure di sistema, stored procedure definita dall'utente, stored procedure CLR, funzione definita dall'utente a valori scalari o stored procedure estesa. L'istruzione EXECUTE può essere utilizzata per inviare comandi pass-through ai server collegati. È inoltre possibile impostare in modo esplicito il contesto di esecuzione di una stringa o di un comando. I metadati per il set di risultati possono essere definiti tramite le opzioni WITH RESULT SETS.
  
> [!IMPORTANT]  
>  Prima di chiamare l'istruzione EXECUTE con una stringa di caratteri, convalidare la stringa di caratteri. Non eseguire mai un comando costruito in base a input utente non convalidato.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  

::: moniker range=">=sql-server-ver15"
Il blocco di codice seguente illustra la sintassi in SQL Server 2019. In alternativa, vedere la [sintassi in SQL Server 2017 e versioni precedenti](execute-transact-sql.md?view=sql-server-2017&preserve-view=true). 

```syntaxsql
-- Syntax for SQL Server 2019
  
Execute a stored procedure or function  
[ { EXEC | EXECUTE } ]  
    {   
      [ @return_status = ]  
      { module_name [ ;number ] | @module_name_var }   
        [ [ @parameter = ] { value   
                           | @variable [ OUTPUT ]   
                           | [ DEFAULT ]   
                           }  
        ]  
      [ ,...n ]  
      [ WITH <execute_option> [ ,...n ] ]  
    }  
[;]  
  
Execute a character string  
{ EXEC | EXECUTE }   
    ( { @string_variable | [ N ]'tsql_string' } [ + ...n ] )  
    [ AS { LOGIN | USER } = ' name ' ]  
[;]  
  
Execute a pass-through command against a linked server  
{ EXEC | EXECUTE }  
    ( { @string_variable | [ N ] 'command_string [ ? ]' } [ + ...n ]  
        [ { , { value | @variable [ OUTPUT ] } } [ ...n ] ]  
    )   
    [ AS { LOGIN | USER } = ' name ' ]  
    [ AT linked_server_name ]  
    [ AT DATA_SOURCE data_source_name ]  
[;]  
  
<execute_option>::=  
{  
        RECOMPILE   
    | { RESULT SETS UNDEFINED }   
    | { RESULT SETS NONE }   
    | { RESULT SETS ( <result_sets_definition> [,...n ] ) }  
}   
  
<result_sets_definition> ::=   
{  
    (  
         { column_name   
           data_type   
         [ COLLATE collation_name ]   
         [ NULL | NOT NULL ] }  
         [,...n ]  
    )  
    | AS OBJECT   
        [ db_name . [ schema_name ] . | schema_name . ]   
        {table_name | view_name | table_valued_function_name }  
    | AS TYPE [ schema_name.]table_type_name  
    | AS FOR XML   
}  
```  
::: moniker-end

::: moniker range=">=sql-server-2016"

Il blocco di codice seguente illustra la sintassi in SQL Server 2017 e versioni precedenti. In alternativa, vedere la [sintassi in SQL Server 2019](execute-transact-sql.md?view=sql-server-ver15&preserve-view=true).


```syntaxsql
-- Syntax for SQL Server 2017 and earleir  
  
Execute a stored procedure or function  
[ { EXEC | EXECUTE } ]  
    {   
      [ @return_status = ]  
      { module_name [ ;number ] | @module_name_var }   
        [ [ @parameter = ] { value   
                           | @variable [ OUTPUT ]   
                           | [ DEFAULT ]   
                           }  
        ]  
      [ ,...n ]  
      [ WITH <execute_option> [ ,...n ] ]  
    }  
[;]  
  
Execute a character string  
{ EXEC | EXECUTE }   
    ( { @string_variable | [ N ]'tsql_string' } [ + ...n ] )  
    [ AS { LOGIN | USER } = ' name ' ]  
[;]  
  
Execute a pass-through command against a linked server  
{ EXEC | EXECUTE }  
    ( { @string_variable | [ N ] 'command_string [ ? ]' } [ + ...n ]  
        [ { , { value | @variable [ OUTPUT ] } } [ ...n ] ]  
    )   
    [ AS { LOGIN | USER } = ' name ' ]  
    [ AT linked_server_name ]  
[;]  
  
<execute_option>::=  
{  
        RECOMPILE   
    | { RESULT SETS UNDEFINED }   
    | { RESULT SETS NONE }   
    | { RESULT SETS ( <result_sets_definition> [,...n ] ) }  
}   
  
<result_sets_definition> ::=   
{  
    (  
         { column_name   
           data_type   
         [ COLLATE collation_name ]   
         [ NULL | NOT NULL ] }  
         [,...n ]  
    )  
    | AS OBJECT   
        [ db_name . [ schema_name ] . | schema_name . ]   
        {table_name | view_name | table_valued_function_name }  
    | AS TYPE [ schema_name.]table_type_name  
    | AS FOR XML   
}  
```  
::: moniker-end

  
```syntaxsql
-- In-Memory OLTP   

Execute a natively compiled, scalar user-defined function  
[ { EXEC | EXECUTE } ]   
    {   
      [ @return_status = ]   
      { module_name | @module_name_var }   
        [ [ @parameter = ] { value   
                           | @variable   
                           | [ DEFAULT ]   
                           }  
        ]   
      [ ,...n ]   
      [ WITH <execute_option> [ ,...n ] ]   
    }  
<execute_option>::=  
{  
    | { RESULT SETS UNDEFINED }   
    | { RESULT SETS NONE }   
    | { RESULT SETS ( <result_sets_definition> [,...n ] ) }  
}  
```  
  
```syntaxsql
-- Syntax for Azure SQL Database   
  
Execute a stored procedure or function  
[ { EXEC | EXECUTE } ]  
    {   
      [ @return_status = ]  
      { module_name  | @module_name_var }   
        [ [ @parameter = ] { value   
                           | @variable [ OUTPUT ]   
                           | [ DEFAULT ]   
                           }  
        ]  
      [ ,...n ]  
      [ WITH RECOMPILE ]  
    }  
[;]  
  
Execute a character string  
{ EXEC | EXECUTE }   
    ( { @string_variable | [ N ]'tsql_string' } [ + ...n ] )  
    [ AS {  USER } = ' name ' ]  
[;]  
  
<execute_option>::=  
{  
        RECOMPILE   
    | { RESULT SETS UNDEFINED }   
    | { RESULT SETS NONE }   
    | { RESULT SETS ( <result_sets_definition> [,...n ] ) }  
}   
  
<result_sets_definition> ::=   
{  
    (  
         { column_name   
           data_type   
         [ COLLATE collation_name ]   
         [ NULL | NOT NULL ] }  
         [,...n ]  
    )  
    | AS OBJECT   
        [ db_name . [ schema_name ] . | schema_name . ]   
        {table_name | view_name | table_valued_function_name }  
    | AS TYPE [ schema_name.]table_type_name  
    | AS FOR XML  
  
```

  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  

-- Execute a stored procedure  
[ { EXEC | EXECUTE } ]  
    procedure_name   
        [ { value | @variable [ OUT | OUTPUT ] } ] [ ,...n ] }  
[;]  
  
-- Execute a SQL string  
{ EXEC | EXECUTE }  
    ( { @string_variable | [ N ] 'tsql_string' } [ +...n ] )  
[;]  
```  


  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 @*return_status*  
 Variabile di tipo integer facoltativa in cui viene archiviato lo stato di restituzione di un modulo. Prima di utilizzare questa variabile in un'istruzione EXECUTE, è necessario dichiararla nel batch, nella stored procedure o nella funzione.  
  
 Se usata per chiamare una funzione definita dall'utente a valori scalari, la variabile @*return_status* può essere di qualsiasi tipo di dati scalari.  
  
 *module_name*  
 Nome completo o non qualificato della stored procedure o della funzione con valori scalari definita dall'utente da chiamare. I nomi di modulo devono essere conformi alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md). Per i nomi delle stored procedure estese la combinazione di maiuscole e minuscole è sempre rilevante, indipendentemente dalle regole di confronto del server.  
  
 Un modulo che è possibile creare in un altro database può essere eseguito se l'utente che lo esegue è il proprietario del modulo o dispone delle autorizzazioni appropriate per eseguirlo in tale database. È possibile eseguire un modulo in un altro server che esegue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se l'utente che lo esegue dispone delle autorizzazioni appropriate per l'utilizzo di tale server (accesso remoto) e per l'esecuzione del modulo nel database specifico. Se si specifica il nome del server ma non quello del database, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] esegue automaticamente la ricerca del modulo nel database predefinito dell'utente.  
  
 ;*number*  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive
  
 Numero intero facoltativo utilizzato per raggruppare le stored procedure con lo stesso nome. Questo parametro non viene usato per stored procedure estese.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 Per altre informazioni sui gruppi di procedure, vedere [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md).  
  
 @*module_name_var*  
 Nome di una variabile definita localmente che rappresenta il nome di un modulo.  
  
 Può essere una variabile che contiene il nome di una funzione scalare definita dall'utente compilata in modo nativo.  
  
 @*parameter*  
 Parametro per *module_name*, come definito nel modulo. I nomi dei parametri devono iniziare con il simbolo di chiocciola (@). Se usati nel formato @*parameter_name*=*value*, i nomi dei parametri e le costanti non devono essere specificati nell'ordine in cui sono definiti nel modulo. Se tuttavia il formato @*parameter_name*=*value* viene usato per uno dei parametri, è necessario usarlo per tutti i parametri successivi.  
  
 Per impostazione predefinita, i parametri ammettono valori Null.  
  
 *value*  
 Valore del parametro da passare al modulo o al comando pass-through. Se i nomi dei parametri vengono omessi, è necessario immettere i relativi valori in base all'ordine definito nel modulo.  
  
 Durante l'esecuzione di comandi pass-through in server collegati, l'ordine dei valori dei parametri dipende dal provider OLE DB del server collegato. La maggior parte dei provider OLE DB associa i valori ai parametri da sinistra a destra.  
  
 Se il valore di un parametro è il nome di un oggetto, una stringa di caratteri o un nome qualificato dal nome del database o dello schema, l'intero nome deve essere racchiuso tra virgolette singole. Se il valore di un parametro è rappresentato da una parola chiave, questa deve essere racchiusa tra virgolette doppie.  
  
Se si passa una singola parola che non inizia con `@` e che non è racchiusa tra virgolette, ad esempio se si omette `@` nel nome di un parametro, la parola viene considerata come una stringa di tipo nvarchar, nonostante le virgolette mancanti.

 Se nel modulo è definito un valore predefinito, l'utente può eseguire il modulo senza specificare un parametro.  
  
 Il valore predefinito può essere inoltre NULL. In genere nella definizione del modulo sono specificate le operazioni da eseguire se un valore di parametro è NULL.  
  
 @*variable*  
 Variabile in cui è archiviato un parametro o un parametro restituito.  
  
 OUTPUT  
 Specifica che il modulo o la stringa di comando restituisce un parametro. Il parametro corrispondente nel modulo o nella stringa di comando deve essere creato tramite la parola chiave OUTPUT. Specificare questa parola chiave quando come parametri si utilizzano variabili di cursore.  
  
 Se *value* viene definito come OUTPUT di un modulo eseguito in un server collegato, qualsiasi modifica apportata al parametro @*parameter* corrispondente eseguito dal provider OLE DB verrà nuovamente copiata nella variabile al termine dell'esecuzione del modulo.  
  
 Se vengono usati parametri OUTPUT e si vogliono usare i valori restituiti in altre istruzioni del batch o del modulo chiamante, il valore del parametro deve essere passato come variabile, ovvero @*parameter* = @*variable*. Non è possibile eseguire un modulo specificando la parola chiave OUTPUT per un parametro non definito come parametro OUTPUT nel modulo. Le costanti non possono essere passate al modulo utilizzando la parola chiave OUTPUT. Il parametro restituito richiede il nome di una variabile. Prima di eseguire una procedura, è necessario dichiarare il tipo dei dati della variabile e assegnare un valore.  
  
 Se si utilizza l'istruzione EXECUTE in una stored procedure remota oppure si esegue un comando pass-through in un server collegato, i parametri OUTPUT non possono essere di nessuno dei tipi di dati LOB.  
  
 I parametri restituiti possono essere di un tipo di dati qualsiasi, tranne i tipi di dati LOB.  
  
 DEFAULT  
 Valore predefinito del parametro, come definito nel modulo. Quando nel modulo è previsto un valore per un parametro privo di valore predefinito ed è stato omesso un parametro o è stata specificata la parola chiave DEFAULT, viene generato un errore.  
  
 @*string_variable*  
 Nome di una variabile locale. @*string_variable* può essere di tipo **char**, **varchar**, **nchar** o **nvarchar**. Sono inclusi i tipi di dati **(max)** .  
  
 [N] '*tsql_string*'  
 Valore stringa costante. *tsql_string* può essere di tipo **nvarchar** o **varchar**. Se si specifica N, la stringa viene interpretata come di tipo **nvarchar**.  
  
 AS \<context_specification>  
 Specifica il contesto in cui viene eseguita l'istruzione.  
  
 LOGIN  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive
  
 Specifica che il contesto da rappresentare è un account di accesso. L'ambito di rappresentazione è il server.  
  
 USER  
 Specifica che il contesto da rappresentare è un utente nel database corrente. L'ambito di rappresentazione è limitato al database corrente. Un cambio di contesto a un utente del database non eredita le autorizzazioni a livello di server di tale utente.  
  
> [!IMPORTANT]  
>  Mentre il cambio di contesto all'utente del database è attivo, qualsiasi tentativo di accesso alle risorse esterne al database comporterà l'esito negativo dell'esecuzione dell'istruzione. Ciò è valido per le istruzioni USE *database*, per le query distribuite e per le query che fanno riferimento a un altro database tramite l'uso di identificatori in tre o quattro parti.  
  
 '*name*'  
 Nome utente o nome account di accesso valido. *name* deve essere membro del ruolo predefinito del server sysadmin oppure esistere come entità di sicurezza rispettivamente in [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md) o in [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md).  
  
 *name* non può essere un account predefinito, ad esempio NT AUTHORITY\LocalService, NT AUTHORITY\NetworkService o NT AUTHORITY\LocalSystem.  
  
 Per altre informazioni, vedere [Specifica di un nome utente o un nome account di accesso](#_user) di seguito in questo argomento.  
  
 [N] '*command_string*'  
 Stringa costante contenente il comando da passare al server collegato. Se si specifica N, la stringa viene interpretata come di tipo **nvarchar**.  
  
 [?]  
 Indica i parametri per i quali vengono specificati i valori nell'elemento \<arg-list> dei comandi pass-through usati in un'istruzione EXEC('...', \<arg-list>) AT \<linkedsrv>.  
  
 AT *linked_server_name*  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive
  
 Specifica che *command_string* viene eseguita in *linked_server_name* e che gli eventuali risultati vengono restituiti al client. *linked_server_name* deve fare riferimento a una definizione esistente nel server locale di un server collegato. I server collegati vengono definiti tramite [sp_addlinkedserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md).  
  
 WITH \<execute_option>  
 Opzioni di esecuzione possibili. Non è possibile specificare opzioni RESULT SETS in un'istruzione INSERT...EXEC.  
 
AT DATA_SOURCE data_source_name **Si applica a**: [!INCLUDE[sssql19](../../includes/sssql19-md.md)] e versioni successive
  
 Specifica che *command_string* viene eseguita in *data_source_name* e che gli eventuali risultati vengono restituiti al client. *data_source_name* deve fare riferimento a una definizione EXTERNAL DATA SOURCE esistente nel database. Sono supportate solo le origini dati che puntano a SQL Server. Inoltre, per le origini dati del cluster Big Data di SQL Server che puntano a un pool di calcolo, è supportato il pool di dati o di archiviazione. Le origini dati vengono definite usando [CREATE EXTERNAL DATA SOURCE](../statements/create-external-data-source-transact-sql.md).  
  
 WITH \<execute_option>  
 Opzioni di esecuzione possibili. Non è possibile specificare opzioni RESULT SETS in un'istruzione INSERT...EXEC.  
  
|Termine|Definizione|  
|----------|----------------|  
|RECOMPILE|Forza la compilazione, l'utilizzo e l'eliminazione di un nuovo piano dopo l'esecuzione del modulo. Se per il modulo è disponibile un piano di query esistente, tale piano rimane nella cache.<br /><br /> Utilizzare questa opzione se il parametro fornito è atipico oppure se i dati sono cambiati notevolmente. Questa opzione non viene utilizzata per stored procedure estese. È consigliabile utilizzarla solo quando è strettamente necessario, in quanto si tratta di un'opzione onerosa.<br /><br /> **Nota:** non è possibile usare WITH RECOMPILE quando viene chiamata una stored procedure che usa la sintassi OPENDATASOURCE. Quando viene specificato un nome di oggetto composto da quattro parti, l'opzione WITH RECOMPILE viene ignorata.<br /><br /> **Nota:** le funzioni scalari definite dall'utente compilate in modo nativo non supportano RECOMPILE. Se è necessario ricompilare, usare [sp_recompile &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-recompile-transact-sql.md).|  
|**RESULT SETS UNDEFINED**|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].<br /><br /> Questa opzione non fornisce alcuna garanzia sugli eventuali risultati restituiti e non viene specificata alcuna definizione. L'istruzione viene eseguita senza errore se vengono restituiti risultati o se non ne vengono restituiti. RESULT SETS UNDEFINED rappresenta il comportamento predefinito se result_sets_option non viene specificato.<br /><br /> Per le funzioni scalari definite dall'utente interpretate e per le funzioni scalari definite dall'utente compilate in modo nativo, questa opzione non è operativa perché le funzioni non restituiscono mai un set di risultati.|  
|RESULT SETS NONE|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].<br /><br /> Garantisce che l'istruzione di esecuzione non restituirà risultati. Se vengono restituiti risultati il batch viene interrotto.<br /><br /> Per le funzioni scalari definite dall'utente interpretate e per le funzioni scalari definite dall'utente compilate in modo nativo, questa opzione non è operativa perché le funzioni non restituiscono mai un set di risultati.|  
|*\<result_sets_definition>*|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].<br /><br /> Garantisce che il risultato verrà restituito come specificato in result_sets_definition. Per le istruzioni che restituiscono più set di risultati, specificare più sezioni *result_sets_definition*. Racchiudere ogni sezione *result_sets_definition* tra parentesi, separando le diverse sezioni con virgole. Per altre informazioni, vedere \<result_sets_definition>, più avanti in questo argomento.<br /><br /> Questa opzione genera sempre un errore per le funzioni scalari definite dall'utente compilate in modo nativo, poiché le funzioni non restituiscono mai un set di risultati.|
  
\<result_sets_definition>
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Descrive i set di risultati restituiti dalle istruzioni eseguite. Le clausole di result_sets_definition hanno il significato seguente  
  
|Termine|Definizione|  
|----------|----------------|  
|{<br /><br /> column_name<br /><br /> data_type<br /><br /> [ COLLATE collation_name]<br /><br /> [NULL &#124; NOT NULL]<br /><br /> }|Vedere la tabella riportata di seguito.|  
|db_name|Nome del database contenente la tabella, la vista o la funzione con valori di tabella.|  
|schema_name|Nome dello schema proprietario della tabella, della vista o della funzione con valori di tabella.|  
|table_name &#124; view_name &#124; table_valued_function_name|Specifica che le colonne restituite saranno quelle specificate nella tabella, nella vista o nella funzione con valori di tabella denominata. La sintassi degli oggetti AS non supporta i sinonimi, le tabelle temporanee e le variabili di tabella.|  
|AS TYPE [schema_name.]table_type_name|Specifica che le colonne restituite saranno quelle specificate nel tipo della tabella.|  
|AS FOR XML|Specifica che i risultati XML dell'istruzione o della stored procedure chiamata dall'istruzione EXECUTE vengono convertiti nel formato come se fossero prodotti da un'istruzione SELECT ... FOR XML ... Tutta la formattazione dalle direttive type nell'istruzione originale viene rimossa e i risultati vengono restituiti come se non fosse stata specificata alcuna direttiva type. AS FOR XML non converte i risultati tabulari non XML dall'istruzione o dalla stored procedure eseguita in XML.|  
  
|Termine|Definizione|  
|----------|----------------|  
|column_name|Nomi di ogni colonna. Se il numero di colonne è diverso dal set di risultati, si verifica un errore e il batch viene interrotto. Se il nome di una colonna è diverso dal set di risultati, il nome della colonna restituito verrà impostato sul nome definito.|  
|data_type|Tipi di dati di ogni colonna. Se i tipi di dati sono diversi, viene eseguita una conversione implicita al tipo di dati definito. Se la conversione ha esito negativo il batch viene interrotto|  
|COLLATE collation_name|Regole di confronto di ogni colonna. In caso di mancata corrispondenza tra regole di confronto, vengono tentate regole di confronto implicite. Se la conversione ha esito negativo il batch viene interrotto.|  
|NULL &#124; NOT NULL|Ammissione di valori Null di ogni colonna. Se l'ammissione di valori Null definita è NOT NULL e i dati restituiti contengono NULL si verifica un errore e il batch viene interrotto. Se non è specificato, il valore predefinito si allinea all'impostazione delle opzioni ANSI_NULL_DFLT_ON e ANSI_NULL_DFLT_OFF.|  
  
 Il set di risultati effettivo restituito durante l'esecuzione può essere diverso dal risultato definito tramite la clausola WITH RESULT SETS in uno dei modi seguenti: numero di set di risultati, numero di colonne, nome della colonna, ammissione di valori Null e tipo di dati. Se il numero di set di risultati è diverso, si verifica un errore e il batch viene interrotto.  
  
## <a name="remarks"></a>Osservazioni  
 È possibile specificare parametri usando *value* o @*parameter_name*=*value*. Un parametro non fa parte di una transazione. Se un parametro viene modificato in una transazione per la quale verrà eseguito il rollback, il valore del parametro non viene ripristinato al suo valore precedente. Il valore restituito al chiamante corrisponde sempre al valore specificato al termine del modulo.  
  
 La nidificazione si verifica quando un modulo ne chiama un altro o quando esegue codice gestito tramite riferimenti a un modulo CLR (Common Language Runtime), un tipo definito dall'utente o una funzione di aggregazione. Il livello di nidificazione viene incrementato quando il modulo chiamato o il riferimento al codice gestito viene eseguito, mentre viene decrementato al termine dell'esecuzione del modulo chiamato o del riferimento al codice gestito. Se viene superato il numero massimo di 32 livelli di nidificazione, l'intera catena di chiamata ha esito negativo. Il livello di annidamento corrente viene archiviato nella funzione di sistema @@NESTLEVEL.  
  
 Poiché le stored procedure remote ed estese non rientrano nell'ambito di una transazione, a meno che non siano eseguite in un'istruzione BEGIN DISTRIBUTED TRANSACTION o utilizzate con diverse opzioni di configurazione, non è possibile eseguire il rollback dei comandi eseguiti tramite chiamate a tali stored procedure. Per altre informazioni, vedere [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md) e [BEGIN DISTRIBUTED TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md).  
  
 Quando vengono utilizzate variabili cursore, se si esegue una procedura che passa una variabile cursore con un cursore assegnato, viene generato un errore.  
  
 Se l'istruzione è la prima di un batch, non è necessario specificare la parola chiave EXECUTE durante l'esecuzione dei moduli.  
  
 Per informazioni aggiuntive specifiche delle stored procedure CLR, vedere Stored procedure CLR.  
  
## <a name="using-execute-with-stored-procedures"></a>Utilizzo di EXECUTE con stored procedure  
 Se l'istruzione è la prima di un batch, non è necessario specificare la parola chiave EXECUTE durante l'esecuzione delle stored procedure.  
  
 I nomi delle stored procedure di sistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] iniziano con i caratteri sp_. Tali stored procedure vengono archiviate nel [database Resource](../../relational-databases/databases/resource-database.md), ma sono visualizzate logicamente nello schema sys di ogni database di sistema e di ogni database definito dall'utente. Se si esegue una stored procedure di sistema in un batch oppure all'interno di un modulo quale una stored procedure o una funzione definita dall'utente, è consigliabile qualificare il nome della stored procedure con il nome dello schema sys.  
  
 I nomi delle stored procedure estese di sistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] iniziano con i caratteri xp_ e tali stored procedure sono incluse nello schema dbo del database master. Se si esegue una stored procedure estesa di sistema in un batch oppure all'interno di un modulo quale una stored procedure o una funzione definita dall'utente, è consigliabile qualificare il nome della stored procedure con master.dbo.  
  
 Se si esegue una stored procedure definita dall'utente in un batch o all'interno di un modulo quale una funzione o una stored procedure definita dall'utente, è consigliabile qualificare il nome della stored procedure con un nome di schema. Non è consigliabile assegnare a una stored procedure definita dall'utente lo stesso nome di una stored procedure di sistema. Per altre informazioni sull'esecuzione di stored procedure, vedere [Eseguire una stored procedure](../../relational-databases/stored-procedures/execute-a-stored-procedure.md).  
  
## <a name="using-execute-with-a-character-string"></a>Utilizzo dell'istruzione EXECUTE con una stringa di caratteri  
 Nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le stringhe di caratteri sono limitate a 8.000 byte. Ciò richiede la concatenazione di stringhe di grandi dimensioni per l'esecuzione dinamica. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è possibile specificare i tipi di dati **varchar(max)** e **nvarchar(max)** che supportano stringhe di caratteri fino a 2 GB di dati.  
  
 Le modifiche al contesto del database rimangono effettive solo fino al termine dell'esecuzione di EXECUTE. Ad esempio, dopo l'esecuzione di `EXEC` nell'istruzione seguente, il contesto di database è master.  
  
```sql  
USE master; EXEC ('USE AdventureWorks2012; SELECT BusinessEntityID, JobTitle FROM HumanResources.Employee;');  
```  
  
## <a name="context-switching"></a>Cambio di contesto  
 È possibile utilizzare la clausola `AS { LOGIN | USER } = ' name '` per cambiare il contesto di esecuzione di un'istruzione dinamica. Se il cambio di contesto viene specificato come `EXECUTE ('string') AS <context_specification>`, la durata del cambio di contesto è limitata all'ambito della query in fase di esecuzione.  
  
###  <a name="specifying-a-user-or-login-name"></a><a name="_user"></a> Indicazione di un nome utente o di un ID di accesso  
 Il nome utente o nome account di accesso specificato in `AS { LOGIN | USER } = ' name '` deve esistere come entità rispettivamente in sys.database_principals o sys.server_principals. In caso contrario, l'istruzione avrà esito negativo. È inoltre necessario concedere le autorizzazioni IMPERSONATE per l'entità. A meno che il chiamante non sia il proprietario del database o membro del ruolo predefinito del server sysadmin, l'entità deve esistere anche quando l'utente effettua l'accesso al database o all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite l'appartenenza a un gruppo di Windows. Si suppongano ad esempio le condizioni seguenti:  
  
-   Il gruppo CompanyDomain\SQLUsers ha accesso al database Sales.  
  
-   CompanyDomain\SqlUser1 è membro del gruppo SQLUsers e pertanto può accedere implicitamente al database Sales.  
  
 Anche se CompanyDomain\SqlUser1 può accedere al database in virtù dell'appartenenza al gruppo SQLUsers, l'istruzione `EXECUTE @string_variable AS USER = 'CompanyDomain\SqlUser1'` avrà esito negativo in quanto `CompanyDomain\SqlUser1` non esiste come entità di sicurezza nel database.  
  
### <a name="best-practices"></a>Procedure consigliate  
 Specificare un account di accesso o un utente che disponga almeno dei privilegi necessari per eseguire le operazioni definite nell'istruzione o nel modulo. Ad esempio, non specificare un nome account di accesso con autorizzazioni a livello di server se sono richieste solo autorizzazioni a livello di database oppure non specificare l'account di un proprietario di database a meno che siano richieste le autorizzazioni corrispondenti.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per eseguire l'istruzione EXECUTE, non è necessario disporre di autorizzazioni specifiche. Sono tuttavia richieste autorizzazioni per le entità a protezione diretta a cui viene fatto riferimento all'interno della stringa EXECUTE. Se, ad esempio, la stringa include un'istruzione INSERT, il chiamante dell'istruzione EXECUTE deve disporre dell'autorizzazione INSERT per la tabella di destinazione. Le autorizzazioni vengono verificate non appena viene rilevata l'istruzione EXECUTE, anche se l'istruzione è inclusa in un modulo.  
  
 Le autorizzazioni per l'istruzione EXECUTE per un modulo vengono assegnate per impostazione predefinita al proprietario del modulo, che può quindi trasferirle ad altri utenti. Quando si esegue un modulo che esegue una stringa, la verifica delle autorizzazioni viene eseguita nel contesto dell'utente che esegue il modulo e non nel contesto dell'utente che l'ha creato. Se, tuttavia, lo stesso utente è proprietario del modulo chiamante e del modulo richiamato, la verifica delle autorizzazioni per l'istruzione EXECUTE non viene eseguita per il secondo modulo.  
  
 Se il modulo accede ad altri oggetti di database, l'esecuzione ha esito positivo se per il modulo si dispone dell'autorizzazione EXECUTE e si verifica una delle condizioni seguenti:  
  
-   Il modulo è contrassegnato come EXECUTE AS USER o SELF e il proprietario del modulo dispone delle autorizzazioni corrispondenti per l'oggetto a cui viene fatto riferimento. Per altre informazioni sulla rappresentazione in un modulo, vedere [Clausola EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md).  
  
-   Il modulo è contrassegnato come EXECUTE AS CALLER e si dispone delle autorizzazioni corrispondenti per l'oggetto.  
  
-   Il modulo è contrassegnato come EXECUTE AS *user_name* e *user_name* ha le autorizzazioni corrispondenti per l'oggetto.  
  
### <a name="context-switching-permissions"></a>Autorizzazioni per il cambio di contesto  
 Per specificare l'istruzione EXECUTE AS per un account di accesso, il chiamante deve disporre delle autorizzazioni IMPERSONATE per il nome account di accesso specificato. Per specificare l'istruzione EXECUTE AS per un utente del database, il chiamante deve disporre delle autorizzazioni IMPERSONATE per il nome utente specificato. Se non si specifica alcun contesto di esecuzione oppure se si specifica EXECUTE AS CALLER, le autorizzazioni IMPERSONATE non sono obbligatorie.  
  
## <a name="examples-sql-server"></a>Esempi: SQL Server
  
### <a name="a-using-execute-to-pass-a-single-parameter"></a>R. Utilizzo dell'istruzione EXECUTE per passare un parametro singolo  
 La stored procedure `uspGetEmployeeManagers` nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] prevede un parametro (`@EmployeeID`). Gli esempi seguenti eseguono la stored procedure `uspGetEmployeeManagers` con `Employee ID 6` come valore di parametro.  
  
```sql    
EXEC dbo.uspGetEmployeeManagers 6;  
GO  
```  
  
 La variabile può essere specificata in modo esplicito durante l'esecuzione.  
  
```sql    
EXEC dbo.uspGetEmployeeManagers @EmployeeID = 6;  
GO  
```  
  
 Se si tratta della prima istruzione in un batch oppure di uno script **osql** o **sqlcmd**, non è necessario specificare EXEC.  
  
```sql    
dbo.uspGetEmployeeManagers 6;  
GO  
--Or  
dbo.uspGetEmployeeManagers @EmployeeID = 6;  
GO  
```  
  
### <a name="b-using-multiple-parameters"></a>B. Utilizzo di più parametri  
 Nell'esempio seguente viene eseguita la stored procedure `spGetWhereUsedProductID` nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Vengono passati due parametri: il primo è un ID prodotto (`819`), il secondo (`@CheckDate,`) è un valore di tipo `datetime`.  
  
```sql    
DECLARE @CheckDate DATETIME;  
SET @CheckDate = GETDATE();  
EXEC dbo.uspGetWhereUsedProductID 819, @CheckDate;  
GO  
```  
  
### <a name="c-using-execute-tsql_string-with-a-variable"></a>C. Utilizzo dell'istruzione EXECUTE 'tsql_string' con una variabile  
 Nell'esempio seguente viene illustrato come l'istruzione `EXECUTE` gestisca stringhe compilate in modo dinamico contenenti variabili. Nell'esempio viene creato il cursore `tables_cursor` che include un elenco di tutte le tabelle definite dall'utente nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], quindi l'elenco viene utilizzato per ricompilare tutti gli indici nella tabella.  
  
```sql    
DECLARE tables_cursor CURSOR  
   FOR  
   SELECT s.name, t.name   
   FROM sys.objects AS t  
   JOIN sys.schemas AS s ON s.schema_id = t.schema_id  
   WHERE t.type = 'U';  
OPEN tables_cursor;  
DECLARE @schemaname sysname;  
DECLARE @tablename sysname;  
FETCH NEXT FROM tables_cursor INTO @schemaname, @tablename;  
WHILE (@@FETCH_STATUS <> -1)  
BEGIN;  
   EXECUTE ('ALTER INDEX ALL ON ' + @schemaname + '.' + @tablename + ' REBUILD;');  
   FETCH NEXT FROM tables_cursor INTO @schemaname, @tablename;  
END;  
PRINT 'The indexes on all tables have been rebuilt.';  
CLOSE tables_cursor;  
DEALLOCATE tables_cursor;  
GO  
  
```  
  
### <a name="d-using-execute-with-a-remote-stored-procedure"></a>D. Utilizzo dell'istruzione EXECUTE con una stored procedure remota  
 Nell'esempio seguente viene eseguita la stored procedure `uspGetEmployeeManagers` nel server remoto `SQLSERVER1` e lo stato restituito, che indica se la stored procedure è stata eseguita correttamente o meno, viene archiviato in `@retstat`.  
  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive
  
```sql    
DECLARE @retstat INT;  
EXECUTE @retstat = SQLSERVER1.AdventureWorks2012.dbo.uspGetEmployeeManagers @BusinessEntityID = 6;  
```  
  
### <a name="e-using-execute-with-a-stored-procedure-variable"></a>E. Utilizzo dell'istruzione EXECUTE con una variabile di stored procedure  
 Nell'esempio seguente viene creata una variabile che rappresenta il nome di una stored procedure.  
  
```sql  
DECLARE @proc_name VARCHAR(30);  
SET @proc_name = 'sys.sp_who';  
EXEC @proc_name;  
  
```  
  
### <a name="f-using-execute-with-default"></a>F. Utilizzo dell'istruzione EXECUTE con la parola chiave DEFAULT  
 Nell'esempio seguente viene creata una stored procedure con valori predefiniti per il primo e il terzo parametro. Quando si esegue la procedura, se nella chiamata non viene passato alcun valore oppure viene specificato il valore predefinito, i valori predefiniti vengono utilizzati per il primo e il terzo parametro. Si notino i vari utilizzi della parola chiave `DEFAULT`.  
  
```sql    
IF OBJECT_ID(N'dbo.ProcTestDefaults', N'P')IS NOT NULL  
   DROP PROCEDURE dbo.ProcTestDefaults;  
GO  
-- Create the stored procedure.  
CREATE PROCEDURE dbo.ProcTestDefaults (  
@p1 SMALLINT = 42,   
@p2 CHAR(1),   
@p3 VARCHAR(8) = 'CAR')  
AS   
   SET NOCOUNT ON;  
   SELECT @p1, @p2, @p3  
;  
GO  
```  
  
 La stored procedure `Proc_Test_Defaults` può essere eseguita in molte combinazioni.  
  
```sql    
-- Specifying a value only for one parameter (@p2).  
EXECUTE dbo.ProcTestDefaults @p2 = 'A';  
-- Specifying a value for the first two parameters.  
EXECUTE dbo.ProcTestDefaults 68, 'B';  
-- Specifying a value for all three parameters.  
EXECUTE dbo.ProcTestDefaults 68, 'C', 'House';  
-- Using the DEFAULT keyword for the first parameter.  
EXECUTE dbo.ProcTestDefaults @p1 = DEFAULT, @p2 = 'D';  
-- Specifying the parameters in an order different from the order defined in the procedure.  
EXECUTE dbo.ProcTestDefaults DEFAULT, @p3 = 'Local', @p2 = 'E';  
-- Using the DEFAULT keyword for the first and third parameters.  
EXECUTE dbo.ProcTestDefaults DEFAULT, 'H', DEFAULT;  
EXECUTE dbo.ProcTestDefaults DEFAULT, 'I', @p3 = DEFAULT;  
  
```  
  
### <a name="g-using-execute-with-at-linked_server_name"></a>G. Utilizzo dell'istruzione EXECUTE con il parametro AT linked_server_name  
 Nell'esempio seguente una stringa di comando viene passata a un server remoto. Verrà creato un server collegato `SeattleSales` che punta a un'altra istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ed esegue un'istruzione DDL (`CREATE TABLE`) in tale server collegato.  
  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive
  
```sql    
EXEC sp_addlinkedserver 'SeattleSales', 'SQL Server'  
GO  
EXECUTE ( 'CREATE TABLE AdventureWorks2012.dbo.SalesTbl   
(SalesID int, SalesName varchar(10)) ; ' ) AT SeattleSales;  
GO  
```  
  
### <a name="h-using-execute-with-recompile"></a>H. Utilizzo dell'istruzione EXECUTE WITH RECOMPILE  
 L'esempio seguente esegue la stored procedure `Proc_Test_Defaults` e impone la compilazione, l'uso e l'eliminazione di un nuovo piano di query dopo l'esecuzione del modulo.  
  
```sql    
EXECUTE dbo.Proc_Test_Defaults @p2 = 'A' WITH RECOMPILE;  
GO  
```  
  
### <a name="i-using-execute-with-a-user-defined-function"></a>I. Utilizzo dell'istruzione EXECUTE con una funzione definita dall'utente  
 Nell'esempio seguente viene eseguita la funzione scalare definita dall'utente `ufnGetSalesOrderStatusText` nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Viene utilizzata la variabile `@returnstatus` per archiviare il valore restituito dalla funzione. Per la funzione è previsto un parametro di input (`@Status`) Questo è definito con il tipo di dati **tinyint**.  
  
```sql    
DECLARE @returnstatus NVARCHAR(15);  
SET @returnstatus = NULL;  
EXEC @returnstatus = dbo.ufnGetSalesOrderStatusText @Status = 2;  
PRINT @returnstatus;  
GO  
```  
  
### <a name="j-using-execute-to-query-an-oracle-database-on-a-linked-server"></a>J. Utilizzo dell'istruzione EXECUTE per eseguire query su un database Oracle in un server collegato  
 Nell'esempio seguente vengono eseguite più istruzioni `SELECT` nel server Oracle remoto. Viene innanzitutto aggiunto il server Oracle come server collegato e quindi viene creato l'account di accesso per il server collegato.  
  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive
  
```sql    
-- Setup the linked server.  
EXEC sp_addlinkedserver    
        @server='ORACLE',  
        @srvproduct='Oracle',  
        @provider='OraOLEDB.Oracle',   
        @datasrc='ORACLE10';  
  
EXEC sp_addlinkedsrvlogin   
    @rmtsrvname='ORACLE',  
    @useself='false',   
    @locallogin=null,   
    @rmtuser='scott',   
    @rmtpassword='tiger';  
  
EXEC sp_serveroption 'ORACLE', 'rpc out', true;  
GO  
  
-- Execute several statements on the linked Oracle server.  
EXEC ( 'SELECT * FROM scott.emp') AT ORACLE;  
GO  
EXEC ( 'SELECT * FROM scott.emp WHERE MGR = ?', 7902) AT ORACLE;  
GO  
DECLARE @v INT;   
SET @v = 7902;  
EXEC ( 'SELECT * FROM scott.emp WHERE MGR = ?', @v) AT ORACLE;  
GO   
```  
  
### <a name="k-using-execute-as-user-to-switch-context-to-another-user"></a>K. Utilizzo dell'istruzione EXECUTE AS USER per cambiare contesto a un altro utente  
 Nell'esempio seguente viene eseguita una stringa [!INCLUDE[tsql](../../includes/tsql-md.md)] che crea una tabella e viene quindi specificata la clausola `AS USER` per cambiare il contesto di esecuzione dell'istruzione dal chiamante a `User1`. Il [!INCLUDE[ssDE](../../includes/ssde-md.md)] controllerà le autorizzazioni di `User1` quando viene eseguita l'istruzione. `User1` deve esistere come utente nel database e deve disporre delle autorizzazioni necessarie per creare tabelle nello schema `Sales`. In caso contrario, l'istruzione avrà esito negativo.  
  
```sql    
EXECUTE ('CREATE TABLE Sales.SalesTable (SalesID INT, SalesName VARCHAR(10));')  
AS USER = 'User1';  
GO  
```  
  
### <a name="l-using-a-parameter-with-execute-and-at-linked_server_name"></a>L. Utilizzo di un parametro con EXECUTE e AT linked_server_name  
 Nell'esempio seguente una stringa di comando viene passata a un server remoto utilizzando un punto interrogativo (`?`) come segnaposto per un parametro. Viene quindi creato un server collegato `SeattleSales` che punta a un'altra istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e viene eseguita un'istruzione `SELECT` applicata a tale server collegato. L'istruzione `SELECT` utilizza il punto interrogativo come segnaposto per il parametro `ProductID` (`952`), specificato dopo l'istruzione.  
  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive
  
```sql    
-- Setup the linked server.  
EXEC sp_addlinkedserver 'SeattleSales', 'SQL Server'  
GO  
-- Execute the SELECT statement.  
EXECUTE ('SELECT ProductID, Name   
    FROM AdventureWorks2012.Production.Product  
    WHERE ProductID = ? ', 952) AT SeattleSales;  
GO  
```  
  
### <a name="m-using-execute-to-redefine-a-single-result-set"></a>M. Utilizzo di EXECUTE per ridefinire un singolo set di risultati  
 In alcuni degli esempi precedenti è stato eseguito `EXEC dbo.uspGetEmployeeManagers 6;` che ha restituito 7 colonne. Nell'esempio seguente viene illustrato l'utilizzo della sintassi `WITH RESULT SET` per modificare i nomi e i tipi di dati del set di risultati ottenuto.  
  
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
```sql    
EXEC uspGetEmployeeManagers 16  
WITH RESULT SETS  
(   
   ([Reporting Level] INT NOT NULL,  
    [ID of Employee] INT NOT NULL,  
    [Employee First Name] NVARCHAR(50) NOT NULL,  
    [Employee Last Name] NVARCHAR(50) NOT NULL,  
    [Employee ID of Manager] NVARCHAR(max) NOT NULL,  
    [Manager First Name] NVARCHAR(50) NOT NULL,  
    [Manager Last Name] NVARCHAR(50) NOT NULL )  
);  
  
```  
  
### <a name="n-using-execute-to-redefine-a-two-result-sets"></a>N. Utilizzo di EXECUTE per ridefinire due set di risultati  
 Quando si esegue un'istruzione che restituisce più di un set di risultati, definire ogni set di risultati previsto. Nell'esempio seguente in [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] viene creata una stored procedure che restituisce due set di risultati. La procedura viene quindi eseguita tramite la clausola **WITH RESULT SETS** e specificando due definizioni di set di risultati.  
  
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
```sql    
--Create the procedure  
CREATE PROC Production.ProductList @ProdName NVARCHAR(50)  
AS  
-- First result set  
SELECT ProductID, Name, ListPrice  
    FROM Production.Product  
    WHERE Name LIKE @ProdName;  
-- Second result set   
SELECT Name, COUNT(S.ProductID) AS NumberOfOrders  
    FROM Production.Product AS P  
    JOIN Sales.SalesOrderDetail AS S  
        ON P.ProductID  = S.ProductID   
    WHERE Name LIKE @ProdName  
    GROUP BY Name;  
GO  
  
-- Execute the procedure   
EXEC Production.ProductList '%tire%'  
WITH RESULT SETS   
(  
    (ProductID INT,   -- first result set definition starts here  
    Name NAME,  
    ListPrice MONEY)  
    ,                 -- comma separates result set definitions  
    (Name NAME,       -- second result set definition starts here  
    NumberOfOrders INT)  
);  
  
```  
  ### <a name="o-using-execute-with-at-data_source-data_source_name-to-query-a-remote-sql-server"></a>O. Uso di EXECUTE con AT DATA_SOURCE data_source_name per eseguire una query su SQL Server remoto 
  
 Nell'esempio seguente una stringa di comando viene passata a un'origine dati esterna che punta a un'istanza di SQL Server. 
  
**Si applica a**: [!INCLUDE[sssql19](../../includes/sssql19-md.md)] e versioni successive
  
```sql    
EXECUTE ( 'SELECT @@SERVERNAME' ) AT DATA_SOURCE my_sql_server;  
GO  
```  
  
### <a name="p-using-execute-with-at-data_source-data_source_name-to-query-compute-pool-in-sql-server-big-data-cluster"></a>P. Uso di EXECUTE con AT DATA_SOURCE data_source_name per eseguire una query su un pool di calcolo nel cluster Big Data di SQL Server 

 Nell'esempio seguente una stringa di comando viene passata a un'origine dati esterna che punta a un pool di calcolo nel cluster Big Data di SQL Server. Nell'esempio viene creata un'origine dati `SqlComputePool` in un pool di calcolo nel cluster Big Data di SQL Server e viene eseguita un'istruzione `SELECT` sull'origine dati. 
  
**Si applica a**: [!INCLUDE[sssql19](../../includes/sssql19-md.md)] e versioni successive
  
```sql  
CREATE EXTERNAL DATA SOURCE SqlComputePool 
WITH (LOCATION = 'sqlcomputepool://controller-svc/default');
EXECUTE ( 'SELECT @@SERVERNAME' ) AT DATA_SOURCE SqlComputePool;  
GO  
```  

### <a name="q-using-execute-with-at-data_source-data_source_name-to-query-data-pool-in-sql-server-big-data-cluster"></a>Q. Uso di EXECUTE con AT DATA_SOURCE data_source_name per eseguire una query su un pool di dati nel cluster Big Data di SQL Server 
 Nell'esempio seguente una stringa di comando viene passata a un'origine dati esterna che punta a un pool di calcolo nel cluster Big Data di SQL Server. Nell'esempio viene creata un'origine dati `SqlDataPool` in un pool di dati nel cluster Big Data di SQL Server e viene eseguita un'istruzione `SELECT` sull'origine dati. 
  
**Si applica a**: [!INCLUDE[sssql19](../../includes/sssql19-md.md)] e versioni successive
  
```sql  
CREATE EXTERNAL DATA SOURCE SqlDataPool 
WITH (LOCATION = 'sqldatapool://controller-svc/default');
EXECUTE ( 'SELECT @@SERVERNAME' ) AT DATA_SOURCE SqlDataPool;  
GO  
```

### <a name="r-using-execute-with-at-data_source-data_source_name-to-query-storage-pool-in-sql-server-big-data-cluster"></a>R. Uso di EXECUTE con AT DATA_SOURCE data_source_name per eseguire una query su un pool di archiviazione nel cluster Big Data di SQL Server 

 Nell'esempio seguente una stringa di comando viene passata a un'origine dati esterna che punta a un pool di calcolo nel cluster Big Data di SQL Server. Nell'esempio viene creata un'origine dati `SqlStoragePool` in un pool di dati nel cluster Big Data di SQL Server e viene eseguita un'istruzione `SELECT` sull'origine dati. 
  
**Si applica a**: [!INCLUDE[sssql19](../../includes/sssql19-md.md)] e versioni successive
  
```sql  
CREATE EXTERNAL DATA SOURCE SqlStoragePool
WITH (LOCATION = 'sqlhdfs://controller-svc/default');
EXECUTE ( 'SELECT @@SERVERNAME' ) AT DATA_SOURCE SqlStoragePool;  
GO  
```

  
## <a name="examples-azure-synapse-analytics"></a>Esempi: Azure Synapse Analytics 
  
### <a name="a-basic-procedure-execution"></a>A: Esecuzione di una routine di base  
 Esecuzione di una stored procedure:  
  
```sql  
EXEC proc1;  
```  
  
 Chiamata di una stored procedure con nome stabilito in runtime:  
  
```sql    
EXEC ('EXEC ' + @var);  
```  
  
 Chiamata di una stored procedure dall'interno di una stored procedure:  
  
```sql   
CREATE sp_first AS EXEC sp_second; EXEC sp_third;  
```  
  
### <a name="b-executing-strings"></a>B: Esecuzione di stringhe  
 Esecuzione di una stringa SQL:  
  
```sql   
EXEC ('SELECT * FROM sys.types');  
```  
  
 Esecuzione di una stringa annidata:  
  
```sql  
EXEC ('EXEC (''SELECT * FROM sys.types'')');  
```  
  
 Esecuzione di una variabile stringa:  
  
```sql  
DECLARE @stringVar NVARCHAR(100);  
SET @stringVar = N'SELECT name FROM' + ' sys.sql_logins';  
EXEC (@stringVar);  
```  
  
### <a name="c-procedures-with-parameters"></a>C: Routine con parametri  

 L'esempio seguente crea una procedura con parametri e illustra tre modalità per l'esecuzione di questa:  
  
```sql  
-- Uses AdventureWorks  
  
CREATE PROC ProcWithParameters  
    @name NVARCHAR(50),  
@color NVARCHAR(15)  
AS   
SELECT ProductKey, EnglishProductName, Color FROM [dbo].[DimProduct]  
WHERE EnglishProductName LIKE @name  
AND Color = @color;  
GO  
  
-- Executing using positional parameters  
EXEC ProcWithParameters N'%arm%', N'Black';  
-- Executing using named parameters in order  
EXEC ProcWithParameters @name = N'%arm%', @color = N'Black';  
-- Executing using named parameters out of order  
EXEC ProcWithParameters @color = N'Black', @name = N'%arm%';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [@@NESTLEVEL &#40;Transact-SQL&#41;](../../t-sql/functions/nestlevel-transact-sql.md)   
 [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [Clausola EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md)   
 [Utilità osql](../../tools/osql-utility.md)   
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [REVERT &#40;Transact-SQL&#41;](../../t-sql/statements/revert-transact-sql.md)   
 [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)   
 [Utilità sqlcmd](../../tools/sqlcmd-utility.md)   
 [SUSER_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/suser-name-transact-sql.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [USER_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/user-name-transact-sql.md)   
 [OPENDATASOURCE &#40;Transact-SQL&#41;](../../t-sql/functions/opendatasource-transact-sql.md)   
 [Funzioni scalari definite dall'utente per OLTP in memoria](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md)  
  
  
