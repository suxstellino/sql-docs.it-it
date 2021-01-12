---
description: syscollector_execution_log_full (Transact-SQL)
title: syscollector_execution_log_full (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- syscollector_execution_log_full
- syscollector_execution_log_full_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- data collector view
- syscollector_execution_log_full view
ms.assetid: 6c8db22d-2e4c-4b7c-ac5a-8762ef1b175b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ddb6f45318528fd88b112453f3f6f6b0aa456b3d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094242"
---
# <a name="syscollector_execution_log_full-transact-sql"></a>syscollector_execution_log_full (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Fornisce informazioni su un set o un pacchetto di raccolte quando il log di esecuzione è completo.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|log_id|**bigint**|Identifica ogni esecuzione del set di raccolta. Utilizzato per unire questa vista agli altri log dettagliati. Ammette i valori Null.|  
|parent_log_id|**bigint**|Identifica il pacchetto o il set di raccolta padre. Non ammette i valori Null. Gli ID sono concatenati nella relazione padre-figlio che consente di determinare il pacchetto avviato e il set di raccolta che lo ha avviato. Questa vista raggruppa le voci di log dal collegamento padre-figlio. I nomi dei pacchetti sono visualizzati con un rientro in maniera tale che la sequenza delle chiamate sia chiaramente visibile.|  
|name|**nvarchar(4000)**|Nome del set di raccolta o del pacchetto rappresentato da questa voce di log. Ammette i valori Null.|  
|status|**smallint**|Indica lo stato corrente del set di raccolta o del pacchetto. Ammette i valori Null.<br /><br /> I valori possibili sono:<br /><br /> 0 = in fase di esecuzione<br /><br /> 1 = completato<br /><br /> 2 = non riuscita|  
|runtime_execution_mode|**smallint**|Indica se l'attività del set di raccolta era raccogliere o caricare dati. Ammette i valori Null.|  
|start_time|**datetime**|Ora di avvio del set di raccolta o del pacchetto. Ammette i valori Null.|  
|last_iteration_time|**datetime**|Per i pacchetti in esecuzione continua, l'ultimo orario in cui il pacchetto ha acquisito uno snapshot. Ammette i valori Null.|  
|finish_time|**datetime**|Ora in cui è stata completata l'esecuzione per i pacchetti e i set di raccolta completati. Ammette i valori Null.|  
|duration|**int**|Tempo di esecuzione del pacchetto o del set di raccolta, espresso in secondi. Ammette i valori Null.|  
|failure_message|**nvarchar(2048)**|Se l'esecuzione di un set di raccolta o di un pacchetto non è riuscita, il messaggio di errore più recente per il componente. Ammette i valori Null. Per ottenere informazioni più dettagliate sull'errore, utilizzare la [fn_syscollector_get_execution_details &#40;funzione&#41;Transact-SQL ](../../relational-databases/system-functions/fn-syscollector-get-execution-details-transact-sql.md) .|  
|Operatore|**nvarchar(128)**|Identifica chi ha avviato il pacchetto o il set di raccolta. Ammette i valori Null.|  
|package_execution_id|**uniqueidentifier**|Fornisce un collegamento alla tabella di log [!INCLUDE[ssIS](../../includes/ssis-md.md)]. Ammette i valori Null.|  
|collection_set_id|**int**|Fornisce un collegamento alla tabella di configurazione della raccolta dati in msdb. Ammette i valori Null.|  
  
## <a name="permissions"></a>Autorizzazioni  
 Richiede SELECT per **dc_operator**.  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure dell'agente di raccolta dati &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [Viste dell'agente di raccolta dati &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/data-collector-views-transact-sql.md)   
 [Raccolta dati](../../relational-databases/data-collection/data-collection.md)  
  
  
