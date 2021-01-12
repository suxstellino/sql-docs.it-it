---
description: sys.service_contract_message_usages (Transact-SQL)
title: sys.service_contract_message_usages (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- service_contract_message_usages_TSQL
- sys.service_contract_message_usages
- sys.service_contract_message_usages_TSQL
- service_contract_message_usages
dev_langs:
- TSQL
helpviewer_keywords:
- sys.service_contract_message_usages catalog view
ms.assetid: f783e662-126c-4595-8e22-f9d05191f5d0
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 59c8ca70580c06e85857fd0392717804cec6e4fa
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096715"
---
# <a name="sysservice_contract_message_usages-transact-sql"></a>sys.service_contract_message_usages (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Questa vista del catalogo contiene una riga per coppia (contratto, tipo di messaggio).  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**service_contract_id**|**int**|Identificatore del contratto che utilizza il tipo di messaggio. Non ammette i valori Null.|  
|**message_type_id**|**int**|Identificatore del tipo di messaggio utilizzato dal contratto. Non ammette i valori Null.|  
|**is_sent_by_initiator**|**bit**|Il tipo di messaggio può essere inviato dall'initiator della conversazione. Non ammette i valori Null.|  
|**is_sent_by_target**|**bit**|Il tipo di messaggio può essere inviato dalla destinazione della conversazione. Non ammette i valori Null.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
  
