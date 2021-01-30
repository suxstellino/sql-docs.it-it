---
description: Broker:Conversation - classe di evento
title: Classe di evento Broker:Conversation | Microsoft Docs
ms.custom: ''
ms.date: 05/24/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Broker:Conversation event class
ms.assetid: 784707b5-cc67-46a3-8ae6-8f8ecf4b27c0
author: stevestein
ms.author: sstein
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 32e74cf95915559a1d8d5e751b79f096d4bb59ce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187468"
---
# <a name="brokerconversation-event-class"></a>Broker:Conversation - classe di evento

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un evento **Broker:Conversation** per indicare lo stato di una conversazione di Service Broker.  
  
## <a name="brokerconversation-event-class-data-columns"></a>Colonne di dati della classe di evento Broker:Conversation  
  
|Colonna di dati|Type|Descrizione|Numero di colonna|Filtrabile|  
|-----------------|----------|-----------------|-------------------|----------------|  
|**ApplicationName**|**nvarchar**|Nome dell'applicazione client in cui è stata creata la connessione a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questa colonna viene popolata con i valori passati dall'applicazione al posto del nome visualizzato del programma.|10|Sì|  
|**ClientProcessID**|**int**|ID assegnato dal computer host al processo in cui è in esecuzione l'applicazione client. Questa colonna di dati viene popolata se l'ID del processo client viene fornito dal client.|9|Sì|  
|**DatabaseID**|**int**|ID del database specificato dall'istruzione USE *database* oppure ID del database predefinito, se non è stata eseguita un'istruzione USE *database*. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] visualizza il nome del database se la colonna di dati **ServerName** è acquisita nella traccia e il server è disponibile. È possibile determinare il valore per un database tramite la funzione **DB_ID** .|3|Sì|  
|**EventClass**|**int**|Tipo di classe di evento acquisita. Per **Broker:Conversation** è sempre **124**.|27|No|  
|**EventSequence**|**int**|Numero di sequenza dell'evento.|51|No|  
|**EventSubClass**|**nvarchar**|Tipo di sottoclasse di evento. Fornisce ulteriori informazioni su ogni classe di evento.|21|Sì|  
|**GUID**|**uniqueidentifier**|ID della conversazione della finestra. Questo identificatore viene trasmesso come parte del messaggio e viene condiviso da entrambi i lati della conversazione.|54|No|  
|**HostName**|**nvarchar**|Nome del computer in cui è in esecuzione il client. Questa colonna di dati viene popolata se il nome host viene fornito dal client. Per determinare il nome host, usare la funzione **HOST_NAME** .|8|Sì|  
|**IsSystem**|**int**|Indica se l'evento è stato generato per un processo di sistema o un processo utente.<br /><br /> 0 = utente<br /><br /> 1 = sistema|60|No|  
|**LoginSid**|**image**|ID di sicurezza (SID) dell'utente connesso. Il SID è univoco per ogni account di accesso nel server.|41|Sì|  
|**MethodName**|**nvarchar**|Gruppo di conversazioni al quale la conversazione appartiene.|47|No|  
|**NTDomainName**|**nvarchar**|Dominio di Windows a cui appartiene l'utente.|7|Sì|  
|**NTUserName**|**nvarchar**|Nome dell'utente proprietario della connessione che ha generato questo evento.|6|Sì|  
|**ObjectName**|**nvarchar**|Handle di conversazione del dialogo.|34|No|  
|**Priorità**|**int**|Livello di priorità della conversazione|5|Sì|  
|**RoleName**|**nvarchar**|Ruolo dell'handle di conversazione. I valori possibili sono **initiator** o **target**.|38|No|  
|**ServerName**|**nvarchar**|Nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracciata.|26|No|  
|**Gravità**|**int**|Gravità dell'errore di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , se l'evento indica un errore.|29|No|  
|**SPID**|**int**|ID del processo server assegnato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al processo associato al client.|12|Sì|  
|**StartTime**|**datetime**|Ora di inizio dell'evento, se disponibile.|14|Sì|  
|**TextData**|**ntext**|Stato corrente della conversazione. I valori consentiti sono i seguenti:|1|Sì|  
|||**SO**. Avviata in uscita (Started outbound). [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha elaborato un'istruzione BEGIN CONVERSATION per la conversazione, ma non sono stati inviati messaggi.|||  
|||**SI**. Avviata in ingresso (Started inbound). Un'altra istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] ha avviato una nuova conversazione con l'istanza corrente, che tuttavia non ha completato la ricezione del primo messaggio. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] potrebbe creare una conversazione in questo stato se il primo messaggio fosse frammentato oppure se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ricevesse messaggi non ordinati. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] potrebbe tuttavia creare la conversazione nello stato CO se la prima trasmissione ricevuta per la conversazione contenesse il primo messaggio per intero.|||  
|||**CO**. In corso (Conversing). La conversazione è stata stabilita ed entrambi i lati della conversazione possono inviare messaggi. La maggior parte delle comunicazioni di un servizio tipico ha luogo quando la conversazione è in questo stato.|||  
|||**DI**. Disconnessa in ingresso (Disconnected inbound). Il lato remoto della conversazione ha eseguito un'istruzione END CONVERSATION. La conversazione rimane in questo stato finché il lato locale della conversazione non esegue un'istruzione END CONVERSATION. Un'applicazione può ancora ricevere messaggi per la conversazione. Poiché sul lato remoto la conversazione è terminata, non può invece inviare messaggi nella conversazione. Quando un'applicazione esegue un'istruzione END CONVERSATION, la conversazione passa allo stato CLOSED (CD).|||  
|||**DO**. Disconnessa in uscita (Disconnected outbound). Il lato locale della conversazione ha eseguito un'istruzione END CONVERSATION. La conversazione rimane in questo stato finché il lato remoto della conversazione invia un acknowledgement dell'istruzione END CONVERSATION. Un'applicazione non può inviare o ricevere messaggi per la conversazione. Quando il lato remoto della conversazione invia un acknowledgement per END CONVERSATION, la conversazione passa allo stato CLOSED (CD).|||  
|||**ER**. Errore. In questo endpoint si è verificato un errore. Le colonne Error, Severity e State contengono informazioni sull'errore specifico verificatosi.|||  
|||**CD**. Chiusa. L'endpoint di conversazione non è più in uso.|||  
|**TransactionID**|**bigint**|ID della transazione assegnato dal sistema.|4|No|  
  
 Nella tabella seguente sono elencati i valori delle sottoclassi di questa classe di evento.  
  
|ID|Sottoclasse|Descrizione|  
|--------|--------------|-----------------|  
|1|SEND Message|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un evento **SEND Message** quando il [!INCLUDE[ssDE](../../includes/ssde-md.md)] esegue un'istruzione SEND.|  
|2|END CONVERSATION|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un evento **END CONVERSATION** quando il [!INCLUDE[ssDE](../../includes/ssde-md.md)] esegue un'istruzione END CONVERSATION che non include la clausola WITH ERROR.|  
|3|END CONVERSATION WITH ERROR|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un evento **END CONVERSATION WITH ERROR** quando il [!INCLUDE[ssDE](../../includes/ssde-md.md)] esegue un'istruzione END CONVERSATION che include la clausola WITH ERROR.|  
|4|Broker Initiated Error|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un evento **Broker Initiated Error** ogni volta che [!INCLUDE[ssSB](../../includes/sssb-md.md)] crea un messaggio di errore. Ad esempio, quando [!INCLUDE[ssSB](../../includes/sssb-md.md)] non può eseguire correttamente il routing di un messaggio per un dialogo, il broker crea un messaggio di errore per il dialogo e genera questo evento. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non genera l'evento quando un'applicazione termina una conversazione con un errore.|  
|5|Terminate Dialog|[!INCLUDE[ssSB](../../includes/sssb-md.md)] ha terminato il dialogo. [!INCLUDE[ssSB](../../includes/sssb-md.md)] termina i dialoghi quando si verificano condizioni che ne impediscono la continuazione diverse da errori o dalla normale fine di una conversazione. Ad esempio, l'eliminazione di un servizio causa la terminazione di tutti i dialoghi di tale servizio da parte di [!INCLUDE[ssSB](../../includes/sssb-md.md)] .|  
|6|Received Sequenced Message|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera una classe di evento **Received Sequenced Message** quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] riceve un messaggio contenente un numero di sequenza messaggio. Tutti i tipi di messaggio definiti dall'utente sono messaggi in sequenza. [!INCLUDE[ssSB](../../includes/sssb-md.md)] genera un messaggio non in sequenza in due casi:<br /><br /> I messaggi di errore generati da [!INCLUDE[ssSB](../../includes/sssb-md.md)] non sono in sequenza.<br /><br /> È possibile che gli acknowledgement dei messaggi siano non in sequenza. Per motivi di efficienza, [!INCLUDE[ssSB](../../includes/sssb-md.md)] include qualsiasi acknowledgement disponibile come parte di un messaggio in sequenza. Se un'applicazione non invia un messaggio in sequenza all'endpoint remoto entro un determinato periodo di tempo, tuttavia, [!INCLUDE[ssSB](../../includes/sssb-md.md)] crea un messaggio non in sequenza per l'acknowledgement del messaggio.|  
|7|Received END CONVERSATION|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un evento Received END CONVERSATION quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] riceve un messaggio di fine dialogo dall'altro lato della conversazione.|  
|8|Received END CONVERSATION WITH ERROR|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un evento **Received END CONVERSATION WITH ERROR** quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] riceve un errore definito dall'utente dall'altro lato della conversazione. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non genera questo evento quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] riceve un errore definito dal broker.|  
|9|Received Broker Error Message|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un evento **Received Broker Error Message** quando [!INCLUDE[ssSB](../../includes/sssb-md.md)] riceve un messaggio di errore definito dal broker dall'altro lato della conversazione. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non genera questo evento quando [!INCLUDE[ssSB](../../includes/sssb-md.md)] riceve un messaggio di errore generato da un'applicazione.<br /><br /> Se il database corrente contiene una route predefinita per un database di inoltro, ad esempio, [!INCLUDE[ssSB](../../includes/sssb-md.md)] esegue il routing di un messaggio con un nome di servizio sconosciuto al database di inoltro. Se il database di inoltro non è in grado di eseguire il routing del messaggio, l'istanza di Service Broker di quel database crea un messaggio di errore e lo restituisce al database corrente. Quando il database corrente riceve il messaggio generato dal broker dal database di inoltro, genera un evento **Received Broker Error Message** .|  
|10|Received END CONVERSATION Ack|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera una classe di evento **Received END CONVERSATION Ack** quando l'altro lato della conversazione invia un acknowledgement per un messaggio di fine dialogo o di errore inviato dal lato locale della conversazione.|  
|11|BEGIN DIALOG|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un evento **BEGIN DIALOG** quando il motore di database esegue un'istruzione BEGIN DIALOG.|  
|12|Dialog Created|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un evento **Dialog Created** quando [!INCLUDE[ssSB](../../includes/sssb-md.md)] crea un endpoint per un dialogo. [!INCLUDE[ssSB](../../includes/sssb-md.md)] crea un endpoint ogni volta che viene stabilito un nuovo dialogo, indipendentemente dal fatto che il database corrente sia l'iniziatore o la destinazione del dialogo.|  
|13|END CONVERSATION WITH CLEANUP|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un evento END CONVERSATION WITH CLEANUP quando il [!INCLUDE[ssDE](../../includes/ssde-md.md)] esegue un'istruzione END CONVERSATION che include la clausola WITH CLEANUP.|  
  
## <a name="see-also"></a>Vedere anche  
 [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)  
  
  
