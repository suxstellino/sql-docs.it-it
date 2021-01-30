---
description: sys.pdw_loader_backup_runs (Transact-SQL)
title: sys.pdw_loader_backup_runs (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 2b72034c-6a11-46b9-a76c-7a88b2bea360
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 12fcde5ded873393a6273a43772b86376f5df607
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208363"
---
# <a name="syspdw_loader_backup_runs-transact-sql"></a>sys.pdw_loader_backup_runs (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Contiene informazioni sulle operazioni di backup e ripristino in corso e completate in [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] e sulle operazioni di backup, ripristino e caricamento in corso e completate in [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] . Le informazioni vengono mantenute tra un riavvio di sistema e l'altro.  
  
|Nome colonna|Tipo di dati|Descrizione|Range|  
|-----------------|---------------|-----------------|-----------|  
|run_id|**int**|Identificatore univoco per una specifica esecuzione di backup, ripristino o caricamento.<br /><br /> Chiave per questa visualizzazione.||  
|name|**nvarchar(255)**|Null per Load. Nome facoltativo per il backup o il ripristino.||  
|submit_time|**datetime**|Ora di invio della richiesta.||  
|start_time|**datetime**|Ora di avvio dell'operazione.||  
|end_time|**datetime**|Ora di completamento dell'operazione, esito negativo o annullato.||  
|total_elapsed_time|**int**|Tempo totale trascorso tra start_time e l'ora corrente oppure tra start_time e end_time per le esecuzioni completate, annullate o non riuscite.|Se total_elapsed_time supera il valore massimo per un numero intero (24,8 giorni in millisecondi), si verificherà un errore di materializzazione causato da un overflow.<br /><br /> Il valore massimo in millisecondi equivale a 24,8 giorni.|  
|operation_type|**nvarchar (16)**|Tipo di carico.|' BACKUP ',' LOAD ',' RESTORE '|  
|mode|**nvarchar (16)**|Modalità all'interno del tipo di esecuzione.|Per operation_type = **backup**<br />**DIFFERENTIAL**<br />**FULL**<br /><br /> Per operation_type = **Load**<br />**AGGIUNGERE**<br />**RICARICARE**<br />**UPSERT**<br /><br /> Per operation_type = **Restore**<br />**DATABASE**<br />**HEADER_ONLY**|  
|database_name|**nvarchar(255)**|Nome del database che rappresenta il contesto di questa operazione||  
|table_name|**nvarchar(255)**|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]||  
|Principal_id|**int**|ID dell'utente che ha richiesto l'operazione.||  
|session_id|**nvarchar(32)**|ID della sessione che esegue l'operazione.|Vedere session_id in [sys.dm_pdw_exec_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-sessions-transact-sql.md).|  
|request_id|**nvarchar(32)**|ID della richiesta che esegue l'operazione. Per i caricamenti, questa è la richiesta corrente o più recente associata a questo carico.|Vedere request_id in [sys.dm_pdw_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md).|  
|status|**nvarchar (16)**|Stato dell'esecuzione.|' CANCELLED ',' COMPLETED ',' FAILED ',' QUEUED ',' RUNNING '|  
|progress|**int**|Percentuale completata.|Da 0 a 100|  
|.|**nvarchar(4000)**|Testo completo del comando inviato dall'utente.|Verrà troncato se è più lungo di 4000 caratteri (conteggio di spazi).|  
|rows_processed|**bigint**|Numero di righe elaborate come parte di questa operazione.||  
|rows_rejected|**bigint**|Numero di righe rifiutate come parte di questa operazione.||  
|rows_inserted|**bigint**|Numero di righe inserite nella tabella o nelle tabelle di database come parte di questa operazione.||  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo di Azure Synapse Analytics e Parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  
