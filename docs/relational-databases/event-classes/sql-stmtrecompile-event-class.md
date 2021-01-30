---
description: SQL:StmtRecompile - classe di evento
title: Classe di evento SQL:StmtRecompile | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- SQL:StmtRecompile event class
ms.assetid: 3a134751-3e93-4fe8-bf22-1e0561189293
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: cf9e300b77c76de9e6679144568a1556a041c194
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161467"
---
# <a name="sqlstmtrecompile-event-class"></a>SQL:StmtRecompile - classe di evento
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La classe di evento SQL:StmtRecompile indica ricompilazioni a livello di istruzione causate da tutti i tipi di batch: stored procedure, trigger, batch ad hoc e query. Le query possono essere inviate utilizzando sp_executesql, linguaggio SQL dinamico, metodi Prepare, metodi Execute o interfacce simili. È consigliabile usare la classe di evento SQL:StmtRecompile anziché SP:Recompile.  
  
## <a name="sqlstmtrecompile-event-class-data-columns"></a>Colonne di dati della classe di evento SQL:StmtRecompile  
  
|Nome colonna di dati|Tipo di dati|Descrizione|ID colonna|Filtrabile|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|Nome dell'applicazione client in cui è stata creata la connessione a un'istanza di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questa colonna viene popolata con i valori passati dall'applicazione anziché con il nome visualizzato del programma.|10|Sì|  
|ClientProcessID|**int**|ID assegnato dal computer host al processo in cui è in esecuzione l'applicazione client. Questa colonna di dati viene popolata se il client fornisce l'ID del processo.|9|Sì|  
|DatabaseID|**int**|ID del database nel quale viene eseguita la stored procedure. Determinare il valore per un database utilizzando la funzione DB_ID.|3|Sì|  
|DatabaseName|**nvarchar**|Nome del database nel quale viene eseguita la stored procedure.|35|Sì|  
|EventSequence|**int**|Sequenza di un evento nella richiesta.|51|No|  
|EventSubClass|**int**|Indica il motivo della ricompilazione:<br /><br /> 1 = Schema modificato<br /><br /> 2 = Statistiche modificate<br /><br /> 3 = Compilazione posticipata<br /><br /> 4 = Opzione impostata modificata<br /><br /> 5 = Tabella temporanea modificata<br /><br /> 6 = Set di righe remoto modificato<br /><br /> 7 = Autorizzazioni FOR BROWSE modificate<br /><br /> 8 = Ambiente di notifica query modificato<br /><br /> 9 = Vista partizionata modificata<br /><br /> 10 = Opzioni cursore modificate<br /><br /> 11 = Opzione (RECOMPILE) richiesta|21|Sì|  
|GroupID|**int**|ID del gruppo del carico di lavoro in cui viene generato l'evento di Traccia SQL.|66|Sì|  
|HostName|**nvarchar**|Nome del computer in cui è in esecuzione il client che ha inviato l'istruzione. Questa colonna di dati viene popolata se il client fornisce il nome host. Per determinare il nome host, usare la funzione HOST_NAME.|8|Sì|  
|IntegerData2|**int**|Offset finale dell'istruzione nella stored procedure o nel batch che ha provocato la ricompilazione. L'offset finale è -1 se l'istruzione rappresenta l'ultima istruzione nel batch.|55|Sì|  
|IsSystem|**int**|Indica se l'evento è stato generato per un processo di sistema o un processo utente.<br /><br /> 1 = sistema<br /><br /> 0 = utente|60|Sì|  
|LineNumber|**int**|Numero di sequenza dell'istruzione nel batch, se applicabile.|5|Sì|  
|LoginName|**nvarchar**|Nome dell'account di accesso che ha inviato il batch.|11|Sì|  
|LoginSid|**image**|ID di sicurezza (SID) dell'utente attualmente connesso. Queste informazioni sono disponibili nella vista del catalogo sys.server_principals. Il SID è univoco per ogni account di accesso nel server.|41|Sì|  
|NestLevel|**int**|Livello di nidificazione della chiamata di stored procedure. Ad esempio, la stored procedure my_proc_a chiama my_proc_b. In questo caso, per my_proc_a il valore di NestLevel è 1, mentre per my_proc_b il valore di NestLevel è 2.|29|Sì|  
|NTDomainName|**nvarchar**|Dominio Windows di appartenenza dell'utente.|7|Sì|  
|NTUserName|**nvarchar**|Nome utente di Windows dell'utente connesso.|6|Sì|  
|ObjectID|**int**|Identificativo assegnato dal sistema all'oggetto contenente l'istruzione che ha causato la ricompilazione. Tale oggetto può essere una stored procedure, un trigger o una funzione definita dall'utente. Nel caso di SQL preparato o di batch ad hoc, ObjectID e ObjectName restituiscono un valore NULL.|22|Sì|  
|ObjectName|**nvarchar**|Nome dell'oggetto identificato da ObjectID.|34|Sì|  
|ObjectType|**int**|Valore che rappresenta il tipo di oggetto coinvolto nell'evento. Per altre informazioni, vedere [Colonna ObjectType per gli eventi di traccia](../../relational-databases/event-classes/objecttype-trace-event-column.md).|28|Sì|  
|Offset|**int**|Offset iniziale dell'istruzione nella stored procedure o nel batch che ha provocato la ricompilazione.|61|Sì|  
|RequestID|**int**|ID della richiesta contenente l'istruzione.|49|Sì|  
|ServerName|**nvarchar**|Nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracciata.|26|No|  
|SessionLoginName|**nvarchar**|Nome dell'account di accesso dell'utente che ha avviato la sessione. Se ad esempio si stabilisce la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con l'account di accesso Login1 e si esegue un'istruzione con l'account di accesso Login2, SessionLoginName indica Login1 e LoginName indica Login2. In questa colonna sono visualizzati sia gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che quelli di Windows.|64|Sì|  
|SPID|**int**|ID del processo server della connessione.|12|Sì|  
|SqlHandle|**varbinary**|Hash a 64 bit basato sul testo di una query ad hoc oppure ID del database e dell'oggetto di un oggetto SQL. È possibile passare questo valore a sys.dm_exec_sql_text per recuperare il testo SQL associato.|63|No|  
|StartTime|**datetime**|Ora di inizio dell'evento, se disponibile.|14|Sì|  
|TextData|**ntext**|Testo dell'istruzione Transact-SQL ricompilata|1|Sì|  
|TransactionID|**bigint**|ID della transazione assegnato dal sistema.|4|Sì|  
|XactSequence|**bigint**|Token utilizzato per descrivere la transazione corrente.|50|Sì|  
  
## <a name="see-also"></a>Vedere anche  
 [Classe di evento SP:Recompile](../../relational-databases/event-classes/sp-recompile-event-class.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
