---
description: sys.plan_guides (Transact-SQL)
title: sys.plan_guides (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.planguides_TSQL
- plan_guides
- sys.planguides
- plan_guides_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.plan_guides catalog view
ms.assetid: 3dde0397-ef6f-4b3f-8250-3f25584eb62b
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 747efa3b00c07d3563a7f05b375f2d16adb01909
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095481"
---
# <a name="sysplan_guides-transact-sql"></a>sys.plan_guides (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Contiene una riga per ogni guida di piano nel database.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**plan_guide_id**|**int**|Identificatore univoco della guida di piano nel database.|  
|**nome**|**sysname**|Nome della guida di piano.|  
|**create_date**|**datetime**|Data e ora di creazione della guida di piano.|  
|**modify_date**|**DateTime**|Data dell'ultima modifica della guida di piano.|  
|**is_disabled**|**bit**|1 = La guida di piano è disabilitata.<br /><br /> 0 = La guida di piano è abilitata.|  
|**query_text**|**nvarchar(max)**|Testo della query in cui è stata creata la guida di piano.|  
|**scope_type**|**tinyint**|Identifica l'ambito della guida di piano.<br /><br /> 1 = OBJECT<br /><br /> 2 = SQL<br /><br /> 3 = TEMPLATE|  
|**scope_type_desc**|**nvarchar(60)**|Descrizione dell'ambito della guida di piano.<br /><br /> OBJECT<br /><br /> SQL<br /><br /> TEMPLATE|  
|**scope_object_id**|**Int**|object_id dell'oggetto che definisce l'ambito della guida di piano, se l'ambito è OBJECT.<br /><br /> NULL se la guida di piano non è definita a livello di ambito di OBJECT.|  
|**scope_batch**|**nvarchar(max)**|Testo del batch, se **scope_type** è SQL.<br /><br /> NULL se il tipo di batch non è SQL.<br /><br /> Se NULL e **scope_type** è SQL, viene applicato il valore di **query_text** .|  
|**parameters**|**nvarchar(max)**|Stringa che definisce l'elenco dei parametri associati alla guida di piano.<br /><br /> NULL = Nessun elenco di parametri è associato alla guida di piano.|  
|**hint**|**nvarchar(max)**|Hint della clausola OPTION associati alla guida di piano.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sp_create_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)   
 [sp_create_plan_guide_from_handle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md)  
  
  
