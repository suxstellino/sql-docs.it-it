---
description: sys.destination_data_spaces (Transact-SQL)
title: sys.destination_data_spaces (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.destination_data_spaces
- destination_data_spaces_TSQL
- destination_data_spaces
- sys.destination_data_spaces_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.destination_data_spaces catalog view
ms.assetid: 92df932b-ad5c-43f8-81f4-b158823ab189
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: be85e170fd9a8c0d081a498516622fa2c91460a6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203940"
---
# <a name="sysdestination_data_spaces-transact-sql"></a>sys.destination_data_spaces (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una riga per ogni destinazione dello spazio dei dati di uno schema di partizionamento.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**partition_scheme_id**|**int**|ID dello schema di partizionamento dello spazio dei dati.|  
|**destination_id**|**int**|ID (ordinale in base 1) del mapping delle destinazioni, univoco nello schema di partizionamento.|  
|**data_space_id**|**int**|ID dello spazio dei dati a cui vengono mappati i dati per la destinazione dello schema.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** . Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
