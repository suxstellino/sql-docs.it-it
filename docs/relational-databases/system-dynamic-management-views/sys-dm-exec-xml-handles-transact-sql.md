---
description: sys.dm_exec_xml_handles (Transact-SQL)
title: sys.dm_exec_xml_handles (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_xml_handles
- dm_exec_xml_handles_TSQL
- sys.dm_exec_xml_handles_TSQL
- sys.dm_exec_xml_handles
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_xml_handles dynamic management function
ms.assetid: a873ce0f-6955-417a-96a1-b2ef11a83633
author: pmasl
ms.author: pelopes
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3e8529f7626ffa25d86cbb73c704198082e32bcf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208061"
---
# <a name="sysdm_exec_xml_handles-transact-sql"></a>sys.dm_exec_xml_handles (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md)]

  Restituisce informazioni sugli handle attivi aperti da **sp_xml_preparedocument**.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
dm_exec_xml_handles (session_id | 0 )  
```  
  
## <a name="arguments"></a>Argomenti  
 *session_id* | 0  
 ID della sessione. Se *session_id* viene specificato, questa funzione restituisce informazioni sugli handle XML nella sessione specificata.  
  
 Se si specifica 0, la funzione restituisce informazioni su tutti gli handle XML di tutte le sessioni.  
  
## <a name="table-returned"></a>Tabella restituita  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|ID della sessione che contiene l'handle del documento XML.|  
|**document_id**|**int**|ID dell'handle del documento XML restituito da **sp_xml_preparedocument**.|  
|**namespace_document_id**|**int**|ID handle interno utilizzato per il documento dello spazio dei nomi associato passato come terzo parametro a **sp_xml_preparedocument**. È NULL se non esiste un documento dello spazio dei nomi.|  
|**sql_handle**|**varbinary(64)**|Handle per il testo del codice SQL in cui l'handle è stato definito.|  
|**statement_start_offset**|**int**|Numero di caratteri nel batch o stored procedure attualmente in esecuzione in corrispondenza del quale si verifica la chiamata al **sp_xml_preparedocument** . Può essere utilizzato insieme all' **sql_handle**, **statement_end_offset** e **sys.dm_exec_sql_text** funzione a gestione dinamica per recuperare l'istruzione attualmente in esecuzione per la richiesta.|  
|**statement_end_offset**|**int**|Numero di caratteri nel batch o stored procedure attualmente in esecuzione in corrispondenza del quale si verifica la chiamata al **sp_xml_preparedocument** . Può essere utilizzato insieme all' **sql_handle**, **statement_start_offset** e **sys.dm_exec_sql_text** funzione a gestione dinamica per recuperare l'istruzione attualmente in esecuzione per la richiesta.|  
|**creation_time**|**datetime**|Timestamp in cui è stato chiamato **sp_xml_preparedocument** .|  
|**original_document_size_bytes**|**bigint**|Dimensioni in byte del documento XML non analizzato.|  
|**original_namespace_document_size_bytes**|**bigint**|Dimensioni in byte del documento dello spazio dei nomi XML non analizzato. È NULL se non esiste un documento dello spazio dei nomi.|  
|**num_openxml_calls**|**bigint**|Numero di chiamate a OPENXML con questo handle di documento.|  
|**row_count**|**bigint**|Numero di righe restituite da tutte le chiamate a OPENXML precedenti per questo handle di documento.|  
|**dormant_duration_ms**|**bigint**|Millisecondi trascorsi dall'ultima chiamata a OPENXML. Se OPENXML non è stato chiamato, restituisce i millisecondi dalla chiamata a **sp_xml_preparedocumen** t.|  
  
## <a name="remarks"></a>Commenti  
 La durata del **sql_handles** utilizzata per recuperare il testo SQL che ha eseguito una chiamata a **sp_xml_preparedocument** sopravvive al piano memorizzato nella cache utilizzato per eseguire la query. Se il testo della query non è disponibile nella cache, non sarà possibile recuperare i dati utilizzando le informazioni incluse nel risultato della funzione. Questa situazione può verificarsi in caso di esecuzione di numerosi batch di grandi dimensioni.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW SERVER STATE sul server per visualizzare tutte le sessioni o gli ID di sessione che non appartengono al chiamante. Un chiamante può sempre visualizzare i dati del proprio ID della sessione corrente.      
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono selezionati tutti gli handle attivi.  
  
```  
SELECT * FROM sys.dm_exec_xml_handles(0);  
```  
  
## <a name="see-also"></a>Vedere anche  
 <br>[Funzioni e viste a gestione dinamica (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)
 <br>[Funzioni e viste a gestione dinamica relative all'esecuzione (Transact-SQL)](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)
 <br>[sp_xml_preparedocument (Transact-SQL)](../system-stored-procedures/sp-xml-preparedocument-transact-sql.md)
 <br>[sp_xml_removedocument (Transact-SQL)](../system-stored-procedures/sp-xml-removedocument-transact-sql.md)


 
  
  
