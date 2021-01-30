---
description: sp_update_proxy (Transact-SQL)
title: sp_update_proxy (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_update_proxy
- sp_update_proxy_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER PROXY statement
- sp_update_proxy
ms.assetid: 864fd0e6-9d61-4f07-92ef-145318d2f881
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f12d4666ed97033b19d7a0d4e39b17806e4d0953
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171720"
---
# <a name="sp_update_proxy-transact-sql"></a>sp_update_proxy (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifica le proprietà di un proxy già esistente.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_update_proxy   
    [ @proxy_id = ] id,  
    [ @proxy_name = ] 'proxy_name',  
    [ @credential_name = ] 'credential_name' ,  
    [ @credential_id = ] credential_id ,  
    [ @new_name = ] 'new_name' ,  
    [ @enabled = ] is_enabled ,  
    [ @description = ] 'description'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @proxy_id = ] id` Numero di identificazione del proxy da modificare. Il *proxy_id* è di **tipo int** e il valore predefinito è null.  
  
`[ @proxy_name = ] 'proxy_name'` Nome del proxy da modificare. Il *proxy_name* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @credential_name = ] 'credential_name'` Nome delle nuove credenziali per il proxy. Il *credential_name* è di **tipo sysname** e il valore predefinito è null. È possibile specificare *credential_name* o *credential_id* .  
  
`[ @credential_id = ] credential_id` Numero di identificazione delle nuove credenziali per il proxy. Il *credential_id* è di **tipo int** e il valore predefinito è null. È possibile specificare *credential_name* o *credential_id* .  
  
`[ @new_name = ] 'new_name'` Nuovo nome del proxy. Il *new_name* è di **tipo sysname** e il valore predefinito è null. Quando viene specificato, la procedura modifica il nome del proxy in *new_name*. Quando questo argomento è NULL, il nome del proxy rimane invariato.  
  
`[ @enabled = ] is_enabled` Indica se il proxy è abilitato. Il flag di *is_enabled* è di **tinyint** e il valore predefinito è null. Quando *is_enabled* è **0**, il proxy non è abilitato e non può essere utilizzato da un passaggio di processo. Quando questo argomento è NULL, lo stato del proxy rimane invariato.  
  
`[ @description = ] 'description'` Nuova descrizione del proxy. La *Descrizione* è di **tipo nvarchar (512)** e il valore predefinito è null. Quando questo argomento è NULL, la descrizione del proxy rimane invariata.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 È necessario specificare **\@ proxy_name** o **\@ proxy_id** . Se si specificano entrambi gli argomenti, devono riferirsi tutti e due allo stesso proxy per consentire la corretta esecuzione della stored procedure.  
  
 È necessario specificare **\@ credential_name** o **\@ credential_id** per modificare le credenziali per il proxy. Se si specificano entrambi gli argomenti, devono riferirsi alle stesse credenziali per consentire la corretta esecuzione della stored procedure.  
  
 Questa procedura modifica il proxy, ma non modifica l'accesso al proxy. Per modificare l'accesso a un proxy, usare **sp_grant_login_to_proxy** e **sp_revoke_login_from_proxy**.  
  
## <a name="permissions"></a>Autorizzazioni  
 Questa procedura può essere eseguita solo dai membri del ruolo di sicurezza predefinito **sysadmin** .  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene impostato il valore abilitato per il proxy `Catalog application proxy` su `0`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_update_proxy  
    @proxy_name = 'Catalog application proxy',  
    @enabled = 0;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di SQL Server Agent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [Implementare SQL Server Agent sicurezza](../../ssms/agent/implement-sql-server-agent-security.md)   
 [sp_add_proxy &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-proxy-transact-sql.md)   
 [sp_delete_proxy &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-proxy-transact-sql.md)   
 [sp_grant_login_to_proxy &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-grant-login-to-proxy-transact-sql.md)   
 [sp_revoke_login_from_proxy &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-revoke-login-from-proxy-transact-sql.md)  
  
  
