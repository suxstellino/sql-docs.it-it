---
description: xp_enumgroups (Transact-SQL)
title: xp_enumgroups (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- xp_enumgroups_TSQL
- xp_enumgroups
dev_langs:
- TSQL
helpviewer_keywords:
- xp_enumgroups
ms.assetid: 0bd3ed36-e260-469c-a5ff-b033fb9ea59d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a0319158b7e2627bac7cd67a8f15e10dc4694b51
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99124070"
---
# <a name="xp_enumgroups-transact-sql"></a>xp_enumgroups (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce un elenco di gruppi locali di Microsoft Windows o di gruppi globali definiti nel dominio Windows specificato.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
xp_enumgroups [ 'domain_name' ]  
```  
  
## <a name="arguments"></a>Argomenti  
 **'** *Domain_name* **'**  
 Nome del dominio Windows per cui si desidera ottenere un elenco di gruppi globali. *Domain_name* è di **tipo sysname** e il valore predefinito è null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**utenti**|**sysname**|Nome del gruppo di Windows.|  
|**Commento**|**sysname**|Descrizione del gruppo di Windows fornita da Windows.|  
  
## <a name="remarks"></a>Commenti  
 Se *Domain_name* è il nome del computer basato [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] su Windows su cui è in esecuzione un'istanza di o se non è specificato alcun nome di dominio, **xp_enumgroups** enumera i gruppi locali dal computer in cui è in esecuzione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Impossibile utilizzare **xp_enumgroups** quando un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in esecuzione in Windows 98.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del database **db_owner** nel database **Master** o l'appartenenza al ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono elencati i gruppi nel dominio `sales`.  
  
```  
EXEC xp_enumgroups 'sales';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_grantlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [sp_revokelogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-revokelogin-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Stored procedure estese generali &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql.md)   
 [xp_loginconfig &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/xp-loginconfig-transact-sql.md)   
 [xp_logininfo &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/xp-logininfo-transact-sql.md)  
  
  
