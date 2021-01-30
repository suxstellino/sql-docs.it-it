---
description: sp_table_privileges_ex (Transact-SQL)
title: sp_table_privileges_ex (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_table_privileges_ex
- sp_table_privileges_ex_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_table_privileges_ex
ms.assetid: b58d4a07-5c40-4f17-b66e-6d6b17188dda
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 41c9548f0c4841b6a9c1845a22c7883cd42ad88b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203680"
---
# <a name="sp_table_privileges_ex-transact-sql"></a>sp_table_privileges_ex (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni sui privilegi per la tabella specificata nel server collegato specificato.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_table_privileges_ex [ @table_server = ] 'table_server'   
     [ , [ @table_name = ] 'table_name' ]   
     [ , [ @table_schema = ] 'table_schema' ]   
     [ , [ @table_catalog = ] 'table_catalog' ]  
     [ , [@fUsePattern =] 'fUsePattern']  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @table_server = ] 'table_server'` Nome del server collegato per cui si desidera ottenere informazioni. *table_server* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @table_name = ] 'table_name']` Nome della tabella per cui si desidera ottenere informazioni sui privilegi di tabella. *table_name* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @table_schema = ] 'table_schema'` Schema della tabella. In alcuni ambienti DBMS corrisponde al proprietario della tabella. *TABLE_SCHEMA* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @table_catalog = ] 'table_catalog'` Nome del database in cui risiede il *table_name* specificato. *TABLE_CATALOG* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @fUsePattern = ] 'fUsePattern'` Determina se i caratteri ' _', '%',' [' è]' sono interpretati come caratteri jolly. I valori validi sono 0 (utilizzo dei criteri di ricerca disattivato) e 1 (utilizzo dei criteri di ricerca attivato). *fUsePattern* è di **bit** e il valore predefinito è 1.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Nessuno  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**TABLE_CAT**|**sysname**|Nome del qualificatore della tabella. Vari prodotti DBMS supportano la denominazione in tre parti per le tabelle (_qualificatore_**.** _proprietario_**.** _nome_). In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questa colonna rappresenta il nome del database. In altri prodotti rappresenta il nome del server dell'ambiente di database della tabella. Questo campo può essere NULL.|  
|**TABLE_SCHEM**|**sysname**|Nome del proprietario della tabella. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questa colonna rappresenta il nome dell'utente del database che ha creato la tabella. Questo campo restituisce sempre un valore.|  
|**TABLE_NAME**|**sysname**|Nome della tabella. Questo campo restituisce sempre un valore.|  
|**GRANTOR**|**sysname**|Nome utente del database che ha concesso le autorizzazioni per questo **table_name** all' **utente autorizzato** specificato. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , questa colonna è sempre identica alla **TABLE_OWNER**. Questo campo restituisce sempre un valore. Inoltre, è possibile che la colonna di concessione sia il proprietario del database (**TABLE_OWNER**) o un utente a cui il proprietario del database ha concesso l'autorizzazione utilizzando la clausola WITH GRANT OPTION dell'istruzione Grant.|  
|**GRANTEE**|**sysname**|Nome utente del database a cui sono state concesse le autorizzazioni per questo **table_name** dall' **utente autorizzato** indicato. Questo campo restituisce sempre un valore.|  
|**PRIVILEGE**|**varchar (** 32 **)**|Una delle autorizzazioni di tabella disponibili. I possibili valori delle autorizzazioni di tabella sono i seguenti. È inoltre possibile utilizzare altri valori supportati dall'origine dei dati al momento della definizione dell'implementazione.<br /><br /> SELECT = l' **utente GRANTEE** può recuperare i dati per una o più colonne.<br /><br /> INSERT = l' **utente GRANTEE** può fornire dati per le nuove righe di una o più colonne.<br /><br /> UPDATE = l' **utente GRANTEE** può modificare i dati esistenti per una o più colonne.<br /><br /> DELETE = l' **utente GRANTEE** può rimuovere righe dalla tabella.<br /><br /> REFERENCEs = l' **utente GRANTEE** può fare riferimento a una colonna di una tabella esterna in una relazione di chiave primaria/chiave esterna. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le relazioni tra chiave primaria e chiave esterna vengono definite tramite l'utilizzo di vincoli di tabella.<br /><br /> L'ambito di azione fornito all' **utente autorizzato** da un privilegio di tabella specifico dipende dall'origine dati. Ad esempio, l'autorizzazione UPDATE potrebbe consentire all' **utente autorizzato** di aggiornare tutte le colonne di una tabella in un'origine dati e solo le colonne per le quali l' **utente autorizzato** dispone dell'autorizzazione Update per un'altra origine dati.|  
|**IS_GRANTABLE**|**varchar (** 3 **)**|Indica se l' **utente autorizzato** è autorizzato a concedere autorizzazioni ad altri utenti. Questa autorizzazione spesso viene denominata "autorizzazione per la concessione di autorizzazioni". I possibili valori sono YES, NO e NULL. Il valore sconosciuto, o NULL, indica un'origine dei dati per la quale l'autorizzazione per la concessione di autorizzazioni non è applicabile.|  
  
## <a name="remarks"></a>Commenti  
 I risultati restituiti vengono ordinati in base **TABLE_QUALIFIER**, **TABLE_OWNER**, **table_name** e **Privilege**.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione SELECT per lo schema.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituite informazioni sui privilegi per le tabelle il cui nome inizia con `Product` incluse nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] disponibile nel server collegato `Seattle1`. ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si presuppone come server collegato).  
  
```  
EXEC sp_table_privileges_ex @table_server = 'Seattle1',   
   @table_name = 'Product%',   
   @table_schema = 'Production',  
   @table_catalog ='AdventureWorks2012';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_column_privileges_ex &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-column-privileges-ex-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Stored procedure per query distribuite &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/distributed-queries-stored-procedures-transact-sql.md)  
  
  
