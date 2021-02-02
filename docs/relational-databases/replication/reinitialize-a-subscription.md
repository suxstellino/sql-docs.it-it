---
title: Reinizializzare una sottoscrizione | Microsoft Docs
description: Informazioni su come reinizializzare una sottoscrizione in SQL Server tramite SQL Server Management Studio, Transact-SQL o Replication Management Objects.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- initializing subscriptions [SQL Server replication], reinitializing
- subscriptions [SQL Server replication], reinitializing
- reinitializing subscriptions
ms.assetid: ca3625c5-c62e-4ab7-9829-d511f838e385
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: 03d97ad323c5e83e7cd0a5de0b09da6bad6b1a22
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250732"
---
# <a name="reinitialize-a-subscription"></a>Reinizializzare una sottoscrizione
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  In questo argomento viene descritto come reinizializzare una sottoscrizione in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]o RMO (Replication Management Objects). È possibile contrassegnare singole sottoscrizioni per la reinizializzazione in modo che nel corso della successiva sincronizzazione venga applicato un nuovo snapshot.  
  
[!INCLUDE[azure-sql-db-replication-supportability-note](../../includes/azure-sql-db-replication-supportability-note.md)]


  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
 Il processo di reinizializzazione di una sottoscrizione è articolato in due parti.  
  
1.  Una singola sottoscrizione o tutte le sottoscrizioni di una pubblicazione vengono *contrassegnate* per la reinizializzazione. Contrassegnare le sottoscrizioni per la reinizializzazione nella finestra di dialogo **Reinizializza sottoscrizioni**, accessibile dalla cartella **Pubblicazioni locali** e dalla cartella **Sottoscrizioni locali** in [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. È inoltre possibile contrassegnare le sottoscrizioni nella scheda **Tutte le sottoscrizioni** e nel nodo delle pubblicazioni in Monitoraggio replica. Per informazioni sull'avvio di Monitoraggio replica, vedere [Avviare Monitoraggio replica](../../relational-databases/replication/monitor/start-the-replication-monitor.md). Quando si contrassegna una sottoscrizione per la reinizializzazione, sono disponibili le opzioni seguenti:  
  
     **Usa lo snapshot corrente**  
     Selezionare questa opzione per applicare lo snapshot corrente al Sottoscrittore alla successiva esecuzione dell'agente di distribuzione o dell'agente di merge. Se non sono disponibili snapshot validi, questa opzione non può essere selezionata.  
  
     **Usa un nuovo snapshot**  
     Selezionare questa opzione per reinizializzare la sottoscrizione con un nuovo snapshot. Lo snapshot può essere applicato al Sottoscrittore solo dopo essere stato generato dall'agente snapshot. Se l'agente snapshot è impostato in modo da essere eseguito in base a una pianificazione, la sottoscrizione verrà reinizializzata soltanto dopo la successiva esecuzione pianificata dell'agente snapshot. Per avviare l'agente snapshot immediatamente, selezionare **Genera il nuovo snapshot ora** .  
  
     **Carica le modifiche non sincronizzate prima della reinizializzazione**  
     Solo per la replica di tipo merge. Selezionare questa opzione per caricare le eventuali modifiche in sospeso dal database della sottoscrizione prima che i dati nel Sottoscrittore vengano sovrascritti con uno snapshot.  
  
     Se si aggiunge, elimina o modifica un filtro con parametri, le modifiche in sospeso nel Sottoscrittore non possono essere caricate nel server di pubblicazione durante la reinizializzazione. Per caricare le modifiche in sospeso, sincronizzare tutte le sottoscrizioni prima di modificare il filtro.  
  
2.  Una sottoscrizione viene reinizializzata alla successiva sincronizzazione. L'agente di distribuzione (per la replica transazionale) o l'agente di merge (per la replica di tipo merge) applica lo snapshot più recente a ogni Sottoscrittore dotato di una sottoscrizione contrassegnata per la reinizializzazione. Per ulteriori informazioni sulla sincronizzazione delle sottoscrizioni, vedere [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md) e [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
#### <a name="to-mark-a-single-push-or-pull-subscription-for-reinitialization-in-management-studio-at-the-publisher"></a>Per contrassegnare una singola sottoscrizione push o pull per la reinizializzazione in Management Studio (nel server di pubblicazione)  
  
1.  Connettersi al server di pubblicazione in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]e quindi espandere il nodo del server.  
  
2.  Espandere la cartella **Replica** e quindi la cartella **Pubblicazioni locali** .  
  
3.  Espandere la pubblicazione di cui si desidera reinizializzare la sottoscrizione.  
  
4.  Fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere **Reinizializza**.  
  
5.  Nella finestra di dialogo **Reinizializza sottoscrizioni** selezionare le opzioni desiderate e quindi fare clic su **Contrassegna per la reinizializzazione**.  
  
#### <a name="to-mark-a-single-pull-subscription-for-reinitialization-in-management-studio-at-the-subscriber"></a>Per contrassegnare una singola sottoscrizione pull per la reinizializzazione in Management Studio (nel Sottoscrittore)  
  
1.  Connettersi al Sottoscrittore in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]e quindi espandere il nodo del server.  
  
2.  Espandere la cartella **Replica** e quindi la cartella **Sottoscrizioni locali** .  
  
3.  Fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere **Reinizializza**.  
  
4.  Nella finestra di dialogo di conferma che viene visualizzata, fare clic su **Sì**.  
  
#### <a name="to-mark-all-subscriptions-for-reinitialization-in-management-studio"></a>Per contrassegnare tutte le sottoscrizioni per la reinizializzazione in Management Studio  
  
1.  Connettersi al server di pubblicazione in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]e quindi espandere il nodo del server.  
  
2.  Espandere la cartella **Replica** e quindi la cartella **Pubblicazioni locali** .  
  
3.  Fare clic con il pulsante destro del mouse sulla pubblicazione di cui desidera reinizializzare le sottoscrizioni e quindi scegliere **Reinizializza tutte le sottoscrizioni**.  
  
4.  Nella finestra di dialogo **Reinizializza sottoscrizioni** selezionare le opzioni desiderate e quindi fare clic su **Contrassegna per la reinizializzazione**.  
  
#### <a name="to-mark-a-single-push-or-pull-subscription-for-reinitialization-in-replication-monitor"></a>Per contrassegnare una singola sottoscrizione push o pull per la reinizializzazione in Monitoraggio replica  
  
1.  In Monitoraggio replica espandere un gruppo di server di pubblicazione nel riquadro di sinistra, espandere un server di pubblicazione e quindi fare clic su una pubblicazione.  
  
2.  Fare clic sulla scheda **Tutte le sottoscrizioni** .  
  
3.  Fare clic con il pulsante destro del mouse sulla sottoscrizione che si desidera reinizializzare e quindi scegliere **Reinizializza sottoscrizione**.  
  
4.  Nella finestra di dialogo **Reinizializza sottoscrizioni** selezionare le opzioni desiderate e quindi fare clic su **Contrassegna per la reinizializzazione**.  
  
#### <a name="to-mark-all-subscriptions-for-reinitialization-in-replication-monitor"></a>Per contrassegnare tutte le sottoscrizioni per la reinizializzazione in Monitoraggio replica  
  
1.  In Monitoraggio replica espandere un gruppo di server di pubblicazione nel riquadro di sinistra e quindi espandere un server di pubblicazione.  
  
2.  Fare clic con il pulsante destro del mouse sulla pubblicazione di cui desidera reinizializzare le sottoscrizioni e quindi scegliere **Reinizializza tutte le sottoscrizioni**.  
  
3.  Nella finestra di dialogo **Reinizializza sottoscrizioni** selezionare le opzioni desiderate e quindi fare clic su **Contrassegna per la reinizializzazione**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
 È possibile reinizializzare le sottoscrizioni a livello di programmazione tramite le stored procedure di replica. La stored procedure utilizzata dipende dal tipo di sottoscrizione, ovvero push o pull, nonché dal tipo di pubblicazione a cui appartiene la sottoscrizione.  
  
#### <a name="to-reinitialize-a-pull-subscription-to-a-transactional-publication"></a>Per reinizializzare una sottoscrizione pull in una pubblicazione transazionale  
  
1.  Nel database di sottoscrizione del Sottoscrittore eseguire [sp_reinitpullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-reinitpullsubscription-transact-sql.md). Specificare **\@publisher**, **\@publisher_db** e **\@publication**. La sottoscrizione verrà contrassegnata per la reinizializzazione alla successiva esecuzione dell'agente di distribuzione.  
  
2.  (Facoltativo) Avviare l'agente di distribuzione nel Sottoscrittore per sincronizzare la sottoscrizione. Per altre informazioni, vedere [Sincronizzazione di una sottoscrizione pull](../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
#### <a name="to-reinitialize-a-push-subscription-to-a-transactional-publication"></a>Per reinizializzare una sottoscrizione push in una pubblicazione transazionale  
  
1.  Nel server di pubblicazione eseguire [sp_reinitsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-reinitsubscription-transact-sql.md). Specificare **\@publication**, **\@subscriber** e **\@destination_db**. La sottoscrizione verrà contrassegnata per la reinizializzazione alla successiva esecuzione dell'agente di distribuzione.  
  
2.  (Facoltativo) Avviare l'agente di distribuzione nel server di distribuzione per sincronizzare la sottoscrizione. Per altre informazioni, vedere [Sincronizzazione di una sottoscrizione push](../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
#### <a name="to-reinitialize-a-pull-subscription-to-a-merge-publication"></a>Per reinizializzare una sottoscrizione pull in una pubblicazione di tipo merge  
  
1.  Nel database di sottoscrizione del Sottoscrittore eseguire [sp_reinitmergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-reinitmergepullsubscription-transact-sql.md). Specificare **\@publisher**, **\@publisher_db** e **\@publication**. Per caricare le modifiche dal Sottoscrittore prima della reinizializzazione, specificare il valore **true** per **\@upload_first**. La sottoscrizione verrà contrassegnata per la reinizializzazione alla successiva esecuzione dell'agente di merge.  
  
    > [!IMPORTANT]  
    >  Se si aggiunge, elimina o modifica un filtro con parametri, le modifiche in sospeso nel Sottoscrittore non possono essere caricate nel server di pubblicazione durante la reinizializzazione. Per caricare le modifiche in sospeso, sincronizzare tutte le sottoscrizioni prima di modificare il filtro.  
  
2.  (Facoltativo) Avviare l'agente di merge nel Sottoscrittore per sincronizzare la sottoscrizione. Per altre informazioni, vedere [Sincronizzazione di una sottoscrizione pull](../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
#### <a name="to-reinitialize-a-push-subscription-to-a-merge-publication"></a>Per reinizializzare una sottoscrizione push in una pubblicazione di tipo merge  
  
1.  Nel server di pubblicazione eseguire [sp_reinitmergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-reinitmergesubscription-transact-sql.md). Specificare **\@publication**, **\@subscriber** e **\@subscriber_db**. Per caricare le modifiche dal Sottoscrittore prima della reinizializzazione, specificare il valore **true** per **\@upload_first**. La sottoscrizione verrà contrassegnata per la reinizializzazione alla successiva esecuzione dell'agente di distribuzione.  
  
    > [!IMPORTANT]  
    >  Se si aggiunge, elimina o modifica un filtro con parametri, le modifiche in sospeso nel Sottoscrittore non possono essere caricate nel server di pubblicazione durante la reinizializzazione. Per caricare le modifiche in sospeso, sincronizzare tutte le sottoscrizioni prima di modificare il filtro.  
  
2.  (Facoltativo) Avviare l'agente di merge nel server di distribuzione per sincronizzare la sottoscrizione. Per altre informazioni, vedere [Sincronizzazione di una sottoscrizione push](../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
#### <a name="to-set-the-reinitialization-policy-when-creating-a-new-merge-publication"></a>Per impostare i criteri di reinizializzazione durante la creazione di una nuova pubblicazione di tipo merge  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_addmergepublication](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md), specificando uno dei valori seguenti per **\@automatic_reinitialization_policy**:  
  
    -   **1** : le modifiche vengono caricate dal Sottoscrittore prima della reinizializzazione automatica di una sottoscrizione in seguito a una modifica della pubblicazione.  
  
    -   **0** : le modifiche nel Sottoscrittore vengono eliminate quando una sottoscrizione viene reinizializzata automaticamente in seguito a una modifica della pubblicazione.  
  
    > [!IMPORTANT]  
    >  Se si aggiunge, elimina o modifica un filtro con parametri, le modifiche in sospeso nel Sottoscrittore non possono essere caricate nel server di pubblicazione durante la reinizializzazione. Per caricare le modifiche in sospeso, sincronizzare tutte le sottoscrizioni prima di modificare il filtro.  
  
     Per altre informazioni, vedere [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md).  
  
#### <a name="to-change-the-reinitialization-policy-for-an-existing-merge-publication"></a>Per modificare i criteri di reinizializzazione per una pubblicazione di tipo merge esistente  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_changemergepublication](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md), specificando **automatic_reinitialization_policy** per **\@property** e uno dei valori seguenti per **\@value**:  
  
    -   **1** : le modifiche vengono caricate dal Sottoscrittore prima della reinizializzazione automatica di una sottoscrizione in seguito a una modifica della pubblicazione.  
  
    -   **0** : le modifiche nel Sottoscrittore vengono eliminate quando una sottoscrizione viene reinizializzata automaticamente in seguito a una modifica della pubblicazione.  
  
    > [!IMPORTANT]  
    >  Se si aggiunge, elimina o modifica un filtro con parametri, le modifiche in sospeso nel Sottoscrittore non possono essere caricate nel server di pubblicazione durante la reinizializzazione. Per caricare le modifiche in sospeso, sincronizzare tutte le sottoscrizioni prima di modificare il filtro.  
  
     Per altre informazioni, vedere [View and Modify Publication Properties](../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilizzo di RMO (Replication Management Objects)  
 È possibile contrassegnare singole sottoscrizioni per la reinizializzazione in modo che nel corso della successiva sincronizzazione venga applicato un nuovo snapshot. Le sottoscrizioni possono essere reinizializzate a livello di programmazione tramite gli oggetti RMO (Replication Management Objects). Le classi utilizzate dipendono dal tipo di pubblicazione a cui appartiene la sottoscrizione e dal tipo di sottoscrizione, ovvero push o pull.  
  
#### <a name="to-reinitialize-a-pull-subscription-to-a-transactional-publication"></a>Per reinizializzare una sottoscrizione pull in una pubblicazione transazionale  
  
1.  Creare una connessione al Sottoscrittore tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.TransPullSubscription> , quindi impostare <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationDBName%2A>e la connessione ottenuta al passaggio 1 per <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> per recuperare le proprietà dell'oggetto.  
  
    > [!NOTE]  
    >  Se questo metodo restituisce **false**, le proprietà di sottoscrizione sono state definite in modo non corretto nel passaggio 2 oppure la sottoscrizione pull non esiste.  
  
4.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.TransPullSubscription.Reinitialize%2A> . Questo metodo contrassegna la sottoscrizione per la reinizializzazione.  
  
5.  Sincronizzare la sottoscrizione pull. Per altre informazioni, vedere [Sincronizzazione di una sottoscrizione pull](../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
#### <a name="to-reinitialize-a-push-subscription-to-a-transactional-publication"></a>Per reinizializzare una sottoscrizione push in una pubblicazione transazionale  
  
1.  Creare una connessione al server di pubblicazione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.TransSubscription> , quindi impostare <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A>e la connessione ottenuta al passaggio 1 per <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> per recuperare le proprietà dell'oggetto.  
  
    > [!NOTE]  
    >  Se questo metodo restituisce **false**, le proprietà di sottoscrizione sono state definite in modo non corretto nel passaggio 2 oppure la sottoscrizione push non esiste.  
  
4.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.TransSubscription.Reinitialize%2A> . Questo metodo contrassegna la sottoscrizione per la reinizializzazione.  
  
5.  Sincronizzare la sottoscrizione push. Per altre informazioni, vedere [Sincronizzazione di una sottoscrizione push](../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
#### <a name="to-reinitialize-a-pull-subscription-to-a-merge-publication"></a>Per reinizializzare una sottoscrizione pull in una pubblicazione di tipo merge  
  
1.  Creare una connessione al Sottoscrittore tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.MergePullSubscription> , quindi impostare <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationDBName%2A>e la connessione ottenuta al passaggio 1 per <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> per recuperare le proprietà dell'oggetto.  
  
    > [!NOTE]  
    >  Se questo metodo restituisce **false**, le proprietà di sottoscrizione sono state definite in modo non corretto nel passaggio 2 oppure la sottoscrizione pull non esiste.  
  
4.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.MergePullSubscription.Reinitialize%2A> . Passare il valore **true** per caricare le modifiche nel Sottoscrittore prima della reinizializzazione oppure il valore **false** per eseguire la reinizializzazione e perdere eventuali modifiche in sospeso nel Sottoscrittore. Questo metodo contrassegna la sottoscrizione per la reinizializzazione.  
  
    > [!NOTE]  
    >  Se la sottoscrizione è scaduta, non sarà possibile caricare le modifiche. Per altre informazioni, vedere [Set the Expiration Period for Subscriptions](../../relational-databases/replication/publish/set-the-expiration-period-for-subscriptions.md).  
  
5.  Sincronizzare la sottoscrizione pull. Per altre informazioni, vedere [Sincronizzazione di una sottoscrizione pull](../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
#### <a name="to-reinitialize-a-push-subscription-to-a-merge-publication"></a>Per reinizializzare una sottoscrizione push in una pubblicazione di tipo merge  
  
1.  Creare una connessione al server di pubblicazione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.MergeSubscription> , quindi impostare <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A>e la connessione ottenuta al passaggio 1 per <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> per recuperare le proprietà dell'oggetto.  
  
    > [!NOTE]  
    >  Se questo metodo restituisce **false**, le proprietà di sottoscrizione sono state definite in modo non corretto nel passaggio 2 oppure la sottoscrizione push non esiste.  
  
4.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.MergeSubscription.Reinitialize%2A> . Passare il valore **true** per caricare le modifiche nel Sottoscrittore prima della reinizializzazione oppure il valore **false** per eseguire la reinizializzazione e perdere eventuali modifiche in sospeso nel Sottoscrittore. Questo metodo contrassegna la sottoscrizione per la reinizializzazione.  
  
    > [!NOTE]  
    >  Se la sottoscrizione è scaduta, non sarà possibile caricare le modifiche. Per altre informazioni, vedere [Set the Expiration Period for Subscriptions](../../relational-databases/replication/publish/set-the-expiration-period-for-subscriptions.md).  
  
5.  Sincronizzare la sottoscrizione push. Per altre informazioni, vedere [Sincronizzazione di una sottoscrizione push](../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Esempi (RMO)  
 In questo esempio viene eseguita la reinizializzazione di una sottoscrizione pull in una pubblicazione transazionale.  
  
 [!code-cs[HowTo#rmo_ReinitTranPullSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_reinittranpullsub)]  
  
 [!code-vb[HowTo#rmo_vb_ReinitTranPullSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_reinittranpullsub)]  
  
 In questo esempio viene eseguita la reinizializzazione di una sottoscrizione pull in una pubblicazione di tipo merge dopo aver prima caricato le modifiche in sospeso nel Sottoscrittore.  
  
 [!code-cs[HowTo#rmo_ReinitMergePullSub_WithUpload](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_reinitmergepullsub_withupload)]  
  
 [!code-vb[HowTo#rmo_vb_ReinitMergePullSub_WithUpload](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_reinitmergepullsub_withupload)]  
  
## <a name="see-also"></a>Vedere anche  
 [Reinizializzare le sottoscrizioni](../../relational-databases/replication/reinitialize-subscriptions.md)   
 [Replication Management Objects Concepts](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [Procedure consigliate per la sicurezza della replica](../../relational-databases/replication/security/replication-security-best-practices.md)  
  
  
