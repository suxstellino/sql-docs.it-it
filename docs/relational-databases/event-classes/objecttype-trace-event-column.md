---
title: Colonna ObjectType per gli eventi di traccia | Microsoft Docs
description: Fare riferimento ai valori possibili della colonna ObjectType per gli eventi di traccia, che viene usata in un'ampia gamma di eventi di traccia in SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- SQL Server event classes, Object Type column values
- events [SQL Server], Object Type column values
- event classes [SQL Server], Object Type column values
- Object Type column values [SQL Server]
ms.assetid: 42f85c50-34c9-49ca-955f-af9595e2707f
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 63b888ac84ff567be7b72eb09cd6271853ab69f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183242"
---
# <a name="objecttype-trace-event-column"></a>Colonna ObjectType per gli eventi di traccia
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La colonna ObjectType viene utilizzata in un'ampia gamma di eventi di traccia. In questo argomento vengono descritti i possibili valori di tale colonna e le definizioni associate.  
  
## <a name="object-type-column-values"></a>Valori della colonna ObjectType  
  
|valore|Definizione|  
|-----------|----------------|  
|8259|Vincolo CHECK|  
|8260|Predefinito (vincolo o autonomo)|  
|8262|Vincolo FOREIGN KEY|  
|8272|Stored Procedure|  
|8274|Regola|  
|8275|Tabella di sistema|  
|8276|Trigger nel server|  
|8277|Tabella (definita dall'utente)|  
|8278|Visualizzazione|  
|8280|Stored procedure estesa|  
|16724|Trigger CLR|  
|16964|Database|  
|16975|Oggetto|  
|17222|Catalogo full-text|  
|17232|Stored procedure CLR|  
|17235|SCHEMA|  
|17475|Credenziale|  
|17491|Evento DDL|  
|17741|Evento di gestione|  
|17747|Evento di sicurezza|  
|17749|Evento utente|  
|17985|Funzione di aggregazione CLR|  
|17993|Funzione SQL inline con valori di tabella|  
|18000|Funzione di partizione|  
|18002|Procedura di filtro della replica|  
|18004|Funzione SQL con valori di tabella|  
|18259|Ruolo del server|  
|18263|[!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows|  
|19265|Chiave asimmetrica|  
|19277|Chiave master|  
|19280|Chiave primaria|  
|19283|ObfusKey|  
|19521|Account di accesso con chiave asimmetrica|  
|19523|Account di accesso con certificato|  
|19538|Ruolo|  
|19539|Account di accesso SQL|  
|19543|Account di accesso Windows|  
|20034|Associazione al servizio remoto|  
|20036|Notifica dell'evento nel database|  
|20037|Notifica dell'evento|  
|20038|Funzione SQL scalare|  
|20047|Notifica dell'evento nell'oggetto|  
|20051|Sinonimo|  
|20307|Sequenza|  
|20549|Endpoint|  
|20801|Query ad hoc che potrebbero essere memorizzate nella cache|  
|20816|Query preparate che potrebbero essere memorizzate nella cache|  
|20819|Coda di servizio di Service Broker|  
|20821|Vincolo UNIQUE|  
|21057|Ruolo applicazione|  
|21059|Certificato|  
|21075|Server|  
|21076|Trigger Transact-SQL|  
|21313|Assembly|  
|21318|Funzione scalare CLR|  
|21321|Funzione SQL inline scalare|  
|21328|Schema partizione|  
|21333|Utente|  
|21571|Contratto di servizio di Service Broker|  
|21572|Trigger nel database|  
|21574|Funzione CLR con valori di tabella|  
|21577|Tabella interna (ad esempio tabella dei nodi XML o tabella della coda)|  
|21581|Tipo di messaggio di Service Broker|  
|21586|Route di Service Broker|  
|21587|Statistiche|  
|21825<br /><br /> 21827<br /><br /> 21831<br /><br /> 21843<br /><br /> 21847|Utente|  
|22099|Servizio di Service Broker|  
|22601|Indice|  
|22604|Account di accesso con certificato|  
|22611|XMLSchema|  
|22868|Type|  
  
## <a name="see-also"></a>Vedere anche  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
