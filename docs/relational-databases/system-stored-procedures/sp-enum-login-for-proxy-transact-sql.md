---
description: sp_enum_login_for_proxy (Transact-SQL)
title: sp_enum_login_for_proxy (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_enum_login_for_proxy_TSQL
- sp_enum_login_for_proxy
dev_langs:
- TSQL
helpviewer_keywords:
- sp_enum_login_for_proxy
ms.assetid: 62a75019-248a-44c8-a5cc-c79f55ea3acf
ms.author: vanto
author: VanMSFT
ms.openlocfilehash: b51d02a66833df04a82a970319bab4e62f462a33
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186959"
---
# <a name="sp_enum_login_for_proxy-transact-sql"></a>sp_enum_login_for_proxy (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Visualizza un elenco di associazioni tra le entità di sicurezza e i proxy.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
sp_enum_login_for_proxy  
    [ @name = ] 'name'  
    [ @proxy_id = ] id,  
    [ @proxy_name = ] 'proxy_name'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @name = ] 'name'` Nome di un' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] entità, di un account di accesso, di un ruolo del server o di un ruolo del database **msdb** per cui elencare i proxy. Il nome è di **tipo nvarchar (256)** e il valore predefinito è null.  
  
`[ @proxy_id = ] id` Numero di identificazione del proxy per cui elencare le informazioni. Il *proxy_id* è di **tipo int** e il valore predefinito è null. È possibile specificare l' *ID* o il *proxy_name* .  
  
`[ @proxy_name = ] 'proxy_name'` Nome del proxy per cui elencare le informazioni. Il *proxy_name* è di **tipo sysname** e il valore predefinito è null. È possibile specificare l' *ID* o il *proxy_name* .  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**proxy_id**|**int**|Numero di identificazione del proxy.|  
|**proxy_name**|**sysname**|Nome del proxy.|  
|**nome**|**sysname**|Nome dell'entità di sicurezza per l'associazione.|  
|**flags**|**int**|Tipo dell'entità di sicurezza.<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] account di accesso<br /><br /> **1** = ruolo predefinito del sistema<br /><br /> **2** = ruolo del database in **msdb**|  
| &nbsp; | &nbsp; | &nbsp; |
  
## <a name="remarks"></a>Commenti  
 Se non vengono specificati parametri, **sp_enum_login_for_proxy** elenca le informazioni relative a tutti gli account di accesso nell'istanza di per ogni proxy.  
  
 Quando viene fornito un nome proxy o un ID proxy, **sp_enum_login_for_proxy** elenca gli account di accesso che hanno accesso al proxy. Quando viene fornito un nome di account di accesso, **sp_enum_login_for_proxy** elenca i proxy a cui l'account di accesso ha accesso.  
  
 Quando vengono specificate le informazioni sul proxy e un nome dell'account di accesso, il set di risultati restituisce una riga se l'account di accesso specificato può accedere al proxy specificato.  
  
 Questo stored procedure si trova nel **database msdb**.  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni di esecuzione per questa procedura vengono assegnate per impostazione predefinita ai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-listing-all-associations"></a>R. Visualizzazione di un elenco di tutte le associazioni  
 Nell'esempio seguente viene visualizzato un elenco di tutte le autorizzazioni stabilite tra gli account di accesso e i proxy nell'istanza corrente.  
  
```sql
USE msdb ;  
GO  
  
EXEC dbo.sp_enum_login_for_proxy ;  
GO  
```  
  
### <a name="b-listing-proxies-for-a-specific-login"></a>B. Visualizzazione di un elenco di proxy per un account di accesso specifico  
 Nell'esempio seguente viene visualizzato un elenco di proxy cui può accedere l'account `terrid`.  
  
```sql
USE msdb ;  
GO  
  
EXEC dbo.sp_enum_login_for_proxy  
    @name = 'terrid' ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_help_proxy &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-proxy-transact-sql.md)   
 [sp_grant_login_to_proxy &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-grant-login-to-proxy-transact-sql.md)   
 [sp_revoke_login_from_proxy &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-revoke-login-from-proxy-transact-sql.md)  
  
  
