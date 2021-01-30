---
description: sp_tables_ex (Transact-SQL)
title: sp_tables_ex (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_tables_ex
- sp_tables_ex_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_tables_ex
ms.assetid: 33755c33-7e1e-4ef7-af14-a9cebb1e2ed4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0126cfd14adde25d88d6990a5d7e78c2141ea21e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182767"
---
# <a name="sp_tables_ex-transact-sql"></a>sp_tables_ex (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni relative alle tabelle del server collegato specificato.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_tables_ex [ @table_server = ] 'table_server'   
     [ , [ @table_name = ] 'table_name' ]   
     [ , [ @table_schema = ] 'table_schema' ]  
     [ , [ @table_catalog = ] 'table_catalog' ]   
     [ , [ @table_type = ] 'table_type' ]   
     [ , [@fUsePattern = ] 'fUsePattern' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @table_server = ] 'table_server'` Nome del server collegato per cui si desidera ottenere informazioni sulla tabella. *table_server* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
``[ , [ @table_name = ] 'table_name']`` Nome della tabella per cui si desidera ottenere informazioni sul tipo di dati. *table_name* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @table_schema = ] 'table_schema']` Schema della tabella. *TABLE_SCHEMA* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @table_catalog = ] 'table_catalog'` Nome del database in cui risiede il *table_name* specificato. *TABLE_CATALOG* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @table_type = ] 'table_type'` Tipo della tabella da restituire. *TABLE_TYPE* è di **tipo sysname** e il valore predefinito è null. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**ALIAS**|Nome di un alias.|  
|**GLOBAL TEMPORARY**|Nome di una tabella temporanea disponibile nell'intero sistema.|  
|**LOCAL TEMPORARY**|Nome di una tabella temporanea disponibile solo nel processo corrente.|  
|**SYNONYM**|Nome di un sinonimo.|  
|**TABELLA DI SISTEMA**|Nome di una tabella di sistema.|  
|**VISUALIZZAZIONE DI SISTEMA**|Nome di una vista di sistema.|  
|**TABLE**|Nome di una tabella utente.|  
|**VIEW**|Nome di una vista.|  
  
`[ @fUsePattern = ] 'fUsePattern'` Determina se i caratteri **_**, **%** , **[** e **]** sono interpretati come caratteri jolly. I valori validi sono 0 (utilizzo dei criteri di ricerca disattivato) e 1 (utilizzo dei criteri di ricerca attivato). *fUsePattern* è di **bit** e il valore predefinito è 1.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Nessuno  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**TABLE_CAT**|**sysname**|Nome del qualificatore della tabella. Vari prodotti DBMS supportano la denominazione in tre parti per le tabelle (_qualificatore_**.** _proprietario_**.** _nome_). In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questa colonna rappresenta il nome del database. In altri prodotti rappresenta il nome del server dell'ambiente di database della tabella. Questo campo può essere NULL.|  
|**TABLE_SCHEM**|**sysname**|Nome del proprietario della tabella. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questa colonna rappresenta il nome dell'utente del database che ha creato la tabella. Questo campo restituisce sempre un valore.|  
|**TABLE_NAME**|**sysname**|Nome della tabella. Questo campo restituisce sempre un valore.|  
|**TABLE_TYPE**|**varchar(32)**|Tabella, tabella di sistema o vista.|  
|**COMMENTI**|**varchar (254)**|In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non viene restituito alcun valore per questa colonna.|  
  
## <a name="remarks"></a>Commenti  
 **sp_tables_ex** viene eseguita eseguendo una query sul set di righe Tables dell'interfaccia **IDBSchemaRowset** del provider OLE DB corrispondente a *table_server*. I parametri *table_name*, *TABLE_SCHEMA*, *TABLE_CATALOG* e *Column* vengono passati a questa interfaccia per limitare le righe restituite.  
  
 **sp_tables_ex** restituisce un set di risultati vuoto se il provider di OLE DB del server collegato specificato non supporta il set di righe Tables dell'interfaccia **IDBSchemaRowset** .  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione SELECT per lo schema.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituite informazioni sulle tabelle contenute nello schema `HumanResources` del database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] nel server collegato `LONDON2`.  
  
```  
EXEC sp_tables_ex @table_server = 'LONDON2',   
@table_catalog = 'AdventureWorks2012',   
@table_schema = 'HumanResources',   
@table_type = 'TABLE';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure per query distribuite &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/distributed-queries-stored-procedures-transact-sql.md)   
 [sp_catalogs &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-catalogs-transact-sql.md)   
 [sp_columns_ex &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-columns-ex-transact-sql.md)   
 [sp_column_privileges &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-column-privileges-transact-sql.md)   
 [sp_foreignkeys &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-foreignkeys-transact-sql.md)   
 [sp_indexes &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-indexes-transact-sql.md)   
 [sp_linkedservers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-linkedservers-transact-sql.md)   
 [sp_table_privileges &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-table-privileges-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
