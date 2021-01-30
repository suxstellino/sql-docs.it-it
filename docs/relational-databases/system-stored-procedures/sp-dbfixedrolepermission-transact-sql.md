---
description: sp_dbfixedrolepermission (Transact-SQL)
title: sp_dbfixedrolepermission (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dbfixedrolepermission
- sp_dbfixedrolepermission_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dbfixedrolepermission
ms.assetid: b8c30191-f532-49cd-83f3-c271f63ce572
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 78f1381d05c5841ce48e395f8cc1cd477b820e01
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201278"
---
# <a name="sp_dbfixedrolepermission-transact-sql"></a>sp_dbfixedrolepermission (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Visualizza le autorizzazioni di un ruolo predefinito del database. **sp_dbfixedrolepermission** restituisce informazioni corrette in [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] . L'output non riflette le modifiche alla gerarchia di autorizzazioni implementate in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Per ulteriori informazioni, vedere [ruoli a livello di database](../../relational-databases/security/authentication-access/database-level-roles.md#fixed-database-roles), che visualizza un elenco di ruoli predefiniti del database e le relative autorizzazioni.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_dbfixedrolepermission [ [ @rolename = ] 'role' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @rolename = ] 'role'` Nome di un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ruolo predefinito del database valido. *Role* è di **tipo sysname** e il valore predefinito è null. Se *Role* non è specificato, vengono visualizzate le autorizzazioni per tutti i ruoli predefiniti del database.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**DbFixedRole**|**sysname**|Nome del ruolo predefinito del database|  
|**Autorizzazione**|**nvarchar (70)**|Autorizzazioni associate a **DbFixedRole**|  
  
## <a name="remarks"></a>Commenti  
 Per visualizzare un elenco dei ruoli predefiniti del database, eseguire **sp_helpdbfixedrole**. Nella tabella seguente vengono elencati i ruoli predefiniti del database.  
  
|Ruolo predefinito del database|Descrizione|  
|-------------------------|-----------------|  
|**db_owner**|Proprietari di database|  
|**db_accessadmin**|Amministratori dell'accesso ai database|  
|**db_securityadmin**|Amministratori della sicurezza dei database|  
|**db_ddladmin**|Amministratori del linguaggio DDL (Data Definition Language)|  
|**db_backupoperator**|Operatori di backup dei database|  
|**db_datareader**|Utenti con autorizzazioni di lettura per i database|  
|**db_datawriter**|Utenti con autorizzazioni di scrittura per i database|  
|**db_denydatareader**|Utenti senza autorizzazioni di lettura per i database|  
|**db_denydatawriter**|Utenti senza autorizzazioni di scrittura per i database|  
  
 I membri del ruolo predefinito del database **db_owner** dispongono delle autorizzazioni di tutti gli altri ruoli predefiniti del database. Per visualizzare le autorizzazioni per i ruoli predefiniti del server, eseguire **sp_srvrolepermission**.  
  
 Il set di risultati include le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] e altre attività speciali che possono essere eseguite dai membri del ruolo del database.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** .  
  
## <a name="examples"></a>Esempi  
 Nella query seguente vengono restituite le autorizzazioni per tutti i ruoli predefiniti del database perché non ne è stato specificato uno.  
  
```  
EXEC sp_dbfixedrolepermission;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sp_droprolemember &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)   
 [sp_helpdbfixedrole &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpdbfixedrole-transact-sql.md)   
 [sp_srvrolepermission &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-srvrolepermission-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
