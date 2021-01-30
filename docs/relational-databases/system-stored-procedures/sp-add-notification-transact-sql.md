---
description: sp_add_notification (Transact-SQL)
title: sp_add_notification (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_notification_TSQL
- sp_add_notification
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_notification
ms.assetid: 0525e0a2-ed0b-4e69-8a4c-a9e3e3622fbd
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f09a5638e55ee6bd8e0dfd6e07defc92b0ed365d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208003"
---
# <a name="sp_add_notification-transact-sql"></a>sp_add_notification (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Imposta una notifica per un avviso.  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_add_notification [ @alert_name = ] 'alert' ,   
    [ @operator_name = ] 'operator' ,   
    [ @notification_method = ] notification_method  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @alert_name = ] 'alert'` Avviso per la notifica. *Alert* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @operator_name = ] 'operator'` Operatore a cui inviare una notifica quando viene generato l'avviso. *operator* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @notification_method = ] notification_method` Metodo con cui l'operatore riceve una notifica. *notification_method* è di **tinyint** e non prevede alcun valore predefinito. *notification_method* possono essere uno o più di questi valori combinati con un operatore logico **or** .  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1**|Posta elettronica|  
|**2**|Cercapersone|  
|**4**|**net send**|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 **sp_add_notification** deve essere eseguito dal database **msdb** .  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] include un semplice strumento grafico per la gestione del sistema di avvisi. [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] è lo strumento consigliato per la configurazione di un'infrastruttura di avvisi.  
  
 Per inviare una notifica in risposta a un avviso, è innanzitutto necessario configurare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent per l'invio di messaggi.  
  
 Gli eventuali errori che si verificano durante l'invio di un messaggio di posta elettronica o di una notifica su cercapersone vengono registrati nel log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_add_notification**.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene aggiunta una notifica di posta elettronica per l'avviso specificato (`Test Alert`).  
  
> **Nota:** In questo esempio si presuppone che `Test Alert` esista già e che `François Ajenstat` sia un nome di operatore valido.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_notification  
 @alert_name = N'Test Alert',  
 @operator_name = N'François Ajenstat',  
 @notification_method = 1 ;  
GO  
```  
  
## <a name="see-also"></a>Vedi anche  
 [sp_delete_notification &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-notification-transact-sql.md)   
 [sp_help_notification &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-notification-transact-sql.md)   
 [sp_update_notification &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-notification-transact-sql.md)   
 [sp_add_operator &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-operator-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
