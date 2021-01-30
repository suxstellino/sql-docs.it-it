---
description: Costruttore di valori di tabella (Transact-SQL)
title: Costruttore di valori di tabella (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- inserting multiple rows
- row value expression
- row constructor [SQL Server]
- table value constructor [SQL Server]
ms.assetid: e57cd31d-140e-422f-8178-2761c27b9deb
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 32074a8c0229ace53d636f8ef9bb31b8caf729b8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200439"
---
# <a name="table-value-constructor-transact-sql"></a>Costruttore di valori di tabella (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Specifica un set di espressioni valore di riga da costruire in una tabella. Il costruttore di valori di tabella [!INCLUDE[tsql](../../includes/tsql-md.md)] consente di specificare più righe di dati in una sola istruzione DML. È possibile specificare il costruttore di valori di tabella come clausola VALUES di un'istruzione INSERT ... VALUES oppure come tabella derivata nella clausola USING dell'istruzione MERGE o nella clausola FROM.
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
VALUES ( <row value expression list> ) [ ,...n ]   
  
<row value expression list> ::=  
    {<row value expression> } [ ,...n ]  
  
<row value expression> ::=  
    { DEFAULT | NULL | expression }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 VALUES  
 Introduce gli elenchi di espressioni valore di riga. Ogni elenco deve essere racchiuso tra parentesi e separato da una virgola.  
  
 Il numero di valori specificato in ciascun elenco deve essere uguale e i valori devono essere nello stesso ordine delle colonne nella tabella. Deve essere specificato un valore per ogni colonna della tabella oppure nell'elenco delle colonne devono essere specificate in modo esplicito le colonne per ciascun valore inserito.  
  
 DEFAULT  
 Forza l'inserimento nel [!INCLUDE[ssDE](../../includes/ssde-md.md)] del valore predefinito di una colonna. Se per la colonna non è disponibile alcun valore predefinito e la colonna ammette valori NULL, viene inserito un valore NULL. DEFAULT non è un valore valido per una colonna Identity. Se specificato in un costruttore di valori di tabella, DEFAULT è consentito solo in un'istruzione INSERT.  
  
 *expression*  
 Costante, variabile o espressione. L'espressione non può contenere un'istruzione EXECUTE.  
  
## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni  
 Quando usato come tabella derivata non esiste alcun limite per il numero di righe.  
 
 Quando usato come clausola VALUES di un'istruzione INSERT ... VALUES esiste un limite di 1000 righe. Se il numero massimo di righe viene superato, viene restituito l'errore 10738. Per inserire più di 1000 righe, utilizzare uno dei metodi seguenti:  
  
- Creare più istruzioni INSERT  
  
- Utilizzare una tabella derivata  
  
- Importare globalmente i dati tramite l'[utilità **bcp**](../../tools/bcp-utility.md), la [classe SqlBulkCopy](/dotnet/api/system.data.sqlclient.sqlbulkcopy) .NET, [OPENROWSET (BULK ...)](../../t-sql/functions/openrowset-transact-sql.md) o l'istruzione [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md)  
  
 Come espressione valore di riga sono consentiti solo valori scalari singoli. Come espressione valore di riga non è consentita una sottoquery che interessa più colonne. Ad esempio, il codice seguente comporta un errore di sintassi perché il terzo elenco di espressioni valore di riga contiene una sottoquery con più colonne.  
  
```sql
USE AdventureWorks2012;  
GO  
CREATE TABLE dbo.MyProducts (Name VARCHAR(50), ListPrice MONEY);  
GO  
-- This statement fails because the third values list contains multiple columns in the subquery.  
INSERT INTO dbo.MyProducts (Name, ListPrice)  
VALUES ('Helmet', 25.50),  
       ('Wheel', 30.00),  
       (SELECT Name, ListPrice FROM Production.Product WHERE ProductID = 720);  
GO  
```  
  
 L'istruzione può tuttavia essere riscritta specificando ogni colonna separatamente nella sottoquery. Nell'esempio seguente vengono inserite tre righe nella tabella `MyProducts` senza errori.  
  
```sql
INSERT INTO dbo.MyProducts (Name, ListPrice)  
VALUES ('Helmet', 25.50),  
       ('Wheel', 30.00),  
       ((SELECT Name FROM Production.Product WHERE ProductID = 720),  
        (SELECT ListPrice FROM Production.Product WHERE ProductID = 720));  
GO  
```  
  
## <a name="data-types"></a>Tipi di dati  
 I valori specificati in un'istruzione INSERT con più righe seguono le proprietà di conversione del tipo di dati della sintassi UNION ALL. Ciò comporta la conversione implicita dei tipi non corrispondenti nel tipo con [precedenza](../../t-sql/data-types/data-type-precedence-transact-sql.md) maggiore. Se la conversione non è una conversione implicita supportata, viene generato un errore. Nell'istruzione seguente, ad esempio, vengono inseriti un valore integer e un valore di tipo char in una colonna di tipo **char**.  
  
```sql
CREATE TABLE dbo.t (a INT, b CHAR);  
GO  
INSERT INTO dbo.t VALUES (1,'a'), (2, 1);  
GO  
```  
  
 Quando l'istruzione INSERT viene eseguita, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene effettuato il tentativo di convertire "a" in un numero intero, in quanto la precedenza relativa al tipo di dati indica che un tipo integer è superiore a un tipo char. La conversione non riesce e viene restituito un errore. Per evitare l'errore, è possibile convertire esplicitamente i valori in modo appropriato. L'istruzione precedente, ad esempio, può essere scritta nel modo seguente:  
  
```sql
INSERT INTO dbo.t VALUES (1,'a'), (2, CONVERT(CHAR,1));  
```  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-inserting-multiple-rows-of-data"></a>R. Inserimento di più righe di dati  
 Nell'esempio seguente viene creata la tabella `dbo.Departments` e successivamente viene utilizzato il costruttore di valori di tabella per inserire cinque righe nella tabella stessa. Poiché i valori per tutte le colonne vengono specificati ed elencati nello stesso ordine delle colonne nella tabella, non è necessario specificare i nomi delle colonne nell'elenco delle colonne.  
  
```sql
USE AdventureWorks2012;  
GO  
INSERT INTO Production.UnitMeasure  
VALUES (N'FT2', N'Square Feet ', '20080923'), (N'Y', N'Yards', '20080923'),
       (N'Y3', N'Cubic Yards', '20080923');  
GO  
```  
  
### <a name="b-inserting-multiple-rows-with-default-and-null-values"></a>B. Inserimento di più righe con valori DEFAULT e NULL  
 Nell'esempio seguente viene illustrata la specifica di valori DEFAULT e NULL quando si utilizza il costruttore di valori di tabella per inserire righe in una tabella.  
  
```sql
USE AdventureWorks2012;  
GO  
CREATE TABLE Sales.MySalesReason(  
SalesReasonID int IDENTITY(1,1) NOT NULL,  
Name dbo.Name NULL ,  
ReasonType dbo.Name NOT NULL DEFAULT 'Not Applicable' );  
GO  
INSERT INTO Sales.MySalesReason   
VALUES ('Recommendation','Other'), ('Advertisement', DEFAULT), (NULL, 'Promotion');  
  
SELECT * FROM Sales.MySalesReason;  
```  
  
### <a name="c-specifying-multiple-values-as-a-derived-table-in-a-from-clause"></a>C. Specifica di più valori come tabella derivata in una clausola FROM  
 Negli esempi seguenti viene usato il costruttore di valori di tabella per specificare più valori nella clausola FROM di un'istruzione SELECT.  
  
```sql
SELECT a, b FROM (VALUES (1, 2), (3, 4), (5, 6), (7, 8), (9, 10) ) AS MyTable(a, b);  
GO  
-- Used in an inner join to specify values to return.  
SELECT ProductID, a.Name, Color  
FROM Production.Product AS a  
INNER JOIN (VALUES ('Blade'), ('Crown Race'), ('AWC Logo Cap')) AS b(Name)   
ON a.Name = b.Name;  
```  
  
### <a name="d-specifying-multiple-values-as-a-derived-source-table-in-a-merge-statement"></a>D. Specifica di più valori come tabella di origine derivata in un'istruzione MERGE  
 Nell'esempio seguente viene utilizzata l'istruzione MERGE per modificare la tabella `SalesReason` eseguendo l'aggiornamento o l'inserimento di righe. Quando il valore di `NewName` nella tabella di origine corrisponde a un valore della colonna `Name` nella tabella di destinazione (`SalesReason`), la colonna `ReasonType` viene aggiornata nella tabella di destinazione. Quando il valore di `NewName` non corrisponde, la riga di origine viene inserita nella tabella di destinazione. La tabella di origine è una tabella derivata che utilizza il costruttore di valori di tabella [!INCLUDE[tsql](../../includes/tsql-md.md)] per specificare più righe per la tabella di origine.  
  
```sql
USE AdventureWorks2012;  
GO  
-- Create a temporary table variable to hold the output actions.  
DECLARE @SummaryOfChanges TABLE(Change VARCHAR(20));  
  
MERGE INTO Sales.SalesReason AS Target  
USING (VALUES ('Recommendation','Other'), ('Review', 'Marketing'), ('Internet', 'Promotion'))  
       AS Source (NewName, NewReasonType)  
ON Target.Name = Source.NewName  
WHEN MATCHED THEN  
UPDATE SET ReasonType = Source.NewReasonType  
WHEN NOT MATCHED BY TARGET THEN  
INSERT (Name, ReasonType) VALUES (NewName, NewReasonType)  
OUTPUT $action INTO @SummaryOfChanges;  
  
-- Query the results of the table variable.  
SELECT Change, COUNT(*) AS CountPerChange  
FROM @SummaryOfChanges  
GROUP BY Change;  
```  
  
### <a name="e-inserting-more-than-1000-rows"></a>E. Inserimento di più di 1000 righe
  L'esempio seguente illustra l'uso del costruttore di valori di tabella come tabella derivata. Ciò consente l'inserimento di più di 1000 righe da un singolo costruttore di valori di tabella.
  
```sql
CREATE TABLE dbo.Test ([Value] INT);  
  
INSERT INTO dbo.Test ([Value])  
  SELECT drvd.[NewVal]
  FROM   (VALUES (0), (1), (2), (3), ..., (5000)) drvd([NewVal]);
```  


## <a name="see-also"></a>Vedere anche  
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [MERGE &#40;Transact-SQL&#41;](../../t-sql/statements/merge-transact-sql.md)   
 [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)  
  
  
