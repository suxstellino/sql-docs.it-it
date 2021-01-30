---
description: log_shipping_primary_databases (Transact-SQL)
title: log_shipping_primary_databases (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- log_shipping_primary_databases
- log_shipping_primary_databases_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- log_shipping_primary_databases system table
ms.assetid: 56888756-a798-42be-9b5e-0f9aa05a2cc6
author: cawrites
ms.author: chadam
ms.openlocfilehash: ba2f633ec533ea83a1dbbcb0bffb9136c776283b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186536"
---
# <a name="log_shipping_primary_databases-transact-sql"></a>log_shipping_primary_databases (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Archivia un record per il database primario in una configurazione per il log shipping. Questa tabella è archiviata nel database **msdb** .  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**primary_id**|**uniqueidentifier**|ID del database primario nella configurazione per il log shipping.|  
|**primary_database**|**sysname**|Nome del database primario nella configurazione di log shipping.|  
|**backup_directory**|**nvarchar (500)**|Directory in cui vengono archiviati i file di backup del log delle transazioni dal server primario.|  
|**backup_share**|**nvarchar (500)**|Percorso di rete o UNC della directory di backup.|  
|**backup_retention_period**|**int**|Intervallo di tempo, espresso in minuti, di conservazione del file di backup dei log nella directory di backup trascorso il quale il file viene eliminato.|  
|**backup_job_id**|**uniqueidentifier**|ID del processo di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent associato al processo di backup nel server primario.|  
|**monitor_server**|**sysname**|Nome dell'istanza di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] utilizzata come server di monitoraggio nella configurazione del log shipping.|  
|**monitor_server_security_mode**|**bit**|Modalità di sicurezza utilizzata per connettersi al server di monitoraggio.<br /><br /> 1 = Autenticazione di Windows.<br /><br /> 0 = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticazione di.|  
|**last_backup_file**|**nvarchar (500)**|Percorso assoluto del backup del log delle transazioni più recente.|  
|**last_backup_date**|**datetime**|Data e ora dell'ultima operazione di backup dei log.|  
|**user_specified_monitor**|**bit**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> **sp_help_log_shipping_primary_database** e **sp_help_log_shipping_secondary_primary** utilizzare questa colonna per controllare la visualizzazione delle impostazioni di monitoraggio in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .<br /><br /> 0 = quando si richiama una di queste due stored procedure, l'utente non ha specificato un valore esplicito per il parametro **\@ monitor_server** .<br /><br /> 1 = È stato specificato un valore esplicito.|  
|**backup_compression**|**tinyint**|Indica se la configurazione per il log shipping esegue l'override del comportamento della compressione dei backup a livello del server.<br /><br /> 0 = disabilitati. I backup del log non vengono mai compressi, indipendentemente dalle impostazioni di compressione dei backup configurate dal server.<br /><br /> 1 = abilitati. I backup del log vengono sempre compressi, indipendentemente dalle impostazioni di compressione dei backup configurate dal server.<br /><br /> 2 = usa la configurazione del server per l'opzione di configurazione server per la [visualizzazione o configurare l'opzione di configurazione del server backup compression default](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md) . Si tratta del valore predefinito.<br /><br /> La compressione dei backup è supportata solo in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise Edition.|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni sul log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [sp_add_log_shipping_primary_database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-database-transact-sql.md)   
 [sp_delete_log_shipping_primary_database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-primary-database-transact-sql.md)   
 [sp_help_log_shipping_primary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-database-transact-sql.md)   
 [Tabelle di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
