---
description: sp_indexes (Transact-SQL)
title: sp_indexes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_indexes_TSQL
- sp_indexes
dev_langs:
- TSQL
helpviewer_keywords:
- sp_indexes
ms.assetid: 25469e72-9d95-463f-912a-193471c8f5e2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f11b9a32c05450dbe97c9130d774e4d92226fcdb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198508"
---
# <a name="sp_indexes-transact-sql"></a>sp_indexes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni sugli indici per la tabella remota specificata.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_indexes [ @table_server = ] 'table_server'   
     [ , [ @table_name = ] 'table_name' ]   
     [ , [ @table_schema = ] 'table_schema' ]   
     [ , [ @table_catalog = ] 'table_db' ]   
     [ , [ @index_name = ] 'index_name' ]   
     [ , [ @is_unique = ] 'is_unique' ]  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @table_server =]'*table_server*'  
 Nome di un server collegato [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di cui vengono richieste informazioni di tabella. *table_server* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
 [ @table_name =]'*table_name*'  
 Nome della tabella remota per cui si desidera ottenere le informazioni di indice. *table_name* è di **tipo sysname** e il valore predefinito è null. con cui vengono restituite tutte le tabelle del database specificato.  
  
 [ @table_schema =]'*TABLE_SCHEMA*'  
 Specifica lo schema di tabella. In ambiente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] corrisponde al proprietario della tabella. *TABLE_SCHEMA* è di **tipo sysname** e il valore predefinito è null.  
  
 [ @table_catalog =]'*table_db*'  
 Nome del database in cui risiede *table_name* . *table_db* è di **tipo sysname** e il valore predefinito è null. Se è NULL, il valore predefinito di *table_db* è **Master**.  
  
 [ @index_name =]'*index_name*'  
 Nome dell'indice per cui si desidera ottenere informazioni. *index* è di **tipo sysname** e il valore predefinito è null.  
  
 [ @is_unique =]'*is_unique*'  
 Tipo di indice per cui si desidera ottenere informazioni. *is_unique* è di **bit** e il valore predefinito è null. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|1|Restituisce informazioni sugli indici univoci.|  
|0|Restituisce informazioni sugli indici non univoci.|  
|NULL|Restituisce informazioni su tutti gli indici.|  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|TABLE_CAT|**sysname**|Nome del database contenente la tabella specificata.|  
|TABLE_SCHEM|**sysname**|Schema della tabella.|  
|TABLE_NAME|**sysname**|Nome della tabella remota.|  
|NON_UNIQUE|**smallint**|Indica se l'indice è o meno univoco:<br /><br /> 0 = Univoco<br /><br /> 1 = Non univoco|  
|INDEX_QUALIFER|**sysname**|Nome del proprietario dell'indice. In alcuni prodotti DBMS gli indici possono essere creati da utenti diversi dal proprietario della tabella. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questa colonna è sempre uguale a **table_name**.|  
|INDEX_NAME|**sysname**|Nome dell'indice.|  
|TYPE|**smallint**|Tipo di indice:<br /><br /> 0 = Statistiche di una tabella<br /><br /> 1 = Cluster<br /><br /> 2 = Hash<br /><br /> 3 = altro|  
|ORDINAL_POSITION|**int**|Posizione ordinale della colonna nell'indice. La prima colonna nell'indice è 1. In questa colonna viene sempre restituito un valore.|  
|COLUMN_NAME|**sysname**|Nome corrispondente alle colonne di TABLE_NAME restituite.|  
|ASC_OR_DESC|**varchar**|Ordine adottato nelle regole di confronto:<br /><br /> A = Crescente<br /><br /> D = Decrescente<br /><br /> NULL = Non applicabile<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce sempre A.|  
|CARDINALITY|**int**|Numero di righe della tabella o valori univoci dell'indice.|  
|PAGES|**int**|Numero di pagine necessarie per l'archiviazione dell'indice o della tabella.|  
|FILTER_CONDITION|**nvarchar (** 4000 **)**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non restituisce un valore.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione SELECT per lo schema.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituite tutte le informazioni sugli indici dalla tabella `Employees` del database `AdventureWorks2012` nel server collegato `Seattle1`.  
  
```  
EXEC sp_indexes @table_server = 'Seattle1',   
   @table_name = 'Employee',   
   @table_schema = 'HumanResources',  
   @table_catalog = 'AdventureWorks2012';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure per query distribuite &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/distributed-queries-stored-procedures-transact-sql.md)   
 [sp_catalogs &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-catalogs-transact-sql.md)   
 [sp_column_privileges &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-column-privileges-transact-sql.md)   
 [sp_foreignkeys &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-foreignkeys-transact-sql.md)   
 [sp_linkedservers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-linkedservers-transact-sql.md)   
 [sp_tables_ex &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-tables-ex-transact-sql.md)   
 [sp_table_privileges &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-table-privileges-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
