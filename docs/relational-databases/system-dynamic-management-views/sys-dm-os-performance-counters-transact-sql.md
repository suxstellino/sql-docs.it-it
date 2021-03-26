---
description: sys.dm_os_performance_counters (Transact-SQL)
title: sys.dm_os_performance_counters (Transact-SQL)
ms.custom: ''
ms.date: 03/22/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_performance_counters
- sys.dm_os_performance_counters_TSQL
- dm_os_performance_counters_TSQL
- sys.dm_os_performance_counters
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_performance_counters dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 62489b131eea77ed67b1207a7606056cea39066d
ms.sourcegitcommit: c242f423cc3b776c20268483cfab0f4be54460d4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/25/2021
ms.locfileid: "105551633"
---
# <a name="sysdm_os_performance_counters-transact-sql"></a>sys.dm_os_performance_counters (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce una riga per contatore delle prestazioni gestito dal server. Per informazioni su ogni contatore delle prestazioni, vedere [usare oggetti SQL Server](../../relational-databases/performance-monitor/use-sql-server-objects.md).  
  
> [!NOTE]  
>  Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome `sys.dm_pdw_nodes_os_performance_counters` .  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**object_name**|**nchar (128)**|Categoria di appartenenza del contatore.|  
|**counter_name**|**nchar (128)**|Nome del contatore. Per ottenere ulteriori informazioni su un contatore, questo è il nome dell'argomento da selezionare dall'elenco di contatori in [utilizzare SQL Server oggetti](../../relational-databases/performance-monitor/use-sql-server-objects.md). |  
|**instance_name**|**nchar (128)**|Nome dell'istanza specifica del contatore. Spesso include il nome del database.|  
|**cntr_value**|**bigint**|Valore corrente del contatore.<br /><br /> **Nota:** Per i contatori per secondo, questo valore è cumulativo. Il valore relativo alla frequenza deve essere calcolato tramite il campionamento del valore a intervalli di tempo discreti. La differenza tra due valori di campionamento successivi è uguale alla frequenza dell'intervallo di tempo utilizzato.|  
|**cntr_type**|**int**|Tipo di contatore definito dall'architettura di controllo delle prestazioni di Windows. Per ulteriori informazioni sui tipi di contatori delle prestazioni, vedere [tipi di contatori delle prestazioni WMI](/windows/desktop/WmiSdk/wmi-performance-counter-types) in docs o nella documentazione di Windows Server.|  
|**pdw_node_id**|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
  
## <a name="remarks"></a>Commenti  
 Se l'istanza dell'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è in grado di visualizzare i contatori delle prestazioni del sistema operativo Windows, utilizzare la query [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente per verificare se i contatori delle prestazioni sono stati disabilitati.  
  
```sql  
SELECT COUNT(*) FROM sys.dm_os_performance_counters;  
```  
  
Se il valore restituito è di 0 righe, i contatori delle prestazioni sono stati disabilitati. Esaminare quindi il registro di installazione e cercare l'errore 3409, `Reinstall sqlctr.ini for this instance, and ensure that the instance login account has correct registry permissions.` che indica che i contatori delle prestazioni non sono stati abilitati. Gli errori immediatamente precedenti all'errore 3409 dovrebbero indicare la causa principale per l'errore relativo all'abilitazione dei contatori delle prestazioni. Per ulteriori informazioni sui file di log del programma di installazione, vedere [visualizzare e leggere SQL Server file di log del programma di installazione](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  

I contatori delle prestazioni in cui il `cntr_type` valore della colonna è 65792 visualizzano uno snapshot dell'ultimo valore osservato, non una media. 

Contatori delle prestazioni in cui il `cntr_type` valore della colonna è 272696320 o 272696576 Visualizza il numero medio di operazioni completate durante ogni secondo dell'intervallo di campionamento. I contatori di questo tipo misurano il tempo in segni di graduazione dell'orologio di sistema. Ad esempio, per ottenere una lettura simile a uno snapshot dell'ultimo secondo solo per i `Buffer Manager:Lazy writes/sec` `Buffer Manager:Checkpoint pages/sec` contatori e, è necessario confrontare il delta tra due punti di raccolta che sono separati da un secondo.    

I contatori delle prestazioni `cntr_type` in cui il valore della colonna è 537003264 visualizzano il rapporto tra un subset e il relativo set come percentuale. Il contatore, ad esempio, `Buffer Manager:Buffer cache hit ratio` Confronta il numero totale di riscontri nella cache e il numero totale di ricerche nella cache. Di conseguenza, per ottenere una lettura simile a uno snapshot dell'ultimo secondo, è necessario confrontare il delta tra il valore corrente e il valore di base (denominatore) tra due punti di raccolta che sono separati da un secondo. Il valore di base corrispondente è il contatore delle prestazioni in `Buffer Manager:Buffer cache hit ratio base` cui il `cntr_type` valore della colonna è 1073939712.

I contatori delle prestazioni in cui il `cntr_type` valore della colonna è 1073874176 visualizzano il numero di elementi elaborati in media, come rapporto tra gli elementi elaborati e il numero di operazioni. I contatori, ad esempio, `Locks:Average Wait Time (ms)` confrontano le attese di blocco al secondo con le richieste di blocco al secondo, per visualizzare la quantità media di tempo di attesa (in millisecondi) per ogni richiesta di blocco che ha generato un'attesa. Di conseguenza, per ottenere una lettura simile a uno snapshot dell'ultimo secondo, è necessario confrontare il delta tra il valore corrente e il valore di base (denominatore) tra due punti di raccolta che sono separati da un secondo. Il valore di base corrispondente è il contatore delle prestazioni in `Locks:Average Wait Time Base` cui il `cntr_type` valore della colonna è 1073939712.

I dati nella `sys.dm_os_performance_counters` DMV non vengono mantenuti dopo il riavvio del motore di database. Utilizzare la `sqlserver_start_time` colonna [sys.dm_os_sys_info](sys-dm-os-sys-info-transact-sql.md) per individuare l'ultima ora di avvio del motore di database.   

## <a name="permission"></a>Autorizzazione

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, è necessario l'account [amministratore del server](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o l'account [amministratore Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   
 
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituiti tutti i contatori delle prestazioni che visualizzano i valori dei contatori dello snapshot.  
  
```sql  
SELECT object_name, counter_name, instance_name, cntr_value, cntr_type  
FROM sys.dm_os_performance_counters
WHERE cntr_type = 65792 OR cntr_type = 272696320 OR cntr_type = 537003264;  
```  
  
## <a name="see-also"></a>Vedere anche  
  [SQL Server viste a gestione dinamica relative al sistema operativo &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [sys.sysperfinfo &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-sysperfinfo-transact-sql.md)  
 [sys.dm_os_sys_info &#40;&#41;Transact-SQL ](sys-dm-os-sys-info-transact-sql.md)
