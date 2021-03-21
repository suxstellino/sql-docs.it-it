---
description: sys.sql_expression_dependencies (Transact-SQL)
title: sys.sql_expression_dependencies (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sql_expression_dependencies
- sql_expression_dependencies_TSQL
- sql_expression_dependencies
- sys.sql_expression_dependencies_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sql_expression_dependencies catalog view
ms.assetid: 78a218e4-bf99-4a6a-acbf-ff82425a5946
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 96eb8bb58aca14dc2ca3eaeb4c2fc1be5f066d2a
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750341"
---
# <a name="syssql_expression_dependencies-transact-sql"></a>sys.sql_expression_dependencies (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Contiene una riga per ogni dipendenza in base al nome in un'entità definita dall'utente nel database corrente. Sono incluse le dipendenze tra funzioni scalari definite dall'utente e compilate in modo nativo e altri [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] moduli. Una dipendenza tra due entità viene creata quando un'entità, denominata *entità a cui si fa riferimento*, viene visualizzata in base al nome in un'espressione SQL permanente di un'altra entità, detta *entità di riferimento*. Ad esempio, quando viene fatto riferimento a una tabella nella definizione di una vista, la vista, ovvero l'entità di riferimento, dipende dalla tabella, ovvero l'entità a cui si fa riferimento. Se la tabella viene eliminata, la vista non è utilizzabile.  
  
 Per altre informazioni, vedere [Funzioni scalari definite dall'utente per OLTP in memoria](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md).  
  
 È possibile utilizzare questa vista del catalogo per creare report relativi alle informazioni sulle dipendenze per le entità seguenti:  
  
-   Entità associate a schema.  
  
-   Entità non associate a schema.  
  
-   Entità tra database e tra server. I nomi delle entità sono indicati; tuttavia, gli ID dell'entità non sono risolti.  
  
-   Dipendenze a livello di colonna relative alle entità associate a schema. Le dipendenze a livello di colonna per gli oggetti non associati a schema possono essere restituite utilizzando [sys.dm_sql_referenced_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md).  
  
-   Trigger DDL a livello di server se nel contesto del database master.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|referencing_id|**int**|ID dell'entità di riferimento. Non ammette i valori Null.|  
|referencing_minor_id|**int**|ID di colonna quando l'entità di riferimento è una colonna, in caso contrario, 0. Non ammette i valori Null.|  
|referencing_class|**tinyint**|Classe dell'entità di riferimento.<br /><br /> 1 = Oggetto o colonna<br /><br /> 12 = Trigger DDL database<br /><br /> 13 = Trigger DDL server<br /><br /> Non ammette i valori Null.|  
|referencing_class_desc|**nvarchar(60)**|Descrizione della classe dell'entità di riferimento.<br /><br /> OBJECT_OR_COLUMN<br /><br /> DATABASE_DDL_TRIGGER<br /><br /> SERVER_DDL_TRIGGER<br /><br /> Non ammette i valori Null.|  
|is_schema_bound_reference|**bit**|1 = L'entità a cui si fa riferimento è associata a schema.<br /><br /> 0 = L'entità a cui si fa riferimento non è associata a schema.<br /><br /> Non ammette i valori Null.|  
|referenced_class|**tinyint**|Classe dell'entità con riferimenti.<br /><br /> 1 = Oggetto o colonna<br /><br /> 6 = Tipo<br /><br /> 10 = Raccolta di XML Schema<br /><br /> 21 = Funzione di partizione<br /><br /> Non ammette i valori Null.|  
|referenced_class_desc|**nvarchar(60)**|Descrizione della classe dell'entità a cui viene fatto riferimento.<br /><br /> OBJECT_OR_COLUMN<br /><br /> TYPE<br /><br /> XML_SCHEMA_COLLECTION<br /><br /> PARTITION_FUNCTION<br /><br /> Non ammette i valori Null.|  
|referenced_server_name|**sysname**|Nome del server dell'entità a cui viene fatto riferimento.<br /><br /> Questa colonna viene popolata per le dipendenze tra server eseguite specificando un nome valido composto da quattro parti. Per informazioni sui nomi multipart, vedere [convenzioni della sintassi Transact-sql &#40;&#41;Transact-SQL ](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).<br /><br /> Valore NULL per le entità non associate a schemi per le quali è stato fatto riferimento all'entità senza specificare un nome composto da quattro parti.<br /><br /> NULL per le entità associate a schema perché devono trovarsi nello stesso database e pertanto possono essere definite solo utilizzando un nome in due parti (*schema. Object*).|  
|referenced_database_name|**sysname**|Nome del database dell'entità a cui viene fatto riferimento.<br /><br /> Questa colonna viene popolata per i riferimenti tra database o tra server eseguiti specificando un nome valido composto da tre o quattro parti.<br /><br /> Valore NULL per i riferimenti non associati a schemi che vengono specificati utilizzando un nome composto da una o due parti.<br /><br /> NULL per le entità associate a schema perché devono trovarsi nello stesso database e pertanto possono essere definite solo utilizzando un nome in due parti (*schema. Object*).|  
|referenced_schema_name|**sysname**|Schema a cui appartiene l'entità a cui viene fatto riferimento.<br /><br /> Valore NULL per i riferimenti non associati a schemi in cui è stato fatto riferimento all'entità senza specificare il nome dello schema.<br /><br /> Non è mai un valore NULL per i riferimenti associati a schemi perché le entità associate a schemi devono essere definite e riferite utilizzando un nome composto da due parti.|  
|referenced_entity_name|**sysname**|Nome dell'entità a cui viene fatto riferimento. Non ammette i valori Null.|  
|referenced_id|**int**|ID dell'entità a cui viene fatto riferimento. Il valore di questa colonna non è mai NULL per i riferimenti associati a schema. Il valore di questa colonna è sempre NULL per i riferimenti tra server e tra database.<br /><br /> Valore NULL per i riferimenti all'interno del database se non è possibile determinare l'ID. Per i riferimenti non associati a schemi, non è possibile risolvere l'ID nei casi seguenti:<br /><br /> L'entità con riferimenti non esiste nel database.<br /><br /> Lo schema dell'entità a cui si fa riferimento dipende dallo schema del chiamante e viene risolto in fase di esecuzione. In questo caso, is_caller_dependent viene impostato su 1.|  
|referenced_minor_id|**int**|ID della colonna con riferimenti quando l'entità di riferimento è una colonna; in caso contrario il valore è 0. Non ammette i valori Null.<br /><br /> Un'entità a cui viene fatto riferimento è una colonna, se il nome nell'entità di riferimento identifica una colonna o se l'entità padre viene utilizzata in un'istruzione SELECT *.|  
|is_caller_dependent|**bit**|Indica che l'associazione di schemi per l'entità con riferimenti si verifica in fase di esecuzione; pertanto, la risoluzione dell'ID dell'entità dipende dallo schema del chiamante. Questo avviene quando l'entità con riferimenti è una stored procedure, una stored procedure estesa o una funzione definita dall'utente non associata a schema chiamata all'interno di un'istruzione EXECUTE.<br /><br /> 1 = L'entità con riferimenti è dipendente dal chiamante e viene risolta in fase di esecuzione. In questo caso, il valore di referenced_id è NULL.<br /><br /> 0 = L'ID dell'entità a cui viene fatto riferimento non è dipendente dal chiamante.<br /><br /> Il valore è sempre 0 per i riferimenti associati a schemi e tra database e tra server che indicano in modo esplicito un nome schema. Ad esempio, un riferimento a un'entità nel formato `EXEC MyDatabase.MySchema.MyProc` non è dipendente dal chiamante. Tuttavia, un riferimento nel formato `EXEC MyDatabase..MyProc` è dipendente dal chiamante.|  
|is_ambiguous|**bit**|Indica che il riferimento è ambiguo e può essere risolto in fase di esecuzione in una funzione definita dall'utente, in un tipo definito dall'utente (UDT) o in un riferimento XQuery a una colonna di tipo **XML**.<br /><br /> Ad esempio, si supponga che l'istruzione `SELECT Sales.GetOrder() FROM Sales.MySales` sia stata definita in una stored procedure. Durante l'esecuzione della stored procedure non è possibile sapere se `Sales.GetOrder()` è una funzione definita dall'utente nello schema `Sales` o una colonna `Sales` di tipo definito dall'utente con un metodo `GetOrder()`.<br /><br /> 1 = Il riferimento è ambiguo.<br /><br /> 0 = Il riferimento non è ambiguo o l'entità può essere associata correttamente quando la vista viene chiamata.<br /><br /> Il valore è sempre 0 per i riferimenti associati a schema.|  
  
## <a name="remarks"></a>Commenti  
 Nella tabella seguente sono elencati i tipi di entità per i quali vengono create e gestite le informazioni sulle dipendenze. Le informazioni sulle dipendenze non vengono create né gestite per regole, impostazioni predefinite, tabelle temporanee, stored procedure temporanee o oggetti di sistema.  

> [!NOTE]
> Le tabelle, le visualizzazioni, le statistiche filtrate e i tipi di entità di stored procedure Transact-SQL di Azure sinapsi Analytics e Parallel data warehouse supportano da questo elenco.  Le informazioni sulle dipendenze vengono create e gestite solo per tabelle, viste e statistiche filtrate.  
  
|Tipo di entità|Entità di riferimento|Entità con riferimenti|  
|-----------------|------------------------|-----------------------|  
|Tabella|Sì*|Sì|  
|Visualizzazione|Sì|Sì|  
|Indice filtrato|Sì**|No|  
|Statistiche filtrate|Sì**|No|  
|Stored procedure*** [!INCLUDE[tsql](../../includes/tsql-md.md)]|Sì|Sì|  
|stored procedure CLR|No|Sì|  
|Funzione [!INCLUDE[tsql](../../includes/tsql-md.md)] definita dall'utente|Sì|Sì|  
|Funzione CLR definita dall'utente|No|Sì|  
|Trigger CLR (DML e DDL)|No|No|  
|Trigger DML [!INCLUDE[tsql](../../includes/tsql-md.md)]|Sì|No|  
|Trigger DDL [!INCLUDE[tsql](../../includes/tsql-md.md)] a livello di database|Sì|No|  
|Trigger DDL [!INCLUDE[tsql](../../includes/tsql-md.md)] a livello di server|Sì|No|  
|Stored procedure estese|No|Sì|  
|Coda|No|Sì|  
|Sinonimo|No|Sì|  
|Tipo (alias e tipo di CLR definito dall'utente)|No|Sì|  
|Raccolta di XML Schema|No|Sì|  
|Funzione di partizione|No|Sì|  
  
 \* Una tabella viene rilevata come entità di riferimento solo quando fa riferimento a un [!INCLUDE[tsql](../../includes/tsql-md.md)] modulo, a un tipo definito dall'utente o a una raccolta di XML Schema nella definizione di una colonna calcolata, un vincolo check o un vincolo Default.  
  
 ** Ogni colonna utilizzata nel predicato del filtro viene registrata come un'entità di riferimento.  
  
 *** Le stored procedure numerate con un valore intero maggiore di 1 non vengono rilevate come entità di riferimento o entità a cui si fa riferimento.  
  
## <a name="permissions"></a>Autorizzazioni  
 Sono richieste l'autorizzazione VIEW DEFINITION sul database e l'autorizzazione SELECT su sys.sql_expression_dependencies per il database. L'autorizzazione SELECT è concessa per impostazione predefinita solo ai membri del ruolo predefinito del database di db_owner. Quando le autorizzazioni SELECT e VIEW DEFINITION vengono concesse a un altro utente, l'utente autorizzato può visualizzare tutte le dipendenze nel database.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-returning-entities-that-are-referenced-by-another-entity"></a>R. Restituzione di entità con riferimenti da un'altra entità  
 Nell'esempio seguente sono restituite le tabelle e colonne con riferimenti nella vista `Production.vProductAndDescription`. La vista dipende dalle entità (tabelle e colonne) restituite nelle colonne `referenced_entity_name` e `referenced_column_name`.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT OBJECT_NAME(referencing_id) AS referencing_entity_name,   
    o.type_desc AS referencing_desciption,   
    COALESCE(COL_NAME(referencing_id, referencing_minor_id), '(n/a)') AS referencing_minor_id,   
    referencing_class_desc,  
    referenced_server_name, referenced_database_name, referenced_schema_name,  
    referenced_entity_name,   
    COALESCE(COL_NAME(referenced_id, referenced_minor_id), '(n/a)') AS referenced_column_name,  
    is_caller_dependent, is_ambiguous  
FROM sys.sql_expression_dependencies AS sed  
INNER JOIN sys.objects AS o ON sed.referencing_id = o.object_id  
WHERE referencing_id = OBJECT_ID(N'Production.vProductAndDescription');  
GO  
  
```  
  
### <a name="b-returning-entities-that-reference-another-entity"></a>B. Restituzione di entità che fanno riferimento a un'altra entità  
 Nell'esempio seguente vengono restituite le entità che fanno riferimento alla tabella `Production.Product`. Le entità restituite nella colonna `referencing_entity_name` dipendono dalla tabella `Product`.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT OBJECT_SCHEMA_NAME ( referencing_id ) AS referencing_schema_name,  
    OBJECT_NAME(referencing_id) AS referencing_entity_name,   
    o.type_desc AS referencing_desciption,   
    COALESCE(COL_NAME(referencing_id, referencing_minor_id), '(n/a)') AS referencing_minor_id,   
    referencing_class_desc, referenced_class_desc,  
    referenced_server_name, referenced_database_name, referenced_schema_name,  
    referenced_entity_name,   
    COALESCE(COL_NAME(referenced_id, referenced_minor_id), '(n/a)') AS referenced_column_name,  
    is_caller_dependent, is_ambiguous  
FROM sys.sql_expression_dependencies AS sed  
INNER JOIN sys.objects AS o ON sed.referencing_id = o.object_id  
WHERE referenced_id = OBJECT_ID(N'Production.Product');  
GO  
  
```  
  
### <a name="c-returning-cross-database-dependencies"></a>C. Restituzione di dipendenze tra database  
 Nell'esempio seguente vengono restituite tutte le dipendenze tra database. L'esempio prima crea il database `db1` e due stored procedure che fanno riferimento alle tabelle nei database `db2` e `db3`. Sulla tabella `sys.sql_expression_dependencies` viene quindi eseguita una query per riportare le dipendenze tra database tra le procedure e le tabelle. Notare che NULL è restituito nella colonna `referenced_schema_name` per l'entità `t3` a cui si fa riferimento perché per l'entità non è stato specificato un nome nella definizione della procedura.  
  
```  
CREATE DATABASE db1;  
GO  
USE db1;  
GO  
CREATE PROCEDURE p1 AS SELECT * FROM db2.s1.t1;  
GO  
CREATE PROCEDURE p2 AS  
    UPDATE db3..t3  
    SET c1 = c1 + 1;  
GO  
SELECT OBJECT_NAME (referencing_id),referenced_database_name,   
    referenced_schema_name, referenced_entity_name  
FROM sys.sql_expression_dependencies  
WHERE referenced_database_name IS NOT NULL;  
GO  
USE master;  
GO  
DROP DATABASE db1;  
GO  
  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sys.dm_sql_referenced_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)   
 [sys.dm_sql_referencing_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)  
  
  
