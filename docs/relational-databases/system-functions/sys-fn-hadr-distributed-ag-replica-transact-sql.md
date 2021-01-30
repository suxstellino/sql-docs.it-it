---
description: sys.fn_hadr_distributed_ag_replica (Transact-SQL)
title: sys.fn_hadr_distributed_ag_replica (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fn_hadr_distributed_ag_replica
- sys.fn_hadr_distributed_ag_replica_TSQL
- fn_hadr_distributed_ag_replica
- fn_hadr_distributed_ag_replica_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fn_hadr_distributed_ag_replica
ms.assetid: a1e5f9cb-c350-4bb4-a04f-7394f6f25d62
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 8d0b1e6df3a2286cfe60dbaf89358683766dc4c3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198734"
---
# <a name="sysfn_hadr_distributed_ag_replica-transact-sql"></a>sys.fn_hadr_distributed_ag_replica (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Utilizzato per eseguire il mapping di una replica in un gruppo di disponibilità distribuito al gruppo di disponibilità locale.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sys.fn_hadr_distributed_ag_replica( lag_Id, replica_id )  
```  
  
## <a name="arguments"></a>Argomenti  
 '*lag_Id*'  
 Identificatore del gruppo di disponibilità distribuito. *lag_Id* è di tipo **uniqueidentifier**.  
  
 '*replica_id*'  
 Identificatore di una replica nel gruppo di disponibilità distribuito. *replica_id* è di tipo **uniqueidentifier**.  
  
## <a name="tables-returned"></a>Tabelle restituite  
 Restituisce le informazioni seguenti.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**group_id**|**uniqueidentifier**|Identificatore univoco (GUID) del gruppo di disponibilità locale.|  
  
## <a name="examples"></a>Esempio  
  
### <a name="using-sysfn_hadr_distributed_ag_replica"></a>Utilizzo di sys.fn_hadr_distributed_ag_replica  
 Nell'esempio seguente viene restituita una tabella con l'identificatore del gruppo di disponibilità locale associato al gruppo di disponibilità e alla replica specificati.  
  
```  
DECLARE @lagId uniqueidentifier = '4A03D1A8-4AE6-B153-E7E9-ED22A546008D'  
DECLARE @replicaId uniqueidentifier = 'D5517513-04A8-FD82-14C6-E684EC913935'  
  
SELECT * FROM sys.fn_hadr_distributed_ag_replica(@lagId, @replicaId)  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni Gruppi di disponibilità AlwaysOn &#40;&#41;Transact-SQL ](../../relational-databases/system-functions/always-on-availability-groups-functions-transact-sql.md)   
 [Gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Gruppi di disponibilità distribuiti &#40;Gruppi di disponibilità AlwaysOn&#41;](../../database-engine/availability-groups/windows/distributed-availability-groups.md)  
 [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-availability-group-transact-sql.md)   
 [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-availability-group-transact-sql.md)  
  
