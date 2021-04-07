---
description: Clausola OUTPUT (Transact-SQL)
title: Clausola OUTPUT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/14/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OUTPUT_TSQL
- OUTPUT
dev_langs:
- TSQL
helpviewer_keywords:
- displaying updated rows
- INSERT statement [SQL Server], OUTPUT clause
- outputs [SQL Server]
- OUTPUT clause
- row additions [SQL Server], OUTPUT clause
- viewing updated rows
- row deletions [SQL Server], OUTPUT clause
- viewing deleted rows
- DELETE statement [SQL Server], OUTPUT clause
- row updates [SQL Server]
- displaying inserted rows
- viewing inserted rows
- displaying deleted rows
- UPDATE statement [SQL Server], OUTPUT clause
ms.assetid: 41b9962c-0c71-4227-80a0-08fdc19f5fe4
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 194ce3acd5db4643053cde4b42bf5c0115ff9a37
ms.sourcegitcommit: 8f40e427c867b9d2e9e7dde43f9546fb4d907c4f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2021
ms.locfileid: "106506646"
---
# <a name="output-clause-transact-sql"></a>Clausola OUTPUT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce le informazioni da (o le espressioni basate su) ogni riga interessata da un'istruzione INSERT, UPDATE, DELETE o MERGE. Questi risultati possono essere restituiti all'applicazione di elaborazione per l'utilizzo nei messaggi di errore, l'archiviazione e altri scopi simili dell'applicazione. I risultati possono anche essere inseriti in una tabella o in una variabile di tabella. Inoltre, è possibile acquisire i risultati di una clausola OUTPUT in un'istruzione nidificata INSERT, UPDATE, DELETE o MERGE e inserire tali risultati in una vista o tabella di destinazione.  
  
> [!NOTE]  
>  Un'istruzione UPDATE, INSERT o DELETE in cui è presente una clausola OUTPUT restituirà righe al client anche se l'istruzione rileva errori e ne viene eseguito il rollback. Se durante l'esecuzione dell'istruzione si verifica un errore, il risultato non deve essere utilizzato.  
  
 **Usata in:**  
  
 [DELETE](../../t-sql/statements/delete-transact-sql.md)  
  
 [INSERT](../../t-sql/statements/insert-transact-sql.md)  
  
 [UPDATE](../../t-sql/queries/update-transact-sql.md)  
  
 [MERGE](../../t-sql/statements/merge-transact-sql.md)  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
<OUTPUT_CLAUSE> ::=  
{  
    [ OUTPUT <dml_select_list> INTO { @table_variable | output_table } [ ( column_list ) ] ]  
    [ OUTPUT <dml_select_list> ]  
}  
<dml_select_list> ::=  
{ <column_name> | scalar_expression } [ [AS] column_alias_identifier ]  
    [ ,...n ]  
  
<column_name> ::=  
{ DELETED | INSERTED | from_table_name } . { * | column_name }  
    | $action
```
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 \@*table_variable*  
 Specifica una variabile **table** in cui vengono inserite le righe restituite invece di essere restituite al chiamante. \@*table_variable* deve essere dichiarato prima dell'istruzione INSERT, UPDATE, DELETE o MERGE.  
  
 Se non si specifica *column_list*, la variabile **table** deve avere lo stesso numero di colonne del set dei risultati OUTPUT. Le eccezioni sono le colonne calcolate e Identity, le quali devono essere ignorate. Se *column_list* viene specificato, le colonne omesse devono consentire i valori Null oppure devono avere valori predefiniti assegnati.  
  
 Per altre informazioni sulle variabili [table](../../t-sql/data-types/table-transact-sql.md), vedere **table &#40;Transact-SQL&#41;** .  
  
 *output_table*  
 Specifica una tabella in cui vengono inserite le righe restituite invece di essere restituite al chiamante. *output_table* può essere una tabella temporanea.  
  
 Se non si specifica *column_list*, la tabella deve avere lo stesso numero di colonne del set dei risultati OUTPUT. Le eccezioni sono le colonne calcolate e Identity, che devono essere ignorate. Se *column_list* viene specificato, le colonne omesse devono consentire i valori Null oppure devono avere valori predefiniti assegnati.  
  
 *output_table* non può:  
  
-   avere trigger abilitati definiti  
  
-   Partecipare in un vincolo di chiave esterna  
  
-   avere regole abilitate o vincoli CHECK.  
  
*column_list*  
 Elenco facoltativo dei nomi di colonna nella tabella di destinazione della clausola INTO. È analogo all'elenco delle colonne consentite nell'istruzione [INSERT](../../t-sql/statements/insert-transact-sql.md).  
  
 *scalar_expression*  
 Qualsiasi combinazione di simboli e operatori che restituisce un unico valore. Le funzioni di aggregazione non sono consentite in *scalar_expression*.  
  
 Qualsiasi riferimento alle colonne della tabella che viene modificata deve essere qualificato con il prefisso INSERTED o DELETED.  
  
 *column_alias_identifier*  
 Nome alternativo utilizzato per fare riferimento al nome della colonna.  
  
 DELETED  
 Prefisso di colonna che specifica il valore eliminato dall'operazione di aggiornamento o di eliminazione. Le colonne con prefisso DELETED riflettono il valore prima del completamento dell'istruzione UPDATE, DELETE o MERGE.  
  
 DELETED non può essere utilizzato con la clausola OUTPUT nell'istruzione INSERT.  
  
 INSERTED  
 Prefisso di colonna che specifica il valore aggiunto dall'operazione di inserimento o aggiornamento. Le colonne con prefisso INSERTED riflettono il valore dopo il completamento dell'istruzione UPDATE, INSERT o MERGE ma prima dell'esecuzione dei trigger.  
  
 INSERTED non può essere utilizzato con la clausola OUTPUT nell'istruzione DELETE.  
  
 *from_table_name*  
 Prefisso di colonna che specifica una tabella inclusa nella clausola FROM di un'istruzione DELETE, UPDATE o MERGE utilizzata per specificare le righe da aggiornare o eliminare.  
  
 Se anche la tabella che viene modificata è specificata nella clausola FROM, qualsiasi riferimento alle colonne della tabella deve essere qualificato con il prefisso INSERTED o DELETED.  
  
 \*  
 Specifica che tutte le colonne interessate dall'azione di eliminazione, inserimento o aggiornamento saranno restituite nell'ordine in cui esistono nella tabella.  
  
 Ad esempio, `OUTPUT DELETED.*` nell'istruzione DELETE seguente restituisce tutte le colonne eliminate dalla tabella `ShoppingCartItem`:  
  
```sql
DELETE Sales.ShoppingCartItem
    OUTPUT DELETED.*;
```
  
 *column_name*  
 Riferimento di colonna esplicito. Qualsiasi riferimento alla tabella che viene modificata deve essere qualificato correttamente tramite il prefisso INSERTED o DELETED in base alle esigenze, ad esempio: INSERTED **.** _column\_name_.  
  
 $action  
 È disponibile solo per l'istruzione MERGE. Specifica una colonna di tipo **nvarchar(10)** nella clausola OUTPUT in un'istruzione MERGE che restituisce uno dei tre valori per ogni riga: 'INSERT', 'UPDATE' o 'DELETE', a seconda dell'azione eseguita su quella riga.  
  
## <a name="remarks"></a>Osservazioni  
 La clausola OUTPUT \<dml_select_list> e la clausola OUTPUT \<dml_select_list> INTO { **\@** _table\_variable_ | _output\_table_ } possono essere definite in un'unica istruzione INSERT, UPDATE, DELETE o MERGE.  
  
> [!NOTE]  
>  Se non specificato diversamente, i riferimenti alla clausola OUTPUT fanno riferimento a entrambe le clausole OUTPUT e OUTPUT INTO.  
  
 La clausola OUTPUT può risultare utile per recuperare il valore delle colonne calcolate o Identity dopo un'operazione di INSERT o UPDATE.  
  
 Quando una colonna calcolata è inclusa nell'elenco \<dml_select_list>, la colonna corrispondente nella tabella di output o nella variabile di tabella non è una colonna calcolata. I valori della nuova colonna corrispondono ai valori calcolati quando è stata eseguita l'istruzione.  
  
 Non vi è alcuna garanzia che l'ordine in cui vengono applicate le modifiche alla tabella e l'ordine in cui le righe vengono inserite nella tabella di output o variabile della tabella corrispondano.  
  
 Se le variabili o i parametri vengono modificati come parte di un'istruzione UPDATE, la clausola OUTPUT restituisce sempre il valore della variabile o del parametro prima dell'esecuzione dell'istruzione e non il valore modificato.  
  
 È possibile utilizzare OUTPUT con un'istruzione UPDATE o DELETE posizionata in un cursore che utilizza la sintassi WHERE CURRENT OF.  
  
 La clausola OUTPUT non è supportata nelle istruzioni seguenti:  
  
-   istruzioni DML che fanno riferimento a viste partizionate locali, viste partizionate distribuite o tabelle remote  
  
-   istruzioni INSERT che contengono un'istruzione EXECUTE.  
  
-   I predicati full-text non sono consentiti nella clausola OUTPUT quando il livello di compatibilità del database è impostato su 100.  
  
-   La clausola OUTPUT INTO non può essere utilizzata per l'inserimento in una vista o funzione di set di righe.  
  
-   Non è possibile creare una funzione definita dall'utente se contiene una clausola OUTPUT INTO con una tabella come destinazione.  
  
 Per impedire un comportamento non deterministico, la clausola OUTPUT non può contenere i riferimenti seguenti:  
  
-   Sottoquery o funzioni definite dall'utente che eseguono l'accesso ai dati dell'utente o di sistema o che si presume eseguano tale accesso. Si presuppone che le funzioni definite dall'utente eseguano l'accesso ai dati se non sono associati allo schema.  
  
-   Colonna di una vista o di una funzione inline con valori di tabella se tale colonna viene definita mediante uno dei metodi seguenti:  
  
    -   Sottoquery.  
  
    -   Funzione definita dall'utente che esegue, o si presume esegua, l'accesso ai dati dell'utente o di sistema.  
  
    -   Colonna calcolata che contiene una funzione definita dall'utente che esegue l'accesso ai dati dell'utente o di sistema nella relativa definizione.  
  
     Quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rileva una colonna di questo tipo nella clausola OUTPUT, viene generato l'errore 4186.   
  
## <a name="inserting-data-returned-from-an-output-clause-into-a-table"></a>Inserimento di dati restituiti da una clausola OUTPUT in una tabella  
 Quando si acquisiscono i risultati di una clausola OUTPUT in un'istruzione nidificata INSERT, UPDATE, DELETE o MERGE e si inseriscono tali risultati in una tabella di destinazione, tenere presente le considerazioni seguenti:  
  
-   L'intera operazione è di tipo atomico. È necessario eseguire sia l'istruzione INSERT esterna che l'istruzione DMl interna contenente la clausola OUTPUT. In caso contrario, l'intera istruzione avrà esito negativo.  
  
-   Alla destinazione dell'istruzione INSERT esterna si applicano le restrizioni seguenti:  
  
    -   La destinazione non può essere una tabella remota, una vista o un'espressione di tabella comune.  
  
    -   Nella destinazione non può essere presente un vincolo FOREIGN KEY né un vincolo FOREIGN KEY può fare riferimento alla destinazione stessa.  
  
    -   Nella destinazione non è possibile definire trigger.  
  
    -   La destinazione non può partecipare alla replica di tipo merge oppure a sottoscrizioni aggiornabili per la replica transazionale.  
  
-   All'istruzione DML nidificata si applicano le restrizioni seguenti:  
  
    -   La destinazione non può essere una tabella remota o una vista partizionata.  
  
    -   Nell'origine non può essere contenuta una clausola \<dml_table_source>.  
  
-   La clausola OUTPUT INTO non è supportata in istruzioni INSERT che contengono una clausola \<dml_table_source>.  
  
-   \@\@ROWCOUNT restituisce solo le righe inserite dall'istruzione INSERT esterna.  
  
-   \@\@IDENTITY, SCOPE_IDENTITY e IDENT_CURRENT restituiscono solo i valori Identity generati dall'istruzione DML annidata e non quelli generati dall'istruzione INSERT esterna.  
  
-   Le notifiche delle query considerano l'istruzione come entità singola e il tipo di qualsiasi messaggio creato sarà il tipo dell'istruzione DML nidificata, anche se la modifica significativa proviene dall'istruzione INSERT esterna stessa.  
  
-   Nella clausola \<dml_table_source> le clausole SELECT e WHERE non possono includere sottoquery, funzioni di aggregazione, funzioni di rango, predicati full-text, funzioni definite dall'utente che eseguono l'accesso ai dati o la funzione TEXTPTR.  

## <a name="parallelism"></a>Parallelismo
 Una clausola OUTPUT che restituisce i risultati al client usa sempre un piano seriale.

Nel contesto di un database impostato a un livello di compatibilità pari a 130 o superiore, se un'operazione INSERT...SELECT usa un hint WITH (TABLOCK) per l'istruzione SELECT e usa anche OUTPUT...INTO per l'inserimento in una tabella temporanea o di utente, la tabella di destinazione per INSERT...SELECT sarà idonea per il parallelismo a seconda del costo del sottoalbero.  La tabella di destinazione cui si fa riferimento nella clausola OUTPUT INTO non sarà idonea per il parallelismo. 
 
## <a name="triggers"></a>Trigger  
 Le colonne restituite da OUTPUT riflettono i dati dopo il completamento dell'istruzione INSERT, UPDATE o DELETE ma prima dell'esecuzione dei trigger.  
  
 Per i trigger INSTEAD OF, i risultati restituiti vengono generati come se le istruzioni INSERT, UPDATE o DELETE siano state effettivamente completate, anche se non si verifica alcuna modifica come risultato dell'operazione trigger. Se un'istruzione che include una clausola OUTPUT viene utilizzata all'interno del corpo di un trigger, gli alias di tabella devono essere utilizzati per fare riferimento al trigger inserted e alle tabelle deleted per evitare la duplicazione dei riferimenti di colonna con le tabelle INSERTED e DELETED associate a OUTPUT.  
  
 Se la clausola OUTPUT viene specificata senza specificare anche la parola chiave INTO, la destinazione dell'operazione DML non può avere trigger abilitati definiti per l'azione DML specificata. Ad esempio, se la clausola OUTPUT viene definita in un'istruzione UPDATE, la tabella di destinazione non può avere alcun trigger UPDATE abilitato.  
  
 Se è impostata l'opzione disallow results from triggers di sp_configure, una clausola OUTPUT senza clausola INTO provoca l'esito negativo dell'istruzione quando viene richiamata dall'interno di un trigger.  
  
## <a name="data-types"></a>Tipi di dati  
 La clausola OUTPUT supporta i tipi di dati Large Object: **nvarchar(max)** , **varchar(max)** , **varbinary(max)** , **text**, **ntext**, **image**, e **xml**. Quando si usa la clausola .WRITE nell'istruzione UPDATE per modificare una colonna di tipo **nvarchar(max)** , **varchar(max)** o **varbinary(max)** , vengono restituite le immagini complete precedenti e successive dei valori se contengono dei riferimenti. La funzione TEXTPTR( ) non può comparire in un'espressione in una colonna di tipo **text**, **ntext** o **image** nella clausola OUTPUT.  
  
## <a name="queues"></a>Code  
 È possibile utilizzare la clausola OUTPUT nelle applicazioni che utilizzano le tabelle come code oppure per mantenere i risultati intermedi delle query. In altre parole, l'applicazione aggiunge o rimuove costantemente le righe dalla tabella. Nell'esempio seguente viene utilizzata la clausola OUTPUT in un'istruzione DELETE per restituire la riga eliminata all'applicazione chiamante.  
  
```sql
USE AdventureWorks2012;
GO

DELETE TOP(1) dbo.DatabaseLog WITH (READPAST)  
OUTPUT deleted.*  
WHERE DatabaseLogID = 7;  
GO
```
  
 In questo esempio viene rimossa una riga da una tabella utilizzata come coda e i valori eliminati vengono restituiti all'applicazione di elaborazione in una singola azione. È possibile implementare anche altri tipi di semantica, ad esempio l'utilizzo di una tabella per implementare uno stack. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non garantisce tuttavia l'ordine in cui le righe vengono elaborate e restituite dalle istruzioni DML che utilizzano la clausola OUTPUT. Spetta all'applicazione includere una clausola WHERE appropriata per garantire la semantica desiderata oppure tenere in considerazione che quando più righe possono essere incluse nell'operazione DML, non viene garantito alcun ordine particolare. Nell'esempio seguente viene utilizzata una sottoquery e viene presupposto che l'unicità sia una caratteristica della colonna `DatabaseLogID` per implementare la semantica di ordinamento desiderata.  
  
```sql
USE tempdb;
GO

CREATE TABLE dbo.table1  
(  
    id INT,  
    employee VARCHAR(32)  
);  
GO  
  
INSERT INTO dbo.table1 VALUES   
      (1, 'Fred')  
     ,(2, 'Tom')  
     ,(3, 'Sally')  
     ,(4, 'Alice');  
GO  
  
DECLARE @MyTableVar TABLE  
(  
    id INT,  
    employee VARCHAR(32)  
);  
  
PRINT 'table1, before delete'   
SELECT * FROM dbo.table1;  
  
DELETE FROM dbo.table1  
OUTPUT DELETED.* INTO @MyTableVar  
WHERE id = 4 OR id = 2;  
  
PRINT 'table1, after delete'  
SELECT * FROM dbo.table1;  
  
PRINT '@MyTableVar, after delete'  
SELECT * FROM @MyTableVar;  
  
DROP TABLE dbo.table1;  
```

```
--Results  
--table1, before delete  
--id          employee  
------------- ------------------------------  
--1           Fred  
--2           Tom  
--3           Sally  
--4           Alice  
--  
--table1, after delete  
--id          employee  
------------- ------------------------------  
--1           Fred  
--3           Sally  
--@MyTableVar, after delete  
--id          employee  
------------- ------------------------------  
--2           Tom  
--4           Alice
```
  
> [!NOTE]  
>  Utilizzare l'hint di tabella READPAST nelle istruzioni UPDATE e DELETE se lo scenario consente a più applicazioni di eseguire una operazione di lettura distruttiva da una tabella. Ciò evita i problemi relativi ai blocchi che possono verificarsi se un'altra applicazione sta già leggendo il primo record qualificato nella tabella.  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni SELECT sono necessarie in ogni colonna recuperata tramite \<dml_select_list> o usata in \<scalar_expression>.  
  
 Le autorizzazioni INSERT sono necessarie in ogni tabella specificata in \<output_table>.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-output-into-with-a-simple-insert-statement"></a>R. Utilizzo di OUTPUT INTO con un'istruzione INSERT semplice  
 Nell'esempio seguente viene inserita una riga nella tabella `ScrapReason` e viene utilizzata la clausola `OUTPUT` per restituire i risultati dell'istruzione alla variabile di tabella `@MyTableVar`. Poiché la colonna `ScrapReasonID` è definita con una proprietà IDENTITY, non è specificato un valore nell'istruzione `INSERT` per quella colonna. Si noti tuttavia che il valore generato da [!INCLUDE[ssDE](../../includes/ssde-md.md)] per quella colonna viene restituito nella clausola `OUTPUT` nella colonna `inserted.ScrapReasonID`.  

  
```sql
USE AdventureWorks2012;
GO

DECLARE @MyTableVar TABLE (
    NewScrapReasonID SMALLINT,  
    Name VARCHAR(50),  
    ModifiedDate DATETIME); 
    
INSERT Production.ScrapReason  
    OUTPUT INSERTED.ScrapReasonID, INSERTED.Name, INSERTED.ModifiedDate  
        INTO @MyTableVar  
VALUES (N'Operator error', GETDATE());  
  
--Display the result set of the table variable.  
SELECT NewScrapReasonID, Name, ModifiedDate FROM @MyTableVar;  
--Display the result set of the table.  
SELECT ScrapReasonID, Name, ModifiedDate   
FROM Production.ScrapReason;  
GO
```
  
### <a name="b-using-output-with-a-delete-statement"></a>B. Utilizzo di OUTPUT con un'istruzione DELETE  
 Nell'esempio seguente vengono eliminate tutte le righe nella tabella `ShoppingCartItem`. La clausola `OUTPUT deleted.*` specifica che i risultati dell'istruzione `DELETE`, ovvero tutte le colonne nelle righe eliminate, devono essere restituiti all'applicazione chiamante. L'istruzione `SELECT` che segue verifica i risultati dell'operazione di eliminazione nella tabella `ShoppingCartItem`.  
  
```sql
USE AdventureWorks2012;
GO

DELETE Sales.ShoppingCartItem  
OUTPUT DELETED.*   
WHERE ShoppingCartID = 20621;  
  
--Verify the rows in the table matching the WHERE clause have been deleted.  
SELECT COUNT(*) AS [Rows in Table] FROM Sales.ShoppingCartItem WHERE ShoppingCartID = 20621;  
GO
```
  
### <a name="c-using-output-into-with-an-update-statement"></a>C. Utilizzo di OUTPUT INTO con un'istruzione UPDATE  
 Nell'esempio seguente viene aggiornata la colonna `VacationHours` nella tabella `Employee` del 25% per le prime 10 righe. La clausola `OUTPUT` restituisce il valore `VacationHours` esistente prima di applicare l'istruzione `UPDATE` nella colonna `deleted.VacationHours` e il valore aggiornato nella colonna `inserted.VacationHours` alla variabile di tabella `@MyTableVar`.  
  
 Questa variabile è seguita da due istruzioni `SELECT` che restituiscono i valori in `@MyTableVar` e i risultati dell'operazione di aggiornamento nella tabella `Employee`.  
  
```sql
USE AdventureWorks2012;
GO
  
DECLARE @MyTableVar TABLE (  
    EmpID INT NOT NULL,  
    OldVacationHours INT,  
    NewVacationHours INT,  
    ModifiedDate DATETIME);  
  
UPDATE TOP (10) HumanResources.Employee  
SET VacationHours = VacationHours * 1.25,  
    ModifiedDate = GETDATE()   
OUTPUT inserted.BusinessEntityID,  
       deleted.VacationHours,  
       inserted.VacationHours,  
       inserted.ModifiedDate  
INTO @MyTableVar;  
  
--Display the result set of the table variable.  
SELECT EmpID, OldVacationHours, NewVacationHours, ModifiedDate  
FROM @MyTableVar;  
GO  
--Display the result set of the table.  
SELECT TOP (10) BusinessEntityID, VacationHours, ModifiedDate  
FROM HumanResources.Employee;  
GO  
```

### <a name="d-using-output-into-to-return-an-expression"></a>D. Utilizzo di OUTPUT INTO per restituire un'espressione  
 Nell'esempio seguente viene utilizzato come base l'esempio C definendo un'espressione nella clausola `OUTPUT` come differenza tra il valore `VacationHours` aggiornato e il valore `VacationHours` prima dell'applicazione dell'aggiornamento. Il valore di questa espressione viene restituito alla `@MyTableVar` variabile di tabella nella colonna `VacationHoursDifference` .  

  
```sql
USE AdventureWorks2012;  
GO

DECLARE @MyTableVar TABLE (  
    EmpID INT NOT NULL,  
    OldVacationHours INT,  
    NewVacationHours INT,  
    VacationHoursDifference INT,  
    ModifiedDate DATETIME);  
  
UPDATE TOP (10) HumanResources.Employee  
SET VacationHours = VacationHours * 1.25,  
    ModifiedDate = GETDATE()  
OUTPUT inserted.BusinessEntityID,  
       deleted.VacationHours,  
       inserted.VacationHours,  
       inserted.VacationHours - deleted.VacationHours,  
       inserted.ModifiedDate  
INTO @MyTableVar;  
  
--Display the result set of the table variable.  
SELECT EmpID, OldVacationHours, NewVacationHours,   
    VacationHoursDifference, ModifiedDate  
FROM @MyTableVar;  
GO  
SELECT TOP (10) BusinessEntityID, VacationHours, ModifiedDate  
FROM HumanResources.Employee;  
GO
```

### <a name="e-using-output-into-with-from_table_name-in-an-update-statement"></a>E. Utilizzo di OUTPUT INTO con from_table_name in un'istruzione UPDATE  
 Nell'esempio seguente viene aggiornata la colonna `ScrapReasonID` nella tabella `WorkOrder` per tutti gli ordini di lavoro con valori `ProductID` e `ScrapReasonID` specificati. La clausola `OUTPUT INTO` restituisce i valori dalla tabella in fase di aggiornamento (`WorkOrder`) e anche dalla tabella `Product`. La tabella `Product` viene utilizzata nella clausola `FROM` per specificare le righe da aggiornare. Poiché per la tabella `WorkOrder` è stato definito un trigger `AFTER UPDATE`, è necessaria la parola chiave `INTO`.  
  
```sql
USE AdventureWorks2012;
GO

DECLARE @MyTestVar TABLE (  
    OldScrapReasonID INT NOT NULL,   
    NewScrapReasonID INT NOT NULL,   
    WorkOrderID INT NOT NULL,  
    ProductID INT NOT NULL,  
    ProductName NVARCHAR(50)NOT NULL);  
  
UPDATE Production.WorkOrder  
SET ScrapReasonID = 4  
OUTPUT deleted.ScrapReasonID,  
       inserted.ScrapReasonID,   
       inserted.WorkOrderID,  
       inserted.ProductID,  
       p.Name  
    INTO @MyTestVar  
FROM Production.WorkOrder AS wo  
    INNER JOIN Production.Product AS p   
    ON wo.ProductID = p.ProductID   
    AND wo.ScrapReasonID= 16  
    AND p.ProductID = 733;  
  
SELECT OldScrapReasonID, NewScrapReasonID, WorkOrderID,   
    ProductID, ProductName   
FROM @MyTestVar;  
GO
```

### <a name="f-using-output-into-with-from_table_name-in-a-delete-statement"></a>F. Utilizzo di OUTPUT INTO con from_table_name in un'istruzione DELETE  
 Nell'esempio seguente vengono eliminate le righe nella tabella `ProductProductPhoto` in base ai criteri di ricerca definiti nella clausola `FROM` dell'istruzione `DELETE`. La clausola `OUTPUT` restituisce le colonne dalla tabella che viene eliminata (`deleted.ProductID`, `deleted.ProductPhotoID`) e dalla tabella `Product`. Questa tabella viene utilizzata nella clausola `FROM` per specificare le righe da eliminare.  
  
```sql
USE AdventureWorks2012;
GO

DECLARE @MyTableVar TABLE (  
    ProductID INT NOT NULL,   
    ProductName NVARCHAR(50)NOT NULL,  
    ProductModelID INT NOT NULL,   
    PhotoID INT NOT NULL);  
  
DELETE Production.ProductProductPhoto  
OUTPUT DELETED.ProductID,  
       p.Name,  
       p.ProductModelID,  
       DELETED.ProductPhotoID  
    INTO @MyTableVar  
FROM Production.ProductProductPhoto AS ph  
JOIN Production.Product as p   
    ON ph.ProductID = p.ProductID   
    WHERE p.ProductModelID BETWEEN 120 and 130;  
  
--Display the results of the table variable.  
SELECT ProductID, ProductName, ProductModelID, PhotoID   
FROM @MyTableVar  
ORDER BY ProductModelID;  
GO
```
  
### <a name="g-using-output-into-with-a-large-object-data-type"></a>G. Utilizzo di OUTPUT INTO con un tipo di dati LOB  
 Nell'esempio seguente viene aggiornato un valore parziale in `DocumentSummary`, una colonna di tipo `nvarchar(max)` della tabella `Production.Document`, tramite la clausola `.WRITE`. La parola `components` viene sostituta con la parola `features` specificando la parola sostitutiva, la posizione iniziale (offset) della parola da sostituire nei dati esistenti e il numero di caratteri da sostituire (lunghezza). Nell'esempio viene utilizzata la `OUTPUT` clausola per restituire le immagini before e After della `DocumentSummary` colonna alla `@MyTableVar` variabile table. Si noti che vengono restituite le immagini complete precedenti e successive della colonna `DocumentSummary`.  

  
```sql
USE AdventureWorks2012;
GO

DECLARE @MyTableVar TABLE (  
    SummaryBefore NVARCHAR(max),  
    SummaryAfter NVARCHAR(max));  
  
UPDATE Production.Document  
SET DocumentSummary .WRITE (N'features',28,10)  
OUTPUT deleted.DocumentSummary,   
       inserted.DocumentSummary   
    INTO @MyTableVar  
WHERE Title = N'Front Reflector Bracket Installation';  
  
SELECT SummaryBefore, SummaryAfter   
FROM @MyTableVar;  
GO
```
  
### <a name="h-using-output-in-an-instead-of-trigger"></a>H. Utilizzo di OUTPUT in un trigger INSTEAD OF  
 Nell'esempio seguente viene utilizzata la clausola `OUTPUT` in un trigger per restituire i risultati dell'operazione trigger. Viene prima creata una vista nella tabella `ScrapReason` e quindi definito un trigger `INSTEAD OF INSERT` nella vista che consente all'utente di modificare esclusivamente la colonna `Name` della tabella di base. Poiché la colonna `ScrapReasonID` è una colonna `IDENTITY` nella tabella di base, il trigger ignora il valore specificato dall'utente, consentendo a [!INCLUDE[ssDE](../../includes/ssde-md.md)] di generare automaticamente il valore corretto. Inoltre, il valore specificato dall'utente per `ModifiedDate` viene ignorato e impostato sulla data corrente. La clausola `OUTPUT` restituisce i valori di fatto inseriti nella tabella `ScrapReason`.  
  
```sql
USE AdventureWorks2012;
GO

IF OBJECT_ID('dbo.vw_ScrapReason','V') IS NOT NULL  
    DROP VIEW dbo.vw_ScrapReason;  
GO  
CREATE VIEW dbo.vw_ScrapReason  
AS (SELECT ScrapReasonID, Name, ModifiedDate  
    FROM Production.ScrapReason);  
GO  
CREATE TRIGGER dbo.io_ScrapReason   
    ON dbo.vw_ScrapReason  
INSTEAD OF INSERT  
AS  
BEGIN  
--ScrapReasonID is not specified in the list of columns to be inserted   
--because it is an IDENTITY column.  
    INSERT INTO Production.ScrapReason (Name, ModifiedDate)  
        OUTPUT INSERTED.ScrapReasonID, INSERTED.Name,   
               INSERTED.ModifiedDate  
    SELECT Name, getdate()  
    FROM inserted;  
END  
GO  
INSERT vw_ScrapReason (ScrapReasonID, Name, ModifiedDate)  
VALUES (99, N'My scrap reason','20030404');  
GO
```
  
 Di seguito è riportato il set di risultati generato il 12 aprile 2004 ('`2004-04-12'`). Si noti che le colonne `ScrapReasonIDActual` e `ModifiedDate` riflettono i valori generati dall'operazione trigger invece dei valori specificati nell'istruzione `INSERT`.  
  
 ```
 ScrapReasonID  Name             ModifiedDate  
 -------------  ---------------- -----------------------  
 17             My scrap reason  2004-04-12 16:23:33.050
 ```  
  
### <a name="i-using-output-into-with-identity-and-computed-columns"></a>I. Utilizzo di OUTPUT INTO con le colonne calcolate e Identity  
 Nell'esempio seguente viene creata la tabella `EmployeeSales`, in cui vengono quindi inserite diverse righe tramite un'istruzione `INSERT` con un'istruzione `SELECT` per il recupero dei dati dalle tabelle di origine. La tabella `EmployeeSales` include una colonna Identity (`EmployeeID`) e una colonna calcolata (`ProjectedSales`).  
  
```sql
USE AdventureWorks2012;
GO

IF OBJECT_ID ('dbo.EmployeeSales', 'U') IS NOT NULL  
    DROP TABLE dbo.EmployeeSales;  
GO  
CREATE TABLE dbo.EmployeeSales  
( EmployeeID   INT IDENTITY (1,5) NOT NULL,  
  LastName     NVARCHAR(20) NOT NULL,  
  FirstName    NVARCHAR(20) NOT NULL,  
  CurrentSales MONEY NOT NULL,  
  ProjectedSales AS CurrentSales * 1.10   
);  
GO  
DECLARE @MyTableVar TABLE (  
  EmployeeID   INT NOT NULL,  
  LastName     NVARCHAR(20) NOT NULL,  
  FirstName    NVARCHAR(20) NOT NULL,  
  CurrentSales MONEY NOT NULL,  
  ProjectedSales MONEY NOT NULL  
  );  
  
INSERT INTO dbo.EmployeeSales (LastName, FirstName, CurrentSales)  
  OUTPUT INSERTED.EmployeeID,
         INSERTED.LastName,   
         INSERTED.FirstName,   
         INSERTED.CurrentSales,
         INSERTED.ProjectedSales
  INTO @MyTableVar  
    SELECT c.LastName, c.FirstName, sp.SalesYTD  
    FROM Sales.SalesPerson AS sp  
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.BusinessEntityID LIKE '2%'  
    ORDER BY c.LastName, c.FirstName;  
  
SELECT EmployeeID, LastName, FirstName, CurrentSales, ProjectedSales  
FROM @MyTableVar;  
GO  
SELECT EmployeeID, LastName, FirstName, CurrentSales, ProjectedSales  
FROM dbo.EmployeeSales;  
GO
```
  
### <a name="j-using-output-and-output-into-in-a-single-statement"></a>J. Utilizzo di OUTPUT e OUTPUT INTO in una singola istruzione  
 Nell'esempio seguente vengono eliminate le righe nella tabella `ProductProductPhoto` in base ai criteri di ricerca definiti nella clausola `FROM` dell'istruzione `DELETE`. La `OUTPUT INTO` clausola restituisce le colonne dalla tabella che viene eliminata ( `deleted.ProductID` , `deleted.ProductPhotoID` ) e dalla tabella `Product` alla `@MyTableVar` variabile di tabella. La tabella `Product` viene utilizzata nella clausola `FROM` per specificare le righe da eliminare. La clausola `OUTPUT` restituisce all'applicazione chiamante le colonne `deleted.ProductID`, `deleted.ProductPhotoID` e la data e l'ora in cui la riga è stata eliminata dalla tabella `ProductProductPhoto`.  

  
```sql
USE AdventureWorks2012;
GO

DECLARE @MyTableVar TABLE (  
    ProductID INT NOT NULL,   
    ProductName NVARCHAR(50)NOT NULL,  
    ProductModelID INT NOT NULL,   
    PhotoID INT NOT NULL);  
  
DELETE Production.ProductProductPhoto  
OUTPUT DELETED.ProductID,  
       p.Name,  
       p.ProductModelID,  
       DELETED.ProductPhotoID  
    INTO @MyTableVar  
OUTPUT DELETED.ProductID, DELETED.ProductPhotoID, GETDATE() AS DeletedDate   
FROM Production.ProductProductPhoto AS ph  
JOIN Production.Product as p   
    ON ph.ProductID = p.ProductID   
WHERE p.ProductID BETWEEN 800 and 810;  
  
--Display the results of the table variable.  
SELECT ProductID, ProductName, PhotoID, ProductModelID   
FROM @MyTableVar;  
GO
```
  
### <a name="k-inserting-data-returned-from-an-output-clause"></a>K. Inserimento dei dati restituiti da una clausola OUTPUT  
 Nell'esempio seguente vengono acquisiti i dati restituiti dalla clausola `OUTPUT` di un'istruzione `MERGE` e tali dati vengono inseriti in un'altra tabella. L'istruzione `MERGE` aggiorna la colonna `Quantity` della tabella `ProductInventory` su base giornaliera, in base agli ordini elaborati nella tabella `SalesOrderDetail`. Vengono inoltre eliminate le righe dei prodotti le cui scorte scendono a `0` oppure a un valore inferiore. In questo esempio vengono acquisite le righe eliminate, che vengono inserite in un'altra tabella, `ZeroInventory`, in cui viene tenuta traccia dei prodotti senza scorte.  
  
```sql
USE AdventureWorks2012;
GO

IF OBJECT_ID(N'Production.ZeroInventory', N'U') IS NOT NULL  
    DROP TABLE Production.ZeroInventory;  
GO  
--Create ZeroInventory table.  
CREATE TABLE Production.ZeroInventory (DeletedProductID int, RemovedOnDate DateTime);  
GO  
  
INSERT INTO Production.ZeroInventory (DeletedProductID, RemovedOnDate)  
SELECT ProductID, GETDATE()  
FROM  
(   MERGE Production.ProductInventory AS pi  
    USING (SELECT ProductID, SUM(OrderQty) FROM Sales.SalesOrderDetail AS sod  
           JOIN Sales.SalesOrderHeader AS soh  
           ON sod.SalesOrderID = soh.SalesOrderID  
           AND soh.OrderDate = '20070401'  
           GROUP BY ProductID) AS src (ProductID, OrderQty)  
    ON (pi.ProductID = src.ProductID)  
    WHEN MATCHED AND pi.Quantity - src.OrderQty <= 0  
        THEN DELETE  
    WHEN MATCHED  
        THEN UPDATE SET pi.Quantity = pi.Quantity - src.OrderQty  
    OUTPUT $action, deleted.ProductID) AS Changes (Action, ProductID)  
WHERE Action = 'DELETE';  
IF @@ROWCOUNT = 0  
PRINT 'Warning: No rows were inserted';  
GO  
SELECT DeletedProductID, RemovedOnDate
FROM Production.ZeroInventory;
GO
```
  
## <a name="see-also"></a>Vedere anche  
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)   
 [table &#40;Transact-SQL&#41;](../../t-sql/data-types/table-transact-sql.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
