---
description: sp_help_publication_access (Transact-SQL)
title: sp_help_publication_access (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_help_publication_access
- sp_help_publication_access_TSQL
helpviewer_keywords:
- sp_help_publication_access
ms.assetid: 9408fa13-54a0-4cb1-8fb0-845e5536ef50
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b33b79c6e632eef28feaa5fa8b3f605018b09953
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199599"
---
# <a name="sp_help_publication_access-transact-sql"></a>sp_help_publication_access (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Restituisce un elenco di tutti gli account di accesso a cui sono state concesse autorizzazioni per una pubblicazione. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_help_publication_access [ @publication = ] 'publication'  
    [ , [ @return_granted = ] 'return_granted' ]   
    [ , [ @login = ] 'login' ]  
    [ , [ @initial_list = ] initial_list ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione a cui accedere. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @return_granted = ] 'return_granted'` ID di accesso. *return_granted* è di **bit** e il valore predefinito è 1. Se viene specificato **0** e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene utilizzata l'autenticazione, vengono restituiti gli account di accesso disponibili visualizzati nel server di pubblicazione, ma non nel server di distribuzione. Se viene specificato **0** e viene utilizzata l'autenticazione di Windows, vengono restituiti gli account di accesso non autorizzati in modo specifico al server di pubblicazione o al server di distribuzione.  
  
`[ @login = ] 'login'` ID di accesso di sicurezza standard. *login* è di **tipo sysname** e il valore predefinito è **%** .  
  
`[ @initial_list = ] initial_list` Specifica se restituire tutti i membri con accesso alla pubblicazione o solo quelli che hanno eseguito l'accesso prima che nuovi membri siano stati aggiunti all'elenco. *initial_list* è di bit e il valore predefinito è **0**.  
  
 **1** restituisce informazioni per tutti i membri del ruolo predefinito del server **sysadmin** con account di accesso validi nel server di distribuzione esistente al momento della creazione della pubblicazione, nonché l'account di accesso corrente.  
  
 **0** restituisce informazioni per tutti i membri del ruolo predefinito del server **sysadmin** con account di accesso validi nel server di distribuzione esistente al momento della creazione della pubblicazione e di tutti gli utenti nell'elenco di accesso alla pubblicazione che non appartengono al ruolo predefinito del server **sysadmin** .  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**LoginName**|**nvarchar(256)**|Nome effettivo dell'account di accesso.|  
|**Isntname**|**int**|**0** = l'account di accesso non è un utente di Windows.<br /><br /> **1** = l'account di accesso è un utente di Windows.|  
|**Isntgroup**|**int**|**0** = l'account di accesso non è un gruppo di Windows.<br /><br /> **1** = l'account di accesso è un gruppo di Windows.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_help_publication_access** viene utilizzato in tutti i tipi di replica.  
  
 Quando **Isntname** e **Isntgroup** nel set di risultati sono **0**, si presuppone che l'account di accesso sia un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] account di accesso.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_help_publication_access**.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_grant_publication_access &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-grant-publication-access-transact-sql.md)   
 [sp_revoke_publication_access &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-revoke-publication-access-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
