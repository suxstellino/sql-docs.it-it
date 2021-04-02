---
description: Viste a gestione dinamica relative al sistema operativo di SQL Server (Transact-SQL)
title: Viste a gestione dinamica relative al sistema operativo SQL Server (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/17/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- operating systems [SQL Server], dynamic management objects
- SQL Operating System dynamic management objects [SQL Server]
- SQL OS dynamic management objects [SQL Server]
- dynamic management objects [SQL Server], SQL OS
ms.assetid: 3030c86a-0a74-4fed-ac0f-392e244cb965
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a7131acd3e343308561d944ec251d76fbd2fbfdb
ms.sourcegitcommit: 295b9dfc758471ef7d238a2b0f92f93e34acbb1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/31/2021
ms.locfileid: "106054305"
---
# <a name="sql-server-operating-system-related-dynamic-management-views-transact-sql"></a>Viste a gestione dinamica relative al sistema operativo di SQL Server (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

In questa sezione vengono documentate le viste a gestione dinamica (DMV) associate al [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sistema operativo (SQLOS). SQLOS è responsabile della gestione delle risorse del sistema operativo specifiche di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

:::row:::
   :::column span="":::
      **sys.dm_os_buffer_descriptors**<br>      **sys.dm_os_buffer_pool_extension_configuration**<br>      **sys.dm_os_child_instances**<br>      **sys.dm_os_cluster_nodes** <br>      **sys.dm_os_cluster_properties**<br>      **sys.dm_os_dispatcher_pools** <br>      **sys.dm_os_enumerate_fixed_drives**<br>      **sys.dm_os_host_info** <br>      **sys.dm_os_hosts**<br>      **sys.dm_os_latch_stats** <br>      **sys.dm_os_loaded_modules**<br>      **sys.dm_os_memory_brokers**<br>      **sys.dm_os_memory_cache_clock_hands**<br>      **sys.dm_os_memory_cache_counters** <br>      **sys.dm_os_memory_cache_entries**<br>      **sys.dm_os_memory_cache_hash_tables**<br>      **sys.dm_os_memory_clerks**<br>      **sys.dm_os_memory_nodes**
   :::column-end:::
   :::column span="":::
      **sys.dm_os_nodes**<br>      **sys.dm_os_performance_counters**<br>      **sys.dm_os_process_memory**<br>      **sys.dm_os_schedulers**<br>      **sys.dm_os_server_diagnostics_log_configurations**<br>      **sys.dm_os_spinlock_stats** <br>      **sys.dm_os_stacks**<br>      **sys.dm_os_sys_info**<br>      **sys.dm_os_sys_memory**<br>      **sys.dm_os_tasks**<br>      **sys.dm_os_threads**<br>      **sys.dm_os_virtual_address_dump**<br>      **sys.dm_os_volume_stats**<br>      **sys.dm_os_waiting_tasks**<br>      **sys.dm_os_wait_stats**<br>      **sys.dm_os_windows_info**<br>      **sys.dm_os_workers** 
   :::column-end:::
:::row-end:::

 Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viste a gestione dinamica relative al sistema operativo seguenti sono [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] .  
  
:::row:::
   :::column span="":::
      **sys.dm_os_function_symbolic_name**<br>      **sys.dm_os_ring_buffers**  <br>      **sys.dm_os_memory_allocations**
   :::column-end:::
   :::column span="":::
      **sys.dm_os_sublatches**  <br>      **sys.dm_os_worker_local_storage**  
   :::column-end:::
:::row-end:::
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  

