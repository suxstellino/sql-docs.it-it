---
description: Classe di evento QN:Subscription
title: Classe di evento QN:Subscription | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- event classes [SQL Server], QN:Subscription
ms.assetid: 4916167e-8541-43b4-900e-ec8e6adcbc34
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1efb259b1cd895c3c7b7978bde78121d0e042d54
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212144"
---
# <a name="qnsubscription-event-class"></a>Classe di evento QN:Subscription
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  L'evento QN:Subscription fornisce informazioni sulle sottoscrizioni di notifica.  
  
## <a name="qnsubscription-event-class-data-columns"></a>Colonne dati della classe di evento QN:Subscription  
  
|Colonna di dati|Type|Descrizione|Numero di colonna|Filtrabile|  
|-----------------|----------|-----------------|-------------------|----------------|  
|ApplicationName|**nvarchar**|Nome dell'applicazione client in cui è stata creata la connessione a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questa colonna viene popolata con i valori passati dall'applicazione e non con il nome visualizzato del programma.|10|Sì|  
|ClientProcessID|**int**|ID assegnato dal computer host al processo in cui è in esecuzione l'applicazione client. Questa colonna di dati viene popolata se l'ID del processo client viene fornito dal client.|9|Sì|  
|DatabaseID|**int**|ID del database specificato dall'istruzione USE *database* oppure ID del database predefinito, se per una determinata istanza non viene eseguita alcuna istruzione USE *database*. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] viene visualizzato il nome del database se la colonna di dati ServerName viene acquisita nella traccia e il server è disponibile. Determinare il valore per un database utilizzando la funzione DB_ID.|3|Sì|  
|DatabaseName|**nvarchar**|Nome del database in cui viene eseguita l'istruzione dell'utente.|35|Sì|  
|EventClass|**int**|Tipo di evento = 199.|27|No|  
|EventSequence|**int**|Numero di sequenza dell'evento.|51|No|  
|EventSubClass|**nvarchar**|Tipo di sottoclasse di evento in cui sono disponibili informazioni aggiuntive su ogni classe di evento. Questa colonna può contenere i valori seguenti:<br /><br /> **Subscription registered**: indica quando la sottoscrizione di notifica delle query viene registrata correttamente nel database.<br /><br /> **Subscription rewound**: indica quando [!INCLUDE[ssDE](../../includes/ssde-md.md)] riceve una richiesta di sottoscrizione che corrisponde esattamente a una sottoscrizione esistente. In tal caso, [!INCLUDE[ssDE](../../includes/ssde-md.md)] imposta il valore di timeout della sottoscrizione esistente sul timeout specificato nella nuova richiesta di sottoscrizione.<br /><br /> **Subscription fired**: indica quando una sottoscrizione di notifica genera un messaggio di notifica.<br /><br /> **Firing failed with broker error**: indica quando un messaggio di notifica ha esito negativo a causa di un errore di [!INCLUDE[ssSB](../../includes/sssb-md.md)] .<br /><br /> **Firing failed without broker error**: indica quando un messaggio di notifica ha esito negativo, ma non a causa di un errore di [!INCLUDE[ssSB](../../includes/sssb-md.md)] .<br /><br /> **Broker error intercepted**: indica che [!INCLUDE[ssSB](../../includes/sssb-md.md)] ha recapitato un errore nella conversazione usata dalla notifica delle query.<br /><br /> **Subscription deletion attempt**: indica che il [!INCLUDE[ssDE](../../includes/ssde-md.md)] ha provato a eliminare una sottoscrizione scaduta per liberare risorse.<br /><br /> **Subscription deletion failed**: indica che il tentativo di eliminazione di una sottoscrizione scaduta non è riuscito. [!INCLUDE[ssDE](../../includes/ssde-md.md)] ripianificherà automaticamente la sottoscrizione per l'eliminazione allo scopo di liberare risorse.<br /><br /> **Subscription destroyed**: indica che il [!INCLUDE[ssDE](../../includes/ssde-md.md)] ha eliminato correttamente una sottoscrizione scaduta.|21|Sì|  
|GroupID|**int**|ID del gruppo del carico di lavoro in cui viene generato l'evento di Traccia SQL.|66|Sì|  
|HostName|**nvarchar**|Nome del computer in cui è in esecuzione il client. Questa colonna di dati viene popolata se il nome host viene fornito dal client. Per determinare il nome host, usare la funzione HOST_NAME.|8|Sì|  
|IsSystem|**int**|Indica se l'evento è stato generato per un processo di sistema o un processo utente.<br /><br /> 0 = utente<br /><br /> 1 = sistema|60|No|  
|LoginName|**nvarchar**|Nome dell'account di accesso dell'utente (account di sicurezza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o credenziali di accesso di Windows nel formato DOMINIO\nomeutente).|11|No|  
|LoginSID|**image**|ID di sicurezza (SID) dell'utente connesso. Queste informazioni sono disponibili nella vista del catalogo sys.server_principals. Il SID è univoco per ogni account di accesso nel server.|41|Sì|  
|NTDomainName|**nvarchar**|Dominio di Windows a cui appartiene l'utente.|7|Sì|  
|NTUserName|**nvarchar**|Nome dell'utente proprietario della connessione che ha generato questo evento.|6|Sì|  
|RequestID|**int**|Identificatore della richiesta contenente l'istruzione.|49|Sì|  
|ServerName|**nvarchar**|Nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracciata.|26|No|  
|SessionLoginName|**nvarchar**|Nome dell'account di accesso dell'utente che ha avviato la sessione. Se ad esempio un'applicazione si connette a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con l'account di accesso Login1 ed esegue un'istruzione con l'account di accesso Login2, SessionLoginName indica "Login1" e LoginName indica "Login2". In questa colonna sono visualizzati sia gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che quelli di Windows.|64|Sì|  
|SPID|**int**|ID della sessione in cui si è verificato l'evento.|12|Sì|  
|StartTime|**datetime**|Ora di inizio dell'evento, se disponibile.|14|Sì|  
|TextData|**ntext**|Restituisce un documento XML contenente informazioni specifiche per questo evento. Questo documento è conforme XML Schema disponibile nella pagina [Schema di eventi di SQL Server Profiler correlati alle notifiche delle query](https://go.microsoft.com/fwlink/?LinkId=63331) .|1|Sì|  
  
  
