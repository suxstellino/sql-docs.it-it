---
description: sys.sysreferences (Transact-SQL)
title: Riferimenti sys.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sysreferences
- sys.sysreferences_TSQL
- sysreferences
- sysreferences_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sysreferences compatibility view
- sysreferences system table
ms.assetid: 81276f13-202e-4e74-962d-46eb98c98d2e
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a40845c5a789a7d04dd604836a34800137c26d16
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99148372"
---
# <a name="syssysreferences-transact-sql"></a>sys.sysreferences (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Include i mapping tra le definizioni del vincolo FOREIGN KEY e le colonne con riferimenti all'interno del database.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**constid**|**int**|ID del vincolo FOREIGN KEY.|  
|**fkeyid**|**int**|ID della tabella di riferimento.|  
|**rkeyid**|**int**|ID della tabella con riferimenti.|  
|**rkeyindid**|**smallint**|ID dell'indice univoco nella tabella con riferimenti che include le colonne chiave con riferimenti.|  
|**keycnt**|**smallint**|Numero di colonne nella chiave.|  
|**forkeys**|**varbinary(32)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**refkeys**|**varbinary(32)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**fkeydbid**|**smallint**|Riservato.|  
|**rkeydbid**|**smallint**|Riservato.|  
|**fkey1**|**smallint**|ID della colonna di riferimento.|  
|**fkey2**|**smallint**|ID della colonna di riferimento.|  
|**fkey3**|**smallint**|ID della colonna di riferimento.|  
|**fkey4**|**smallint**|ID della colonna di riferimento.|  
|**fkey5**|**smallint**|ID della colonna di riferimento.|  
|**fkey6**|**smallint**|ID della colonna di riferimento.|  
|**fkey7**|**smallint**|ID della colonna di riferimento.|  
|**fkey8**|**smallint**|ID della colonna di riferimento.|  
|**fkey9**|**smallint**|ID della colonna di riferimento.|  
|**fkey10**|**smallint**|ID della colonna di riferimento.|  
|**fkey11**|**smallint**|ID della colonna di riferimento.|  
|**fkey12**|**smallint**|ID della colonna di riferimento.|  
|**fkey13**|**smallint**|ID della colonna di riferimento.|  
|**fkey14**|**smallint**|ID della colonna di riferimento.|  
|**fkey15**|**smallint**|ID della colonna di riferimento.|  
|**fkey16**|**smallint**|ID della colonna di riferimento.|  
|**rkey1**|**smallint**|ID della colonna con riferimenti.|  
|**rkey2**|**smallint**|ID della colonna con riferimenti.|  
|**rkey3**|**smallint**|ID della colonna con riferimenti.|  
|**rkey4**|**smallint**|ID della colonna con riferimenti.|  
|**rkey5**|**smallint**|ID della colonna con riferimenti.|  
|**rkey6**|**smallint**|ID della colonna con riferimenti.|  
|**rkey7**|**smallint**|ID della colonna con riferimenti.|  
|**rkey8**|**smallint**|ID della colonna con riferimenti.|  
|**rkey9**|**smallint**|ID della colonna con riferimenti.|  
|**rkey10**|**smallint**|ID della colonna con riferimenti.|  
|**rkey11**|**smallint**|ID della colonna con riferimenti.|  
|**rkey12**|**smallint**|ID della colonna con riferimenti.|  
|**rkey13**|**smallint**|ID della colonna con riferimenti.|  
|**rkey14**|**smallint**|ID della colonna con riferimenti.|  
|**rkey15**|**smallint**|ID della colonna con riferimenti.|  
|**rkey16**|**smallint**|ID della colonna con riferimenti.|  
  
## <a name="see-also"></a>Vedere anche  
 [Mapping di tabelle di sistema a viste di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Viste della compatibilità &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
