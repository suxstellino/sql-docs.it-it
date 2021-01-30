---
description: sys.dm_os_windows_info (Transact-SQL)
title: sys.dm_os_windows_info (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_windows_info
- dm_os_windows_info_TSQL
- sys.dm_os_windows_info
- sys.dm_os_windows_info_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_windows_info dynamic management view
ms.assetid: adc81283-fdc2-46c0-bb48-abe82bbf2459
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a7e693efca0460c234004d8db21d1496a3f305ff
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99144432"
---
# <a name="sysdm_os_windows_info-transact-sql"></a>sys.dm_os_windows_info (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce una riga in cui sono visualizzate le informazioni sulla versione del sistema operativo Windows.  
  
  Si applica solo a SQL Server in esecuzione in Windows. Per visualizzare un sistema simile per SQL Server in esecuzione in un host non Windows, ad esempio Linux, usare [sys.dm_os_host_info &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/sys-dm-os-host-info-transact-sql.md). 
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**windows_release**|**nvarchar(256)**|Per Windows, restituisce il numero di versione. Per un elenco di valori e descrizioni, vedere [versione del sistema operativo (Windows)](/windows/desktop/SysInfo/operating-system-version). Non può essere NULL.|  
|**windows_service_pack_level**|**nvarchar(256)**| Per Windows, restituisce il numero Service Pack. Non può essere NULL. |  
|**windows_sku**|**int**|Per Windows, restituisce l'ID dell'unità di supporto di Windows. Per un elenco di ID SKU e descrizioni, vedere [funzione GetProductInfo](/windows/win32/api/sysinfoapi/nf-sysinfoapi-getproductinfo). Ammette valori null. |  
|**os_language_version**|**int**| Per Windows, restituisce l'identificatore delle impostazioni locali (LCID) di Windows del sistema operativo. Per un elenco di valori e descrizioni LCID, vedere [ID delle impostazioni locali assegnati da Microsoft](/openspecs/windows_protocols/ms-lcid/a9eac961-e77d-41a6-90a5-ce1a8b0cdb9c). Non può essere NULL.|  
  
  
## <a name="permissions"></a>Autorizzazioni  
Per impostazione predefinita, l'autorizzazione SELECT per sys.dm_os_windows_info viene concessa al ruolo public. Se revocato, è richiesta l'autorizzazione VIEW SERVER STATE per il server.  

## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni
Per visualizzare l'instradamento per SQL in esecuzione su un host non Windows, ad esempio Linux, usare [sys.dm_os_host_info &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-host-info-transact-sql.md). 
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituite tutte le colonne dalla vista **sys.dm_os_windows_info** .  
  
```  
SELECT windows_release, windows_service_pack_level, windows_sku, os_language_version  
FROM sys.dm_os_windows_info;  
```  
  
 Set di risultati di esempio:  
  
 `windows_release  windows_service_pack_level   windows_sku   os_language_version`  
  
 `---------------  ---------------------------  ------------  -------------------`  
  
 `6.0              Service Pack 2                4            1033`  
  
## <a name="see-also"></a>Vedere anche  
 [sys.dm_os_sys_info &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)   
 [sys.dm_os_host_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-host-info-transact-sql.md)  
  
