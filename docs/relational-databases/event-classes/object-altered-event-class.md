---
description: Object:Altered - classe di evento
title: Classe di evento Object:Altered | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Object:Altered event class
ms.assetid: f94e3b59-ff2f-4d8d-8479-e85ce5b3483e
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 71f3ee23c865c153de4626dda24f345a4751b5a5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204564"
---
# <a name="objectaltered-event-class"></a>Object:Altered - classe di evento
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La classe di evento Object:Altered indica che un oggetto è stato modificato, ad esempio mediante un'istruzione ALTER INDEX, ALTER TABLE o ALTER DATABASE. È possibile utilizzare questa classe di evento per determinare se gli oggetti sono stati modificati, ad esempio tramite applicazioni ODBC che spesso creano stored procedure temporanee.  
  
 La classe di evento Object:Altered viene generata sempre con due eventi. Il primo evento indica la fase di inizio, mentre il secondo evento indica la fase di rollback o di commit.  
  
 Mediante il monitoraggio delle colonne di dati LoginName e NTUserName, è possibile determinare il nome dell'utente che crea, elimina o modifica gli oggetti.  
  
## <a name="objectaltered-event-class-data-columns"></a>Colonne di dati della classe di evento Object:Altered  
  
|Nome colonna di dati|Tipo di dati|Descrizione|ID colonna|Filtrabile|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|Nome dell'applicazione client in cui è stata creata la connessione a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questa colonna viene popolata con i valori passati dall'applicazione e non con il nome visualizzato del programma.|10|Sì|  
|ClientProcessID|**int**|ID assegnato dal computer host al processo in cui è in esecuzione l'applicazione client. Questa colonna di dati viene popolata se tramite il client viene indicato l'ID del processo client.|9|Sì|  
|DatabaseID|**int**|ID del database specificato nell'istruzione di *database* USE oppure il database predefinito se per un'istanza specifica l'istruzione di *database* USE non è stata eseguita. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] visualizza il nome del database se la colonna di dati ServerName è acquisita nella traccia e il server è disponibile. Determinare il valore per un database utilizzando la funzione DB_ID.|3|Sì|  
|DatabaseName|**nvarchar**|Nome del database nel quale viene eseguita l'istruzione dell'utente.|35|Sì|  
|EventClass|**int**|Tipo di evento = 164.|27|No|  
|EventSequence|**int**|Sequenza di un determinato evento all'interno della richiesta.|51|No|  
|EventSubClass|**int**|Tipo di sottoclasse di evento.<br /><br /> 0=Inizio<br /><br /> 1=Commit<br /><br /> 2=Rollback|21|Sì|  
|GroupID|**int**|ID del gruppo del carico di lavoro in cui viene generato l'evento di Traccia SQL.|66|Sì|  
|HostName|**nvarchar**|Nome del computer in cui viene eseguito il client. Questa colonna di dati viene popolata se il client fornisce il nome host. Per determinare il nome host, usare la funzione HOST_NAME.|8|Sì|  
|IndexID|**int**|ID dell'indice dell'oggetto interessato dall'evento. Per determinare l'ID dell'indice per un oggetto, utilizzare la colonna index_id della vista del catalogo sys.indexes.|24|Sì|  
|IntegerData|**int**|Numero di sequenza dell'evento corrispondente della fase di inizio. Questa colonna è disponibile solo per la sottoclasse di evento di tipo Commit o Rollback. Sottoclasse.|25|Sì|  
|IsSystem|**int**|Indica se l'evento è stato generato per un processo di sistema o un processo utente. 1 = sistema, NULL = utente.|60|Sì|  
|LoginName|**nvarchar**|Nome dell'account di accesso dell'utente (account di accesso di sicurezza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o credenziali di accesso di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows nel formato DOMINIO\nomeutente).|11|Sì|  
|LoginSid|**image**|ID di sicurezza (SID) dell'utente connesso. Queste informazioni sono disponibili nella vista del catalogo sys.server_principals. Il SID è univoco per ogni account di accesso nel server.|41|Sì|  
|NTDomainName|**nvarchar**|Dominio Windows di appartenenza dell'utente.|7|Sì|  
|NTUserName|**nvarchar**|Nome utente di Windows.|6|Sì|  
|ObjectID|**int**|ID dell'oggetto assegnato dal sistema.|22|Sì|  
|ObjectID2|**bigint**|ID della funzione di partizione in caso di modifica dello schema di partizione, ID della coda in caso di modifica del servizio o ID dello schema di raccolta in caso di modifica dell'XML Schema.|56|Sì|  
|ObjectName|**nvarchar**|Nome dell'oggetto a cui si fa riferimento.|34|Sì|  
|ObjectType|**int**|Valore che rappresenta il tipo di oggetto coinvolto nell'evento. Questo valore corrisponde alla colonna type nella vista del catalogo sys.objects. Per i valori, vedere [Colonna ObjectType per gli eventi di traccia](../../relational-databases/event-classes/objecttype-trace-event-column.md).|28|Sì|  
|RequestID|**int**|ID della richiesta di batch contenente l'istruzione.|49|Sì|  
|ServerName|**nvarchar**|Nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracciata.|26|No|  
|SessionLoginName|**nvarchar**|Nome dell'account di accesso dell'utente che ha avviato la sessione. Se ad esempio si stabilisce la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con l'account di accesso Login1 e si esegue un'istruzione con l'account di accesso Login2, SessionLoginName indica Login1 e LoginName indica Login2. In questa colonna sono visualizzati sia gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che quelli di Windows.|64|Sì|  
|SPID|**int**|ID della sessione in cui si è verificato l'evento.|12|Sì|  
|StartTime|**datetime**|Ora di inizio dell'evento, se disponibile.|14|Sì|  
|TransactionID|**bigint**|ID della transazione assegnato dal sistema.|4|Sì|  
|XactSequence|**bigint**|Token utilizzato per descrivere la transazione corrente.|50|Sì|  
  
## <a name="see-also"></a>Vedere anche  
 [Eventi estesi](../../relational-databases/extended-events/extended-events.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
