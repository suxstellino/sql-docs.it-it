---
description: sys.fn_listextendedproperty (Transact-SQL)
title: sys.fn_listextendedproperty (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_listextendedproperty
- fn_listextendedproperty_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fn_listextendedproperty function
- displaying extended properties
- database extended properties [SQL Server]
- viewing extended properties
- column extended properties [SQL Server]
- sys.fn_listextendedproperties function
- database objects [SQL Server], extended properties
- extended properties [SQL Server], columns
- table extended properties [SQL Server]
ms.assetid: 59bbb91f-a277-4a35-803e-dcb91e847a49
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e8b60e2fabcccd2040c3dceb1307b7f7e7907bda
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201936"
---
# <a name="sysfn_listextendedproperty-transact-sql"></a>sys.fn_listextendedproperty (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce i valori delle proprietà estese degli oggetti di database.  
 
 
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
fn_listextendedproperty (   
    { default | 'property_name' | NULL }   
  , { default | 'level0_object_type' | NULL }   
  , { default | 'level0_object_name' | NULL }   
  , { default | 'level1_object_type' | NULL }   
  , { default | 'level1_object_name' | NULL }   
  , { default | 'level2_object_type' | NULL }   
  , { default | 'level2_object_name' | NULL }   
  )   
```  
  
## <a name="arguments"></a>Argomenti  
 {impostazione predefinita | '*property_name*' | NULL  
 Nome della proprietà. *property_name* è di **tipo sysname**. I possibili valori sono default, NULL o un nome di proprietà.  
  
 {impostazione predefinita | '*level0_object_type*' | NULL  
 Utente o tipo definito dall'utente. *level0_object_type* è di tipo **varchar (128)** e il valore predefinito è null. I possibili valori sono ASSEMBLY, CONTRACT, EVENT NOTIFICATION, FILEGROUP, MESSAGE TYPE, PARTITION FUNCTION, PARTITION SCHEME, REMOTE SERVICE BINDING, ROUTE, SCHEMA, SERVICE, TRIGGER, TYPE, USER e NULL.  
  
> [!IMPORTANT]  
>  I tipi USER e TYPE come tipi di livello 0 verranno rimossi in una versione futura di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evitare pertanto di utilizzarle in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui sono state implementate. Utilizzare SCHEMA come tipo di livello 0 anziché USER. Per TYPE utilizzare SCHEMA come tipo di livello 0 e TYPE come tipo di livello 1.  
  
 {impostazione predefinita | '*level0_object_name*' | NULL  
 Nome del tipo di oggetto di livello 0 specificato. *level0_object_name* è di **tipo sysname** e il valore predefinito è null. I possibili valori sono default, NULL o un nome di oggetto.  
  
 {impostazione predefinita | '*level1_object_type*' | NULL  
 Tipo di oggetto di livello 1. *level1_object_type* è di tipo **varchar (128)** e il valore predefinito è null. I possibili valori sono AGGREGATE, DEFAULT, FUNCTION, LOGICAL FILE NAME, PROCEDURE, QUEUE, RULE, SYNONYM, TABLE, TYPE, VIEW, XML SCHEMA COLLECTION e NULL.  
  
> [!NOTE]  
>  Per il valore predefinito viene eseguito il mapping a NULL e per il parametro 'default' viene eseguito il mapping al tipo di oggetto DEFAULT.  
  
 {impostazione predefinita | '*level1_object_name*' | NULL  
 Nome del tipo di oggetto di livello 1 specificato. *level1_object_name* è di **tipo sysname** e il valore predefinito è null. I possibili valori sono default, NULL o un nome di oggetto.  
  
 {impostazione predefinita | '*level2_object_type*' | NULL  
 Tipo di oggetto di livello 2. *level2_object_type* è di tipo **varchar (128)** e il valore predefinito è null. I possibili valori sono DEFAULT, default (con mapping a NULL) e NULL. Gli input validi per *level2_object_type* sono Column, Constraint, Event Notification, index, Parameter, trigger e null.  
  
 {impostazione predefinita | '*level2_object_name*' | NULL  
 Nome del tipo di oggetto di livello 2 specificato. *level2_object_name* è di **tipo sysname** e il valore predefinito è null. I possibili valori sono default, NULL o un nome di oggetto.  
  
## <a name="tables-returned"></a>Tabelle restituite  
 Il formato delle tabelle restituite da fn_listextendedproperty è il seguente.  
  
|Nome colonna|Tipo di dati|  
|-----------------|---------------|  
|objtype|**sysname**|  
|objname|**sysname**|  
|name|**sysname**|  
|Valore|**sql_variant**|  
  
 Se la tabella restituita è vuota, significa che all'oggetto non sono associate proprietà estese o che l'utente non è autorizzato a elencare le proprietà estese dell'oggetto. In caso di restituzione di proprietà estese per il database, le colonne objtype e objname saranno NULL.  
  
## <a name="remarks"></a>Commenti  
 Se il valore per *property_name* è null o predefinito, fn_listextendedproperty restituisce tutte le proprietà per l'oggetto specificato.  
  
 Se è specificato il tipo di oggetto e il valore del nome oggetto corrispondente è NULL o default, fn_listextendedproperty restituisce tutte le proprietà estese di tutti gli oggetti del tipo specificato.  
  
 Gli oggetti si differenziano in base al livello compreso tra 0, il livello superiore, e 2, il livello inferiore. Se si specificano il nome e il tipo di un oggetto di livello inferiore (1 o 2), per il tipo e il nome dell'oggetto padre è necessario assegnare valori diversi da NULL o dal valore predefinito. In caso contrario, la funzione restituisce un set di risultati vuoto.  
  
 **ObjName** è fisso come Latin1_General_CI_AI. Tuttavia, è possibile aggirare questo problema eseguendo l'override delle regole di confronto.  
  
```  
SELECT o.[object_id] AS 'table_id', o.[name] 'table_name',  
0 AS 'column_order', NULL AS 'column_name', NULL AS 'column_datatype',  
NULL AS 'column_length', Cast(e.value AS varchar(500)) AS 'column_description'  
FROM AdventureWorks.sys.objects AS o  
LEFT JOIN sys.fn_listextendedproperty(N'MS_Description', N'user',N'HumanResources',N'table', N'Employee', null, default) AS e  
    ON o.name = e.objname COLLATE SQL_Latin1_General_CP1_CI_AS  
WHERE o.name = 'Employee';  
```  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni per elencare le proprietà estese degli oggetti variano in base al tipo di oggetto.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-displaying-extended-properties-on-a-database"></a>R. Visualizzazione delle proprietà estese in un database  
 Nell'esempio seguente vengono visualizzate tutte le proprietà estese impostate in un oggetto di database.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT objtype, objname, name, value  
FROM fn_listextendedproperty(default, default, default, default, default, default, default);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `objtype    objname     name            value`  
  
 `---------  ---------   -----------     ----------------------------`  
  
 `NULL       NULL        MS_Description  AdventureWorks2008 Sample OLTP Database`  
  
 `(1 row(s) affected)`  
  
### <a name="b-displaying-extended-properties-on-all-columns-in-a-table"></a>B. Visualizzazione delle proprietà estese in tutte le colonne di una tabella  
 Nell'esempio seguente vengono elencate le proprietà estese per le colonne nella `ScrapReason` tabella. inclusa nello schema `Production`.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT objtype, objname, name, value  
FROM fn_listextendedproperty (NULL, 'schema', 'Production', 'table', 'ScrapReason', 'column', default);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `objtype objname      name            value`  
  
 `------- -----------  -------------   ------------------------`  
  
 `COLUMN ScrapReasonID MS_Description  Primary key for ScrapReason records.`  
  
 `COLUMN Name          MS_Description  Failure description.`  
  
 `COLUMN ModifiedDate  MS_Description  Date the record was last updated.`  
  
 `(3 row(s) affected)`  
  
### <a name="c-displaying-extended-properties-on-all-tables-in-a-schema"></a>C. Visualizzazione delle proprietà estese in tutte le tabelle incluse in uno schema  
 Nell'esempio seguente vengono elencate le proprietà estese per tutte le tabelle contenute nello `Sales` schema.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT objtype, objname, name, value  
FROM fn_listextendedproperty (NULL, 'schema', 'Sales', 'table', default, NULL, NULL);  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_addextendedproperty &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql.md)   
 [sp_dropextendedproperty &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropextendedproperty-transact-sql.md)   
 [sp_updateextendedproperty &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-updateextendedproperty-transact-sql.md)   
 [sys.extended_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/extended-properties-catalog-views-sys-extended-properties.md)  
  
  
