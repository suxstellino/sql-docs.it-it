---
description: Operatori sui set - UNION (Transact-SQL)
title: UNION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- UNION
- UNION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- UNION queries
- combining query results
- UNION operator [SQL Server]
ms.assetid: 607c296f-8a6a-49bc-975a-b8d0c0914df7
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 980dfe20df5c3909e8b35a6e5b460db2568b0fe3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097091"
---
# <a name="set-operators---union-transact-sql"></a>Operatori sui set - UNION (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Concatena i risultati di due query in un unico set di risultati. È possibile controllare se il set di risultati include righe duplicate:

- **UNION ALL**: include i duplicati.
- **UNION**: esclude i duplicati.

Un'operazione **UNION** è diversa da un **[JOIN](../queries/from-transact-sql.md)** :

- **UNION** concatena i set di risultati di due query. **UNION** tuttavia non crea singole righe da colonne raccolte da due tabelle.
- Un **JOIN** confronta le colonne di due tabelle, per creare righe di risultati costituite dalle colonne di due tabelle.
  
Di seguito sono riportate le regole di base per la combinazione dei set di risultati di due query tramite l'istruzione **UNION**:  
  
-   Tutte le query devono includere lo stesso numero di colonne nello stesso ordine.  
  
-   I tipi di dati devono essere compatibili.  
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
{ <query_specification> | ( <query_expression> ) }   
{ UNION [ ALL ]   
  { <query_specification> | ( <query_expression> ) } 
  [ ...n ] }
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
\<query_specification> | ( \<query_expression> ) È una specifica di query o un'espressione di query che restituisce i dati da combinare con i dati di un'altra specifica di query o un'espressione di query. Le definizioni delle colonne di un'operazione UNION non devono essere necessariamente identiche, ma devono essere compatibili tramite una conversione implicita. Se i tipi di dati sono diversi, il tipo di dati risultante viene definito in base alle regole valide per la [precedenza dei tipi di dati](../../t-sql/data-types/data-type-precedence-transact-sql.md). Quando i tipi sono gli stessi ma differiscono per precisione, scala o lunghezza, il risultato viene determinato in base alle stesse regole previste per la combinazione di espressioni. Per altre informazioni, vedere [Precisione, scala e lunghezza (Transact-SQL)](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).  
  
Le colonne con tipo di dati **xml** devono essere equivalenti. Tutte le colonne devono essere tipizzate in un XML Schema oppure senza tipo. In caso di colonne tipizzate, esse devono essere tipizzate nella stessa raccolta di XML Schema.  
  
UNION  
Specifica che più set di risultati devono essere combinati e restituiti come singolo set di risultati.  
  
ALL  
Incorpora tutte le righe nei risultati, inclusi i duplicati. Se viene omesso, le righe duplicate vengono rimosse.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-a-simple-union"></a>R. Utilizzo di un semplice operatore UNION  
Nell'esempio seguente il set di risultati include il contenuto delle colonne `ProductModelID` e `Name` di entrambe le tabelle `ProductModel` e `Gloves`.  
 
```sql
-- Uses AdventureWorks  
  
IF OBJECT_ID ('dbo.Gloves', 'U') IS NOT NULL  
DROP TABLE dbo.Gloves;  
GO  
-- Create Gloves table.  
SELECT ProductModelID, Name  
INTO dbo.Gloves  
FROM Production.ProductModel  
WHERE ProductModelID IN (3, 4);  
GO  
  
-- Here is the simple union.  
-- Uses AdventureWorks  
  
SELECT ProductModelID, Name  
FROM Production.ProductModel  
WHERE ProductModelID NOT IN (3, 4)  
UNION  
SELECT ProductModelID, Name  
FROM dbo.Gloves  
ORDER BY Name;  
GO  
```  
  
### <a name="b-using-select-into-with-union"></a>B. Utilizzo di SELECT INTO con UNION  
Nell'esempio seguente la clausola `INTO` nella seconda istruzione `SELECT` specifica che la tabella denominata `ProductResults` contiene il set di risultati finale ottenuto con l'unione delle colonne selezionate delle tabelle `ProductModel` e `Gloves`. La tabella `Gloves` viene creata nella prima istruzione `SELECT`.  
  
```sql
-- Uses AdventureWorks  
  
IF OBJECT_ID ('dbo.ProductResults', 'U') IS NOT NULL  
DROP TABLE dbo.ProductResults;  
GO  
IF OBJECT_ID ('dbo.Gloves', 'U') IS NOT NULL  
DROP TABLE dbo.Gloves;  
GO  
-- Create Gloves table.  
SELECT ProductModelID, Name  
INTO dbo.Gloves  
FROM Production.ProductModel  
WHERE ProductModelID IN (3, 4);  
GO  
  
-- Uses AdventureWorks  
  
SELECT ProductModelID, Name  
INTO dbo.ProductResults  
FROM Production.ProductModel  
WHERE ProductModelID NOT IN (3, 4)  
UNION  
SELECT ProductModelID, Name  
FROM dbo.Gloves;  
GO  
  
SELECT ProductModelID, Name   
FROM dbo.ProductResults;  
```  
  
### <a name="c-using-union-of-two-select-statements-with-order-by"></a>C. Utilizzo dell'operatore UNION in due istruzioni SELECT con la clausola ORDER BY  
L'ordine di alcuni parametri utilizzati con la clausola UNION è importante. Nell'esempio seguente vengono illustrati l'utilizzo errato e quello corretto di `UNION` in due istruzioni `SELECT` in cui una colonna deve essere rinominata nell'output.  
  
```sql
-- Uses AdventureWorks  
  
IF OBJECT_ID ('dbo.Gloves', 'U') IS NOT NULL  
DROP TABLE dbo.Gloves;  
GO  
-- Create Gloves table.  
SELECT ProductModelID, Name  
INTO dbo.Gloves  
FROM Production.ProductModel  
WHERE ProductModelID IN (3, 4);  
GO  
  
/* INCORRECT */  
-- Uses AdventureWorks  
  
SELECT ProductModelID, Name  
FROM Production.ProductModel  
WHERE ProductModelID NOT IN (3, 4)  
ORDER BY Name  
UNION  
SELECT ProductModelID, Name  
FROM dbo.Gloves;  
GO  
  
/* CORRECT */  
-- Uses AdventureWorks  
  
SELECT ProductModelID, Name  
FROM Production.ProductModel  
WHERE ProductModelID NOT IN (3, 4)  
UNION  
SELECT ProductModelID, Name  
FROM dbo.Gloves  
ORDER BY Name;  
GO  
```  
  
### <a name="d-using-union-of-three-select-statements-to-show-the-effects-of-all-and-parentheses"></a>D. Utilizzo dell'operatore UNION in tre istruzioni SELECT per illustrare gli effetti dell'opzione ALL e delle parentesi  
Negli esempi seguenti viene utilizzato l'operatore `UNION` per combinare i risultati di tre tabelle contenenti 5 righe di dati identiche. Nel primo esempio viene utilizzato `UNION ALL` per mostrare i record duplicati e vengono restituite tutte le 15 righe. Nel secondo esempio l'operatore `UNION` viene utilizzato senza l'opzione `ALL` per eliminare le righe duplicate dai risultati combinati delle tre istruzioni `SELECT` e vengono restituite 5 righe.  
  
Il terzo esempio usa `ALL` con il primo operatore `UNION` e il secondo operatore `UNION`, che non usa `ALL`, viene racchiuso tra parentesi. Il secondo operatore `UNION` viene elaborato per primo in quanto è racchiuso tra parentesi e restituisce cinque righe perché l'opzione `ALL` non viene usata e i duplicati vengono rimossi. Queste 5 righe vengono combinate con i risultati della prima istruzione `SELECT` mediante le parole chiave `UNION ALL`. Questo esempio non rimuove i duplicati tra i due set di cinque righe. Il risultato finale include 10 righe.  
  
```sql
-- Uses AdventureWorks  
  
IF OBJECT_ID ('dbo.EmployeeOne', 'U') IS NOT NULL  
DROP TABLE dbo.EmployeeOne;  
GO  
IF OBJECT_ID ('dbo.EmployeeTwo', 'U') IS NOT NULL  
DROP TABLE dbo.EmployeeTwo;  
GO  
IF OBJECT_ID ('dbo.EmployeeThree', 'U') IS NOT NULL  
DROP TABLE dbo.EmployeeThree;  
GO  
  
SELECT pp.LastName, pp.FirstName, e.JobTitle   
INTO dbo.EmployeeOne  
FROM Person.Person AS pp JOIN HumanResources.Employee AS e  
ON e.BusinessEntityID = pp.BusinessEntityID  
WHERE LastName = 'Johnson';  
GO  
SELECT pp.LastName, pp.FirstName, e.JobTitle   
INTO dbo.EmployeeTwo  
FROM Person.Person AS pp JOIN HumanResources.Employee AS e  
ON e.BusinessEntityID = pp.BusinessEntityID  
WHERE LastName = 'Johnson';  
GO  
SELECT pp.LastName, pp.FirstName, e.JobTitle   
INTO dbo.EmployeeThree  
FROM Person.Person AS pp JOIN HumanResources.Employee AS e  
ON e.BusinessEntityID = pp.BusinessEntityID  
WHERE LastName = 'Johnson';  
GO  
-- Union ALL  
SELECT LastName, FirstName, JobTitle  
FROM dbo.EmployeeOne  
UNION ALL  
SELECT LastName, FirstName ,JobTitle  
FROM dbo.EmployeeTwo  
UNION ALL  
SELECT LastName, FirstName,JobTitle   
FROM dbo.EmployeeThree;  
GO  
  
SELECT LastName, FirstName,JobTitle  
FROM dbo.EmployeeOne  
UNION   
SELECT LastName, FirstName, JobTitle   
FROM dbo.EmployeeTwo  
UNION   
SELECT LastName, FirstName, JobTitle   
FROM dbo.EmployeeThree;  
GO  
  
SELECT LastName, FirstName,JobTitle   
FROM dbo.EmployeeOne  
UNION ALL  
(  
SELECT LastName, FirstName, JobTitle   
FROM dbo.EmployeeTwo  
UNION  
SELECT LastName, FirstName, JobTitle   
FROM dbo.EmployeeThree  
);  
GO  
  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-a-simple-union"></a>E. Utilizzo di un semplice operatore UNION  
Nell'esempio seguente il set di risultati include il contenuto delle colonne `CustomerKey` di entrambe le tabelle `FactInternetSales` e `DimCustomer`. Poiché la parola chiave ALL non viene usata, i duplicati vengono esclusi dai risultati.  
  
```sql
-- Uses AdventureWorks  
  
SELECT CustomerKey   
FROM FactInternetSales    
UNION   
SELECT CustomerKey   
FROM DimCustomer   
ORDER BY CustomerKey;  
```  
  
### <a name="f-using-union-of-two-select-statements-with-order-by"></a>F. Utilizzo dell'operatore UNION in due istruzioni SELECT con la clausola ORDER BY  
 Quando un'istruzione SELECT in un'istruzione UNION include una clausola ORDER BY, tale clausola deve essere inserita dopo tutte le istruzioni SELECT. Nell'esempio seguente vengono illustrati l'uso errato e quello corretto di `UNION` in due istruzioni `SELECT` in cui una colonna è ordinata con ORDER BY.  
  
```sql
-- Uses AdventureWorks  
  
-- INCORRECT  
SELECT CustomerKey   
FROM FactInternetSales    
ORDER BY CustomerKey  
UNION   
SELECT CustomerKey   
FROM DimCustomer  
ORDER BY CustomerKey;  
  
-- CORRECT   
USE AdventureWorksPDW2012;  
  
SELECT CustomerKey   
FROM FactInternetSales    
UNION   
SELECT CustomerKey   
FROM DimCustomer   
ORDER BY CustomerKey;  
```  
  
### <a name="g-using-union-of-two-select-statements-with-where-and-order-by"></a>G. Uso dell'operatore UNION in due istruzioni SELECT con WHERE e ORDER BY  
Nell'esempio seguente vengono illustrati l'uso errato e quello corretto di `UNION` in due istruzioni `SELECT` in cui sono necessarie le clausole WHERE e ORDER BY.  
  
```sql
-- Uses AdventureWorks  
  
-- INCORRECT   
SELECT CustomerKey   
FROM FactInternetSales   
WHERE CustomerKey >= 11000  
ORDER BY CustomerKey   
UNION   
SELECT CustomerKey   
FROM DimCustomer   
ORDER BY CustomerKey;  
  
-- CORRECT  
USE AdventureWorksPDW2012;  
  
SELECT CustomerKey   
FROM FactInternetSales   
WHERE CustomerKey >= 11000  
UNION   
SELECT CustomerKey   
FROM DimCustomer   
ORDER BY CustomerKey;  
```  
  
### <a name="h-using-union-of-three-select-statements-to-show-effects-of-all-and-parentheses"></a>H. Uso dell'operatore UNION in tre istruzioni SELECT per illustrare gli effetti dell'opzione ALL e delle parentesi  
L'esempio seguente usa `UNION` per combinare i risultati della **stessa tabella** per illustrare gli effetti dell'opzione ALL e delle parentesi quando si usa `UNION`.  
  
Il primo esempio usa `UNION ALL` per visualizzare i record duplicati e restituisce ogni riga nella tabella di origine tre volte. Nel secondo esempio l'operatore `UNION` viene usato senza l'opzione `ALL` per eliminare le righe duplicate dai risultati combinati delle tre istruzioni `SELECT` e vengono restituite solo le righe non duplicate dalla tabella di origine.  
  
Il terzo esempio usa `ALL` con il primo operatore `UNION` e il secondo operatore `UNION`, che non usa`ALL`, viene racchiuso tra parentesi. Il secondo operatore `UNION` viene elaborato per primo perché è racchiuso tra parentesi. Restituisce solo le righe non duplicate della tabella perché l'opzione `ALL` non viene usata e i duplicati vengono rimossi. Queste righe vengono combinate con i risultati della prima istruzione `SELECT` mediante le parole chiave `UNION ALL`. Questo esempio non rimuove i duplicati tra i due set.  
  
```sql
-- Uses AdventureWorks  
  
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION ALL   
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION ALL   
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer;  
  
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION   
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION   
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer;  
  
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION ALL  
(  
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION   
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
);  
```  
  
## <a name="see-also"></a>Vedere anche  
[SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
[Esempi di istruzioni SELECT (Transact-SQL)](../../t-sql/queries/select-examples-transact-sql.md)  
