---
description: DROP EVENT NOTIFICATION (Transact-SQL)
title: DROP EVENT NOTIFICATION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP EVENT NOTIFICATION
- DROP_EVENT_NOTIFICATION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- event notifications [SQL Server], removing
- deleting event notifications
- dropping event notifications
- DROP EVENT NOTIFICATION statement
- removing event notifications
ms.assetid: 0ffd8f47-4ea3-4238-9e73-c318df710cf7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9fb977f1f6f9627b56707f037a4524e98a7e3c0a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095712"
---
# <a name="drop-event-notification-transact-sql"></a>DROP EVENT NOTIFICATION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Rimuove un trigger di notifica degli eventi dal database corrente.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
  
DROP EVENT NOTIFICATION notification_name [ ,...n ]  
ON { SERVER | DATABASE | QUEUE queue_name }  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *notification_name*  
 Nome della notifica degli eventi da rimuovere. È possibile specificare più notifiche. Per visualizzare un elenco delle notifiche degli eventi disponibili, usare [sys.event_notifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-event-notifications-transact-sql.md).  
  
 SERVER  
 Indica che l'ambito della notifica degli eventi corrisponde al server corrente. È necessario specificare SERVER se questo è l'ambito impostato al momento della creazione della notifica degli eventi.  
  
 DATABASE  
 Indica che l'ambito della notifica degli eventi corrisponde al database corrente. È necessario specificare DATABASE se questo è l'ambito impostato al momento della creazione della notifica degli eventi.  
  
 QUEUE *queue_name*  
 Indica che l'ambito della notifica degli eventi corrisponde alla coda specificata da *queue_name*. È necessario specificare QUEUE se questo è l'ambito impostato al momento della creazione della notifica degli eventi. L'argomento *queue_name* corrisponde al nome della coda ed è obbligatorio specificarlo.  
  
## <a name="remarks"></a>Commenti  
 Se viene attivata una notifica degli eventi all'interno di una transazione e tale notifica viene eliminata all'interno della stessa transazione, l'istanza della notifica degli eventi viene inviata e quindi la notifica degli eventi viene eliminata.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per eliminare una notifica degli eventi definita a livello di ambito del database, è necessario come minimo che l'utente sia il proprietario della notifica degli eventi o disponga dell'autorizzazione ALTER ANY DATABASE EVENT NOTIFICATION nel database corrente.  
  
 Per eliminare una notifica degli eventi definita a livello di ambito del server, è necessario come minimo che l'utente sia il proprietario della notifica degli eventi o disponga dell'autorizzazione ALTER ANY EVENT NOTIFICATION nel server.  
  
 Per eliminare una notifica degli eventi in una coda specifica, è necessario come minimo che l'utente sia il proprietario della notifica degli eventi o disponga dell'autorizzazione ALTER nella coda padre.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene creata una notifica degli eventi definita a livello di ambito di database, che viene quindi eliminata.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE EVENT NOTIFICATION NotifyALTER_T1  
ON DATABASE  
FOR ALTER_TABLE  
TO SERVICE 'NotifyService',  
    '8140a771-3c4b-4479-8ac0-81008ab17984';  
GO  
DROP EVENT NOTIFICATION NotifyALTER_T1  
ON DATABASE;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE EVENT NOTIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-event-notification-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.event_notifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-event-notifications-transact-sql.md)   
 [sys.events &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-events-transact-sql.md)  
  
  
