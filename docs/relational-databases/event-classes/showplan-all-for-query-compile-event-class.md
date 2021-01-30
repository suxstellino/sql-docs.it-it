---
description: Showplan All for Query Compile - classe di evento
title: Classe di evento Showplan All for Query Compile | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Showplan All for Query Compile event class
ms.assetid: bb1dc446-5e6c-43d6-9db8-78c76cc2e01f
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9f6e6d5eb3f0c26d664acb5af8edd783792faeeb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99151116"
---
# <a name="showplan-all-for-query-compile-event-class"></a>Showplan All for Query Compile - classe di evento
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La classe di evento Showplan All for Query Compile viene generata quando [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compila un'istruzione SQL. Includere questa classe di evento per identificare gli operatori Showplan. Le informazioni include sono un subset delle informazioni disponibili nella classe di evento Showplan XML For Query Compile.  
  
 Nella classe di evento Showplan All for Query Compile sono visualizzati dati completi relativi alla fase di compilazione e quindi le tracce che includono Showplan All for Query Compile possono determinare significativi overhead delle prestazioni. Per ridurre al minimo tale rischio, limitare l'utilizzo di questa classe di evento alle tracce che consentono di monitorare problemi specifici per brevi periodi di tempo.  
  
 Quando la classe di evento Showplan All for Query Compile è inclusa in una traccia, è necessario che la colonna di dati BinaryData sia selezionata. In caso contrario, le informazioni relative a questa classe di evento non verranno visualizzate nella traccia.  
  
## <a name="showplan-all-for-query-compile-event-class-data-columns"></a>Colonne di dati della classe di evento Showplan All for Query Compile  
  
|Nome colonna di dati|Tipo di dati|Descrizione|ID colonna|Filtrabile|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|Nome dell'applicazione client in cui è stata creata la connessione a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questa colonna viene popolata con i valori passati dall'applicazione e non con il nome visualizzato del programma.|10|Sì|  
|BinaryData|**image**|Costo stimato della query.|2|No|  
|ClientProcessID|**int**|ID assegnato dal computer host al processo in cui è in esecuzione l'applicazione client. Questa colonna di dati viene popolata se l'ID del processo client viene fornito dal client.|9|Sì|  
|DatabaseID|**int**|ID del database specificato nell'istruzione di *database* USE oppure il database predefinito se per un'istanza specifica l'istruzione di *database* USE non è stata eseguita. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] visualizza il nome del database se la colonna di dati ServerName è acquisita nella traccia e il server è disponibile. Determinare il valore per un database utilizzando la funzione DB_ID.|3|Sì|  
|DatabaseName|**nvarchar**|Nome del database nel quale viene eseguita l'istruzione dell'utente.|35|Sì|  
|EventClass|**int**|Tipo di evento = 169.|27|No|  
|EventSequence|**int**|Sequenza di un determinato evento all'interno della richiesta.|51|No|  
|GroupID|**int**|ID del gruppo del carico di lavoro in cui viene generato l'evento di Traccia SQL.|66|Sì|  
|HostName|**nvarchar**|Nome del computer in cui viene eseguito il client. Questa colonna di dati viene popolata se il nome host viene fornito dal client. Per determinare il nome host, usare la funzione HOST_NAME.|8|Sì|  
|IntegerData|**int**|Numero stimato di righe restituite.|25|Sì|  
|IsSystem|**int**|Indica se l'evento è stato generato per un processo di sistema o un processo utente. 1 = sistema, 0 = utente.|60|Sì|  
|LineNumber|**int**|Visualizza il numero della riga contenente l'errore.|5|Sì|  
|LoginName|**nvarchar**|Nome dell'account di accesso dell'utente (account di sicurezza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o credenziali di accesso di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows nel formato DOMINIO\nomeutente).|11|Sì|  
|LoginSID|**image**|ID di sicurezza (SID) dell'utente connesso. Queste informazioni sono disponibili nella vista del catalogo sys.server_principals. Il SID è univoco per ogni account di accesso nel server.|41|No|  
|NestLevel|**int**|Valore intero che rappresenta i dati restituiti da @@NESTLEVEL.|29|Sì|  
|NTDomainName|**nvarchar**|Dominio Windows di appartenenza dell'utente.|7|Sì|  
|NTUserName|**nvarchar**|Nome utente di Windows.|6|Sì|  
|ObjectID|**int**|ID dell'oggetto assegnato dal sistema.|22|Sì|  
|ObjectName|**nvarchar**|Nome dell'oggetto a cui si fa riferimento.|34|Sì|  
|ObjectType|**int**|Valore che rappresenta il tipo di oggetto coinvolto nell'evento. Questo valore corrisponde alla colonna type nella tabella sys.objects. Per i valori, vedere [Colonna ObjectType per gli eventi di traccia](../../relational-databases/event-classes/objecttype-trace-event-column.md).|28|Sì|  
|RequestID|**int**|ID della richiesta contenente l'istruzione.|49|Sì|  
|ServerName|**nvarchar**|Nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracciata.|26|No|  
|SessionLoginName|**nvarchar**|Nome dell'account di accesso dell'utente che ha avviato la sessione. Se ad esempio si stabilisce la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con l'account di accesso Login1 e si esegue un'istruzione con l'account di accesso Login2, SessionLoginName indica Login1 e LoginName indica Login2. In questa colonna sono visualizzati sia gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che quelli di Windows.|64|Sì|  
|SPID|**int**|ID del processo server assegnato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al processo associato al client.|12|Sì|  
|StartTime|**datetime**|Ora di inizio dell'evento, se disponibile.|14|Sì|  
|TransactionID|**bigint**|ID della transazione assegnato dal sistema.|4|Sì|  
|XactSequence|**bigint**|Token usato per descrivere la transazione corrente.|50|Sì|  
  
## <a name="see-also"></a>Vedere anche  
 [Eventi estesi](../../relational-databases/extended-events/extended-events.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [Guida di riferimento a operatori Showplan logici e fisici](../../relational-databases/showplan-logical-and-physical-operators-reference.md)  
  
  
