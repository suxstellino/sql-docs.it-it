---
description: sys.dm_os_enumerate_fixed_drives (Transact-SQL)
title: sys.dm_os_enumerate_fixed_drives (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/18/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_os_enumerate_fixed_drives
- sys.dm_os_enumerate_fixed_drives_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_enumerate_fixed_drives dynamic management view
ms.assetid: 2e27489e-cf69-4a89-9036-77723ac3de66
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e909c2b38df99f0e2cd5aa4addc2e657f60b5cfa
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097578"
---
# <a name="sysdm_os_enumerate_fixed_drives-transact-sql"></a>sys.dm_os_enumerate_fixed_drives (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Introdotta in SQL Server 2019.

Enumera i volumi montati in lettere di unità quali `C:\` .

|Nome colonna|Tipo di dati|Descrizione|
|-----------------|---------------|-----------------|  
|`fixed_drive_path`|`nvarchar(512)`|Percorso del volume, ad esempio `C:\` .|  
|`drive_type`|`int`|Codice per il tipo di unità. Vedere la [ `GetDriveTypeW` funzione](/windows/win32/api/fileapi/nf-fileapi-getdrivetypew).|
|`drive_type_desc`|`nvarchar(512)`|Descrizione del tipo di unità. Vedere la [ `GetDriveTypeW` funzione](/windows/win32/api/fileapi/nf-fileapi-getdrivetypew).|
|`free_space_in_bytes`|`bigint`|Spazio disponibile su disco in byte.|

## <a name="permissions"></a>Autorizzazioni

L'utente deve disporre dell' `VIEW SERVER STATE` autorizzazione per il server.

## <a name="see-also"></a>Vedere anche  

 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funzioni e viste a gestione dinamica relative a I/O &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md)  
