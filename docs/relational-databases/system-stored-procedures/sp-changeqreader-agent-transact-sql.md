---
description: sp_changeqreader_agent (Transact-SQL)
title: sp_changeqreader_agent (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changeqreader_agent_TSQL
- sp_changeqreader_agent
helpviewer_keywords:
- sp_changeqreader_agent
ms.assetid: d3fe79c5-31ef-4565-bf38-b476b5fb16f7
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 83be24af54593c4630284e07cf9464899d2b5cfd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207061"
---
# <a name="sp_changeqreader_agent-transact-sql"></a>sp_changeqreader_agent (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Consente di modificare le proprietà di sicurezza di un agente di lettura coda. Questa stored procedure viene eseguita nel database di distribuzione del server di distribuzione o nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_changeqreader_agent [ [ @job_login = ] 'job_login' ]  
    [ , [ @job_password = ] 'job_password' ]  
    [ , [ @frompublisher = ] frompublisher   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @job_login = ] 'job_login'` Account di accesso per l'account di Windows utilizzato per l' [!INCLUDE[msCoName](../../includes/msconame-md.md)] esecuzione dell'agente. *job_login* è di **tipo nvarchar (257)** e il valore predefinito è null.  
  
`[ @job_password = ] 'job_password'` Password per l'account di Windows utilizzato per l'esecuzione dell'agente. *job_password* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @frompublisher = ] frompublisher` Indica se la stored procedure viene eseguita nel server di pubblicazione. *frompublisher* è di bit e il valore predefinito è **0**. Il valore **1** indica che la stored procedure viene eseguita dal server di pubblicazione nel database di pubblicazione.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_changeqreader_agent** viene utilizzata nella replica transazionale.  
  
 **sp_changeqreader_agent** viene utilizzata per modificare l'account di Windows utilizzato per l'esecuzione di un agente di lettura coda. È possibile cambiare la password di un account di accesso di Windows esistente oppure specificare un nuovo account di accesso di Windows e la password.  
  
 Dopo la modifica dell'account di accesso o della password di un agente, è necessario arrestare e riavviare l'agente per rendere effettiva la modifica.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_changeqreader_agent**.  
  
## <a name="see-also"></a>Vedere anche  
 [Visualizzare e modificare le impostazioni di sicurezza della replica](../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)   
 [sp_addqreader_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addqreader-agent-transact-sql.md)  
  
  
