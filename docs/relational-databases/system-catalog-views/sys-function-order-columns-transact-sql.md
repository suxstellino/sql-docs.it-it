---
description: sys.function_order_columns (Transact-SQL)
title: sys.function_order_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- function_order_columns
- sys.function_order_columns_TSQL
- function_order_columns_TSQL
- sys.function_order_columns
dev_langs:
- TSQL
helpviewer_keywords:
- sys.function_order_columns catalog view
ms.assetid: 29287973-3125-4d35-8ca9-92cb45828854
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d66bf0eebbe2eb37c95a06cd03f07e2b8e2471f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191440"
---
# <a name="sysfunction_order_columns-transact-sql"></a>sys.function_order_columns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce una riga per ogni colonna che fa parte di un'espressione di **ordinamento** di una funzione con valori di tabella Common Language Runtime (CLR).  

  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID dell'oggetto (funzione CLR con valori di tabella) nel quale è definito l'ordinamento.|  
|**order_column_id**|**int**|ID della colonna di ordinamento. **order_column_id** è univoco solo all'interno **object_id**.<br /><br /> **order_column_id** rappresenta la posizione di questa colonna nell'ordinamento.|  
|**column_id**|**int**|ID della colonna in **object_id**.<br /><br /> **column_id** è univoco solo all'interno **object_id**.|  
|**is_descending**|**bit**|1 = Direzione di ordinamento decrescente per la colonna di ordinamento.<br /><br /> 0 = Direzione di ordinamento crescente per la colonna di ordinamento.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
