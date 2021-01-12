---
description: dbo.sysjobs (Transact-SQL)
title: Processi di dbo.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysjobs
- sysjobs_TSQL
- dbo.sysjobs
- dbo.sysjobs_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysjobs system table
ms.assetid: e244a6a5-54c2-47a6-8039-dd1852b0ae59
author: cawrites
ms.author: chadam
ms.openlocfilehash: e36afae06020e9e970359cb09bc79a9bdf346b03
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091504"
---
# <a name="dbosysjobs-transact-sql"></a>dbo.sysjobs (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Archivia le informazioni su tutti i processi pianificati da eseguire tramite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Questa tabella è archiviata nel database **msdb** .  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**job_id**|**uniqueidentifier**|ID univoco del processo.|  
|**originating_server_id**|**int**|ID del server di provenienza del processo.|  
|**nome**|**sysname**|Nome del processo.|  
|**abilitato**|**tinyint**|Indica se il processo è abilitato per l'esecuzione.|  
|**description**|**nvarchar(512)**|Descrizione del processo.|  
|**start_step_id**|**int**|ID del passaggio del processo da cui deve iniziare l'esecuzione.|  
|**category_id**|**int**|ID della categoria del processo.|  
|**owner_sid**|**varbinary(85)**|ID di sicurezza (SID) del proprietario del processo.|  
|**notify_level_eventlog**|**int**|**Maschera di maschera** che indica le condizioni per la registrazione di un evento di notifica nel registro applicazioni di Microsoft Windows:<br /><br /> **0** = mai<br /><br /> **1** = in caso di esito positivo processo<br /><br /> **2** = quando il processo ha esito negativo<br /><br /> **3** = ogni volta che il processo viene completato (indipendentemente dal risultato del processo)|  
|**notify_level_email**|**int**|Maschera di bit che indica le condizioni per l'invio di un messaggio di posta elettronica di notifica al termine di un processo:<br /><br /> **0** = mai<br /><br /> **1** = in caso di esito positivo processo<br /><br /> **2** = quando il processo ha esito negativo<br /><br /> **3** = ogni volta che il processo viene completato (indipendentemente dal risultato del processo)|  
|**notify_level_netsend**|**int**|Maschera di bit che indica le condizioni per l'invio di un messaggio in rete al termine di un processo:<br /><br /> **0** = mai<br /><br /> **1** = in caso di esito positivo processo<br /><br /> **2** = quando il processo ha esito negativo<br /><br /> **3** = ogni volta che il processo viene completato (indipendentemente dal risultato del processo)|  
|**notify_level_page**|**int**|Maschera di bit che indica le condizioni per l'invio di un messaggio su cercapersone al termine di un processo:<br /><br /> **0** = mai<br /><br /> **1** = in caso di esito positivo processo<br /><br /> **2** = quando il processo ha esito negativo<br /><br /> **3** = ogni volta che il processo viene completato (indipendentemente dal risultato del processo)|  
|**notify_email_operator_id**|**int**|Nome di posta elettronica dell'operatore a cui inviare la notifica.|  
|**notify_netsend_operator_id**|**int**|ID del computer o dell'utente utilizzato quando si invia un messaggio in rete.|  
|**notify_page_operator_id**|**int**|ID del computer o dell'utente utilizzato quando si invia un messaggio su cercapersone.|  
|**delete_level**|**int**|**Maschera di maschera** che indica le circostanze in cui il processo deve essere eliminato al completamento di un processo:<br /><br /> **0** = mai<br /><br /> **1** = in caso di esito positivo processo<br /><br /> **2** = quando il processo ha esito negativo<br /><br /> **3** = ogni volta che il processo viene completato (indipendentemente dal risultato del processo)|  
|**date_created**|**datetime**|Data di creazione del processo.|  
|**date_modified**|**datetime**|Data dell'ultima modifica del processo.|  
|**version_number**|**int**|Versione del processo.|  
  
  
