---
description: classe di evento PreConnect:Completed
title: Classe di evento PreConnect:Completed | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- PreConnect:Completed Event Class
ms.assetid: 7ed2f620-6511-4985-9961-d2927c2b1759
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b432f36c72b2118c6fd6a04e7f56a9a1d40ff24b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180840"
---
# <a name="preconnectcompleted-event-class"></a>classe di evento PreConnect:Completed
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La classe di evento PreConnect:Completed indica la fine dell'esecuzione di un trigger LOGON o di una funzione di classificazione di Resource Governor.  
  
## <a name="preconnectcompleted-event-class-data-columns"></a>Colonne di dati della classe di evento PreConnect:Completed  
  
|Nome colonna di dati|Tipo di dati|Descrizione|ID colonna|Filtrabile|  
|----------------------|---------------|-----------------|---------------|----------------|  
|EventClass|**int**|216|27|No|  
|SPID|**int**|ID del processo del server che genera l'evento.|12|Sì|  
|EventSubClass|**int**|1 per la funzione di classificazione definita dall'utente.|21|Sì|  
|StartTime|**datetime**|Ora di avvio della funzione di classificazione definita dall'utente.|14|Sì|  
|EndTime|**datetime**|Ora di avvio della funzione di classificazione definita dall'utente.|15|Sì|  
|Duration|**bigint**|Durata della funzione di classificazione in microsecondi.|13|Sì|  
|ObjectID|**int**|ID dell'oggetto di classificazione definito dall'utente.|22|Sì|  
|CPU|**int**|Utilizzo della CPU in millisecondi.|18|Sì|  
|Letture|**int**|Numero di letture logiche.|16|Sì|  
|Scritture|**int**|Numero di scritture logiche.|17|Sì|  
|GroupID|**int**|ID del gruppo del carico di lavoro classificato.|66|Sì|  
|Errore|**int**|Ultimo numero di errore se non è possibile eseguire la funzione di classificazione definita dall'utente.|31|Sì|  
|State|**int**|Stato dell'ultimo errore.|30|Sì|  
|TargetUserName|**sysname**|Valore restituito (nome del gruppo del carico di lavoro) per la funzione di classificazione definita dall'utente se non è possibile trovare un gruppo attivo corrispondente. Negli altri casi la colonna è impostata su NULL.|39|Sì|  
|ObjectName|**nvarchar(256)**|Nome in due parti della funzione di classificazione definita dall'utente, ad esempio dbo.classifier.|34|Sì|  
  
## <a name="see-also"></a>Vedere anche  
 [Eventi estesi](../../relational-databases/extended-events/extended-events.md)   
 [Classe di evento PreConnect:Starting](../../relational-databases/event-classes/preconnect-starting-event-class.md)   
 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)  
  
  
