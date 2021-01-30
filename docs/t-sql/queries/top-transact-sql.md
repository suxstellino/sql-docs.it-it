---
description: TOP (Transact-SQL)
title: TOP (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TOP_TSQL
- TOP
dev_langs:
- TSQL
helpviewer_keywords:
- TOP clause
- first set of query result rows [SQL Server]
- TOP clause, about TOP clause
- queries [SQL Server], results
ms.assetid: da983c0a-06c5-4cf8-a6a4-7f9d66f34f2c
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d633ac65508c50895943b5604a704a5041510529
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211032"
---
# <a name="top-transact-sql"></a>TOP (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Limita le righe restituite nel set di risultati di una query a un numero specificato o a una percentuale di righe in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Quando si usa TOP con la clausola ORDER BY, il set di risultati è limitato alle prime *N* righe ordinate. In caso contrario, TOP restituisce le prime *N* righe in un ordine non definito. Usare questa clausola per specificare il numero di righe restituito da un'istruzione SELECT. In alternativa, usare TOP per specificare le righe interessate da un'istruzione INSERT, UPDATE, MERGE o DELETE.  
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
 
 Di seguito è riportata la sintassi per SQL Server e il database SQL di Azure:

```syntaxsql  
[   
    TOP (expression) [PERCENT]  
    [ WITH TIES ]  
]  
```  

Di seguito è riportata la sintassi per [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]:

```syntaxsql  
[   
    TOP ( expression )   
    [ WITH TIES ]  
]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*expression*  
Espressione numerica che specifica il numero di righe da restituire. Se si specifica PERCENT, viene eseguita la conversione implicita di *expression* in un valore **float**. In caso contrario, l'argomento *expression* viene convertito in **bigint**.  
  
PERCENT  
Indica che la query restituisce solo la prima percentuale *expression* di righe dal set di risultati. I valori frazionari vengono arrotondati al valore intero più vicino.  
  
WITH TIES  
Restituisce due o più righe con valori equivalenti per l'ultima posizione del set di risultati limitato. È necessario usare questo argomento con la clausola **ORDER BY**. **WITH TIES** potrebbe causare la restituzione di un numero di righe maggiore rispetto al valore specificato in *expression*. Se l'argomento *expression* è impostato su 5 ma ai valori delle colonne **ORDER BY** nella riga 5 corrispondono due righe in più, ad esempio, il set di risultati conterrà sette righe.  
  
È possibile specificare la clausola TOP con l'argomento WITH TIES solo nelle istruzioni SELECT e solo se viene usata anche la clausola ORDER BY. L'ordine restituito per l'associazione dei record è arbitrario. ORDER BY non influisce su questa regola.  
  
## <a name="best-practices"></a>Procedure consigliate  
In un'istruzione SELECT utilizzare sempre una clausola ORDER BY con la clausola TOP. È infatti l'unico modo per indicare in modo prevedibile le righe interessate dalla clausola TOP.  
  
Utilizzare OFFSET e FETCH nella clausola ORDER BY anziché la clausola TOP per implementare una soluzione di paging delle query. Una soluzione di paging, ovvero l'invio di blocchi o "pagine" di dati al client, è di più facile implementazione con le clausole OFFSET e FETCH. Per altre informazioni, vedere [Clausola ORDER BY &#40;Transact-SQL&#41;](../../t-sql/queries/select-order-by-clause-transact-sql.md).  
  
Utilizzare TOP (o OFFSET e FETCH) anziché SET ROWCOUNT per limitare il numero di righe restituite. Questi metodi vengono preferiti all'utilizzo di SET ROWCOUNT per i motivi seguenti:  
  
-   Come parte di un'istruzione SELECT, in Query Optimizer il valore di *expression* nella clausola TOP o FETCH può essere preso in considerazione durante l'ottimizzazione della query. Dato che si usa SET ROWCOUNT al di fuori di un'istruzione che esegue una query, il relativo valore non può essere considerato in un piano di query.  
  
## <a name="compatibility-support"></a>Informazioni sulla compatibilità  
Per compatibilità con le versioni precedenti, le parentesi sono facoltative nelle istruzioni SELECT se l'espressione è una costante Integer. È consigliabile usare sempre le parentesi per TOP nelle istruzioni SELECT, in modo da mantenere la coerenza con l'uso obbligatorio nelle istruzioni INSERT, UPDATE, MERGE e DELETE. 
  
## <a name="interoperability"></a>Interoperabilità  
L'espressione di TOP non influisce sulle istruzioni eventualmente eseguite a causa di un trigger. Le tabelle **inserted** e **deleted** nei trigger restituiscono solo le righe effettivamente interessate dalle istruzioni INSERT, UPDATE, MERGE o DELETE. Questo si verifica ad esempio se un trigger INSERT viene attivato come risultato di un'istruzione INSERT in cui è stata usata una clausola TOP.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consente l'aggiornamento delle righe attraverso le viste. Dato che è possibile includere la clausola TOP nella definizione della vista, alcune righe potrebbero non essere più presenti nella vista se a causa di un aggiornamento non soddisfano più i requisiti dell'espressione di TOP.  
  
Quando viene specificata nell'istruzione MERGE, la clausola TOP si applica *dopo* l'unione in join dell'intera tabella di origine e dell'intera tabella di destinazione. Le righe unite in join non qualificate per un'azione di inserimento, aggiornamento o eliminazione, inoltre, vengono rimosse. La clausola TOP riduce ulteriormente il numero di righe unite in join in base al valore specificato e l'azione di inserimento, aggiornamento o eliminazione si applica alle restanti righe unite in join in modo non ordinato. Ciò significa che le righe vengono distribuite tra le azioni definite nelle clausole WHEN senza alcun ordine. Se specificando TOP (10) le righe interessate sono 10, ad esempio, sette di queste righe potrebbero essere aggiornate e tre inserite oppure una potrebbe essere eliminata, cinque aggiornate e quattro inserite e così via. Dato che l'istruzione MERGE esegue una scansione di tabella completa sulle tabelle sia di origine che di destinazione, l'uso della clausola TOP per modificare una tabella di grandi dimensioni creando più batch può influire sulle prestazioni di I/O. In questo scenario è importante assicurarsi che tutti i batch successivi abbiano come destinazione nuove righe.  
  
Prestare attenzione quando si specifica la clausola TOP in una query contenente un operatore UNION, UNION ALL, EXCEPT o INTERSECT. È possibile scrivere una query che restituisce risultati imprevisti perché l'ordine in cui le clausole TOP e ORDER BY vengono elaborate logicamente non è sempre intuitivo quando questi operatori vengono usati in un'operazione di selezione. Ad esempio, considerati i dati e la tabella seguenti, si supponga di voler ottenere come risultato la macchina rossa meno costosa e la macchina blu più costosa, ovvero la berlina rossa e il furgone blu.  
  
```sql  
CREATE TABLE dbo.Cars(Model VARCHAR(15), Price MONEY, Color VARCHAR(10));  
INSERT dbo.Cars VALUES  
    ('sedan', 10000, 'red'), ('convertible', 15000, 'blue'),   
    ('coupe', 20000, 'red'), ('van', 8000, 'blue');  
```  
  
Per ottenere questi risultati, è possibile scrivere la query seguente.  
  
```sql  
SELECT TOP(1) Model, Color, Price  
FROM dbo.Cars  
WHERE Color = 'red'  
UNION ALL  
SELECT TOP(1) Model, Color, Price  
FROM dbo.Cars  
WHERE Color = 'blue'  
ORDER BY Price ASC;  
GO    
```  
  
Di seguito è riportato il set di risultati.  
  
 ```
 Model         Color      Price  
 ------------- ---------- -------  
 sedan         red        10000.00  
 convertible   blue       15000.00
 ```  
  
Vengono restituiti risultati imprevisti perché la clausola TOP viene eseguita logicamente prima della clausola ORDER BY che ordina i risultati dell'operatore (in questo caso UNION ALL). La query precedente restituisce pertanto qualsiasi macchina rossa e qualsiasi macchina blu e quindi ordina il risultato dell'unione in base al prezzo. Nell'esempio seguente viene illustrato il metodo corretto per scrivere questa query per ottenere il risultato desiderato.  
  
```sql  
SELECT Model, Color, Price  
FROM (SELECT TOP(1) Model, Color, Price  
      FROM dbo.Cars  
      WHERE Color = 'red'  
      ORDER BY Price ASC) AS a  
UNION ALL  
SELECT Model, Color, Price  
FROM (SELECT TOP(1) Model, Color, Price  
      FROM dbo.Cars  
      WHERE Color = 'blue'  
      ORDER BY Price ASC) AS b;  
GO    
```  
  
L'uso di TOP e ORDER BY in un'operazione sub-SELECT assicura che i risultati della clausola ORDER BY vengano applicati alla clausola TOP e non all'ordinamento del risultato dell'operazione UNION.  
  
 Questo è il set di risultati.  
  
 ```
 Model         Color      Price  
 ------------- ---------- -------  
 sedan         red        10000.00  
 van           blue        8000.00
 ```  
  
## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni  
Quando si usa TOP con INSERT, UPDATE, MERGE o DELETE, le righe a cui viene fatto riferimento non vengono disposte in alcun ordine e non è possibile specificare direttamente la clausola ORDER BY in queste istruzioni. Se è necessario usare TOP per inserire, eliminare o modificare righe in un ordine cronologico significativo, usare TOP specificando una clausola ORDER BY in un'istruzione sub-SELECT. Vedere la successiva sezione Esempi di questo articolo.  
  
Non è possibile usare TOP in istruzioni UPDATE e DELETE su viste partizionate.  
  
Non è possibile combinare TOP con OFFSET e FETCH nella stessa espressione di query (nello stesso ambito query). Per altre informazioni, vedere [Clausola ORDER BY &#40;Transact-SQL&#41;](../../t-sql/queries/select-order-by-clause-transact-sql.md).  
  
## <a name="examples"></a>Esempi  
  
|Category|Elementi di sintassi inclusi|  
|--------------|------------------------------|  
|[Sintassi di base](#BasicSyntax)|TOP • PERCENT|  
|[Inclusione di valori equivalenti](#tie)|WITH TIES|  
|[Limitazione delle righe interessate da DELETE, INSERT o UPDATE](#DML)|DELETE • INSERT • UPDATE|  
  
###  <a name="basic-syntax"></a><a name="BasicSyntax"></a> Sintassi di base  
Negli esempi contenuti in questa sezione vengono illustrate le funzionalità di base della clausola ORDER BY utilizzando la sintassi minima necessaria.  
  
#### <a name="a-using-top-with-a-constant-value"></a>R. Utilizzo di TOP con un valore costante  
Negli esempi seguenti viene utilizzato un valore costante per specificare il numero di dipendenti restituiti nel set di risultati della query. Nel primo esempio vengono restituite le prime 10 righe non definite perché non viene usata una clausola ORDER BY. Nel secondo esempio viene utilizzata una clausola ORDER BY per restituire i primi 10 dipendenti assunti di recente.  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Select the first 10 random employees.  
SELECT TOP(10)JobTitle, HireDate  
FROM HumanResources.Employee;  
GO  
-- Select the first 10 employees hired most recently.  
SELECT TOP(10)JobTitle, HireDate  
FROM HumanResources.Employee  
ORDER BY HireDate DESC;  
GO  
```  
  
#### <a name="b-using-top-with-a-variable"></a>B. Utilizzo di TOP con una variabile  
Nell'esempio seguente viene utilizzata una variabile per specificare il numero di dipendenti restituiti nel set di risultati della query.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @p AS INT = 10;  
SELECT TOP(@p)JobTitle, HireDate, VacationHours  
FROM HumanResources.Employee  
ORDER BY VacationHours DESC;  
GO  
```  
  
#### <a name="c-specifying-a-percentage"></a>C. Specifica di una percentuale  
Nell'esempio seguente viene utilizzato PERCENT per specificare il numero di dipendenti restituiti nel set di risultati della query. Nella tabella `HumanResources.Employee` sono presenti 290 dipendenti. Dato che il 5% di 290 è un valore frazionario, il valore viene arrotondato al numero intero successivo.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT TOP(5)PERCENT JobTitle, HireDate  
FROM HumanResources.Employee  
ORDER BY HireDate DESC;  
GO    
```  
  
###  <a name="including-tie-values"></a><a name="tie"></a> Inclusione di valori equivalenti  
  
#### <a name="a-using-with-ties-to-include-rows-that-match-the-values-in-the-last-row"></a>R. Utilizzo di WITH TIES per includere righe corrispondenti ai valori nell'ultima riga  
L'esempio seguente recupera il primo `10`% di tutti i dipendenti con lo stipendio più alto e restituisce i dipendenti in ordine decrescente in base allo stipendio. Specificando `WITH TIES`, nel set di risultati vengono inclusi anche i dipendenti con stipendio pari allo stipendio più basso restituito (ultima riga), anche se in questo modo il set di risultati supera il `10`% dei dipendenti.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT TOP(10) PERCENT WITH TIES  
pp.FirstName, pp.LastName, e.JobTitle, e.Gender, r.Rate  
FROM Person.Person AS pp   
    INNER JOIN HumanResources.Employee AS e  
        ON pp.BusinessEntityID = e.BusinessEntityID  
    INNER JOIN HumanResources.EmployeePayHistory AS r  
        ON r.BusinessEntityID = e.BusinessEntityID  
ORDER BY Rate DESC;  
GO    
```  
  
###  <a name="limiting-the-rows-affected-by-delete-insert-or-update"></a><a name="DML"></a> Limitazione delle righe interessate da DELETE, INSERT o UPDATE  
  
#### <a name="a-using-top-to-limit-the-number-of-rows-deleted"></a>R. Utilizzo di TOP per limitare il numero di righe eliminate  
Quando si usa una clausola TOP (*n*) con DELETE, l'operazione di eliminazione viene eseguita su una selezione non definita di *n* righe. In altre parole, l'istruzione DELETE sceglie un numero (*n*) di righe che soddisfano i criteri definiti nella clausola WHERE. L'esempio seguente elimina `20` righe con scadenze precedenti al 1° luglio 2002 dalla tabella `PurchaseOrderDetail`.  
  
```sql  
USE AdventureWorks2012;  
GO  
DELETE TOP (20)   
FROM Purchasing.PurchaseOrderDetail  
WHERE DueDate < '20020701';  
GO  
```  
  
Se si vuole usare TOP per eliminare le righe in un ordine cronologico significativo, usare TOP con ORDER BY in un'istruzione sub-SELECT. Tramite la query seguente vengono eliminate le 10 righe della tabella `PurchaseOrderDetail` contenenti le date di scadenza più imminenti. Per assicurarsi che vengano eliminate solo 10 righe, la colonna specificata nell'istruzione di selezione secondaria (`PurchaseOrderID`) è la chiave primaria della tabella. L'utilizzo di una colonna non chiave nell'istruzione sub-SELECT può avere come conseguenza l'eliminazione di più di 10 righe se la colonna specificata contiene valori duplicati.  
  
```sql  
USE AdventureWorks2012;  
GO  
DELETE FROM Purchasing.PurchaseOrderDetail  
WHERE PurchaseOrderDetailID IN  
   (SELECT TOP 10 PurchaseOrderDetailID   
    FROM Purchasing.PurchaseOrderDetail   
    ORDER BY DueDate ASC);  
GO  
```  
  
#### <a name="b-using-top-to-limit-the-number-of-rows-inserted"></a>B. Utilizzo di TOP per limitare il numero di righe inserite  
L'esempio seguente crea la tabella `EmployeeSales` e inserisce il nome e i dati sulle vendite da inizio anno per i primi cinque dipendenti della tabella `HumanResources.Employee`. L'istruzione INSERT sceglie cinque righe qualsiasi restituite dall'istruzione `SELECT` che soddisfano i criteri definiti nella clausola WHERE. La clausola OUTPUT consente di visualizzare le righe inserite nella tabella `EmployeeSales`. Si noti che la clausola ORDER BY nell'istruzione SELECT non viene usate per determinare i primi cinque dipendenti.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
IF OBJECT_ID ('dbo.EmployeeSales', 'U') IS NOT NULL  
    DROP TABLE dbo.EmployeeSales;  
GO  
CREATE TABLE dbo.EmployeeSales  
( EmployeeID   NVARCHAR(11) NOT NULL,  
  LastName     NVARCHAR(20) NOT NULL,  
  FirstName    NVARCHAR(20) NOT NULL,  
  YearlySales  MONEY NOT NULL  
 );  
GO  
INSERT TOP(5)INTO dbo.EmployeeSales  
    OUTPUT inserted.EmployeeID, inserted.FirstName, inserted.LastName, inserted.YearlySales  
    SELECT sp.BusinessEntityID, c.LastName, c.FirstName, sp.SalesYTD   
    FROM Sales.SalesPerson AS sp  
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.SalesYTD > 250000.00  
    ORDER BY sp.SalesYTD DESC;  
GO    
```  
  
Se si vuole usare TOP per inserire le righe in un ordine cronologico significativo, usare TOP con ORDER BY in un'istruzione sub-SELECT. L'esempio seguente illustra come effettuare questa operazione. La clausola OUTPUT consente di visualizzare le righe inserite nella tabella `EmployeeSales`. Si noti che vengono ora inseriti i primi cinque dipendenti in base ai risultati della clausola ORDER BY, anziché righe non definite.  
  
```sql  
INSERT INTO dbo.EmployeeSales  
    OUTPUT inserted.EmployeeID, inserted.FirstName, inserted.LastName, inserted.YearlySales  
    SELECT TOP (5) sp.BusinessEntityID, c.LastName, c.FirstName, sp.SalesYTD   
    FROM Sales.SalesPerson AS sp  
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.SalesYTD > 250000.00  
    ORDER BY sp.SalesYTD DESC;  
GO    
```  
  
#### <a name="c-using-top-to-limit-the-number-of-rows-updated"></a>C. Utilizzo di TOP per limitare il numero di righe aggiornate  
Nell'esempio seguente viene utilizzata la clausola TOP per aggiornare righe in una tabella. Quando si usa una clausola TOP (*n*) con UPDATE, l'operazione di aggiornamento viene eseguita su un numero non definito di righe. In altre parole, l'istruzione UPDATE sceglie un numero (*n*) di righe che soddisfano i criteri definiti nella clausola WHERE. Nell'esempio seguente vengono assegnati 10 clienti da un venditore a un altro.  
  
```sql  
USE AdventureWorks2012;  
UPDATE TOP (10) Sales.Store  
SET SalesPersonID = 276  
WHERE SalesPersonID = 275;  
GO  
```  
  
Se è necessario utilizzare TOP per applicare gli aggiornamenti in un ordine cronologico significativo, è necessario utilizzare questa clausola insieme a ORDER BY in un'istruzione sub-SELECT. Nell'esempio seguente le ore di ferie dei 10 dipendenti vengono aggiornate con le prime date di assunzione.  
  
```sql  
UPDATE HumanResources.Employee  
SET VacationHours = VacationHours + 8  
FROM (SELECT TOP 10 BusinessEntityID FROM HumanResources.Employee  
     ORDER BY HireDate ASC) AS th  
WHERE HumanResources.Employee.BusinessEntityID = th.BusinessEntityID;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
L'esempio seguente restituisce le prime 31 righe corrispondenti ai criteri di query. La clausola **ORDER BY** garantisce che le 31 righe restituite siano le prime 31 righe in base all'ordinamento alfabetico della colonna `LastName`.  
  
Uso di **TOP** senza specificare i valori equivalenti.  
  
```sql  
SELECT TOP (31) FirstName, LastName   
FROM DimEmployee ORDER BY LastName;  
```  
  
Risultato: vengono restituite 31 righe.  
  
Uso di TOP specificando WITH TIES.  
  
```sql  
SELECT TOP (31) WITH TIES FirstName, LastName   
FROM DimEmployee ORDER BY LastName;  
```  
  
Risultato: vengono restituite 33 righe perché tre dipendenti di nome Brown hanno un valore equivalente per la 31esima riga.  
  
## <a name="see-also"></a>Vedere anche  
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [Clausola ORDER BY &#40;Transact-SQL&#41;](../../t-sql/queries/select-order-by-clause-transact-sql.md)   
 [SET ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/statements/set-rowcount-transact-sql.md)   
 [MERGE &#40;Transact-SQL&#41;](../../t-sql/statements/merge-transact-sql.md)  
  
 
