---
description: Categoria di eventi CLR
title: Categoria di eventi CLR | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- event classes [SQL Server], CLR event category
- SQL Server event classes, CLR event category
- CLR event category [SQL Server]
ms.assetid: a7c0cd60-3bec-42be-ad5e-473bd26a06d9
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9bedd7b9a5243aa391e8124716a879f3873cb8c0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205693"
---
# <a name="clr-event-category"></a>Categoria di eventi CLR
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
   La categoria di eventi **CLR** include le classi di eventi generate dall'esecuzione di oggetti [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] Common Language Runtime (CLR) in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
 
 ## <a name="assembly-load-event-class"></a>Classe di evento Assembly Load 
  La classe di evento **Assembly Load** viene generata quando viene eseguita la richiesta di caricamento di un assembly.  
  
 Includere la classe di evento **Assembly Load** nelle tracce quando si desidera monitorare i caricamenti di assembly. Può essere utile per la risoluzione dei problemi di una query che utilizza CLR (Common Language Runtime), per la risoluzione dei problemi di un server lento che esegue query CLR o per il monitoraggio di un server finalizzato alla raccolta di informazioni sugli utenti, i database, l'esito o altre informazioni relative ai caricamenti di assembly.  
  
## <a name="assembly-load-event-class-data-columns"></a>Colonne di dati della classe di evento Assembly Load  
  
|Nome colonna di dati|Tipo di dati|Descrizione|ID colonna|Filtrabile|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**ApplicationName**|**nvarchar**|Nome dell'applicazione che ha richiesto il caricamento.|10|Sì|  
|**ClientProcessID**|**int**|ID assegnato dal computer host al processo in cui è in esecuzione l'applicazione client. Questa colonna di dati viene popolata se tramite il client viene indicato l'ID del processo client.|9|Sì|  
|**DatabaseID**|**int**|ID del database specificato nell'istruzione USE database oppure ID del database predefinito, se per una determinata istanza non viene eseguita un'istruzione USE database. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] visualizza il nome del database se la colonna di dati **ServerName** è acquisita nella traccia e il server è disponibile. Determinare il valore per un database utilizzando la funzione DB_ID.|3|Sì|  
|**DatabaseName**|**nvarchar**|Nome del database nel quale viene eseguita l'istruzione dell'utente.|35|Sì|  
|**EventSequence**|**int**|Sequenza di un determinato evento all'interno della richiesta.|51|No|  
|**GroupID**|**int**|ID del gruppo del carico di lavoro in cui viene generato l'evento di Traccia SQL.|66|Sì|  
|**HostName**|**nvarchar**|Nome del computer in cui viene eseguito il client. Questa colonna di dati viene popolata se il client fornisce il nome host. Per determinare il nome host, usare la funzione HOST_NAME.|8|Sì|  
|**LoginName**|**nvarchar**|Nome dell'account di accesso dell'utente (account di accesso di sicurezza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o credenziali di accesso di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows nel formato DOMINIO\nomeutente).|11|Sì|  
|**LoginSID**|**image**|ID di sicurezza (SID) dell'utente connesso. Queste informazioni sono disponibili nella vista del catalogo **sys.server_principals** . Il SID è univoco per ogni account di accesso nel server.|41|Sì|  
|**NTDomainName**|**nvarchar**|Dominio Windows di appartenenza dell'utente.|7|Sì|  
|**NTUserName**|**nvarchar**|Nome utente di Windows.|6|Sì|  
|**ObjectID**|**int**|ID dell'assembly.|22|Sì|  
|**ObjectName**|**nvarchar**|Nome completo dell'assembly.|34|Sì|  
|**RequestID**|**int**|ID della richiesta contenente l'istruzione.|49|Sì|  
|**ServerName**|**nvarchar**|Nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracciata.|26|No|  
|**SessionLoginName**|**nvarchar**|Nome dell'account di accesso dell'utente che ha avviato la sessione. Se ad esempio si stabilisce la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con l'account di accesso Login1 e si esegue un'istruzione con l'account di accesso Login2, **SessionLoginName** indica Login1 e **LoginName** indica Login2. In questa colonna sono visualizzati sia gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che quelli di Windows.|64|Sì|  
|**SPID**|**int**|ID della sessione in cui si è verificato l'evento.|12|Sì|  
|**StartTime**|**datetime**|Ora di inizio dell'evento, se disponibile.|14|Sì|  
|**Success**|**int**|Indica se il caricamento dell'assembly ha avuto esito positivo (1) o negativo (0).|23|Sì|  
|**TextData**|**ntext**|"Assembly Load Succeeded" se il caricamento ha esito positivo, altrimenti "Assembly Load Failed".|1|Sì|  
  
## <a name="see-also"></a>Vedere anche  
 [Eventi estesi](../../relational-databases/extended-events/extended-events.md)   
 [Assembly &#40;Motore di database&#41;](../../relational-databases/clr-integration/assemblies-database-engine.md)  

