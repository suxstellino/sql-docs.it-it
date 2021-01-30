---
description: sp_update_jobschedule (Transact-SQL)
title: sp_update_jobschedule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_update_jobschedule_TSQL
- sp_update_jobschedule
dev_langs:
- TSQL
helpviewer_keywords:
- sp_update_jobschedule
ms.assetid: 4df02594-4cd1-49a9-8d97-37c44e4d5423
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d75398c9caa77430947248472f0a6c625def901f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207966"
---
# <a name="sp_update_jobschedule-transact-sql"></a>sp_update_jobschedule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifica le impostazioni di pianificazione del processo specificato.  
  
 **sp_update_jobschedule** è disponibile solo per compatibilità con le versioni precedenti.  
  
> [!IMPORTANT]
>  Per ulteriori informazioni sulla sintassi utilizzata nelle versioni precedenti di Microsoft SQL Server, vedere Transact-SQL Referencefor Microsoft SQL Server 2000 *.*  
  
## <a name="remarks"></a>Commenti  
 È possibile gestire le pianificazioni dei processi in modo indipendente dai processi. Per aggiornare una pianificazione, utilizzare **sp_update_schedule**.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per impostazione predefinita, questa stored procedure può essere eseguita dai membri del ruolo predefinito del server **sysadmin** . Gli altri utenti devono essere membri di uno dei ruoli predefiniti del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent seguenti nel database **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Per informazioni dettagliate sulle autorizzazioni di questi ruoli, vedere [Ruoli di database predefiniti di SQL Server Agent](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Solo i membri di **sysadmin** possono utilizzare questa stored procedure per aggiornare le pianificazioni dei processi di proprietà di altri utenti.  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di SQL Server Agent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [sp_update_schedule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-schedule-transact-sql.md)  
  
  
