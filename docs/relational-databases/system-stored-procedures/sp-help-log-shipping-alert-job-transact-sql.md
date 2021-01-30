---
description: sp_help_log_shipping_alert_job (Transact-SQL)
title: sp_help_log_shipping_alert_job (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_log_shipping_alert_job_TSQL
- sp_help_log_shipping_alert_job
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_log_shipping_alert_job
ms.assetid: 4d4b4577-c393-4961-b2d3-b56e980b787b
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f0db0352d8499b9c2242bf9b7e460365a1a5cc70
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200090"
---
# <a name="sp_help_log_shipping_alert_job-transact-sql"></a>sp_help_log_shipping_alert_job (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Questa stored procedure restituisce l'ID del processo per la gestione degli avvisi dal server di monitoraggio log shipping.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_help_log_shipping_alert_job  
  
```  
  
## <a name="arguments"></a>Argomenti  
 nessuno  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 Questo stored procedure restituisce l' [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ID del processo di log shipping Alert. Se non esiste alcun processo per la gestione degli avvisi di log shipping, restituisce un set di risultati vuoto.  
  
## <a name="remarks"></a>Commenti  
 **sp_help_log_shipping_alert_job** deve essere eseguito dal database **Master** sul server di monitoraggio.  
  
## <a name="permissions"></a>Autorizzazioni  
 Questa procedura può essere eseguita solo dai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni sul log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
