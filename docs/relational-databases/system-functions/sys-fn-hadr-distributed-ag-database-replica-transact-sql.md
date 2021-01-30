---
description: sys.fn_hadr_distributed_ag_database_replica (Transact-SQL)
title: sys.fn_hadr_distributed_ag_database_replica (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/14/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fn_hadr_distributed_ag_database_replica
- sys.fn_hadr_distributed_ag_database_replica_TSQL
- fn_hadr_distributed_ag_database_replica
- fn_hadr_distributed_ag_database_replica_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fn_hadr_distributed_ag_database_replica
ms.assetid: 0e6202a1-e872-4f53-99d7-c16b6f712efc
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: e704af7bcdf5df2a27bcb3a9bd8ae37494f7f6de
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199245"
---
# <a name="sysfn_hadr_distributed_ag_database_replica-transact-sql"></a>sys.fn_hadr_distributed_ag_database_replica (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Utilizzato per eseguire il mapping di un database in un gruppo di disponibilità distribuito al database nel gruppo di disponibilità locale.  
   
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sys.fn_hadr_distributed_ag_database_replica( lag_Id, database_id )  
```  
  
## <a name="arguments"></a>Argomenti  
 '*lag_Id*'  
 Identificatore del gruppo di disponibilità distribuito. *lag_Id* è di tipo **uniqueidentifier**.  
  
 '*database_id*'  
 Identificatore del database in un gruppo di disponibilità distribuito. *database_id* è di tipo **uniqueidentifier**.  
  
## <a name="tables-returned"></a>Tabelle restituite  
 Restituisce le informazioni seguenti.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**group_database_id**|**uniqueidentifier**|ID del database nel gruppo di disponibilità locale.|  
  
## <a name="examples"></a>Esempio  
  
### <a name="using-sysfn_hadr_distributed_ag_database_replica"></a>Utilizzo di sys.fn_hadr_distributed_ag_database_replica  
 Nell'esempio seguente l'ID del database viene passato in un gruppo di disponibilità distribuito. Restituisce una tabella con l'ID del database associato al gruppo di disponibilità locale.  
  
```  
DECLARE @lagId uniqueidentifier = '4A03D1A8-4AE6-B153-E7E9-ED22A546008D'  
DECLARE @databaseId uniqueidentifier = '3FFA882A-C4C3-5B9E-A203-8F44BD9659F7'  
  
SELECT * FROM sys.fn_hadr_distributed_ag_database_replica(@lagId, @databaseId)  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni di gruppi di disponibilità Always On &#40;&#41;Transact-SQL ](../../relational-databases/system-functions/always-on-availability-groups-functions-transact-sql.md)   
 [Gruppi di disponibilità Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Gruppi di disponibilità distribuiti &#40;Always On gruppi di disponibilità&#41;](../../database-engine/availability-groups/windows/distributed-availability-groups.md)   
 [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-availability-group-transact-sql.md)   
 [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-availability-group-transact-sql.md)  
  
