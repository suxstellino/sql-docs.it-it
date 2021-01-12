---
description: dbo.sysoperators (Transact-SQL)
title: Operatori di dbo.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysoperators
- dbo.sysoperators
- dbo.sysoperators_TSQL
- sysoperators_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysoperators system table
ms.assetid: c2afa20c-b15f-46ca-ae74-2eb65909409e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 124ba525abf1bc9e96b065d9ef824ec35ef37b50
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096279"
---
# <a name="dbosysoperators-transact-sql"></a>dbo.sysoperators (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una riga per ogni operatore di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**id**|**int**|ID dell'operatore.|  
|**nome**|**sysname**|Nome dell'operatore.|  
|**abilitato**|**tinyint**|Stato delle notifiche di avvisi (booleano). Se è **1**, questo operatore può ricevere notifiche quando si verifica un avviso.|  
|**email_address**|**nvarchar (100)**|Indirizzo di posta elettronica dell'operatore.|  
|**last_email_date**|**int**|Data dell'ultima notifica di avviso tramite posta elettronica ricevuta dall'operatore.|  
|**last_email_time**|**int**|Ora dell'ultima notifica di avviso tramite posta elettronica ricevuta dall'operatore.|  
|**pager_address**|**nvarchar (100)**|Indirizzo cercapersone dell'operatore.|  
|**last_pager_date**|**int**|Data dell'ultima notifica di avviso tramite cercapersone ricevuta dall'operatore.|  
|**last_pager_time**|**int**|Ora dell'ultima notifica di avviso tramite cercapersone ricevuta dall'operatore|  
|**weekday_pager_start_time**|**int**|Ora dopo la quale l'operatore è disponibile per ricevere notifiche di avvisi tramite cercapersone nei giorni feriali (da lunedì a venerdì).|  
|**weekday_pager_end_time**|**int**|Ora dopo la quale l'operatore non è disponibile per ricevere notifiche di avvisi tramite cercapersone nei giorni feriali (da lunedì a venerdì).|  
|**saturday_pager_start_time**|**int**|Ora dopo la quale l'operatore è disponibile per ricevere notifiche di avvisi tramite cercapersone il sabato.|  
|**saturday_pager_end_time**|**int**|Ora dopo la quale l'operatore non è disponibile per ricevere notifiche di avvisi tramite cercapersone il sabato.|  
|**sunday_pager_start_time**|**int**|Ora dopo la quale l'operatore è disponibile per ricevere notifiche di avvisi tramite cercapersone la domenica.|  
|**sunday_pager_end_time**|**int**|Ora dopo la quale l'operatore non è disponibile per ricevere notifiche di avvisi tramite cercapersone la domenica.|  
|**pager_days**|**tinyint**|Maschera di bit che rappresenta i giorni della settimana durante i quali l'operatore è disponibile per ricevere una notifica di avviso tramite cercapersone.|  
|**netsend_address**|**nvarchar (100)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**last_netsend_date**|**int**|Data dell'ultimo messaggio di rete inviato all'ID dell'operatore specificato.|  
|**last_netsend_time**|**int**|Ora dell'ultimo messaggio di rete inviato all'ID dell'operatore specificato.|  
|**category_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
  
## <a name="see-also"></a>Vedere anche  
 [SQL Server Agent tabelle &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/sql-server-agent-tables-transact-sql.md)  
  
  
