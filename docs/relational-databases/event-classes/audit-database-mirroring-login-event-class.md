---
description: Classe di evento Audit Database Mirroring Login
title: Classe di evento Audit Database Mirroring Login | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- event notifications [SQL Server], database mirroring
- Audit Database Mirroring Login event class
- database mirroring [SQL Server], event notifications
ms.assetid: d0bd436d-aade-4208-a7e5-75cf3b5d0ce9
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: eb9145c2eb8c6d29f70a3c1426c2a35dc717154e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193107"
---
# <a name="audit-database-mirroring-login-event-class"></a>Classe di evento Audit Database Mirroring Login
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crea un evento **Audit Database Mirroring Login** per segnalare messaggi di controllo relativi alla sicurezza del trasporto per il mirroring di database.  
  
## <a name="audit-database-mirroring-login-event-class-data-columns"></a>Colonne di dati della classe di evento Audit Database Mirroring Login  
  
|Colonna di dati|Type|Descrizione|Numero di colonna|Filtrabile|  
|-----------------|----------|-----------------|-------------------|----------------|  
|**ApplicationName**|**nvarchar**|Non utilizzata per questa classe di evento.|10|Sì|  
|**ClientProcessID**|**int**|Non utilizzata per questa classe di evento.|9|Sì|  
|**DatabaseID**|**int**|[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] visualizza il nome del database se la colonna di dati **ServerName** è acquisita nella traccia e il server è disponibile. Determinare il valore per un database utilizzando la funzione DB_ID.|3|Sì|  
|**EventClass**|**int**|Tipo di classe di evento acquisita. Sempre **154** per la classe di evento **Audit Database Mirroring Login**.|27|No|  
|**EventSequence**|**int**|Numero di sequenza dell'evento.|51|No|  
|**EventSubClass**|**int**|Tipo di sottoclasse di evento in cui sono disponibili informazioni aggiuntive su ogni classe di evento. Nella tabella seguente sono elencati i valori di sottoclasse per questo evento.|21|Sì|  
|**FileName**|**nvarchar**|Supporta il metodo di autenticazione configurato nell'endpoint del mirroring del database remoto. Se è disponibile più di un metodo, l'endpoint di accettazione (destinazione) determina quale metodo viene utilizzato per primo. I valori possibili sono:<br /><br /> <br /><br /> **Nessuno**. Non è configurato alcun metodo di autenticazione.<br /><br /> **NTLM**. Richiede un'autenticazione NTLM.<br /><br /> **KERBEROS**. Richiede un'autenticazione Kerberos.<br /><br /> **NEGOTIATE**. Il metodo di autenticazione viene negoziato da Windows.<br /><br /> **CERTIFICATE**. Richiede il certificato configurato per l'endpoint, archiviato nel database **master** .<br /><br /> **NTLM, CERTIFICATE**. Accetta un'autenticazione NTLM o il certificato di autenticazione dell'endpoint.<br /><br /> **KERBEROS, CERTIFICATE**. Accetta un'autenticazione Kerberos o il certificato di autenticazione dell'endpoint.<br /><br /> **NEGOTIATE, CERTIFICATE**. Il metodo di autenticazione viene negoziato da Windows oppure per l'autenticazione può essere utilizzato un certificato dell'endpoint.<br /><br /> **CERTIFICATE, NTLM**. Accetta un certificato dell'endpoint o un'autenticazione NTLM.<br /><br /> **CERTIFICATE, KERBEROS**. Accetta il certificato di un endpoint o l'autenticazione Kerberos.<br /><br /> **CERTIFICATE, NEGOTIATE**. Accetta un certificato di autenticazione dell'endpoint o il metodo di autenticazione viene negoziato da Windows.|36|No|  
|**HostName**|**nvarchar**|Non utilizzata per questa classe di evento.|8|Sì|  
|**IsSystem**|**int**|Indica se l'evento è stato generato per un processo di sistema o un processo utente. 1 = sistema, 0 = utente.|60|No|  
|**LoginSid**|**image**|ID di sicurezza (SID) dell'utente connesso. Il SID è univoco per ogni account di accesso nel server.|41|Sì|  
|**NTDomainName**|**nvarchar**|Dominio di Windows a cui appartiene l'utente.|7|Sì|  
|**NTUserName**|**nvarchar**|Nome dell'utente proprietario della connessione che ha generato questo evento.|6|Sì|  
|**ObjectName**|**nvarchar**|La stringa di connessione utilizzata per questa connessione.|34|No|  
|**OwnerName**|**nvarchar**|Supporta il metodo di autenticazione configurato nell'endpoint del mirroring del database locale. Se è disponibile più di un metodo, l'endpoint di accettazione (destinazione) determina quale metodo viene utilizzato per primo. I valori possibili sono:<br /><br /> <br /><br /> **Nessuno**. Non è configurato alcun metodo di autenticazione.<br /><br /> **NTLM**. Richiede un'autenticazione NTLM.<br /><br /> **KERBEROS**. Richiede un'autenticazione Kerberos.<br /><br /> **NEGOTIATE**. Il metodo di autenticazione viene negoziato da Windows.<br /><br /> **CERTIFICATE**. Richiede il certificato configurato per l'endpoint, archiviato nel database **master** .<br /><br /> **NTLM, CERTIFICATE**. Accetta un'autenticazione NTLM o il certificato di autenticazione dell'endpoint.<br /><br /> **KERBEROS, CERTIFICATE**. Accetta un'autenticazione Kerberos o il certificato di autenticazione dell'endpoint.<br /><br /> **NEGOTIATE, CERTIFICATE**. Il metodo di autenticazione viene negoziato da Windows oppure per l'autenticazione può essere utilizzato un certificato dell'endpoint.<br /><br /> **CERTIFICATE, NTLM**. Accetta un certificato dell'endpoint o un'autenticazione NTLM.<br /><br /> **CERTIFICATE, KERBEROS**. Accetta il certificato di un endpoint o l'autenticazione Kerberos.<br /><br /> **CERTIFICATE, NEGOTIATE**. Accetta un certificato di autenticazione dell'endpoint o il metodo di autenticazione viene negoziato da Windows.|37|No|  
|**ProviderName**|**nvarchar**|Metodo di autenticazione utilizzato per questa connessione.|46|No|  
|**RoleName**|**nvarchar**|Ruolo della connessione. I valori possibili sono **initiator** o **target**.|38|No|  
|**ServerName**|**nvarchar**|Nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracciata.|26|No|  
|**SPID**|**int**|ID del processo server assegnato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al processo associato al client.|12|Sì|  
|**StartTime**|**datetime**|Ora di inizio dell'evento, se disponibile.|14|Sì|  
|**State**|**int**|Indica la posizione che ha generato l'evento all'interno del codice sorgente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ogni punto che può generare questo evento è contraddistinto da un codice di stato diverso. Questo codice di stato consente al supporto tecnico Microsoft di individuare la posizione in cui è stato generato l'evento.|30|No|  
|**TargetUserName**|**nvarchar**|Stato di accesso. Uno dei valori possibili:<br /><br /> **INITIAL**<br /><br /> **WAIT LOGIN NEGOTIATE**<br /><br /> **ONE ISC**<br /><br /> **ONE ASC**<br /><br /> **TWO ISC**<br /><br /> **TWO ASC**<br /><br /> **WAIT ISC Confirm**<br /><br /> **WAIT ASC Confirm**<br /><br /> **WAIT REJECT**<br /><br /> **WAIT PRE-MASTER SECRET**<br /><br /> **WAIT VALIDATION**<br /><br /> **WAIT ARBITRATION**<br /><br /> **ONLINE**<br /><br /> **ERROR**<br /><br /> <br /><br /> Nota: ISC = Initiate Security Context. ASC = Accept Security Context.|39|No|  
|**TransactionID**|**bigint**|ID della transazione assegnato dal sistema.|4|No|  
  
 Nella tabella seguente sono elencati i valori di sottoclasse per questa classe di evento.  
  
|ID|Sottoclasse|Descrizione|  
|--------|--------------|-----------------|  
|1|Login Success|Un evento Login Success segnala che il processo di accesso per il mirroring del database adiacente è stato completato correttamente.|  
|2|Login Protocol Error|Un evento Login Protocol Error segnala che durante l'accesso per il mirroring del database è stato ricevuto un messaggio di formato corretto ma non valido per lo stato corrente del processo di accesso. Questo messaggio potrebbe essere andato perduto o potrebbe essere stato inviato fuori sequenza.|  
|3|Message Format Error|Un evento Message Format Error segnala che durante l'accesso per il mirroring del database è stato ricevuto un messaggio non conforme al formato previsto. Il messaggio potrebbe essere stato danneggiato oppure un programma diverso da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] potrebbe inviare messaggi alla porta utilizzata per il mirroring del database.|  
|4|Negotiate Failure|Un evento Negotiate Failure segnala che l'endpoint del mirroring del database locale e l'endpoint del mirroring del database remoto supportano livelli di autenticazione che si escludono a vicenda.|  
|5|Authentication Failure|Un evento Authentication Failure segnala che un endpoint del mirroring del database non può eseguire l'autenticazione per la connessione in seguito a un errore. Per l'autenticazione di Windows, questo evento segnala che l'endpoint del mirroring del database non è in grado di utilizzare l'autenticazione di Windows. Per l'autenticazione basata su certificati, questo evento segnala che l'endpoint del mirroring del database non è in grado di accedere al certificato.|  
|6|Errore di autorizzazione|Un evento Authorization Failure segnala che un endpoint del mirroring del database ha negato l'autorizzazione per la connessione. Per l'autenticazione di Windows, questo evento segnala che l'ID di sicurezza per la connessione non corrisponde a un utente del database. Per l'autenticazione basata su certificati, questo evento segnala che la chiave pubblica recapitata nel messaggio non corrisponde a un certificato contenuto nel database **master** .|  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md)   
 [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [Mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)  
  
  
