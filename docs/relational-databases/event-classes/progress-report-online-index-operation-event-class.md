---
description: 'Classe di evento Progress Report: Online Index Operation'
title: 'Classe di evento Progress Report: Online Index Operation'
ms.date: 06/03/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 'Progress Report: Online Index Operation event class [SQL Server]'
ms.assetid: 491616c1-f666-4b16-a5ea-1192bf156692
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.custom: seo-lt-2019
ms.openlocfilehash: c612dfd308f20c56f1e216861c1f59e6e169107a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205662"
---
# <a name="progress-report-online-index-operation-event-class"></a>Classe di evento Progress Report: Online Index Operation
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La classe di evento Progress Report: Online Index Operation indica lo stato di un'operazione di compilazione di un indice online durante l'esecuzione del processo di compilazione.  
  
## <a name="progress-report-online-index-operation-event-class-data-columns"></a>Colonne di dati della classe di evento Progress Report: Online Index Operation  
  
|Nome colonna di dati|Tipo di dati|Descrizione|ID colonna|Filtrabile|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|Nome dell'applicazione client in cui è stata creata la connessione a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questa colonna viene popolata con i valori passati dall'applicazione e non con il nome visualizzato del programma.|10|Sì|  
|BigintData1|**bigint**|Numero di righe inserite.|52|Sì|  
|BigintData2|**bigint**|0 = piano seriale; altrimenti, ID del thread durante l'esecuzione parallela.|53|Sì|  
|ClientProcessID|**int**|ID assegnato dal computer host al processo in cui è in esecuzione l'applicazione client. Questa colonna di dati viene popolata se tramite il client viene indicato l'ID del processo client.|9|Sì|  
|DatabaseID|**int**|ID del database specificato nell'istruzione di *database* USE oppure il database predefinito se per un'istanza specifica l'istruzione di *database* USE non è stata eseguita. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] visualizza il nome del database se la colonna di dati ServerName è acquisita nella traccia e il server è disponibile. Determinare il valore per un database utilizzando la funzione DB_ID.|3|Sì|  
|DatabaseName|**nvarchar**|Nome del database nel quale viene eseguita l'istruzione dell'utente.|35|Sì|  
|Duration|**bigint**|Durata dell'evento in microsecondi.|13|Sì|  
|EndTime|**datetime**|Ora del completamento dell'operazione di creazione dell'indice online.|15|Sì|  
|EventClass|**int**|Tipo di evento = 190.|27|No|  
|EventSequence|**int**|Sequenza di un determinato evento all'interno della richiesta.|51|No|  
|EventSubClass|**int**|Tipo di sottoclasse di evento.<br /><br /> 1 = Avvio<br /><br /> 2 = Inizio esecuzione fase 1<br /><br /> 3 = Fine esecuzione fase 1<br /><br /> 4 = Inizio esecuzione fase 2<br /><br /> 5 = Fine esecuzione fase 2<br /><br /> 6 = Conteggio righe inserite<br /><br /> 7 = Fine<br /><br /> La fase 1 si riferisce all'oggetto di base (indice cluster o heap) o indica se l'operazione sull'indice include solo un indice non cluster. La fase 2 viene usata quando un'operazione di compilazione di un indice include sia la ricompilazione originale che gli indici non cluster aggiuntivi.  Se, ad esempio, un oggetto ha un indice cluster e molti indici non cluster, con "Ricompila tutto" verrebbero ricompilati tutti gli indici. L'oggetto di base (indice cluster) viene ricompilato nella fase 1 e tutti gli indici non cluster vengono ricompilati quindi nella fase 2.|21|Sì|  
|GroupID|**int**|ID del gruppo del carico di lavoro in cui viene generato l'evento di Traccia SQL.|66|Sì|  
|HostName|**nvarchar**|Nome del computer in cui viene eseguito il client. Questa colonna di dati viene popolata se il client fornisce il nome host. Per determinare il nome host, usare la funzione HOST_NAME.|8|Sì|  
|IndexID|**int**|ID dell'indice dell'oggetto interessato dall'evento.|24|Sì|  
|IsSystem|**int**|Indica se l'evento è stato generato per un processo di sistema o un processo utente. 1 = sistema, 0 = utente.|60|Sì|  
|LoginName|**nvarchar**|Nome dell'account di accesso dell'utente (account di accesso di sicurezza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o credenziali di accesso di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows nel formato DOMINIO\nomeutente).|11|Sì|  
|LoginSid|**image**|ID di sicurezza (SID) dell'utente connesso. Queste informazioni sono disponibili nella vista del catalogo sys.server_principals. Il SID è univoco per ogni account di accesso nel server.|41|Sì|  
|NTDomainName|**nvarchar**|Dominio Windows di appartenenza dell'utente.|7|Sì|  
|NTUserName|**nvarchar**|Nome utente di Windows.|6|Sì|  
|ObjectID|**int**|ID dell'oggetto assegnato dal sistema.|22|Sì|  
|ObjectName|**nvarchar**|Nome dell'oggetto a cui si fa riferimento.|34|Sì|  
|PartitionId|**bigint**|ID della partizione da compilare.|65|Sì|  
|PartitionNumber|**int**|Numero ordinario della partizione da compilare.|25|Sì|  
|ServerName|**nvarchar**|Nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracciata.|26|No|  
|SessionLoginName|**nvarchar**|Nome dell'account di accesso dell'utente che ha avviato la sessione. Se ad esempio si stabilisce la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con l'account di accesso Login1 e si esegue un'istruzione con l'account di accesso Login2, SessionLoginName indica Login1 e LoginName indica Login2. In questa colonna sono visualizzati sia gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che quelli di Windows.|64|Sì|  
|SPID|**int**|ID della sessione in cui si è verificato l'evento.|12|Sì|  
|StartTime|**datetime**|Ora di inizio dell'evento.|14|Sì|  
|TransactionID|**bigint**|ID della transazione assegnato dal sistema.|4|Sì|  
  
## <a name="see-also"></a>Vedere anche  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
