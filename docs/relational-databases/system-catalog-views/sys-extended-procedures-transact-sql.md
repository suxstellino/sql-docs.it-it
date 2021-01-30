---
description: sys.extended_procedures (Transact-SQL)
title: sys.extended_procedures (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- extended_procedures
- sys.extended_procedures
- sys.extended_procedures_TSQL
- extended_procedures_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.extended_procedures catalog view
ms.assetid: 310e0f87-0044-4fdf-bd12-51a723a74ce6
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c148fb0bd8aa9af0f49a6d6e7d5a239af00b64e1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203824"
---
# <a name="sysextended_procedures-transact-sql"></a>sys.extended_procedures (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una riga per ogni oggetto che è un stored procedure esteso, con **sys. Objects. Type** = X. Poiché le stored procedure estese sono installate nel database **Master** , sono visibili solo da quel contesto di database. Se si seleziona dalla vista **sys.extended_procedures** in un altro contesto di database, viene restituito un set di risultati vuoto.  

  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**\<Columns inherited from sys.objects>**||Per un elenco di colonne ereditate da questa vista, vedere [sys. objects &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).|  
|**dll_name**|**nvarchar(260)**|Nome, incluso il percorso, della DLL di questa stored procedure estesa.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
