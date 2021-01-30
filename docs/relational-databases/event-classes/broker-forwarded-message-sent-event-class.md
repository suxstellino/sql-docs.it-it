---
description: Broker:Forwarded Message Sent - classe di evento
title: Classe di evento Broker:Forwarded Message Sent | Microsoft Docs
ms.custom: ''
ms.date: 05/24/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Broker:Forwarded Message Sent event class
ms.assetid: d0ef74d9-a4ef-4918-aa21-6b267e85569f
author: stevestein
ms.author: sstein
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: eb714e530973b719cf89a860534af7799ad31d31
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205691"
---
# <a name="brokerforwarded-message-sent-event-class"></a>Broker:Forwarded Message Sent - classe di evento

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un evento Broker:Forwarded Message Sent quando viene inoltrato un messaggio da Service Broker.  
  
## <a name="brokerforwarded-message-sent-event-class-data-columns"></a>Colonne di dati della classe di evento Broker:Forwarded Message Sent  
  
|Colonna di dati|Type|Descrizione|Numero di colonna|Filtrabile|  
|-----------------|----------|-----------------|-------------------|----------------|  
|ApplicationName|**nvarchar**|Nome dell'applicazione client in cui è stata creata la connessione a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questa colonna viene popolata con i valori passati dall'applicazione e non con il nome visualizzato del programma.|10|Sì|  
|BigintData1|**bigint**|Numero di sequenza del messaggio.|52|No|  
|ClientProcessID|**int**|ID assegnato dal computer host al processo in cui è in esecuzione l'applicazione client. Questa colonna di dati viene popolata se l'ID del processo client viene fornito dal client.|9|Sì|  
|DatabaseID|**int**|ID del database specificato dall'istruzione USE *database* oppure ID del database predefinito, se per una determinata istanza non viene eseguita alcuna istruzione USE *database*. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] viene visualizzato il nome del database se la colonna di dati ServerName viene acquisita nella traccia e il server è disponibile. Determinare il valore per un database utilizzando la funzione DB_ID.|3|Sì|  
|DBUserName|**nvarchar**|ID dell'istanza di Service Broker per il servizio da cui proviene il messaggio.|40|No|  
|EventClass|**int**|Tipo di classe di evento acquisita. Per Broker:Forwarded Message Sent, corrisponde sempre a 139.|27|No|  
|EventSequence|**int**|Numero di sequenza dell'evento.|51|No|  
|FileName|**nvarchar**|Nome del servizio a cui è destinato il messaggio.|36|No|  
|GUID|**uniqueidentifier**|ID di conversazione del dialogo. Questo identificatore viene trasmesso come parte del messaggio e viene condiviso da entrambi i lati della conversazione.|54|No|  
|HostName|**nvarchar**|Nome del computer in cui è in esecuzione il client. Questa colonna di dati viene popolata se il nome host viene fornito dal client. Per determinare il nome host, usare la funzione HOST_NAME.|8|Sì|  
|IndexID|**int**|Numero di hop rimanenti per il messaggio inoltrato.|24|No|  
|IntegerData|**int**|Numero di frammento del messaggio inoltrato.|25|No|  
|IsSystem|**int**|Indica se l'evento è stato generato per un processo di sistema o un processo utente. 1 = sistema, 0 = utente.|60|No|  
|LoginSid|**image**|ID di sicurezza (SID) dell'utente connesso. Il SID è univoco per ogni account di accesso nel server.|41|Sì|  
|NTDomainName|**nvarchar**|Dominio di Windows a cui appartiene l'utente.|7|Sì|  
|NTUserName|**nvarchar**|Nome dell'utente proprietario della connessione che ha generato questo evento.|6|Sì|  
|ObjectId|**int**|Valore di durata (TTL) del messaggio inoltrato al momento dell'inoltro.|22|No|  
|ObjectName|**nvarchar**|ID del messaggio inoltrato.|34|No|  
|OwnerName|**nvarchar**|Identificatore dell'istanza di Service Broker a cui è indirizzato il messaggio.|37|No|  
|RoleName|**nvarchar**|Ruolo dell'handle di conversazione. I valori validi sono:<br /><br /> Initiator. Istanza di Service Broker che ha iniziato la conversazione.<br /><br /> Destinazione. Istanza di Service Broker che funge da destinazione della conversazione.|38|No|  
|ServerName|**nvarchar**|Nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracciata.|26|No|  
|SPID|**int**|ID del processo server assegnato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al processo associato al client.|12|Sì|  
|StartTime|**datetime**|Ora di inizio dell'evento, se disponibile.|14|Sì|  
|Operazione riuscita|**int**|Quantità di tempo utilizzata durante il processo di inoltro.|23|No|  
|TargetLoginName|**nvarchar**|Indirizzo di rete a cui l'istanza ha inviato il messaggio. Si noti che questo indirizzo può essere diverso dalla destinazione finale del messaggio.|42|No|  
|TargetUserName|**nvarchar**|Nome del servizio di origine per il messaggio.|39|No|  
|TransactionID|**bigint**|ID della transazione assegnato dal sistema.|4|No|  
  
  
