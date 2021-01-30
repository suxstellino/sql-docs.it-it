---
description: sp_delete_notification (Transact-SQL)
title: sp_delete_notification (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_notification_TSQL
- sp_delete_notification
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_notification
ms.assetid: b55d3898-596d-47a5-a4f0-d65dc736223b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 85ce389d41eb57a38507d09be5c9ebdd6814ac51
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181315"
---
# <a name="sp_delete_notification-transact-sql"></a>sp_delete_notification (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Rimuove una definizione di notifica di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent per un avviso e un operatore specifici.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_delete_notification  
     [ @alert_name = ] 'alert' ,   
     [ @operator_name = ] 'operator'   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @alert_name = ] 'alert'` Nome dell'avviso. *Alert* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @operator_name = ] 'operator'` Nome dell'operatore. *operator* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 Quando si rimuove una notifica, l'avviso e l'operatore non vengono modificati.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per eseguire questa stored procedure, è necessario che agli utenti venga concesso il ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene rimossa la notifica inviata all'operatore `François Ajenstat` quando viene restituito l'avviso `Test Alert`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_notification  
    @alert_name = 'Test Alert',  
    @operator_name = 'François Ajenstat' ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_add_alert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-alert-transact-sql.md)   
 [sp_add_notification &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-notification-transact-sql.md)   
 [sp_add_operator &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-operator-transact-sql.md)   
 [sp_delete_alert &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-alert-transact-sql.md)   
 [sp_help_alert &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-alert-transact-sql.md)   
 [sp_help_notification &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-notification-transact-sql.md)   
 [sp_help_operator &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-operator-transact-sql.md)   
 [sp_update_notification &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-notification-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
