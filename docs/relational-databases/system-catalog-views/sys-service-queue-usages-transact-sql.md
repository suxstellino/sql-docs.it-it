---
description: sys.service_queue_usages (Transact-SQL)
title: sys.service_queue_usages (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.service_queue_usages
- sys.service_queue_usages_TSQL
- service_queue_usages
- service_queue_usages_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.service_queue_usages catalog view
ms.assetid: d58dcdaf-f82d-43d9-941b-f520581442bf
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 38f1bd8d054dfbc4b8a90ce9a9fa9554bee24685
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204484"
---
# <a name="sysservice_queue_usages-transact-sql"></a>sys.service_queue_usages (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Questa vista del catalogo restituisce una riga per ogni riferimento tra servizio e coda di servizio. Un servizio può essere associato a una sola coda. Una coda può essere associata a più servizi.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**service_id**|**int**|Identificatore del servizio. Univoco all'interno del database. Non ammette i valori Null.|  
|**service_queue_id**|**int**|Identificatore della coda di servizio utilizzata dal servizio. Non ammette i valori Null.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [sys. Services &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-services-transact-sql.md)  
  
  
