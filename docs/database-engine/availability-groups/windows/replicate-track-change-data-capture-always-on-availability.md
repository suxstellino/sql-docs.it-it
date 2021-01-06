---
title: Replica, rilevamento modifiche, Change Data Capture e gruppi di disponibilità
description: Informazioni sull'interoperabilità della replica, il rilevamento delle modifiche e Change Data Capture quando vengono usati con i gruppi di disponibilità Always On di SQL Server.
ms.custom: seo-lt-2019
ms.date: 08/21/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- change tracking [SQL Server], AlwaysOn Availability Groups
- change data capture [SQL Server], AlwaysOn Availability Groups
- Availability Groups [SQL Server], interoperability
- replication [SQL Server], AlwaysOn Availability Groups
ms.assetid: e17a9ca9-dd96-4f84-a85d-60f590da96ad
author: cawrites
ms.author: chadam
ms.openlocfilehash: fcb67787d6f29272d2496dbfe76788d476ef8ea0
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641143"
---
# <a name="replication-change-tracking--change-data-capture---always-on-availability-groups"></a>Replica, rilevamento modifiche e Change Data Capture per i gruppi di disponibilità Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Le funzionalità di replica, di rilevamento delle modifiche (CT, Change Tracking) e Change Data Capture (CDC) sono supportate in [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] vengono fornite disponibilità elevata e funzionalità aggiuntive di recupero database.  
  
##  <a name="overview-of-replication-with-availability-groups"></a><a name="Overview"></a> Panoramica della replica con i gruppi di disponibilità  
  
###  <a name="publisher-redirection"></a><a name="PublisherRedirect"></a> Reindirizzamento del server di pubblicazione  
 Se un database pubblicato è compatibile con [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], il database di distribuzione tramite cui viene fornito agli agenti l'accesso al database di pubblicazione viene configurato con le voci redirected_publishers. Tramite queste voci la coppia server di pubblicazione/database configurata originariamente viene reindirizzata, usando un nome del listener del gruppo di disponibilità per connettersi al server e al database di pubblicazione. Il failover delle connessioni stabilite tramite il nome del listener del gruppo di disponibilità avrà esito negativo. Al riavvio dell'agente di replica dopo il failover, la connessione verrà reindirizzata automaticamente al nuovo database primario.  
  
 In un gruppo di disponibilità un database secondario non può essere un server di pubblicazione. La ripubblicazione è supportata solo quando la replica transazionale è combinata con [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)].  
  
 Se un database pubblicato è membro di un gruppo di disponibilità e il server di pubblicazione viene reindirizzato, è necessario reindirizzarlo a un nome del listener del gruppo di disponibilità associato al gruppo di disponibilità. Non è possibile reindirizzarlo a un nodo esplicito.  
  
> [!NOTE]  
>  Dopo il failover su una replica secondaria, tramite Monitoraggio replica non è possibile regolare il nome dell'istanza di pubblicazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e le informazioni sulla replica continueranno a essere visualizzate con il nome dell'istanza primaria originale di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Dopo il failover, non è possibile immettere un token di traccia tramite Monitoraggio replica, nondimeno un token di traccia immesso nel nuovo server di pubblicazione tramite [!INCLUDE[tsql](../../../includes/tsql-md.md)]è visibile in Monitoraggio replica.  
  
###  <a name="general-changes-to-replication-agents-to-support-availability-groups"></a><a name="Changes"></a> Modifiche generali apportate agli agenti di replica per supportare i gruppi di disponibilità  
 Tre agenti di replica sono stati modificati per supportare la funzionalità [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Gli agenti di lettura log, snapshot e di merge sono stati modificati in modo da eseguire query sul database di distribuzione per il server di pubblicazione reindirizzato e usare il nome del listener del gruppo di disponibilità restituito, se è stato dichiarato un server di pubblicazione reindirizzato, per connettersi al server di pubblicazione del database.  
  
 Per impostazione predefinita, quando gli agenti eseguono una query sul database di distribuzione per determinare se il server di pubblicazione originale è stato reindirizzato, l'appropriatezza del database di destinazione o del reindirizzamento corrente verrà verificata prima che l'host reindirizzato venga restituito all'agente. Questo è il comportamento consigliato. Tuttavia, se l'avvio dell'agente si verifica molto frequentemente, l'overhead associato alla stored procedure di convalida può essere ritenuto troppo costoso. Una nuova opzione della riga di comando, *BypassPublisherValidation*, è stata aggiunta agli agenti di lettura log, snapshot e di merge. Quando viene usata l'opzione, il server di pubblicazione reindirizzato viene restituito immediatamente all'agente e l'esecuzione della stored procedure di convalida viene ignorata.  
  
 Gli errori restituiti dalla stored procedure di convalida vengono registrati nei log della cronologia degli agenti. Gli errori con gravità maggiore o uguale a 16 causeranno l'interruzione degli agenti. Gli agenti includono ora alcune funzionalità per effettuare nuovi tentativi in modo da gestire la disconnessione prevista da un database pubblicato in caso di failover su un nuovo database primario.  
  
#### <a name="log-reader-agent-modifications"></a>Modifiche all'agente di lettura log  
 L'agente di lettura log include le modifiche riportate di seguito.  
  
-   **Coerenza dei database replicati**  
  
     Quando un database pubblicato è membro di un gruppo di disponibilità, per impostazione predefinita tramite la lettura log non vengono elaborati record di log che non sono già stati finalizzati in tutte le repliche secondarie del gruppo di disponibilità. In questo modo, viene garantito che al failover tutte le righe replicate a un sottoscrittore siano anche presenti nel nuovo database primario.  
  
     Se nel server di pubblicazione sono disponibili solo due repliche di disponibilità, una primaria e una secondaria, e si verifica un failover, la replica primaria originale rimane inattiva perché l'agente di lettura log non procede finché tutti i database secondari non saranno riportati online o finché le repliche secondarie in cui si è verificato l'errore non verranno rimosse dal gruppo di disponibilità. L'agente di lettura log, che attualmente viene eseguito nel database secondario, non procederà poiché tramite AlwaysOn non è possibile finalizzare le eventuali modifiche in nessun database secondario. Per consentire all'agente di lettura log di procedere e mantenere la funzionalità di ripristino di emergenza, rimuovere la replica primaria originale dal gruppo di disponibilità usando ALTER AVAILABITY GROUP <nome_gruppo> REMOVE REPLICA. Successivamente, aggiungere una nuova replica secondaria al gruppo di disponibilità.  
  
-   **Flag di traccia 1448**  
  
     Con il flag di traccia 1448 è possibile far procedere l'agente lettura log repliche anche se il ricevimento di una modifica non è stato riconosciuto dalle repliche secondarie asincrone. Anche con questo flag di traccia abilitato, tramite l'agente di lettura log vengono sempre attese le repliche secondarie sincrone. L'agente di lettura log non procederà oltre il riconoscimento minimo delle repliche secondarie sincrone. Questo flag di traccia si applica all'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], non solo a un gruppo di disponibilità, a un database di disponibilità o a un'istanza di lettura log. Questo flag di traccia diventa immediatamente effettivo senza un riavvio. Può essere attivato anticipatamente o quando si verifica un errore in una replica secondaria asincrona.  
  
###  <a name="stored-procedures-supporting-availability-groups"></a><a name="StoredProcs"></a> Stored procedure che supportano i gruppi di disponibilità  
  
-   **sp_redirect_publisher**  
  
     La stored procedure **sp_redirect_publisher** viene usata per specificare un server di pubblicazione reindirizzato per una coppia server di pubblicazione/database esistente. Se il server di pubblicazione appartiene a un gruppo di disponibilità, il server di pubblicazione reindirizzato è il nome del listener del gruppo di disponibilità.  
  
-   **sp_get_redirected_publisher**  
  
     La stored procedure **sp_get_redirected_publisher** viene usata dagli agenti di replica per eseguire una query su un database di distribuzione allo scopo di determinare se per una coppia server di pubblicazione/database è definito un server di pubblicazione reindirizzato. Questa stored procedure ha due scopi. Innanzitutto, consente all'agente di determinare se il server di pubblicazione originale è stato reindirizzato. In secondo luogo, può anche avviare l'esecuzione di una stored procedure di convalida nel database di distribuzione (**sp_validate_redirected_publisher**) per verificare l'appropriatezza del nodo di destinazione del reindirizzamento a fungere da server di pubblicazione per il database denominato.  
  
     Per eseguire questa stored procedure è necessario che il chiamante sia membro del ruolo del server **sysadmin** , del ruolo del database **db_owner** per il database di distribuzione o di un **elenco di accesso alla pubblicazione** per una pubblicazione definita associata al server di pubblicazione.  
  
-   **sp_validate_redirected_publisher**  
  
     Questa stored procedure tenta di convalidare la capacità del server di pubblicazione corrente di ospitare il database pubblicato. Può essere chiamata in qualsiasi momento per verificare che l'host corrente per il database pubblicato sia in grado di supportare la replica.  
  
-   **sp_validate_replicate_hosts_as_publishers**  
  
     Benché sia utile per gli agenti garantire che il server primario corrente funzioni come server di pubblicazione di replica per un server di pubblicazione, una funzionalità di convalida più generale è necessaria per stabilire la validità di un'intera topologia di replica in un database di disponibilità AlwaysOn. La stored procedure **sp_validate_replica_hosts_as_publishers** è progettata per tale esigenza.  
  
     Questa stored procedure viene sempre eseguita manualmente. Il chiamante deve avere il ruolo di **sysadmin** nel database di distribuzione, **dbowner** nel database di distribuzione o deve essere membro dell' **elenco di accesso alla pubblicazione** di una pubblicazione del server di pubblicazione. Inoltre, l'accesso del chiamante deve essere valido per tutti gli host di replica di disponibilità e devono essere disponibili privilegi selezionati nel database di disponibilità associato al server di pubblicazione.  
  
###  <a name="change-data-capture"></a><a name="CDC"></a> Change Data Capture  
 Nei database abilitati per Change Data Capture (CDC) è possibile sfruttare [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] non solo per assicurare che il database rimanga disponibile in caso di errore, ma anche per fare in modo che le modifiche apportate alle tabelle del database continuino a essere monitorate e depositate nelle tabelle delle modifiche CDC. L'ordine in cui CDC e [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] sono configurati non è importante. I database abilitati per CDC possono essere aggiunti a [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]e i database che sono membri di un gruppo di disponibilità AlwaysOn possono essere abilitati per CDC. In entrambi i casi, tuttavia, la configurazione di CDC viene sempre eseguita nella replica corrente o progettata. In CDC viene usato l'agente di lettura log e sono presenti le stesse limitazioni descritte nella sezione **Modifiche all'agente di lettura log** riportata in precedenza in questo argomento.  
  
-   **Raccolta di modifiche per Change Data Capture senza replica**  
  
     Se CDC è abilitato per un database, ma la replica non lo è, il processo di acquisizione usato per raccogliere le modifiche dal log e depositarle nelle tabelle delle modifiche CDC viene eseguito nell'host CDC come processo di SQL Agent.  
  
     Per riprendere la raccolta delle modifiche dopo il failover, è necessario eseguire la stored procedure **sp_cdc_add_job** nel nuovo database primario per creare il processo di acquisizione locale.  
  
     Nell'esempio seguente viene creato il processo di acquisizione.  
  
    ```sql  
    EXEC sys.sp_cdc_add_job @job_type = 'capture';  
    ```  
  
-   **Raccolta di modifiche per Change Data Capture con replica**  
  
     Se sia CDC sia la replica sono abilitati per un database, tramite la lettura log viene gestito il popolamento delle tabelle delle modifiche CDC. In questo caso, le tecniche usate dalla replica per sfruttare [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] garantiranno che le modifiche continueranno a essere raccolte dal log e depositate nelle tabelle delle modifiche CDC dopo il failover. Non è necessario aggiungere alcuna azione in questa configurazione di CDC per assicurare che le tabelle delle modifiche vengano popolate.  
  
-   **Pulizia di Change Data Capture**  
  
     Per assicurare l'esecuzione della pulizia appropriata nel nuovo database primario, è necessario creare sempre un processo di pulizia locale. Nell'esempio seguente viene creato il processo di pulizia.  
  
    ```sql  
    EXEC sys.sp_cdc_add_job @job_type = 'cleanup';  
    ```  
  
    > [!NOTE]  
    >  È consigliabile creare i processi per tutte le possibili destinazioni di failover prima del failover e contrassegnarli come disabilitati finché la replica di disponibilità in un host non diventa la nuova replica primaria. È inoltre necessario disabilitare i processi CDC in esecuzione nel database primario precedente quando il database locale diventa un database secondario. Per disabilitare e abilitare i processi, usare l'opzione *\@enabled* di [sp_update_job &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md). Per altre informazioni sulla creazione di processi CDC, vedere [sys.sp_cdc_add_job &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sys-sp-cdc-add-job-transact-sql.md).  
  
-   **Aggiunta di ruoli CDC a una replica di database primario AlwaysOn**  
  
     Quando una tabella è abilitata per CDC, è possibile associare un ruolo di database all'istanza di acquisizione. Se viene specificato un ruolo, l'utente che desidera usare le funzioni con valori di tabella di CDC per accedere alle modifiche della tabella deve non solo disporre dell'accesso SELECT alle colonne della tabella con rilevamento, ma essere anche un membro del ruolo denominato. Se il ruolo specificato non esiste già, verrà creato. Quando i ruoli del database vengono aggiunti automaticamente a un database primario AlwaysOn, vengono propagati anche nei database secondari del gruppo di disponibilità.  
  
-   **Applicazioni client che accedono a CDC e AlwaysOn**  
  
     Le applicazioni client che usano le funzioni con valori di tabella o server collegati per accedere ai dati delle tabelle delle modifiche devono poter essere in grado di individuare un host CDC appropriato dopo il failover. Il nome del listener del gruppo di disponibilità è il meccanismo fornito da [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] per consentire in modo trasparente il reindirizzamento di una connessione a un host differente. Una volta associato a un gruppo di disponibilità, un nome del listener del gruppo di disponibilità è disponibile per essere usato nelle stringhe di connessione TCP. Due diversi scenari di connessione sono supportati tramite il nome del listener del gruppo di disponibilità.  
  
    -   In uno si assicura che le richieste di connessione vengano sempre indirizzate alla replica primaria corrente.  
  
    -   Nell'altro si garantisce che le richieste di connessione vengano indirizzate a una replica secondaria di sola lettura.  
  
     Se usato per individuare una replica secondaria di sola lettura, è necessario anche definire un elenco di routing di sola lettura per il gruppo di disponibilità. Per altre informazioni sull'accesso in routing ai database secondari leggibili, vedere [Per configurare repliche di disponibilità per il routing di sola lettura](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md).  
  
    > [!NOTE]  
    >  Nella creazione di un nome del listener del gruppo di disponibilità e nell'utilizzo dello stesso nelle applicazioni client per accedere alla replica di database del gruppo di disponibilità si verifica un ritardo di propagazione.  
  
     Usare la query seguente per determinare se un nome del listener del gruppo di disponibilità è stato definito per il gruppo di disponibilità che ospita un database CDC. La query restituirà il nome del listener del gruppo di disponibilità se ne è stato creato uno.  
  
    ```sql  
    SELECT dns_name   
    FROM sys.availability_group_listeners AS l  
    INNER JOIN sys.availability_databases_cluster AS d  
        ON l.group_id = d.group_id  
    WHERE d.database_name = N'MyCDCDB';  
    ```  
  
-   **Reindirizzamento del carico delle query a una replica secondaria leggibile**  
  
     Anche se in molti casi tramite un'applicazione client verrà sempre effettuato il tentativo di connessione alla replica primaria corrente, questo non è il solo modo per sfruttare [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Se un gruppo di disponibilità viene configurato per supportare repliche secondarie leggibili, è possibile raggruppare i dati delle modifiche anche dai nodi secondari.  
  
     Quando viene configurato un gruppo di disponibilità, l'attributo ALLOW_CONNECTIONS associato a SECONDARY_ROLE viene usato per specificare il tipo di accesso secondario supportato. Se viene configurato come ALL, tutte le connessioni al database secondario saranno consentite, ma solo quelle per cui viene richiesto l'accesso in sola lettura avranno esito positivo. Se viene configurato come READ_ONLY, è necessario specificare la finalità di sola lettura mentre si stabilisce la connessione al database secondario affinché la connessione venga stabilita. Per altre informazioni, vedere [Configurare l'accesso in sola lettura in una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md).  
  
     La query seguente può essere usata per determinare se la finalità di sola lettura è necessaria per connettersi a una replica secondaria leggibile.  
  
    ```sql  
    SELECT g.name AS AG, replica_server_name, secondary_role_allow_connections_desc  
    FROM sys.availability_replicas AS r  
    JOIN sys.availability_groups AS g  
        ON r.group_id = g.group_id  
    WHERE g.name = N'MY_AG_NAME';  
    ```  
  
     Per individuare la replica secondaria è possibile usare il nome del listener del gruppo di disponibilità o il nome del nodo esplicito. Se viene usato il nome del listener del gruppo di disponibilità, l'accesso verrà indirizzato a qualsiasi replica secondaria adatta.  
  
     Quando **sp_addlinkedserver** viene usato per creare un server collegato per accedere al database secondario, il parametro *\@datasrc* viene usato per il nome del listener del gruppo di disponibilità o il nome del server esplicito e il parametro *\@provstr* viene usato per specificare la finalità di sola lettura.  
  
    ```sql  
    EXEC sp_addlinkedserver   
    @server = N'linked_svr',   
    @srvproduct=N'SqlServer',  
    @provider=N'SQLNCLI11',   
    @datasrc=N'AG_Listener_Name',   
    @provstr=N'ApplicationIntent=ReadOnly',   
    @catalog=N'MY_DB_NAME';  
    ```  
  
-   **Accesso client ai dati delle modifiche CDC e account di accesso al dominio**  
  
     In generale, è consigliabile usare account di accesso al dominio per l'accesso client ai dati delle modifiche nei database che sono membri di gruppi di disponibilità AlwaysOn. Per assicurare accesso continuato ai dati delle modifiche dopo il failover, l'utente del dominio avrà bisogno di privilegi di accesso su tutti gli host che supportano repliche del gruppo di disponibilità. Se un utente di database viene aggiunto a un database in una replica primaria e l'utente è associato a un account di accesso al dominio, tale utente viene propagato nei database secondari e continua a essere associato all'account di accesso al dominio specificato. Se il nuovo utente del database è associato a un account di accesso con autenticazione SQL Server, l'utente nei database secondari sarà propagato senza un account di accesso. Mentre l'accesso con autenticazione [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] associato potrebbe essere usato per accedere ai dati delle modifiche nel database primario dove l'utente del database è stato definito originariamente, il nodo è il solo in cui sarebbe possibile l'accesso. L'accesso con autenticazione [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] non consentirebbe di accedere ai dati da un database secondario, né da un nuovo database primario diverso dal database originale dove è stato definito l'utente del database.  
     
-   **Disabilitazione di Change Data Capture**  
Se Change Data Capture deve essere disabilitato in un database che fa parte di un gruppo di disponibilità AlwaysOn, sarà necessario eseguire alcuni passaggi aggiuntivi per assicurarsi che il troncamento del log non sia interessato. È necessario implementare uno dei passaggi seguenti per impedire a Change Data Capture di bloccare il troncamento del log dopo la disabilitazione di Change Data Capture:
    - Riavviare il servizio SQL Server in ogni istanza di replica secondaria
    - Rimuovere il database da tutte le istanze di replica secondaria del gruppo di disponibilità e aggiungerlo alle istanze di replica del gruppo di disponibilità con seeding automatico o manuale
  
###  <a name="change-tracking"></a><a name="CT"></a> Rilevamento delle modifiche  
 Un database abilitato per il rilevamento delle modifiche (CT) può far parte di un gruppo di disponibilità AlwaysOn. Non è necessaria alcuna configurazione aggiuntiva. Le applicazioni client di rilevamento delle modifiche in cui vengono usate le funzioni con valori di tabella CDC per accedere ai dati delle modifiche devono poter essere in grado di individuare la replica primaria dopo il failover. Se l'applicazione client viene connessa tramite il nome del listener del gruppo di disponibilità, le richieste di connessione verranno sempre indirizzate in modo appropriato alla replica primaria corrente.  
  
> [!NOTE]  
>  I dati del rilevamento modifiche devono sempre essere ottenuti dalla replica primaria. Un tentativo di accedere ai dati delle modifiche da una replica secondaria comporterà l'errore seguente:  
>   
>  Messaggio 22117, livello 16, stato 1, riga 1  
>   
>  Per i database membri di una replica secondaria (cioè per i database secondari), il rilevamento delle modifiche non è supportato. Eseguire le query di rilevamento modifiche sui database nella replica primaria.  
  
##  <a name="prerequisites-restrictions-and-considerations-for-using-replication"></a><a name="Prereqs"></a> Prerequisiti, restrizioni e considerazioni per l'utilizzo della replica  
 In questa sezione vengono descritte le considerazioni per la distribuzione della replica con la funzionalità [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], inclusi prerequisiti, restrizioni e suggerimenti.  
  
### <a name="prerequisites"></a>Prerequisiti  
  
-   Quando si usano la replica transazionale e il database di pubblicazione si trova in un gruppo di disponibilità, sia nel server di pubblicazione che nel database di distribuzione deve essere in esecuzione almeno [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]. Il sottoscrittore può invece usare un livello inferiore di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
-   Quando si usano la replica di tipo merge e il database di pubblicazione è in un gruppo di disponibilità:  
  
    -   Sottoscrizione push: sia nel server di pubblicazione che nel database di distribuzione deve essere in esecuzione almeno [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)].  
  
    -   Sottoscrizione pull: nel server di pubblicazione e nei database di distribuzione e sottoscrittore deve essere in esecuzione almeno [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]. Ciò è dovuto al fatto che l'agente di merge nel sottoscrittore deve comprendere il modo in cui in un gruppo di disponibilità può essere eseguito il failover sul secondario.  
  
-   Le istanze del server di pubblicazione soddisfano tutti i prerequisiti richiesti per fare parte di un gruppo di disponibilità AlwaysOn. Per altre informazioni, vedere [Prerequisiti, restrizioni e consigli per i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
### <a name="restrictions"></a>Restrizioni  
 Combinazioni supportate di replica in [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]:  
  
|Replica|Editore|Database di distribuzione|Sottoscrittore|  
|-|-|-|-|  
|**Transazionale**|Sì<br /><br /> Nota: non è incluso il supporto per la replica transazionale bidirezionale e reciproca.|Sì|sì| 
|**P2P**|No|No|No|  
|**Merge**|sì|No|No|  
|**Snapshot**|Sì|No|Sì|
  
 **Non è supportato l'uso del database di distribuzione con il mirroring del database.  
  
### <a name="considerations"></a>Considerazioni  
  
-   Il database di distribuzione non è supportato per l'uso con il mirroring del database, ma è supportato con [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] con determinate limitazioni. Vedere [Configurare il gruppo di disponibilità di distribuzione](../../../relational-databases/replication/configure-distribution-availability-group.md#limitations-or-exclusions). La configurazione della replica è associata all'istanza di SQL Server in cui è configurato il database di distribuzione; pertanto non è possibile eseguire il mirroring o la replica del database di distribuzione. Per fornire la disponibilità elevata per il database di distribuzione, è anche possibile usare un cluster di failover di SQL Server. Per altre informazioni, vedere [Istanze del cluster di failover Always On &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md).  
  
-   Il failover del Sottoscrittore in un database secondario, se supportato, è una procedura manuale per Sottoscrittori della replica di tipo merge. La procedura è essenzialmente identica al metodo usato per il failover a un database sottoscrittore con mirroring. I Sottoscrittori della replica transazionale non necessitano di una gestione speciale durante la partecipazione a [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. È necessario che nei Sottoscrittori sia in esecuzione [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] o versione successiva perché possano fare parte di un gruppo di disponibilità.  Per altre informazioni, vedere [Sottoscrittori della replica e gruppi di disponibilità AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/replication-subscribers-and-always-on-availability-groups-sql-server.md).
  
-   I metadati e gli oggetti esterni al database, ad esempio account di accesso, processi e server collegati, non vengono propagati alle repliche secondarie. Se i metadati e gli oggetti sono necessari nel nuovo database primario dopo il failover, è necessario copiarli manualmente. Per altre informazioni, vedere [Gestione di account di accesso e processi per i database di un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/logins-and-jobs-for-availability-group-databases.md).  

### <a name="distributed-availability-groups"></a>Gruppi di disponibilità distribuiti

Il server di pubblicazione o il database di distribuzione in un gruppo di disponibilità non può essere configurato come parte di un gruppo di disponibilità distribuito. Il database del server di pubblicazione in un gruppo di disponibilità e il database di distribuzione in un gruppo di disponibilità richiedono entrambi un endpoint listener per la configurazione e l'utilizzo appropriati. Tuttavia, non è possibile configurare un endpoint listener per un gruppo di disponibilità distribuito.
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
 **Replica**  
  
-   [Configurare la replica per i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-replication-for-always-on-availability-groups-sql-server.md)  
  
-   [Gestione di un database di pubblicazione AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/maintaining-an-always-on-publication-database-sql-server.md)  
  
-   [Domande frequenti sull'amministrazione della replica](../../../relational-databases/replication/administration/frequently-asked-questions-for-replication-administrators.md)  
  
 **Change data capture**  
  
-   [Abilitare e disabilitare Change Data Capture &#40;SQL Server&#41;](../../../relational-databases/track-changes/enable-and-disable-change-data-capture-sql-server.md)  
  
-   [Amministrare e monitorare Change Data Capture &#40;SQL Server&#41;](../../../relational-databases/track-changes/administer-and-monitor-change-data-capture-sql-server.md)  
  
-   [Usare i dati delle modifiche &#40;SQL Server&#41;](../../../relational-databases/track-changes/work-with-change-data-sql-server.md)  
  
 **Change tracking**  
  
-   [Abilitare e disabilitare il rilevamento delle modifiche &#40;SQL Server&#41;](../../../relational-databases/track-changes/enable-and-disable-change-tracking-sql-server.md)  
  
-   [Gestire il rilevamento delle modifiche &#40;SQL Server&#41;](../../../relational-databases/track-changes/manage-change-tracking-sql-server.md)  
  
-   [Utilizzare il rilevamento delle modifiche &#40;SQL Server&#41;](../../../relational-databases/track-changes/work-with-change-tracking-sql-server.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Sottoscrittori della replica e gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replication-subscribers-and-always-on-availability-groups-sql-server.md)   
 [Prerequisiti, restrizioni e raccomandazioni per i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Gruppi di disponibilità Always On: Interoperabilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)   
 [Istanze del cluster di failover AlwaysOn &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)   
 [Informazioni su Change Data Capture &#40;SQL Server&#41;](../../../relational-databases/track-changes/about-change-data-capture-sql-server.md)   
 [Informazioni sul rilevamento delle modifiche &#40;SQL Server&#41;](../../../relational-databases/track-changes/about-change-tracking-sql-server.md)   
 [Replica di SQL Server](../../../relational-databases/replication/sql-server-replication.md)   
 [Tenere traccia delle modifiche ai dati &#40;SQL Server&#41;](../../../relational-databases/track-changes/track-data-changes-sql-server.md)   
 [sys.sp_cdc_add_job &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sys-sp-cdc-add-job-transact-sql.md)  
  
  
