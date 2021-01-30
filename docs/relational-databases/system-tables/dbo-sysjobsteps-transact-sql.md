---
description: dbo.sysjobsteps (Transact-SQL)
title: dbo.sysJobSteps (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dbo.sysjobsteps
- dbo.sysjobsteps_TSQL
- sysjobsteps
- sysjobsteps_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysjobsteps system table
ms.assetid: 978b8205-535b-461c-91f3-af9b08eca467
author: cawrites
ms.author: chadam
ms.openlocfilehash: d4e06b76f069086a6223ba02969c15e5cc1c7908
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202319"
---
# <a name="dbosysjobsteps-transact-sql"></a>dbo.sysjobsteps (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene informazioni relative a tutti i passaggi di un processo da eseguire tramite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Questa tabella è archiviata nel database **msdb** .  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**job_id**|**uniqueidentifier**|ID del processo.|  
|**step_id**|**int**|ID del passaggio del processo.|  
|**step_name**|**sysname**|Nome del passaggio del processo.|  
|**sottosistema**|**nvarchar(40)**|Nome del sottosistema utilizzato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent per eseguire il passaggio del processo|  
|**command**|**nvarchar(max)**|Comando che deve essere eseguito dal **sottosistema**.|  
|**flags**|**int**|Riservato.|  
|**additional_parameters**|**ntext**|Riservato.|  
|**cmdexec_success_code**|**int**|Valore a livello di errore restituito dai passaggi del sottosistema **CmdExec** per indicare l'esito positivo.|  
|**on_success_action**|**tinyint**|Azione da eseguire quando un passaggio viene eseguito correttamente.<br /><br /> **1** = (impostazione predefinita) termina con esito positivo<br /><br /> **2** = Esci con errore<br /><br /> **3** = Vai al passaggio successivo<br /><br /> **4** = Vai al passaggio _on_success_step_id_|
|**on_success_step_id**|**int**|ID del passaggio successivo da eseguire quando un passaggio viene eseguito correttamente.|  
|**on_fail_action**|**tinyint**|Azione da eseguire quando un passaggio non viene eseguito correttamente.<br /><br /> **1** = Esci con esito positivo<br /><br /> **2** = (impostazione predefinita) termina con errore<br /><br /> **3** = Vai al passaggio successivo<br /><br /> **4** = Vai al passaggio _on_fail_step_id_|
|**on_fail_step_id**|**int**|ID del passaggio successivo da eseguire quando un passaggio non viene eseguito correttamente.|  
|**server**|**sysname**|Riservato.|  
|**database_name**|**sysname**|Nome del database in cui viene eseguito il **comando** se **sottosistema** è TSQL.|  
|**database_user_name**|**sysname**|Nome dell'utente del database di cui viene utilizzato l'account quando si esegue il passaggio.|  
|**retry_attempts**|**int**|Numero di tentativi in caso di esecuzione errata del passaggio.|  
|**retry_interval**|**int**|Periodo di attesa tra un tentativo e il successivo.|  
|**os_run_priority**|**int**|Riservato.|  
|**output_file_name**|**nvarchar(200)**|Nome del file in cui viene salvato l'output del passaggio quando il **sottosistema** è TSQL, PowerShell o **CmdExec**_._|  
|**last_run_outcome**|**int**|Risultato dell'esecuzione precedente del passaggio del processo.<br /><br /> **0** = non riuscito<br /><br /> **1** = operazione completata<br /><br /> **2** = nuovo tentativo<br /><br /> **3** = annullato<br /><br /> **5** = sconosciuto|  
|**last_run_duration**|**int**|Durata (hhmmss) dell'ultima esecuzione del passaggio.|  
|**last_run_retries**|**int**|Numero di tentativi durante l'ultima esecuzione del passaggio del processo.|  
|**last_run_date**|**int**|Data (yyyymmdd) di inizio dell'ultima esecuzione del passaggio.|  
|**last_run_time**|**int**|Ora (hhmmss) di inizio dell'ultima esecuzione del passaggio.|  
|**proxy_id**|**int**|Proxy per il passaggio del processo.|  
|**step_uid**|**uniqueidentifier**|Identificatore per il passaggio del processo.|  
  
## <a name="see-also"></a>Vedere anche  
 [SQL Server Agent tabelle &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/sql-server-agent-tables-transact-sql.md)  
  
  
