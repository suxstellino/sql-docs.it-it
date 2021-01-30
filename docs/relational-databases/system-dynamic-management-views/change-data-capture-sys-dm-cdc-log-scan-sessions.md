---
description: Change Data Capture-sys.dm_cdc_log_scan_sessions
title: sys.dm_cdc_log_scan_sessions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_cdc_log_scan_sessions
- dm_cdc_log_scan_sessions_TSQL
- sys.dm_cdc_log_scan_sessions_TSQL
- sys.dm_cdc_log_scan_sessions
dev_langs:
- TSQL
helpviewer_keywords:
- change data capture [SQL Server], log scan reporting
- sys.dm_cdc_log_scan_sessions dynamic management view
ms.assetid: d337e9d0-78b1-4a07-8820-2027d0b9f87c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e8f69ed4f8b1ca90bc6de468b4136c6c36687cfc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99148194"
---
# <a name="change-data-capture---sysdm_cdc_log_scan_sessions"></a>Change Data Capture-sys.dm_cdc_log_scan_sessions
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce una riga per ogni sessione di analisi dei log nel database corrente. L'ultima riga restituita rappresenta la sessione corrente. È possibile utilizzare questa vista per restituire le informazioni sullo stato relative alla sessione di analisi dei log corrente o informazioni aggregate relative a tutte le sessioni dall'ultimo avvio dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
   
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|ID della sessione.<br /><br /> 0 = i dati restituiti in questa riga costituiscono un'aggregazione di tutte le sessioni dall'ultimo avvio dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**start_time**|**datetime**|Ora di inizio della sessione.<br /><br /> Quando **session_id** = 0, inizia la raccolta di dati aggregati dell'ora.|  
|**end_time**|**datetime**|Ora di fine della sessione.<br /><br /> NULL = la sessione è attiva.<br /><br /> Quando **session_id** = 0, l'ora di fine dell'ultima sessione.|  
|**duration**|**bigint**|Durata della sessione espressa in secondi.<br /><br /> 0 = la sessione non contiene transazioni di acquisizione dei dati delle modifiche.<br /><br /> Quando **session_id** = 0, somma della durata (in secondi) di tutte le sessioni con Change Data Capture transazioni.|  
|**scan_phase**|**nvarchar(200)**|La fase corrente della sessione. Di seguito sono riportati i valori possibili e le relative descrizioni:<br /><br /> 1: lettura della configurazione<br />2: prima analisi, compilazione della tabella hash<br />3: seconda analisi<br />4: seconda analisi<br />5: seconda analisi<br />6: controllo delle versioni dello schema<br />7: ultima analisi<br />8: operazione eseguita<br /><br /> Quando **session_id** = 0, questo valore è sempre "aggregate".|  
|**error_count**|**int**|Numero di errori.<br /><br /> Quando **session_id** = 0, il numero totale di errori in tutte le sessioni.|  
|**start_lsn**|**nvarchar (23)**|Avvio di LSN per la sessione.<br /><br /> Quando **session_id** = 0, il numero LSN iniziale per l'ultima sessione.|  
|**current_lsn**|**nvarchar (23)**|LSN corrente in corso di analisi.<br /><br /> Quando **session_id** = 0, l'LSN corrente è 0.|  
|**end_lsn**|**nvarchar (23)**|Numero LSN finale per la sessione.<br /><br /> NULL = la sessione è attiva.<br /><br /> Quando **session_id** = 0, il numero LSN finale per l'ultima sessione.|  
|**tran_count**|**bigint**|Numero di transazioni di acquisizione dei dati delle modifiche elaborate. Questo contatore viene popolato nella fase 2.<br /><br /> Quando **session_id** = 0, il numero di transazioni elaborate in tutte le sessioni.|  
|**last_commit_lsn**|**nvarchar (23)**|LSN dell'ultimo record di log del commit elaborato.<br /><br /> Quando **session_id** = 0, l'ultimo LSN del record di log del commit per qualsiasi sessione.|  
|**last_commit_time**|**datetime**|Ora di elaborazione dell'ultimo record di log del commit.<br /><br /> Quando **session_id** = 0, l'ora dell'ultimo record di log del commit per qualsiasi sessione.|  
|**log_record_count**|**bigint**|Numero dei record di log analizzati.<br /><br /> Quando **session_id** = 0, numero di record analizzati per tutte le sessioni.|  
|**schema_change_count**|**int**|Numero di operazioni DDL (Data Definition Language) rilevate. Questo contatore viene popolato durante la fase 6.<br /><br /> Quando **session_id** = 0, il numero di operazioni DDL elaborate in tutte le sessioni.|  
|**command_count**|**bigint**|Numero di comandi elaborati.<br /><br /> Quando **session_id** = 0, il numero di comandi elaborati in tutte le sessioni.|  
|**first_begin_cdc_lsn**|**nvarchar (23)**|Primo numero LSN contenente transazioni di acquisizione dei dati delle modifiche.<br /><br /> Quando **session_id** = 0, il primo LSN che conteneva Change Data Capture transazioni.|  
|**last_commit_cdc_lsn**|**nvarchar (23)**|Numero LSN dell'ultimo record di log del commit contenente transazioni di acquisizione dei dati delle modifiche.<br /><br /> Quando **session_id** = 0, l'ultimo LSN del record di log del commit per qualsiasi sessione che conteneva transazioni Change Data Capture|  
|**last_commit_cdc_time**|**datetime**|Ora di elaborazione dell'ultimo record di log del commit contenente transazioni di acquisizione dei dati delle modifiche.<br /><br /> Quando **session_id** = 0, l'ora dell'ultimo record di log del commit per qualsiasi sessione che conteneva transazioni Change Data Capture.|  
|**latenza**|**int**|Differenza, in secondi, tra **end_time** e **last_commit_cdc_time** nella sessione. Questo contatore viene popolato al termine della fase 7.<br /><br /> Quando **session_id** = 0, l'ultimo valore di latenza diverso da zero registrato da una sessione.|  
|**empty_scan_count**|**int**|Numero di sessioni consecutive che non contengono transazioni di acquisizione dei dati delle modifiche.|  
|**failed_sessions_count**|**int**|Numero di sessioni non riuscite.|  
  
## <a name="remarks"></a>Commenti  
 I valori in questa vista a gestione dinamica vengono reimpostati ogni volta che l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene avviata.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW DATABASE STATE per eseguire query sulla vista a gestione dinamica **sys.dm_cdc_log_scan_sessions** . Per ulteriori informazioni sulle autorizzazioni per le viste a gestione dinamica, vedere [funzioni e viste a gestione dinamica &#40;&#41;Transact-SQL ](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituite informazioni relative alla sessione più recente.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT session_id, start_time, end_time, duration, scan_phase,  
    error_count, start_lsn, current_lsn, end_lsn, tran_count,  
    last_commit_lsn, last_commit_time, log_record_count, schema_change_count,  
    command_count, first_begin_cdc_lsn, last_commit_cdc_lsn,   
    last_commit_cdc_time, latency, empty_scan_count, failed_sessions_count  
FROM sys.dm_cdc_log_scan_sessions  
WHERE session_id = (SELECT MAX(b.session_id) FROM sys.dm_cdc_log_scan_sessions AS b);  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sys.dm_cdc_errors &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/change-data-capture-sys-dm-cdc-errors.md)  
  
  

