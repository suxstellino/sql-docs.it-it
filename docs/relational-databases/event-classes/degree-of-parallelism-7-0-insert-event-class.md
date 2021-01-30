---
description: Classe di evento Degree of Parallelism (7.0 Insert)
title: Classe di evento Degree of Parallelism (7.0 Insert) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Degree of Parallelism event class
ms.assetid: 6753ef30-890f-47a3-b0b6-8abb184e1d83
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 197dc975e6ca6e8119644188fac33aec77a541f2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182930"
---
# <a name="degree-of-parallelism-70-insert-event-class"></a>Classe di evento Degree of Parallelism (7.0 Insert)
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La classe di evento **Degree of Parallelism (7.0 Insert)** viene generata ogni volta che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esegue un'istruzione SELECT, INSERT, UPDATE o DELETE.  
  
 Quando questa classe di evento viene inclusa in una traccia, la quantità di overhead conseguente potrebbe causare un peggioramento significativo delle prestazioni, se questi eventi si verificano di frequente. Per ridurre l'overhead, è consigliabile limitare l'utilizzo della classe di evento alle tracce che eseguono brevemente il monitoraggio di problemi specifici.  
  
## <a name="degree-of-parallelism-70-insert-event-class-data-columns"></a>Colonne di dati della classe di evento Degree of Parallelism (7.0 Insert)  
  
|Nome colonna di dati|Tipo di dati|Descrizione|ID colonna|Filtrabile|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**ApplicationName**|**nvarchar**|Nome dell'applicazione client in cui è stata creata la connessione a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questa colonna viene popolata con i valori passati dall'applicazione e non con il nome visualizzato del programma.|10|Sì|  
|**BinaryData**|**image**|Numero di CPU utilizzate per eseguire il processo in base ai valori seguenti:<br /><br /> 0x00000000, indica un piano seriale eseguito in serie.<br /><br /> 0x01000000, indica un piano parallelo eseguito in serie.<br /><br /> >= 0x02000000, indica un piano parallelo eseguito in parallelo.|2|No|  
|**ClientProcessID**|**int**|ID assegnato dal computer host al processo in cui è in esecuzione l'applicazione client. Questa colonna di dati viene popolata se l'ID del processo client viene fornito dal client.|9|Sì|  
|**DatabaseID**|**int**|ID del database specificato nell'istruzione USE database oppure ID del database predefinito, se per una determinata istanza non viene eseguita un'istruzione USE database. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] visualizza il nome del database se la colonna di dati **ServerName** è acquisita nella traccia e il server è disponibile. Determinare il valore per un database utilizzando la funzione DB_ID.|3|Sì|  
|**DatabaseName**|**nvarchar**|Nome del database nel quale viene eseguita l'istruzione dell'utente.|35|Sì|  
|**Event Class**|**int**|Tipo di evento = 28.|27|No|  
|**EventSequence**|**int**|Sequenza di un determinato evento all'interno della richiesta.|51|No|  
|**EventSubClass**|**int**|Indica l'istruzione eseguita, in base ai valori seguenti:<br /><br /> 1=Seleziona<br /><br /> 2=Inserisci<br /><br /> 3=Aggiorna<br /><br /> 4=Elimina|21|No|  
|**GroupID**|**int**|ID del gruppo del carico di lavoro in cui viene generato l'evento di Traccia SQL.|66|Sì|  
|**HostName**|**nvarchar**|Nome del computer in cui viene eseguito il client. Questa colonna di dati viene popolata se il nome host viene fornito dal client. Per determinare il nome host, usare la funzione HOST_NAME.|8|Sì|  
|**Integer Data**|**int**|Quantità di "memoria dell'area di lavoro", espressa in KB, assegnata alla query per eseguire operazioni che implicano l'hashing, l'ordinamento o la creazione di indici. La memoria verrà acquisita durante l'esecuzione in base alle necessità.|25|Sì|  
|**IsSystem**|**int**|Indica se l'evento è stato generato per un processo di sistema o un processo utente. 1 = sistema, 0 = utente.|60|Sì|  
|**LoginName**|**nvarchar**|Nome dell'account di accesso dell'utente: account di sicurezza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o credenziali di accesso di Windows nel formato DOMINIO\\*nomeutente*.|11|Sì|  
|**LoginSid**|**image**|ID di sicurezza (SID) dell'utente connesso. Queste informazioni sono disponibili nella vista del catalogo sys.server_principals. Il SID è univoco per ogni account di accesso nel server.|41|Sì|  
|**NTDomainName**|**nvarchar**|Dominio Windows di appartenenza dell'utente.|7|Sì|  
|**NTUserName**|**nvarchar**|Nome utente di Windows.|6|Sì|  
|**RequestID**|**int**|Identificazione della richiesta che ha avviato la query full-text.|49|Sì|  
|**ServerName**|**nvarchar**|Nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracciata.|26|No|  
|**SessionLoginName**|**nvarchar**|Nome dell'account di accesso dell'utente che ha avviato la sessione. Se ad esempio si stabilisce la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con l'account di accesso Login1 e si esegue un'istruzione con l'account di accesso Login2, **SessionLoginName** indica Login1 e **LoginName** indica Login2. In questa colonna sono visualizzati sia gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che quelli di Windows.|64|Sì|  
|**SPID**|**int**|ID della sessione in cui si è verificato l'evento.|12|Sì|  
|**StartTime**|**datetime**|Ora di inizio dell'evento, se disponibile.|14|Sì|  
|**TransactionID**|**bigint**|ID della transazione assegnato dal sistema.|4|Sì|  
  
## <a name="see-also"></a>Vedere anche  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)  
  
  
