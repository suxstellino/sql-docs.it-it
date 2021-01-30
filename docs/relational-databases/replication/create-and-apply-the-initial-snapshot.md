---
title: Creare e applicare lo snapshot iniziale | Microsoft Docs
description: Informazioni su come creare e applicare lo snapshot iniziale in SQL Server tramite SQL Server Management Studio, Transact-SQL o Replication Management Objects.
ms.custom: ''
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- snapshots [SQL Server replication], creating
- snapshot replication [SQL Server], initial snapshots
ms.assetid: 742727a1-5189-44ec-b3ae-6fd7aa1f5347
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: e1ed0fd0126f63032fe7d041bfc4cd949561c14f
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076343"
---
# <a name="create-and-apply-the-initial-snapshot"></a>Creazione e applicazione dello snapshot iniziale
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
In questo argomento viene descritto come creare e applicare lo snapshot iniziale in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]o RMO (Replication Management Objects). Per le pubblicazioni di tipo merge che utilizzano filtri con parametri è necessario uno snapshot a due parti. Per altre informazioni, vedere [Creazione di uno snapshot per una pubblicazione di tipo merge con filtri con parametri](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  Gli snapshot vengono generati dall'agente snapshot al termine della creazione di una pubblicazione. La generazione può essere eseguita:  
  
-   Immediatamente. Per impostazione predefinita, per una pubblicazione di tipo merge uno snapshot viene generato immediatamente dopo la creazione di una pubblicazione mediante la Creazione guidata nuova pubblicazione.    
-   A un'ora pianificata. Specificare una pianificazione nella pagina **Agente snapshot** della Creazione guidata nuova pubblicazione o quando si utilizzano stored procedure o Replication Management Objects (RMO).    
-   Manualmente. Eseguire l'agente snapshot dal prompt dei comandi o da [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Per altre informazioni sull'esecuzione degli agenti, vedere [Concetti di base relativi ai file eseguibili dell'agente di replica](../../relational-databases/replication/concepts/replication-agent-executables-concepts.md) e [Avviare e arrestare un agente di replica &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md).  
  
Per la replica di tipo merge, viene generato uno snapshot a ogni esecuzione dell'agente snapshot. Per la replica transazionale, la generazione dello snapshot dipende dall'impostazione della proprietà di pubblicazione **immediate_sync**. Se tale proprietà è impostata su TRUE, ovvero il valore predefinito quando si utilizza la Creazione guidata nuova pubblicazione, viene generato uno snapshot ogni volta che viene eseguito l'agente snapshot, che può essere applicato a un Sottoscrittore in qualsiasi momento. Se invece è impostata su FALSE, ovvero il valore predefinito quando si utilizza **sp_addpublication**, lo snapshot viene generato solo se è stata aggiunta una nuova sottoscrizione dopo l'ultima esecuzione dell'agente snapshot. Per poter eseguire la sincronizzazione, è necessario che i sottoscrittori attendano il completamento dell'agente snapshot.  
  
Per impostazione predefinita, una volta generati gli snapshot vengono salvati nella cartella snapshot predefinita nel server di distribuzione. È anche possibile salvare i file di snapshot su supporti rimovibili come dischi rimovibili, CD-ROM o in posizioni diverse dalla cartella snapshot predefinita. È inoltre possibile comprimere i file in modo da semplificarne l'archiviazione e il trasferimento ed eseguire script prima o dopo aver applicato lo snapshot nel Sottoscrittore. Per altre informazioni su queste opzioni, vedere [Snapshot Options](../../relational-databases/replication/snapshot-options.md).  
  
Se lo snapshot è per una pubblicazione di tipo merge che utilizza filtri con parametri, verrà creato utilizzando un processo a due fasi. Viene innanzitutto creato uno snapshot dello schema contenente gli script di replica e lo schema degli oggetti pubblicati, ma non i dati. Ogni sottoscrizione viene quindi inizializzata con uno snapshot che include gli script e lo schema copiati dallo snapshot dello schema e i dati appartenenti alla partizione della sottoscrizione. Per altre informazioni, vedere [Snapshots for Merge Publications with Parameterized Filters](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
Dopo aver creato lo snapshot nel server di pubblicazione e averlo archiviato in una posizione snapshot predefinita o alternativa, sarà possibile trasferirlo nel Sottoscrittore e applicarlo. L'agente di distribuzione (per la replica snapshot o transazionale) o l'agente di merge (per la replica di tipo merge) trasferisce lo snapshot e applica lo schema e i file di dati al database di sottoscrizione sul Sottoscrittore durante la sincronizzazione iniziale. Per impostazione predefinita, se si utilizza la Creazione guidata nuova sottoscrizione la sincronizzazione iniziale viene eseguita subito dopo la creazione di una sottoscrizione. Questo comportamento è controllato dall'opzione **Inizializza quando** della pagina **Inizializza sottoscrizioni** della procedura guidata. Gli snapshot generati in seguito all'inizializzazione di una sottoscrizione non vengono applicati a un Sottoscrittore a meno che la sottoscrizione non sia contrassegnata per la reinizializzazione. Per altre informazioni, vedere [Reinizializzare le sottoscrizioni](../../relational-databases/replication/reinitialize-subscriptions.md).  
  
Dopo aver applicato lo snapshot iniziale, l'agente di distribuzione o l'agente di merge propaga gli aggiornamenti successivi e le altre modifiche dei dati. La distribuzione e l'applicazione degli snapshot nei Sottoscrittori hanno effetto solamente sui Sottoscrittori in attesa dello snapshot iniziale o di nuovi snapshot, mentre non hanno alcun effetto sui Sottoscrittori della pubblicazione in cui sono già state eseguite operazioni di inserimento, aggiornamento ed eliminazione o altre modifiche dei dati pubblicati.  

Per visualizzare o modificare la posizione della cartella snapshot predefinita, vedere  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]: [Modificare le opzioni snapshot](../../relational-databases/replication/snapshot-options.md)  
  
-   Programmazione della replica e programmazione di RMO: [Configurare la pubblicazione e la distribuzione](../../relational-databases/replication/configure-publishing-and-distribution.md)  

## <a name="default-snapshot-location"></a>Posizione predefinita degli snapshot

 Specificare la posizione predefinita degli snapshot nella pagina **Cartella snapshot** della Configurazione guidata distribuzione. Per altre informazioni sull'uso di questa procedura guidata, vedere [Configurare la pubblicazione e la distribuzione](../../relational-databases/replication/configure-publishing-and-distribution.md). Se si crea una pubblicazione su un server non configurato come server di distribuzione, specificare una posizione predefinita degli snapshot nella pagina **Cartella snapshot** della Creazione guidata nuova pubblicazione. Per altre informazioni sull'uso di questa procedura guidata, vedere [Creare una pubblicazione](../../relational-databases/replication/publish/create-a-publication.md).  
  
 Modificare la posizione predefinita degli snapshot nella pagina **Server di pubblicazione** della finestra di dialogo **Proprietà database di distribuzione - \<Distributor>** . Per altre informazioni, vedere [Visualizzare e modificare le proprietà del server di pubblicazione e del database di distribuzione](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md). Impostare la cartella snapshot per ogni pubblicazione nella finestra di dialogo **Proprietà pubblicazione - \<Publication>** . Per altre informazioni, vedere [View and Modify Publication Properties](../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
### <a name="modify-the-default-snapshot-location"></a>Modificare la posizione predefinita degli snapshot  
  
1.  Nella pagina **Server di pubblicazione** della finestra di dialogo **Proprietà database di distribuzione - \<Distributor>** fare clic sul pulsante delle proprietà ( **...** ) per il server di pubblicazione di cui modificare la posizione predefinita degli snapshot.  
  
2.  Nella finestra di dialogo **Proprietà server di pubblicazione - \<Publisher>** immettere un valore per la proprietà **Cartella snapshot predefinita**.  
  
    > [!NOTE]  
    >  L'agente snapshot deve disporre delle autorizzazioni di scrittura per la directory specificata, mentre l'agente di distribuzione o l'agente di merge deve disporre delle autorizzazioni di lettura. Se si usano sottoscrizioni pull, è necessario specificare una directory condivisa come percorso UNC (Universal Naming Convention), ad esempio \\\nomecomputer\snapshot. Per altre informazioni, vedere [Proteggere la cartella snapshot](../../relational-databases/replication/security/secure-the-snapshot-folder.md).  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  

## <a name="create-snapshot"></a>Creare snapshot
Per impostazione predefinita, se SQL Server Agent è in esecuzione, l'agente di snapshot genera immediatamente uno snapshot dopo aver creato una pubblicazione con la Creazione guidata nuova pubblicazione. Sempre per impostazione predefinita, tale snapshot viene quindi applicato dall'agente di distribuzione (per la replica snapshot e transazionale) o dall'agente di merge (per le sottoscrizioni di tipo merge) per tutte le sottoscrizioni. È anche possibile generare uno snapshot utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] e Monitoraggio replica. Per informazioni sull'avvio di Monitoraggio replica, vedere [Avviare Monitoraggio replica](../../relational-databases/replication/monitor/start-the-replication-monitor.md).  

### <a name="using-sql-server-management-studio"></a>Utilizzare SQL Server Management Studio

1.  Connettersi al server di pubblicazione in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]e quindi espandere il nodo del server.    
2.  Espandere la cartella **Replica** e quindi la cartella **Pubblicazioni locali** .    
3.  Fare clic con il pulsante destro del mouse sulla pubblicazione per la quale si desidera creare uno snapshot, quindi scegliere **Visualizza stato agente snapshot**.    
4.  Nella finestra di dialogo **Visualizza stato agente snapshot - \<Publication>** fare clic su **Avvia**.    
 Al termine della generazione dello snapshot, verrà visualizzato un messaggio del tipo "[100%] Generato uno snapshot di 17 articoli."  
  
### <a name="in-replication-monitor"></a>In Monitoraggio replica  
  
1.  In Monitoraggio replica espandere un gruppo di server di pubblicazione nel riquadro di sinistra e quindi espandere un server di pubblicazione.    
2.  Fare clic con il pulsante destro del mouse sulla pubblicazione per la quale si desidera generare uno snapshot, quindi scegliere **Genera snapshot**.    
3.  Per visualizzare lo stato dell'agente snapshot, fare clic sulla scheda **Agenti** . Per informazioni più dettagliate, fare clic con il pulsante destro del mouse sull'agente snapshot nella griglia e scegliere **Visualizza dettagli**.  

## <a name="using-transact-sql"></a>Uso di Transact-SQL
Gli snapshot iniziali possono essere creati a livello di programmazione creando ed eseguendo un processo dell'agente snapshot o eseguendo il file eseguibile dell'agente snapshot da un file batch. Dopo la generazione, lo snapshot iniziale viene trasferito e applicato al Sottoscrittore la prima volta che la sottoscrizione viene sincronizzata. Se si esegue l'agente snapshot da un prompt dei comandi o un file batch, sarà necessario rieseguirlo ogni volta che lo snapshot esistente diventa non valido.  
  
> [!IMPORTANT]  
>  Se possibile, richiedere agli utenti di immettere le credenziali di sicurezza in fase di esecuzione. Se è necessario archiviare le credenziali in un file script, è fondamentale proteggere il file per evitare accessi non autorizzati.  

1.  Creare una pubblicazione snapshot, transazionale o di tipo merge. Per altre informazioni, vedere [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md).  
  
2.  Eseguire [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md). Specificare **\@publication** e i parametri seguenti:  
  
    -   **\@job_login**, che specifica le credenziali dell'autenticazione di Windows con cui l'agente di snapshot viene eseguito nel server di distribuzione.  
  
    -   **\@job_password**, che corrisponde alla password per le credenziali di Windows specificate.  
  
    -   (Facoltativo) Valore **0** per **\@publisher_security_mode** se l'agente userà l'autenticazione di SQL Server per la connessione al server di pubblicazione. In questo caso, è necessario specificare anche le informazioni di accesso dell'autenticazione di SQL Server per **\@publisher_login** e **\@publisher_password**.  
  
    -   (Facoltativo) Pianificazione della sincronizzazione per il processo dell'agente snapshot. Per altre informazioni, vedere [Specify Synchronization Schedules](../../relational-databases/replication/specify-synchronization-schedules.md).  
  
    > [!IMPORTANT]  
    >  Quando si configura un server di pubblicazione con un server di distribuzione remoto, i valori specificati per tutti i parametri, inclusi *job_login* e *job_password*, vengono inviati al server di distribuzione come testo normale. È consigliabile crittografare la connessione tra il server di pubblicazione e il server di distribuzione remoto prima di eseguire questa stored procedure. Per altre informazioni, vedere [Abilitare le connessioni crittografate al motore di database &#40;Gestione configurazione SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
3.  Aggiungere articoli alla pubblicazione. Per altre informazioni, vedere [definire un articolo](../../relational-databases/replication/publish/define-an-article.md).  
  
4.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_startpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-startpublication-snapshot-transact-sql.md), specificando il valore **\@publication** dal passaggio 1.  
  
## <a name="apply-a-snapshot"></a>Applicare uno snapshot  

### <a name="using-sql-server-management-studio"></a>Utilizzare SQL Server Management Studio
  
1.  Al termine della generazione, lo snapshot verrà applicato mediante la sincronizzazione della sottoscrizione con l'agente di distribuzione o l'agente di merge:   
    -   Se l'agente è impostato per l'esecuzione continua, ovvero l'impostazione predefinita per la replica transazionale, lo snapshot verrà applicato automaticamente al termine della generazione.   
    -   Se è invece impostato per l'esecuzione in base a una pianificazione, lo snapshot verrà applicato alla successiva esecuzione pianificata dell'agente.    
    -   Se l'agente è impostato per l'esecuzione su richiesta, lo snapshot verrà applicato alla successiva esecuzione dell'agente.  
  
     Per ulteriori informazioni sulla sincronizzazione delle sottoscrizioni, vedere [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md) e [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
###   <a name="use-transact-sql"></a>Usare Transact-SQL  
 
1.  Creare una pubblicazione snapshot, transazionale o di tipo merge. Per altre informazioni, vedere [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md).  
  
2.  Aggiungere articoli alla pubblicazione. Per altre informazioni, vedere [definire un articolo](../../relational-databases/replication/publish/define-an-article.md).  
  
3.  Dal prompt dei comandi o in un file batch avviare l' [Agente merge repliche](../../relational-databases/replication/agents/replication-snapshot-agent.md) eseguendo **snapshot.exe** con gli argomenti della riga di comando seguenti:  
  
    -   **-Publication**  
    -   **-Publisher**  
    -   **-Distributor**   
    -   **-PublisherDB**   
    -   **-ReplicationType**  
  
     Se si usano l'autenticazione di SQL Server, è inoltre necessario specificare gli argomenti seguenti:  
  
    -   **-DistributorLogin**    
    -   **-DistributorPassword**   
    -   **-DistributorSecurityMode** = **0**    
    -   **-PublisherLogin**    
    -   **-PublisherPassword**    
    -   **-PublisherSecurityMode** = **0**  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Esempi (Transact-SQL)  
 In questo esempio viene illustrato come creare una pubblicazione transazionale e aggiungere un processo dell'agente snapshot per la nuova pubblicazione (utilizzando le variabili di scripting **SQLCMD** ). Viene inoltre avviato il processo.  
  
 [!code-sql[HowTo#sp_trangenerate_snapshot](../../relational-databases/replication/codesnippet/tsql/create-and-apply-the-ini_1.sql)]  
  
 In questo esempio viene creata una pubblicazione di tipo merge e viene aggiunto un processo dell'agente snapshot (utilizzando le variabili **sqlcmd** ) per la pubblicazione. Viene inoltre avviato il processo.  
  
 [!code-sql[HowTo#sp_mergegenerate_snapshot](../../relational-databases/replication/codesnippet/tsql/create-and-apply-the-ini_2.sql)]  
  
 Con gli argomenti della riga di comando seguenti l'agente snapshot viene avviato per generare lo snapshot per una pubblicazione di tipo merge.  
  
> [!NOTE]  
>  Le interruzioni di riga sono state aggiunte per agevolare la lettura. I comandi in un file batch devono essere inseriti in un'unica riga.  
  
```  
  
REM -- Declare variables  
SET Publisher=%InstanceName%  
SET PublicationDB=AdventureWorks2012   
SET Publication=AdvWorksSalesOrdersMerge   
  
REM --Start the Snapshot Agent to generate the snapshot for AdvWorksSalesOrdersMerge.  
"C:\Program Files\Microsoft SQL Server\120\COM\SNAPSHOT.EXE" -Publication %Publication%   
-Publisher %Publisher% -Distributor %Publisher% -PublisherDB %PublicationDB%   
-ReplicationType 2 -OutputVerboseLevel 1 -DistributorSecurityMode 1  
  
```  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilizzo di RMO (Replication Management Objects)  
 L'agente snapshot genera gli snapshot al termine della creazione di una pubblicazione. È possibile generare questi snapshot a livello di programmazione tramite gli oggetti RMO (Replication Management Objects) e l'accesso diretto tramite codice gestito alle funzionalità dell'agente di replica. Gli oggetti utilizzati dipendono dal tipo di replica. L'agente snapshot può essere avviato in modo sincrono tramite l'oggetto <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent> o in modo asincrono tramite il processo dell'agente. Dopo la generazione, lo snapshot iniziale viene trasferito e applicato al Sottoscrittore la prima volta che la sottoscrizione viene sincronizzata. È necessario rieseguire l'agente ogni volta che lo snapshot esistente non contiene più dati validi e aggiornati. Per altre informazioni, vedere [Gestire le pubblicazioni](../../relational-databases/replication/publish/maintain-publications.md).  
  
> [!IMPORTANT]  
>  Se possibile, richiedere agli utenti di immettere le credenziali di sicurezza in fase di esecuzione. Se è necessario archiviare le credenziali, utilizzare i [servizi di crittografia](/previous-versions/aa719848(v=vs.71)) offerti da [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows .NET Framework.  
  
#### <a name="to-generate-the-initial-snapshot-for-a-snapshot-or-transactional-publication-by-starting-the-snapshot-agent-job-asynchronous"></a>Per generare lo snapshot iniziale per una pubblicazione snapshot o transazionale avviando il processo dell'agente snapshot (modo asincrono)  
  
1.  Creare una connessione al server di pubblicazione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.TransPublication>. Impostare le proprietà <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> e <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> per la pubblicazione, quindi impostare la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> sulla connessione creata nel passaggio 1.  
  
3.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> per caricare le proprietà rimanenti dell'oggetto. Se questo metodo restituisce **false**, le proprietà della pubblicazione sono state definite in modo non corretto nel passaggio 2 oppure la pubblicazione non esiste.  
  
4.  Se il valore di <xref:Microsoft.SqlServer.Replication.Publication.SnapshotAgentExists%2A> è **false**, chiamare <xref:Microsoft.SqlServer.Replication.Publication.CreateSnapshotAgent%2A> per creare il processo dell'agente snapshot per questa pubblicazione.  
  
5.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.Publication.StartSnapshotGenerationAgentJob%2A> per avviare il processo dell'agente che genera lo snapshot per la pubblicazione.  
  
6.  (Facoltativo) Quando il valore di <xref:Microsoft.SqlServer.Replication.TransPublication.SnapshotAvailable%2A> è **true**, lo snapshot è disponibile per i Sottoscrittori.  
  
#### <a name="to-generate-the-initial-snapshot-for-a-snapshot-or-transactional-publication-by-running-the-snapshot-agent-synchronous"></a>Per generare lo snapshot iniziale per una pubblicazione snapshot o transazionale eseguendo il processo dell'agente snapshot (modo sincrono)  
  
1.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent> e impostare le seguenti proprietà obbligatorie:  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Publisher%2A> : nome del server di pubblicazione  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherDatabase%2A> : nome del database di pubblicazione  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Publication%2A> : nome della pubblicazione  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Distributor%2A> : nome del server di distribuzione  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherSecurityMode%2A> : valore <xref:Microsoft.SqlServer.Replication.SecurityMode.Integrated> per utilizzare l'autenticazione di Windows per le connessioni al server di pubblicazione o valore <xref:Microsoft.SqlServer.Replication.SecurityMode.Standard> e valori relativi a <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherLogin%2A> e <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherPassword%2A> per utilizzare l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per le connessioni al server di pubblicazione. L'autenticazione di Windows è la scelta consigliata.  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DistributorSecurityMode%2A> : valore <xref:Microsoft.SqlServer.Replication.SecurityMode.Integrated> per utilizzare l'autenticazione di Windows per le connessioni al server di distribuzione o valore <xref:Microsoft.SqlServer.Replication.SecurityMode.Standard> e valori relativi a <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DistributorLogin%2A> e <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DistributorPassword%2A> per utilizzare l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per le connessioni al server di distribuzione. L'autenticazione di Windows è la scelta consigliata.  
  
2.  Impostare il valore <xref:Microsoft.SqlServer.Replication.ReplicationType.Transactional> o <xref:Microsoft.SqlServer.Replication.ReplicationType.Snapshot> per <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.ReplicationType%2A>.  
  
3.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.GenerateSnapshot%2A> .  
  
#### <a name="to-generate-the-initial-snapshot-for-a-merge-publication-by-starting-the-snapshot-agent-job-asynchronous"></a>Per generare lo snapshot iniziale per una pubblicazione di tipo merge avviando il processo dell'agente snapshot (modo asincrono)  
  
1.  Creare una connessione al server di pubblicazione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.MergePublication>. Impostare le proprietà <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> e <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> per la pubblicazione, quindi impostare la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> sulla connessione creata nel passaggio 1.  
  
3.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> per caricare le proprietà rimanenti dell'oggetto. Se questo metodo restituisce **false**, le proprietà della pubblicazione sono state definite in modo non corretto nel passaggio 2 oppure la pubblicazione non esiste.  
  
4.  Se il valore di <xref:Microsoft.SqlServer.Replication.Publication.SnapshotAgentExists%2A> è **false**, chiamare <xref:Microsoft.SqlServer.Replication.Publication.CreateSnapshotAgent%2A> per creare il processo dell'agente snapshot per questa pubblicazione.  
  
5.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.Publication.StartSnapshotGenerationAgentJob%2A> per avviare il processo dell'agente che genera lo snapshot per la pubblicazione.  
  
6.  (Facoltativo) Quando il valore di <xref:Microsoft.SqlServer.Replication.MergePublication.SnapshotAvailable%2A> è **true**, lo snapshot è disponibile per i Sottoscrittori.  
  
#### <a name="to-generate-the-initial-snapshot-for-a-merge-publication-by-running-the-snapshot-agent-synchronous"></a>Per generare lo snapshot iniziale per una pubblicazione di tipo merge eseguendo il processo dell'agente snapshot (modo sincrono)  
  
1.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent> e impostare le seguenti proprietà obbligatorie:  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Publisher%2A> : nome del server di pubblicazione  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherDatabase%2A> : nome del database di pubblicazione  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Publication%2A> : nome della pubblicazione  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Distributor%2A> : nome del server di distribuzione  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherSecurityMode%2A> : valore <xref:Microsoft.SqlServer.Replication.SecurityMode.Integrated> per utilizzare l'autenticazione di Windows per le connessioni al server di pubblicazione o valore <xref:Microsoft.SqlServer.Replication.SecurityMode.Standard> e valori relativi a <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherLogin%2A> e <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherPassword%2A> per utilizzare l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per le connessioni al server di pubblicazione. L'autenticazione di Windows è la scelta consigliata.  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DistributorSecurityMode%2A> : valore <xref:Microsoft.SqlServer.Replication.SecurityMode.Integrated> per utilizzare l'autenticazione di Windows per le connessioni al server di distribuzione o valore <xref:Microsoft.SqlServer.Replication.SecurityMode.Standard> e valori relativi a <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DistributorLogin%2A> e <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DistributorPassword%2A> per utilizzare l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per le connessioni al server di distribuzione. L'autenticazione di Windows è la scelta consigliata.  
  
2.  Impostare un valore di <xref:Microsoft.SqlServer.Replication.ReplicationType.Merge> per <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.ReplicationType%2A>.  
  
3.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.GenerateSnapshot%2A> .  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Esempi (RMO)  
 In questo esempio viene eseguito l'agente snapshot in modo sincrono per generare lo snapshot iniziale per una pubblicazione transazionale.  
  
 [!code-cs[HowTo#rmo_GenerateSnapshot](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_generatesnapshot)]  
  
 [!code-vb[HowTo#rmo_vb_GenerateSnapshot](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_generatesnapshot)]  
  
 In questo esempio viene avviato l'agente snapshot in modo asincrono per generare lo snapshot iniziale per una pubblicazione transazionale.  
  
 [!code-cs[HowTo#rmo_GenerateSnapshot_WithJob](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_generatesnapshot_withjob)]  
  
 [!code-vb[HowTo#rmo_vb_GenerateSnapshot_WithJob](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_generatesnapshot_withjob)]  
  
## <a name="see-also"></a>Vedere anche  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md)   
 [Specify Synchronization Schedules](../../relational-databases/replication/specify-synchronization-schedules.md)   
 [Inizializzare una sottoscrizione con uno snapshot](../../relational-databases/replication/initialize-a-subscription-with-a-snapshot.md)   
 [Replication Management Objects Concepts](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [Replication Security Best Practices](../../relational-databases/replication/security/replication-security-best-practices.md)   
 [Replication System Stored Procedures Concepts](../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Utilizzo di sqlcmd con variabili di scripting](../../ssms/scripting/sqlcmd-use-with-scripting-variables.md)  
  
