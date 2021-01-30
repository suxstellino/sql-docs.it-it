---
description: dbo.syssubsystems (Transact-SQL)
title: Sottosistemi dbo.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dbo.syssubsystems
- syssubsystems
- syssubsystems_TSQL
- dbo.syssubsystems_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- syssubsystems system table
ms.assetid: 114b3d55-1ad6-4777-b868-8ef0c86ba596
author: cawrites
ms.author: chadam
ms.openlocfilehash: 71de173aa467cd3365df8154f63bc7e469fa83b4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201211"
---
# <a name="dbosyssubsystems-transact-sql"></a>dbo.syssubsystems (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene informazioni su tutti i sottosistemi proxy di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent disponibili. La tabella **syssubsystems** è archiviata nel database **msdb** .  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**subsystem_id**|**int**|ID del sottosistema.|  
|**sottosistema**|**nvarchar(40)**|Nome del sottosistema.|  
|**description_id**|**int**|ID del messaggio della riga nella vista del catalogo **sys. messages** che contiene la descrizione del sottosistema.|  
|**subsystem_dll**|**nvarchar(255)**|Posizione del file dll del sottosistema.|  
|**agent_exe**|**nvarchar(255)**|Percorso completo del file eseguibile che utilizza il sottosistema.|  
|**start_entry_point**|**nvarchar(30)**|Funzione richiamata quando il sottosistema viene inizializzato.|  
|**event_entry_point**|**nvarchar(30)**|Funzione richiamata quando viene eseguito un passaggio del sottosistema.|  
|**stop_entry_point**|**nvarchar(30)**|Funzione richiamata quando l'esecuzione di un sottosistema viene interrotta.|  
|**max_worker_threads**|**int**|Numero massimo di passaggi simultanei per un sottosistema specifico.|  
  
## <a name="remarks"></a>Commenti  
 Solo i membri del ruolo predefinito del server **sysadmin** possono accedere a questa tabella.  
  
## <a name="see-also"></a>Vedere anche  
 [dbo.sysproxysubsystem &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/dbo-sysproxysubsystem-transact-sql.md)   
 [ Proxy didbo.sys&#40;&#41;Transact-SQL ](../../relational-databases/system-tables/dbo-sysproxies-transact-sql.md)   
 [sys.messages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)  
  
  
