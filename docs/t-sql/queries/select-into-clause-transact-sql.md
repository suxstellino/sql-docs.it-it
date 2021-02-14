---
description: Clausola SELECT - INTO (Transact-SQL)
title: Clausola INTO (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- INTO_TSQL
- INSERT_INTO_TSQL
- INSERT INTO
- INTO
- INTO clause
- INTO_clause_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- copying data [SQL Server], into a new table
- INTO clause
- moving data, to a new table
- table creation [SQL Server], INTO clause
- SELECT INTO statement
- inserting rows
- clauses [SQL Server], INTO
- row additions [SQL Server], INTO clause
ms.assetid: b48d69e8-5a00-48bf-b2f3-19278a72dd88
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4a12bbc7b4f8d4178217b09b0a3a08233ab6892c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347925"
---
# <a name="select---into-clause-transact-sql"></a>Clausola SELECT - INTO (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

SELECT...INTO crea una nuova tabella nel filegroup predefinito e vi inserisce le righe restituite dalla query. Per visualizzare la sintassi SELECT completa, vedere [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md).  
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
[ INTO new_table ]
[ ON filegroup ]
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *new_table*   
 Specifica il nome di una nuova tabella da creare in base alle colonne dell'elenco di selezione e alle righe scelte dall'origine dati.  
 
 Il formato dell'argomento *new_table* viene determinato tramite la valutazione delle espressioni nell'elenco di selezione. Le colonne di *new_table* vengono create nell'ordine indicato nell'elenco di selezione. A ogni colonna di *new_table* vengono assegnati lo stesso nome, lo stesso tipo di dati, il supporto dei valori Null e lo stesso valore dell'espressione corrispondente nell'elenco di selezione. La proprietà IDENTITY di una colonna viene trasferita, tranne nelle condizioni definite in "Utilizzo di colonne Identity" nella sezione Osservazioni.  
  
 Per creare la tabella in un altro database della stessa istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], specificare *new_table* come nome completo nel formato *database.schema.table_name*.  
  
 Non è possibile creare *new_table* in un server remoto. È tuttavia possibile popolare *new_table* da un'origine dati remota. Per creare *new_table* da una tabella di origine remota, specificare la tabella di origine usando un nome in quattro parti nel formato *linked_server*.*catalog*.*schema*.*object* nella clausola FROM dell'istruzione SELECT. In alternativa, è possibile usare la funzione [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md) o [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md) nella clausola FROM per specificare l'origine dati remota.  
 
 *filegroup*    
 Specifica il nome del filegroup in cui verrà creata la nuova tabella. Il filegroup specificato deve esistere nel database anche se il motore di SQL Server genera un errore.   
 
 **Si applica a:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 e versioni successive.
  
## <a name="data-types"></a>Tipi di dati  
 L'attributo FILESTREAM non viene trasferito nella nuova tabella. Gli oggetti binari di grandi dimensioni FILESTREAM vengono copiati e archiviati nella nuova tabella come oggetti binari di grandi dimensioni di tipo **varbinary(max)** . Senza l'attributo FILESTREAM, il tipo di dati **varbinary(max)** è soggetto al limite di 2 GB. Se un oggetto BLOB FILESTREAM supera questo valore, viene generato l'errore 7119 e l'istruzione viene arrestata.  
  
 Quando viene selezionata una colonna Identity esistente in una nuova tabella, la nuova colonna eredita la proprietà IDENTITY, a meno che non si verifichi una delle seguenti condizioni:  
  
-   L'istruzione SELECT contiene un join.  
  
-   Più istruzioni SELECT sono unite in join tramite l'operatore UNION.  
  
-   La colonna Identity è inclusa più di una volta nell'elenco di selezione.  
  
-   La colonna Identity fa parte di un'espressione.  
  
-   La colonna Identity proviene da un'origine dei dati remota.  
  
Se una di queste condizioni risulta vera, la colonna viene creata come colonna NOT NULL, anziché ereditare la proprietà IDENTITY. Se una colonna Identity è richiesta nella nuova tabella ma tale colonna non è disponibile o si desidera un valore di inizializzazione o di incremento diverso della colonna Identity di origine, definire la colonna nell'elenco di selezione utilizzando la funzione IDENTITY. Vedere "Creazione di una colonna Identity tramite la funzione IDENTITY" nella sezione Esempi più avanti.  

## <a name="remarks"></a>Osservazioni  
Il funzionamento dell'istruzione `SELECT...INTO` è costituito da due parti: viene creata la nuova tabella e poi vengono inserite le righe.  Ciò significa che verrà eseguito il rollback degli inserimenti non riusciti, ma la nuova tabella (vuota) rimarrà.  Se è necessario che l'intera operazione abbia o esito positivo o esito negativo, usare una [transazione esplicita](../language-elements/begin-transaction-transact-sql.md).
  
## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni  
 Non è possibile specificare una variabile di tabella o un parametro con valori di tabella come nuova tabella.  
  
 Non è possibile usare `SELECT...INTO` per creare una tabella partizionata, anche quando la tabella di origine è partizionata. Lo schema di partizione della tabella di origine non viene usato in `SELECT...INTO`. La nuova tabella viene invece creata nel filegroup predefinito. Per inserire righe in una tabella partizionata, per prima cosa è necessario creare la tabella partizionata e quindi usare l'istruzione `INSERT INTO...SELECT...FROM`.  
  
 Indici, vincoli e trigger definiti nella tabella di origine non vengono trasferiti alla nuova tabella e non possono essere specificati nell'istruzione `SELECT...INTO`. Se questi oggetti sono richiesti, è possibile crearli dopo avere eseguito l'istruzione `SELECT...INTO`.  
  
 La specifica della clausola `ORDER BY` non garantisce che le righe vengano inserite nell'ordine specificato.  
  
 Quando nell'elenco di selezione è presente una colonna di tipo sparse, la relativa proprietà non viene trasferita nella colonna della nuova tabella. Se questa proprietà è richiesta nella nuova tabella, modificare la definizione di colonna dopo l'esecuzione dell'istruzione SELECT...INTO per includere la proprietà.  
  
 Quando nell'elenco di selezione è presente una colonna calcolata, la colonna corrispondente della nuova tabella non è di tipo calcolato. I valori della nuova colonna corrispondono ai valori calcolati quando è stata eseguita l'istruzione `SELECT...INTO`.  
  
## <a name="logging-behavior"></a>Comportamento di registrazione  
 La quantità di registrazioni per `SELECT...INTO` dipende dal modello di recupero attivato per il database. Nel modello di recupero con registrazione minima o in quello con registrazione minima delle operazioni bulk, per tali operazioni la registrazione prevista è quella minima. Con la registrazione minima, l'uso dell'istruzione `SELECT...INTO` può essere più efficiente della creazione di una tabella e del popolamento della stessa con un'istruzione INSERT. Per altre informazioni, vedere [Log delle transazioni &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md).
 
Le istruzioni `SELECT...INTO` che contengono funzioni definite dall'utente sono operazioni con registrazione completa. Se le funzioni definite dall'utente usate nell'istruzione `SELECT...INTO` non eseguono alcuna operazione di accesso ai dati, per tali funzioni è possibile specificare la clausola SCHEMABINDING, che ne imposta la proprietà UserDataAccess derivata su 0. Dopo questa modifica, per le istruzioni `SELECT...INTO` viene eseguita la registrazione minima. Se l'istruzione `SELECT...INTO` fa ancora riferimento ad almeno una funzione definita dall'utente la cui proprietà è impostata su 1, l'operazione viene registrata completamente.
  
## <a name="permissions"></a>Autorizzazioni  
 È necessaria l'autorizzazione CREATE TABLE nel database di destinazione.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-creating-a-table-by-specifying-columns-from-multiple-sources"></a>R. Creazione di una tabella specificando colonne provenienti da più origini  
 Nell'esempio seguente viene creata la tabella `dbo.EmployeeAddresses` nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] selezionando sette colonne da diverse tabelle relative a dipendenti e indirizzi.  
  
```sql  
SELECT c.FirstName, c.LastName, e.JobTitle, a.AddressLine1, a.City,   
    sp.Name AS [State/Province], a.PostalCode  
INTO dbo.EmployeeAddresses  
FROM Person.Person AS c  
    JOIN HumanResources.Employee AS e   
    ON e.BusinessEntityID = c.BusinessEntityID  
    JOIN Person.BusinessEntityAddress AS bea  
    ON e.BusinessEntityID = bea.BusinessEntityID  
    JOIN Person.Address AS a  
    ON bea.AddressID = a.AddressID  
    JOIN Person.StateProvince as sp   
    ON sp.StateProvinceID = a.StateProvinceID;  
GO  
```  
  
### <a name="b-inserting-rows-using-minimal-logging"></a>B. Inserimento di righe utilizzando la registrazione minima  
 Nell'esempio seguente viene creata la tabella `dbo.NewProducts`, in cui vengono inserite righe della tabella `Production.Product`. L'esempio presuppone che il modello di recupero del database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] sia impostato su FULL. Per assicurare l'utilizzo della registrazione minima, il modello di recupero del database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] viene impostato su BULK_LOGGED prima che le righe vengano inserite e reimpostato su FULL dopo l'istruzione SELECT...INTO. In tal modo, si assicura l'utilizzo da parte dell'istruzione SELECT...INTO di uno spazio minimo nel log delle transazioni con risultati efficienti.  
  
```sql  
ALTER DATABASE AdventureWorks2012 SET RECOVERY BULK_LOGGED;  
GO  
  
SELECT * INTO dbo.NewProducts  
FROM Production.Product  
WHERE ListPrice > $25   
AND ListPrice < $100;  
GO  
ALTER DATABASE AdventureWorks2012 SET RECOVERY FULL;  
GO  
```  
  
### <a name="c-creating-an-identity-column-using-the-identity-function"></a>C. Creazione di una colonna Identity tramite la funzione IDENTITY  
 Nell'esempio seguente viene utilizzata la funzione IDENTITY per creare una colonna Identity nella nuova tabella `Person.USAddress` del database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Questa operazione è necessaria perché l'istruzione SELECT che definisce la tabella contiene un join che fa in modo che la proprietà IDENTITY non venga trasferita nella nuova tabella. Si noti che il valore di inizializzazione e il valore di incremento specificati nella funzione IDENTITY sono diversi da quelli della colonna `AddressID` nella tabella di origine `Person.Address`.  
  
```sql  
-- Determine the IDENTITY status of the source column AddressID.  
SELECT OBJECT_NAME(object_id) AS TableName, name AS column_name, 
  is_identity, seed_value, increment_value  
FROM sys.identity_columns  
WHERE name = 'AddressID';  
  
-- Create a new table with columns from the existing table Person.Address. 
-- A new IDENTITY column is created by using the IDENTITY function.  
SELECT IDENTITY (int, 100, 5) AS AddressID,   
       a.AddressLine1, a.City, b.Name AS State, a.PostalCode  
INTO Person.USAddress   
FROM Person.Address AS a  
INNER JOIN Person.StateProvince AS b 
  ON a.StateProvinceID = b.StateProvinceID  
WHERE b.CountryRegionCode = N'US';   
  
-- Verify the IDENTITY status of the AddressID columns in both tables.  
SELECT OBJECT_NAME(object_id) AS TableName, name AS column_name, 
  is_identity, seed_value, increment_value  
FROM sys.identity_columns  
WHERE name = 'AddressID';  
```  
  
### <a name="d-creating-a-table-by-specifying-columns-from-a-remote-data-source"></a>D. Creazione di una tabella specificando colonne provenienti da un'origine dei dati remota  
 Nell'esempio seguente vengono illustrati tre metodi per creare una nuova tabella nel server locale da un'origine dati remota. L'esempio inizia con la creazione di un collegamento all'origine dati remota. Il nome del server collegato, `MyLinkServer,` viene specificato nella clausola FROM della prima istruzione SELECT...INTO e nella funzione OPENQUERY della seconda istruzione SELECT...INTO. La terza istruzione SELECT...INTO utilizza la funzione OPENDATASOURCE che specifica direttamente l'origine dei dati remota anziché utilizzare il nome del server collegato.  
  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
```sql
USE master;  
GO  
-- Create a link to the remote data source.   
-- Specify a valid server name for @datasrc as 'server_name' 
-- or 'server_name\instance_name'.  
EXEC sp_addlinkedserver @server = N'MyLinkServer',  
    @srvproduct = N' ',  
    @provider = N'SQLNCLI',   
    @datasrc = N'server_name',  
    @catalog = N'AdventureWorks2012';  
GO  

USE AdventureWorks2012;  
GO  
-- Specify the remote data source in the FROM clause using a four-part name   
-- in the form linked_server.catalog.schema.object.  
SELECT DepartmentID, Name, GroupName, ModifiedDate  
INTO dbo.Departments  
FROM MyLinkServer.AdventureWorks2012.HumanResources.Department  
GO  
-- Use the OPENQUERY function to access the remote data source.  
SELECT DepartmentID, Name, GroupName, ModifiedDate  
INTO dbo.DepartmentsUsingOpenQuery  
FROM OPENQUERY(MyLinkServer, 'SELECT *  
               FROM AdventureWorks2012.HumanResources.Department');   
GO  
-- Use the OPENDATASOURCE function to specify the remote data source.  
-- Specify a valid server name for Data Source using the format 
-- server_name or server_name\instance_name.  
SELECT DepartmentID, Name, GroupName, ModifiedDate  
INTO dbo.DepartmentsUsingOpenDataSource  
FROM OPENDATASOURCE('SQLNCLI',  
    'Data Source=server_name;Integrated Security=SSPI')  
    .AdventureWorks2012.HumanResources.Department;  
GO  
```  
  
### <a name="e-import-from-an-external-table-created-with-polybase"></a>E. Importare da una tabella esterna creata con PolyBase  
 Importare dati da Hadoop o dall'archiviazione di Azure in SQL Server per l'archivio permanente. Usare `SELECT INTO` per importare i dati a cui fa riferimento una tabella esterna per l'archiviazione permanente in SQL Server. Creare un tabella relazionale e quindi creare un indice di archivio colonne nella parte superiore della tabella in un secondo passaggio.  
  
 **Si applica a:** [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
```sql
-- Import data for car drivers into SQL Server to do more in-depth analysis.  
SELECT DISTINCT   
        Insured_Customers.FirstName, Insured_Customers.LastName,   
        Insured_Customers.YearlyIncome, Insured_Customers.MaritalStatus  
INTO Fast_Customers from Insured_Customers INNER JOIN   
(  
        SELECT * FROM CarSensor_Data where Speed > 35   
) AS SensorD  
ON Insured_Customers.CustomerKey = SensorD.CustomerKey  
ORDER BY YearlyIncome;  
```  
### <a name="f-copying-the-data-from-one-table-to-another-and-create-the-new-table-on-a-specified-filegroup"></a>F. Copia dei dati da una tabella a un'altra e creazione della nuova tabella in un filegroup specificato
Nell'esempio seguente viene illustrato come creare una nuova tabella come copia di un'altra tabella e come caricarla in un filegroup specificato diverso dal filegroup predefinito dell'utente.

 **Si applica a:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 e versioni successive.

```sql
ALTER DATABASE [AdventureWorksDW2016] ADD FILEGROUP FG2;
ALTER DATABASE [AdventureWorksDW2016]
ADD FILE
(
NAME='FG2_Data',
FILENAME = '/var/opt/mssql/data/AdventureWorksDW2016_Data1.mdf'
)
TO FILEGROUP FG2;
GO
SELECT * INTO [dbo].[FactResellerSalesXL] ON FG2 FROM [dbo].[FactResellerSales];
```
  
## <a name="see-also"></a>Vedere anche  
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [Esempi di istruzioni SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-examples-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [Funzione IDENTITY &#40;Transact-SQL&#41;](../../t-sql/functions/identity-function-transact-sql.md)  
  
  
