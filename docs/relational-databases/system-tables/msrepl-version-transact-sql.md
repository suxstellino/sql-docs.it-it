---
description: MSrepl_version (Transact-SQL)
title: MSrepl_version (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSrepl_version
- MSrepl_version_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSrepl_version system table
ms.assetid: c1330f03-940b-4564-ac42-6030c6e21173
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2705f8bba91c26bebf886064b795f0f27a78ec2b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100571"
---
# <a name="msrepl_version-transact-sql"></a>MSrepl_version (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella **MSrepl_version** contiene una riga con la versione corrente della replica installata. Questa tabella è archiviata nel database di distribuzione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**major_version**|**int**|Numero di versione principale del database di distribuzione.|  
|**minor_version**|**int**|Numero di versione secondario del database di distribuzione.|  
|**revision**|**int**|Il numero di revisione.|  
|**db_existed**|**bit**|Indica se il database di distribuzione esiste prima della chiamata a **sp_adddistributiondb** .|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste della replica &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
