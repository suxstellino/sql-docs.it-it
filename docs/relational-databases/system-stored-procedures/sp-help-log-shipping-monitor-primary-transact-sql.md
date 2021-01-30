---
description: sp_help_log_shipping_monitor_primary (Transact-SQL)
title: sp_help_log_shipping_monitor_primary (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_log_shipping_monitor_primary
- sp_help_log_shipping_monitor_primary_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_log_shipping_monitor_primary
ms.assetid: d9dfcb8f-1da6-49ca-a2c8-411574915434
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 732db5c1427526a62b8103444c0ca7063442dae6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200073"
---
# <a name="sp_help_log_shipping_monitor_primary-transact-sql"></a>sp_help_log_shipping_monitor_primary (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce le informazioni relative a un database primario dalle tabelle di monitoraggio.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_help_log_shipping_monitor_primary  
[ @primary_server = ] 'primary_server',   
[ @primary_database = ] 'primary_database'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @primary_server = ] 'primary_server'` Nome dell'istanza primaria di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] nella configurazione log shipping. *primary_server* è di **tipo sysname** e non può essere null.  
  
`[ @primary_database = ] 'primary_database'` Nome del database nel server primario. *primary_database* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Descrizione|  
|-----------------|-----------------|  
|**primary_id**|ID del database primario nella configurazione per il log shipping.|  
|**primary_server**|Nome dell'istanza primaria di [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] nella configurazione per il log shipping.|  
|**primary_database**|Nome del database primario nella configurazione di log shipping.|  
|**backup_threshold**|Numero di minuti consentiti tra le operazioni di backup prima che venga generato un avviso.|  
|**threshold_alert**|Avviso da generare quando viene superata la soglia per il backup.|  
|**threshold_alert_enabled**|Determina se sono abilitati gli avvisi relativi alla soglia per il backup. 1 = abilitato; 0 = disabilitato.|  
|**last_backup_file**|Percorso assoluto del backup del log delle transazioni più recente.|  
|**last_backup_date**|Data e ora dell'ultima operazione di backup del log delle transazioni nel database primario.|  
|**last_backup_date_utc**|Data e ora dell'ultima operazione di backup del log delle transazioni nel database primario in base all'ora UTC (Coordinated Universal Time).|  
|**history_retention_period**|Intervallo di tempo, espresso in minuti, di conservazione dei record relativi alla cronologia di log shipping per un database primario specifico trascorso il quale i record vengono eliminati.|  
  
## <a name="remarks"></a>Commenti  
 **sp_help_log_shipping_monitor_primary** deve essere eseguito dal database **Master** sul server di monitoraggio.  
  
## <a name="permissions"></a>Autorizzazioni  
 Questa procedura può essere eseguita solo dai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni sul log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
