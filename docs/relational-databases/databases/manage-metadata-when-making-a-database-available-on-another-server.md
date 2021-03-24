---
description: Gestire i metadati quando si rende disponibile un database in un altro server
title: Gestire i metadati quando si rende disponibile un database in un altro server
ms.date: 06/03/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
ms.assetid: 5d98cf2a-9fc2-4610-be72-b422b8682681
author: stevestein
ms.author: sstein
ms.custom: seo-dt-2019
helpviewer_keywords:
- cross-database queries [SQL Server]
- logins [SQL Server], recreating on another server instance
- triggers [SQL Server], DLL
- user-defined error messages [SQL Server]
- permissions [SQL Server], metadata access
- Full-Text Engine [SQL Server]
- metadata [SQL Server], databases available to other instances
- jobs [SQL Server Agent], recreating on another server instance
- failover [SQL Server], managing metadata
- event notifications [SQL Server], metadata
- database mirroring [SQL Server], metadata
- startup options [SQL Server]
- restoring [SQL Server], onto another server instance
- linked servers [SQL Server], metadata
- WMI Provider for Server Events, metadata
- attaching databases [SQL Server]
- log shipping [SQL Server], metadata
- encryption [SQL Server], metadata
- server configuration [SQL Server]
- distributed queries [SQL Server], metadata
- extended stored procedures [SQL Server], metadata
- credentials [SQL Server], metadata
- copying databases
ms.openlocfilehash: 8502271ba3b04e66fd6fcc0b2723e0c8e4533f96
ms.sourcegitcommit: c09ef164007879a904a376eb508004985ba06cf0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/23/2021
ms.locfileid: "104890668"
---
# <a name="manage-metadata-when-making-a-database-available-on-another-server"></a>Gestire i metadati quando si rende disponibile un database in un altro server
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Le informazioni contenute in questo articolo sono relative alle situazioni seguenti:  
  
-   Configurazione delle repliche di disponibilità di un gruppo di disponibilità [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] .  
  
-   Impostazione del mirroring per un database.  
  
-   Preparazione per il cambio di ruoli tra server primario e server secondario in una configurazione per il log shipping.  
  
-   Ripristino di un database in un'altra istanza del server.  
  
-   Collegamento di una copia di un database in un'altra istanza del server.  
  
 Alcune applicazioni dipendono da informazioni, entità e/o oggetti esterni all'ambito di un database in modalità a utente singolo. Un'applicazione include in genere dipendenze nei database **master** e **msdb** , nonché nel database utente. Qualsiasi elemento archiviato all'esterno di un database utente necessario per il corretto funzionamento di tale database deve essere reso disponibile nell'istanza del server di destinazione. Ad esempio, gli account di accesso per un'applicazione vengono archiviati come metadati nel database **master** e devono essere creati nuovamente nel server di destinazione. Se il piano di manutenzione di un'applicazione o un database dipende da processi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent i cui metadati sono archiviati nel database **msdb** , è necessario creare nuovamente tali processi nell'istanza del server di destinazione. Analogamente, i metadati per un trigger a livello di server vengono archiviati nel database **master**.  
  
 Quando il database per un'applicazione viene spostato in un'altra istanza del server, è necessario creare di nuovo tutti i metadati delle entità e degli oggetti dipendenti nei database **master** e **msdb** dell'istanza del server di destinazione. Ad esempio, se un'applicazione del database utilizza trigger a livello di server, non è sufficiente collegare o ripristinare il database nel nuovo sistema. Il database non funzionerà come previsto a meno che non si ricreino manualmente i metadati per tali trigger nel database **master** .  
  
##  <a name="information-entities-and-objects-that-are-stored-outside-of-user-databases"></a><a name="information_entities_and_objects"></a> Informazioni, entità e oggetti archiviati all'esterno dei database utente  
 Nel resto dell'articolo vengono riepilogate le potenziali problematiche che possono influenzare un database reso disponibile in un'altra istanza del server. Potrebbe essere necessario ricreare uno o più tipi di informazioni, entità o oggetti indicati nell'elenco seguente. Per visualizzare un riepilogo, fare clic sul collegamento per l'elemento.  
  
-   [Impostazioni di configurazione del server](#server_configuration_settings)  
  
-   [Credenziali](#credentials)  
  
-   [Query tra database](#cross_database_queries)  
  
-   [Proprietà dei database](#database_ownership)  
  
-   [Query distribuite o server collegati](#distributed_queries_and_linked_servers)  
  
-   [Dati crittografati](#encrypted_data)  
  
-   [Messaggi di errore definiti dall'utente](#user_defined_error_messages)  
  
-   [Notifiche degli eventi ed eventi di Strumentazione gestione Windows (WMI) (a livello del server)](#event_notif_and_wmi_events)  
  
-   [Stored procedure estese](#extended_stored_procedures)  
  
-   [Proprietà del motore di ricerca full-text per SQL Server](#ifts_service_properties)  
  
-   [Processi](#jobs)  
  
-   [Account di accesso](#logins)  
  
-   [Autorizzazioni](#permissions)  
  
-   [Impostazioni di replica](#replication_settings)  
  
-   [Applicazioni Service Broker](#sb_applications)  
  
-   [Procedure di avvio](#startup_procedures)  
  
-   [Trigger (a livello del server)](#triggers)  
  
##  <a name="server-configuration-settings"></a><a name="server_configuration_settings"></a> Server Configuration Settings  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] e versioni successive i servizi e le funzionalità chiave vengono installati e avviati in modo selettivo. In questo modo, è possibile ridurre la superficie di attacco del sistema. Nella configurazione predefinita di nuove installazioni, molte funzionalità non sono abilitate. Se il database si basa su qualsiasi funzionalità o servizio disabilitato per impostazione predefinita, tale funzionalità o servizio dovrà essere abilitato nell'istanza del server di destinazione.  
  
 Per altre informazioni su queste impostazioni e sulla relativa abilitazione o disabilitazione, vedere [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
  
##  <a name="credentials"></a><a name="credentials"></a> Credenziali  
 Una credenziale è un record contenente le informazioni di autenticazione necessarie per connettersi a una risorsa all'esterno di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La maggior parte delle credenziali è costituita da un account di accesso e da una password di Windows.  
  
 Per altre informazioni su questa funzionalità, vedere [Credenziali &#40;Motore di database&#41;](../../relational-databases/security/authentication-access/credentials-database-engine.md).  
  
> **NOTA:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent usano credenziali. Per conoscere l'ID delle credenziali di un account proxy, utilizzare la tabella di sistema [sysproxies](../../relational-databases/system-tables/dbo-sysproxies-transact-sql.md) .  
  
  
##  <a name="cross-database-queries"></a><a name="cross_database_queries"></a> Cross-Database Queries  
 Il valore predefinito delle opzioni DB_CHAINING e TRUSTWORTHY è OFF. Se una di queste opzioni è impostata su ON per il database originale, può essere necessario abilitarla nel database nell'istanza del server di destinazione. Per altre informazioni, vedere [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
 Le operazioni di collegamento e scollegamento consentono la disabilitazione del concatenamento della proprietà tra database per il database. Per informazioni su come abilitare il concatenamento, vedere [Opzione di configurazione del server cross db ownership chaining](../../database-engine/configure-windows/cross-db-ownership-chaining-server-configuration-option.md).  
  
 Per altre informazioni, vedere [Impostare un database mirror per l'uso della proprietà Trustworthy &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md)  
  
  
##  <a name="database-ownership"></a><a name="database_ownership"></a> Database Ownership  
 Quando un database viene ripristinato in un altro computer, l'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o l'utente di Windows che ha iniziato l'operazione di ripristino diventa automaticamente il proprietario del nuovo database. Al momento del ripristino, l'amministratore di sistema o il nuovo proprietario del database possono modificare il proprietario del database.  
  
##  <a name="distributed-queries-and-linked-servers"></a><a name="distributed_queries_and_linked_servers"></a> Query distribuite e server collegati  
 Le query distribuite e i server collegati sono supportati per le applicazioni OLE DB. Le query distribuite consentono di accedere ai dati da più origini di dati eterogenee nello stesso computer o in computer diversi. Una configurazione con server collegati consente a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di eseguire comandi su origini dei dati OLE DB in server remoti. Per altre informazioni su queste funzionalità, vedere [Server collegati &#40;Motore di database&#41;](../../relational-databases/linked-servers/linked-servers-database-engine.md).  
  
  
##  <a name="encrypted-data"></a><a name="encrypted_data"></a> Encrypted Data  
 Se nel database che si sta rendendo disponibile in un'altra istanza del server sono contenuti dati crittografati e se la chiave master del database è protetta dalla chiave master del servizio nel server originale, potrebbe essere necessario ricreare la crittografia della chiave master del servizio. La *chiave master del database* è una chiave simmetrica che viene utilizzata per proteggere le chiavi private di certificati e chiavi asimmetriche in un database crittografato. Al momento della creazione, la chiave master del database viene crittografata con l'algoritmo Triple DES e una password specificata dall'utente.  
  
 Per abilitare la decrittografia automatica della chiave master del database in un'istanza del server, viene crittografata una copia di questa chiave utilizzando la chiave master del servizio. Questa copia crittografata viene archiviata sia nel database che nel database **master**. La copia archiviata nel database **master** viene generalmente aggiornata in modo automatico in seguito a ogni modifica della chiave master. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si tenta innanzitutto di decrittografare la chiave master del database con la chiave master del servizio dell'istanza. Se il tentativo non riesce, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si esegue una ricerca nell'archivio delle credenziali per individuare le credenziali di chiave master con lo stesso GUID del database per cui è richiesta la chiave master. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si tenta quindi di decrittografare la chiave master del database con ogni credenziale corrispondente fino a quando non si riesce a completare l'operazione o non sono disponibili altre credenziali da provare. Per aprire una chiave master non crittografata con la chiave master del servizio, è necessario utilizzare l'istruzione OPEN MASTER KEY e una password.  
  
 Quando viene copiato, ripristinato o collegato un database crittografato in una nuova istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], una copia della chiave master del database crittografata dalla chiave master del servizio non viene archiviata nel database **master** nell'istanza del server di destinazione. Nell'istanza del server di destinazione, è necessario aprire la chiave master del database. Per aprire la chiave master, eseguire l'istruzione seguente: OPEN MASTER KEY DECRYPTION BY PASSWORD **='** _password_ **'** . È quindi consigliabile abilitare la decrittografia automatica della chiave master del database eseguendo l'istruzione ALTER MASTER KEY ADD ENCRYPTION BY SERVICE MASTER KEY. L'istruzione ALTER MASTER KEY fornisce all'istanza del server una copia della chiave master del database crittografata con la chiave master del servizio. Per altre informazioni, vedere [OPEN MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/open-master-key-transact-sql.md) e [ALTER MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-master-key-transact-sql.md).  
  
 Per informazioni sull'abilitazione della decrittografia automatica della chiave master del database di un database mirror, vedere [Impostare un database mirror crittografato](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md).  
  
 Per ulteriori informazioni, vedere anche:  
  
-   [Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
-   [Impostazione di un database mirror crittografato](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)  
  
-   [Creare chiavi simmetriche identiche su due server](../../relational-databases/security/encryption/create-identical-symmetric-keys-on-two-servers.md)  
  
  
##  <a name="user-defined-error-messages"></a><a name="user_defined_error_messages"></a> User-defined Error Messages  
 I messaggi di errore definiti dall'utente sono contenuti nella vista del catalogo [sys.messages](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md) . Questa vista del catalogo è archiviata nel database **master**. Se un'applicazione del database dipende da messaggi di errore definiti dall'utente e il database è reso disponibile in un'altra istanza del server, usare [sp_addmessage](../../relational-databases/system-stored-procedures/sp-addmessage-transact-sql.md) per aggiungere tali messaggi definiti dall'utente nell'istanza del server di destinazione.  

  
##  <a name="event-notifications-and-windows-management-instrumentation-wmi-events-at-server-level"></a><a name="event_notif_and_wmi_events"></a> Le notifiche degli eventi e gli eventi di Windows Management Instrumentation (WMI) (a livello di Server)  
  
### <a name="server-level-event-notifications"></a>Notifiche degli eventi a livello di server  
 Le notifiche degli eventi a livello di server sono archiviate nel database **msdb**. Se un'applicazione del database si basa su una notifica degli eventi a livello di server, tale notifica deve essere nuovamente creata nell'istanza del server di destinazione. Per visualizzare le notifiche degli eventi in un'istanza del server, usare la vista del catalogo [sys.server_event_notifications](../../relational-databases/system-catalog-views/sys-server-event-notifications-transact-sql.md) . Per altre informazioni, vedere [Event Notifications](../../relational-databases/service-broker/event-notifications.md).  
  
 Le notifiche degli eventi vengono inoltre recapitate utilizzando [!INCLUDE[ssSB](../../includes/sssb-md.md)]. I route per i messaggi in ingresso non sono inclusi nel database che contiene un servizio. I route espliciti sono invece archiviati nel database **msdb**. Se il servizio consente di usare una route esplicita nel database **msdb** per eseguire il routing dei messaggi in arrivo al servizio, quando si collega un database in un'istanza diversa è necessario ricreare questa route.  
  
### <a name="windows-management-instrumentation-wmi-events"></a>Eventi di Strumentazione gestione Windows (WMI)  
 Il provider WMI per eventi del server consente di utilizzare il servizio Strumentazione gestione Windows (WMI) per monitorare eventi in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Qualsiasi applicazione che si basi su eventi a livello di server esposti tramite il provider WMI utilizzato da un database deve essere definita nel computer dell'istanza del server di destinazione. Il provider di eventi WMI crea le notifiche degli eventi con un servizio di destinazione definito nel database **msdb**.  
  
> **NOTA:** per altre informazioni, vedere [Concetti relativi al provider WMI per eventi del server](../../relational-databases/wmi-provider-server-events/wmi-provider-for-server-events-concepts.md).  
  
 **Per creare un avviso di WMI utilizzando SQL Server Management Studio**  
  
-   [Creare un avviso per evento WMI](../../ssms/agent/create-a-wmi-event-alert.md)  
  
### <a name="how-event-notifications-work-for-a-mirrored-database"></a>Funzionamento delle notifiche degli eventi per un database con mirroring  
 Il recapito tra database delle notifiche degli eventi che richiede un database con mirroring è remoto, per definizione, in quanto per il database con mirroring è possibile eseguire il failover. [!INCLUDE[ssSB](../../includes/sssb-md.md)] offre uno speciale supporto per i database con mirroring, sotto forma di *route con mirroring*. Un route con mirroring dispone di due indirizzi, uno per l'istanza del server principale e uno per l'istanza del server mirror.  
  
 Impostando i route con mirroring, si rende il routing di [!INCLUDE[ssSB](../../includes/sssb-md.md)] compatibile con il mirroring del database. I route con mirroring consentono a [!INCLUDE[ssSB](../../includes/sssb-md.md)] di reindirizzare in modo trasparente le conversazioni all'istanza del server principale corrente. Prendere, ad esempio, in considerazione un servizio, Service_A, ospitato da un database con mirroring, Database_A. Si supponga che sia necessario un altro servizio, Service_B ospitato da Database_B, che comunichi con Service_A. Affinché tale comunicazione sia possibile, Database_B deve contenere una route con mirroring per Service_A. Inoltre, Database_A deve contenere una route di trasporto TCP senza mirroring a Service_B che, diversamente da una route locale, rimane valido dopo il failover. Questi route consentono il ritorno degli ACK dopo un failover. Dato che il servizio del mittente è sempre denominato nello stesso modo, il route deve specificare l'istanza del broker.  
  
 Il requisito per i route con mirroring si applica indipendentemente dal fatto che il servizio nel database con mirroring sia il servizio Initiator o il servizio di destinazione:  
  
-   Se il servizio di destinazione si trova nel database con mirroring, il servizio Initiator deve disporre di un route con mirroring verso la destinazione. Tuttavia, la destinazione può disporre di un route regolare verso l'Initiator.  
  
-   Se il servizio Initiator si trova nel database con mirroring, il servizio di destinazione deve disporre di un route con mirroring verso l'Initiator per recapitare acknowledgement e risposte. Tuttavia, l'Initiator può disporre di una route regolare verso la destinazione.  
  
  
##  <a name="extended-stored-procedures"></a><a name="extended_stored_procedures"></a> Extended Stored Procedures  
  
> **IMPORTANTE** [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Usare invece la funzionalità [Integrazione CLR](../../relational-databases/clr-integration/common-language-runtime-integration-overview.md) .  
  
 Le stored procedure estese sono programmate utilizzando l'API per stored procedure estese di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Un membro del ruolo predefinito del server **sysadmin** può registrare una stored procedure estesa con un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e concedere agli utenti l'autorizzazione a eseguire la procedura. Le stored procedure estese possono essere aggiunte soltanto al database **master** .  
  
 Le stored procedure estese vengono eseguite direttamente nello spazio degli indirizzi di un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]e possono produrre perdite di memoria o altri problemi che riducono le prestazioni e l'affidabilità del server. È consigliabile valutare l'opportunità di archiviare le stored procedure estese in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] distinta dall'istanza contenente i dati di riferimento. Valutare inoltre l'opportunità di utilizzare query distribuite per accedere al database.  
  
  > [!IMPORTANT]
  > Prima di aggiungere stored procedure estese al server e concedere le autorizzazioni EXECUTE ad altri utenti, è necessario che l'amministratore di sistema esamini con attenzione ogni stored procedure estesa per verificare che non contenga codice dannoso o malware.  
  
 Per altre informazioni, vedere [GRANT - autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md), [DENY - autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md) e [REVOKE - autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-object-permissions-transact-sql.md).  
  
  
##  <a name="full-text-engine-for-sql-server-properties"></a><a name="ifts_service_properties"></a> Full-Text Engine for SQL Server Properties  
 Le proprietà per il motore di ricerca full-text vengono impostate da [sp_fulltext_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md). Verificare che l'istanza del server di destinazione disponga delle impostazioni necessarie per queste proprietà. Per altre informazioni su queste proprietà, vedere [FULLTEXTSERVICEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/fulltextserviceproperty-transact-sql.md).  
  
 Se le versioni del componente [word breaker e stemmer](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md) o del componente [filtri di ricerca full-text](../../relational-databases/search/configure-and-manage-filters-for-search.md) sono diverse nelle istanze del server originale e del server di destinazione, l'indice e le query full-text possono comportarsi in modo diverso. Anche il [thesaurus](../../relational-databases/search/configure-and-manage-thesaurus-files-for-full-text-search.md) viene archiviato in file specifici dell'istanza. È necessario trasferire una copia di questi file in un percorso equivalente nell'istanza del server di destinazione oppure ricrearli nella nuova istanza.  
  
> **NOTA:** quando si collega un database di [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] contenente file di catalogo full-text in un'istanza del server di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] , i file di catalogo vengono collegati dal percorso precedente insieme agli altri file del database, come in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Per altre informazioni, vedere [Aggiornamento della ricerca full-text](../../relational-databases/search/upgrade-full-text-search.md).  
  
 Per ulteriori informazioni, vedere anche:  
  
-   [Backup e ripristino di indici e cataloghi full-text](../../relational-databases/search/back-up-and-restore-full-text-catalogs-and-indexes.md)  
  
-   [Mirroring di database e cataloghi full-text &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-full-text-catalogs-sql-server.md)  

  
##  <a name="jobs"></a><a name="jobs"></a> Processi  
 Se il database si basa su processi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, sarà necessario ricrearli nell'istanza del server di destinazione. I processi dipendono dai relativi ambienti. Se si pianifica di ricreare un processo esistente nell'istanza del server di destinazione, può essere necessario modificare l'istanza del server di destinazione in modo che corrisponda all'ambiente di tale processo nell'istanza del server originale. I fattori ambientali seguenti sono significativi:  
  
-   Account di accesso utilizzato dal processo  
  
     Per creare o eseguire i processi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, è innanzitutto necessario aggiungere tutti gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] richiesti dal processo all'istanza del server di destinazione. Per altre informazioni, vedere [Configurare un utente per la creazione e la gestione di processi di SQL Server Agent](../../ssms/agent/configure-a-user-to-create-and-manage-sql-server-agent-jobs.md).  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent  
  
     L'account di avvio del servizio definisce l'account di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows in cui viene eseguito [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, nonché le relative autorizzazioni di rete. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent viene eseguito con un account utente specificato. Il contesto del servizio SQL Server Agent influisce sulle impostazioni per il processo e per il relativo ambiente di esecuzione. È necessario che l'account abbia accesso alle risorse, ad esempio alle condivisioni di rete, richieste dal processo. Per informazioni su come selezionare e modificare l'account di avvio del servizio, vedere [Selezionare un account per il servizio SQL Server Agent](../../ssms/agent/select-an-account-for-the-sql-server-agent-service.md).  
  
     Per un corretto funzionamento, è necessario che l'account di avvio del servizio sia configurato con dominio, file system e autorizzazioni per il Registro di sistema appropriati. Inoltre, un processo potrebbe richiedere una risorsa di rete condivisa che deve essere configurata per l'account del servizio. Per informazioni, vedere [Configurare account di servizio e autorizzazioni di Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, associato a un'istanza specifica di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], dispone di un proprio hive del Registro di sistema e i relativi processi presentano in genere dipendenze da una o più delle impostazioni in questo hive del Registro di sistema. Per funzionare come previsto, un processo richiede queste impostazioni del Registro di sistema. Se si utilizza uno script per ricreare un processo in un altro servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, è possibile che nel Registro di sistema relativo non siano disponibili le impostazioni corrette per tale processo. Affinché i processi ricreati funzionino correttamente in un'istanza del server di destinazione, è necessario che i servizi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent originale e di destinazione presentino le stesse impostazioni del Registro di sistema.  
  
    > [!CAUTION]  
    >  La modifica delle impostazioni del Registro di sistema nel servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent di destinazione per gestire un processo ricreato può essere problematica se le impostazioni correnti sono necessarie per altri processi. Se, inoltre, il Registro di sistema viene modificato in modo non appropriato, il sistema potrebbe venire gravemente danneggiato. Prima di modificare il Registro di sistema, è consigliabile eseguire il backup di tutti i dati importanti disponibili nel computer.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent  
  
     Un proxy di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent definisce il contesto di sicurezza per il passaggio di processo specificato. Affinché un processo venga eseguito nell'istanza del server di destinazione, è necessario ricreare in tale istanza tutti i proxy di cui necessita il processo. Per altre informazioni, vedere [Creare un proxy di SQL Server Agent](../../ssms/agent/create-a-sql-server-agent-proxy.md) e [Risolvere i problemi relativi a processi multiserver che usano proxy](../../ssms/agent/troubleshoot-multiserver-jobs-that-use-proxies.md).  
  
 Per ulteriori informazioni, vedere anche:  
  
-   [Implementazione di processi](../../ssms/agent/implement-jobs.md)  
  
-   [Gestione di account di accesso e di processi dopo un cambio di ruolo &#40;SQL Server&#41;](../../sql-server/failover-clusters/management-of-logins-and-jobs-after-role-switching-sql-server.md) (per il mirroring del database)  
  
-   [Configurare account di servizio e autorizzazioni di Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md) (quando si installa un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)])  
  
-   [Configurare SQL Server Agent](../../ssms/agent/configure-sql-server-agent.md) (quando si installa un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)])  
  
-   [Implementazione della sicurezza di SQL Server Agent](../../ssms/agent/implement-sql-server-agent-security.md)  
  
 **Per visualizzare processi esistenti e relative proprietà**  
  
-   [Monitoraggio delle attività del processo](../../ssms/agent/monitor-job-activity.md)  
  
-   [sp_help_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)  
  
-   [Visualizzare informazioni sui passaggi di processo](../../ssms/agent/view-job-step-information.md)  
  
-   [dbo.sysjobs &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-sysjobs-transact-sql.md)  
  
 **Per creare un processo**  
  
-   [Creazione di un processo](../../ssms/agent/create-a-job.md)  
  
#### <a name="best-practices-for-using-a-script-to-re-create-a-job"></a>Procedure consigliate per l'utilizzo di uno script per ricreare un processo  
 Per iniziare, è consigliabile creare lo script di un processo semplice, ricreare il processo nell'altro servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent ed eseguire il processo per verificare se funziona come previsto. In questo modo, è possibile identificare eventuali incompatibilità e tentare di risolverle. Se un processo per cui è stato creato uno script non funziona come previsto nel nuovo ambiente, è consigliabile creare un processo equivalente che funzioni correttamente in tale ambiente.  
  

##  <a name="logins"></a><a name="logins"></a> Account di accesso  
 L'accesso a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] richiede un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valido. Questo account di accesso viene utilizzato nel processo di autenticazione che verifica se l'entità può connettersi all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un utente del database il cui account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] corrispondente non è definito o è definito in modo errato in un'istanza del server non potrà accedere a tale istanza. Questo utente viene definito *utente orfano* del database nell'istanza del server. Un utente del database può divenire isolato (orfano) dopo il ripristino, il collegamento o la copia di un database in un'altra istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Per generare uno script per tutti gli oggetti nella copia originale del database o per alcuni di essi, è possibile utilizzare Generazione guidata script e, nella finestra di dialogo **Selezione opzioni generazione script** , impostare l'opzione **Script per account di accesso** su **True**.  
  
> **NOTA:** Per informazioni su come configurare gli account di accesso per un database con mirroring, vedere [Configurare gli account di accesso per il mirroring del database o i gruppi di disponibilità Always On (SQL Server)](../../database-engine/database-mirroring/set-up-login-accounts-database-mirroring-always-on-availability.md) e [Gestione di account di accesso e di processi dopo un cambio di ruolo &#40;SQL Server&#41;](../../sql-server/failover-clusters/management-of-logins-and-jobs-after-role-switching-sql-server.md).  
  
  
##  <a name="permissions"></a><a name="permissions"></a> Autorizzazioni  
 I tipi seguenti di autorizzazioni possono essere influenzati quando un database viene reso disponibile in un'altra istanza del server.  
  
-   Autorizzazioni GRANT, REVOKE o DENY per gli oggetti di sistema  
  
-   Autorizzazioni GRANT, REVOKE o DENY nell'istanza del server (*autorizzazioni a livello di server*)  
  
### <a name="grant-revoke-and-deny-permissions-on-system-objects"></a>Autorizzazioni GRANT, REVOKE o DENY per gli oggetti di sistema  
 Le autorizzazioni per gli oggetti di sistema, ad esempio stored procedure, stored procedure estese, funzioni e viste, sono archiviate nel database **master** e devono essere configurate nell'istanza del server di destinazione.  
  
 Per generare uno script per alcuni o tutti gli oggetti nella copia originale del database è possibile usare la procedura guidata di generazione script e, nella finestra di dialogo **Selezione opzioni generazione script** impostare l'opzione **Script per autorizzazioni a livello oggetto** su **True**.  
  
   > [!IMPORTANT]
   > Se si creano script per account di accesso, le password non vengono incluse negli script. Se sono presenti account di accesso che utilizzano l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è necessario modificare lo script nella destinazione.  
  
 Gli oggetti di sistema sono visibili nella vista del catalogo [sys.system_objects](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md) . Le autorizzazioni per gli oggetti di sistema sono visibili nella vista del catalogo [sys.database_permissions](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md) nel database **master** . Per informazioni su come eseguire query in queste viste del catalogo e su come concedere autorizzazioni per gli oggetti di sistema, vedere [GRANT - autorizzazioni per oggetti di sistema &#40;Transact-SQL&#41;](../../t-sql/statements/grant-system-object-permissions-transact-sql.md). Per altre informazioni, vedere [REVOKE - autorizzazioni per oggetti di sistema &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-system-object-permissions-transact-sql.md) e [DENY - autorizzazioni per oggetti di sistema &#40;Transact-SQL&#41;](../../t-sql/statements/deny-system-object-permissions-transact-sql.md).  
  
### <a name="grant-revoke-and-deny-permissions-on-a-server-instance"></a>Autorizzazioni GRANT, REVOKE o DENY per un'istanza del server  
 Le autorizzazioni nell'ambito del server vengono archiviate nel database **master** e devono essere configurate nell'istanza del server di destinazione. Per informazioni sulle autorizzazioni del server di un'istanza del server, eseguire una query nella vista del catalogo [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md) . Per informazioni sulle entità del server, eseguire una query nella vista del catalogo [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)e per informazioni sull'appartenenza ai ruoli del server, eseguire una query nella vista del catalogo [sys.server_role_members](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md) .  
  
 Per altre informazioni, vedere [GRANT - autorizzazioni per server&#40;Transact-SQL&#41;](../../t-sql/statements/grant-server-permissions-transact-sql.md), [REVOKE - autorizzazioni per server &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-server-permissions-transact-sql.md) e [DENY - autorizzazioni per server &#40;Transact-SQL&#41;](../../t-sql/statements/deny-server-permissions-transact-sql.md).  
  
#### <a name="server-level-permissions-for-a-certificate-or-asymmetric-key"></a>Autorizzazioni a livello del server per un certificato o una chiave asimmetrica  
 Non è possibile concedere autorizzazioni a livello del server direttamente a un certificato o a una chiave asimmetrica. Le autorizzazioni a livello del server vengono viceversa concesse a un account di accesso con mapping creato esclusivamente per un certificato o una chiave asimmetrica specifica. Ogni certificato o chiave asimmetrica che richiede autorizzazioni a livello del server richiede quindi un proprio *account di accesso con mapping al certificato* o un *account di accesso con mapping alla chiave asimmetrica*. Per concedere autorizzazioni a livello del server per un certificato o una chiave asimmetrica, concedere le autorizzazioni al relativo account di accesso con mapping.  
  
> **NOTA:** un account di accesso con mapping viene usato solo per l'autorizzazione del codice firmato con il certificato o la chiave asimmetrica corrispondente. Gli account di accesso con mapping non possono essere utilizzati per l'autenticazione.  
  
 L'account di accesso con mapping e le relative autorizzazioni risiedono nel database **master**. Se un certificato o una chiave asimmetrica risiede in un database diverso da **master** è necessario ricreare tale certificato o chiave asimmetrica nel database **master** ed eseguirne il mapping a un account di accesso. Se si sposta, copia o ripristina il database in un'altra istanza del server, è necessario ricreare tale certificato o chiave asimmetrica nel database **master** dell'istanza del server di destinazione, eseguirne il mapping a un account di accesso e concedere le autorizzazioni a livello del server richieste all'account di accesso.  
  
 **Per creare un certificato o una chiave asimmetrica**  
  
-   [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)  
  
-   [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)  
  
 **Per eseguire il mapping di un certificato o una chiave asimmetrica a un account di accesso**  
  
-   [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)  
  
 **Per assegnare autorizzazioni all'account di accesso con mapping**  
  
-   [GRANT - autorizzazioni per server &#40;Transact-SQL&#41;](../../t-sql/statements/grant-server-permissions-transact-sql.md)  
  
 Per ulteriori informazioni su certificati e chiavi asimmetriche, vedere [Encryption Hierarchy](../../relational-databases/security/encryption/encryption-hierarchy.md).  
  
## <a name="trustworthy-property"></a>Proprietà TRUSTWORHTY
La proprietà di database TRUSTWORTHY consente di indicare se l'istanza di SQL Server considera attendibile il database e il relativo contenuto. Quando viene collegato un database, per impostazione predefinita e per questioni di sicurezza, questa opzione è impostata su OFF, anche se era impostata su ON nel server originale. Per alte informazioni su questa proprietà, vedere [Proprietà di database TRUSTWORTHY](../security/trustworthy-database-property.md). Per informazioni su come impostare questa opzione su ON, vedere [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  


##  <a name="replication-settings"></a><a name="replication_settings"></a> Replication Settings  
 Se si ripristina un backup di un database replicato in un altro server o database, le impostazioni di replica non potranno essere mantenute. In questo caso, è necessario ricreare tutte le pubblicazioni e le sottoscrizioni dopo il ripristino dei backup. Per semplificare questo processo, creare script per le impostazioni di replica correnti e per l'abilitazione e la disabilitazione della replica. Per ricreare più agevolmente le impostazioni di replica, copiare questi script e modificare i riferimenti al nome del server in base all'istanza del server di destinazione.  
  
 Per altre informazioni, vedere [Backup e ripristino di database replicati](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md), [Mirroring e replica del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md) e [Log shipping e replica &#40;SQL Server&#41;](../../database-engine/log-shipping/log-shipping-and-replication-sql-server.md).  
  
  
##  <a name="service-broker-applications"></a><a name="sb_applications"></a> Service Broker Applications  
 Insieme al database vengono spostati molti aspetti di un'applicazione di [!INCLUDE[ssSB](../../includes/sssb-md.md)] . Tuttavia, alcuni aspetti dell'applicazione dovranno essere ricreati o riconfigurati nella nuova posizione.  Quando un database viene collegato da un altro server, per impostazione predefinita e per questioni di sicurezza, le opzioni *is_broker_enabled* e *is_honoor_broker_priority_on* sono impostate su OFF. Per informazioni su come impostare queste opzioni su ON, vedere [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
  
##  <a name="startup-procedures"></a><a name="startup_procedures"></a> Startup Procedures  
 Una procedura di avvio è una stored procedure contrassegnata per l'esecuzione automatica che viene eseguita a ogni avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Se il database dipende da procedure di avvio, è necessario definire tali procedure nell'istanza del server di destinazione e configurarle per l'esecuzione automatica all'avvio.  

  
##  <a name="triggers-at-server-level"></a><a name="triggers"></a> Triggers (at Server Level)  
 I trigger DDL attivano stored procedure in risposta a vari eventi DDL (Data Definition Language). Questi eventi corrispondono principalmente a istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] che iniziano con le parole chiave CREATE, ALTER e DROP. Alcune stored procedure di sistema che eseguono operazioni di tipo DDL possono inoltre attivare trigger DDL.  
  
 Per ulteriori informazioni su questa funzionalità, vedere [DDL Triggers](../../relational-databases/triggers/ddl-triggers.md).  
  
  
## <a name="see-also"></a>Vedere anche  
 [Database indipendenti](../../relational-databases/databases/contained-databases.md)   
 [Copia di database in altri server](../../relational-databases/databases/copy-databases-to-other-servers.md)   
 [Collegamento e scollegamento di un database &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [Eseguire il failover in un database secondario per il log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md)   
 [Cambio di ruolo durante una sessione di mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)   
 [Impostare un database mirror crittografato](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)   
 [Gestione configurazione SQL Server](../../relational-databases/sql-server-configuration-manager.md)   
 [Risolvere i problemi relativi agli utenti isolati &#40;SQL Server&#41;](../../sql-server/failover-clusters/troubleshoot-orphaned-users-sql-server.md)  
  
  
