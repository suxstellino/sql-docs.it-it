---
description: sys.sp_xtp_control_proc_exec_stats (Transact-SQL)
title: sys.sp_xtp_control_proc_exec_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sp_xtp_control_proc_exec_stats
- sys.sp_xtp_control_proc_exec_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_xtp_control_proc_exec_stats
ms.assetid: f5119808-76a1-4522-8529-9e02ee39adcb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 055cf66665ebfe97196dda88cf0d297eb75f7f71
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200580"
---
# <a name="syssp_xtp_control_proc_exec_stats-transact-sql"></a>sys.sp_xtp_control_proc_exec_stats (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  Abilita la raccolta delle statistiche per le stored procedure compilate in modo nativo per l'istanza.  
  
 Per abilitare la raccolta delle statistiche a livello di query per le stored procedure compilate in modo nativo, vedere [sys.sp_xtp_control_query_exec_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-query-exec-stats-transact-sql.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
sp_xtp_control_proc_exec_stats [ [ @new_collection_value = ] collection_value ], [ @old_collection_value]  
```  
  
## <a name="arguments"></a>Argomenti  
 @new_collection_value = *valore*  
 Determina se la raccolta delle statistiche a livello di stored procedure è attivata (1) o disattivata (0).  
  
 @new_collection_value è impostato su zero quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o il database viene avviato.  
  
 @old_collection_value = *valore*  
 Restituisce lo stato corrente.  
  
## <a name="return-code"></a>Codice restituito  
 0 per l'esito positivo. Diverso da zero per l'esito negativo.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito sysadmin.  
  
## <a name="code-samples"></a>Esempi di codice  
 Per impostare @new_collection_value ed eseguire una query per il valore di @new_collection_value:  
  
```  
exec [sys].[sp_xtp_control_proc_exec_stats] @new_collection_value = 1  
declare @c bit  
exec sp_xtp_control_proc_exec_stats @old_collection_value=@c output  
select @c as 'collection status'  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [OLTP in memoria &#40;ottimizzazione per la memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
