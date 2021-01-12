---
description: ALTER FUNCTION (Transact-SQL)
title: ALTER FUNCTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/07/2017
ms.prod: sql
ms.prod_service: database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_FUNCTION_TSQL
- ALTER FUNCTION
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER FUNCTION statement
- modifying functions
- functions [SQL Server], modifying
ms.assetid: 89f066ee-05ac-4439-ab04-d8c3d5911179
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1785a65f08952660bd3a8e3cad72355d8db5b373
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98081949"
---
# <a name="alter-function-transact-sql"></a>ALTER FUNCTION (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-pdw-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-pdw-md.md)]

  Modifica una funzione [!INCLUDE[tsql](../../includes/tsql-md.md)] o CLR esistente, creata in precedenza tramite l'istruzione CREATE FUNCTION, senza modificare le autorizzazioni o alterare eventuali trigger, stored procedure o funzioni dipendenti.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Transact-SQL Scalar Function Syntax    
ALTER FUNCTION [ schema_name. ] function_name   
( [ { @parameter_name [ AS ][ type_schema_name. ] parameter_data_type   
    [ = default ] }   
    [ ,...n ]  
  ]  
)  
RETURNS return_data_type  
    [ WITH <function_option> [ ,...n ] ]  
    [ AS ]  
    BEGIN   
        function_body   
        RETURN scalar_expression  
    END  
[ ; ]
```  

```syntaxsql
-- Transact-SQL Inline Table-Valued Function Syntax
ALTER FUNCTION [ schema_name. ] function_name   
( [ { @parameter_name [ AS ] [ type_schema_name. ] parameter_data_type   
    [ = default ] }   
    [ ,...n ]  
  ]  
)  
RETURNS TABLE  
    [ WITH <function_option> [ ,...n ] ]  
    [ AS ]  
    RETURN [ ( ] select_stmt [ ) ]  
[ ; ]  
```
  
```syntaxsql
-- Transact-SQL Multistatement Table-valued Function Syntax
ALTER FUNCTION [ schema_name. ] function_name   
( [ { @parameter_name [ AS ] [ type_schema_name. ] parameter_data_type   
    [ = default ] }   
    [ ,...n ]  
  ]  
)  
RETURNS @return_variable TABLE <table_type_definition>  
    [ WITH <function_option> [ ,...n ] ]  
    [ AS ]  
    BEGIN   
        function_body   
        RETURN  
    END  
[ ; ]  
```

```syntaxsql
-- Transact-SQL Function Clauses   
<function_option>::=   
{  
    [ ENCRYPTION ]  
  | [ SCHEMABINDING ]  
  | [ RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT ]  
  | [ EXECUTE_AS_Clause ]  
} 

<table_type_definition>:: =   
( { <column_definition> <column_constraint>   
  | <computed_column_definition> }   
    [ <table_constraint> ] [ ,...n ]  
)   
<column_definition>::=  
{  
    { column_name data_type }  
    [ [ DEFAULT constant_expression ]   
      [ COLLATE collation_name ] | [ ROWGUIDCOL ]  
    ]  
    | [ IDENTITY [ (seed , increment ) ] ]  
    [ <column_constraint> [ ...n ] ]   
}  

<column_constraint>::=   
{  
    [ NULL | NOT NULL ]   
    { PRIMARY KEY | UNIQUE }  
      [ CLUSTERED | NONCLUSTERED ]   
        [ WITH FILLFACTOR = fillfactor   
        | WITH ( < index_option > [ , ...n ] )  
      [ ON { filegroup | "default" } ]  
  | [ CHECK ( logical_expression ) ] [ ,...n ]  
}  
  
<computed_column_definition>::=  
column_name AS computed_column_expression   
  
<table_constraint>::=  
{   
    { PRIMARY KEY | UNIQUE }  
      [ CLUSTERED | NONCLUSTERED ]   
      ( column_name [ ASC | DESC ] [ ,...n ] )  
        [ WITH FILLFACTOR = fillfactor   
        | WITH ( <index_option> [ , ...n ] )  
  | [ CHECK ( logical_expression ) ] [ ,...n ]  
}  
  
<index_option>::=  
{   
    PAD_INDEX = { ON | OFF }   
  | FILLFACTOR = fillfactor   
  | IGNORE_DUP_KEY = { ON | OFF }  
  | STATISTICS_NORECOMPUTE = { ON | OFF }   
  | ALLOW_ROW_LOCKS = { ON | OFF }  
  | ALLOW_PAGE_LOCKS ={ ON | OFF }   
}  
```
  
```syntaxsql
-- CLR Scalar and Table-Valued Function Syntax
ALTER FUNCTION [ schema_name. ] function_name   
( { @parameter_name [AS] [ type_schema_name. ] parameter_data_type   
    [ = default ] }   
    [ ,...n ]  
)  
RETURNS { return_data_type | TABLE <clr_table_type_definition> }  
    [ WITH <clr_function_option> [ ,...n ] ]  
    [ AS ] EXTERNAL NAME <method_specifier>  
[ ; ]  
```
  
```syntaxsql
-- CLR Function Clauses
<method_specifier>::=  
    assembly_name.class_name.method_name  
 
  
<clr_function_option>::=  
}  
    [ RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT ]  
  | [ EXECUTE_AS_Clause ]  
}  
  
<clr_table_type_definition>::=   
( { column_name data_type } [ ,...n ] )  
  
```  
  
```syntaxsql
-- Syntax for In-Memory OLTP: Natively compiled, scalar user-defined function  
ALTER FUNCTION [ schema_name. ] function_name    
 ( [ { @parameter_name [ AS ][ type_schema_name. ] parameter_data_type   
    [ NULL | NOT NULL ] [ = default ] }   
    [ ,...n ]   
  ]   
)   
RETURNS return_data_type  
    [ WITH <function_option> [ ,...n ] ]   
    [ AS ]   
    BEGIN ATOMIC WITH (set_option [ ,... n ])  
        function_body   
        RETURN scalar_expression  
    END  
    
<function_option>::=   
{ |  NATIVE_COMPILATION   
  |  SCHEMABINDING   
  | [ EXECUTE_AS_Clause ]   
  | [ RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT ]   
}  
```   
  
## <a name="arguments"></a>Argomenti  
 *schema_name*  
 Nome dello schema a cui appartiene la funzione definita dall'utente.  
  
 *function_name*  
 Funzione definita dall'utente che si desidera modificare.  
  
> [!NOTE]  
>  È necessario apporre le parentesi dopo il nome della funzione anche se non viene specificato alcun parametro.  
  
 **@** _parameter_name_  
 Parametro della funzione definita dall'utente. È possibile dichiarare uno o più parametri.  
  
 Una funzione può avere al massimo 2.100 parametri. Il valore di ciascun parametro dichiarato deve essere specificato dall'utente quando viene eseguita la funzione, a meno che non venga definito un valore predefinito per tale parametro.  
  
 Specificare un nome di parametro usando come primo carattere il simbolo di chiocciola ( **@** ). Il nome di parametro deve essere conforme alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md). I parametri sono locali rispetto alla funzione. È pertanto possibile utilizzare gli stessi nomi di parametro in altre funzioni. I parametri possono rappresentare solo costanti, non nomi di tabella, di colonna o di altri oggetti di database.  
  
> [!NOTE]  
>  ANSI_WARNINGS non viene applicata quando vengono trasmessi parametri in una stored procedure o in una funzione definita dall'utente oppure in caso di dichiarazione e impostazione delle variabili in un'istruzione batch. Se, ad esempio, la variabile viene definita come **char(3)** e quindi impostata su un valore maggiore di tre caratteri, i dati verranno troncati alla dimensione definita e l'istruzione INSERT o UPDATE avrà esito positivo.  
  
 [ *type_schema_name.* ] *parameter_data_type*  
 Tipo di dati del parametro e, facoltativamente, lo schema a cui appartiene. Per le funzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] sono consentiti tutti i tipi di dati, compresi i tipi CLR definiti dall'utente, eccetto il tipo di dati **timestamp**. Per le funzioni CLR, sono consentiti tutti i tipi di dati, compresi i tipi CLR definiti dall'utente, eccetto i tipi di dati **text**, **ntext**, **image** e **timestamp**. Non è possibile specificare i tipi non scalari **cursor** e **table**, come tipo di dati dei parametri nelle funzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] o CLR.  
  
 Se *type_schema_name* non viene specificato, il [!INCLUDE[ssDEversion2005](../../includes/ssdeversion2005-md.md)] cerca *scalar_parameter_data_type* nell'ordine seguente:  
  
-   Schema contenente i nomi dei tipi di dati di sistema di SQL Server.  
  
-   Schema predefinito dell'utente corrente nel database corrente.  
  
-   Schema **dbo** nel database corrente.  
  
 [ **=** _default_ ]  
 Valore predefinito del parametro. Se viene definito un valore *default*, è possibile eseguire la funzione senza specificare un valore per il parametro corrispondente a tale valore.  
  
> [!NOTE]  
>  È possibile specificare parametri predefiniti per le funzioni CLR, ad eccezione dei tipi di dati **varchar(max)** e **varbinary(max)** .  
  
 Se a un parametro della funzione è associato un valore predefinito, quando si richiama la funzione per ottenere il valore predefinito è necessario specificare la parola chiave "default". Questo comportamento risulta diverso dall'utilizzo di parametri con valore predefinito nelle stored procedure in cui l'omissione del parametro implica l'utilizzo del valore predefinito.  
  
 *return_data_type*  
 Valore restituito di una funzione scalare definita dall'utente. Per le funzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] sono consentiti tutti i tipi di dati, compresi i tipi CLR definiti dall'utente, eccetto il tipo di dati **timestamp**. Per le funzioni CLR, sono consentiti tutti i tipi di dati, compresi i tipi CLR definiti dall'utente, eccetto i tipi di dati **text**, **ntext**, **image** e **timestamp**. Non è possibile specificare un tipo non scalare,**cursor** o **table**, come tipo di dati restiuito nelle funzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] o CLR.  
  
 *function_body*  
 Specifica che una serie di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)], che nel loro complesso non producono alcun effetto collaterale, ad esempio la modifica di una tabella, definisce il valore della funzione. *function_body* viene usato solo in funzioni scalari e funzioni composte da più istruzioni con valori di tabella.  
  
 Nelle funzioni scalari *function_body* corrisponde a una serie di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] che in combinazione restituiscono un valore scalare.  
  
 Nelle funzioni composte da più istruzioni con valori di tabella *function_body* corrisponde a una serie di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] che popolano una variabile restituita TABLE.  
  
 *scalar_expression*  
 Specifica che la funzione scalare restituisce un valore scalare.  
  
 TABLE  
 Specifica che il valore restituito della funzione con valori di tabella è una tabella. Alle funzioni con valori di tabella è possibile passare solo costanti e **@** _local\_variables_.  
  
 Nelle funzioni inline con valori di tabella, il valore restituito TABLE viene definito tramite una sola istruzione SELECT. Alle funzioni inline non sono associate variabili restituite.  
  
 Nelle funzioni con valori di tabella composte da più istruzioni **@** _return\_variable_ è una variabile TABLE usata per l'archiviazione e l'accumulo delle righe da restituire come valore della funzione. È possibile specificare **@** _return\_variable_ solo per le funzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] e non per le funzioni CLR.  
  
 *select-stmt*  
 Istruzione SELECT che definisce il valore restituito di una funzione inline con valori di tabella.  
  
 EXTERNAL NAME \<method_specifier>*assembly_name.class_name*.*method_name*  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 Specifica il metodo di un assembly da associare alla funzione. *assembly_name* deve corrispondere a un assembly esistente in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel database corrente con visibilità attivata. *class_name* deve essere un identificatore [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valido e deve esistere come classe nell'assembly. Se alla classe è stato assegnato un nome qualificato dallo spazio dei nomi le cui parti sono separate da un punto ( **.** ), il nome della classe deve essere delimitato tramite parentesi quadre ( **[]** ) o virgolette ( **""** ). *method_name* deve essere un identificatore [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valido e deve esistere come metodo statico nella classe specificata.  
  
> [!NOTE]  
>  Per impostazione predefinita, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non può eseguire il codice CLR. È possibile creare, modificare ed eliminare gli oggetti di database che fanno riferimento a moduli CLR; tuttavia non è possibile eseguire questi riferimenti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] finché non viene abilitata l'opzione [clr enabled option](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md). Per abilitare questa opzione, usare [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
> [!NOTE]  
>  Questa opzione non è disponibile in un database indipendente.  
  
 _\<_table\_type\_definition_\>_ **(** { \<column_definition\> \<column\_constraint\> | \<computed\_column\_definition\> } [ \<table\_constraint\> ] [ **,** ...*n* ] **)**  
 Definisce il tipo di dati della tabella per una funzione [!INCLUDE[tsql](../../includes/tsql-md.md)]. La dichiarazione di tabella include definizioni di colonna, nonché vincoli di colonna o tabella.  
  
\< clr_table_type_definition \> **(** { *column_name**data_type* } [ **,** ...*n* ] **)** **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] ([in anteprima in alcune aree geografiche](/azure/azure-sql/database/features-comparison?WT.mc_id=TSQL_GetItTag)).  
  
 Definisce i tipi di dati della tabella per una funzione CLR. La dichiarazione di tabella include solo nomi di colonna e tipi di dati.  
  
 NULL|NOT NULL  
 Supportato solo per funzioni definite dall'utente scalari compilate in modo nativo. Per altre informazioni, vedere [Funzioni scalari definite dall'utente per OLTP in memoria](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md).  
  
 NATIVE_COMPILATION  
 Indica se una funzione definita dall'utente è compilata in modo nativo. Questo argomento è obbligatorio per funzioni definite dall'utente scalari compilate in modo nativo.  
  
 L'argomento NATIVE_COMPILATION è obbligatorio quando si applica ALTER alla funzione e può essere usato solo se la funzione è stata creata con l'argomento NATIVE_COMPILATION.  
  
 BEGIN ATOMIC WITH  
 Supportato solo per funzioni definite dall'utente scalari compilate in modo nativo. Obbligatorio. Per altre informazioni, vedere [Atomic Blocks](../../relational-databases/in-memory-oltp/atomic-blocks-in-native-procedures.md).  
  
 SCHEMABINDING  
 L'argomento SCHEMABINDING è obbligatorio per funzioni definite dall'utente scalari compilate in modo nativo.  
  
 **\<function_option>::= e \<clr_function_option>::=**  
  
 Specifica che la funzione avrà una o più opzioni seguenti.  
  
 ENCRYPTION  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 Indica che nel [!INCLUDE[ssDE](../../includes/ssde-md.md)] vengono crittografate le colonne della vista del catalogo contenenti il testo dell'istruzione ALTER FUNCTION. Tramite il parametro ENCRYPTION è possibile evitare la pubblicazione della funzione come parte della replica di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Non è possibile specificare l'opzione ENCRYPTION per le funzioni CLR.  
  
 SCHEMABINDING  
 Specifica che la funzione è associata agli oggetti di database a cui fa riferimento. Quando la clausola SCHEMABINDING viene specificata, non è possibile apportare agli oggetti di base modifiche che hanno effetto sulla definizione della funzione. È necessario prima modificare o eliminare la definizione della funzione per rimuovere le dipendenze dall'oggetto da modificare.  
  
 L'associazione della funzione agli oggetti cui fa riferimento viene rimossa solo quando viene eseguita una delle azioni seguenti:  
  
-   La funzione viene eliminata.  
  
-   La funzione viene modificata tramite l'istruzione ALTER senza specificare l'opzione SCHEMABINDING.  
  
Per un elenco delle condizioni che devono essere soddisfatte per consentire l'associazione di una funzione a uno schema, vedere [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md).  
  
 RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT  
 Specifica l'attributo **OnNULLCall** di una funzione a valori scalari. Se omesso, viene utilizzata l'opzione CALLED ON NULL INPUT per impostazione predefinita. Ciò significa che viene eseguito il corpo della funzione anche se come argomento viene passato NULL.  
  
 Se in una funzione CLR si specifica l'opzione RETURNS NULL ON NULL INPUT, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce NULL se uno qualsiasi degli argomenti ricevuti è NULL senza effettivamente richiamare il corpo della funzione. Se per il metodo specificato in \<method_specifier> è già stato definito un attributo personalizzato impostato su RETURNS NULL ON NULL INPUT, ma l'istruzione ALTER FUNCTION include CALLED ON NULL INPUT, l'istruzione ALTER FUNCTION risulta prioritaria. Non è possibile specificare l'attributo **OnNULLCall** per le funzioni CLR con valori di tabella.  
  
 Clausola EXECUTE AS  
 Specifica il contesto di sicurezza nel quale viene eseguita la funzione definita dall'utente. Sarà pertanto possibile controllare l'account utente usato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per convalidare le autorizzazioni per qualsiasi oggetto di database a cui la funzione fa riferimento.  
  
> [!NOTE]  
>  Non è possibile specificare la clausola EXECUTE AS per le funzioni inline definite dall'utente.  
  
 Per altre informazioni, vedere [Clausola EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md).  
  
**\< column_definition >::=**
  
 Definisce il tipo di dati della tabella. La dichiarazione di tabella include definizioni di colonna e vincoli. Per le funzioni CLR, è possibile specificare solo *column_name* e *data_type*.  
  
 *column_name*  
 Nome di una colonna della tabella. I nomi delle colonne devono essere conformi alle regole per gli identificatori e devono essere univoci nella tabella. *column_name* può essere costituito da un numero di caratteri compreso tra 1 e 128.  
  
 *data_type*  
 Specifica il tipo di dati della colonna. Per le funzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] sono consentiti tutti i tipi di dati, compresi i tipi CLR definiti dall'utente, eccetto il tipo di dati **timestamp**. Per le funzioni CLR sono consentiti tutti i tipi di dati, compresi i tipi CLR definiti dall'utente, eccetto i tipi di dati **text**, **ntext**, **image**, **char**, **varchar**, **varchar(max)** e **timestamp**. Non è possibile specificare il tipo non scalare **cursor** come tipo di dati di colonna nelle funzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] o CLR.  
  
 DEFAULT *constant_expression*  
 Specifica il valore assegnato alla colonna quando non viene specificato un valore in modo esplicito durante un inserimento. *constant_expression* è una costante, un valore NULL o il valore di una funzione di sistema. Le definizioni dell'opzione DEFAULT possono essere applicate a qualsiasi colonna eccetto quelle che includono la proprietà IDENTITY. Non è possibile specificare l'opzione DEFAULT per le funzioni CLR con valori di tabella.  
  
 COLLATE *collation_name*  
 Specifica le regole di confronto per la colonna. Se viene omesso, alla colonna vengono assegnate le regole di confronto predefinite del database. È possibile usare nomi di regole di confronto di Windows o SQL. Per un elenco e altre informazioni, vedere [Windows_collation_name & #40; Transact-SQL & #41; ](../../t-sql/statements/windows-collation-name-transact-sql.md) e [Nome delle regole di confronto di SQL Server &#40;Transact-SQL&#41;](../../t-sql/statements/sql-server-collation-name-transact-sql.md).  
  
 La clausola COLLATE consente di modificare le regole di confronto solo per le colonne con tipo di dati **char**, **varchar**, **nchar** e **nvarchar**.  
  
 Non è possibile specificare l'opzione COLLATE per le funzioni CLR con valori di tabella.  
  
 ROWGUIDCOL  
 Specifica che la nuova colonna funge da identificatore di riga univoco globale. È possibile designare come colonna ROWGUIDCOL una sola colonna di tipo **uniqueidentifier** per ogni tabella. La proprietà ROWGUIDCOL può essere assegnata solo a una colonna **uniqueidentifier**.  
  
 La proprietà ROWGUIDCOL non impone l'univocità dei valori archiviati nella colonna e non genera automaticamente valori per le nuove righe inserite nella tabella. Per generare valori univoci per ogni colonna, utilizzare la funzione NEWID nelle istruzioni INSERT. È possibile specificare un valore predefinito. Non è tuttavia possibile specificare l'opzione NEWID come valore predefinito.  
  
 IDENTITY  
 Indica che la nuova colonna è una colonna Identity. Quando si aggiunge una nuova riga alla tabella, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] assegna alla colonna un valore univoco e incrementale. Le colonne Identity vengono comunemente utilizzate in combinazione con vincoli PRIMARY KEY e svolgono la funzione di identificatore di riga univoco per la tabella. La proprietà IDENTITY può essere assegnata a colonne **tinyint**, **smallint**, **int**, **bigint**, **decimal(p,0)** o **numeric(p,0)** . Ogni tabella può includere una sola colonna Identity. Non è consentito associare valori predefiniti e vincoli DEFAULT alle colonne Identity. È necessario specificare sia il valore di *seed* che di *increment* oppure è possibile omettere entrambi questi valori. In questo secondo caso, il valore predefinito è (1,1).  
  
 Non è possibile specificare l'opzione IDENTITY per le funzioni CLR con valori di tabella.  
  
 *seed*  
 Valore intero da assegnare alla prima riga della tabella.  
  
 *increment*  
 Valore intero da aggiungere al valore *seed* per le righe successive della tabella.  
  
**\< column_constraint >::= e \< table_constraint>::=**
  
 Definisce il vincolo per una colonna o tabella specificata. Per le funzioni CLR l'unico tipo di vincolo consentito è NULL. I vincoli denominati non sono consentiti.  
  
 NULL | NOT NULL  
 Determina se i valori Null sono supportati nella colonna. L'opzione NULL non è esattamente un vincolo, ma può essere specificata allo stesso modo di NOT NULL. Non è possibile specificare l'opzione NOT NULL per le funzioni CLR con valori di tabella.  
  
 PRIMARY KEY  
 Vincolo che impone l'integrità di entità per una colonna specificata tramite un indice univoco. Nelle funzioni definite dall'utente con valori di tabella, il vincolo PRIMARY KEY può essere creato solo su una colonna per tabella. Non è possibile specificare il vincolo PRIMARY KEY per le funzioni CLR con valori di tabella.  
  
 UNIQUE  
 Vincolo che impone l'integrità di entità per una o più colonne specificate tramite un indice univoco. Una tabella può includere più vincoli UNIQUE. Non è possibile specificare il vincolo UNIQUE per le funzioni CLR con valori di tabella.  
  
 CLUSTERED | NONCLUSTERED  
 Definisce la creazione di un indice cluster o non cluster per il vincolo PRIMARY KEY o UNIQUE. I vincoli PRIMARY KEY usano l'opzione CLUSTERED, mentre i vincoli UNIQUE usano l'opzione NONCLUSTERED.  
  
 L'opzione CLUSTERED può essere specificata solo per un vincolo. Se per un vincolo UNIQUE si specifica CLUSTERED e viene specificato anche un vincolo PRIMARY KEY, quest'ultimo usano l'opzione NONCLUSTERED.  
  
 Non è possibile specificare le opzioni CLUSTERED e NON CLUSTERED per le funzioni CLR con valori di tabella.  
  
 CHECK  
 Vincolo che impone l'integrità di dominio tramite la limitazione dei valori che è possibile inserire in una o più colonne. Non è possibile specificare i vincoli CHECK per le funzioni CLR con valori di tabella.  
  
 *logical_expression*  
 Espressione logica che restituisce TRUE o FALSE.  
  
 **\<computed_column_definition>::=**  
  
 Specifica una colonna calcolata. Per altre informazioni sulle colonne calcolate, vedere [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).  
  
 *column_name*  
 Nome della colonna calcolata.  
  
 *computed_column_expression*  
 Espressione che definisce il valore di una colonna calcolata.  
  
 **\<index_option>::=**  
  
 Specifica le opzioni per l'indice PRIMARY KEY o UNIQUE. Per altre informazioni sulle opzioni per gli indici, vedere [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
 PAD_INDEX = { ON | OFF }  
 Specifica il riempimento dell'indice. Il valore predefinito è OFF.  
  
 FILLFACTOR = *fillfactor*  
 Specifica una percentuale indicante il livello di riempimento del livello foglia di ogni pagina di indice applicato dal [!INCLUDE[ssDE](../../includes/ssde-md.md)] durante la creazione o la modifica dell'indice. *fillfactor* deve essere un valore intero compreso tra 1 e 100. Il valore predefinito è 0.  
  
 IGNORE_DUP_KEY = { ON | OFF }  
 Specifica l'errore restituito quando un'operazione di inserimento tenta di inserire valori di chiave duplicati in un indice univoco. L'opzione IGNORE_DUP_KEY viene applicata solo alle operazioni di inserimento eseguite dopo la creazione o la ricompilazione dell'indice. Il valore predefinito è OFF.  
  
 STATISTICS_NORECOMPUTE = { ON | OFF }  
 Specifica se le statistiche di distribuzione vengono ricalcolate. Il valore predefinito è OFF.  
  
 ALLOW_ROW_LOCKS = { ON | OFF }  
 Specifica se sono consentiti blocchi di riga. Il valore predefinito è ON.  
  
 ALLOW_PAGE_LOCKS = { ON | OFF }  
 Specifica se sono consentiti blocchi a livello di pagina. Il valore predefinito è ON.  
  
## <a name="remarks"></a>Osservazioni  
 L'istruzione ALTER FUNCTION non può essere usata per trasformare una funzione a valori scalari in una funzione con valori di tabella e viceversa, né per convertire una funzione inline in una funzione a più istruzioni e viceversa. Non può inoltre essere usata per convertire una funzione [!INCLUDE[tsql](../../includes/tsql-md.md)] in una funzione CLR e viceversa.  
  
 Nella definizione di una funzione [!INCLUDE[tsql](../../includes/tsql-md.md)] definita dall'utente non è possibile includere le istruzioni di Service Broker seguenti:  
  
-   BEGIN DIALOG CONVERSATION  
-   END CONVERSATION  
-   GET CONVERSATION GROUP  
-   MOVE CONVERSATION  
-   RECEIVE  
-   SEND  
  
## <a name="permissions"></a>Autorizzazioni  
 È necessario disporre dell'autorizzazione ALTER per la funzione o lo schema. Se per la funzione viene specificato un tipo definito dall'utente, è necessario disporre dell'autorizzazione EXECUTE per tale tipo.  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md)   
 [DROP FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-function-transact-sql.md)   
 [Apportare modifiche allo schema nei database di pubblicazione](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
