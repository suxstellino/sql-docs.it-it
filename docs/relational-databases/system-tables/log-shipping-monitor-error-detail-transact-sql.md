---
description: log_shipping_monitor_error_detail (Transact-SQL)
title: log_shipping_monitor_error_detail (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- log_shipping_monitor_error_detail_TSQL
- log_shipping_monitor_error_detail
dev_langs:
- TSQL
helpviewer_keywords:
- log_shipping_monitor_error_detail system table
ms.assetid: 0c38a625-60d2-4ee2-bcf3-2ba367914220
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8de8ba7737ce51d1ff6a47996250efb7c6862ca0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186942"
---
# <a name="log_shipping_monitor_error_detail-transact-sql"></a>log_shipping_monitor_error_detail (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Archivia i dettagli relativi agli errori per processi di log shipping. Questa tabella è archiviata nel database **msdb** .  
  
 Le tabelle correlate alla cronologia e al monitoraggio vengono utilizzate anche nel server primario e nei server secondari.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**agent_id**|**uniqueidentifier**|ID primario per il backup o ID secondario per la copia o il ripristino.|  
|**agent_type**|**tinyint**|Tipo di processo di log shipping.<br /><br /> 0 = backup.<br /><br /> 1 = copia.<br /><br /> 2 = ripristino.|  
|**session_id**|**int**|ID della sessione per il processo di backup/copia/ripristino.|  
|**database_name**|**sysname**|Nome del database associato a questo record di errore. Database primario per il backup, database secondario per il ripristino, o vuoto per la copia.|  
|**sequence_number**|**int**|Numero incrementale che indica l'ordine corretto delle informazioni relative agli errori che interessano più record.|  
|**log_time**|**datetime**|Data e ora di creazione del record.|  
|**log_time_utc**|**datetime**|Data e ora UTC (Coordinated Universal Time o ora di Greenwich) di creazione del record.|  
|**message**|**nvarchar**|Testo del messaggio.|  
|**source**|**nvarchar**|Origine del messaggio di errore o dell'evento.|  
|**help_url**|**nvarchar**|URL, se disponibile, in cui sono disponibili informazioni aggiuntive sull'errore.|  
  
## <a name="remarks"></a>Commenti  
 Questa tabella include i dettagli degli errori relativi ai processi di log shipping. Ogni errore viene registrato come una sequenza di eccezioni. Per ogni sessione dell'agente possono essere disponibili più errori (sequenze).  
  
 Oltre a essere archiviate sul server di monitoraggio remoto, le informazioni relative al server primario vengono archiviate nel server primario nella tabella **log_shipping_monitor_error_detail** e le informazioni relative a un server secondario vengono inoltre archiviate nel server secondario nella tabella **log_shipping_monitor_error_detail** .  
  
 Per identificare una sessione di Agent, utilizzare le colonne **agent_id**, **agent_type** e **session_id**. Ordina per **log_time** per visualizzare gli errori nell'ordine in cui sono stati registrati.  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni sul log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [log_shipping_monitor_history_detail &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/log-shipping-monitor-history-detail-transact-sql.md)   
 [sp_cleanup_log_shipping_history &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cleanup-log-shipping-history-transact-sql.md)   
 [sp_delete_log_shipping_primary_database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-primary-database-transact-sql.md)   
 [sp_delete_log_shipping_secondary_database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-secondary-database-transact-sql.md)   
 [sp_refresh_log_shipping_monitor &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-refresh-log-shipping-monitor-transact-sql.md)   
 [Tabelle di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
