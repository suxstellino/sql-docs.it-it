---
description: sys.sp_xtp_control_query_exec_stats (Transact-SQL)
title: sys.sp_xtp_control_query_exec_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/13/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sp_xtp_control_query_exec_stats_TSQL
- sys.sp_xtp_control_query_exec_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_xtp_control_query_exec_stats
ms.assetid: 4838125d-ad1e-479e-b7d2-42655e8f4f02
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a1047918fd6d337d863dbf43d7058d1e3571e1cb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180211"
---
# <a name="syssp_xtp_control_query_exec_stats-transact-sql"></a>sys.sp_xtp_control_query_exec_stats (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  Abilita la raccolta per statistiche di query di tutte le stored procedure compilate in modo nativo per l'istanza o stored procedure specifiche compilate in modo nativo.  
  
 Le prestazioni diminuiscono quando si abilita la raccolta delle statistiche. Se è necessario risolvere il problema di una o poche stored procedure compilate in modo nativo, è possibile abilitare la raccolta delle statistiche solo per tali stored procedure compilate in modo nativo.  
  
 Per abilitare la raccolta delle statistiche a livello di routine per tutte le stored procedure compilate in modo nativo, vedere [sys.sp_xtp_control_proc_exec_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-proc-exec-stats-transact-sql.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
sp_xtp_control_query_exec_stats [ [ @new_collection_value = ] collection_value ],  
[ [ @database_id = ] database_id   
[ , [ @xtp_object_id = ] procedure_id ] ,   
[ @old_collection_value] ]  
```  
  
## <a name="arguments"></a>Argomenti  
 @new_collection_value = *valore*  
 Determina se la raccolta delle statistiche a livello di stored procedure è attivata (1) o disattivata (0).  
  
 @new_collection_value viene impostato su zero quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene avviato.  
  
 @database_id = = *database_id*, @xtp_object_id = *procedure_id*  
 L'ID database e l'ID oggetto per la stored procedure compilata in modo nativo. Se la raccolta delle statistiche è abilitata per l'istanza ([sys.sp_xtp_control_proc_exec_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-proc-exec-stats-transact-sql.md)), vengono raccolte le statistiche su una stored procedure compilata in modo nativo. La disabilitazione della raccolta delle statistiche dell'istanza non disabilita la raccolta delle statistiche delle singole stored procedure compilate in modo nativo.  
  
 Utilizzare [sys. databases &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md), [sys. Procedures &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-procedures-transact-sql.md), DB_ID &#40;[Transact-SQL ](../../t-sql/functions/db-id-transact-sql.md)&#41;oppure OBJECT_ID &#40;[Transact-SQL ](../../t-sql/functions/object-id-transact-sql.md) per ottenere gli ID per un database e&#41;.  
  
 @old_collection_value = *valore*  
 Restituisce lo stato corrente.  
  
## <a name="return-code"></a>Codice restituito  
 0 per l'esito positivo. Diverso da zero per l'esito negativo.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito sysadmin.  
  
## <a name="code-sample"></a>Codice di esempio  
 Il seguente esempio di codice indica come abilitare la raccolta per statistiche di tutte le stored procedure compilate in modo nativo per l'istanza e per una stored procedure specifica compilata in modo nativo.  
  
```sql   
DECLARE @c bit  
  
EXEC [sys].[sp_xtp_control_query_exec_stats] @new_collection_value = 1;  
  
EXEC sp_xtp_control_query_exec_stats @old_collection_value=@c output;  
SELECT @c AS 'collection status';  
  
EXEC [sys].[sp_xtp_control_query_exec_stats] @new_collection_value = 1,   
@database_id = 5, @xtp_object_id = 341576255;  
  
EXEC sp_xtp_control_query_exec_stats @database_id = 5,   
@xtp_object_id = 341576255, @old_collection_value=@c output;  
  
SELECT @c AS 'collection status';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [OLTP in memoria &#40;ottimizzazione per la memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
