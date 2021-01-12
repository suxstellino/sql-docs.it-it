---
description: dbo.sysproxysubsystem (Transact-SQL)
title: dbo.sysproxysubsystem (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.sysproxysubsystem_TSQL
- dbo.sysproxysubsystem
- sysproxysubsystem_TSQL
- sysproxysubsystem
dev_langs:
- TSQL
helpviewer_keywords:
- sysproxysubsystem system table
ms.assetid: 6d7713f5-1253-4a19-b1fb-635c377c95c1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 700d55e45dc153834cfb12aa9883f8faa6fac909
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098310"
---
# <a name="dbosysproxysubsystem-transact-sql"></a>dbo.sysproxysubsystem (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Registra quale sottosistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent viene utilizzato da ogni account proxy. Questa tabella è archiviata nel database **msdb** .  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**subsystem_id**|**int**|ID del sottosistema. Questo valore corrisponde alla colonna **subsystem_id** nella tabella **syssubsystems** .|  
|**proxy_id**|**int**|ID dell'account proxy. Questo valore corrisponde alla colonna **proxy_id** nella tabella **sysproxies** .|  
  
## <a name="remarks"></a>Osservazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono accedere a questa tabella.  
  
## <a name="see-also"></a>Vedere anche  
 [ Sottosistemidbo.sys&#40;&#41;Transact-SQL ](../../relational-databases/system-tables/dbo-syssubsystems-transact-sql.md)   
 [ Proxy didbo.sys&#40;&#41;Transact-SQL ](../../relational-databases/system-tables/dbo-sysproxies-transact-sql.md)  
  
  
