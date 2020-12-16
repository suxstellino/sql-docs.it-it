---
description: Categoria di eventi Broker
title: Categoria di eventi Broker | Microsoft Docs
ms.custom: ''
ms.date: 05/24/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- SQL Server event classes, Broker event category
- Broker event category [SQL Server]
- event classes [SQL Server], Broker event category
ms.assetid: 470dc93c-0dda-4d89-829b-937738d59b31
author: stevestein
ms.author: sstein
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d2e5763593a3d041dd1021f48645c35eb867ed41
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97476242"
---
# <a name="broker-event-category"></a>Categoria di eventi Broker

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

 La categoria di eventi **Broker** include gli eventi generali di Service Broker.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
|Argomento|Descrizione|  
|-----------|-----------------|  
|[Classe di evento Broker:Activation](../../relational-databases/event-classes/broker-activation-event-class.md)|Evento generato quando un monitoraggio di coda avvia una stored procedure di attivazione.|  
|[Classe di evento Broker:Connection](../../relational-databases/event-classes/broker-connection-event-class.md)|Evento generato per indicare lo stato di una connessione di trasporto gestita da Service Broker.|  
|[Classe di evento Broker:Conversation](../../relational-databases/event-classes/broker-conversation-event-class.md)|Evento generato per indicare lo stato di una conversazione.|  
|[Classe di evento Broker:Conversation Group](../../relational-databases/event-classes/broker-conversation-group-event-class.md)|Evento generato quando il database crea o elimina un gruppo di conversazione.|  
|[Classe di evento Broker:Corrupted Message](../../relational-databases/event-classes/broker-corrupted-message-event-class.md)|Evento generato per indicare che il database ha ricevuto un messaggio danneggiato.|  
|[Classe di evento Broker:Forwarded Message Dropped](../../relational-databases/event-classes/broker-forwarded-message-dropped-event-class.md)|Evento generato quando SQL Server elimina un messaggio di Service Broker per cui era previsto l'inoltro.|  
|[Classe di evento Broker:Forwarded Message Sent](../../relational-databases/event-classes/broker-forwarded-message-sent-event-class.md)|Evento generato quando SQL Server inoltra un messaggio di Service Broker.|  
|[Classe di evento Broker:Message Classify](../../relational-databases/event-classes/broker-message-classify-event-class.md)|Evento generato quando Service Broker stabilisce il routing di un messaggio.|  
|[Classe di evento Broker:Message Drop](../../relational-databases/event-classes/broker-message-drop-event-class.md)|Evento generato quando Service Broker non è in grado di memorizzare un messaggio ricevuto che avrebbe dovuto essere recapitato a un servizio nell'istanza corrente.|  
|[Classe di evento Broker:Remote Message Ack](../../relational-databases/event-classes/broker-remote-message-ack-event-class.md)|Evento generato quando Service Broker invia o riceve l'acknowledgement di un messaggio.|  
  
 Vengono inoltre generati due eventi di controllo della sicurezza per Service Broker. Per altre informazioni sugli eventi, vedere [Classe di evento Audit Broker Login](../../relational-databases/event-classes/audit-broker-login-event-class.md) e [Classe di evento Audit Broker Conversation](../../relational-databases/event-classes/audit-broker-conversation-event-class.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Categoria di eventi Controllo di sicurezza](/analysis-services/trace-events/security-audit-event-category)  
  
