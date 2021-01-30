---
description: KILL STATS JOB (Transact-SQL)
title: KILL STATS JOB (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/27/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- KILL STATS JOB
- KILL_STATS_JOB_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ending statistics update jobs [SQL Server]
- stopping statistics update jobs
- terminating statistics update jobs
- asynchronous statistics updates [SQL Server]
- KILL STATS JOB statement
- statistics update jobs [SQL Server]
ms.assetid: 55a8f9f1-3259-45c0-8ab9-60b9c088b4b4
author: cawrites
ms.author: chadam
ms.openlocfilehash: d52e417bef5c998d381ee9720d626d39430d8fee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99113118"
---
# <a name="kill-stats-job-transact-sql"></a>KILL STATS JOB (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Interrompe un processo asincrono di aggiornamento delle statistiche in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
KILL STATS JOB job_id   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *job_id*  
 Campo job_id restituito dalla vista a gestione dinamica sys.dm_exec_background_job_queue per il processo.  
  
## <a name="remarks"></a>Osservazioni  
 job_id è indipendente da session_id o dall'unità di lavoro utilizzata nelle altre forme dell'istruzione KILL.  
  
## <a name="permissions"></a>Autorizzazioni  
 Gli utenti devono disporre dell'autorizzazione VIEW SERVER STATE per accedere alle informazioni della vista a gestione dinamica sys.dm_exec_background_job_queue.  
  
 Le autorizzazioni per l'istruzione KILL STATS JOB vengono assegnate per impostazione predefinita ai membri dei ruoli predefiniti del server sysadmin e processadmin e non sono trasferibili.  
  
## <a name="examples"></a>Esempi  
 L'esempio seguente mostra come interrompere l'aggiornamento delle statistiche associato a un processo in cui *job_id* = `53`.  
  
```sql  
KILL STATS JOB 53;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [KILL &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-transact-sql.md)   
 [KILL QUERY NOTIFICATION SUBSCRIPTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-query-notification-subscription-transact-sql.md)   
 [sys.dm_exec_background_job_queue &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-background-job-queue-transact-sql.md)   
 [Statistiche](../../relational-databases/statistics/statistics.md)  
  
  
