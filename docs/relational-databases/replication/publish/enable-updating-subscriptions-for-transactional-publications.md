---
title: Abilitare le sottoscrizioni aggiornabili per le pubblicazioni transazionali
description: Informazioni su come abilitare le sottoscrizioni aggiornabili per una pubblicazione transazionale in SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- transactional replication, updatable subscriptions
- updatable subscriptions, enabling
- subscriptions [SQL Server replication], updatable
ms.assetid: 539d5bb0-b808-4d8c-baf4-cb6d32d2c595
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 2d31aa07018e7c41ce4922428d7196eb66c050b8
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076689"
---
# <a name="enable-updating-subscriptions-for-transactional-publications"></a>Abilitazione delle sottoscrizioni aggiornabili per le pubblicazioni transazionali
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene descritto come abilitare l'aggiornamento delle sottoscrizioni per pubblicazioni transazionali in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
> **NOTA** [!INCLUDE[ssNoteDepFutureAvoid](../../../includes/ssnotedepfutureavoid-md.md)]  
  

##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
 Se possibile, richiedere agli utenti di immettere le credenziali di sicurezza in fase di esecuzione. Se è necessario archiviare le credenziali in un file script, è fondamentale proteggere il file per evitare accessi non autorizzati.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Con SQL Server Management Studio  
 Abilitare le sottoscrizioni aggiornabili per le pubblicazioni transazionali nella pagina **Tipo di pubblicazione** della Creazione guidata nuova pubblicazione.  
  
 Per utilizzare le sottoscrizioni aggiornabili, è inoltre necessario configurare le opzioni della Creazione guidata nuova sottoscrizione.  
  
#### <a name="to-enable-updating-subscriptions"></a>Per attivare le sottoscrizioni aggiornabili  
  
1.  Nella pagina **Tipo di pubblicazione** della Creazione guidata nuova pubblicazione, selezionare **Pubblicazione transazionale con sottoscrizioni aggiornabili**.  
  
2.  Nella pagina **Sicurezza agente** , specificare le impostazioni di sicurezza per l'agente di lettura coda, per l'agente snapshot e per l'agente di lettura log. Per ulteriori informazioni sulle autorizzazioni necessarie per l'account con cui viene eseguito l'agente di lettura coda, vedere [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md).  

    > **NOTA:** L'agente di lettura coda viene configurato anche se si usano solo le sottoscrizioni ad aggiornamento immediato.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Con Transact-SQL  
 Quando si crea una pubblicazione transazionale a livello di programmazione utilizzando stored procedure di replica, è possibile abilitare sottoscrizioni ad aggiornamento immediato o in coda.  
  
#### <a name="to-create-a-publication-that-supports-immediate-updating-subscriptions"></a>Per creare una pubblicazione che supporta sottoscrizioni ad aggiornamento immediato  
  
1.  Se necessario, creare un processo dell'agente di lettura log per il database di pubblicazione.  
  
    -   Se per il database di pubblicazione esiste già un processo dell'agente di lettura log, procedere con il passaggio 2.  
  
    -   Per sapere se un processo dell'agente di lettura log esiste già per un database pubblicato, eseguire [sp_helplogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helplogreader-agent-transact-sql.md) nel database di pubblicazione nel server di pubblicazione. Se il set di risultati è vuoto, sarà necessario creare un processo dell'agente di lettura log.  
  
    -   Nel server di pubblicazione eseguire [sp_addlogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md). Specificare le credenziali di [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows usate per l'esecuzione dell'agente per **\@job_name** e **\@password**. Se l'agente userà l'autenticazione di SQL Server per la connessione al server di pubblicazione, è anche necessario specificare il valore **0** per **\@publisher_security_mode** e le informazioni sull'account di accesso di [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per **\@publisher_login** e **\@publisher_password**.  
  
2.  Eseguire [sp_addpublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md), specificando il valore **true** per il parametro **\@allow_sync_tran**.  
  
3.  Nel server di pubblicazione eseguire [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md). Specificare il nome della pubblicazione usato nel passaggio 2 per **\@publication** e le credenziali di Windows usate per l'agente snapshot per **\@job_name** e **\@password**. Se l'agente userà l'autenticazione di SQL Server per la connessione al server di pubblicazione, è anche necessario specificare il valore **0** per **\@publisher_security_mode** e le informazioni sull'account di accesso di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per **\@publisher_login** e **\@publisher_password**. Verrà creato un processo dell'agente snapshot per la pubblicazione.  
  
4.  Aggiungere articoli alla pubblicazione. Per altre informazioni, vedere [definire un articolo](../../../relational-databases/replication/publish/define-an-article.md).  
  
5.  Nel Sottoscrittore creare una sottoscrizione aggiornabile per la pubblicazione.   
  
#### <a name="to-create-a-publication-that-supports-queued-updating-subscriptions"></a>Per creare una pubblicazione che supporta sottoscrizioni ad aggiornamento in coda  
  
1.  Se necessario, creare un processo dell'agente di lettura log per il database di pubblicazione.  
  
    -   Se per il database di pubblicazione esiste già un processo dell'agente di lettura log, procedere con il passaggio 2.  
  
    -   Per sapere se un processo dell'agente di lettura log esiste già per un database pubblicato, eseguire [sp_helplogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helplogreader-agent-transact-sql.md) nel database di pubblicazione nel server di pubblicazione. Se il set di risultati è vuoto, sarà necessario creare un processo dell'agente di lettura log.  
  
    -   Nel server di pubblicazione eseguire [sp_addlogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md). Specificare le credenziali di Windows usate per l'esecuzione dell'agente per **\@job_name** e **\@password**. Se l'agente userà l'autenticazione di SQL Server per la connessione al server di pubblicazione, è anche necessario specificare il valore **0** per **\@publisher_security_mode** e le informazioni sull'account di accesso di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per **\@publisher_login** e **\@publisher_password**.  
  
2.  Se necessario, creare un processo dell'agente di lettura coda per il server di distribuzione.  
  
    -   Se per il database di distribuzione esiste già un processo dell'agente di lettura coda, procedere con il passaggio 3.  
  
    -   Per sapere se un processo dell'agente di lettura coda esiste già per un database di distribuzione, eseguire [sp_helpqreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpqreader-agent-transact-sql.md) nel database di distribuzione nel server di distribuzione. Se il set di risultati è vuoto, sarà necessario creare un processo dell'agente di lettura coda.  
  
    -   Nel database di distribuzione eseguire [sp_addqreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addqreader-agent-transact-sql.md). Specificare le credenziali di Windows usate per l'esecuzione dell'agente per **\@job_name** e **\@password**. Queste credenziali vengono utilizzate quando l'agente di lettura coda si connette al server di pubblicazione e al Sottoscrittore. Per altre informazioni, vedere [Modello di sicurezza dell'agente di replica](../../../relational-databases/replication/security/replication-agent-security-model.md).  
  
3.  Eseguire [sp_addpublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md), specificando il valore **true** per il parametro **\@allow_queued_tran** e il valore **pub wins**, **sub reinit** o **sub wins** per **\@conflict_policy**.  
  
4.  Nel server di pubblicazione eseguire [sp_addpublication_snapshot (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md). Specificare il nome della pubblicazione usato nel passaggio 3 per **\@publication** e le credenziali di Windows usate per l'agente di snapshot per **\@snapshot_job_name** e **\@password**. Se l'agente userà l'autenticazione di SQL Server per la connessione al server di pubblicazione, è anche necessario specificare il valore **0** per **\@publisher_security_mode** e le informazioni sull'account di accesso di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per **\@publisher_login** e **\@publisher_password**. Verrà creato un processo dell'agente snapshot per la pubblicazione.  
  
5.  Aggiungere articoli alla pubblicazione. Per altre informazioni, vedere [definire un articolo](../../../relational-databases/replication/publish/define-an-article.md).  
  
6.  Nel Sottoscrittore creare una sottoscrizione aggiornabile per la pubblicazione.  
  
#### <a name="to-change-the-conflict-policy-for-a-publication-that-allows-queued-updating-subscriptions"></a>Per modificare i criteri per la gestione dei conflitti per una pubblicazione che consente le sottoscrizioni ad aggiornamento in coda  
  
1.  Nel database di pubblicazione nel server di pubblicazione eseguire [sp_changepublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md). Specificare il valore **conflict_policy** per **\@property** e i criteri desiderati per la gestione dei conflitti **pub wins**, **sub reinit** o **sub wins** per **\@value**.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Esempio (Transact-SQL)  
 In questo esempio viene creata una pubblicazione che supporta le sottoscrizioni pull ad aggiornamento immediato e in coda.  
  
 [!code-sql[HowTo#sp_createtranupdatingpub](../../../relational-databases/replication/codesnippet/tsql/enable-updating-subscrip_1.sql)]  
  
## <a name="see-also"></a>Vedere anche  
 [Impostare le opzioni di risoluzione dei conflitti per l'aggiornamento in coda &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/publish/create-an-updatable-subscription-to-a-transactional-publication.md)   
 [Replica transazionale](../../../relational-databases/replication/transactional/transactional-replication.md)   
 [Updatable Subscriptions for Transactional Replication](../../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)   
 [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md)   
 [Create an Updatable Subscription to a Transactional Publication](create-an-updatable-subscription-to-a-transactional-publication.md)   
 [Updatable Subscriptions for Transactional Replication](../../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)   
 [Utilizzo di sqlcmd con variabili di scripting](../../../ssms/scripting/sqlcmd-use-with-scripting-variables.md)  
  
