---
description: sys.conversation_groups (Transact-SQL)
title: sys.conversation_groups (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- conversation_groups_TSQL
- conversation_groups
- sys.conversation_groups
- sys.conversation_groups_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.conversation_groups catalog view
ms.assetid: 3f35815e-2de4-42a2-a972-8f0141dad0b3
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8efd718c9c5ab4d7c1186d3a85c70c1ced1c51ae
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100655"
---
# <a name="sysconversation_groups-transact-sql"></a>sys.conversation_groups (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Questa vista del catalogo contiene una riga per ogni gruppo di conversazioni.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**conversation_group_id**|**uniqueidentifier**|Identificatore del gruppo di conversazioni. Non ammette i valori Null.|  
|**service_id**|**int**|Identificatore del servizio per le conversazioni nel gruppo. Non ammette i valori Null.|  
|**is_system**|**bit**|Indica se si tratta di un'istanza di sistema. Ammette valori Null.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
  
