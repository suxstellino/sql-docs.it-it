---
description: CREATE TYPE (Transact-SQL)
title: CREATE TYPE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/05/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- sql13.swb.sysdatatype.properties.f1
- CREATE TYPE
- TYPE_TSQL
- TYPE
- CREATE_TYPE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- UDTs [SQL Server], creating
- CLR user-defined types [SQL Server]
- user-defined table types [SQL Server]
- user-defined types [SQL Server], creating
- CREATE TYPE statement
- alias data types [SQL Server], creating
- data types [SQL Server], creating
ms.assetid: 2202236b-e09f-40a1-bbc7-b8cff7488905
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2063543c497070bd8366cae9a8691b3701fc4330
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177911"
---
# <a name="create-type-transact-sql"></a>CREATE TYPE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Crea un tipo di dati alias o un tipo definito dall'utente (UDT) nel database corrente in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. L'implementazione di un tipo di dati alias è basata su un tipo di sistema nativo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. I tipi definiti dall'utente vengono invece implementati tramite una classe di un assembly CLR (Common Language Runtime) di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. Per associare un tipo definito dall'utente alla relativa implementazione, è necessario prima registrare l'assembly CLR che contiene l'implementazione del tipo in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite [CREATE ASSEMBLY](../../t-sql/statements/create-assembly-transact-sql.md).  
  
 Per impostazione predefinita, l'esecuzione di codice CLR è disattivata in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. È possibile creare, modificare ed eliminare oggetti di database che fanno riferimento a moduli di codice gestito, ma tali riferimenti non verranno eseguiti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a meno che non si abiliti l'opzione [clr enabled](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md) tramite [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
 
> [!NOTE]  
>  In questo argomento viene illustrata l'integrazione di CLR di .NET Framework in SQL Server. L'integrazione di CLR non si applica al [!INCLUDE[ssSDS](../../includes/sssds-md.md)] di Azure.
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- User-defined Data Type Syntax    
CREATE TYPE [ schema_name. ] type_name  
{   
    [
      FROM base_type   
      [ ( precision [ , scale ] ) ]  
      [ NULL | NOT NULL ]
    ]
    | EXTERNAL NAME assembly_name [ .class_name ]   
    | AS TABLE ( { <column_definition> | <computed_column_definition> [ ,... n ] }
      [ <table_constraint> ] [ ,... n ]    
      [ <table_index> ] [ ,... n ] } )
 
} [ ; ]  
  
<column_definition> ::=  
column_name <data_type>  
    [ COLLATE collation_name ]   
    [ NULL | NOT NULL ]  
    [   
        DEFAULT constant_expression ]   
      | [ IDENTITY [ ( seed ,increment ) ]   
    ]  
    [ ROWGUIDCOL ] [ <column_constraint> [ ...n ] ]   
  
<data type> ::=   
[ type_schema_name . ] type_name   
    [ ( precision [ , scale ] | max |   
                [ { CONTENT | DOCUMENT } ] xml_schema_collection ) ]   
  
<column_constraint> ::=   
{     { PRIMARY KEY | UNIQUE }   
        [ CLUSTERED | NONCLUSTERED ]   
        [   
            WITH ( <index_option> [ ,...n ] )   
        ]  
  | CHECK ( logical_expression )   
}   
  
<computed_column_definition> ::=  
  
column_name AS computed_column_expression   
[ PERSISTED [ NOT NULL ] ]  
[   
    { PRIMARY KEY | UNIQUE }  
        [ CLUSTERED | NONCLUSTERED ]  
        [   
            WITH ( <index_option> [ ,...n ] )  
        ]  
    | CHECK ( logical_expression )   
]   
  
<table_constraint> ::=  
{   
    { PRIMARY KEY | UNIQUE }   
        [ CLUSTERED | NONCLUSTERED ]   
    ( column [ ASC | DESC ] [ ,...n ] )   
        [   
    WITH ( <index_option> [ ,...n ] )   
        ]  
    | CHECK ( logical_expression )   
}   
  
<index_option> ::=  
{  
    IGNORE_DUP_KEY = { ON | OFF }  
}  

< table_index > ::=  
  INDEX constraint_name  
     [ CLUSTERED | NONCLUSTERED ]   (column [ ASC | DESC ] [ ,... n ] )} }  
```  
  
```syntaxsql
-- User-defined Memory Optimized Table Types Syntax  
CREATE TYPE [schema_name. ] type_name  
AS TABLE ( { <column_definition> [ ,... n ] }  
    | [ <table_constraint> ] [ ,... n ]    
    | [ <table_index> ] [ ,... n ] } )
    [ WITH ( <table_option> [ ,... n ] ) ]  
 [ ; ]  
  
<column_definition> ::=  
column_name <data_type>  
    [ COLLATE collation_name ]   [ NULL | NOT NULL ]    [  
      [ IDENTITY [ (1 , 1) ]  
    ]  
    [ <column_constraint> [ ... n ] ]    [ <column_index> ]  
  
<data type> ::=  
 [type_schema_name . ] type_name [ (precision [ , scale ]) ]  
  
<column_constraint> ::=  
{ PRIMARY KEY {   NONCLUSTERED HASH WITH (BUCKET_COUNT = bucket_count) 
                | NONCLUSTERED } }  
  
< table_constraint > ::=  
{ PRIMARY KEY { NONCLUSTERED HASH (column [ ,... n ] ) 
                   WITH (BUCKET_COUNT = bucket_count) 
               | NONCLUSTERED  (column [ ASC | DESC ] [ ,... n ] )  } }  
  
<column_index> ::=  
  INDEX index_name  
{ { [ NONCLUSTERED ] HASH WITH (BUCKET_COUNT = bucket_count) 
     | NONCLUSTERED } }  
  
< table_index > ::=  
  INDEX constraint_name  
{ { [ NONCLUSTERED ] HASH (column [ ,... n ] ) WITH (BUCKET_COUNT = bucket_count) 
 |  [NONCLUSTERED]  (column [ ASC | DESC ] [ ,... n ] )} }  
  
<table_option> ::=  
{  
    [MEMORY_OPTIMIZED = {ON | OFF}]  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *schema_name*  
 Nome dello schema a cui appartiene il tipo di dati alias o il tipo definito dall'utente.  
  
 *type_name*  
 Nome del tipo di dati alias o del tipo definito dall'utente. I nomi dei tipi devono essere conformi alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md).  
  
 *base_type*  
 È il tipo di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] su cui è basato il tipo di dati alias. *base_type* è di tipo **sysname** e non prevede alcun valore predefinito. I possibili valori sono i seguenti:  
  
|||||  
|-|-|-|-|  
|**bigint**|**binary(** *n* **)**|**bit**|**char(** *n* **)**|  
|**date**|**datetime**|**datetime2**|**datetimeoffset**|  
|**decimal**|**float**|**image**|**int**|  
|**money**|**nchar(** *n* **)**|**ntext**|**numeric**|  
|**nvarchar(** *n* &#124; **max)**|**real**|**smalldatetime**|**smallint**|  
|**smallmoney**|**sql_variant**|**text**|**time**|  
|**tinyint**|**uniqueidentifier**|**varbinary(** *n* &#124; **max)**|**varchar(** *n* &#124; **max)**|  
  
 Per l'argomento *base_type* è anche possibile specificare qualsiasi sinonimo di tipo di dati che esegue il mapping a uno di questi tipi di dati di sistema.  
  
 *precisione*  
 Per i tipi **decimal** o **numeric**, valore integer non negativo che indica il numero massimo totale di cifre decimali che è possibile archiviare, sia a sinistra che a destra del separatore decimale. Per altre informazioni, vedere [decimal e numeric &#40;Transact-SQL&#41;](../../t-sql/data-types/decimal-and-numeric-transact-sql.md).  
  
 *scale*  
 Per i tipi **decimal** o **numeric**, valore integer non negativo che indica il numero massimo di cifre decimali che è possibile archiviare a destra del separatore decimale. Deve essere minore o uguale al valore della precisione. Per altre informazioni, vedere [decimal e numeric &#40;Transact-SQL&#41;](../../t-sql/data-types/decimal-and-numeric-transact-sql.md).  
  
 **NULL** | NOT NULL  
 Specifica se il tipo supporta la memorizzazione di valori Null. Se omesso, l'impostazione predefinita è NULL.  
  
 *assembly_name*  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 Specifica l'assembly di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che fa riferimento all'implementazione del tipo definito dall'utente in CLR. *assembly_name* deve corrispondere a un assembly esistente in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel database corrente.  
  
> [!NOTE]  
>  EXTERNAL_NAME non è disponibile in un database indipendente.  
  
 **[.** *class_name*  **]**  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 Specifica la classe nell'assembly che implementa il tipo definito dall'utente. *class_name* deve essere un identificatore valido e deve esistere come classe nell'assembly con visibilità dell'assembly. *class_name* supporta la distinzione tra maiuscole e minuscole, indipendentemente dalle regole di confronto del database e deve corrispondere esattamente al nome di classe nell'assembly corrispondente. Il nome di classe può essere un nome completo con lo spazio dei nomi e racchiuso tra parentesi quadre ( **[ ]** ) se il linguaggio di programmazione usato per scrivere la classe usa il concetto degli spazi dei nomi, come nel caso, ad esempio, di C#. Se *class_name* viene omesso, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] presume che equivalga a *type_name*.  
  
 \<column_definition>  
 Definisce le colonne per un tipo di tabella definito dall'utente.  
  
 \<data type>  
 Definisce il tipo di dati in una colonna con un tipo di tabella definito dall'utente. Per altre informazioni sui tipi di dati , vedere [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md). Per altre informazioni sulle tabelle , vedere [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).  
  
 \<column_constraint>  
 Definisce i vincoli di colonna per un tipo di tabella definito dall'utente. I vincoli supportati includono PRIMARY KEY, UNIQUE e CHECK. Per altre informazioni sulle tabelle , vedere [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).  
  
 \<computed_column_definition>  
 Definisce un'espressione di colonna calcolata come una colonna in un tipo di tabella definito dall'utente. Per altre informazioni sulle tabelle , vedere [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).  
  
 \<table_constraint>  
 Definisce un vincolo di tabella per un tipo di tabella definito dall'utente. I vincoli supportati includono PRIMARY KEY, UNIQUE e CHECK.  
  
 \<index_option>  
 Specifica il tipo di risposta in caso di errori di valori di chiave duplicati in un'operazione di inserimento di più righe su un indice cluster o non cluster univoco. Per altre informazioni sulle opzioni per gli indici, vedere [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
 
  `INDEX *index_name* [ CLUSTERED | NONCLUSTERED ] (*column_name* [ ASC | DESC ] [ ,... *n* ] )`  
     
**SI APPLICA A**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Specifica che deve essere creato un indice per la tabella. Può trattarsi di un indice cluster o un indice non cluster. L'indice conterrà le colonne indicate e i dati in ordine crescente o decrescente.
  
 INDEX  
 È necessario specificare gli indici di tabella e di colonna come parte dell'istruzione CREATE TABLE. CREATE INDEX e DROP INDEX non sono supportati per le tabelle ottimizzate per la memoria.  
  
 MEMORY_OPTIMIZED  
 **SI APPLICA A**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Indica se il tipo di tabella è con ottimizzazione per la memoria. Questa opzione è disabilitata per impostazione predefinita; il tipo di tabella o la tabella non è con ottimizzazione per la memoria. I tipi di tabella ottimizzata per la memoria sono tabelle utente ottimizzate per la memoria, il cui schema è persistente su disco in modo analogo ad altre tabelle utente.  
  
 BUCKET_COUNT  
 **SI APPLICA A**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Indica il numero di bucket che deve essere creato nell'indice hash. Il valore massimo per BUCKET_COUNT in indici hash è 1.073.741.824. Per altre informazioni sui numeri di bucket, vedere [Indici in tabelle con ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md). *bucket_count* è un argomento obbligatorio.  
  
 HASH  
 **SI APPLICA A**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Indica che viene creato un indice HASH. Gli indici hash sono supportati solo nelle tabelle con ottimizzazione per la memoria.  
  
## <a name="remarks"></a>Osservazioni  
 La classe dell'assembly cui si fa riferimento in *assembly_name* e i relativi metodi devono soddisfare tutti i requisiti per l'implementazione di un tipo definito dall'utente in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni su tali requisiti, vedere [Tipi CLR definiti dall'utente](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md).  
  
 Ulteriori considerazioni:  
  
-   La classe può contenere metodi di overload, ma tali metodi possono essere chiamati solo da codice gestito e non da [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
-   Tutti i membri statici devono essere dichiarati come **const** o **readonly** se *assembly_name* è impostato su SAFE o EXTERNAL_ACCESS.  
  
 Nell'ambito di un database può esistere un solo tipo definito dall'utente registrato per qualsiasi tipo specificato caricato in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da CLR. Se si crea un tipo definito dall'utente basato su un tipo CLR per cui esiste già un tipo definito dall'utente nel database, l'istruzione CREATE TYPE viene interrotta e viene generato un errore. Questa restrizione risulta necessaria per evitare ambiguità durante la risoluzione dei tipi SQL, nel caso su un tipo CLR possa essere eseguito il mapping a più di un tipo definito dall'utente.  
  
 Se qualsiasi metodo mutatore nel tipo non restituisce *void*, l'istruzione CREATE TYPE non viene eseguita.  
  
 Per modificare un tipo definito dall'utente, è necessario eliminare il tipo tramite l'istruzione DROP TYPE e quindi ricrearlo.  
  
 Diversamente dai tipi definiti dall'utente creati tramite **sp_addtype**, al ruolo del database **public** non viene concessa automaticamente l'autorizzazione REFERENCES per i tipi creati tramite CREATE TYPE. È necessario concedere l'autorizzazione separatamente.  
  
 Nei tipi di tabella definiti dall'utente, i tipi strutturati definiti dall'utente usati in *column_name* \<data type> sono parte dell'ambito dello schema del database in cui il tipo di tabella è definito. Per accedere ai tipi strutturati definiti dall'utente in un ambito diverso all'interno del database, usare nomi in due parti.  
  
 Nei tipi di tabella definiti dall'utente, la chiave primaria sulle colonne calcolate deve essere PERSISTED e NOT NULL.  
  
## <a name="memory-optimized-table-types"></a>Tipi di tabella con ottimizzazione per la memoria  
 A partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], l'elaborazione dei dati in un tipo di tabella può essere eseguita nella memoria primaria e non su disco. Per altre informazioni, vedere [OLTP in memoria &#40;ottimizzazione in memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md). Per esempi di codice che illustrano come creare tipi di tabelle ottimizzate per la memoria, vedere [Creazione di una tabella con ottimizzazione per la memoria e di una stored procedure compilata in modo nativo](../../relational-databases/in-memory-oltp/creating-a-memory-optimized-table-and-a-natively-compiled-stored-procedure.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CREATE TYPE nel database corrente e l'autorizzazione ALTER per *schema_name*. Se *schema_name* viene omesso, vengono applicate le regole predefinite per la risoluzione dei nomi per determinare lo schema dell'utente corrente. Se l'argomento *assembly_name* viene specificato, è necessario che l'utente sia il proprietario dell'assembly o che abbia l'autorizzazione REFERENCES per tale assembly.  

 Se una colonna nell'istruzione CREATE TABLE è definita con un tipo definito dall'utente, è necessaria l'autorizzazione REFERENCES per il tipo definito dall'utente.
 
   >[!NOTE]
  > Un utente che crea una tabella con una colonna che usa un tipo definito dall'utente necessita dell'autorizzazione REFERENCES per il tipo definito dall'utente.
  > Se questa tabella deve essere creata in TempDB, è necessario concedere ogni volta l'autorizzazione REFERENCES in modo esplicito **prima** che la tabella venga creata oppure è necessario aggiungere questo tipo di dati e le autorizzazioni REFERENCES al database modello. In tal caso, questo tipo di dati e le autorizzazioni saranno disponibili in TempDB in modo permanente. In caso contrario, il tipo di dati definito dall'utente e le autorizzazioni verranno rimossi al riavvio di SQL Server. Per altre informazioni, vedere [CREATE TABLE](./create-table-transact-sql.md#permissions-1).
  
## <a name="examples"></a>Esempi  
  
### <a name="a-creating-an-alias-type-based-on-the-varchar-data-type"></a>R. Creazione di un tipo alias basato sul tipo di dati varchar  
 Nell'esempio seguente viene creato un tipo alias basato sul tipo di dati di sistema `varchar`.  
  
```sql  
CREATE TYPE SSN  
FROM varchar(11) NOT NULL ;  
```  
  
### <a name="b-creating-a-user-defined-type"></a>B. Creazione di un tipo definito dall'utente  
 Nell'esempio seguente viene creato un tipo `Utf8String` che fa riferimento alla classe `utf8string` nell'assembly `utf8string`. Prima di creare il tipo, l'assembly `utf8string` viene registrato nel database locale. Sostituire la parte binaria dell'istruzione CREATE ASSEMBLY con una descrizione valida.  
  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
```sql  
CREATE ASSEMBLY utf8string  
AUTHORIZATION [dbi]   
FROM 0x4D... ;  
GO  
CREATE TYPE Utf8String   
EXTERNAL NAME utf8string.[Microsoft.Samples.SqlServer.utf8string] ;  
GO  
```  
  
### <a name="c-creating-a-user-defined-table-type"></a>C. Creazione di un tipo di tabella definito dall'utente  
 Nell'esempio seguente viene creato un tipo di tabella definito dall'utente con due colonne. Per altre informazioni su come creare e usare parametri con valori di tabella, vedere [Usare parametri con valori di tabella &#40;motore di database&#41;](../../relational-databases/tables/use-table-valued-parameters-database-engine.md).  
  
```sql  
CREATE TYPE LocationTableType AS TABLE   
    ( LocationName VARCHAR(50)  
    , CostRate INT );  
GO  
```  

### <a name="d-creating-a-user-defined-table-type-with-primary-key-and-index"></a>D. Creazione di un tipo di tabella definito dall'utente con chiave primaria e indice
Nell'esempio seguente viene creato un tipo di tabella definito dall'utente che contiene tre colonne, una delle quali (`Name`) è la chiave primaria e un'altra (`Price`) ha un indice non cluster.  Per altre informazioni su come creare e usare parametri con valori di tabella, vedere [Usare parametri con valori di tabella &#40;motore di database&#41;](../../relational-databases/tables/use-table-valued-parameters-database-engine.md).

```sql
CREATE TYPE InventoryItem AS TABLE
(
    [Name] NVARCHAR(50) NOT NULL,
    SupplierId BIGINT NOT NULL,
    Price DECIMAL (18, 4) NULL,
    PRIMARY KEY (
        Name
    ),
    INDEX IX_InventoryItem_Price (
        Price
    )
)
GO
```
  
## <a name="see-also"></a>Vedere anche  
 [CREATE ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/create-assembly-transact-sql.md)   
 [DROP TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-type-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)    
 [Tipi CLR definiti dall'utente](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md)     
 [Uso di tipi definiti dall'utente in SQL Server](../../relational-databases/clr-integration-database-objects-user-defined-types/working-with-user-defined-types-in-sql-server.md)     
