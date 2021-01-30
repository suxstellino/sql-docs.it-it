---
description: sp_add_proxy (Transact-SQL)
title: sp_add_proxy (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_proxy
- sp_add_proxy_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE PROXY statement
- sp_add_proxy
ms.assetid: cb59df37-f103-439b-bec1-2871fb669a8b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c60c3bb1ea9f96352662b72080afbfa1e76b1dce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192402"
---
# <a name="sp_add_proxy-transact-sql"></a>sp_add_proxy (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Aggiunge il proxy [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent specificato.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_add_proxy  
    [ @proxy_name = ] 'proxy_name' ,  
    [ @enabled = ] is_enabled ,  
    [ @description = ] 'description' ,  
    [ @credential_name = ] 'credential_name' ,  
    [ @credential_id = ] credential_id ,  
    [ @proxy_id = ] id OUTPUT   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @proxy_name = ] 'proxy_name'` Nome del proxy da creare. Il *proxy_name* è di **tipo sysname** e il valore predefinito è null. Quando il *proxy_name* è null o una stringa vuota, il nome del proxy viene impostato automaticamente sul *user_name* fornito.  
  
`[ @enabled = ] is_enabled` Specifica se il proxy è abilitato. Il flag di *is_enabled* è di **tinyint** e il valore predefinito è 1. Quando *is_enabled* è **0**, il proxy non è abilitato e non può essere utilizzato da un passaggio di processo.  
  
`[ @description = ] 'description'` Descrizione del proxy. La descrizione è di **tipo nvarchar (512)** e il valore predefinito è null. La descrizione consente di documentare il proxy, ma non viene altrimenti utilizzata da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Questo argomento è pertanto facoltativo.  
  
`[ @credential_name = ] 'credential_name'` Nome delle credenziali per il proxy. Il *credential_name* è di **tipo sysname** e il valore predefinito è null. È necessario specificare *credential_name* o *credential_id* .  
  
`[ @credential_id = ] credential_id` Numero di identificazione della credenziale per il proxy. Il *credential_id* è di **tipo int** e il valore predefinito è null. È necessario specificare *credential_name* o *credential_id* .  
  
`[ @proxy_id = ] id OUTPUT` Numero di identificazione del proxy assegnato al proxy, se creato correttamente.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 Questa stored procedure deve essere eseguita nel database **msdb** .  
  
 Un proxy di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent gestisce la sicurezza per i passaggi di processo che implicano i sottosistemi diversi da [!INCLUDE[tsql](../../includes/tsql-md.md)]. Ogni proxy corrisponde a una credenziale di sicurezza Un proxy può avere accesso a qualsiasi numero di sottosistemi.  
  
## <a name="permissions"></a>Autorizzazioni  
 Questa procedura può essere eseguita solo dai membri del ruolo di sicurezza predefinito **sysadmin** .  
  
 I membri del ruolo di sicurezza predefinito **sysadmin** possono creare passaggi di processo che utilizzano qualsiasi proxy. Utilizzare il stored procedure [sp_grant_login_to_proxy &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grant-login-to-proxy-transact-sql.md) per concedere ad altri account di accesso al proxy.  
  
## <a name="examples"></a>Esempio  
 In questo esempio viene creato un proxy per le credenziali `CatalogApplicationCredential`. Nel codice si presuppone che le credenziali esistano già. Per ulteriori informazioni sulle credenziali, vedere [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-credential-transact-sql.md).  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_proxy  
    @proxy_name = 'Catalog application proxy',  
    @enabled = 1,  
    @description = 'Maintenance tasks on catalog application.',  
    @credential_name = 'CatalogApplicationCredential' ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-credential-transact-sql.md)   
 [sp_grant_login_to_proxy &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-grant-login-to-proxy-transact-sql.md)   
 [sp_revoke_login_from_proxy &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-revoke-login-from-proxy-transact-sql.md)  
  
  
