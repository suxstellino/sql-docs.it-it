---
description: Variabili (Transact-SQL)
title: Variabili (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: f372ae86-a003-40af-92de-fa52e3eea13f
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a2d924766608bce14b5db6098d5259d6c4d7f5f3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095917"
---
# <a name="variables-transact-sql"></a>Variabili (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Una variabile Transact-SQL locale è un oggetto che può contenere un solo valore di dati di un tipo specifico. Le variabili vengono in genere utilizzate in batch e script per gli scopi seguenti: 

* Contare o controllare il numero di esecuzioni di un ciclo fungendo da contatori.
* Archiviare un valore di dati che deve essere testato da un'istruzione per il controllo di flusso.
* Salvare un valore di dati che deve essere restituito dal codice restituito di una stored procedure o dal valore restituito da una funzione.

> [!NOTE]
> - Il nome di alcune funzioni di sistema Transact-SQL inizia con due simboli di *chiocciola* (\@\@). Anche se nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le funzioni \@\@ vengono definite variabili globali, le funzioni \@\@ non sono variabili e non hanno lo stesso comportamento delle variabili. Le funzioni \@\@ sono in effetti funzioni di sistema e la relativa sintassi segue le regole previste per le funzioni.
> - Non è possibile usare le variabili in una vista.
> - Le modifiche apportate alle variabili non sono interessate dal rollback di una transazione.

Lo script seguente crea e popola con 26 righe una piccola tabella di prova. Lo script utilizza una variabile per eseguire tre operazioni: 

* Controllare il numero di righe inserite verificando il numero di esecuzioni del ciclo.
* Specificare il valore inserito nella colonna integer.
* Funzionare come parte dell'espressione che genera le lettere da inserire nella colonna di tipo carattere.  

```sql
-- Create the table.
CREATE TABLE TestTable (cola INT, colb CHAR(3));
GO
SET NOCOUNT ON;
GO
-- Declare the variable to be used.
DECLARE @MyCounter INT;

-- Initialize the variable.
SET @MyCounter = 0;

-- Test the variable to see if the loop is finished.
WHILE (@MyCounter < 26)
BEGIN;
   -- Insert a row into the table.
   INSERT INTO TestTable VALUES
       -- Use the variable to provide the integer value
       -- for cola. Also use it to generate a unique letter
       -- for each row. Use the ASCII function to get the
       -- integer value of 'a'. Add @MyCounter. Use CHAR to
       -- convert the sum back to the character @MyCounter
       -- characters after 'a'.
       (@MyCounter,
        CHAR( ( @MyCounter + ASCII('a') ) )
       );
   -- Increment the variable to count this iteration
   -- of the loop.
   SET @MyCounter = @MyCounter + 1;
END;
GO
SET NOCOUNT OFF;
GO
-- View the data.
SELECT cola, colb
FROM TestTable;
GO
DROP TABLE TestTable;
GO
```

## <a name="declaring-a-transact-sql-variable"></a>Dichiarazione di una variabile Transact-SQL
L'istruzione DECLARE inizializza una variabile Transact-SQL tramite l'esecuzione delle seguenti operazioni: 
* Assegnazione di un nome. Il nome deve iniziare con il carattere chiocciola (\@).
* Assegnazione di un tipo di dati definito dall'utente o di sistema, nonché la lunghezza del tipo di dati. Alle variabili numeriche vengono inoltre assegnate precisione e scala. Alle variabili XML è possibile assegnare una raccolta di schemi facoltativa.
* Impostazione del valore su NULL.

Ad esempio, l'istruzione **DECLARE** seguente crea la variabile locale **\@mycounter** con tipo di dati int.  
```sql
DECLARE @MyCounter INT;
```
Per dichiarare più variabili locali, è necessario inserire una virgola dopo la prima variabile locale e quindi specificare il nome della variabile locale successiva con il tipo di dati corrispondente.

Ad esempio, l'istruzione **DECLARE** seguente crea le tre variabili locali **\@LastName**, **\@FirstName** e **\@StateProvince**, quindi le inizializza con il valore NULL:  
```sql
DECLARE @LastName NVARCHAR(30), @FirstName NVARCHAR(20), @StateProvince NCHAR(2);
```

L'ambito di una variabile definisce l'intervallo di istruzioni Transact-SQL in cui è possibile fare riferimento alla variabile. L'ambito inizia in corrispondenza del punto in cui la variabile è stata dichiarata e termina alla fine del batch o della stored procedure che include la dichiarazione. Lo script seguente, ad esempio, genera un errore di sintassi, perché la variabile viene dichiarata in un batch ma vi viene fatto riferimento in un altro batch:  
```sql
USE AdventureWorks2014;
GO
DECLARE @MyVariable INT;
SET @MyVariable = 1;
-- Terminate the batch by using the GO keyword.
GO 
-- @MyVariable has gone out of scope and no longer exists.

-- This SELECT statement generates a syntax error because it is
-- no longer legal to reference @MyVariable.
SELECT BusinessEntityID, NationalIDNumber, JobTitle
FROM HumanResources.Employee
WHERE BusinessEntityID = @MyVariable;
```

Le variabili hanno un ambito locale e sono disponibili unicamente nel batch o nella stored procedure in cui sono state definite. Nell'esempio seguente, l'ambito nidificato creato per l'esecuzione di sp_executesql non può accedere alla variabile dichiarata nell'ambito superiore e pertanto viene restituito un errore.  

```sql
DECLARE @MyVariable INT;
SET @MyVariable = 1;
EXECUTE sp_executesql N'SELECT @MyVariable'; -- this produces an error
```

## <a name="setting-a-value-in-a-transact-sql-variable"></a>Impostazione di un valore in una variabile Transact-SQL

Quando si dichiara una variabile, il valore corrispondente viene impostato su NULL. Per assegnare un valore alla variabile, utilizzare l'istruzione SET. È consigliabile adottare sempre questo metodo. Per assegnare un valore a una variabile, è inoltre possibile fare riferimento alla variabile stessa nell'elenco di selezione di un'istruzione SELECT.

Se si desidera utilizzare l'istruzione SET per assegnare un valore a una variabile, specificare il nome della variabile e il valore da assegnarvi. È consigliabile adottare sempre questo metodo. Nel batch seguente vengono dichiarate due variabili, alle quali vengono quindi assegnati valori, utilizzati successivamente nella clausola `WHERE` di un'istruzione `SELECT`:  

```sql
USE AdventureWorks2014;
GO
-- Declare two variables.
DECLARE @FirstNameVariable NVARCHAR(50),
   @PostalCodeVariable NVARCHAR(15);

-- Set their values.
SET @FirstNameVariable = N'Amy';
SET @PostalCodeVariable = N'BA5 3HX';

-- Use them in the WHERE clause of a SELECT statement.
SELECT LastName, FirstName, JobTitle, City, StateProvinceName, CountryRegionName
FROM HumanResources.vEmployee
WHERE FirstName = @FirstNameVariable
   OR PostalCode = @PostalCodeVariable;
GO
```

Quando si assegna un valore a una variabile tramite un riferimento in un elenco di selezione, è necessario assegnare un valore scalare oppure l'istruzione SELECT deve restituire una sola riga. Ad esempio:  

```sql
USE AdventureWorks2014;
GO
DECLARE @EmpIDVariable INT;

SELECT @EmpIDVariable = MAX(EmployeeID)
FROM HumanResources.Employee;
GO
```

> [!WARNING]
> Se in una singola istruzione SELECT sono presenti più clausole di assegnazione, l'ordine di valutazione delle espressioni non viene garantito. Si noti che gli effetti sono visibili sole se sono presenti riferimenti per le assegnazioni.

Se l'istruzione SELECT restituisce più di una riga e la variabile fa riferimento a un'espressione non scalare, la variabile viene impostata sul valore restituito per l'espressione nell'ultima riga del set di risultati. Ad esempio nel batch seguente **\@EmpIDVariable** viene impostata sul valore **BusinessEntityID** dell'ultima riga restituita, ovvero 1:  

```sql
USE AdventureWorks2014;
GO
DECLARE @EmpIDVariable INT;

SELECT @EmpIDVariable = BusinessEntityID
FROM HumanResources.Employee
ORDER BY BusinessEntityID DESC;

SELECT @EmpIDVariable;
GO
```

## <a name="see-also"></a>Vedere anche  
 [Declare @local_variable](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
 [SET @local_variable](../../t-sql/language-elements/set-local-variable-transact-sql.md)  
 [SELECT @local_variable](../../t-sql/language-elements/select-local-variable-transact-sql.md)  
 [Espressioni &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Operatori composti &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)   
  
  
