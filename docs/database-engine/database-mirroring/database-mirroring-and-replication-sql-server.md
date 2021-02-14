---
title: Mirroring e replica del database (SQL Server) | Microsoft Docs
description: Informazioni su come usare il mirroring del database in combinazione con la replica per migliorare la disponibilità del database di pubblicazione in SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], interoperability
- replication [SQL Server], database mirroring and
ms.assetid: 82796217-02e2-4bc5-9ab5-218bae11a2d6
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 16746834acfe8a53511166a35667f136b561c28a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353181"
---
# <a name="database-mirroring-and-replication-sql-server"></a>Mirroring e replica del database (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Per migliorare la disponibilità del database di pubblicazione è possibile utilizzare il mirroring del database in combinazione con la replica. Il mirroring del database coinvolge due copie di un unico database che in genere risiedono in computer diversi. In un momento dato solo una copia del database risulta disponibile per i client. Questa copia viene definita database principale. Gli aggiornamenti apportati dai client al database principale vengono applicati all'altra copia del database, definita database mirror. Il processo di mirroring prevede l'applicazione nel database mirror del log delle transazioni relativo a ogni operazione di inserimento, aggiornamento o eliminazione eseguita sul database principale.  
  
 Il failover della replica su un server mirror è supportato completamente per i database di pubblicazione e parzialmente per i database di sottoscrizione. Non è supportato l'utilizzo del database di distribuzione con il mirroring del database. Per informazioni sul recupero di un database di distribuzione o di un database di sottoscrizione senza che sia necessario riconfigurare la replica, vedere [Backup e ripristino di database replicati](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md).   
  
> [!NOTE]  
>  Dopo un failover, il server mirror diventa il server principale. In questo argomento "principale" e "mirror" si riferiscono sempre al server principale originale e al server mirror.  
  
## <a name="requirements-and-considerations-for-using-replication-with-database-mirroring"></a>Requisiti e considerazioni per l'utilizzo della replica con il mirroring del database  
 Prendere in considerazione i requisiti e le considerazioni seguenti quando si utilizza la replica con il mirroring del database:  
  
-   I server principale e mirror devono condividere un server di distribuzione. È consigliabile che quest'ultimo sia un server di distribuzione remoto, in quanto garantisce una maggiore tolleranza di errore nel caso si verifichi un failover non pianificato sul server di pubblicazione.  
  
-   Il mirroring del database di pubblicazione è supportato nella replica di tipo merge e nella replica transazionale con Sottoscrittori di sola lettura o Sottoscrittori ad aggiornamento in coda. Non sono supportati Sottoscrittori ad aggiornamento immediato, server di pubblicazione Oracle, server di pubblicazione in una topologia peer-to-peer e ripubblicazione.  
  
-   I metadati e gli oggetti esterni al database, ad esempio account di accesso, processi, server collegati e così via, non vengono copiati nel database mirror. Se i metadati e gli oggetti sono necessari nel database mirror, è necessario copiarli manualmente. Per altre informazioni, vedere [Gestione di account di accesso e di processi dopo un cambio di ruolo &#40;SQL Server&#41;](../../sql-server/failover-clusters/management-of-logins-and-jobs-after-role-switching-sql-server.md).  
  
## <a name="configuring-replication-with-database-mirroring"></a>Configurazione della replica con il mirroring del database  
 Per configurare la replica e il mirroring del database è necessario eseguire cinque operazioni. Ogni operazione è descritta in dettaglio nella sezione seguente.  
  
1.  Configurare il server di pubblicazione.  
  
2.  Configurare il mirroring del database.  
  
3.  Configurare il database mirror in modo che utilizzi lo stesso server di distribuzione del database principale.  
  
4.  Configurare gli agenti di replica per il failover.  
  
5.  Aggiungere i database principale e mirror a Monitoraggio replica.  
  
 Le operazioni ai passaggi 1 e 2 possono anche essere eseguite in ordine inverso.  
  
#### <a name="to-configure-database-mirroring-for-a-publication-database"></a>Per configurare il mirroring di un database di pubblicazione  
  
1.  Configurare il server di pubblicazione:  
  
    1.  È consigliabile utilizzare un server di distribuzione remoto. Per altre informazioni sulla configurazione della distribuzione, vedere [Configurare la distribuzione](../../relational-databases/replication/configure-distribution.md).  
  
    2.  È possibile abilitare un database per pubblicazioni snapshot e transazionali e/o pubblicazioni di tipo merge. Nel caso di database con mirroring che includono più tipi di pubblicazione, è necessario abilitare il database per entrambi i tipi in corrispondenza dello stesso nodo usando [sp_replicationdboption](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md). È ad esempio possibile eseguire le chiamate di stored procedure seguenti nel server principale:  
  
        ```  
        exec sp_replicationdboption @dbname='<PublicationDatabase>', @optname='publish', @value=true;  
        exec sp_replicationdboption @dbname='<PublicationDatabase>', @optname='mergepublish', @value=true;  
        ```  
  
         Per altre informazioni sulla creazione di pubblicazioni, vedere [Pubblicare dati e oggetti di database](../../relational-databases/replication/publish/publish-data-and-database-objects.md).  
  
2.  Configurare il mirroring del database. Per altre informazioni, vedere [Stabilire una sessione di mirroring del database tramite autenticazione di Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md) e [Impostazione del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md).  
  
3.  Configurare la distribuzione per il server mirror. Assegnare al database mirror lo stesso nome del server di pubblicazione e specificare lo stesso server di distribuzione e la stessa cartella snapshot utilizzati dal database principale. Se ad esempio si configura la replica con stored procedure, eseguire [sp_adddistpublisher](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md) nel server di distribuzione e quindi [sp_adddistributor](../../relational-databases/system-stored-procedures/sp-adddistributor-transact-sql.md) nel mirror. Per **sp_adddistpublisher**:  
  
    -   Impostare il valore del parametro **\@publisher** sul nome di rete del server mirror.  
  
    -   Impostare il valore del parametro **\@working_directory** sulla cartella snapshot usata dal server principale.  
  
4.  Specificare il nome del database mirror per il parametro dell'agente **-PublisherFailoverPartner**. Questo parametro è necessario per l'identificazione del database mirror dopo il failover da parte degli agenti seguenti:  
  
    -   Agente snapshot (per tutte le pubblicazioni)  
  
    -   Agente di lettura log (per tutte le pubblicazioni transazionali)  
  
    -   Agente di lettura coda (per le pubblicazioni transazionali che supportano le sottoscrizioni ad aggiornamento in coda)  
  
    -   Agente di merge (per le sottoscrizioni di tipo merge)  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Listener per la replica di (replisapi.dll: per le sottoscrizioni di tipo merge sincronizzate usando sincronizzazione Web)  
  
    -   Controllo ActiveX merge SQL (per le sottoscrizioni di tipo merge sincronizzate tramite il controllo)  
  
     Questo parametro non è necessario per l'agente di distribuzione e il controllo ActiveX distribuzione SQL in quanto questi ultimi non si connettono al server di pubblicazione.  
  
     Le modifiche apportate al parametro dell'agente verranno applicate al successivo avvio dell'agente. Se l'agente viene eseguito in modo continuo, è necessario arrestarlo e riavviarlo. I parametri possono essere specificati nei profili agenti e al prompt dei comandi. Per altre informazioni, vedere:  
  
    -   [Visualizzare e modificare i parametri del prompt dei comandi dell'agente di replica &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/agents/view-and-modify-replication-agent-command-prompt-parameters.md)  
  
    -   [Replication Agent Executables Concepts](../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)  
  
     È consigliabile aggiungere il parametro **-PublisherFailoverPartner** a un profilo agente e quindi specificare il nome del database mirror nel profilo. Se ad esempio si sta configurando la replica con stored procedure:  
  
    ```  
    -- Execute sp_help_agent_profile in the context of the distribution database to get the list of profiles.  
    -- Select the profile id of the profile that needs to be updated from the result set.  
    -- In the agent_type column returned by sp_help_agent_profile:   
    -- 1 = Snapshot Agent; 2 = Log Reader Agent; 3 = Distribution Agent; 4 = Merge Agent; 9 = Queue Reader Agent.  
  
    exec sp_help_agent_profile;  
  
    -- Setting the -PublisherFailoverPartner parameter in the default Snapshot Agent profile (profile 1).  
    -- Execute sp_add_agent_parameter in the context of the distribution database.  
    exec sp_add_agent_parameter @profile_id = 1, @parameter_name = N'-PublisherFailoverPartner', @parameter_value = N'<Failover Partner Name>';  
  
    -- Setting the -PublisherFailoverPartner parameter in the default Merge Agent profile (profile 6).  
    -- Execute sp_add_agent_parameter in the context of the distribution database.  
    exec sp_add_agent_parameter @profile_id = 6, @parameter_name = N'-PublisherFailoverPartner', @parameter_value = N'<Failover Partner Name>';  
    ```  
  
5.  Aggiungere i database principale e mirror a Monitoraggio replica. Per altre informazioni, vedere [Aggiungere e rimuovere server di pubblicazione da Monitoraggio replica](../../relational-databases/replication/monitor/add-and-remove-publishers-from-replication-monitor.md).  
  
## <a name="maintaining-a-mirrored-publication-database"></a>Gestione di un database di pubblicazione con mirroring  
 La gestione di un database di pubblicazione con mirroring è sostanzialmente analoga a quella di un database senza mirroring, con le considerazioni seguenti:  
  
-   L'amministrazione e il monitoraggio devono essere eseguiti sul server attivo. In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]le pubblicazioni sono incluse nella cartella **Pubblicazioni locali** solo per il server attivo. Se si esegue ad esempio il failover sul database mirror, le pubblicazioni vengono visualizzate nel database mirror e non più nel database principale. Se viene eseguito il failover del database sul database mirror, potrebbe essere necessario aggiornare manualmente [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] e Monitoraggio replica per applicare la modifica.  
  
-   In Monitoraggio replica vengono visualizzati i nodi del server di pubblicazione nell'albero degli oggetti per il server principale e il server mirror. Se il server principale è il server attivo, le informazioni relative alla pubblicazione vengono visualizzate solo nel nodo principale in Monitoraggio replica.  
  
     Se il server mirror è il server attivo:  
  
    -   Se si è verificato un errore nell'agente, l'errore è indicato solo nel nodo principale e non nel nodo mirror.  
  
    -   Se il server principale non è disponibile, i nodi principale e mirror visualizzano elenchi di pubblicazioni identici. È necessario eseguire il monitoraggio delle pubblicazioni nel nodo mirror.  
  
-   Se si amministra la replica sul database mirror mediante stored procedure o oggetti RMO (Replication Management Objects), per i casi in cui si specifica il nome del server di pubblicazione indicare il nome dell'istanza in cui il database è stato abilitato per la replica. Per determinare il nome appropriato, usare la funzione [publishingnomeserver](../../t-sql/functions/replication-functions-publishingservername.md).  
  
     Quando si esegue il mirroring di un database di pubblicazione, i metadati della replica archiviati nel database con mirroring sono identici a quelli archiviati nel database principale. Ne consegue che, per i database di pubblicazione abilitati per la replica nel database principale, il nome dell'istanza del server di pubblicazione archiviato in tabelle di sistema nel database mirror equivale al nome del database principale e non a quello del database mirror. Ciò influisce sulla manutenzione e sulla configurazione della replica se viene eseguito il failover del database di pubblicazione sul server mirror. Ad esempio, se si configura la replica con stored procedure sul database mirror dopo un failover e si vuole aggiungere una sottoscrizione pull a un database di pubblicazione abilitato nel server principale, è necessario specificare il nome del server principale invece del nome del server mirror per il parametro **\@publisher** di **sp_addpullsubscription** o **sp_addmergepullsubscription**.  
  
     Se si abilita un database di pubblicazione nel server mirror dopo che è stato eseguito il failover sul server mirror, il nome dell'istanza del server di pubblicazione archiviato nelle tabelle di sistema equivale al nome del server mirror. In questo caso, per il parametro **\@publisher** è necessario usare il nome del server mirror.  
  
    > [!NOTE]  
    >  In alcuni casi, ad esempio se si usa **sp_addpublication**, il parametro **\@publisher** è supportato solo per server di pubblicazione non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Non è quindi rilevante per il mirroring del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Per sincronizzare una sottoscrizione in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] dopo un failover, sincronizzare le sottoscrizioni pull dal Sottoscrittore e le sottoscrizioni push dal server di pubblicazione attivo.  
  
### <a name="replication-behavior-if-mirroring-is-removed"></a>Funzionamento della replica dopo l'eliminazione del mirroring  
 Se si elimina il mirroring da un database pubblicato tenere presente quanto segue:  
  
-   Se il database di pubblicazione nel server principale è senza mirroring, la replica continua senza alcuna variazione per il server principale originale.  
  
-   Se viene eseguito il failover del database di pubblicazione dal server principale sul server mirror e la relazione di mirroring viene disabilitata o eliminata, gli agenti di replica non funzioneranno sul database mirror. Se il database principale è definitivamente perso, disabilitare e quindi riconfigurare la replica con il server mirror specificato come server di pubblicazione.  
  
-   Se il mirroring del database viene eliminato completamente, il database mirror si trova in stato di recupero e deve essere ripristinato affinché funzioni. Il funzionamento del database recuperato in relazione alla replica dipende dall'impostazione dell'opzione KEEP_REPLICATION. Questa opzione indica che durante il ripristino di un database pubblicato in un server diverso da quello in cui è stato creato il backup le impostazioni di replica devono essere mantenute inalterate. Utilizzare l'opzione KEEP_REPLICATION solo quando l'altro database di pubblicazione non è disponibile. L'opzione non è infatti supportata se l'altro database di pubblicazione non è ancora stato modificato ed è oggetto di replica. Per altre informazioni sull'opzione KEEP_REPLICATION, vedere [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
## <a name="log-reader-agent-behavior"></a>Funzionamento dell'agente di lettura log  
 Nella tabella seguente viene descritto il funzionamento dell'agente di lettura log per le diverse modalità operative del mirroring del database.  
  
|Modalità operativa|Funzionamento dell'agente di lettura log quando il database mirror non è disponibile|  
|--------------------|------------------------------------------------------------|  
|Modalità a sicurezza elevata con failover automatico|Se il database mirror non è disponibile, l'agente di lettura log propaga i comandi al database di distribuzione. Non è possibile eseguire il failover del database principale sul database mirror finché il database mirror non è nuovamente online e include tutte le transazioni del database principale.|  
|Modalità a prestazioni elevate|Se il database mirror non è disponibile, il database principale è in esecuzione non protetta, ovvero è senza mirroring. Tuttavia, l'agente di lettura log replica solo le transazioni salvate nel database mirror. Se il servizio è forzato e il server mirror diventa server principale, l'agente di lettura log viene eseguito nel database mirror e recupera tutte le nuove transazioni.<br /><br /> La latenza di replica aumenta se il server mirror non è sincronizzato con il server principale.|  
|Modalità a sicurezza elevata senza failover automatico|Tutte le transazioni di cui è stato eseguito il commit vengono salvate su disco nel database mirror. L'agente di lettura log replica solo le transazioni salvate nel server mirror. Se il server mirror non è disponibile, il server principale non consente alcuna operazione nel database. L'agente di lettura log non può pertanto replicare alcuna transazione.|  
  
## <a name="see-also"></a>Vedere anche  
 [Replica di SQL Server](../../relational-databases/replication/sql-server-replication.md)   
 [Log shipping e replica &#40;SQL Server&#41;](../../database-engine/log-shipping/log-shipping-and-replication-sql-server.md)  
  
  
