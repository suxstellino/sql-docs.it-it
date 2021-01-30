---
description: RECEIVE (Transact-SQL)
title: RECEIVE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RECEIVE_TSQL
- RECEIVE
dev_langs:
- TSQL
helpviewer_keywords:
- queues [Service Broker], message retrieval
- messages [Service Broker], retrieving
- RECEIVE statement
- receiving messages
- retrieving messages
ms.assetid: 878c6c14-37ab-4b87-9854-7f8f42bac7dd
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e1665c7fa5a0cfcbd1d3d14fc83dbcfc82697cef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187214"
---
# <a name="receive-transact-sql"></a>RECEIVE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Recupera uno o più messaggi da una coda. A seconda del periodo di memorizzazione impostato per la coda, questa istruzione rimuove il messaggio dalla coda o aggiorna lo stato del messaggio nella coda.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
[ WAITFOR ( ]  
    RECEIVE [ TOP ( n ) ]   
        <column_specifier> [ ,...n ]  
        FROM <queue>  
        [ INTO table_variable ]  
        [ WHERE {  conversation_handle = conversation_handle  
                 | conversation_group_id = conversation_group_id } ]  
[ ) ] [ , TIMEOUT timeout ]  
[ ; ]  
  
<column_specifier> ::=  
{    *   
  |  { column_name | [ ] expression } [ [ AS ] column_alias ]  
  |  column_alias = expression   
}     [ ,...n ]   
  
<queue> ::=  
{ database_name.schema_name.queue_name | schema_name.queue_name | queue_name }
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 WAITFOR  
 Specifica che l'istruzione RECEIVE deve rimanere in attesa dell'arrivo di un messaggio nella coda, se non sono presenti messaggi.  
  
 TOP( *n* )  
 Specifica il numero massimo di messaggi da restituire. Se questa clausola viene omessa, vengono restituiti tutti i messaggi corrispondenti ai criteri dell'istruzione.  
  
 \*  
 Specifica che il set di risultati contiene tutte le colonne nella coda.  
  
 *column_name*  
 Nome di una colonna da includere nel set di risultati.  
  
 *expression*  
 Nome di colonna, costante, funzione o qualsiasi combinazione di nomi di colonna, costanti e funzioni concatenati da un operatore.  
  
 *column_alias*  
 Nome alternativo con cui sostituire il nome di colonna nel set di risultati.  
  
 FROM  
 Specifica la coda contenente i messaggi da recuperare.  
  
 *database_name*  
 Nome del database contenente la coda da cui ricevere i messaggi. Se non si specifica *database_name*, per impostazione predefinita viene usato il database corrente.  
  
 *schema_name*  
 Nome dello schema proprietario della coda da cui ricevere i messaggi. Se non si specifica *schema_name*, per impostazione predefinita viene usato lo schema predefinito dell'utente corrente.  
  
 *queue_name*  
 Nome della coda da cui ricevere i messaggi.  
  
 INTO *table_variable*  
 Specifica la variabile di tabella in cui vengono inseriti i messaggi tramite RECEIVE. La variabile di tabella deve includere un numero di colonne uguale a quello presente nei messaggi. Il tipo di dati di ogni colonna nella variabile di tabella deve supportare la conversione implicita nel tipo di dati della colonna corrispondente nei messaggi. Se non si specifica INTO, i messaggi vengono restituiti come set di risultati.  
  
 WHERE  
 Specifica la conversazione o il gruppo di conversazioni per i messaggi ricevuti. Se la clausola viene omessa, vengono restituiti i messaggi dal successivo gruppo di conversazioni disponibile.  
  
 conversation_handle = *conversation_handle*  
 Specifica la conversazione per i messaggi ricevuti. L'*handle di conversazione* specificato deve essere un tipo **uniqueidentifier** o un tipo convertibile in **uniqueidentifier**.  
  
 conversation_group_id = *conversation_group_id*  
 Specifica il gruppo di conversazioni per i messaggi ricevuti. L'*ID gruppo di conversazioni* specificato deve essere un tipo **uniqueidentifier** o un tipo convertibile in **uniqueidentifier**.  
  
 TIMEOUT *timeout*  
 Specifica l'intervallo di tempo, in millisecondi, per cui l'istruzione deve rimanere in attesa di un messaggio. È possibile utilizzare questa clausola solo insieme alla clausola WAITFOR. Se non si specifica questa clausola o se il timeout è -**1**, il tempo di attesa è illimitato. Alla scadenza del timeout, l'istruzione RECEIVE restituisce un set di risultati vuoto.  
  
## <a name="remarks"></a>Commenti  
  
> [!IMPORTANT]  
>  Se l'istruzione RECEIVE non è la prima istruzione in un batch o in una stored procedure, l'istruzione precedente deve terminare con un punto e virgola (;).  
  
 L'istruzione RECEIVE legge i messaggi da una coda e restituisce un set di risultati. Il set di risultati è composto da zero o più righe, ognuna contenente un singolo messaggio. Se non si usa la clausola INTO e il parametro *column_specifier* non assegna valori a variabili locali, l'istruzione restituisce un set di risultati al programma chiamante.  
  
 I messaggi restituiti dall'istruzione RECEIVE possono essere di tipi diversi. Le applicazioni possono usare la colonna **message_type_name** per indirizzare ogni messaggio a codice che gestisce il tipo di messaggio associato. Esistono due classi di tipi di messaggi:  
  
-   Tipi di messaggi definiti dall'applicazione creati utilizzando l'istruzione CREATE MESSAGE TYPE. Il set di tipi di messaggi definiti dall'applicazione consentiti in una conversazione è definito dal contratto [!INCLUDE[ssSB](../../includes/sssb-md.md)] specificato per la conversazione.  
  
-   Messaggi del sistema di [!INCLUDE[ssSB](../../includes/sssb-md.md)] che restituiscono informazioni sullo stato o sull'errore.  
  
 L'istruzione RECEIVE rimuove i messaggi ricevuti dalla coda a meno che per la coda non sia specificato un periodo di memorizzazione dei messaggi. Se l'impostazione di RETENTION per la coda è ON, l'istruzione RECEIVE aggiorna la colonna **status** impostando il valore **0** e lascia i messaggi nella coda. Se viene eseguito il rollback di una transazione che contiene un'istruzione RECEIVE, viene eseguito il rollback anche di tutte le modifiche apportate alla coda nella transazione, con conseguente ripristino dei messaggi nella coda.  
  
 Tutti i messaggi restituiti da un'istruzione RECEIVE appartengono allo stesso gruppo di conversazioni. L'istruzione RECEIVE blocca il gruppo di conversazioni per i messaggi restituiti fino al completamento della transazione che contiene l'istruzione. Un'istruzione **RECEIVE** restituisce messaggi per cui il valore della colonna status è **1**. Il set di risultati restituito da un'istruzione RECEIVE viene ordinato in modo implicito:  
  
-   Se i messaggi di più conversazioni soddisfano le condizioni della clausola WHERE, l'istruzione RECEIVE restituisce tutti i messaggi di una conversazione prima di restituire i messaggi di qualsiasi altra conversazione. Le conversazioni vengono elaborate in ordine di livello di priorità decrescente.  
  
-   Per una determinata conversazione, l'istruzione RECEIVE restituisce i messaggi in base all'ordine crescente di **message_sequence_number**.  
  
 Nella clausola WHERE dell'istruzione RECEIVE è possibile includere una sola condizione di ricerca che usa parametri **conversation_handle** o **conversation_group_id**. La condizione di ricerca non può contenere una o più altre colonne nella coda. **conversation_handle** o **conversation_group_id** non può essere un'espressione. Il set di messaggi restituito dipende dalle condizioni specificate nella clausola WHERE:  
  
-   Se è specificato **conversation_handle**, RECEIVE restituisce tutti i messaggi della conversazione specificata disponibili nella coda.  
  
-   Se è specificato **conversation_group_id**, RECEIVE restituisce tutti i messaggi disponibili nella coda di qualsiasi conversazione membro del gruppo di conversazioni specificato.  
  
-   Se non è presente la clausola WHERE, RECEIVE determina quale gruppo di conversazioni:  
  
    -   Dispone di uno o più messaggi nella coda.  
  
    -   Non è stato bloccato da un'altra istruzione RECEIVE.  
  
    -   Ha il livello di priorità più elevato di tutti i gruppi di conversazioni che soddisfano questi criteri.  
  
     RECEIVE restituisce quindi tutti i messaggi disponibili nella coda di qualsiasi conversazione membro del gruppo di conversazioni selezionato.  
  
 Se l'handle di conversazione o l'identificatore del gruppo di conversazioni specificato nella clausola WHERE non esiste oppure non è associato alla coda specificata, l'istruzione RECEIVE restituisce un errore.  
  
 Se lo stato della coda specificata nell'istruzione RECEIVE è impostato su OFF, l'istruzione ha esito negativo e viene generato un errore [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 Se si specifica la clausola WAITFOR, l'istruzione rimane in attesa per il periodo di timeout specificato oppure fino a quando non è disponibile un set di risultati. Se la coda viene eliminata o lo stato della coda viene impostato su OFF mentre l'istruzione è in attesa, l'istruzione restituisce immediatamente un errore. Se l'istruzione RECEIVE specifica un gruppo di conversazioni o un handle di conversazione e il servizio per tale conversazione viene eliminato o spostato in un'altra coda, viene generato un errore [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 RECEIVE non è un'istruzione valida in una funzione definita dall'utente.  
  
 L'istruzione RECEIVE non dispone di un meccanismo di prevenzione della mancanza di priorità. Se una singola istruzione RECEIVE blocca un gruppo di conversazioni e recupera numerosi messaggi da conversazioni con priorità bassa, non è possibile ricevere messaggi di conversazioni con priorità alta nel gruppo. Per impedire che questo si verifichi, quando si recuperano messaggi da conversazioni con priorità bassa, utilizzare la clausola TOP per limitare il numero di messaggi recuperati da ogni istruzione RECEIVE.  
  
## <a name="queue-columns"></a>Colonne della coda  
 Nella tabella seguente vengono elencate le colonne di una coda:  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**Stato**|**tinyint**|Stato del messaggio. Per i messaggi restituiti dal comando RECEIVE, lo stato è sempre **0**. I messaggi nella coda possono contenere uno dei valori seguenti:<br /><br /> **0**=Pronto **1**=Messaggio ricevuto **2**=Non ancora completo **3**=Messaggio inviato memorizzato|  
|**priority**|**tinyint**|Livello di priorità della conversazione applicato al messaggio.|  
|**queuing_order**|**bigint**|Numero progressivo del messaggio nella coda.|  
|**conversation_group_id**|**uniqueidentifier**|Identificatore del gruppo di conversazioni a cui appartiene il messaggio.|  
|**conversation_handle**|**uniqueidentifier**|Handle della conversazione di cui fa parte il messaggio.|  
|**message_sequence_number**|**bigint**|Numero di sequenza del messaggio nella conversazione.|  
|**service_name**|**nvarchar(512)**|Nome del servizio a cui è destinata la conversazione.|  
|**service_id**|**int**|Identificatore di oggetto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del servizio a cui è destinata la conversazione.|  
|**service_contract_name**|**nvarchar(256)**|Nome del contratto rispettato dalla conversazione.|  
|**service_contract_id**|**int**|Identificatore di oggetto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del contratto rispettato dalla conversazione.|  
|**message_type_name**|**nvarchar(256)**|Nome del tipo di messaggio che descrive il formato del messaggio. I messaggi possono essere tipi di messaggi dell'applicazione o messaggi di sistema di Service Broker.|  
|**message_type_id**|**int**|Identificatore di oggetto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del tipo di messaggio che descrive il messaggio.|  
|**validation**|**nchar(2)**|Convalida utilizzata per il messaggio.<br /><br /> **E**=Empty **N**=None **X**=XML|  
|**message_body**|**varbinary(MAX)**|Contenuto del messaggio.|  
  
## <a name="permissions"></a>Autorizzazioni  
 Per ricevere un messaggio, l'utente corrente deve disporre dell'autorizzazione RECEIVE per la coda.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-receiving-all-columns-for-all-messages-in-a-conversation-group"></a>R. Ricezione di tutte le colonne di tutti i messaggi in un gruppo di conversazioni  
 Nell'esempio seguente, l'istruzione è impostata per la ricezione di tutti i messaggi disponibili per il successivo gruppo di conversazioni disponibile dalla coda `ExpenseQueue`. L'istruzione restituisce i messaggi in forma di set di risultati.  
  
```sql  
RECEIVE * FROM ExpenseQueue ;  
```  
  
### <a name="b-receiving-specified-columns-for-all-messages-in-a-conversation-group"></a>B. Ricezione delle colonne specificate per tutti i messaggi in un gruppo di conversazioni  
 Nell'esempio seguente, l'istruzione è impostata per la ricezione di tutti i messaggi disponibili per il successivo gruppo di conversazioni disponibile dalla coda `ExpenseQueue`. L'istruzione restituisce i messaggi in forma di set di risultati contenente le colonne `conversation_handle`, `message_type_name` e `message_body`.  
  
```sql  
RECEIVE conversation_handle, message_type_name, message_body  
FROM ExpenseQueue ;  
```  
  
### <a name="c-receiving-the-first-available-message-in-the-queue"></a>C. Ricezione del primo messaggio disponibile nella coda  
 Nell'esempio seguente, l'istruzione è impostata per la ricezione del primo messaggio disponibile dalla coda `ExpenseQueue` in forma di set di risultati.  
  
```sql  
RECEIVE TOP (1) * FROM ExpenseQueue ;  
```  
  
### <a name="d-receiving-all-messages-for-a-specified-conversation"></a>D. Ricezione di tutti i messaggi per una conversazione specificata  
 Nell'esempio seguente, l'istruzione è impostata per la ricezione di tutti i messaggi disponibili per la conversazione specificata dalla coda `ExpenseQueue` in forma di set di risultati.  
  
```sql  
DECLARE @conversation_handle UNIQUEIDENTIFIER ;  
  
SET @conversation_handle = <retrieve conversation from database> ;  
  
RECEIVE *  
FROM ExpenseQueue  
WHERE conversation_handle = @conversation_handle ;  
```  
  
### <a name="e-receiving-messages-for-a-specified-conversation-group"></a>E. Ricezione dei messaggi per un gruppo di conversazioni specificato  
 Nell'esempio seguente, l'istruzione è impostata per la ricezione di tutti i messaggi disponibili per il gruppo di conversazioni specificato dalla coda `ExpenseQueue` in forma di set di risultati.  
  
```sql  
DECLARE @conversation_group_id UNIQUEIDENTIFIER ;  
  
SET @conversation_group_id =   
    <retrieve conversation group ID from database> ;  
  
RECEIVE *  
FROM ExpenseQueue  
WHERE conversation_group_id = @conversation_group_id ;  
```  
  
### <a name="f-receiving-into-a-table-variable"></a>F. Ricezione in una variabile di tabella  
 Nell'esempio seguente, l'istruzione è impostata per la ricezione di tutti i messaggi disponibili per il gruppo di conversazioni specificato dalla coda `ExpenseQueue` in una variabile di tabella.  
  
```sql  
DECLARE @conversation_group_id UNIQUEIDENTIFIER ;  
  
DECLARE @procTable TABLE(  
     service_instance_id UNIQUEIDENTIFIER,  
     handle UNIQUEIDENTIFIER,  
     message_sequence_number BIGINT,  
     service_name NVARCHAR(512),  
     service_contract_name NVARCHAR(256),  
     message_type_name NVARCHAR(256),  
     validation NCHAR,  
     message_body VARBINARY(MAX)) ;  
  
SET @conversation_group_id = <retrieve conversation group ID from database> ;  
  
RECEIVE TOP (1)  
    conversation_group_id,  
    conversation_handle,  
    message_sequence_number,  
    service_name,  
    service_contract_name,  
    message_type_name,  
    validation,  
    message_body  
FROM ExpenseQueue  
INTO @procTable  
WHERE conversation_group_id = @conversation_group_id ;  
```  
  
### <a name="g-receiving-messages-and-waiting-indefinitely"></a>G. Ricezione dei messaggi e attesa illimitata  
 Nell'esempio seguente, l'istruzione è impostata per la ricezione di tutti i messaggi disponibili per il successivo gruppo di conversazioni disponibile dalla coda `ExpenseQueue`. L'istruzione attende fino a quando non diventa disponibile almeno un messaggio, quindi restituisce un set di risultati contenente tutte le colonne del messaggio.  
  
```sql  
WAITFOR (  
    RECEIVE *  
    FROM ExpenseQueue) ;  
```  
  
### <a name="h-receiving-messages-and-waiting-for-a-specified-interval"></a>H. Ricezione dei messaggi e attesa per un intervallo di tempo specificato  
 Nell'esempio seguente, l'istruzione è impostata per la ricezione di tutti i messaggi disponibili per il successivo gruppo di conversazioni disponibile dalla coda `ExpenseQueue`. L'istruzione attende 60 secondi o fino a quando non diventa disponibile almeno un messaggio, a seconda dell'evento che si verifica per primo. L'istruzione restituisce un set di risultati contenente tutte le colonne del messaggio, se è disponibile almeno un messaggio. In caso contrario, l'istruzione restituisce un set di risultati vuoto.  
  
```sql  
WAITFOR (  
    RECEIVE *  
    FROM ExpenseQueue ),  
TIMEOUT 60000 ;  
```  
  
### <a name="i-receiving-messages-modifying-the-type-of-a-column"></a>I. Ricezione dei messaggi con modifica del tipo di una colonna  
 Nell'esempio seguente, l'istruzione è impostata per la ricezione di tutti i messaggi disponibili per il successivo gruppo di conversazioni disponibile dalla coda `ExpenseQueue`. Se il tipo di messaggio indica che il messaggio contiene un documento XML, l'istruzione converte il corpo del messaggio in XML.  
  
```sql  
WAITFOR (  
    RECEIVE message_type_name,  
        CASE  
            WHEN validation = 'X' THEN CAST(message_body as XML)  
            ELSE NULL  
         END AS message_body   
         FROM ExpenseQueue ),  
TIMEOUT 60000 ;  
```  
  
### <a name="j-receiving-a-message-extracting-data-from-the-message-body-retrieving-conversation-state"></a>J. Ricezione di un messaggio con estrazione dei dati dal corpo del messaggio e recupero dello stato della conversazione  
 Nell'esempio seguente, l'istruzione è impostata per la ricezione del successivo messaggio disponibile per il successivo gruppo di conversazioni disponibile dalla coda `ExpenseQueue`. Se il messaggio è di tipo `//Adventure-Works.com/Expenses/SubmitExpense`, l'istruzione estrae l'ID del dipendente e un elenco di elementi dal corpo del messaggio. L'istruzione recupera inoltre lo stato della conversazione dalla tabella `ConversationState`.  
  
```sql  
WAITFOR(  
    RECEIVE   
    TOP(1)  
      message_type_name,  
      COALESCE(  
           (SELECT TOP(1) ConversationState  
            FROM CurrentConversations AS cc  
            WHERE cc.ConversationHandle = conversation_handle),  
           'NEW')  
      AS ConversationState,  
      COALESCE(  
          (SELECT TOP(1) ErrorCount  
           FROM CurrentConversations AS cc  
           WHERE cc.ConversationHandle = conversation_handle),   
           0)  
      AS ConversationErrors,  
      CASE WHEN message_type_name = N'//Adventure-Works.com/Expenses/SubmitExpense'  
          THEN CAST(message_body AS XML).value(  
                'declare namespace rpt = "https://Adventure-Works.com/schemas/expenseReport"  
                   (/rpt:ExpenseReport/rpt:EmployeeID)[1]', 'nvarchar(20)')  
         ELSE NULL  
      END AS EmployeeID,  
      CASE WHEN message_type_name = N'//Adventure-Works.com/Expenses/SubmitExpense'  
          THEN CAST(message_body AS XML).query(  
                'declare namespace rpt = "https://Adventure-Works.com/schemas/expenseReport"   
                     /rpt:ExpenseReport/rpt:ItemDetail')  
          ELSE NULL  
      END AS ItemList  
    FROM ExpenseQueue   
), TIMEOUT 60000 ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [BEGIN DIALOG CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [BEGIN CONVERSATION TIMER &#40;Transact-SQL&#41;](../../t-sql/statements/begin-conversation-timer-transact-sql.md)   
 [END CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/end-conversation-transact-sql.md)   
 [CREATE CONTRACT &#40;Transact-SQL&#41;](../../t-sql/statements/create-contract-transact-sql.md)   
 [CREATE MESSAGE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-message-type-transact-sql.md)   
 [SEND &#40;Transact-SQL&#41;](../../t-sql/statements/send-transact-sql.md)   
 [CREATE QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/create-queue-transact-sql.md)   
 [ALTER QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-queue-transact-sql.md)   
 [DROP QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-queue-transact-sql.md)  
  
  
