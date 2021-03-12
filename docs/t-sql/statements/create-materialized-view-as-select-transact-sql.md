---
description: CREATE MATERIALIZED VIEW AS SELECT (Transact-SQL)
title: CREATE MATERIALIZED VIEW AS SELECT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse
ms.reviewer: jrasnick
ms.technology: data-warehouse
ms.topic: reference
f1_keywords:
- CREATE VIEW
- VIEW_TSQL
- VIEW
- CREATE_VIEW_TSQL
- SCHEMABINDING_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- table creation [SQL Server], CREATE VIEW
- views [SQL Server], creating
- CREATE VIEW statement
- updatable partitioned views
- tables [SQL Server], virtual
- updatable views
- modifying partitioned views
- virtual tables [SQL Server]
- number of columns per view
- partitioned views [SQL Server], creating
- WITH ENCRYPTION clause
- WITH CHECK OPTION clause
- partitioned views [SQL Server], modifying
- partitioned views [SQL Server], replication
- distributed partitioned views [SQL Server]
- views [SQL Server], indexed views
- maximum number of columns per view
ms.assetid: aecc2f73-2ab5-4db9-b1e6-2f9e3c601fb9
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: 81073d458d69e28ee145c1bb1dd38f3474800b35
ms.sourcegitcommit: f10f0d604be1dce6c600a92aec4c095e7b52e19c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/11/2021
ms.locfileid: "102770539"
---
# <a name="create-materialized-view-as-select-transact-sql"></a>CREATE MATERIALIZED VIEW AS SELECT (Transact-SQL)  

[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

Questo articolo illustra l'istruzione CREATE MATERIALIZED VIEW AS SELECT T-SQL in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] per lo sviluppo di soluzioni. L'articolo include anche esempi di codice.

Una vista materializzata rende persistenti i dati restituiti dalla query di definizione della vista e viene aggiornata automaticamente in caso di modifiche dei dati nelle tabelle sottostanti.   Migliora le prestazioni delle query complesse, in genere le query con join e aggregazioni, offrendo allo stesso tempo operazioni di manutenzione semplici.   Con la funzionalità di corrispondenza automatica del piano di esecuzione, non sarà necessario fare riferimento a una vista materializzata nella query per fare in modo che Query Optimizer prenda in considerazione la vista per la sostituzione.  Questa funzionalità consente ai progettisti dei dati di implementare le viste materializzate come meccanismo per migliorare il tempo di risposta delle query, senza doverle modificare.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CREATE MATERIALIZED VIEW [ schema_name. ] materialized_view_name
    WITH (  
      <distribution_option>
    )
    AS <select_statement>
[;]

<distribution_option> ::=
    {  
        DISTRIBUTION = HASH ( distribution_column_name )  
      | DISTRIBUTION = ROUND_ROBIN  
    }

<select_statement> ::=
    SELECT select_criteria

```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="arguments"></a>Argomenti

*schema_name*     
 Nome dello schema a cui appartiene la vista.  
  
*materialized_view_name*   
Nome della vista. I nomi di vista devono essere conformi alle regole per gli identificatori. Il nome del proprietario della vista è facoltativo.  

*Opzione di distribuzione*     
Sono supportate solo le distribuzioni HASH e ROUND_ROBIN.

*select_statement*   
L'elenco SELECT nella definizione della vista materializzata deve soddisfare almeno uno di questi due criteri:
- L'elenco SELECT contiene una funzione di aggregazione.
- Viene usata l'istruzione GROUP BY nella definizione della vista materializzata e tutte le colonne in GROUP BY vengono incluse nell'elenco SELECT.  Nella clausola GROUP BY si possono usare fino a 32 colonne.

Sono necessarie funzioni di aggregazione nell'elenco SELECT della definizione della vista materializzata.  Le aggregazioni supportate includono MAX, MIN, AVG, COUNT, COUNT_BIG, SUM, VAR, STDEV.

Se si usano le aggregazioni MIN/MAX nell'elenco SELECT della definizione della vista materializzata, si applicano i requisiti seguenti:
 
- FOR_APPEND è obbligatorio.  Ad esempio:
  ```sql 
  CREATE MATERIALIZED VIEW mv_test2  
  WITH (distribution = hash(i_category_id), FOR_APPEND)  
  AS
  SELECT MAX(i.i_rec_start_date) as max_i_rec_start_date, MIN(i.i_rec_end_date) as min_i_rec_end_date, i.i_item_sk, i.i_item_id, i.i_category_id
  FROM syntheticworkload.item i  
  GROUP BY i.i_item_sk, i.i_item_id, i.i_category_id
  ```

- La vista materializzata verrà disabilitata quando si verifica un'operazione UPDATE o DELETE nelle tabelle di base a cui viene fatto riferimento.  Questa restrizione non si applica alle operazioni INSERT.  Per abilitare nuovamente la vista materializzata, eseguire ALTER MATERIALIZED VIEW con REBUILD.
  
## <a name="remarks"></a>Commenti

Una vista materializzata nel data warehouse di Azure è simile a una vista indicizzata in SQL Server.Condivide quasi le stesse restrizioni della vista indicizzata (vedere [Creare viste indicizzate](../../relational-databases/views/create-indexed-views.md) per informazioni dettagliate) ad eccezione del fatto che una vista materializzata supporta le funzioni di aggregazione.   

>[!Note]
>Sebbene CREATE MATERIALIZED VIEW non supporti COUNT, DISTINCT, COUNT(espressione DISTINCT) o COUNT_BIG (espressione DISTINCT), le query SELECT con queste funzioni possono comunque trarre vantaggio dalle viste materializzate per ottenere prestazioni più veloci perché l'ottimizzatore Synapse SQL è in grado di riscrivere automaticamente le aggregazioni nella query utente in modo che corrispondano a viste materializzate esistenti.  Per informazioni dettagliate, vedere la sezione di esempio di questo articolo. 

La funzione APPROX_COUNT_DISTINCT non è supportata in CREATE MATERIALIZED VIEW AS SELECT.

La vista materializzata supporta solo CLUSTERED COLUMNSTORE INDEX. 

Una vista materializzata non può fare riferimento ad altre viste.  

Non è possibile creare una vista materializzata in una tabella con Maschera dati dinamica (DDM), anche se la colonna DDM non fa parte della vista materializzata.  Se una colonna della tabella fa parte di una vista materializzata attiva o una vista materializzata disabilitata, non è possibile aggiungere DDM a questa colonna.  

Non è possibile creare una vista materializzata in una tabella con sicurezza a livello di riga abilitata.

È possibile creare viste materializzate su tabelle partizionate.  Le operazioni SPLIT/MERGE sulle partizioni sono supportate per le tabelle di base delle viste materializzate. L'operazione SWITCH per una partizione non è supportata.  
 
Le operazioni ALTER TABLE SWITCH non sono supportate sulle tabelle a cui fanno riferimento le viste materializzate. Disabilitare o eliminare le viste materializzate prima di usare ALTER TABLE SWITCH. Negli scenari seguenti la creazione della vista materializzata richiede l'aggiunta di nuove colonne alla vista materializzata:

|Scenario|Nuove colonne da aggiungere alla vista materializzata|Commento|  
|-----------------|---------------|-----------------|
|COUNT_BIG() non è presente nell'elenco SELECT di una definizione di vista materializzata| COUNT_BIG (*) |Viene aggiunta automaticamente dalla creazione di una vista materializzata.  Non è richiesta alcuna azione da parte dell'utente.|
|La funzione SUM(a) viene specificata dagli utenti nell'elenco SELECT della definizione di una vista materializzata e 'a' è un'espressione che ammette i valori Null |COUNT_BIG (a) |Gli utenti devono aggiungere l'espressione 'a' manualmente nella definizione della vista materializzata.|
|La funzione AVG(a) viene specificata dagli utenti nell'elenco SELECT della definizione di una vista materializzata e 'a' è un'espressione.|SUM(a), COUNT_BIG(a)|Viene aggiunta automaticamente dalla creazione di una vista materializzata.  Non è richiesta alcuna azione da parte dell'utente.|
|La funzione STDEV(a) viene specificata dagli utenti nell'elenco SELECT della definizione di una vista materializzata e 'a' è un'espressione.|SUM(a), COUNT_BIG(a), SUM(square(a))|Viene aggiunta automaticamente dalla creazione di una vista materializzata.  Non è richiesta alcuna azione da parte dell'utente. |
| | | |

Dopo la creazione, le viste materializzate sono visibili all'interno di SQL Server Management Studio nella cartella delle viste dell'istanza di [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)].

Gli utenti possono eseguire [SP_SPACEUSED](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md?view=azure-sqldw-latest&preserve-view=true) e [DBCC PDW_SHOWSPACEUSED](../database-console-commands/dbcc-pdw-showspaceused-transact-sql.md?view=azure-sqldw-latest&preserve-view=true) per determinare lo spazio usato da una vista materializzata.  

È possibile eliminare una vista materializzata tramite DROP VIEW.  È possibile usare ALTER MATERIALIZED VIEW per disabilitare o ricostruire una vista materializzata.   

La vista materializzata è un meccanismo automatico di ottimizzazione delle query.  Gli utenti non devono eseguire query direttamente in una vista materializzata.  Quando viene inviata una query utente, il motore controlla le autorizzazioni dell'utente per gli oggetti di query e non riesce a eseguire la query senza esecuzione se l'utente non ha accesso alle tabelle o alle visualizzazioni regolari nella query.  Se l'autorizzazione dell'utente è stata verificata, Query Optimizer utilizza automaticamente una vista materializzata corrispondente per eseguire la query per ottenere prestazioni più veloci.  Gli utenti restituiscono gli stessi dati indipendentemente dal fatto che la query venga gestita eseguendo una query sulle tabelle di base o sulla vista materializzata.  

EXPLAIN PLAN e il piano di esecuzione stimato grafico in SQL Server Management Studio possono indicare se una vista materializzata viene presa in considerazione da Query Optimizer per l'esecuzione delle query. Il piano di esecuzione stimato grafico in SQL Server Management Studio può indicare se una vista materializzata viene presa in considerazione da Query Optimizer per l'esecuzione delle query.

Per scoprire se un'istruzione SQL può trarre vantaggio da una nuova vista materializzata, eseguire il comando `EXPLAIN` con `WITH_RECOMMENDATIONS`.  Per informazioni dettagliate, vedere [EXPLAIN (Transact-SQL)](../queries/explain-transact-sql.md?view=azure-sqldw-latest&preserve-view=true).

## <a name="ownership"></a>Proprietario
- Non è possibile creare una vista materializzata se i proprietari delle tabelle di base e la vista materializzata da creare non sono uguali.
- Una vista materializzata e le relative tabelle di base possono risiedere in schemi diversi. Quando viene creata la vista materializzata, il proprietario dello schema della vista diventa automaticamente il proprietario della vista materializzata e la proprietà della vista non può essere modificata.     

## <a name="permissions"></a>Autorizzazioni
Un utente deve avere le autorizzazioni seguenti per creare una vista materializzata, oltre a soddisfare i requisiti di proprietà dell'oggetto: 
1) Autorizzazione CREATE VIEW nel database
2) Autorizzazione SELECT per le tabelle di base della vista materializzata
3) Autorizzazione REFERENCEs per lo schema contenente le tabelle di base
4) Autorizzazione ALTER per lo schema che contiene la vista materializzata 


## <a name="example"></a>Esempio
R. Questo esempio illustra come l'ottimizzatore Synapse SQL usa automaticamente le viste materializzate per eseguire una query per ottenere prestazioni migliori anche quando la query usa funzioni non supportate in CREATE MATERIALIZED VIEW, come COUNT(espressione DISTINCT). Una query che richiedeva vari secondi per il completamento viene ora completata in meno di un secondo senza alcuna modifica.   

``` sql 

-- Create a table with ~536 million rows
create table t(a int not null, b int not null, c int not null) with (distribution=hash(a), clustered columnstore index);

insert into t values(1,1,1);

declare @p int =1;
while (@P < 30)
    begin
    insert into t select a+1,b+2,c+3 from t;  
    select @p +=1;
end

-- A SELECT query with COUNT_BIG (DISTINCT expression) took multiple seconds to complete and it reads data directly from the base table a. 
select a, count_big(distinct b) from t group by a;

-- Create two materialized views, not using COUNT_BIG(DISTINCT expression).
create materialized view V1 with(distribution=hash(a)) as select a, b from dbo.t group by a, b;

-- Clear all cache.

DBCC DROPCLEANBUFFERS;
DBCC freeproccache;

-- Check the estimated execution plan in SQL Server Management Studio.  It shows the SELECT query is first step (GET operator) is to read data from the materialized view V1, not from base table a.
select a, count_big(distinct b) from t group by a;

-- Now execute this SELECT query.  This time it took sub-second to complete because Synapse SQL engine automatically matches the query with materialized view V1 and uses it for faster query execution.  There was no change in the user query.

DECLARE @timerstart datetime2, @timerend datetime2;
SET @timerstart = sysdatetime();

select a, count_big(distinct b) from t group by a;

SET @timerend = sysdatetime()
select DATEDIFF(ms,@timerstart,@timerend);

```

B. In questo esempio, User2 crea una vista materializzata sulle tabelle di proprietà di User1.  La vista materializzata è di proprietà di User1.
```sql
/****************************************************************
Setup:
SchemaX owner = DBO
SchemaX.T1 owner = User1
SchemaX.T2 owner = User1
SchemaY owner = User1
*****************************************************************/
CREATE USER User1 WITHOUT LOGIN ;
CREATE USER User2 WITHOUT LOGIN ;
CREATE SCHEMA SchemaX;  
CREATE SCHEMA SchemaY AUTHORIZATION User1;
GO
CREATE TABLE [SchemaX].[T1] (   [vendorID] [varchar](255) Not NULL, [totalAmount] [float] Not NULL, [puYear] [int] NULL );
CREATE TABLE [SchemaX].[T2] (   [vendorID] [varchar](255) Not NULL, [totalAmount] [float] Not NULL, [puYear] [int] NULL);
GO
ALTER AUTHORIZATION ON OBJECT::SchemaX.[T1] TO User1;
ALTER AUTHORIZATION ON OBJECT::SchemaX.[T2] TO User1;

/*****************************************************************************
For user2 to create a MV in SchemaY on SchemaX.T1 and SchemaX.T2, user2 needs:
1. CREATE VIEW permission in the database
2. REFERENCES permission on the schema1
3. SELECT permission on base table T1, T2  
4. ALTER permission on SchemaY
******************************************************************************/
GRANT CREATE VIEW to User2;
GRANT REFERENCES ON SCHEMA::SchemaX to User2;  
GRANT SELECT ON OBJECT::SchemaX.T1 to User2; 
GRANT SELECT ON OBJECT::SchemaX.T2 to User2;
GRANT ALTER ON SCHEMA::SchemaY to User2; 
GO
EXECUTE AS USER = 'User2';  
GO
CREATE materialized VIEW [SchemaY].MV_by_User2 with(distribution=round_robin) 
as 
        select A.vendorID, sum(A.totalamount) as S, Count_Big(*) as T 
        from [SchemaX].[T1] A
        inner join [SchemaX].[T2] B on A.vendorID = B.vendorID group by A.vendorID ;
GO
revert;
GO
```

## <a name="see-also"></a>Vedi anche

[Ottimizzazione delle prestazioni con vista materializzata](/azure/sql-data-warehouse/performance-tuning-materialized-views)   
[ALTER MATERIALIZED VIEW &#40;Transact-SQL&#41;](./alter-materialized-view-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)      
[DROP VIEW](./drop-view-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)  
[EXPLAIN &#40;Transact-SQL&#41;](../queries/explain-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_column_distribution_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-column-distribution-properties-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_distribution_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-distribution-properties-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_mappings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-mappings-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD &#40;Transact-SQL&#41;](../database-console-commands/dbcc-pdw-showmaterializedviewoverhead-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[Viste del catalogo di [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
[Viste di sistema supportate in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-system-views)   
[Istruzioni T-SQL supportate in Azure [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-statements)
