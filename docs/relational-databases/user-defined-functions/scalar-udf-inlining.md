---
title: Inlining di funzioni definite dall'utente scalari in Microsoft SQL Server | Microsoft Docs
description: Funzionalità di inlining di funzioni definite dall'utente scalari per migliorare le prestazioni delle query che richiamano funzioni definite dall'utente scalari in SQL Server (a partire da SQL Server 2019).
ms.custom: ''
ms.date: 08/04/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords: ''
ms.assetid: ''
author: s-r-k
ms.author: karam
monikerRange: = azuresqldb-current || >= sql-server-ver15
ms.openlocfilehash: 5e2dd566b0c5842636619c8331bfc88378dc4f84
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350867"
---
# <a name="scalar-udf-inlining"></a>Inlining di funzioni definite dall'utente scalari

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Questo articolo presenta l'inlining di funzioni definite dall'utente scalari, una delle funzionalità incluse nel gruppo di funzionalità di [elaborazione di query intelligenti](../../relational-databases/performance/intelligent-query-processing.md). Questa funzionalità migliora le prestazioni delle query che chiamano funzioni definite dall'utente scalari in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[sssql19](../../includes/sssql19-md.md)]).

## <a name="t-sql-scalar-user-defined-functions"></a>Funzioni definite dall'utente scalari T-SQL
Le funzioni definite dall'utente (UDF) implementate in [!INCLUDE[tsql](../../includes/tsql-md.md)] che restituiscono un unico valore di dati sono dette funzioni definite dall'utente scalari T-SQL. Le funzioni definite dall'utente T-SQL consentono di riusare e modulare il codice in più query [!INCLUDE[tsql](../../includes/tsql-md.md)] in modo elegante. Alcuni calcoli (ad esempio regole business complesse) sono più facili da esprimere nella forma imperativa delle funzioni definite dall'utente. Le funzioni definite dall'utente consentono di creare una logica complessa senza richiedere l'esperienza necessaria per la scrittura di query SQL complesse. Per altre informazioni sulle funzioni definite dall'utente, vedere [Creare funzioni definite dall'utente (motore di database)](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md).

## <a name="performance-of-scalar-udfs"></a>Prestazioni delle funzioni definite dall'utente scalari
Le funzioni definite dall'utente scalari offrono in genere prestazioni scarse per i motivi seguenti:

- **Chiamata iterativa:** Le funzioni definite dall'utente vengono chiamate in modo iterativo, una volta per ogni tupla idonea. Ciò comporta costi aggiuntivi a causa del cambio di contesto ripetuto dovuto alla chiamata di funzione. Questo aspetto interessa in modo particolarmente grave le funzioni definite dall'utente che eseguono query [!INCLUDE[tsql](../../includes/tsql-md.md)] all'interno della propria definizione.

- **Mancanza di determinazione costi:** Durante l'ottimizzazione vengono definiti i costi dei soli operatori relazionali, non degli operatori scalari. Prima dell'introduzione delle funzioni definite dall'utente scalari, i costi degli altri operatori scalari erano in genere bassi e non richiedevano una determinazione costi. Per un'operazione scalare era sufficiente aggiungere un costo ridotto per la CPU. In alcuni scenari, il costo effettivo è significativo ma rimane comunque sottorappresentato.

- **Esecuzione interpretata:** Le funzioni definite dall'utente vengono valutate come batch di istruzioni e vengono eseguite istruzione per istruzione. Ogni istruzione viene compilata e il piano compilato viene memorizzato nella cache. Questa strategia di memorizzazione nella cache consente di risparmiare tempo perché consente di evitare le ricompilazioni, ma ogni istruzione viene eseguita in modo isolato. Non vengono eseguite ottimizzazioni tra istruzioni diverse.

- **Esecuzione seriale:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non consente il parallelismo interno alla query nelle query che richiamano funzioni definite dall'utente. 

## <a name="automatic-inlining-of-scalar-udfs"></a>Inlining automatico di funzioni definite dall'utente scalari
L'obiettivo della funzionalità di inlining delle funzioni definite dall'utente scalari è di migliorare le prestazioni delle query che chiamano funzioni definite dall'utente scalari T-SQL, in cui il collo di bottiglia principale è costituito dall'esecuzione di funzioni definite dall'utente.

Con questa nuova funzionalità, le funzioni definite dall'utente scalari vengono trasformate automaticamente in espressioni scalari o sottoquery scalari che sostituiscono l'operatore della funzione definita dall'utente nella query chiamante. Queste espressioni e sottoquery vengono quindi ottimizzate. Il risultato di questa operazione è che il piano di query non ha più un operatore per la funzione definita dall'utente, ma gli effetti di questo, ad esempio viste o funzioni con valori di tabella inline, sono riscontrabili nel piano.

### <a name="example-1---single-statement-scalar-udf"></a>Esempio 1: funzione definita dall'utente scalare a istruzione singola
Si consideri la query seguente.

```sql
SELECT L_SHIPDATE, O_SHIPPRIORITY, SUM (L_EXTENDEDPRICE *(1 - L_DISCOUNT)) 
FROM LINEITEM
INNER JOIN ORDERS
  ON O_ORDERKEY = L_ORDERKEY 
GROUP BY L_SHIPDATE, O_SHIPPRIORITY ORDER BY L_SHIPDATE;
```

Questa query calcola la somma dei prezzi scontati per le linee e presenta i risultati raggruppati per data e priorità di spedizione. L'espressione `L_EXTENDEDPRICE *(1 - L_DISCOUNT)` è la formula del prezzo scontato per una linea specifica. Tali formule possono essere estratte in funzioni a scopo di modularità e riutilizzo.

```sql
CREATE FUNCTION dbo.discount_price(@price DECIMAL(12,2), @discount DECIMAL(12,2)) 
RETURNS DECIMAL (12,2) AS
BEGIN
  RETURN @price * (1 - @discount);
END
```

È ora possibile modificare la query per chiamare questa funzione definita dall'utente.

```sql
SELECT L_SHIPDATE, O_SHIPPRIORITY, SUM (dbo.discount_price(L_EXTENDEDPRICE, L_DISCOUNT)) 
FROM LINEITEM
INNER JOIN ORDERS
  ON O_ORDERKEY = L_ORDERKEY 
GROUP BY L_SHIPDATE, O_SHIPPRIORITY ORDER BY L_SHIPDATE
```

Per i motivi descritti in precedenza, la query con la funzione definita dall'utente offre prestazioni scarse. Ora, con l'inlining della funzione definita dall'utente scalare, l'espressione scalare nel corpo della funzione definita dall'utente viene sostituita direttamente all'interno della query. I risultati dell'esecuzione di questa query sono illustrati nella tabella seguente:

| Query: | Query senza funzione definita dall'utente | Query con funzione definita dall'utente (senza inlining) | Query con inlining della funzione definita dall'utente scalare | 
| --- | --- | --- | --- |
| Tempo di esecuzione: | 1,6 secondi | 29 minuti 11 secondi | 1,6 secondi |

Queste cifre si basano su un database CCI di 10 GB (con schema TPC-H) in esecuzione in un computer a doppio processore (12 core), 96 GB di RAM e unità SSD. Le cifre includono il tempo di compilazione ed esecuzione con cache di routine a freddo e pool di buffer. È stata usata la configurazione predefinita e non sono stati creati altri indici.

### <a name="example-2---multi-statement-scalar-udf"></a>Esempio 2: funzione definita dall'utente scalare a più istruzioni
È possibile eseguire l'inlining anche delle funzioni definite dall'utente scalari implementate con più istruzioni T-SQL, ad esempio assegnazioni di variabili e diramazioni condizionali. Si consideri la funzione definita dall'utente scalare seguente che, data una chiave cliente, determina la categoria di servizio per tale cliente. Per arrivare alla categoria, prima calcola il prezzo totale di tutti gli ordini effettuati dal cliente tramite una query SQL. Usa quindi la logica `IF (...) ELSE` per stabilire la categoria in base al prezzo totale.

```sql
CREATE OR ALTER FUNCTION dbo.customer_category(@ckey INT) 
RETURNS CHAR(10) AS
BEGIN
  DECLARE @total_price DECIMAL(18,2);
  DECLARE @category CHAR(10);

  SELECT @total_price = SUM(O_TOTALPRICE) FROM ORDERS WHERE O_CUSTKEY = @ckey;

  IF @total_price < 500000
    SET @category = 'REGULAR';
  ELSE IF @total_price < 1000000
    SET @category = 'GOLD';
  ELSE 
    SET @category = 'PLATINUM';

  RETURN @category;
END
```

Si consideri ora una query che chiami questa funzione definita dall'utente.

```sql
SELECT C_NAME, dbo.customer_category(C_CUSTKEY) FROM CUSTOMER;
```

Il piano di esecuzione per questa query in [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] (livello di compatibilità 140 e versioni precedenti) è il seguente:

![Piano di query senza inlining](./media/query-plan-without-udf-inlining.png)

Come illustrato dal piano, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] adotta una strategia semplice: per ogni tupla nella tabella `CUSTOMER` richiama la funzione definita dall'utente e genera l'output dei risultati. Questa strategia è semplicistica e inefficiente. Con l'inlining, una funzione definita dall'utente di questo tipo viene trasformata in una sottoquery scalare equivalente, che viene inserita nella query chiamante al posto della funzione definita dall'utente.

Per la stessa query, il piano con l'inlining della funzione definita dall'utente ha l'aspetto seguente.

![Piano di query con inlining](./media/query-plan-with-udf-inlining.png)

Come detto in precedenza, il piano di query non ha più un operatore per la funzione definita dall'utente, ma gli effetti di questo, ad esempio viste o funzioni con valori di tabella inline, sono osservabili nel piano. Ecco alcune osservazioni chiave per il piano precedente:

-  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha dedotto il join implicito tra `CUSTOMER` e `ORDERS`, rendendolo esplicito tramite un operatore di join.
-  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha anche dedotto la clausola `GROUP BY O_CUSTKEY on ORDERS` implicita e ha usato IndexSpool + StreamAggregate per implementarla.
-  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa ora il parallelismo tra tutti gli operatori.

A seconda della complessità della logica della funzione definita dall'utente, il piano di query generato risultante può essere anche più grande e più complesso. Come si può vedere, le operazioni all'interno della funzione definita dall'utente non sono più una black box. Query Optimizer è quindi in grado di determinare i costi di queste operazioni e di ottimizzarle. Poiché, poi, la funzione definita dall'utente non è più all'interno del piano, la chiamata iterativa a tale funzione viene sostituita da un piano che evita completamente il sovraccarico delle chiamate di funzione.

## <a name="inlineable-scalar-udfs-requirements"></a>Requisiti delle funzioni definite dall'utente scalari abilitate per l'inlining
<a name="requirements"></a> È possibile eseguire l'inlining di una funzione definita dall'utente scalare Transact-SQL se si verificano tutte le condizioni seguenti:

- La funzione definita dall'utente è scritta con i costrutti seguenti:
    - `DECLARE`, `SET`: Dichiarazione e assegnazione di variabili.
    - `SELECT`: Query SQL con assegnazioni di variabili singole/multiple <sup>1</sup>.
    - `IF`/`ELSE`: Diramazione con livelli di annidamento arbitrari.
    - `RETURN`: Istruzione return singola o istruzioni return multiple.
    - `UDF`: Chiamate di funzioni ricorsive/annidate <sup>2</sup>.
    - Altri: Operazioni relazionali, ad esempio `EXISTS`, `ISNULL`.
- La funzione definita dall'utente non chiama alcuna funzione intrinseca dipendente dal tempo (ad esempio `GETDATE()`) o con effetti collaterali <sup>3</sup> (ad esempio `NEWSEQUENTIALID()`).
- La funzione definita dall'utente usa la clausola `EXECUTE AS CALLER` (comportamento predefinito se la clausola `EXECUTE AS` non viene specificata).
- La funzione definita dall'utente non fa riferimento a variabili di tabella o a parametri con valori di tabella.
- La query che chiama una funzione definita dall'utente scalare non fa riferimento a una chiamata di funzione definita dall'utente scalare nella relativa clausola `GROUP BY`.
- La query che richiama una funzione definita dall'utente scalare nel relativo elenco di selezione con clausola `DISTINCT` non include la clausola `ORDER BY`.
- La funzione definita dall'utente non viene usata nella clausola `ORDER BY`.
- La funzione definita dall'utente non è compilata in modo nativo (interoperabilità supportata).
- La funzione definita dall'utente non viene usata in una colonna calcolata o in una definizione di vincolo di controllo.
- La funzione definita dall'utente non fa riferimento a tipi definiti dall'utente.
- Non sono state aggiunte firme alla funzione definita dall'utente.
- La funzione definita dall'utente non è una funzione di partizione.
- La funzione definita dall'utente non contiene riferimenti a espressioni di tabella comuni (CTE).
- La funzione definita dall'utente non contiene riferimenti a funzioni intrinseche che potrebbero alterare i risultati quando sono inline (ad esempio, `@@ROWCOUNT`) <sup>4</sup>.
- La funzione definita dall'utente non contiene funzioni di aggregazione passate come parametri a una funzione definita dall'utente scalare <sup>4</sup>.
- La funzione definita dall'utente non fa riferimento a visualizzazioni predefinite (ad esempio, `OBJECT_ID`) <sup>4</sup>.
- La funzione definita dall'utente non fa riferimento ai metodi XML <sup>5</sup>.
- La funzione definita dall'utente non contiene una query SELECT con `ORDER BY` senza una clausola `TOP 1` <sup>5</sup>.
- La funzione definita dall'utente non contiene una query SELECT che esegue un'assegnazione in combinazione con la clausola `ORDER BY` (ad esempio, `SELECT @x = @x + 1 FROM table1 ORDER BY col1`) <sup>5</sup>.
- La funzione definita dall'utente non contiene più istruzioni RETURN <sup>6</sup>.
- La funzione definita dall'utente non viene chiamata da un'istruzione RETURN <sup>6</sup>.
- La funzione definita dall'utente non fa riferimento alla funzione `STRING_AGG` <sup>6</sup>. 
- La funzione definita dall'utente non fa riferimento a tabelle remote <sup>7</sup>.
- La query che chiama la funzione definita dall'utente non usa `GROUPING SETS`, `CUBE`o `ROLLUP` <sup>7</sup>.
- La query che chiama la funzione definita dall'utente non contiene una variabile usata come parametro UDF per l'assegnazione (ad esempio, `SELECT @y = 2`, `@x = UDF(@y)`)<sup>7</sup>.

<sup>1</sup> `SELECT` con accumulo/aggregazione di variabili non è supportata per l'inlining (ad esempio, `SELECT @val += col1 FROM table1`).

<sup>2</sup> L'inlining delle funzioni definite dall'utente ricorsive viene eseguito solo fino a una determinata profondità.

<sup>3</sup> Le funzioni intrinseche i cui risultati dipendono dall'ora di sistema corrente sono dipendenti dall'ora. Un esempio di funzione con effetti collaterali può essere costituito da una funzione intrinseca in grado di aggiornare uno stato globale interno. Tali funzioni restituiscono risultati diversi ogni volta che vengono chiamate, a seconda dello stato interno.

<sup>4</sup> Restrizione aggiunta in [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CU2

<sup>5</sup> Restrizione aggiunta in [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CU4

<sup>6</sup> Restrizione aggiunta in [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CU5

<sup>7</sup> Restrizione aggiunta in [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CU6

> [!NOTE]
> Per informazioni sulle correzioni e sulle modifiche più recenti dell'inlining di funzioni definite dall'utente scalari di T-SQL in scenari di idoneità all'inlining, vedere l'articolo della Knowledge Base: [CORREZIONE: Problemi relativi all'inlining di funzioni definite dall'utente scalari in SQL Server 2019](https://support.microsoft.com/help/4538581).

### <a name="checking-whether-or-not-a-udf-can-be-inlined"></a>Verifica dell'idoneità all'inlining di una funzione definita dall'utente
Per ogni funzione definita dall'utente scalare T-SQL, la vista del catalogo [Sys. sql_modules](../system-catalog-views/sys-sql-modules-transact-sql.md) include la proprietà `is_inlineable`, che indica se una funzione definita dall'utente è idonea all'inlining o meno. 

> [!NOTE]
> La proprietà `is_inlineable` è derivata dai costrutti presenti nella definizione della funzione definita dall'utente. Non controlla se la funzione definita dall'utente supporta effettivamente l'inlining in fase di compilazione. Per altre informazioni, vedere le [condizioni per l'inlining](#requirements).

Il valore 1 indica che è idonea, mentre 0 indica il contrario. Questa proprietà ha un valore pari a 1 anche per tutte le funzioni con valori di tabella inline. Per tutti gli altri moduli, il valore è 0.

Se una funzione definita dall'utente scalare è idonea all'inlining, non significa che ne verrà sempre eseguito l'inlining. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] decide (per ogni query e per ogni funzione definita dall'utente) se eseguire o meno l'inlining di una funzione definita dall'utente. Alcuni esempi di quando non è possibile eseguire l'inlining di una funzione definita dall'utente includono:

-  Se la definizione di una funzione definita dall'utente viene eseguita in migliaia di righe di codice, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può scegliere di non eseguire l'inlining. 
-  Non verrà eseguito l'inlining di una chiamata UDF in una clausola `GROUP BY`. Questa decisione viene presa quando viene compilata la query che fa riferimento a una funzione definita dall'utente scalare.
-  Se la funzione definita dall'utente è firmata con un certificato. Poiché le firme possono essere aggiunte ed eliminate dopo la creazione di una funzione definita dall'utente, è necessario decidere se eseguire o meno l'inlining quando viene compilata la query che fa riferimento a una funzione definita dall'utente scalare. Le funzioni di sistema, ad esempio, vengono in genere firmate con un certificato. È possibile usare [sys.crypt_properties](../../relational-databases/system-catalog-views/sys-crypt-properties-transact-sql.md) per individuare gli oggetti firmati. 

   ```sql
   SELECT * 
   FROM sys.crypt_properties AS cp
   INNER JOIN sys.objects AS o ON cp.major_id = o.object_id;
   ```

### <a name="checking-whether-inlining-has-happened-or-not"></a>Verifica dell'effettiva esecuzione dell'inlining
Se vengono soddisfatte tutte le condizioni preliminari e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] decide di eseguire l'inlining, la funzione definita dall'utente viene trasformata in un'espressione relazionale. Dal piano di query, è facile capire se l'inlining è stato eseguito o meno:

- Per una funzione definita dall'utente in cui l'inlining è stato eseguito, il codice xml del piano non contiene un nodo xml `<UserDefinedFunction>`. 
- Vengono generati XEvents specifici.

## <a name="enabling-scalar-udf-inlining"></a>Abilitazione dell'inlining di funzioni definite dall'utente scalari
È possibile impostare automaticamente i carichi di lavoro come idonei all'inlining di funzioni definite dall'utente scalari abilitando il livello di compatibilità 150 per il database. Questa opzione è impostabile con [!INCLUDE[tsql](../../includes/tsql-md.md)]. Ad esempio:  

```sql
ALTER DATABASE [WideWorldImportersDW] SET COMPATIBILITY_LEVEL = 150;
```

A parte questo, per sfruttare i vantaggi di questa funzionalità non è necessario apportare altri cambiamenti alle funzioni definite dall'utente o alle query.

## <a name="disabling-scalar-udf-inlining-without-changing-the-compatibility-level"></a>Disabilitazione dell'inlining di funzioni definite dall'utente scalari senza modificare il livello di compatibilità
È possibile disabilitare l'inlining di funzioni definite dall'utente scalari nell'ambito del database, dell'istruzione o della funzione definita dall'utente mantenendo comunque il livello di compatibilità del database 150 o superiore. Per disabilitare l'inlining di funzioni definite dall'utente scalari nell'ambito del database, eseguire l'istruzione seguente all'interno del contesto del database applicabile: 

```sql
ALTER DATABASE SCOPED CONFIGURATION SET TSQL_SCALAR_UDF_INLINING = OFF;
```

Per riabilitare l'inlining di funzioni definite dall'utente scalari per il database, eseguire l'istruzione seguente all'interno del contesto del database applicabile:

```sql
ALTER DATABASE SCOPED CONFIGURATION SET TSQL_SCALAR_UDF_INLINING = ON;
```

Se attiva, questa impostazione viene visualizzata come abilitata in [`sys.database_scoped_configurations`](../system-catalog-views/sys-database-scoped-configurations-transact-sql.md). È anche possibile disabilitare l'inlining di funzioni definite dall'utente scalari per una query specifica designando `DISABLE_TSQL_SCALAR_UDF_INLINING` come hint per la query `USE HINT`. Ad esempio:

```sql
SELECT L_SHIPDATE, O_SHIPPRIORITY, SUM (dbo.discount_price(L_EXTENDEDPRICE, L_DISCOUNT)) 
FROM LINEITEM
INNER JOIN ORDERS
  ON O_ORDERKEY = L_ORDERKEY 
GROUP BY L_SHIPDATE, O_SHIPPRIORITY ORDER BY L_SHIPDATE
OPTION (USE HINT('DISABLE_TSQL_SCALAR_UDF_INLINING'));
```

> [!TIP]
> Un hint per la query `USE HINT` ha la precedenza sulla configurazione con ambito database o sull'impostazione del livello di compatibilità.

È anche possibile disabilitare l'inlining di funzioni definite dall'utente scalari per una funzione definita dall'utente specifica tramite la clausola INLINE nell'istruzione `CREATE FUNCTION` o `ALTER FUNCTION`.
Ad esempio:

```sql
CREATE OR ALTER FUNCTION dbo.discount_price(@price DECIMAL(12,2), @discount DECIMAL(12,2))
RETURNS DECIMAL (12,2)
WITH INLINE = OFF
AS
BEGIN
    RETURN @price * (1 - @discount);
END;
```

Dopo l'esecuzione dell'istruzione precedente, non verrà mai eseguito l'inlining di questa funzione definita dall'utente in alcuna delle query che la chiameranno. Per riabilitare l'inlining per questa funzione definita dall'utente, eseguire l'istruzione seguente:

```sql
CREATE OR ALTER FUNCTION dbo.discount_price(@price DECIMAL(12,2), @discount DECIMAL(12,2))
RETURNS DECIMAL (12,2)
WITH INLINE = ON
AS
BEGIN
    RETURN @price * (1 - @discount);
END
```

> [!NOTE]
> La clausola `INLINE` non è obbligatoria. Se la clausola `INLINE` viene omessa, viene automaticamente impostata su `ON`/`OFF` in base al fatto che possa essere eseguito l'inline della funzione. Se viene specificato `INLINE = ON`, ma la funzione definita dall'utente non è idonea per l'inlining, viene generato un errore.

## <a name="important-notes"></a>Note importanti
Come descritto in questo articolo, l'inlining di una funzione definita dall'utente scalare trasforma una query con funzioni definite dall'utente scalari in una query con una sottoquery scalare equivalente. A causa di questa trasformazione, si possono notare alcune differenze di comportamento negli scenari seguenti:

1. L'inlining ha come risultato un hash di query diverso per lo stesso testo della query.
1. Alcuni avvisi nelle istruzioni all'interno della funzione definita dall'utente (ad esempio divisione per zero e così via), che in precedenza erano nascosti, possono essere visualizzati a causa dell'inlining.
1. Gli hint di join a livello di query possono non essere più validi, poiché l'inlining può introdurre nuovi join. È necessario usare hint di join locale.
1. Non è possibile indicizzare le viste che fanno riferimento a funzioni definite dall'utente scalari. Se è necessario creare un indice per tali viste, disabilitare l'inlining per le funzioni definite dall'utente interessate.
1. Con l'inlining di funzioni definite dall'utente possono presentarsi alcune differenze nel comportamento del [Dynamic Data Masking](../security/dynamic-data-masking.md). In determinate situazioni (a seconda della logica della funzione definita dall'utente), l'inlining può essere più conservativo rispetto alla maschera delle colonne di output. Negli scenari in cui le colonne a cui si fa riferimento in una funzione definita dall'utente non sono colonne di output, queste non vengono mascherate. 
1. Se una funzione definita dall'utente fa riferimento a funzioni predefinite, ad esempio `SCOPE_IDENTITY()`, `@@ROWCOUNT` o `@@ERROR`, il valore restituito dalla funzione predefinita cambierà con l'inlining. Questa modifica nel comportamento è dovuta al fatto che l'inlining modifica l'ambito delle istruzioni all'interno della funzione definita dall'utente. A partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CU2, l'inlining è bloccato se la funzione definita dall'utente fa riferimento a determinate funzioni intrinseche (ad esempio, `@@ROWCOUNT`).

## <a name="see-also"></a>Vedere anche
[Creare funzioni definite dall'utente (motore di database)](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md)   
[Centro prestazioni per il motore di database di SQL Server e il database SQL di Azure](../../relational-databases/performance/performance-center-for-sql-server-database-engine-and-azure-sql-database.md)     
[Guida sull'architettura di elaborazione delle query](../../relational-databases/query-processing-architecture-guide.md)     
[Guida di riferimento a operatori Showplan logici e fisici](../../relational-databases/showplan-logical-and-physical-operators-reference.md)     
[Join](../../relational-databases/performance/joins.md)     
[Dimostrazione dell'elaborazione di query intelligenti](https://aka.ms/IQPDemos)     
[CORREZIONE: Problemi relativi all'inlining di funzioni definite dall'utente scalari in SQL Server 2019](https://support.microsoft.com/help/4538581)     

