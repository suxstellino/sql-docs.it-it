---
description: sp_helpdbfixedrole (Transact-SQL)
title: sp_helpdbfixedrole (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpdbfixedrole
- sp_helpdbfixedrole_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpdbfixedrole
ms.assetid: ad87e9a0-b901-4e37-9950-aa517d680fc3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 817947901ca7f35ba2972004d23ed8d7f85215ed
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176425"
---
# <a name="sp_helpdbfixedrole-transact-sql"></a>sp_helpdbfixedrole (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce un elenco dei ruoli predefiniti del database.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helpdbfixedrole [ [ @rolename = ] 'role' ]   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @rolename = ] 'role'` Nome di un ruolo predefinito del database. *Role* è di **tipo sysname** e il valore predefinito è null. Se *Role* viene specificato, vengono restituite solo le informazioni relative a tale ruolo. in caso contrario, viene restituito un elenco e una descrizione di tutti i ruoli predefiniti del database.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**DbFixedRole**|**sysname**|Nome del ruolo predefinito del database.|  
|**Descrizione**|**nvarchar (70)**|Descrizione di **DbFixedRole.**|  
  
## <a name="remarks"></a>Commenti  
 I ruoli predefiniti del database vengono definiti a livello del database e dispongono delle autorizzazioni per l'esecuzione di specifiche attività amministrative a livello del database, come indicato nella tabella seguente. Non è possibile aggiungere o rimuovere i ruoli predefiniti del database e non è possibile modificare le autorizzazioni concesse a un ruolo predefinito del database.  
  
|Ruolo predefinito del database|Descrizione|  
|-------------------------|-----------------|  
|**db_owner**|Proprietari di database|  
|**db_accessadmin**|Amministratori dell'accesso ai database|  
|**db_securityadmin**|Amministratori della sicurezza dei database|  
|**db_ddladmin**|Amministratori DDL dei database|  
|**db_backupoperator**|Operatori di backup dei database|  
|**db_datareader**|Utenti con autorizzazioni di lettura per i database|  
|**db_datawriter**|Utenti con autorizzazioni di scrittura per i database|  
|**db_denydatareader**|Utenti senza autorizzazioni di lettura per i database|  
|**db_denydatawriter**|Utenti senza autorizzazioni di scrittura per i database|  
  
 Nella tabella seguente vengono descritte le stored procedure utilizzate per la modifica dei ruoli del database.  
  
|Stored procedure|Azione|  
|----------------------|------------|  
|**sp_addrolemember**|Aggiunge un utente di database a un ruolo predefinito del database.|  
|**sp_helprole**|Visualizza un elenco dei membri di un ruolo predefinito del database.|  
|**sp_droprolemember**|Rimuove un membro da un ruolo predefinito del database.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** .  
  
 Le informazioni restituite sono soggette a limitazioni di accesso ai metadati. Non vengono visualizzate le entità per le quali l'entità di database non dispone dell'autorizzazione. Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene visualizzato un elenco di tutti i ruoli predefiniti del database.  
  
```  
EXEC sp_helpdbfixedrole;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sp_dbfixedrolepermission &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dbfixedrolepermission-transact-sql.md)   
 [sp_droprolemember &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)   
 [sp_helprole &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helprole-transact-sql.md)   
 [sp_helprolemember &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helprolemember-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
