---
description: sys.dm_sql_referencing_entities (Transact-SQL)
title: sys.dm_sql_referencing_entities (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_sql_referencing_entities
- dm_sql_referencing_entities_TSQL
- sys.dm_sql_referencing_entities_TSQL
- dm_sql_referencing_entities
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_sql_referencing_entities dynamic management function
ms.assetid: c16f8f0a-483f-4feb-842e-da90426045ae
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 085ce7551097feee4fdbc9b6ec56fa530b955701
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99133861"
---
# <a name="sysdm_sql_referencing_entities-transact-sql"></a>sys.dm_sql_referencing_entities (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce una riga per ogni entità nel database corrente che fa riferimento a un'altra entità definita dall'utente in base al nome. Una dipendenza tra due entità viene creata quando un'entità, denominata *entità a cui si fa riferimento*, viene visualizzata in base al nome in un'espressione SQL permanente di un'altra entità, detta *entità di riferimento*. Ad esempio, se un tipo definito dall'utente (UDT) è specificato come entità con riferimenti, questa funzione restituisce ogni entità definita dall'utente che nella propria definizione fa riferimento a quel tipo in base al nome. La funzione non restituisce entità negli altri database che possono fare riferimento all'entità specificata. Questa funzione deve essere eseguita nel contesto del database master perché restituisca un trigger DDL a livello di server come entità di riferimento.  
  
 È possibile usare questa funzione a gestione dinamica per creare un report sui seguenti tipi di entità del database corrente che fanno riferimento all'entità specificata:  
  
-   Entità associate o non associate a schema  
  
-   Trigger DDL a livello di database  
  
-   Trigger DDL a livello di server  
  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive), [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
sys.dm_sql_referencing_entities (  
    ' schema_name.referenced_entity_name ' , ' <referenced_class> ' )  
  
<referenced_class> ::=  
{  
    OBJECT  
  | TYPE  
  | XML_SCHEMA_COLLECTION  
  | PARTITION_FUNCTION  
}  
```  
  
## <a name="arguments"></a>Argomenti  
 `schema_name.referenced_entity_name` Nome dell'entità a cui si fa riferimento.  
  
 `schema_name` è obbligatorio, eccetto quando la classe a cui si fa riferimento è PARTITION_FUNCTION.  
  
 `schema_name.referenced_entity_name` è di **tipo nvarchar (517)**.  
  
 `<referenced_class> ::= { OBJECT  | TYPE | XML_SCHEMA_COLLECTION | PARTITION_FUNCTION }` È la classe dell'entità a cui si fa riferimento. È possibile specificare solo una classe per istruzione.  
  
 `<referenced_class>` è di **tipo nvarchar**(60).  
  
## <a name="table-returned"></a>Tabella restituita  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|referencing_schema_name|**sysname**|Schema a cui appartiene l'entità di riferimento. Ammette i valori Null.<br /><br /> NULL per trigger DDL a livello di database e a livello di server.|  
|referencing_entity_name|**sysname**|Nome dell'entità di riferimento. Non ammette i valori Null.|  
|referencing_id|**int**|ID dell'entità di riferimento. Non ammette i valori Null.|  
|referencing_class|**tinyint**|Classe dell'entità di riferimento. Non ammette i valori Null.<br /><br /> 1 = Oggetto<br /><br /> 12 = Trigger DDL a livello di database<br /><br /> 13 = Trigger DDL a livello di server|  
|referencing_class_desc|**nvarchar(60)**|Descrizione della classe dell'entità di riferimento.<br /><br /> OBJECT<br /><br /> DATABASE_DDL_TRIGGER<br /><br /> SERVER_DDL_TRIGGER|  
|is_caller_dependent|**bit**|Indica che la risoluzione dell'ID dell'entità a cui si fa riferimento si verifica in fase di esecuzione poiché dipende dallo schema del chiamante.<br /><br /> 1 = L'entità di riferimento ha la possibilità di fare riferimento all'entità, tuttavia la risoluzione dell'ID dell'entità a cui si fa riferimento è dipendente dal chiamante e non può essere determinata. Ciò avviene solo per riferimenti a stored procedure non associati a schema, stored procedure estese o funzioni definite dall'utente chiamate all'interno di un'istruzione EXECUTE.<br /><br /> 0 = L'entità a cui si fa riferimento non è dipendente dal chiamante.|  
  
## <a name="exceptions"></a>Eccezioni  
 In una delle seguenti condizioni, restituisce un set di risultati vuoto:  
  
-   Viene specificato un oggetto di sistema.  
  
-   L'entità specificata non è presente nel database corrente.  
  
-   L'entità specificata non fa riferimento ad alcuna entità.  
  
-   Viene passato un parametro non valido.  
  
 Restituisce un errore quando l'entità a cui si fa riferimento specificata è una stored procedure numerata.  
  
## <a name="remarks"></a>Commenti  
 Nella tabella seguente sono elencati i tipi di entità per i quali vengono create e gestite le informazioni sulle dipendenze. Le informazioni sulle dipendenze non vengono create né gestite per regole, impostazioni predefinite, tabelle temporanee, stored procedure temporanee o oggetti di sistema.  
  
|Tipo di entità|Entità di riferimento|Entità con riferimenti|  
|-----------------|------------------------|-----------------------|  
|Tabella|Sì*|Sì|  
|Visualizzazione|Sì|Sì|  
|Stored procedure [!INCLUDE[tsql](../../includes/tsql-md.md)]**|Sì|Sì|  
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
  
 ** Le stored procedure numerate con un valore intero maggiore di 1 non vengono registrate come entità di riferimento o a cui viene fatto riferimento.  
  
## <a name="permissions"></a>Autorizzazioni  
  
### <a name="sskatmai---sssql11"></a>[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] - [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
-   È richiesta l'autorizzazione CONTROL per l'oggetto a cui viene fatto riferimento. Quando l'entità a cui si fa riferimento è una funzione di partizione, è necessaria l'autorizzazione CONTROL per il database.  
  
-   È richiesta l'autorizzazione SELECT per sys.dm_sql_referencing_entities. Per impostazione predefinita, l'autorizzazione SELECT è concessa al ruolo public.  
  
### <a name="sssql14---sscurrent"></a>[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] - [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]  
  
-   Non sono richieste autorizzazioni per l'oggetto a cui viene fatto riferimento. I risultati parziali possono essere restituiti se l'utente dispone di VIEW DEFINITION solo per alcune entità di riferimento.  
  
-   È richiesta l'autorizzazione VIEW DEFINITION per l'oggetto quando l'entità di riferimento è un oggetto.  
  
-   È richiesta l'autorizzazione VIEW ANY DEFINITION per il database quando l'entità di riferimento è un trigger DDL a livello di database.  
  
-   È richiesta l'autorizzazione VIEW ANY DEFINITION per il server quando l'entità di riferimento è un trigger DDL a livello di server.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-returning-the-entities-that-refer-to-a-given-entity"></a>R. Restituzione delle entità che fanno riferimento a un'entità specificata  
 Nell'esempio seguente vengono restituite le entità nel database corrente che fanno riferimento alla tabella specificata.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT referencing_schema_name, referencing_entity_name, referencing_id, referencing_class_desc, is_caller_dependent  
FROM sys.dm_sql_referencing_entities ('Production.Product', 'OBJECT');  
GO  
```  
  
### <a name="b-returning-the-entities-that-refer-to-a-given-type"></a>B. Restituzione delle entità che fanno riferimento a un tipo specificato  
 Nell'esempio seguente vengono restituite le entità che fanno riferimento al tipo alias `dbo.Flag`. Il set di risultati mostra che questo tipo è usato da due stored procedure. Il `dbo.Flag` tipo viene inoltre utilizzato nella definizione di diverse colonne della `HumanResources.Employee` tabella. Tuttavia, poiché il tipo non è presente nella definizione di una colonna calcolata, di un vincolo check o di un vincolo default nella tabella, non viene restituita alcuna riga per la `HumanResources.Employee` tabella.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT referencing_schema_name, referencing_entity_name, referencing_id, referencing_class_desc, is_caller_dependent  
FROM sys.dm_sql_referencing_entities ('dbo.Flag', 'TYPE');  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 referencing_schema_name referencing_entity_name   referencing_id referencing_class_desc is_caller_dependent  
 ----------------------- -------------------------  ------------- ---------------------- -------------------  
 HumanResources          uspUpdateEmployeeHireInfo  1803153469    OBJECT_OR_COLUMN       0  
 HumanResources          uspUpdateEmployeeLogin     1819153526    OBJECT_OR_COLUMN       0  
 (2 row(s) affected)`  
 ``` 
 
## <a name="see-also"></a>Vedere anche  
 [sys.dm_sql_referenced_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)   
 [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)  
  
  
