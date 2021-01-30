---
description: sp_enum_sqlagent_subsystems (Transact-SQL)
title: sp_enum_sqlagent_subsystems (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_enum_sqlagent_subsystems
- sp_enum_sqlagent_subsystems_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_enum_sqlagent_subsystems
ms.assetid: 019a3c9d-bac3-495b-a70a-2c19f1d2e20e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3a65369cb7c4769d930acdaf655a006c60bdd55f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186952"
---
# <a name="sp_enum_sqlagent_subsystems-transact-sql"></a>sp_enum_sqlagent_subsystems (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Elenca i sottosistemi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_enum_sqlagent_subsystems  
```  
  
## <a name="arguments"></a>Argomenti  
 nessuno  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**sottosistema**|**nvarchar(40)**|Nome del sottosistema.|  
|**description**|**nvarchar(512)**|Descrizione del sottosistema.|  
|**subsystem_dll**|**nvarchar (510)**|Modulo DLL contenente il sottosistema.|  
|**agent_exe**|**nvarchar (510)**|Modulo eseguibile utilizzato dal sottosistema.|  
|**start_entry_point**|**nvarchar(30)**|Procedura richiamata da SQL Server Agent durante l'esecuzione del passaggio del processo.|  
|**event_entry_point**|**nvarchar(30)**|Procedura richiamata da SQL Server Agent durante l'esecuzione del passaggio del processo.|  
|**stop_entry_point**|**nvarchar(30)**|Procedura richiamata da SQL Server Agent durante l'esecuzione del passaggio del processo.|  
|**max_worker_threads**|**int**|Numero massimo di thread avviati da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent per questo sottosistema.|  
|**subsystem_id**|**int**|Identificatore del sottosistema.|  
  
## <a name="remarks"></a>Commenti  
 Questa procedura elenca i sottosistemi disponibili nell'istanza.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per impostazione predefinita, questa stored procedure può essere eseguita dai membri del ruolo predefinito del server **sysadmin** . Gli altri utenti devono appartenere al ruolo predefinito del database **SQLAgentOperatorRole** nel database **msdb** .  
  
 Per informazioni dettagliate su **SQLAgentOperatorRole**, vedere [SQL Server Agent ruoli](../../ssms/agent/sql-server-agent-fixed-database-roles.md)predefiniti del database.  
  
## <a name="see-also"></a>Vedere anche  
 [Implementare SQL Server Agent sicurezza](../../ssms/agent/implement-sql-server-agent-security.md)   
 [sp_add_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)  
  
  
