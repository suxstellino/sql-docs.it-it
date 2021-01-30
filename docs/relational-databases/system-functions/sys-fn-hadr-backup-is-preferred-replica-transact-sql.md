---
description: sys.fn_hadr_backup_is_preferred_replica (Transact-SQL)
title: sys.fn_hadr_backup_is_preferred_replica (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fn_hadr_backup_is_preferred_replica_TSQL
- sys.fn_hadr_backup_is_preferred_replica
- fn_hadr_backup_is_preferred_replica_TSQL
- fn_hadr_backup_is_preferred_replica
dev_langs:
- TSQL
helpviewer_keywords:
- backup on secondary replicas
- active secondary replicas [SQL Server], backup on secondary replicas
- sys.fn_hadr_backup_is_preferred_replica function
ms.assetid: 61b9be77-e2f6-4da1-b2ae-a62cbe226145
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 009e237b8d7ac7d100100969cf9f8a75bbad1195
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206035"
---
# <a name="sysfn_hadr_backup_is_preferred_replica--transact-sql"></a>sys.fn_hadr_backup_is_preferred_replica (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Utilizzato per determinare se la replica corrente è la replica di backup preferita.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sys.fn_hadr_backup_is_preferred_replica ( 'dbname' )  
```  
  
## <a name="arguments"></a>Argomenti  
 '*dbname*'  
 Nome del database di cui eseguire il backup. *dbname* è di tipo sysname.  
  
## <a name="returns"></a>Restituisce  
 Restituisce il tipo di dati **bool**: 1 se il database nell'istanza corrente è nella replica preferita; in caso contrario, 0.  
  
## <a name="remarks"></a>Commenti  
 Utilizzare questa funzione in uno script di backup per determinare se il database corrente si trova nella replica preferita per i backup. È possibile eseguire uno script in ogni replica di disponibilità. Ognuno di questi processi analizza gli stessi dati per determinare il processo da eseguire in modo tale che solo uno dei processi pianificati procede effettivamente alla fase di backup. Il codice di esempio avrà un aspetto analogo al seguente:  
  
```  
If sys.fn_hadr_backup_is_preferred_replica( @dbname ) <> 1   
BEGIN  
-- If this is not the preferred replica, exit (probably without error).  
END  
-- If this is the preferred replica, continue to do the backup.  
  
```  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-sysfn_hadr_backup_is_preferred_replica"></a>R. Utilizzo di sys.fn_hadr_backup_is_preferred_replica  
 Nell'esempio seguente viene restituito 1 se il database corrente è la replica di backup preferita.  
  
```  
SELECT sys.fn_hadr_backup_is_preferred_replica ('TestDB');  
GO  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Configurare il backup su repliche di disponibilità &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni di gruppi di disponibilità Always On &#40;&#41;Transact-SQL ](../../relational-databases/system-functions/always-on-availability-groups-functions-transact-sql.md)   
 [Gruppi di disponibilità Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-availability-group-transact-sql.md)   
 [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-availability-group-transact-sql.md)   
 Repliche secondarie [attive: backup in repliche secondarie &#40;always on gruppi di disponibilità&#41;](../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md) [Always on viste del catalogo dei gruppi di disponibilità &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)      
  
  
