---
description: Audit Broker Conversation - classe di evento
title: Classe di evento Audit Broker Conversation | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Audit Broker Conversation event class
ms.assetid: d58e3577-e297-42e5-b8fe-206665a75d13
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 41813ebefcdf014045b343719249009a2908584f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179795"
---
# <a name="audit-broker-conversation-event-class"></a>Audit Broker Conversation - classe di evento
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un evento **Audit Broker Conversation** per visualizzare messaggi di controllo relativi alla sicurezza del dialogo di Service Broker.  
  
## <a name="audit-broker-conversation-event-class-data-columns"></a>Colonne di dati della classe di evento Audit Broker Conversation  
  
|Colonna di dati|Type|Descrizione|Numero di colonna|Filtrabile|  
|-----------------|----------|-----------------|-------------------|----------------|  
|**ApplicationName**|**nvarchar**|Nome dell'applicazione client in cui è stata creata la connessione a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questa colonna viene popolata con i valori passati dall'applicazione e non con il nome visualizzato del programma.|10|Sì|  
|**BigintData1**|**bigint**|Numero di sequenza del messaggio.|52|No|  
|**ClientProcessID**|**int**|ID assegnato dal computer host al processo in cui è in esecuzione l'applicazione client. Questa colonna di dati viene popolata se l'ID del processo client viene fornito dal client.|9|Sì|  
|**DatabaseID**|**int**|ID del database specificato nell'istruzione USE *database* oppure ID del database predefinito, se per una determinata istanza non viene eseguita un'istruzione USE *database* . [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] visualizza il nome del database se la colonna di dati **ServerName** è acquisita nella traccia e il server è disponibile. Determinare il valore per un database utilizzando la funzione DB_ID.|3|Sì|  
|**Error (Errore) (Error (Errore)e)**|**int**|Numero di errore di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , se l'evento indica un errore.|31|No|  
|**EventClass**|**int**|Tipo di classe di evento acquisita. Per **Audit Broker Conversation** , corrisponde sempre a **158**.|27|No|  
|**EventSubClass**|**int**|Tipo di sottoclasse di evento in cui sono disponibili informazioni aggiuntive su ogni classe di evento. Nella tabella seguente sono elencati i valori di sottoclasse per questo evento.|21|Sì|  
|**FileName**|**nvarchar**|Motivo dell'errore di accesso. Se l'accesso è riuscito, la colonna è vuota.|36|No|  
|**GUID**|**uniqueidentifier**|ID di conversazione del dialogo. Questo identificatore viene trasmesso come parte del messaggio e viene condiviso da entrambi i lati della conversazione.|54|No|  
|**HostName**|**nvarchar**|Nome del computer in cui è in esecuzione il client. Questa colonna di dati viene popolata se il nome host viene fornito dal client. Per determinare il nome host, usare la funzione **HOST_NAME** .|8|Sì|  
|**IntegerData**|**int**|Numero di frammento del messaggio.|25|No|  
|**NTDomainName**|**nvarchar**|Dominio di Windows a cui appartiene l'utente.|7|Sì|  
|**NTUserName**|**nvarchar**|Nome dell'utente proprietario della connessione che ha generato questo evento.|6|Sì|  
|**ObjectId**|**int**|ID utente del servizio di destinazione.|22|No|  
|**RoleName**|**nvarchar**|Ruolo dell'handle di conversazione. I valori possibili sono **initiator** o **target**.|38|No|  
|**ServerName**|**nvarchar**|Nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracciata.|26|No|  
|**Gravità**|**int**|Gravità dell'errore di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , se l'evento indica un errore.|29|No|  
|**SPID**|**int**|ID del processo server assegnato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al processo associato al client.|12|Sì|  
|**StartTime**|**datetime**|Ora di inizio dell'evento, se disponibile.|14|Sì|  
|**State**|**int**|Indica la posizione che ha generato l'evento all'interno del codice sorgente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ogni punto che può generare questo evento è contraddistinto da un codice di stato diverso. Questo codice di stato consente al supporto tecnico Microsoft di individuare la posizione in cui è stato generato l'evento.|30|No|  
|**TextData**|**ntext**|Per gli errori, contiene un messaggio che descrive il motivo corrispondente. Uno dei valori seguenti:<br /><br /> <br /><br /> **Certificato non trovato**. L'utente specificato per la sicurezza del protocollo del dialogo non dispone di un certificato.<br /><br /> **Certificato scaduto**. L'utente specificato per la sicurezza del protocollo del dialogo dispone di un certificato scaduto.<br /><br /> **Le dimensioni del certificato sono maggiori delle dimensioni massime consentite**. L'utente specificato per la sicurezza del protocollo del dialogo dispone di un certificato con dimensioni eccessive. Le dimensioni massime del certificato supportate da Service Broker corrispondono a 32.768 byte.<br /><br /> **Impossibile trovare la chiave privata del certificato**. L'utente specificato per la sicurezza del protocollo del dialogo dispone di un certificato a cui non è associata alcuna chiave privata.<br /><br /> **Le dimensioni della chiave privata del certificato non sono compatibili con il provider della crittografia**. Non è possibile elaborare correttamente le dimensioni della chiave privata del certificato. Tali dimensioni devono corrispondere a un multiplo di 64 byte.<br /><br /> **Le dimensioni della chiave pubblica del certificato non sono compatibili con il provider della crittografia**. Non è possibile elaborare correttamente le dimensioni della chiave pubblica del certificato. Tali dimensioni devono corrispondere a un multiplo di 64 byte.<br /><br /> **Le dimensioni della chiave privata del certificato non sono compatibili con la chiave per lo scambio delle chiavi crittografata**. Le dimensioni specificate nella chiave per lo scambio delle chiavi non corrispondono a quelle della chiave privata del certificato. In genere, questo indica che il certificato del computer remoto non corrisponde al certificato del database.<br /><br /> **Le dimensioni della chiave pubblica del certificato non sono compatibili con la firma dell'intestazione di sicurezza**. L'intestazione di sicurezza contiene una firma che non è possibile convalidare con la chiave pubblica del certificato. In genere, questo indica che il certificato del computer remoto non corrisponde al certificato del database.|1|Sì|  
  
 Nella tabella seguente sono elencati i valori di sottoclasse per questa classe di evento.  
  
|ID|Sottoclasse|Descrizione|  
|--------|--------------|-----------------|  
|1|Intestazione di sicurezza non disponibile|Durante una conversazione sicura, Service Broker ha ricevuto un messaggio che non contiene una chiave della sessione. Dopo che è stata stabilita una conversazione sicura, il protocollo del dialogo richiede che tutti i messaggi della conversazione contengano una chiave della sessione.|  
|2|Certificato non disponibile|Service Broker non è riuscito a individuare un certificato utilizzabile per uno dei partecipanti alla conversazione. Per proteggere una conversazione, è necessario che il database contenga un certificato sia per il mittente che per il destinatario della conversazione.|  
|3|Firma non valida|Service Broker non è riuscito a verificare la firma del messaggio fornita dal mittente utilizzando la chiave pubblica inclusa nel certificato del mittente. Ciò potrebbe indicare che il messaggio è corrotto, che il messaggio è stato alterato, che il servizio remoto e il servizio locale non sono configurati con lo stesso certificato utente o che il certificato non è aggiornato.|  
|4|Autorizzazioni non disponibili per l'utente di destinazione|L'utente di destinazione non dispone delle autorizzazioni RECEIVE. Per impedire la ricezione di messaggi da parte di utenti non autorizzati, Service Broker non accoda i messaggi diretti a un utente di destinazione che non può ricevere dati dalla coda, indipendentemente dal fatto che l'utente di origine disponga o meno dell'autorizzazione per accodare i messaggi.|  
  
## <a name="see-also"></a>Vedere anche  
 [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)  
  
  
