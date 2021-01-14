---
description: sys.objects (Transact-SQL)
title: sys. Objects (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/20/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.objects_TSQL
- objects
- sys.objects
- objects_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.objects catalog view
- table-valued parameters, sys.objects catalog view
- user-defined table types [SQL Server]
- table types [SQL Server]
ms.assetid: f8d6163a-2474-410c-a794-997639f31b3b
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 92f2d9d61f1f04af35b9d0d5a8450de7c8cceebc
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171323"
---
# <a name="sysobjects-transact-sql"></a>sys.objects (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contiene una riga per ogni oggetto con ambito schema definito dall'utente creato all'interno di un database, inclusa la funzione scalare definita dall'utente compilata in modo nativo.  
  
 Per altre informazioni, vedere [Funzioni scalari definite dall'utente per OLTP in memoria](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md).  
  
> [!NOTE]  
>  sys.objects non visualizza trigger DDL. Questi oggetti, infatti, non sono definiti a livello di ambito di schema. Tutti i trigger, sia DML che DDL, sono disponibili in [sys. Triggers](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md). sys. triggers supporta una combinazione di regole per l'ambito dei nomi per i diversi tipi di trigger.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|name|**sysname**|Nome dell'oggetto.|  
|object_id|**int**|Numero di identificazione dell'oggetto. Valore univoco all'interno di un database.|  
|principal_id|**int**|ID del singolo proprietario, se diverso dal proprietario dello schema. Per impostazione predefinita, gli oggetti contenuti nello schema appartengono al proprietario dello schema stesso. È tuttavia possibile specificare un altro proprietario modificando la proprietà mediante l'istruzione ALTER AUTHORIZATION.<br /><br /> È NULL se non esiste un singolo proprietario alternativo.<br /><br /> È NULL se il tipo di oggetto è uno dei seguenti:<br /><br /> C = vincolo CHECK<br /><br /> D = DEFAULT (vincolo o valore autonomo)<br /><br /> F = vincolo FOREIGN KEY<br /><br /> PK = vincolo PRIMARY KEY<br /><br /> R = regola (tipo obsoleto, autonoma)<br /><br /> TA = trigger di assembly (integrazione con CLR)<br /><br /> TR = trigger SQL<br /><br /> UQ = vincolo UNIQUE<br /><br /> EC = vincolo Edge |  
|schema_id|**int**|ID dello schema che contiene l'oggetto.<br /><br /> Gli oggetti di sistema con ambito costituito dallo schema sono sempre inclusi negli schemi sys o INFORMATION_SCHEMA.|  
|parent_object_id|**int**|ID dell'oggetto a cui appartiene l'oggetto.<br /><br /> 0 = non è un oggetto figlio.|  
|tipo|**char(2)**|Tipo di oggetto:<br /><br /> AF = funzione di aggregazione (CLR)<br /><br /> C = vincolo CHECK<br /><br /> D = DEFAULT (vincolo o valore autonomo)<br /><br /> F = vincolo FOREIGN KEY<br /><br /> FN = funzione scalare SQL<br /><br /> FS = funzione scalare di assembly (CLR)<br /><br /> FT = funzione valutata a livello di tabella assembly (CLR)<br /><br /> IF = funzione SQL inline valutata a livello di tabella<br /><br /> IT = tabella interna<br /><br /> P = stored procedure SQL<br /><br /> PC = stored procedure di assembly (CLR)<br /><br /> PG = guida di piano<br /><br /> PK = vincolo PRIMARY KEY<br /><br /> R = regola (tipo obsoleto, autonoma)<br /><br /> RF = procedura-filtro-replica<br /><br /> S = tabella di base di sistema<br /><br /> SN = sinonimo<br /><br /> SO = oggetto sequenza<br /><br /> U = tabella (definita dall'utente)<br /><br /> V = vista<br /><br /> EC = vincolo Edge <br /><br /> <br /><br /> **Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> <br /><br /> SQ = coda di servizio<br /><br /> TA = trigger DML assembly (CLR)<br /><br /> TF = funzione valutata a livello di tabella SQL<br /><br /> TR = trigger DML SQL<br /><br /> TT = tipo tabella<br /><br /> UQ = vincolo UNIQUE<br /><br /> X = stored procedure estesa<br /><br /> <br /><br /> **Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] .<br /><br /> <br /><br /> ET = tabella esterna|  
|type_desc|**nvarchar(60)**|Descrizione del tipo di oggetto:<br /><br /> AGGREGATE_FUNCTION<br /><br /> CHECK_CONSTRAINT<br /><br /> CLR_SCALAR_FUNCTION<br /><br /> CLR_STORED_PROCEDURE<br /><br /> CLR_TABLE_VALUED_FUNCTION<br /><br /> CLR_TRIGGER<br /><br /> DEFAULT_CONSTRAINT<br /><br /> EXTENDED_STORED_PROCEDURE<br /><br /> FOREIGN_KEY_CONSTRAINT<br /><br /> INTERNAL_TABLE<br /><br /> PLAN_GUIDE<br /><br /> PRIMARY_KEY_CONSTRAINT<br /><br /> REPLICATION_FILTER_PROCEDURE<br /><br /> RULE<br /><br /> SEQUENCE_OBJECT<br /><br /> <br /><br /> **Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> <br /><br /> SERVICE_QUEUE<br /><br /> SQL_INLINE_TABLE_VALUED_FUNCTION<br /><br /> SQL_SCALAR_FUNCTION<br /><br /> SQL_STORED_PROCEDURE<br /><br /> SQL_TABLE_VALUED_FUNCTION<br /><br /> SQL_TRIGGER<br /><br /> SYNONYM<br /><br /> SYSTEM_TABLE<br /><br /> TABLE_TYPE<br /><br /> UNIQUE_CONSTRAINT<br /><br /> USER_TABLE<br /><br /> VIEW|  
|create_date|**datetime**|Data di creazione dell'oggetto.|  
|modify_date|**datetime**|Data dell'ultima modifica apportata all'oggetto mediante un'istruzione ALTER. Se l'oggetto è una tabella o una vista, modify_date cambia anche quando si crea o si modifica un indice nella tabella o nella vista.|  
|is_ms_shipped|**bit**|Oggetto creato da un componente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] interno.|  
|is_published|**bit**|L'oggetto viene pubblicato.|  
|is_schema_published|**bit**|Viene pubblicato solo lo schema dell'oggetto.|  
  
## <a name="remarks"></a>Commenti  
 È possibile applicare le funzioni predefinite [object_id](../../t-sql/functions/object-id-transact-sql.md), [object_name](../../t-sql/functions/object-name-transact-sql.md)e [OBJECTPROPERTY](../../t-sql/functions/objectproperty-transact-sql.md)() agli oggetti visualizzati in sys. Objects.  
  
 È disponibile una versione di questa visualizzazione con lo stesso schema, denominato [sys.system_objects](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md), che Mostra gli oggetti di sistema. È disponibile un'altra visualizzazione denominata [sys.all_objects](../../relational-databases/system-catalog-views/sys-all-objects-transact-sql.md) che Mostra gli oggetti di sistema e utente. Le tre viste del catalogo hanno tutte la stessa struttura.  
  
 In questa versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] un indice esteso, ad esempio un indice XML o un indice spaziale, è considerato una tabella interna in sys.objects (type = IT e type_desc = INTERNAL_TABLE). Per un indice esteso:  
  
-   name è il nome interno della tabella dell'indice.  
  
-   parent_object_id corrisponde a object_id della tabella di base.  
  
-   Le colonne is_ms_shipped, is_published e is_schema_published sono impostate su 0.  

**Viste di sistema utili correlate**  
I subset degli oggetti possono essere visualizzati utilizzando viste di sistema per un tipo specifico di oggetto, ad esempio:  
- [sys.tables](sys-tables-transact-sql.md)  
- [sys.views](sys-views-transact-sql.md)  
- [sys.procedures](sys-procedures-transact-sql.md)  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-returning-all-the-objects-that-have-been-modified-in-the-last-n-days"></a>R. Restituzione di tutti gli oggetti modificati negli ultimi N giorni  
 Prima di eseguire la query seguente, sostituire `<database_name>` e `<n_days>` con valori validi.  
  
```sql  
USE <database_name>;  
GO  
SELECT name AS object_name   
  ,SCHEMA_NAME(schema_id) AS schema_name  
  ,type_desc  
  ,create_date  
  ,modify_date  
FROM sys.objects  
WHERE modify_date > GETDATE() - <n_days>  
ORDER BY modify_date;  
GO  
```  
  
### <a name="b-returning-the-parameters-for-a-specified-stored-procedure-or-function"></a>B. Restituzione dei parametri per una stored procedure o una funzione specifica  
 Prima di eseguire la query seguente, sostituire `<database_name>` e `<schema_name.object_name>` con nomi validi.  
  
```sql  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(schema_id) AS schema_name  
    ,o.name AS object_name  
    ,o.type_desc  
    ,p.parameter_id  
    ,p.name AS parameter_name  
    ,TYPE_NAME(p.user_type_id) AS parameter_type  
    ,p.max_length  
    ,p.precision  
    ,p.scale  
    ,p.is_output  
FROM sys.objects AS o  
INNER JOIN sys.parameters AS p ON o.object_id = p.object_id  
WHERE o.object_id = OBJECT_ID('<schema_name.object_name>')  
ORDER BY schema_name, object_name, p.parameter_id;  
GO  
```  
  
### <a name="c-returning-all-the-user-defined-functions-in-a-database"></a>C. Restituzione di tutte le funzioni definite dall'utente in un database  
 Prima di eseguire la query seguente, sostituire `<database_name>` con un nome di database valido.  
  
```sql  
USE <database_name>;  
GO  
SELECT name AS function_name   
  ,SCHEMA_NAME(schema_id) AS schema_name  
  ,type_desc  
  ,create_date  
  ,modify_date  
FROM sys.objects  
WHERE type_desc LIKE '%FUNCTION%';  
GO  
```  
  
### <a name="d-returning-the-owner-of-each-object-in-a-schema"></a>D. Restituzione del proprietario di ogni oggetto in uno schema.  
 Prima di eseguire la query seguente, sostituire tutte le occorrenze di `<database_name>` e `<schema_name>` con nomi validi.  
  
```sql  
USE <database_name>;  
GO  
SELECT 'OBJECT' AS entity_type  
    ,USER_NAME(OBJECTPROPERTY(object_id, 'OwnerId')) AS owner_name  
    ,name   
FROM sys.objects WHERE SCHEMA_NAME(schema_id) = '<schema_name>'  
UNION   
SELECT 'TYPE' AS entity_type  
    ,USER_NAME(TYPEPROPERTY(SCHEMA_NAME(schema_id) + '.' + name, 'OwnerId')) AS owner_name  
    ,name   
FROM sys.types WHERE SCHEMA_NAME(schema_id) = '<schema_name>'   
UNION  
SELECT 'XML SCHEMA COLLECTION' AS entity_type   
    ,COALESCE(USER_NAME(xsc.principal_id),USER_NAME(s.principal_id)) AS owner_name  
    ,xsc.name   
FROM sys.xml_schema_collections AS xsc JOIN sys.schemas AS s  
    ON s.schema_id = xsc.schema_id  
WHERE s.name = '<schema_name>';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sys.all_objects &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-all-objects-transact-sql.md)   
 [sys.system_objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md)   
 [sys.triggers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md)   
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Domande frequenti sull'esecuzione di query sul catalogo di sistema SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [sys.internal_tables &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-internal-tables-transact-sql.md)  
  
  
