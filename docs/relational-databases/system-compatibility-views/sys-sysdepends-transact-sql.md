---
description: sys.sysdepends (Transact-SQL)
title: sys.sysdepends (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sysdepends_TSQL
- sysdepends
- sysdepends_TSQL
- sys.sysdepends
dev_langs:
- TSQL
helpviewer_keywords:
- sysdepends system table
- sys.sysdepends compatibility view
ms.assetid: f9c182cb-386f-4e72-859f-9f1115b389f9
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 75cd326cb42798f6541ea1d40cb962b92bf617e1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201392"
---
# <a name="syssysdepends-transact-sql"></a>sys.sysdepends (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene informazioni sulle dipendenze tra oggetti (viste, procedure e trigger) nel database e oggetti (tabelle, viste e procedure) inclusi nella relativa definizione.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**id**|**int**|ID oggetto.|  
|**depid**|**int**|ID dell'oggetto dipendente.|  
|**number**|**smallint**|Numero della procedura.|  
|**depnumber**|**smallint**|Numero della procedura dipendente.|  
|**Stato**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**deptype**|**tinyint**|Identifica il tipo di oggetto dipendente:<br /><br /> 0 = Oggetto o colonna (solo riferimenti non associati a schema)<br /><br /> 1 = Oggetto o colonna (riferimenti associati a schema)|  
|**depdbid**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**depsiteid**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**selall**|**bit**|1 = l'oggetto viene utilizzato in un'istruzione SELECT *.<br /><br /> 0 = No.|  
|**resultobj**|**bit**|1 = l'oggetto viene aggiornato.<br /><br /> 0 = No.|  
|**readobj**|**bit**|1 = l'oggetto viene letto.<br /><br /> 0 = No.|  
  
## <a name="see-also"></a>Vedere anche  
 [Mapping di tabelle di sistema a viste di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Viste di compatibilità &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)   
 [sp_depends &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-depends-transact-sql.md)   
 [sys.sql_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-dependencies-transact-sql.md)  
  
  
