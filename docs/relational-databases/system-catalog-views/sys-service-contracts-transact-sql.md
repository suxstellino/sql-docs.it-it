---
description: sys.service_contracts (Transact-SQL)
title: sys.service_contracts (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- service_contracts_TSQL
- sys.service_contracts_TSQL
- sys.service_contracts
- service_contracts
dev_langs:
- TSQL
helpviewer_keywords:
- sys.service_contracts catalog view
ms.assetid: 787dd47e-4210-439d-9c4a-57a727a0dbd8
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 384a0cca9866bd47473e9ab49f0a98177d5a83ed
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204506"
---
# <a name="sysservice_contracts-transact-sql"></a>sys.service_contracts (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Questa vista del catalogo contiene una riga per ogni contratto nel database.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**nome**|**sysname**|Nome del contratto, univoco all'interno del database. Non ammette i valori Null.|  
|**service_contract_id**|**int**|Identificatore del contratto. Non ammette i valori Null.|  
|**principal_id**|**int**|Identificatore dell'entità di database proprietaria del contratto. Ammette valori Null.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
  
