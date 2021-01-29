---
title: Guide di piano | Microsoft Docs
description: Informazioni sulle guide di piano, che consentono di ottimizzare le prestazioni delle query senza modificare direttamente il testo della query in SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- TEMPLATE plan guide
- SQL plan guides
- OPTIMIZE FOR query hint
- RECOMPILE query hint
- OBJECT plan guide
- plan guides [SQL Server], about plan guides
- OPTION clause
- plan guides [SQL Server]
- USE PLAN query hint
ms.assetid: bfc97632-c14c-4768-9dc5-a9c512f6b2bd
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e230b163fdbe9f48ec4f436b4ef2453040b89b24
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048851"
---
# <a name="plan-guides"></a>Guide di piano
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Le guide di piano consentono di ottimizzare le prestazioni delle query quando non è possibile o non si desidera modificare direttamente il testo della query corrente in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Le guide di piano influiscono sull'ottimizzazione delle query mediante l'aggiunta di hint per la query o di un piano di query fisso. Le guide di piano risultano utili quando le prestazioni di un piccolo subset di query eseguite su un database di terze parti sono inferiori a quelle previste. In una guida di piano, viene specificata l'istruzione Transact-SQL da ottimizzare e la clausola OPTION che contiene gli hint per la query da utilizzare o un piano di query specifico da utilizzare per ottimizzare la query. Quando viene eseguita la query, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] associa l'istruzione Transact-SQL alla guida di piano e, in fase di esecuzione, associa la clausola OPTION alla query oppure utilizza il piano di query specificato.  
  
 Il numero totale di guide di piano che è possibile creare è limitato solo dalle risorse di sistema disponibili. È comunque consigliabile utilizzare le guide di piano per le sole query critiche di cui si desidera migliorare o stabilizzare le prestazioni. Le guide di piano non vanno utilizzate per modificare la maggior parte del carico di query di un'applicazione distribuita.  
  
> [!NOTE]
>  Le guide di piano sono supportate solo in alcune edizioni di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Funzionalità supportate dalle edizioni di SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md). Le guide di piano sono visibili in qualsiasi edizione. È inoltre possibile collegare un database che contiene guide di piano a qualsiasi edizione. Quando si ripristina o si collega un database a una versione aggiornata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le guide di piano non vengono modificate.  
  
## <a name="types-of-plan-guides"></a>Tipi di guide di piano  
 È possibile creare i seguenti tipi di guide di piano:  
  
 ### <a name="object-plan-guide"></a>guida di piano di tipo OBJECT  
 Una guida di piano di tipo OBJECT corrisponde alle query eseguite nel contesto di stored procedure [!INCLUDE[tsql](../../includes/tsql-md.md)] , funzioni scalari definite dall'utente, funzioni con valori di tabella definite dall'utente con istruzioni multiple e trigger DML.  
  
 Si supponga che la stored procedure seguente, che accetta il parametro `@Country_region`, si trovi in un'applicazione di database distribuita sul database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]:  
  
```sql  
CREATE PROCEDURE Sales.GetSalesOrderByCountry (@Country_region nvarchar(60))  
AS  
BEGIN  
    SELECT *  
    FROM Sales.SalesOrderHeader AS h, Sales.Customer AS c,   
        Sales.SalesTerritory AS t  
    WHERE h.CustomerID = c.CustomerID  
        AND c.TerritoryID = t.TerritoryID  
        AND CountryRegionCode = @Country_region  
END;  
```  
  
 Si supponga che questa stored procedure è stata compilata e ottimizzata per `@Country_region = N'AU'` (Australia). Tuttavia, poiché sono presenti relativamente pochi ordini di vendita con origine in Australia, le prestazioni diminuiscono quando viene eseguita la query utilizzando i valori del parametro dei paesi con più ordini di vendita. Poiché la maggior parte degli ordini di vendita proviene dagli Stati Uniti, un piano di query generato per `@Country_region = N'US'` offrirebbe probabilmente prestazioni migliori per tutti i possibili valori del parametro `@Country_region`.  
  
 Per risolvere il problema è possibile modificare la stored procedure aggiungendo alla query l'hint `OPTIMIZE FOR` . Poiché la stored procedure si trova in un'applicazione distribuita, non è possibile modificare direttamente il codice dell'applicazione. È invece possibile creare la guida di piano seguente nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
```sql  
sp_create_plan_guide   
@name = N'Guide1',  
@stmt = N'SELECT *FROM Sales.SalesOrderHeader AS h,  
        Sales.Customer AS c,  
        Sales.SalesTerritory AS t  
        WHERE h.CustomerID = c.CustomerID   
            AND c.TerritoryID = t.TerritoryID  
            AND CountryRegionCode = @Country_region',  
@type = N'OBJECT',  
@module_or_batch = N'Sales.GetSalesOrderByCountry',  
@params = NULL,  
@hints = N'OPTION (OPTIMIZE FOR (@Country_region = N''US''))';  
```  
  
 Al momento dell'esecuzione della query specificata nell'istruzione `sp_create_plan_guide` , la query viene modificata prima dell'ottimizzazione per includere la clausola `OPTIMIZE FOR (@Country = N''US'')` .  
  
 ### <a name="sql-plan-guide"></a>Guida di piano di tipo SQL  
 Una guida di piano di tipo SQL corrisponde alle query eseguite nel contesto di batch e istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] autonome che non fanno parte di un oggetto di database. Le guide di piano basate su SQL possono inoltre essere utilizzate per query con parametrizzazioni specifiche. Le guide di piano di tipo SQL vengono applicate a istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] autonome e batch. Spesso tali istruzioni vengono inoltrate da un'applicazione mediante la stored procedure di sistema [sp_executesql](../../relational-databases/system-stored-procedures/sp-executesql-transact-sql.md) . Ad esempio, si consideri il batch autonomo seguente:  
  
```sql  
SELECT TOP 1 * FROM Sales.SalesOrderHeader ORDER BY OrderDate DESC;  
```  
  
 Per impedire la generazione di un piano di esecuzione parallelo su questa query, creare la seguente guida di piano e impostare l'hint per la query `MAXDOP` su `1` nel parametro `@hints` .  
  
```sql  
sp_create_plan_guide   
@name = N'Guide2',   
@stmt = N'SELECT TOP 1 * FROM Sales.SalesOrderHeader ORDER BY OrderDate DESC',  
@type = N'SQL',  
@module_or_batch = NULL,   
@params = NULL,   
@hints = N'OPTION (MAXDOP 1)';  
```  
Un altro esempio è la seguente istruzione SQL inviata usando [sp_executesql](../../relational-databases/system-stored-procedures/sp-executesql-transact-sql.md).

```sql  
exec sp_executesql N'SELECT * FROM Sales.SalesOrderHeader
where SalesOrderID =  @so_id', N'@so_id int', @so_id = 43662;  
```  
 Per creare un piano univoco per ogni esecuzione di questa query, creare la guida di piano seguente e usare l'hint per la query `OPTION (RECOMPILE)` nel parametro `@hints`. 

```sql  
exec sp_create_plan_guide   
@name = N'PlanGuide1_SalesOrders',   
@stmt = N'SELECT * FROM Sales.SalesOrderHeader
where SalesOrderID =  @so_id',
@type = N'SQL',  
@module_or_batch = NULL,   
@params = N'@so_id int',   
@hints = N'OPTION (recompile)';
```

> [!IMPORTANT]  
>  I valori specificati per gli argomenti `@module_or_batch` e `@params` dell'istruzione `sp_create_plan guide` devono corrispondere al testo specificato nella query effettiva. Per altre informazioni, vedere [sp_create_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md) e [Usare SQL Server Profiler per creare e testare guide di piano](../../relational-databases/performance/use-sql-server-profiler-to-create-and-test-plan-guides.md).  
  
 È possibile creare guide di piano SQL anche per le query con parametrizzazione forzata quando l'opzione di database PARAMETERIZATION è impostata su FORCED, oppure quando si crea una guida di piano di tipo TEMPLATE per specificare la parametrizzazione di una classe di query.  
  
 ### <a name="template-plan-guide"></a>TEMPLATE - guida di piano  
 Una guida di piano di tipo TEMPLATE corrisponde alle query autonome con parametrizzazioni specifiche. Tali guide di piano vengono utilizzate per sostituire l'opzione SET di database PARAMETERIZATION di un database per una classe di query.  
  
 È possibile creare una guida di piano di tipo TEMPLATE nelle seguenti situazioni:  
  
-   L'opzione di database PARAMETERIZATION è impostata su FORCED, ma si vogliono compilare alcune query in base alle regole della [parametrizzazione semplice](../../relational-databases/query-processing-architecture-guide.md#SimpleParam).  
  
-   L'opzione di database PARAMETERIZATION è impostata su SIMPLE (impostazione predefinita), ma si vuole che una classe di query venga sottoposta a [parametrizzazione forzata](../../relational-databases/query-processing-architecture-guide.md#ForcedParam).  
  
## <a name="plan-guide-matching-requirements"></a>Requisiti di corrispondenza per la guida di piano  
 Le guide di piano sono definite a livello di ambito del database in cui vengono create. Pertanto, è possibile far corrispondere alla query solo le guide di piano presenti nel database corrente al momento dell'esecuzione della query. Ad esempio, se [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] è il database corrente e viene eseguita la query seguente:  
  
 ```sql
 SELECT FirstName, LastName FROM Person.Person;
 ```  
  
 È possibile far corrispondere alla query solo le guide di piano nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . Se tuttavia il database corrente è [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] e vengono eseguite le istruzioni seguenti:  
  
 ```sql
 USE DB1; 
 SELECT FirstName, LastName FROM Person.Person;
 ```  
  
 È possibile far corrispondere alla query solo le guide di piano in `DB1` , poiché la query è in esecuzione nel contesto di `DB1`.  
  
 Per guide di piano basate su SQL o TEMPLATE, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esegue la corrispondenza tra i valori per gli argomenti @module_or_batch e @params e una query, confrontando i due valori carattere per carattere. Per questo motivo è necessario immettere il testo esattamente come [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lo riceve nel batch.  
  
 Se @type = 'SQL' e @module_or_batch vengono impostate su NULL, il valore di @module_or_batch viene impostato sul valore di @stmt. Di conseguenza, il valore per *statement_text* deve essere specificato nello stesso formato, carattere per carattere, così come viene inviato a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per semplificare questa corrispondenza, non viene eseguita alcuna conversione interna.  
  
 Quando è possibile applicare sia una guida di piano normale (SQL o OBJECT) sia una guida di piano TEMPLATE a un'istruzione, verrà utilizzata solo la guida di piano normale.  
  
> [!NOTE]  
>  Il batch contenente l'istruzione sulla quale si desidera creare una guida di piano non può contenere un'istruzione USE *database* .  
  
## <a name="plan-guide-effect-on-the-plan-cache"></a>Effetto delle guide di piano sulla cache dei piani  
 La creazione di una guida di piano su un modulo rimuove il piano di query per il dato modulo dalla cache dei piani. La creazione di una guida di piano di tipo OBJECT o SQL su un batch rimuove il piano di query per un batch con lo stesso valore hash. La creazione di una guida di piano di tipo TEMPLATE rimuove tutti i batch a istruzione singola dalla cache dei piani all'interno del database.  
  
## <a name="related-tasks"></a>Attività correlate  
  
|Attività|Argomento|  
|----------|-----------|  
|Viene descritto come creare una guida di piano.|[Creare una nuova guida di piano](../../relational-databases/performance/create-a-new-plan-guide.md)|  
|Viene descritto come creare una guida di piano per le query con parametri.|[Creare una guida di piano per le query con parametri](../../relational-databases/performance/create-a-plan-guide-for-parameterized-queries.md)|  
|Viene descritto come controllare il comportamento di parametrizzazione delle query utilizzando guide di piano.|[Specificare il comportamento di parametrizzazione delle query tramite guide di piano](../../relational-databases/performance/specify-query-parameterization-behavior-by-using-plan-guides.md)|  
|Viene descritto come includere un piano di query fisso in una guida di piano.|[Applicare un piano di query fisso a una guida di piano](../../relational-databases/performance/apply-a-fixed-query-plan-to-a-plan-guide.md)|  
|Viene descritto come specificare hint per la query in una guida di piano.|[Associare gli hint per le query a una guida di piano](../../relational-databases/performance/attach-query-hints-to-a-plan-guide.md)|  
|Viene descritto come visualizzare le proprietà di una guida di piano.|[Visualizzare le proprietà delle guide di piano](../../relational-databases/performance/view-plan-guide-properties.md)|  
|Viene descritto come utilizzare SQL Server Profiler per creare e testare guide di piano.|[Usare SQL Server Profiler per creare e testare guide di piano](../../relational-databases/performance/use-sql-server-profiler-to-create-and-test-plan-guides.md)|  
|Viene descritto come convalidare una guida di piano.|[Convalidare le guide di piano dopo l'aggiornamento](../../relational-databases/performance/validate-plan-guides-after-upgrade.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [sp_create_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)   
 [sp_create_plan_guide_from_handle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md)   
 [sp_control_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-control-plan-guide-transact-sql.md)   
 [sys.plan_guides &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-plan-guides-transact-sql.md)   
 [sys.fn_validate_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-validate-plan-guide-transact-sql.md)  
  
  
