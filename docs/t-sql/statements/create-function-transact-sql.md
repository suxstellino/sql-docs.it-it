---
description: CREATE FUNCTION (Transact-SQL)
title: CREATE FUNCTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FUNCTION
- CREATE FUNCTION
- CREATE_FUNCTION_TSQL
- FUNCTION_TSQL
- TVF
- MSTVF
dev_langs:
- TSQL
helpviewer_keywords:
- invoking functions
- extended stored procedures [SQL Server], functions
- table-valued parameters
- user-defined functions [SQL Server], creating
- CLR functions [SQL Server]
- CREATE FUNCTION statement
- nondeterministic functions
- user-defined data types
- functions [SQL Server], creating
- inline table-valued functions [SQL Server]
- multi-statement table-valued functions [SQL Server]
- table-valued functions [SQL Server], CREATE FUNCTION
- parameters [SQL Server], functions
- nesting user-defined functions
- deterministic functions
- scalar-valued functions
- scalar UDF
- MSTVF
- TVF
- functions [SQL Server], invoking
ms.assetid: 864b393f-225f-4895-8c8d-4db59ea60032
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8b2c611d9f4ee1aaf169137864edba15b9da0c15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344030"
---
# <a name="create-function-transact-sql"></a>CREATE FUNCTION (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Crea una funzione definita dall'utente in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e nel [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Una funzione definita dall'utente è una routine [!INCLUDE[tsql](../../includes/tsql-md.md)] o Common Language Runtime (CLR) tramite cui vengono accettati parametri, viene effettuata un'azione, ad esempio un calcolo complesso, e viene restituito il risultato di tale azione sotto forma di valore. Il valore restituito può essere un valore scalare (singolo) o una tabella. Utilizzare questa istruzione per creare una routine riutilizzabile che può essere utilizzata in queste modalità:

- Nelle istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)], ad esempio SELECT.

- Nelle applicazioni che chiamano la funzione.

- Nella definizione di un'altra funzione definita dall'utente.

- Per parametrizzare una vista o per migliorare le funzionalità di una vista indicizzata.

- Per definire una colonna di una tabella.

- Per definire un vincolo CHECK su una colonna.

- Per sostituire una stored procedure.

- Usare una funzione inline come predicato di filtro per un criterio di sicurezza

> [!NOTE]
>
> - In questo argomento viene illustrata l'integrazione di CLR di .NET Framework in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'integrazione di CLR non si applica a [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
> - Per [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], vedere [CREATE FUNCTION (Azure Synapse Analytics)](../../t-sql/statements/create-function-sql-data-warehouse.md).

![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintassi

```syntaxsql
-- Transact-SQL Scalar Function Syntax
CREATE [ OR ALTER ] FUNCTION [ schema_name. ] function_name
( [ { @parameter_name [ AS ][ type_schema_name. ] parameter_data_type
 [ = default ] [ READONLY ] }
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
CREATE [ OR ALTER ] FUNCTION [ schema_name. ] function_name
( [ { @parameter_name [ AS ] [ type_schema_name. ] parameter_data_type
    [ = default ] [ READONLY ] }
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
-- Transact-SQL Multi-Statement Table-Valued Function Syntax
CREATE [ OR ALTER ] FUNCTION [ schema_name. ] function_name
( [ { @parameter_name [ AS ] [ type_schema_name. ] parameter_data_type
    [ = default ] [READONLY] }
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
  | [ INLINE = { ON | OFF }]
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
      ( column_name [ ASC | DESC ] [ ,...n ]
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
-- CLR Scalar Function Syntax
CREATE [ OR ALTER ] FUNCTION [ schema_name. ] function_name
( { @parameter_name [AS] [ type_schema_name. ] parameter_data_type
    [ = default ] }
    [ ,...n ]
)
RETURNS { return_data_type }
    [ WITH <clr_function_option> [ ,...n ] ]
    [ AS ] EXTERNAL NAME <method_specifier>
[ ; ]
```

```syntaxsql
-- CLR Table-Valued Function Syntax
CREATE [ OR ALTER ] FUNCTION [ schema_name. ] function_name
( { @parameter_name [AS] [ type_schema_name. ] parameter_data_type
    [ = default ] }
    [ ,...n ]
)
RETURNS TABLE <clr_table_type_definition>
    [ WITH <clr_function_option> [ ,...n ] ]
    [ ORDER ( <order_clause> ) ]
    [ AS ] EXTERNAL NAME <method_specifier>
[ ; ]
```

```syntaxsql
-- CLR Function Clauses
<order_clause> ::=
{
   <column_name_in_clr_table_type_definition>
   [ ASC | DESC ]
} [ ,...n]

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
-- In-Memory OLTP: Syntax for natively compiled, scalar user-defined function
CREATE [ OR ALTER ] FUNCTION [ schema_name. ] function_name
 ( [ { @parameter_name [ AS ][ type_schema_name. ] parameter_data_type
    [ NULL | NOT NULL ] [ = default ] [ READONLY ] }
    [ ,...n ]
  ]
)
RETURNS return_data_type
     WITH <function_option> [ ,...n ]
    [ AS ]
    BEGIN ATOMIC WITH (set_option [ ,... n ])
        function_body
        RETURN scalar_expression
    END

<function_option>::=
{
  |  NATIVE_COMPILATION
  |  SCHEMABINDING
  | [ EXECUTE_AS_Clause ]
  | [ RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT ]
}

```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

*OR ALTER*
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ( [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Modifica la funzione in modo condizionale solo se esiste già.

> [!NOTE]
> Sintassi [OR ALTER] facoltativa per CLR è disponibile a partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 CU1.

*schema_name* Nome dello schema a cui appartiene la funzione definita dall'utente.

*function_name* Nome della funzione definita dall'utente. I nomi di funzione devono essere conformi alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md) e devono essere univoci all'interno del database e rispetto al relativo schema.

> [!NOTE]
> È necessario apporre le parentesi dopo il nome della funzione anche se non viene specificato alcun parametro.

@*parameter_name* Parametro della funzione definita dall'utente. È possibile dichiarare uno o più parametri.

Una funzione può avere al massimo 2.100 parametri. Il valore di ciascun parametro dichiarato deve essere specificato dall'utente quando viene eseguita la funzione, a meno che non venga definito un valore predefinito per tale parametro.

Specificare un nome di parametro utilizzando come primo carattere il simbolo di chiocciola (@). I nomi di parametro devono essere conformi alle regole per gli identificatori. I parametri sono locali rispetto alla funzione. È pertanto possibile utilizzare gli stessi nomi di parametro in altre funzioni. I parametri possono rappresentare solo costanti, non nomi di tabella, di colonna o di altri oggetti di database.

> [!NOTE]
> ANSI_WARNINGS non viene applicata quando vengono passati parametri a una stored procedure, una funzione definita dall'utente oppure in caso di dichiarazione e impostazione delle variabili in un'istruzione batch. Se, ad esempio, la variabile viene definita come **char(3)** e quindi impostata su un valore maggiore di tre caratteri, i dati verranno troncati alle dimensioni definite e l'istruzione `INSERT` o `UPDATE` avrà esito positivo.

[ *type_schema_name*. ] *parameter_data_type* Tipo di dati del parametro e, facoltativamente, lo schema a cui appartiene. Per le funzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] sono consentiti tutti i tipi di dati, compresi i tipi CLR definiti dall'utente e i tipi di tabella definiti dall'utente, eccetto il tipo di dati **timestamp**. Per le funzioni CLR sono consentiti tutti i tipi di dati, inclusi i tipi CLR definiti dall'utente, eccetto i tipi di dati **text**, **ntext**, **image**, i tipi di dati definiti dall'utente e **timestamp**. Non è possibile specificare i tipi non scalari,**cursor** e **table**, come tipo di dati dei parametri nelle funzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] o CLR.

Se *type_schema_name* non viene specificato, [!INCLUDE[ssDE](../../includes/ssde-md.md)] cerca *scalar_parameter_data_type* nell'ordine seguente:

- Schema contenente i nomi dei tipi di dati di sistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
- Schema predefinito dell'utente corrente nel database corrente.
- Schema **dbo** nel database corrente.

[ =*default* ] Valore predefinito del parametro. Se viene definito un valore *default*, è possibile eseguire la funzione senza specificare un valore per il parametro corrispondente a tale valore.

> [!NOTE]
> È possibile specificare parametri predefiniti per le funzioni CLR, ad eccezione dei tipi di dati **varchar(max)** e **varbinary(max)** .

Se a un parametro della funzione è associato un valore predefinito, alla chiamata della funzione è necessario specificare la parola chiave DEFAULT per recuperare il valore predefinito. Questo comportamento risulta diverso dall'utilizzo di parametri con valore predefinito nelle stored procedure in cui l'omissione del parametro implica l'utilizzo del valore predefinito. Tuttavia, la parola chiave DEFAULT non è richiesta in caso di richiamo di una funzione scalare tramite l'istruzione EXECUTE.

READONLY indica che il parametro non può essere aggiornato o modificato all'interno della definizione della funzione. READONLY è obbligatorio per i parametri di tipo tabella definiti dall'utente e non può essere usato per altri tipi di parametro.

*return_data_type* Valore restituito di una funzione scalare definita dall'utente. Per le funzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] sono consentiti tutti i tipi di dati, compresi i tipi CLR definiti dall'utente, eccetto il tipo di dati **timestamp**. Per le funzioni CLR, sono consentiti tutti i tipi di dati, compresi i tipi CLR definiti dall'utente, eccetto i tipi di dati **text**, **ntext**, **image** e **timestamp**. Non è possibile specificare un tipo non scalare,**cursor** o **table**, come tipo di dati restituito nelle funzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] o CLR.

*function_body* Specifica che una serie di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)], che nel loro complesso non producono alcun effetto collaterale, ad esempio la modifica di una tabella, definisce il valore della funzione. *function_body* viene usato solo in funzioni scalari e funzioni con valori di tabella con istruzioni multiple.

Nelle funzioni scalari *function_body* corrisponde a una serie di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] che in combinazione restituiscono un valore scalare.

Nelle funzioni con valori di tabella con istruzioni multiple *function_body* corrisponde a una serie di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] che popolano una variabile restituita TABLE.

*scalar_expression* Specifica il valore scalare restituito dalla funzione scalare.

TABLE specifica che il valore restituito della funzione con valori di tabella è una tabella. Alle funzioni con valori di tabella è possibile passare solo costanti e @*local_variables*.

Nelle funzioni inline con valori di tabella il valore restituito TABLE viene definito tramite una sola istruzione SELECT. Alle funzioni inline non sono associate variabili restituite.

<a name="mstvf"></a> Nelle funzioni con valori di tabella con istruzioni multiple \@*return_variable* è una variabile TABLE usata per l'archiviazione e l'accumulo delle righe da restituire come valore della funzione. È possibile specificare \@*return_variable* solo per le funzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] e non per le funzioni CLR.

*select_stmt* Istruzione SELECT che definisce il valore restituito di una funzione inline con valori di tabella.

ORDER (\<order_clause>) Specifica l'ordine in base a cui vengono restituiti i risultati dalla funzione con valori di tabella. Per altre informazioni, vedere la sezione "[Uso dell'ordinamento nelle funzioni CLR con valori di tabella](#using-sort-order-in-clr-table-valued-functions)" più avanti in questo argomento.

EXTERNAL NAME \<method_specifier> *assembly_name*.*class_name*.*method_name*
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ( [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP1 e versioni successive)

Specifica l'assembly e il metodo a cui dovrà fare riferimento il nome della funzione creata.

- *assembly_name*: deve corrispondere a un valore nella colonna `name` di `SELECT * FROM sys.assemblies;`.

  Si tratta del nome usato nell'istruzione `CREATE ASSEMBLY`.

- *class_name* deve corrispondere a un valore nella colonna `assembly_name` di `SELECT * FROM sys.assembly_modules;`.

  Spesso il valore contiene un punto incorporato. In tal caso, la sintassi Transact-SQL richiede che il valore venga racchiuso tra parentesi quadre [ ] o tra virgolette doppie "".

- *method_name* deve corrispondere a un valore nella colonna `method_name` di `SELECT * FROM sys.assembly_modules;`.

  Il metodo deve essere statico.

In un tipico esempio di MyFood.DLL, in cui tutti i tipi sono nello spazio dei nomi MyFood, il valore `EXTERNAL NAME` potrebbe essere: `MyFood.[MyFood.MyClass].MyStaticMethod`

> [!NOTE]
>
> - Per impostazione predefinita, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non può eseguire il codice CLR. È possibile creare, modificare ed eliminare gli oggetti di database che fanno riferimento a moduli CLR; tuttavia non è possibile eseguire questi riferimenti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] finché non viene abilitata l'opzione [clr enabled option](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md). Per abilitare questa opzione, usare [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).
> - Questa opzione non è disponibile in un database indipendente.

*\<*table_type_definition*>* ( { \<column_definition> \<column_constraint>| \<computed_column_definition> } [ \<table_constraint> ] [ ,...*n* ] ) Definisce il tipo di dati tabella per una funzione [!INCLUDE[tsql](../../includes/tsql-md.md)]. La dichiarazione di tabella include definizioni di colonna, nonché vincoli di colonna o tabella. La tabella viene sempre inserita nel filegroup primario.

*\< clr_table_type_definition >* ( { *column_name**data_type* } [ ,...*n* ] ) **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP1 e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] ([anteprima in alcune aree](/azure/azure-sql/database/features-comparison?WT.mc_id=TSQL_GetItTag)).

Definisce i tipi di dati della tabella per una funzione CLR. La dichiarazione di tabella include solo nomi di colonna e tipi di dati. La tabella viene sempre inserita nel filegroup primario.

NULL|NOT NULL Supportato solo per funzioni definite dall'utente scalari compilate in modo nativo. Per altre informazioni, vedere [Funzioni scalari definite dall'utente per OLTP in memoria](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md).

NATIVE_COMPILATION Indica se una funzione definita dall'utente è compilata in modo nativo. Questo argomento è obbligatorio per funzioni definite dall'utente scalari compilate in modo nativo.

BEGIN ATOMIC WITH Supportato solo per funzioni definite dall'utente scalari compilate in modo nativo. Obbligatorio. Per altre informazioni, vedere [Atomic Blocks](../../relational-databases/in-memory-oltp/atomic-blocks-in-native-procedures.md).

SCHEMABINDING L'argomento SCHEMABINDING è obbligatorio per funzioni definite dall'utente scalari compilate in modo nativo.

EXECUTE AS EXECUTE AS è obbligatorio per funzioni definite dall'utente scalari compilate in modo nativo.

**\<function_option>::= e \<clr_function_option>::=**

Specifica che la funzione includerà una o più delle opzioni seguenti.

ENCRYPTION **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ( [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP1 e versioni successive)

Indica che il testo originale dell'istruzione CREATE FUNCTION verrà convertito da [!INCLUDE[ssDE](../../includes/ssde-md.md)] in un formato offuscato. L'output dell'offuscamento non è visibile direttamente nelle viste del catalogo. Gli utenti che non hanno accesso a tabelle di sistema o file del database non possono recuperare il testo offuscato. Il testo è tuttavia disponibile per gli utenti con privilegi che consentono l'accesso alle tabelle di sistema attraverso la [porta DAC](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md) o l'accesso diretto a file del database. Gli utenti in grado di collegare un debugger al processo del server possono inoltre recuperare la procedura originale dalla memoria in fase di esecuzione. Per altre informazioni sull'accesso ai metadati di sistema, vedere [Configurazione della visibilità dei metadati](../../relational-databases/security/metadata-visibility-configuration.md).

Tramite questa opzione è possibile evitare la pubblicazione della funzione come parte della replica di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questa opzione non può essere specificata per le funzioni CLR.

SCHEMABINDING Specifica che la funzione è associata agli oggetti di database a cui fa riferimento. Quando la clausola SCHEMABINDING viene specificata, non è possibile apportare agli oggetti di base modifiche che hanno effetto sulla definizione della funzione. È necessario prima modificare o eliminare la definizione della funzione per rimuovere le dipendenze dall'oggetto da modificare.

L'associazione della funzione agli oggetti cui fa riferimento viene rimossa solo quando viene eseguita una delle azioni seguenti:

- La funzione viene eliminata.
- La funzione viene modificata tramite l'istruzione `ALTER` senza specificare l'opzione `SCHEMABINDING`.

Una funzione può essere associata a uno schema solo se vengono soddisfatte le condizioni seguenti:

- Si tratta di una funzione [!INCLUDE[tsql](../../includes/tsql-md.md)].
- Le funzioni definite dall'utente e le viste a cui la funzione fa riferimento sono anch'esse associate a uno schema.
- Per i riferimenti agli oggetti, nella funzione viene utilizzato un nome in due parti.
- La funzione e gli oggetti a cui fa riferimento appartengono allo stesso database.
- L'utente che ha eseguito l'istruzione `CREATE FUNCTION` dispone dell'autorizzazione `REFERENCES` per gli oggetti di database a cui la funzione fa riferimento.

RETURNS NULL ON NULL INPUT | **CALLED ON NULL INPUT** Specifica l'attributo **OnNULLCall** di una funzione scalare. Se omesso, viene utilizzata l'opzione CALLED ON NULL INPUT per impostazione predefinita. Ciò significa che viene eseguito il corpo della funzione anche se come argomento viene passato NULL.

Se in una funzione CLR si specifica l'opzione RETURNS NULL ON NULL INPUT, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce NULL se uno qualsiasi degli argomenti ricevuti è NULL senza effettivamente richiamare il corpo della funzione. Se per il metodo di una funzione CLR specificato in \<method_specifier> è stato definito un attributo personalizzato impostato su RETURNS NULL ON NULL INPUT, ma l'istruzione CREATE FUNCTION include CALLED ON NULL INPUT, l'istruzione CREATE FUNCTION risulta prioritaria. Non è possibile specificare l'attributo **OnNULLCall** per le funzioni CLR con valori di tabella.

Clausola EXECUTE AS Specifica il contesto di sicurezza nel quale viene eseguita la funzione definita dall'utente. Sarà pertanto possibile controllare l'account utente utilizzato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per convalidare le autorizzazioni per qualsiasi oggetto di database a cui la funzione fa riferimento.

> [!NOTE]
> Non è possibile specificare la clausola `EXECUTE AS` per le funzioni inline con valori di tabella.

Per altre informazioni, vedere [Clausola EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md).

INLINE = { ON | OFF } **SI APPLICA A**: SQL Server 2019 e versioni successive.

Specifica se questa funzione scalare definita dall'utente deve essere impostata come inline. Questa clausola è applicabile solo alle funzioni scalari definite dall'utente. La clausola `INLINE` non è obbligatoria. Se la clausola `INLINE` viene omessa, viene automaticamente impostata su ON/OFF in base al fatto che possa essere eseguito l'inline della funzione. Se viene specificato `INLINE=ON` ma la funzione definita dall'utente non è idonea per l'inlining, viene generato un errore. Per altre informazioni, vedere [Inlining di funzioni definite dall'utente scalari](../../relational-databases/user-defined-functions/scalar-udf-inlining.md).

**\< column_definition >::=**

Definisce il tipo di dati della tabella. La dichiarazione di tabella include definizioni di colonna e vincoli. Per le funzioni CLR, è possibile specificare solo *column_name* e *data_type*.

*column_name* Nome di una colonna della tabella. I nomi delle colonne devono essere conformi alle regole per gli identificatori e devono essere univoci nella tabella. *column_name* può essere costituito da un numero di caratteri compreso tra 1 e 128.

*data_type* Specifica il tipo di dati della colonna. Per le funzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] sono consentiti tutti i tipi di dati, compresi i tipi CLR definiti dall'utente, eccetto il tipo di dati **timestamp**. Per le funzioni CLR sono consentiti tutti i tipi di dati, compresi i tipi CLR definiti dall'utente, eccetto i tipi di dati **text**, **ntext**, **image**, **char**, **varchar**, **varchar(max)** e **timestamp**. Non è possibile specificare il tipo non scalare **cursor** come tipo di dati di colonna nelle funzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] o CLR.

DEFAULT *constant_expression* Specifica il valore assegnato alla colonna quando non ne viene specificato uno in modo esplicito durante un inserimento. *constant_expression* è una costante, un valore NULL o il valore di una funzione di sistema. Le definizioni dell'opzione DEFAULT possono essere applicate a qualsiasi colonna eccetto quelle che includono la proprietà IDENTITY. Non è possibile specificare l'opzione DEFAULT per le funzioni CLR con valori di tabella.

COLLATE *collation_name* specifica le regole di confronto per la colonna. Se viene omesso, alla colonna vengono assegnate le regole di confronto predefinite del database. È possibile usare nomi di regole di confronto di Windows o SQL. Per un elenco di regole di confronto e altre informazioni su queste, vedere [Windows_collation_name & &#40;Transact-SQL&#41;](../../t-sql/statements/windows-collation-name-transact-sql.md) e [Nome delle regole di confronto di SQL Server &#40;Transact-SQL&#41;](../../t-sql/statements/sql-server-collation-name-transact-sql.md).

La clausola COLLATE consente di modificare le regole di confronto solo per le colonne con tipo di dati **char**, **varchar**, **nchar** e **nvarchar**.

> [!NOTE]
> Non è possibile specificare la clausola `COLLATE` per le funzioni CLR con valori di tabella.

ROWGUIDCOL Indica che la nuova colonna è un identificatore univoco di riga globale. È possibile designare come colonna ROWGUIDCOL una sola colonna di tipo **uniqueidentifier** per ogni tabella. La proprietà ROWGUIDCOL può essere assegnata solo a una colonna **uniqueidentifier**.

La proprietà ROWGUIDCOL non impone l'univocità dei valori archiviati nella colonna e non genera automaticamente valori per le nuove righe inserite nella tabella. Per generare valori univoci per ogni colonna, utilizzare la funzione NEWID nelle istruzioni INSERT. È possibile specificare un valore predefinito. Non è tuttavia possibile specificare l'opzione NEWID come valore predefinito.

IDENTITY indica che la nuova colonna è una colonna Identity. Quando si aggiunge una nuova riga alla tabella, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] assegna alla colonna un valore univoco e incrementale. Le colonne Identity vengono comunemente utilizzate in combinazione con vincoli PRIMARY KEY e svolgono la funzione di identificatore di riga univoco per la tabella. La proprietà IDENTITY può essere assegnata a colonne **tinyint**, **smallint**, **int**, **bigint**, **decimal(p,0)** o **numeric(p,0)** . Ogni tabella può includere una sola colonna Identity. Non è consentito associare valori predefiniti e vincoli DEFAULT alle colonne Identity. È necessario specificare sia il valore di *seed* che di *increment* oppure è possibile omettere entrambi questi valori. In questo secondo caso, il valore predefinito è (1,1).

Non è possibile specificare l'opzione IDENTITY per le funzioni CLR con valori di tabella.

*seed* Valore intero da assegnare alla prima riga della tabella.

*increment* Valore intero da aggiungere al valore *seed* per le righe successive della tabella.

 **\< column_constraint >::= e \< table_constraint>::=**

Definisce il vincolo per una colonna o tabella specificata. Per le funzioni CLR l'unico tipo di vincolo consentito è NULL. I vincoli denominati non sono consentiti.

NULL | NOT NULL Determina se i valori Null sono consentiti nella colonna. L'opzione NULL non è esattamente un vincolo, ma può essere specificata allo stesso modo di NOT NULL. Non è possibile specificare l'opzione NOT NULL per le funzioni CLR con valori di tabella.

PRIMARY KEY Vincolo che impone l'integrità di entità per una colonna specificata tramite un indice univoco. Nelle funzioni definite dall'utente con valori di tabella, il vincolo PRIMARY KEY può essere creato solo su una colonna per tabella. Non è possibile specificare il vincolo PRIMARY KEY per le funzioni CLR con valori di tabella.

UNIQUE è un vincolo che impone l'integrità di entità per una o più colonne specificate tramite un indice univoco. Una tabella può includere più vincoli UNIQUE. Non è possibile specificare il vincolo UNIQUE per le funzioni CLR con valori di tabella.

CLUSTERED | NONCLUSTERED indica che per il vincolo PRIMARY KEY o UNIQUE viene creato un indice cluster o non cluster. I vincoli PRIMARY KEY usano l'opzione CLUSTERED, mentre i vincoli UNIQUE usano l'opzione NONCLUSTERED.

L'opzione CLUSTERED può essere specificata solo per un vincolo. Se per un vincolo UNIQUE si specifica CLUSTERED e viene specificato anche un vincolo PRIMARY KEY, quest'ultimo usano l'opzione NONCLUSTERED.

Non è possibile specificare le opzioni CLUSTERED e NON CLUSTERED per le funzioni CLR con valori di tabella.

CHECK è un vincolo che impone l'integrità di dominio tramite la limitazione dei valori che è possibile immettere in una o più colonne. Non è possibile specificare i vincoli CHECK per le funzioni CLR con valori di tabella.

*logical_expression* è un'espressione logica che restituisce TRUE o FALSE.

**\<computed_column_definition>::=**

Specifica una colonna calcolata. Per altre informazioni sulle colonne calcolate, vedere [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).

*column_name* Nome della colonna calcolata.

*computed_column_expression* Espressione che definisce il valore di una colonna calcolata.

**\<index_option>::=**

Specifica le opzioni per l'indice PRIMARY KEY o UNIQUE. Per altre informazioni sulle opzioni per gli indici, vedere [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).

PAD_INDEX = { ON | **OFF** } Specifica il riempimento dell'indice. Il valore predefinito è OFF.

FILLFACTOR = *fillfactor* Specifica una percentuale indicante il livello di riempimento del livello foglia di ogni pagina di indice applicato dal [!INCLUDE[ssDE](../../includes/ssde-md.md)] durante la creazione o la modifica dell'indice. *fillfactor* deve essere un valore intero compreso tra 1 e 100. Il valore predefinito è 0.

IGNORE_DUP_KEY = { ON | **OFF** } specifica la risposta all'errore quando un'operazione di inserimento tenta di inserire valori di chiave duplicati in un indice univoco. L'opzione IGNORE_DUP_KEY viene applicata solo alle operazioni di inserimento eseguite dopo la creazione o la ricompilazione dell'indice. Il valore predefinito è OFF.

STATISTICS_NORECOMPUTE = { ON | **OFF** } Specifica se le statistiche di distribuzione vengono ricalcolate. Il valore predefinito è OFF.

ALLOW_ROW_LOCKS = { **ON** | OFF } Specifica se sono consentiti blocchi di riga. Il valore predefinito è ON.

ALLOW_PAGE_LOCKS = { **ON** | OFF } Specifica se sono consentiti blocchi di pagina. Il valore predefinito è ON.

## <a name="best-practices"></a>Procedure consigliate

Se una funzione definita dall'utente non viene creata tramite la clausola `SCHEMABINDING`, le modifiche apportate agli oggetti sottostanti possono influire sulla definizione della funzione e generare risultati imprevisti quando viene richiamata. È consigliabile implementare uno dei metodi seguenti per assicurarsi che la funzione non diventi obsoleta in seguito a modifiche degli oggetti sottostanti:

- Specificare la clausola `WITH SCHEMABINDING` quando si crea la funzione. In questo modo, gli oggetti a cui si fa riferimento nella definizione della funzione possono essere modificati solo se viene modificata anche la funzione.
- Eseguire la stored procedure [sp_refreshsqlmodule](../../relational-databases/system-stored-procedures/sp-refreshsqlmodule-transact-sql.md) dopo avere modificato qualsiasi oggetto specificato nella definizione della funzione.

> [!IMPORTANT]
> Per altre informazioni e considerazioni sulle prestazioni delle funzioni inline con valori di tabella e delle funzioni con valori di tabella con istruzioni multiple, vedere [Creare funzioni definite dall'utente &#40;Motore di database&#41;](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md).

## <a name="data-types"></a>Tipi di dati

Se si specificano parametri in una funzione CLR, essi dovranno essere di tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] come precedentemente definito per *scalar_parameter_data_type*. Per informazioni sul confronto tra i tipi di dati di sistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e i tipi di dati di integrazione CLR oppure i tipi di dati CLR di [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], vedere [Mapping dei dati dei parametri CLR](../../relational-databases/clr-integration-database-objects-types-net-framework/mapping-clr-parameter-data.md).

Perché [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] faccia riferimento al metodo corretto in caso di un sovraccarico a livello di classe, è necessario che il metodo specificato in \<method_specifier>:

- Riceva lo stesso numero di parametri specificati in [ ,...*n* ].
- Riceva tutti i parametri in base al valore e non in base al riferimento.
- Utilizzi tipi di parametro compatibili con quelli specificati nella funzione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Se il tipo di dati restituito della funzione CLR specifica un tipo tabella (RETURNS TABLE), il tipo di dati restituito del metodo specificato in \<method_specifier> deve essere **IEnumerator** o **IEnumerable**. Si presuppone che l'interfaccia venga implementata dall'autore della funzione. A differenza delle funzioni [!INCLUDE[tsql](../../includes/tsql-md.md)], le funzioni CLR non possono includere vincoli PRIMARY KEY, UNIQUE o CHECK in \<table_type_definition>. I tipi di dati delle colonne specificati in \<table_type_definition> devono corrispondere ai tipi delle rispettive colonne del set di risultati restituito dal metodo specificato in \<method_specifier> in fase di esecuzione. Questa verifica del tipo non viene eseguita al momento della creazione della funzione.

Per altre informazioni sulla programmazione delle funzioni CLR, vedere [Funzioni CLR definite dall'utente](../../relational-databases/clr-integration-database-objects-user-defined-functions/clr-user-defined-functions.md).

## <a name="general-remarks"></a>Osservazioni generali

È possibile richiamare funzioni scalari laddove vengono usate espressioni scalari. Sono incluse le colonne calcolate e le definizioni dei vincoli CHECK. Le funzioni scalari possono essere eseguite anche tramite l'uso dell'istruzione [EXECUTE](../../t-sql/language-elements/execute-transact-sql.md). È necessario richiamare funzioni scalari usando almeno il nome in due parti della funzione ( *<schema>.<function>* ). Per altre informazioni sui nomi a più parti, vedere [Convenzioni della sintassi Transact-SQL (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md). Le funzioni con valori di tabella possono essere richiamate laddove sono consentite le espressioni di tabella nella clausola `FROM` delle istruzioni `SELECT`, `INSERT`, `UPDATE`, o `DELETE`. Per altre informazioni, vedere [Eseguire funzioni definite dall'utente](../../relational-databases/user-defined-functions/execute-user-defined-functions.md).

## <a name="interoperability"></a>Interoperabilità

In una funzione le istruzioni seguenti sono valide:

- Istruzioni di assegnazione.
- Istruzioni per il controllo di flusso, ad eccezione delle istruzioni `TRY...CATCH`.
- Istruzioni `DECLARE` che definiscono le variabili dati locali e i cursori locali.
- Istruzioni `SELECT` contenenti gli elenchi di selezione con espressioni che assegnano valori alle variabili locali.
- Operazioni di cursore che fanno riferimento a cursori locali dichiarati, aperti, chiusi e deallocati nella funzione. Sono consentite solo istruzioni `FETCH` che assegnano valori alle variabili locali tramite la clausola `INTO`. Non sono consentite istruzioni `FETCH` che restituiscono dati al client.
- Istruzioni `INSERT`, `UPDATE` e `DELETE` che modificano le variabili di tabella locali.
- Istruzioni `EXECUTE` che chiamano stored procedure estese.

Per altre informazioni, vedere [Creare funzioni definite dall'utente &#40;Motore di database&#41;](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md).

### <a name="computed-column-interoperability"></a>Interoperabilità delle colonne calcolate

Le funzioni presentano le proprietà seguenti. I valori di tali proprietà determinano se le funzioni sono utilizzabili nelle colonne calcolate che possono essere persistenti o indicizzate.

|Proprietà|Descrizione|Note|
|--------------|-----------------|-----------|
|**IsDeterministic**|La funzione è deterministica o non deterministica.|L'accesso ai dati locali è consentito nelle funzioni deterministiche. Ad esempio, le funzioni che restituiscono sempre lo stesso valore ogni volta che vengono richiamate utilizzando un set specifico di valori di input e con lo stesso stato del database vengono definite funzioni deterministiche.|
|**IsPrecise**|La funzione è precisa o imprecisa.|Le funzioni imprecise includono operazioni quali le operazioni a virgola mobile.|
|**IsSystemVerified**|Le proprietà relative alla precisione e le proprietà deterministiche della funzione possono essere verificate tramite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].||
|**SystemDataAccess**|La funzione accede ai dati di sistema (cataloghi di sistema o tabelle di sistema virtuali) nell'istanza locale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].||
|**UserDataAccess**|La funzione accede ai dati utente nell'istanza locale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|Include le tabelle definite dall'utente e le tabelle temporanee, ma non le variabili di tabella.|

Le proprietà relative alla precisione e le proprietà deterministiche delle funzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] vengono definite automaticamente da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le proprietà relative all'accesso ai dati e le proprietà deterministiche delle funzioni CLR possono essere specificate dall'utente. Per altre informazioni, vedere [Panoramica degli attributi personalizzati dell'integrazione con CLR](../../relational-databases/clr-integration/database-objects/clr-integration-custom-attributes-for-clr-routines.md).

Per visualizzare i valori correnti di queste proprietà, usare [OBJECTPROPERTYEX](../../t-sql/functions/objectpropertyex-transact-sql.md).

> [!IMPORTANT]
> Le funzioni devono essere create con `SCHEMABINDING` per essere deterministiche.

È possibile utilizzare una colonna calcolata che richiama una funzione definita dall'utente in un indice se per le proprietà della funzione definita dall'utente sono stati impostati i valori seguenti:

- **IsDeterministic** = true
- **IsSystemVerified** = true (a meno che la colonna calcolata non sia persistente)
- **UserDataAccess** = false
- **SystemDataAccess** = false

Per altre informazioni, vedere [Indici per le colonne calcolate](../../relational-databases/indexes/indexes-on-computed-columns.md).

### <a name="calling-extended-stored-procedures-from-functions"></a>Chiamata di stored procedure estese da funzioni

Una stored procedure estesa, quando viene richiamata dall'interno di una funzione, non può restituire set di risultati al client. Le API ODS che restituiscono set di risultati al client restituiscono FAIL. La stored procedure estesa può riconnettersi a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], tuttavia è necessario che non partecipi alla stessa transazione della funzione che l'ha richiamata.

In modo analogo a una chiamata eseguita dall'interno di un batch o di una stored procedure, la stored procedure estesa viene eseguita nel contesto dell'account di sicurezza di Windows in base a cui [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in esecuzione. È importante che il proprietario della stored procedure prenda in considerazione questo fattore quando concede l'autorizzazione EXECUTE agli utenti.

## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni

Non è possibile utilizzare funzioni definite dall'utente per eseguire azioni che modificano lo stato del database.

Le funzioni definite dall'utente non possono contenere una clausola `OUTPUT INTO` che ha una tabella come destinazione.

Nella definizione di una funzione [!INCLUDE[ssSB](../../includes/sssb-md.md)] definita dall'utente non è possibile includere le istruzioni di [!INCLUDE[tsql](../../includes/tsql-md.md)] seguenti:

- `BEGIN DIALOG CONVERSATION`
- `END CONVERSATION`
- `GET CONVERSATION GROUP`
- `MOVE CONVERSATION`
- `RECEIVE`
- `SEND`

È possibile nidificare le funzioni definite dall'utente, ovvero una funzione definita dall'utente ne può richiamare un'altra. Il livello di nidificazione aumenta all'avvio della funzione richiamata e diminuisce al termine dell'esecuzione della funzione. Le funzioni definite dall'utente possono essere nidificate fino a un massimo di 32 livelli. Se viene superato il livello massimo di nidificazioni, l'intera sequenza di funzioni chiamanti ha esito negativo. Qualsiasi riferimento al codice gestito presente in una funzione [!INCLUDE[tsql](../../includes/tsql-md.md)] definita dall'utente viene considerato come un livello nel contesto del limite di 32 livelli di nidificazione. I metodi richiamati da codice gestito non vengono inclusi nel conteggio per questo limite.

### <a name="using-sort-order-in-clr-table-valued-functions"></a>Utilizzo dell'ordinamento nelle funzioni CLR con valori di tabella

Quando si usa la clausola `ORDER` in funzioni CLR con valori di tabella, attenersi alle linee guida seguenti:

- È necessario assicurarsi che i risultati siano ordinati sempre in base all'ordine specificato. Se i risultati non sono nell'ordine specificato, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verrà generato un messaggio di errore quando viene eseguita la query.
- Se è specificata una clausola `ORDER`, l'output della funzione con valori di tabella deve essere ordinato in base alle regole di confronto della colonna (esplicite o implicite). Se, ad esempio, le regole di confronto della colonna sono impostate per il cinese (in base a quanto specificato nella DDL per la funzione con valori di tabella o ottenuto dalle regole di confronto del database), i risultati restituiti devono essere ordinati in base alle regole di ordinamento per il cinese.
- La clausola [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se specificata, viene sempre verificata da `ORDER` quando vengono restituiti i risultati, indipendentemente dal fatto che venga usata da Query Processor per eseguire altre ottimizzazioni. Usare la clausola `ORDER` solo se si è certi che sia utile per Query Processor.
- Query Processor di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa automaticamente la clausola `ORDER` nei casi seguenti:

  - Query di inserimento in cui la clausola `ORDER` è compatibile con un indice.
  - Clausole `ORDER BY` compatibili con la clausola `ORDER`.
  - Aggregazioni, in cui `GROUP BY` è compatibile con la clausola `ORDER`.
  - Aggregazioni `DISTINCT` in cui le colonne distinte sono compatibili con la clausola `ORDER`.

La clausola `ORDER` non garantisce risultati ordinati quando viene eseguita una query SELECT, a meno che nella query non venga specificata anche la clausola `ORDER BY`. Vedere [sys.function_order_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-function-order-columns-transact-sql.md) per informazioni su come eseguire una query relativa alle colonne incluse nell'ordinamento per le funzioni con valori di tabella.

## <a name="metadata"></a>Metadati

Nella tabella seguente vengono elencate le viste del catalogo di sistema utilizzate per restituire i metadati sulle funzioni definite dall'utente.

|Vista di sistema|Descrizione|
|-----------------|-----------------|
|[sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)|Vedere l'esempio E nella sezione Esempi riportata più avanti.|
|[sys.assembly_modules](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)|Visualizza le informazioni sulle funzioni CLR definite dall'utente.|
|[sys.parameters](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md)|Visualizza le informazioni sui parametri definiti nelle funzioni definite dall'utente.|
|[sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)|Visualizza gli oggetti sottostanti a cui fa riferimento una funzione.|

## <a name="permissions"></a>Autorizzazioni

È necessario disporre dell'autorizzazione `CREATE FUNCTION` nel database e dell'autorizzazione `ALTER` per lo schema in cui la funzione è in fase di creazione. Se per la funzione viene specificato un tipo definito dall'utente, è necessario disporre dell'autorizzazione `EXECUTE` per tale tipo.

## <a name="examples"></a>Esempi

> [!NOTE]
> Per altri esempi e considerazioni sulle prestazioni delle funzioni definite dall'utente, vedere [Creare funzioni definite dall'utente &#40;Motore di database&#41;](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md).

### <a name="a-using-a-scalar-valued-user-defined-function-that-calculates-the-iso-week"></a>R. Utilizzo di una funzione definita dall'utente a valori scalari per il calcolo della settimana ISO

Nell'esempio seguente viene creata la funzione definita dall'utente `ISOweek`. Questa funzione calcola il numero di settimana ISO in base a un argomento di data specificato. Per consentire alla funzione di eseguire il calcolo correttamente, è necessario richiamare `SET DATEFIRST 1` prima della funzione.

L'esempio illustra anche l'uso della clausola [EXECUTE AS](../../t-sql/statements/execute-as-clause-transact-sql.md) per specificare il contesto di protezione in cui è possibile eseguire una stored procedure. Nell'esempio, l'opzione `CALLER` specifica che la procedura verrà eseguita nel contesto dell'utente che l'ha richiamata. Le altre opzioni che è possibile specificare sono `SELF`, `OWNER` e *user_name*.

Di seguito è riportata la chiamata della funzione, in cui `DATEFIRST` è impostato su `1`.

```sql
CREATE FUNCTION dbo.ISOweek (@DATE datetime)
RETURNS int
WITH EXECUTE AS CALLER
AS
BEGIN
    DECLARE @ISOweek int;
    SET @ISOweek= DATEPART(wk,@DATE)+1
        -DATEPART(wk,CAST(DATEPART(yy,@DATE) as CHAR(4))+'0104');
--Special cases: Jan 1-3 may belong to the previous year
    IF (@ISOweek=0)
        SET @ISOweek=dbo.ISOweek(CAST(DATEPART(yy,@DATE)-1
            AS CHAR(4))+'12'+ CAST(24+DATEPART(DAY,@DATE) AS CHAR(2)))+1;
--Special case: Dec 29-31 may belong to the next year
    IF ((DATEPART(mm,@DATE)=12) AND
        ((DATEPART(dd,@DATE)-DATEPART(dw,@DATE))>= 28))
    SET @ISOweek=1;
    RETURN(@ISOweek);
END;
GO
SET DATEFIRST 1;
SELECT dbo.ISOweek(CONVERT(DATETIME,'12/26/2004',101)) AS 'ISO Week';
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
ISO Week
----------------
52
```

### <a name="b-creating-an-inline-table-valued-function"></a>B. Creazione di una funzione inline con valori di tabella

Nell'esempio seguente viene restituita una funzione con valori di tabella inline nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Vengono restituite tre colonne `ProductID`, `Name` e il valore aggregato dei totali da inizio anno per negozio come `YTD Total`per ogni prodotto venduto al negozio.

```sql
CREATE FUNCTION Sales.ufn_SalesByStore (@storeid int)
RETURNS TABLE
AS
RETURN
(
    SELECT P.ProductID, P.Name, SUM(SD.LineTotal) AS 'Total'
    FROM Production.Product AS P
    JOIN Sales.SalesOrderDetail AS SD ON SD.ProductID = P.ProductID
    JOIN Sales.SalesOrderHeader AS SH ON SH.SalesOrderID = SD.SalesOrderID
    JOIN Sales.Customer AS C ON SH.CustomerID = C.CustomerID
    WHERE C.StoreID = @storeid
    GROUP BY P.ProductID, P.Name
);
GO
```

Per richiamare la funzione, eseguire la query seguente.

```sql
SELECT * FROM Sales.ufn_SalesByStore (602);
```

### <a name="c-creating-a-multi-statement-table-valued-function"></a>C. Creazione di una funzione a istruzioni multiple con valori di tabella

Nell'esempio seguente viene creata la funzione con valori di tabella `fn_FindReports(InEmpID)` nel database AdventureWorks2012. Se si specifica un ID dipendente valido, la funzione restituisce una tabella che include tutti i dipendenti che hanno rapporti diretti o indiretti con il dipendente specificato. La funzione utilizza un'espressione di tabella comune ricorsiva (CTE, Common Table Expression) per restituire l'elenco gerarchico dei dipendenti. Per altre informazioni sulle espressioni CTE ricorsive, vedere [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md).

```sql
CREATE FUNCTION dbo.ufn_FindReports (@InEmpID INTEGER)
RETURNS @retFindReports TABLE
(
    EmployeeID int primary key NOT NULL,
    FirstName nvarchar(255) NOT NULL,
    LastName nvarchar(255) NOT NULL,
    JobTitle nvarchar(50) NOT NULL,
    RecursionLevel int NOT NULL
)
--Returns a result set that lists all the employees who report to the
--specific employee directly or indirectly.*/
AS
BEGIN
WITH EMP_cte(EmployeeID, OrganizationNode, FirstName, LastName, JobTitle, RecursionLevel) -- CTE name and columns
    AS (
        -- Get the initial list of Employees for Manager n
        SELECT e.BusinessEntityID, e.OrganizationNode, p.FirstName, p.LastName, e.JobTitle, 0
        FROM HumanResources.Employee e
INNER JOIN Person.Person p
ON p.BusinessEntityID = e.BusinessEntityID
        WHERE e.BusinessEntityID = @InEmpID
        UNION ALL
        -- Join recursive member to anchor
        SELECT e.BusinessEntityID, e.OrganizationNode, p.FirstName, p.LastName, e.JobTitle, RecursionLevel + 1
        FROM HumanResources.Employee e
            INNER JOIN EMP_cte
            ON e.OrganizationNode.GetAncestor(1) = EMP_cte.OrganizationNode
INNER JOIN Person.Person p
ON p.BusinessEntityID = e.BusinessEntityID
        )
-- copy the required columns to the result of the function
    INSERT @retFindReports
    SELECT EmployeeID, FirstName, LastName, JobTitle, RecursionLevel
    FROM EMP_cte
    RETURN
END;
GO
-- Example invocation
SELECT EmployeeID, FirstName, LastName, JobTitle, RecursionLevel
FROM dbo.ufn_FindReports(1);

GO
```

### <a name="d-creating-a-clr-function"></a>D. Creazione di una funzione CLR

L'esempio crea la funzione CLR `len_s`, ma prima che venga effettivamente creata la funzione, l'assembly `SurrogateStringFunction.dll` viene registrato nel database locale.

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ( [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP1 e versioni successive)

```sql
DECLARE @SamplesPath nvarchar(1024);
-- You may have to modify the value of this variable if you have
-- installed the sample in a location other than the default location.
SELECT @SamplesPath = REPLACE
    (  physical_name
     , 'Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\master.mdf'
     , 'Microsoft SQL Server\130\Samples\Engine\Programmability\CLR\'
    )
  FROM master.sys.database_files
  WHERE name = 'master';

CREATE ASSEMBLY [SurrogateStringFunction]
FROM @SamplesPath + 'StringManipulate\CS\StringManipulate\bin\debug\SurrogateStringFunction.dll'
WITH PERMISSION_SET = EXTERNAL_ACCESS;
GO

CREATE FUNCTION [dbo].[len_s] (@str nvarchar(4000))
RETURNS bigint
AS EXTERNAL NAME [SurrogateStringFunction].[Microsoft.Samples.SqlServer.SurrogateStringFunction].[LenS];
GO
```

Per un esempio relativo alla creazione di una funzione CLR con valori di tabella, vedere [Funzioni CLR con valori di tabella](../../relational-databases/clr-integration-database-objects-user-defined-functions/clr-table-valued-functions.md).

### <a name="e-displaying-the-definition-of-user-defined-functions"></a>E. Visualizzazione della definizione di funzioni definite dall'utente

```sql
SELECT definition, type
FROM sys.sql_modules AS m
JOIN sys.objects AS o ON m.object_id = o.object_id
    AND type IN ('FN', 'IF', 'TF');
GO
```

La definizione delle funzioni create tramite l'opzione `ENCRYPTION` non possono essere visualizzate tramite la vista sys.sql_modules. Vengono tuttavia visualizzate altre informazioni sulle funzioni crittografate.

## <a name="see-also"></a>Vedere anche

- [Creare funzioni definite dall'utente &#40;motore di database&#41;](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md)
- [ALTER FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-function-transact-sql.md)
- [DROP FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-function-transact-sql.md)
- [OBJECTPROPERTYEX &#40;Transact-SQL&#41;](../../t-sql/functions/objectpropertyex-transact-sql.md)
- [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)
- [sys.assembly_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)
- [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)
- [Funzioni CLR definite dall'utente](../../relational-databases/clr-integration-database-objects-user-defined-functions/clr-user-defined-functions.md)
- [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)
- [CREATE SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/create-security-policy-transact-sql.md)