---
title: Specificare i parametri | Microsoft Docs
description: Informazioni sul passaggio dei valori nei parametri e sulla modalità d'uso di ognuno degli attributi di parametro durante una chiamata alla procedura.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.technology: stored-procedures
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- parameters [SQL Server], stored procedures
- stored procedures [SQL Server], parameters
- output parameters [SQL Server]
- input parameters [SQL Server]
ms.assetid: 902314fe-5f9c-4d0d-a0b7-27e67c9c70ec
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 476741da7da2e0e463a5a5a0abf4ebf0a8e70d40
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467082"
---
# <a name="specify-parameters"></a>Specificare i parametri
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Se si specificano parametri di procedura, i programmi chiamanti sono in grado di passare i valori nel corpo della procedura. Tali valori possono essere utilizzati per diversi scopi durante l'esecuzione della procedura. Inoltre, i parametri di procedura possono restituire valori al programma chiamante se il parametro è contrassegnato come parametro OUTPUT.  
  
 Una procedura può disporre al massimo di 2100 parametri, a ciascuno dei quali vengono assegnati un nome, un tipo di dati e una direzione. Facoltativamente, ai parametri possono essere assegnati i valori predefiniti.  
  
 Nella sezione seguente vengono fornite informazioni sul passaggio dei valori nei parametri e sulla modalità di utilizzo di ognuno degli attributi di parametro durante una chiamata alla procedura.  
  
## <a name="passing-values-into-parameters"></a>Passaggio dei valori nei parametri  
 I valori dei parametri forniti con una chiamata alla procedura devono essere costanti o una variabile. Non è possibile utilizzare un nome di funzione come valore di parametro. Le variabili possono essere definite dall'utente oppure di sistema, ad esempio \@\@spid.  
  
 Negli esempi seguenti viene illustrato il passaggio dei valori dei parametri alla procedura `uspGetWhereUsedProductID`. Viene illustrato come passare i parametri come costanti e variabili e utilizzare una variabile per passare il valore di una funzione.  
  
```  
USE AdventureWorks2012;  
GO  
-- Passing values as constants.  
EXEC dbo.uspGetWhereUsedProductID 819, '20050225';  
GO  
-- Passing values as variables.  
DECLARE @ProductID int, @CheckDate datetime;  
SET @ProductID = 819;  
SET @CheckDate = '20050225';  
EXEC dbo.uspGetWhereUsedProductID @ProductID, @CheckDate;  
GO  
-- Try to use a function as a parameter value.  
-- This produces an error message.  
EXEC dbo.uspGetWhereUsedProductID 819, GETDATE();  
GO  
-- Passing the function value as a variable.  
DECLARE @CheckDate datetime;  
SET @CheckDate = GETDATE();  
EXEC dbo.uspGetWhereUsedProductID 819, @CheckDate;  
GO  
```  
  
## <a name="specifying-parameter-names"></a>Specifica dei nomi di parametri  
 Quando si crea una procedura e si dichiara un nome di parametro, tale nome deve iniziare con un singolo carattere \@ e deve essere univoco nell'ambito della procedura.  
  
 La denominazione dei parametri e l'assegnazione dei valori appropriati in modo esplicito a ogni parametro in una chiamata alla procedura consentono ai parametri di essere forniti in qualsiasi ordine. Se ad esempio per la procedura **my_proc** sono previsti tre parametri denominati **\@first**, **\@second** e **\@third**, i valori passati alla procedura possono essere assegnati ai nomi dei parametri, ad esempio: `EXECUTE my_proc @second = 2, @first = 1, @third = 3;`  
  
> [!NOTE]  
>  Se un valore del parametro viene specificato nel formato **\@parametro =** _valore_, tutti i parametri successivi devono essere specificati in questo modo. Se i valori dei parametri non vengono passati nel formato **\@parametro =** _valore_, devono essere specificati nello stesso ordine, da sinistra a destra, dei parametri elencati nell'istruzione CREATE PROCEDURE.  
  
> [!WARNING]  
>  Qualsiasi parametro passato nel formato **\@parametro =** _valore_ contenente un errore di ortografia causerà la generazione di un errore in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e impedirà l'esecuzione della procedura.  
  
## <a name="specifying-parameter-data-types"></a>Specifica dei tipi di dati per i parametri  
 I parametri devono essere definiti con un tipo di dati quando vengono dichiarati in un'istruzione CREATE PROCEDURE. Il tipo di dati di un parametro consente di determinare il tipo e l'intervallo di valori accettati per il parametro quando viene chiamata la procedura. Se ad esempio si definisce un parametro con un tipo di dati **tinyint** , verranno accettati solo i valori numerici nell'intervallo compreso tra 0 e 255 quando vengono passati in tale parametro. Se una procedura viene eseguita con un valore incompatibile con il tipo di dati, verrà restituito un errore.  
  
## <a name="specifying-parameter-default-values"></a>Specifica dei valori predefiniti per i parametri  
 Un parametro è considerato facoltativo se dispone di un valore predefinito specificato al momento della relativa dichiarazione. Non è necessario fornire un valore per un parametro facoltativo in una chiamata alla procedura.  
  
 Il valore predefinito di un parametro viene utilizzato quando:  
  
-   Non viene specificato alcun valore nella chiamata alla procedura.  
  
-   Viene specificata la parola chiave DEFAULT come valore nella chiamata alla procedura.  
  
> [!NOTE]  
>  Se il valore predefinito è una stringa di caratteri contenente spazi vuoti o punteggiatura o se inizia con un numero, ad esempio 6xxx, è necessario racchiuderlo tra virgolette singole.  

> [!NOTE] 
> I parametri predefiniti non sono supportati in Azure Synapse Analytics o in Parallel Data Warehouse. 
  
 Se per il parametro non può essere specificato in modo appropriato alcun valore come predefinito, specificare come tale NULL. È consigliabile la restituzione di un messaggio personalizzato da parte della procedura se quest'ultima viene eseguita senza un valore per il parametro.  
  
 Nell'esempio seguente viene creata la procedura `uspGetSalesYTD` con un parametro di input, `@SalesPerson`. NULL viene assegnato come valore predefinito per il parametro e utilizzato nelle istruzioni di gestione degli errori per restituire un messaggio di errore personalizzato nei casi in cui la procedura venga eseguita senza un valore per il parametro `@SalesPerson` .  
  
```  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID('Sales.uspGetSalesYTD', 'P') IS NOT NULL  
    DROP PROCEDURE Sales.uspGetSalesYTD;  
GO  
CREATE PROCEDURE Sales.uspGetSalesYTD  
@SalesPerson nvarchar(50) = NULL  -- NULL default value  
AS   
    SET NOCOUNT ON;   
  
-- Validate the @SalesPerson parameter.  
IF @SalesPerson IS NULL  
BEGIN  
   PRINT 'ERROR: You must specify the last name of the sales person.'  
   RETURN  
END  
-- Get the sales for the specified sales person and   
-- assign it to the output parameter.  
SELECT SalesYTD  
FROM Sales.SalesPerson AS sp  
JOIN HumanResources.vEmployee AS e ON e.BusinessEntityID = sp.BusinessEntityID  
WHERE LastName = @SalesPerson;  
RETURN  
GO  
  
```  
  
 Di seguito è riportato un esempio di esecuzione della procedura. Tramite la prima istruzione la procedura viene eseguita senza specificare un valore di input. Tale operazione determina la restituzione del messaggio di errore personalizzato da parte delle istruzioni di gestione degli errori nella procedura. Tramite la seconda istruzione viene fornito un valore di input e viene restituito il set di risultati previsto.  
  
```  
-- Run the procedure without specifying an input value.  
EXEC Sales.uspGetSalesYTD;  
GO  
-- Run the procedure with an input value.  
EXEC Sales.uspGetSalesYTD N'Blythe';  
GO  
```  
  
 Sebbene sia possibile omettere i parametri per cui sono stati forniti valori predefiniti, è possibile troncare soltanto l'elenco di parametri. Ad esempio, se una procedura dispone di cinque parametri, è possibile omettere sia il quarto sia il quinto parametro. Non è tuttavia possibile ignorare il quarto parametro finché è incluso il quinto, a meno che i parametri non vengano specificati nel formato **\@parametro =** _valore_.  
  
## <a name="specifying-parameter-direction"></a>Specifica della direzione di un parametro  
 La direzione di un parametro può essere input, cioè un valore viene passato nel corpo della procedura, o output, vale a dire che tramite la procedura viene restituito un valore al programma chiamante. Il parametro di input è l'impostazione predefinita.  
  
 Per specificare un parametro di output, è necessario includere la parola chiave OUTPUT nella definizione del parametro nell'istruzione CREATE PROCEDURE. Tramite la procedura, al programma chiamante viene restituito il valore corrente del parametro di output quando la procedura è disponibile. Nel programma chiamante deve inoltre essere utilizzata la parola chiave OUTPUT quando si esegue la procedura per salvare il valore del parametro in una variabile utilizzabile nel programma chiamante.  
  
 Nell'esempio seguente viene creata la procedura `Production.usp_GetList`, mediante la quale viene restituito un elenco di prodotti i cui prezzi non superano un determinato importo. Nell'esempio viene illustrato l'utilizzo di più istruzioni SELECT e di più parametri OUTPUT. I parametri OUTPUT consentono a una procedura esterna, a un batch o a più istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] di accedere a un valore impostato durante l'esecuzione della procedura.  
  
```  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID ( 'Production.uspGetList', 'P' ) IS NOT NULL   
    DROP PROCEDURE Production.uspGetList;  
GO  
CREATE PROCEDURE Production.uspGetList @Product varchar(40)   
    , @MaxPrice money   
    , @ComparePrice money OUTPUT  
    , @ListPrice money OUT  
AS  
    SET NOCOUNT ON;  
    SELECT p.[Name] AS Product, p.ListPrice AS 'List Price'  
    FROM Production.Product AS p  
    JOIN Production.ProductSubcategory AS s   
      ON p.ProductSubcategoryID = s.ProductSubcategoryID  
    WHERE s.[Name] LIKE @Product AND p.ListPrice < @MaxPrice;  
-- Populate the output variable @ListPprice.  
SET @ListPrice = (SELECT MAX(p.ListPrice)  
        FROM Production.Product AS p  
        JOIN  Production.ProductSubcategory AS s   
          ON p.ProductSubcategoryID = s.ProductSubcategoryID  
        WHERE s.[Name] LIKE @Product AND p.ListPrice < @MaxPrice);  
-- Populate the output variable @compareprice.  
SET @ComparePrice = @MaxPrice;  
GO  
  
```  
  
 Eseguire `usp_GetList` per restituire un elenco dei prodotti di [!INCLUDE[ssSampleDBCoShort](../../includes/sssampledbcoshort-md.md)] (biciclette) con un prezzo inferiore a 700 dollari. I parametri OUTPUT **\@cost** e **\@compareprices** vengono usati con elementi del linguaggio per il controllo di flusso per restituire un messaggio nella finestra **Messaggi**.  
  
> [!NOTE]  
>  La variabile OUTPUT deve essere definita durante la creazione della procedura e durante l'utilizzo della variabile. Il nome di parametro e quello della variabile non devono corrispondere. Il tipo di dati e la posizione del parametro devono tuttavia corrispondere, a meno che non si usi **\@listprice=** _variabile_.  
  
```  
DECLARE @ComparePrice money, @Cost money ;  
EXECUTE Production.uspGetList '%Bikes%', 700,   
    @ComparePrice OUT,   
    @Cost OUTPUT  
IF @Cost <= @ComparePrice   
BEGIN  
    PRINT 'These products can be purchased for less than   
    $'+RTRIM(CAST(@ComparePrice AS varchar(20)))+'.'  
END  
ELSE  
    PRINT 'The prices for all products in this category exceed   
    $'+ RTRIM(CAST(@ComparePrice AS varchar(20)))+'.';  
  
```  
  
 Di seguito è riportato il set di risultati parziale:  
  
```  
Product                                            List Price  
-------------------------------------------------- ------------------  
Road-750 Black, 58                                 539.99  
Mountain-500 Silver, 40                            564.99  
Mountain-500 Silver, 42                            564.99  
...  
Road-750 Black, 48                                 539.99  
Road-750 Black, 52                                 539.99  
  
(14 row(s) affected)  
  
These items can be purchased for less than $700.00.  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)  
  
  
