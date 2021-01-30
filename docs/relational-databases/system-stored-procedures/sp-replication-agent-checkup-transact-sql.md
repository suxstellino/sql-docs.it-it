---
description: sp_replication_agent_checkup (Transact-SQL)
title: sp_replication_agent_checkup (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replication_agent_checkup_TSQL
- sp_replication_agent_checkup
helpviewer_keywords:
- sp_replication_agent_checkup
ms.assetid: 50357c2e-71aa-4e13-9e2e-0977a3655cc9
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 77dd179aa4024f969d8e27a3d57cdc03374a4497
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204401"
---
# <a name="sp_replication_agent_checkup-transact-sql"></a>sp_replication_agent_checkup (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Controlla tutti i database di distribuzione per identificare gli agenti di replica in esecuzione che non hanno eseguito alcuna registrazione della cronologia entro l'intervallo di attività specificato. Questa stored procedure viene eseguita in qualsiasi database del server di distribuzione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_replication_agent_checkup [ [ @heartbeat_interval = ] heartbeat_interval ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @heartbeat_interval = ] 'heartbeat_interval'` Numero massimo di minuti per cui un agente può passare senza registrare un messaggio di stato. *heartbeat_interval* è di **tipo int** e il valore predefinito è 10 minuti.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **sp_replication_agent_checkup** genera l'errore 14151 per ogni agente rilevato come sospetto. Registra inoltre nella cronologia un messaggio di errore relativo agli agenti.  
  
## <a name="remarks"></a>Commenti  
 **sp_replication_agent_checkup** viene utilizzata per la replica snapshot, la replica transazionale e la replica di tipo merge.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_replication_agent_checkup**.  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
