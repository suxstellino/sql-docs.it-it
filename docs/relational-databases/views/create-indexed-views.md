---
description: Creazione di viste indicizzate
title: Creare viste indicizzate | Microsoft Docs
ms.custom: ''
ms.date: 11/19/2018
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- indexed views [SQL Server], creating
- clustered indexes, views
- CREATE INDEX statement
- large_value_types_out_of_row option
- indexed views [SQL Server]
- views [SQL Server], indexed views
ms.assetid: f86dd29f-52dd-44a9-91ac-1eb305c1ca8d
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 148f93b43f704686b3083954cb3d7353f33a16e0
ms.sourcegitcommit: c6cc0b669b175ae290cf5b08952010661ebd03c3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/16/2021
ms.locfileid: "100530862"
---
# <a name="create-indexed-views"></a>Creazione di viste indicizzate

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Questo articolo descrive come creare indici in una vista. Il primo indice creato per una vista deve essere un indice cluster univoco. Dopo aver creato l'indice cluster univoco, è possibile creare più indici non cluster. La creazione di un indice cluster univoco per una vista consente un miglioramento delle prestazioni delle query, in quanto la vista viene archiviata nel database in modo analogo a una tabella con un indice cluster. Le viste indicizzate possono essere usate da Query Optimizer per velocizzare l'esecuzione delle query. Non è necessario fare riferimento alla vista nella query affinché venga usata da Query Optimizer per una sostituzione.

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare

Per la creazione e la corretta implementazione di una vista indicizzata, è fondamentale effettuare le operazioni seguenti:

1. Verificare che le opzioni SET siano corrette per tutte le tabelle esistenti a cui verrà fatto riferimento nella vista.
2. Verificare che le opzioni SET della sessione siano impostate in modo corretto prima di creare qualsiasi tabella e la vista.
3. Verificare che la definizione della vista sia deterministica.
4. Verificare che la tabella di base abbia lo stesso proprietario della vista.
5. Creare la vista usando l'opzione `WITH SCHEMABINDING`.
6. Creare l'indice cluster univoco per la vista.

> [!IMPORTANT]
> Durante l'esecuzione di DML<sup>1</sup> in una tabella cui viene fatto riferimento da un numero elevato di viste indicizzate o da un numero minore di viste indicizzate molto complesse, è necessario aggiornare anche le viste indicizzate cui viene fatto riferimento. Di conseguenza, è possibile che le prestazioni delle query DML si riducano notevolmente o, in alcuni casi, non venga prodotto un piano di query.
> In questi scenari, testare le query DML prima dell'uso in produzione, analizzare il piano di query e ottimizzare/semplificare l'istruzione DML.
>
> <sup>1</sup> Ad esempio, operazioni UPDATE, DELETE o INSERT.

### <a name="required-set-options-for-indexed-views"></a><a name="Restrictions"></a> Opzioni SET necessarie per le viste indicizzate

La valutazione di una stessa espressione può produrre risultati diversi nel [!INCLUDE[ssDE](../../includes/ssde-md.md)] se sono attive diverse opzioni SET quando la query viene eseguita. Ad esempio, dopo aver impostato l'opzione SET `CONCAT_NULL_YIELDS_NULL` su ON, l'espressione `'abc' + NULL` restituisce il valore `NULL`. Tuttavia, dopo aver impostato `CONCAT_NULL_YIELDS_NULL` su OFF, la stessa espressione produce `'abc'`.

Per essere certi che le viste possano essere gestite in modo corretto e restituiscano risultati coerenti, è necessario usare valori fissi per varie opzioni SET delle viste indicizzate. Le opzioni SET specificate nella tabella seguente devono essere impostate sui valori indicati nella colonna **Valore obbligatorio** quando si verificano le seguenti condizioni:

- Vengono creati la vista e gli indici successivi nella vista.
- Le tabelle di base a cui si fa riferimento nella vista quando viene creata la vista stessa.
- Quando viene eseguita un'operazione di inserimento, aggiornamento o eliminazione su una qualsiasi tabella usata nella vista indicizzata, incluse operazioni quali la copia bulk, la replica e le query distribuite.
- Quando la vista indicizzata viene usata in Query Optimizer per generare il piano di query.

|Opzioni SET|Valore richiesto|Valore server predefinito|Predefinito<br /><br /> OLE DB e ODBC predefinito|Predefinito<br /><br /> DB-Library predefinito|
|-----------------|--------------------|--------------------------|---------------------------------------|-----------------------------------|
|ANSI_NULLS|ON|ON|ON|OFF|
|ANSI_PADDING|ON|ON|ON|OFF|
|ANSI_WARNINGS<sup>1</sup>|ON|ON|ON|OFF|
|ARITHABORT|ON|ON|OFF|OFF|
|CONCAT_NULL_YIELDS_NULL|ON|ON|ON|OFF|
|NUMERIC_ROUNDABORT|OFF|OFF|OFF|OFF|
|QUOTED_IDENTIFIER|ON|ON|ON|OFF|
|&nbsp;|&nbsp;|&nbsp;|&nbsp;|&nbsp;|

<sup>1</sup> L'impostazione di `ANSI_WARNINGS` su ON imposta in modo implicito `ARITHABORT` su ON.

Se si usa una connessione server OLE DB o ODBC, l'unico valore da modificare è l'impostazione `ARITHABORT`. Tutti i valori DB-Library devono essere impostati in modo corretto a livello di server tramite **sp_configure** oppure dall'applicazione tramite il comando SET.

> [!IMPORTANT]
> È consigliabile impostare l'opzione utente `ARITHABORT` su ON per l'intero server immediatamente dopo la creazione della prima vista indicizzata o del primo indice in una colonna calcolata in qualsiasi database del server.

### <a name="deterministic-views"></a>Viste deterministiche

La definizione di una vista indicizzata deve essere deterministica. Una vista è deterministica se tutte le espressioni nell'elenco di selezione, nonché nelle clausole `WHERE` e `GROUP BY`, sono deterministiche. Le espressioni deterministiche restituiscono sempre lo stesso risultato ogni volta che vengono valutate con un set specifico di valori di input. Nelle espressioni deterministiche è possibile usare solo funzioni deterministiche. La funzione `DATEADD`, ad esempio, è deterministica perché restituisce sempre lo stesso risultato per un dato set di valori dei relativi tre parametri. `GETDATE` non è deterministica perché viene sempre richiamata con lo stesso argomento, ma il valore restituito cambia ogni volta che viene eseguita.

Per determinare se una colonna della vista è deterministica, usare la proprietà **IsDeterministic** della funzione [COLUMNPROPERTY](../../t-sql/functions/columnproperty-transact-sql.md) . Usare la proprietà **IsPrecise** della funzione `COLUMNPROPERTY` per determinare se una colonna deterministica in una vista con associazione di schema è precisa. `COLUMNPROPERTY` restituisce 1 se TRUE, 0 se FALSE e NULL se il valore di input non è valido. Questo significa che la colonna non è deterministica o non è precisa.

Se in un'espressione deterministica sono contenute espressioni float, il risultato esatto può dipendere dall'architettura del processore o dalla versione del microcodice. Per garantire l'integrità dei dati, le espressioni di questo tipo possono essere usate solo come colonne non chiave delle viste indicizzate. Le espressioni deterministiche che non contengono espressioni float sono definite precise. Nelle colonne chiave e nelle clausole `WHERE` o `GROUP BY` delle viste indicizzate è possibile usare solo espressioni deterministiche precise.

### <a name="additional-requirements"></a>Requisiti aggiuntivi

Oltre alle impostazioni delle opzioni SET e ai requisiti relativi alle funzioni deterministiche, è necessario che vengano soddisfatti i requisiti seguenti:

- L'utente che esegue `CREATE INDEX` deve essere il proprietario della vista.
- Quando si crea l'indice, l'opzione `IGNORE_DUP_KEY` deve essere impostata su OFF (impostazione predefinita).
- I riferimenti alle tabelle devono essere specificati come nomi composti da due parti, ovvero _schema_ **.** _nometabella_ , nella definizione della vista.
- Le funzioni definite dall'utente a cui viene fatto riferimento nella vista devono essere create usando l'opzione `WITH SCHEMABINDING`.
- A qualsiasi funzione definita dall'utente a cui si fa riferimento nella vista deve essere fatto riferimento usando nomi in due parti, _\<schema\>_ **.** _\<function\>_ .
- La proprietà di accesso ai dati di una funzione definita dall'utente deve essere `NO SQL` e la proprietà di accesso esterno deve essere `NO`.
- Le funzioni CLR (Common Language Runtime) possono essere incluse solo nell'elenco SELECT della vista ma non possono fare parte della definizione della chiave di indice cluster. Le funzioni CLR non possono essere incluse nella clausola WHERE della vista o nella clausola ON di un'operazione JOIN nella vista.
- Le proprietà delle funzioni CLR e dei metodi di tipi CLR definiti dall'utente usati nella definizione della vista devono essere impostate come illustrato nella tabella seguente.

   |Proprietà|Note|
   |--------------|----------|
   |DETERMINISTIC = TRUE|Deve essere dichiarata in modo esplicito come attributo del metodo di Microsoft .NET Framework.|
   |PRECISE = TRUE|Deve essere dichiarata in modo esplicito come attributo del metodo di .NET Framework.|
   |DATA ACCESS = NO SQL|Determinata dall'impostazione dell'attributo DataAccess su DataAccessKind.None e dell'attributo SystemDataAccess su SystemDataAccessKind.None.|
   |EXTERNAL ACCESS = NO|Per le routine CLR il valore predefinito di questa proprietà è NO.|
   |&nbsp;|&nbsp;|

- La vista deve essere creata usando l'opzione `WITH SCHEMABINDING`.
- La vista deve contenere riferimenti solo a tabelle di base che si trovano nello stesso database della vista. La vista non può fare riferimento ad altre viste.

- Se è presente `GROUP BY`, la definizione di VIEW deve contenere `COUNT_BIG(*)` e non deve contenere `HAVING`. Queste restrizioni di `GROUP BY` vengono applicate solo alla definizione della vista indicizzata. Una query può usare una vista indicizzata nel relativo piano di esecuzione anche se non soddisfa le restrizioni di `GROUP BY`.
- Se la definizione della vista include una clausola `GROUP BY`, la chiave dell'indice cluster univoco può contenere riferimenti solo alle colonne specificate nella clausola `GROUP BY`.

- Nell'istruzione SELECT della definizione della vista non possono essere contenuti gli elementi Transact-SQL seguenti:

   | Elementi di Transact-SQL | (continua) | (continua) |
   | --------------------- | ----------- | ----------- |
   |`COUNT`|Funzioni ROWSET (`OPENDATASOURCE`, `OPENQUERY`, `OPENROWSET` e `OPENXML`)|Join `OUTER` (`LEFT`, `RIGHT` o `FULL`)|
   |Tabella derivata (definita specificando un'istruzione `SELECT` nella clausola `FROM`)|Self-join|Specifica di colonne tramite `SELECT *` o `SELECT <table_name>.*`|
   |`DISTINCT`|`STDEV`, `STDEVP`, `VAR`, `VARP` o `AVG`|Espressione di tabella comune (CTE)|
   |Colonne **float**<sup>1</sup>, **text**, **ntext**, **image**, **XML** o **filestream**|Sottoquery|Clausola `OVER`, che include funzioni di rango o funzioni finestra di aggregazione|
   |Predicati full-text (`CONTAINS`, `FREETEXT`)|Funzione `SUM` che fa riferimento a un'espressione che ammette i valori Null|`ORDER BY`|
   |Funzione di aggregazione CLR definita dall'utente|`TOP`|Operatori `CUBE`, `ROLLUP` o `GROUPING SETS`|
   |`MIN`, `MAX`|Operatori `UNION`, `EXCEPT` o `INTERSECT`|`TABLESAMPLE`|
   |Variabili di tabella|`OUTER APPLY` o `CROSS APPLY`|`PIVOT`, `UNPIVOT`|
   |Set di colonne di tipo sparse|Funzione con valori di tabella inline o con istruzioni multiple|`OFFSET`|
   |`CHECKSUM_AGG`|||

   <sup>1</sup> La vista indicizzata può contenere colonne di tipo **float** che, tuttavia, non possono essere incluse nella chiave di indice cluster.

   > [!IMPORTANT]
   > Le viste indicizzate non sono supportate sulle query temporali, ovvero quelle che usano la clausola `FOR SYSTEM_TIME`.

### <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni

Quando si fa riferimento a valori letterali stringa **datetime** e **smalldatetime** nelle viste indicizzate, è consigliabile convertire in modo esplicito il valore letterale nel tipo di dati desiderato usando uno stile del formato di data deterministico. Per un elenco degli stili del formato di data deterministici, vedere [CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md). Per altre informazioni sulle espressioni deterministiche e non deterministiche, vedere la sezione [Considerazioni](#nondeterministic) in questa pagina.

Durante l'esecuzione di DML, ad esempio di `UPDATE`, `DELETE` o `INSERT`, in una tabella cui viene fatto riferimento da un numero elevato di viste indicizzate o da un numero minore di viste indicizzate molto complesse, è necessario aggiornare anche le viste indicizzate. Di conseguenza, è possibile che le prestazioni delle query DML si riducano notevolmente o, in alcuni casi, non venga prodotto un piano di query. In questi scenari, testare le query DML prima dell'uso in produzione, analizzare il piano di query e ottimizzare/semplificare l'istruzione DML.

### <a name="considerations"></a><a name="Considerations"></a> Considerazioni

L'impostazione dell'opzione **large_value_types_out_of_row** delle colonne di una vista indicizzata è ereditata dall'impostazione della colonna corrispondente nella tabella di base. Questo valore viene impostato mediante [sp_tableoption](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md). L'impostazione predefinita per le colonne generate da espressioni è 0. Ciò significa che i tipi per valori di grandi dimensioni vengono archiviati all'interno delle righe.

È possibile creare viste indicizzate per una tabella partizionata, nonché partizionare questo tipo di viste.

Per impedire l'utilizzo di viste indicizzate in [!INCLUDE[ssDE](../../includes/ssde-md.md)], includere l'hint `OPTION (EXPAND VIEWS)` nella query. Inoltre, l'errata impostazione di una qualsiasi delle opzione elencate impedisce l'utilizzo degli indici delle viste in Query Optimizer. Per altre informazioni sull'hint `OPTION (EXPAND VIEWS)`, vedere [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md).

Tutti gli indici di una vista vengono eliminati con la rimozione della vista. Tutti gli indici non cluster e tutte le statistiche create automaticamente nella vista vengono eliminati con l'eliminazione dell'indice cluster. Le statistiche create dall'utente nella vista vengono conservate. È possibile eliminare gli indici non cluster singolarmente. L'eliminazione dell'indice cluster nella vista determina la rimozione del set di risultati archiviato e la vista tornerà a essere elaborata come standard da Query Optimizer.

È possibile disabilitare gli indici di tabelle e viste. Quando l'indice cluster di una tabella è disabilitato, anche gli indici delle viste associate alla tabella sono disabilitati.

<a name="nondeterministic"></a> Le espressioni che prevedono la conversione implicita di stringhe di caratteri nel tipo di dati **datetime** o **smalldatetime** sono considerate non deterministiche. Per altre informazioni, vedere [Conversione non deterministica di stringhe di valori letterali in valori DATE](../../t-sql/data-types/nondeterministic-convert-date-literals.md).

### <a name="security"></a><a name="Security"></a> Sicurezza

#### <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni

Per creare la vista, è necessario che l'utente disponga dell'autorizzazione **Create View** per il database e dell'autorizzazione **ALTER** per lo schema in cui viene creata la vista. Se la tabella di base risiede in uno schema diverso, l'autorizzazione **References** per la tabella è obbligatoria come minimo. Se l'utente che crea l'indice è diverso da quello degli utenti che hanno creato la vista, per la creazione dell'indice è necessario che l'autorizzazione **ALTER** per la vista sia obbligatoria (coperta da ALTER sullo schema).

    > [!NOTE]  
    > Indexes can only be created on views which have the same owner as the referenced table or tables. This is also called an intact **ownership-chain** between the view and the table(s). Typically, when table and view reside within the same schema, the same schema-owner applies to all objects within the schema. But it is possible that individual objects have different explicit owners. The column **principal_id** in sys.tables contains a value if the owner is different from the schema-owner.


## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL

### <a name="to-create-an-indexed-view"></a>Per creare una vista indicizzata

Nell'esempio seguente vengono creati una vista e un indice per tale vista, quindi vengono incluse due query che usano la vista indicizzata nel database AdventureWorks.

```sql
--Set the options to support indexed views.
SET NUMERIC_ROUNDABORT OFF;
SET ANSI_PADDING, ANSI_WARNINGS, CONCAT_NULL_YIELDS_NULL, ARITHABORT,
   QUOTED_IDENTIFIER, ANSI_NULLS ON;
--Create view with schemabinding.
IF OBJECT_ID ('Sales.vOrders', 'view') IS NOT NULL
   DROP VIEW Sales.vOrders ;
GO
CREATE VIEW Sales.vOrders
   WITH SCHEMABINDING
   AS  
      SELECT SUM(UnitPrice*OrderQty*(1.00-UnitPriceDiscount)) AS Revenue,
         OrderDate, ProductID, COUNT_BIG(*) AS COUNT
      FROM Sales.SalesOrderDetail AS od, Sales.SalesOrderHeader AS o
      WHERE od.SalesOrderID = o.SalesOrderID
      GROUP BY OrderDate, ProductID;
GO
--Create an index on the view.
CREATE UNIQUE CLUSTERED INDEX IDX_V1
   ON Sales.vOrders (OrderDate, ProductID);
GO
--This query can use the indexed view even though the view is
--not specified in the FROM clause.
SELECT SUM(UnitPrice*OrderQty*(1.00-UnitPriceDiscount)) AS Rev,
   OrderDate, ProductID
FROM Sales.SalesOrderDetail AS od
JOIN Sales.SalesOrderHeader AS o
   ON od.SalesOrderID=o.SalesOrderID
      AND ProductID BETWEEN 700 and 800
      AND OrderDate >= CONVERT(datetime,'05/01/2002',101)
   GROUP BY OrderDate, ProductID
   ORDER BY Rev DESC;
GO
--This query can use the above indexed view.
SELECT OrderDate, SUM(UnitPrice*OrderQty*(1.00-UnitPriceDiscount)) AS Rev
FROM Sales.SalesOrderDetail AS od
JOIN Sales.SalesOrderHeader AS o
   ON od.SalesOrderID=o.SalesOrderID
      AND DATEPART(mm,OrderDate)= 3
      AND DATEPART(yy,OrderDate) = 2002
    GROUP BY OrderDate
    ORDER BY OrderDate ASC;
```

Per altre informazioni, vedere [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md).

## <a name="see-also"></a>Vedere anche

- [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md)
- [SET ANSI_NULLS](../../t-sql/statements/set-ansi-nulls-transact-sql.md)
- [SET ANSI_PADDING](../../t-sql/statements/set-ansi-padding-transact-sql.md)
- [SET ANSI_WARNINGS](../../t-sql/statements/set-ansi-warnings-transact-sql.md)
- [SET ARITHABORT](../../t-sql/statements/set-arithabort-transact-sql.md)
- [SET CONCAT_NULL_YIELDS_NULL](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md)
- [SET NUMERIC_ROUNDABORT](../../t-sql/statements/set-numeric-roundabort-transact-sql.md)
- [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md)  
