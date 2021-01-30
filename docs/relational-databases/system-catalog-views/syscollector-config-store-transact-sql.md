---
description: syscollector_config_store (Transact-SQL)
title: syscollector_config_store (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- syscollector_config_store_TSQL
- syscollector_config_store
dev_langs:
- TSQL
helpviewer_keywords:
- data collector view
- syscollector_config_store view
ms.assetid: f15f6b05-6808-4b76-b6a8-48dec844cf63
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0ffb12c3aa43b1e18e20fdf43cf0311c02881138
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199771"
---
# <a name="syscollector_config_store-transact-sql"></a>syscollector_config_store (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce le proprietà che si applicano all'intero agente di raccolta dati, a differenza di un'istanza di un set di raccolta. Ogni riga in questa vista descrive una specifica proprietà dell'agente di raccolta dati, ad esempio il nome del data warehouse di gestione e il nome dell'istanza in cui si trova il data warehouse di gestione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|parameter_name|**nvarchar(128)**|Nome della proprietà. Non ammette i valori Null.|  
|parameter_value|**sql_variant**|Valore effettivo della proprietà. Ammette i valori Null.|  
  
## <a name="permissions"></a>Autorizzazioni  
 Richiede l'autorizzazione SELECT sulla vista o l'appartenenza ai ruoli predefiniti del database dc_operator, dc_proxy o dc_admin.  
  
## <a name="remarks"></a>Commenti  
 L'elenco di proprietà disponibile è fisso e i relativi valori possono essere modificati utilizzando solo la stored procedure appropriata. Nella tabella seguente vengono descritte le proprietà esposte tramite questa vista.  
  
|Nome proprietà|Descrizione|  
|-------------------|-----------------|  
|CacheDirectory|Nome della directory nel file system in cui i pacchetti del tipo di agente di raccolta archiviano le informazioni temporanee.<br /><br /> NULL = viene utilizzata la directory predefinita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|CacheWindow|Indica i criteri di memorizzazione dei dati relativi alla directory della cache per i caricamenti dei dati con errori.<br /><br /> -1 = Vengono memorizzati i dati relativi a tutti gli errori di caricamento.<br /><br /> 0 = Non viene memorizzato alcun dato relativo a errori di caricamento.<br /><br /> *n* = conserva i dati di *n* errori di caricamento precedenti, dove *n* >= 1.<br /><br /> Utilizzare la stored procedure sp_syscollector_set_cache_window stored per modificare questo valore.|  
|CollectorEnabled|Indica lo stato dell'agente di raccolta dati.<br /><br /> 0 = disabilitato<br /><br /> 1 = abilitato<br /><br /> Utilizzare la stored procedure sp_syscollector_enable_collector o sp_syscollector_disable_collector per modificare questo valore.|  
|MDWDatabase|Nome del data warehouse di gestione. Utilizzare la stored procedure sp_syscollector_set_warehouse_database_name per modificare questo valore.|  
|MDWInstance|Nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per il data warehouse di gestione. Utilizzare la stored procedure sp_syscollector_set_warehouse_instance_name per modificare questo valore.|  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene eseguita una query sulla vista syscollector_config_store.  
  
```sql  
SELECT parameter_name, parameter_value  
FROM msdb.dbo.syscollector_config_store;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure dell'agente di raccolta dati &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [Viste dell'agente di raccolta dati &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/data-collector-views-transact-sql.md)   
 [Raccolta dati](../../relational-databases/data-collection/data-collection.md)   
 [sp_syscollector_enable_collector &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syscollector-enable-collector-transact-sql.md)   
 [sp_syscollector_disable_collector &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syscollector-disable-collector-transact-sql.md)   
 [sp_syscollector_set_warehouse_database_name &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syscollector-set-warehouse-database-name-transact-sql.md)   
 [sp_syscollector_set_warehouse_instance_name &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syscollector-set-warehouse-instance-name-transact-sql.md)   
 [sp_syscollector_set_cache_window &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syscollector-set-cache-window-transact-sql.md)  
  
  
