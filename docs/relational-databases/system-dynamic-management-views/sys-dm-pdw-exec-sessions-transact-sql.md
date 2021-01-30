---
description: sys.dm_pdw_exec_sessions (Transact-SQL)
title: sys.dm_pdw_exec_sessions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/22/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 31c262b3-7e4d-44c4-af71-aaef0fd1a980
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 829eb5d53fc4e60c7c975a75a2e3159885b85171
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99142450"
---
# <a name="sysdm_pdw_exec_sessions-transact-sql"></a>sys.dm_pdw_exec_sessions (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Include informazioni su tutte le sessioni attualmente o aperte di recente nell'appliance. Elenca una riga per sessione.  
  
|Nome colonna|Tipo di dati|Descrizione|Range|  
|-----------------|---------------|-----------------|-----------|  
|session_id|**nvarchar(32)**|ID della query corrente o dell'ultima esecuzione della query (se la sessione viene TERMINAta e la query è stata eseguita al momento della terminazione). Chiave per questa visualizzazione.|Univoco in tutte le sessioni del sistema.|  
|status|**nvarchar (10)**|Per le sessioni correnti, indica se la sessione è attualmente attiva o inattiva. Per le sessioni passate è possibile che lo stato della sessione venga visualizzato chiuso o terminato (se la sessione è stata chiusa forzatamente).|' ACTIVE ',' CLOSED ',' IDLE ',' TERMINATE '|  
|request_id|**nvarchar(32)**|ID della query corrente o dell'ultima esecuzione della query.|Univoco tra tutte le richieste nel sistema. Null se non ne è stato eseguito nessuno.|  
|security_id|**varbinary(85)**|ID di sicurezza dell'entità che esegue la sessione.||  
|login_name|**nvarchar(128)**|Nome dell'account di accesso dell'entità che esegue la sessione.|Qualsiasi stringa conforme alle convenzioni di denominazione degli utenti.|  
|login_time|**datetime**|Data e ora in cui l'utente ha effettuato l'accesso e questa sessione è stata creata.|**DateTime** valido prima dell'ora corrente.|  
|query_count|**int**|Acquisisce il numero di query o la sessione requeststhis è stata eseguita dopo la creazione.|Maggiore o uguale a 0.|  
|is_transactional|**bit**|Acquisisce se una sessione è attualmente all'interno di una transazione.|0 per il commit automatico, 1 per transazionale.|  
|client_id|**nvarchar(255)**|Acquisisce le informazioni client per la sessione.|Qualsiasi stringa valida.|  
|app_name|**nvarchar(255)**|Acquisisce le informazioni sul nome dell'applicazione facoltativamente impostate come parte del processo di connessione.|Qualsiasi stringa valida.|  
|sql_spid|**int**|Numero ID dello SPID. Usare la `session_id` sessione. Utilizzare la `sql_spid` colonna per aggiungere **sys.dm_pdw_nodes_exec_sessions**.<br /><br /> Avviso questa colonna contiene SPID chiusi. **\* \* \* \***||  
  
 Per informazioni sul numero massimo di righe mantenute da questa visualizzazione, vedere la sezione metadati nell'argomento [limiti di capacità](/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits#metadata) .  
  
## <a name="permissions"></a>Autorizzazioni  
 È necessaria l'autorizzazione `VIEW SERVER STATE`.  
  
## <a name="see-also"></a>Vedere anche  
 [Analisi delle sinapsi di Azure e DMV Parallel data warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
