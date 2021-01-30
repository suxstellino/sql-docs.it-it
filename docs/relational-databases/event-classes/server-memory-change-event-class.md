---
description: Server Memory Change - classe di evento
title: Classe di evento Server Memory Change | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Server Memory Change event class
ms.assetid: c9836484-39c5-4a89-b080-3567783b6fff
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9ad51322afad05e7ff233c698d9d0ac8c073daa7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99151700"
---
# <a name="server-memory-change-event-class"></a>Server Memory Change - classe di evento
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La classe di evento **Server Memory Change** viene generata quando l'utilizzo di memoria di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è aumentato o diminuito di 1 MB o del 5% della quantità di memoria massima del server, a seconda del valore maggiore.  
  
## <a name="server-memory-change-event-class-data-columns"></a>Colonne di dati della classe di evento Server Memory Change  
  
|Nome colonna di dati|Tipo di dati|Descrizione|ID colonna|Sì|  
|----------------------|---------------|-----------------|---------------|---------|  
|**EventClass**|**int**|Tipo di evento = 81.|27|No|  
|**EventSequence**|**int**|Sequenza di un determinato evento all'interno della richiesta.|51|No|  
|**EventSubClass**|**int**|Tipo di sottoclasse di evento.<br /><br /> 1=Aumento della memoria<br /><br /> 2=Riduzione della memoria|21|Sì|  
|**IntegerData**|**int**|Nuova quantità di memoria, in megabyte (MB).|25|Sì|  
|**IsSystem**|**int**|Indica se l'evento è stato generato per un processo di sistema o un processo utente. 1 = sistema, 0 = utente.|60|Sì|  
|**RequestID**|**int**|ID della richiesta contenente l'istruzione.|49|Sì|  
|**ServerName**|**nvarchar**|Nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracciata.|26|No|  
|**SessionLoginName**|**nvarchar**|Nome dell'account di accesso dell'utente che ha avviato la sessione. Se ad esempio si stabilisce la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con l'account di accesso Login1 e si esegue un'istruzione con l'account di accesso Login2, **SessionLoginName** indica Login1 e **LoginName** indica Login2. In questa colonna sono visualizzati sia gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che quelli di Windows.|64|Sì|  
|**SPID**|**int**|ID della sessione in cui si è verificato l'evento.|12|Sì|  
|**StartTime**|**datetime**|Ora di inizio dell'evento, se disponibile.|14|Sì|  
|**TransactionID**|**bigint**|ID della transazione assegnato dal sistema.|4|Sì|  
|**XactSequence**|**bigint**|Token utilizzato per descrivere la transazione corrente.|50|Sì|  
  
## <a name="see-also"></a>Vedere anche  
 [Eventi estesi](../../relational-databases/extended-events/extended-events.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [Opzioni di configurazione del server Server Memory](../../database-engine/configure-windows/server-memory-server-configuration-options.md)  
  
  
