---
description: Missing Column Statistics - classe di evento
title: Classe di evento Missing Column Statistics | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Missing Column Statistics event class
ms.assetid: b9321143-3f29-4a19-8471-7085669282ef
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e2367b7039fea3de4ea09468a093614a77bc2619
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204602"
---
# <a name="missing-column-statistics-event-class"></a>Missing Column Statistics - classe di evento
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La classe di evento Missing Column Statistics indica che le statistiche di colonna, utilizzabili da Query Optimizer, non sono disponibili.  
  
 Monitorando la classe di evento Missing Column Statistics è possibile determinare se mancano statistiche per una colonna utilizzata da una query. Di conseguenza è possibile che Query Optimizer scelga un piano di query meno efficiente del previsto.  
  
## <a name="missing-column-statistics-event-class-data-columns"></a>Colonne di dati della classe di eventi Missing Column Statistics  
  
|Nome colonna di dati|Tipo di dati|Descrizione|ID colonna|Filtrabile|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|Nome dell'applicazione client in cui è stata creata la connessione a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questa colonna viene popolata con i valori passati dall'applicazione e non con il nome visualizzato del programma.|10|Sì|  
|ClientProcessID|**int**|ID assegnato dal computer host al processo in cui è in esecuzione l'applicazione client. Questa colonna di dati viene popolata se tramite il client viene indicato l'ID del processo client.|9|Sì|  
|DatabaseID|**int**|ID del database specificato nell'istruzione di *database* USE oppure il database predefinito se per un'istanza specifica l'istruzione di *database* USE non è stata eseguita. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] visualizza il nome del database se la colonna di dati ServerName è acquisita nella traccia e il server è disponibile. Determinare il valore per un database utilizzando la funzione DB_ID.|3|Sì|  
|DatabaseName|**nvarchar**|Nome del database nel quale viene eseguita l'istruzione dell'utente.|35|Sì|  
|EventClass|**int**|Tipo di evento=79.|27|No|  
|EventSequence|**int**|Sequenza di un determinato evento all'interno della richiesta.|51|No|  
|GroupID|**int**|ID del gruppo del carico di lavoro in cui viene generato l'evento di Traccia SQL.|66|Sì|  
|HostName|**nvarchar**|Nome del computer in cui viene eseguito il client. Questa colonna di dati viene popolata se il client fornisce il nome host. Per determinare il nome host, usare la funzione HOST_NAME.|8|Sì|  
|IsSystem|**int**|Indica se l'evento è stato generato per un processo di sistema o un processo utente. 1 = sistema, 0 = utente.|60|Sì|  
|LoginName|**nvarchar**|Nome dell'account di accesso dell'utente (account di accesso di sicurezza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o credenziali di accesso di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows nel formato DOMINIO\nomeutente).|11|Sì|  
|LoginSid|**image**|ID di sicurezza (SID) dell'utente connesso. Queste informazioni sono disponibili nella vista del catalogo sys.server_principals. Il SID è univoco per ogni account di accesso nel server.|41|Sì|  
|NTDomainName|**nvarchar**|Dominio Windows di appartenenza dell'utente.|7|Sì|  
|NTUserName|**nvarchar**|Nome utente di Windows.|6|Sì|  
|RequestID|**int**|ID della richiesta contenente l'istruzione.|49|Sì|  
|ServerName|**nvarchar**|Nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracciata.|26|No|  
|SessionLoginName|**nvarchar**|Nome dell'account di accesso dell'utente che ha avviato la sessione. Se ad esempio si stabilisce la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con l'account di accesso Login1 e si esegue un'istruzione con l'account di accesso Login2, SessionLoginName indica Login1 e LoginName indica Login2. In questa colonna sono visualizzati sia gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che quelli di Windows.|64|Sì|  
|SPID|**int**|ID della sessione in cui si è verificato l'evento.|12|Sì|  
|StartTime|**datetime**|Ora di inizio dell'evento, se disponibile.|14|Sì|  
|TextData|**ntext**|Elenco delle colonne con statistiche mancanti.|1|Sì|  
|TransactionID|**bigint**|ID della transazione assegnato dal sistema.|4|Sì|  
|XactSequence|**bigint**|Token utilizzato per descrivere la transazione corrente.|50|Sì|  
  
## <a name="see-also"></a>Vedere anche  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md)   
 [Monitoraggio e risposta agli eventi](../../ssms/agent/monitor-and-respond-to-events.md)  
  
  
