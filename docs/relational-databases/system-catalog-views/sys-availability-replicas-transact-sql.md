---
description: sys.availability_replicas (Transact-SQL)
title: sys.availability_replicas (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- availability_replicas_TSQL
- availability_replicas
- sys.availability_replicas
- sys.availability_replicas_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.availability_replicas catalog view
ms.assetid: 0a06e9b6-a1e4-4293-867b-5c3f5a8ff62c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: b78d5e65fe9f6c60e94ddbf241c9eabfed332f0c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338921"
---
# <a name="sysavailability_replicas-transact-sql"></a>sys.availability_replicas (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Restituisce una riga per ognuna delle repliche di disponibilità che appartiene a un gruppo di disponibilità Always On nel cluster di failover WSFC.  
  
Se l'istanza del server locale non è in grado di comunicare con il cluster di failover WSFC, ad esempio perché il cluster non è attivo o perché è stato perso il quorum, vengono restituite solo le righe delle repliche di disponibilità locali. Tali righe conterranno solo le colonne di dati memorizzate nella cache dei metadati in locale.  
  
 
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**replica_id**|**uniqueidentifier**|ID univoco della replica.|  
|**group_id**|**uniqueidentifier**|ID univoco del gruppo di disponibilità a cui appartiene la replica.|  
|**replica_metadata_id**|**int**|ID dell'oggetto di metadati locale per le repliche di disponibilità nel motore di database.|  
|**replica_server_name**|**nvarchar(256)**|Nome del server dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che ospita la replica corrente e, per un'istanza non predefinita, il nome dell'istanza.|  
|**owner_sid**|**varbinary(85)**|ID di sicurezza (SID) registrato nell'istanza del server per il proprietario esterno della replica di disponibilità.<br /><br /> NULL per le repliche di disponibilità non locali.|  
|**endpoint_url**|**nvarchar(128)**|Rappresentazione di stringa dell'endpoint del mirroring di database specificato dall'utente usato dalle connessioni tra repliche primarie e secondarie per la sincronizzazione dei dati. Per informazioni sulla sintassi degli URL dell'endpoint, vedere [Specificare l'URL dell'endpoint quando si aggiunge o si modifica una replica di disponibilità &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md).<br /><br /> NULL = Impossibile comunicare con il cluster di failover WSFC.<br /><br /> Per modificare questo endpoint, utilizzare l'opzione ENDPOINT_URL dell'istruzione [ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] .|  
|**availability_mode**|**tinyint**|Modalità di disponibilità della replica. I valori possibili sono:<br /><br /> 0 &#124; commit asincrono. La replica primaria può eseguire il commit delle transazioni senza attendere che la replica secondaria salvi il log su disco.<br /><br /> 1 &#124; commit sincrono. La replica primaria attende che la replica secondaria salvi la transazione su disco prima di eseguirne il commit.<br /><br />4 &#124; solo la configurazione. La replica primaria invia i metadati di configurazione del gruppo di disponibilità alla replica in modo sincrono. I dati utente non vengono trasmessi alla replica. Disponibile in SQL Server 2017 CU1 e versioni successive.<br /><br /> Per altre informazioni, vedere [Modalità di disponibilità &#40;gruppi di disponibilità Always On&#41;](../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md).|  
|**availability_mode_desc**|**nvarchar(60)**|Descrizione della **\_ modalità di disponibilità**, tra le seguenti:<br /><br /> \_commit asincrono<br /><br /> COMMIT SINCRONo \_<br /><br /> \_solo configurazione<br /><br /> Per modificare la modalità di disponibilità di una replica di disponibilità, utilizzare l'opzione AVAILABILITY_MODE dell'istruzione [ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] .<br/><br>Non è possibile modificare la modalità di disponibilità di una replica in \_ solo configurazione. Non è possibile modificare una \_ replica solo di configurazione in una replica secondaria o primaria. |  
|**modalità di failover \_**|**tinyint**|[Modalità di failover](../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md) della replica di disponibilità, tra le seguenti:<br /><br /> 0 &#124; il failover automatico. La replica è una destinazione potenziale per i failover automatici.  Il failover automatico è supportato solo se la modalità di disponibilità è impostata su commit sincrono (**\_ modalità di disponibilità** = 1) e la replica di disponibilità è attualmente sincronizzata.<br /><br /> 1 &#124; failover manuale. Un failover in una replica secondaria impostata sul failover manuale deve essere avviato manualmente dall'amministratore del database. Il tipo di failover eseguito dipenderà dalla sincronizzazione della replica secondaria, come segue:<br /><br /> Se la replica di disponibilità non è sincronizzata o è ancora in fase di sincronizzazione, è possibile eseguire solo il failover forzato (con la possibile perdita di dati).<br /><br /> Se la modalità di disponibilità è impostata su commit sincrono (**\_ modalità di disponibilità** = 1) e la replica di disponibilità è attualmente sincronizzata, è possibile che si verifichi un failover manuale senza perdita di dati.<br /><br /> Per visualizzare un rollup dello stato di sincronizzazione del database di ogni database di disponibilità in una replica di disponibilità, utilizzare le **colonne \_ \_ desc** **\_ Health** and Synchronization Health della sincronizzazione della [sys.dm_hadr_availability_replica_states](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md) vista a gestione dinamica. Tramite il rollup vengono presi in considerazione lo stato di sincronizzazione di ogni database di disponibilità e la modalità di disponibilità della relativa replica di disponibilità.<br /><br /> **Nota:** Per visualizzare l'integrità della sincronizzazione di un determinato database di disponibilità, eseguire una query sullo **\_ stato di sincronizzazione** e le colonne di **\_ integrità della sincronizzazione** della [sys.dm_hadr_database_replica_states](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) vista a gestione dinamica.|  
|**\_desc modalità di failover \_**|**nvarchar(60)**|Descrizione della **\_ modalità di failover**, uno dei seguenti:<br /><br /> MANUAL<br /><br /> AUTOMATIC<br /><br /> Per modificare la modalità di failover, utilizzare l' \_ opzione modalità di failover dell'istruzione [ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] .|  
|**\_timeout sessione**|**int**|Periodo di timeout in secondi. Il periodo di timeout è il tempo di attesa massimo rispettato dalla replica per la ricezione di un messaggio da un'altra replica, prima di considerare la connessione tra la replica primaria e secondaria non riuscita. Il timeout della sessione rileva se le repliche secondarie sono connesse alla replica primaria.<br /><br /> Quando rileva una connessione non riuscita con una replica secondaria, la replica primaria considera la replica secondaria non \_ sincronizzata. Se viene rilevata una connessione non riuscita con una replica primaria, una replica secondaria tenta di riconnettersi.<br /><br /> **Nota:** I timeout della sessione non provocano failover automatici.<br /><br /> Per modificare questo valore, utilizzare l'opzione SESSION_TIMEOUT dell'istruzione [ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] .|  
|**il \_ ruolo primario \_ consente le \_ connessioni**|**tinyint**|Specifica se la disponibilità consente tutte le connessioni o solo connessioni di lettura e scrittura. I valori possibili sono:<br /><br /> 2 = Tutte (impostazione predefinita)<br /><br /> 3 = lettura e scrittura|  
|**il \_ ruolo primario \_ consente le \_ connessioni \_ DESC**|**nvarchar(60)**|La descrizione **del \_ ruolo primario \_ consente le \_ connessioni**, tra le seguenti:<br /><br /> ALL<br /><br /> LETTURA/ \_ scrittura|  
|**il \_ ruolo secondario \_ consente le \_ connessioni**|**tinyint**|Specifica se una replica di disponibilità che esegue il ruolo secondario, ovvero una replica secondaria, può accettare connessioni dai client. I valori possibili sono:<br /><br /> 0 = No. Non è consentita alcuna connessione ai database nella replica secondaria e i database non sono disponibili per l'accesso in lettura. Si tratta dell'impostazione predefinita.<br /><br /> 1 = Sola lettura. Sono consentite solo le connessioni di sola lettura ai database nella replica secondaria. Tutti i database nella replica sono disponibili per l'accesso in lettura.<br /><br /> 2 = Tutte. Sono consentite tutte le connessioni ai database nella replica secondaria per l'accesso in sola lettura.<br /><br /> Per altre informazioni, vedere [Repliche secondarie attive: Repliche secondarie leggibili &#40;Gruppi di disponibilità Always On&#41;](../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).|  
|**secondary_role_allow_connections_desc**|**nvarchar(60)**|Descrizione di **secondary_role_allow_connections**, uno di:<br /><br /> NO<br /><br /> READ_ONLY<br /><br /> ALL|  
|**create_date**|**datetime**|Data di creazione della replica.<br /><br /> NULL = La replica non risiede nell'istanza del server.|  
|**modify_date**|**datetime**|Data dell'ultima modifica apportata alla replica.<br /><br /> NULL = La replica non risiede nell'istanza del server.|  
|**backup_priority**|**int**|Rappresenta la priorità specificata dall'utente per l'esecuzione dei backup nella replica rispetto alle altre repliche nello stesso gruppo di disponibilità. Il valore è un numero intero compreso nell'intervallo 0-100.<br /><br /> Per altre informazioni, vedere [Repliche secondarie attive: Backup su repliche secondarie &#40;Gruppi di disponibilità Always On&#41;](../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md).|  
|**read_only_routing_url**|**nvarchar(256)**|Endpoint di connettività (URL) della replica di disponibilità di sola lettura. Per altre informazioni, vedere [Configurare il routing di sola lettura per un gruppo di disponibilità &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md).|  
|**seeding_mode**|**tinyint**|Uno dei valori possibili: </br></br> 0: automatico </br></br> 1: manuale|
|**seeding_mode_desc**|**nvarchar(60)**|Viene descritta la modalità di seeding. </br></br> AUTOMATIC </br></br>MANUAL|
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW ANY DEFINITION nell'istanza del server.  
  
## <a name="see-also"></a>Vedere anche  
 [sys.availability_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)   
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Gruppi di disponibilità Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Monitorare i gruppi di disponibilità &#40;&#41;Transact-SQL ](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Monitorare Gruppi di disponibilità &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
  
