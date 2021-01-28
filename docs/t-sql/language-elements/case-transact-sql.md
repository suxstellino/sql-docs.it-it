---
title: CASE (Transact-SQL)
description: Informazioni di riferimento Transact-SQL per l'espressione CASE. L'espressione CASE valuta un elenco di condizioni per restituire risultati specifici.
ms.date: 01/26/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CASE_TSQL
- CASE
dev_langs:
- TSQL
helpviewer_keywords:
- CASE expression [Transact-SQL]
- simple CASE expression
- comparing expressions
- searched CASE expression
ms.assetid: 658039ec-8dc2-4251-bc82-30ea23708cee
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2028219edb60da014419fe6b2f0313874ab6a1ca
ms.sourcegitcommit: 3e2421ae45a8e9fa57fb590a5d1a5566721ea74a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/27/2021
ms.locfileid: "98921414"
---
# <a name="case-transact-sql"></a>CASE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Valuta un elenco di condizioni e restituisce una tra più espressioni di risultato possibili.  
  
 L'espressione CASE ha due formati:  
  
-   L'espressione CASE semplice confronta un'espressione con un set di espressioni semplici per determinare il risultato.  
  
-   L'espressione CASE avanzata valuta un set di espressioni booleane per determinare il risultato.  
  
 Entrambi i formati supportano un argomento facoltativo ELSE.  
  
 L'espressione CASE può essere utilizzata in qualsiasi istruzione o clausola che consenta un'espressione valida. È possibile, ad esempio, utilizzare CASE in istruzioni quali SELECT, UPDATE, DELETE e SET e in clausole quali select_list, IN, WHERE, ORDER BY e HAVING.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Syntax for SQL Server, Azure SQL Database and Azure Synapse Analytics
  
--Simple CASE expression:   
CASE input_expression   
     WHEN when_expression THEN result_expression [ ...n ]   
     [ ELSE else_result_expression ]   
END   

--Searched CASE expression:  
CASE  
     WHEN Boolean_expression THEN result_expression [ ...n ]   
     [ ELSE else_result_expression ]   
END  
```  
  
```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
CASE  
     WHEN when_expression THEN result_expression [ ...n ]   
     [ ELSE else_result_expression ]   
END
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti  
 *input_expression*  
 Espressione valutata quando viene utilizzato il formato CASE semplice. *input_expression* è qualsiasi [espressione](../../t-sql/language-elements/expressions-transact-sql.md) valida.  
  
 WHEN *when_expression*  
 Espressione semplice con cui viene confrontato *input_expression* quando viene usato il formato CASE semplice. *when_expression* è qualsiasi espressione valida. A *input_expression* e a ogni occorrenza di *when_expression* deve essere associato lo stesso tipo di dati o un tipo di dati convertibile in modo implicito.  
  
 THEN *result_expression*  
 Espressione restituita quando *input_expression* uguale a *when_expression* restituisce TRUE o *Boolean_expression* restituisce TRUE. *result_expression* è qualsiasi [espressione](../../t-sql/language-elements/expressions-transact-sql.md) valida.  
  
 ELSE *else_result_expression*  
 Espressione restituita quando nessuna operazione di confronto restituisce TRUE. Se questo argomento viene omesso e nessuna operazione di confronto restituisce TRUE, CASE restituisce NULL. *else_result_expression* è qualsiasi espressione valida. A *else_result_expression* e a ogni occorrenza di *result_expression* deve essere associato lo stesso tipo di dati o un tipo di dati convertibile in modo implicito.  
  
 WHEN *Boolean_expression*  
 Espressione booleana valutata quando viene utilizzato il formato CASE avanzato. *Boolean_expression* è qualsiasi espressione booleana valida.  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
 Restituisce il tipo con precedenza maggiore nel set di tipi in *result_expressions* e l'argomento facoltativo *else_result_expression*. Per altre informazioni, vedere [Precedenza dei tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
  
### <a name="return-values"></a>Valori restituiti  
 **Espressione CASE semplice:**  
  
 L'espressione CASE semplice confronta la prima espressione con l'espressione in ogni clausola WHEN per determinarne l'equivalenza. Se le espressioni sono equivalenti, viene restituita l'espressione nella clausola THEN.  
  
-   Consente solo un controllo di uguaglianza.  
  
-   Nell'ordine specificato, restituisce input_expression = when_expression per ogni clausola WHEN.  
  
-   Restituisce l'argomento *result_expression* del primo confronto *input_expression* = *when_expression* che restituisce TRUE.  
  
-   Se nessun confronto *input_expression* = *when_expression* restituisce TRUE, il [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] restituisce *else_result_expression* se è stata specificata una clausola ELSE. In caso contrario, restituisce un valore Null.  
  
 **Espressione CASE avanzata:**  
  
-   Valuta nell'ordine specificato *Boolean_expression* per ogni clausola WHEN.  
  
-   Restituisce *result_expression* del primo argomento *Boolean_expression* che restituisce TRUE.  
  
-   Se nessun confronto *Boolean_expression* restituisce TRUE, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] restituisce *else_result_expression* se è stata specificata una clausola ELSE. In caso contrario, restituisce un valore Null.  
  
## <a name="remarks"></a>Osservazioni  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consente solo 10 livelli di nidificazione nelle espressioni CASE.  
  
 L'espressione CASE non può essere utilizzata per controllare il flusso di esecuzione di istruzioni, blocchi di istruzioni, funzioni definite dall'utente e stored procedure Transact-SQL. Per un elenco di metodi di controllo di flusso, vedere [Controllo di flusso &#40;Transact-SQL&#41;](~/t-sql/language-elements/control-of-flow.md).  
  
 Con l'espressione CASE è possibile valutare in sequenza le relative condizioni e viene arrestata alla prima condizione soddisfatta. In alcune situazioni, un'espressione viene valutata prima che in un'espressione CASE vengano ricevuti i risultati dell'espressione come input. È possibile che si verifichino errori nella valutazione di queste espressioni. Le espressioni di aggregazione visualizzate negli argomenti WHEN di un'espressione CASE vengono innanzitutto valutate e, successivamente, fornite all'espressione CASE. Ad esempio, nella query seguente si verifica un errore di divisione per zero durante la generazione del valore dell'aggregazione MAX. Questa condizione si presenta prima della valutazione dell'espressione CASE.  
  
```sql  
WITH Data (value) AS   
(   
SELECT 0   
UNION ALL   
SELECT 1   
)   
SELECT   
   CASE   
      WHEN MIN(value) <= 0 THEN 0   
      WHEN MAX(1/value) >= 100 THEN 1   
   END   
FROM Data ;  
```  
  
 È necessario dipendere dall'ordine di valutazione delle condizioni WHEN solo per le espressioni scalari (incluse le sottoquery non correlate tramite cui vengono restituiti scalari), non per le espressioni di aggregazione.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-a-select-statement-with-a-simple-case-expression"></a>R. Utilizzo di un'istruzione SELECT con un'espressione CASE semplice  
 In un'istruzione `SELECT` un'espressione `CASE` semplice consente di eseguire solo un controllo di uguaglianza senza ulteriori confronti. Nell'esempio seguente viene utilizzata l'espressione `CASE` per modificare la visualizzazione delle categorie delle linee di prodotti in modo da renderle più intuitive.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT   ProductNumber, Category =  
      CASE ProductLine  
         WHEN 'R' THEN 'Road'  
         WHEN 'M' THEN 'Mountain'  
         WHEN 'T' THEN 'Touring'  
         WHEN 'S' THEN 'Other sale items'  
         ELSE 'Not for sale'  
      END,  
   Name  
FROM Production.Product  
ORDER BY ProductNumber;  
GO  
```  
  
### <a name="b-using-a-select-statement-with-a-searched-case-expression"></a>B. Utilizzo di un'istruzione SELECT con un'espressione CASE avanzata  
 In un'istruzione `SELECT` l'espressione `CASE` avanzata consente di sostituire valori nel set di risultati in base ai valori di confronto. Nell'esempio seguente viene visualizzato il prezzo di listino come commento di testo in base alla fascia di prezzi per un prodotto.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT   ProductNumber, Name, "Price Range" =   
      CASE   
         WHEN ListPrice =  0 THEN 'Mfg item - not for resale'  
         WHEN ListPrice < 50 THEN 'Under $50'  
         WHEN ListPrice >= 50 and ListPrice < 250 THEN 'Under $250'  
         WHEN ListPrice >= 250 and ListPrice < 1000 THEN 'Under $1000'  
         ELSE 'Over $1000'  
      END  
FROM Production.Product  
ORDER BY ProductNumber ;  
GO  
```  
  
### <a name="c-using-case-in-an-order-by-clause"></a>C. Utilizzo di CASE in una clausola ORDER BY  
 Nell'esempio seguente viene utilizzata l'espressione CASE in una clausola ORDER BY per determinare l'ordinamento delle righe in base al valore di una colonna. Nel primo esempio, viene calcolato il valore nella colonna `SalariedFlag` della tabella `HumanResources.Employee`. I dipendenti per cui `SalariedFlag` è impostato su 1 vengono restituiti in ordine decrescente in base a `BusinessEntityID`. I dipendenti per cui `SalariedFlag` è impostato su 0 vengono restituiti in ordine crescente in base a `BusinessEntityID`. Nel secondo esempio il set di risultati viene ordinato in base alla colonna `TerritoryName` quando la colonna `CountryRegionName` è uguale a 'Stati Uniti' e in base a `CountryRegionName` per tutte le altre righe.  
  
```sql  
SELECT BusinessEntityID, SalariedFlag  
FROM HumanResources.Employee  
ORDER BY CASE SalariedFlag WHEN 1 THEN BusinessEntityID END DESC  
        ,CASE WHEN SalariedFlag = 0 THEN BusinessEntityID END;  
GO    
```  
  
```sql  
SELECT BusinessEntityID, LastName, TerritoryName, CountryRegionName  
FROM Sales.vSalesPerson  
WHERE TerritoryName IS NOT NULL  
ORDER BY CASE CountryRegionName WHEN 'United States' THEN TerritoryName  
         ELSE CountryRegionName END;  
```  
  
### <a name="d-using-case-in-an-update-statement"></a>D. Utilizzo di CASE in un'istruzione UPDATE  
 Nell'esempio seguente viene utilizzata l'espressione CASE in un'istruzione UPDATE per determinare il valore impostato per la colonna `VacationHours` per i dipendenti per cui `SalariedFlag` è impostato su 0. Se sottraendo 10 ore da `VacationHours` viene restituito un valore negativo, `VacationHours` viene aumentato di 40 ore; in caso contrario, `VacationHours` viene aumentato di 20 ore. La clausola OUTPUT viene utilizzata per visualizzare i valori precedenti e successivi alle ferie.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE HumanResources.Employee  
SET VacationHours =   
    ( CASE  
         WHEN ((VacationHours - 10.00) < 0) THEN VacationHours + 40  
         ELSE (VacationHours + 20.00)  
       END  
    )  
OUTPUT Deleted.BusinessEntityID, Deleted.VacationHours AS BeforeValue,   
       Inserted.VacationHours AS AfterValue  
WHERE SalariedFlag = 0;   
```  
  
### <a name="e-using-case-in-a-set-statement"></a>E. Utilizzo di CASE in un'istruzione SET  
 Negli esempi seguenti viene utilizzata l'espressione CASE in un'istruzione SET nella funzione con valori di tabella `dbo.GetContactInfo`. Nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] tutti i dati correlati alle persone vengono archiviati nella tabella `Person.Person`. La persona, ad esempio, può essere un dipendente, un rappresentante del fornitore o un cliente. La funzione restituisce il nome e il cognome di un elemento `BusinessEntityID` specifico e il tipo di contatto per tale persona. L'espressione CASE nell'istruzione SET determina il valore da visualizzare per la colonna `ContactType` in base all'esistenza della colonna `BusinessEntityID` nelle tabelle `Employee`, `Vendor` o `Customer`.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE FUNCTION dbo.GetContactInformation(@BusinessEntityID INT)  
    RETURNS @retContactInformation TABLE   
(  
BusinessEntityID INT NOT NULL,  
FirstName NVARCHAR(50) NULL,  
LastName NVARCHAR(50) NULL,  
ContactType NVARCHAR(50) NULL,  
    PRIMARY KEY CLUSTERED (BusinessEntityID ASC)  
)   
AS   
-- Returns the first name, last name and contact type for the specified contact.  
BEGIN  
    DECLARE   
        @FirstName NVARCHAR(50),   
        @LastName NVARCHAR(50),   
        @ContactType NVARCHAR(50);  
  
    -- Get common contact information  
    SELECT   
        @BusinessEntityID = BusinessEntityID,   
@FirstName = FirstName,   
        @LastName = LastName  
    FROM Person.Person   
    WHERE BusinessEntityID = @BusinessEntityID;  
  
    SET @ContactType =   
        CASE   
            -- Check for employee  
            WHEN EXISTS(SELECT * FROM HumanResources.Employee AS e   
                WHERE e.BusinessEntityID = @BusinessEntityID)   
                THEN 'Employee'  
  
            -- Check for vendor  
            WHEN EXISTS(SELECT * FROM Person.BusinessEntityContact AS bec  
                WHERE bec.BusinessEntityID = @BusinessEntityID)   
                THEN 'Vendor'  
  
            -- Check for store  
            WHEN EXISTS(SELECT * FROM Purchasing.Vendor AS v            
                WHERE v.BusinessEntityID = @BusinessEntityID)   
                THEN 'Store Contact'  
  
            -- Check for individual consumer  
            WHEN EXISTS(SELECT * FROM Sales.Customer AS c   
                WHERE c.PersonID = @BusinessEntityID)   
                THEN 'Consumer'  
        END;  
  
    -- Return the information to the caller  
    IF @BusinessEntityID IS NOT NULL   
    BEGIN  
        INSERT @retContactInformation  
        SELECT @BusinessEntityID, @FirstName, @LastName, @ContactType;  
    END;  
  
    RETURN;  
END;  
GO  
  
SELECT BusinessEntityID, FirstName, LastName, ContactType  
FROM dbo.GetContactInformation(2200);  
GO  
SELECT BusinessEntityID, FirstName, LastName, ContactType  
FROM dbo.GetContactInformation(5);  
```  
  
### <a name="f-using-case-in-a-having-clause"></a>F. Utilizzo di CASE in una clausola HAVING  
 Nell'esempio seguente viene utilizzata l'espressione CASE in una clausola HAVING per limitare le righe restituite dall'istruzione SELECT. L'istruzione restituisce la retribuzione oraria massima per ogni titolo professionale nella tabella `HumanResources.Employee`. La clausola HAVING limita i titoli professionali a quelli associati a uomini con una retribuzione massima maggiore di 40 dollari o a donne con una retribuzione massima maggiore di 42 dollari.  
  
```sql 
USE AdventureWorks2012;  
GO  
SELECT JobTitle, MAX(ph1.Rate)AS MaximumRate  
FROM HumanResources.Employee AS e  
JOIN HumanResources.EmployeePayHistory AS ph1 ON e.BusinessEntityID = ph1.BusinessEntityID  
GROUP BY JobTitle  
HAVING (MAX(CASE WHEN Gender = 'M'   
        THEN ph1.Rate   
        ELSE NULL END) > 40.00  
     OR MAX(CASE WHEN Gender  = 'F'   
        THEN ph1.Rate    
        ELSE NULL END) > 42.00)  
ORDER BY MaximumRate DESC;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="g-using-a-select-statement-with-a-case-expression"></a>G. Uso di un'istruzione SELECT con un'espressione CASE  
 In un'istruzione SELECT l'espressione CASE avanzata consente di sostituire valori nel set di risultati in base ai valori di confronto. Nell'esempio seguente viene usata l'espressione CASE per modificare la visualizzazione delle categorie delle linee di prodotti in modo da renderle più intuitive. Se un valore non esiste, viene visualizzato il testo "Not for sale".  
  
```sql 
-- Uses AdventureWorks  
  
SELECT   ProductAlternateKey, Category =  
      CASE ProductLine  
         WHEN 'R' THEN 'Road'  
         WHEN 'M' THEN 'Mountain'  
         WHEN 'T' THEN 'Touring'  
         WHEN 'S' THEN 'Other sale items'  
         ELSE 'Not for sale'  
      END,  
   EnglishProductName  
FROM dbo.DimProduct  
ORDER BY ProductKey;  
```  
  
### <a name="h-using-case-in-an-update-statement"></a>H. Utilizzo di CASE in un'istruzione UPDATE  
 Nell'esempio seguente viene utilizzata l'espressione CASE in un'istruzione UPDATE per determinare il valore impostato per la colonna `VacationHours` per i dipendenti per cui `SalariedFlag` è impostato su 0. Se sottraendo 10 ore da `VacationHours` viene restituito un valore negativo, `VacationHours` viene aumentato di 40 ore; in caso contrario, `VacationHours` viene aumentato di 20 ore.  
  
```sql 
-- Uses AdventureWorks   
  
UPDATE dbo.DimEmployee  
SET VacationHours =   
    ( CASE  
         WHEN ((VacationHours - 10.00) < 0) THEN VacationHours + 40  
         ELSE (VacationHours + 20.00)   
       END  
    )   
WHERE SalariedFlag = 0;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Espressioni &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [COALESCE &#40;Transact-SQL&#41;](../../t-sql/language-elements/coalesce-transact-sql.md)   
 [IIF &#40;Transact-SQL&#41;](../../t-sql/functions/logical-functions-iif-transact-sql.md)   
 [CHOOSE &#40;Transact-SQL&#41;](../../t-sql/functions/logical-functions-choose-transact-sql.md)  
  
  



