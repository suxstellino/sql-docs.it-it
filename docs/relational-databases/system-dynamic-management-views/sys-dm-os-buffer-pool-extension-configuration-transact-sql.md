---
description: sys.dm_os_buffer_pool_extension_configuration (Transact-SQL)
title: sys.dm_os_buffer_pool_extension_configuration (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/09/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_buffer_pool_extension_configuration
- sys.dm_os_buffer_pool_extension_configuration_TSQL
- dm_os_buffer_pool_extension_configuration_TSQL
- sys.dm_os_buffer_pool_extension_configuration
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_buffer_pool_extension_configuration dynamic management view
ms.assetid: d52cc481-4d29-4f33-b63d-231ec35d092f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 60e65c3dc79cccb0c3b01d7369e68cd6ff8efa53
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184952"
---
# <a name="sysdm_os_buffer_pool_extension_configuration-transact-sql"></a>sys.dm_os_buffer_pool_extension_configuration (Transact-SQL)

[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  Restituisce le informazioni di configurazione sull'estensione del pool di buffer in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Restituisce una riga per ogni file di estensione del pool di buffer.  
  

  
| Nome colonna | Tipo di dati | Descrizione |
| :---------- | :-------- | :---------- |
|path|**nvarchar**(256)|Percorso e nome del file della cache di estensione del pool di buffer. Ammette valori Null.|  
|file_id|**int**|ID del file di estensione del pool di buffer. Non ammette i valori Null.|  
|state|**int**|Stato della funzionalità di estensione del pool di buffer. Non ammette i valori Null.<br /><br /> 0 - Estensione pool di buffer disabilitata<br /><br /> 1 - Disabilitazione estensione pool di buffer<br /><br /> 2-riservato per l'uso futuro<br /><br /> 3 - Abilitazione estensione pool di buffer<br /><br /> 4 - Riservato per utilizzi futuri<br /><br /> 5 - Estensione pool di buffer abilitata|  
|state_description|**nvarchar**(60)|Descrive lo stato della funzionalità di estensione del pool di buffer. Ammette i valori Null.<br /><br /> 0 = BUFFER POOL EXTENSION DISABLED<br /><br /> 5 = ESTENSIONE DEL POOL DI BUFFER ABILITATA|
|current_size_in_kb|**bigint**|Dimensione corrente del file di estensione del pool di buffer. Non ammette i valori Null.|
| &nbsp; | &nbsp; | &nbsp; |

## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW SERVER STATE per il server.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-returning-configuration-buffer-pool-extension-information"></a>R. Restituzione delle informazioni di configurazione sull'estensione del pool di buffer  
 Nell'esempio seguente vengono restituite tutte le colonne dalla DMV sys.dm_os_buffer_pool_extension_configuration.  
  
```sql  
SELECT path, file_id, state, state_description, current_size_in_kb  
FROM sys.dm_os_buffer_pool_extension_configuration;  
```  
  
### <a name="b-returning-the-number-of-cached-pages-in-the-buffer-pool-extension-file"></a>B. Restituzione del numero di pagine memorizzate nella cache di estensione del pool di buffer  
 Nell'esempio seguente viene restituito il numero delle pagine memorizzate nella cache di ogni file di estensione del pool di buffer.  
  
```sql  
SELECT COUNT(*) AS cached_pages_count  
FROM sys.dm_os_buffer_descriptors  
WHERE is_in_bpool_extension <> 0  
;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Estensione pool di buffer](../../database-engine/configure-windows/buffer-pool-extension.md)   
 [sys.dm_os_buffer_descriptors &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-buffer-descriptors-transact-sql.md)  
  
  
