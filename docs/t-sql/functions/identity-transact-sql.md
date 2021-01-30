---
description: '&#x40;&#x40;IDENTITY (Transact-SQL)'
title: '@@IDENTITY (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 08/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '@@IDENTITY_TSQL'
- '@@IDENTITY'
dev_langs:
- TSQL
helpviewer_keywords:
- last-inserted identity values
- identity values [SQL Server], last-inserted
- '@@IDENTITY function'
ms.assetid: 912e4485-683c-41c2-97b3-8831c0289ee4
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: a9366f4b63cace1787d2bdb777c9765830b87843
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211459"
---
# <a name="x40x40identity-transact-sql"></a>&#x40;&#x40;IDENTITY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Funzione di sistema che restituisce l'ultimo valore Identity immesso.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
@@IDENTITY  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
 **numeric(38,0)**  
  
## <a name="remarks"></a>Osservazioni  
 Al termine di un'istruzione INSERT, SELECT INTO o di copia bulk, la funzione @@IDENTITY include l'ultimo valore Identity generato dall'istruzione. Se l'istruzione non ha modificato alcuna tabella contenente la colonna Identity, la funzione @@IDENTITY restituisce NULL. Se vengono inserite più righe, con la conseguente generazione di più valori Identity, la funzione @@IDENTITY restituisce l'ultimo valore Identity generato. Se l'istruzione attiva uno o più trigger per l'esecuzione di inserimenti che generano valori Identity, la chiamata della funzione @@IDENTITY subito dopo l'istruzione restituisce l'ultimo valore Identity generato dai trigger. Se un trigger viene attivato dopo un'operazione di inserimento in una tabella che include una colonna Identity e il trigger esegue l'inserimento in un'altra tabella che non include una colonna Identity, la funzione @@IDENTITY restituisce il valore Identity del primo inserimento. Il valore della funzione @@IDENTITY non viene ripristinato su un'impostazione precedente se l'istruzione INSERT, SELECT INTO o l'operazione di copia bulk ha esito negativo o se viene eseguito il rollback della transazione.  
  
 Le istruzioni e le transazioni con esito negativo sono in grado di modificare i dati Identity correnti di una tabella e creare gap nei valori della colonna Identity. Non viene mai eseguito il rollback del valore Identity, anche se non si esegue il commit della transazione che ha tentato l'inserimento del valore nella tabella. Se, ad esempio, un'istruzione INSERT ha esito negativo a causa di una violazione di IGNORE_DUP_KEY, il valore Identity corrente per la tabella viene comunque incrementato.  
  
 @@IDENTITY, SCOPE_IDENTITY e IDENT_CURRENT sono funzioni simili perché restituiscono l'ultimo valore inserito nella colonna IDENTITY di una tabella.  
  
 Le funzioni @@IDENTITY e SCOPE_IDENTITY restituiscono l'ultimo valore Identity generato in una tabella durante la sessione corrente. La funzione SCOPE_IDENTITY tuttavia restituisce il valore solo all'interno dell'ambito corrente, mentre la funzione @@IDENTITY non è limitata a un ambito specifico.  
  
 Per la funzione IDENT_CURRENT non esiste alcuna restrizione di ambito o di sessione. La funzione è limitata tuttavia a una tabella specifica. La funzione IDENT_CURRENT restituisce il valore Identity generato per una tabella specifica in qualsiasi sessione e in qualsiasi ambito. Per altre informazioni, vedere [IDENT_CURRENT &#40;Transact-SQL&#41;](../../t-sql/functions/ident-current-transact-sql.md).  
  
 L'ambito della funzione @@IDENTITY è quello della sessione corrente nel server locale in cui viene eseguita. Questa funzione non può essere applicata a server remoti o collegati. Per ottenere un valore Identity in un server diverso, eseguire una stored procedure nel contesto di un server remoto o collegato specifico in modo che recuperi il valore Identity e lo restituisca alla connessione chiamante nel server locale.  
  
 La replica può influire sul valore di @@IDENTITY poiché viene usato nei trigger di replica e nelle stored procedure. Se la colonna è inclusa in un articolo di replica, @@IDENTITY non costituisce un indicatore affidabile del valore Identity più recente creato dall'utente. È possibile usare la sintassi della funzione SCOPE_IDENTITY() anziché @@IDENTITY. Per altre informazioni, vedere [SCOPE_IDENTITY &#40;Transact-SQL&#41;](../../t-sql/functions/scope-identity-transact-sql.md)  
  
> [!NOTE]  
>  La stored procedure chiamante o l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] deve essere riscritta in modo che usi la funzione `SCOPE_IDENTITY()`. Verrà così restituito l'ultimo valore Identity usato nell'ambito dell'istruzione utente e non il valore Identity nell'ambito del trigger annidato usato dalla replica.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene inserita una riga in una tabella contenente una colonna Identity (`LocationID`) e viene utilizzata la funzione `@@IDENTITY` per visualizzare il valore Identity nella nuova riga.  
  
```sql  
USE AdventureWorks2012;  
GO  
--Display the value of LocationID in the last row in the table.  
SELECT MAX(LocationID) FROM Production.Location;  
GO  
INSERT INTO Production.Location (Name, CostRate, Availability, ModifiedDate)  
VALUES ('Damaged Goods', 5, 2.5, GETDATE());  
GO  
SELECT @@IDENTITY AS 'Identity';  
GO  
--Display the value of LocationID of the newly inserted row.  
SELECT MAX(LocationID) FROM Production.Location;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [IDENT_CURRENT &#40;Transact-SQL&#41;](../../t-sql/functions/ident-current-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [SCOPE_IDENTITY &#40;Transact-SQL&#41;](../../t-sql/functions/scope-identity-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)  
  
  
