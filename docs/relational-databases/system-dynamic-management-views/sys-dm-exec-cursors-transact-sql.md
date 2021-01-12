---
description: sys.dm_exec_cursors (Transact-SQL)
title: sys.dm_exec_cursors (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_exec_cursors_TSQL
- dm_exec_cursors
- dm_exec_cursors_TSQL
- sys.dm_exec_cursors
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_cursors dynamic management function
ms.assetid: f520b63c-36af-40f1-bf71-6901d6331d3d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8ab1ed9e722f2363c0568932e7eb337bc09dbe41
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099004"
---
# <a name="sysdm_exec_cursors-transact-sql"></a>sys.dm_exec_cursors (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni sui cursori aperti in vari database.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
dm_exec_cursors (session_id | 0 )  
```  
  
## <a name="arguments"></a>Argomenti  
 *session_id* | 0  
 ID della sessione. Se *session_id* viene specificato, questa funzione restituisce informazioni sui cursori nella sessione specificata.  
  
 Se viene specificato 0, la funzione restituisce informazioni su tutti i cursori per tutte le sessioni.  
  
## <a name="table-returned"></a>Tabella restituita  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|ID della sessione che include il cursore.|  
|**cursor_id**|**int**|ID dell'oggetto cursore.|  
|**nome**|**nvarchar(256)**|Nome del cursore definito dall'utente.|  
|**properties**|**nvarchar(256)**|Specifica le proprietà del cursore. I valori delle proprietà seguenti vengono concatenati per comporre il valore di questa colonna:<br />Interfaccia di dichiarazione<br />Tipo di cursore <br />Concorrenza dei cursori<br />Scopo del cursore<br />Livello di nidificazione del cursore<br /><br /> Il valore restituito in questa colonna, ad esempio, potrebbe essere "TSQL &#124; Dynamic &#124; ottimistica &#124; globale (0)".|  
|**sql_handle**|**varbinary(64)**|Handle per il testo del batch che ha dichiarato il cursore.|  
|**statement_start_offset**|**int**|Numero di caratteri nella stored procedure o nel batch attualmente in esecuzione in cui inizia l'istruzione in esecuzione. Può essere utilizzato insieme all' **sql_handle**, **statement_end_offset** e [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md) funzione a gestione dinamica per recuperare l'istruzione attualmente in esecuzione per la richiesta.|  
|**statement_end_offset**|**int**|Numero di caratteri nella stored procedure o nel batch attualmente in esecuzione in cui termina l'istruzione in esecuzione. Può essere utilizzato insieme all' **sql_handle**, **statement_start_offset** e **sys.dm_exec_sql_text** funzione a gestione dinamica per recuperare l'istruzione attualmente in esecuzione per la richiesta.|  
|**plan_generation_num**|**bigint**|Numero di sequenza utilizzabile per distinguere le istanze dei piani dopo la ricompilazione.|  
|**creation_time**|**datetime**|Timestamp relativo alla creazione del cursore.|  
|**is_open**|**bit**|Specifica se il cursore è aperto.|  
|**is_async_population**|**bit**|Specifica se il thread in background sta ancora popolando un cursore KEYSET o STATIC in modo asincrono.|  
|**is_close_on_commit**|**bit**|Specifica se il cursore è stato dichiarato tramite CURSOR_CLOSE_ON_COMMIT.<br /><br /> 1 = Il cursore verrà chiuso al termine della transazione.|  
|**fetch_status**|**int**|Restituisce l'ultimo stato di recupero del cursore. Si tratta dell'ultimo valore @ restituito @FETCH_STATUS .|  
|**fetch_buffer_size**|**int**|Restituisce le informazioni sulle dimensioni del buffer di recupero.<br /><br /> 1 = Cursori Transact-SQL. Può essere impostato su un valore più elevato per i cursori API.|  
|**fetch_buffer_start**|**int**|Per i cursori FAST_FORWARD e DYNAMIC, restituisce 0 se il cursore non è aperto o se è posizionato prima della riga iniziale. In caso contrario, restituisce -1.<br /><br /> Per i cursori STATIC e KEYSET, restituisce 0 se il cursore non è aperto e -1 se il cursore è posizionato oltre l'ultima riga.<br /><br /> In caso contrario, restituisce il numero di riga in cui è posizionato.|  
|**ansi_position**|**int**|Posizione del cursore all'interno del buffer di recupero.|  
|**worker_time**|**bigint**|Tempo impiegato, in microsecondi, dai thread worker che eseguono il cursore.|  
|**legge**|**bigint**|Numero di letture eseguite dal cursore.|  
|**scrive**|**bigint**|Numero di scritture eseguite dal cursore.|  
|**dormant_duration**|**bigint**|Millisecondi trascorsi a partire dall'avvio dell'ultima query (apertura o recupero) sul cursore.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW SERVER STATE per il server.  
  
## <a name="remarks"></a>Osservazioni  
 Nella tabella seguente vengono fornite informazioni sull'interfaccia di dichiarazione del cursore e vengono indicati i possibili valori per la colonna delle proprietà.  
  
|Proprietà|Descrizione|  
|--------------|-----------------|  
|API|Il cursore è stato dichiarato tramite una delle API di accesso ai dati (ODBC, OLEDB).|  
|TSQL|Il cursore è stato dichiarato tramite la sintassi Transact-SQL DECLARE CURSOR.|  
  
 Nella tabella seguente vengono fornite informazioni sul tipo di cursore e vengono inclusi i possibili valori per la colonna delle proprietà.  
  
|Type|Descrizione|  
|----------|-----------------|  
|Keyset|Il cursore è stato dichiarato come Keyset.|  
|Dynamic|Il cursore è stato dichiarato come Dynamic.|  
|Snapshot|Il cursore è stato dichiarato come Snapshot o Static.|  
|Fast_Forward|Il cursore è stato dichiarato come Fast Forward.|  
  
 Nella tabella seguente vengono fornite informazioni sulla concorrenza dei cursori e vengono inclusi i possibili valori per la colonna delle proprietà.  
  
|Concorrenza|Descrizione|  
|-----------------|-----------------|  
|Sola lettura|Il cursore è stato dichiarato come di sola lettura.|  
|Scroll Locks|Il cursore utilizza i blocchi di scorrimento.|  
|Optimistic|Il cursore utilizza il controllo della concorrenza ottimistica.|  
  
 Nella tabella seguente vengono fornite informazioni sullo scopo dei cursori e vengono inclusi i possibili valori per la colonna delle proprietà.  
  
|Ambito|Descrizione|  
|-----------|-----------------|  
|Locale|Specifica che l'ambito del cursore è locale rispetto al batch, alla stored procedure o al trigger in cui il cursore è stato creato.|  
|Globale|Specifica che l'ambito del cursore è globale rispetto alla connessione.|  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-detecting-old-cursors"></a>R. Individuazione dei cursori meno recenti  
 Nell'esempio seguente vengono restituite informazioni sui cursori che sono rimasti aperti nel server oltre il periodo di tempo specificato di 36 ore.  
  
```  
SELECT creation_time, cursor_id, name, c.session_id, login_name   
FROM sys.dm_exec_cursors(0) AS c   
JOIN sys.dm_exec_sessions AS s ON c.session_id = s.session_id   
WHERE DATEDIFF(hh, c.creation_time, GETDATE()) > 36;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funzioni e viste a gestione dinamica relative all'esecuzione &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_exec_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)  
  
  

