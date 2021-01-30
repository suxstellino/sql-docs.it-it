---
description: sp_delete_jobschedule (Transact-SQL)
title: sp_delete_jobschedule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_jobschedule
- sp_delete_jobschedule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_jobschedule
ms.assetid: 82fbb48b-603a-4016-a7fb-1ce17fb76919
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c06030d2cc646dd269a0b3cb6bf564ce9248c9de
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195516"
---
# <a name="sp_delete_jobschedule-transact-sql"></a>sp_delete_jobschedule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Elimina una pianificazione per un processo.  
  
 **sp_delete_jobschedule** è disponibile solo per compatibilità con le versioni precedenti.  
  
  
## <a name="remarks"></a>Commenti  
 È possibile gestire le pianificazioni dei processi in modo indipendente dai processi. Per rimuovere una pianificazione da un processo, utilizzare **sp_detach_schedule**. Per eliminare una pianificazione, utilizzare **sp_delete_schedule**.  
  
> **Nota: sp_delete_jobschedule** non supporta le pianificazioni associate a più processi. Se uno script esistente chiama **sp_delete_jobschedule** per rimuovere una pianificazione collegata a più di un processo, la procedura restituisce un errore.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per impostazione predefinita, questa stored procedure può essere eseguita dai membri del ruolo predefinito del server **sysadmin** . Gli altri utenti devono essere membri di uno dei ruoli predefiniti del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent seguenti nel database **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Per informazioni dettagliate sulle autorizzazioni di questi ruoli, vedere [Ruoli di database predefiniti di SQL Server Agent](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 I membri del ruolo **sysadmin** possono eliminare qualsiasi pianificazione del processo. Gli utenti che non sono membri del ruolo **sysadmin** possono eliminare solo le pianificazioni dei processi di cui sono proprietari.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_delete_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-schedule-transact-sql.md)   
 [sp_detach_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-detach-schedule-transact-sql.md)   
 [Visualizzare o modificare i processi](../../ssms/agent/view-or-modify-jobs.md)   
 [sp_add_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_help_jobschedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-jobschedule-transact-sql.md)   
 [sp_update_jobschedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-jobschedule-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
