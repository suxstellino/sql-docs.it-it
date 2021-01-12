---
description: sys.resource_governor_resource_pools (Transact-SQL)
title: sys.resource_governor_resource_pools (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.resource_governor_resource_pools
- resource_governor_resource_pools
- sys.resource_governor_resource_pools_TSQL
- resource_governor_resource_pools_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.resource_governor_resource_pools catalog view
ms.assetid: 56793e9c-aa90-452e-88c6-d9b799239888
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9ffab459e53f26d040c301adf818618b28fa05f1
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101782"
---
# <a name="sysresource_governor_resource_pools-transact-sql"></a>sys.resource_governor_resource_pools (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce la configurazione memorizzata del pool di risorse in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ogni riga della vista determina la configurazione di un pool.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|pool_id|**int**|ID univoco del pool di risorse. Non ammette i valori Null.|  
|name|**sysname**|Nome del pool di risorse. Non ammette i valori Null.|  
|min_cpu_percent|**int**|Larghezza di banda media garantita della CPU per tutte le richieste nel pool di risorse, in caso di contesa di CPU. Non ammette i valori Null.|  
|max_cpu_percent|**int**|Larghezza di banda media massima della CPU concessa per tutte le richieste nel pool di risorse, in caso di contesa di CPU. Non ammette i valori Null.|  
|min_memory_percent|**int**|Quantità di memoria garantita per tutte le richieste nel pool di risorse. Non è condivisa con altri pool di risorse. Non ammette i valori Null.|  
|max_memory_percent|**int**|Percentuale di memoria totale del server utilizzabile dalle richieste in questo pool di risorse. Non ammette i valori Null. Il valore massimo effettivo dipende dai valori minimi del pool. Ad esempio, impostando max_memory_percent su 100, il valore massimo effettivo risulta inferiore.|  
|cap_cpu_percent|**int**|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> Limite di utilizzo massimo della larghezza di banda della CPU concesso per tutte le richieste nel pool di risorse. Limita il livello massimo della larghezza di banda della CPU al livello specificato. L'intervallo consentito per il valore è compreso tra 1 e 100.|  
|min_iops_per_volume|**int**|**Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.<br /><br /> Operazioni di I/O minime al secondo (IOPS) per l'impostazione del volume del pool. 0 = Nessuna prenotazione. Non può essere null.|  
|max_iops_per_volume|**int**|**Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.<br /><br /> Operazioni di I/O massime al secondo (IOPS) per l'impostazione del volume del pool. 0 = Numero illimitato. Non può essere null.|  
  
## <a name="remarks"></a>Osservazioni  
 La vista del catalogo visualizza i metadati memorizzati. Per visualizzare la configurazione in memoria, utilizzare la vista a gestione dinamica corrispondente [sys.dm_resource_governor_resource_pools &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 Richiede l'autorizzazione VIEW ANY DEFINITION per visualizzare i contenuti e l'autorizzazione CONTROL SERVER per modificarli.  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo di Resource Governor &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/resource-governor-catalog-views-transact-sql.md)   
 [sys.dm_resource_governor_resource_pools &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md)   
 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)   
 [sys.resource_governor_external_resource_pools &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-resource-governor-external-resource-pools-transact-sql.md)  
  
  
