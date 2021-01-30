---
description: SP:Recompile - classe di evento
title: Classe di evento SP:Recompile | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- SP:Recompile event class
ms.assetid: 526c8eae-a07b-4d0e-b91e-8e537835d77d
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3772823e0d7dba3ac83e152da2634b8841102520
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161580"
---
# <a name="sprecompile-event-class"></a>SP:Recompile - classe di evento
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La classe di evento SP:Recompile indica che una stored procedure, una funzione definita dall'utente o un trigger è stato ricompilato. Le ricompilazioni segnalate da questa classe di evento si verificano a livello di istruzione.  
  
 La strategia ottimale per tracciare ricompilazioni a livello di istruzione consiste nell'usare la classe di evento SQL:StmtRecompile. La classe di evento SP:Recompile è deprecata. Per altre informazioni, vedere [Classe di evento SQL:StmtRecompile](../../relational-databases/event-classes/sql-stmtrecompile-event-class.md).  
  
## <a name="sprecompile-event-class-data-columns"></a>Colonne di dati della classe di evento SP:Recompile  
  
|Nome colonna di dati|**Tipo di dati**|Descrizione|ID colonna|Filtrabile|  
|----------------------|-------------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|Nome dell'applicazione client in cui è stata creata la connessione a un'istanza di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questa colonna viene popolata con i valori passati dall'applicazione e non con il nome visualizzato del programma.|10|Sì|  
|ClientProcessID|**int**|ID assegnato dal computer host al processo in cui è in esecuzione l'applicazione client. Questa colonna di dati viene popolata se il client fornisce l'ID del processo.|9|Sì|  
|DatabaseID|**int**|ID del database nel quale viene eseguita la stored procedure. Determinare il valore per un database utilizzando la funzione DB_ID.|3|Sì|  
|DatabaseName|**nvarchar**|Nome del database nel quale viene eseguita la stored procedure.|35|Sì|  
|EventClass|**int**|Tipo di evento = 37.|27|No|  
|EventSequence|**int**|Sequenza di un determinato evento all'interno della richiesta.|51|No|  
|EventSubClass|**int**|Tipo di sottoclasse di evento. Indica il motivo della ricompilazione.<br /><br /> 1 = Schema modificato<br /><br /> 2 = Statistiche modificate<br /><br /> 3 = DNR ricompilazione<br /><br /> 4 = Opzione impostata modificata<br /><br /> 5 = Tabella temporanea modificata<br /><br /> 6 = Set di righe remoto modificato<br /><br /> 7 = Autorizzazioni FOR BROWSE modificate<br /><br /> 8 = Ambiente di notifica query modificato<br /><br /> 9 = Vista MPI modificata<br /><br /> 10 = Opzioni cursore modificate<br /><br /> 11 = Con opzione di ricompilazione|21|Sì|  
|GroupID|**int**|ID del gruppo del carico di lavoro in cui viene generato l'evento di Traccia SQL.|66|Sì|  
|HostName|**nvarchar**|Nome del computer in cui viene eseguito il client. Questa colonna di dati viene popolata se il client fornisce il nome host. Per determinare il nome host, usare la funzione HOST_NAME.|8|Sì|  
|IntegerData2|**int**|Offset finale dell'istruzione nella stored procedure o nel batch che ha provocato la ricompilazione. L'offset finale è -1 se l'istruzione rappresenta l'ultima istruzione nel batch.|55|Sì|  
|IsSystem|**int**|Indica se l'evento è stato generato per un processo di sistema o un processo utente. 1 = sistema, 0 = utente.|60|Sì|  
|LoginName|**nvarchar**|Nome dell'account di accesso dell'utente (account di accesso di sicurezza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o credenziali di accesso di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows nel formato DOMINIO\nomeutente).|11|Sì|  
|LoginSid|**image**|ID di sicurezza (SID) dell'utente connesso. Queste informazioni sono disponibili nella vista del catalogo sys.server_principals. Il SID è univoco per ogni account di accesso nel server.|41|Sì|  
|NestLevel|**int**|Livello di nidificazione della stored procedure.|29|Sì|  
|NTDomainName|**nvarchar**|Dominio Windows di appartenenza dell'utente.|7|Sì|  
|NTUserName|**nvarchar**|Nome utente di Windows.|6|Sì|  
|ObjectID|**int**|ID della stored procedure assegnato dal sistema.|22|Sì|  
|ObjectName|**nvarchar**|Nome dell'oggetto che ha attivato la ricompilazione.|34|Sì|  
|ObjectType|**int**|Valore che rappresenta il tipo di oggetto coinvolto nell'evento. Per altre informazioni, vedere [Colonna ObjectType per gli eventi di traccia](../../relational-databases/event-classes/objecttype-trace-event-column.md).|28|Sì|  
|Offset|**int**|Offset iniziale dell'istruzione nella stored procedure o nel batch che ha provocato la ricompilazione.|61|Sì|  
|RequestID|**int**|ID della richiesta contenente l'istruzione.|49|Sì|  
|ServerName|**nvarchar**|Nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracciata.|26|No|  
|SessionLoginName|**nvarchar**|Nome dell'account di accesso dell'utente che ha avviato la sessione. Se ad esempio si stabilisce la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con l'account di accesso Login1 e si esegue un'istruzione con l'account di accesso Login2, SessionLoginName indica Login1 e LoginName indica Login2. In questa colonna sono visualizzati sia gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che quelli di Windows.|64|Sì|  
|SPID|**int**|ID della sessione in cui si è verificato l'evento.|12|Sì|  
|SqlHandle|**varbinary**|Hash a 64 bit basato sul testo di una query ad hoc oppure ID del database e dell'oggetto di un oggetto SQL. È possibile passare questo valore a sys.dm_exec_sql_text per recuperare il testo SQL associato.|63|Sì|  
|StartTime|**datetime**|Ora di inizio dell'evento, se disponibile.|14|Sì|  
|TextData|**ntext**|Testo dell'istruzione Transact-SQL che ha provocato una ricompilazione a livello di istruzione.|1|Sì|  
|TransactionID|**bigint**|ID della transazione assegnato dal sistema.|4|Sì|  
|XactSequence|**bigint**|Token usato per descrivere la transazione corrente.|50|Sì|  
  
## <a name="see-also"></a>Vedere anche  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [Classe di evento SQL:StmtRecompile](../../relational-databases/event-classes/sql-stmtrecompile-event-class.md)  
  
  
