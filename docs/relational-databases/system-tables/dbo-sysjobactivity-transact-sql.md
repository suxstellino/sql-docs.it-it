---
description: dbo.sysjobactivity (Transact-SQL)
title: dbo.sysjobactivity (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/05/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dbo.sysjobactivity_TSQL
- dbo.sysjobactivity
- sysjobactivity
- sysjobactivity_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysjobactivity system table
ms.assetid: fd17cac9-5d1f-4b44-b2dc-ee9346d8bf1e
author: cawrites
ms.author: chadam
ms.openlocfilehash: c375367cdca6b7005d0c74f90654b7a6911070ee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207028"
---
# <a name="dbosysjobactivity-transact-sql"></a>dbo.sysjobactivity (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Registra l'attività e lo stato del processo corrente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  Questa tabella è archiviata nel database **msdb** .
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|ID della sessione archiviata nella tabella **syssessions** del database **msdb** .|  
|**job_id**|**uniqueidentifier**|ID del processo.|  
|**run_requested_date**|**datetime**|Data e ora della richiesta di esecuzione del processo.|  
|**run_requested_source**|**sysname(nvarchar(128))**|Autore della richiesta di esecuzione del processo.<br /><br /> **1** = SOURCE_SCHEDULER<br /><br /> **2** = SOURCE_ALERTER<br /><br /> **3** = SOURCE_BOOT<br /><br /> **4** = SOURCE_USER<br /><br /> **6** = SOURCE_ON_IDLE_SCHEDULE|  
|**queued_date**|**datetime**|Data e ora di accodamento del processo. Se il processo viene eseguito direttamente, questa colonna è NULL.|  
|**start_execution_date**|**datetime**|Data e ora in cui è pianificata l'esecuzione del processo.|  
|**last_executed_step_id**|**int**|ID dell'ultimo passaggio del processo eseguito.|  
|**last_executed_step_**<br /><br /> **date**|**datetime**|Data e ora di inizio dell'esecuzione dell'ultimo passaggio del processo.|  
|**stop_execution_date**|**datetime**|Data e ora in cui l'esecuzione del processo è stata completata.|  
|**job_history_id**|**int**|Utilizzato per identificare una riga nella tabella **sysjobhistory** .|  
|**next_scheduled_run_date**|**datetime**|Data e ora in cui è stata pianificata l'esecuzione successiva del processo.|  

## <a name="example"></a>Esempio
Questo esempio restituirà lo stato della fase di esecuzione per tutti i processi di SQL Server Agent.  Eseguire il seguente [!INCLUDE[tsql](../../includes/tsql-md.md)] in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].
```sql
SELECT sj.Name, 
    CASE
        WHEN sja.start_execution_date IS NULL THEN 'Not running'
        WHEN sja.start_execution_date IS NOT NULL AND sja.stop_execution_date IS NULL THEN 'Running'
        WHEN sja.start_execution_date IS NOT NULL AND sja.stop_execution_date IS NOT NULL THEN 'Not running'
    END AS 'RunStatus'
FROM msdb.dbo.sysjobs sj
JOIN msdb.dbo.sysjobactivity sja
ON sj.job_id = sja.job_id
WHERE session_id = (
    SELECT MAX(session_id) FROM msdb.dbo.sysjobactivity); 
```
  
## <a name="see-also"></a>Vedere anche  
 [dbo.sysjobhistory &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/dbo-sysjobhistory-transact-sql.md)  
  
  
