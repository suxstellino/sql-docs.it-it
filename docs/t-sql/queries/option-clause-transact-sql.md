---
description: Clausola OPTION (Transact-SQL)
title: Clausola OPTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OPTION clause
- OPTION_TSQL
- OPTION
- OPTION_clause_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- clauses [SQL Server], OPTION
- OPTION clause
ms.assetid: f47e2f3f-9302-4711-9d66-16b1a2a7ffe3
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 29f3bd1f215e4a915a9f7a51bc51d78e91d9f293
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750501"
---
# <a name="option-clause-transact-sql"></a>Clausola OPTION (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Specifica che l'hint per la query indicato deve essere utilizzato in tutta la query. Sono consentiti più hint per la query. Ogni hint, tuttavia, può essere specificato una sola volta. In una istruzione è consentito utilizzare una sola clausola OPTION.  
  
 La clausola può essere specificata nelle istruzioni SELECT, DELETE, UPDATE e MERGE.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
### <a name="syntax-for-ssnoversion-mdmd-and-ssazure_mdmd"></a>Sintassi per [!INCLUDE[ssnoversion-md.md](../../includes/ssnoversion-md.md)] e [!INCLUDE[ssazure_md.md](../../includes/ssazure_md.md)] .

```syntaxsql

[ OPTION ( <query_hint> [ ,...n ] ) ]   
```  
  
### <a name="syntax-for-sssdw-mdmd-and-sspdw-mdmd"></a>Sintassi per [!INCLUDE[sssdw-md.md](../../includes/sssdw-md.md)] e [!INCLUDE[sspdw-md.md](../../includes/sspdw-md.md)]

```syntaxsql
  
OPTION ( <query_option> [ ,...n ] )  
  
<query_option> ::=  
    LABEL = label_name |  
    <query_hint>  
  
<query_hint> ::=  
    HASH JOIN   
    | LOOP JOIN   
    | MERGE JOIN  
    | FORCE ORDER  
    | { FORCE | DISABLE } EXTERNALPUSHDOWN  
```  
  
### <a name="syntax-for-sssodfull-mdmd"></a>Sintassi per [!INCLUDE[sssodfull-md.md](../../includes/sssodfull-md.md)]

```syntaxsql

OPTION ( <query_option> [ ,...n ] )

<query_option> ::=
    LABEL = label_name
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *query_hint*  
 Parole chiave che indicano quali hint di ottimizzazione vengono utilizzati per personalizzare la modalità di elaborazione dell'istruzione nel motore di database. Per altre informazioni, vedere [Hint per la query &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-an-option-clause-with-a-group-by-clause"></a>R. Uso di una clausola OPTION con una clausola GROUP BY  
 Nell'esempio seguente viene illustrato l'utilizzo della clausola `OPTION` con una clausola `GROUP BY`.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ProductID, OrderQty, SUM(LineTotal) AS Total  
FROM Sales.SalesOrderDetail  
WHERE UnitPrice < $5.00  
GROUP BY ProductID, OrderQty  
ORDER BY ProductID, OrderQty  
OPTION (HASH GROUP, FAST 10);  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="b-select-statement-with-a-label-in-the-option-clause"></a>B. Istruzione SELECT con un'etichetta nella clausola OPTION  
 Nell'esempio seguente viene illustrata un'istruzione [!INCLUDE[ssDW](../../includes/ssdw-md.md)]SELECT semplice con un'etichetta nella clausola OPTION.  
  
```sql
-- Uses AdventureWorks  
  
SELECT * FROM FactResellerSales  
  OPTION ( LABEL = 'q17' );  
```  
  
### <a name="c-select-statement-with-a-query-hint-in-the-option-clause"></a>C. Istruzione SELECT con un hint per la query nella clausola OPTION  
 Nell'esempio seguente viene illustrata un'istruzione SELECT che usa un hint per la query HASH JOIN nella clausola OPTION.  
  
```sql
-- Uses AdventureWorks  
  
SELECT COUNT (*) FROM dbo.DimCustomer a  
INNER JOIN dbo.FactInternetSales b   
ON (a.CustomerKey = b.CustomerKey)  
OPTION (HASH JOIN);  
```  
  
### <a name="d-select-statement-with-a-label-and-multiple-query-hints-in-the-option-clause"></a>D. Istruzione SELECT con un'etichetta e più hint per la query nella clausola OPTION  
 L'esempio seguente è un'istruzione [!INCLUDE[ssDW](../../includes/ssdw-md.md)] SELECT che contiene un'etichetta e più hint per la query. Quando viene eseguita la query sui nodi di calcolo, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] applicherà un hash join o un merge join, in base alla strategia che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] determina come ottimale.  
  
```sql
-- Uses AdventureWorks  
  
SELECT COUNT (*) FROM dbo.DimCustomer a  
INNER JOIN dbo.FactInternetSales b   
ON (a.CustomerKey = b.CustomerKey)  
OPTION ( Label = 'CustJoin', HASH JOIN, MERGE JOIN);  
```  
  
### <a name="e-using-a-query-hint-when-querying-a-view"></a>E. Tramite un hint per la query quando si eseguono query di una vista  
 Nell'esempio seguente viene creata una CustomerView denominata e quindi viene usato un hint per la query HASH JOIN in una query che fa riferimento a una vista e una tabella.  
  
```sql
-- Uses AdventureWorks  
  
CREATE VIEW CustomerView  
AS  
SELECT CustomerKey, FirstName, LastName FROM ssawPDW..DimCustomer;  
  
SELECT COUNT (*) FROM dbo.CustomerView a  
INNER JOIN dbo.FactInternetSales b  
ON (a.CustomerKey = b.CustomerKey)  
OPTION (HASH JOIN);  
  
DROP VIEW CustomerView;
```  
  
### <a name="f-query-with-a-subselect-and-a-query-hint"></a>F. Query con un'istruzione sub-SELECT e un hint per la query  
 Nell'esempio seguente viene illustrata una query che contiene un'istruzione sub-SELECT e un hint per la query. L'hint per la query viene applicato a livello globale. Non è consentito aggiungere hint per la query all'istruzione sub-SELECT.  
  
```sql
-- Uses AdventureWorks  
  
CREATE VIEW CustomerView AS  
SELECT CustomerKey, FirstName, LastName FROM ssawPDW..DimCustomer;  
  
SELECT * FROM (  
SELECT COUNT (*) AS a FROM dbo.CustomerView a  
INNER JOIN dbo.FactInternetSales b  
ON ( a.CustomerKey = b.CustomerKey )) AS t  
OPTION (HASH JOIN);  
```  
  
### <a name="g-force-the-join-order-to-match-the-order-in-the-query"></a>G. Forzare l'ordine di join in modo che corrisponda a quello della query  
 Nell'esempio seguente viene usato l'hint FORCE ORDER per forzare il piano di query in modo che venga usato l'ordine di join specificato dalla query. Ciò migliorerà le prestazioni in alcune query ma non in tutte.  
  
```sql
-- Uses AdventureWorks  
  
-- Obtain partition numbers, boundary values, boundary value types, and rows per boundary  
-- for the partitions in the ProspectiveBuyer table of the ssawPDW database.  
SELECT sp.partition_number, prv.value AS boundary_value, lower(sty.name) AS boundary_value_type, sp.rows   
FROM sys.tables st JOIN sys.indexes si ON st.object_id = si.object_id AND si.index_id <2  
JOIN sys.partitions sp ON sp.object_id = st.object_id AND sp.index_id = si.index_id  
JOIN sys.partition_schemes ps ON ps.data_space_id = si.data_space_id   
JOIN sys.partition_range_values prv ON prv.function_id = ps.function_id   
JOIN sys.partition_parameters pp ON pp.function_id = ps.function_id   
JOIN sys.types sty ON sty.user_type_id = pp.user_type_id AND prv.boundary_id = sp.partition_number   
WHERE st.object_id = (SELECT object_id FROM sys.objects WHERE name = 'FactResellerSales')   
ORDER BY sp.partition_number  
OPTION ( FORCE ORDER )  
;  
```  
  
### <a name="h-using-externalpushdown"></a>H. Tramite EXTERNALPUSHDOWN  
 Nell'esempio seguente viene forzata la distribuzione della clausola WHERE per il processo MapReduce nella tabella esterna Hadoop.  
  
```sql
SELECT ID FROM External_Table_AS A   
WHERE ID < 1000000  
OPTION (FORCE EXTERNALPUSHDOWN);  
```  
  
 Nell'esempio seguente viene evitata la distribuzione della clausola WHERE per il processo MapReduce nella tabella esterna Hadoop. Dove viene applicata la clausola WHERE, tutte le righe vengono restituite a PDW.  
  
```sql
SELECT ID FROM External_Table_AS A   
WHERE ID < 10  
OPTION (DISABLE EXTERNALPUSHDOWN);  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Hint &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)   
 [MERGE &#40;Transact-SQL&#41;](../../t-sql/statements/merge-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)  
  
  

