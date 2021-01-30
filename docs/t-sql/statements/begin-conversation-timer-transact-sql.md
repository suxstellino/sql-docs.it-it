---
description: BEGIN CONVERSATION TIMER (Transact-SQL)
title: BEGIN CONVERSATION TIMER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CONVERSATION
- BEGIN_CONVERSATION_TSQL
- TIMER_TSQL
- TIMER
- CONVERSATION TIMER
- CONVERSATION_TIMER_TSQL
- BEGIN CONVERSATION TIMER
- BEGIN_CONVERSATION_TIMER_TSQL
- CONVERSATION_TSQL
- BEGIN CONVERSATION
dev_langs:
- TSQL
helpviewer_keywords:
- BEGIN CONVERSATION TIMER statement
- DialogTimer message
- dialogs [Service Broker], beginning
- timeouts [Service Broker]
- timer messages [Service Broker]
- conversations [Service Broker], timers
- starting timers [Service Broker]
- https://schemas.microsoft.com/SQL/ServiceBroker/DialogTimer message
ms.assetid: 98e49b3f-a38f-4180-8171-fa9cb30db4cb
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8212a0d9589c325eb716ea2491fd7c78551929fd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184482"
---
# <a name="begin-conversation-timer-transact-sql"></a>BEGIN CONVERSATION TIMER (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Avvia un timer. Alla scadenza del timeout, [!INCLUDE[ssSB](../../includes/sssb-md.md)] inserisce un messaggio di tipo `https://schemas.microsoft.com/SQL/ServiceBroker/DialogTimer` nella coda locale per la conversazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
BEGIN CONVERSATION TIMER ( conversation_handle )  
   TIMEOUT = timeout   
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 BEGIN CONVERSATION TIMER **(** _conversation\_handle_ **)**  
 Specifica la conversazione per cui avviare il timer. *conversation_handle* deve essere di tipo **uniqueidentifier**.  
  
 TIMEOUT  
 Specifica, in secondi, quanto tempo deve trascorrere prima che il messaggio venga inserito nella coda.  
  
## <a name="remarks"></a>Commenti  
 Il timer di conversazione consente a un'applicazione di ricevere un messaggio per una conversazione dopo un periodo di tempo specifico. Se si chiama BEGIN CONVERSATION TIMER per una conversazione prima della scadenza del timer, per il timeout verrà impostato il nuovo valore. A differenza di quanto avviene per la durata della conversazione, ogni lato della conversazione ha un timer indipendente. Il messaggio **DialogTimer** arriva nella coda locale senza che venga interessato il lato remoto della conversazione. Un'applicazione può, quindi, utilizzare un messaggio timer per qualsiasi motivo.  
  
 È possibile, ad esempio, utilizzare il timer di conversazione per evitare che un'applicazione attenda troppo a lungo una risposta scaduta. Se si prevede che un'applicazione completi un dialogo in 30 secondi, è possibile impostare il timer di conversazione per tale dialogo su 60 secondi (30 secondi più altri 30 secondi di tolleranza). Se il dialogo è ancora aperto dopo 60 secondi, l'applicazione riceve un messaggio di timeout nella coda.  
  
 In alternativa un'applicazione può utilizzare un timer di conversazione per richiedere l'attivazione in un determinato momento. Si potrebbe, ad esempio, creare un servizio che segnali il numero di connessioni attive a intervalli di pochi minuti oppure un servizio che segnali il numero di ordini di acquisto aperti ogni sera. Il servizio imposta la scadenza desiderata per un timer di conversazione. Alla scadenza, [!INCLUDE[ssSB](../../includes/sssb-md.md)] invia un messaggio **DialogTimer**, Il messaggio **DialogTimer** fa in modo che [!INCLUDE[ssSB](../../includes/sssb-md.md)] avvii la stored procedure di attivazione per la coda. La stored procedure invia un messaggio al servizio remoto e riavvia il timer di conversazione.  
  
 BEGIN CONVERSATION TIMER non è valida in una funzione definita dall'utente.  
  
## <a name="permissions"></a>Autorizzazioni  
 L'autorizzazione per l'impostazione di un timer di conversazione viene assegnata per impostazione predefinita agli utenti che dispongono delle autorizzazioni SEND per il servizio della conversazione, ai membri del ruolo predefinito del server **sysadmin** e ai membri del ruolo predefinito del database **db_owner**.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene impostato un timeout di due minuti per il dialogo identificato da `@dialog_handle`.  
  
```sql 
-- @dialog_handle is of type uniqueidentifier and  
-- contains a valid conversation handle.  
  
BEGIN CONVERSATION TIMER (@dialog_handle)  
TIMEOUT = 120 ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [BEGIN DIALOG CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [END CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/end-conversation-transact-sql.md)   
 [RECEIVE &#40;Transact-SQL&#41;](../../t-sql/statements/receive-transact-sql.md)  
  
  
