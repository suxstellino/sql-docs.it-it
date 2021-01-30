---
description: DELETE (Transact-SQL)
title: DELETE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/19/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DELETE
- DELETE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- row removal [SQL Server]
- DELETE statement [SQL Server], about DELETE statement
- views [SQL Server], deleting rows
- removing rows
- tables [SQL Server], deleting rows
- DELETE statement [SQL Server]
- deleting rows
- row removal [SQL Server], DELETE statement
- deleting data
ms.assetid: ed6b2105-0f35-408f-ba51-e36ade7ad5b2
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 47dcac82e79c2b7d839a7072c995b252c0b45e8a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177869"
---
# <a name="delete-transact-sql"></a>DELETE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Rimuove una o più righe da una tabella o vista in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
[ WITH <common_table_expression> [ ,...n ] ]  
DELETE   
    [ TOP ( expression ) [ PERCENT ] ]   
    [ FROM ]   
    { { table_alias  
      | <object>   
      | rowset_function_limited   
      [ WITH ( table_hint_limited [ ...n ] ) ] }   
      | @table_variable  
    }  
    [ <OUTPUT Clause> ]  
    [ FROM table_source [ ,...n ] ]   
    [ WHERE { <search_condition>   
            | { [ CURRENT OF   
                   { { [ GLOBAL ] cursor_name }   
                       | cursor_variable_name   
                   }   
                ]  
              }  
            }   
    ]   
    [ OPTION ( <Query Hint> [ ,...n ] ) ]   
[; ]  
  
<object> ::=  
{   
    [ server_name.database_name.schema_name.   
      | database_name. [ schema_name ] .   
      | schema_name.  
    ]  
    table_or_view_name   
}  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics

[ WITH <common_table_expression> [ ,...n ] ] 
DELETE [database_name . [ schema ] . | schema. ] table_name  
FROM [database_name . [ schema ] . | schema. ] table_name 
JOIN {<join_table_source>}[ ,...n ]  
ON <join_condition>
[ WHERE <search_condition> ]   
[ OPTION ( <query_options> [ ,...n ]  ) ]  
[; ]  

<join_table_source> ::=   
{  
    [ database_name . [ schema_name ] . | schema_name . ] table_or_view_name [ AS ] table_or_view_alias 
    [ <tablesample_clause>]  
    | derived_table [ AS ] table_alias [ ( column_alias [ ,...n ] ) ]  
}  
```

```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
DELETE 
    [ FROM [database_name . [ schema ] . | schema. ] table_name ]   
    [ WHERE <search_condition> ]   
    [ OPTION ( <query_options> [ ,...n ]  ) ]  
[; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 WITH \<common_table_expression>  
 Specifica il set di risultati denominato temporaneo, anche noto come espressione di tabella comune, definito nell'ambito dell'istruzione DELETE. Il set di risultati deriva da un'istruzione SELECT.  
  
 Le espressioni di tabella comuni possono inoltre essere utilizzate con istruzioni SELECT, INSERT, UPDATE e CREATE VIEW. Per altre informazioni, vedere [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md).  
  
 TOP **(** _expression_ **)** [ PERCENT ]  
 Viene specificato il numero o la percentuale di righe casuali che verranno eliminate. Il valore di *expression* può essere specificato come numero o come percentuale di righe. Le righe a cui viene fatto riferimento nell'espressione TOP utilizzata con INSERT, UPDATE o DELETE non sono disposte in alcun ordine. Per altre informazioni, vedere [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md).  
  
 FROM  
 Parola chiave facoltativa che è possibile specificare tra la parola chiave DELETE e l'oggetto di destinazione *table_or_view_name* o *rowset_function_limited*.  
  
 *table_alias*  
 Alias specificato nella clausola FROM *table_source* che rappresenta la tabella o la vista da cui devono essere eliminate le righe.  
  
 *server_name*  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 Nome del server, che usa come nome un nome di server collegato o la funzione [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md), in cui è contenuta la tabella o la vista. Se *server_name* è specificato, è obbligatorio specificare *database_name* e *schema_name*.  
  
 *database_name*  
 Nome del database.  
  
 *schema_name*  
 Nome dello schema a cui appartiene la tabella o la vista.  
  
 *table_or_view_name*  
 Nome della tabella o della vista da cui si desidera rimuovere le righe.  
  
 È inoltre possibile utilizzare una variabile di tabella, nel relativo ambito, come origine della tabella in un'istruzione DELETE.  
  
 È necessario che la vista a cui viene fatto riferimento in *table_or_view_name* sia aggiornabile e includa un riferimento esatto a una tabella di base nella clausola FROM della definizione della vista. Per altre informazioni sulle viste aggiornabili, vedere [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md).  
  
 *rowset_function_limited*  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 Funzione [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md) o [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md), in base alle funzionalità del provider.  
  
 WITH **(** \<table_hint_limited> [... *n*] **)**  
 Specifica uno o più hint di tabella consentiti per una tabella di destinazione. La parola chiave WITH e le parentesi sono obbligatorie. Le opzioni NOLOCK e READUNCOMMITTED non sono consentite. Per altre informazioni sugli hint di tabella, vedere [Hint di tabella &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
 \<OUTPUT_Clause>  
 Restituisce le righe eliminate o le espressioni basate su tali righe nell'ambito di un'operazione DELETE. La clausola OUTPUT non è supportata nelle istruzioni DML eseguite su viste o tabelle remote. Per altre informazioni sugli argomenti e il comportamento di questa clausola, vedere [Clausola OUTPUT &#40;Transact-SQL&#41;](../../t-sql/queries/output-clause-transact-sql.md).  
  
 FROM *table_source*  
 Specifica una clausola FROM aggiuntiva. Questa estensione di [!INCLUDE[tsql](../../includes/tsql-md.md)] dell'istruzione DELETE consente di specificare dati di \<table_source> e di eliminare le righe corrispondenti dalla tabella specificata nella prima clausola FROM.  
  
 È possibile utilizzare questa estensione, specificando un join, al posto di una sottoquery nella clausola WHERE per identificare le righe che si desidera rimuovere.  
  
 Per altre informazioni, vedere [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md).  
  
 WHERE  
 Specifica le condizioni utilizzate per limitare il numero di righe da eliminare. Se la clausola WHERE viene omessa, l'istruzione DELETE elimina tutte le righe della tabella.  
  
 Le operazioni di eliminazione possono essere di due diversi tipi in base al contenuto della clausola WHERE:  
  
-   Le eliminazioni con ricerca specificano una condizione di ricerca che qualifica le righe da eliminare. Ad esempio, WHERE *column_name* = *value*.  
  
-   Le eliminazioni posizionate utilizzano la clausola CURRENT OF per specificare un cursore. L'operazione di eliminazione viene eseguita nella posizione corrente del cursore. Questo tipo di eliminazione risulta più accurato rispetto a un'istruzione DELETE con ricerca che usa una clausola WHERE *search_condition* per qualificare le righe da eliminare. Un'istruzione DELETE con ricerca elimina più righe se la condizione di ricerca non identifica una singola riga in modo univoco.  
  
\<search_condition>  
 Specifica le condizioni di restrizione per le righe da eliminare. Non sono previsti limiti per il numero di predicati che è possibile includere in una condizione di ricerca. Per altre informazioni, vedere [Condizione di ricerca&#40;Transact-SQL&#41;](../../t-sql/queries/search-condition-transact-sql.md).  
  
 CURRENT OF  
 Specifica che l'istruzione DELETE viene eseguita nella posizione corrente del cursore specificato.  
  
 GLOBAL  
 Specifica che *cursor_name* fa riferimento a un cursore globale.  
  
 *cursor_name*  
 Nome del cursore aperto da cui viene eseguita l'operazione di recupero. Se sono presenti un cursore globale e un cursore locale denominati *cursor_name* ed è stato specificato l'argomento GLOBAL, l'argomento fa riferimento al cursore globale. Se non è stato specificato l'argomento GLOBAL, fa riferimento al cursore locale. Il cursore deve consentire operazioni di aggiornamento.  
  
 *cursor_variable_name*  
 Nome di una variabile di cursore. La variabile di cursore deve fare riferimento a un cursore che consente operazioni di aggiornamento.  
  
 OPTION **(** \<query_hint> [ **,** ... *n*] **)**  
 Parole chiave che indicano quali hint di ottimizzazione vengono utilizzati per personalizzare la modalità di elaborazione dell'istruzione nel [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Per altre informazioni, vedere [Hint per la query &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
  
## <a name="best-practices"></a>Procedure consigliate  
 Per eliminare tutte le righe in una tabella, utilizzare TRUNCATE TABLE. L'esecuzione di TRUNCATE TABLE è più rapida rispetto a quella di DELETE e comporta un minor utilizzo di risorse del log delle transazioni e di sistema. TRUNCATE TABLE presenta alcune restrizioni, ad esempio, la tabella non può partecipare alla replica. Per altre informazioni, vedere [TRUNCATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/truncate-table-transact-sql.md)  
  
 Usare la funzione @@ROWCOUNT per restituire il numero di righe eliminate nell'applicazione client. Per altre informazioni, vedere [@@ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/rowcount-transact-sql.md).  
  
## <a name="error-handling"></a>Gestione degli errori  
 È possibile implementare la gestione degli errori per l'istruzione DELETE specificando l'istruzione in un costrutto TRY...CATCH.  
  
 L'istruzione DELETE può avere esito negativo se viola un trigger o tenta di rimuovere una riga a cui fanno riferimento i dati di un'altra tabella contenente un vincolo FOREIGN KEY. Se l'istruzione DELETE tenta di rimuovere più righe e l'eliminazione di una qualsiasi di queste righe viola un trigger o un vincolo, l'istruzione viene annullata, viene restituito un errore e non viene rimossa alcuna riga.  
  
 Quando un'istruzione DELETE rileva un errore aritmetico, ovvero un errore di overflow, una divisione per zero o un errore di dominio, durante la valutazione di un'espressione, [!INCLUDE[ssDE](../../includes/ssde-md.md)] gestisce l'errore come se l'opzione SET ARITHABORT fosse impostata su ON. La parte rimanente del batch viene annullata e viene restituito un messaggio di errore.  
  
## <a name="interoperability"></a>Interoperabilità  
 È possibile utilizzare l'istruzione DELETE nel corpo di una funzione definita dall'utente se l'oggetto modificato è una variabile di tabella.  
  
 Quando si elimina una riga che contiene una colonna FILESTREAM, vengono eliminati anche i file del file system sottostanti. I file sottostanti vengono rimossi dal Garbage Collector di FILESTREAM. Per altre informazioni, vedere [Accedere a dati FILESTREAM con Transact-SQL](../../relational-databases/blob/access-filestream-data-with-transact-sql.md).  
  
 Non è possibile specificare la clausola FROM in un'istruzione DELETE contenente un riferimento diretto o indiretto a una vista per cui è stato definito un trigger INSTEAD OF. Per altre informazioni sui trigger INSTEAD OF, vedere [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md).  
  
## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni  
 Se TOP viene utilizzato con DELETE, le righe a cui viene fatto riferimento non vengono disposte in alcun ordine e la clausola ORDER BY non può essere specificata in modo diretto in questa istruzione. Se si desidera utilizzare TOP per eliminare le righe in un ordine cronologico significativo, è necessario utilizzare TOP insieme a una clausola ORDER BY in un'istruzione sub-SELECT. Vedere la sezione Esempi più avanti in questo argomento.  
  
 Non è possibile utilizzare TOP in un'istruzione DELETE in viste partizionate.  
  
## <a name="locking-behavior"></a>Comportamento di blocco  
 Per impostazione predefinita, un'istruzione DELETE acquisisce sempre un blocco preventivo esclusivo (IX) sull'oggetto tabella che modifica e mantiene tale blocco fino al completamento della transazione. Con un blocco preventivo esclusivo (IX) nessun'altra transazione può modificare dati. Le operazioni di lettura possono essere eseguite solo usando l'hint NOLOCK o il livello di isolamento Read Uncommitted. È possibile specificare hint di tabella per eseguire l'override di questo comportamento predefinito per la durata dell'istruzione DELETE specificando un altro metodo di blocco. Gli hint dovrebbero comunque essere utilizzati solo se strettamente necessario ed esclusivamente da sviluppatori e amministratori di database esperti. Per altre informazioni, vedere [Hint di tabella &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
 Quando si eliminano righe da un heap, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] può utilizzare il blocco di riga o di pagina per l'operazione. Le pagine svuotate dall'operazione di eliminazione rimangono pertanto allocate all'heap. Se le pagine vuote non vengono deallocate, non è possibile riutilizzare lo spazio associato per altri oggetti nel database.  
  
 Per eliminare le righe di un heap e deallocare le pagine, utilizzare uno dei metodi seguenti.  
  
-   Specificare l'hint TABLOCK nell'istruzione DELETE. Se si utilizza l'hint TABLOCK, nell'operazione di eliminazione viene accettato un blocco esclusivo nella tabella anziché un blocco di riga o di pagina. In questo modo sarà possibile deallocare le pagine. Per altre informazioni sugli hint TABLOCK, vedere [Hint di tabella &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
-   Usare `TRUNCATE TABLE` se è necessario eliminare tutte le righe della tabella.  
  
-   Creare un indice cluster sull'heap prima di eliminare le righe. È possibile eliminare l'indice cluster dopo l'eliminazione delle righe. Questo metodo richiede più tempo rispetto ai precedenti e utilizza una maggiore quantità di risorse temporanee.  
  
> [!NOTE]  
>  Le pagine vuote possono essere rimosse da un heap in qualsiasi momento con l'istruzione `ALTER TABLE <table_name> REBUILD`.  
  
## <a name="logging-behavior"></a>Comportamento di registrazione  
L'istruzione DELETE viene sempre registrata completamente.  
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni `DELETE` sono necessarie nella tabella di destinazione. Se l'istruzione contiene una clausola WHERE, sono inoltre richieste le autorizzazioni `SELECT`.  
  
 Le autorizzazioni DELETE vengono concesse per impostazione predefinita ai membri del ruolo predefinito del server `sysadmin`, ai membri dei ruoli predefiniti del database `db_owner` e `db_datawriter` e al proprietario della tabella. I membri dei ruoli `sysadmin`, `db_owner` e `db_securityadmin` e il proprietario della tabella possono trasferire le autorizzazioni ad altri utenti.  
  
## <a name="examples"></a>Esempi  
  
|Category|Elementi di sintassi inclusi|  
|--------------|------------------------------|  
|[Sintassi di base](#BasicSyntax)|DELETE|  
|[Limitazione delle righe eliminate](#LimitRows)|WHERE • FROM • cursore •|  
|[Eliminazione di righe da una tabella remota](#RemoteTables)|Server collegato • funzione per set di righe OPENQUERY • funzione per set di righe OPENDATASOURCE|  
|[Acquisizione dei risultati dell'istruzione DELETE](#CaptureResults)|Clausola OUTPUT|  
  
###  <a name="basic-syntax"></a><a name="BasicSyntax"></a> Sintassi di base  
 Negli esempi contenuti in questa sezione vengono illustrate le funzionalità di base dell'istruzione DELETE tramite la sintassi minima richiesta.  
  
#### <a name="a-using-delete-with-no-where-clause"></a>R. Utilizzo di DELETE senza una clausola WHERE  
 Nell'esempio seguente vengono eliminate tutte le righe dalla tabella `SalesPersonQuotaHistory` nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] perché una clausola WHERE non è utilizzata per limitare il numero di righe eliminate.  
  
```sql
DELETE FROM Sales.SalesPersonQuotaHistory;  
GO  
```  
  
###  <a name="limiting-the-rows-deleted"></a><a name="LimitRows"></a>Limitazione delle righe eliminate  
 Negli esempi riportati in questa sezione viene illustrato come limitare il numero di righe che verranno eliminate.  
  
#### <a name="b-using-the-where-clause-to-delete-a-set-of-rows"></a>B. Utilizzo della clausola WHERE per eliminare un set di righe  
 Nell'esempio seguente vengono eliminate tutte le righe della tabella `ProductCostHistory` del database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] in cui il valore della colonna `StandardCost` è maggiore di `1000.00`.  
  
```sql
DELETE FROM Production.ProductCostHistory  
WHERE StandardCost > 1000.00;  
GO  
```  
  
 Nell'esempio seguente viene illustrata una clausola WHERE più complessa. La clausola WHERE definisce due condizioni che devono essere soddisfatte per determinare le righe da eliminare. Il valore nella colonna `StandardCost` deve essere compreso tra `12.00` e `14.00` , mentre quello nella colonna `SellEndDate` deve essere Null. Nell'esempio viene inoltre stampato il valore dalla funzione **\@\@ROWCOUNT** per restituire il numero di righe eliminate.  
  
```sql
DELETE Production.ProductCostHistory  
WHERE StandardCost BETWEEN 12.00 AND 14.00  
      AND EndDate IS NULL;  
PRINT 'Number of rows deleted is ' + CAST(@@ROWCOUNT as char(3));  
```  
  
#### <a name="c-using-a-cursor-to-determine-the-row-to-delete"></a>C. Utilizzo di un cursore per determinare la riga da eliminare  
 Nell'esempio seguente viene eliminata una riga dalla tabella `EmployeePayHistory` nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] usando un cursore denominato `complex_cursor`. L'operazione di eliminazione interessa unicamente la riga attualmente recuperata dal cursore.  
  
```sql
DECLARE complex_cursor CURSOR FOR  
    SELECT a.BusinessEntityID  
    FROM HumanResources.EmployeePayHistory AS a  
    WHERE RateChangeDate <>   
         (SELECT MAX(RateChangeDate)  
          FROM HumanResources.EmployeePayHistory AS b  
          WHERE a.BusinessEntityID = b.BusinessEntityID) ;  
OPEN complex_cursor;  
FETCH FROM complex_cursor;  
DELETE FROM HumanResources.EmployeePayHistory  
WHERE CURRENT OF complex_cursor;  
CLOSE complex_cursor;  
DEALLOCATE complex_cursor;  
GO  
```  
  
#### <a name="d-using-joins-and-subqueries-to-data-in-one-table-to-delete-rows-in-another-table"></a>D. Utilizzo di join e sottoquery per i dati di una tabella per eliminare righe di un'altra tabella  
 Negli esempi seguenti vengono illustrati due modi per eliminare righe di una tabella in base ai dati di un'altra tabella. In entrambi gli esempi le righe della tabella `SalesPersonQuotaHistory` del database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] vengono eliminate in base alle vendite da inizio anno archiviate nella tabella `SalesPerson`. La prima istruzione `DELETE` illustra la soluzione di sottoquery compatibile con ISO, mentre la seconda istruzione `DELETE` illustra l'estensione FROM [!INCLUDE[tsql](../../includes/tsql-md.md)] per creare un join tra le due tabelle.  
  
```sql
-- SQL-2003 Standard subquery  
  
DELETE FROM Sales.SalesPersonQuotaHistory   
WHERE BusinessEntityID IN   
    (SELECT BusinessEntityID   
     FROM Sales.SalesPerson   
     WHERE SalesYTD > 2500000.00);  
GO  
```  
  
```sql
-- Transact-SQL extension  
  
DELETE FROM Sales.SalesPersonQuotaHistory   
FROM Sales.SalesPersonQuotaHistory AS spqh  
INNER JOIN Sales.SalesPerson AS sp  
ON spqh.BusinessEntityID = sp.BusinessEntityID  
WHERE sp.SalesYTD > 2500000.00;  
GO  
```  
  
```sql
-- No need to mention target table more than once.  
  
DELETE spqh  
  FROM  
        Sales.SalesPersonQuotaHistory AS spqh  
    INNER JOIN Sales.SalesPerson AS sp  
        ON spqh.BusinessEntityID = sp.BusinessEntityID  
  WHERE  sp.SalesYTD > 2500000.00;  
```  
  
#### <a name="e-using-top-to-limit-the-number-of-rows-deleted"></a>E. Utilizzo di TOP per limitare il numero di righe eliminate  
 Quando si usa una clausola TOP (*n*) con l'istruzione DELETE, l'operazione di eliminazione viene eseguita su una selezione casuale di un numero di righe *n*. Nell'esempio seguente vengono eliminate `20` righe casuali della tabella `PurchaseOrderDetail` del database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] che contengono date di scadenza precedenti alla data 1 luglio 2006.  
  
```sql
DELETE TOP (20)   
FROM Purchasing.PurchaseOrderDetail  
WHERE DueDate < '20020701';  
GO  
```  
  
 Se è necessario utilizzare TOP per eliminare le righe in un ordine cronologico significativo, è necessario utilizzare TOP insieme a ORDER BY in un'istruzione subselect. Tramite la query seguente vengono eliminate le 10 righe della tabella `PurchaseOrderDetail` contenenti le date di scadenza più imminenti. Per assicurarsi che vengano eliminate solo 10 righe, la colonna specificata nell'istruzione di selezione secondaria (`PurchaseOrderID`) è la chiave primaria della tabella. L'utilizzo di una colonna non chiave nell'istruzione sub-SELECT può avere come conseguenza l'eliminazione di più di 10 righe se la colonna specificata contiene valori duplicati.  
  
```sql
DELETE FROM Purchasing.PurchaseOrderDetail  
WHERE PurchaseOrderDetailID IN  
   (SELECT TOP 10 PurchaseOrderDetailID   
    FROM Purchasing.PurchaseOrderDetail   
    ORDER BY DueDate ASC);  
GO  
```  
  
###  <a name="deleting-rows-from-a-remote-table"></a><a name="RemoteTables"></a> Eliminazione di righe da una tabella remota  
 Negli esempi riportati in questa sezione viene illustrato come eliminare righe da una tabella remota tramite un [server collegato](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) o una [funzione per i set di righe](../functions/opendatasource-transact-sql.md) per fare riferimento alla tabella remota. Esiste una tabella remota in un server diverso o un'istanza di SQL Server.  
  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
#### <a name="f-deleting-data-from-a-remote-table-by-using-a-linked-server"></a>F. Eliminazione di dati da una tabella remota tramite un server collegato  
 Nell'esempio seguente vengono eliminate righe da una tabella remota. L'esempio inizia con la creazione di un collegamento all'origine dati remota tramite [sp_addlinkedserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md). Il nome del server collegato, `MyLinkServer`, viene specificato come parte del nome di oggetto in quattro parti nel formato *server.catalogo.schema.oggetto*.  
  
```sql
USE master;  
GO  
-- Create a link to the remote data source.   
-- Specify a valid server name for @datasrc as 'server_name' or 'server_name\instance_name'.  
  
EXEC sp_addlinkedserver @server = N'MyLinkServer',  
    @srvproduct = N' ',  
    @provider = N'SQLNCLI',   
    @datasrc = N'server_name',  
    @catalog = N'AdventureWorks2012';  
GO  
```  
  
```sql
-- Specify the remote data source using a four-part name   
-- in the form linked_server.catalog.schema.object.  
  
DELETE MyLinkServer.AdventureWorks2012.HumanResources.Department 
WHERE DepartmentID > 16;  
GO  
```  
  
#### <a name="g-deleting-data-from-a-remote-table-by-using-the-openquery-function"></a>G. Eliminazione di dati da una tabella remota tramite una funzione OPENQUERY  
 Nell'esempio seguente vengono eliminate righe da una tabella remota specificando la funzione per i set di righe [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md). Viene utilizzato il nome del server collegato creato nell'esempio precedente.  
  
```sql
DELETE OPENQUERY (MyLinkServer, 'SELECT Name, GroupName 
FROM AdventureWorks2012.HumanResources.Department  
WHERE DepartmentID = 18');  
GO  
```  
  
#### <a name="h-deleting-data-from-a-remote-table-by-using-the-opendatasource-function"></a>H. Eliminazione di dati da una tabella remota tramite una funzione OPENDATASOURCE  
 Nell'esempio seguente vengono eliminate righe da una tabella remota specificando la funzione per i set di righe [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md). Specificare un nome server valido per l'origine dati usando il formato *nome_server* oppure *nome_server\nome_istanza*.  
  
```sql
DELETE FROM OPENDATASOURCE('SQLNCLI',  
    'Data Source= <server_name>; Integrated Security=SSPI')  
    .AdventureWorks2012.HumanResources.Department   
WHERE DepartmentID = 17;
```  
  
###  <a name="capturing-the-results-of-the-delete-statement"></a><a name="CaptureResults"></a> Acquisizione dei risultati dell'istruzione DELETE  
  
#### <a name="i-using-delete-with-the-output-clause"></a>I. Utilizzo di DELETE con la clausola OUTPUT  
 Nell'esempio seguente viene illustrato come salvare i risultati di un'istruzione `DELETE` in una variabile di tabella nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql
DELETE Sales.ShoppingCartItem  
OUTPUT DELETED.*   
WHERE ShoppingCartID = 20621;  
  
--Verify the rows in the table matching the WHERE clause have been deleted.  
SELECT COUNT(*) AS [Rows in Table] 
FROM Sales.ShoppingCartItem 
WHERE ShoppingCartID = 20621;  
GO  
```  
  
#### <a name="j-using-output-with-from_table_name-in-a-delete-statement"></a>J. Uso di OUTPUT con <from_table_name> in un'istruzione DELETE  
 Nell'esempio seguente vengono eliminate alcune righe della tabella `ProductProductPhoto` del database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] in base ai criteri di ricerca definiti nella clausola `FROM` dell'istruzione `DELETE`. La clausola `OUTPUT` restituisce le colonne della tabella che si desidera eliminare, `DELETED.ProductID`, `DELETED.ProductPhotoID`e alcune colonne della tabella `Product` . Queste informazioni vengono utilizzate nella clausola `FROM` per specificare le righe da eliminare.  
  
```sql
DECLARE @MyTableVar table (  
    ProductID int NOT NULL,   
    ProductName nvarchar(50)NOT NULL,  
    ProductModelID int NOT NULL,   
    PhotoID int NOT NULL);  
  
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
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="k-delete-all-rows-from-a-table"></a>K. Eliminare tutte le righe di una tabella  
 Nell'esempio seguente vengono eliminate tutte le righe dalla tabella `Table1` perché non viene utilizzata una clausola WHERE per limitare il numero di righe eliminate.  
  
```sql
DELETE FROM Table1;  
```  
  
### <a name="l-delete-a-set-of-rows-from-a-table"></a>L. Eliminare un set di righe di una tabella

 Nell'esempio seguente vengono eliminate dalla tabella `Table1` tutte le righe in cui il valore della colonna `StandardCost` è maggiore di 1000,00.  
  
```sql
DELETE FROM Table1  
WHERE StandardCost > 1000.00;  
```  
  
### <a name="m-using-label-with-a-delete-statement"></a>M. Uso di LABEL con un'istruzione DELETE

 Nell'esempio seguente viene usata un'etichetta con l'istruzione DELETE.  
  
```sql
DELETE FROM Table1  
OPTION ( LABEL = N'label1' );  
  
```  
  
### <a name="n-using-a-label-and-a-query-hint-with-the-delete-statement"></a>N. Uso di un'etichetta e di un hint per la query con l'istruzione DELETE

 Questa query illustra la sintassi di base per l'uso di un hint di join per la query con l'istruzione DELETE. Per altre informazioni sugli hint di join e su come usare la clausola OPTION, vedere [Clausola OPTION (Transact-SQL)](../queries/option-clause-transact-sql.md).
  
```sql
-- Uses AdventureWorks  
  
DELETE FROM dbo.FactInternetSales  
WHERE ProductKey IN (   
    SELECT T1.ProductKey FROM dbo.DimProduct T1   
    JOIN dbo.DimProductSubcategory T2  
    ON T1.ProductSubcategoryKey = T2.ProductSubcategoryKey  
    WHERE T2.EnglishProductSubcategoryName = 'Road Bikes' )  
OPTION ( LABEL = N'CustomJoin', HASH JOIN ) ;  
```  

### <a name="o-delete-using-a-where-clause"></a>O. Eliminare usando una clausola WHERE

Questa query mostra come eliminare usando la clausola WHERE e non una clausola FROM.

```sql
DELETE tableA WHERE EXISTS (
SELECT TOP 1 1 FROM tableB tb WHERE tb.col1 = tableA.col1
)
```

### <a name="p-delete-based-on-the-result-of-joining-with-another-table"></a>P. Eliminare in base al risultato del join con un'altra tabella

Questo esempio illustra come eseguire un'eliminazione da una tabella in base al risultato del join con un'altra tabella.

```sql
CREATE TABLE dbo.Table1   
    (ColA int NOT NULL, ColB decimal(10,3) NOT NULL);  
GO  

CREATE TABLE dbo.Table2   
    (ColA int PRIMARY KEY NOT NULL, ColB decimal(10,3) NOT NULL);  
GO  
INSERT INTO dbo.Table1 VALUES(1, 10.0), (1, 20.0);  
INSERT INTO dbo.Table2 VALUES(1, 0.0);  
GO  

DELETE dbo.Table2   
FROM dbo.Table2   
    INNER JOIN dbo.Table1   
    ON (dbo.Table2.ColA = dbo.Table1.ColA)
    WHERE dboTable2.ColA = 1;  
```

## <a name="see-also"></a>Vedere anche

 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [TRUNCATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/truncate-table-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)   
 [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md)   
 [@@ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/rowcount-transact-sql.md)  
  
