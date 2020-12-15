---
description: Classe di evento CPU Threshold Exceeded
title: Classe di evento CPU Threshold Exceeded | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- CPU Threshold Exceeded Event Class
ms.assetid: eb106f7d-baa3-4a2b-96b2-f9fe0844057d
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e2051df175d07ea98dcccb4b3aa7b6f385fde4c0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97420542"
---
# <a name="cpu-threshold-exceeded-event-class"></a>Classe di evento CPU Threshold Exceeded
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  Questa classe di evento indica il rilevamento di una query che supera il valore soglia della CPU specificato per REQUEST_MAX_CPU_TIME_SEC.  
  
> [!NOTE]  
>  L'intervallo di rilevamento per questo evento è cinque secondi. È garantita la generazione di un evento se una query supera il limite specificato di almeno cinque secondi. Tuttavia, se una query supera la soglia specificata di meno di cinque secondi, il rilevamento potrebbe non riuscire a seconda del momento della query e dell'ora dell'ultima operazione di rilevamento.  
  
## <a name="cpu-threshold-exceeded-data-columns"></a>Colonne di dati di CPU Threshold Exceeded  
  
|Nome colonna di dati|Tipo di dati|Descrizione|ID colonna|Filtrabile|  
|----------------------|---------------|-----------------|---------------|----------------|  
|CPU|**int**|Utilizzo della CPU in millisecondi.|18|Sì|  
|EventClass|**int**|214|27|No|  
|EventSubClass|**int**|Violazione del limite della CPU.|21|Sì|  
|GroupID|**int**|ID del gruppo in cui si verifica la violazione.|66|Sì|  
|OwnerID|**int**|SPID del processo che provoca la violazione.|58|Sì|  
|SPID|**int**|ID del processo del server che genera l'evento.<br /><br /> Nota: può essere differente dallo SPID effettivo dell'utente se il thread di sistema convalida l'utilizzo della CPU come attività in background.|12|Sì|  
|StartTime|**datetime**|Ora di generazione dell'evento.|14|Sì|  
  
## <a name="see-also"></a>Vedere anche  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
