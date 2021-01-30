---
description: Auto Stats - classe di evento
title: Classe di evento Auto Stats | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Auto Stats event class
ms.assetid: cd613fce-01e1-4d8f-86cc-7ffbf0759f9e
author: stevestein
ms.author: sstein
ms.openlocfilehash: d84664b1ad1dbddc8ab1595c8cddf6ee1abd33b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196341"
---
# <a name="auto-stats-event-class"></a>Auto Stats - classe di evento
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
   La classe di evento **Auto Stats** indica che si è verificato un evento di aggiornamento automatico delle statistiche di indice e di colonna.  **Auto Stats** viene attivata anche quando vengono caricate statistiche che per l'uso in Query Optimizer.
  
## <a name="auto-stats-event-class-data-columns"></a>Colonne di dati della classe di evento Auto Stats  
  
|Nome colonna di dati|Tipo di dati|Descrizione|ID colonna|Filtrabile|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**ApplicationName**|**nvarchar**|Nome dell'applicazione client in cui è stata creata la connessione a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questa colonna viene popolata con i valori passati dall'applicazione e non con il nome visualizzato del programma.|10|Sì|  
|**ClientProcessID**|**int**|ID assegnato dal computer host al processo in cui è in esecuzione l'applicazione client. Questa colonna di dati viene popolata se tramite il client viene indicato l'ID del processo client.|9|Sì|  
|**DatabaseID**|**int**|ID del database specificato nell'istruzione USE *database* oppure ID del database predefinito, se per una determinata istanza non viene eseguita un'istruzione USE *database* . [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] visualizza il nome del database se la colonna di dati **ServerName** è acquisita nella traccia e il server è disponibile. Determinare il valore per un database utilizzando la funzione DB_ID.|3|Sì|  
|**DatabaseName**|**nvarchar**|Nome del database nel quale viene eseguita l'istruzione dell'utente.|35|Sì|  
|**Duration**|**bigint**|Durata dell'evento in microsecondi.|13|Sì|  
|**EndTime**|**datetime**|Ora di fine dell'evento.|15|Sì|  
|**Error (Errore) (Error (Errore)e)**|**int**|Numero di errore di un evento specifico. Corrisponde spesso al numero di errore archiviato nella vista del catalogo **sys.messages** .|31|Sì|  
|**EventClass**|**int**|Tipo di evento = 58.|27|No|  
|**EventSequence**|**int**|Sequenza di un determinato evento all'interno della richiesta.|51|No|  
|**EventSubClass**|**int**|Tipo di sottoclasse di evento:<br /><br /> 1: statistiche create/aggiornate in modo sincrono; la colonna **TextData** indica le statistiche e dove sono state create o aggiornate.<br /><br /> 2: aggiornamento statistiche asincrono; processo in coda.<br /><br /> 3: aggiornamento statistiche asincrono; processo in avvio.<br /><br /> 4: aggiornamento statistiche asincrono; processo completato.|21|Sì|  
|**GroupID**|**int**|ID del gruppo del carico di lavoro in cui viene generato l'evento di Traccia SQL.|66|Sì|  
|**HostName**|**nvarchar**|Nome del computer in cui viene eseguito il client. Questa colonna di dati viene popolata se il nome host viene fornito dal client. Per determinare il nome host, usare la funzione HOST_NAME.|8|Sì|  
|**IndexID**|**int**|ID della voce di statistiche/indice nell'oggetto interessato dall'evento. Per determinare l'ID dell'indice per un oggetto, utilizzare la colonna **index_id** della vista del catalogo **sys.indexes** .|24|Sì|  
|**IntegerData**|**int**|Numero delle raccolte di statistiche correttamente aggiornate.|25|Sì|  
|**IntegerData2**|**int**|Numero di sequenza del processo.|55|Sì|  
|**IsSystem**|**int**|Indica se l'evento è stato generato per un processo di sistema o un processo utente. 1 = sistema, 0 = utente.|60|Sì|  
|**LoginName**|**nvarchar**|Nome dell'account di accesso dell'utente (account di sicurezza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o credenziali di accesso di Windows nel formato DOMINIO\nomeutente).|11|Sì|  
|**LoginSid**|**image**|ID di sicurezza (SID) dell'utente connesso. Queste informazioni sono disponibili nella vista del catalogo **sys.server_principals** . Il SID è univoco per ogni account di accesso nel server.|41|Sì|  
|**NTDomainName**|**nvarchar**|Dominio Windows di appartenenza dell'utente.|7|Sì|  
|**NTUserName**|**nvarchar**|Nome utente di Windows.|6|Sì|  
|**ObjectID**|**int**|ID dell'oggetto assegnato dal sistema.|22|Sì|  
|**RequestID**|**int**|ID della richiesta contenente l'istruzione.|49|Sì|  
|**ServerName**|**nvarchar**|Nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracciata.|26|No|  
|**SessionLoginName**|**nvarchar**|Nome dell'account di accesso dell'utente che ha avviato la sessione. Se ad esempio si stabilisce la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con l'account di accesso Login1 e si esegue un'istruzione con l'account di accesso Login2, **SessionLoginName** indica Login1 e **LoginName** indica Login2. In questa colonna sono visualizzati sia gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che quelli di Windows.|64|Sì|  
|**SPID**|**int**|ID della sessione in cui si è verificato l'evento.|12|Sì|  
|**StartTime**|**datetime**|Ora di inizio dell'evento, se disponibile.|14|Sì|  
|**Success**|**int**|0 = errore.<br /><br /> 1 = esito positivo.<br /><br /> 2 = ignorato a causa di limitazione del server (MSDE).|23|Sì|  
|**TextData**|**ntext**|Il contenuto della colonna dipende dal fatto che le statistiche vengano aggiornate in modo sincrono (**EventSubClass** 1) o asincrono (**EventSubClass** 2, 3 o 4):<br /><br /> 1: elenca le statistiche aggiornate/create<br /><br /> 2, 3 o 4: NULL; la colonna **IndexID** viene popolata con l'ID dell'indice/statistiche per le statistiche aggiornate.|1|Sì|  
|**TransactionID**|**bigint**|ID della transazione assegnato dal sistema.|4|Sì|  
|**Tipo**|**int**|Tipo di processo.|57|Sì|  
  
## <a name="see-also"></a>Vedere anche  
 [Eventi estesi](../../relational-databases/extended-events/extended-events.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
