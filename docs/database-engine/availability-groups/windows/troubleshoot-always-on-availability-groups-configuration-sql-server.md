---
title: Problemi comuni e soluzioni per i gruppi di disponibilità
description: Risolvere i problemi tipici relativi alla configurazione delle istanze del server per gruppi di disponibilità Always On in SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- troubleshooting [SQL Server], deploying
- Availability Groups [SQL Server], troubleshooting
- Availability Groups [SQL Server], configuring
ms.assetid: 8c222f98-7392-4faf-b7ad-5fb60ffa237e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7b7c6a5df1bcef7e8438c01910233c2c3a38d070
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337935"
---
# <a name="troubleshoot-always-on-availability-groups-configuration-sql-server"></a>Risolvere i problemi relativi alla configurazione di Gruppi di disponibilità Always On (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  In questo argomento vengono fornite informazioni per la risoluzione dei problemi tipici relativi alla configurazione delle istanze del server per [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Tra i problemi di configurazione tipici sono inclusi la disabilitazione di [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] , la configurazione errata degli account, l'endpoint del mirroring del database inesistente, l'endpoint inaccessibile (errore di SQL Server 1418), l'accesso alla rete inesistente e l'esito negativo di un comando di creazione di join del database (errore di SQL Server 35250).  
  
> [!NOTE]  
>  Verificare che vengano soddisfatti i prerequisiti [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] . Per altre informazioni, vedere [Prerequisiti, restrizioni e consigli per i gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
 **Contenuto dell'argomento:**  
  
|Sezione|Descrizione|  
|-------------|-----------------|  
|[Funzionalità Gruppi di disponibilità Always On non abilitata](#IsHadrEnabled)|Se un'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] non è abilitata per [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], l'istanza non supporta la creazione del gruppo di disponibilità e non è in grado di ospitare alcuna replica di disponibilità.|  
|[Accounts](#Accounts)|Illustra i requisiti per la corretta configurazione degli account in cui viene eseguito [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|  
|[Endpoints](#Endpoints)|Illustra come diagnosticare problemi relativi all'endpoint del mirroring del database di un'istanza del server.|  
|[Nome di sistema](#SystemName)|Riepiloga le alternative per la specifica del nome di sistema di un'istanza del server in un URL endpoint.|  
|[Accesso alla rete](#NetworkAccess)|Documenta il requisito in base a cui ogni istanza del server che ospita una replica di disponibilità deve essere in grado di accedere alla porta di ciascuna altra istanza del server su TCP.|  
|[Accesso all'endpoint (errore di SQL Server 1418)](#Msg1418)|Contiene informazioni su questo messaggio di errore di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|  
|[Errore nella creazione del join del database (errore di SQL Server 35250)](#JoinDbFails)|Illustra le possibili cause e la risoluzione di un errore nella creazione di join dei database secondari a un gruppo di disponibilità perché la connessione alla replica primaria non è attiva.|  
|[Il routing di sola lettura non funziona correttamente](#ROR)||  
|[Attività correlate](#RelatedTasks)|Contiene un elenco di argomenti orientati alle attività nella documentazione online di [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] particolarmente rilevanti per risolvere i problemi relativi a una configurazione del gruppo di disponibilità.|  
|[Contenuto correlato](#RelatedContent)|Contiene un elenco di risorse rilevanti esterne alla documentazione online di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|  
  
##  <a name="always-on-availability-groups-is-not-enabled"></a><a name="IsHadrEnabled"></a> Funzionalità Gruppi di disponibilità Always On non abilitata  
 La funzionalità [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] deve essere abilitata su ognuna delle istanze di [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]. Per altre informazioni, vedere [Abilitare e disabilitare la funzionalità Gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md).  
  
##  <a name="accounts"></a><a name="Accounts"></a> Accounts  
 È necessario configurare correttamente gli account utilizzati per l'esecuzione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
1.  Autorizzazioni corrette per gli account  
  
    1.  Se i partner vengono eseguiti con lo stesso account utente di dominio, gli account di accesso corretti saranno disponibili automaticamente in ambedue i database **master** . Questa scelta semplifica la configurazione della sicurezza del database ed è quella consigliata.  
  
    2.  Se due istanze del server vengono eseguite con account diversi, è necessario creare l'accesso per ogni account nel database **master** nell'istanza del server remoto e a tale account di accesso è necessario concedere le autorizzazioni CONNECT per la connessione all'endpoint del mirroring del database di tale istanza del server. Per altre informazioni, vedere [Configurare gli account di accesso per il mirroring del database o i gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/database-mirroring/set-up-login-accounts-database-mirroring-always-on-availability.md).  
  
2.  Se [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] viene eseguito come account predefinito, ad esempio Sistema locale, Servizio locale o Servizio di rete, oppure come account non di dominio, è necessario utilizzare certificati per l'autenticazione dell'endpoint. Se gli account del servizio utilizzano account di dominio nello stesso dominio, è possibile scegliere di concedere l'accesso CONNECT per ogni account del servizio su tutti i percorsi di replica oppure utilizzare certificati. Per altre informazioni, vedere [Usare certificati per un endpoint del mirroring del database &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md).  
  
##  <a name="endpoints"></a><a name="Endpoints"></a> Endpoints  
 È necessario configurare correttamente gli endpoint.  
  
1.  Verificare che ogni istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] che ospiterà una replica di disponibilità (ogni *percorso di replica*) disponga di un endpoint del mirroring del database. Per determinare se in una determinata istanza del server è presente un endpoint del mirroring del database, usare la vista del catalogo [sys.database_mirroring_endpoints](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md). Per altre informazioni, vedere [Creare un endpoint del mirroring del database per l'autenticazione Windows &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md) oppure [Impostare l'endpoint del mirroring del database per l'uso di certificati per le connessioni in uscita &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md).  
  
2.  Verificare che i numeri di porta siano corretti.  
  
     Per individuare la porta attualmente associata all'endpoint di mirroring del database per un'istanza del server, utilizzare l'istruzione [!INCLUDE[tsql](../../../includes/tsql-md.md)] seguente:  
  
    ```  
    SELECT type_desc, port FROM sys.tcp_endpoints;  
    GO  
    ```  
  
3.  Per i problemi di impostazione di [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] che sono difficili da diagnosticare, è consigliabile controllare ogni istanza del server per verificare che sia in attesa sulle porte corrette.  
  
4.  Verificare che gli endpoint siano stati avviati (STATE=STARTED). Utilizzare l'istruzione [!INCLUDE[tsql](../../../includes/tsql-md.md)] seguente su ogni istanza del server:  
  
    ```  
    SELECT state_desc FROM sys.database_mirroring_endpoints  
    ```  
  
     Per altre informazioni sulla colonna **state_desc**, vedere [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md).  
  
     Per avviare un endpoint, utilizzare l'istruzione [!INCLUDE[tsql](../../../includes/tsql-md.md)] seguente:  
  
    ```  
    ALTER ENDPOINT Endpoint_Mirroring   
    STATE = STARTED   
    AS TCP (LISTENER_PORT = <port_number>)  
    FOR database_mirroring (ROLE = ALL);  
    GO  
    ```  
  
     Per altre informazioni, vedere [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-endpoint-transact-sql.md).  
  
5.  Verificare che l'account di accesso dell'altro server disponga dell'autorizzazione CONNECT. Per individuare gli account che dispongono dell'autorizzazione CONNECT per un endpoint, utilizzare l'istruzione [!INCLUDE[tsql](../../../includes/tsql-md.md)] seguente su ogni istanza del server:  
  
    ```  
    SELECT 'Metadata Check';  
    SELECT EP.name, SP.STATE,   
       CONVERT(nvarchar(38), suser_name(SP.grantor_principal_id))   
          AS GRANTOR,   
       SP.TYPE AS PERMISSION,  
       CONVERT(nvarchar(46),suser_name(SP.grantee_principal_id))   
          AS GRANTEE   
       FROM sys.server_permissions SP , sys.endpoints EP  
       WHERE SP.major_id = EP.endpoint_id  
       ORDER BY Permission,grantor, grantee;   
    GO  
  
    ```  
  
##  <a name="system-name"></a><a name="SystemName"></a> System Name  
 Quale nome di sistema di un'istanza del server in un URL endpoint, è possibile utilizzare qualsiasi nome che identifichi univocamente il sistema. L'indirizzo del server può essere un nome di sistema (se i sistemi si trovano nello stesso dominio), un nome di dominio completo o un indirizzo IP (preferibilmente un indirizzo IP statico). L'utilizzo del nome di dominio completo è una soluzione efficace. Per altre informazioni, vedere [Specificare l'URL dell'endpoint quando si aggiunge o si modifica una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md).  
  
##  <a name="network-access"></a><a name="NetworkAccess"></a> Network Access  
 Ogni istanza del server che ospita una replica di disponibilità deve essere in grado di accedere alla porta di ciascuna altra istanza del server su TCP. Questo requisito è particolarmente importante quando le istanze del server appartengono a domini diversi non trusted.  
  
##  <a name="endpoint-access-sql-server-error-1418"></a><a name="Msg1418"></a> Accesso all'endpoint (errore di SQL Server 1418)  
 Questo messaggio di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] indica che l'indirizzo di rete del server specificato nell'URL endpoint non è raggiungibile o non esiste, pertanto si consiglia di controllare il nome dell'indirizzo di rete e quindi eseguire nuovamente il comando.  
  
##  <a name="join-database-fails-sql-server-error-35250"></a><a name="JoinDbFails"></a> Errore nella creazione del join del database (errore di SQL Server 35250)  
 In questa sezione vengono illustrate le possibili cause e la risoluzione di un errore nella creazione di join dei database secondari al gruppo di disponibilità perché la connessione alla replica primaria non è attiva.  
  
 **Risoluzione:**  
  
1.  Controllare l'impostazione del firewall per verificare se è consentita la comunicazione della porta dell'endpoint tra le istanze del server che ospitano la replica primaria e la replica secondaria (porta 5022 per impostazione predefinita).  
  
2.  Controllare se l'account del servizio di rete dispone di autorizzazione CONNECT all'endpoint.  
  
##  <a name="read-only-routing-is-not-working-correctly"></a><a name="ROR"></a> Il routing di sola lettura non funziona correttamente  
 Verificare le seguenti impostazioni relative ai valori di configurazione e correggerle se necessario.  
  
|In...|Azione|Commenti|Collegamento|  
|---------|------------|--------------|----------|  
|Replica primaria corrente|Assicurarsi che il listener del gruppo di disponibilità sia online.|**Per verificare se il listener è online:**<br /><br /> `SELECT * FROM sys.dm_tcp_listener_states;`<br /><br /> **Per riavviare un listener offline:**<br /><br /> `ALTER AVAILABILITY GROUP myAG RESTART LISTENER 'myAG_Listener';`|[sys.dm_tcp_listener_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-tcp-listener-states-transact-sql.md)<br /><br /> [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md)|  
|Replica primaria corrente|Verificare che READ_ONLY_ROUTING_LIST contenga solo le istanze del server che ospitano una replica secondaria leggibile.|**Per identificare repliche secondarie leggibili:** sys.availability_replicas (colonna **secondary_role_allow_connections_desc** )<br /><br /> **Per visualizzare un elenco di routing di sola lettura:** sys.availability_read_only_routing_lists<br /><br /> **Per modificare un elenco di routing di sola lettura:** ALTER AVAILABILITY GROUP|[sys.availability_replicas &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)<br /><br /> [sys.availability_read_only_routing_lists &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-read-only-routing-lists-transact-sql.md)<br /><br /> [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md)|  
|Ogni replica in read_only_routing_list|Verificare che Windows Firewall non blocchi la porta READ_ONLY_ROUTING_URL.|-|[Configurare Windows Firewall per l'accesso al motore di database](../../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)|  
|Ogni replica in read_only_routing_list|In Gestione configurazione [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] verificare quanto segue:<br /><br /> La connettività remota di SQL Server è abilitata.<br /><br /> TCP/IP è abilitato.<br /><br /> Gli indirizzi IP sono configurati correttamente.|-|[Visualizzare o modificare le proprietà del server &#40;SQL Server&#41;](../../../database-engine/configure-windows/view-or-change-server-properties-sql-server.md)<br /><br /> [Configurare un server per l'attesa su una porta TCP specifica &#40;Gestione configurazione SQL Server&#41;](../../../database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port.md)|  
|Ogni replica in read_only_routing_list|Verificare che READ_ONLY_ROUTING_URL (TCP <strong>://</strong>*system-address*<strong>:</strong>*port*) contenga il nome di dominio completo (FQDN) e il numero di porta corretti.|-|[Calcolo di read_only_routing_url per Always On](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson)<br /><br /> [sys.availability_replicas &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)<br /><br /> [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md)|  
|Sistema client|Verificare che il driver client supporti il routing di sola lettura.|-|[Connettività client Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md)|  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Creazione e configurazione di gruppi di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)  
  
-   [Creare un endpoint del mirroring del database per l'autenticazione Windows &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Specifica dell'URL dell'endpoint quando si aggiunge o si modifica una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
-   [Preparare manualmente un database secondario per un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
-   [Risolvere i problemi relativi a una operazione di aggiunta file non riuscita &#40;Gruppi di disponibilità Always On&#41;](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)  
  
-   [Gestione di account di accesso e processi per i database di un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/logins-and-jobs-for-availability-group-databases.md)  
  
-   [Gestire i metadati quando si rende disponibile un database in un'altra istanza del server &#40;SQL Server&#41;](../../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenuto correlato  
  
-   [Visualizzare eventi e log per un cluster di failover](https://technet.microsoft.com/library/cc772342\(WS.10\).aspx)  
  
-   [Pagina relativa al cluster di failover Get-ClusterLog](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee461045(v=technet.10))  
  
-   [Blog del team di SQL Server Always On: blog ufficiale del team di SQL Server Always On](/archive/blogs/sqlalwayson/)  
  
## <a name="see-also"></a>Vedere anche  
 [Sicurezza trasporto per il mirroring del database e i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Configurazione di rete dei client](../../../database-engine/configure-windows/client-network-configuration.md)   
 [Prerequisiti, restrizioni e consigli per i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
  
