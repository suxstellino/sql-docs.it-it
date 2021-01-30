---
description: sp_get_redirected_publisher (Transact-SQL)
title: sp_get_redirected_publisher (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_get_redirected_publisher_TSQL
- sp_get_redirected_publisher
ms.assetid: d47a9ab5-f2cc-42a8-8be9-a33895ce44f0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: fa1a1b8f7cffe3f98435ebd48a404fb76df34c8a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204762"
---
# <a name="sp_get_redirected_publisher-transact-sql"></a>sp_get_redirected_publisher (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Utilizzato dagli agenti di replica per eseguire una query su un database di distribuzione in modo da determinare se è stato reindirizzato il server di pubblicazione originale.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_get_redirected_publisher   
    [ @original_publisher = ] 'original_publisher',  
    [ @publisher_db = ] 'database_name',   
    [ @bypass_publisher_validation = ] [0 | 1 ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @original_publisher = ] 'original_publisher'` Nome dell'istanza di SQL Server che ha pubblicato originariamente il database. *original_publisher* è di **tipo sysname** e non prevede alcun valore predefinito.
  
`[ @publisher_db = ] 'publisher_db'` Nome del database da pubblicare. *publisher_db* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @bypass_publisher_validation = ] [0 | 1 ]` Utilizzato per ignorare la convalida del server di pubblicazione reindirizzato. Se è 0, viene eseguita la convalida. Se pari a 1, non viene eseguita la convalida. *bypass_publisher_validation* è di **bit** e il valore predefinito è 0.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**redirected_publisher**|**sysname**|Il nome del server di pubblicazione dopo il reindirizzamento.|  
|**error_number**|**int**|Il numero dell'errore di convalida.|  
|**error_severity**|**int**|La gravità dell'errore di convalida.|  
|**error_message**|**nvarchar(4000)**|Il testo del messaggio di errore di convalida.|  
  
## <a name="remarks"></a>Commenti  
 *redirected_publisher* restituisce il nome del server di pubblicazione corrente. Restituisce null se il server di pubblicazione e i database di pubblicazione non sono stati reindirizzati utilizzando **sp_redirect_publisher**.  
  
 Se la convalida non è richiesta o se non esiste alcuna voce per il server di pubblicazione e il database di pubblicazione, *ERROR_NUMBER* e *ERROR_SEVERITY* restituiscono 0 e *ERROR_MESSAGE* restituisce null.  
  
 Se viene richiesta la convalida, viene chiamata la convalida stored procedure [sp_validate_redirected_publisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-validate-redirected-publisher-transact-sql.md) per verificare che la destinazione del reindirizzamento sia un host adatto per il database di pubblicazione. Se la convalida ha esito positivo, **sp_get_redirected_publisher** restituisce il nome del server di pubblicazione reindirizzato, 0 per le colonne *ERROR_NUMBER* e *error_severity* e null nella colonna *ERROR_MESSAGE* .  
  
 Se la convalida viene richiesta e non riesce, il nome del server di pubblicazione reindirizzato viene restituito insieme alle informazioni sull'errore.  
  
## <a name="permissions"></a>Autorizzazioni  
 Il chiamante deve essere un membro del ruolo predefinito del server **sysadmin** , il **db_owner** ruolo predefinito del database per il database di distribuzione o un membro di un elenco di accesso alla pubblicazione per una pubblicazione definita associata al database del server di pubblicazione.  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [sp_validate_redirected_publisher &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-validate-redirected-publisher-transact-sql.md)   
 [sp_redirect_publisher &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-redirect-publisher-transact-sql.md)   
 [sp_validate_replica_hosts_as_publishers &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-validate-replica-hosts-as-publishers-transact-sql.md)  
  
  
