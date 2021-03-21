---
description: sys.dm_exec_background_job_queue (Transact-SQL)
title: sys.dm_exec_background_job_queue (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_background_job_queue
- sys.dm_exec_background_job_queue_TSQL
- dm_exec_background_job_queue_TSQL
- sys.dm_exec_background_job_queue
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_background_job_queue dynamic management function
ms.assetid: 05d9884f-b74c-4e3c-a23b-c90c1ea5ef02
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 90c3d1c6c607c795b7405fd51caadec8f090cf6d
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753771"
---
# <a name="sysdm_exec_background_job_queue-transact-sql"></a>sys.dm_exec_background_job_queue (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce una riga per ogni processo di Query Processor pianificato per l'esecuzione asincrona (in background).  
  
> **NOTA** Per chiamare questo **[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]** oggetto da o **[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]** , usare il nome **sys.dm_pdw_nodes_exec_background_job_queue**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**time_queued**|**datetime**|Ora in cui il processo viene aggiunto alla coda.|  
|**job_id**|**int**|Identificatore di processo.|  
|**database_id**|**int**|Database in cui il processo viene eseguito.|  
|**object_id1**|**int**|Il valore dipende dal tipo di processo. Per altre informazioni, vedere la sezione Osservazioni.|  
|**object_id2**|**int**|Il valore dipende dal tipo di processo. Per altre informazioni, vedere la sezione Osservazioni.|  
|**object_id3**|**int**|Il valore dipende dal tipo di processo. Per altre informazioni, vedere la sezione Osservazioni.|  
|**object_id4**|**int**|Il valore dipende dal tipo di processo. Per altre informazioni, vedere la sezione Osservazioni.|  
|**error_code**|**int**|Codice di errore nel caso di reinserimento del processo a causa di un errore. È NULL in caso di processo sospeso, non prelevato o completato.|  
|**request_type**|**smallint**|Tipo di richiesta del processo.|  
|**retry_count**|**smallint**|Numero di volte che il processo è stato prelevato dalla coda e reinserito nella coda per mancanza di risorse o altri motivi.|  
|**in_progress**|**smallint**|Indica se è stata avviata l'esecuzione del processo.<br /><br /> 1 = avviato<br /><br /> 0 = Processo in attesa di avvio|  
|**session_id**|**smallint**|Identificatore di sessione.|  
|**pdw_node_id**|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
  
## <a name="permissions"></a>Autorizzazioni

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, è necessario l'account [amministratore del server](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o l'account [amministratore Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   
  
## <a name="remarks"></a>Commenti  
 In questa vista vengono restituite solo le informazioni relative ai processi asincroni di aggiornamento delle statistiche. Per ulteriori informazioni sulle statistiche di aggiornamento asincrone, vedere [statistiche](../../relational-databases/statistics/statistics.md).  
  
 I valori di **object_id1** tramite **object_id4** dipendono dal tipo della richiesta di processo. Nella tabella seguente viene descritto il significato delle colonne per i diversi tipi di processo.  
  
|Tipo di richiesta|object_id1|object_id2|object_id3|object_id4|  
|------------------|-----------------|-----------------|-----------------|-----------------|  
|Aggiornamenti asincroni delle statistiche|ID di tabella o vista|ID delle statistiche|Non usato|Non usato|  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene restituito il numero di processi asincroni attivi nella coda in background per ogni database nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```  
SELECT DB_NAME(database_id) AS [Database], COUNT(*) AS [Active Async Jobs]  
FROM sys.dm_exec_background_job_queue  
WHERE in_progress = 1  
GROUP BY database_id;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funzioni e viste a gestione dinamica relative all'esecuzione &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Statistiche](../../relational-databases/statistics/statistics.md)   
 [KILL STATS JOB &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-stats-job-transact-sql.md)  
  
