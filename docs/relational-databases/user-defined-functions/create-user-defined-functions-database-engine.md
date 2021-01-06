---
description: Creazione di funzioni definite dall'utente (Motore di database)
title: Creare funzioni definite dall'utente (motore di database) | Microsoft Docs
ms.custom: ''
ms.date: 11/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- SCHEMABINDING clause
- schema-bound functions [SQL Server]
- user-defined functions [SQL Server], creating
- CREATE FUNCTION statement
- valid statements [SQL Server]
- UDF
- TVF
ms.assetid: f0d5dd10-73fd-4e05-9177-07f56552bdf7
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3303df6ac71b7a6c92d4a1c65a8b7b4b3229cd6e
ms.sourcegitcommit: 4419e99d77ee2c73f9da1559c7944f7702f2de30
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/23/2020
ms.locfileid: "97743678"
---
# <a name="create-user-defined-functions-database-engine"></a>Creazione di funzioni definite dall'utente (Motore di database)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  In questo argomento viene descritto come creare una funzione definita dall'utente in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usando [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   Non è possibile utilizzare funzioni definite dall'utente per eseguire azioni che modificano lo stato del database.  
  
-   Le funzioni definite dall'utente non possono contenere una clausola `OUTPUT INTO` che ha una tabella come destinazione.  
  
-   Tramite le funzioni definite dall'utente non possono essere restituiti più set di risultati. Utilizzare una stored procedure se è necessario restituire più set di risultati.  
  
-   In una funzione definita dall'utente la gestione degli errori è limitata. Una funzione definita dall'utente non supporta `TRY...CATCH`, `@ERROR` o `RAISERROR`.  
  
-   Tramite le funzioni definite dall'utente non è possibile chiamare una stored procedure normale, bensì una estesa.  
  
-   Nelle funzioni definite dall'utente non è possibile utilizzare tabelle temporanee o SQL dinamiche. Sono consentite le variabili di tabella.  
  
-   In una funzione definita dall'utente non sono consentite istruzioni `SET`.  
  
-   Non è consentita la clausola `FOR XML`.  
  
-   È possibile nidificare le funzioni definite dall'utente, ovvero una funzione definita dall'utente ne può richiamare un'altra. Il livello di nidificazione aumenta all'avvio della funzione richiamata e diminuisce al termine dell'esecuzione della funzione. Le funzioni definite dall'utente possono essere nidificate fino a un massimo di 32 livelli. Se viene superato il livello massimo di nidificazioni, l'intera sequenza di funzioni chiamanti ha esito negativo. Qualsiasi riferimento al codice gestito presente in una funzione definita dall'utente Transact-SQL viene considerato come un livello nel contesto del limite di 32 livelli di nidificazione. I metodi richiamati da codice gestito non vengono inclusi nel conteggio per questo limite.  
  
-   Nella definizione di una funzione **definita dall'utente** non è possibile includere[!INCLUDE[tsql](../../includes/tsql-md.md)] le istruzioni di Service Broker seguenti:  
  
    -   `BEGIN DIALOG CONVERSATION`  
  
    -   `END CONVERSATION`  
  
    -   `GET CONVERSATION GROUP`  
  
    -   `MOVE CONVERSATION`  
  
    -   `RECEIVE`  
  
    -   `SEND`  
  
###  <a name="permissions"></a><a name="Security"></a> Autorizzazioni 

È necessario disporre dell'autorizzazione `CREATE FUNCTION` nel database e dell'autorizzazione `ALTER` per lo schema in cui la funzione è in fase di creazione. Se per la funzione viene specificato un tipo definito dall'utente, è necessario disporre dell'autorizzazione `EXECUTE` per tale tipo.  
  
##  <a name="scalar-functions"></a><a name="Scalar"></a> Funzioni scalari  
 Nell'esempio seguente viene creata una **funzione scalare** con istruzioni multiple nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. La funzione accetta un valore di input, un valore `ProductID`e restituisce un singolo valore di dati, la quantità aggregata del prodotto specificato nelle scorte.  
  
```sql  
IF OBJECT_ID (N'dbo.ufnGetInventoryStock', N'FN') IS NOT NULL  
    DROP FUNCTION ufnGetInventoryStock;  
GO  
CREATE FUNCTION dbo.ufnGetInventoryStock(@ProductID int)  
RETURNS int   
AS   
-- Returns the stock level for the product.  
BEGIN  
    DECLARE @ret int;  
    SELECT @ret = SUM(p.Quantity)   
    FROM Production.ProductInventory p   
    WHERE p.ProductID = @ProductID   
        AND p.LocationID = '6';  
     IF (@ret IS NULL)   
        SET @ret = 0;  
    RETURN @ret;  
END; 
```  
  
 Nell'esempio seguente viene utilizzata la funzione `ufnGetInventoryStock` per conoscere la quantità di scorte corrente dei prodotti il cui valore `ProductModelID` è compreso tra 75 e 80.  
  
```sql  
SELECT ProductModelID, Name, dbo.ufnGetInventoryStock(ProductID)AS CurrentSupply  
FROM Production.Product  
WHERE ProductModelID BETWEEN 75 and 80;  
```  

> [!NOTE]  
> Per altre informazioni ed esempi di funzioni scalari, vedere [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md). 

##  <a name="table-valued-functions"></a><a name="TVF"></a> Funzioni con valori di tabella  
Nell'esempio seguente viene creata una **funzione inline con valori di tabella** nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. La funzione accetta un parametro di input, un ID (punto vendita) cliente e restituisce le colonne `ProductID`, `Name`e l'aggregazione delle vendite per l'anno in corso come valore `YTD Total` per ogni prodotto venduto al punto vendita.  
  
```sql  
IF OBJECT_ID (N'Sales.ufn_SalesByStore', N'IF') IS NOT NULL  
    DROP FUNCTION Sales.ufn_SalesByStore;  
GO  
CREATE FUNCTION Sales.ufn_SalesByStore (@storeid int)  
RETURNS TABLE  
AS  
RETURN   
(  
    SELECT P.ProductID, P.Name, SUM(SD.LineTotal) AS 'Total'  
    FROM Production.Product AS P   
    JOIN Sales.SalesOrderDetail AS SD ON SD.ProductID = P.ProductID  
    JOIN Sales.SalesOrderHeader AS SH ON SH.SalesOrderID = SD.SalesOrderID  
    JOIN Sales.Customer AS C ON SH.CustomerID = C.CustomerID  
    WHERE C.StoreID = @storeid  
    GROUP BY P.ProductID, P.Name  
);  
```  
  
Nell'esempio seguente viene richiamata la funzione e viene specificato l'ID cliente 602.  
  
```sql  
SELECT * FROM Sales.ufn_SalesByStore (602);  
```  
  
Nell'esempio seguente viene creata una **funzione con valori di tabella con istruzioni multiple** nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Nella funzione viene accettato un solo parametro di input, un valore `EmployeeID` , e tramite essa viene restituito un elenco di tutti i dipendenti che fanno riferimento direttamente o indirettamente al dipendente specificato. La funzione viene quindi richiamata specificando l'ID dipendente 109.  
  
```sql  
IF OBJECT_ID (N'dbo.ufn_FindReports', N'TF') IS NOT NULL  
    DROP FUNCTION dbo.ufn_FindReports;  
GO  
CREATE FUNCTION dbo.ufn_FindReports (@InEmpID INTEGER)  
RETURNS @retFindReports TABLE   
(  
    EmployeeID int primary key NOT NULL,  
    FirstName nvarchar(255) NOT NULL,  
    LastName nvarchar(255) NOT NULL,  
    JobTitle nvarchar(50) NOT NULL,  
    RecursionLevel int NOT NULL  
)  
--Returns a result set that lists all the employees who report to the   
--specific employee directly or indirectly.*/  
AS  
BEGIN  
WITH EMP_cte(EmployeeID, OrganizationNode, FirstName, LastName, JobTitle, RecursionLevel) -- CTE name and columns  
    AS (  
        SELECT e.BusinessEntityID, e.OrganizationNode, p.FirstName, p.LastName, e.JobTitle, 0 -- Get the initial list of Employees for Manager n  
        FROM HumanResources.Employee e   
INNER JOIN Person.Person p   
ON p.BusinessEntityID = e.BusinessEntityID  
        WHERE e.BusinessEntityID = @InEmpID  
        UNION ALL  
        SELECT e.BusinessEntityID, e.OrganizationNode, p.FirstName, p.LastName, e.JobTitle, RecursionLevel + 1 -- Join recursive member to anchor  
        FROM HumanResources.Employee e   
            INNER JOIN EMP_cte  
            ON e.OrganizationNode.GetAncestor(1) = EMP_cte.OrganizationNode  
INNER JOIN Person.Person p   
ON p.BusinessEntityID = e.BusinessEntityID  
        )  
-- copy the required columns to the result of the function   
   INSERT @retFindReports  
   SELECT EmployeeID, FirstName, LastName, JobTitle, RecursionLevel  
   FROM EMP_cte   
   RETURN  
END;  
GO  
```  
  
Nell'esempio seguente viene richiamata la funzione e viene specificato l'ID dipendente 1.  
  
```sql  
SELECT EmployeeID, FirstName, LastName, JobTitle, RecursionLevel  
FROM dbo.ufn_FindReports(1);  
```  

> [!NOTE]  
> Per altre informazioni ed esempi sulle funzioni inline con valori di tabella e sulle funzioni con valori di tabella con istruzioni multiple, vedere [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md). 

## <a name="best-practices"></a>Procedure consigliate  
Se una funzione definita dall'utente non viene creata tramite la clausola `SCHEMABINDING`, le modifiche apportate agli oggetti sottostanti possono influire sulla definizione della funzione e generare risultati imprevisti quando viene richiamata. È consigliabile implementare uno dei metodi seguenti per assicurarsi che la funzione non diventi obsoleta in seguito a modifiche degli oggetti sottostanti:  
  
-   Specificare la clausola `WITH SCHEMABINDING` quando si crea la funzione definita dall'utente. In questo modo, gli oggetti a cui si fa riferimento nella definizione della funzione possono essere modificati solo se viene modificata anche la funzione.  
  
-   Eseguire la stored procedure [sp_refreshsqlmodule](../../relational-databases/system-stored-procedures/sp-refreshsqlmodule-transact-sql.md) dopo avere modificato qualsiasi oggetto specificato nella definizione della funzione definita dall'utente.  

Se si crea una funzione definita dall'utente che non accede ai dati, specificare l'opzione `SCHEMABINDING`. In questo modo Query Optimizer non genererà operatori di spool inutili per piani di query che interessano queste funzioni. Per altre informazioni sugli spool, vedere [Guida di riferimento a operatori Showplan logici e fisici](../../relational-databases/showplan-logical-and-physical-operators-reference.md). Per altre informazioni sulla creazione di una funzione associata a uno schema, vedere [Funzioni associate a schema](../../relational-databases/user-defined-functions/user-defined-functions.md#SchemaBound).

La creazione di un join di una funzione con valori di tabella con istruzioni multiple in una clausola `FROM` è possibile, ma può compromettere le prestazioni. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è possibile usare tutte le tecniche ottimizzate con alcune istruzioni che possono essere incluse in una funzione con valori di tabella con istruzioni multiple, comportando un piano di query non ottimale. Per ottenere le migliori prestazioni possibili, utilizzare i join tra le tabelle di base anziché tra le funzioni, quando possibile.  

> [!IMPORTANT]
> Le funzioni con valori di tabella con istruzioni multiple hanno una stima di cardinalità predefinita pari a 100 a partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e pari a 1 nelle versioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] precedenti.    
> A partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], è possibile ottimizzazione un piano di esecuzione che usa le funzioni con valori di tabella con istruzioni multiple per sfruttare l'esecuzione interleaved. In questo modo si usa la cardinalità effettiva anziché i valori euristici specificati in precedenza.     
> Per altre informazioni, vedere [Esecuzione interleaved per funzioni con valori di tabella a più istruzioni](../../relational-databases/performance/intelligent-query-processing.md#interleaved-execution-for-mstvfs).

> [!NOTE]  
> ANSI_WARNINGS non viene applicata quando vengono passati parametri a una stored procedure, una funzione definita dall'utente oppure in caso di dichiarazione e impostazione delle variabili in un'istruzione batch. Se, ad esempio, la variabile viene definita come **char(3)** e quindi impostata su un valore maggiore di tre caratteri, i dati verranno troncati alle dimensioni definite e l'istruzione `INSERT` o `UPDATE` avrà esito positivo.

## <a name="see-also"></a>Vedere anche  
 [Funzioni definite dall'utente](../../relational-databases/user-defined-functions/user-defined-functions.md)     
 [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md)    
 [ALTER FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-function-transact-sql.md)    
 [DROP FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-function-transact-sql.md)     
 [DROP PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-partition-function-transact-sql.md)    
 Altri esempi nella [community](https://www.bing.com/search?q=user%20defined%20function%20%22sql%20server%202016%22%20examples&qs=n&form=QBRE&pq=user%20defined%20function%20%22sql%20server%202016%22%20examples&sc=0-48&sp=-1&sk=&cvid=C3AD337125A840AD9EEFA3AAC36A3712)   
  
