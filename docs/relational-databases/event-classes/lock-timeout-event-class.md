---
title: Classe di evento Lock:Timeout | Microsoft Docs
description: La classe di evento Lock:Timeout segnala il timeout di una richiesta di blocco su una risorsa dovuto alla presenza di un blocco sulla risorsa in SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Timeout event class
ms.assetid: 8492f4be-4ea9-4059-80e0-9e7b71597da9
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ec230556d518a6f84b7e0ad6266a68c383921a5f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212002"
---
# <a name="locktimeout-event-class"></a>Classe di evento Lock:Timeout
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  La classe di evento Lock:Timeout segnala il timeout di una richiesta di blocco su una risorsa, ad esempio una pagina, dovuto alla presenza di un blocco su tale risorsa mantenuto attivo da un'altra transazione. Il valore di timeout è determinato dalla funzione di sistema @@LOCK_TIMEOUT e può essere impostato tramite l'istruzione SET LOCK_TIMEOUT.  
  
 La classe di evento Lock:Timeout consente di eseguire il monitoraggio delle condizioni di timeout. In questo modo è possibile determinare se i timeout influiscono in modo significativo sulle prestazioni dell'applicazione, nonché individuare gli oggetti coinvolti. È quindi possibile esaminare il codice dell'applicazione che modifica tali oggetti per verificare se possono essere apportate modifiche che riducano i timeout.  
  
 Gli eventi Lock:Timeout di durata pari a 0 corrispondono in genere al risultato di probe per i blocchi interni e non indicano necessariamente la presenza di un problema. Per ignorare i timeout di durata pari a 0, è possibile utilizzare l'evento Lock:Timeout (timeout > 0).  
  
## <a name="locktimeout-event-class-data-columns"></a>Colonne di dati della classe di evento Lock:Timeout  
  
|Nome colonna di dati|Tipo di dati|Descrizione|ID colonna|Filtrabile|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|Nome dell'applicazione client in cui è stata creata la connessione a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questa colonna viene popolata con i valori passati dall'applicazione e non con il nome visualizzato del programma.|10|Sì|  
|BinaryData|**image**|Identificatore della risorsa blocco.|2|Sì|  
|ClientProcessID|**int**|ID assegnato dal computer host al processo in cui è in esecuzione l'applicazione client. Questa colonna di dati viene popolata se tramite il client viene indicato l'ID del processo client.|9|Sì|  
|DatabaseID|**int**|ID del database in cui si è verificato il timeout di blocco. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] visualizza il nome del database se la colonna di dati ServerName è acquisita nella traccia e il server è disponibile. Determinare il valore per un database utilizzando la funzione DB_ID.|3|Sì|  
|DatabaseName|**nvarchar**|Nome del database in cui si è verificato il timeout.|35|Sì|  
|Duration|**bigint**|Intervallo in microsecondi tra la richiesta del blocco e il timeout.|13|Sì|  
|EndTime|**datetime**|Ora di fine dell'evento.|15|Sì|  
|EventClass|**int**|Tipo di evento = 27.|27|No|  
|EventSequence|**int**|Sequenza di un determinato evento all'interno della richiesta.|51|No|  
|GroupID|**int**|ID del gruppo del carico di lavoro in cui viene generato l'evento di Traccia SQL.|66|Sì|  
|HostName|**nvarchar**|Nome del computer in cui viene eseguito il client. Questa colonna di dati viene popolata se il client fornisce il nome host. Per determinare il nome host, usare la funzione HOST_NAME.|8|Sì|  
|IntegerData2|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|55|Sì|  
|IsSystem|**int**|Indica se l'evento è stato generato per un processo di sistema o un processo utente. 1 = sistema, 0 = utente.|60|Sì|  
|LoginName|**nvarchar**|Nome dell'account di accesso dell'utente (account di accesso di sicurezza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o credenziali di accesso di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows nel formato DOMINIO\nomeutente).|11|Sì|  
|LoginSid|**image**|ID di sicurezza (SID) dell'utente connesso. Queste informazioni sono disponibili nella vista del catalogo sys.server_principals. Il SID è univoco per ogni account di accesso nel server.|41|Sì|  
|Mode|**int**|Modalità risultante dopo il timeout.<br /><br /> 0=NULL - Compatibile con tutte le altre modalità di blocco (LCK_M_NL)<br /><br /> 1=Blocco di stabilità dello schema (LCK_M_SCH_S)<br /><br /> 1=Blocco di modifica dello schema (LCK_M_SCH_M)<br /><br /> 3=Blocco condiviso (LCK_M_S)<br /><br /> 4=Blocco di aggiornamento (LCK_M_U)<br /><br /> 5=Blocco esclusivo (LCK_M_X)<br /><br /> 6=Blocco condiviso preventivo (LCK_M_IS)<br /><br /> 7=Blocco di aggiornamento preventivo (LCK_M_IU)<br /><br /> 8=Blocco esclusivo preventivo (LCK_M_IX)<br /><br /> 9=Condiviso-Preventivo-Aggiornamento (LCK_M_SIU)<br /><br /> 10=Condiviso-Preventivo-Esclusivo (LCK_M_SIX)<br /><br /> 10=Aggiornamento-Preventivo-Esclusivo (LCK_M_SIX)<br /><br /> 12=Blocco aggiornamenti bulk (LCK_M_BU)<br /><br /> 13=Intervalli di chiavi-Condiviso/Condiviso (LCK_M_RS_S)<br /><br /> 14=Intervalli di chiavi-Condiviso/Aggiornamento (LCK_M_RS_U)<br /><br /> 15=Intervalli di chiavi-Inserimento-NULL (LCK_M_RI_NL)<br /><br /> 16=Intervalli di chiavi-Inserimento-Condiviso (LCK_M_RI_S)<br /><br /> 17=Intervalli di chiavi-Inserimento-Aggiornamento (LCK_M_RI_U)<br /><br /> 18=Intervalli di chiavi-Inserimento-Esclusivo (LCK_M_RI_X)<br /><br /> 19=Intervalli di chiavi-Esclusivo-Condiviso (LCK_M_RX_S)<br /><br /> 20=Intervalli di chiavi-Esclusivo-Aggiornamento (LCK_M_RX_U)<br /><br /> 21=Intervalli di chiavi-Esclusivo-Esclusivo (LCK_M_RX_X)|32|Sì|  
|NTDomainName|**nvarchar**|Dominio Windows di appartenenza dell'utente.|7|Sì|  
|NTUserName|**nvarchar**|Nome utente di Windows.|6|Sì|  
|ObjectID|**int**|ID dell'oggetto per il quale si è verificato il timeout, se disponibile e applicabile.|22|Sì|  
|ObjectID2|**bigint**|ID dell'entità o dell'oggetto correlato, se disponibile e applicabile.|56|Sì|  
|OwnerID|**int**|1=TRANSACTION<br /><br /> 2=CURSOR<br /><br /> 3=SESSION<br /><br /> 4=SHARED_TRANSACTION_WORKSPACE<br /><br /> 5=EXCLUSIVE_TRANSACTION_WORKSPACE|58|Sì|  
|RequestID|**int**|ID della richiesta contenente l'istruzione.|49|Sì|  
|ServerName|**nvarchar**|Nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracciata.|26|No|  
|SessionLoginName|**nvarchar**|Nome dell'account di accesso dell'utente che ha avviato la sessione. Se ad esempio si stabilisce la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con l'account di accesso Login1 e si esegue un'istruzione con l'account di accesso Login2, SessionLoginName indica Login1 e LoginName indica Login2. In questa colonna sono visualizzati sia gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che quelli di Windows.|64|Sì|  
|SPID|**int**|ID della sessione in cui si è verificato l'evento.|12|Sì|  
|StartTime|**datetime**|Ora di inizio dell'evento, se disponibile.|14|Sì|  
|TextData|**ntext**|Valore di testo che dipende dal tipo di blocco in fase di acquisizione nel momento in cui si è verificato il timeout.|1|Sì|  
|TransactionID|**bigint**|ID della transazione assegnato dal sistema.|4|Sì|  
|Type|**int**|1=NULL_RESOURCE<br /><br /> 2=DATABASE<br /><br /> 3=FILE<br /><br /> 5=OBJECT<br /><br /> 6=PAGE<br /><br /> 7=KEY<br /><br /> 8=EXTENT<br /><br /> 9=RID<br /><br /> 10=APPLICATION<br /><br /> 11=METADATA<br /><br /> 12=AUTONAMEDB<br /><br /> 13=HOBT<br /><br /> 14=ALLOCATION_UNIT|57|Sì|  
  
## <a name="see-also"></a>Vedere anche  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [Classe di evento Lock:Timeout &#40;timeout &#62; 0&#41;](../../relational-databases/event-classes/lock-timeout-timeout-0-event-class.md)   
 [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)  
  
  
