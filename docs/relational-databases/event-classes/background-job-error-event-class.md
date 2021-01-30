---
description: Background Job Error - classe di evento
title: Classe di evento Background Job Error | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Background Job Error event class
ms.assetid: 9e6d2a0e-919d-4fe2-a306-b20b8d41c197
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1d16479ce4a8a4218cb704ac670c30401b1f55c5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187492"
---
# <a name="background-job-error-event-class"></a>Background Job Error - classe di evento
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La classe di evento **Background Job Error** si verifica quando un processo in background viene terminato in maniera anomala. Questa condizione potrebbe richiedere l'attenzione dell'amministratore di sistema.  
  
## <a name="background-job-error-event-class-data-columns"></a>Colonne di dati della classe di evento Background Job Error  
  
|Nome colonna di dati|Tipo di dati|Descrizione|ID colonna|Filtrabile|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**DatabaseID**|**int**|ID del database specificato dal processo. Determinare il valore per un database utilizzando la funzione DB_ID.|3|Sì|  
|**DatabaseName**|**nvarchar**|Nome del database nel quale viene eseguita l'istruzione dell'utente.|35|Sì|  
|**Error (Errore) (Error (Errore)e)**|**int**|Numero di errore relativo all'ultimo tentativo (solo **EventSubClass** 1).|31|Sì|  
|**EventClass**|**int**|Tipo di evento = 193.|27|No|  
|**EventSequence**|**int**|Sequenza di un determinato evento all'interno della richiesta.|51|No|  
|**EventSubClass**|**int**|Tipo di sottoclasse di evento.<br /><br /> 1 = Il processo in background è in fase di terminazione dopo l'errore.<br /><br /> 2 = Il processo in background è stato eliminato, coda piena.<br /><br /> 3 = Il processo in background ha restituito un errore.|21|Sì|  
|**IndexID**|**int**|ID dell'indice dell'oggetto interessato dall'evento. Per determinare l'ID di indice di un oggetto, utilizzare la colonna **indid** della tabella di sistema **sysindexes** .|24|Sì|  
|**IntegerData**|**int**|Numero di tentativi eseguiti dal processo (solo **EventSubClass** 1).|25|Sì|  
|**IntegerData2**|**int**|Numero di sequenza del processo.|55|Sì|  
|**ObjectID**|**int**|ID dell'oggetto assegnato dal sistema.|22|Sì|  
|**SessionLoginName**|**nvarchar**|Nome dell'account di accesso dell'utente che ha avviato la sessione. Se ad esempio si stabilisce la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con l'account di accesso Login1 e si esegue un'istruzione con l'account di accesso Login2, **SessionLoginName** indica Login1 e **LoginName** indica Login2. In questa colonna vengono visualizzati sia gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che quelli di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.|64|Sì|  
|**Gravità**|**int**|Livello di gravità dell'errore all'ultimo tentativo (solo **EventSubClass** 1).|20|Sì|  
|**StartTime**|**datetime**|Istante in cui è stato creato il processo.|14|Sì|  
|**State**|**int**|Stato dell'errore all'ultimo tentativo (solo **EventSubClass** 1).|30|Sì|  
|**TextData**|**ntext**|Descrizione testuale del valore di sottoclasse dell'evento.|1|Sì|  
|**Tipo**|**int**|Tipo di processo.|57|Sì|  
  
## <a name="see-also"></a>Vedere anche  
 [Eventi estesi](../../relational-databases/extended-events/extended-events.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [Classe di evento Auto Stats](../../relational-databases/event-classes/auto-stats-event-class.md)  
  
  
