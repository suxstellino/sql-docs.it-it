---
description: sys.dm_resource_governor_resource_pool_affinity (Transact-SQL)
title: sys.dm_resource_governor_resource_pool_affinity (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_resource_governor_resource_pool_affinity_TSQL
- sys.dm_resource_governor_resource_pool_affinity
- dm_resource_governor_resource_pool_affinity
- dm_resource_governor_resource_pool_affinity_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dm_resource_governor_resource_pool_affinity
- sys.dm_resource_governor_resource_pool_affinity
ms.assetid: a197ec19-a2ba-44f5-a4f2-3eee33ebd77d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d5fa2993e4ae716f72ad93bc388c466e69f00da3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097545"
---
# <a name="sysdm_resource_governor_resource_pool_affinity-transact-sql"></a>sys.dm_resource_governor_resource_pool_affinity (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Consente di tenere traccia dell'affinità di pool di risorse.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
|Nome colonna|Tipo di dati|Descrizione|  
|----------------|---------------|-----------------|  
|Pool_id|**int**|ID del pool di risorse. Non ammette i valori Null.|  
|Processor_group|**smallint**|ID del gruppo di processori logici Windows. Non ammette i valori Null.|  
|Scheduler_mask|**bigint**|Maschera binaria che rappresenta le utilità di pianificazione associate a questo pool. Non ammette i valori Null.|  
  
## <a name="remarks"></a>Osservazioni  
 I pool creati con un'affinità equivalente ad AUTO non saranno visualizzati in questa vista perché non presentano affinità. Per ulteriori informazioni, vedere le istruzioni per la creazione di un [pool di risorse &#40;&#41;Transact-SQL ](../../t-sql/statements/create-resource-pool-transact-sql.md) e il [pool di risorse alter Resource &#40;transact-SQL&#41;](../../t-sql/statements/alter-resource-pool-transact-sql.md) .  
  
## <a name="see-also"></a>Vedere anche  
 [sys.dm_resource_governor_external_resource_pool_affinity &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pool-affinity-transact-sql.md)  
  
  
