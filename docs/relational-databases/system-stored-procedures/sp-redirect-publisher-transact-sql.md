---
description: sp_redirect_publisher (Transact-SQL)
title: sp_redirect_publisher (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_redirect_publisher_TSQL
- sp_redirect_publisher
helpviewer_keywords:
- sp_redirect_publisher
ms.assetid: af45e2b2-57fb-4bcd-a58b-e61401fb3b26
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d7fef5e51b8a41e6e21d570a398775810dd23155
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185748"
---
# <a name="sp_redirect_publisher-transact-sql"></a>sp_redirect_publisher (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Specifica un server di pubblicazione reindirizzato per una coppia server di pubblicazione/database esistente. Se il database del server di pubblicazione appartiene a un gruppo di disponibilità Always On, il server di pubblicazione reindirizzato è il nome del listener del gruppo di disponibilità associato al gruppo di disponibilità.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_redirect_publisher   
    [ @original_publisher = ] 'original_publisher',  
    [ @publisher_db = ] 'database_name'   
    [ , [ @redirected_publisher = ] 'new_publisher' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @original_publisher = ] 'original_publisher'` Nome dell'istanza di che ha [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicato originariamente il database. *original_publisher* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publisher_db = ] 'publisher_db'` Nome del database da pubblicare. *publisher_db* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @redirected_publisher = ] 'redirected_publisher'` Nome del listener del gruppo di disponibilità associato al gruppo di disponibilità che sarà il nuovo server di pubblicazione. *redirected_publisher* è di **tipo sysname** e non prevede alcun valore predefinito. Quando il listener del gruppo di disponibilità è configurato per una porta non predefinita, specificare il numero della porta insieme al nome del listener, ad esempio `'Listenername,51433'`  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 **sp_redirect_publisher** viene utilizzato per consentire il reindirizzamento di un server di pubblicazione della replica al database primario corrente di un gruppo di disponibilità always on associando la coppia server di pubblicazione/database al listener di un gruppo di disponibilità. Eseguire **sp_redirect_publisher** dopo che il listener del gruppo di disponibilità è stato configurato per il gruppo di disponibilità che contiene il database pubblicato.  
  
 Se il database di pubblicazione nel server di pubblicazione originale viene rimosso da un gruppo di disponibilità nella replica primaria, eseguire **sp_redirect_publisher** senza specificare un valore per il parametro *\@ redirected_publisher* per rimuovere il reindirizzamento per la coppia server di pubblicazione/database. Per ulteriori informazioni sul reindirizzamento del server di pubblicazione quando, vedere [gestione di un database di pubblicazione AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/maintaining-an-always-on-publication-database-sql-server.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 Il chiamante deve essere un membro del ruolo predefinito del server **sysadmin** , il **db_owner** ruolo predefinito del database per il database di distribuzione o un membro di un elenco di accesso alla pubblicazione per una pubblicazione definita associata al database del server di pubblicazione.  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [sp_validate_redirected_publisher &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-validate-redirected-publisher-transact-sql.md)   
 [sp_get_redirected_publisher &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-get-redirected-publisher-transact-sql.md)   
 [sp_validate_replica_hosts_as_publishers &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-validate-replica-hosts-as-publishers-transact-sql.md)  
  
  
