---
title: DMV e viste del catalogo di sistema per i gruppi di disponibilità
description: Una raccolta di DMV e viste del catalogo che consente di monitorare e diagnosticare l'integrità di un gruppo di disponibilità Always On.
ms.custom: seo-lt-2019
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: troubleshooting
ms.assetid: 4320a4a4-6183-462b-8bda-e7424e7cb706
author: rothja
ms.author: jroth
ms.openlocfilehash: a2a69aa016ae258375c6095bbf38e58ddd75b981
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643592"
---
# <a name="dynamic-management-views-and-system-catalog-views-always-on-availability-groups"></a>DMV e viste del catalogo di sistema (Gruppi di disponibilità Always On)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Questo argomento illustra alcune query comuni sulle DMV Always On che è possibile usare per monitorare e risolvere i problemi relativi ai gruppi di disponibilità.  
  
> [!TIP]  
>  Nel dashboard Always On è possibile configurare facilmente l'interfaccia utente grafica (GUI) per visualizzare molte delle DMV disponibili per le repliche di disponibilità e i database di disponibilità. Fare clic con il pulsante destro del mouse sull'intestazione di tabella corrispondente e selezionare la DMV che si vuole visualizzare o nascondere.  
  
 Per altre informazioni sulle DMV del gruppo di disponibilità, vedere [Funzioni e DMV di Gruppi di disponibilità AlwaysOn &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md). Per altre informazioni sulle viste del catalogo dei gruppi di disponibilità, vedere [Viste del catalogo di Gruppi di disponibilità AlwaysOn &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md).  
  
## <a name="check-the-wsfc-cluster-node-configuration"></a>Controllare la configurazione del nodo del cluster WSFC  
 La query Transact-SQL (T-SQL) seguente recupera lo stato di tutti i nodi nel cluster WSFC (Windows Server Failover Clustering ) corrente.  
  
```sql  
use master  
go  
select * from sys.dm_hadr_cluster_members  
go  
```  
  
 Questo set di risultati segnala lo stato di ogni nodo membro del cluster WSFC corrente. Se il quorum viene definito come **Maggioranza dei nodi e delle condivisioni file**, viene segnalata anche la condivisione file. È possibile visualizzare lo stato di ogni nodo, incluso il peso di votazione di ogni nodo (valore [number_of_quorum_votes](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md)).  
  
## <a name="explore-the-cluster-network"></a>Esplorare la rete del cluster  
 La query seguente recupera la configurazione di rete del cluster WSFC corrente.  
  
```sql  
select * from sys.dm_hadr_cluster_networks  
```  
  
 Il set di risultati contiene una riga per ogni scheda di rete nel cluster WSFC. Ad esempio, in un cluster a due nodi che contiene due schede di rete in ogni nodo, questa query restituisce quattro righe.  
  
## <a name="explore-the-availability-groups"></a>Esplorare i gruppi di disponibilità  
 La query seguente recupera informazioni su un gruppo di disponibilità.  
  
```sql  
select primary_replica, primary_recovery_health_desc, synchronization_health_desc from sys.dm_hadr_availability_group_states  
go  
select * from sys.availability_groups  
go  
select * from sys.availability_groups_cluster  
go  
```  
  
 Tutte le DMV [sys.dm_hadr_availability_group_states &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md), [sys.availability_groups &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md), e [sys.availability_groups_cluster](~/relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md) restituiscono informazioni sui gruppi di disponibilità del cluster WSFC corrente. In realtà sembra che [availability_groups &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md) e [sys.availability_groups_cluster](~/relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md) restituiscano informazioni identiche.  
  
 La DMV [sys.availability_groups_cluster](~/relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md) segnala invece i metadati del gruppo di disponibilità archiviati nel cluster WSFC, mentre la DMV [sys.availability_groups &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md) specifica i metadati del gruppo di disponibilità memorizzati nella cache dello spazio del processo di SQL Server. Queste due DMV segnalano anche informazioni sulla configurazione, mentre [sys.dm_hadr_availability_group_states &#40;Transact-SQL&#41; ](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md) indica gli stati di integrità correnti dei gruppi di disponibilità.  
  
> [!IMPORTANT]  
>  La nomenclatura riporta le DMV che documentano le repliche e i database di disponibilità.  
  
## <a name="explore-the-availability-replicas"></a>Esplorare le repliche di disponibilità  
 La query seguente recupera le informazioni sulle repliche di disponibilità definite nei gruppi di disponibilità.  
  
```sql  
select replica_id, role_desc, connected_state_desc, synchronization_health_desc from sys.dm_hadr_availability_replica_states  
go  
select replica_server_name, replica_id, availability_mode_desc, endpoint_url from sys.availability_replicas  
go  
select replica_server_name, join_state_desc from sys.dm_hadr_availability_replica_cluster_states  
go  
```  
  
 Analogamente alle DMV del gruppo di disponibilità, esistono tre DMV che segnalano informazioni sulle repliche di disponibilità. La DMV [sys.dm_hadr_availability_replica_states](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md) segnala informazioni sulle repliche di disponibilità memorizzate in locale nella cache di SQL Server, mentre la DMV [sys.dm_hadr_availability_replica_cluster_states](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md) segnala informazioni di stato relative alle repliche di disponibilità del cluster WSFC. Infine, la DMV [availability_replicas](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md) segnala i dati di configurazione relativi alle repliche di disponibilità, che vengono memorizzati in locale nella cache di SQL Server.  
  
## <a name="explore-availability-replica-health"></a>Esplorare l'integrità della replica di disponibilità  
 La query seguente recupera le informazioni sull'integrità corrente delle repliche di disponibilità.  
  
```sql  
select replica_id, role_desc, recovery_health_desc, synchronization_health_desc from sys.dm_hadr_availability_replica_states  
go  
```  
  
 Confrontare i risultati della query nella replica primaria e nella replica secondaria e notare che nella replica secondaria le informazioni di stato vengono segnalate solo per la replica specifica e non per qualsiasi altra replica nel gruppo di disponibilità.  
  
## <a name="explore-the-availability-databases"></a>Esplorare i database di disponibilità  
 La query seguente recupera le informazioni sulle repliche di disponibilità definite nel gruppo di disponibilità. È possibile osservare la variazione nei risultati della query prima e dopo aver sospeso lo spostamento dei dati in un database di disponibilità.  
  
```sql
select * from sys.availability_databases_cluster  
go  
select group_database_id, database_name, is_failover_ready  from sys.dm_hadr_database_replica_cluster_states  
go  
select database_id, synchronization_state_desc, synchronization_health_desc, last_hardened_lsn, redo_queue_size, log_send_queue_size from sys.dm_hadr_database_replica_states  
go  
```  
  
 Anche in questo caso, sono disponibili tre DMV Always On che segnalano informazioni sui database di disponibilità. La DMV [sys. availability_databases_cluster](~/relational-databases/system-catalog-views/sys-availability-databases-cluster-transact-sql.md) segnala informazioni di configurazione sui database di disponibilità del cluster WSFC. La DMV [sys.dm_hadr_database_replica_cluster_states](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md) segnala informazioni di stato sulle repliche di database, che vengono memorizzate in locale nella cache di SQL Server. Contiene informazioni importanti sullo stato, ad esempio la conformità di failover della replica di disponibilità. Infine, la DMV [sys.dm hadr_database_replica_states](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md), ossia un set di risultati molto dettagliato che segnala informazioni sull'identità e sullo stato di ogni database di disponibilità, ad esempio informazioni sullo stato LSN per i log delle repliche del database primario e secondario.  
  
## <a name="explore-availability-database-health"></a>Esplorare l'integrità del database di disponibilità  
 La query seguente recupera informazioni sull'integrità di ogni database di disponibilità nelle repliche. È possibile osservare la variazione nei risultati della query prima e dopo aver sospeso lo spostamento dei dati in un database di disponibilità.  
  
```sql  
select dc.database_name, dr.database_id, dr.synchronization_state_desc,   
dr.suspend_reason_desc, dr.synchronization_health_desc  
from sys.dm_hadr_database_replica_states dr  join sys.availability_databases_cluster dc  
on dr.group_database_id=dc.group_database_id   
where is_local=1  
go  
```  
  
  
