---
description: log_shipping_primary_secondaries (Transact-SQL)
title: log_shipping_primary_secondaries (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- log_shipping_primary_secondaries_TSQL
- log_shipping_primary_secondaries
dev_langs:
- TSQL
helpviewer_keywords:
- log_shipping_primary_secondaries system table
ms.assetid: 4b315c70-7265-4acd-b35b-a4dbb7881d98
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7394f6fc33db6e439bcbc5d34b0f53c25b9bf147
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102109"
---
# <a name="log_shipping_primary_secondaries-transact-sql"></a>log_shipping_primary_secondaries (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Esegue il mapping tra il database primario e i corrispondenti database secondari. Questa tabella è archiviata nel database **msdb** .  

  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**primary_id**|**uniqueidentifier**|ID del database primario nella configurazione per il log shipping.|  
|**secondary_server**|**sysname**|Nome dell'istanza secondaria di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] nella configurazione log shipping.|  
|**secondary_database**|**sysname**|Nome del database secondario nella configurazione di log shipping.|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni sul log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [sp_add_log_shipping_primary_secondary &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-secondary-transact-sql.md)   
 [sp_delete_log_shipping_primary_secondary &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-primary-secondary-transact-sql.md)   
 [sp_help_log_shipping_primary_secondary &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-secondary-transact-sql.md)   
 [Tabelle di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
