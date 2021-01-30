---
description: sp_link_publication (Transact-SQL)
title: sp_link_publication (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_link_publication_TSQL
- sp_link_publication
helpviewer_keywords:
- sp_link_publication
ms.assetid: 1945ed24-f9f1-4af6-94ca-16d8e864706e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b6a6e0b057f0d01c838aa5ac7b57143221fec13c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210782"
---
# <a name="sp_link_publication-transact-sql"></a>sp_link_publication (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Imposta le informazioni di configurazione e sicurezza utilizzate dai trigger di sincronizzazione dei server di sottoscrizione ad aggiornamento immediato durante la connessione al server di pubblicazione. Questa stored procedure viene eseguita nel database di sottoscrizione del Sottoscrittore.  
  
> [!IMPORTANT]
>  Quando si configura un server di pubblicazione con un server di distribuzione remoto, i valori specificati per tutti i parametri, inclusi *job_login* e *job_password*, vengono inviati al server di distribuzione come testo normale. È consigliabile crittografare la connessione tra il server di pubblicazione e il server di distribuzione remoto prima di eseguire questa stored procedure. Per altre informazioni, vedere [Abilitare le connessioni crittografate al motore di database &#40;Gestione configurazione SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
> 
> [!IMPORTANT]
>  In determinate condizioni, questo stored procedure può non riuscire se il Sottoscrittore esegue [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 1 o versione successiva e il server di pubblicazione esegue una versione precedente. In caso di esito negativo della stored procedure in questo scenario, aggiornare il server di pubblicazione a [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 1 o versioni successive.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_link_publication [ @publisher = ] 'publisher'   
        , [ @publisher_db = ] 'publisher_db'   
        , [ @publication = ] 'publication'   
        , [ @security_mode = ] security_mode  
    [ , [ @login = ] 'login' ]  
    [ , [ @password = ]'password' ]  
    [ , [ @distributor = ] 'distributor' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione a cui collegarsi. *Publisher* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publisher_db = ] 'publisher_db'` Nome del database del server di pubblicazione a cui collegarsi. *publisher_db* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publication = ] 'publication'` Nome della pubblicazione a cui collegarsi. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @security_mode = ] security_mode` Modalità di sicurezza utilizzata dal Sottoscrittore per la connessione a un server di pubblicazione remoto per l'aggiornamento immediato. *security_mode* è di **tipo int**. i possibili valori sono i seguenti. [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**0**|Usa l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticazione di con l'account di accesso specificato in questo stored procedure come *account di accesso* e *password*.<br /><br /> Nota: nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questa opzione è stata usata per specificare una chiamata di procedura remota (RPC) dinamica.|  
|**1**|Utilizza il contesto di sicurezza (autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o autenticazione di Windows) dell'utente che esegue la modifica nel Sottoscrittore.<br /><br /> Nota: questo account deve esistere anche nel server di pubblicazione con privilegi sufficienti. Se si utilizza l'autenticazione di Windows è necessario che sia supportata la delega degli account di sicurezza.|  
|**2**|Usa un account di accesso del server collegato definito dall'utente esistente creato con **sp_link_publication**.|  
  
`[ @login = ] 'login'` Account di accesso. *login* è di tipo **sysname** e il valore predefinito è NULL. Questo parametro deve essere specificato quando *security_mode* è **0**.  
  
`[ @password = ] 'password'` Password. *password* è di **tipo sysname** e il valore predefinito è null. Questo parametro deve essere specificato quando *security_mode* è **0**.  
  
`[ @distributor = ] 'distributor'` Nome del server di distribuzione. *Distributor* è di **tipo sysname** e il valore predefinito è null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_link_publication** viene utilizzata dalle sottoscrizioni ad aggiornamento immediato nella replica transazionale.  
  
 **sp_link_publication** può essere utilizzato sia per le sottoscrizioni push che per le sottoscrizioni pull. Questa stored procedure può essere chiamata prima o dopo la creazione della sottoscrizione. Una voce viene inserita o aggiornata nell'MSsubscription_properties &#40;tabella di sistema [&#41;Transact-SQL ](../../relational-databases/system-tables/mssubscription-properties-transact-sql.md) .  
  
 Per le sottoscrizioni push, la voce può essere pulita [sp_subscription_cleanup &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-subscription-cleanup-transact-sql.md). Per le sottoscrizioni pull, la voce può essere pulita [sp_droppullsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-droppullsubscription-transact-sql.md) o [Sp_subscription_cleanup &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-subscription-cleanup-transact-sql.md). È inoltre possibile chiamare **sp_link_publication** con una password null per cancellare la voce nella MSsubscription_properties &#40;tabella di sistema [&#41;Transact-SQL](../../relational-databases/system-tables/mssubscription-properties-transact-sql.md) per motivi di sicurezza.  
  
 La modalità predefinita utilizzata in un Sottoscrittore ad aggiornamento immediato per la connessione al server di pubblicazione non consente la connessione tramite l'autenticazione di Windows. Per eseguire la connessione con questa modalità di autenticazione, è necessario configurare un server collegato per il server di pubblicazione e il Sottoscrittore ad aggiornamento immediato deve utilizzare questa connessione per l'aggiornamento del Sottoscrittore. Questa operazione richiede l'esecuzione del **sp_link_publication** con *security_mode*  =  **2**. Se si utilizza l'autenticazione di Windows è necessario che sia supportata la delega degli account di sicurezza.  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_addtranpullsubscriptionagent_failover](../../relational-databases/replication/codesnippet/tsql/sp-link-publication-tran_1.sql)]  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_link_publication**.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_droppullsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-droppullsubscription-transact-sql.md)   
 [sp_helpsubscription_properties &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)   
 [sp_subscription_cleanup &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-subscription-cleanup-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
