---
description: CREATE STATISTICS (Transact-SQL)
title: CREATE STATISTICS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/04/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STATISTICS
- STATISTICS_TSQL
- CREATE STATISTICS
- CREATE_STATISTICS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- query optimization statistics [SQL Server], creating
- indexed views [SQL Server], statistics
- FULLSCAN option
- CREATE STATISTICS statement
- filtered statistics [SQL Server]
- creating statistics [SQL Server]
- NORECOMPUTE clause
ms.assetid: b23e2f6b-076c-4e6d-9281-764bdb616ad2
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f7dbfafd8cdcca4c34f25d979b660cd9cbaa679b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748222"
---
# <a name="create-statistics-transact-sql"></a>CREATE STATISTICS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Crea statistiche di ottimizzazione query per una o più colonne di una tabella o di una vista indicizzata o in una tabella esterna. Per la maggior parte delle query, Query Optimizer genera già le statistiche necessarie per un piano di query di alta qualità. In alcuni casi, è necessario creare statistiche aggiuntive con CREATE STATISTICS o modificare la progettazione delle query per ottenere prestazioni migliori di esecuzione delle query.  
  
 Per altre informazioni, vedere [Statistiche](../../relational-databases/statistics/statistics.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
-- Create statistics on an external table  
CREATE STATISTICS statistics_name   
ON { table_or_indexed_view_name } ( column [ ,...n ] )   
    [ WITH FULLSCAN ] ;  
  
-- Create statistics on a regular table or indexed view  
CREATE STATISTICS statistics_name   
ON { table_or_indexed_view_name } ( column [ ,...n ] )   
    [ WHERE <filter_predicate> ]  
    [ WITH   
        [ [ FULLSCAN   
            [ [ , ] PERSIST_SAMPLE_PERCENT = { ON | OFF } ]    
          | SAMPLE number { PERCENT | ROWS }   
            [ [ , ] PERSIST_SAMPLE_PERCENT = { ON | OFF } ]    
          | <update_stats_stream_option> [ ,...n ]    
        [ [ , ] NORECOMPUTE ]   
        [ [ , ] INCREMENTAL = { ON | OFF } ] 
        [ [ , ] MAXDOP = max_degree_of_parallelism ]
    ] ;  
  
<filter_predicate> ::=   
    <conjunct> [AND <conjunct>]  
  
<conjunct> ::=  
    <disjunct> | <comparison>  
  
<disjunct> ::=  
        column_name IN (constant ,...)  
  
<comparison> ::=  
        column_name <comparison_op> constant  
  
<comparison_op> ::=  
    IS | IS NOT | = | <> | != | > | >= | !> | < | <= | !<  
    
<update_stats_stream_option> ::=  
    [ STATS_STREAM = stats_stream ]  
    [ ROWCOUNT = numeric_constant ]  
    [ PAGECOUNT = numeric_contant ] 
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
CREATE STATISTICS statistics_name   
    ON { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( column_name  [ ,...n ] )   
    [ WHERE <filter_predicate> ]  
    [ WITH {  
           FULLSCAN   
           | SAMPLE number PERCENT   
      }  
    ]  
[;]  
  
<filter_predicate> ::=   
    <conjunct> [AND <conjunct>]  
  
<conjunct> ::=  
    <disjunct> | <comparison>  
  
<disjunct> ::=  
        column_name IN (constant ,...)  
  
<comparison> ::=  
        column_name <comparison_op> constant  
  
<comparison_op> ::=  
    IS | IS NOT | = | <> | != | > | >= | !> | < | <= | !<  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *statistics_name*  
 Nome delle statistiche da creare.  
  
 *table_or_indexed_view_name*  
 Nome della tabella, della vista indicizzata o della tabella esterna su cui creare le statistiche. Per creare statistiche su un altro database, specificare un nome di tabella completo.  
  
 *column [ ,...n]*  
 Una o più colonne da includere nelle statistiche. Le colonne devono essere in ordine di priorità da sinistra a destra. Per creare l'istogramma viene usata solo la prima colonna. Per le statistiche di correlazione tra colonne, denominate densità, vengono usate tutte le colonne.  
  
 È possibile specificare qualsiasi colonna utilizzabile come colonna chiave di indice, con le eccezioni seguenti:  
  
-   Non è possibile specificare colonne **Xml**, full-text e FILESTREAM.  
  
-   È possibile specificare colonne calcolate solo se le opzioni del database ARITHABORT e QUOTED_IDENTIFIER sono impostate su ON.  
  
-   È possibile specificare colonne di tipo CLR definito dall'utente se il tipo supporta l'ordinamento binario. È possibile specificare colonne calcolate definite come chiamate di metodi da una colonna con tipo definito dall'utente se tali metodi sono contrassegnati come deterministici.  
  
 WHERE \<filter_predicate> Specifica un'espressione per la selezione di un subset di righe da includere durante la creazione dell'oggetto statistiche. Le statistiche create con un predicato del filtro vengono definite statistiche filtrate. Il predicato del filtro usa una logica di confronto semplice e non può fare riferimento a una colonna calcolata, con tipo definito dall'utente (UDT), con tipo di dati spaziale o con tipo di dati **hierarchyID**. I confronti in cui vengono utilizzati valori letterali NULL non sono consentiti con gli operatori di confronto. In alternativa, utilizzare gli operatori IS NULL e IS NOT NULL.  
  
 Di seguito sono riportati alcuni esempi di predicati di filtro per la tabella Production.BillOfMaterials:  
  
 * `WHERE StartDate > '20000101' AND EndDate <= '20000630'`  
  
 * `WHERE ComponentID IN (533, 324, 753)`  
  
 * `WHERE StartDate IN ('20000404', '20000905') AND EndDate IS NOT NULL`  
  
 Per altre informazioni sui predicati di filtro, vedere [Creare indici filtrati](../../relational-databases/indexes/create-filtered-indexes.md).  
  
 FULLSCAN  
 Consente di calcolare le statistiche analizzando tutte le righe. FULLSCAN e SAMPLE 100 PERCENT generano gli stessi risultati. Non è possibile utilizzare FULLSCAN con l'opzione SAMPLE.  
  
 Se si omette FULLSCAN, SQL Server crea le statistiche tramite campionamento e determina le dimensioni del campione necessarie per creare un piano di query di qualità elevata  
  
 SAMPLE *number* { PERCENT | ROWS }  
 Specifica la percentuale approssimativa o il numero di righe presenti nella tabella o nella vista indicizzata utilizzate da Query Optimizer durante la creazione delle statistiche. Per PERCENT, *number* può essere compreso tra 0 e 100, mentre per ROWS *number* può essere compreso tra 0 e il numero totale di righe. La percentuale effettiva o il numero di righe campionate da Query Optimizer potrebbero non corrispondere alla percentuale o al numero specificato. Query Optimizer analizza ad esempio tutte le righe in una pagina di dati.  
  
 SAMPLE è utile per i casi speciali in cui il piano di query, basato sul campionamento predefinito, non è ottimale. Nella maggior parte delle situazioni, non è necessario specificare SAMPLE perché Query Optimizer utilizza già il campionamento e determina le dimensioni del campione statisticamente significative per impostazione predefinita, come richiesto per creare piani di query di alta qualità.  
  
 Questa opzione non può essere utilizzata quando viene specificata l'opzione FULLSCAN. Se non si specifica né SAMPLE né FULLSCAN, Query Optimizer utilizza i dati campionati e calcola le dimensioni del campione per impostazione predefinita.  
  
 Si sconsiglia di specificare 0 PERCENT o 0 ROWS. Se si specifica 0 PERCENT o ROWS, l'oggetto statistiche viene creato ma non conterrà i dati delle statistiche.  
 
 PERSIST_SAMPLE_PERCENT = { ON | OFF }  
 Se **ON**, le statistiche mantengono la percentuale di campionamento di creazione per gli aggiornamenti successivi che non specificano in modo esplicito una percentuale di campionamento. Se **OFF**, la percentuale di campionamento delle statistiche viene reimpostata sul campionamento predefinito per gli aggiornamenti successivi che non la specificano in modo esplicito. Il valore predefinito è **OFF**. 
 
 > [!NOTE]
 > Se la tabella viene troncata, per tutte le statistiche compilate in base alla struttura HoBT viene ripristinato l'uso della percentuale di campionamento predefinita.

 **Si applica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] (a partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 CU4) e versioni successive (a partire da [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU1).    
  
 STATS_STREAM **=** _stats_stream_  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
 NORECOMPUTE  
 Consente di disabilitare l'opzione di aggiornamento automatico delle statistiche AUTO_STATISTICS_UPDATE per *statistics_name*. Se viene specificata questa opzione, Query Optimizer completa gli aggiornamenti delle statistiche in corso per *statistics_name* e disabilita gli aggiornamenti futuri.  
  
 Per riabilitare gli aggiornamenti delle statistiche, rimuovere le statistiche con [DROP STATISTICS](../../t-sql/statements/drop-statistics-transact-sql.md) ed eseguire CREATE STATISTICS senza l'opzione NORECOMPUTE.  
  
> [!WARNING]  
> L'utilizzo di questa opzione può produrre piani di query non ottimali. È consigliabile limitare l'utilizzo di questa opzione e riservarne l'applicazione a un amministratore del sistema qualificato.  
  
 Per altre informazioni sull'opzione AUTO_STATISTICS_UPDATE, vedere [Opzioni ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md). Per altre informazioni sulla disabilitazione e sulla riabilitazione degli aggiornamenti delle statistiche, vedere [Statistiche](../../relational-databases/statistics/statistics.md).  
  
 INCREMENTAL = { ON | OFF }  
 Se è specificato **ON**, le statistiche create sono statistiche per partizione. Se **OFF**, le statistiche sono combinate per tutte le partizioni. Il valore predefinito è **OFF**.  
  
 Se le statistiche per partizione non sono supportate, viene generato un errore. Le statistiche incrementali non sono supportate per i seguenti tipi di statistiche:  
  
-   Statistiche create con indici che non hanno il partizionamento allineato con la tabella di base.  
-   Statistiche create per i database secondari leggibili Always On.  
-   Statistiche create per i database di sola lettura.  
-   Statistiche create per gli indici filtrati.  
-   Statistiche create per le viste.  
-   Statistiche create per le tabelle interne.  
-   Statistiche create con indici spaziali o indici XML.  
  
**Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.  
  
MAXDOP = *max_degree_of_parallelism*  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 e [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3).  
  
 Esegue l'override dell'opzione di configurazione **max_degree_of_parallelism** per la durata dell'operazione statistica. Per altre informazioni, vedere [Configurare l'opzione di configurazione del server max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md). Utilizzare MAXDOP per limitare il numero di processori utilizzati durante l'esecuzione di un piano parallelo. Il valore massimo è 64 processori.  
  
 *max_degree_of_parallelism* può essere:  
  
 1  
 Disattiva la generazione di piani paralleli.  
  
 \>1  
 Consente di limitare al valore specificato, o a un valore più basso in base al carico di lavoro corrente del sistema, il numero massimo di processori usati in un'operazione parallela statistica.  
  
 0 (predefinito)  
 Utilizza il numero effettivo di processori o un numero inferiore in base al carico di lavoro corrente del sistema.  
  
 \<update_stats_stream_option> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  

## <a name="permissions"></a>Autorizzazioni  
 Richiede una di queste autorizzazioni:  
  
-   MODIFICA TABELLA  
-   L'utente è il proprietario della tabella  
-   Appartenenza al ruolo predefinito del database **db_ddladmin**  
  
## <a name="general-remarks"></a>Osservazioni generali  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può usare tempdb per ordinare le righe campionate prima di creare le statistiche.  
  
### <a name="statistics-for-external-tables"></a>Statistiche per le tabelle esterne  
 Quando si creano statistiche per le tabelle esterne, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] importa la tabella esterna in una tabella di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] temporanea e quindi crea le statistiche. Per le statistiche di campioni, vengono importate solo le righe campionate. Se si ha una tabella esterna di grandi dimensioni, è molto più veloce usare il campionamento predefinito anziché l'opzione di analisi completa.  
  
### <a name="statistics-with-a-filtered-condition"></a>Statistiche con una condizione filtrata  
 Le statistiche filtrate possono migliorare le prestazioni di esecuzione delle query che effettuano la selezione da subset ben definiti di dati. Le statistiche filtrate utilizzano un predicato del filtro nella clausola WHERE per selezionare il subset di dati incluso nelle statistiche.  
  
### <a name="when-to-use-create-statistics"></a>Quando utilizzare CREATE STATISTICS  
 Per altre informazioni sulle situazioni in cui usare CREATE STATISTICS, vedere [Statistiche](../../relational-databases/statistics/statistics.md).  
  
### <a name="referencing-dependencies-for-filtered-statistics"></a>Dipendenze di riferimento per le statistiche filtrate  
 La vista del catalogo [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) registra ogni colonna nel predicato delle statistiche filtrate come dipendenza di riferimento. Tenere presente le operazioni eseguite sulle colonne della tabella prima di creare statistiche filtrate poiché non è possibile eliminare, rinominare o modificare la definizione di una colonna della tabella definita in un predicato delle statistiche filtrate.  
  
## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni  
* L'aggiornamento delle statistiche non è supportato per le tabelle esterne. Per aggiornare le statistiche in una tabella esterna, eliminare e ricreare le statistiche.  
* È possibile elencare fino a 64 colonne per ogni oggetto statistiche.
* L'opzione MAXDOP non è compatibile con le opzioni STATS_STREAM, ROWCOUNT e PAGECOUNT.
* L'opzione MAXDOP è limitata dall'impostazione MAX_DOP del gruppo di carico di lavoro di Resource Governor, se in uso.
* CREATE e DROP STATISTICS su tabelle esterne non sono supportate nel database SQL di Azure.
  
## <a name="examples"></a>Esempi  

### <a name="examples-use-the-adventureworks-database"></a>Gli esempi usano il database AdventureWorks.  

### <a name="a-using-create-statistics-with-sample-number-percent"></a>R. Utilizzo di CREATE STATISTICS con l'opzione SAMPLE number PERCENT  
 Nell'esempio seguente vengono create le statistiche `ContactMail1` usando un campione casuale del 5% delle colonne `BusinessEntityID` e `EmailPromotion` della tabella `Person` del database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
CREATE STATISTICS ContactMail1  
    ON Person.Person (BusinessEntityID, EmailPromotion)  
    WITH SAMPLE 5 PERCENT;  
```  
  
### <a name="b-using-create-statistics-with-fullscan-and-norecompute"></a>B. Utilizzo di CREATE STATISTICS con le opzioni FULLSCAN e NORECOMPUTE  
 Nell'esempio seguente vengono create le statistiche `NamePurchase` per tutte le righe nelle colonne `BusinessEntityID` e `EmailPromotion` della tabella `Person` e viene disabilitato il ricalcolo automatico delle statistiche.  
  
```sql  
CREATE STATISTICS NamePurchase  
    ON AdventureWorks2012.Person.Person (BusinessEntityID, EmailPromotion)  
    WITH FULLSCAN, NORECOMPUTE;  
```  
  
### <a name="c-using-create-statistics-to-create-filtered-statistics"></a>C. Utilizzo di CREATE STATISTICS per creare statistiche filtrate  
 Nell'esempio seguente vengono create le statistiche filtrate `ContactPromotion1`. [!INCLUDE[ssDE](../../includes/ssde-md.md)] utilizza come campione il 50% dei dati, quindi seleziona le righe in cui `EmailPromotion` è uguale a 2.  
  
```sql  
CREATE STATISTICS ContactPromotion1  
    ON Person.Person (BusinessEntityID, LastName, EmailPromotion)  
WHERE EmailPromotion = 2  
WITH SAMPLE 50 PERCENT;  
GO  
```  
  
### <a name="d-create-statistics-on-an-external-table"></a>D. Creare statistiche per una tabella esterna  
 L'unica decisione necessaria quando si creano statistiche per una tabella esterna, oltre a specificare l'elenco delle colonne, è se creare le statistiche tramite campionamento delle righe o tramite l'analisi di tutte le righe. CREATE e DROP STATISTICS su tabelle esterne non sono supportate nel database SQL di Azure.
  
 Poiché per creare le statistiche [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] importa i dati dalla tabella esterna in una tabella temporanea, l'opzione di analisi completa richiede molto più tempo. Per una tabella di grandi dimensioni, il metodo predefinito, corrispondente al campionamento, è in genere sufficiente.  
  
```sql  
--Create statistics on an external table and use default sampling.  
CREATE STATISTICS CustomerStats1 ON DimCustomer (CustomerKey, EmailAddress);  
  
--Create statistics on an external table and scan all the rows  
CREATE STATISTICS CustomerStats1 ON DimCustomer (CustomerKey, EmailAddress) WITH FULLSCAN;  
```  

### <a name="e-using-create-statistics-with-fullscan-and-persist_sample_percent"></a>E. Uso di CREATE STATISTICS con le opzioni FULLSCAN e PERSIST_SAMPLE_PERCENT  
 L'esempio seguente crea le statistiche `NamePurchase` per tutte le righe nelle colonne `BusinessEntityID` e `EmailPromotion` della tabella `Person` e imposta una percentuale di campionamento pari al 100% per tutti gli aggiornamenti successivi che non specificano in modo esplicito una percentuale di campionamento.  
  
```sql  
CREATE STATISTICS NamePurchase  
    ON AdventureWorks2012.Person.Person (BusinessEntityID, EmailPromotion)  
    WITH FULLSCAN, PERSIST_SAMPLE_PERCENT = ON;  
```  
  
### <a name="examples-using-adventureworksdw-database"></a>Esempi che usano il database AdventureWorksDW. 
  
### <a name="f-create-statistics-on-two-columns"></a>F. Creare statistiche per due colonne  
 L'esempio seguente crea le statistiche `CustomerStats1` in base alle colonne `CustomerKey` e `EmailAddress` della tabella `DimCustomer`. Le statistiche vengono create in base a un campione statisticamente significativo delle righe nella tabella `Customer`.  
  
```sql  
CREATE STATISTICS CustomerStats1 ON DimCustomer (CustomerKey, EmailAddress);  
```  
  
### <a name="g-create-statistics-by-using-a-full-scan"></a>G. Creare statistiche tramite un'analisi completa  
 L'esempio seguente crea le statistiche `CustomerStatsFullScan` in base all'analisi di tutte le righe della tabella `DimCustomer`.  
  
```sql  
CREATE STATISTICS CustomerStatsFullScan 
ON DimCustomer (CustomerKey, EmailAddress) WITH FULLSCAN;  
```  
  
### <a name="h-create-statistics-by-specifying-the-sample-percentage"></a>H. Creare statistiche specificando la percentuale di campionamento  
 L'esempio seguente crea le statistiche `CustomerStatsSampleScan` in base all'analisi del 50% delle righe della tabella `DimCustomer`.  
  
```sql  
CREATE STATISTICS CustomerStatsSampleScan 
ON DimCustomer (CustomerKey, EmailAddress) WITH SAMPLE 50 PERCENT;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Statistiche](../../relational-databases/statistics/statistics.md)   
 [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)   
 [sp_updatestats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md)   
 [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [DROP STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/drop-statistics-transact-sql.md)   
 [sys.stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md)   
 [sys.stats_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-columns-transact-sql.md)  
  
  

