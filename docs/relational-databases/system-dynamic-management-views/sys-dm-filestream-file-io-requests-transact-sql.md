---
description: sys.dm_filestream_file_io_requests (Transact-SQL)
title: sys.dm_filestream_file_io_requests (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_filestream_file_io_requests
- dm_filestream_file_io_requests
- sys.dm_filestream_file_io_requests_TSQL
- dm_filestream_file_io_requests_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_filestream_file_io_requests catalog view
ms.assetid: d41e39a5-14d5-4f3d-a2e3-a822b454c1ed
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e4ee0854a59ae465f8eff35dadb90ac33a499651
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186093"
---
# <a name="sysdm_filestream_file_io_requests-transact-sql"></a>sys.dm_filestream_file_io_requests (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Viene visualizzato un elenco di richieste di I/O elaborate dal proprietario dello spazio dei nomi (NSO, Namespace Owner) in quel preciso momento.  
  
|Colonna|Tipo|Descrizione|  
|------------|----------|-----------------|  
|**request_context_address**|**varbinary (8)**|Viene visualizzato l'indirizzo interno del blocco di memoria NSO in cui è contenuta la richiesta di I/O del driver. Non ammette i valori Null.|  
|**current_spid**|**smallint**|Mostra l'ID del processo di sistema (SPID) per la connessione del SQL Server corrente. Non ammette i valori Null.|  
|**request_type**|**nvarchar(60)**|Viene mostrato il tipo di pacchetto di richiesta di I/O (IRP). I possibili tipi di richiesta sono REQ_PRE_CREATE, REQ_POST_CREATE, REQ_RESOLVE_VOLUME, REQ_GET_VOLUME_INFO, REQ_GET_LOGICAL_NAME, REQ_GET_PHYSICAL_NAME, REQ_PRE_CLEANUP, REQ_POST_CLEANUP, REQ_CLOSE, REQ_FSCTL, REQ_QUERY_INFO, REQ_SET_INFO, REQ_ENUM_DIRECTORY, REQ_QUERY_SECURITY e REQ_SET_SECURITY. Non ammette i valori Null|  
|**request_state**|**nvarchar(60)**|Viene mostrato lo stato della richiesta di I/O in NSO. I valori possibili sono REQ_STATE_RECEIVED, REQ_STATE_INITIALIZED, REQ_STATE_ENQUEUED, REQ_STATE_PROCESSING, REQ_STATE_FORMATTING_RESPONSE, REQ_STATE_SENDING_RESPONSE, REQ_STATE_COMPLETING e REQ_STATE_COMPLETED. Non ammette i valori Null.|  
|**request_id**|**int**|Viene mostrato l'ID univoco della richiesta assegnato dal driver a questa richiesta. Non ammette i valori Null.|  
|**irp_id**|**int**|Viene mostrato l'ID IRP univoco. È utile per identificare tutte le richieste di I/O correlate all'IRP specificato. Non ammette i valori Null.|  
|**handle_id**|**int**|Viene indicato l'ID handle dello spazio dei nomi. Si tratta dell'identificatore specifico dell'NSO ed è univoco in tutta l'istanza. Non ammette i valori Null.|  
|**client_thread_id**|**varbinary (8)**|Mostra l'ID del thread dell'applicazione client che ha origine la richiesta.<br /><br /> **\* \* Questo \* avviso \*** è significativo solo se l'applicazione client è in esecuzione nello stesso computer del SQL Server. Quando l'applicazione client è in esecuzione in modalità remota, il **client_thread_id** Mostra l'ID del thread di un processo di sistema che funziona per conto del client remoto.<br /><br /> Ammette i valori Null.|  
|**client_process_id**|**varbinary (8)**|Viene mostrato l'ID processo dell'applicazione client se quest'ultima è in esecuzione nello stesso computer in cui è installato SQL Server. Per un client remoto, viene mostrato l'ID processo di sistema in funzione a nome dell'applicazione client. Ammette i valori Null.|  
|**handle_context_address**|**varbinary (8)**|Mostra l'indirizzo della struttura NSO interna associata all'handle del client. Ammette i valori Null.|  
|**filestream_transaction_id**|**varbinary(128)**|Vengono mostrati l'ID della transazione associata all'handle specifico e tutte le richieste associate a questo handle. Si tratta del valore restituito dalla funzione **GET_FILESTREAM_TRANSACTION_CONTEXT** . Ammette i valori Null.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW SERVER STATE per il server.  
  
## <a name="see-also"></a>Vedere anche  
 [Viste a gestione dinamica FILESTREAM e FileTable &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)  
  
  
