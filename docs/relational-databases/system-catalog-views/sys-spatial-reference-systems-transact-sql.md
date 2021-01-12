---
description: sys.spatial_reference_systems (Transact-SQL)
title: sys.spatial_reference_systems (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- spatial_reference_systems_TSQL
- sys.spatial_reference_systems_TSQL
- sys.spatial_reference_systems
- spatial_reference_systems
dev_langs:
- TSQL
helpviewer_keywords:
- sys.spatial_reference_systems catalog view
- spatial_reference_systems
ms.assetid: 3c9bc120-67c3-463f-9e24-29fd623f25a0
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d968fc9ccb4d54e5345250477d72281152b29f68
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100185"
---
# <a name="sysspatial_reference_systems-transact-sql"></a>sys.spatial_reference_systems (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Consente di elencare i sistemi di riferimento spaziale (SRID) supportati da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|spatial_reference_id|**int**|Identificatore SRID supportato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|authority_name|**nvarchar(128)**|Autorità dell'identificatore SRID.|  
|authorized_spatial_reference_id|**int**|Identificatore SRID fornito dall'autorità denominata in **authority_name**.|  
|well_known_text|**nvarchar(4000)**|Rappresentazione WKT dell'identificatore SRID.|  
|unit_of_measure|**nvarchar(128)**|Nome dell'unità di misura.|  
|unit_conversion_factor|**float**|Lunghezza dell'unità di misura in metri.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]  
  
  
