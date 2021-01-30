---
title: sp_MSchange_distribution_agent_properties (T-SQL)
description: Descrive il sp_MSchange_distribution_agent_properties stored procedure utilizzato per modificare le proprietà del agente di distribuzione per una topologia replica di SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_MSchange_distribution_agent_properties
- sp_MSchange_distribution_agent_properties_TSQL
helpviewer_keywords:
- sp_MSchange_distribution_agent_properties
ms.assetid: 7dac5e68-bf84-433a-a531-66921f35126f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 37bb9e7af79fbc3ef99febd84cce78eb8db8a557
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195461"
---
# <a name="sp_mschange_distribution_agent_properties-transact-sql"></a>sp_MSchange_distribution_agent_properties (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifica le proprietà di un processo di agente di distribuzione eseguito in un [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] server di distribuzione o versione successiva. Questa stored procedure viene utilizzata per modificare le proprietà quando il server di pubblicazione viene eseguito in un'istanza di [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]. La stored procedure viene eseguita nel database di distribuzione del server di distribuzione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_MSchange_distribution_agent_properties [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'   
        , [ @subscriber = ] 'subscriber'   
        , [ @subscriber_db = ] 'subscriber_db'   
        , [ @property = ] 'property'   
        , [ @value = ] 'value' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione. *Publisher* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publisher_db = ] 'publisher_db'` Nome del database di pubblicazione. *publisher_db* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @subscriber = ] 'subscriber'` Nome del Sottoscrittore. *Subscriber* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @subscriber_db = ] 'subscriber_db'` Nome del database di sottoscrizione. *subscriber_db* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @property = ] 'property'` Proprietà della pubblicazione da modificare. *Property* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @value = ] 'value'` Nuovo valore della proprietà. *value* è di **tipo nvarchar (524)** e il valore predefinito è null.  
  
 Nella tabella seguente vengono descritte le proprietà del processo dell'agente di distribuzione che è possibile modificare e le limitazioni previste per i valori delle proprietà.  
  
|Proprietà|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**distrib_job_login**||Account di accesso per l'account di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows utilizzato per l'esecuzione dell'agente.|  
|**distrib_job_password**||Password dell'account di Windows utilizzato per l'esecuzione del processo dell'agente.|  
|**subscriber_catalog**||Catalogo da utilizzare per stabilire una connessione al provider OLE DB *Questa proprietà è valida solo per non* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *Sottoscrittori.*|  
|**subscriber_datasource**||Nome dell'origine dei dati riconosciuto dal provider OLE DB. *Questa proprietà è valida solo per non* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *Sottoscrittori.*|  
|**subscriber_location**||Percorso del database riconosciuto dal provider OLE DB. *Questa proprietà è valida solo per non* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *Sottoscrittori.*|  
|**subscriber_login**||Account di accesso da utilizzare durante la connessione a un Sottoscrittore per sincronizzare la sottoscrizione.|  
|**subscriber_password**||Password del Sottoscrittore.<br /><br /> [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]|  
|**subscriber_provider**||ProgID univoco con il quale viene registrato il provider OLE DB per l'origine dei dati non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *Questa proprietà è valida solo per non* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *Sottoscrittori.*|  
|**subscriber_providerstring**||Stringa di connessione specifica del provider OLE DB che identifica l'origine dei dati. *Questa proprietà è valida solo per i Sottoscrittori non SQL Server.*|  
|**subscriber_security_mode**|**1**|Autenticazione di Windows.<br /><br /> [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]|  
||**0**|Autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**subscriber_type**|**0**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sottoscrittore|  
||**1**|Server dell'origine dei dati ODBC.|  
||**3**|Provider OLE DB|  
|**SubscriptionStreams**||Numero di connessioni consentite per agente di distribuzione per l'applicazione in parallelo di modifiche a un Sottoscrittore. *Non supportato per non* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *Sottoscrittori, autori Oracle o sottoscrizioni peer-to-peer.*|  
  
> [!NOTE]  
>  Dopo la modifica dell'account di accesso o della password di un agente, è necessario arrestare e riavviare l'agente per rendere effettiva la modifica.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_MSchange_distribution_agent_properties** viene utilizzata nella replica snapshot e nella replica transazionale.  
  
 Quando il server di pubblicazione viene eseguito in un'istanza di [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] o versione successiva, è necessario utilizzare [sp_changesubscription](../../relational-databases/system-stored-procedures/sp-changesubscription-transact-sql.md) per modificare le proprietà di un processo agente di merge che sincronizza una sottoscrizione push eseguita nel server di distribuzione.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** nel server di distribuzione possono eseguire **sp_MSchange_distribution_agent_properties**.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_addpushsubscription_agent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md)   
 [sp_addsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)  
  
  
