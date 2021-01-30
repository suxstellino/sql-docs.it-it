---
description: sp_addmergepullsubscription (Transact-SQL)
title: sp_addmergepullsubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addmergepullsubscription_TSQL
- sp_addmergepullsubscription
helpviewer_keywords:
- sp_addmergepullsubscription
ms.assetid: d63909a0-8ea7-4734-9ce8-8204d936a3e4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 54975d84e497f02dd28191c59041553994dac24d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209411"
---
# <a name="sp_addmergepullsubscription-transact-sql"></a>sp_addmergepullsubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Aggiunge una sottoscrizione pull a una pubblicazione di tipo merge. Questa stored procedure viene eseguita nel database di sottoscrizione del Sottoscrittore.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_addmergepullsubscription [ @publication= ] 'publication'   
    [ , [ @publisher= ] 'publisher' ]   
    [ , [ @publisher_db = ] 'publisher_db' ]   
    [ , [ @subscriber_type= ] 'subscriber_type' ]   
    [ , [ @subscription_priority= ] subscription_priority ]   
    [ , [ @sync_type= ] 'sync_type' ]   
    [ , [ @description= ] 'description' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione. *Publisher* è di **tipo sysname** e il valore predefinito è il nome del server locale. Il server di pubblicazione deve essere un server valido.  
  
`[ @publisher_db = ] 'publisher_db'` Nome del database del server di pubblicazione. *publisher_db* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @subscriber_type = ] 'subscriber_type'` Tipo di Sottoscrittore. *subscriber_type* è **di tipo nvarchar (15)** e può essere **globale**, **locale** o **Anonimo**. In [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] e versioni successive le sottoscrizioni locali vengono dette sottoscrizioni client e le sottoscrizioni globali vengono dette sottoscrizioni server.  
  
`[ @subscription_priority = ] subscription_priority` Priorità della sottoscrizione. *subscription_priority* è **reale** e il valore predefinito è null. Per le sottoscrizioni locali e anonime, la priorità è **0,0**. La priorità viene utilizzata dal sistema di risoluzione predefinito per eseguire una selezione in caso di conflitti. Per i Sottoscrittori globali, la priorità della sottoscrizione deve essere minore di 100, che corrisponde al livello di priorità del server di pubblicazione.  
  
`[ @sync_type = ] 'sync_type'` Tipo di sincronizzazione della sottoscrizione. *sync_type* è di **tipo nvarchar (15)** e il valore predefinito è **Automatic**. Può essere **automatico** o **None**. Se **automatico**, lo schema e i dati iniziali per le tabelle pubblicate vengono trasferiti per primi nel Sottoscrittore. Se non è presente **alcun** valore, si presuppone che il Sottoscrittore disponga già dello schema e dei dati iniziali per le tabelle pubblicate. Le tabelle e i dati di sistema vengono sempre trasferiti.  
  
> [!NOTE]  
>  Non è consigliabile specificare il valore **None**.  
  
`[ @description = ] 'description'` Breve descrizione della sottoscrizione pull. *Description* è di **tipo nvarchar (255)** e il valore predefinito è null. Questo valore viene visualizzato da monitoraggio replica nella colonna **nome descrittivo** , che può essere utilizzato per ordinare le sottoscrizioni per una pubblicazione monitorata.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_addmergepullsubscription** viene utilizzata per la replica di tipo merge.  
  
 Se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si utilizza Agent per sincronizzare la sottoscrizione, è necessario eseguire il [sp_addmergepullsubscription_agent](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md) stored procedure nel Sottoscrittore per creare un agente e un processo da sincronizzare con la pubblicazione.  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_addmergepullsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/sp-addmergepullsubscript_0_1.sql)]  
  
 [!code-sql[HowTo#sp_addmergepullsub_websync_anon](../../relational-databases/replication/codesnippet/tsql/sp-addmergepullsubscript_0_2.sql)]  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_addmergepullsubscription**.  
  
## <a name="see-also"></a>Vedere anche  
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [Subscribe to Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [sp_addmergepullsubscription_agent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md)   
 [sp_changemergepullsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changemergepullsubscription-transact-sql.md)   
 [sp_dropmergepullsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropmergepullsubscription-transact-sql.md)   
 [sp_helpmergepullsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpmergepullsubscription-transact-sql.md)   
 [sp_helpsubscription_properties &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)  
  
  
