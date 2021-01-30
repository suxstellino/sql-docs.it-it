---
description: sys.availability_groups_cluster (Transact-SQL)
title: sys.availability_groups_cluster (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.availability_groups_cluster
- availability_groups_cluster
- availability_groups_cluster_TSQL
- sys.availability_groups_cluster_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.availability_groups_cluster catalog view
- Availability Groups [SQL Server], WSFC clusters
ms.assetid: d0f4683f-cdf0-4227-8b68-720ffe58f158
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e483e88fad533c05e741bf6bcff0d5b6e6b0dcba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186160"
---
# <a name="sysavailability_groups_cluster-transact-sql"></a>sys.availability_groups_cluster (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce una riga per ogni gruppo di disponibilità Always On in WSFC (Windows Server Failover Clustering). Ogni riga contiene i metadati dei gruppi di disponibilità del cluster WSFC.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**group_id**|**uniqueidentifier**|Identificatore univoco (GUID) del gruppo di disponibilità.|  
|**nome**|**sysname**|Nome del gruppo di disponibilità. Si tratta di un nome specificato dall'utente che deve essere univoco all'interno del cluster di failover di Windows Server (WSFC).|  
|**resource_id**|**nvarchar(40)**|ID della risorsa del cluster WSFC.|  
|**resource_group_id**|**nvarchar(40)**|ID del gruppo di risorse del cluster WSFC del gruppo di disponibilità.|  
|**failure_condition_level**|**int**|Livello della condizione di errore definito dall'utente al di sotto del quale deve essere attivato un failover automatico. Sono disponibili i valori interi seguenti:<br /><br /> 1: specifica che deve essere avviato un failover automatico quando si verifica una delle condizioni seguenti: <br />-Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servizio è inattivo.<br />-Il lease del gruppo di disponibilità per la connessione al cluster di failover WSFC scade perché non è stato ricevuto alcun ACK dall'istanza del server. Per altre informazioni, vedere [How It Works: SQL Server Always On Lease Timeout](https://techcommunity.microsoft.com/t5/sql-server-support/how-it-works-sql-server-alwayson-lease-timeout/ba-p/317268) (Funzionamento: timeout lease di SQL Server Always On).<br /><br /> 2: specifica che deve essere avviato un failover automatico quando si verifica una delle condizioni seguenti:  <br />-L'istanza di non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si connette al cluster e viene superata la soglia **HEALTH_CHECK_TIMEOUT** specificata dall'utente del gruppo di disponibilità. <br />-La replica di disponibilità è in stato di errore. <br />3: specifica che deve essere avviato un failover automatico in caso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] errori interni critici, ad esempio spinlock orfani, gravi violazioni dell'accesso in scrittura o dump troppo elevato. Si tratta del valore predefinito. <br />4: specifica che deve essere avviato un failover automatico in caso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] errori interni moderati, ad esempio una condizione persistente di memoria insufficiente nel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pool di risorse interno.<br />5: specifica che deve essere avviato un failover automatico in caso di qualsiasi condizione di errore qualificata, tra cui:<br />-Esaurimento dei thread di lavoro del motore SQL. <br />-Rilevamento di un deadlock irrisolvibile.<br /><br /> I livelli delle condizioni di errore (1-5) vanno dal livello 1, meno restrittivo, al livello 5, più restrittivo. Un livello della condizione specifico include tutti i livelli meno restrittivi. Il livello della condizione più restrittivo, ovvero il livello 5, include pertanto i quattro livelli della condizione meno restrittivi (1-4), il livello 4 include i livelli 1-3 e così via.<br /><br /> Per modificare questo valore, utilizzare l'opzione FAILURE_CONDITION_LEVEL dell'istruzione [ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] .|  
|**health_check_timeout**|**int**|Tempo di attesa (in millisecondi) per cui il sistema [sp_server_diagnostics](../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) stored procedure restituire le informazioni sull'integrità del server, prima che venga presupposto che l'istanza del server sia lenta o non risponda. Il valore predefinito è 30000 millisecondi (30 secondi).<br /><br /> Per modificare questo valore, utilizzare l'opzione HEALTH_CHECK_TIMEOUT dell'istruzione [ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] .|  
|**automated_backup_preference**|**tinyint**|Percorso preferito per l'esecuzione di backup nei database di disponibilità del gruppo di disponibilità. Uno dei valori seguenti:<br /><br /> 0: primario. I backup devono essere sempre eseguiti sulla replica primaria.<br />1: solo secondaria. È preferibile eseguire i backup in una replica secondaria.<br />2: Preferisco secondario. È preferibile eseguire i backup in una replica secondaria, ma nel caso in cui non sia disponibile alcuna replica secondaria per le operazioni di backup, è possibile eseguire i backup nella replica primaria. Comportamento predefinito.<br />3: qualsiasi replica. Nessuna preferenza sull'utilizzo della replica primaria o di una replica secondaria per l'esecuzione dei backup.<br /><br /> Per altre informazioni, vedere [Repliche secondarie attive: Backup su repliche secondarie &#40;Gruppi di disponibilità Always On&#41;](../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md).|  
|**automated_backup_preference_desc**|**nvarchar(60)**|Descrizione di **AUTOMATED_BACKUP_PREFERENCE**, uno di:<br /><br /> PRIMARY<br /><br /> SECONDARY_ONLY<br /><br /> SECONDARY<br /><br /> NONE|  
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW ANY DEFINITION nell'istanza del server.  
  
## <a name="see-also"></a>Vedere anche  
 [sys.availability_replicas &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)   
 [Gruppi di disponibilità Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Monitorare i gruppi di disponibilità &#40;&#41;Transact-SQL ](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Monitorare Gruppi di disponibilità &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
  
