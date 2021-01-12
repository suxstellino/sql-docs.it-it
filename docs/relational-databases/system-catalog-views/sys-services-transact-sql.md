---
description: sys.services (Transact-SQL)
title: sys. Services (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.services
- services
- services_TSQL
- sys.services_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.services catalog view
ms.assetid: 16d0b0c5-5cce-469b-aa3d-4b9248e0c085
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ebaeddaead87c1c53367cfa9eadb81dd7be076a4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101731"
---
# <a name="sysservices-transact-sql"></a>sys.services (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Questa vista del catalogo contiene una riga per ogni servizio nel database.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**nome**|**sysname**|Nome del servizio, con distinzione tra maiuscole e minuscole e univoco all'interno del database. Non ammette i valori Null.|  
|**service_id**|**int**|Identificatore del servizio. Non ammette i valori Null.|  
|**principal_id**|**int**|Identificatore dell'entità di database proprietaria del servizio. Ammette valori Null.|  
|**service_queue_id**|**int**|ID oggetto per la coda utilizzata dal servizio. Non ammette i valori Null.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
  
