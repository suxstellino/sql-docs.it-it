---
description: log_shipping_monitor_alert (Transact-SQL)
title: log_shipping_monitor_alert (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- log_shipping_monitor_alert
- log_shipping_monitor_alert_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- log_shipping_monitor_alert system table
ms.assetid: 1c775e48-9898-4149-b9d1-04d465f23438
author: cawrites
ms.author: chadam
ms.openlocfilehash: ae126f82cce72ec9270fd2779bae986bb317da7f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201622"
---
# <a name="log_shipping_monitor_alert-transact-sql"></a>log_shipping_monitor_alert (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Archivia l'ID del processo di gestione degli avvisi per il log shipping. Questa tabella è archiviata nel database **msdb** .   
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**alert_job_id**|**uniqueidentifier**|ID del processo di gestione degli avvisi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent per il log shipping.|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni sul log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [sp_add_log_shipping_alert_job &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-log-shipping-alert-job-transact-sql.md)   
 [sp_delete_log_shipping_alert_job &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-alert-job-transact-sql.md)   
 [sp_help_log_shipping_alert_job &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-log-shipping-alert-job-transact-sql.md)   
 [Tabelle di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
