---
title: Informazioni di riferimento sugli oggetti di sistema per i gruppi di disponibilità
description: Riferimento per i vari oggetti di sistema che possono essere usati quando si lavora con i gruppi di disponibilità Always On.
ms.custom: seo-lt-2019
ms.date: 04/03/2010
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: reference
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 340fcfa12f3432a09821132f44e8d58672be8198
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97640972"
---
# <a name="always-on-availability-group-system-object-reference"></a>Informazioni di riferimento sugli oggetti di sistema per i gruppi di disponibilità Always On

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]
    
Questo argomento funge da pagina di riferimento per tutti i vari oggetti di sistema che possono essere usati quando si lavora con i gruppi di disponibilità Always On. 

## <a name="system-catalog-views"></a>Viste del catalogo di sistema

| Vista del catalogo di sistema | Descrizione|
| :------ | :----------------------------- |
| [sys.availability_databases_cluster](../../../relational-databases/system-catalog-views/sys-availability-databases-cluster-transact-sql.md)   | Contiene una riga per ogni database di disponibilità sull'istanza di SQL Server che ospita una replica di disponibilità per qualsiasi gruppo di disponibilità Always On nel cluster WSFC (Windows Server Failover Clustering), indipendentemente dal fatto che il database della copia locale sia già stato aggiunto o meno al gruppo di disponibilità. |
| [sys.availability_group_listener_ip_addresses](../../../relational-databases/system-catalog-views/sys-availability-group-listener-ip-addresses-transact-sql.md)  | Restituisce una riga per ogni indirizzo IP associato a un listener del gruppo di disponibilità Always On nel cluster WSFC (Windows Server Failover Clustering). |
| [sys.availability_group_listeners](../../../relational-databases/system-catalog-views/sys-availability-group-listeners-transact-sql.md)    | Per ogni gruppo di disponibilità Always On, restituisce zero righe, che indica che nessun nome di rete è associato al gruppo di disponibilità, o restituisce una riga per ogni configurazione del listener del gruppo di disponibilità nel cluster WSFC (Windows Server Failover Clustering).  |
| [sys.availability_groups](../../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)   | Restituisce una riga per ogni gruppo di disponibilità per il quale l'istanza locale di SQL Server ospita una replica di disponibilità. Ogni riga contiene una copia memorizzata nella cache dei metadati del gruppo di disponibilità. |
| [sys.availability_groups_cluster](../../../relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md)    | Restituisce una riga per ogni gruppo di disponibilità Always On in WSFC (Windows Server Failover Clustering). Ogni riga contiene i metadati dei gruppi di disponibilità del cluster WSFC. |
| [sys.availability_read_only_routing_lists](../../../relational-databases/system-catalog-views/sys-availability-read-only-routing-lists-transact-sql.md)    | Restituisce una riga per l'elenco di routing di sola lettura di ogni replica di disponibilità in un gruppo di disponibilità AlwaysOn nel cluster di failover WSFC. |
| [sys.availability_replicas](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)    | Restituisce una riga per ognuna delle repliche di disponibilità che appartiene a un gruppo di disponibilità Always On nel cluster di failover WSFC. |
| &nbsp; | &nbsp; |

## <a name="system-dynamic-management-views"></a>Viste a gestione dinamica (DMV) di sistema


| Vista a gestione dinamica (DMV) di sistema | Descrizione|
| :------ | :----------------------------- |
| [sys.dm_hadr_auto_page_repair](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-auto-page-repair-transact-sql.md)   | Restituisce una riga per ogni tentativo di correzione automatica della pagina in qualsiasi database di disponibilità in una replica di disponibilità ospitata per qualsiasi gruppo di disponibilità dall'istanza del server.  |
| [sys.dm_hadr_availability_group_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md)    | Restituisce una riga per ogni gruppo di disponibilità Always On che dispone di una replica di disponibilità sull'istanza locale di SQL Server. Ogni riga visualizza gli stati che definiscono l'integrità di un determinato gruppo di disponibilità. |
| [sys.dm_hadr_availability_replica_cluster_nodes](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-nodes-transact-sql.md)     | Restituisce una riga per ogni replica di disponibilità, indipendentemente dallo stato di join, dei gruppi di disponibilità Always On nel cluster WSFC (Windows Server Failover Clustering). |
| [sys.dm_hadr_availability_replica_cluster_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md)     | Restituisce una riga per ogni replica di disponibilità Always On, indipendentemente dallo stato del join, di tutti i gruppi di disponibilità Always On, indipendentemente dal percorso della replica, nel cluster WSFC (Windows Server Failover Clustering). |
| [sys.dm_hadr_availability_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md)    | Restituisce una riga per ogni replica locale e una riga per ogni replica remota nello stesso gruppo di disponibilità Always On di una replica locale. Ogni riga contiene informazioni sullo stato di una determinata replica. |
| [sys.dm_hadr_cluster](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-transact-sql.md)    | Restituisce una riga che espone il nome del cluster e le informazioni sul quorum. |
| [sys.dm_hadr_cluster_members](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md)    | Restituisce una riga per ogni membro che costituisce il quorum e lo stato di ognuno. |
| [sys.dm_hadr_cluster_networks](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-networks-transact-sql.md)    | Restituisce una riga per ogni membro del cluster WSFC che partecipa alla configurazione della subnet di un gruppo di disponibilità.  |
| [sys.dm_hadr_database_replica_cluster_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md)     | Restituisce una riga contenente informazioni relative all'integrità dei database di disponibilità nei gruppi di disponibilità Always On nel cluster WSFC (Windows Server Failover Clustering).  |
| [sys.dm_hadr_database_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)    | Restituisce una riga per ogni database che partecipa a un gruppo di disponibilità Always On per il quale l'istanza locale di SQL Server ospita una replica di disponibilità. |
| [sys.dm_hadr_instance_node_map](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-instance-node-map-transact-sql.md)    | Per ogni istanza di SQL Server che ospita una replica di disponibilità unita in join al gruppo di disponibilità Always On, restituisce il nome del nodo WSFC (Windows Server Failover Clustering) che ospita l'istanza del server. |
| [sys.dm_hadr_name_id_map](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-name-id-map-transact-sql.md)    | Mostra il mapping dei gruppi di disponibilità Always On dove per l'istanza corrente di SQL Server è stato creato un join a tre ID univoci: un ID gruppo di disponibilità, un ID risorsa WSFC e un ID gruppo WSFC. |
| [sys.dm_tcp_listener_states](../../../relational-databases/system-dynamic-management-views/sys-dm-tcp-listener-states-transact-sql.md)    | Restituisce una riga contenente informazioni sullo stato dinamico per ogni listener TCP. |
| &nbsp; | &nbsp; |

## <a name="system-functions"></a>Funzioni di sistema


| Funzioni di sistema | Descrizione|
| :------ | :----------------------------- |
| [sys.fn_hadr_is_primary_replica](../../../relational-databases/system-functions/sys-fn-hadr-is-primary-replica-transact-sql.md)  | Utilizzato per determinare se la replica corrente è la replica primaria. |
| [sys.fn_hadr_backup_is_preferred_replica](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md)    | Utilizzato per determinare se la replica corrente è la replica di backup preferita. |
| [sys.fn_hadr_distributed_ag_replica](../../../relational-databases/system-functions/sys-fn-hadr-distributed-ag-replica-transact-sql.md)    | Usata per eseguire il mapping di una replica in un gruppo di disponibilità distribuito al gruppo di disponibilità locale. |
| &nbsp; | &nbsp; |


  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di Gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   

  
