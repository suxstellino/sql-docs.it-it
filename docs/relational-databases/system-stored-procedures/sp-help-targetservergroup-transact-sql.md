---
description: sp_help_targetservergroup (Transact-SQL)
title: sp_help_targetservergroup (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_targetservergroup_TSQL
- sp_help_targetservergroup
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_targetservergroup
ms.assetid: ec3a4a68-b591-431c-9518-053ede522d0c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 200b6b0a8d6a6700f7aa98db0cde45cfc278dc53
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186708"
---
# <a name="sp_help_targetservergroup-transact-sql"></a>sp_help_targetservergroup (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Visualizza un elenco di tutti i server di destinazione del gruppo specificato. Se non viene specificato alcun gruppo, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce informazioni su tutti i gruppi di server di destinazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_help_targetservergroup  
    [ [ @name = ] 'name' ]  
```  
  
## <a name="argument"></a>Argomento  
`[ @name = ] 'name'` Nome del gruppo di server di destinazione per il quale si desidera ottenere informazioni. *Name* è di **tipo sysname** e il valore predefinito è null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**servergroup_id**|**int**|Numero di identificazione del gruppo di server.|  
|**nome**|**sysname**|Nome del gruppo di server.|  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni per eseguire questa procedura vengono assegnate per impostazione predefinita al ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-listing-information-for-all-target-server-groups"></a>R. Visualizzazione dell'elenco delle informazioni per tutti i gruppi di server di destinazione  
 Nell'esempio seguente vengono visualizzate le informazioni per tutti i gruppi di server di destinazione.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_targetservergroup ;  
GO  
```  
  
### <a name="b-listing-information-for-a-specific-target-server-group"></a>B. Visualizzazione dell'elenco delle informazioni per un gruppo di server di destinazione specifico  
 Nell'esempio seguente vengono visualizzate le informazioni per il gruppo di server di destinazione `Servers Maintaining Customer Information`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_targetservergroup   
    N'Servers Maintaining Customer Information' ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_add_targetservergroup &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-targetservergroup-transact-sql.md)   
 [sp_delete_targetservergroup &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-targetservergroup-transact-sql.md)   
 [sp_update_targetservergroup &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-targetservergroup-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
