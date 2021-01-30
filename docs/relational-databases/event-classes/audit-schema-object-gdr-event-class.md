---
description: Audit Schema Object GDR - classe di evento
title: Classe di evento Audit Schema Object GDR | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Audit Schema Object GDR event class
ms.assetid: a0187811-dc71-4792-a282-3bfe1ca90c21
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ee41c341294720193824844dad6bcd5d639b1295
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198619"
---
# <a name="audit-schema-object-gdr-event-class"></a>Audit Schema Object GDR - classe di evento
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La classe di evento **Audit Schema Object GDR** si verifica ogni volta che viene generata un'istruzione GRANT, REVOKE o DENY relativa all'autorizzazione per un oggetto dello schema da uno degli utenti di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="audit-schema-object-gdr-event-class-data-columns"></a>Colonne di dati della classe di evento Audit Schema Object GDR  
  
|Nome colonna di dati|Tipo di dati|Descrizione|ID colonna|Filtrabile|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**ApplicationName**|**nvarchar**|Nome dell'applicazione client in cui è stata creata la connessione a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questa colonna viene popolata con i valori passati dall'applicazione e non con il nome visualizzato del programma.|10|Sì|  
|**ClientProcessID**|**int**|ID assegnato dal computer host al processo in cui è in esecuzione l'applicazione client. Questa colonna di dati viene popolata se tramite il client viene indicato l'ID del processo client.|9|Sì|  
|**ColumnPermissions**|**int**|Indicatore dell'impostazione o meno di un'autorizzazione per la colonna. Analizzare il testo dell'istruzione per determinare con esattezza quali autorizzazioni sono state impostate per quali colonne. 1 = Sì, 0 = No.|44|Sì|  
|**DatabaseID**|**int**|ID del database specificato nell'istruzione di *database* USE oppure il database predefinito se per un'istanza specifica l'istruzione di *database* USE non è stata eseguita. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] visualizza il nome del database se la colonna di dati **ServerName** è acquisita nella traccia e il server è disponibile. Determinare il valore per un database utilizzando la funzione DB_ID.|3|Sì|  
|**DatabaseName**|**nvarchar**|Nome del database nel quale viene eseguita l'istruzione dell'utente.|35|Sì|  
|**DBUserName**|**nvarchar**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del client.|40|Sì|  
|**EventClass**|**int**|Tipo di evento = 103.|27|No|  
|**EventSequence**|**int**|Sequenza di un determinato evento all'interno della richiesta.|51|No|  
|**EventSubClass**|**int**|Tipo di sottoclasse di evento.<br /><br /> 1=Concedi<br /><br /> 2=Revoca<br /><br /> 3=Nega|21|Sì|  
|**HostName**|**nvarchar**|Nome del computer in cui viene eseguito il client. Questa colonna di dati viene popolata se il nome host viene fornito dal client. Per determinare il nome host, usare la funzione HOST_NAME.|8|Sì|  
|**IsSystem**|**int**|Indica se l'evento è stato generato per un processo di sistema o un processo utente. 1 = sistema, 0 = utente.|60|Sì|  
|**LoginName**|**nvarchar**|Nome dell'account di accesso dell'utente (account di sicurezza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o credenziali di accesso di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows nel formato DOMINIO\nomeutente).|11|Sì|  
|**LoginSid**|**image**|ID di sicurezza (SID) dell'utente connesso. Queste informazioni sono disponibili nella vista del catalogo **sys.server_principals** . Il SID è univoco per ogni account di accesso nel server.|41|Sì|  
|**NTDomainName**|**nvarchar**|Dominio Windows di appartenenza dell'utente.|7|Sì|  
|**NTUserName**|**nvarchar**|Nome utente di Windows.|6|Sì|  
|**ObjectName**|**nvarchar**|Nome dell'oggetto di destinazione dell'istruzione GRANT/REVOKE/DENY.|34|Sì|  
|**ObjectType**|**int**|Valore che rappresenta il tipo di oggetto coinvolto nell'evento. Questo valore corrisponde alla colonna type nella vista del catalogo **sys.objects** . Per i valori, vedere [Colonna ObjectType per gli eventi di traccia](../../relational-databases/event-classes/objecttype-trace-event-column.md).|28|Sì|  
|**OwnerName**|**nvarchar**|Nome utente del database relativo al proprietario dell'oggetto di destinazione dell'istruzione GRANT/REVOKE/DENY.|37|Sì|  
|**ParentName**|**nvarchar**|Nome dello schema in cui è incluso l'oggetto.|59|Sì|  
|**Autorizzazioni**|**bigint**|Valore intero che rappresenta il tipo di autorizzazioni controllato.<br /><br /> 1=SELECT ALL<br /><br /> 2=UPDATE ALL<br /><br /> 4=REFERENCES ALL<br /><br /> 8=INSERT<br /><br /> 16=DELETE<br /><br /> 32=EXECUTE (solo procedure)<br /><br /> 4096=SELECT ANY (almeno una colonna)<br /><br /> 8192=UPDATE ANY<br /><br /> 16384=REFERENCES ANY|19|Sì|  
|**RequestID**|**int**|ID della richiesta contenente l'istruzione.|49|Sì|  
|**ServerName**|**nvarchar**|Nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracciata.|26|No|  
|**SessionLoginName**|**nvarchar**|Nome dell'account di accesso dell'utente che ha avviato la sessione. Se ad esempio si stabilisce la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con l'account di accesso Login1 e si esegue un'istruzione con l'account di accesso Login2, **SessionLoginName** indica Login1 e **LoginName** indica Login2. In questa colonna sono visualizzati sia gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che quelli di Windows.|64|Sì|  
|**SPID**|**int**|ID della sessione in cui si è verificato l'evento.|12|Sì|  
|**StartTime**|**datetime**|Ora di inizio dell'evento, se disponibile.|14|Sì|  
|**Success**|**int**|1 = esito positivo. 0 = esito negativo. Ad esempio, il valore 1 indica che il controllo delle autorizzazioni ha avuto esito positivo e il valore 0 che il controllo ha avuto esito negativo.|23|Sì|  
|**TargetLoginName**|**nvarchar**|Per le azioni relative a un account di accesso (ad esempio l'aggiunta di un nuovo account di accesso), il nome dell'account di accesso specifico.|42|Sì|  
|**TargetLoginSid**|**image**|Per le azioni relative a un account di accesso (ad esempio l'aggiunta di un nuovo account di accesso), l'ID di sicurezza (SID) dell'account di accesso specifico.|43|Sì|  
|**TargetUserName**|**nvarchar**|Per le azioni relative a un utente del database, ad esempio la concessione di un'autorizzazione a un utente, il nome di tale utente.|39|Sì|  
|**TextData**|**ntext**|Valore di testo che dipende dalla classe di evento acquisita nella traccia.|1|Sì|  
|**TransactionID**|**bigint**|ID della transazione assegnato dal sistema.|4|Sì|  
|**XactSequence**|**bigint**|Token usato per descrivere la transazione corrente.|50|Sì|  
  
## <a name="see-also"></a>Vedere anche  
 [Eventi estesi](../../relational-databases/extended-events/extended-events.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)  
  
  
