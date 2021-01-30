---
description: sp_syspolicy_configure (Transact-SQL)
title: sp_syspolicy_configure (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syspolicy_configure
- sp_syspolicy_configure_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syspolicy_configure
ms.assetid: 70c10922-9345-4190-ba69-808a43f760da
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: abf343b9230e8eeef0ce95ca8e2667c20c714cb8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201243"
---
# <a name="sp_syspolicy_configure-transact-sql"></a>sp_syspolicy_configure (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Configura le impostazioni della gestione basata su criteri, ad esempio se la stessa è abilitata.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_syspolicy_configure [ @name = ] 'name'  
    , [ @value = ] value  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @name = ] 'name'` Nome dell'impostazione che si desidera configurare. *Name* è di **tipo sysname**, è obbligatorio e non può essere null o una stringa vuota.  
  
 il *nome* può essere uno dei valori seguenti:  
  
-   'Enabled' - Determina se la gestione basata su criteri è abilitata.  
  
-   'HistoryRetentionInDays' - Specifica il numero di giorni di conservazione della cronologia di valutazione dei criteri. Se il valore è pari a 0, la cronologia non verrà rimossa automaticamente.  
  
-   'LogOnSuccess' - Specifica se vengono registrate le valutazioni di criteri riuscite nella gestione basata su criteri.  
  
`[ @value = ] value` Valore associato al valore specificato per *Name*. il *valore* è **sql_variant** ed è obbligatorio.  
  
-   Se si specifica ' Enabled ' per il *nome*, è possibile usare uno dei valori seguenti:  
  
    -   0 = Disabilita la gestione basata su criteri.  
  
    -   1 = Abilita la gestione basata su criteri.  
  
-   Se si specifica "HistoryRententionInDays" per il *nome*, specificare il numero di giorni come valore intero.  
  
-   Se si specifica "LogOnSuccess" per il *nome*, è possibile usare uno dei valori seguenti:  
  
    -   0 = Registra solo le valutazioni di criteri non riuscite.  
  
    -   1 = Registra sia le valutazioni di criteri riuscite che quelle fallite.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 È necessario eseguire sp_syspolicy_configure nel contesto del database di sistema msdb.  
  
 Per visualizzare i valori impostati attualmente, eseguire una query sulla vista di sistema msdb.dbo.syspolicy_configuration.  
  
## <a name="permissions"></a>Autorizzazioni  
 È necessaria l'appartenenza al ruolo predefinito del database PolicyAdministratorRole.  
  
> [!IMPORTANT]  
>  Possibile elevazione di credenziali: gli utenti nel ruolo PolicyAdministratorRole possono creare trigger server e pianificare le esecuzioni di criteri che influiscono sul funzionamento dell'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Gli utenti con il ruolo PolicyAdministratorRole possono ad esempio creare criteri che impediscono la creazione della maggior parte degli oggetti nel [!INCLUDE[ssDE](../../includes/ssde-md.md)]. A causa di questa possibile elevazione di credenziali, il ruolo PolicyAdministratorRole deve essere concesso solo a utenti ritenuti attendibili per il controllo della configurazione del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene abilitata la gestione basata su criteri.  
  
```  
EXEC msdb.dbo.sp_syspolicy_configure @name = N'Enabled'  
, @value = 1;  
  
GO  
```  
  
 Nell'esempio seguente viene impostato un periodo di memorizzazione cronologia criteri di 14 giorni.  
  
```  
EXEC msdb.dbo.sp_syspolicy_configure @name = N'HistoryRetentionInDays'  
, @value = 14;  
  
GO  
```  
  
 Nell'esempio seguente viene configurata la registrazione sia delle valutazioni di criteri riuscite che di quelle non riuscite nella gestione basata su criteri.  
  
```  
EXEC msdb.dbo.sp_syspolicy_configure @name = N'LogOnSuccess'  
, @value = 1;  
  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure per la gestione basata su criteri &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)   
 [sp_syspolicy_set_config_enabled &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syspolicy-set-config-enabled-transact-sql.md)   
 [sp_syspolicy_set_config_history_retention &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syspolicy-set-config-history-retention-transact-sql.md)   
 [sp_syspolicy_set_log_on_success &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syspolicy-set-log-on-success-transact-sql.md)  
  
  
