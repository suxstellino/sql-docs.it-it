---
description: sp_manage_jobs_by_login (Transact-SQL)
title: sp_manage_jobs_by_login (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_manage_jobs_by_login
- sp_manage_jobs_by_login_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_manage_jobs_by_login
ms.assetid: 832ec15a-6e92-4eb5-8c4a-af4dba79fbaa
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5f367f869092bde5458c732fda3e79bc06e43cb8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185352"
---
# <a name="sp_manage_jobs_by_login-transact-sql"></a>sp_manage_jobs_by_login (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Elimina o riassegna i processi che appartengono all'account di accesso specificato.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_manage_jobs_by_login  
     [ @action = ] 'action'  
     [, [@current_owner_login_name = ] 'current_owner_login_name']  
     [, [@new_owner_login_name = ] 'new_owner_login_name']  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @action = ] 'action'` Azione da eseguire per l'account di accesso specificato. *Action* è di tipo **varchar (10)** e non prevede alcun valore predefinito. Quando *Action* è **Delete**, **sp_manage_jobs_by_login** Elimina tutti i processi di proprietà di *current_owner_login_name*. Quando *Action* viene **riassegnata**, tutti i processi vengono assegnati a *new_owner_login_name*.  
  
`[ @current_owner_login_name = ] 'current_owner_login_name'` Nome dell'account di accesso del proprietario del processo corrente. *current_owner_login_name* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @new_owner_login_name = ] 'new_owner_login_name'` Nome dell'account di accesso del nuovo proprietario del processo. Utilizzare questo parametro solo se l' *azione* viene **riassegnata**. *new_owner_login_name* è di **tipo sysname** e il valore predefinito è null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="permissions"></a>Autorizzazioni  
 Per eseguire questa stored procedure, è necessario che agli utenti venga concesso il ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempio  
 In questo esempio tutti i processi di `danw` vengono riassegnati a `françoisa`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_manage_jobs_by_login  
    @action = N'REASSIGN',  
    @current_owner_login_name = N'danw',  
    @new_owner_login_name = N'françoisa' ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_delete_job &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-job-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
