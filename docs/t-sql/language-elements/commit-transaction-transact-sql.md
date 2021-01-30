---
description: COMMIT TRANSACTION (Transact-SQL)
title: COMMIT TRANSACTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/09/2016
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- COMMIT
- COMMIT TRANSACTION
- COMMIT_TSQL
- COMMIT_TRANSACTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ending transactions [SQL Server]
- user-defined transactions [SQL Server]
- committed transactions
- transactions [SQL Server], ending
- marking end of transactions [SQL Server]
- implicit transactions
- distributed transactions [SQL Server], committed
- transactions [SQL Server], committed
- COMMIT TRANSACTION statement
- rolling back transactions, COMMIT TRANSACTION
ms.assetid: f8fe26a9-7911-497e-b348-4e69c7435dc1
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b3f3997b0d5dfec86b9a12dc9645a0315e40253e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208246"
---
# <a name="commit-transaction-transact-sql"></a>COMMIT TRANSACTION (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contrassegna la fine di una transazione esplicita o implicita completata correttamente. Se il valore di @@TRANCOUNT è 1, COMMIT TRANSACTION rende permanenti nel database tutte le modifiche apportate ai dati dall'inizio della transazione, libera le risorse della transazione e decrementa il valore di @@TRANCOUNT a 0. Se il valore di @@TRANCOUNT è superiore a 1, COMMIT TRANSACTION decrementa @@TRANCOUNT solo di 1 e la transazione rimane attiva.  
  
 ![Icona di collegamento a un articolo](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un articolo") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Applies to SQL Server (starting with 2008) and Azure SQL Database
  
COMMIT [ { TRAN | TRANSACTION }  [ transaction_name | @tran_name_variable ] ] [ WITH ( DELAYED_DURABILITY = { OFF | ON } ) ]  
[ ; ]  
```  
 
```syntaxsql
-- Applies to Azure Synapse Analytics and Parallel Data Warehouse Database
  
COMMIT [ TRAN | TRANSACTION ] 
[ ; ]  
``` 
 
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *transaction_name*  
 **SI APPLICA A:** SQL Server e database SQL di Azure
 
 Ignorato dal [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. *transaction_name* consente di specificare il nome di una transazione assegnato da un'istruzione BEGIN TRANSACTION precedente. *transaction_name* deve essere conforme alle regole relative agli identificatori, ma non può superare i 32 caratteri. *transaction_name* indica ai programmatori quale istruzione BEGIN TRANSACTION annidata è associata a COMMIT TRANSACTION.  
  
 *\@tran_name_variable*  
 **SI APPLICA A:** SQL Server e database SQL di Azure  
 
Nome di una variabile definita dall'utente contenente un nome di transazione valido. La variabile deve essere dichiarata con un tipo di dati char, varchar, nchar o nvarchar. Se vengono passati più di 32 caratteri alla variabile vengono utilizzati solo i primi 32 caratteri. Quelli rimanenti vengono troncati.  
  
 DELAYED_DURABILITY  
 **SI APPLICA A:** SQL Server e database SQL di Azure   

 Opzione che richiede il commit della transazione con durabilità ritardata. La richiesta viene ignorata se il database è stato modificato con `DELAYED_DURABILITY = DISABLED` o `DELAYED_DURABILITY = FORCED`. Per altre informazioni, vedere [Controllo della durabilità delle transazioni](../../relational-databases/logs/control-transaction-durability.md).  
  
## <a name="remarks"></a>Commenti  
 È compito del programmatore [!INCLUDE[tsql](../../includes/tsql-md.md)] eseguire COMMIT TRANSACTION solo quando tutti i dati a cui la transazione fa riferimento sono logicamente corretti.  
  
 Se la transazione di cui è stato eseguito il commit è una transazione distribuita di [!INCLUDE[tsql](../../includes/tsql-md.md)], l'istruzione COMMIT TRANSACTION attiva l'utilizzo di un protocollo di commit in due fasi in MS DTC per il commit di tutti i server coinvolti nella transazione. Quando una transazione locale include due o più database nella stessa istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)], l'istanza esegue il commit di tutti i database coinvolti nella transazione usando un commit in due fasi interno.  
  
 Se usati in transazioni annidate, i commit delle transazioni interne non liberano le risorse o non rendono permanenti le modifiche. Queste operazioni vengono eseguite solo durante il commit delle transazioni esterne. Ogni istruzione COMMIT TRANSACTION eseguita quando il valore di @@TRANCOUNT è superiore a uno decrementa semplicemente @@TRANCOUNT di 1. Quando il valore di @@TRANCOUNT risulta uguale a 0, viene eseguito il commit dell'intera transazione esterna. Poiché *transaction_name* viene ignorato dal [!INCLUDE[ssDE](../../includes/ssde-md.md)], l'esecuzione di COMMIT TRANSACTION con riferimento al nome di una transazione esterna quando esistono transazioni interne in attesa comporta la riduzione del valore di @@TRANCOUNT di una unità.  
  
 L'esecuzione di COMMIT TRANSACTION quando @@TRANCOUNT è zero genera un errore. Non esiste infatti alcuna istruzione BEGIN TRANSACTION corrispondente.  
  
 Non è possibile eseguire il rollback di una transazione dopo l'esecuzione di un'istruzione COMMIT TRANSACTION, perché le modifiche dei dati sono diventate permanenti nel database.  
  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] incrementa il conteggio delle transazioni all'interno di un'istruzione solo quando il numero di transazioni all'inizio dell'istruzione è uguale a 0.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** .  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-committing-a-transaction"></a>R. Esecuzione del commit di una transazione  
**SI APPLICA A:** SQL Server, database SQL di Azure, [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]   

Nell'esempio seguente viene eliminato un candidato di processo. Viene usato AdventureWorks. 
  
```sql   
BEGIN TRANSACTION;   
DELETE FROM HumanResources.JobCandidate  
    WHERE JobCandidateID = 13;   
COMMIT TRANSACTION;   
```  
  
### <a name="b-committing-a-nested-transaction"></a>B. Esecuzione del commit di una transazione nidificata  
**SI APPLICA A:** SQL Server e database SQL di Azure    

Nell'esempio seguente viene creata una tabella, viene generata una transazione nidificata su tre livelli e viene quindi eseguito il commit della transazione nidificata. Nonostante ogni istruzione `COMMIT TRANSACTION` includa un parametro *transaction_name*, non esiste alcuna relazione tra le istruzioni `COMMIT TRANSACTION` e `BEGIN TRANSACTION`. I parametri *transaction_name* consentono al programmatore di verificare che venga codificato il numero corretto di commit per il decremento di `@@TRANCOUNT` a 0 e il conseguenze commit della transazione esterna. 
  
```sql   
IF OBJECT_ID(N'TestTran',N'U') IS NOT NULL  
    DROP TABLE TestTran;  
GO  
CREATE TABLE TestTran (Cola INT PRIMARY KEY, Colb CHAR(3));  
GO  
-- This statement sets @@TRANCOUNT to 1.  
BEGIN TRANSACTION OuterTran;  
  
PRINT N'Transaction count after BEGIN OuterTran = '  
    + CAST(@@TRANCOUNT AS NVARCHAR(10));  
 
INSERT INTO TestTran VALUES (1, 'aaa');  
 
-- This statement sets @@TRANCOUNT to 2.  
BEGIN TRANSACTION Inner1;  
 
PRINT N'Transaction count after BEGIN Inner1 = '  
    + CAST(@@TRANCOUNT AS NVARCHAR(10));  
  
INSERT INTO TestTran VALUES (2, 'bbb');  
  
-- This statement sets @@TRANCOUNT to 3.  
BEGIN TRANSACTION Inner2;  
  
PRINT N'Transaction count after BEGIN Inner2 = '  
    + CAST(@@TRANCOUNT AS NVARCHAR(10));  
  
INSERT INTO TestTran VALUES (3, 'ccc');  
  
-- This statement decrements @@TRANCOUNT to 2.  
-- Nothing is committed.  
COMMIT TRANSACTION Inner2;  
 
PRINT N'Transaction count after COMMIT Inner2 = '  
    + CAST(@@TRANCOUNT AS NVARCHAR(10));  
 
-- This statement decrements @@TRANCOUNT to 1.  
-- Nothing is committed.  
COMMIT TRANSACTION Inner1;  
 
PRINT N'Transaction count after COMMIT Inner1 = '  
    + CAST(@@TRANCOUNT AS NVARCHAR(10));  
  
-- This statement decrements @@TRANCOUNT to 0 and  
-- commits outer transaction OuterTran.  
COMMIT TRANSACTION OuterTran;  
  
PRINT N'Transaction count after COMMIT OuterTran = '  
    + CAST(@@TRANCOUNT AS NVARCHAR(10));  
```  
  
## <a name="see-also"></a>Vedere anche  
 [BEGIN DISTRIBUTED TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md)   
 [BEGIN TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-transaction-transact-sql.md)   
 [COMMIT WORK &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-work-transact-sql.md)   
 [ROLLBACK TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-transaction-transact-sql.md)   
 [ROLLBACK WORK &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-work-transact-sql.md)   
 [SAVE TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/save-transaction-transact-sql.md)   
 [@@TRANCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/trancount-transact-sql.md)  
  
