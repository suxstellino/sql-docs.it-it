---
description: sp_refresh_log_shipping_monitor (Transact-SQL)
title: sp_refresh_log_shipping_monitor (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_refresh_log_shipping_monitor
- sp_refresh_log_shipping_monitor_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_refresh_log_shipping_monitor
ms.assetid: edefb912-31c5-4d99-9aba-06629afd0171
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 16c1fb1de57fb0e1c61410071ec8fde31951f2ab
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185710"
---
# <a name="sp_refresh_log_shipping_monitor-transact-sql"></a>sp_refresh_log_shipping_monitor (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Questa stored procedure aggiorna le tabelle di monitoraggio remote in base alle informazioni più recenti recuperate da un server primario o secondario specificato per l'agente di log shipping specificato. La procedura viene richiamata dal server primario o secondario.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_refresh_log_shipping_monitor  
[ @agent_id = ] 'agent_id',  
[ @agent_type = ] 'agent_type'  
[ @database = ] 'database'  
[ @mode ] n  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @agent_id = ] 'agent_id'` ID primario per il backup o ID secondario per la copia o il ripristino. *agent_id* è di tipo **uniqueidentifier** e non può essere null.  
  
`[ @agent_type = ] 'agent_type'` Tipo di log shipping processo.  
  
 0 = backup.  
  
 1 = copia.  
  
 2 = ripristino.  
  
 *agent_type* è di **tinyint** e non può essere null.  
  
`[ @database = ] 'database'` Database primario o secondario usato dalla registrazione dagli agenti di backup o ripristino.  
  
`[ @mode ] n` Specifica se aggiornare o pulire i dati di monitoraggio. Il tipo di dati di *m* è tinyint e i valori supportati sono:  
  
 1 = aggiornamento (valore predefinito)  
  
 2 = elimina  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 No.  
  
## <a name="remarks"></a>Osservazioni  
 **sp_refresh_log_shipping_monitor** aggiorna le tabelle **log_shipping_monitor_primary**, **log_shipping_monitor_secondary**, **log_shipping_monitor_history_detail** e **log_shipping_monitor_error_detail** con le informazioni di sessione che non sono già state trasferite. Ciò consente di sincronizzare il server di monitoraggio con il server primario o secondario nel caso in cui il server di monitoraggio non sia più in sincronia. Consente inoltre di eliminare le informazioni di monitoraggio nel server di monitoraggio, se necessario.  
  
 **sp_refresh_log_shipping_monitor** deve essere eseguito dal database **Master** nel server primario o secondario.  
  
## <a name="permissions"></a>Autorizzazioni  
 Questa procedura può essere eseguita solo dai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni sul log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
