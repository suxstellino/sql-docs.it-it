---
description: sys.dm_database_copies (Database di SQL Azure)
title: sys.dm_database_copies (database SQL di Azure) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.service: sql-database
ms.reviewer: ''
ms.topic: language-reference
f1_keywords:
- dm_database_copies_TSQL
- sys.dm_database_copies
- dm_database_copies
- sys.dm_database_copies_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dm_database_copies
- sys.dm_database_copies
ms.assetid: d03d4657-86d1-4496-97e6-cc3bc292e0b1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.openlocfilehash: ef0c92766cfb03f56dfc08a476cb7e81dbf4743d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095184"
---
# <a name="sysdm_database_copies-azure-sql-database"></a>sys.dm_database_copies (Database di SQL Azure)
[!INCLUDE[Azure SQL Database](../../includes/applies-to-version/asdb.md)]

  Restituisce informazioni sulla copia del database.  
  
Per restituire informazioni sui collegamenti di replica geografica, usare le visualizzazioni [sys.geo_replication_links](../../relational-databases/system-dynamic-management-views/sys-geo-replication-links-azure-sql-database.md) o [sys.dm_geo_replication_link_status](../../relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database.md) (disponibili nel database SQL V12).
  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|ID del database corrente nella vista `sys.databases`.|  
|**start_date**|**datetimeoffset**|Ora UTC in un data center del [!INCLUDE[ssSDS](../../includes/sssds-md.md)] regionale in cui è stata avviata la copia del database.|  
|**modify_date**|**datetimeoffset**|Ora UTC in un data center del [!INCLUDE[ssSDS](../../includes/sssds-md.md)] regionale in cui è stata completata la copia del database. Al momento, il nuovo database è transazionalmente coerente con il database primario. Le informazioni di completamento vengono aggiornate ogni minuto.<br /><br />Ora UTC che riflette l'ultimo aggiornamento del campo percent_complete.|  
|**percent_complete**|**real**|Percentuale di byte copiati. I valori validi sono compresi tra 0 e 100. Il [!INCLUDE[ssSDS](../../includes/sssds-md.md)] potrebbe recuperare automaticamente da alcuni errori, ad esempio il failover, e riavviare la copia del database. In questo caso, percent_complete inizierebbe di nuovo da 0.|  
|**error_code**|**int**|Se maggiore di 0, il codice che indica l'errore che si è verificato durante copia. Il valore è uguale a 0 se non si sono verificati errori.|  
|**error_desc**|**nvarchar (4096)**|Descrizione dell'errore che si è verificato durante la copia.|  
|**error_severity**|**int**|Restituisce 16 se la copia del database ha esito negativo.|  
|**error_state**|**int**|Restituisce 1 se la copia ha esito negativo.|  
|**copy_guid**|**uniqueidentifier**|ID univoco dell'operazione di copia.|  
|**partner_server**|**sysname**|Nome del server di database SQL in cui viene creata la copia.|  
|**partner_database**|**sysname**|Nome della copia del database nel server partner.|  
|**replication_state**|**tinyint**|Stato della replica a copia continua per questo database. I valori possibili sono:<br /><br /> 0 = in sospeso. La creazione della copia del database è stata pianificata, ma i passaggi necessari per la preparazione non sono ancora stati completati o sono temporaneamente bloccati dalla quota di seeding.<br /><br /> 1 = seeding. Il database di copia sottoposta a seeding non è ancora completamente sincronizzato con il database di origine. In questo stato non è possibile connettersi alla copia. Per annullare l'operazione di seeding in corso, è necessario eliminare il database di copia.|  
|**replication_state_desc**|**nvarchar(256)**|Descrizione di replication_state. I valori possibili sono:<br /><br /> PENDING<br /><br /> SEEDING<br />|  
|**maximum_lag**|**int**|Campo riservato.|  
|**is_continuous_copy**|**bit**|0 = restituisce 0|  
|**is_target_role**|**bit**|0 = database di origine<br /><br /> 1 = copia database|  
|**is_interlink_connected**|bit|Campo riservato.|  
|**is_offline_secondary**|bit|Campo riservato.|  
  
## <a name="permissions"></a>Autorizzazioni  
 Questa vista è disponibile solo nel database **Master** per l'account di accesso dell'entità di livello server.  
  
## <a name="remarks"></a>Osservazioni  
 È possibile utilizzare la vista **sys.dm_database_copies** nel database **Master** del server di origine o di destinazione [!INCLUDE[ssSDS](../../includes/sssds-md.md)] . Quando la copia del database viene completata correttamente e il nuovo database diventa ONLINE, la riga nella vista **sys.dm_database_copies** viene rimossa automaticamente.  
  
  
