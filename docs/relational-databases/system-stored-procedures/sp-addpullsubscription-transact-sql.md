---
description: sp_addpullsubscription (Transact-SQL)
title: sp_addpullsubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/09/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addpullsubscription
- sp_addpullsubscription_TSQL
helpviewer_keywords:
- sp_addpullsubscription
ms.assetid: 0f4bbedc-0c1c-414a-b82a-6fd47f0a6a7f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c119173ba184ef8fe3a03c59e90965650bb9c35c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206675"
---
# <a name="sp_addpullsubscription-transact-sql"></a>sp_addpullsubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Aggiunge una sottoscrizione pull a una pubblicazione snapshot o transazionale. Questa stored procedure viene eseguita nel Sottoscrittore nel database in cui deve essere creata la sottoscrizione pull.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_addpullsubscription [ @publisher= ] 'publisher'  
    [ , [ @publisher_db= ] 'publisher_db' ]  
        , [ @publication= ] 'publication'  
    [ , [ @independent_agent= ] 'independent_agent' ]  
    [ , [ @subscription_type= ] 'subscription_type' ]  
    [ , [ @description= ] 'description' ]  
    [ , [ @update_mode= ] 'update_mode' ]  
    [ , [ @immediate_sync = ] immediate_sync ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione. *Publisher* è di **tipo sysname** e non prevede alcun valore predefinito.  

> [!NOTE]
> Il nome del server può essere specificato come `<Hostname>,<PortNumber>` . Potrebbe essere necessario specificare il numero di porta per la connessione quando SQL Server viene distribuito in Linux o Windows con una porta personalizzata e il servizio browser è disabilitato.
  
`[ @publisher_db = ] 'publisher_db'` Nome del database del server di pubblicazione. *publisher_db* è di **tipo sysname** e il valore predefinito è null. *publisher_db* viene ignorato dai publisher Oracle.  
  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @independent_agent = ] 'independent_agent'` Specifica se è presente un agente di distribuzione autonomo per la pubblicazione. *independent_agent* è di **tipo nvarchar (5)** e il valore predefinito è true. Se **true**, esiste una agente di distribuzione autonoma per questa pubblicazione. Se **false**, esiste una agente di distribuzione per ogni coppia di database del server di pubblicazione/database del Sottoscrittore. *independent_agent* è una proprietà della pubblicazione e deve avere lo stesso valore presente nel server di pubblicazione.  
  
`[ @subscription_type = ] 'subscription_type'` Tipo di sottoscrizione. *subscription_type* è di **tipo nvarchar (9)** e il valore predefinito è **Anonymous**. È necessario specificare il valore **pull** per *subscription_type*, a meno che non si desideri creare una sottoscrizione senza registrare la sottoscrizione nel server di pubblicazione. In questo caso, è necessario specificare un valore **Anonimo**. Ciò è necessario in casi in cui non è possibile stabilire una connessione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al server di pubblicazione durante la configurazione della sottoscrizione.  
  
`[ @description = ] 'description'` Descrizione della pubblicazione. *Description* è di **tipo nvarchar (100)** e il valore predefinito è null.  
  
`[ @update_mode = ] 'update_mode'` Tipo di aggiornamento. *update_mode* è di **tipo nvarchar (30)**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**sola lettura** (impostazione predefinita)|La sottoscrizione è di sola lettura. Le modifiche apportate nel Sottoscrittore non vengono ritrasmesse al server di pubblicazione. È consigliabile utilizzare questo valore quando non sono previsti aggiornamenti nel Sottoscrittore.|  
|**synctran**|Abilita il supporto per sottoscrizioni ad aggiornamento immediato.|  
|**queued tran**|Abilita la sottoscrizione per l'aggiornamento in coda. Le modifiche dei dati possono essere apportate nel Sottoscrittore, archiviate in una coda e quindi distribuite al server di pubblicazione.|  
|**failover**|Abilita la sottoscrizione per l'aggiornamento immediato sostituito dall'aggiornamento in coda in caso di failover. Le modifiche dei dati possono essere apportate nel Sottoscrittore e distribuite immediatamente al server di pubblicazione. Se il server di pubblicazione e il Sottoscrittore non sono connessi, le modifiche apportate ai dati nel Sottoscrittore possono essere archiviate in una coda fino al ripristino della connessione tra il Sottoscrittore e il server di pubblicazione.|  
|**queued failover**|Abilita la sottoscrizione come sottoscrizione con aggiornamento in coda con la possibilità di passare alla modalità di aggiornamento immediato. Le modifiche ai dati possono essere apportate nel Sottoscrittore e archiviate in una coda fino alla riconnessione del Sottoscrittore e del server di pubblicazione. Quando viene ristabilita una connessione continua, la modalità di aggiornamento può essere modificata nella modalità di aggiornamento immediato. *Non supportato per i Publisher Oracle*.|  
  
`[ @immediate_sync = ] immediate_sync` Indica se i file di sincronizzazione vengono creati o ricreati a ogni esecuzione del agente di snapshot. *immediate_sync* è di **bit** e il valore predefinito è 1. deve essere impostato sullo stesso valore di *immediate_sync* in **sp_addpublication**. *immediate_sync* è una proprietà della pubblicazione e deve avere lo stesso valore presente nel server di pubblicazione.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_addpullsubscription** viene utilizzata nella replica snapshot e nella replica transazionale.  
  
> [!IMPORTANT]  
>  Per le sottoscrizioni ad aggiornamento in coda utilizzare l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per le connessioni ai Sottoscrittori e specificare un account diverso per la connessione a ogni Sottoscrittore. Quando si crea una sottoscrizione pull che supporta l'aggiornamento in coda, la replica imposta sempre la connessione per l'utilizzo dell'autenticazione di Windows. Per le sottoscrizioni pull, il sistema di replica non è in grado di accedere ai metadati nel Sottoscrittore necessari per l'utilizzo dell'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. In questo caso, è necessario eseguire [sp_changesubscription](../../relational-databases/system-stored-procedures/sp-changesubscription-transact-sql.md) per modificare la connessione per utilizzare l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticazione dopo la configurazione della sottoscrizione.  
  
 Se il [MSreplication_subscriptions &#40;tabella&#41;Transact-SQL](../../relational-databases/system-tables/msreplication-subscriptions-transact-sql.md) non esiste nel Sottoscrittore, **sp_addpullsubscription** lo crea. Viene inoltre aggiunta una riga al [MSreplication_subscriptions &#40;tabella&#41;Transact-SQL ](../../relational-databases/system-tables/msreplication-subscriptions-transact-sql.md) . Per le sottoscrizioni pull, [sp_addsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md) deve essere chiamato prima nel server di pubblicazione.  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_addtranpullsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/sp-addpullsubscription-t_1.sql)]  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_addpullsubscription**.  
  
## <a name="see-also"></a>Vedere anche  
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [Creare una sottoscrizione aggiornabile di una pubblicazione transazionale](../../relational-databases/replication/publish/create-an-updatable-subscription-to-a-transactional-publication.md) [sottoscrivere le pubblicazioni](../../relational-databases/replication/subscribe-to-publications.md)   
 [sp_addpullsubscription_agent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md)   
 [sp_change_subscription_properties &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-change-subscription-properties-transact-sql.md)   
 [sp_droppullsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-droppullsubscription-transact-sql.md)   
 [sp_helppullsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helppullsubscription-transact-sql.md)   
 [sp_helpsubscription_properties &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
