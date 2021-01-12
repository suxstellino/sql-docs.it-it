---
description: sys.system_components_surface_area_configuration (Transact-SQL)
title: sys.system_components_surface_area_configuration (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.system_components_surface_area_configuration_TSQL
- system_components_surface_area_configuration
- system_components_surface_area_configuration_TSQL
- sys.system_components_surface_area_configuration
dev_langs:
- TSQL
helpviewer_keywords:
- sys.system_components_surface_area_configuration catalog view
ms.assetid: d9920008-3387-4f9e-8f21-47473f2ba04f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7f5eecd0b62dc819f3526053fb22779299d4c449
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092981"
---
# <a name="syssystem_components_surface_area_configuration-transact-sql"></a>sys.system_components_surface_area_configuration (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce una riga per ogni oggetto di sistema eseguibile che può essere attivato o disabilitato da un componente di Configurazione superficie di attacco. Per ulteriori informazioni, vedere [Surface Area Configuration](../../relational-databases/security/surface-area-configuration.md).  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**component_name**|**sysname**|Nome del componente, le cui regole di confronto sono Latin1_General_CI_AS_KS_WS. Non può essere NULL.|  
|**database_name**|**sysname**|Database contenente l'oggetto, le cui regole di confronto sono Latin1_General_CI_AS_KS_WS. I possibili valori sono i seguenti:<br /><br /> **master**<br /><br /> **msdb**<br /><br /> **mssqlsystemresource**|  
|**schema_name**|**sysname**|Schema contenente l'oggetto, le cui regole di confronto sono Latin1_General_CI_AS_KS_WS. Non può essere NULL.|  
|**object_name**|**sysname**|Nome dell'oggetto. le cui regole di confronto sono Latin1_General_CI_AS_KS_WS. Non può essere NULL.|  
|**state**|**tinyint**|0 = Disabilitato<br /><br /> 1 = Abilitato|  
|**type**|**char(2)**|Tipo di oggetto. Può essere uno dei seguenti:<br /><br /> P = SQL_STORED_PROCEDURE<br /><br /> PC = CLR_STORED_PROCEDURE<br /><br /> FN = SQL_SCALAR_FUNCTION<br /><br /> FS = CLR_SCALAR_FUNCTION<br /><br /> FT = CLR_TABLE_VALUED_FUNCTION<br /><br /> IF = SQL_INLINE_TABLE_VALUED_FUNCTION<br /><br /> TF = SQL_TABLE_VALUED_FUNCTION<br /><br /> X = EXTENDED_STORED_PROCEDURE|  
|**type_desc**|**nvarchar(60)**|Descrizione del nome del tipo di oggetto.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Viste del catalogo relative alla sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
