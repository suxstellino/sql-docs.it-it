---
description: Classe di evento Plan Guide Unsuccessful
title: Classe di evento Plan Guide Unsuccessful | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Plan Guide Unsuccessful event class
ms.assetid: ef9759f8-5613-4884-9257-86b609313f69
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 29b9b3a853b91f0a91c7431eb821fa0b6ebd218e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202044"
---
# <a name="plan-guide-unsuccessful-event-class"></a>Classe di evento Plan Guide Unsuccessful
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Questa classe di evento indica che in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è stato possibile creare un piano di esecuzione per una query o un batch contenente una guida di piano. Il piano è stato invece compilato senza utilizzare la guida di piano. L'evento viene generato quando vengono soddisfatte le condizioni seguenti:  
  
-   Il modulo/batch nella definizione della guida di piano corrisponde al batch in esecuzione.  
  
-   La query nella definizione della guida di piano corrisponde alla query in esecuzione.  
  
-   Gli hint nella definizione della guida di piano, incluso l'hint USE PLAN, non vengono applicati correttamente alla query o batch. Quindi, il piano di query compilato non è in grado di applicare gli hint specificati e il piano viene compilato utilizzando la guida di piano.  
  
 Una guida di piano non valida può causare la generazione di questo evento. Convalidare la guida di piano usata dalla query o batch tramite la funzione [sys.fn_validate_plan_guide](../../relational-databases/system-functions/sys-fn-validate-plan-guide-transact-sql.md) e correggere l'errore riportato da questa funzione.  
  
 Questo evento è incluso nel modello Tuning di [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] .  
  
## <a name="plan-guide-unsuccessful-event-class-data-columns"></a>Colonne di dati della classe di evento Plan Guide Unsuccessful  
  
|Nome colonna di dati|Tipo di dati|Descrizione|ID colonna|Filtrabile|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|Nome dell'applicazione client in cui è stata creata la connessione a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questa colonna viene popolata con i valori passati dall'applicazione anziché con il nome visualizzato del programma.|10|Sì|  
|ClientProcessID|**int**|ID assegnato dal computer host al processo in cui è in esecuzione l'applicazione client. Questa colonna di dati viene popolata se tramite il client viene indicato l'ID del processo client.|9|Sì|  
|DatabaseID|**int**|ID del database specificato nell'istruzione USE *database* oppure ID del database predefinito, se per un'istanza specificata non viene eseguita un'istruzione USE *database* . [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] visualizza il nome del database se la colonna di dati ServerName è acquisita nella traccia e il server è disponibile. Determinare il valore per un database utilizzando la funzione DB_ID.|3|Sì|  
|DatabaseName|**nvarchar**|Nome del database nel quale viene eseguita l'istruzione dell'utente.|35|Sì|  
|EventClass|**int**|Tipo di evento = 218.|27|No|  
|EventSequence|**int**|Sequenza di un determinato evento all'interno della richiesta.|51|No|  
|HostName|**nvarchar**|Nome del computer in cui viene eseguito il client. Questa colonna di dati viene popolata se il client fornisce il nome host. Per determinare il nome host, usare la funzione HOST_NAME.|8|Sì|  
|IsSystem|**int**|Indica se l'evento si è verificato in un processo di sistema o in un processo utente: 1 = sistema, 0 = utente.|60|Sì|  
|LoginName|**nvarchar**|Nome dell'account di accesso dell'utente (account di accesso di sicurezza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o credenziali di accesso di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows nel formato DOMINIO\\*nomeutente*).|11|Sì|  
|LoginSid|**image**|ID di sicurezza (SID) dell'utente connesso. Tali informazioni sono disponibili nelle viste del catalogo [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md) o [sys.sql_logins](../../relational-databases/system-catalog-views/sys-sql-logins-transact-sql.md) . Il SID è univoco per ogni account di accesso nel server.|41|Sì|  
|NTDomainName|**nvarchar**|Dominio Windows di appartenenza dell'utente.|7|Sì|  
|NTUserName|**nvarchar**|Nome utente di Windows.|6|Sì|  
|ObjectID|**int**|ID oggetto del modulo in fase di compilazione quando viene applicata la guida di piano. Se la guida di piano non viene applicata a un modulo, questa colonna è impostata su NULL.|22|Sì|  
|RequestID|**int**|ID della richiesta contenente l'istruzione.|49|Sì|  
|ServerName|**nvarchar**|Nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracciata.|26|No|  
|SessionLoginName|**nvarchar**|Nome dell'account di accesso dell'utente che ha avviato la sessione. Se ad esempio si stabilisce la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con l'account di accesso Login1 e si esegue un'istruzione con l'account di accesso Login2, SessionLoginName indica Login1 e LoginName indica Login2. In questa colonna sono visualizzati sia gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che quelli di Windows.|64|Sì|  
|SPID|**int**|ID della sessione in cui si è verificato l'evento.|12|Sì|  
|StartTime|**datetime**|Ora di inizio dell'evento, se disponibile.|14|Sì|  
|TextData|**ntext**|Nome della guida di piano.|1|Sì|  
|TransactionID|**bigint**|ID della transazione assegnato dal sistema.|4|Sì|  
|XactSequence|**bigint**|Token utilizzato per descrivere la transazione corrente.|50|Sì|  
  
## <a name="see-also"></a>Vedere anche  
 [Classe di evento Plan Guide Successful](../../relational-databases/event-classes/plan-guide-successful-event-class.md)   
 [Eventi estesi](../../relational-databases/extended-events/extended-events.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sys.fn_validate_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-validate-plan-guide-transact-sql.md)   
 [sp_create_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)   
 [sp_create_plan_guide_from_handle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md)  
  
  
