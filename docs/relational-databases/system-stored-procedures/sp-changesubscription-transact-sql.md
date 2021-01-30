---
description: sp_changesubscription (Transact-SQL)
title: sp_changesubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/28/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- changesubscription
- sp_changesubscription
- changesubscription_TSQL
- sp_changesubscription_TSQL
helpviewer_keywords:
- sp_changesubscription
ms.assetid: f9d91fe3-47cf-4915-b6bf-14c9c3d8a029
author: markingmyname
ms.author: maghan
ms.openlocfilehash: db0e26092acb2a8cb0fc546a18c0b8f4010fa8fc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189583"
---
# <a name="sp_changesubscription-transact-sql"></a>sp_changesubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifica le proprietà di una sottoscrizione pull o push transazionale o snapshot coinvolta in una replica transazionale ad aggiornamento in coda. Per modificare le proprietà di tutti gli altri tipi di sottoscrizioni pull, utilizzare [sp_change_subscription_properties &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-change-subscription-properties-transact-sql.md). **sp_changesubscription** viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
> [!IMPORTANT]  
>  Quando si configura un server di pubblicazione con un server di distribuzione remoto, i valori specificati per tutti i parametri, inclusi *job_login* e *job_password*, vengono inviati al server di distribuzione come testo normale. È consigliabile crittografare la connessione tra il server di pubblicazione e il server di distribuzione remoto prima di eseguire questa stored procedure. Per altre informazioni, vedere [Abilitare le connessioni crittografate al motore di database &#40;Gestione configurazione SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_changesubscription [ @publication = ] 'publication'  
        , [ @article = ] 'article'  
        , [ @subscriber = ] 'subscriber'  
        , [ @destination_db = ] 'destination_db'  
        , [ @property = ] 'property'  
        , [ @value = ] 'value'  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione da modificare. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito  
  
`[ @article = ] 'article'` Nome dell'articolo da modificare. *article* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @subscriber = ] 'subscriber'` Nome del Sottoscrittore. *Subscriber* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @destination_db = ] 'destination_db'` Nome del database di sottoscrizione. *destination_db* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @property = ] 'property'` Proprietà da modificare per la sottoscrizione specificata. *Property* è di **tipo nvarchar (30)**. i possibili valori sono i valori della tabella.  
  
`[ @value = ] 'value'` Nuovo valore per la *Proprietà* specificata. *value* è di **tipo nvarchar (4000)**. i possibili valori sono i valori della tabella.  
  
|Proprietà|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**distrib_job_login**||Account di accesso per l'account di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows utilizzato per l'esecuzione dell'agente.|  
|**distrib_job_password**||Password dell'account di Windows utilizzato per l'esecuzione dell'agente.|  
|**subscriber_catalog**||Catalogo da utilizzare per stabilire una connessione al provider OLE DB Questa proprietà è valida solo per i [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sottoscrittori non.|  
|**subscriber_datasource**||Nome dell'origine dei dati riconosciuto dal provider OLE DB. *Questa proprietà è valida solo per non* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *Sottoscrittori.*|  
|**subscriber_location**||Percorso del database riconosciuto dal provider OLE DB. *Questa proprietà è valida solo per non* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *Sottoscrittori.*|  
|**subscriber_login**||Nome dell'account di accesso nel Sottoscrittore.|  
|**subscriber_password**||Password complessa per l'account di accesso fornito.|  
|**subscriber_security_mode**|**1**|Esegue la connessione al Sottoscrittore utilizzando l'autenticazione di Windows.|  
||**0**|Esegue la connessione al Sottoscrittore utilizzando l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**subscriber_provider**||ProgID univoco con il quale viene registrato il provider OLE DB per l'origine dei dati non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *Questa proprietà è valida solo per non* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *Sottoscrittori.*|  
|**subscriber_providerstring**||Stringa di connessione specifica del provider OLE DB che identifica l'origine dei dati. *Questa proprietà è valida solo per non* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *Sottoscrittori.*|  
|**SubscriptionStreams**||Numero di connessioni consentite per agente di distribuzione per l'applicazione di batch di modifiche in parallelo a un Sottoscrittore. Per i Publisher è supportato un intervallo di valori compresi tra **1** e **64** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Questa proprietà deve essere **0** per i [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sottoscrittori non, i Publisher Oracle o le sottoscrizioni peer-to-peer.|  
|**subscriber_type**|**1**|Server dell'origine dei dati ODBC.|  
||**3**|Provider OLE DB|  
|**memory_optimized**|**bit**|Indica che la sottoscrizione supporta le tabelle con ottimizzazione per la memoria. *memory_optimized* è di **bit**, dove 1 è uguale a true (la sottoscrizione supporta le tabelle con ottimizzazione per la memoria).|  
  
`[ @publisher = ] 'publisher'` Specifica un server di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazione non. *Publisher* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  il *Server* di pubblicazione non deve essere specificato per un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] server di pubblicazione.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_changesubscription** viene utilizzata per la replica snapshot e transazionale.  
  
 **sp_changesubscription** può essere utilizzato solo per modificare le proprietà delle sottoscrizioni push o delle sottoscrizioni pull necessarie per la replica transazionale ad aggiornamento in coda. Per modificare le proprietà di tutti gli altri tipi di sottoscrizioni pull, utilizzare [sp_change_subscription_properties &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-change-subscription-properties-transact-sql.md).  
  
 Dopo la modifica dell'account di accesso o della password di un agente, è necessario arrestare e riavviare l'agente per rendere effettiva la modifica.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_changesubscription**.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_addsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)   
 [sp_dropsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md)  
  
  
