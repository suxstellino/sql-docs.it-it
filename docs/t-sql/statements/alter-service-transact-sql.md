---
description: ALTER SERVICE (Transact-SQL)
title: ALTER SERVICE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER SERVICE
- ALTER_SERVICE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- modifying services
- contracts [Service Broker], modifying
- ALTER SERVICE statement
- services [Service Broker], modifying
ms.assetid: 2b4608f7-bb2e-4246-aa29-b52c55995b3a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 746e1aa7215acd584e6512c3e39fb79014b7821b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100262"
---
# <a name="alter-service-transact-sql"></a>ALTER SERVICE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifica un servizio esistente.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql 
ALTER SERVICE service_name   
   [ ON QUEUE [ schema_name . ]queue_name ]   
   [ ( < opt_arg > [ , ...n ] ) ]  
[ ; ]  
  
<opt_arg> ::=  
   ADD CONTRACT contract_name | DROP CONTRACT contract_name  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *service_name*  
 Nome del servizio da modificare. Non è possibile specificare i nomi del server, del database e dello schema.  
  
 ON QUEUE [ _schema_name_**.** ] *queue_name*  
 Specifica la nuova coda per questo servizio. [!INCLUDE[ssSB](../../includes/sssb-md.md)] sposta tutti i messaggi per questo servizio dalla coda corrente alla nuova coda.  
  
 ADD CONTRACT *contract_name*  
 Specifica un contratto da aggiungere al set dei contratti esposti da questo servizio.  
  
 DROP CONTRACT *contract_name*  
 Specifica un contratto da eliminare dal set dei contratti esposti da questo servizio. [!INCLUDE[ssSB](../../includes/sssb-md.md)] invia un messaggio di errore per tutte le conversazioni esistenti con il servizio che utilizzano questo contratto.  
  
## <a name="remarks"></a>Commenti  
 Quando l'istruzione ALTER SERVICE elimina un contratto da un servizio, il servizio non può più essere la destinazione per le conversazioni che utilizzano tale contratto. [!INCLUDE[ssSB](../../includes/sssb-md.md)] non consente pertanto nuove conversazioni con il servizio in base a quel contratto. Le conversazioni esistenti che utilizzano il contratto non subiscono variazioni.  
  
 Per modificare il parametro AUTHORIZATION per un servizio, utilizzare l'istruzione ALTER AUTHORIZATION.  
  
## <a name="permissions"></a>Autorizzazioni  
 L'autorizzazione per la modifica di un servizio viene assegnata per impostazione predefinita al proprietario del servizio, ai membri del ruolo predefinito del database **db_ddladmin** o **db_owner** e ai membri del ruolo predefinito del server **sysadmin**.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-changing-the-queue-for-a-service"></a>R. Modifica della coda per un servizio  
 Nell'esempio seguente il servizio `//Adventure-Works.com/Expenses` viene modificato in modo che utilizzi la coda `NewQueue`.  
  
```sql  
ALTER SERVICE [//Adventure-Works.com/Expenses]  
    ON QUEUE NewQueue ;  
```  
  
### <a name="b-adding-a-new-contract-to-the-service"></a>B. Aggiunta di un nuovo contratto al servizio  
 Nell'esempio seguente il servizio `//Adventure-Works.com/Expenses` viene modificato in modo da consentire i dialoghi nel contratto `//Adventure-Works.com/Expenses`.  
  
```sql  
ALTER SERVICE [//Adventure-Works.com/Expenses]  
    (ADD CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission]) ;  
```  
  
### <a name="c-adding-a-new-contract-to-the-service-dropping-existing-contract"></a>C. Aggiunta di un nuovo contratto al servizio, eliminazione del contratto esistente  
 Nell'esempio seguente il servizio `//Adventure-Works.com/Expenses` viene modificato in modo da consentire i dialoghi nel contratto `//Adventure-Works.com/Expenses/ExpenseProcessing` e da non consentire i dialoghi nel contratto `//Adventure-Works.com/Expenses/ExpenseSubmission`.  
  
```sql  
ALTER SERVICE [//Adventure-Works.com/Expenses]  
    (ADD CONTRACT [//Adventure-Works.com/Expenses/ExpenseProcessing],   
     DROP CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission]) ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE SERVICE &#40;Transact-SQL&#41;](../../t-sql/statements/create-service-transact-sql.md)   
 [DROP SERVICE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-service-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
