---
description: sp_srvrolepermission (Transact-SQL)
title: sp_srvrolepermission (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_srvrolepermission_TSQL
- sp_srvrolepermission
dev_langs:
- TSQL
helpviewer_keywords:
- sp_srvrolepermission
ms.assetid: 5709667f-e3e4-48a2-93ec-af5e22a2ac58
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 98356145bf3333e6027cff6cef825e92a3e15afd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207317"
---
# <a name="sp_srvrolepermission-transact-sql"></a>sp_srvrolepermission (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Visualizza le autorizzazioni di un ruolo predefinito del server.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_srvrolepermission [ [ @srvrolename = ] 'role']  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @srvrolename = ] 'role'` Nome del ruolo predefinito del server per il quale vengono restituite le autorizzazioni. *Role* è di **tipo sysname** e il valore predefinito è null. Se il ruolo viene omesso, vengono restituite le autorizzazioni di tutti i ruoli predefiniti del server. il *ruolo* può avere uno dei valori seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**sysadmin**|Amministratori di sistema|  
|**securityadmin**|Amministratori di sicurezza|  
|**serveradmin**|Amministratori di server|  
|**setupadmin**|Amministratori di installazione|  
|**processadmin**|Amministratori di processi|  
|**diskadmin**|Amministratori di dischi|  
|**dbcreator**|Creatori di database|  
|**bulkadmin**|Può eseguire le istruzioni BULK INSERT|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**ServerRole**|**sysname**|Nome di un ruolo predefinito del server.|  
|**Autorizzazione**|**sysname**|Autorizzazione associata a **ServerRole**|  
  
## <a name="remarks"></a>Commenti  
 Le autorizzazioni visualizzate includono le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] e altre attività speciali che possono essere eseguite dai membri del ruolo predefinito del server. Per visualizzare un elenco dei ruoli predefiniti del server, eseguire **sp_helpsrvrole**.  
  
 Il ruolo predefinito del server **sysadmin** dispone delle autorizzazioni di tutti gli altri ruoli predefiniti del server.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** .  
  
## <a name="examples"></a>Esempi  
 Nella query seguente vengono restituite le autorizzazioni associate al ruolo predefinito del server `sysadmin`.  
  
```  
EXEC sp_srvrolepermission 'sysadmin';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addsrvrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsrvrolemember-transact-sql.md)   
 [sp_dropsrvrolemember &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropsrvrolemember-transact-sql.md)   
 [sp_helpsrvrole &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpsrvrole-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
