---
description: sp_dropdistpublisher (Transact-SQL)
title: sp_dropdistpublisher (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropdistpublisher
- sp_dropdistpublisher_TSQL
helpviewer_keywords:
- sp_dropdistpublisher
ms.assetid: c0bdd3de-3be0-455c-898a-98d4660e7ce3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 09db4d47afee6795b403542c8442cc9d74724f4b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200701"
---
# <a name="sp_dropdistpublisher-transact-sql"></a>sp_dropdistpublisher (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Elimina un server di pubblicazione di distribuzione. Questa stored procedure viene eseguita in qualsiasi database del server di distribuzione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_dropdistpublisher [ @publisher = ] 'publisher'  
    [ , [ @no_checks = ] no_checks ]  
    [ , [ @ignore_distributor = ] ignore_distributor ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publisher = ] 'publisher'` Server di pubblicazione da eliminare. *Publisher* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @no_checks = ] no_checks` Specifica se **sp_dropdistpublisher** verifica che il server di pubblicazione abbia disinstallato il server come database di distribuzione. *no_checks* è di **bit** e il valore predefinito è **0**.  
  
 Se è **0**, la replica verifica che il server di pubblicazione remoto abbia disinstallato il server locale come server di distribuzione. Se il server di pubblicazione è locale, durante la replica viene verificato che nel server locale non siano più presenti oggetti di pubblicazione o di distribuzione.  
  
 Se è **1**, tutti gli oggetti di replica associati al server di pubblicazione di distribuzione vengono eliminati anche se non è possibile raggiungere un server di pubblicazione remoto. Al termine di questa operazione, il server di pubblicazione remoto deve disinstallare la replica utilizzando [sp_dropdistributor](../../relational-databases/system-stored-procedures/sp-dropdistributor-transact-sql.md) con **\@ ignore_distributor**  =  **1**.  
  
`[ @ignore_distributor = ] ignore_distributor` Specifica se gli oggetti di distribuzione vengono lasciati nel server di distribuzione quando il server di pubblicazione viene rimosso. *ignore_distributor* è di **bit** . i possibili valori sono i seguenti:  
  
 **1** = gli oggetti di distribuzione appartenenti al *server di pubblicazione* rimangono nel database di distribuzione.  
  
 **0** = gli oggetti di distribuzione per il *server di pubblicazione* vengono puliti nel database di distribuzione.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_dropdistpublisher** viene utilizzato in tutti i tipi di replica.  
  
 Quando si elimina un server di pubblicazione Oracle, se non è possibile eliminare il server di pubblicazione **sp_dropdistpublisher** restituisce un errore e gli oggetti del server di distribuzione per il server di pubblicazione vengono rimossi.  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_DropDistPub](../../relational-databases/replication/codesnippet/tsql/sp-dropdistpublisher-tra_1.sql)]  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_dropdistpublisher**.  
  
## <a name="see-also"></a>Vedere anche  
 [Disabilitare la pubblicazione e la distribuzione](../../relational-databases/replication/disable-publishing-and-distribution.md)   
 [sp_adddistpublisher &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)   
 [sp_changedistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedistpublisher-transact-sql.md)   
 [sp_helpdistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistpublisher-transact-sql.md)   
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
