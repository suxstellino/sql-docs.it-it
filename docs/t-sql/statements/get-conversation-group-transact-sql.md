---
description: GET CONVERSATION GROUP (Transact-SQL)
title: GET CONVERSATION GROUP (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- GET
- CONVERSATION_GROUP_TSQL
- GET_TSQL
- GET_CONVERSATION_GROUP_TSQL
- GET CONVERSATION GROUP
- CONVERSATION GROUP
- GET CONVERSATION
- GET_CONVERSATION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- GET CONVERSATION GROUP statement
- conversations [Service Broker], groups
ms.assetid: 4da8a855-33c0-43b2-a49d-527487cb3b5c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e54839bd7690b0417142fb4ac77994470f3a076e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091915"
---
# <a name="get-conversation-group-transact-sql"></a>GET CONVERSATION GROUP (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Restituisce l'identificatore del gruppo di conversazioni per il messaggio successivo da ricevere e blocca il gruppo di conversazioni per la conversazione contenente il messaggio. L'identificatore del gruppo di conversazioni può essere utilizzato per recuperare le informazioni sullo stato della conversazione prima di recuperare il messaggio stesso.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql 
[ WAITFOR ( ]  
   GET CONVERSATION GROUP @conversation_group_id  
      FROM <queue>  
[ ) ] [ , TIMEOUT timeout ]  
[ ; ]  
  
<queue> ::=  
{ database_name.schema_name.queue_name | schema_name.queue_name | queue_name }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 WAITFOR  
 Specifica che l'istruzione GET CONVERSATION GROUP deve rimanere in attesa dell'arrivo di un messaggio nella coda, se non sono presenti messaggi.  
  
 *\@conversation_group_id*  
 Variabile utilizzata per archiviare l'ID del gruppo di conversazioni restituito dall'istruzione GET CONVERSATION GROUP. La variabile deve essere di tipo **uniqueidentifier**. Se non sono disponibili gruppi di conversazione, la variabile viene impostata su NULL.  
  
 FROM  
 Specifica la coda da cui recuperare il gruppo di conversazioni.  
  
 *database_name*  
 Nome del database contenente la coda da cui recuperare il gruppo di messaggi. Se non si specifica *database_name*, per impostazione predefinita viene usato il database corrente.  
  
 *schema_name*  
 Nome dello schema proprietario della coda da cui recuperare il gruppo di messaggi. Se non si specifica *schema_name*, per impostazione predefinita viene usato lo schema predefinito dell'utente corrente.  
  
 *queue_name*  
 Nome della coda da cui recuperare il gruppo di conversazioni.  
  
 TIMEOUT *timeout*  
 Specifica la quantità di tempo, in millisecondi, che Service Broker attende l'arrivo di un messaggio nella coda. È possibile utilizzare questa clausola solo insieme alla clausola WAITFOR. Se un'istruzione che usa l'istruzione WAITFOR non include questa clausola oppure se *timeout* è -1, il tempo di attesa è illimitato. Se il timeout scade, l'istruzione GET CONVERSATION GROUP imposta la variabile *\@conversation_group_id* su NULL.  
  
## <a name="remarks"></a>Osservazioni  
  
> [!IMPORTANT]  
>  Se l'istruzione GET CONVERSATION GROUP non è la prima istruzione in un batch o in una stored procedure, l'istruzione precedente deve terminare con un punto e virgola (**;**), ovvero il terminatore di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 Se la coda specificata nell'istruzione GET CONVERSATION GROUP non è disponibile, l'istruzione viene interrotta con un errore [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 Questa istruzione restituisce il gruppo di conversazioni successivo in cui tutte le condizioni seguenti sono vere:  
  
-   Il gruppo di conversazioni può essere correttamente bloccato.  
  
-   Nella coda del gruppo di conversazioni sono disponibili messaggi.  
  
-   Il gruppo di conversazioni ha il livello di priorità più elevato di tutti i gruppi di conversazioni che soddisfano i criteri elencati in precedenza. Il livello di priorità di un gruppo di conversazioni corrisponde a quello più elevato assegnato a qualsiasi conversazione membro del gruppo e per cui sono presenti messaggi nella coda.  
  
 Le chiamate all'istruzione GET CONVERSATION GROUP all'interno della stessa transazione potrebbero bloccare più di un gruppo di conversazioni. Se non è disponibile alcun gruppo di conversazioni, l'istruzione restituisce NULL come identificatore del gruppo di conversazioni.  
  
 Se si specifica la clausola WAITFOR, l'istruzione rimane in attesa per il periodo di timeout specificato oppure fino a quando non è disponibile un gruppo di conversazioni. Se la coda viene eliminata durante l'attesa dell'istruzione, viene restituito un errore.  
  
 GET CONVERSATION GROUP non è valida in una funzione definita dall'utente.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per recuperare un identificatore di un gruppo di conversazioni da una coda, l'utente corrente deve disporre dell'autorizzazione RECEIVE per la coda.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-getting-a-conversation-group-waiting-indefinitely"></a>R. Recupero di un gruppo di conversazioni e attesa illimitata  
 Nell'esempio seguente `@conversation_group_id` viene impostato sull'identificatore del gruppo di conversazioni per il successivo messaggio disponibile in `ExpenseQueue`. Il comando rimane in attesa finché non è disponibile un messaggio.  
  
```sql  
DECLARE @conversation_group_id UNIQUEIDENTIFIER ;  
  
WAITFOR (  
 GET CONVERSATION GROUP @conversation_group_id  
     FROM ExpenseQueue  
) ;  
```  
  
### <a name="b-getting-a-conversation-group-waiting-one-minute"></a>B. Recupero di un gruppo di conversazioni e attesa di un minuto  
 Nell'esempio seguente `@conversation_group_id` viene impostato sull'identificatore del gruppo di conversazioni per il successivo messaggio disponibile in `ExpenseQueue`. Se nessun messaggio diventa disponibile entro un minuto, GET CONVERSATION GROUP restituisce il valore di `@conversation_group_id` senza modificarlo.  
  
```sql  
DECLARE @conversation_group_id UNIQUEIDENTIFIER  
  
WAITFOR (  
    GET CONVERSATION GROUP @conversation_group_id   
    FROM ExpenseQueue ),  
TIMEOUT 60000 ;  
```  
  
### <a name="c-getting-a-conversation-group-returning-immediately"></a>C. Recupero di un gruppo di conversazioni e restituzione immediata del valore  
 Nell'esempio seguente `@conversation_group_id` viene impostato sull'identificatore del gruppo di conversazioni per il successivo messaggio disponibile in `ExpenseQueue`. Se non è disponibile alcun messaggio, `GET CONVERSATION GROUP` restituisce immediatamente il valore di `@conversation_group_id` senza modificarlo.  
  
```sql  
DECLARE @conversation_group_id UNIQUEIDENTIFIER ;  
  
GET CONVERSATION GROUP @conversation_group_id  
FROM AdventureWorks.dbo.ExpenseQueue ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [BEGIN DIALOG CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [MOVE CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/move-conversation-transact-sql.md)  
  
  
