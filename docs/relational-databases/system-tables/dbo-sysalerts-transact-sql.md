---
description: dbo.sysalerts (Transact-SQL)
title: Avvisi di dbo.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/24/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dbo.sysalerts
- sysalerts_TSQL
- dbo.sysalerts_TSQL
- sysalerts
dev_langs:
- TSQL
helpviewer_keywords:
- sysalerts system table
ms.assetid: a2c2f50d-61f3-4951-996a-add5ad092cc2
author: cawrites
ms.author: chadam
ms.openlocfilehash: 69250eb1576daa37aeb4b633f86ee7dc019255c5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195839"
---
# <a name="dbosysalerts-transact-sql"></a>dbo.sysalerts (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una riga per ogni avviso. Un avviso è un messaggio inviato in risposta a un evento con cui è possibile inoltrare messaggi all'esterno dell'ambiente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite posta elettronica o cercapersone. Un avviso può generare inoltre un'attività.  Questa tabella è archiviata nel database **msdb** .
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**id**|**int**|ID dell'avviso.|  
|**nome**|**sysname**|Nome dell'avviso.|  
|**event_source**|**nvarchar (100)**|Origine dell'evento: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**event_category_id**|**int**|Riservato per utilizzi futuri.|  
|**event_id**|**int**|Riservato per utilizzi futuri.|  
|**message_id**|**int**|ID messaggio definito dall'utente o riferimento al messaggio **sysmessages** che attiva questo avviso.|  
|**severity**|**int**|Livello di gravità che attiva l'avviso.|  
|**abilitato**|**tinyint**|Stato dell'avviso:<br /><br /> **0** = disabilitato.<br /><br /> **1** = abilitata.|  
|**delay_between_responses**|**int**|Intervallo espresso in secondi tra due notifiche dell'avviso.|  
|**last_occurrence_date**|**int**|Data dell'ultima occorrenza dell'avviso.|  
|**last_occurrence_time**|**int**|Ora dell'ultima occorrenza dell'avviso.|  
|**last_response_date**|**int**|Data dell'ultima notifica dell'avviso.|  
|**last_response_time**|**int**|Ora dell'ultima notifica dell'avviso.|  
|**notification_message**|**nvarchar(512)**|Informazioni aggiuntive inviate con l'avviso.|  
|**include_event_description**|**tinyint**|Maschera di maschera che indica se la descrizione dell'evento viene inviata tramite posta elettronica, cercapersone o net send. Vedere il grafico seguente per i valori.|  
|**database_name**|**nvarchar(512)**|Database in cui è necessario che si verifichi l'avviso affinché venga attivato.|  
|**event_description_keyword**|**nvarchar (100)**|Modello a cui deve corrispondere l'errore affinché venga attivato l'avviso.|  
|**occurrence_count**|**int**|Numero di occorrenze dell'avviso.|  
|**count_reset_date**|**int**|Il numero di giorni (date) verrà reimpostato su **0**.|  
|**count_reset_time**|**int**|Il conteggio dell'ora del giorno verrà reimpostato su **0**.|  
|**job_id**|**uniqueidentifier**|ID dell'attività eseguita quando si verifica l'avviso.|  
|**has_notification**|**int**|Numero di operatori che ricevono una notifica tramite posta elettronica quando si verifica un avviso.|  
|**flags**|**int**|Riservato.|  
|**performance_condition**|**nvarchar(512)**|Riservato.|  
|**category_id**|**int**|Riservato.|  
  
 ## <a name="remarks"></a>Commenti

Nella tabella seguente vengono illustrati i valori per la maschera di maschera include_event_description. Il valore decimale viene restituito da dbo.sysavvisi. 

|decimal | BINARY | significato |
|------|------|------|
|0 |0000 |Nessun messaggio |
|1 |0001 |email |
|2 |0010 |pager |
|3 |0011 |cercapersone e messaggi di posta elettronica |
|4 |0100 |Net send |
|5 |0101 |NET SEND e posta elettronica |
|6 |0110 |NET SEND e cercapersone |
|7 |0111 |NET SEND, cercapersone e messaggi di posta elettronica |
  
