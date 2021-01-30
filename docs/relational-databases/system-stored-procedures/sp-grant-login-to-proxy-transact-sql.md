---
description: sp_grant_login_to_proxy (Transact-SQL)
title: sp_grant_login_to_proxy (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_grant_login_to_proxy
- sp_grant_login_to_proxy_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_grant_login_to_proxy
ms.assetid: 90e1a6d5-a692-4462-a163-4b0709d83150
ms.author: vanto
author: VanMSFT
ms.openlocfilehash: c59f8b19e63f491757386ba5562ea9e0106e92a8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159810"
---
# <a name="sp_grant_login_to_proxy-transact-sql"></a>sp_grant_login_to_proxy (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Concede a un'entità di sicurezza l'accesso a un proxy.  

  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
sp_grant_login_to_proxy   
     { [ @login_name = ] 'login_name'   
     | [ @fixed_server_role = ] 'fixed_server_role'   
     | [ @msdb_role = ] 'msdb_role' } ,   
     { [ @proxy_id = ] id | [ @proxy_name = ] 'proxy_name' }  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @login_name = ] 'login_name'` Nome dell'account di accesso a cui concedere l'accesso. Il *login_name* è di **tipo nvarchar (256)** e il valore predefinito è null. È necessario specificare uno dei **\@ login_name**, **\@ fixed_server_role** o **\@ msdb_role** oppure il stored procedure ha esito negativo.  
  
`[ @fixed_server_role = ] 'fixed_server_role'` Ruolo predefinito del server al quale concedere l'accesso. Il *fixed_server_role* è di **tipo nvarchar (256)** e il valore predefinito è null. È necessario specificare uno dei **\@ login_name**, **\@ fixed_server_role** o **\@ msdb_role** oppure il stored procedure ha esito negativo.  
  
`[ @msdb_role = ] 'msdb_role'` Ruolo del database nel database **msdb** al quale concedere l'accesso. Il *msdb_role* è di **tipo nvarchar (256)** e il valore predefinito è null. È necessario specificare uno dei **\@ login_name**, **\@ fixed_server_role** o **\@ msdb_role** oppure il stored procedure ha esito negativo.  
  
`[ @proxy_id = ] id` Identificatore del proxy al quale concedere l'accesso. *ID* è di **tipo int** e il valore predefinito è null. È necessario specificare uno dei **\@ proxy_id** o **\@ proxy_name** oppure il stored procedure ha esito negativo.  
  
`[ @proxy_name = ] 'proxy_name'` Nome del proxy per il quale concedere l'accesso. Il *proxy_name* è di **tipo nvarchar (256)** e il valore predefinito è null. È necessario specificare uno dei **\@ proxy_id** o **\@ proxy_name** oppure il stored procedure ha esito negativo.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_grant_login_to_proxy** deve essere eseguito dal database **msdb** .  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_grant_login_to_proxy**.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene consentito all'account di accesso `adventure-works\terrid` di utilizzare il proxy `Catalog application proxy`.  
  
```sql
USE msdb ;  
GO  
  
EXEC dbo.sp_grant_login_to_proxy  
    @login_name = N'adventure-works\terrid',  
    @proxy_name = N'Catalog application proxy' ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [sp_add_proxy &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-proxy-transact-sql.md)   
 [sp_revoke_login_from_proxy &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-revoke-login-from-proxy-transact-sql.md)  
  
  
