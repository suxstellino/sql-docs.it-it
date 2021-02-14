---
title: Monitorare i gruppi di disponibilità con Transact-SQL (T-SQL)
description: Usare istruzioni Transact-SQL SELECT per monitorare i gruppi di disponibilità Always On e i relativi database e repliche.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- dynamic management views [SQL Server], AlwaysOn Availability Groups
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], listeners
- Availability Groups [SQL Server], databases
- catalog views [SQL Server], AlwaysOn Availability Groups
ms.assetid: 881a34de-8461-4811-8c62-322bf7226bed
author: cawrites
ms.author: chadam
ms.openlocfilehash: d2bccd9808ad397d7cc83285577aea3483b253c4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352660"
---
# <a name="monitor-availability-groups-transact-sql"></a>Monitorare Gruppi di disponibilità (Transact-SQL)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Per monitorare le repliche e i gruppi di disponibilità e i database associati tramite [!INCLUDE[tsql](../../../includes/tsql-md.md)], [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] offre un set di viste del catalogo, di DMV e di proprietà del server. Tramite le istruzioni [!INCLUDE[tsql](../../../includes/tsql-md.md)] SELECT, è possibile utilizzare le viste per monitorare i gruppi di disponibilità e i relativi database e repliche. Le informazioni restituite per un gruppo di disponibilità variano a seconda che l'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] a cui si è connessi ospiti la replica primaria o una replica secondaria.  
  
> [!TIP]  
>  Molte di queste viste possono essere unite tramite le relative colonne ID in modo che le informazioni vengano restituite da più viste in una singola query.  
  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] richiedono l'autorizzazione VIEW ANY DEFINITION sull'istanza del server. [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] richiedono l'autorizzazione VIEW SERVER STATE sul server.  
  
##  <a name="monitoring-the-always-on-availability-groups-feature-on-a-server-instance"></a><a name="AoAgFeatureOnSI"></a> Monitoraggio della funzionalità Gruppi di disponibilità Always On su un'istanza del server  
 Per monitorare la funzionalità [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] in un'istanza del server, utilizzare la funzione predefinita seguente:  
  
 Funzione[SERVERPROPERTY](../../../t-sql/functions/serverproperty-transact-sql.md)  
 Restituisce informazioni sulle proprietà del server in cui è specificato se [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] è abilitato e, in caso affermativo, se è stato avviato sull'istanza del server.  
  
 **Nomi di colonna:** IsHadrEnabled, HadrManagerStatus  
  
##  <a name="monitoring-availability-groups-on-the-wsfc-cluster"></a><a name="WSFC"></a> Monitoraggio di Gruppi di disponibilità nel cluster WSFC  
 Per monitorare il cluster WSFC (Windows Server Failover Clustering) che ospita un'istanza del server locale abilitata per [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], utilizzare le viste seguenti:  
  
 [sys.dm_hadr_cluster](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-transact-sql.md)  
 Se il nodo WSFC (Windows Server Failover Clustering) che ospita un'istanza di SQL Server con [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] abilitato ha il quorum WSFC, **sys.dm_hadr_cluster** restituisce una riga che espone il nome del cluster e informazioni sul quorum. Se il nodo WSFC non dispone di quorum, non viene restituita alcuna riga.  
  
 **Nomi delle colonne:** cluster_name, quorum_type, quorum_type_desc, quorum_state, quorum_state_desc  
  
 [sys.dm_hadr_cluster_members](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md)  
 Se il nodo WSFC che ospita l'istanza locale Always On di SQL Server ha il quorum WSFC, restituisce una riga per ogni membro che costituisce il quorum e lo stato di ognuno di essi.  
  
 **Nomi delle colonne:** member_name, member_type, member_type_desc, member_state, member_state_desc, number_of_quorum_votes  
  
 [sys.dm_hadr_cluster_networks](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-networks-transact-sql.md)  
 Restituisce una riga per ogni membro che partecipa alla configurazione della subnet di un gruppo di disponibilità. È possibile utilizzare questa DMV per convalidare l'indirizzo IP virtuale di rete configurato per ogni replica di disponibilità.  
  
 **Nomi delle colonne:** member_name, network_subnet_ip, network_subnet_ipv4_mask, network_subnet_prefix_length, is_public, is_ipv4  
  
 **Chiave primaria:** member_name + network_subnet_IP + network_subnet_prefix_length  
  
 [sys.dm_hadr_instance_node_map](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-instance-node-map-transact-sql.md)  
 Per ogni istanza di SQL Server che ospita una replica di disponibilità che ha creato un join al gruppo di disponibilità Always On, restituisce il nome del nodo Windows Server Failover Clustering (WSFC) che ospita l'istanza del server. Questa DMV offre i seguenti utilizzi:  
  
-   Questa DMV è utile per il rilevamento di un gruppo di disponibilità con più repliche di disponibilità ospitate nello stesso nodo WSFC. Si tratta di una configurazione non supportata che si potrebbe verificare dopo un failover dell'istanza del cluster di failover se il gruppo di disponibilità non è stato correttamente configurato.  
  
-   Quando più istanze di SQL Server sono ospitate sullo stesso nodo WSFC, la DLL della risorsa utilizza questa DMV per determinare l'istanza di SQL Server a cui connettersi.  
  
 **:** ag_resource_id, instance_name, node_name  
  
 [sys.dm_hadr_name_id_map](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-name-id-map-transact-sql.md)  
 Mostra il mapping dei gruppi di disponibilità Always On dove per l'istanza corrente di SQL Server è stato creato un join a tre ID univoci: un ID gruppo di disponibilità, un ID risorsa WSFC e un ID gruppo WSFC. Lo scopo di questo mapping è gestire lo scenario in cui il gruppo/risorsa WSFC viene rinominato.  
  
 **Nomi delle colonne:** ag_name, ag_id, ag_resource_id, ag_group_id  
  
> [!NOTE]  
>  Vedere anche **sys.dm_hadr_availability_replica_cluster_nodes** e **sys.dm_hadr_availability_replica_cluster_states** nella sezione [Monitoraggio delle repliche di disponibilità](#AvReplicas) e **sys.availability_databases_cluster** e **sys.dm_hadr_database_replica_cluster_states** nella sezione [Monitoraggio dei database di disponibilità](#AvDbs) più avanti in questo argomento.  
  
 Per informazioni sui cluster WSFC e [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], vedere [WSFC &#40;Windows Server Failover Clustering&#41; con SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md) e [Clustering di failover e gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md).  
  
##  <a name="monitoring-availability-groups"></a><a name="AvGroups"></a> Monitoraggio dei gruppi disponibilità  
 Per monitorare i gruppi di disponibilità per cui l'istanza del server ospita una replica di disponibilità, utilizzare le viste seguenti:  
  
 [sys.availability_groups](../../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)  
 Restituisce una riga per ogni gruppo di disponibilità per il quale l'istanza locale di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ospita una replica di disponibilità. Ogni riga contiene una copia memorizzata nella cache dei metadati del gruppo di disponibilità.  
  
 **Nomi delle colonne:** group_id, name, resource_id, resource_group_id, failure_condition_level, health_check_timeout, automated_backup_preference, automated_backup_preference_desc  
  
 [sys.availability_groups_cluster](../../../relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md)  
 Restituisce una riga per ogni gruppo di disponibilità nel cluster WSFC. Ogni riga contiene i metadati del gruppo di disponibilità del cluster WSFC (Windows Server Failover Clustering).  
  
 **Nomi delle colonne:** group_id, name, resource_id, resource_group_id, failure_condition_level, health_check_timeout, automated_backup_preference, automated_backup_preference_desc  
  
 [sys.dm_hadr_availability_group_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md)  
 Restituisce una riga per ogni gruppo di disponibilità che dispone di una replica di disponibilità sull'istanza locale di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Ogni riga visualizza gli stati che definiscono l'integrità di un determinato gruppo di disponibilità.  
  
 **:** group_id, primary_replica, primary_recovery_health, primary_recovery_health_desc, secondary_recovery_health, secondary_recovery_health_desc, synchronization_health, synchronization_health_desc  
  
##  <a name="monitoring-availability-replicas"></a><a name="AvReplicas"></a> Monitoraggio delle repliche di disponibilità  
 Per monitorare le repliche di disponibilità, utilizzare la funzione di sistema e le viste seguenti:  
  
 [sys.availability_replicas](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)  
 Restituisce una riga per ogni replica di disponibilità in ogni gruppo di disponibilità per il quale l'istanza locale di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ospita una replica di disponibilità.  
  
 **Nomi colonne:** replica_id, group_id, replica_metadata_id, replica_server_name, owner_sid, endpoint_url, availability_mode, availability_mode_desc, failover_mode, failover_mode_desc, session_timeout, primary_role_allow_connections, primary_role_allow_connections_desc, secondary_role_allow_connections, secondary_role_allow_connections_desc, create_date, modify_date, backup_priority, read_only_routing_url  
  
 [sys.availability_read_only_routing_lists](../../../relational-databases/system-catalog-views/sys-availability-read-only-routing-lists-transact-sql.md)  
 Restituisce una riga per l'elenco di routing di sola lettura di ogni replica di disponibilità in un gruppo di disponibilità AlwaysOn nel cluster di failover WSFC.  
  
 **Nomi colonne:** replica_id, routing_priority, read_only_replica_id  
  
 [sys.dm_hadr_availability_replica_cluster_nodes](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-nodes-transact-sql.md)  
 Restituisce una riga per ogni replica di disponibilità, indipendentemente dallo stato di join, dei gruppi di disponibilità AlwaysOn nel cluster WSFC (Windows Server Failover Clustering).  
  
 **Nomi colonne:** group_name, replica_server_name, node_name  
  
 [sys.dm_hadr_availability_replica_cluster_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md)  
 Restituisce una riga per ogni replica, indipendentemente dallo stato del join, di tutti i gruppi di disponibilità AlwaysOn, indipendentemente dal percorso della replica, nel cluster WSFC (Windows Server Failover Clustering).  
  
 **Nomi colonne:** replica_id, replica_server_name, group_id, join_state, join_state_desc  
  
 [sys.dm_hadr_availability_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md)  
 Restituisce una riga in cui viene mostrato lo stato di ogni replica di disponibilità locale e una riga per ogni replica di disponibilità remota nello stesso gruppo di disponibilità.  
  
 **Nomi colonne:** replica_id, group_id, is_local, role, role_desc, operational_state, operational_state_desc, connected_state, connected_state_desc, recovery_health, recovery_health_desc, synchronization_health, synchronization_health_desc, last_connect_error_number, last_connect_error_description e last_connect_error_timestamp  
  
 [sys.fn_hadr_backup_is_preferred_replica](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md)  
 Determina se la replica corrente è la replica di backup preferita.  
  
> [!NOTE]  
>  Per informazioni sui contatori delle prestazioni per le repliche di disponibilità (oggetto prestazioni **SQLServer:Availability Replica**  ), vedere [SQL Server, replica di disponibilità](../../../relational-databases/performance-monitor/sql-server-availability-replica.md).  
  
##  <a name="monitoring-availability-databases"></a><a name="AvDbs"></a> Monitoraggio dei database di disponibilità  
 Per monitorare i database di disponibilità, utilizzare le viste seguenti:  
  
 [sys.availability_databases_cluster](../../../relational-databases/system-catalog-views/sys-availability-databases-cluster-transact-sql.md)  
 Contiene una riga per ogni database sull'istanza di SQL Server che fa parte di tutti i gruppi di disponibilità Always On nel cluster, indipendentemente dal fatto che il database della copia locale sia già stato aggiunto o meno al gruppo di disponibilità.  
  
> [!NOTE]  
>  Quando un database viene aggiunto a un gruppo di disponibilità, viene automaticamente creato un join del database primario con il gruppo. È necessario preparare i database secondari su ogni replica secondaria prima di poterne creare un join al gruppo di disponibilità.  
  
 **Nomi delle colonne:** group_id, group_database_id, database_name  
  
 [sys.databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
 Contiene una riga per ogni database nell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Se un database appartiene a una replica di disponibilità, la riga per quel database contiene il GUID della replica e l'identificatore univoco del database all'interno del gruppo di disponibilità.  
  
 **[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] :** replica_id, group_database_id  
  
 [sys.dm_hadr_auto_page_repair](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-auto-page-repair-transact-sql.md)  
 Restituisce una riga per ogni tentativo di correzione automatica della pagina in qualsiasi database di disponibilità in una replica di disponibilità ospitata per qualsiasi gruppo di disponibilità dall'istanza del server. Questa vista contiene le righe degli ultimi tentativi automatici di correzione automatica della pagina in un database primario o secondario, con un massimo di 100 righe per database. Non appena un database raggiunge il limite massimo, la riga per il tentativo successivo di correzione automatica della pagina sostituisce una delle voci esistenti.  
  
 **Nomi delle colonne:** database_id, file_id, page_id, error_type, page_status, modification_time  
  
 [sys.dm_hadr_database_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)  
 Restituisce una riga per ogni database che partecipa a un qualsiasi gruppo di disponibilità per il quale l'istanza locale di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ospita una replica di disponibilità.  
  
 **:** database_id, group_id, replica_id, group_database_id, is_local, synchronization_state, synchronization_state_desc, is_commit_participant, synchronization_health, synchronization_health_desc, database_state, database_state_desc, is_suspended, suspend_reason, suspend_reason_desc, recovery_lsn, truncation_lsn, last_sent_lsn, last_sent_time, last_received_lsn, last_received_time, last_hardened_lsn, last_hardened_time, last_redone_lsn, last_redone_time, log_send_queue_size, log_send_rate, redo_queue_size, redo_rate, filestream_send_rate, end_of_log_lsn, last_commit_lsn, last_commit_time, low_water_mark_for_ghosts  
  
 [sys.dm_hadr_database_replica_cluster_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md)  
 Restituisce una riga contenente informazioni relative all'integrità dei database nei database di disponibilità in ciascun gruppo di disponibilità nel cluster WSFC (Windows Server Failover Clustering). Questa DMV è utile per la pianificazione o la risposta a un failover o per comprendere quale replica secondaria di un gruppo di disponibilità trattiene il troncamento del log su un determinato database primario.  
  
 **Nomi delle colonne:** replica_id, group_database_id, database_name, is_failover_ready, is_pending_secondary_suspend, is_database_joined, recovery_lsn, truncation_lsn  
  
> [!NOTE]  
>  Il percorso della replica primaria è l'origine autorevole per un gruppo di disponibilità.  
  
> [!NOTE]  
>  Per informazioni sui contatori delle prestazioni di [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] per i database di disponibilità (oggetto prestazioni **SQLServer:Database Replica** ), vedere [SQL Server, replica di database](../../../relational-databases/performance-monitor/sql-server-database-replica.md). Inoltre, per monitorare l'attività del log delle transazioni sui database di disponibilità, usare i seguenti contatori dell'oggetto prestazione **SQLServer:Databases**: **Ora di scrittura scaricamento log (ms)** , **Scaricamenti log/sec**, **Mancati riscontri cache del pool di log/sec**, **Letture disco del pool di log/sec** e **Richieste del pool di log/sec**. Per altre informazioni, vedere [SQL Server, Databases Object](../../../relational-databases/performance-monitor/sql-server-databases-object.md).  
  
##  <a name="monitoring-availability-group-listeners"></a><a name="AGlisteners"></a> Monitoraggio dei listener del gruppo di disponibilità  
 Per monitorare i listener del gruppo di disponibilità sulle subnet del cluster WSFC, utilizzare le viste seguenti:  
  
 [sys.availability_group_listener_ip_addresses](../../../relational-databases/system-catalog-views/sys-availability-group-listener-ip-addresses-transact-sql.md)  
 Restituisce una riga per ogni indirizzo IP virtuale conforme attualmente online per un listener del gruppo di disponibilità.  
  
 **Nomi delle colonne:** listener_id, ip_address, ip_subnet_mask, is_dhcp, network_subnet_ip, network_subnet_prefix_length, network_subnet_ipv4_mask, state, state_desc  
  
 [sys.availability_group_listeners](../../../relational-databases/system-catalog-views/sys-availability-group-listeners-transact-sql.md)  
 Per un determinato gruppo di disponibilità, restituisce zero righe, cosa che indica che nessun nome di rete è associato al gruppo di disponibilità, oppure restituisce una riga per ogni configurazione del listener del gruppo di disponibilità nel cluster WSFC (Windows Server Failover Clustering).  
  
 **Nomi delle colonne:** group_id, listener_id, dns_name, port, is_conformant, ip_configuration_string_from_cluster  
  
 [sys.dm_tcp_listener_states](../../../relational-databases/system-dynamic-management-views/sys-dm-tcp-listener-states-transact-sql.md)  
 Restituisce una riga contenente informazioni sullo stato dinamico per ogni listener TCP.  
  
 **Nomi delle colonne:** listener_id, ip_address, is_ipv4, port, type, type_desc, state, state_desc, start_time  
  
 **Chiave primaria:** listener_id  
  
 Per informazioni sui listener dei gruppi di disponibilità, vedere [Listener del gruppo di disponibilità, connettività client e failover dell'applicazione &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
 **Attività di monitoraggio dei gruppi di disponibilità Always On:**  
  
-   [Usare Dettagli Esplora oggetti per monitorare i gruppi di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-object-explorer-details-to-monitor-availability-groups.md)  
  
-   [Visualizzazione delle Proprietà dei gruppi di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-group-properties-sql-server.md)  
  
-   [Visualizzazione delle proprietà della replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-replica-properties-sql-server.md)  
  
-   [Visualizzare le proprietà del listener del gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-group-listener-properties-sql-server.md)  
  
 **Riferimento relativo al monitoraggio dei gruppi di disponibilità Always On (Transact-SQL):**  
  
-   [SERVERPROPERTY &#40;Transact-SQL&#41;](../../../t-sql/functions/serverproperty-transact-sql.md)  
  
-   [sys.availability_group_listener_ip_addresses &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-group-listener-ip-addresses-transact-sql.md)  
  
-   [sys.availability_group_listeners &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-group-listeners-transact-sql.md)  
  
-   [sys.availability_databases_cluster &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-databases-cluster-transact-sql.md)  
  
-   [sys.availability_groups &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)  
  
-   [sys.availability_read_only_routing_lists &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-read-only-routing-lists-transact-sql.md)  
  
-   [sys.availability_replicas &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)  
  
-   [sys.dm_hadr_availability_replica_cluster_nodes &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-nodes-transact-sql.md)  
  
-   [sys.dm_hadr_availability_replica_cluster_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md)  
  
-   [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)  
  
-   [sys.dm_hadr_auto_page_repair &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-auto-page-repair-transact-sql.md)  
  
-   [sys.dm_hadr_availability_group_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md)  
  
-   [sys.dm_hadr_availability_replica_cluster_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md)  
  
-   [sys.dm_hadr_availability_replica_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md)  
  
-   [sys.dm_hadr_database_replica_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)  
  
-   [sys.dm_hadr_database_replica_cluster_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md)  
  
-   [sys.dm_hadr_cluster &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-transact-sql.md)  
  
-   [sys.dm_hadr_cluster_members &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md)  
  
-   [sys.dm_hadr_cluster_networks &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-networks-transact-sql.md)  
  
-   [sys.dm_hadr_database_replica_cluster_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md)  
  
-   [sys.dm_hadr_database_replica_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)  
  
-   [sys.dm_hadr_instance_node_map &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-instance-node-map-transact-sql.md)  
  
-   [sys.dm_hadr_name_id_map &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-name-id-map-transact-sql.md)  
  
-   [sys.dm_os_performance_counters &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)  
  
-   [sys.dm_tcp_listener_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-tcp-listener-states-transact-sql.md)  
  
-   [sys.fn_hadr_backup_is_preferred_replica &#40;Transact-SQL&#41;](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md)  
  
 **Contatori delle prestazioni Always On:**  
  
-   [SQL Server, replica di disponibilità](../../../relational-databases/performance-monitor/sql-server-availability-replica.md)  
  
-   [Replica di database di SQL Server](../../../relational-databases/performance-monitor/sql-server-database-replica.md)  
  
-   [SQL Server, Databases Object](../../../relational-databases/performance-monitor/sql-server-databases-object.md)  
  
 **Gestione basata su criteri per gruppi di disponibilità Always On**  
  
-   [Utilizzare i criteri AlwaysOn per visualizzare l'integrità di un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/use-always-on-policies-to-view-the-health-of-an-availability-group-sql-server.md)  
  
-   [Usare il dashboard Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Monitoraggio di Gruppi di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)  
  
  
