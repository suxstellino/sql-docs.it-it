---
title: Creare uno snapshot con filtri con parametri (merge)
description: Informazioni sulla creazione di uno snapshot per una pubblicazione di tipo merge usando filtri con parametri.
ms.custom: seo-lt-2019
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- parameterized filters [SQL Server replication], snapshots
- snapshots [SQL Server replication], parameterized filters and
- filters [SQL Server replication], parameterized
ms.assetid: 00dfb229-f1de-4d33-90b0-d7c99ab52dcb
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 89a470964db8b0475a6c5d4f20a5007337645cb1
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076153"
---
# <a name="create-a-snapshot-for-a-merge-publication-with-parameterized-filters"></a>Creazione di uno snapshot per una pubblicazione di tipo merge con filtri con parametri
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
In questo argomento viene descritto come creare un snapshot per una pubblicazione di tipo merge con i filtri con parametri in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]o Replication Management Objects (RMO).  

Quando si utilizzano filtri di riga con parametri nelle pubblicazioni di tipo merge, ogni sottoscrizione con uno snapshot in due parti viene inizializzata dalla replica. Viene innanzitutto creato uno snapshot dello schema contenente tutti gli oggetti necessari alla replica e lo schema degli oggetti pubblicati, ma non i dati. Ogni sottoscrizione viene quindi inizializzata con uno snapshot che include gli oggetti e lo schema dello snapshot dello schema e i dati appartenenti alla partizione della sottoscrizione. Se più di una sottoscrizione riceve una determinata partizione, ovvero riceve lo stesso schema e gli stessi dati, lo snapshot di tale partizione viene creato una sola volta. Dallo stesso snapshot vengono inizializzate più sottoscrizioni. Per ulteriori informazioni sui filtri di riga con parametri, vedere [Filtri di riga con parametri](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
 È possibile creare snapshot per pubblicazioni con filtri con parametri in tre modi diversi:  
  
-   **Pregenerare uno snapshot per ogni partizione.** Questa opzione consente di controllare il momento in cui vengono generati gli snapshot.    
     È inoltre possibile aggiornare gli snapshot in base a una pianificazione. I nuovi Sottoscrittori che sottoscrivono una partizione per cui è stato creato uno snapshot riceveranno uno snapshot aggiornato.   
-   **Consentire ai Sottoscrittori di richiedere la generazione** e l'applicazione dello snapshot alla prima sincronizzazione. Questa opzione consente ai nuovi Sottoscrittori di eseguire la sincronizzazione senza l'intervento di un amministratore. Per consentire la generazione dello snapshot, è necessario che[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent sia in esecuzione nel server di pubblicazione.  
  
    > [!NOTE]  
    >  Se il filtro di uno o più articoli nella pubblicazione restituisce partizioni non sovrapposte univoche per ogni sottoscrizione, i metadati vengono eliminati a ogni esecuzione dell'agente di merge. Lo snapshot partizionato scade quindi più rapidamente. Quando si utilizza questa opzione, è consigliabile consentire ai Sottoscrittori di inizializzare la generazione e il recapito dello snapshot. Per ulteriori informazioni sulle opzioni di filtro, vedere [Filtri di riga con parametri](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
-   **Generare manualmente uno snapshot per ogni Sottoscrittore con l'agente di snapshot**. Il Sottoscrittore deve quindi indicare il percorso dello snapshot all'agente di merge, in modo che sia possibile recuperare e applicare lo snapshot corretto.  
  
    > [!NOTE]  
    >  Questa opzione è supportata per garantire la compatibilità con le versioni precedenti e non consente le condivisioni snapshot FTP.  
  
 Il metodo più flessibile consiste nell'utilizzo di una combinazione di opzioni snapshot pregenerate e richieste dal Sottoscrittore: gli snapshot vengono pregenerati e aggiornati in base a una pianificazione, in genere durante i periodi di minore attività, ma un Sottoscrittore può generare il proprio snapshot nel caso in cui venga creata una sottoscrizione per cui è necessaria una nuova partizione.  
  
 Considerare [!INCLUDE[ssSampleDBCoShort](../../includes/sssampledbcoshort-md.md)], che dispone di forza lavoro mobile per la distribuzione delle scorte ai singoli negozi. Ogni venditore riceve una sottoscrizione basata sul proprio account di accesso, che consente di recuperare i dati relativi ai negozi serviti. L'amministratore decide di pregenerare gli snapshot e di aggiornarli ogni domenica. Occasionalmente viene aggiunto al sistema un nuovo utente per il quale sono necessari i dati per una partizione per cui non è disponibile alcuno snapshot. L'amministratore decide inoltre di consentire gli snapshot inizializzati dal Sottoscrittore, per evitare che si verifichi una situazione in cui un Sottoscrittore non può sottoscrivere la pubblicazione in quanto lo snapshot non è ancora disponibile. Quando il nuovo Sottoscrittore effettua la prima connessione, lo snapshot per la partizione specificata viene generato e viene applicato al Sottoscrittore. Per consentire la generazione dello snapshot, è necessario che[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent sia in esecuzione nel server di pubblicazione.  
  
 Per creare uno snapshot per una pubblicazione con filtri con parametri, vedere [Creazione di uno snapshot per una pubblicazione di tipo merge con filtri con parametri](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
## <a name="security-settings-for-the-snapshot-agent"></a>Impostazioni di sicurezza per l'agente snapshot  
 L'agente snapshot crea gli snapshot per ogni partizione. Per gli snapshot pregenerati e per quelli richiesti da un Sottoscrittore, viene eseguito l'agente e vengono effettuate le connessioni con le credenziali specificate al momento della creazione del processo dell'agente snapshot. Tale processo viene creato tramite la Creazione guidata nuova pubblicazione o **sp_addpublication_snapshot**. Per modificare le credenziali, utilizzare **sp_changedynamicsnapshot_job**. Per altre informazioni, vedere [sp_changedynamicsnapshot_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedynamicsnapshot-job-transact-sql.md).  

  
##  <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni  
  
-   Quando si genera uno snapshot per una pubblicazione di tipo merge utilizzando filtri con parametri, è necessario innanzitutto generare uno snapshot (schema) standard che contiene tutti i dati pubblicati e i metadati del Sottoscrittore per la sottoscrizione. Per altre informazioni, vedere [Creazione e applicazione dello snapshot iniziale](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md). Dopo aver creato lo snapshot dello schema, è possibile generare lo snapshot che contiene la partizione dei dati pubblicati specifica del Sottoscrittore.  
  
-   Se il filtro di uno o più articoli nella pubblicazione restituisce partizioni non sovrapposte univoche per ogni sottoscrizione, i metadati vengono eliminati a ogni esecuzione dell'agente di merge. Lo snapshot partizionato scade quindi più rapidamente. Quando si utilizza questa opzione, è consigliabile consentire ai Sottoscrittori di inizializzare la generazione e il recapito dello snapshot. 
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
 Generare snapshot per le partizioni nella pagina **Partizioni dati** della finestra di dialogo **Proprietà pubblicazione - \<Publication>** . Per ulteriori informazioni sull'accesso a questa finestra di dialogo, vedere [View and Modify Publication Properties](../../relational-databases/replication/publish/view-and-modify-publication-properties.md). È possibile consentire ai Sottoscrittori di avviare la generazione e il recapito degli snapshot e/o di generare snapshot.  
  
 Prima di generare gli snapshot per una o più partizioni, è necessario:  
  
1.  Creare una pubblicazione di tipo merge mediante Creazione guidata nuova pubblicazione e specificare uno o più filtri di righe con parametri nella pagina **Aggiungi filtro** della procedura guidata. Per altre informazioni, vedere [Definizione e modifica di un filtro di riga con parametri per un articolo di merge](../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md).  
  
2.  Generare uno snapshot dello schema per la pubblicazione. Per impostazione predefinita, lo snapshot dello schema viene generato quando si completa la Creazione guidata nuova pubblicazione, ma è possibile generarne uno anche mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  

#### <a name="to-generate-a-schema-snapshot"></a>Per generare uno snapshot dello schema  
  
1.  Connettersi al server di pubblicazione in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]e quindi espandere il nodo del server.  
  
2.  Espandere la cartella **Replica** e quindi la cartella **Pubblicazioni** .  
  
3.  Fare clic con il pulsante destro del mouse sulla pubblicazione per la quale si desidera creare uno snapshot, quindi scegliere **Visualizza stato agente snapshot**.  
  
4.  Nella finestra di dialogo **Visualizza stato agente snapshot - \<Publication>** fare clic su **Avvia**.  
  
     Al termine della generazione dello snapshot, verrà visualizzato un messaggio del tipo "[100%] Generato uno snapshot di 17 articoli."  
  
#### <a name="to-allow-subscribers-to-initiate-snapshot-generation-and-delivery"></a>Per consentire ai Sottoscrittori di avviare la generazione e il recapito di snapshot  
  
1.  Nella pagina **Partizioni dati** della finestra di dialogo **Proprietà pubblicazione - \<Publication>** selezionare **Definisci automaticamente una partizione e genera uno snapshot, se necessario, quando un nuovo Sottoscrittore cerca di eseguire la sincronizzazione**.  
  
2.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
#### <a name="to-generate-and-refresh-snapshots"></a>Per generare e aggiornare gli snapshot  
  
1.  Nella pagina **Partizioni dati** della finestra di dialogo **Proprietà pubblicazione - \<Publication>** fare clic su **Aggiungi**.  
  
2.  Immettere un valore per le funzioni **HOST_NAME()** e/o **SUSER_SNAME()** associate alla partizione per cui si desidera creare uno snapshot.  
  
3.  Facoltativamente, specificare una pianificazione per l'aggiornamento degli snapshot:  
  
    1.  Selezionare **Usa la pianificazione seguente per l'esecuzione dell'agente snapshot per questa partizione**.  
  
    2.  Accettare la pianificazione predefinita per l'aggiornamento degli snapshot oppure fare clic su **Cambia** per specificare una pianificazione diversa.  
  
4.  Facendo clic su **OK** si torna nella finestra di dialogo **Proprietà pubblicazione - \<Publication>** .  
  
5.  Selezionare la partizione nella griglia delle proprietà e quindi fare clic su **Genera gli snapshot selezionati adesso**.  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
 L'utilizzo di stored procedure e dell'agente snapshot consente di eseguire le attività indicate di seguito:  
  
-   Consentire ai Sottoscrittori di richiedere la generazione e l'applicazione dello snapshot alla prima sincronizzazione.  
  
-   Pregenerare uno snapshot per ogni partizione.  
  
-   Generare manualmente uno snapshot per ciascun Sottoscrittore.  
  
    > [!IMPORTANT]  
    >  Se possibile, richiedere agli utenti di immettere le credenziali di sicurezza in fase di esecuzione. Se è necessario archiviare le credenziali in un file script, è fondamentale proteggere il file per evitare accessi non autorizzati.  
  
#### <a name="to-create-a-publication-that-allows-subscribers-to-initiate-snapshot-generation-and-delivery"></a>Per creare una pubblicazione che consente ai Sottoscrittori di avviare la generazione e il recapito di snapshot  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_addmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md). Specificare i parametri seguenti:  
  
    -   Il nome della pubblicazione per **\@publication**.  
  
    -   Il valore **true** per **\@allow_subscriber_initiated_snapshot**, che consente ai Sottoscrittori di avviare il processo di snapshot.  
  
    -   (Facoltativo) Il numero di processi di snapshot dinamici che è possibile eseguire simultaneamente per **\@max_concurrent_dynamic_snapshots**. Se è in esecuzione il numero massimo di processi e un Sottoscrittore tenta di generare uno snapshot, il processo verrà inserito in una coda. Per impostazione predefinita, non esiste nessun limite per il numero di processi simultanei.  
  
2.  Nel server di pubblicazione eseguire [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md). Specificare il nome della pubblicazione usato nel passaggio 1 per **\@publication** e le credenziali di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows usate per l'[agente di snapshot della replica](../../relational-databases/replication/agents/replication-snapshot-agent.md) per **\@job_login** e **\@password**. Se l'agente usa l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per la connessione al server di pubblicazione, è necessario specificare anche il valore **0** per **\@publisher_security_mode** e le informazioni di accesso di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per **\@publisher_login** e **\@publisher_password**. Verrà creato un processo dell'agente snapshot per la pubblicazione. Per ulteriori informazioni sulla generazione di uno snapshot iniziale e sulla definizione di una pianificazione personalizzata per l'agente snapshot, vedere [Create and Apply the Initial Snapshot](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
    > [!IMPORTANT]  
    >  Quando si configura un server di pubblicazione con un server di distribuzione remoto, i valori specificati per tutti i parametri, inclusi *job_login* e *job_password*, vengono inviati al server di distribuzione come testo normale. È consigliabile crittografare la connessione tra il server di pubblicazione e il server di distribuzione remoto prima di eseguire questa stored procedure. Per altre informazioni, vedere [Abilitare le connessioni crittografate al motore di database &#40;Gestione configurazione SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
3.  Per aggiungere articoli alla pubblicazione, eseguire [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Questa stored procedure deve essere eseguita una sola volta per ogni articolo della pubblicazione. Quando si usano filtri con parametri, è necessario specificare un filtro di riga con parametri per uno o più articoli usando il parametro **\@subset_filterclause**. Per altre informazioni, vedere [Definizione e modifica di un filtro di riga con parametri per un articolo di merge](../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md).  
  
4.  Se il filtro di riga con parametri verrà usato per filtrare altri articoli, eseguire [sp_addmergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md) per definire le relazioni di join o tra record logici tra gli articoli. Questa stored procedure deve essere eseguita una sola volta per ogni relazione da definire. Per altre informazioni, vedere [Definizione e modifica di un filtro di join tra articoli di merge](../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md).  
  
5.  Quando l'agente di merge richiede che il Sottoscrittore venga inizializzato dallo snapshot, lo snapshot relativo alla partizione della sottoscrizione che ha eseguito la richiesta viene generato automaticamente.  
  
#### <a name="to-create-a-publication-and-pre-generate-or-automatically-refresh-snapshots"></a>Per creare una pubblicazione e pregenerare o aggiornare automaticamente gli snapshot  
  
1.  Eseguire [sp_addmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md) per creare la pubblicazione. Per altre informazioni, vedere [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md).  
  
2.  Nel server di pubblicazione eseguire [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md). Specificare il nome della pubblicazione usato nel passaggio 1 per **\@publication** e le credenziali di Windows usate per l'agente di snapshot per **\@job_login** e **\@password**. Se l'agente userà l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per la connessione al server di pubblicazione, è necessario specificare anche il valore **0** per **\@publisher_security_mode** e le informazioni di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per **\@publisher_login** e **\@publisher_password**. Verrà creato un processo dell'agente snapshot per la pubblicazione. Per ulteriori informazioni sulla generazione di uno snapshot iniziale e sulla definizione di una pianificazione personalizzata per l'agente snapshot, vedere [Create and Apply the Initial Snapshot](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
    > [!IMPORTANT]  
    >  Quando si configura un server di pubblicazione con un server di distribuzione remoto, i valori specificati per tutti i parametri, inclusi *job_login* e *job_password*, vengono inviati al server di distribuzione come testo normale. È consigliabile crittografare la connessione tra il server di pubblicazione e il server di distribuzione remoto prima di eseguire questa stored procedure. Per altre informazioni, vedere [Abilitare le connessioni crittografate al motore di database &#40;Gestione configurazione SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
3.  Per aggiungere articoli alla pubblicazione, eseguire [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Questa stored procedure deve essere eseguita una sola volta per ogni articolo della pubblicazione. Quando si usano filtri con parametri, è necessario specificare un filtro di riga con parametri per un articolo usando il parametro **\@subset_filterclause**. Per altre informazioni, vedere [Definizione e modifica di un filtro di riga con parametri per un articolo di merge](../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md).  
  
4.  Se il filtro di riga con parametri verrà usato per filtrare altri articoli, eseguire [sp_addmergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md) per definire le relazioni di join o tra record logici tra gli articoli. Questa stored procedure deve essere eseguita una sola volta per ogni relazione da definire. Per altre informazioni, vedere [Definizione e modifica di un filtro di join tra articoli di merge](../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md).  
  
5.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_helpmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md), specificando il valore **\@publication** dal passaggio 1. Si noti il valore di **snapshot_jobid** nel set di risultati.  
  
6.  Convertire il valore di **snapshot_jobid** ottenuto al passaggio 5 in **uniqueidentifier**.  
  
7.  Nel database **msdb** del server di pubblicazione eseguire [sp_start_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md) specificando il valore convertito ottenuto nel passaggio 6 per **\@job_id**.  
  
8.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_addmergepartition &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepartition-transact-sql.md). Specificare il nome della pubblicazione ottenuto nel passaggio 1 per **\@publication** e il valore usato per definire la partizione per **\@suser_sname** se si usa [SUSER_SNAME &#40;Transact-SQL&#41;](../../t-sql/functions/suser-sname-transact-sql.md) nella clausola di filtro oppure per **\@host_name** se si usa [HOST_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/host-name-transact-sql.md) nella clausola di filtro.  
  
9. Nel database di pubblicazione del server di pubblicazione eseguire [sp_adddynamicsnapshot_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddynamicsnapshot-job-transact-sql.md). Specificare il nome della pubblicazione ottenuto nel passaggio 1 per **\@publication**, il valore di **\@suser_sname** o **\@host_name** dal passaggio 8 e una pianificazione per il processo. In tal modo verrà creato il processo che genera lo snapshot con parametri per la partizione specificata. Per altre informazioni, vedere [Specify Synchronization Schedules](../../relational-databases/replication/specify-synchronization-schedules.md).  
  
    > [!NOTE]  
    >  Per l'esecuzione di questo processo viene utilizzato lo stesso account di Windows del processo snapshot iniziale definito al passaggio 2. Per rimuovere il processo snapshot con parametri e la partizione dati correlata, eseguire [sp_dropdynamicsnapshot_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdynamicsnapshot-job-transact-sql.md).  
  
10. Nel database di pubblicazione del server di pubblicazione eseguire [sp_helpmergepartition &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepartition-transact-sql.md), specificando il valore di **\@publication** ottenuto nel passaggio 1 e il valore di **\@suser_sname** o **\@host_name** dal passaggio 8. Si noti il valore di **snapshot job** nel set di risultati.  
  
11. Nel database **msdb** del server di distribuzione eseguire [sp_start_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md) specificando il valore ottenuto nel passaggio 9 per **\@job_id**. In tal modo verrà avviato il processo snapshot con parametri per la partizione.  
  
12. Ripetere i passaggi da 8 a 11 per generare uno snapshot partizionato per ogni sottoscrizione.  
  
#### <a name="to-create-a-publication-and-manually-create-snapshots-for-each-partition"></a>Per creare una pubblicazione e creare manualmente gli snapshot per ogni partizione  
  
1.  Eseguire [sp_addmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md) per creare la pubblicazione. Per altre informazioni, vedere [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md).  
  
2.  Nel server di pubblicazione eseguire [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md). Specificare il nome della pubblicazione usato nel passaggio 1 per **\@publication** e le credenziali di Windows usate per l'agente di snapshot per **\@job_login** e **\@password**. Se l'agente userà l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per la connessione al server di pubblicazione, è necessario specificare anche il valore **0** per **\@publisher_security_mode** e le informazioni di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per **\@publisher_login** e **\@publisher_password**. Verrà creato un processo dell'agente snapshot per la pubblicazione. Per ulteriori informazioni sulla generazione di uno snapshot iniziale e sulla definizione di una pianificazione personalizzata per l'agente snapshot, vedere [Create and Apply the Initial Snapshot](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
    > [!IMPORTANT]  
    >  Quando si configura un server di pubblicazione con un server di distribuzione remoto, i valori specificati per tutti i parametri, inclusi *job_login* e *job_password*, vengono inviati al server di distribuzione come testo normale. È consigliabile crittografare la connessione tra il server di pubblicazione e il server di distribuzione remoto prima di eseguire questa stored procedure. Per altre informazioni, vedere [Abilitare le connessioni crittografate al motore di database &#40;Gestione configurazione SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
3.  Per aggiungere articoli alla pubblicazione, eseguire [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Questa stored procedure deve essere eseguita una sola volta per ogni articolo della pubblicazione. Quando si usano filtri con parametri, è necessario specificare un filtro di riga con parametri per almeno un articolo usando il parametro **\@subset_filterclause**. Per altre informazioni, vedere [Definizione e modifica di un filtro di riga con parametri per un articolo di merge](../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md).  
  
4.  Se il filtro di riga con parametri verrà usato per filtrare altri articoli, eseguire [sp_addmergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md) per definire le relazioni di join o tra record logici tra gli articoli. Questa stored procedure deve essere eseguita una sola volta per ogni relazione da definire. Per altre informazioni, vedere [Definizione e modifica di un filtro di join tra articoli di merge](../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md).  
  
5.  Avviare il processo snapshot o eseguire Agente snapshot repliche dal prompt dei comandi per generare lo schema standard dello snapshot standard e gli altri file. Per altre informazioni, vedere [Creazione e applicazione dello snapshot iniziale](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
6.  Eseguire nuovamente Agente snapshot repliche dal prompt dei comandi per generare file della copia bulk (estensione bcp), specificando il percorso dello snapshot partizionato per **- DynamicSnapshotLocation** e una o entrambe le proprietà seguenti che definiscono la partizione:  
  
    -   **-DynamicFilterHostName** - valore se viene usato [HOST_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/host-name-transact-sql.md).  
  
    -   **-DynamicFilterLogin** - valore se viene usato [SUSER_SNAME &#40;Transact-SQL&#41;](../../t-sql/functions/suser-sname-transact-sql.md).  
  
7.  Ripetere il passaggio 6 per generare uno snapshot partizionato per ogni sottoscrizione.  
  
8.  Eseguire l'agente di merge per consentire a ogni sottoscrizione di applicare lo snapshot partizionato iniziale ai Sottoscrittori, specificando le proprietà seguenti:  
  
    -   **-Hostname** : valore utilizzato per definire la partizione se viene eseguito l'override del valore effettivo di HOST_NAME.  
  
    -   **-DynamicSnapshotLocation** : percorso dello snapshot dinamico per questa partizione.  
  
> [!NOTE]  
>  Per altre informazioni sulla programmazione degli agenti di replica, vedere [Concetti di base relativi ai file eseguibili dell'agente di replica](../../relational-databases/replication/concepts/replication-agent-executables-concepts.md).  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Esempi (Transact-SQL)  
 In questo esempio viene creata una pubblicazione di tipo merge con filtri con parametri in cui i Sottoscrittori avviano il processo di generazione dello snapshot. I valori per **\@job_login** e **\@job_password** vengono passati tramite variabili di scripting.  
  
 [!code-sql[HowTo#sp_MergeDynamicPub1](../../relational-databases/replication/codesnippet/tsql/create-a-snapshot-for-a-_1.sql)]  
  
 In questo esempio viene creata una pubblicazione utilizzando un filtro con parametri in cui la partizione di ogni Sottoscrittore viene definita eseguendo [sp_addmergepartition](../../relational-databases/system-stored-procedures/sp-addmergepartition-transact-sql.md) e il processo snapshot con filtro viene creato eseguendo [sp_adddynamicsnapshot_job](../../relational-databases/system-stored-procedures/sp-adddynamicsnapshot-job-transact-sql.md) passando le informazioni sul partizionamento. I valori per **\@job_login** e **\@job_password** vengono passati tramite variabili di scripting.  
  
 [!code-sql[HowTo#sp_MergeDynamicPubPlusPartition](../../relational-databases/replication/codesnippet/tsql/create-a-snapshot-for-a-_2.sql)]  
  
 In questo esempio viene creata una pubblicazione utilizzando un filtro con parametri in cui il Sottoscrittore deve disporre di una propria partizione dati e il processo snapshot con filtro viene creato fornendo le informazioni sul partizionamento. Un Sottoscrittore fornisce le informazioni sul partizionamento utilizzando i parametri della riga di comando durante l'esecuzione manuale degli agenti di replica. In questo esempio si presuppone che sia stata creata anche una sottoscrizione della pubblicazione.  
  
 [!code-sql[HowTo#sp_MergeDynamicPubPartitionManual](../../relational-databases/replication/codesnippet/tsql/create-a-snapshot-for-a-_3.sql)]  
  
```  
  
REM Line breaks are added to improve readability.   
REM In a batch file, commands must be made in a single line.  
REM Run the Snapshot agent from the command line to generate the standard snapshot   
REM schema and other files.   
SET DistPub=%computername%  
SET PubDB=AdventureWorks2012   
SET PubName=AdvWorksSalesPersonMerge  
  
"C:\Program Files\Microsoft SQL Server\120\COM\SNAPSHOT.EXE" -Publication %PubName%    
-Publisher %DistPub% -Distributor  %DistPub%  -PublisherDB %PubDB%  -ReplicationType 2    
-OutputVerboseLevel 1  -DistributorSecurityMode 1  
  
PAUSE  
  
```  
  
```  
  
REM Run the Snapshot agent from the command line, this time to generate   
REM the bulk copy (.bcp) data for each Subscriber partition.    
SET DistPub=%computername%  
SET PubDB=AdventureWorks2012   
SET PubName=AdvWorksSalesPersonMerge  
SET SnapshotDir=\\%DistPub%\repldata\unc\fernando  
  
MD %SnapshotDir%  
  
"C:\Program Files\Microsoft SQL Server\120\COM\SNAPSHOT.EXE" -Publication %PubName%    
-Publisher %DistPub%  -Distributor  %DistPub%  -PublisherDB %PubDB%  -ReplicationType 2    
-OutputVerboseLevel 1  -DistributorSecurityMode 1  -DynamicFilterHostName "adventure-works\Fernando"    
-DynamicSnapshotLocation %SnapshotDir%  
  
PAUSE  
  
```  
  
```  
  
REM Run the Merge Agent for each subscription to apply the partitioned   
REM snapshot for each Subscriber.    
SET Publisher = %computername%  
SET Subscriber = %computername%  
SET PubDB = AdventureWorks2012   
SET SubDB = AdventureWorks2012Replica   
SET PubName = AdvWorksSalesPersonMerge   
SET SnapshotDir=\\%DistPub%\repldata\unc\fernando  
  
"C:\Program Files\Microsoft SQL Server\120\COM\REPLMERG.EXE" -Publisher  %Publisher%    
-Subscriber  %Subscriber%  -Distributor %Publisher%  -PublisherDB %PubDB%    
-SubscriberDB %SubDB% -Publication %PubName%  -PublisherSecurityMode 1  -OutputVerboseLevel 3    
-Output -SubscriberSecurityMode 1  -SubscriptionType 3 -DistributorSecurityMode 1    
-Hostname "adventure-works\Fernando"  -DynamicSnapshotLocation %SnapshotDir%  
  
PAUSE  
  
```  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilizzo di RMO (Replication Management Objects)  
 È possibile utilizzare oggetti RMO (Replication Management Objects) per generare snapshot partizionati a livello di programmazione nelle modalità seguenti:  
  
-   Consentire ai Sottoscrittori di richiedere la generazione e l'applicazione dello snapshot alla prima sincronizzazione.  
  
-   Pregenerare uno snapshot per ogni partizione.  
  
-   Generare manualmente uno snapshot per ogni Sottoscrittore eseguendo l'agente snapshot.  
  
> [!NOTE]  
>  Quando il filtro di un articolo produce partizioni non sovrapposte univoche per ogni sottoscrizione (specificando un valore F:Microsoft.SqlServer.Replication.PartitionOptions.NonOverlappingSingleSubscription per P:Microsoft.SqlServer.Replication.MergeArticle.PartitionOption nel caso della creazione di un articolo di tipo merge), i metadati vengono puliti ogni volta che viene eseguito l'agente di merge. Lo snapshot partizionato scade quindi più rapidamente. Quando si utilizza questa opzione, è consigliabile consentire ai Sottoscrittori di richiedere la generazione degli snapshot. Per ulteriori informazioni, vedere la sezione relativa all'utilizzo delle opzioni di filtro appropriate nell'argomento [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
> [!IMPORTANT]  
>  Se possibile, richiedere agli utenti di immettere le credenziali di sicurezza in fase di esecuzione. Se è necessario archiviare le credenziali, utilizzare i [servizi di crittografia](/previous-versions/aa719848(v=vs.71)) offerti da [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows .NET Framework.  
  
#### <a name="to-create-a-publication-that-allows-subscribers-to-initiate-snapshot-generation-and-delivery"></a>Per creare una pubblicazione che consente ai Sottoscrittori di avviare la generazione e il recapito di snapshot  
  
1.  Creare una connessione al server di pubblicazione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.ReplicationDatabase> per il database di pubblicazione, impostare la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> sull'istanza di <xref:Microsoft.SqlServer.Management.Common.ServerConnection> del passaggio 1, quindi chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . If <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> restituisce **false**, verificare che il database esista.  
  
3.  If <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.EnabledMergePublishing%2A> è **false**, impostarla su **true** e chiamare <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A>.  
  
4.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.MergePublication> e impostare le proprietà seguenti per questo oggetto:  
  
    -   Oggetto <xref:Microsoft.SqlServer.Management.Common.ServerConnection> indicato nel passaggio 1 per <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
    -   Nome del database pubblicato per <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A>.  
  
    -   Nome per la pubblicazione per <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>.  
  
    -   Numero massimo di processi di snapshot dinamici da eseguire per <xref:Microsoft.SqlServer.Replication.MergePublication.MaxConcurrentDynamicSnapshots%2A>. Poiché le richieste di snapshot avviate dal Sottoscrittore possono verificarsi in qualsiasi momento, questa proprietà limita il numero di processi dell'agente snapshot che possono essere eseguiti simultaneamente quando più Sottoscrittori richiedono il rispettivo snapshot partizionato contemporaneamente. Quando è in esecuzione il numero massimo di processi, le richieste aggiuntive di snapshot partizionati vengono inserite nella coda fino al completamento di uno dei processi.  
  
    -   Utilizzare l'operatore logico OR bit per bit ( **|** in Visual C# e **Or** in Visual Basic) per aggiungere il valore <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowSubscriberInitiatedSnapshot> a <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A>.  
  
    -   Campi <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> e <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> di <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A> per fornire le credenziali per l'account di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows con cui verrà eseguito il processo dell'agente snapshot.  
  
        > [!NOTE]  
        >  È consigliabile impostare <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A> quando la pubblicazione viene creata da un membro del ruolo predefinito del server **sysadmin** . Per altre informazioni, vedere [Modello di sicurezza dell'agente di replica](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
5.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.Publication.Create%2A> per creare la pubblicazione.  
  
    > [!IMPORTANT]  
    >  Quando si configura un server di pubblicazione con un server di distribuzione remoto, i valori specificati per tutte le proprietà, inclusa <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A>, vengono inviati al server di distribuzione come testo normale. È consigliabile crittografare la connessione tra il server di pubblicazione e il server di distribuzione remoto prima di chiamare il metodo <xref:Microsoft.SqlServer.Replication.Publication.Create%2A>. Per altre informazioni, vedere [Abilitare le connessioni crittografate al motore di database &#40;Gestione configurazione SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
6.  Utilizzare la proprietà <xref:Microsoft.SqlServer.Replication.MergeArticle> per aggiungere articoli alla pubblicazione. Specificare la proprietà <xref:Microsoft.SqlServer.Replication.MergeArticle.FilterClause%2A> per almeno un articolo che definisce il filtro con parametri. (Facoltativo) Creare oggetti <xref:Microsoft.SqlServer.Replication.MergeJoinFilter> che definiscono i filtri join tra gli articoli. Per altre informazioni, vedere [definire un articolo](../../relational-databases/replication/publish/define-an-article.md).  
  
7.  Se il valore di <xref:Microsoft.SqlServer.Replication.Publication.SnapshotAgentExists%2A> è **false**, chiamare <xref:Microsoft.SqlServer.Replication.Publication.CreateSnapshotAgent%2A> per creare il processo dell'agente snapshot iniziale per questa pubblicazione.  
  
8.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.Publication.StartSnapshotGenerationAgentJob%2A> dell'oggetto <xref:Microsoft.SqlServer.Replication.MergePublication> creato nel passaggio 4. In questo modo viene avviato il processo dell'agente che genera lo snapshot iniziale. Per ulteriori informazioni sulla generazione di uno snapshot iniziale e sulla definizione di una pianificazione personalizzata per l'agente snapshot, vedere [Create and Apply the Initial Snapshot](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
9. (Facoltativo) Verificare la presenza di un valore **true** per la proprietà <xref:Microsoft.SqlServer.Replication.MergePublication.SnapshotAvailable%2A> per determinare quando lo snapshot iniziale è pronto per l'utilizzo.  
  
10. La prima volta che l'agente di merge per un Sottoscrittore si connette, verrà generato automaticamente uno snapshot partizionato.  
  
#### <a name="to-create-a-publication-and-pregenerate-or-automatically-refresh-snapshots"></a>Per creare una pubblicazione e pregenerare o aggiornare automaticamente gli snapshot  
  
1.  Utilizzare un'istanza della classe <xref:Microsoft.SqlServer.Replication.MergePublication> per definire una pubblicazione di tipo merge. Per altre informazioni, vedere [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md).  
  
2.  Utilizzare la proprietà <xref:Microsoft.SqlServer.Replication.MergeArticle> per aggiungere articoli alla pubblicazione. Specificare la proprietà <xref:Microsoft.SqlServer.Replication.MergeArticle.FilterClause%2A> per almeno un articolo che definisce il filtro con parametri, quindi creare eventuali oggetti <xref:Microsoft.SqlServer.Replication.MergeJoinFilter> che definiscono filtri join tra gli articoli. Per altre informazioni, vedere [definire un articolo](../../relational-databases/replication/publish/define-an-article.md).  
  
3.  Se il valore di <xref:Microsoft.SqlServer.Replication.Publication.SnapshotAgentExists%2A> è **false**, chiamare <xref:Microsoft.SqlServer.Replication.Publication.CreateSnapshotAgent%2A> per creare il processo dell'agente snapshot per questa pubblicazione.  
  
4.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.Publication.StartSnapshotGenerationAgentJob%2A> dell'oggetto <xref:Microsoft.SqlServer.Replication.MergePublication> creato nel passaggio 1. Questo metodo avvia il processo dell'agente che genera lo snapshot iniziale. Per ulteriori informazioni sulla generazione di uno snapshot iniziale e sulla definizione di una pianificazione personalizzata per l'agente snapshot, vedere [Creazione e applicazione dello snapshot iniziale](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
5.  (Facoltativo) Verificare la presenza di un valore **true** per la proprietà <xref:Microsoft.SqlServer.Replication.MergePublication.SnapshotAvailable%2A> per determinare quando lo snapshot iniziale è pronto per l'utilizzo.  
  
6.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.MergePartition> e impostare i criteri di filtro con parametri per il Sottoscrittore utilizzando una o entrambe le proprietà seguenti:  
  
    -   Se la partizione del Sottoscrittore è definita dal risultato di [SUSER_SNAME &#40;Transact-SQL&#41;](../../t-sql/functions/suser-sname-transact-sql.md), usare <xref:Microsoft.SqlServer.Replication.MergePartition.DynamicFilterLogin%2A>.  
  
    -   Se la partizione del Sottoscrittore è definita dal risultato di [HOST_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/host-name-transact-sql.md) o da un overload di questa funzione, usare <xref:Microsoft.SqlServer.Replication.MergePartition.DynamicFilterHostName%2A>.  
  
7.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.MergeDynamicSnapshotJob> e impostare la stessa proprietà indicata nel passaggio 6.  
  
8.  Utilizzare la classe <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule> per definire una pianificazione per la generazione dello snapshot filtrato per la partizione del Sottoscrittore.  
  
9. Utilizzando l'istanza di <xref:Microsoft.SqlServer.Replication.MergePublication> indicata nel passaggio 1, chiamare <xref:Microsoft.SqlServer.Replication.MergePublication.AddMergePartition%2A>. Passare l'oggetto <xref:Microsoft.SqlServer.Replication.MergePartition> indicato nel passaggio 6.  
  
10. Utilizzando l'istanza di <xref:Microsoft.SqlServer.Replication.MergePublication> indicata nel passaggio 1, chiamare il metodo <xref:Microsoft.SqlServer.Replication.MergePublication.AddMergeDynamicSnapshotJob%2A> . Passare l'oggetto <xref:Microsoft.SqlServer.Replication.MergeDynamicSnapshotJob> indicato nel passaggio 7 e l'oggetto <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule> indicato nel passaggio 8.  
  
11. Chiamare <xref:Microsoft.SqlServer.Replication.MergePublication.EnumMergeDynamicSnapshotJobs%2A>e individuare l'oggetto <xref:Microsoft.SqlServer.Replication.MergeDynamicSnapshotJob> per il processo di snapshot partizionato appena aggiunto nella matrice restituita.  
  
12. Ottenere la proprietà <xref:Microsoft.SqlServer.Replication.MergeDynamicSnapshotJob.Name%2A> per il processo.  
  
13. Creare una connessione al server di distribuzione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
14. Creare un'istanza della classe <xref:Microsoft.SqlServer.Management.Smo.Server> SMO (SQL Server Management Objects) passando l'oggetto <xref:Microsoft.SqlServer.Management.Common.ServerConnection> indicato nel passaggio 13.  
  
15. Creare un'istanza della classe <xref:Microsoft.SqlServer.Management.Smo.Agent.Job> passando la proprietà <xref:Microsoft.SqlServer.Management.Smo.Server.JobServer%2A> dell'oggetto <xref:Microsoft.SqlServer.Management.Smo.Server> indicato nel passaggio 14 e il nome del processo indicato nel passaggio 12.  
  
16. Chiamare il metodo <xref:Microsoft.SqlServer.Management.Smo.Agent.Job.Start%2A> per avviare il processo di snapshot partizionato.  
  
17. Ripetere i passaggi da 6 a 16 per ogni Sottoscrittore.  
  
#### <a name="to-create-a-publication-and-manually-create-snapshots-for-each-partition"></a>Per creare una pubblicazione e creare manualmente gli snapshot per ogni partizione  
  
1.  Utilizzare un'istanza della classe <xref:Microsoft.SqlServer.Replication.MergePublication> per definire una pubblicazione di tipo merge. Per altre informazioni, vedere [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md).  
  
2.  Utilizzare la proprietà <xref:Microsoft.SqlServer.Replication.MergeArticle> per aggiungere articoli alla pubblicazione. Specificare la proprietà <xref:Microsoft.SqlServer.Replication.MergeArticle.FilterClause%2A> per almeno un articolo che definisce il filtro con parametri, quindi creare eventuali oggetti <xref:Microsoft.SqlServer.Replication.MergeJoinFilter> che definiscono i filtri join tra gli articoli. Per altre informazioni, vedere [definire un articolo](../../relational-databases/replication/publish/define-an-article.md).  
  
3.  Generare lo snapshot iniziale. Per altre informazioni, vedere [Creazione e applicazione dello snapshot iniziale](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
4.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent> e impostare le seguenti proprietà obbligatorie:  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Publisher%2A> : nome del server di pubblicazione  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherDatabase%2A> : nome del database di pubblicazione  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Publication%2A> : nome della pubblicazione  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Distributor%2A> : nome del server di distribuzione  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherSecurityMode%2A> : valore di <xref:Microsoft.SqlServer.Replication.SecurityMode.Integrated> per usare l'autenticazione integrata di Windows o valore di <xref:Microsoft.SqlServer.Replication.SecurityMode.Standard> per usare l'autenticazione di SQL Server.  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DistributorSecurityMode%2A> : valore di <xref:Microsoft.SqlServer.Replication.SecurityMode.Integrated> per usare l'autenticazione integrata di Windows o valore di <xref:Microsoft.SqlServer.Replication.SecurityMode.Standard> per usare l'autenticazione di SQL Server.  
  
5.  Impostare un valore di <xref:Microsoft.SqlServer.Replication.ReplicationType.Merge> per <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.ReplicationType%2A>.  
  
6.  Impostare una o più delle seguenti proprietà per definire i parametri di partizionamento:  
  
    -   Se la partizione del Sottoscrittore è definita dal risultato di [SUSER_SNAME &#40;Transact-SQL&#41;](../../t-sql/functions/suser-sname-transact-sql.md), usare <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DynamicFilterLogin%2A>.  
  
    -   Se la partizione del Sottoscrittore è definita dal risultato di [HOST_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/host-name-transact-sql.md) o da un overload di questa funzione, usare <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DynamicFilterHostName%2A>.  
  
7.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.GenerateSnapshot%2A> .  
  
8.  Ripetere i passaggi da 4 a 7 per ogni Sottoscrittore.  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Esempi (RMO)  
 In questo esempio viene creata una pubblicazione di tipo merge che consente ai Sottoscrittori di richiedere la generazione di snapshot.  
  
 [!code-cs[HowTo#rmo_CreateMergePub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createmergepub)]  
  
 [!code-vb[HowTo#rmo_vb_CreateMergePub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createmergepub)]  
  
 In questo esempio vengono creati manualmente la partizione del Sottoscrittore e lo snapshot filtrato per una pubblicazione di tipo merge con filtri di riga con parametri.  
  
 [!code-cs[HowTo#rmo_CreateMergePartition](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createmergepartition)]  
  
 [!code-vb[HowTo#rmo_vb_CreateMergePartition](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createmergepartition)]  
  
 In questo esempio viene avviato manualmente l'agente snapshot per generare lo snapshot di dati filtrati per un Sottoscrittore in una pubblicazione di tipo merge con filtri di riga con parametri.  
  
 [!code-cs[HowTo#rmo_GenerateFilteredSnapshot](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_generatefilteredsnapshot)]  
  
 [!code-vb[HowTo#rmo_vb_GenerateFilteredSnapshot](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_generatefilteredsnapshot)]  
  
## <a name="see-also"></a>Vedere anche  
 [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)   
 [Replication System Stored Procedures Concepts](../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Procedure consigliate per la sicurezza della replica](../../relational-databases/replication/security/replication-security-best-practices.md)  
  
