---
description: FETCH (Transact-SQL)
title: FETCH (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FETCH
- FETCH_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- FETCH statement
- cursors [SQL Server], fetching
- Transact-SQL cursors, fetching and scrolling
- retrieving rows
- fetching [SQL Server]
- SCROLL option
- row fetching [SQL Server]
ms.assetid: 5d68dac2-f91b-4342-bb4e-209ee132665f
author: cawrites
ms.author: chadam
ms.openlocfilehash: da86d5cdd7889383c370f9eb9a8a6b6322100c9d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99115315"
---
# <a name="fetch-transact-sql"></a>FETCH (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Recupera una riga specifica da un cursore server [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
FETCH   
          [ [ NEXT | PRIOR | FIRST | LAST   
                    | ABSOLUTE { n | @nvar }   
                    | RELATIVE { n | @nvar }   
               ]   
               FROM   
          ]   
{ { [ GLOBAL ] cursor_name } | @cursor_variable_name }   
[ INTO @variable_name [ ,...n ] ]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 NEXT  
 Restituisce la riga del set di risultati successiva alla riga corrente e imposta la riga corrente sulla riga restituita. Se `FETCH NEXT` è la prima operazione di recupero eseguita con un cursore, viene restituita la prima riga del set di risultati. `NEXT` è l'opzione di recupero predefinita del cursore.  
  
 PRIOR  
 Restituisce la riga del set di risultati precedente alla riga corrente e imposta la riga corrente sulla riga restituita. Se `FETCH PRIOR` è la prima operazione di recupero eseguita con un cursore, non viene restituita alcuna riga e il cursore rimane nella posizione che precede la prima riga.  
  
 FIRST  
 Restituisce la prima riga nel cursore, che diventa la riga corrente.  
  
 LAST  
 Restituisce l'ultima riga nel cursore, che diventa la riga corrente.  
  
 ABSOLUTE { *n*| \@*nvar*}  
 Se *n* o \@*nvar* è un valore positivo, restituisce la riga che si trova a *n* righe dall'inizio del cursore e imposta la riga restituita come nuova riga corrente. Se *n* o \@*nvar* è un valore negativo, restituisce la riga che si trova *n* righe prima della fine del cursore e imposta la riga restituita come nuova riga corrente. Se *n* o \@*nvar* è 0, non vengono restituite righe. *n* deve essere una costante di tipo integer e \@*nvar* deve essere di tipo **smallint**, **tinyint** o **int**.  
  
 RELATIVE { *n*| \@*nvar*}  
 Se *n* o \@*nvar* è un valore positivo, restituisce la riga che si trova *n* righe oltre la riga corrente e imposta la riga restituita come nuova riga corrente. Se *n* o \@*nvar* è un valore negativo, restituisce la riga che si trova *n* righe prima della riga corrente e imposta la riga restituita come nuova riga corrente. Se *n* o \@*nvar* è 0, restituisce la riga corrente. Se si specifica `FETCH RELATIVE` con *n* o \@*nvar* impostato su un numero negativo o su 0 per la prima operazione di recupero eseguita in un cursore, non viene restituita alcuna riga. *n* deve essere una costante di tipo integer e \@*nvar* deve essere di tipo **smallint**, **tinyint** o **int**.  
  
 GLOBAL  
 Specifica che *cursor_name* fa riferimento a un cursore globale.  
  
 *cursor_name*  
 Nome del cursore aperto dal quale deve essere eseguita l'operazione di recupero. Se esistono sia un cursore globale che un cursore locale con nome *cursor_name*, *cursor_name* indica il cursore globale se la parola chiave GLOBAL viene specificata e il cursore locale se la parola chiave GLOBAL viene omessa.  
  
 \@*cursor_variable_name*  
 Nome di una variabile di cursore che fa riferimento al cursore da cui deve essere eseguita l'operazione di recupero.  
  
 INTO \@*variable_name*[ ,...*n*]  
 Consente di inserire in variabili locali i dati delle colonne ottenute da un'operazione di recupero. Ogni variabile dell'elenco, da sinistra a destra, è associata alla colonna corrispondente nel set di risultati del cursore. A ogni variabile deve essere associato lo stesso tipo di dati o un tipo di dati che supporti la conversione implicita dal tipo di dati della colonna corrispondente nel set di risultati. Il numero di variabili deve corrispondere al numero di colonne dell'elenco di selezione del cursore.  
  
## <a name="remarks"></a>Osservazioni  
 Se l'opzione `SCROLL` non è specificata in un'istruzione `DECLARE CURSOR` in formato ISO, `NEXT` è l'unica opzione `FETCH` supportata. Se `SCROLL` viene specificata in formato ISO in un'istruzione `DECLARE CURSOR`, tutte le opzioni `FETCH` sono supportate.  
  
 Quando vengono utilizzate le estensioni del cursore DECLARE di [!INCLUDE[tsql](../../includes/tsql-md.md)], vengono applicate le regole seguenti:  
  
-   Se `FORWARD_ONLY` o `FAST_FORWARD` è specificata, `NEXT` è l'unica opzione `FETCH` supportata.  
  
-   Se `DYNAMIC`, `FORWARD_ONLY` o `FAST_FORWARD` non è specificata e una tra `KEYSET`, `STATIC` o `SCROLL` è specificata, tutti le opzioni `FETCH` sono supportate.  
  
-   I cursori `DYNAMIC SCROLL` supportano tutte le opzioni `FETCH` ad eccezione di `ABSOLUTE`.  
  
 La funzione `@@FETCH_STATUS` restituisce lo stato dell'ultima istruzione `FETCH`. Le stesse informazioni vengono registrate nella colonna fetch_status del cursore restituito dalla stored procedure sp_describe_cursor. In base a queste informazioni sullo stato, è necessario determinare la validità dei dati restituiti da un'istruzione `FETCH` prima di eseguire qualsiasi operazione con i dati. Per altre informazioni, vedere [@@FETCH_STATUS &#40;Transact-SQL&#41;](../../t-sql/functions/fetch-status-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni per l'istruzione `FETCH` vengono assegnate per impostazione predefinita a qualsiasi utente valido.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-fetch-in-a-simple-cursor"></a>R. Utilizzo dell'istruzione FETCH in un cursore semplice  
 Nell'esempio seguente viene dichiarato un cursore semplice per le righe della tabella `Person.Person` contenenti un cognome che inizia per `B`. Viene inoltre utilizzata l'istruzione `FETCH NEXT` per la riproduzione delle varie righe. Le istruzioni `FETCH` restituiscono il valore delle colonne specificate nell'istruzione `DECLARE CURSOR`, come set di risultati a riga singola.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE contact_cursor CURSOR FOR  
SELECT LastName FROM Person.Person  
WHERE LastName LIKE 'B%'  
ORDER BY LastName;  
  
OPEN contact_cursor;  
  
-- Perform the first fetch.  
FETCH NEXT FROM contact_cursor;  
  
-- Check @@FETCH_STATUS to see if there are any more rows to fetch.  
WHILE @@FETCH_STATUS = 0  
BEGIN  
   -- This is executed as long as the previous fetch succeeds.  
   FETCH NEXT FROM contact_cursor;  
END  
  
CLOSE contact_cursor;  
DEALLOCATE contact_cursor;  
GO  
```  
  
### <a name="b-using-fetch-to-store-values-in-variables"></a>B. Utilizzo dell'istruzione FETCH per archiviare valori nelle variabili  
 L'esempio seguente è simile all'esempio A, con la sola differenza che il risultato delle istruzioni `FETCH` viene archiviato in variabili locali anziché essere restituito direttamente al client. L'istruzione `PRINT` combina le variabili in un'unica stringa e le restituisce al client.  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Declare the variables to store the values returned by FETCH.  
DECLARE @LastName VARCHAR(50), @FirstName VARCHAR(50);  
  
DECLARE contact_cursor CURSOR FOR  
SELECT LastName, FirstName FROM Person.Person  
WHERE LastName LIKE 'B%'  
ORDER BY LastName, FirstName;  
  
OPEN contact_cursor;  
  
-- Perform the first fetch and store the values in variables.  
-- Note: The variables are in the same order as the columns  
-- in the SELECT statement.   
  
FETCH NEXT FROM contact_cursor  
INTO @LastName, @FirstName;  
  
-- Check @@FETCH_STATUS to see if there are any more rows to fetch.  
WHILE @@FETCH_STATUS = 0  
BEGIN  
  
   -- Concatenate and display the current values in the variables.  
   PRINT 'Contact Name: ' + @FirstName + ' ' +  @LastName  
  
   -- This is executed as long as the previous fetch succeeds.  
   FETCH NEXT FROM contact_cursor  
   INTO @LastName, @FirstName;  
END  
  
CLOSE contact_cursor;  
DEALLOCATE contact_cursor;  
GO  
```  
  
### <a name="c-declaring-a-scroll-cursor-and-using-the-other-fetch-options"></a>C. Dichiarazione di un cursore SCROLL e utilizzo delle altre opzioni FETCH  
 Nell'esempio seguente viene creato un cursore `SCROLL` per supportare funzioni di scorrimento complete attraverso le opzioni `LAST`, `PRIOR`, `RELATIVE` e `ABSOLUTE`.  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Execute the SELECT statement alone to show the   
-- full result set that is used by the cursor.  
SELECT LastName, FirstName FROM Person.Person  
ORDER BY LastName, FirstName;  
  
-- Declare the cursor.  
DECLARE contact_cursor SCROLL CURSOR FOR  
SELECT LastName, FirstName FROM Person.Person  
ORDER BY LastName, FirstName;  
  
OPEN contact_cursor;  
  
-- Fetch the last row in the cursor.  
FETCH LAST FROM contact_cursor;  
  
-- Fetch the row immediately prior to the current row in the cursor.  
FETCH PRIOR FROM contact_cursor;  
  
-- Fetch the second row in the cursor.  
FETCH ABSOLUTE 2 FROM contact_cursor;  
  
-- Fetch the row that is three rows after the current row.  
FETCH RELATIVE 3 FROM contact_cursor;  
  
-- Fetch the row that is two rows prior to the current row.  
FETCH RELATIVE -2 FROM contact_cursor;  
  
CLOSE contact_cursor;  
DEALLOCATE contact_cursor;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CLOSE &#40;Transact-SQL&#41;](../../t-sql/language-elements/close-transact-sql.md)   
 [DEALLOCATE &#40;Transact-SQL&#41;](../../t-sql/language-elements/deallocate-transact-sql.md)   
 [DECLARE CURSOR &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-cursor-transact-sql.md)   
 [OPEN &#40;Transact-SQL&#41;](../../t-sql/language-elements/open-transact-sql.md)  
  
  
