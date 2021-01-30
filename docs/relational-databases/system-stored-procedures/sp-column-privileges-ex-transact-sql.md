---
description: sp_column_privileges_ex (Transact-SQL)
title: sp_column_privileges_ex (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_column_privileges_ex
- sp_column_privileges_ex_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_column_privileges_ex
ms.assetid: 98cb6e58-4007-40fc-b048-449fb2e7e6be
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c4ce393362851e17471829272544df86840153fd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174503"
---
# <a name="sp_column_privileges_ex-transact-sql"></a>sp_column_privileges_ex (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce i privilegi di una colonna della tabella specificata nel server collegato specificato.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_column_privileges_ex [ @table_server = ] 'table_server'   
     [ , [ @table_name = ] 'table_name' ]   
     [ , [ @table_schema = ] 'table_schema' ]   
     [ , [ @table_catalog = ] 'table_catalog' ]   
     [ , [ @column_name = ] 'column_name' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @table_server = ] 'table_server'` Nome del server collegato per cui si desidera ottenere informazioni. *table_server* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @table_name = ] 'table_name'` Nome della tabella contenente la colonna specificata. *table_name* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @table_schema = ] 'table_schema'` Schema della tabella. *TABLE_SCHEMA* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @table_catalog = ] 'table_catalog'` Nome del database in cui risiede il *table_name* specificato. *TABLE_CATALOG* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @column_name = ] 'column_name'` Nome della colonna per cui si desidera ottenere informazioni sui privilegi. *column_name* è di **tipo sysname** e il valore predefinito è null (tutti comuni).  
  
## <a name="result-sets"></a>Set di risultati  
 Nella tabella seguente vengono descritte le colonne dei set di risultati. I risultati restituiti vengono ordinati in base **TABLE_QUALIFIER**, **TABLE_OWNER**, **table_name**, **column_name** e **Privilege**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**TABLE_CAT**|**sysname**|Nome del qualificatore della tabella. Vari prodotti DBMS supportano la denominazione in tre parti per le tabelle (_qualificatore_**.** _proprietario_**.** _nome_). In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questa colonna rappresenta il nome del database. In altri prodotti rappresenta il nome del server dell'ambiente di database della tabella. Questo campo può essere NULL.|  
|**TABLE_SCHEM**|**sysname**|Nome del proprietario della tabella. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questa colonna rappresenta il nome dell'utente del database che ha creato la tabella. Questo campo restituisce sempre un valore.|  
|**TABLE_NAME**|**sysname**|Nome della tabella. Questo campo restituisce sempre un valore.|  
|**COLUMN_NAME**|**sysname**|Nome della colonna, per ogni colonna della **table_name** restituita. Questo campo restituisce sempre un valore.|  
|**GRANTOR**|**sysname**|Nome utente del database che ha concesso le autorizzazioni per questo **column_name** all'utente **autorizzato** indicato. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , questa colonna è sempre identica alla **TABLE_OWNER**. Questo campo restituisce sempre un valore.<br /><br /> La colonna di **concessione** può essere il proprietario del database (**TABLE_OWNER**) o un utente a cui il proprietario del database ha concesso le autorizzazioni tramite la clausola WITH GRANT OPTION dell'istruzione Grant.|  
|**GRANTEE**|**sysname**|Nome utente del database a cui sono state concesse le autorizzazioni per questo **column_name** dall'utente **autorizzato** indicato. Questo campo restituisce sempre un valore.|  
|**PRIVILEGE**|**varchar (** 32 **)**|Una delle autorizzazioni di colonna disponibili. Le autorizzazioni di colonna possono essere rappresentate da uno dei valori riportati di seguito o da altri valori supportati dall'origine dei dati in fase di definizione dell'implementazione:<br /><br /> SELECT = l' **utente GRANTEE** può recuperare dati per le colonne.<br /><br /> INSERT = l' **utente GRANTEE** può fornire dati per la colonna quando vengono inserite nuove righe (dall' **utente autorizzato**) nella tabella.<br /><br /> UPDATE = l' **utente GRANTEE** può modificare i dati esistenti nella colonna.<br /><br /> REFERENCEs = l' **utente GRANTEE** può fare riferimento a una colonna di una tabella esterna in una relazione di chiave primaria/chiave esterna. Questo tipo di relazione viene definito tramite vincoli di tabella.|  
|**IS_GRANTABLE**|**varchar (** 3 **)**|Indica se l' **utente autorizzato** ha la possibilità di concedere autorizzazioni ad altri utenti (spesso denominate autorizzazione "Grant with Grant"). I possibili valori sono YES, NO e NULL. Un valore sconosciuto, o NULL, corrisponde a un'origine dei dati per la quale questo tipo di assegnazione delle autorizzazioni non è consentito.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione SELECT per lo schema.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituite informazioni relative ai privilegi di colonna della tabella `HumanResources.Department` del database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] nel server collegato `Seattle1`.  
  
```  
EXEC sp_column_privileges_ex @table_server = 'Seattle1',   
   @table_name = 'Department',   
   @table_schema = 'HumanResources',  
   @table_catalog ='AdventureWorks2012';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_table_privileges_ex &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-table-privileges-ex-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
