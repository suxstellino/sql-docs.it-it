---
description: xp_loginconfig (Transact-SQL)
title: xp_loginconfig (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- xp_loginconfig_TSQL
- xp_loginconfig
dev_langs:
- TSQL
helpviewer_keywords:
- xp_loginconfig
ms.assetid: d380e799-2857-408a-bcbf-5e73a8e6aa5a
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: da53f9c3666a4d4d1e75136dd81caa45c9aaee81
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99124417"
---
# <a name="xp_loginconfig-transact-sql"></a>xp_loginconfig (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce la configurazione di sicurezza dell'account di accesso di un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
xp_loginconfig ['config_name']  
```  
  
## <a name="arguments"></a>Argomenti  
 **'** *config_name* **'**  
 Valore di configurazione da visualizzare. Se *config_name* viene omesso, vengono restituiti tutti i valori di configurazione. *config_name* è di **tipo sysname** e il valore predefinito è null. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**login mode**|Modalità di sicurezza dell'account di accesso. I valori possibili sono **mixed** e **l'autenticazione di Windows**.<br /><br /> Sostituito da:<br /><br /> `SELECT SERVERPROPERTY('IsIntegratedSecurityOnly'); GO`|  
|**default login**|Nome dell'ID di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] predefinito per gli utenti autorizzati su connessioni trusted (ovvero gli utenti che non dispongono di un nome di account di accesso corrispondente). L'account di accesso predefinito è **Guest**. Questo valore è disponibile per compatibilità con le versioni precedenti.|  
|**Dominio predefinito**|Nome del dominio di Windows predefinito per gli utenti di rete su connessioni trusted. Il dominio predefinito è il dominio del computer in cui sono in esecuzione Windows e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questo valore è disponibile per compatibilità con le versioni precedenti.|  
|**livello di controllo**|Livello di controllo. I valori possibili sono **None**, **Success**, **Failure** e **All**. I controlli vengono scritti nel log degli errori e nel Visualizzatore eventi di Windows.|  
|**set hostname**|Indica se il nome dell'host proveniente dal record dell'account di accesso del client viene sostituito con il nome utente di rete di Windows. I valori possibili sono **true** o **false**. Se questa opzione è impostata, il nome utente di rete verrà visualizzato nell'output del **sp_who**.|  
|**Mappa**|Restituisce i caratteri speciali di Windows sui quali viene eseguito il mapping al carattere di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valido _ (sottolineatura). I valori possibili sono il **separatore di dominio** (impostazione predefinita), **lo spazio**, un **valore null** o un carattere singolo. Questo valore è disponibile per compatibilità con le versioni precedenti.|  
|**Mappa $**|Restituisce i caratteri speciali di Windows sui quali viene eseguito il mapping al carattere di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valido $ (segno di dollaro). I valori possibili sono **separatore di dominio**, **spazio**, **null** o qualsiasi carattere singolo. Il valore predefinito è **spazio**. Questo valore è disponibile per compatibilità con le versioni precedenti.|  
|**map #**|Restituisce i caratteri speciali di Windows sui quali viene eseguito il mapping al carattere di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valido # (simbolo di cancelletto). I valori possibili sono **separatore di dominio**, **spazio**, **null** o qualsiasi carattere singolo. Il valore predefinito è il segno meno. Questo valore è disponibile per compatibilità con le versioni precedenti.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**nome**|**sysname**|Valore di configurazione|  
|**config value**|**sysname**|Impostazione del valore di configurazione|  
  
## <a name="remarks"></a>Commenti  
 non è possibile usare **xp_loginconfig** per impostare i valori di configurazione.  
  
 Per impostare la modalità di accesso e il livello di controllo, utilizzare [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CONTROL per il database **Master** .  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-how-to-report-all-configuration-values"></a>R. Visualizzazione di tutti i valori di configurazione  
 Nell'esempio seguente vengono visualizzate tutte le impostazioni correnti.  
  
```  
EXEC xp_loginconfig;  
GO  
```  
  
### <a name="b-how-to-report-a-specific-configuration-value"></a>B. Modalità di segnalazione di un valore di configurazione specifico  
 Nell'esempio seguente viene visualizzata l'impostazione relativa alla modalità di accesso.  
  
```  
EXEC xp_loginconfig 'login mode';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_denylogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-denylogin-transact-sql.md)   
 [sp_grantlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sp_revokelogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-revokelogin-transact-sql.md)   
 [xp_logininfo &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/xp-logininfo-transact-sql.md)  
  
  
