---
description: log_shipping_monitor_secondary (Transact-SQL)
title: log_shipping_monitor_secondary (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- log_shipping_monitor_secondary_TSQL
- log_shipping_monitor_secondary
dev_langs:
- TSQL
helpviewer_keywords:
- log_shipping_monitor_secondary system table
ms.assetid: afbe1bb7-89a7-4020-9408-0af64a043c2e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4c43277d1dd9203f4727a536250b7469ffb9fa55
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99122383"
---
# <a name="log_shipping_monitor_secondary-transact-sql"></a>log_shipping_monitor_secondary (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Archivia un record di monitoraggio per database secondario in una configurazione per il log shipping. Questa tabella è archiviata nel database **msdb** .  
  
 Le tabelle correlate alla cronologia e al monitoraggio vengono utilizzate anche nel server primario e nei server secondari.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**secondary_server**|**sysname**|Nome dell'istanza secondaria di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] nella configurazione log shipping.|  
|**secondary_database**|**sysname**|Nome del database secondario nella configurazione di log shipping.|  
|**secondary_id**|**uniqueidentifier**|ID del server secondario nella configurazione per il log shipping.|  
|**primary_server**|**sysname**|Nome dell'istanza primaria di [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] nella configurazione per il log shipping.|  
|**primary_database**|**sysname**|Nome del database primario nella configurazione di log shipping.|  
|**restore_threshold**|**int**|Numero di minuti che può trascorrere tra operazioni di ripristino prima che venga generato un avviso.|  
|**threshold_alert**|**int**|Avviso da generare quando viene superata la soglia di ripristino.|  
|**threshold_alert_enabled**|**bit**|Determina se sono abilitati gli avvisi di soglia di ripristino. 1 = abilitati.<br /><br /> 0 = disabilitati.|  
|**last_copied_file**|**nvarchar (500)**|Nome dell'ultimo file di backup copiato nel server secondario.|  
|**last_copied_date**|**datetime**|Data e ora dell'ultima operazione di copia nel server secondario.|  
|**last_copied_date_utc**|**datetime**|Data e ora UTC (Coordinated Universal Time o ora di Greenwich) dell'ultima operazione di copia nel server secondario.|  
|**last_restored_file**|**nvarchar (500)**|Nome dell'ultimo file di backup ripristinato nel database secondario.|  
|**last_restored_date**|**datetime**|Data e ora dell'ultima operazione di ripristino nel database secondario.|  
|**last_restored_date_utc**|**datetime**|Data e ora UTC (Coordinated Universal Time o ora di Greenwich) dell'ultima operazione di ripristino nel database secondario.|  
|**last_restored_latency**|**int**|Intervallo, in minuti, intercorso tra la creazione del backup del log nel server primario e il relativo ripristino nel server secondario.<br /><br /> Il valore iniziale è NULL.|  
|**history_retention_period**|**int**|Periodo di tempo, in minuti, durante il quale i record della cronologia di log shipping vengono mantenuti per un database secondario specificato prima di essere eliminati.|  
  
## <a name="remarks"></a>Commenti  
 Oltre a essere archiviate sul server di monitoraggio remoto, le informazioni correlate a un server secondario vengono inoltre archiviate nel server secondario nella relativa tabella **log_shipping_monitor_secondary** .  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni sul log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [sp_refresh_log_shipping_monitor &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-refresh-log-shipping-monitor-transact-sql.md)   
 [sp_add_log_shipping_secondary_database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-log-shipping-secondary-database-transact-sql.md)   
 [sp_change_log_shipping_secondary_database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-change-log-shipping-secondary-database-transact-sql.md)   
 [sp_delete_log_shipping_secondary_database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-secondary-database-transact-sql.md)   
 [sp_help_log_shipping_secondary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-database-transact-sql.md)   
 [sp_help_log_shipping_monitor_secondary &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-secondary-transact-sql.md)   
 [sp_refresh_log_shipping_monitor &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-refresh-log-shipping-monitor-transact-sql.md)   
 [Tabelle di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
