---
description: Guida di ottimizzazione e convalida post-migrazione
title: Guida di ottimizzazione e convalida post-migrazione | Microsoft Docs
ms.date: 01/09/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- post-migration validation and optimization
- guide, post-migration validation and optimization
ms.assetid: 11f8017e-5bc3-4bab-8060-c16282cfbac1
author: pelopes
ms.author: harinid
ms.openlocfilehash: 397e5b7acd88f8e10dea05ce3ab2dc2f7dd105c2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350657"
---
# <a name="post-migration-validation-and-optimization-guide"></a>Guida di ottimizzazione e convalida post-migrazione

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

Il passaggio post-migrazione di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] è molto importante per riconciliare l'accuratezza e la completezza dei dati, nonché per individuare problemi di prestazioni relativi al carico di lavoro.

## <a name="common-performance-scenarios"></a>Scenari comuni relativi alle prestazioni

Di seguito sono riportati alcuni scenari comuni relativi alle prestazioni rilevati dopo la migrazione alla piattaforma [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] e viene indicato come risolverli. Sono inclusi gli scenari specifici della migrazione da [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (versioni precedenti a versioni più recenti), nonché la migrazione dalla piattaforma esterna, ad esempio Oracle, DB2, MySQL e Sybase, a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

## <a name="query-regressions-due-to-change-in-ce-version"></a><a name="CEUpgrade"></a> Regressioni delle query dovute a modifiche della versione CE

**Si applica a: migrazione da** [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

Quando si esegue la migrazione da una versione precedente di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] a [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] o versioni successive e si aggiorna il [livello di compatibilità del database](../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md) alla versione più recente disponibile, un carico di lavoro può essere esposto al rischio di regressione delle prestazioni.

Ciò avviene perché a partire da [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] tutte le modifiche di Query Optimizer sono legate al [livello di compatibilità del database](../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md) più recente, quindi i piani non vengono modificati esattamente nel punto di aggiornamento, ma quando un utente passa dall'opzione di database `COMPATIBILITY_LEVEL` a una versione più recente. Questa funzionalità, in combinazione con Archivio query, offre un alto livello di controllo sulle prestazioni delle query nel processo di aggiornamento. 

Per altre informazioni sulle modifiche di Query Optimizer introdotte in [!INCLUDE[ssSQL14](../includes/sssql14-md.md)], vedere la sezione relativa all'[ottimizzazione dei piani di query con la stima di cardinalità di SQL Server 2014](/previous-versions/dn673537(v=msdn.10)).

### <a name="steps-to-resolve"></a>Procedura di risoluzione

Modificare il [livello di compatibilità del database](../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md) in base alla versione di origine e seguire il flusso di lavoro consigliato per l'aggiornamento, come illustrato nell'immagine seguente:

![Diagramma che illustra il flusso di lavoro di aggiornamento consigliato.](../relational-databases/performance/media/query-store-usage-5.png "utilizzo archivio query 5")  

Per altre informazioni su questo argomento, vedere [Mantenere la stabilità delle prestazioni durante l'aggiornamento a SQL Server](../relational-databases/performance/query-store-usage-scenarios.md#CEUpgrade).

## <a name="sensitivity-to-parameter-sniffing"></a><a name="ParameterSniffing"></a> Sensibilità all'analisi dei parametri

**Si applica a:** migrazione da piattaforma esterna, ad esempio Oracle, DB2, MySQL e Sybase, a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

> [!NOTE]
> Per le migrazioni da [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], se questo problema si verificava nell'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] di origine, la semplice migrazione a una versione più recente di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] non risolverà il problema descritto in questo scenario. 

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] compila i piani di query sulle stored procedure analizzando i parametri di input alla prima compilazione e generando un piano con parametri riutilizzabile e ottimizzato per la distribuzione di questi dati di input. Anche se non vengono usate stored procedure, la maggior parte delle istruzioni che generano piani semplici includerà dei parametri. Dopo che un piano è stato inizialmente memorizzato nella cache, per le esecuzioni future verrà eseguito il mapping a un piano precedentemente memorizzato nella cache.
Un potenziale problema si presenta nel caso in cui durante questa prima compilazione non siano stati usati i set di parametri più comuni per il normale carico di lavoro. Per parametri diversi, lo stesso piano di esecuzione diventa inefficiente. Per altre informazioni su questo argomento, vedere il blog sull'[analisi dei parametri](../relational-databases/query-processing-architecture-guide.md#ParamSniffing).

### <a name="steps-to-resolve"></a>Procedura di risoluzione

1.  Usare l'hint `RECOMPILE`. Ogni volta viene calcolato un piano adattato al valore di ogni parametro.
2.  Riscrivere la stored procedure in modo da usare l'opzione `(OPTIMIZE FOR(<input parameter> = <value>))`. Individuare il valore che soddisfa la maggior parte del carico di lavoro pertinente, creando e gestendo un piano che diventa efficiente per il valore con parametri.
3.  Riscrivere la stored procedure usando la variabile locale all'interno della stored procedure. Ora l'utilità di ottimizzazione usa il vettore di densità per le stime, ottenendo lo stesso piano indipendentemente dal valore del parametro.
4.  Riscrivere la stored procedure in modo da usare l'opzione `(OPTIMIZE FOR UNKNOWN)`. Si otterrà lo stesso effetto dell'uso della tecnica della variabile locale.
5.  Riscrivere la query in modo da usare l'hint `DISABLE_PARAMETER_SNIFFING`. Si otterrà lo stesso effetto dell'uso della tecnica della variabile locale disabilitando totalmente l'analisi dei parametri, a meno che non vengano usate `OPTION(RECOMPILE)`, `WITH RECOMPILE` o `OPTIMIZE FOR <value>`.

> [!TIP] 
> Usare la funzionalità di analisi del piano di [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] per comprendere rapidamente se si tratta di un problema. Altre informazioni sono disponibili [qui](/archive/blogs/sql_server_team/new-in-ssms-query-performance-troubleshooting-made-easier).

## <a name="missing-indexes"></a><a name="MissingIndexes"></a> Indici mancanti

**Si applica a:** migrazione da piattaforma esterna, ad esempio Oracle, DB2, MySQL e Sybase, e da [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

Gli indici non corretti o mancanti causano un maggiore I/O che a sua volta determina un uso superiore della memoria e uno spreco di CPU. La causa del problema può essere la modifica del profilo del carico di lavoro, ad esempio l'uso di predicati diversi, che può avere invalidato la struttura degli indici esistente. Le prove di una strategia di indicizzazione inadeguata o di modifiche apportate al profilo del carico di lavoro includono:
-   Ricerca di indici duplicati, ridondati, raramente usati e completamente inutilizzati.
-   Particolare attenzione prestata a indici inutilizzati con aggiornamenti.

### <a name="steps-to-resolve"></a>Procedura di risoluzione

1.  Usare il piano di esecuzione grafico per i riferimenti a indici mancanti.
2.  Suggerimenti di indicizzazione generati da [Ottimizzazione guidata motore di database](../tools/dta/tutorial-database-engine-tuning-advisor.md).
3.  Usare [Missing Indexes DMV](../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md) (DMV degli indici mancanti) oppure [SQL Server Performance Dashboard](https://www.microsoft.com/download/details.aspx?id=29063).
4.  Usare script pre-esistenti in grado di usare DMV esistenti che offrano informazioni su indici mancanti, duplicati, ridondanti, raramente usati e completamente inutilizzati, ma che possano anche rivelare se i riferimenti agli indici includono hint o sono hardcoded in procedure e funzioni esistenti nel database. 

> [!TIP] 
> Esempi di questi script pre-esistenti includono [Index Creation](https://github.com/Microsoft/tigertoolbox/tree/master/Index-Creation) e [Index Information](https://github.com/Microsoft/tigertoolbox/tree/master/Index-Information). 

## <a name="inability-to-use-predicates-to-filter-data"></a><a name="InabilityPredicates"></a> Impossibilità di usare i predicati per filtrare i dati

**Si applica a:** migrazione da piattaforma esterna, ad esempio Oracle, DB2, MySQL e Sybase, e da [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

> [!NOTE]
> Per le migrazioni da [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], se questo problema si verificava nell'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] di origine, la semplice migrazione a una versione più recente di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] non risolverà il problema descritto in questo scenario.

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Query Optimizer si basa solo sulle informazioni note in fase di compilazione. Se un carico di lavoro si basa su predicati che è possibile conoscere solo in fase di esecuzione, le probabilità di scegliere un piano insoddisfacente aumentano. Per un piano di qualità migliore, i predicati devono essere **SARGable** o **S** earch **Arg** ument **able**.

Alcuni esempi di predicati non SARGable:
-   Conversioni di dati implicite, ad esempio da VARCHAR a NVARCHAR o da INT a VARCHAR. Cercare avvisi di runtime CONVERT_IMPLICIT nei piani di esecuzione effettivi. Anche la conversione da un tipo a un altro può causare una perdita di precisione.
-   Espressioni indeterminate complesse, ad esempio `WHERE UnitPrice + 1 < 3.975`, ma non `WHERE UnitPrice < 320 * 200 * 32`.
-   Espressioni che usano funzioni, ad esempio `WHERE ABS(ProductID) = 771` o `WHERE UPPER(LastName) = 'Smith'`
-   Stringhe con un carattere jolly iniziale, ad esempio `WHERE LastName LIKE '%Smith'`, ma non `WHERE LastName LIKE 'Smith%'`.

### <a name="steps-to-resolve"></a>Procedura di risoluzione

1. Dichiarare sempre variabili/parametri come [tipo di dati](../t-sql/data-types/data-types-transact-sql.md) di destinazione desiderato. 
  -   Ciò può richiedere il confronto del costrutto di codice definito dall'utente archiviato nel database, ad esempio stored procedure, funzioni o viste definite dall'utente, con le tabelle di sistema contenenti informazioni sui tipi di dati usati nelle tabelle sottostanti (ad esempio, [sys.columns](../relational-databases/system-catalog-views/sys-columns-transact-sql.md)).
2. Se non è possibile passare al punto precedente del codice, modificare il tipo di dati nella tabella in modo che corrisponda alla dichiarazione di variabile o parametro.
3. Riflettere sull'utilità dei costrutti seguenti:

  -   Funzioni usate come predicati
  -   Ricerche con caratteri jolly
  -   Espressioni complesse basate su dati a colonne: valutare la necessità di creare invece colonne calcolate persistenti che possono essere indicizzate.

> [!NOTE] 
> Tutte le operazioni indicate sopra possono essere eseguite a livello di codice.

## <a name="use-of-table-valued-functions-multi-statement-vs-inline"></a><a name="TableValuedFunctions"></a> Uso di funzioni con valori di tabella (con istruzioni multiple e inline)

**Si applica a:** migrazione da piattaforma esterna, ad esempio Oracle, DB2, MySQL e Sybase, e da [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

> [!NOTE]
> Per le migrazioni da [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], se questo problema si verificava nell'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] di origine, la semplice migrazione a una versione più recente di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] non risolverà il problema descritto in questo scenario.

Le funzioni con valori di tabella restituiscono un tipo di dati tabella che può costituire un'alternativa alle viste. Per le viste è possibile usare una sola istruzione `SELECT`, mentre le funzioni definite dall'utente possono contenere istruzioni aggiuntive che consentono una logica più efficace di quella consentita nelle viste.

> [!IMPORTANT] 
> Poiché la tabella di output di una funzione con valori di tabella con istruzioni multiple non viene creata in fase di compilazione, Query Optimizer di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] si basa sull'euristica e non su statistiche effettive per determinare le stime delle righe. L'aggiunta di indici alle tabelle di base non risolverà il problema. Per le funzioni con valori di tabella con istruzioni multiple, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] usa una stima fissa di 1 per il numero di righe che si prevede verrà restituito da una funzione con valori di tabella con istruzioni multiple (a partire da [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] questa stima fissa è di 100 righe).

### <a name="steps-to-resolve"></a>Procedura di risoluzione

1.  Se la funzione con valori di tabella con istruzioni multiple è un'istruzione singola, convertirla in funzione inline con valori di tabella.

    ```sql
    CREATE FUNCTION dbo.tfnGetRecentAddress(@ID int)
    RETURNS @tblAddress TABLE
    ([Address] VARCHAR(60) NOT NULL)
    AS
    BEGIN
      INSERT INTO @tblAddress ([Address])
      SELECT TOP 1 [AddressLine1]
      FROM [Person].[Address]
      WHERE  AddressID = @ID
      ORDER BY [ModifiedDate] DESC
    RETURN
    END
    ```

    L'esempio di formato inline viene visualizzato successivamente.

    ```sql
    CREATE FUNCTION dbo.tfnGetRecentAddress_inline(@ID int)
    RETURNS TABLE
    AS
    RETURN (
      SELECT TOP 1 [AddressLine1] AS [Address]
      FROM [Person].[Address]
      WHERE  AddressID = @ID
      ORDER BY [ModifiedDate] DESC
    )
    ```

2.  Se è più complessa, valutare l'uso dei risultati intermedi archiviati nelle tabelle ottimizzate per la memoria o nelle tabelle temporanee.

##  <a name="additional-reading"></a><a name="Additional_Reading"></a> Ulteriori informazioni

 [Procedure consigliate per l'archivio query](../relational-databases/performance/best-practice-with-the-query-store.md)  
[Tabelle ottimizzate per la memoria](./in-memory-oltp/sample-database-for-in-memory-oltp.md)  
[Funzioni definite dall'utente](../relational-databases/user-defined-functions/user-defined-functions.md)  
[Table Variables and Row Estimations - Part 1](/archive/blogs/blogdoezequiel/table-variables-and-row-estimations-part-1) (Variabili di tabella e stime delle righe: parte 1)  
[Table Variables and Row Estimations - Part 2](/archive/blogs/blogdoezequiel/table-variables-and-row-estimations-part-2) (Variabili di tabella e stime delle righe: parte 2)  
[Memorizzazione nella cache e riutilizzo del piano di esecuzione](../relational-databases/query-processing-architecture-guide.md#execution-plan-caching-and-reuse)