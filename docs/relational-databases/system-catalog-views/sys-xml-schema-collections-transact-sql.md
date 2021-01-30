---
description: sys.xml_schema_collections (Transact-SQL)
title: sys.xml_schema_collections (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.xml_schema_collections_TSQL
- sys.xml_schema_collections
- xml_schema_collections
- xml_schema_collections_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.xml_schema_collections catalog view
ms.assetid: f3f7f3dc-029f-4942-ab3c-75fa9814e40f
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8533727e5e59196b84625010f079e5a3864c9d0f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192531"
---
# <a name="sysxml_schema_collections-transact-sql"></a>sys.xml_schema_collections (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce una riga per ogni raccolta di XML Schema. Una raccolta di XML Schema è un set denominato di definizioni XSD. La raccolta di XML Schema è a sua volta contenuta in uno schema relazionale e viene identificata da un nome [!INCLUDE[tsql](../../includes/tsql-md.md)] con ambito schema. Le tuple seguenti sono univoche: xml_collection_id, schema_id e name.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|xml_collection_id|**int**|Nome della raccolta di XML Schema. Univoco all'interno del database.|  
|schema_id|**int**|ID dello schema relazionale contenente la raccolta di XML Schema.|  
|principal_id|**int**|ID del singolo proprietario se diverso dal proprietario dello schema. Per impostazione predefinita, gli oggetti contenuti nello schema appartengono al proprietario dello schema stesso. È tuttavia possibile specificare un proprietario alternativo tramite l'istruzione ALTER AUTHORIZATION per modificare la proprietà.<br /><br /> NULL = Nessun proprietario singolo alternativo.|  
|name|**sysname**|Nome della raccolta di XML Schema.|  
|create_date|**datetime**|Data di creazione della raccolta di XML Schema.|  
|modify_date|**datetime**|Data dell'ultima modifica della raccolta di XML Schema.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [XML Schema &#40;viste del catalogo&#41; di sistema di tipo XML &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/xml-schemas-xml-type-system-catalog-views-transact-sql.md)   
 [Domande frequenti sull'esecuzione di query sul catalogo di sistema di SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
