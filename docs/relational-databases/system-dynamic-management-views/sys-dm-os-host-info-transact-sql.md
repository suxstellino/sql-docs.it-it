---
description: sys.dm_os_host_info (Transact-SQL)
title: sys.dm_os_host_info (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/10/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sys.dm_os_host_info
- sys.dm_os_host_info_TSQL
- dm_os_host_info
- dm_os_host_info_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_host_info dynamic management view
ms.assetid: 9bb6ef86-957b-4ca1-ad20-ca2f8460a86d
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 41462fd558a30639342dc75be5bb68a44fcedf67
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097561"
---
# <a name="sysdm_os_host_info-transact-sql"></a>sys.dm_os_host_info (Transact-SQL)
[!INCLUDE[SQL Server 2017](../../includes/applies-to-version/sqlserver2017.md)]

Restituisce una riga in cui sono visualizzate le informazioni sulla versione del sistema operativo.  
  
|Nome colonna |Tipo di dati |Descrizione |  
|-----------------|---------------|-----------------|  
|**host_platform** |**nvarchar(256)** |Tipo di sistema operativo: Windows o Linux |
|**host_distribution** |**nvarchar(256)** |Descrizione del sistema operativo. |
|**host_release**|**nvarchar(256)**|Versione del sistema operativo [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows (numero di versione). Per un elenco di valori e descrizioni, vedere [versione del sistema operativo (Windows)](/windows/desktop/SysInfo/operating-system-version). <br> Per Linux, restituisce una stringa vuota. |  
|**host_service_pack_level**|**nvarchar(256)**|Livello Service Pack del sistema operativo Windows <br> Per Linux, restituisce una stringa vuota. |  
|**host_sku**|**int**|ID Windows del codice di riferimento del prodotto (SKU). Per un elenco di ID SKU e descrizioni, vedere [funzione GetProductInfo](/windows/win32/api/sysinfoapi/nf-sysinfoapi-getproductinfo). Ammette i valori Null. <br> Per Linux, restituisce NULL. |  
|**os_language_version**|**int**|Identificatore delle impostazioni locali (LCID) Windows del sistema operativo. Per un elenco di valori e descrizioni LCID, vedere [ID delle impostazioni locali assegnati da Microsoft](/openspecs/windows_protocols/ms-lcid/a9eac961-e77d-41a6-90a5-ce1a8b0cdb9c). Non può essere null.|  

## <a name="remarks"></a>Osservazioni  
Questa visualizzazione è simile a [sys.dm_os_windows_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-windows-info-transact-sql.md), aggiungendo colonne per distinguere Windows e Linux.
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
`SELECT` `sys.dm_os_host_info` Per impostazione predefinita, l'autorizzazione per è concessa al `public` ruolo. Se revocato, richiede `VIEW SERVER STATE` l'autorizzazione per il server.   
 
> [!CAUTION]
>  A partire dalla versione [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1,3, la [!INCLUDE[ssManStudioFull_md](../../includes/ssmanstudiofull-md.md)] versione 17 richiede l'autorizzazione per la `SELECT` `sys.dm_os_host_info` connessione a [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] . Se `SELECT` l'autorizzazione viene revocata da `public` , solo gli account di accesso con `VIEW SERVER STATE` autorizzazione possono connettersi con la versione più recente di SSMS. Altri strumenti, ad esempio, `sqlcmd.exe` possono connettersi senza `SELECT` autorizzazione `sys.dm_os_host_info` .

  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente vengono restituite tutte le colonne dalla vista **sys.dm_os_host_info** .  
  
```  
SELECT host_platform, host_distribution, host_release, 
    host_service_pack_level, host_sku, os_language_version  
FROM sys.dm_os_host_info;  
```  

Ecco un set di risultati di esempio per Windows:
 
 |host_platform |host_distribution |host_release |host_service_pack_level |host_sku |os_language_version |
 |----- |----- |----- |----- |----- |----- |
 |Windows   |Windows Server 2012 R2 Standard    |6.3    |   |7  |1033 |  

Ecco un set di risultati di esempio in Linux:
 
 |host_platform |host_distribution |host_release |host_service_pack_level |host_sku |os_language_version |
 |----- |----- |----- |----- |----- |----- |
 |Linux |Ubuntu |16.04  |   |NULL   |1033 |  

  
## <a name="see-also"></a>Vedere anche  
 [sys.dm_os_sys_info &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)   
 [sys.dm_os_windows_info (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-windows-info-transact-sql.md)  
