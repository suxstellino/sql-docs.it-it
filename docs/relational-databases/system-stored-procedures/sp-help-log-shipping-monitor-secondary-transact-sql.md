---
description: sp_help_log_shipping_monitor_secondary (Transact-SQL)
title: sp_help_log_shipping_monitor_secondary (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_log_shipping_monitor_secondary
- sp_help_log_shipping_monitor_secondary_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_log_shipping_monitor_secondary
ms.assetid: 3ac091ea-c9a8-4c05-a0b6-1ccf4e001339
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 7a16b96c6edd21e2af3a78b5fa8f91e2c6b98880
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200054"
---
# <a name="sp_help_log_shipping_monitor_secondary-transact-sql"></a>sp_help_log_shipping_monitor_secondary (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce le informazioni relative a un database secondario dalle tabelle di monitoraggio.  
  
 
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_help_log_shipping_monitor_secondary  
[ @secondary_server = ] 'secondary_server',  
[ @secondary_database = ] 'secondary_database'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @secondary_server = ] 'secondary_server'` Nome del server secondario. *secondary_server* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @secondary_database = ] 'secondary_database'` Nome del database secondario. *secondary_database* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Colonna|Descrizione|  
|------------|-----------------|  
|**secondary_server**|Nome dell'istanza secondaria di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] nella configurazione log shipping.|  
|**secondary_database**|Nome del database secondario nella configurazione di log shipping.|  
|**secondary_id**|ID del server secondario nella configurazione per il log shipping.|  
|**primary_server**|Nome dell'istanza primaria di [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] nella configurazione per il log shipping.|  
|**primary_database**|Nome del database primario nella configurazione di log shipping.|  
|**restore_threshold**|Numero di minuti che può trascorrere tra operazioni di ripristino prima che venga generato un avviso.|  
|**threshold_alert**|Avviso da generare quando viene superata la soglia di ripristino.|  
|**threshold_alert_enabled**|Determina se sono abilitati gli avvisi di soglia di ripristino.<br /><br /> 1 = abilitati.<br /><br /> 0 = disabilitati.|  
|**last_copied_file**|Nome dell'ultimo file di backup copiato nel server secondario.|  
|**last_copied_date**|Data e ora dell'ultima operazione di copia nel server secondario.|  
|**last_copied_date_utc**|Data e ora UTC (Coordinated Universal Time o ora di Greenwich) dell'ultima operazione di copia nel server secondario.|  
|**last_restored_file**|Nome dell'ultimo file di backup ripristinato nel database secondario.|  
|**last_restored_date**|Data e ora dell'ultima operazione di ripristino nel database secondario.|  
|**last_restored_date_utc**|Data e ora UTC (Coordinated Universal Time o ora di Greenwich) dell'ultima operazione di ripristino nel database secondario.|  
|**history_retention_period**|Periodo di tempo, in minuti, durante il quale i record della cronologia di log shipping vengono mantenuti per un database secondario specificato prima di essere eliminati.|  
  
## <a name="remarks"></a>Commenti  
 **sp_help_log_shipping_monitor_secondary** deve essere eseguito dal database **Master** sul server di monitoraggio.  
  
## <a name="permissions"></a>Autorizzazioni  
 Questa procedura può essere eseguita solo dai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="see-also"></a>Vedi anche  
 [Informazioni sul log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
