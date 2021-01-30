---
description: sp_vupgrade_mergeobjects (Transact-SQL)
title: sp_vupgrade_mergeobjects (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_vupgrade_mergeobjects
- sp_vupgrade_mergeobjects_TSQL
helpviewer_keywords:
- sp_vupgrade_mergeobjects
ms.assetid: 73257c2e-cc4c-48e7-9d66-7ef045bdd4f5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 26e7405647b80d26c760bf88caf5e6aa786346ff
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201837"
---
# <a name="sp_vupgrade_mergeobjects-transact-sql"></a>sp_vupgrade_mergeobjects (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Rigenera stored procedure, viste e trigger specifici dell'articolo che vengono utilizzati per il rilevamento e l'applicazione delle modifiche dei dati per la replica di tipo merge. Eseguire questa stored procedure nelle situazioni seguenti:  
  
-   Se un oggetto necessario per la replica viene accidentalmente eliminato.  
  
-   Se si applica un aggiornamento, ad esempio un hotfix, che richiede la modifica di uno o più oggetti di replica. Eseguire la stored procedure in ogni nodo dopo l'applicazione dell'aggiornamento.  
  
 In caso di esecuzione di questa stored procedure non è necessaria la reinizializzazione delle sottoscrizioni. La stored procedure non è necessaria se si installa un Service Pack o un aggiornamento a una nuova versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_vupgrade_mergeobjects [ [@login = ] 'login' ]  
    [ , [ @password = ] 'password' ]  
    [ , [ @security_mode = ] security_mode ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @login = ] 'login'` Account di accesso dell'amministratore di sistema da utilizzare per la creazione di nuovi oggetti di sistema nel database di distribuzione. *login* è di tipo **sysname** e il valore predefinito è NULL. Questo parametro non è obbligatorio se *security_mode* è impostato su **1**, ovvero l'autenticazione di Windows.  
  
`[ @password = ] 'password'` Password dell'amministratore di sistema da utilizzare per la creazione di nuovi oggetti di sistema nel database di distribuzione. *password* è di **tipo sysname** e il valore predefinito è **''** (stringa vuota). Questo parametro non è obbligatorio se *security_mode* è impostato su **1**, ovvero l'autenticazione di Windows.  
  
`[ @security_mode = ] 'security_mode'` Modalità di sicurezza di accesso da utilizzare per la creazione di nuovi oggetti di sistema nel database di distribuzione. *security_mode* è di **bit** e il valore predefinito è **1**. Se è **0**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verrà utilizzata l'autenticazione. Se è **1**, verrà utilizzata l'autenticazione di Windows. [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_vupgrade_mergeobjects** viene utilizzata solo per la replica di tipo merge.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server **sysadmin** .  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [Aggiornare database replicati](../../database-engine/install-windows/upgrade-replicated-databases.md)  
  
  
