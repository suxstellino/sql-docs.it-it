---
description: BEGIN DIALOG CONVERSATION (Transact-SQL)
title: BEGIN DIALOG CONVERSATION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DIALOG CONVERSATION
- DIALOG
- BEGIN_DIALOG_TSQL
- BEGIN_DIALOG_CONVERSATION_TSQL
- DIALOG_CONVERSATION_TSQL
- DIALOG_TSQL
- BEGIN DIALOG
- BEGIN DIALOG CONVERSATION
dev_langs:
- TSQL
helpviewer_keywords:
- conversations [Service Broker]
- beginning dialogs
- dialogs [Service Broker], beginning
- BEGIN DIALOG CONVERSATION statement
- cryptography [SQL Server], conversations
- opening conversations
- encryption [SQL Server], conversations
- starting conversations
ms.assetid: 8e814f9d-77c1-4906-b8e4-668a86fc94ba
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f1c32ad7e6a970b9ebb50dc29e7dda767f5e4f40
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098508"
---
# <a name="begin-dialog-conversation-transact-sql"></a>BEGIN DIALOG CONVERSATION (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Inizia un dialogo da un servizio a un altro servizio. Un dialogo è una conversazione per lo scambio di messaggi tra due servizi, messaggi che vengono inviati una sola volta e rispettando l'ordine di invio.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
BEGIN DIALOG [ CONVERSATION ] @dialog_handle  
   FROM SERVICE initiator_service_name  
   TO SERVICE 'target_service_name'  
       [ , { 'service_broker_guid' | 'CURRENT DATABASE' }]   
   [ ON CONTRACT contract_name ]  
   [ WITH  
   [  { RELATED_CONVERSATION = related_conversation_handle   
      | RELATED_CONVERSATION_GROUP = related_conversation_group_id } ]   
   [ [ , ] LIFETIME = dialog_lifetime ]   
   [ [ , ] ENCRYPTION = { ON | OFF }  ] ]  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 **@** _dialog_handle_  
 Variabile utilizzata per archiviare l'handle di dialogo generato dal sistema per il nuovo dialogo restituito dall'istruzione BEGIN DIALOG CONVERSATION. La variabile deve essere di tipo **uniqueidentifier**.  
  
 FROM SERVICE *initiator_service_name*  
 Specifica il servizio che inizia il dialogo. Il nome specificato deve essere il nome di un servizio nel database corrente. La coda specificata per il servizio initiator riceve i messaggi restituiti dal servizio di destinazione e i messaggi creati da Service Broker per la conversazione.  
  
 TO SERVICE **'** _target_service_name_ **'**  
 Specifica il servizio di destinazione con cui iniziare il dialogo. *target_service_name* è di tipo **nvarchar(256)**. Per trovare la corrispondenza con la stringa *target_service_name*, [!INCLUDE[ssSB](../../includes/sssb-md.md)] usa un confronto byte per byte. In altre parole, viene eseguito un confronto con distinzione tra maiuscole e minuscole senza tenere conto delle regole di confronto correnti.  
  
 *service_broker_guid*  
 Specifica il database che ospita il servizio di destinazione. Se un'istanza del servizio di destinazione è ospitata in più di un database, è possibile comunicare con un database specifico indicando un *service_broker_guid*.  
  
 *service_broker_guid* è di tipo **nvarchar(128)**. Per individuare il *service_broker_guid* di un database, eseguire la query seguente nel database:  
  
```sql  
SELECT service_broker_guid  
FROM sys.databases  
WHERE database_id = DB_ID() ;  
```  
  
> [!NOTE]  
>  Questa opzione non è disponibile in un database indipendente.  
  
 **'** CURRENT DATABASE **'**  
 Specifica che la conversazione usa il *service_broker_guid* per il database corrente.  
  
 ON CONTRACT *contract_name*  
 Specifica il contratto rispettato dalla conversazione. Il contratto deve esistere nel database corrente. Se il servizio di destinazione non accetta nuove conversazioni in base al contratto specificato, [!INCLUDE[ssSB](../../includes/sssb-md.md)] restituisce un messaggio di errore nella conversazione. Se la clausola viene omessa, la conversazione rispetta il contratto denominato **DEFAULT**.  
  
 RELATED_CONVERSATION **=** _related_conversation_handle_  
 Specifica il gruppo di conversazioni esistente a cui viene aggiunto il nuovo dialogo. Se si specifica questa clausola, il nuovo dialogo appartiene allo stesso gruppo di conversazioni del dialogo specificato da *related_conversation_handle*. L'argomento *related_conversation_handle* deve essere di un tipo convertibile in modo implicito nel tipo **uniqueidentifier**. L'istruzione ha esito negativo se *related_conversation_handle* non fa riferimento a un dialogo esistente.  
  
 RELATED_CONVERSATION_GROUP **=** _related_conversation_group_id_  
 Specifica il gruppo di conversazioni esistente a cui viene aggiunto il nuovo dialogo. Se si specifica questa clausola, il nuovo dialogo verrà aggiunto al gruppo di conversazioni specificato da *related_conversation_group_id*. L'argomento *related_conversation_group_id* deve essere di un tipo convertibile in modo implicito nel tipo **uniqueidentifier**. Se *related_conversation_group_id* non fa riferimento a un gruppo di conversazioni esistente, Service Broker crea un nuovo gruppo di conversazioni con l'argomento *related_conversation_group_id* specificato e associa il nuovo dialogo a questo gruppo di conversazioni.  
  
 LIFETIME **=** _dialog_lifetime_  
 Specifica la quantità massima di tempo per cui il dialogo rimarrà aperto. Per il corretto completamento del dialogo, è necessario che entrambi gli endpoint terminino il dialogo in modo esplicito prima della scadenza della durata. Il valore di *dialog_lifetime* deve essere espresso in secondi. La durata è di tipo **int**. Se non si specifica la clausola LIFETIME, la durata del dialogo corrisponde al valore massimo del tipo di dati **int**.  
  
 ENCRYPTION  
 Specifica se i messaggi inviati e ricevuti nel dialogo devono essere crittografati quando vengono inviati all'esterno di un'istanza di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un dialogo che deve essere crittografato viene definito *dialogo protetto*. Se ENCRYPTION = ON e non sono configurati i certificati necessari per il supporto della crittografia, [!INCLUDE[ssSB](../../includes/sssb-md.md)] restituisce un messaggio di errore nella conversazione. Se ENCRYPTION = OFF, la crittografia viene usata se è configurata un'associazione al servizio remoto per *target_service_name*. In caso contrario, i messaggi verranno inviati in formato non crittografato. Se la clausola viene omessa, il valore predefinito è ON.  
  
> [!NOTE]  
>  I messaggi scambiati tra servizi nella stessa istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non vengono mai crittografati. Tuttavia, sono comunque necessari una chiave master del database e i certificati per la crittografia per le conversazioni che utilizzano la crittografia, se i servizi per la conversazione si trovano in database diversi. In questo modo le conversazioni non vengono interrotte nel caso uno dei database venga spostato in un'istanza diversa mentre è in corso una conversazione.  
  
## <a name="remarks"></a>Commenti  
 Tutti i messaggi fanno parte di una conversazione. Pertanto, un servizio initiator deve iniziare una conversazione con il servizio di destinazione prima di inviare un messaggio a quest'ultimo. Le informazioni specificate nell'istruzione BEGIN DIALOG CONVERSATION, paragonabili all'indirizzo di una lettera, vengono utilizzate da [!INCLUDE[ssSB](../../includes/sssb-md.md)] per recapitare i messaggi al servizio corretto. Il servizio specificato nella clausola TO SERVICE è l'indirizzo a cui vengono inviati i messaggi. Il servizio specificato nella clausola FROM SERVICE è l'indirizzo del mittente utilizzato per i messaggi di risposta.  
  
 Non è necessario che la destinazione di una conversazione chiami BEGIN DIALOG CONVERSATION. [!INCLUDE[ssSB](../../includes/sssb-md.md)] crea una conversazione nel database di destinazione quando arriva dall'initiator il primo messaggio della conversazione.  
  
 Con l'inizio di un dialogo viene creato un endpoint di conversazione nel database per il servizio di origine, ma non viene creata una connessione di rete all'istanza che ospita il servizio di destinazione. La comunicazione con la destinazione del dialogo viene stabilita da [!INCLUDE[ssSB](../../includes/sssb-md.md)] solo dopo l'invio del primo messaggio.  
  
 Se nell'istruzione BEGIN DIALOG CONVERSATION non viene specificata una conversazione o un gruppo di conversazioni correlato, [!INCLUDE[ssSB](../../includes/sssb-md.md)] crea un nuovo gruppo di conversazioni per la nuova conversazione.  
  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] non consente raggruppamenti arbitrari delle conversazioni. Per tutte le conversazioni in un gruppo è necessario specificare il servizio nella clausola FROM, come initiator o destinazione della conversazione.  
  
 Il comando BEGIN DIALOG CONVERSATION blocca il gruppo di conversazioni che contiene l'argomento *dialog_handle* restituito. Se il comando include una clausola RELATED_CONVERSATION_GROUP, il gruppo di conversazioni per *dialog_handle* è il gruppo di conversazioni specificato nel parametro *related_conversation_group_id*. Se il comando include una clausola RELATED_CONVERSATION, il gruppo di conversazioni per *dialog_handle* è il gruppo di conversazioni associato all'argomento *related_conversation_handle* specificato.  
  
 BEGIN DIALOG CONVERSATION non è un'istruzione valida in una funzione definita dall'utente.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per iniziare un dialogo, l'utente corrente deve disporre dell'autorizzazione RECEIVE per la coda del servizio specificato nella clausola FROM del comando e dell'autorizzazione REFERENCES per il contratto specificato.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-beginning-a-dialog"></a>R. Inizio di un dialogo  
 Nell'esempio seguente viene avviata una conversazione di dialogo e viene archiviato un identificatore per il dialogo in `@dialog_handle.`. Il servizio `//Adventure-Works.com/ExpenseClient` è l'initiator del dialogo, mentre il servizio `//Adventure-Works.com/Expenses` è la destinazione del dialogo. Il dialogo è basato sul contratto `//Adventure-Works.com/Expenses/ExpenseSubmission`.  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER ;  
  
BEGIN DIALOG CONVERSATION @dialog_handle  
   FROM SERVICE [//Adventure-Works.com/ExpenseClient]  
   TO SERVICE '//Adventure-Works.com/Expenses'  
   ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission] ;  
```  
  
### <a name="b-beginning-a-dialog-with-an-explicit-lifetime"></a>B. Inizio di un dialogo con una durata esplicita  
 Nell'esempio seguente viene avviata una conversazione di dialogo e viene archiviato un identificatore per il dialogo in `@dialog_handle`. `//Adventure-Works.com/ExpenseClient` è il servizio initiator del dialogo, mentre `//Adventure-Works.com/Expenses` rappresenta il servizio di destinazione del dialogo. Il dialogo è basato sul contratto `//Adventure-Works.com/Expenses/ExpenseSubmission`. Se il dialogo non viene chiuso dal comando END CONVERSATION entro `60` secondi, Service Broker termina il dialogo con un errore.  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER ;  
  
BEGIN DIALOG CONVERSATION @dialog_handle  
   FROM SERVICE [//Adventure-Works.com/ExpenseClient]  
   TO SERVICE '//Adventure-Works.com/Expenses'  
   ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission]  
   WITH LIFETIME = 60 ;  
```  
  
### <a name="c-beginning-a-dialog-with-a-specific-broker-instance"></a>C. Inizio di un dialogo con un'istanza specifica di Service Broker  
 Nell'esempio seguente viene avviata una conversazione di dialogo e viene archiviato un identificatore per il dialogo in `@dialog_handle`. `//Adventure-Works.com/ExpenseClient` è il servizio initiator del dialogo, mentre `//Adventure-Works.com/Expenses` rappresenta il servizio di destinazione del dialogo. Il dialogo è basato sul contratto `//Adventure-Works.com/Expenses/ExpenseSubmission`. Service Broker indirizza i messaggi di questo dialogo all'istanza di Service Broker identificata dal GUID `a326e034-d4cf-4e8b-8d98-4d7e1926c904.`  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER ;  
  
BEGIN DIALOG CONVERSATION @dialog_handle  
   FROM SERVICE [//Adventure-Works.com/ExpenseClient]  
   TO SERVICE '//Adventure-Works.com/Expenses',   
              'a326e034-d4cf-4e8b-8d98-4d7e1926c904'  
   ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission] ;  
```  
  
### <a name="d-beginning-a-dialog-and-relating-it-to-an-existing-conversation-group"></a>D. Inizio di un dialogo e associazione del dialogo a un gruppo di conversazioni esistente  
 Nell'esempio seguente viene avviata una conversazione di dialogo e viene archiviato un identificatore per il dialogo in `@dialog_handle`. `//Adventure-Works.com/ExpenseClient` è il servizio initiator del dialogo, mentre `//Adventure-Works.com/Expenses` rappresenta il servizio di destinazione del dialogo. Il dialogo è basato sul contratto `//Adventure-Works.com/Expenses/ExpenseSubmission`. Service Broker associa il dialogo al gruppo di conversazioni identificato da `@conversation_group_id`, anziché creare un nuovo gruppo di conversazioni.  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER ;  
DECLARE @conversation_group_id UNIQUEIDENTIFIER ;  
  
SET @conversation_group_id = <retrieve conversation group ID from database>  
  
BEGIN DIALOG CONVERSATION @dialog_handle  
   FROM SERVICE [//Adventure-Works.com/ExpenseClient]  
   TO SERVICE '//Adventure-Works.com/Expenses'  
   ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission]  
   WITH RELATED_CONVERSATION_GROUP = @conversation_group_id ;  
```  
  
### <a name="e-beginning-a-dialog-with-an-explicit-lifetime-and-relating-the-dialog-to-an-existing-conversation"></a>E. Inizio di un dialogo con una durata esplicita e associazione del dialogo a una conversazione esistente  
 Nell'esempio seguente viene avviata una conversazione di dialogo e viene archiviato un identificatore per il dialogo in `@dialog_handle`. `//Adventure-Works.com/ExpenseClient` è il servizio initiator del dialogo, mentre `//Adventure-Works.com/Expenses` rappresenta il servizio di destinazione del dialogo. Il dialogo è basato sul contratto `//Adventure-Works.com/Expenses/ExpenseSubmission`. Il nuovo dialogo appartiene allo stesso gruppo di conversazioni a cui appartiene `@existing_conversation_handle`. Se il dialogo non viene chiuso dal comando END CONVERSATION entro `600` secondi, [!INCLUDE[ssSB](../../includes/sssb-md.md)] termina il dialogo con un errore.  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER  
DECLARE @existing_conversation_handle UNIQUEIDENTIFIER  
  
SET @existing_conversation_handle = <retrieve conversation handle from database>  
  
BEGIN DIALOG CONVERSATION @dialog_handle  
   FROM SERVICE [//Adventure-Works.com/ExpenseClient]  
   TO SERVICE '//Adventure-Works.com/Expenses'  
   ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission]  
   WITH RELATED_CONVERSATION = @existing_conversation_handle  
   LIFETIME = 600 ;  
```  
  
### <a name="f-beginning-a-dialog-with-optional-encryption"></a>F. Inizio di un dialogo con crittografia facoltativa  
 Nell'esempio seguente viene avviato un dialogo e viene archiviato un identificatore per il dialogo in `@dialog_handle`. `//Adventure-Works.com/ExpenseClient` è il servizio initiator del dialogo, mentre `//Adventure-Works.com/Expenses` rappresenta il servizio di destinazione del dialogo. Il dialogo è basato sul contratto `//Adventure-Works.com/Expenses/ExpenseSubmission`. La conversazione in questo esempio consente la trasmissione in rete del messaggio senza crittografia, se la crittografia non è disponibile.  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER  
  
BEGIN DIALOG CONVERSATION @dialog_handle  
   FROM SERVICE [//Adventure-Works.com/ExpenseClient]  
   TO SERVICE '//Adventure-Works.com/Expenses'  
   ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission]  
   WITH ENCRYPTION = OFF ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [BEGIN CONVERSATION TIMER &#40;Transact-SQL&#41;](../../t-sql/statements/begin-conversation-timer-transact-sql.md)   
 [END CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/end-conversation-transact-sql.md)   
 [MOVE CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/move-conversation-transact-sql.md)   
 [sys.conversation_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-conversation-endpoints-transact-sql.md)  
  
  
