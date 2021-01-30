---
description: Classe di evento FT:Crawl Aborted
title: Classe di evento FT:Crawl Aborted | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Crawl Aborted event class
ms.assetid: eead8ea6-5051-4689-ab30-4dfbfda01fb9
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3f1eeab215405e25f1b9d31f846cfe7d20ffbdcb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160735"
---
# <a name="ftcrawl-aborted-event-class"></a>Classe di evento FT:Crawl Aborted
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
   La classe di evento **FT:Crawl Aborted** indica che si è verificata un'eccezione durante una ricerca per indicizzazione full-text. In genere, l'errore causa l'arresto della ricerca. Per ulteriori informazioni sull'errore, controllare il registro eventi di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows o il log di tipo ricerca per indicizzazione.  
  
## <a name="ftcrawl-aborted-event-class-data-columns"></a>Colonne di dati della classe di evento FT:Crawl Aborted  
  
|Nome colonna di dati|Tipo di dati|Descrizione|ID colonna|Filtrabile|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**DatabaseID**|**int**|ID del database nel quale viene eseguita la ricerca per indicizzazione full-text. Determinare il valore per un database utilizzando la funzione DB_ID.|3|Sì|  
|**Error (Errore) (Error (Errore)e)**|**int**|Numero di errore di un evento specifico. Corrisponde spesso al numero di errore archiviato nella tabella **sysmessages** .|31|Sì|  
|**EventClass**|**int**|Tipo di evento = 157.|27|No|  
|**EventSequence**|**int**|Sequenza di un determinato evento all'interno della richiesta.|51|No|  
|**IsSystem**|**int**|Indica se l'evento è stato generato per un processo di sistema o un processo utente. 1 = sistema, 0 = utente.|60|Sì|  
|**ObjectID**|**int**|ID assegnato dal sistema all'oggetto sul quale viene eseguita la ricerca per indicizzazione full-text quando si verifica l'errore.|22|Sì|  
|**SessionLoginName**|**nvarchar**|Nome dell'account di accesso dell'utente che ha avviato la sessione. Se ad esempio si stabilisce la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con l'account di accesso Login1 e si esegue un'istruzione con l'account di accesso Login2, **SessionLoginName** indica Login1 e **LoginName** indica Login2. In questa colonna sono visualizzati sia gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che quelli di Windows.|64|Sì|  
|**SPID**|**int**|ID della sessione in cui si è verificato l'evento.|12|Sì|  
|**StartTime**|**datetime**|Ora di inizio dell'evento, se disponibile.|14|Sì|  
|**State**|**int**|Equivalente a un codice di stato dell'errore.|30|Sì|  
|**TransactionID**|**bigint**|ID della transazione assegnato dal sistema.|4|Sì|  
  
## <a name="see-also"></a>Vedere anche  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
