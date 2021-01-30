---
title: Creare una sottoscrizione push | Microsoft Docs
description: Informazioni su come creare una sottoscrizione push in SQL Server tramite SQL Server Management Studio, Transact-SQL o Replication Management Objects.
ms.custom: ''
ms.date: 08/25/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- push subscriptions [SQL Server replication], creating
- merge replication subscribing [SQL Server replication], push subscriptions
- subscriptions [SQL Server replication], push
- snapshot replication [SQL Server], subscribing
- transactional replication, subscribing
ms.assetid: adfbbc61-58d1-4330-9ad6-b14ab1142e2b
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: 9cb0106cc23c6c7bbae05528b82ad5d83617cefe
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076173"
---
# <a name="create-a-push-subscription"></a>Creare una sottoscrizione push
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  In questo argomento viene descritto come creare una sottoscrizione push in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]o RMO (Replication Management Objects). Per informazioni sulla creazione di una sottoscrizione push per un Sottoscrittore non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Creare una sottoscrizione per un Sottoscrittore non SQL Server](../../relational-databases/replication/create-a-subscription-for-a-non-sql-server-subscriber.md).  

[!INCLUDE[azure-sql-db-replication-supportability-note](../../includes/azure-sql-db-replication-supportability-note.md)]
  
 
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
Creare una sottoscrizione push nel server di pubblicazione o nel Sottoscrittore usando la Creazione guidata nuova sottoscrizione. Attenersi alle indicazioni presenti nelle pagine della procedura guidata per:  
  
- Specificare il server di pubblicazione e la pubblicazione.  
  
- Selezionare la posizione di esecuzione degli agenti di replica. Per una sottoscrizione push, selezionare **Esegui tutti gli agenti nel server di distribuzione (sottoscrizioni push)** nella pagina **Posizione in cui eseguire l'agente di distribuzione** o nella pagina **Posizione in cui eseguire l'agente di merge** , in base al tipo di pubblicazione.  
  
- Specificare i Sottoscrittori e i database di sottoscrizione.  
  
- Specificare gli accessi e le password utilizzati per le connessioni stabilite dagli agenti di replica:  
  
  - Per effettuare sottoscrizioni di pubblicazioni snapshot e transazionali, specificare le credenziali nella pagina **Sicurezza agente di distribuzione** .  
  
  - Per effettuare sottoscrizioni di pubblicazioni di tipo merge, specificare le credenziali nella pagina **Sicurezza agente di merge** .  
  
    Per informazioni sulle autorizzazioni richieste per ogni agente, vedere [Modello di sicurezza dell'agente di replica](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
- Specificare una pianificazione della sincronizzazione e definire quando il Sottoscrittore dovrà essere inizializzato.  
  
- Specificare le opzioni aggiuntive per le pubblicazioni di tipo merge, ovvero tipo di sottoscrizione e valori per il filtro con parametri.  
  
- Specificare le opzioni aggiuntive per le pubblicazioni transazionali che consentono l'aggiornamento delle sottoscrizioni. Un'opzione consiste nel decidere se i Sottoscrittori devono eseguire immediatamente il commit delle modifiche nel server di pubblicazione o scriverle in una coda. Un'altra opzione consiste nel configurare le credenziali usate per la connessione dal Sottoscrittore al server di pubblicazione.  
  
- Facoltativamente, creare uno script per la sottoscrizione.  
  
#### <a name="to-create-a-push-subscription-from-the-publisher"></a>Per creare una sottoscrizione push dal server di pubblicazione  
  
1. Connettersi al server di pubblicazione in [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] e quindi espandere il nodo del server.  
  
2. Espandere la cartella **Replica** e quindi la cartella **Pubblicazioni locali** .  
  
3. Fare clic con il pulsante destro del mouse sulla pubblicazione per la quale si vuole creare una o più sottoscrizioni e scegliere **Nuove sottoscrizioni**.  
  
4. Completare i passaggi della Creazione guidata nuova sottoscrizione.  
  
#### <a name="to-create-a-push-subscription-from-the-subscriber"></a>Per creare una sottoscrizione push dal Sottoscrittore  
  
1. Connettersi al Sottoscrittore in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]e quindi espandere il nodo del server.  
  
2. Espandere la cartella **Replica** .  
  
3. Fare clic col pulsante destro del mouse sulla cartella **Sottoscrizioni locali** e quindi scegliere **Nuove sottoscrizioni**.  
  
4. Nella pagina **Pubblicazione** della Creazione guidata nuova sottoscrizione selezionare **\<Find SQL Server Publisher>** o **\<Find Oracle Publisher>** nell'elenco a discesa **Server di pubblicazione**.  
  
5. Connettersi al server di pubblicazione nella finestra di dialogo **Connetti a server** .  
  
6. Selezionare una pubblicazione nella pagina **Pubblicazione** .  
  
7. Completare i passaggi della Creazione guidata nuova sottoscrizione.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
È possibile creare sottoscrizioni push a livello di programmazione tramite le store procedure di replica. Le stored procedure utilizzate dipenderanno dal tipo di pubblicazione a cui appartiene la sottoscrizione.  
  
> [!IMPORTANT]
> Se possibile, richiedere agli utenti di immettere le credenziali di sicurezza in fase di esecuzione. Se è necessario archiviare le credenziali in un file script, è fondamentale proteggere il file per evitare accessi non autorizzati.  
  
#### <a name="to-create-a-push-subscription-to-a-snapshot-or-transactional-publication"></a>Per creare una sottoscrizione push di una pubblicazione snapshot o transazionale  
  
1. Nel database di pubblicazione del server di pubblicazione eseguire [sp_helppublication](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md) per verificare che la pubblicazione supporti le sottoscrizioni push.  
  
   - Se il valore di **allow_push** è **1**, le sottoscrizioni push sono supportate.  
  
   - Se il valore di **allow_push** è **0**, eseguire [sp_changepublication](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md). Specificare **allow_push** per **\@property** e **true** per **\@value**.  
  
2. Nel database di pubblicazione del server di pubblicazione eseguire [sp_addsubscription](../system-stored-procedures/sp-addsubscription-transact-sql.md). Specificare **\@publication**, **\@subscriber** e **\@destination_db**. Specificare il valore **push** per **\@subscription_type**. Per informazioni sull'aggiornamento delle sottoscrizioni, vedere [Creare una sottoscrizione aggiornabile di una pubblicazione transazionale](publish/create-an-updatable-subscription-to-a-transactional-publication.md).  
  
3. Nel database di pubblicazione del server di pubblicazione eseguire [sp_addpushsubscription_agent](../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md). Specificare le opzioni seguenti:  
  
   - I parametri **\@subscriber**, **\@subscriber_db** e **\@publication**.  
  
   - Le credenziali di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows usate per l'esecuzione dell'agente di distribuzione nel server di distribuzione per **\@job_login** e **\@job_password**.  
  
     > [!NOTE]
     > Per le connessioni effettuate tramite l'autenticazione integrata di Windows vengono usate sempre le credenziali di Windows specificate da **\@job_login** e **\@job_password**. L'agente di distribuzione esegue sempre la connessione locale al server di distribuzione usando l'autenticazione integrata di Windows. Per impostazione predefinita, l'agente si connette al Sottoscrittore usando l'autenticazione integrata di Windows.  
  
   - (Facoltativo) Il valore **0** per **\@subscriber_security_mode** e le informazioni sull'account di accesso di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per **\@subscriber_login** e **\@subscriber_password**. Specificare questi parametri se è necessario utilizzare l'autenticazione di SQL Server per la connessione al Sottoscrittore.  
  
   - Specificare una pianificazione per il processo dell'agente di distribuzione da eseguire per la sottoscrizione. Per altre informazioni, vedere [Specificare le pianificazioni di sincronizzazione](../../relational-databases/replication/specify-synchronization-schedules.md).  
  
> [!IMPORTANT]
> Quando si crea una sottoscrizione push in un server di pubblicazione con un server di distribuzione remoto, i valori specificati per tutti i parametri, compresi *job_login* e *job_password*, vengono inviati al server di distribuzione come testo normale. È consigliabile crittografare la connessione tra il server di pubblicazione e il server di distribuzione remoto prima di eseguire questa stored procedure. Per altre informazioni, vedere [Abilitare le connessioni crittografate al motore di database &#40;Gestione configurazione SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
#### <a name="to-create-a-push-subscription-to-a-merge-publication"></a>Per creare una sottoscrizione push di una pubblicazione di tipo merge  
  
1. Nel database di pubblicazione del server di pubblicazione eseguire [sp_helpmergepublication](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md) per verificare che la pubblicazione supporti le sottoscrizioni push.  
  
   - Se il valore di **allow_push** è **1**, la pubblicazione supporta le sottoscrizioni push.  
  
   - Se il valore di **allow_push** non è **1**, eseguire [sp_changemergepublication](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md). Specificare **allow_push** per **\@property** e **true** per **\@value**.  
  
2. Nel database di pubblicazione del server di pubblicazione eseguire [sp_addmergesubscription](../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md). Specificare i parametri seguenti:  
  
   - **\@publication**. Nome della pubblicazione.  
  
   - **\@subscriber_type**. Per una sottoscrizione client, specificare **local**. Per una sottoscrizione server, specificare **global**.  
  
   - **\@subscription_priority**. Per una sottoscrizione server, specificare una priorità per la pubblicazione utilizzando un valore compreso tra **0.00** e **99.99**.  
  
   Per altre informazioni, vedere [Rilevamento e risoluzione avanzati dei conflitti nella replica di tipo merge](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md).  
  
3. Nel database di pubblicazione del server di pubblicazione eseguire [sp_addmergepushsubscription_agent](../../relational-databases/system-stored-procedures/sp-addmergepushsubscription-agent-transact-sql.md). Specificare le opzioni seguenti:  
  
   - I parametri **\@subscriber**, **\@subscriber_db** e **\@publication**.  
  
   - Le credenziali di Windows usate per l'esecuzione dell'agente di merge nel server di distribuzione per **\@job_login** e **\@job_password**.  
  
     > [!NOTE]
     > Per le connessioni effettuate tramite l'autenticazione integrata di Windows vengono usate sempre le credenziali di Windows specificate da **\@job_login** e **\@job_password**. L'agente di merge esegue sempre la connessione locale al server di distribuzione usando l'autenticazione integrata di Windows. Per impostazione predefinita, l'agente si connette al Sottoscrittore usando l'autenticazione integrata di Windows.  
  
   - (Facoltativo) Il valore **0** per **\@subscriber_security_mode** e le informazioni sull'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per **\@subscriber_login** e **\@subscriber_password**. Specificare questi parametri se è necessario utilizzare l'autenticazione di SQL Server per la connessione al Sottoscrittore.  
  
   - (Facoltativo) Il valore **0** per **\@publisher_security_mode** e le informazioni sull'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per **\@publisher_login** e **\@publisher_password**. Specificare questi valori se è necessario utilizzare l'autenticazione di SQL Server per la connessione al server di pubblicazione.  
  
   - Una pianificazione per il processo dell'agente di merge per la sottoscrizione. Per altre informazioni, vedere [Specificare le pianificazioni di sincronizzazione](../../relational-databases/replication/specify-synchronization-schedules.md).  
  
> [!IMPORTANT]
> Quando si crea una sottoscrizione push in un server di pubblicazione con un server di distribuzione remoto, i valori specificati per tutti i parametri, compresi *job_login* e *job_password*, vengono inviati al server di distribuzione come testo normale. È consigliabile crittografare la connessione tra il server di pubblicazione e il server di distribuzione remoto prima di eseguire questa stored procedure. Per altre informazioni, vedere [Abilitare le connessioni crittografate al motore di database &#40;Gestione configurazione SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Esempi (Transact-SQL)  
 Nell'esempio seguente viene creata una sottoscrizione push a una pubblicazione transazionale. I valori per l'account di accesso e la relativa password vengono specificati in fase di esecuzione tramite variabili di scripting **sqlcmd**.  
  
 [!code-sql[HowTo#sp_addtranpushsubscription_agent](../../relational-databases/replication/codesnippet/tsql/create-a-push-subscription_1.sql)]  
  
 Nell'esempio seguente viene creata una sottoscrizione push a una pubblicazione di tipo merge. I valori per l'account di accesso e la relativa password vengono specificati in fase di esecuzione tramite variabili di scripting **sqlcmd**.  
  
 [!code-sql[HowTo#sp_addmergepushsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/create-a-push-subscription_2.sql)]  
  
##  <a name="using-replication-management-objects"></a><a name="RMOProcedure"></a> Uso di RMO (Replication Management Objects)  
 È possibile creare sottoscrizioni push a livello di programmazione tramite gli oggetti RMO (Replication Management Objects). Le classi RMO usate per la creazione di una sottoscrizione push dipendono dal tipo di pubblicazione per cui viene creata la sottoscrizione.  
  
> [!IMPORTANT]
> Se possibile, richiedere agli utenti di immettere le credenziali di sicurezza in fase di esecuzione. Se è necessario archiviare le credenziali, usare i [servizi di crittografia](/previous-versions/aa719848(v=vs.71)) offerti da [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows .NET Framework.  
  
#### <a name="to-create-a-push-subscription-to-a-snapshot-or-transactional-publication"></a>Per creare una sottoscrizione push di una pubblicazione snapshot o transazionale  
  
1. Creare una connessione al server di pubblicazione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2. Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.TransPublication> utilizzando la connessione al server di pubblicazione creata nel passaggio 1. Specificare <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>, <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A>e <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3. Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Se il metodo restituisce **false**, le proprietà specificate nel passaggio 2 non sono corrette oppure la pubblicazione non esiste nel server.  
  
4. Eseguire un'operazione con AND logico bit per bit ( **&** in Visual C# e **And** in Visual Basic) tra la proprietà <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> e <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPush>. Se il risultato è <xref:Microsoft.SqlServer.Replication.PublicationAttributes.None>, impostare <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> sul risultato di un'operazione con OR logico bit per bit ( **|** in Visual C# e **Or** in Visual Basic) tra <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> e <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPush>. Chiamare quindi <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> per abilitare le sottoscrizioni push.  
  
5. Se il database di sottoscrizione non esiste, crearlo utilizzando la classe <xref:Microsoft.SqlServer.Management.Smo.Database> . Per altre informazioni, vedere [Creazione, modifica e rimozione di database](../../relational-databases/server-management-objects-smo/tasks/creating-altering-and-removing-databases.md).  
  
6. Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.TransSubscription>.  
  
7. Impostare le proprietà seguenti per la sottoscrizione:  
  
   - La connessione <xref:Microsoft.SqlServer.Management.Common.ServerConnection> al server di pubblicazione creata nel passaggio 1 per <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
   - Nome del database di sottoscrizione per <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A>.  
  
   - Nome del Sottoscrittore per <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>. 
  
   - Nome del database di pubblicazione per <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>.  
  
   - Nome della pubblicazione per <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>.
  
   - I parametri <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> e <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> di <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A> per fornire le credenziali per l'account di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows con il quale verrà eseguito l'agente di distribuzione nel server di distribuzione. Questo account viene usato per attivare connessioni locali al server di distribuzione e stabilire connessioni remote tramite l'autenticazione di Windows.  
  
     > [!NOTE]
     > Non è obbligatorio impostare <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A> quando la sottoscrizione viene creata da un membro del ruolo predefinito del server **sysadmin**, ma si tratta di un'impostazione consigliata. In questo caso l'agente rappresenterà l'account di SQL Server Agent. Per altre informazioni, vedere [Modello di sicurezza dell'agente di replica](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
   - (Facoltativo) Il valore **true** (impostazione predefinita) per <xref:Microsoft.SqlServer.Replication.Subscription.CreateSyncAgentByDefault%2A> per creare un processo dell'agente, utilizzato per sincronizzare la sottoscrizione. Se si specifica **false**, la sottoscrizione può essere sincronizzata solo a livello di programmazione.  
  
   - (Facoltativo) Impostare i campi <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> e <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> o <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> di <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberSecurity%2A> quando si utilizza l'autenticazione di SQL Server per la connessione al Sottoscrittore.  
  
8. Chiamare il metodo <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A> .  
  
> [!IMPORTANT]
> Quando si crea una sottoscrizione push in un server di pubblicazione con un server di distribuzione remoto, i valori specificati per tutte le proprietà, compresa <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A>, vengono inviati al server di distribuzione come testo normale. È consigliabile crittografare la connessione tra il server di pubblicazione e il server di distribuzione remoto prima di chiamare il metodo <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A>. Per altre informazioni, vedere [Abilitare le connessioni crittografate al motore di database &#40;Gestione configurazione SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
#### <a name="to-create-a-push-subscription-to-a-merge-publication"></a>Per creare una sottoscrizione push di una pubblicazione di tipo merge  
  
1. Creare una connessione al server di pubblicazione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2. Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.MergePublication> utilizzando la connessione al server di pubblicazione creata nel passaggio 1. Specificare <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>, <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A>e <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3. Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Se il metodo restituisce **false**, le proprietà specificate nel passaggio 2 non sono corrette oppure la pubblicazione non esiste nel server.  
  
4. Eseguire un'operazione con AND logico bit per bit ( **&** in Visual C# e **And** in Visual Basic) tra la proprietà <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> e <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPush>. Se il risultato è <xref:Microsoft.SqlServer.Replication.PublicationAttributes.None>, impostare <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> sul risultato di un'operazione con OR logico bit per bit ( **|** in Visual C# e **Or** in Visual Basic) tra <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> e <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPush>. Chiamare quindi <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> per abilitare le sottoscrizioni push.  
  
5. Se il database di sottoscrizione non esiste, crearlo utilizzando la classe <xref:Microsoft.SqlServer.Management.Smo.Database> . Per altre informazioni, vedere [Creazione, modifica e rimozione di database](../../relational-databases/server-management-objects-smo/tasks/creating-altering-and-removing-databases.md).  
  
6. Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.MergeSubscription>.  
  
7. Impostare le proprietà seguenti per la sottoscrizione:  
  
   - La connessione <xref:Microsoft.SqlServer.Management.Common.ServerConnection> al server di pubblicazione creata nel passaggio 1 per <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
   - Nome del database di sottoscrizione per <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A>.  
  
   - Nome del Sottoscrittore per <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>.    
   - Nome del database di pubblicazione per <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>.  
  
   - Nome della pubblicazione per <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>.    
   - I parametri <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> e <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> di <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A> per fornire le credenziali per l'account di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows con il quale verrà eseguito l'agente di merge nel server di distribuzione. Questo account viene usato per attivare connessioni locali al server di distribuzione e stabilire connessioni remote tramite l'autenticazione di Windows.  
  
     > [!NOTE]
     > Non è obbligatorio impostare <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A> quando la sottoscrizione viene creata da un membro del ruolo predefinito del server **sysadmin**, ma si tratta di un'impostazione consigliata. In questo caso l'agente rappresenterà l'account di SQL Server Agent. Per altre informazioni, vedere [Modello di sicurezza dell'agente di replica](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
   - (Facoltativo) Il valore **true** (impostazione predefinita) per <xref:Microsoft.SqlServer.Replication.Subscription.CreateSyncAgentByDefault%2A> per creare un processo dell'agente, utilizzato per sincronizzare la sottoscrizione. Se si specifica **false**, la sottoscrizione può essere sincronizzata solo a livello di programmazione.  
  
   - (Facoltativo) Impostare i campi <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> e <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> o <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> di <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberSecurity%2A> quando si utilizza l'autenticazione di SQL Server per la connessione al Sottoscrittore.  
  
   - (Facoltativo) Impostare i campi <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> e <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> o <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> di <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherSecurity%2A> quando si utilizza l'autenticazione di SQL Server per la connessione al server di pubblicazione.  
  
8. Chiamare il metodo <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A> .  
  
> [!IMPORTANT]  
> Quando si crea una sottoscrizione push in un server di pubblicazione con un server di distribuzione remoto, i valori specificati per tutte le proprietà, compresa <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A>, vengono inviati al server di distribuzione come testo normale. È consigliabile crittografare la connessione tra il server di pubblicazione e il server di distribuzione remoto prima di chiamare il metodo <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A>. Per altre informazioni, vedere [Abilitare le connessioni crittografate al motore di database &#40;Gestione configurazione SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Esempi (RMO)  
 In questo esempio viene creata una nuova sottoscrizione push di una pubblicazione transazionale. Le credenziali dell'account di Windows usate per eseguire il processo dell'agente di distribuzione vengono passate in fase di esecuzione.  
  
 [!code-cs[HowTo#rmo_CreateTranPushSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createtranpushsub)]  
  
 [!code-vb[HowTo#rmo_vb_CreateTranPushSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createtranpushsub)]  
  
 In questo esempio viene creata una nuova sottoscrizione push di una pubblicazione di tipo merge. Le credenziali dell'account di Windows usate per eseguire il processo dell'agente di merge vengono passate in fase di esecuzione.  
  
 [!code-cs[HowTo#rmo_CreateMergePushSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createmergepushsub)]  
  
 [!code-vb[HowTo#rmo_vb_CreateMergePushSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createmergepushsub)]  
  
## <a name="see-also"></a>Vedere anche  
 [Visualizzare e modificare le proprietà delle sottoscrizioni push](../../relational-databases/replication/view-and-modify-push-subscription-properties.md)   
 [Procedure consigliate per la sicurezza della replica](../../relational-databases/replication/security/replication-security-best-practices.md)   
 [Creare una pubblicazione](../../relational-databases/replication/publish/create-a-publication.md)   
 [Concetti di base relativi a RMO (Replication Management Objects)](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [Sincronizzare una sottoscrizione push](../../relational-databases/replication/synchronize-a-push-subscription.md)   
 [Sottoscrivere le pubblicazioni](../../relational-databases/replication/subscribe-to-publications.md)   
 [Usare sqlcmd con variabili di scripting](../../ssms/scripting/sqlcmd-use-with-scripting-variables.md)  
  
