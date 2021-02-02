---
title: Sincronizzare una sottoscrizione push | Microsoft Docs
description: Informazioni su come sincronizzare una sottoscrizione push in SQL Server tramite SQL Server Management Studio, agenti di replica o Replication Management Objects.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- synchronization [SQL Server replication], push subscriptions
- subscriptions [SQL Server replication], push
- push subscriptions [SQL Server replication], synchronizing
ms.assetid: 0cfa7ae5-91d3-4a4f-9edf-a852d45783b5
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: 14240f40dcd00f439c0aa5f368ebe4f331641414
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250563"
---
# <a name="synchronize-a-push-subscription"></a>Sincronizzazione di una sottoscrizione push
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  In questo argomento viene descritto come sincronizzare una sottoscrizione push in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [agenti di replica](../../relational-databases/replication/agents/replication-agents-overview.md)o RMO (Replication Management Objects).  
  
[!INCLUDE[azure-sql-db-replication-supportability-note](../../includes/azure-sql-db-replication-supportability-note.md)]

##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
 Le sottoscrizioni vengono sincronizzate dall'agente di distribuzione, per la replica snapshot e transazionale, o dall'agente di merge, per la replica di tipo merge. Gli agenti possono essere in esecuzione continuamente, essere in esecuzione su richiesta o essere in esecuzione su una pianificazione. Per altre informazioni sull'impostazione delle pianificazioni della sincronizzazione, vedere [Specificare le pianificazioni della sincronizzazione](../../relational-databases/replication/specify-synchronization-schedules.md).  
  
 Una sottoscrizione può essere sincronizzata su richiesta dalle cartelle **Pubblicazioni locali** e **Sottoscrizioni locali** in [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] e dalla scheda **Tutte le sottoscrizioni** in Monitoraggio replica. Le sottoscrizioni a pubblicazioni Oracle non possono essere sincronizzate su richiesta dal Sottoscrittore. Per informazioni sull'avvio di Monitoraggio replica, vedere [Avviare Monitoraggio replica](../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
#### <a name="to-synchronize-a-push-subscription-on-demand-in-management-studio-at-the-publisher"></a>Per sincronizzare una sottoscrizione push su richiesta in Management Studio (nel server di pubblicazione)  
  
1.  Connettersi al server di pubblicazione in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]e quindi espandere il nodo del server.  
  
2.  Espandere la cartella **Replica** e quindi la cartella **Pubblicazioni locali** .  
  
3.  Espandere la pubblicazione per cui sincronizzare le sottoscrizioni.  
  
4.  Fare clic con il pulsante destro del mouse sulla sottoscrizione che si desidera sincronizzare e quindi scegliere **Visualizza stato sincronizzazione**.  
  
5.  Nella finestra di dialogo **Visualizza stato sincronizzazione - \<Subscriber>:\<SubscriptionDatabase>** fare clic su **Avvia**. Al termine della sincronizzazione verrà visualizzato il messaggio **Sincronizzazione completata** .  
  
6.  Fare clic su **Close**.  

#### <a name="to-synchronize-a-push-subscription-on-demand-in-management-studio-at-the-subscriber"></a>Per sincronizzare una sottoscrizione push su richiesta in Management Studio (nel Sottoscrittore)  
  
1.  Connettersi al Sottoscrittore in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]e quindi espandere il nodo del server.  
  
2.  Espandere la cartella **Replica** e quindi la cartella **Sottoscrizioni locali** .  
  
3.  Fare clic con il pulsante destro del mouse sulla sottoscrizione che si desidera sincronizzare e quindi scegliere **Visualizza stato sincronizzazione**.  
  
4.  Viene visualizzato un messaggio in cui si chiede di stabilire una connessione al server di distribuzione. Fare clic su **OK**.  
  
5.  Nella finestra di dialogo **Visualizza stato sincronizzazione - \<Subscriber>:\<SubscriptionDatabase>** fare clic su **Avvia**. Al termine della sincronizzazione verrà visualizzato il messaggio **Sincronizzazione completata** .  
  
6.  Fare clic su **Close**.  
  
#### <a name="to-synchronize-a-push-subscription-on-demand-in-replication-monitor"></a>Per sincronizzare una sottoscrizione push su richiesta in Monitoraggio replica  
  
1.  In Monitoraggio replica espandere un gruppo di server di pubblicazione nel riquadro di sinistra, espandere un server di pubblicazione e quindi fare clic su una pubblicazione.  
  
2.  Fare clic sulla scheda **Tutte le sottoscrizioni** .  
  
3.  Fare clic con il pulsante destro del mouse sulla sottoscrizione che si desidera sincronizzare e quindi scegliere **Avvia sincronizzazione**.  
  
4.  Per visualizzare lo stato della sincronizzazione, fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere **Visualizza dettagli**.  
  
##  <a name="using-replication-agents"></a><a name="ReplProg"></a> Utilizzo degli agenti di replica  
 Le sottoscrizioni push possono essere sincronizzate a livello di programmazione e su richiesta richiamando il file eseguibile dell'agente di replica appropriato dal prompt dei comandi. Il file eseguibile dell'agente di replica richiamato dipenderà dal tipo di pubblicazione a cui appartiene la sottoscrizione push.  
  
#### <a name="to-start-the-distribution-agent-to-synchronize-a-push-subscription-to-a-transactional-publication"></a>Per avviare l'agente di distribuzione per sincronizzare una sottoscrizione push di una pubblicazione transazionale  
  
1.  Eseguire **distrib.exe** dal prompt dei comandi o in un file batch nel server di distribuzione. Specificare gli argomenti della riga di comando seguenti:  
  
    -   **-Publisher**  
  
    -   **-PublisherDB**  
  
    -   **-Distributor**  
  
    -   **-Subscriber**  
  
    -   **-SubscriberDB**  
  
    -   **-SubscriptionType = 0**  
  
     Se si usano l'autenticazione di SQL Server, è inoltre necessario specificare gli argomenti seguenti:  
  
    -   **-DistributorLogin**  
  
    -   **-DistributorPassword**  
  
    -   **-DistributorSecurityMode = 0**  
  
    -   **-PublisherLogin**  
  
    -   **-PublisherPassword**  
  
    -   **-PublisherSecurityMode = 0**  
  
    -   **-SubscriberLogin**  
  
    -   **-SubscriberPassword**  
  
    -   **-SubscriberSecurityMode = 0**  
  
        > [!IMPORTANT]  
        >  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
#### <a name="to-start-the-merge-agent-to-synchronize-a-push-subscription-to-a-merge-publication"></a>Per avviare l'agente di merge per sincronizzare una sottoscrizione push di una pubblicazione di tipo merge  
  
1.  Eseguire **replmerg.exe** dal prompt dei comandi o in un file batch nel server di distribuzione. Specificare gli argomenti della riga di comando seguenti:  
  
    -   **-Publisher**  
  
    -   **-PublisherDB**  
  
    -   **-Publication**  
  
    -   **-Distributor**  
  
    -   **-Subscriber**  
  
    -   **-SubscriberDB**  
  
    -   **-SubscriptionType = 0**  
  
     Se si usano l'autenticazione di SQL Server, è inoltre necessario specificare gli argomenti seguenti:  
  
    -   **-DistributorLogin**  
  
    -   **-DistributorPassword**  
  
    -   **-DistributorSecurityMode = 0**  
  
    -   **-PublisherLogin**  
  
    -   **-PublisherPassword**  
  
    -   **-PublisherSecurityMode = 0**  
  
    -   **-SubscriberLogin**  
  
    -   **-SubscriberPassword**  
  
    -   **-SubscriberSecurityMode = 0**  
  
        > [!IMPORTANT]  
        >  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
###  <a name="examples-replication-agents"></a><a name="TsqlExample"></a> Esempi (agenti di replica)  
 Nell'esempio seguente viene avviato l'agente di distribuzione per sincronizzare una sottoscrizione push.  
  
```  
  
REM -- Declare the variables.  
SET Publisher=%instancename%  
SET Subscriber=%instancename%  
SET PublicationDB=AdventureWorks2012  
SET SubscriptionDB=AdventureWorks2012Replica   
SET Publication=AdvWorksProductsTran  
  
REM -- Start the Distribution Agent with four subscription streams.  
REM -- The following command must be supplied without line breaks.  
"C:\Program Files\Microsoft SQL Server\120\COM\DISTRIB.EXE" -Subscriber %Subscriber%   
-SubscriberDB %SubscriptionDB% -SubscriberSecurityMode 1 -Publication %Publication%   
-Publisher %Publisher% -PublisherDB %PublicationDB% -Distributor %Publisher%   
-DistributorSecurityMode 1 -Continuous -SubscriptionType 0 -SubscriptionStreams 4  
  
```  
  
 Nell'esempio seguente viene avviato l'agente di merge per sincronizzare una sottoscrizione push.  
  
```  
  
REM -- Declare the variables.  
SET Publisher=%instancename%  
SET Subscriber=%instancename%  
SET PublicationDB=AdventureWorks2012  
SET SubscriptionDB=AdventureWorks2012Replica   
SET Publication=AdvWorksSalesOrdersMerge  
  
REM -- Start the Merge Agent.  
REM -- The following command must be supplied without line breaks.  
"C:\Program Files\Microsoft SQL Server\120\COM\REPLMERG.EXE"  -Publisher %Publisher%   
-Subscriber  %Subscriber%  -Distributor %Publisher% -PublisherDB  %PublicationDB%   
-SubscriberDB %SubscriptionDB% -Publication %Publication% -PublisherSecurityMode 1   
-OutputVerboseLevel 3  -Output -SubscriberSecurityMode 1  -SubscriptionType 0   
-DistributorSecurityMode 1  
  
```  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilizzo di RMO (Replication Management Objects)  
 È possibile sincronizzare le sottoscrizioni push a livello di programmazione tramite gli oggetti RMO (Replication Management Objects) e l'accesso tramite codice gestito alle funzionalità dell'agente di replica. Le classi usate per la sincronizzazione di una sottoscrizione push dipendono dal tipo di pubblicazione a cui appartiene la sottoscrizione.  
  
> [!NOTE]
>  Se si desidera avviare una sincronizzazione eseguita in modo autonomo senza effetti sull'applicazione, avviare l'agente in modo asincrono. Se invece si desidera monitorare i risultati della sincronizzazione e ricevere callback dall'agente durante il processo di sincronizzazione (ad esempio per la visualizzazione di una barra di stato) è necessario avviare l'agente in modo sincrono. Per i sottoscrittori di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssExpressEd2005](../../includes/ssexpressed2005-md.md)] , l'agente deve essere avviato in modo sincrono.  
  
#### <a name="to-synchronize-a-push-subscription-to-a-snapshot-or-transactional-publication"></a>Per sincronizzare una sottoscrizione push di una pubblicazione snapshot o transazionale  
  
1.  Creare una connessione al server di distribuzione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.TransSubscription> e impostare le proprietà seguenti:  
  
    -   Nome del database di pubblicazione per <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>.  
  
    -   Nome della pubblicazione a cui appartiene la sottoscrizione per <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>.  
  
    -   Nome del database di sottoscrizione per <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A>.  
  
    -   Nome del Sottoscrittore per <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>.  
  
    -   La connessione creata nel passaggio 1 per <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> per recuperare le proprietà rimanenti della sottoscrizione. Se il metodo restituisce **false**, verificare che la sottoscrizione esista.  
  
4.  Avviare l'agente di distribuzione nel server di distribuzione in uno dei modi seguenti:  
  
    -   Chiamare il metodo <xref:Microsoft.SqlServer.Replication.TransSubscription.SynchronizeWithJob%2A> nell'istanza di <xref:Microsoft.SqlServer.Replication.TransSubscription> creata al passaggio 2. Questo metodo consente di avviare l'agente di distribuzione in modo asincrono e il controllo viene restituito immediatamente all'applicazione durante l'esecuzione del processo dell'agente. Non è possibile chiamare questo metodo se la sottoscrizione è stata creata con il valore **false** for <xref:Microsoft.SqlServer.Replication.Subscription.CreateSyncAgentByDefault%2A>.  
  
    -   Recuperare un'istanza della classe <xref:Microsoft.SqlServer.Replication.TransSynchronizationAgent> dalla proprietà <xref:Microsoft.SqlServer.Replication.TransSubscription.SynchronizationAgent%2A> e chiamare il metodo <xref:Microsoft.SqlServer.Replication.TransSynchronizationAgent.Synchronize%2A> . Questo metodo avvia l'agente in modo sincrono e il controllo rimane al processo dell'agente in esecuzione. Nella modalità sincrona è possibile gestire l'evento <xref:Microsoft.SqlServer.Replication.TransSynchronizationAgent.Status> durante l'esecuzione dell'agente.  
  
#### <a name="to-synchronize-a-push-subscription-to-a-merge-publication"></a>Per sincronizzare una sottoscrizione push di una pubblicazione di tipo merge  
  
1.  Creare una connessione al server di distribuzione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.MergeSubscription> e impostare le proprietà seguenti:  
  
    -   Nome del database di pubblicazione per <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>.  
  
    -   Nome della pubblicazione a cui appartiene la sottoscrizione per <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>.  
  
    -   Nome del database di sottoscrizione per <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A>.  
  
    -   Nome del Sottoscrittore per <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>.  
  
    -   La connessione creata nel passaggio 1 per <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> per recuperare le proprietà rimanenti della sottoscrizione. Se il metodo restituisce **false**, verificare che la sottoscrizione esista.  
  
4.  Avviare l'agente di merge nel server di distribuzione in uno dei modi seguenti:  
  
    -   Chiamare il metodo <xref:Microsoft.SqlServer.Replication.MergeSubscription.SynchronizeWithJob%2A> nell'istanza di <xref:Microsoft.SqlServer.Replication.MergeSubscription> creata al passaggio 2. Questo metodo consente di avviare l'agente di merge in modo asincrono e il controllo viene restituito immediatamente all'applicazione durante l'esecuzione del processo dell'agente. Non è possibile chiamare questo metodo se la sottoscrizione è stata creata con il valore **false** for <xref:Microsoft.SqlServer.Replication.Subscription.CreateSyncAgentByDefault%2A>.  
  
    -   Recuperare un'istanza della classe <xref:Microsoft.SqlServer.Replication.MergeSynchronizationAgent> dalla proprietà <xref:Microsoft.SqlServer.Replication.MergeSubscription.SynchronizationAgent%2A> e chiamare il metodo <xref:Microsoft.SqlServer.Replication.MergeSynchronizationAgent.Synchronize%2A> . Questo metodo avvia l'agente di merge in modo sincrono e il controllo rimane al processo dell'agente in esecuzione. Nella modalità sincrona è possibile gestire l'evento <xref:Microsoft.SqlServer.Replication.MergeSynchronizationAgent.Status> durante l'esecuzione dell'agente.  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Esempi (RMO)  
 In questo esempio viene illustrata la sincronizzazione di una sottoscrizione push di una pubblicazione transazionale, con avvio asincrono dell'agente usando il processo dell'agente.  
  
 [!code-cs[HowTo#rmo_SyncTranPushSub_WithJob](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_synctranpushsub_withjob)]  
  
 [!code-vb[HowTo#rmo_vb_SyncTranPushSub_WithJob](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_synctranpushsub_withjob)]  
  
 In questo esempio viene illustrata la sincronizzazione di una sottoscrizione push di una pubblicazione transazionale, con avvio sincrono dell'agente.  
  
 [!code-cs[HowTo#rmo_SyncTranPushSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_synctranpushsub)]  
  
 [!code-vb[HowTo#rmo_vb_SyncTranPushSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_synctranpushsub)]  
  
 In questo esempio viene illustrata la sincronizzazione di una sottoscrizione push di una pubblicazione di tipo merge, con avvio asincrono dell'agente usando il processo dell'agente.  
  
 [!code-cs[HowTo#rmo_SyncMergePushSub_WithJob](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_syncmergepushsub_withjob)]  
  
 [!code-vb[HowTo#rmo_vb_SyncMergePushSub_WithJob](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_syncmergepushsub_withjob)]  
  
 In questo esempio viene illustrata la sincronizzazione di una sottoscrizione push di una pubblicazione di tipo merge, con avvio sincrono dell'agente.  
  
 [!code-cs[HowTo#rmo_SyncMergePushSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_syncmergepushsub)]  
  
 [!code-vb[HowTo#rmo_vb_SyncMergePushSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_syncmergepushsub)]  
  
## <a name="see-also"></a>Vedere anche  
 [Replication Management Objects Concepts](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [Sincronizzare i dati](../../relational-databases/replication/synchronize-data.md)   
 [Procedure consigliate per la sicurezza della replica](../../relational-databases/replication/security/replication-security-best-practices.md)  
  
  
