---
description: sys.column_xml_schema_collection_usages (Transact-SQL)
title: sys.column_xml_schema_collection_usages (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- column_xml_schema_collection_usages_TSQL
- sys.column_xml_schema_collection_usages
- column_xml_schema_collection_usages
- sys.column_xml_schema_collection_usages_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.column_xml_schema_collection_usages catalog view
ms.assetid: 4fd1ec7f-b9dc-4ddb-ab3a-0d59ab05ad20
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 919c411fee5ef0a1a8dc13e46fa9632a72e89629
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102911"
---
# <a name="syscolumn_xml_schema_collection_usages-transact-sql"></a>sys.column_xml_schema_collection_usages (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce una riga per ogni colonna convalidata da un XML Schema.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID dell'oggetto a cui appartiene la colonna.|  
|**column_id**|**int**|ID della colonna. Valore univoco all'interno dell'oggetto.|  
|**xml_collection_id**|**int**|ID della raccolta contenente lo spazio dei nomi dell'XML Schema di convalida per la colonna.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [XML Schema &#40;viste del catalogo&#41; di sistema di tipo XML &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/xml-schemas-xml-type-system-catalog-views-transact-sql.md)  
  
  
