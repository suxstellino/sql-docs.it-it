---
title: Creare una pubblicazione | Microsoft Docs
description: Informazioni su come creare una pubblicazione in SQL Server tramite SQL Server Management Studio, Transact-SQL o Replication Management Objects.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- publications [SQL Server replication], creating
- articles [SQL Server replication], defining
- adding articles
- articles [SQL Server replication], adding
ms.assetid: 52ee6de9-1d58-4cb9-8711-372bddbe7154
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: b3c1655be2909c46368c42ea7781617674dc3025
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076819"
---
# <a name="create-a-publication"></a>Create a Publication
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  In questo argomento viene descritto come creare una pubblicazione in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o RMO (Replication Management Objects).  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Sicurezza](#Security)  
  
-   **Per creare una pubblicazione e definire articoli, utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Oggetti RMO (Replication Management Objects)](#RMOProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   I nomi di pubblicazioni e articoli non possono includere i caratteri seguenti: % , \* , [ , ] , | , : , " , ? , ' , \ , / , < , >. Se gli oggetti del database includono uno di questi caratteri e si vuole replicarli, è necessario specificare un nome di articolo diverso dal nome oggetto nella finestra di dialogo **Proprietà articolo - \<Article>** , accessibile dalla pagina **Articoli** della procedura guidata.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
 Se possibile, richiedere agli utenti di immettere le credenziali di sicurezza in fase di esecuzione. Se è necessario archiviare le credenziali, utilizzare i [servizi di crittografia](/previous-versions/aa719848(v=vs.71)) offerti da [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows .NET Framework.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
 Creare le pubblicazioni e definire gli articoli utilizzando la Creazione guidata nuova pubblicazione. Dopo aver creato una pubblicazione, visualizzare e modificare le relative proprietà nella finestra di dialogo **Proprietà pubblicazione - \<Publication>** . Per informazioni sulla creazione di una pubblicazione da un database Oracle, vedere [Creare una pubblicazione da un database Oracle](../../../relational-databases/replication/publish/create-a-publication-from-an-oracle-database.md).  
  
#### <a name="to-create-a-publication-and-define-articles"></a>Per creare una pubblicazione e definire articoli  
  
1.  Connettersi al server di pubblicazione in [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] e quindi espandere il nodo del server.  
  
2.  Espandere la cartella **Replica** e quindi fare clic con il pulsante destro del mouse sulla cartella **Pubblicazioni locali** .  
  
3.  Fare clic su **Nuova pubblicazione**.  
  
4.  Eseguire i vari passaggi della Creazione guidata nuova pubblicazione per:  
  
    -   Specificare un server di distribuzione se la distribuzione non è stata configurata sul server. Per altre informazioni sulla configurazione della distribuzione, vedere [Configurare la pubblicazione e la distribuzione](../../../relational-databases/replication/configure-publishing-and-distribution.md).  
  
         Se nella pagina **Server di distribuzione** si specifica che il server di pubblicazione deve agire come server di distribuzione per se stesso (server di distribuzione locale) e il server non è configurato come server di distribuzione, la Creazione guidata nuova pubblicazione eseguirà la configurazione del server. Nella pagina **Cartella snapshot** è necessario specificare una cartella snapshot predefinita per il server di distribuzione. La cartella snapshot è semplicemente una directory designata come condivisione. Gli agenti che eseguono letture e scritture in questa cartella devono disporre di autorizzazioni sufficienti per accedervi. Per altre informazioni sulle impostazioni di sicurezza appropriate per la cartella, vedere [Proteggere la cartella snapshot](../../../relational-databases/replication/security/secure-the-snapshot-folder.md).  
  
         Se si specifica che un altro server deve agire come server di distribuzione, è necessario immettere una password nella pagina **Password amministrativa** per le connessioni effettuate dal server di pubblicazione a quello di distribuzione. Questa password deve corrispondere a quella specificata quando il server di pubblicazione è stato attivato nel server di distribuzione remoto.  
  
         Per altre informazioni, vedere [Configure Distribution](../../../relational-databases/replication/configure-distribution.md).  
  
    -   Scegliere un database di pubblicazione.  
  
    -   Selezionare un tipo di pubblicazione. Per altre informazioni, vedere [Tipi di replica](../../../relational-databases/replication/types-of-replication.md).  
  
    -   Specificare i dati e gli oggetti di database da pubblicare; facoltativamente, filtrare le colonne dagli articoli di tabelle e impostare le proprietà degli articoli.  
  
    -   Facoltativamente, filtrate le righe dagli articoli di tabelle. Per altre informazioni, vedere [Filtrare i dati pubblicati](../../../relational-databases/replication/publish/filter-published-data.md).  
  
    -   Impostare la pianificazione dell'agente snapshot.  
  
    -   Specificare le credenziali con cui gli agenti di replica seguenti vengono eseguiti e stabiliscono le connessioni:  
  
         \- Agente snapshot per tutte le pubblicazioni.  
  
         \- Agente di lettura log per tutte le pubblicazioni transazionali.  
  
         \- Agente di lettura coda per le pubblicazioni transazionali che consentono l'aggiornamento delle sottoscrizioni.  
  
         Per ulteriori informazioni, vedere [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md) e [Replication Security Best Practices](../../../relational-databases/replication/security/replication-security-best-practices.md).  
  
    -   Facoltativamente, creare lo script della pubblicazione. Per altre informazioni, vedere [Scripting Replication](../../../relational-databases/replication/scripting-replication.md).  
  
    -   Specificare un nome per la pubblicazione.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
 È possibile creare pubblicazioni a livello di programmazione tramite le stored procedure di replica. Le stored procedure utilizzate dipenderanno dal tipo di pubblicazione creato.  
  
#### <a name="to-create-a-snapshot-or-transactional-publication"></a>Per creare una pubblicazione snapshot o transazionale  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_replicationdboption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md) per abilitare la pubblicazione del database corrente usando la replica snapshot o transazionale.  
  
2.  Per una pubblicazione transazionale, determinare se esiste un processo dell'agente di lettura log per il database di pubblicazione. Questo passaggio non è necessario per le pubblicazioni snapshot.  
  
    -   Se per il database di pubblicazione esiste un processo dell'agente di lettura log, procedere con il passaggio 3.  
  
    -   Per sapere se un processo dell'agente di lettura log esiste già per un database pubblicato, eseguire [sp_helplogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helplogreader-agent-transact-sql.md) nel database di pubblicazione nel server di pubblicazione.  
  
    -   Se il set di risultati è vuoto, creare un processo dell'agente di lettura log. Nel server di pubblicazione eseguire [sp_addlogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md). Specificare le credenziali di [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows usate per l'esecuzione dell'agente per **\@job_name** e **\@password**. Se l'agente userà l'autenticazione di SQL Server per la connessione al server di pubblicazione, è anche necessario specificare il valore **0** per **\@publisher_security_mode** e le informazioni sull'account di accesso di [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per **\@publisher_login** e **\@publisher_password**. Procedere al passaggio 3.  
  
3.  Nel server di pubblicazione eseguire [sp_addpublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md). Specificare il nome della pubblicazione per **\@publication** e per il parametro **\@repl_freq** specificare il valore **snapshot** per una pubblicazione snapshot o il valore **continuous** per una pubblicazione transazionale. Specificare eventuali altre opzioni della pubblicazione. In questo modo viene definita la pubblicazione.  
  
    > [!NOTE]  
    >  I nomi delle pubblicazioni non possono includere i caratteri seguenti:  
    >   
    >  % * [ ] | : " ? \ / < >  
  
4.  Nel server di pubblicazione eseguire [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md). Specificare il nome della pubblicazione usato nel passaggio 3 per **\@publication** e le credenziali di Windows usate per l'agente di snapshot per **\@snapshot_job_name** e **\@password**. Se l'agente userà l'autenticazione di SQL Server per la connessione al server di pubblicazione, è anche necessario specificare il valore **0** per **\@publisher_security_mode** e le informazioni sull'account di accesso di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per **\@publisher_login** e **\@publisher_password**. Verrà creato un processo dell'agente snapshot per la pubblicazione.  
  
    > [!IMPORTANT]  
    >  Quando si configura un server di pubblicazione con un server di distribuzione remoto, i valori specificati per tutti i parametri, inclusi *job_login* e *job_password*, vengono inviati al server di distribuzione come testo normale. È consigliabile crittografare la connessione tra il server di pubblicazione e il server di distribuzione remoto prima di eseguire questa stored procedure. Per altre informazioni, vedere [Abilitare le connessioni crittografate al motore di database &#40;Gestione configurazione SQL Server&#41;](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
5.  Aggiungere articoli alla pubblicazione. Per altre informazioni, vedere [definire un articolo](../../../relational-databases/replication/publish/define-an-article.md).  
  
6.  Avviare il processo dell'agente snapshot per generare lo snapshot iniziale per la pubblicazione. Per altre informazioni, vedere [Creazione e applicazione dello snapshot iniziale](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
#### <a name="to-create-a-merge-publication"></a>Per creare una pubblicazione di tipo merge  
  
1.  Nel server di pubblicazione eseguire [sp_replicationdboption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md) per abilitare la pubblicazione del database corrente usando la replica di tipo merge.  
  
2.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_addmergepublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md). Specificare il nome della pubblicazione per **\@publication** ed eventuali altre opzioni della pubblicazione. In questo modo viene definita la pubblicazione.  
  
    > [!NOTE]  
    >  I nomi delle pubblicazioni non possono includere i caratteri seguenti:  
    >   
    >  % * [ ] | : " ? \ / < >  
  
3.  Nel server di pubblicazione eseguire [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md). Specificare il nome della pubblicazione usato nel passaggio 2 per **\@publication** e le credenziali di Windows usate per l'agente di snapshot per **\@snapshot_job_name** e **\@password**. Se l'agente userà l'autenticazione di SQL Server per la connessione al server di pubblicazione, è anche necessario specificare il valore **0** per **\@publisher_security_mode** e le informazioni sull'account di accesso di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per **\@publisher_login** e **\@publisher_password**. Verrà creato un processo dell'agente snapshot per la pubblicazione.  
  
    > [!IMPORTANT]  
    >  Quando si configura un server di pubblicazione con un server di distribuzione remoto, i valori specificati per tutti i parametri, inclusi *job_login* e *job_password*, vengono inviati al server di distribuzione come testo normale. È consigliabile crittografare la connessione tra il server di pubblicazione e il server di distribuzione remoto prima di eseguire questa stored procedure. Per altre informazioni, vedere [Abilitare le connessioni crittografate al motore di database &#40;Gestione configurazione SQL Server&#41;](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
4.  Aggiungere articoli alla pubblicazione. Per altre informazioni, vedere [definire un articolo](../../../relational-databases/replication/publish/define-an-article.md).  
  
5.  Avviare il processo dell'agente snapshot per generare lo snapshot iniziale per la pubblicazione. Per altre informazioni, vedere [Creazione e applicazione dello snapshot iniziale](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Esempio (Transact-SQL)  
 In questo esempio viene creata una pubblicazione transazionale. Per passare le credenziali di Windows necessarie per la creazione di processi per l'agente snapshot e per l'agente di lettura log, vengono utilizzate variabili di scripting.  
  
 [!code-sql[HowTo#sp_AddTranPub](../../../relational-databases/replication/codesnippet/tsql/create-a-publication_1.sql)]  
  
 In questo esempio viene creata una pubblicazione di tipo merge. Per passare le credenziali di Windows necessarie per la creazione del processo per l'agente snapshot, vengono utilizzate variabili di scripting.  
  
 [!code-sql[HowTo#sp_AddMergePub](../../../relational-databases/replication/codesnippet/tsql/create-a-publication_2.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilizzo di RMO (Replication Management Objects)  
 È possibile creare pubblicazioni a livello di programmazione tramite gli oggetti RMO (Replication Management Objects). Le classi RMO utilizzate per creare una pubblicazione dipendono dal tipo di pubblicazione creata.  
  
#### <a name="to-create-a-snapshot-or-transactional-publication"></a>Per creare una pubblicazione snapshot o transazionale  
  
1.  Creare una connessione al server di pubblicazione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.ReplicationDatabase> per il database di pubblicazione, impostare la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> sull'istanza di <xref:Microsoft.SqlServer.Management.Common.ServerConnection> del passaggio 1, quindi chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Se <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> restituisce **false**, verificare che il database esista.  
  
3.  Se la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.EnabledTransPublishing%2A> è impostata su **false**, impostarla su **true**.  
  
4.  Per una pubblicazione transazionale, verificare il valore della proprietà <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.LogReaderAgentExists%2A> . Se questa proprietà è impostata su **true**, per il database esiste già un processo dell'agente di lettura log. Se questa proprietà è impostata su **false**, eseguire le operazioni seguenti:  
  
    -   Impostare i campi <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> e <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> di <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.LogReaderAgentProcessSecurity%2A> in modo da fornire le credenziali dell'account [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows utilizzato per l'esecuzione dell'agente di lettura log.  
  
        > [!NOTE]  
        >  Non è necessario impostare <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.LogReaderAgentProcessSecurity%2A> quando la pubblicazione viene creata da un membro del ruolo predefinito del server **sysadmin** . In questo caso l'agente rappresenterà l'account di SQL Server Agent. Per altre informazioni, vedere [Modello di sicurezza dell'agente di replica](../../../relational-databases/replication/security/replication-agent-security-model.md).  
  
    -   (Facoltativo) Impostare i campi <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> e <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> o <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> di <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.LogReaderAgentPublisherSecurity%2A> quando si utilizza l'autenticazione di SQL Server per la connessione al server di pubblicazione.  
  
    -   Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.CreateLogReaderAgent%2A> per creare il processo dell'agente di lettura log per il database.  
  
5.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.TransPublication> e impostare le proprietà seguenti per questo oggetto:  
  
    -   Oggetto <xref:Microsoft.SqlServer.Management.Common.ServerConnection> indicato nel passaggio 1 per <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
    -   Nome del database pubblicato per <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A>.  
  
    -   Nome per la pubblicazione per <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>.  
  
    -   Valore <xref:Microsoft.SqlServer.Replication.PublicationType> di <xref:Microsoft.SqlServer.Replication.PublicationType.Transactional> o <xref:Microsoft.SqlServer.Replication.PublicationType.Snapshot>.  
  
    -   Campi <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> e <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> di <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A> per fornire le credenziali per l'account di Windows con cui verrà eseguito il processo dell'agente snapshot. Questo account viene utilizzato anche quando l'agente snapshot stabilisce connessioni al server di distribuzione locale e per qualsiasi connessione remota quando si utilizza l'autenticazione di Windows.  
  
        > [!NOTE]  
        >  Non è necessario impostare <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A> quando la pubblicazione viene creata da un membro del ruolo predefinito del server **sysadmin** . In questo caso l'agente rappresenterà l'account di SQL Server Agent. Per altre informazioni, vedere [Modello di sicurezza dell'agente di replica](../../../relational-databases/replication/security/replication-agent-security-model.md).  
  
    -   (Facoltativo) Campi <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> e <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> o <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> di <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentPublisherSecurity%2A> quando si utilizza l'autenticazione di SQL Server per la connessione al server di pubblicazione.  
  
    -   (Facoltativo) Utilizzare l'operatore logico OR inclusivo ( **|** in Visual C# e **Or** in Visual Basic) e l'operatore logico OR esclusivo ( **^** in Visual C# e **Xor** in Visual Basic) per impostare i valori di <xref:Microsoft.SqlServer.Replication.PublicationAttributes> per la proprietà <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> .  
  
    -   (Facoltativo) Nome del server di pubblicazione per <xref:Microsoft.SqlServer.Replication.TransPublication.PublisherName%2A> quando il server di pubblicazione non è non SQL Server.  
  
6.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.Publication.Create%2A> per creare la pubblicazione.  
  
    > [!IMPORTANT]  
    >  Quando si configura un server di pubblicazione con un server di distribuzione remoto, i valori specificati per tutte le proprietà, inclusa <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A>, vengono inviati al server di distribuzione come testo normale. È consigliabile crittografare la connessione tra il server di pubblicazione e il server di distribuzione remoto prima di chiamare il metodo <xref:Microsoft.SqlServer.Replication.Publication.Create%2A>. Per altre informazioni, vedere [Abilitare le connessioni crittografate al motore di database &#40;Gestione configurazione SQL Server&#41;](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
7.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.Publication.CreateSnapshotAgent%2A> per creare il processo dell'agente snapshot per la pubblicazione.  
  
#### <a name="to-create-a-merge-publication"></a>Per creare una pubblicazione di tipo merge  
  
1.  Creare una connessione al server di pubblicazione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.ReplicationDatabase> per il database di pubblicazione, impostare la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> sull'istanza di <xref:Microsoft.SqlServer.Management.Common.ServerConnection> del passaggio 1, quindi chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Se <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> restituisce **false**, verificare che il database esista.  
  
3.  Se <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.EnabledMergePublishing%2A> Property is **false**, impostarla su **true** e chiamare <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A>.  
  
4.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.MergePublication> e impostare le proprietà seguenti per questo oggetto:  
  
    -   Oggetto <xref:Microsoft.SqlServer.Management.Common.ServerConnection> indicato nel passaggio 1 per <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
    -   Nome del database pubblicato per <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A>.  
  
    -   Nome per la pubblicazione per <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>.  
  
    -   Campi <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> e <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> di <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A> per fornire le credenziali per l'account di Windows con cui verrà eseguito il processo dell'agente snapshot. Questo account viene utilizzato anche quando l'agente snapshot stabilisce connessioni al server di distribuzione locale e per qualsiasi connessione remota quando si utilizza l'autenticazione di Windows.  
  
        > [!NOTE]  
        >  Non è necessario impostare <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A> quando la pubblicazione viene creata da un membro del ruolo predefinito del server **sysadmin** . Per altre informazioni, vedere [Modello di sicurezza dell'agente di replica](../../../relational-databases/replication/security/replication-agent-security-model.md).  
  
    -   (Facoltativo) Utilizzare l'operatore logico OR inclusivo ( **|** in Visual C# e **Or** in Visual Basic) e l'operatore logico OR esclusivo ( **^** in Visual C# e **Xor** in Visual Basic) per impostare i valori di <xref:Microsoft.SqlServer.Replication.PublicationAttributes> per la proprietà <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> .  
  
5.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.Publication.Create%2A> per creare la pubblicazione.  
  
    > [!IMPORTANT]  
    >  Quando si configura un server di pubblicazione con un server di distribuzione remoto, i valori specificati per tutte le proprietà, inclusa <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A>, vengono inviati al server di distribuzione come testo normale. È consigliabile crittografare la connessione tra il server di pubblicazione e il server di distribuzione remoto prima di chiamare il metodo <xref:Microsoft.SqlServer.Replication.Publication.Create%2A>. Per altre informazioni, vedere [Abilitare le connessioni crittografate al motore di database &#40;Gestione configurazione SQL Server&#41;](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
6.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.Publication.CreateSnapshotAgent%2A> per creare il processo dell'agente snapshot per la pubblicazione.  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Esempi (RMO)  
 In questo esempio il database AdventureWorks viene abilitato per la pubblicazione transazionale, viene definito un processo dell'agente di lettura log e viene creata la pubblicazione AdvWorksProductTran. Per questa pubblicazione è necessario definire un articolo. Le credenziali dell'account di Windows necessarie per creare il processo dell'agente di lettura log e il processo dell'agente snapshot vengono passate in fase di esecuzione. Per informazioni sull'utilizzo di oggetti RMO per definire articoli snapshot e transazionali, vedere [Define an Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
 [!code-cs[HowTo#rmo_CreateTranPub](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createtranpub)]  
  
 [!code-vb[HowTo#rmo_vb_CreateTranPub](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createtranpub)]  
  
 In questo esempio il database AdventureWorks viene abilitato per la pubblicazione di tipo merge e viene creata la pubblicazione AdvWorksSalesOrdersMerge. Per questa pubblicazione è ancora necessario definire gli articoli. Le credenziali dell'account di Windows necessarie per creare il processo dell'agente snapshot vengono passate in fase di esecuzione. Per informazioni sull'utilizzo di oggetti RMO per definire articoli di merge, vedere [Define an Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
 [!code-cs[HowTo#rmo_CreateMergePub](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createmergepub)]  
  
 [!code-vb[HowTo#rmo_vb_CreateMergePub](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createmergepub)]  
  
## <a name="see-also"></a>Vedere anche  
 [Utilizzo di sqlcmd con variabili di scripting](../../../ssms/scripting/sqlcmd-use-with-scripting-variables.md)   
 [Pubblicare dati e oggetti di database](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Replication Management Objects Concepts](../../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [Define an Article](../../../relational-databases/replication/publish/define-an-article.md)   
 [Visualizzare e modificare le proprietà della pubblicazione](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [Configura distribuzione](../../../relational-databases/replication/configure-distribution.md)   
 [Proteggere il database di distribuzione](../../../relational-databases/replication/security/secure-the-distributor.md)   
 [Proteggere il server di pubblicazione](../../../relational-databases/replication/security/secure-the-publisher.md)  
  
