---
description: sys.sysforeignkeys (Transact-SQL)
title: sys.sysforeignkeys (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysforeignkeys
- sys.sysforeignkeys
- sys.sysforeignkeys_TSQL
- sysforeignkeys_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysforeignkeys system table
- sys.sysforeignkeys compatibility view
ms.assetid: 41544236-1c46-4501-be88-18c06963b6e8
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 265284479a511cd74bc3724bed8b0724aa1570e0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094211"
---
# <a name="syssysforeignkeys-transact-sql"></a>sys.sysforeignkeys (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Include informazioni sui vincoli FOREIGN KEY contenuti nelle definizioni delle tabelle nel database.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**constid**|**int**|ID del vincolo FOREIGN KEY.|  
|**fkeyid**|**int**|ID di oggetto della tabella che include il vincolo FOREIGN KEY.|  
|**rkeyid**|**int**|ID di oggetto della tabella a cui fa riferimento il vincolo FOREIGN KEY.|  
|**fkey**|**smallint**|ID della colonna di riferimento.|  
|**rkey**|**smallint**|ID della colonna con riferimenti.|  
|**keyno**|**smallint**|Posizione della colonna nell'elenco delle colonne di riferimento.|  
  
## <a name="see-also"></a>Vedere anche  
 [Mapping di tabelle di sistema a viste di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Viste della compatibilità &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
