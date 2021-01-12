---
description: syscollector_execution_log (Transact-SQL)
title: syscollector_execution_log (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- syscollector_execution_log_TSQL
- syscollector_execution_log
dev_langs:
- TSQL
helpviewer_keywords:
- data collector view
- syscollector_execution_log view
ms.assetid: 11554d64-0426-42ce-b7ce-5591f67864d2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f6c9cc330c87debc0a80a9d282ea8c40c904c59c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094248"
---
# <a name="syscollector_execution_log-transact-sql"></a>syscollector_execution_log (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Fornisce informazioni provenienti dal log di esecuzione per un set di raccolta o un pacchetto.   
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|log_id|**bigint**|Identifica ogni esecuzione del set di raccolta. Utilizzato per unire questa vista agli altri log dettagliati. Non ammette i valori Null.|  
|parent_log_id|**bigint**|Identifica il pacchetto o il set di raccolta padre. Non ammette i valori Null. Gli ID sono concatenati nella relazione padre-figlio che consente di determinare il pacchetto avviato e il set di raccolta che lo ha avviato. Questa vista raggruppa le voci di log in base al collegamento padre-figlio. I nomi dei pacchetti sono visualizzati con un rientro in maniera tale che la sequenza delle chiamate sia chiaramente visibile.|  
|collection_set_id|**int**|Identifica il set di raccolta o il pacchetto rappresentato da questa voce di log. Non ammette i valori Null.|  
|collection_item_id|**int**|Identifica un elemento della raccolta. Ammette i valori Null.|  
|start_time|**datetime**|Ora di avvio del set di raccolta o del pacchetto. Non ammette i valori Null.|  
|last_iteration_time|**datetime**|Per i pacchetti in esecuzione continua, l'ultimo orario in cui il pacchetto ha acquisito uno snapshot. Ammette i valori Null.|  
|finish_time|**datetime**|Ora in cui è stata completata l'esecuzione per i pacchetti e i set di raccolta completati. Ammette i valori Null.|  
|runtime_execution_mode|**smallint**|Indica se l'attività del set di raccolta era raccogliere o caricare dati. Ammette i valori Null.<br /><br /> I valori possibili sono:<br /><br /> 0 = raccolta<br /><br /> 1 = Caricamento|  
|status|**smallint**|Indica lo stato corrente del set di raccolta o del pacchetto. Non ammette i valori Null.<br /><br /> I valori possibili sono:<br /><br /> 0 = in fase di esecuzione<br /><br /> 1 = completato<br /><br /> 2 = non riuscita|  
|Operatore|**nvarchar(128)**|Identifica chi ha avviato il pacchetto o il set di raccolta. Non ammette i valori Null.|  
|package_id|**uniqueidentifier**|Identifica il set di raccolta o il pacchetto che ha generato questo log. Ammette i valori Null.|  
|package_name|**nvarchar(4000)**|Nome del pacchetto che ha generato il log. Ammette i valori Null.|  
|package_execution_id|**uniqueidentifier**|Fornisce un collegamento alla tabella di log [!INCLUDE[ssIS](../../includes/ssis-md.md)]. Ammette i valori Null.|  
|failure_message|**nvarchar(2048)**|Se l'esecuzione di un set di raccolta o di un pacchetto non è riuscita, il messaggio di errore più recente per il componente. Ammette i valori Null. Per ottenere informazioni più dettagliate sull'errore, utilizzare la [fn_syscollector_get_execution_details &#40;funzione&#41;Transact-SQL ](../../relational-databases/system-functions/fn-syscollector-get-execution-details-transact-sql.md) .|  
  
## <a name="permissions"></a>Autorizzazioni  
 Richiede SELECT per dc_operator.  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure dell'agente di raccolta dati &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [Viste dell'agente di raccolta dati &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/data-collector-views-transact-sql.md)   
 [Raccolta dati](../../relational-databases/data-collection/data-collection.md)  
  
  
