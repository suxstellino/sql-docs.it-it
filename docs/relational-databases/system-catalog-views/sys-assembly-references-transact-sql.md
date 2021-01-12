---
description: sys.assembly_references (Transact-SQL)
title: sys.assembly_references (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- assembly_references
- sys.assembly_references_TSQL
- assembly_references_TSQL
- sys.assembly_references
dev_langs:
- TSQL
helpviewer_keywords:
- sys.assembly_references catalog view
ms.assetid: 50a5ed42-2d5b-4a11-a0d2-9a02241b078d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 278214acd7a8a0fd0b78fb8bb77d30e73f68000c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099247"
---
# <a name="sysassembly_references-transact-sql"></a>sys.assembly_references (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una riga per ogni coppia di assembly, in cui uno fa riferimento diretto all'altro.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**assembly_id**|**int**|ID dell'assembly a cui appartiene il riferimento.|  
|**referenced_assembly_id**|**int**|ID dell'assembly a cui si fa riferimento.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo degli assembly CLR &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/clr-assembly-catalog-views-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [ASSEMBLYPROPERTY &#40;&#41;Transact-SQL ](../../t-sql/functions/assemblyproperty-transact-sql.md)  
  
  
