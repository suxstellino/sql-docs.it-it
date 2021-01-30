---
description: sp_help_notification (Transact-SQL)
title: sp_help_notification (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_notification
- sp_help_notification_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_notification
ms.assetid: 0273457f-9d2a-4a6f-9a16-6a6bf281cba0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d37d091fdb380f0a08f3f0064f2ce408f439eee8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199986"
---
# <a name="sp_help_notification-transact-sql"></a>sp_help_notification (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce un elenco di avvisi per un determinato operatore o un elenco di operatori per un determinato avviso.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_help_notification  
     [ @object_type = ] 'object_type' ,  
     [ @name = ] 'name' ,  
     [ @enum_type = ] 'enum_type' ,   
     [ @notification_method = ] notification_method   
     [ , [ @target_name = ] 'target_name' ]   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @object_type = ] 'object_type'` Tipo di informazioni da restituire. *object_type* è **char (9)** e non prevede alcun valore predefinito. *object_type* possono essere avvisi, che elenca gli avvisi assegnati al nome dell'*operatore o agli operatori specificati, che* elenca gli operatori responsabili del nome di avviso specificato *.*  
  
`[ @name = ] 'name'` Nome di un operatore (se *object_type* è Operators) o nome di un avviso (se *object_type* è alerts). *Name* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @enum_type = ] 'enum_type'` Informazioni *object_type* restituite. nella maggior parte dei casi *enum_type* è effettivo. *enum_type* è di **carattere (10)** e non prevede alcun valore predefinito. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|ACTUAL|Elenca solo le *object_types* associate al *nome*.|  
|ALL|Elenca tutti i *object_types* inclusi quelli non associati al *nome*.|  
|TARGET|Elenca solo le *object_types* che corrispondono al *target_name* fornito, indipendentemente dall'associazione con il *nome*.|  
  
`[ @notification_method = ] notification_method` Valore numerico che determina le colonne del metodo di notifica da restituire. *notification_method* è di **tinyint**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1**|Posta elettronica: restituisce solo la colonna **use_email** .|  
|**2**|Cercapersone: restituisce solo la colonna **use_pager** .|  
|**4**|NetSend: restituisce solo la colonna **use_netsend** .|  
|**7**|Tutto: restituisce tutte le colonne.|  
  
`[ @target_name = ] 'target_name'` Nome dell'avviso da cercare (se *object_type* è alerts) o nome di un operatore da cercare (se *object_type* è Operators). *target_name* è necessario solo se *ENUM_TYPE* è la destinazione. *target_name* è di **tipo sysname** e il valore predefinito è null.  
  
## <a name="return-code-valves"></a>Valori restituiti  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 Se *object_type* è **Alerts**, il set di risultati elenca tutti gli avvisi per un operatore specificato.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**alert_id**|**int**|Numero di identificazione dell'avviso.|  
|**alert_name**|**sysname**|Nome dell'avviso.|  
|**use_email**|**int**|Specifica se il metodo di notifica utilizzato è la posta elettronica:<br /><br /> **1** = Sì<br /><br /> **0** = No|  
|**use_pager**|**int**|Specifica se il metodo di notifica utilizzato è il cercapersone:<br /><br /> **1** = Sì<br /><br /> **0** = No|  
|**use_netsend**|**int**|Specifica se il metodo di notifica utilizzato è NetSend:<br /><br /> **1** = Sì<br /><br /> **0** = No|  
|**has_email**|**int**|Numero di notifiche inviate tramite posta elettronica per l'avviso specificato.|  
|**has_pager**|**int**|Numero di notifiche inviate tramite cercapersone per l'avviso specificato.|  
|**has_netsend**|**int**|Numero di notifiche **net send** inviate per l'avviso.|  
  
 Se **object_type** è **Operators**, il set di risultati elenca tutti gli operatori per un determinato avviso.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**operator_id**|**int**|Numero di identificazione dell'operatore.|  
|**operator_name**|**sysname**|Nome dell'operatore.|  
|**use_email**|**int**|Specifica se il metodo di notifica utilizzato è la posta elettronica:<br /><br /> **1** = Sì<br /><br /> **0** = No|  
|**use_pager**|**int**|Specifica se il metodo di notifica utilizzato è il cercapersone:<br /><br /> **1** = Sì<br /><br /> **0** = No|  
|**use_netsend**|**int**|Specifica se il metodo di notifica utilizzato è NetSend:<br /><br /> **1** = Sì<br /><br /> **0** = No|  
|**has_email**|**int**|Specifica se all'operatore è associato un indirizzo di posta elettronica:<br /><br /> **1** = Sì<br /><br /> **0** = No|  
|**has_pager**|**int**|Specifica se all'operatore è associato un indirizzo cercapersone:<br /><br /> **1** = Sì<br /><br /> **0** = No|  
|**has_netsend**|**int**|Specifica se per l'operatore è stata specificata la notifica tramite Net Send.<br /><br /> **1** = Sì<br /><br /> **0** = No|  
  
## <a name="remarks"></a>Commenti  
 Questo stored procedure deve essere eseguito dal database **msdb** .  
  
## <a name="permissions"></a>Autorizzazioni  
 Per eseguire questa stored procedure, è necessario che gli utenti siano membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-listing-alerts-for-a-specific-operator"></a>R. Visualizzazione di un elenco di avvisi per un operatore specifico  
 Nell'esempio seguente vengono restituiti tutti gli avvisi per i quali `François Ajenstat` riceve una notifica.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_notification   
    @object_type = N'ALERTS',  
    @name = N'François Ajenstat',  
    @enum_type = N'ACTUAL',  
    @notification_method = 7 ;  
GO  
```  
  
### <a name="b-listing-operators-for-a-specific-alert"></a>B. Visualizzazione di un elenco di operatori per un avviso specifico  
 Nell'esempio seguente vengono restituiti tutti gli operatori che ricevono una notifica per l'avviso `Test Alert`.  
  
```  
USE msdb ;  
GO  
  
EXEC sp_help_notification  
    @object_type = N'OPERATORS',  
    @name = N'Test Alert',  
    @enum_type = N'ACTUAL',  
    @notification_method = 7 ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_add_notification &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-notification-transact-sql.md)   
 [sp_delete_notification &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-notification-transact-sql.md)   
 [sp_update_notification &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-notification-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
