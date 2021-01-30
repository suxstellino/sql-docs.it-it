---
description: Broker:Remote Message Ack - classe di evento
title: Classe di evento Broker:Remote Message Ack | Microsoft Docs
ms.custom: ''
ms.date: 05/24/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Broker:Remote Message Ack event class
ms.assetid: 3d67efe1-74b4-4633-b029-c6e05b19f4dc
author: stevestein
ms.author: sstein
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ef4d192eac8b973285039bbd0d7573a9aa934bc2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99110549"
---
# <a name="brokerremote-message-ack-event-class"></a>Broker:Remote Message Ack - classe di evento

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un evento **Broker:Remote Message Ack** quando [!INCLUDE[ssSB](../../includes/sssb-md.md)] invia o riceve l'acknowledgement di un messaggio.  
  
## <a name="brokerremote-message-ack-event-class-data-columns"></a>Colonne di dati della classe di evento Broker:Remote Message Ack  
  
|Colonna di dati|Type|Descrizione|Numero di colonna|Filtrabile|  
|-----------------|----------|-----------------|-------------------|----------------|  
|**ApplicationName**|**nvarchar**|Nome dell'applicazione client in cui è stata creata la connessione a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questa colonna viene popolata con i valori passati dall'applicazione anziché con il nome visualizzato del programma.|10|Sì|  
|**BigintData1**|**bigint**|Numero di sequenza del messaggio che contiene l'acknowledgement.|52|No|  
|**BigintData2**|**bigint**|Numero di sequenza del messaggio che contiene l'acknowledgement.|53|No|  
|**ClientProcessID**|**int**|ID assegnato dal computer host al processo in cui è in esecuzione l'applicazione client. Questa colonna di dati viene popolata se l'ID del processo client viene fornito dal client.|9|Sì|  
|**DatabaseID**|**int**|ID del database specificato dall'istruzione USE *database* oppure ID del database predefinito, se per una determinata istanza non viene eseguita un'istruzione USE *database* . [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] visualizza il nome del database se la colonna di dati **ServerName** è acquisita nella traccia e il server è disponibile. Determinare il valore per un database utilizzando la funzione DB_ID.|3|Sì|  
|**EventClass**|**int**|Tipo di classe di evento acquisita. Per **Broker:Message Ack** , corrisponde sempre a **149**.|27|No|  
|**EventSequence**|**int**|Numero di sequenza dell'evento.|51|No|  
|**EventSubClass**|**nvarchar**|Tipo di sottoclasse di evento che offre maggiori informazioni su ogni classe di evento. Questa colonna può contenere i valori seguenti:<br /><br /> **Message With Acknowledgement Sent**:<br />                    [!INCLUDE[ssSB](../../includes/sssb-md.md)] ha inviato un acknowledgement come parte di un normale messaggio in sequenza.<br /><br /> **Acknowledgement Sent**:<br />                    [!INCLUDE[ssSB](../../includes/sssb-md.md)] ha inviato un acknowledgement al di fuori di un normale messaggio in sequenza.<br /><br /> **Message With Acknowledgement Received**:<br />                  [!INCLUDE[ssSB](../../includes/sssb-md.md)] ha ricevuto un acknowledgement come parte di un normale messaggio in sequenza.<br /><br /> **Acknowledgement Received**:<br />                  [!INCLUDE[ssSB](../../includes/sssb-md.md)] ha ricevuto un acknowledgement al di fuori di un messaggio in sequenza.|21|Sì|  
|**GUID**|**uniqueidentifier**|ID della conversazione della finestra. Questo identificatore viene trasmesso come parte del messaggio e viene condiviso da entrambi i lati della conversazione.|54|No|  
|**HonorBrokerPriority**|**Int**|Valore corrente dell'opzione di database HONOR_BROKER_PRIORITY: 0 = OFF, 1 = ON.|32|Sì|  
|**HostName**|**nvarchar**|Nome del computer in cui è in esecuzione il client. Questa colonna di dati viene popolata se il nome host viene fornito dal client. Per determinare il nome host, usare la funzione HOST_NAME.|8|Sì|  
|**IntegerData**|**int**|Numero del frammento del messaggio che contiene l'acknowledgement.|25|No|  
|**IntegerData2**|**int**|Numero del frammento del messaggio per il quale è stato inviato o ricevuto l'acknowledgement.|55|No|  
|**IsSystem**|**int**|Indica se l'evento è stato generato per un processo di sistema o un processo utente.<br /><br /> 0 = utente<br /><br /> 1 = sistema|60|No|  
|**LoginSid**|**image**|ID di sicurezza (SID) dell'utente connesso. Il SID è univoco per ogni account di accesso nel server.|41|Sì|  
|**NTDomainName**|**nvarchar**|Dominio di Windows a cui appartiene l'utente.|7|Sì|  
|**NTUserName**|**nvarchar**|Nome dell'utente proprietario della connessione che ha generato questo evento.|6|Sì|  
|**Priorità**|**int**|Livello di priorità della conversazione.|5|Sì|  
|**RoleName**|**nvarchar**|Ruolo dell'istanza che invia o riceve l'acknowledgement del messaggio. I valori possibili sono **initiator** o **target**.|38|No|  
|**ServerName**|**nvarchar**|Nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracciata.|26|No|  
|**SPID**|**int**|ID del processo server assegnato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al processo associato al client.|12|Sì|  
|**StartTime**|**datetime**|Ora di inizio dell'evento, se disponibile.|14|Sì|  
|**StarvationElevation**|**int**|Il messaggio è stato inviato con una priorità più elevata rispetto alla priorità configurata per la conversazione: 0 = falso, 1 = vero.|33|Sì|  
|**TransactionID**|**bigint**|ID della transazione assegnato dal sistema.|4|No|  
  
  
