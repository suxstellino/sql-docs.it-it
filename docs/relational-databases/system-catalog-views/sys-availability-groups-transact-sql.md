---
description: sys.availability_groups (Transact-SQL)
title: sys.availability_groups (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.availability_groups_TSQL
- availability_groups_TSQL
- sys.availability_groups
- availability_groups
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.availability_groups catalog view
ms.assetid: da7fa55f-c008-45d9-bcfc-3513b02d9e71
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: cc784f8bbd811ad50f2fcb1b64bd3f5bdf8cd2d6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338917"
---
# <a name="sysavailability_groups-transact-sql"></a>sys.availability_groups (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce una riga per ogni gruppo di disponibilità per il quale l'istanza locale di SQL Server ospita una replica di disponibilità. Ogni riga contiene una copia memorizzata nella cache dei metadati del gruppo di disponibilità.  
   
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**group_id**|**uniqueidentifier**|Identificatore univoco (GUID) del gruppo di disponibilità.|  
|**nome**|**sysname**|Nome del gruppo di disponibilità. Si tratta di un nome specificato dall'utente che deve essere univoco all'interno del cluster di failover di Windows Server (WSFC).|  
|**resource_id**|**nvarchar(40)**|ID della risorsa del cluster WSFC.|  
|**resource_group_id**|**nvarchar(40)**|ID del gruppo di risorse del cluster WSFC del gruppo di disponibilità.|  
|**failure_condition_level**|**int**|Livello di condizione di errore definito dall'utente in cui deve essere attivato un failover automatico, uno dei valori integer indicati nella tabella immediatamente sotto la tabella.<br /><br /> I livelli delle condizioni di errore (1-5) vanno dal livello 1, meno restrittivo, al livello 5, più restrittivo. Un livello della condizione specifico include tutti i livelli meno restrittivi. Il livello della condizione più restrittivo, ovvero il livello 5, include pertanto i quattro livelli della condizione meno restrittivi (1-4), il livello 4 include i livelli 1-3 e così via.<br /><br /> Per modificare questo valore, utilizzare l'opzione FAILURE_CONDITION_LEVEL dell'istruzione [ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] .|  
|**health_check_timeout**|**int**|Tempo di attesa (in millisecondi) per cui il sistema [sp_server_diagnostics](../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) stored procedure restituire le informazioni sull'integrità del server, prima che venga presupposto che l'istanza del server sia lenta o non risponda. Il valore predefinito è 30000 millisecondi (30 secondi).<br /><br /> Per modificare questo valore, utilizzare l'opzione HEALTH_CHECK_TIMEOUT dell'istruzione [ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] .|  
|**automated_backup_preference**|**tinyint**|Percorso preferito per l'esecuzione di backup nei database di disponibilità del gruppo di disponibilità. Di seguito sono riportati i valori possibili e le relative descrizioni.<br /><br /> <br /><br /> 0: primario. I backup devono essere sempre eseguiti sulla replica primaria.<br /><br /> 1: solo secondaria. È preferibile eseguire i backup in una replica secondaria.<br /><br /> 2: Preferisco secondario. È preferibile eseguire i backup in una replica secondaria, ma nel caso in cui non sia disponibile alcuna replica secondaria per le operazioni di backup, è possibile eseguire i backup nella replica primaria. Comportamento predefinito.<br /><br /> 3: qualsiasi replica. Nessuna preferenza sull'utilizzo della replica primaria o di una replica secondaria per l'esecuzione dei backup.<br /><br /> <br /><br /> Per altre informazioni, vedere [Repliche secondarie attive: Backup su repliche secondarie &#40;Gruppi di disponibilità Always On&#41;](../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md).|  
|**automated_backup_preference_desc**|**nvarchar(60)**|Descrizione di **AUTOMATED_BACKUP_PREFERENCE**, uno di:<br /><br /> PRIMARY<br /><br /> SECONDARY_ONLY<br /><br /> SECONDARY<br /><br /> NONE|  
|**version**|**smallint**|Versione dei metadati del gruppo di disponibilità archiviata nel cluster di failover di Windows. Questo numero di versione viene incrementato quando vengono aggiunte nuove funzionalità.|  
|**basic_features**|**bit**|Specifica se si tratta di un gruppo di disponibilità di base. Per altre informazioni, vedere [Gruppi di disponibilità di base &#40;gruppi di disponibilità Always On&#41;](../../database-engine/availability-groups/windows/basic-availability-groups-always-on-availability-groups.md).|  
|**dtc_support**|**bit**|Specifica se il supporto DTC è stato abilitato per questo gruppo di disponibilità. L'opzione **DTC_SUPPORT** di **Crea gruppo di disponibilità** controlla questa impostazione.|  
|**db_failover**|**bit**|Specifica se il gruppo di disponibilità supporta il failover per le condizioni di integrità del database. L'opzione **DB_FAILOVER** di **Crea gruppo di disponibilità** controlla questa impostazione.|  
|**is_distributed**|**bit**|Specifica se si tratta di un gruppo di disponibilità distribuito. Per altre informazioni, vedere [Gruppi di disponibilità distribuiti &#40;gruppi di disponibilità Always On&#41;](../../database-engine/availability-groups/windows/distributed-availability-groups.md).|
|**cluster_type**|**tinyint**|0: cluster di failover di Windows Server <br/><br/>1: cluster esterno (ad esempio, pacemaker Linux)<br/><br/>2: nessuna|
|**cluster_type_desc**|**nvarchar(60)**|Descrizione testo del tipo di cluster|
|**required_synchronized_secondaries_to_commit**|**int**| Il numero di repliche secondarie che devono trovarsi in uno stato sincronizzato per il completamento di un commit|
|**sequence_number**|**bigint**|Identifica la sequenza di configurazione del gruppo di disponibilità. Aumenta in modo incrementale ogni volta che la replica primaria del gruppo di disponibilità aggiorna la configurazione del gruppo.|
|**is_contained**|**bit**|1: istanza master del cluster di Big Data configurata per la disponibilità elevata. <br/><br/> 0: tutti gli altri.|
  
## <a name="failure-condition-level--values"></a>Valori del livello di condizione di errore  
 Nella tabella seguente vengono descritti i possibili livelli di condizione di errore per la colonna **FAILURE_CONDITION_LEVEL** .  
  
|Valore|Condizione di errore|  
|-----------|-----------------------|  
|1|Specifica che deve essere avviato un failover automatico quando si verifica una delle condizioni seguenti:<br /><br /> <br /><br /> -Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servizio è inattivo.<br /><br /> -Il lease del gruppo di disponibilità per la connessione al cluster di failover WSFC scade perché non è stato ricevuto alcun ACK dall'istanza del server. Per altre informazioni, vedere [How It Works: SQL Server Always On Lease Timeout](https://techcommunity.microsoft.com/t5/sql-server-support/how-it-works-sql-server-alwayson-lease-timeout/ba-p/317268) (Funzionamento: timeout lease di SQL Server Always On).|  
|2|Specifica che deve essere avviato un failover automatico quando si verifica una delle condizioni seguenti:<br /><br /> <br /><br /> -L'istanza di non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si connette al cluster e viene superata la soglia **HEALTH_CHECK_TIMEOUT** specificata dall'utente del gruppo di disponibilità.<br /><br /> -La replica di disponibilità è in stato di errore.|  
|3|Specifica che deve essere avviato un failover automatico in caso di errori interni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] critici, ad esempio spinlock orfani, gravi violazioni dell'accesso in scrittura o dumping eccessivo.<br /><br /> Si tratta del valore predefinito.|  
|4|Specifica che deve essere avviato un failover automatico in caso di errori interni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con gravità moderata, ad esempio una condizione persistente di memoria insufficiente nel pool di risorse interno di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|5|Specifica che deve essere avviato un failover automatico in caso di qualsiasi condizione di errore qualificata, tra cui:<br /><br /> <br /><br /> -Esaurimento dei thread di lavoro del motore SQL.<br /><br /> -Rilevamento di un deadlock irrisolvibile.|  
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW ANY DEFINITION nell'istanza del server.  
  
## <a name="see-also"></a>Vedere anche  
 [sys.availability_replicas &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)   
 [Gruppi di disponibilità Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Monitorare i gruppi di disponibilità &#40;&#41;Transact-SQL ](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Monitorare Gruppi di disponibilità &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
