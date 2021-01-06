---
title: Tabelle e stored procedure relative al log shipping | Microsoft Docs
description: Rivedere tutte le tabelle e le stored procedure associate a una configurazione per il log shipping. Ogni server archivia le tabelle di log shipping in msdb.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- secondary servers [SQL Server]
- monitor servers [SQL Server]
- log shipping [SQL Server], system tables
- log shipping [SQL Server], stored procedures
- primary servers [SQL Server]
ms.assetid: 03420810-4c38-4c0c-adf0-913eb044c50a
author: cawrites
ms.author: chadam
ms.openlocfilehash: 93990b05d791fab8edb1d7e194f5c70cb448db3e
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643878"
---
# <a name="log-shipping-tables-and-stored-procedures"></a>Log Shipping Tables and Stored Procedures
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento vengono descritte tutte le tabelle e le stored procedure associate a una configurazione di log shipping. Tutte le tabelle relative al log shipping sono archiviate in **msdb** in ogni server. Nelle tabelle seguenti viene illustrato su quali server sono utilizzate le diverse tabelle e le stored procedure in una configurazione di log shipping.  
  
## <a name="primary-server-tables"></a>Tabelle del server primario  
  
|Tabella|Descrizione|  
|-----------|-----------------|  
|[log_shipping_monitor_alert](../../relational-databases/system-tables/log-shipping-monitor-alert-transact-sql.md)|Memorizza l'ID del processo per la gestione degli avvisi. Questa tabella viene utilizzata nel server primario solo se non è stato configurato alcun server di monitoraggio remoto.|  
|[log_shipping_monitor_error_detail](../../relational-databases/system-tables/log-shipping-monitor-error-detail-transact-sql.md)|Archivia i dettagli relativi agli errori per processi di log shipping associati a questo server primario.|  
|[log_shipping_monitor_history_detail](../../relational-databases/system-tables/log-shipping-monitor-history-detail-transact-sql.md)|Archivia i dettagli relativi alla cronologia per processi di log shipping associati a questo server primario.|  
|[log_shipping_monitor_primary](../../relational-databases/system-tables/log-shipping-monitor-primary-transact-sql.md)|Archivia un record di monitoraggio per questo database primario.|  
|[log_shipping_primary_databases](../../relational-databases/system-tables/log-shipping-primary-databases-transact-sql.md)|Include informazioni di configurazione per database primari in un determinato server. Archivia una riga per ogni database primario.|  
|[log_shipping_primary_secondaries](../../relational-databases/system-tables/log-shipping-primary-secondaries-transact-sql.md)|Esegue il mapping dei database primari ai database secondari.|  
  
## <a name="primary-server-stored-procedures"></a>Stored procedure del server primario  
  
|Stored Procedure|Descrizione|  
|----------------------|-----------------|  
|[sp_add_log_shipping_primary_database](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-database-transact-sql.md)|Imposta il database primario per una configurazione di log shipping, inclusi il processo di backup, il record di monitoraggio locale e il record di monitoraggio remoto.|  
|[sp_add_log_shipping_primary_secondary](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-secondary-transact-sql.md)|Aggiunge un nome di database secondario a un database primario esistente.|  
|[sp_change_log_shipping_primary_database](../../relational-databases/system-stored-procedures/sp-change-log-shipping-primary-database-transact-sql.md)|Modifica le impostazioni del database primario, incluso il record di monitoraggio locale e remoto.|  
|[sp_cleanup_log_shipping_history](../../relational-databases/system-stored-procedures/sp-cleanup-log-shipping-history-transact-sql.md)|Elimina la cronologia a livello locale e sul monitor, in base al periodo di memorizzazione.|  
|[sp_delete_log_shipping_primary_database](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-primary-database-transact-sql.md)|Rimuove il log shipping del database primario, inclusi il processo di backup e la cronologia locale e remota.|  
|[sp_delete_log_shipping_primary_secondary](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-primary-secondary-transact-sql.md)|Rimuove un nome di database secondario da un database primario.|  
|[sp_help_log_shipping_primary_database](../../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-database-transact-sql.md)|Recupera le impostazioni del database primario e visualizza i valori dalle tabelle **log_shipping_primary_databases** e **log_shipping_monitor_primary** .|  
|[sp_help_log_shipping_primary_secondary](../../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-secondary-transact-sql.md)|Recupera i nomi dei database secondari per un database primario.|  
|[sp_refresh_log_shipping_monitor](../../relational-databases/system-stored-procedures/sp-refresh-log-shipping-monitor-transact-sql.md)|Aggiorna il monitor, visualizzando le informazioni più recenti per l'agente di log shipping specificato.|  
  
## <a name="secondary-server-tables"></a>Tabelle del server secondario  
  
|Tabella|Descrizione|  
|-----------|-----------------|  
|[log_shipping_monitor_alert](../../relational-databases/system-tables/log-shipping-monitor-alert-transact-sql.md)|Memorizza l'ID del processo per la gestione degli avvisi. Questa tabella viene utilizzata nel server secondario solo se non è stato configurato alcun server di monitoraggio remoto.|  
|[log_shipping_monitor_error_detail](../../relational-databases/system-tables/log-shipping-monitor-error-detail-transact-sql.md)|Archivia i dettagli relativi agli errori per processi di log shipping associati a questo server secondario.|  
|[log_shipping_monitor_history_detail](../../relational-databases/system-tables/log-shipping-monitor-history-detail-transact-sql.md)|Archivia i dettagli relativi alla cronologia per processi di log shipping associati a questo server secondario.|  
|[log_shipping_monitor_secondary](../../relational-databases/system-tables/log-shipping-monitor-secondary-transact-sql.md)|Archivia un record di monitoraggio per ogni database secondario associato a questo server secondario.|  
|[log_shipping_secondary](../../relational-databases/system-tables/log-shipping-secondary-transact-sql.md)|Include informazioni di configurazione per database secondari in un determinato server. Archivia una riga per ogni ID secondario.|  
|[log_shipping_secondary_databases](../../relational-databases/system-tables/log-shipping-secondary-databases-transact-sql.md)|Archivia informazioni di configurazione relative a un determinato database secondario. Archivia una riga per ogni database secondario.|  
  
> [!NOTE]  
>  Le impostazioni disponibili nella tabella **log_shipping_secondary** vengono condivise dai database secondari nello stesso server secondario per un determinato database primario. Se si modifica un'impostazione condivisa per un database secondario, tale impostazione verrà modificata in tutti i database secondari.  
  
## <a name="secondary-server-stored-procedures"></a>Stored procedure del server secondario  
  
|Stored Procedure|Descrizione|  
|----------------------|-----------------|  
|[sp_add_log_shipping_secondary_database](../../relational-databases/system-stored-procedures/sp-add-log-shipping-secondary-database-transact-sql.md)|Imposta un database secondario per il log shipping.|  
|[sp_add_log_shipping_secondary_primary](../../relational-databases/system-stored-procedures/sp-add-log-shipping-secondary-primary-transact-sql.md)|Imposta le informazioni primarie, aggiunge collegamenti di monitoraggio locale e remoto e crea processi di copia e di ripristino nel server secondario per il database primario specificato.|  
|[sp_change_log_shipping_secondary_database](../../relational-databases/system-stored-procedures/sp-change-log-shipping-secondary-database-transact-sql.md)|Modifica le impostazioni del database secondario, inclusi i record di monitoraggio locali e remoti.|  
|[sp_change_log_shipping_secondary_primary](../../relational-databases/system-stored-procedures/sp-change-log-shipping-secondary-primary-transact-sql.md)|Modifica le impostazioni del database secondario, ad esempio la directory di origine e di destinazione e il periodo di memorizzazione dei file.|  
|[sp_cleanup_log_shipping_history](../../relational-databases/system-stored-procedures/sp-cleanup-log-shipping-history-transact-sql.md)|Elimina la cronologia a livello locale e sul monitor, in base al periodo di memorizzazione.|  
|[sp_delete_log_shipping_secondary_database](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-secondary-database-transact-sql.md)|Rimuove un database secondario e la cronologia locale e remota.|  
|[sp_delete_log_shipping_secondary_primary](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-secondary-primary-transact-sql.md)|Rimuove dal server secondario le informazioni relative al server primario specificato.|  
|[sp_help_log_shipping_secondary_database](../../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-database-transact-sql.md)|Recupera le impostazioni del database secondario dalle tabelle **log_shipping_secondary**, **log_shipping_secondary_databases** e **log_shipping_monitor_secondary** .|  
|[sp_help_log_shipping_secondary_primary](../../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-primary-transact-sql.md)|Tramite questa stored procedure vengono recuperate le impostazioni di un database primario specificato nel server secondario.|  
|[sp_refresh_log_shipping_monitor](../../relational-databases/system-stored-procedures/sp-refresh-log-shipping-monitor-transact-sql.md)|Aggiorna il monitor, visualizzando le informazioni più recenti per l'agente di log shipping specificato.|  
  
## <a name="monitor-server-tables"></a>Tabelle del server di monitoraggio  
  
|Tabella|Descrizione|  
|-----------|-----------------|  
|[log_shipping_monitor_alert](../../relational-databases/system-tables/log-shipping-monitor-alert-transact-sql.md)|Memorizza l'ID del processo per la gestione degli avvisi.|  
|[log_shipping_monitor_error_detail](../../relational-databases/system-tables/log-shipping-monitor-error-detail-transact-sql.md)|Archivia i dettagli relativi agli errori per processi di log shipping.|  
|[log_shipping_monitor_history_detail](../../relational-databases/system-tables/log-shipping-monitor-history-detail-transact-sql.md)|Archivia i dettagli relativi alla cronologia per processi di log shipping.|  
|[log_shipping_monitor_primary](../../relational-databases/system-tables/log-shipping-monitor-primary-transact-sql.md)|Archivia un record di monitoraggio per ogni database primario associato a questo server di monitoraggio.|  
|[log_shipping_monitor_secondary](../../relational-databases/system-tables/log-shipping-monitor-secondary-transact-sql.md)|Archivia un record di monitoraggio per ogni database secondario associato con questo server di monitoraggio.|  
  
## <a name="monitor-server-stored-procedures"></a>Stored procedure del server di monitoraggio  
  
|Stored Procedure|Descrizione|  
|----------------------|-----------------|  
|[sp_add_log_shipping_alert_job](../../relational-databases/system-stored-procedures/sp-add-log-shipping-alert-job-transact-sql.md)|Crea un processo di avviso di log shipping, se non ne è ancora stato creato uno.|  
|[sp_delete_log_shipping_alert_job](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-alert-job-transact-sql.md)|Rimuove un processo di avviso di log shipping se non è disponibile alcun database primario associato.|  
|[sp_help_log_shipping_alert_job](../../relational-databases/system-stored-procedures/sp-help-log-shipping-alert-job-transact-sql.md)|Restituisce l'ID processo del processo per la gestione degli avvisi.|  
|[sp_help_log_shipping_monitor_primary](../../relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-primary-transact-sql.md)|Restituisce i record di monitoraggio per il database primario specificato dalla tabella **log_shipping_monitor_primary** .|  
|[sp_help_log_shipping_monitor_secondary](../../relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-secondary-transact-sql.md)|Restituisce i record di monitoraggio per il database secondario specificato dalla tabella **log_shipping_monitor_secondary** .|  
  
  
