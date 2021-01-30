---
description: sp_create_plan_guide (Transact-SQL)
title: sp_create_plan_guide (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_create_plan_guide
- sp_create_plan_guide_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_create_plan_guide
ms.assetid: 5a8c8040-4f96-4c74-93ab-15bdefd132f0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0203cbe0539d0733740cf495aa0c149409468fa8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205214"
---
# <a name="sp_create_plan_guide-transact-sql"></a>sp_create_plan_guide (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Crea una guida di piano per associare gli hint per le query o gli effettivi piani di query con le query in un database. Per altre informazioni sulle guide di piano, vedere [Guide di piano](../../relational-databases/performance/plan-guides.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
sp_create_plan_guide [ @name = ] N'plan_guide_name'  
    , [ @stmt = ] N'statement_text'  
    , [ @type = ] N'{ OBJECT | SQL | TEMPLATE }'  
    , [ @module_or_batch = ]  
      {   
                    N'[ schema_name. ] object_name'  
        | N'batch_text'  
        | NULL  
      }  
    , [ @params = ] { N'@parameter_name data_type [ ,...n ]' | NULL }   
    , [ @hints = ] { N'OPTION ( query_hint [ ,...n ] )'   
                 | N'XML_showplan'  
                 | NULL }  
```  
  
## <a name="arguments"></a>Argomenti  
 [ \@ Name =] n'*plan_guide_name*'  
 Nome della guida di piano. I nomi delle guide di piano vengono definiti a livello dell'ambito del database corrente. *plan_guide_name* devono essere conformi alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md) e non possono iniziare con il simbolo di cancelletto (#). La lunghezza massima consentita di *plan_guide_name* è 124 caratteri.  
  
 [ \@ stmt =] n'*statement_text*'  
 Istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] in cui creare una guida di piano. Quando il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] query optimizer riconosce una query che corrisponde *statement_text*, *plan_guide_name* ha effetto. Per la creazione di una guida di piano, *statement_text* deve essere visualizzato nel contesto specificato dai \@ parametri Type, \@ module_or_batch e \@ params.  
  
 è necessario specificare *statement_text* in modo che il Query Optimizer corrisponda all'istruzione corrispondente fornita all'interno del batch o del modulo identificato da \@ module_or_batch e \@ params. Per ulteriori informazioni, vedere la sezione "Note". Le dimensioni del *statement_text* sono limitate solo dalla memoria disponibile del server.  
  
 [ \@ Type =] n'{Object | SQL | TEMPLATE}'  
 Tipo di entità in cui viene visualizzato *statement_text* . Specifica il contesto per la corrispondenza *statement_text* *plan_guide_name*.  
  
 OBJECT  
 Indica *statement_text* viene visualizzato nel contesto di un [!INCLUDE[tsql](../../includes/tsql-md.md)] stored procedure, una funzione scalare, una funzione a più istruzioni con valori di tabella o un [!INCLUDE[tsql](../../includes/tsql-md.md)] trigger DML nel database corrente.  
  
 SQL  
 Indica *statement_text* viene visualizzato nel contesto di un'istruzione autonoma o di un batch che può essere inviato a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite qualsiasi meccanismo. [!INCLUDE[tsql](../../includes/tsql-md.md)] le istruzioni inviate dagli oggetti Common Language Runtime (CLR) o dalle stored procedure estese o tramite EXEC N'*sql_string*' vengono elaborate come batch nel server e pertanto devono essere identificate come \@ tipo **=** ' SQL '. Se si specifica SQL, la parametrizzazione dell'hint per la query {FORCEd | Non è possibile specificare SIMPLE} nel \@ parametro hints.  
  
 TEMPLATE  
 Indica che la Guida di piano si applica a qualsiasi query che parametrizza al form indicato in *statement_text*. Se si specifica TEMPLATE, solo la parametrizzazione {FORCEd | SIMPLE} l'hint per la query può essere specificato nel \@ parametro hints. Per ulteriori informazioni sulle guide di piano TEMPLATE, vedere [impostazione del comportamento di parametrizzazione delle query tramite guide di piano](../../relational-databases/performance/specify-query-parameterization-behavior-by-using-plan-guides.md).  
  
 [ \@ module_or_batch =] {n'[ *schema_name*. ] *object_name*' | N'*batch_text*' | NULL  
 Specifica il nome dell'oggetto in cui viene visualizzato *statement_text* o il testo del batch in cui viene visualizzato *statement_text* . Il testo del batch non può includere un'istruzione USE *database* .  
  
 Affinché una guida di piano corrisponda a un batch inviato da un'applicazione, *batch_tex* t deve essere specificato nello stesso formato, carattere per carattere, così come viene inviato a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per semplificare questa corrispondenza, non viene eseguita alcuna conversione interna. Per altre informazioni, vedere la sezione Osservazioni.  
  
 [*schema_name*.] *object_name* specifica il nome di un [!INCLUDE[tsql](../../includes/tsql-md.md)] stored procedure, una funzione scalare, una funzione a più istruzioni con valori di tabella o un [!INCLUDE[tsql](../../includes/tsql-md.md)] trigger DML che contiene *statement_text*. Se non viene specificato *schema_name* , *schema_name* utilizza lo schema dell'utente corrente. Se viene specificato NULL e \@ Type =' SQL ', il valore di \@ module_or_batch viene impostato sul valore di \@ stmt. Se \@ Type =' template **\'** , \@ MODULE_OR_BATCH deve essere null.  
  
 [ \@ params =] {n'*\@ parameter_name data_type* [,*... n* ]' | NULL  
 Specifica le definizioni di tutti i parametri incorporati in *statement_text*. \@params si applica solo quando si verifica una delle condizioni seguenti:  
  
-   \@digitare = "SQL" o "TEMPLATE". Se ' TEMPLATE ', i \@ parametri non devono essere null.  
  
-   *statement_text* viene inviato utilizzando sp_executesql e viene specificato un valore per il \@ parametro params oppure [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] internamente viene inviata un'istruzione dopo l'parametrizzazione. Le query con parametri inviate dalle API del database, incluse ODBC, OLE DB e ADO.NET, vengono considerate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] come chiamate a sp_executesql o alle routine dei cursori API del server. È pertanto possibile eseguire le corrispondenze in base alle guide di piano di tipo SQL o TEMPLATE.  
  
 *\@ parameter_name data_type* devono essere specificati nello stesso formato in cui vengono inviati a utilizzando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sp_executesql o inviati internamente dopo la parametrizzazione. Per altre informazioni, vedere la sezione Osservazioni. Se il batch non include parametri, è necessario specificare NULL. Le dimensioni dei \@ parametri sono limitate solo dalla memoria del server disponibile.  
  
 [ \@ hints =] {n'Option (*query_hint* [,...*n* ])' | N'*XML_showplan*' | NULL  
 N'Option (*query_hint* *[,... n* ])  
 Specifica una clausola OPTION per la connessione a una query che corrisponde a \@ stmt. \@ gli hint devono essere sintatticamente identici a una clausola OPTION in un'istruzione SELECT e possono contenere qualsiasi sequenza valida di hint per la query.  
  
 N'*XML_showplan*'  
 Il piano di query in formato XML da applicare come hint.  
  
 Si consiglia di assegnare Showplan XML a una variabile; in caso contrario, è necessario utilizzare caratteri di escape per ogni virgoletta singola nello Showplan facendola precedere da un'altra virgoletta singola. Vedere l'esempio E.  
  
 NULL  
 Indica che tutti gli hint esistenti specificati nella clausola OPTION della query non vengono applicati alla query. Per ulteriori informazioni, vedere [clausola OPTION &#40;&#41;Transact-SQL ](../../t-sql/queries/option-clause-transact-sql.md).  
  
## <a name="remarks"></a>Commenti  
 Gli argomenti per sp_create_plan_guide devono essere inseriti nell'ordine illustrato. Quando si forniscono valori per i parametri di **sp_create_plan_guide**, è necessario specificare in modo esplicito tutti i nomi dei parametri oppure nessuno. Se, ad esempio, si specifica **\@ Name =** , è necessario specificare anche **\@ stmt =** , **\@ Type =** e così via. Analogamente, se **\@ nome =** viene omesso e viene specificato solo il valore del parametro, è necessario omettere anche i nomi dei parametri rimanenti e solo i relativi valori specificati. I nomi degli argomenti hanno scopo esclusivamente descrittivo, per facilitare la comprensione della sintassi. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non verifica che il nome di parametro specificato corrisponda al nome del parametro nella posizione in cui il nome viene utilizzato.  
  
 È possibile creare più guide di piano OBJECT o SQL per la stessa query e batch o modulo. Tuttavia è possibile abilitare una sola guida di piano alla volta.  
  
 Non è possibile creare guide di piano di tipo OBJECT per un \@ valore module_or_batch che fa riferimento a un stored procedure, una funzione o un trigger DML che specifica la clausola WITH ENCRYPTION o che è temporaneo.  
  
 Se si tenta di eliminare o modificare una funzione, una stored procedure o un trigger DML a cui viene fatto riferimento in una guida di piano abilitata o disabilitata, viene generato un errore. Viene generato un errore anche se si cerca di eliminare una tabella per la quale è stato definito un trigger a cui una guida di piano fa riferimento.  
  
> [!NOTE]
> Le guide di piano sono supportate solo in alcune edizioni di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Funzionalità supportate dalle edizioni di SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md). Le guide di piano sono visibili in qualsiasi edizione. È inoltre possibile collegare un database che contiene guide di piano a qualsiasi edizione. Quando si ripristina o si collega un database a una versione aggiornata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le guide di piano non vengono modificate. Dopo l'esecuzione di un aggiornamento del server è opportuno verificare l'effettiva necessità delle guide di piano di ogni database.  
  
## <a name="plan-guide-matching-requirements"></a>Requisiti di corrispondenza per la Guida di piano  
 Per le guide di piano che specificano \@ Type =' SQL ' o \@ Type =' template ' per trovare una corrispondenza con una query, i valori per *batch_text* e *\@ parameter_name data_type* [,*... n* ] deve essere specificato esattamente nello stesso formato delle rispettive controparti inviate dall'applicazione. Ciò significa che è necessario specificare il testo del batch esattamente come il compilatore di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lo riceve. Per acquisire il testo effettivo del batch e del parametro, è possibile utilizzare [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. Per ulteriori informazioni, vedere [utilizzare SQL Server Profiler per creare e testare le guide di piano](../../relational-databases/performance/use-sql-server-profiler-to-create-and-test-plan-guides.md).  
  
 Quando \@ Type =' SQL ' e \@ module_or_batch è impostato su null, il valore di \@ module_or_batch è impostato sul valore di \@ stmt. Ciò significa che il valore di *statement_text* deve essere fornito esattamente nello stesso formato, carattere per carattere, così come viene inviato a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per semplificare questa corrispondenza, non viene eseguita alcuna conversione interna.  
  
 Quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] corrisponde al valore di *statement_text* *batch_text* e *\@ parameter_name data_type* [,.*.. n* ], o se \@ Type = **\'** Object ', al testo della query corrispondente all'interno *object_name*, gli elementi stringa seguenti non vengono considerati:  
  
-   Spazi vuoti (tabulazioni, spazi, ritorni a capo o avanzamenti di riga) all'interno della stringa.  
  
-   Commenti ( **--** o **/\*   \*/** ).  
  
-   Punto e virgola finale.  
  
 Ad esempio, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può corrispondere alla  stringa `N'SELECT * FROM T WHERE a = 10'` di statement_text al *batch_text* seguente:  
  
 ```
 N'SELECT *
 FROM T
 WHERE a = 10' 
 ```
 
 Tuttavia, la stessa stringa non verrà confrontata con questo *batch_text*:  
  
 `N'SELECT * FROM T WHERE b = 10'`  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ignora i caratteri di ritorno a capo, avanzamento riga e gli spazi all'interno della prima query. Nella seconda query la sequenza `WHERE b = 10` viene interpretata in modo diverso da `WHERE a = 10`. Nella corrispondenza viene sempre fatta distinzione tra maiuscole e minuscole e tra caratteri accentati e non accentati, anche quando la distinzione tra maiuscole e minuscole non è significativa per le regole di confronto del database, tranne il caso delle parole chiave, dove la distinzione tra maiuscole e minuscole non è significativa. La forma abbreviata delle parole chiave non è rilevante nella corrispondenza. Ad esempio, le parole chiave `EXECUTE`, `EXEC` e `execute` vengono considerate equivalenti.  
  
## <a name="plan-guide-effect-on-the-plan-cache"></a>Effetto della Guida di piano sulla cache dei piani  
 La creazione di una guida di piano su un modulo rimuove il piano di query per il dato modulo dalla cache dei piani. La creazione di una guida di piano di tipo OBJECT o SQL su un batch rimuove il piano di query per un batch con lo stesso valore hash. La creazione di una guida di piano di tipo TEMPLATE rimuove tutti i batch a istruzione singola dalla cache dei piani all'interno del database.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per creare una guida di piano di tipo OBJECT, è richiesta `ALTER` l'autorizzazione per l'oggetto a cui si fa riferimento. Per creare una guida di piano di tipo SQL o TEMPLATE, è richiesta `ALTER` l'autorizzazione per il database corrente.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-creating-a-plan-guide-of-type-object-for-a-query-in-a-stored-procedure"></a>R. Creazione di una guida di piano di tipo OBJECT per una query in una stored procedure  
 Nell'esempio seguente viene creata una guida di piano corrispondente a una query eseguita nel contesto di una stored procedure basata sull'applicazione e alla query viene applicato l'hint `OPTIMIZE FOR`.  
  
 Di seguito è riportata la stored procedure:  
  
```sql  
IF OBJECT_ID(N'Sales.GetSalesOrderByCountry', N'P') IS NOT NULL  
    DROP PROCEDURE Sales.GetSalesOrderByCountry;  
GO  
CREATE PROCEDURE Sales.GetSalesOrderByCountry   
    (@Country_region nvarchar(60))  
AS  
BEGIN  
    SELECT *  
    FROM Sales.SalesOrderHeader AS h   
    INNER JOIN Sales.Customer AS c ON h.CustomerID = c.CustomerID  
    INNER JOIN Sales.SalesTerritory AS t   
        ON c.TerritoryID = t.TerritoryID  
    WHERE t.CountryRegionCode = @Country_region;  
END  
GO  
```  
  
 Di seguito è riportata la guida di piano creata in base alla query nella stored procedure:  
  
```sql  
EXEC sp_create_plan_guide   
    @name =  N'Guide1',  
    @stmt = N'SELECT *  
              FROM Sales.SalesOrderHeader AS h   
              INNER JOIN Sales.Customer AS c   
                 ON h.CustomerID = c.CustomerID  
              INNER JOIN Sales.SalesTerritory AS t   
                 ON c.TerritoryID = t.TerritoryID  
              WHERE t.CountryRegionCode = @Country_region',  
    @type = N'OBJECT',  
    @module_or_batch = N'Sales.GetSalesOrderByCountry',  
    @params = NULL,  
    @hints = N'OPTION (OPTIMIZE FOR (@Country_region = N''US''))';  
```  
  
### <a name="b-creating-a-plan-guide-of-type-sql-for-a-stand-alone-query"></a>B. Creazione di una guida di piano di tipo SQL per una query autonoma  
 Nell'esempio seguente viene creata una guida di piano corrispondente a una query in un batch inviato da un'applicazione che utilizza la stored procedure di sistema sp_executesql.  
  
 Di seguito è riportato il batch:  
  
```sql  
SELECT TOP 1 * FROM Sales.SalesOrderHeader ORDER BY OrderDate DESC;  
```  
  
 Per evitare la generazione di un piano di esecuzione parallela in base a questa query, creare la guida di piano seguente:  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide1',   
    @stmt = N'SELECT TOP 1 *   
              FROM Sales.SalesOrderHeader   
              ORDER BY OrderDate DESC',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (MAXDOP 1)';  
```  
  
### <a name="c-creating-a-plan-guide-of-type-template-for-the-parameterized-form-of-a-query"></a>C. Creazione di una guida di piano di tipo TEMPLATE per il formato con parametri di una query  
 Nel seguente esempio viene creata una guida di piano corrispondente a qualsiasi query che parametrizza un formato specifico e forza in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'esecuzione della parametrizzazione della query. Le due query seguenti sono equivalenti a livello sintattico. L'unica differenza risiede nei relativi valori letterali costanti.  
  
```sql  
SELECT * FROM AdventureWorks2012.Sales.SalesOrderHeader AS h  
INNER JOIN AdventureWorks2012.Sales.SalesOrderDetail AS d   
    ON h.SalesOrderID = d.SalesOrderID  
WHERE h.SalesOrderID = 45639;  
  
SELECT * FROM AdventureWorks2012.Sales.SalesOrderHeader AS h  
INNER JOIN AdventureWorks2012.Sales.SalesOrderDetail AS d   
    ON h.SalesOrderID = d.SalesOrderID  
WHERE h.SalesOrderID = 45640;  
```  
  
 Di seguito è riportata la guida di piano nel formato con parametri della query:  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'TemplateGuide1',  
    @stmt = N'SELECT * FROM AdventureWorks2012.Sales.SalesOrderHeader AS h  
              INNER JOIN AdventureWorks2012.Sales.SalesOrderDetail AS d   
                  ON h.SalesOrderID = d.SalesOrderID  
              WHERE h.SalesOrderID = @0',  
    @type = N'TEMPLATE',  
    @module_or_batch = NULL,  
    @params = N'@0 int',  
    @hints = N'OPTION(PARAMETERIZATION FORCED)';  
```  
  
 Nell'esempio precedente il valore del parametro `@stmt` corrisponde al formato con parametri della query. L'unico modo affidabile per ottenere questo valore da usare in sp_create_plan_guide è usare la stored procedure di sistema [sp_get_query_template](../../relational-databases/system-stored-procedures/sp-get-query-template-transact-sql.md) . Lo script seguente può essere utilizzato sia per ottenere la query con parametri che per creare una guida di piano in base a essa.  
  
```sql  
DECLARE @stmt nvarchar(max);  
DECLARE @params nvarchar(max);  
EXEC sp_get_query_template   
    N'SELECT * FROM AdventureWorks2012.Sales.SalesOrderHeader AS h  
      INNER JOIN AdventureWorks2012.Sales.SalesOrderDetail AS d   
          ON h.SalesOrderID = d.SalesOrderID  
      WHERE h.SalesOrderID = 45639;',  
    @stmt OUTPUT,   
    @params OUTPUT  
EXEC sp_create_plan_guide N'TemplateGuide1',   
    @stmt,   
    N'TEMPLATE',   
    NULL,   
    @params,   
    N'OPTION(PARAMETERIZATION FORCED)';  
```  
  
> [!IMPORTANT]  
>  Il valore letterale costante nel parametro `@stmt` passato a sp_get_query_template potrebbe interessare il tipo di dati scelto per il parametro che sostituisce il valore letterale. Ciò potrebbe avere ripercussioni sulla corrispondenza eseguita in base alla guida di piano. Potrebbe essere necessario creare più guide di piano per gestire intervalli di valori dei parametri diversi.  
  
### <a name="d-creating-a-plan-guide-on-a-query-submitted-by-using-an-api-cursor-request"></a>D. Creazione di una guida di piano in una query inviata utilizzando una richiesta di cursore API  
 È possibile eseguire la corrispondenza tra le guide di piano e le query inviate da routine dei cursori API del server. Le routine includono sp_cursorprepare, sp_cursorprepexec e sp_cursoropen. Le applicazioni che utilizzano le API ADO, OLE DB e ODBC interagiscono frequentemente con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante cursori API del server. È possibile visualizzare la chiamata delle routine dei cursori API del server nei file di traccia di [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] mediante la visualizzazione dell'evento di traccia RPC:Starting del profiler.  
  
 Si supponga che in un evento di traccia RPC:Starting del profiler vengano visualizzati i seguenti dati per una query che si desidera ottimizzare con una guida di piano:  
  
```sql  
DECLARE @p1 int;  
SET @p1=-1;  
DECLARE @p2 int;  
SET @p2=0;  
DECLARE @p5 int;  
SET @p5=4104;  
DECLARE @p6 int;  
SET @p6=8193;  
DECLARE @p7 int;  
SET @p7=0;  
EXEC sp_cursorprepexec @p1 output,@p2 output,N'@P1 varchar(255),@P2 varchar(255)',N'SELECT * FROM Sales.SalesOrderHeader AS h INNER JOIN Sales.SalesOrderDetail AS d ON h.SalesOrderID = d.SalesOrderID WHERE h.OrderDate BETWEEN @P1 AND @P2',@p5 OUTPUT,@p6 OUTPUT,@p7 OUTPUT,'20040101','20050101'  
SELECT @p1, @p2, @p5, @p6, @p7;  
```  
  
 Si noti che il piano per la query `SELECT` nella chiamata a `sp_cursorprepexec` utilizza un merge join. Si desidera utilizzare invece un hash join. La query sottomessa tramite `sp_cursorprepexec` viene parametrizzata e include una stringa query e una stringa parametro. È possibile creare la guida di piano seguente per modificare la scelta del piano utilizzando le stringhe query e parametro esattamente come sono, carattere per carattere, nella chiamata a `sp_cursorprepexec`.  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'APICursorGuide',  
    @stmt = N'SELECT * FROM Sales.SalesOrderHeader AS h   
              INNER JOIN Sales.SalesOrderDetail AS d   
                ON h.SalesOrderID = d.SalesOrderID   
              WHERE h.OrderDate BETWEEN @P1 AND @P2',  
    @type = N'SQL',  
    @module_or_batch = NULL,  
    @params = N'@P1 varchar(255),@P2 varchar(255)',  
    @hints = N'OPTION(HASH JOIN)';  
```  
  
 Le esecuzioni successive di questa query da parte dell'applicazione saranno interessate da questa guida di piano e per l'elaborazione della query verrà utilizzato un hash join.  
  
### <a name="e-creating-a-plan-guide-by-obtaining-the-xml-showplan-from-a-cached-plan"></a>E. Creazione di una guida di piano ottenendo lo Showplan XML da un piano memorizzato nella cache  
 Nell'esempio seguente viene creata una guida di piano per una semplice istruzione SQL ad hoc. Il piano di query desiderato per questa istruzione è fornito nella guida di piano specificando lo Showplan XML per la query direttamente nel parametro `@hints` . Viene innanzitutto eseguita l'istruzione SQL per generare un piano nella cache dei piani. Ai fini di questo esempio, si presuppone che il piano generato sia il piano desiderato, senza che sia richiesta alcuna ottimizzazione aggiuntiva della query. Lo Showplan XML per la query si ottiene eseguendo una query sulle viste a gestione dinamica `sys.dm_exec_query_stats`, `sys.dm_exec_sql_text`e `sys.dm_exec_text_query_plan` e assegnandolo alla variabile `@xml_showplan` . La variabile `@xml_showplan` passa quindi all'istruzione `sp_create_plan_guide` nel parametro `@hints` . In alternativa, è possibile creare una guida di piano da un piano di query nella cache dei piani usando la stored procedure [sp_create_plan_guide_from_handle](../../relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md) .  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT City, StateProvinceID, PostalCode FROM Person.Address ORDER BY PostalCode DESC;  
GO  
DECLARE @xml_showplan nvarchar(max);  
SET @xml_showplan = (SELECT query_plan  
    FROM sys.dm_exec_query_stats AS qs   
    CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) AS st  
    CROSS APPLY sys.dm_exec_text_query_plan(qs.plan_handle, DEFAULT, DEFAULT) AS qp  
    WHERE st.text LIKE N'SELECT City, StateProvinceID, PostalCode FROM Person.Address ORDER BY PostalCode DESC;%');  
  
EXEC sp_create_plan_guide   
    @name = N'Guide1_from_XML_showplan',   
    @stmt = N'SELECT City, StateProvinceID, PostalCode FROM Person.Address ORDER BY PostalCode DESC;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints =@xml_showplan;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Guide di piano](../../relational-databases/performance/plan-guides.md)   
 [sp_control_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-control-plan-guide-transact-sql.md)   
 [sys.plan_guides &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-plan-guides-transact-sql.md)   
 [Stored procedure di motore di database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
 [sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)   
 [sys.dm_exec_query_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
 [sp_create_plan_guide_from_handle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md)   
 [sys.fn_validate_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-validate-plan-guide-transact-sql.md)   
 [sp_get_query_template &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-get-query-template-transact-sql.md)  
  
  
