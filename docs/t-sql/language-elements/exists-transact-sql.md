---
description: EXISTS (Transact-SQL)
title: EXISTS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- EXISTS_TSQL
- EXISTS
dev_langs:
- TSQL
helpviewer_keywords:
- existence testing [SQL Server]
- testing existence
- EXISTS keyword
- subqueries [SQL Server], EXISTS keyword
- queries [SQL Server], comparing
- comparing queries
- NOT EXISTS keyword
- row existence testing [SQL Server]
ms.assetid: b6510a65-ac38-4296-a3d5-640db0c27631
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fdc6284a52a35cdd196b6cde8e1aa5184657da33
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749881"
---
# <a name="exists-transact-sql"></a>EXISTS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Specifica una sottoquery per testare l'esistenza di righe.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
EXISTS ( subquery )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *subquery*  
 Istruzione SELECT con restrizioni. La parola chiave INTO non è consentita. Per altre informazioni, vedere la descrizione delle sottoquery in [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md).  
  
## <a name="result-types"></a>Tipi restituiti  
 **Boolean**  
  
## <a name="result-values"></a>Valori dei risultati  
 Restituisce TRUE se una sottoquery include una o più righe.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-null-in-a-subquery-to-still-return-a-result-set"></a>R. Utilizzo di NULL in una sottoquery per restituire sempre un set di risultati  
 Nell'esempio seguente viene restituito un set di risultati grazie all'aggiunta di `NULL` nella sottoquery e viene comunque restituito TRUE tramite l'utilizzo della parola chiave `EXISTS`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT DepartmentID, Name   
FROM HumanResources.Department   
WHERE EXISTS (SELECT NULL)  
ORDER BY Name ASC ;  
```  
  
### <a name="b-comparing-queries-by-using-exists-and-in"></a>B. Confronto di query con le parole chiave EXISTS e IN  
 Nell'esempio seguente vengono confrontate due query equivalenti da un punto di vista semantico. Nella prima query viene utilizzata la parola chiave `EXISTS`, mentre nella seconda query viene utilizzata la parola chiave `IN`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT a.FirstName, a.LastName  
FROM Person.Person AS a  
WHERE EXISTS  
(SELECT *   
    FROM HumanResources.Employee AS b  
    WHERE a.BusinessEntityID = b.BusinessEntityID  
    AND a.LastName = 'Johnson') ;  
GO  
```  
  
 Nella query seguente viene utilizzata la parola chiave `IN`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT a.FirstName, a.LastName  
FROM Person.Person AS a  
WHERE a.LastName IN  
(SELECT a.LastName  
    FROM HumanResources.Employee AS b  
    WHERE a.BusinessEntityID = b.BusinessEntityID  
    AND a.LastName = 'Johnson') ;  
GO  
```  
  
 Di seguito è riportato il set di risultati per entrambe le query.  
  
 ```
FirstName                                          LastName
-------------------------------------------------- ----------
Barry                                              Johnson
David                                              Johnson
Willis                                             Johnson
  
(3 row(s) affected)
 ```  
  
### <a name="c-comparing-queries-by-using-exists-and--any"></a>C. Confronto di query con le parole chiave EXISTS e = ANY  
 Nell'esempio seguente vengono illustrate due query per la ricerca di negozi il cui nome coincide con quello del fornitore. La prima query usa `EXISTS` e la seconda usa `=``ANY`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT DISTINCT s.Name  
FROM Sales.Store AS s   
WHERE EXISTS  
(SELECT *  
    FROM Purchasing.Vendor AS v  
    WHERE s.Name = v.Name) ;  
GO  
```  
  
 Nella query seguente viene utilizzata la parola chiave `= ANY`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT DISTINCT s.Name  
FROM Sales.Store AS s   
WHERE s.Name = ANY  
(SELECT v.Name  
    FROM Purchasing.Vendor AS v ) ;  
GO  
```  
  
### <a name="d-comparing-queries-by-using-exists-and-in"></a>D. Confronto di query con le parole chiave EXISTS e IN  
 Nell'esempio seguente vengono illustrate query per la ricerca di dipendenti facenti parte di reparti il cui nome inizia con la lettera `P`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT p.FirstName, p.LastName, e.JobTitle  
FROM Person.Person AS p   
JOIN HumanResources.Employee AS e  
   ON e.BusinessEntityID = p.BusinessEntityID   
WHERE EXISTS  
(SELECT *  
    FROM HumanResources.Department AS d  
    JOIN HumanResources.EmployeeDepartmentHistory AS edh  
       ON d.DepartmentID = edh.DepartmentID  
    WHERE e.BusinessEntityID = edh.BusinessEntityID  
    AND d.Name LIKE 'P%') ;  
GO  
```  
  
 Nella query seguente viene utilizzata la parola chiave `IN`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT p.FirstName, p.LastName, e.JobTitle  
FROM Person.Person AS p JOIN HumanResources.Employee AS e  
   ON e.BusinessEntityID = p.BusinessEntityID   
JOIN HumanResources.EmployeeDepartmentHistory AS edh  
   ON e.BusinessEntityID = edh.BusinessEntityID   
WHERE edh.DepartmentID IN  
(SELECT DepartmentID  
   FROM HumanResources.Department  
   WHERE Name LIKE 'P%') ;  
GO  
```  
  
### <a name="e-using-not-exists"></a>E. Utilizzo della parola chiave NOT EXISTS  
 La parola chiave NOT EXISTS funziona in modo inverso rispetto a EXISTS. La clausola WHERE in NOT EXISTS viene soddisfatta se la sottoquery non restituisce alcuna riga. Nell'esempio seguente vengono cercati i dipendenti che non fanno parte di reparti il cui nome inizia con la lettera `P`.  
  
```sql  
SELECT p.FirstName, p.LastName, e.JobTitle  
FROM Person.Person AS p   
JOIN HumanResources.Employee AS e  
   ON e.BusinessEntityID = p.BusinessEntityID   
WHERE NOT EXISTS  
(SELECT *  
   FROM HumanResources.Department AS d  
   JOIN HumanResources.EmployeeDepartmentHistory AS edh  
      ON d.DepartmentID = edh.DepartmentID  
   WHERE e.BusinessEntityID = edh.BusinessEntityID  
   AND d.Name LIKE 'P%')  
ORDER BY LastName, FirstName  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
FirstName                      LastName                       Title
------------------------------ ------------------------------ ------------
Syed                           Abbas                          Pacific Sales Manager
Hazem                          Abolrous                       Quality Assurance Manager
Humberto                       Acevedo                        Application Specialist
Pilar                          Ackerman                       Shipping & Receiving Superviso
François                       Ajenstat                       Database Administrator
Amy                            Alberts                        European Sales Manager
Sean                           Alexander                      Quality Assurance Technician
Pamela                         Ansman-Wolfe                   Sales Representative
Zainal                         Arifin                         Document Control Manager
David                          Barber                         Assistant to CFO
Paula                          Barreto de Mattos              Human Resources Manager
Shai                           Bassli                         Facilities Manager
Wanida                         Benshoof                       Marketing Assistant
Karen                          Berg                           Application Specialist
Karen                          Berge                          Document Control Assistant
Andreas                        Berglund                       Quality Assurance Technician
Matthias                       Berndt                         Shipping & Receiving Clerk
Jo                             Berry                          Janitor
Jimmy                          Bischoff                       Stocker
Michael                        Blythe                         Sales Representative
David                          Bradley                        Marketing Manager
Kevin                          Brown                          Marketing Assistant
David                          Campbell                       Sales Representative
Jason                          Carlson                        Information Services Manager
Fernando                       Caro                           Sales Representative
Sean                           Chai                           Document Control Assistant
Sootha                         Charncherngkha                 Quality Assurance Technician
Hao                            Chen                           HR Administrative Assistant
Kevin                          Chrisulis                      Network Administrator
Pat                            Coleman                        Janitor
Stephanie                      Conroy                         Network Manager
Debra                          Core                           Application Specialist
Ovidiu                         Crãcium                        Sr. Tool Designer
Grant                          Culbertson                     HR Administrative Assistant
Mary                           Dempsey                        Marketing Assistant
Thierry                        D'Hers                         Tool Designer
Terri                          Duffy                          VP Engineering
Susan                          Eaton                          Stocker
Terry                          Eminhizer                      Marketing Specialist
Gail                           Erickson                       Design Engineer
Janice                         Galvin                         Tool Designer
Mary                           Gibson                         Marketing Specialist
Jossef                         Goldberg                       Design Engineer
Sariya                         Harnpadoungsataya              Marketing Specialist
Mark                           Harrington                     Quality Assurance Technician
Magnus                         Hedlund                        Facilities Assistant
Shu                            Ito                            Sales Representative
Stephen                        Jiang                          North American Sales Manager
Willis                         Johnson                        Recruiter
Brannon                        Jones                          Finance Manager
Tengiz                         Kharatishvili                  Control Specialist
Christian                      Kleinerman                     Maintenance Supervisor
Vamsi                          Kuppa                          Shipping & Receiving Clerk
David                          Liu                            Accounts Manager
Vidur                          Luthra                         Recruiter
Stuart                         Macrae                         Janitor
Diane                          Margheim                       Research & Development Enginee
Mindy                          Martin                         Benefits Specialist
Gigi                           Matthew                        Research & Development Enginee
Tete                           Mensa-Annan                    Sales Representative
Ramesh                         Meyyappan                      Application Specialist
Dylan                          Miller                         Research & Development Manager
Linda                          Mitchell                       Sales Representative
Barbara                        Moreland                       Accountant
Laura                          Norman                         Chief Financial Officer
Chris                          Norred                         Control Specialist
Jae                            Pak                            Sales Representative
Wanda                          Parks                          Janitor
Deborah                        Poe                            Accounts Receivable Specialist
Kim                            Ralls                          Stocker
Tsvi                           Reiter                         Sales Representative
Sharon                         Salavaria                      Design Engineer
Ken                            Sanchez                        Chief Executive Officer
José                           Saraiva                        Sales Representative
Mike                           Seamans                        Accountant
Ashvini                        Sharma                         Network Administrator
Janet                          Sheperdigian                   Accounts Payable Specialist
Candy                          Spoon                          Accounts Receivable Specialist
Michael                        Sullivan                       Sr. Design Engineer
Dragan                         Tomic                          Accounts Payable Specialist
Lynn                           Tsoflias                       Sales Representative
Rachel                         Valdez                         Sales Representative
Garrett                        Vargar                         Sales Representative
Ranjit                         Varkey Chudukatil              Sales Representative
Bryan                          Walton                         Accounts Receivable Specialist
Jian Shuo                      Wang                           Engineering Manager
Brian                          Welcker                        VP Sales
Jill                           Williams                       Marketing Specialist
Dan                            Wilson                         Database Administrator
John                           Wood                           Marketing Specialist
Peng                           Wu                             Quality Assurance Supervisor
  
(91 row(s) affected)
 ```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="f-using-exists"></a>F. Uso di EXISTS  
 L'esempio seguente consente di stabilire se nella tabella `ProspectiveBuyer` sono presenti righe corrispondenti a righe nella tabella `DimCustomer`. La query restituirà righe solo in caso di corrispondenza di entrambi i valori `LastName` e `BirthDate` nelle due tabelle.  
  
```sql
-- Uses AdventureWorks  
  
SELECT a.LastName, a.BirthDate  
FROM DimCustomer AS a  
WHERE EXISTS  
(SELECT *   
    FROM dbo.ProspectiveBuyer AS b  
    WHERE (a.LastName = b.LastName) AND (a.BirthDate = b.BirthDate)) ;  
```  
  
### <a name="g-using-not-exists"></a>G. Utilizzo della parola chiave NOT EXISTS  
 La parola chiave NOT EXISTS funziona in modo inverso rispetto a EXISTS. La clausola WHERE in NOT EXISTS viene soddisfatta se la sottoquery non restituisce alcuna riga. L'esempio seguente consente di trovare le righe nella tabella `DimCustomer` in cui `LastName` e `BirthDate` non corrispondono ad alcuna voce nella tabella `ProspectiveBuyers`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT a.LastName, a.BirthDate  
FROM DimCustomer AS a  
WHERE NOT EXISTS  
(SELECT *   
    FROM dbo.ProspectiveBuyer AS b  
    WHERE (a.LastName = b.LastName) AND (a.BirthDate = b.BirthDate)) ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Espressioni &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Funzioni predefinite &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)  
  
  


