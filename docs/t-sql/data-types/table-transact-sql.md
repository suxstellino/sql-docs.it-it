---
description: table (Transact-SQL)
title: table (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/27/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- table data type [SQL Server]
- table variables [SQL Server]
ms.assetid: 1ef0b60e-a64c-4e97-847b-67930e3973ef
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 4ed393c00500c0f195d9552daf9bf6414db1da88
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179710"
---
# <a name="table-transact-sql"></a>table (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

È un tipo di dati speciale usato per archiviare un set di risultati per elaborazioni successive. Il tipo **table** viene principalmente usato per l'archiviazione temporanea di un set di righe restituito come set di risultati di una funzione con valori di tabella. È possibile dichiarare funzioni e variabili di tipo **table**. Le variabili di tipo **table** possono essere usate in funzioni, stored procedure e batch. Per dichiarare variabili di tipo **table**, usare [DECLARE @local_variable](../../t-sql/language-elements/declare-local-variable-transact-sql.md).
  

**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
table_type_definition ::=   
    TABLE ( { <column_definition> | <table_constraint> } [ ,...n ] )   
  
<column_definition> ::=   
    column_name scalar_data_type   
    [ COLLATE <collation_definition> ]   
    [ [ DEFAULT constant_expression ] | IDENTITY [ ( seed , increment ) ] ]   
    [ ROWGUIDCOL ]   
    [ column_constraint ] [ ...n ]   
  
 <column_constraint> ::=   
    { [ NULL | NOT NULL ]   
    | [ PRIMARY KEY | UNIQUE ]   
    | CHECK ( logical_expression )   
    }   
  
<table_constraint> ::=   
     { { PRIMARY KEY | UNIQUE } ( column_name [ ,...n ] )  
     | CHECK ( logical_expression )   
     }   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*table_type_definition*  
Stesso subset di informazioni utilizzate per definire una tabella nell'istruzione CREATE TABLE. La dichiarazione di tabella include definizioni di colonna, nomi, tipi di dati e vincoli. Gli unici tipi di vincoli consentiti sono PRIMARY KEY, UNIQUE KEY e NULL.  
Per altre informazioni sulla sintassi, vedere [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md), [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md) e [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md).
  
*collation_definition*  
Regole di confronto della colonna costituite da impostazioni locali di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows e stile di confronto, impostazioni locali di Windows e notazione binaria oppure regole di confronto di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se non si specifica *collation_definition*, la colonna eredita le regole di confronto del database corrente. Se invece viene specificata come tipo CLR (Common Language Runtime) definito dall'utente, la colonna eredita le regole di confronto del tipo definito dall'utente.
  
## <a name="remarks"></a>Osservazioni  
Nella clausola FROM di un batch, alle variabili di tipo **table** viene fatto riferimento in base al nome, come illustrato nell'esempio seguente:
  
```sql
SELECT Employee_ID, Department_ID FROM @MyTableVar;  
```  
  
All'esterno di una clausola FROM, è necessario fare riferimento alle variabili di tipo **table** tramite un alias, come illustrato nell'esempio seguente:
  
```sql
SELECT EmployeeID, DepartmentID   
FROM @MyTableVar m  
JOIN Employee on (m.EmployeeID =Employee.EmployeeID AND  
   m.DepartmentID = Employee.DepartmentID);  
```  
  
Per query in scala ridotta con piani di query che non vengono modificati e quando la ricompilazione è una preoccupazione dominante, le variabili di tipo **table** offrono i vantaggi seguenti:
-   Le variabili di tipo **table** funzionano in modo analogo alle variabili locali. Queste variabili hanno un ambito ben definito, Questa variabile può essere usata nella funzione, nella stored procedure o nel batch in cui è dichiarata.  
     All'interno del proprio ambito, le variabili **table** possono essere usate come normali tabelle. in tutti i casi in cui è possibile utilizzare una tabella o espressione di tabella in istruzioni SELECT, INSERT, UPDATE e DELETE. Non è tuttavia possibile usare **table** nell'istruzione seguente:  
  
```sql
SELECT select_list INTO table_variable;
```
  
La pulizia delle variabili di tipo **table** viene eseguita automaticamente alla fine della funzione, della stored procedure o del batch in cui sono definite.
  
-   Quando si usano variabili di tipo **table** nelle stored procedure, il numero di ricompilazioni delle stored procedure risulta minore rispetto a quando vengono usate tabelle temporanee, in assenza di scelte basate sui costi che influiscono sulle prestazioni.  
-   La durata delle transazioni che includono variabili di tipo **table** corrisponde solo alla durata dell'aggiornamento della variabile di tipo **table**. Le variabili di tipo **table** richiedono quindi una minore quantità di risorse di blocco e registrazione.  
  
## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni
Le variabili di tipo **table** non includono statistiche di distribuzione e non attivano ricompilazioni. In molti casi, l'utilità di ottimizzazione creerà un piano di query basandosi sul presupposto che la variabile di tabella non contenga righe. Per questo motivo, è necessario prestare attenzione in caso di utilizzo di una variabile di tabella se si prevede un numero elevato di righe (maggiore di 100). In tal caso, le tabelle temporanee potrebbero rappresentare una soluzione migliore. Per le query che uniscono in join la variabile di tabella con altre tabelle, usare l'hint RECOMPILE, con cui l'utilità di ottimizzazione userà la cardinalità corretta per la variabile di tabella.
  
Le variabili di tipo **table** non sono supportate nel modello di ragionamento basato sui costi dell'utilità di ottimizzazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. È quindi consigliabile non usarle quando sono necessarie scelte basate sui costi per ottenere un piano di query efficiente. È preferibile utilizzare le tabelle temporanee quando sono necessarie scelte basate sui costi, Tale piano include in genere query con join, decisioni di parallelismo e scelte di selezione degli indici.
  
Per le query che modificano le variabili di tipo **table** non vengono generati piani di esecuzione di query parallele. La modifica di variabili di tipo **table** di grandi dimensioni o di variabili di tipo **table** in query complesse può influire sulle prestazioni. Nei casi in cui le variabili di tipo **table** vengono modificate, valutare la possibilità di usare invece tabelle temporanee. Per altre informazioni, vedere [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md). Le query che leggono le variabili di tipo **table** senza modificarle possono comunque essere eseguite in parallelo.

> [!IMPORTANT]
> Il livello di compatibilità del database 150 migliora le prestazioni delle variabili di tabella grazie all'introduzione della **compilazione posticipata delle variabili di tabella**.  Per altre informazioni, vedere [Compilazione posticipata delle variabili di tabella](../../relational-databases/performance/intelligent-query-processing.md#table-variable-deferred-compilation).
>
  
Non è possibile creare indici in modo esplicito su variabili di tipo **table** e per le variabili di tipo **table** non vengono mantenute statistiche. A partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], è stata introdotta una nuova sintassi che consente di creare determinati tipi di indice inline con la definizione della tabella.  Usando questa nuova sintassi, è possibile creare indici su variabili **tabella** come parte della definizione della tabella. In alcuni casi, è possibile ottenere un miglioramento delle prestazioni usando tabelle temporanee, che offrono statistiche e supporto completo per l'indice. Per altre informazioni sulle tabelle temporanee e la creazione di indici inline, vedere [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).

I vincoli CHECK, i valori DEFAULT e le colonne calcolate nella dichiarazione del tipo **table** non possono chiamare funzioni definite dall'utente.
  
L'operazione di assegnazione tra variabili di tipo **table** non è supportata.
  
Dato che hanno ambito limitato e non fanno parte del database permanente, le variabili di tipo **table** non sono interessate dalle operazioni di rollback di transazioni.
  
Le variabili di tabella non possono essere modificate dopo la creazione.
  
## <a name="examples"></a>Esempi  
  
### <a name="a-declaring-a-variable-of-type-table"></a>R. Dichiarazione di una variabile di tipo table  
Nell'esempio seguente viene creata una variabile di tipo `table` in cui vengono archiviati i valori specificati nella clausola OUTPUT dell'istruzione UPDATE. Questa variabile è seguita da due istruzioni `SELECT` che restituiscono i valori in `@MyTableVar` e i risultati dell'operazione di aggiornamento nella tabella `Employee`. I risultati nella colonna `INSERTED.ModifiedDate` sono diversi rispetto ai valori nella colonna `ModifiedDate` della tabella `Employee`. Questa differenza è causata dalla definizione nella tabella `Employee` del trigger `AFTER UPDATE`, che aggiorna il valore di `ModifiedDate` alla data corrente. Le colonne restituite da `OUTPUT`, tuttavia, riflettono i dati prima dell'attivazione dei trigger. Per altre informazioni, vedere [Clausola OUTPUT &#40;Transact-SQL&#41;](../../t-sql/queries/output-clause-transact-sql.md).
  
```sql
USE AdventureWorks2012;  
GO  
DECLARE @MyTableVar table(  
    EmpID INT NOT NULL,  
    OldVacationHours INT,  
    NewVacationHours INT,  
    ModifiedDate datetime);  
UPDATE TOP (10) HumanResources.Employee  
SET VacationHours = VacationHours * 1.25   
OUTPUT INSERTED.BusinessEntityID,  
       DELETED.VacationHours,  
       INSERTED.VacationHours,  
       INSERTED.ModifiedDate  
INTO @MyTableVar;  
--Display the result set of the table variable.  
SELECT EmpID, OldVacationHours, NewVacationHours, ModifiedDate  
FROM @MyTableVar;  
GO  
--Display the result set of the table.  
--Note that ModifiedDate reflects the value generated by an  
--AFTER UPDATE trigger.  
SELECT TOP (10) BusinessEntityID, VacationHours, ModifiedDate  
FROM HumanResources.Employee;  
GO  
```  
  
### <a name="b-creating-an-inline-table-valued-function"></a>B. Creazione di una funzione inline con valori di tabella  
Nell'esempio seguente viene restituita una funzione inline con valori di tabella. L'esempio restituisce tre colonne `ProductID`, `Name` e l'aggregazione dei totali da inizio anno per negozio, come `YTD Total` per ogni prodotto venduto al negozio.
  
```sql
USE AdventureWorks2012;  
GO  
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
GO  
```  
  
Per richiamare la funzione, eseguire la query seguente.
  
```sql
SELECT * FROM Sales.ufn_SalesByStore (602);  
```  
  
## <a name="see-also"></a>Vedere anche
[COLLATE &#40;Transact-SQL&#41;](../statements/collations.md)  
[CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md)  
[Funzioni definite dall'utente](../../relational-databases/user-defined-functions/user-defined-functions.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[Usare parametri con valori di tabella &#40;Motore di database&#41;](../../relational-databases/tables/use-table-valued-parameters-database-engine.md)  
[Hint di query &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md)