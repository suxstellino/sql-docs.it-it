---
description: dbo.sysjobschedules (Transact-SQL)
title: dbo.sysJobSchedules (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysjobschedules
- dbo.sysjobschedules
- dbo.sysjobschedules_TSQL
- sysjobschedules_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysjobschedules system table
ms.assetid: ccdafec7-2a9b-4356-bffb-1caa3a12db59
author: cawrites
ms.author: chadam
ms.openlocfilehash: f82fae854a00e73ec7d8d45304ee486e0df33373
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159999"
---
# <a name="dbosysjobschedules-transact-sql"></a>dbo.sysjobschedules (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Include informazioni di pianificazione per i processi da eseguire tramite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questa tabella è archiviata nel database **msdb** .  
  
> **Nota:** La tabella **sysjobschedules** viene aggiornata ogni 20 minuti, che può influire sui valori restituiti dal **sp_help_jobschedule** stored procedure.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**schedule_id**|**int**|ID della pianificazione.|  
|**job_id**|**uniqueidentifier**|ID del processo.|  
|**next_run_date**|**int**|Data pianificata per la successiva esecuzione del processo. La data è nel formato AAAAMMGG.|  
|**next_run_time**|**int**|Ora pianificata per l'esecuzione del processo. L'ora è nel formato HHMMSS, a 24 ore.|  
  
## <a name="see-also"></a>Vedere anche  
 [ Pianificazioni didbo.sys&#40;&#41;Transact-SQL ](../../relational-databases/system-tables/dbo-sysschedules-transact-sql.md)  
  
  
