---
title: Specificare le pianificazioni della sincronizzazione | Microsoft Docs
description: Informazioni su come specificare le pianificazioni della sincronizzazione in SQL Server tramite SQL Server Management Studio, Transact-SQL o Replication Management Objects.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- subscriptions [SQL Server replication], synchronizing
- scheduling synchronization [SQL Server replication]
- synchronization [SQL Server replication], schedules
- replication [SQL Server], synchronization
ms.assetid: 97f2535b-ec19-4973-823d-bcf3d5aa0216
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: a0dac3922e006d34bf1f93c617ae70d79aff27f6
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250602"
---
# <a name="specify-synchronization-schedules"></a>Impostazione di pianificazioni della sincronizzazione
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  In questo argomento viene descritto come specificare le pianificazioni di sincronizzazione in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]o RMO (Replication Management Objects). Quando si crea una sottoscrizione, è possibile definire una pianificazione della sincronizzazione per controllare l'esecuzione dell'agente di replica per la sottoscrizione. Se non si specificano parametri di pianificazione, per la sottoscrizione verrà utilizzata la pianificazione predefinita.  
  
 Le sottoscrizioni vengono sincronizzate dall'agente di distribuzione, per la replica snapshot e transazionale, o dall'agente di merge, per la replica di tipo merge. Gli agenti possono essere in esecuzione continuamente, essere in esecuzione su richiesta o essere in esecuzione su una pianificazione.  
  
 **Contenuto dell'articolo**  
  
-   **Per impostare le pianificazioni della sincronizzazione, utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Oggetti RMO (Replication Management Objects)](#RMOProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
 Impostare le pianificazioni della sincronizzazione nella pagina **Pianificazione della sincronizzazione** della Creazione guidata nuova sottoscrizione. Per ulteriori informazioni sull'accesso a questa procedura guidata, vedere [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md) e [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md).  
  
 Modificare le pianificazioni della sincronizzazione nella finestra di dialogo **Proprietà pianificazione processo** , alla quale è possibile accedere dalla cartella **Processi** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] e dalle finestre relative ai dettagli dell'agente in Monitoraggio replica. Per informazioni sull'avvio di Monitoraggio replica, vedere [Avviare Monitoraggio replica](../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
 Se si specificano le pianificazioni dalla cartella **Processi** , utilizzare la tabella seguente per determinare il nome del processo dell'agente.  
  
|Agente|Nome processo|  
|-----------|--------------|  
|Agente di merge per le sottoscrizioni pull|**\<Publisher>-\<PublicationDatabase>-\<Publication>-\<Subscriber>-\<SubscriptionDatabase>-\<integer>**|  
|Agente di merge per le sottoscrizioni push|**\<Publisher>-\<PublicationDatabase>-\<Publication>-\<Subscriber>-\<integer>**|  
|Agente di distribuzione per le sottoscrizioni push|**\<Publisher>-\<PublicationDatabase>-\<Publication>-\<Subscriber>-\<integer>** <sup>1</sup>|  
|Agente di distribuzione per le sottoscrizioni pull|**\<Publisher>-\<PublicationDatabase>-\<Publication>-\<Subscriber>-\<SubscriptionDatabase>-\<GUID>** <sup>2</sup>|  
|Agente di distribuzione per le sottoscrizioni push di Sottoscrittori non SQL Server|**\<Publisher>-\<PublicationDatabase>-\<Publication>-\<Subscriber>-\<integer>**|  
  
 <sup>1</sup> Per le sottoscrizioni push di pubblicazioni Oracle, è**\<Publisher>-\<Publisher**> invece di **\<Publisher>-\<PublicationDatabase>**  
  
 <sup>2</sup> Per le sottoscrizioni pull di pubblicazioni Oracle, è **\<Publisher>-\<DistributionDatabase**> invece di **\<Publisher>-\<PublicationDatabase>**  
  
#### <a name="to-specify-synchronization-schedules"></a>Per impostare le pianificazioni della sincronizzazione  
  
1.  Nella pagina **Pianificazione della sincronizzazione** della Creazione guidata nuova sottoscrizione selezionare uno dei valori seguenti nell'elenco a discesa **Pianificazione agente** per ogni sottoscrizione creata:  
  
    -   **Esecuzione continua**  
  
    -   **Esecuzione solo su richiesta**  
  
    -   **\<Define Schedule...>**  
  
2.  Se si seleziona **\<Define Schedule...>** , specificare una pianificazione nella finestra di dialogo **Proprietà pianificazione processo** e quindi fare clic su **OK**.  
  
3.  Completare la procedura guidata.  

#### <a name="to-modify-a-synchronization-schedule-for-a-push-subscription-in-replication-monitor"></a>Per modificare una pianificazione della sincronizzazione per una sottoscrizione push in Monitoraggio replica  
  
1.  Espandere un gruppo di server di pubblicazione nel riquadro a sinistra di Monitoraggio replica, espandere un server di pubblicazione e quindi fare clic su una pubblicazione.  
  
2.  Fare clic sulla scheda **Tutte le sottoscrizioni** .  
  
3.  Fare clic con il pulsante destro del mouse su una sottoscrizione e quindi scegliere **Visualizza dettagli**.  
  
4.  Nella finestra **Sottoscrizione < NomeSottoscrizione>** fare clic su **Azione** e quindi su **Proprietà processo - \<AgentName>** .  
  
5.  Nella pagina **Pianificazioni** della finestra di dialogo **Proprietà processo - \<JobName>** fare clic su **Modifica**.  
  
6.  Nella finestra di dialogo **Proprietà pianificazione processo** selezionare un valore nell'elenco a discesa **Tipo pianificazione** :  
  
    -   Per specificare che l'agente deve essere eseguito in modo continuo, selezionare **Avvia automaticamente all'avvio di SQL Server Agent**.  
  
    -   Per specificare che l'agente deve essere eseguito in base a una pianificazione, selezionare **Periodica**.  
  
    -   Per specificare che l'agente deve essere eseguito su richiesta, selezionare **Singola occorrenza**.  
  
7.  Se si seleziona **Periodica**, specificare una pianificazione per l'agente.  
  
8.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
#### <a name="to-modify-a-synchronization-schedule-for-a-push-subscription-in-management-studio"></a>Per modificare una pianificazione della sincronizzazione per una sottoscrizione push in Management Studio  
  
1.  Connettersi al database di distribuzione in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], quindi espandere il nodo del server.  
  
2.  Espandere la cartella **SQL Server Agent** e quindi la cartella **Processi** .  
  
3.  Fare clic con il pulsante destro del mouse sull'agente di distribuzione o sull'agente di merge associato alla sottoscrizione e quindi scegliere **Proprietà**.  
  
4.  Nella pagina **Pianificazioni** della finestra di dialogo **Proprietà processo - \<JobName>** fare clic su **Modifica**.  
  
5.  Nella finestra di dialogo **Proprietà pianificazione processo** selezionare un valore nell'elenco a discesa **Tipo pianificazione** :  
  
    -   Per specificare che l'agente deve essere eseguito in modo continuo, selezionare **Avvia automaticamente all'avvio di SQL Server Agent**.  
  
    -   Per specificare che l'agente deve essere eseguito in base a una pianificazione, selezionare **Periodica**.  
  
    -   Per specificare che l'agente deve essere eseguito su richiesta, selezionare **Singola occorrenza**.  
  
6.  Se si seleziona **Periodica**, specificare una pianificazione per l'agente.  
  
7.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
#### <a name="to-modify-a-synchronization-schedule-for-a-pull-subscription-in-management-studio"></a>Per modificare una pianificazione della sincronizzazione per una sottoscrizione pull in Management Studio  
  
1.  Connettersi al Sottoscrittore in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]e quindi espandere il nodo del server.  
  
2.  Espandere la cartella **SQL Server Agent** e quindi la cartella **Processi** .  
  
3.  Fare clic con il pulsante destro del mouse sull'agente di distribuzione o sull'agente di merge associato alla sottoscrizione e quindi scegliere **Proprietà**.  
  
4.  Nella pagina **Pianificazioni** della finestra di dialogo **Proprietà processo - \<JobName>** fare clic su **Modifica**.  
  
5.  Nella finestra di dialogo **Proprietà pianificazione processo** selezionare un valore nell'elenco a discesa **Tipo pianificazione** :  
  
    -   Per specificare che l'agente deve essere eseguito in modo continuo, selezionare **Avvia automaticamente all'avvio di SQL Server Agent**.  
  
    -   Per specificare che l'agente deve essere eseguito in base a una pianificazione, selezionare **Periodica**.  
  
    -   Per specificare che l'agente deve essere eseguito su richiesta, selezionare **Singola occorrenza**.  
  
6.  Se si seleziona **Periodica**, specificare una pianificazione per l'agente.  
  
7.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
 È possibile definire pianificazioni della sincronizzazione a livello di programmazione tramite stored procedure di replica. Le stored procedure utilizzate dipendono dal tipo di replica e dal tipo di sottoscrizione (pull o push).  
  
 Una pianificazione è definita dai parametri di programmazione seguenti, i cui comportamenti vengono ereditati da [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md):  
  
-   **\@frequency_type**: tipo di frequenza usata per la pianificazione dell'agente.  
  
-   **\@frequency_interval**: giorno della settimana in cui viene eseguito un agente.  
  
-   **\@frequency_relative_interval**: settimana di un determinato mese in cui è pianificata l'esecuzione mensile dell'agente.  
  
-   **\@frequency_recurrence_factor**: numero di unità relative al tipo di frequenza tra una sincronizzazione e l'altra.  
  
-   **\@frequency_subday**: unità della frequenza quando l'agente viene eseguito più di una volta al giorno.  
  
-   **\@frequency_subday_interval**: numero di unità della frequenza tra un'esecuzione e l'altra quando l'agente viene eseguito più di una volta al giorno.  
  
-   **\@active_start_time_of_day**: ora di un determinato giorno in cui un agente viene avviato per la prima volta.  
  
-   **\@active_end_time_of_day**: ora di un determinato giorno in cui un agente viene avviato per l'ultima volta.  
  
-   **\@active_start_date**: primo giorno di applicazione della pianificazione dell'agente.  
  
-   **\@active_end_date**: ultimo giorno di applicazione della pianificazione dell'agente.  
  
#### <a name="to-define-the-synchronization-schedule-for-a-pull-subscription-to-a-transactional-publication"></a>Per definire la pianificazione della sincronizzazione per una sottoscrizione pull di una pubblicazione transazionale  
  
1.  Creare una nuova sottoscrizione pull di una pubblicazione transazionale. Per altre informazioni, vedere [Creazione di una sottoscrizione pull](../../relational-databases/replication/create-a-pull-subscription.md).  
  
2.  Nel Sottoscrittore eseguire [sp_addpullsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md). Specificare **\@publisher**, **\@publisher_db**, **\@publication** e le credenziali di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows usate per eseguire l'agente di distribuzione nel Sottoscrittore per **\@job_name** e **\@password**. Specificare i parametri di sincronizzazione, descritti in dettaglio in precedenza, con cui definire la pianificazione per il processo dell'agente di distribuzione che sincronizza la sottoscrizione.  
  
#### <a name="to-define-the-synchronization-schedule-for-a-push-subscription-to-a-transactional-publication"></a>Per definire la pianificazione della sincronizzazione per una sottoscrizione push di una pubblicazione transazionale  
  
1.  Creare una nuova sottoscrizione push di una pubblicazione transazionale. Per altre informazioni, vedere [Creazione di una sottoscrizione push](../../relational-databases/replication/create-a-push-subscription.md).  
  
2.  Nel Sottoscrittore eseguire [sp_addpushsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md). Specificare **\@subscriber**, **\@subscriber_db**, **\@publication** e le credenziali di Windows usate per eseguire l'agente di distribuzione nel Sottoscrittore per **\@job_name** e **\@password**. Specificare i parametri di sincronizzazione, descritti in dettaglio in precedenza, con cui definire la pianificazione per il processo dell'agente di distribuzione che sincronizza la sottoscrizione.  
  
#### <a name="to-define-the-synchronization-schedule-for-a-pull-subscription-to-a-merge-publication"></a>Per definire la pianificazione della sincronizzazione per una sottoscrizione pull di una pubblicazione di tipo merge  
  
1.  Creare una nuova sottoscrizione pull di una pubblicazione di tipo merge. Per altre informazioni, vedere [Creazione di una sottoscrizione pull](../../relational-databases/replication/create-a-pull-subscription.md).  
  
2.  Nel Sottoscrittore eseguire [sp_addmergepullsubscription_agent](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md). Specificare **\@publisher**, **\@publisher_db**, **\@publication** e le credenziali di Windows usate per eseguire l'agente di merge nel Sottoscrittore per **\@job_name** e **\@password**. Specificare i parametri di sincronizzazione, descritti in dettaglio in precedenza, con cui definire la pianificazione per il processo dell'agente di merge che sincronizza la sottoscrizione.  
  
#### <a name="to-define-the-synchronization-schedule-for-a-push-subscription-to-a-merge-publication"></a>Per definire la pianificazione della sincronizzazione per una sottoscrizione push di una pubblicazione di tipo merge  
  
1.  Creare una nuova sottoscrizione push di una pubblicazione di tipo merge. Per altre informazioni, vedere [Creazione di una sottoscrizione push](../../relational-databases/replication/create-a-push-subscription.md).  
  
2.  Nel Sottoscrittore eseguire [sp_addmergepushsubscription_agent](../../relational-databases/system-stored-procedures/sp-addmergepushsubscription-agent-transact-sql.md). Specificare **\@subscriber**, **\@subscriber_db**, **\@publication** e le credenziali di Windows usate per eseguire l'agente di merge nel Sottoscrittore per **\@job_name** e **\@password**. Specificare i parametri di sincronizzazione, descritti in dettaglio in precedenza, con cui definire la pianificazione per il processo dell'agente di merge che sincronizza la sottoscrizione.  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilizzo di RMO (Replication Management Objects)  
 La replica utilizza SQL Server Agent per pianificare i processi per attività che vengono svolte periodicamente, ad esempio la generazione di snapshot e la sincronizzazione delle sottoscrizioni. È possibile utilizzare gli oggetti RMO (Replication Management Objects) a livello di programmazione per specificare le pianificazioni per i processi dell'agente di replica.  
  
> [!NOTE]  
>  Quando si crea una sottoscrizione e si specifica un valore **false** per **CreateSyncAgentByDefault** (comportamento predefinito per le sottoscrizioni pull), il processo dell'agente non viene creato e le proprietà di pianificazione vengono ignorate. In questo caso, la pianificazione della sincronizzazione deve essere determinata dall'applicazione. Per ulteriori informazioni, vedere [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md) e [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md).  
  
#### <a name="to-define-a-replication-agent-schedule-when-you-create-a-push-subscription-to-a-transactional-publication"></a>Per definire una pianificazione dell'agente di replica quando si crea una sottoscrizione push di una pubblicazione transazionale  
  
1.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.TransSubscription> per la sottoscrizione da creare. Per altre informazioni, vedere [Creazione di una sottoscrizione push](../../relational-databases/replication/create-a-push-subscription.md).  
  
2.  Prima di chiamare <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A>, impostare uno o più dei seguenti campi della proprietà <xref:Microsoft.SqlServer.Replication.Subscription.AgentSchedule%2A> :  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyType%2A> : tipo di frequenza (ad esempio giornaliera o settimanale) utilizzata per la pianificazione dell'agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyInterval%2A> : giorno della settimana in cui viene eseguito un agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRelativeInterval%2A> : settimana di un determinato mese in cui è pianificata l'esecuzione mensile dell'agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRecurrenceFactor%2A> : numero di unità relative al tipo di frequenza tra sincronizzazioni.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDay%2A> : unità della frequenza quando l'agente viene eseguito più di una volta al giorno.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDayInterval%2A> : numero di unità della frequenza tra un'esecuzione e l'altra quando l'agente viene eseguito più di una volta al giorno.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartTime%2A> : ora di un determinato giorno in cui un agente viene avviato per la prima volta.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndTime%2A> : ora di un determinato giorno in cui un agente viene avviato per l'ultima volta.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartDate%2A> : primo giorno di applicazione della pianificazione dell'agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndDate%2A> : ultimo giorno di applicazione della pianificazione dell'agente.  
  
    > [!NOTE]  
    >  Se una di queste proprietà viene omessa, viene impostato un valore predefinito.  
  
3.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A> per creare la sottoscrizione.  
  
#### <a name="to-define-a-replication-agent-schedule-when-you-create-a-pull-subscription-to-a-transactional-publication"></a>Per definire una pianificazione dell'agente di replica quando si crea una sottoscrizione pull di una pubblicazione transazionale  
  
1.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.TransPullSubscription> per la sottoscrizione da creare. Per altre informazioni, vedere [Creazione di una sottoscrizione pull](../../relational-databases/replication/create-a-pull-subscription.md).  
  
2.  Prima di chiamare <xref:Microsoft.SqlServer.Replication.PullSubscription.Create%2A>, impostare uno o più dei seguenti campi della proprietà <xref:Microsoft.SqlServer.Replication.PullSubscription.AgentSchedule%2A> :  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyType%2A> : tipo di frequenza (ad esempio giornaliera o settimanale) utilizzata per la pianificazione dell'agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyInterval%2A> : giorno della settimana in cui viene eseguito un agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRelativeInterval%2A> : settimana di un determinato mese in cui è pianificata l'esecuzione mensile dell'agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRecurrenceFactor%2A> : numero di unità relative al tipo di frequenza tra sincronizzazioni.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDay%2A> : unità della frequenza quando l'agente viene eseguito più di una volta al giorno.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDayInterval%2A> : numero di unità della frequenza tra un'esecuzione e l'altra quando l'agente viene eseguito più di una volta al giorno.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartTime%2A> : ora di un determinato giorno in cui un agente viene avviato per la prima volta.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndTime%2A> : ora di un determinato giorno in cui un agente viene avviato per l'ultima volta.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartDate%2A> : primo giorno di applicazione della pianificazione dell'agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndDate%2A> : ultimo giorno di applicazione della pianificazione dell'agente.  
  
    > [!NOTE]  
    >  Se una di queste proprietà viene omessa, viene impostato un valore predefinito.  
  
3.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.PullSubscription.Create%2A> per creare la sottoscrizione.  
  
#### <a name="to-define-a-replication-agent-schedule-when-you-create-a-pull-subscription-to-a-merge-publication"></a>Per definire una pianificazione dell'agente di replica quando si crea una sottoscrizione pull di una pubblicazione di tipo merge  
  
1.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.MergePullSubscription> per la sottoscrizione da creare. Per altre informazioni, vedere [Creazione di una sottoscrizione pull](../../relational-databases/replication/create-a-pull-subscription.md).  
  
2.  Prima di chiamare <xref:Microsoft.SqlServer.Replication.PullSubscription.Create%2A>, impostare uno o più dei seguenti campi della proprietà <xref:Microsoft.SqlServer.Replication.PullSubscription.AgentSchedule%2A> :  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyType%2A> : tipo di frequenza (ad esempio giornaliera o settimanale) utilizzata per la pianificazione dell'agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyInterval%2A> : giorno della settimana in cui viene eseguito un agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRelativeInterval%2A> : settimana di un determinato mese in cui è pianificata l'esecuzione mensile dell'agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRecurrenceFactor%2A> : numero di unità relative al tipo di frequenza tra sincronizzazioni.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDay%2A> : unità della frequenza quando l'agente viene eseguito più di una volta al giorno.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDayInterval%2A> : numero di unità della frequenza tra un'esecuzione e l'altra quando l'agente viene eseguito più di una volta al giorno.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartTime%2A> : ora di un determinato giorno in cui un agente viene avviato per la prima volta.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndTime%2A> : ora di un determinato giorno in cui un agente viene avviato per l'ultima volta.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartDate%2A> : primo giorno di applicazione della pianificazione dell'agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndDate%2A> : ultimo giorno di applicazione della pianificazione dell'agente.  
  
    > [!NOTE]  
    >  Se una di queste proprietà viene omessa, viene impostato un valore predefinito.  
  
3.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.PullSubscription.Create%2A> per creare la sottoscrizione.  
  
#### <a name="to-define-a-replication-agent-schedule-when-you-create-a-push-subscription-to-a-merge-publication"></a>Per definire una pianificazione dell'agente di replica quando si crea una sottoscrizione push di una pubblicazione di tipo merge  
  
1.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.MergeSubscription> per la sottoscrizione da creare. Per altre informazioni, vedere [Creazione di una sottoscrizione push](../../relational-databases/replication/create-a-push-subscription.md).  
  
2.  Prima di chiamare <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A>, impostare uno o più dei seguenti campi della proprietà <xref:Microsoft.SqlServer.Replication.Subscription.AgentSchedule%2A> :  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyType%2A> : tipo di frequenza (ad esempio giornaliera o settimanale) utilizzata per la pianificazione dell'agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyInterval%2A> : giorno della settimana in cui viene eseguito un agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRelativeInterval%2A> : settimana di un determinato mese in cui è pianificata l'esecuzione mensile dell'agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRecurrenceFactor%2A> : numero di unità relative al tipo di frequenza tra sincronizzazioni.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDay%2A> : unità della frequenza quando l'agente viene eseguito più di una volta al giorno.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDayInterval%2A> : numero di unità della frequenza tra un'esecuzione e l'altra quando l'agente viene eseguito più di una volta al giorno.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartTime%2A> : ora di un determinato giorno in cui un agente viene avviato per la prima volta.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndTime%2A> : ora di un determinato giorno in cui un agente viene avviato per l'ultima volta.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartDate%2A> : primo giorno di applicazione della pianificazione dell'agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndDate%2A> : ultimo giorno di applicazione della pianificazione dell'agente.  
  
    > [!NOTE]  
    >  Se una di queste proprietà viene omessa, viene impostato un valore predefinito.  
  
3.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A> per creare la sottoscrizione.  
  
###  <a name="example-rmo"></a><a name="PShellExample"></a> Esempio (RMO)  
 In questo esempio viene creata una sottoscrizione push di una pubblicazione di tipo merge e viene specificata la pianificazione per la sincronizzazione di tale sottoscrizione.  
  
 [!code-cs[HowTo#rmo_CreateMergePushSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createmergepushsub)]  
  
 [!code-vb[HowTo#rmo_vb_CreateMergePushSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createmergepushsub)]  
  
## <a name="see-also"></a>Vedere anche  
 [Replication Security Best Practices](../../relational-databases/replication/security/replication-security-best-practices.md)   
 [Subscribe to Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [Sincronizzare una sottoscrizione push](../../relational-databases/replication/synchronize-a-push-subscription.md)   
 [Sincronizzare una sottoscrizione pull](../../relational-databases/replication/synchronize-a-pull-subscription.md)   
 [Sincronizzare i dati](../../relational-databases/replication/synchronize-data.md)  
  
  
