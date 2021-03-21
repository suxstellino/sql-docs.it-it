---
description: ROLLBACK TRANSACTION (Transact-SQL)
title: ROLLBACK TRANSACTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/12/2017
ms.prod: sql
ms.prod_service: synapse-analytics, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ROLLBACK TRANSACTION
- ROLLBACK
- ROLLBACK_TSQL
- ROLLBACK_TRANSACTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- transaction rollbacks [SQL Server]
- ROLLBACK TRANSACTION statement
- erasing data modifications [SQL Server]
- rolling back transactions, ROLLBACK TRANSACTION
- roll back transactions [SQL Server]
- savepoints [SQL Server]
ms.assetid: 6882c5bc-ff74-476a-984b-164aeb036c66
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1d61827c2493379bd71d184949d963ddc24daf2d
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748591"
---
# <a name="rollback-transaction-transact-sql"></a>ROLLBACK TRANSACTION (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Esegue il rollback di una transazione implicita o esplicita fino all'inizio della transazione o fino a un punto di salvataggio. L'istruzione ROLLBACK TRANSACTION consente di cancellare tutte le modifiche dei dati eseguite dall'inizio della transazione o fino a un punto di salvataggio, nonché di liberare le risorse utilizzate dalla transazione.  
  
  Non sono incluse le modifiche apportate alle variabili locali o alle variabili tabella, che non vengono cancellate a questa istruzione.
  

 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
ROLLBACK { TRAN | TRANSACTION }   
     [ transaction_name | @tran_name_variable  
     | savepoint_name | @savepoint_variable ]   
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *transaction_name*  
 Nome assegnato alla transazione su BEGIN TRANSACTION. *transaction_name* deve essere conforme alle regole per gli identificatori, ma vengono usati solo i primi 32 caratteri del nome della transazione. Quando le transazioni sono annidate, *transaction_name* deve essere il nome dell'istruzione BEGIN TRANSACTION più esterna. In *transaction_name* viene sempre applicata la distinzione tra maiuscole e minuscole, anche quando l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non prevede distinzione tra maiuscole e minuscole.  
  
 **@** *tran_name_variable*  
 Nome di una variabile definita dall'utente contenente un nome di transazione valido. La variabile deve essere dichiarata con un tipo di dati **char**, **varchar**, **nchar** o **nvarchar**.  
  
 *savepoint_name*  
 *savepoint_name* di un'istruzione SAVE TRANSACTION. *savepoint_name* deve essere conforme alle regole per gli identificatori. Usare *savepoint_name* quando un rollback condizionale deve interessare solo parte della transazione.  
  
 **@** *savepoint_variable*  
 Nome di una variabile definita dall'utente contenente un punto di salvataggio valido. La variabile deve essere dichiarata con un tipo di dati **char**, **varchar**, **nchar** o **nvarchar**.  
  
## <a name="error-handling"></a>Gestione degli errori  
 Un'istruzione ROLLBACK TRANSACTION non genera messaggi per l'utente. Se nelle stored procedure o nei trigger è necessario visualizzare avvisi, utilizzare l'istruzione RAISERROR o PRINT. RAISERROR è l'istruzione consigliata per la segnalazione di errori.  
  
## <a name="general-remarks"></a>Osservazioni generali  
 L'istruzione ROLLBACK TRANSACTION senza un argomento *savepoint_name* o *transaction_name* esegue il rollback fino all'inizio della transazione. Quando le transazioni sono nidificate, viene eseguito il rollback di tutte le transazioni interne all'istruzione BEGIN TRANSACTION più esterna. In entrambi i casi, ROLLBACK TRANSACTION decrementa la funzione di sistema @@TRANCOUNT a 0. ROLLBACK TRANSACTION *savepoint_name* non decrementa @@TRANCOUNT.  
  
 L'istruzione ROLLBACK TRANSACTION non può fare riferimento a un *savepoint_name* in transazioni distribuite avviate in modo esplicito con l'istruzione BEGIN DISTRIBUTED TRANSACTION o risultanti dall'escalation di una transazione locale.  
  
 Non è possibile eseguire il rollback di una transazione dopo l'esecuzione di un'istruzione COMMIT TRANSACTION, tranne quando l'istruzione COMMIT TRANSACTION viene associata a una transazione nidificata contenuta all'interno della transazione sottoposta a rollback. In questo caso, il rollback della transazione annidata viene eseguito anche se per la transazione è stata eseguita un'istruzione COMMIT TRANSACTION.  
  
 In una transazione sono consentiti nomi di punti di salvataggio duplicati. In tal caso il rollback viene tuttavia eseguito solo fino all'istruzione SAVE TRANSACTION più recente che utilizza il punto di salvataggio.  
  
## <a name="interoperability"></a>Interoperabilità  
 Nelle stored procedure le istruzioni ROLLBACK TRANSACTION senza un argomento *savepoint_name* o *transaction_name* eseguono il rollback di tutte le istruzioni fino all'istruzione BEGIN TRANSACTION più esterna. Se con un'istruzione ROLLBACK TRANSACTION in una stored procedure si ottiene dopo il completamento della stored procedure un valore @@TRANCOUNT diverso rispetto al valore @@TRANCOUNT al momento della chiamata della stored procedure, viene visualizzato un messaggio informativo. Tale messaggio non ha alcun effetto sulle elaborazioni successive.  
  
 Se viene eseguita un'istruzione ROLLBACK TRANSACTION in un trigger:  
  
-   Viene eseguito il rollback di tutte le modifiche dei dati apportate fino a quel punto nella transazione corrente, comprese le modifiche apportate dal trigger.  
  
-   Il trigger continua l'esecuzione delle istruzioni successive all'istruzione ROLLBACK. Se tali istruzioni modificano i dati, non viene eseguito il rollback delle modifiche eseguite. L'esecuzione delle istruzioni rimanenti non attiva trigger nidificati.  
  
-   Le istruzioni del batch successive all'istruzione che ha attivato il trigger non vengono eseguite.  
  
Quando si immette un trigger, il valore di @@TRANCOUNT viene incrementato di una unità, anche in modalità autocommit. Il sistema considera il trigger come transazione nidificata implicita.  
  
Le istruzioni ROLLBACK TRANSACTION nelle stored procedure non hanno alcun effetto sulle istruzioni successive del batch che hanno richiamato la procedura. Tali istruzioni pertanto vengono eseguite. Le istruzioni ROLLBACK TRANSACTION nei trigger comportano l'interruzione del batch contenente l'istruzione che ha attivato il trigger. Le successive istruzioni del batch pertanto non vengono eseguite.  
  
L'effetto di ROLLBACK sui cursori viene definito dalle tre regole seguenti:  
  
1.  Se CURSOR_CLOSE_ON_COMMIT è impostato su ON, i cursori aperti vengono chiusi, ma non deallocati.  
  
2.  Se CURSOR_CLOSE_ON_COMMIT è impostato su OFF, l'istruzione non ha alcun effetto sui cursori STATIC o INSENSITIVE sincroni aperti o sui cursori STATIC asincroni completamente popolati. I cursori aperti di qualsiasi altro tipo vengono chiusi, ma non deallocati.  
  
3.  In seguito a un errore che comporta l'interruzione di un batch e l'esecuzione di un rollback interno, vengono deallocati tutti i cursori dichiarati nel batch contenente l'istruzione di errore. Vengono deallocati tutti i cursori indipendentemente dal tipo o dall'impostazione di CURSOR_CLOSE_ON_COMMIT, tra cui i cursori dichiarati in stored procedure richiamate dal batch di errore. I cursori dichiarati in un batch precedentemente all'errore sul batch sono soggetti alle regole 1 e 2. Un errore di deadlock è un esempio di questo tipo di errore. Questo tipo di errore inoltre viene generato automaticamente da un'istruzione ROLLBACK eseguita in un trigger.  
  
## <a name="locking-behavior"></a>Comportamento di blocco  
 Un'istruzione ROLLBACK TRANSACTION che specifica un argomento *savepoint_name* rilascia tutti i blocchi acquisiti oltre il punto di salvataggio, ad eccezione di escalation e conversioni. Tali blocchi non vengono rilasciati né riconvertiti alla loro precedente modalità di blocco.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** .  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene illustrato l'effetto dell'esecuzione del rollback di una transazione denominata. Dopo aver creato una tabella, le istruzioni seguenti determinano l'avvio di una transazione denominata, l'inserimento di due righe e quindi il rollback della transazione denominata nella variabile @TransactionName. Un'altra istruzione all'esterno della transazione denominata inserisce due righe. La query restituisce i risultati delle istruzioni precedenti.   
  
```sql    
USE tempdb;  
GO  
CREATE TABLE ValueTable ([value] INT);  
GO  
  
DECLARE @TransactionName VARCHAR(20) = 'Transaction1';  
  
BEGIN TRAN @TransactionName  
       INSERT INTO ValueTable VALUES(1), (2);  
ROLLBACK TRAN @TransactionName;  
  
INSERT INTO ValueTable VALUES(3),(4);  
  
SELECT [value] FROM ValueTable;  
  
DROP TABLE ValueTable;  
```  
[!INCLUDE[ssresult-md](../../includes/ssresult-md.md)]  
```  
value  
-----   
3    
4  
```  
  
  
## <a name="see-also"></a>Vedere anche  
 [BEGIN DISTRIBUTED TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md)   
 [BEGIN TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-transaction-transact-sql.md)   
 [COMMIT TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-transaction-transact-sql.md)   
 [COMMIT WORK &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-work-transact-sql.md)   
 [ROLLBACK WORK &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-work-transact-sql.md)   
 [SAVE TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/save-transaction-transact-sql.md)  
  
  
