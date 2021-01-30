---
description: sp_foreignkeys (Transact-SQL)
title: sp_foreignkeys (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_foreignkeys_TSQL
- sp_foreignkeys
dev_langs:
- TSQL
helpviewer_keywords:
- sp_foreignkeys
ms.assetid: 935fe385-19ff-41a4-8d0b-30618966991d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 377ef64c76238e2e840107dbbe6b32dc9ba4c00f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189565"
---
# <a name="sp_foreignkeys-transact-sql"></a>sp_foreignkeys (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce le chiavi esterne che fanno riferimento alle chiavi primarie nella tabella del server collegato.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_foreignkeys [ @table_server = ] 'table_server'   
     [ , [ @pktab_name = ] 'pktab_name' ]   
     [ , [ @pktab_schema = ] 'pktab_schema' ]   
     [ , [ @pktab_catalog = ] 'pktab_catalog' ]   
     [ , [ @fktab_name = ] 'fktab_name' ]   
     [ , [ @fktab_schema = ] 'fktab_schema' ]   
     [ , [ @fktab_catalog = ] 'fktab_catalog' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @table_server = ] 'table_server'` Nome del server collegato per cui si desidera ottenere informazioni sulla tabella. *table_server* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @pktab_name = ] 'pktab_name'` Nome della tabella contenente una chiave primaria. *pktab_name* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @pktab_schema = ] 'pktab_schema'` Nome dello schema con una chiave primaria. *pktab_schema* è di **tipo sysname** e il valore predefinito è null. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contiene il nome del proprietario.  
  
`[ @pktab_catalog = ] 'pktab_catalog'` Nome del catalogo con una chiave primaria. *pktab_catalog* è di **tipo sysname** e il valore predefinito è null. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contiene il nome del database.  
  
`[ @fktab_name = ] 'fktab_name'` Nome della tabella con una chiave esterna. *fktab_name* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @fktab_schema = ] 'fktab_schema'` Nome dello schema con una chiave esterna. *fktab_schema* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @fktab_catalog = ] 'fktab_catalog'` Nome del catalogo con una chiave esterna. *fktab_catalog* è di **tipo sysname** e il valore predefinito è null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Nessuno  
  
## <a name="result-sets"></a>Set di risultati  
 Vari prodotti DBMS supportano la denominazione in tre parti per le tabelle (_Catalog_**.** _schema_**.** _Table_), rappresentata nel set di risultati.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**PKTABLE_CAT**|**sysname**|Catalogo della tabella contenente la chiave primaria.|  
|**PKTABLE_SCHEM**|**sysname**|Schema della tabella contenente la chiave primaria.|  
|**PKTABLE_NAME**|**sysname**|Nome della tabella contenente la chiave primaria. Questo campo restituisce sempre un valore.|  
|**PKCOLUMN_NAME**|**sysname**|Nome della colonna o delle colonne chiave primaria, per ogni colonna della **table_name** restituita. Questo campo restituisce sempre un valore.|  
|**FKTABLE_CAT**|**sysname**|Catalogo della tabella contenente la chiave esterna.|  
|**FKTABLE_SCHEM**|**sysname**|Schema della tabella contenente la chiave esterna.|  
|**FKTABLE_NAME**|**sysname**|Nome della tabella contenente una chiave esterna. Questo campo restituisce sempre un valore.|  
|**FKCOLUMN_NAME**|**sysname**|Nome delle colonne chiavi esterne, per ogni colonna della tabella TABLE_NAME restituita. Questo campo restituisce sempre un valore.|  
|**KEY_SEQ**|**smallint**|Numero sequenziale della colonna in una chiave primaria a più colonne. Questo campo restituisce sempre un valore.|  
|**UPDATE_RULE**|**smallint**|Azione applicata alla chiave esterna quando l'operazione SQL è un aggiornamento. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce 0, 1 o 2 per queste colonne:<br /><br /> 0 = modifiche di tipo CASCADE alla chiave esterna.<br /><br /> 1 = modifiche di tipo NO ACTION se la chiave esterna è presente.<br /><br /> 2 = SET_NULL; la chiave esterna viene impostata su NULL.|  
|**DELETE_RULE**|**smallint**|Azione applicata alla chiave esterna quando l'operazione SQL è un'operazione di eliminazione. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce 0, 1 o 2 per queste colonne:<br /><br /> 0 = modifiche di tipo CASCADE alla chiave esterna.<br /><br /> 1 = modifiche di tipo NO ACTION se la chiave esterna è presente.<br /><br /> 2 = SET_NULL; la chiave esterna viene impostata su NULL.|  
|**FK_NAME**|**sysname**|Identificatore della chiave esterna. NULL se non è applicabile all'origine dati. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce il nome del vincolo FOREIGN KEY.|  
|**PK_NAME**|**sysname**|Identificatore della chiave primaria. NULL se non è applicabile all'origine dati. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce il nome del vincolo PRIMARY KEY.|  
|**DEFERRABILITY**|**smallint**|Indica se è possibile posticipare il controllo dei vincoli.|  
  
 Nel set di risultati le colonne FK_NAME e PK_NAME restituiscono sempre NULL.  
  
## <a name="remarks"></a>Commenti  
 **sp_foreignkeys** esegue una query sul set di righe FOREIGN_KEYS dell'interfaccia **IDBSchemaRowset** del provider OLE DB che corrisponde a *table_server*. I parametri *table_name*, *TABLE_SCHEMA*, *TABLE_CATALOG* e *Column* vengono passati a questa interfaccia per limitare le righe restituite.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione SELECT per lo schema.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituite informazioni relative alla chiave esterna per la tabella `Department` del database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] presente nel server collegato `Seattle1`.  
  
```  
EXEC sp_foreignkeys @table_server = N'Seattle1',   
   @pktab_name = N'Department',   
   @pktab_catalog = N'AdventureWorks2012';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_catalogs &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-catalogs-transact-sql.md)   
 [sp_column_privileges &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-column-privileges-transact-sql.md)   
 [sp_indexes &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-indexes-transact-sql.md)   
 [sp_linkedservers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-linkedservers-transact-sql.md)   
 [sp_primarykeys &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-primarykeys-transact-sql.md)   
 [sp_tables_ex &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-tables-ex-transact-sql.md)   
 [sp_table_privileges &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-table-privileges-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
