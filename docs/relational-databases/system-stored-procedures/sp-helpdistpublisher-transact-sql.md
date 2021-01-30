---
description: sp_helpdistpublisher (Transact-SQL)
title: sp_helpdistpublisher (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpdistpublisher_TSQL
- sp_helpdistpublisher
helpviewer_keywords:
- sp_helpdistpublisher
ms.assetid: f207c22d-8fb2-4756-8a9d-6c51d6cd3470
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 461a186656cbef0d1a83eecb2a3e58d63ab896ac
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209306"
---
# <a name="sp_helpdistpublisher-transact-sql"></a>sp_helpdistpublisher (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Restituisce le proprietà dei server di pubblicazione utilizzando un server di distribuzione. Questa stored procedure viene eseguita in qualsiasi database del server di distribuzione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helpdistpublisher [ [ @publisher=] 'publisher']   
    [ , [ @check_user = ] check_user  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publisher = ] 'publisher'` Server di pubblicazione per il quale vengono restituite le proprietà. *Publisher* è di **tipo sysname** e il valore predefinito è **%** .  
  
`[ @check_user = ] check_user` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**nome**|**sysname**|Nome del server di pubblicazione.|  
|**distribution_db**|**sysname**|Database di distribuzione per il server di pubblicazione specificato.|  
|**security_mode**|**int**|Modalità di sicurezza utilizzata dagli agenti di replica per connettersi al server di pubblicazione per le sottoscrizioni ad aggiornamento in coda o a un server di pubblicazione non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br />   =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticazione 0<br /><br /> **1** = autenticazione di Windows|  
|**accesso**|**sysname**|Nome account di accesso utilizzato dagli agenti di replica per connettersi al server di pubblicazione per le sottoscrizioni ad aggiornamento in coda o a un server di pubblicazione non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**password**|**nvarchar (524)**|Password restituita in formato crittografato semplice. La password è NULL per utenti diversi da **sysadmin**.|  
|**active**|**bit**|Indica se un server di pubblicazione remoto utilizza il server locale come server di distribuzione:<br /><br /> **0** = No<br /><br /> **1** = Sì|  
|**working_directory**|**nvarchar(255)**|Nome della directory di lavoro.|  
|**trusted**|**bit**|Indica se la password è obbligatoria per la connessione del server di pubblicazione al server di distribuzione. Per [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] e versioni successive, deve sempre restituire **0**, il che significa che la password è obbligatoria.|  
|**thirdparty_flag**|**bit**|Indica se la pubblicazione è abilitata da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o da un'applicazione di terze parti:<br /><br />   =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] server di pubblicazione 0, Oracle o Oracle Gateway.<br /><br /> **1** = l'editore è stato integrato con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'uso di un'applicazione di terze parti.|  
|**publisher_type**|**sysname**|Tipo di server di pubblicazione. Può essere uno dei tipi seguenti:<br /><br /> **MSSQLSERVER**<br /><br /> **ORACLE**<br /><br /> **ORACLE GATEWAY**|  
|**publisher_data_source**|**nvarchar(4000)**|Nome dell'origine dati OLE DB nel server di pubblicazione.|  
|**storage_connection_string**|**nvarchar(4000)**|Chiave di accesso alle archiviazione per la directory di lavoro quando il server di distribuzione o il server di pubblicazione nel database SQL|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_helpdistpublisher** viene utilizzato in tutti i tipi di replica.  
  
 **sp_helpdistpublisher** non visualizzerà l'account di accesso o la password del server di pubblicazione nel set di risultati per gli account di accesso non **sysadmin** .  
  
## <a name="permissions"></a>Autorizzazioni  
 I membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_helpdistpublisher** per tutti i server di pubblicazione che utilizzano il server locale come server di distribuzione. I membri del ruolo predefinito del database **db_owner** o del ruolo **replmonitor** in un database di distribuzione possono eseguire **sp_helpdistpublisher** per tutti i server di pubblicazione che utilizzano il database di distribuzione. Gli utenti nell'elenco di accesso alla pubblicazione per una pubblicazione nel *Server* di pubblicazione specificato possono eseguire **sp_helpdistpublisher**. Se il *server di pubblicazione* non è specificato, vengono restituite informazioni per tutti i server di pubblicazione per i quali l'utente dispone dei diritti di accesso.  
  
## <a name="see-also"></a>Vedere anche  
 [Visualizzare e modificare le proprietà del server di pubblicazione e del database di distribuzione](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)   
 [sp_adddistpublisher &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)   
 [sp_changedistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedistpublisher-transact-sql.md)   
 [sp_dropdistpublisher &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md)  
  
  
