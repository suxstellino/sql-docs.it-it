---
description: Usare una funzione di filtro per selezionare righe di cui eseguire la migrazione (Stretch Database)
title: Selezionare le righe di cui eseguire la migrazione tramite una funzione di filtro
ms.date: 06/27/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, predicates
- predicates for Stretch Database
- Stretch Database, inline table-valued functions
- inline table-valued functions for Stretch Database
ms.assetid: 090890ee-7620-4a08-8e15-d2fbc71dd12f
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 72162b4ed6339bcf7939e1903886170e3df36e63
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030898"
---
# <a name="select-rows-to-migrate-by-using-a-filter-function-stretch-database"></a>Usare una funzione di filtro per selezionare righe di cui eseguire la migrazione (Stretch Database)
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  Se si archiviano dati inattivi in una tabella separata, è possibile configurare Stretch Database per eseguire la migrazione dell'intera tabella. Se invece la tabella contiene dati usati più di frequente e dati usati meno di frequente, è possibile specificare un predicato del filtro per selezionare le righe di cui eseguire la migrazione. Il predicato del filtro è una funzione inline con valori di tabella. Questo articolo illustra come scrivere una funzione inline con valori di tabella per selezionare le righe di cui eseguire la migrazione.  
  
> [!IMPORTANT]
> Se si specifica una funzione di filtro dalle prestazioni scarse, anche la migrazione dei dati avrà prestazioni scarse. Stretch Database applica la funzione del filtro alla tabella tramite l'operatore CROSS APPLY.  
  
 Se non si specifica una funzione di filtro, viene eseguita la migrazione dell'intera tabella.  
  
 Quando si esegue l'Abilitazione guidata del database per l'estensione, è possibile eseguire la migrazione di un'intera tabella oppure specificare una funzione di filtro semplice nella procedura guidata. Se si vuole usare un tipo diverso di funzione di filtro per selezionare le righe di cui eseguire la migrazione, eseguire una di queste operazioni.  
  
-   Uscire dalla procedura guidata ed eseguire l'istruzione ALTER TABLE per abilitare l'estensione per la tabella e specificare una funzione di filtro.  
  
-   Eseguire l'istruzione ALTER TABLE per specificare una funzione di filtro dopo l'uscita dalla procedura guidata.  
  
 La sintassi di ALTER TABLE per l'aggiunta di una funzione è descritta più avanti in questo articolo.  
  
## <a name="basic-requirements-for-the-filter-function"></a>Requisiti di base per la funzione di filtro  
 La funzione con valori di tabella inline necessaria per un predicato del filtro di Stretch Database è simile all'esempio seguente.  
  
```sql  
CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datatype1, @column2 datatype2 [, ...n])  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
RETURN  SELECT 1 AS is_eligible  
        WHERE <predicate>  
```  
  
 I parametri della funzione devono essere identificatori per le colonne della tabella.  
  
 L'associazione allo schema è necessaria per evitare l'eliminazione o la modifica delle colonne usate dalla funzione di filtro.  
  
### <a name="return-value"></a>Valore restituito  
 Se la funzione restituisce un risultato non vuoto, la riga è idonea per la migrazione. In caso contrario, ovvero se la funzione non restituisce un risultato, la riga non è idonea per la migrazione.  
  
### <a name="conditions"></a>Condizioni  
 Il &lt;*predicato*&gt; può essere costituito da una singola condizione oppure da più condizioni unite tramite l'operatore logico AND.  
  
```  
<predicate> ::= <condition> [ AND <condition> ] [ ...n ]  
```  
  
 Ogni condizione può essere costituita a sua volta da una condizione primitiva o da più condizioni primitive unite con l'operatore logico OR.  
  
```  
<condition> ::= <primitive_condition> [ OR <primitive_condition> ] [ ...n ]  
```  
  
### <a name="primitive-conditions"></a>Condizioni primitive  
 Una condizione di primitiva può eseguire uno dei confronti seguenti.  
  
```  
<primitive_condition> ::=   
{  
<function_parameter> <comparison_operator> constant  
| <function_parameter> { IS NULL | IS NOT NULL }  
| <function_parameter> IN ( constant [ ,...n ] )  
}  
  
```  
  
-   Confrontare un parametro di funzione con un'espressione costante. Ad esempio: `@column1 < 1000`.  
  
     Ecco un esempio che controlla se il valore di una colonna *date* è &lt; 1/1/2016.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datetime)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)  
    GO  
  
    ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (  
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date),  
        MIGRATION_STATE = OUTBOUND  
    ) )  
  
    ```  
  
-   Applicare l'operatore IS NULL o IS NOT NULL a un parametro della funzione.  
  
-   Usare l'operatore IN per confrontare un parametro della funzione con un elenco di valori costanti.  
  
     Ecco un esempio che controlla se il valore di una colonna *shipment_status*  è `IN (N'Completed', N'Returned', N'Cancelled')`.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 nvarchar(15))  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 IN (N'Completed', N'Returned', N'Cancelled')  
    GO  
  
    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (  
        FILTER_PREDICATE = dbo.fn_stretchpredicate(shipment_status),  
        MIGRATION_STATE = OUTBOUND  
    ) )  
  
    ```  
  
### <a name="comparison-operators"></a>Operatori di confronto  
 Sono supportati gli operatori di confronto seguenti.  
  
 `<, <=, >, >=, =, <>, !=, !<, !>`  
  
```  
<comparison_operator> ::= { < | <= | > | >= | = | <> | != | !< | !> }  
```  
  
### <a name="constant-expressions"></a>Espressioni costanti  
 Le costanti usate in una funzione di filtro possono essere costituite da qualsiasi espressione deterministica che può essere valutata quando si definisce la funzione. Le espressioni costanti possono contenere gli elementi seguenti.  
  
-   Valori letterali. Ad esempio: `N'abc', 123`.  
  
-   Espressioni algebriche. Ad esempio: `123 + 456`.  
  
-   Funzioni deterministiche. Ad esempio: `SQRT(900)`.  
  
-   Conversioni deterministiche che usano CAST o CONVERT. Ad esempio: `CONVERT(datetime, '1/1/2016', 101)`.  
  
### <a name="other-expressions"></a>Altre espressioni  
 È possibile usare gli operatori BETWEEN e NOT BETWEEN se la funzione risultante è conforme alle regole descritte qui dopo la sostituzione degli operatori BETWEEN e NOT BETWEEN con le espressioni AND e OR equivalenti.  
  
 Non è possibile usare sottoquery o funzioni non deterministiche, ad esempio RAND() o GETDATE().  
  
## <a name="add-a-filter-function-to-a-table"></a>Aggiungere una funzione di filtro a una tabella  
 Per aggiungere una funzione di filtro a una tabella, eseguire l'istruzione **ALTER TABLE** e specificare una funzione inline con valori di tabella esistente come valore del parametro **FILTER_PREDICATE** . Ad esempio:  
  
```sql  
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (  
    FILTER_PREDICATE = dbo.fn_stretchpredicate(column1, column2),  
    MIGRATION_STATE = <desired_migration_state>  
) )  
  
```  
  
 Dopo aver associato la funzione alla tabella come predicato, si verifica quanto segue.  
  
-   Alla migrazione di dati successiva, viene eseguita la migrazione soltanto delle righe per le quali la funzione restituisce un valore non vuoto.  
  
-   Le colonne usate dalla funzione sono associate allo schema. Non è possibile modificare le colonne fino a quando una tabella usa la funzione come predicato del filtro.  
  
 Non è possibile eliminare la funzione inline con valori di tabella fino a quando una tabella usa la funzione come predicato del filtro. 

> [!TIP]
> Per migliorare le prestazioni della funzione di filtro, creare un indice per le colonne usate dalla funzione.

 ### <a name="passing-column-names-to-the-filter-function"></a>Passaggio di nomi di colonna alla funzione di filtro
 
 Quando si assegna una funzione di filtro a una tabella, specificare nomi in una parte per le colonne passate alla funzione di filtro. Se quando si passano i nomi di colonna si specificano nomi in tre parti, le query successive sulla tabella con estensione abilitata avranno esito negativo.

Se, come illustrato nell'esempio seguente, si specifica un nome di colonna in tre parti, l'istruzione verrà eseguita correttamente, ma le query successive sulla tabella avranno esito negativo.

```sql
ALTER TABLE SensorTelemetry 
  SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(dbo.SensorTelemetry.ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```

Specificare invece la funzione di filtro con un nome di colonna in una sola parte, come illustrato nell'esempio seguente.

```sql
ALTER TABLE SensorTelemetry 
  SET ( REMOTE_DATA_ARCHIVE = ON  (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```
  
## <a name="add-a-filter-function-after-running-the-wizard"></a><a name="addafterwiz"></a>Aggiungere una funzione di filtro dopo l'esecuzione della procedura guidata  
  
Se si vuole usare una funzione che non è possibile creare nell' **Abilitazione guidata del database per l'estensione** , è possibile eseguire l'istruzione **ALTER TABLE** per specificare una funzione, dopo l'uscita dalla procedura guidata. Prima di applicare una funzione, tuttavia, è necessario interrompere la migrazione dei dati in corso e ripristinare i dati migrati. Per altre informazioni sul perché è necessario, vedere [Sostituire una funzione di filtro esistente](#replacePredicate).
  
1. Invertire la direzione della migrazione e ripristinare i dati già migrati. Non è possibile annullare questa operazione dopo l'avvio. In Azure, poi, i trasferimenti di dati in uscita (traffico in uscita) sono soggetti ad addebito. Per altre informazioni, vedere [Dettagli prezzi dei trasferimenti di dati](https://azure.microsoft.com/pricing/details/data-transfers/).  
  
    ```sql  
    ALTER TABLE <table name>  
        SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;   
    ```  
  
2. Attendere il completamento della migrazione. È possibile controllare lo stato in **Stretch Database Monitor** da SQL Server Management Studio oppure eseguire una query della visualizzazione **sys.dm_db_rda_migration_status**. Per altre informazioni, vedere [Monitor and troubleshoot data migration](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md) (Monitoraggio e risoluzione dei problemi della migrazione dei dati) o [sys.dm_db_rda_migration_status](../../relational-databases/system-dynamic-management-views/stretch-database-sys-dm-db-rda-migration-status.md).  
  
3. Creare la funzione di filtro che si vuole applicare alla tabella.  
  
4. Aggiungere la funzione alla tabella e riavviare la migrazione dei dati in Azure.  
  
    ```sql  
    ALTER TABLE <table name>  
        SET ( REMOTE_DATA_ARCHIVE  
            (           
                FILTER_PREDICATE = <predicate>,  
                MIGRATION_STATE = OUTBOUND  
            )  
            );   
    ```  
  
## <a name="filter-rows-by-date"></a>Filtrare le righe in base alla data  
 L'esempio seguente esegue la migrazione delle righe in cui la colonna **date** contiene un valore precedente all'1 gennaio 2016.  
  
```sql  
-- Filter by date  
--  
CREATE FUNCTION dbo.fn_stretch_by_date(@date datetime2)  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
       RETURN SELECT 1 AS is_eligible WHERE @date < CONVERT(datetime2, '1/1/2016', 101)  
GO  
  
```  
  
## <a name="filter-rows-by-the-value-in-a-status-column"></a>Filtrare le righe in base al valore della colonna status  
 L'esempio seguente esegue la migrazione delle righe in cui la colonna **status** contiene uno dei valori specificati.  
  
```sql  
-- Filter by status column  
--  
CREATE FUNCTION dbo.fn_stretch_by_status(@status nvarchar(128))  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
       RETURN SELECT 1 AS is_eligible WHERE @status IN (N'Completed', N'Returned', N'Cancelled')  
GO  
  
```  
  
## <a name="filter-rows-by-using-a-sliding-window"></a>Filtrare le righe usando una finestra temporale scorrevole  
 Per filtrare le righe usando una finestra temporale scorrevole, tenere presente i requisiti seguenti per la funzione di filtro.  
  
-   La funzione deve essere deterministica. Di conseguenza, non è possibile creare una funzione che ricalcola automaticamente la finestra temporale scorrevole col passare del tempo.  
  
-   La funzione usa l'associazione allo schema. Per questo motivo non è possibile limitarsi ad aggiornare la funzione "sul posto" chiamando ogni giorno l'istruzione **ALTER FUNCTION** per spostare la finestra temporale scorrevole.  
  
 Iniziare con una funzione di filtro come quella dell'esempio seguente, che esegue la migrazione delle righe in cui la colonna **systemEndTime** contiene un valore precedente al 1° gennaio 2016.  
  
```sql  
CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160101(@systemEndTime datetime2)   
RETURNS TABLE   
WITH SCHEMABINDING    
AS    
RETURN SELECT 1 AS is_eligible   
  WHERE @systemEndTime < CONVERT(datetime2, '2016-01-01T00:00:00', 101) ;  
  
```  
  
 Applicare la funzione di filtro alla tabella.  
  
```sql  
ALTER TABLE <table name>   
SET (   
        REMOTE_DATA_ARCHIVE = ON   
                (   
                        FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160101(SysEndTime)  
                                , MIGRATION_STATE = OUTBOUND   
                )  
        )   
;  
  
```  
  
 Quando si desidera aggiornare la finestra temporale scorrevole, eseguire le operazioni seguenti.  
  
1.  Creare una nuova funzione che specifica la nuova finestra temporale scorrevole. L'esempio seguente seleziona le date precedenti al 2 gennaio 2016 anziché all'1 gennaio 2016.  
  
2.  Sostituire la funzione di filtro precedente con quella nuova chiamando **ALTER TABLE**, come illustrato nell'esempio seguente.  
  
3.  Facoltativamente, eliminare la precedente funzione di filtro non più in uso chiamando **DROP FUNCTION**. Questo passaggio non è illustrato nell'esempio.  
  
```sql  
BEGIN TRAN  
GO  
        /*(1) Create new predicate function definition */  
        CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160102(@systemEndTime datetime2)  
        RETURNS TABLE  
        WITH SCHEMABINDING   
        AS   
        RETURN SELECT 1 AS is_eligible  
               WHERE @systemEndTime < CONVERT(datetime2,'2016-01-02T00:00:00', 101)  
        GO  
  
        /*(2) Set the new function as the filter predicate */  
        ALTER TABLE <table name>  
        SET   
        (  
               REMOTE_DATA_ARCHIVE = ON  
               (  
                       FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160102(SysEndTime),  
                       MIGRATION_STATE = OUTBOUND  
               )  
        )   
COMMIT ;  
  
```  
  
## <a name="more-examples-of-valid-filter-functions"></a>Altri esempi di funzioni di filtro valide  
  
-   L'esempio seguente unisce due condizioni primitive usando l'operatore logico AND.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_stretchpredicate((@column1 datetime, @column2 nvarchar(15))  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
      WHERE @column1 < N'20150101' AND @column2 IN (N'Completed', N'Returned', N'Cancelled')  
    GO  
  
    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (  
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date, shipment_status),  
        MIGRATION_STATE = OUTBOUND  
    ) )  
  
    ```  
  
-   L'esempio seguente usa diverse condizioni e una conversione deterministica con CONVERT.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_stretchpredicate_example1(@column1 datetime, @column2 int, @column3 nvarchar)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101) AND (@column2 < -100 OR @column2 > 100 OR @column2 IS NULL) AND @column3 IN (N'Completed', N'Returned', N'Cancelled')  
    GO  
  
    ```  
  
-   L'esempio seguente usa funzioni e operatori matematici.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_stretchpredicate_example2(@column1 float)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 < SQRT(400) + 10  
    GO  
  
    ```  
  
-   L'esempio seguente usa gli operatori BETWEEN e NOT BETWEEN. Questo utilizzo è valido poiché la funzione risultante è conforme alle regole descritte qui dopo aver sostituito gli operatori BETWEEN e NOT BETWEEN con le espressioni AND e OR equivalenti.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_stretchpredicate_example3(@column1 int, @column2 int)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 BETWEEN 0 AND 100  
                AND (@column2 NOT BETWEEN 200 AND 300 OR @column1 = 50)  
    GO  
  
    ```  
  
     La funzione precedente è equivalente alla funzione riportata di seguito dopo la sostituzione degli operatori BETWEEN e NOT BETWEEN con le espressioni equivalenti AND e OR.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_stretchpredicate_example4(@column1 int, @column2 int)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 >= 0 AND @column1 <= 100 AND (@column2 < 200 OR @column2 > 300 OR @column1 = 50)  
    GO  
  
    ```  
  
## <a name="examples-of-filter-functions-that-arent-valid"></a>Esempi di funzioni di filtro non valide  
  
-   La funzione seguente non è valida perché contiene una conversione non deterministica.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_example5(@column1 datetime)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 < CONVERT(datetime, '1/1/2016')  
    GO  
  
    ```  
  
-   La funzione seguente non è valida perché contiene una chiamata di funzione non deterministica.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_example6(@column1 datetime)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 < DATEADD(day, -60, GETDATE())  
    GO  
  
    ```  
  
-   La funzione seguente non è valida perché contiene una sottoquery.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_example7(@column1 int)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 IN (SELECT SupplierID FROM Supplier WHERE Status = 'Defunct')  
    GO  
  
    ```  
  
-   Le funzioni seguenti non sono valide perché le espressioni che usano operatori algebrici o funzioni predefinite devono restituire una costante quando si definisce la funzione. È possibile includere riferimenti a colonne in chiamate di funzione o espressioni algebriche.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_example8(@column1 int)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 % 2 =  0  
    GO  
  
    CREATE FUNCTION dbo.fn_example9(@column1 int)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE SQRT(@column1) = 30  
    GO  
  
    ```  
  
-   La funzione seguente non è valida perché viola le regole descritte qui dopo la sostituzione dell'operatore BETWEEN con l'espressione AND equivalente.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_example10(@column1 int, @column2 int)  
    RETURNS TABLE  
    WITH SCHEMABINDING  
    AS  
    RETURN  SELECT 1 AS is_eligible  
            WHERE (@column1 BETWEEN 1 AND 200 OR @column1 = 300) AND @column2 > 1000  
    GO  
  
    ```  
  
     La funzione precedente è equivalente alla funzione riportata di seguito dopo la sostituzione dell'operatore BETWEEN con l'espressione AND equivalente. Questa funzione non è valida perché le condizioni primitive possono usare solo l'operatore logico OR.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_example11(@column1 int, @column2 int)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE (@column1 >= 1 AND @column1 <= 200 OR @column1 = 300) AND @column2 > 1000  
    GO  
  
    ```  
  
## <a name="how-stretch-database-applies-the-filter-function"></a>Come viene applicata la funzione del filtro da parte di Stretch Database  
 Stretch Database applica la funzione del filtro alla tabella e determina le righe idonee tramite l'operatore CROSS APPLY. Ad esempio:  
  
```sql  
SELECT * FROM stretch_table_name CROSS APPLY fn_stretchpredicate(column1, column2)  
```  
  
 Se la funzione restituisce un risultato non vuoto per la riga, la riga è idonea per la migrazione.  
  
## <a name="replace-an-existing-filter-function"></a><a name="replacePredicate"></a>Sostituire una funzione di filtro esistente  
 Per sostituire una funzione di filtro specificata in precedenza, eseguire di nuovo l'istruzione **ALTER TABLE** e specificare un valore nuovo per il parametro **FILTER_PREDICATE** . Ad esempio:  
  
```sql  
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (  
    FILTER_PREDICATE = dbo.fn_stretchpredicate2(column1, column2),  
    MIGRATION_STATE = <desired_migration_state>  
    ) ) 
  
```  
  
 La nuova funzione inline con valori di tabella ha i requisiti seguenti.  
  
-   La nuova funzione deve essere meno restrittiva rispetto alla funzione precedente.  
  
-   Tutti gli operatori presenti nella funzione precedente devono essere presente nella nuova funzione.  
  
-   La nuova funzione non può contenere operatori che non sono presenti nella funzione precedente.  
  
-   Non è possibile modificare l'ordine degli argomenti dell'operatore.  
  
-   Solo i valori costanti che fanno parte di un confronto `<, <=, >, >=`  possono essere modificati in modo da rendere meno restrittiva la funzione.  
  
### <a name="example-of-a-valid-replacement"></a>Esempio di sostituzione valida  
 Si supponga che la funzione seguente sia la funzione di filtro corrente.  
  
```sql  
CREATE FUNCTION dbo.fn_stretchpredicate_old(@column1 datetime, @column2 int)  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
RETURN  SELECT 1 AS is_eligible  
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)  
            AND (@column2 < -100 OR @column2 > 100)  
GO  
  
```  
  
 La funzione seguente è una sostituzione valida perché la nuova costante di data, che specifica una data limite successiva, rende meno restrittiva la funzione.  
  
```sql  
CREATE FUNCTION dbo.fn_stretchpredicate_new(@column1 datetime, @column2 int)  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
RETURN  SELECT 1 AS is_eligible  
        WHERE @column1 < CONVERT(datetime, '2/1/2016', 101)  
            AND (@column2 < -50 OR @column2 > 50)  
GO  
  
```  
  
### <a name="examples-of-replacements-that-arent-valid"></a>Esempi di sostituzioni non valide  
 La funzione seguente non è una sostituzione valida perché la nuova costante di data, che specifica una data limite precedente, non rende meno restrittiva la funzione.  
  
```sql  
CREATE FUNCTION dbo.fn_notvalidreplacement_1(@column1 datetime, @column2 int)  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
RETURN  SELECT 1 AS is_eligible  
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101)  
            AND (@column2 < -100 OR @column2 > 100)  
GO  
  
```  
  
 La funzione seguente non è una sostituzione valida perché uno degli operatori di confronto è stato rimosso.  
  
```sql  
CREATE FUNCTION dbo.fn_notvalidreplacement_2(@column1 datetime, @column2 int)  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
RETURN  SELECT 1 AS is_eligible  
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)  
            AND (@column2 < -50)  
GO  
  
```  
  
 La funzione seguente non è una sostituzione valida perché è stata aggiunta una nuova condizione con l'operatore logico AND.  
  
```sql  
CREATE FUNCTION dbo.fn_notvalidreplacement_3(@column1 datetime, @column2 int)  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
RETURN  SELECT 1 AS is_eligible  
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)  
            AND (@column2 < -100 OR @column2 > 100)  
            AND (@column2 <> 0)  
GO  
  
```  
  
## <a name="remove-a-filter-function-from-a-table"></a>Rimuovere una funzione di filtro da una tabella  
 Per eseguire la migrazione dell'intera tabella anziché delle righe selezionate, rimuovere la funzione esistente impostando **FILTER_PREDICATE**  su null. Ad esempio:  
  
```sql  
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (  
    FILTER_PREDICATE = NULL,  
    MIGRATION_STATE = <desired_migration_state>  
) )  
  
```  
  
 Dopo aver rimosso la funzione di filtro, tutte le righe della tabella sono idonee per la migrazione. Di conseguenza, è possibile specificare una funzione di filtro per la stessa tabella in un secondo momento solo se si recuperano prima tutti i dati remoti della tabella da Azure. Questa restrizione ha lo scopo di evitare che quando si specifica una nuova funzione di filtro le righe non idonee per la migrazione siano già state migrate in Azure.  
  
## <a name="check-the-filter-function-applied-to-a-table"></a>Controllare la funzione di filtro applicata a una tabella  
 Per controllare la funzione di filtro applicata a una tabella, aprire la vista del catalogo **sys.remote_data_archive_tables** e controllare il valore per la colonna **filter_predicate** . Se il valore è null, l'intera tabella è idonea per l'archiviazione. Per altre info, vedere [sys.remote_data_archive_tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/stretch-database-catalog-views-sys-remote-data-archive-tables.md).  
  
## <a name="security-notes-for-filter-functions"></a>Note sulla sicurezza per le funzioni di filtro  
Un account compromesso con privilegi db_owner può eseguire le operazioni seguenti.  
  
-   Creare e applicare una funzione con valori di tabella che usa grandi quantità di risorse del server o rimane in attesa per un lungo periodo, risultando in un risultante in un attacco Denial of Service.  
  
-   Creare e applicare una funzione con valori di tabella che rende possibile dedurre il contenuto di una tabella per la quale all'utente è stato esplicitamente negato l'accesso in lettura.  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
  
  
