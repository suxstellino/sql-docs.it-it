---
description: PERCENTILE_CONT (Transact-SQL)
title: PERCENTILE_CONT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/20/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- PERCENTILE_CONT_TSQL
- PERCENTILE_CONT
dev_langs:
- TSQL
helpviewer_keywords:
- analytic functions, PERCENTILE_CONT
- PERCENTILE_CONT function
ms.assetid: d019419e-5297-4994-97d5-e9c8fc61bbf4
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2e2ab7fa86b8888f105428847bdd751ac749bba5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186060"
---
# <a name="percentile_cont-transact-sql"></a>PERCENTILE_CONT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Calcola un percentile basato su una distribuzione continua del valore della colonna in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il risultato viene interpolato e potrebbe non essere uguale ad alcuno dei valori specifici nella colonna.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
PERCENTILE_CONT ( numeric_literal )   
    WITHIN GROUP ( ORDER BY order_by_expression [ ASC | DESC ] )  
    OVER ( [ <partition_by_clause> ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *numeric_literal*  
 Percentile da calcolare. Il valore deve essere compreso tra 0 e 1.  
  
 WITHIN GROUP **(** ORDER BY *order_by_expression* [ **ASC** | DESC ] **)**  
 Specifica un elenco di valori numerici per ordinare e calcolare il percentile. È consentito un solo *order_by_expression*. L'espressione deve restituire un tipo numerico esatto o approssimato, con nessun altro tipo di dati consentito. I tipi numerici esatti sono **int**, **bigint**, **smallint**, **tinyint**, **numeric**, **bit**, **decimal**, **smallmoney** e **money**. I tipi numerici approssimati sono **float** e **real**. Per impostazione predefinita, l'ordinamento è crescente.  
  
 OVER **(** \<partition_by_clause> **)**  
 Suddivide il set di risultati generato dalla clausola FROM in partizioni alle quali viene applicata la funzione di percentile. Per altre informazioni, vedere [Clausola OVER - &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md). Non è possibile specificare \<ORDER BY clause> e \<rows or range clause> della sintassi OVER in una funzione PERCENTILE_CONT.  
  
## <a name="return-types"></a>Tipi restituiti  
 **float(53)**  
  
## <a name="compatibility-support"></a>Informazioni sulla compatibilità  
 Se il valore del livello di compatibilità è 110 o superiore, WITHIN GROUP è una parola chiave riservata. Per altre informazioni, vedere [Livello di compatibilità ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
  
## <a name="general-remarks"></a>Osservazioni generali  
 Tutti i valori Null nel set di dati vengono ignorati.  
  
 PERCENTILE_CONT è non deterministico. Per altre informazioni, vedere [Funzioni deterministiche e non deterministiche](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-basic-syntax-example"></a>R. Esempio della sintassi di base  
 Nell'esempio seguente vengono utilizzate le funzioni PERCENTILE_CONT e PERCENTILE_DISC per trovare lo stipendio medio del dipendente in ogni reparto. È possibile che queste funzioni non restituiscano lo stesso valore. PERCENTILE_CONT esegue l'interpolazione del valore appropriato, che può esistere o meno nel set di dati, mentre PERCENTILE_DISC restituisce sempre un valore effettivo dal set.  
  
```sql  
USE AdventureWorks2012;  
  
SELECT DISTINCT Name AS DepartmentName  
      ,PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY ph.Rate)   
                            OVER (PARTITION BY Name) AS MedianCont  
      ,PERCENTILE_DISC(0.5) WITHIN GROUP (ORDER BY ph.Rate)   
                            OVER (PARTITION BY Name) AS MedianDisc  
FROM HumanResources.Department AS d  
INNER JOIN HumanResources.EmployeeDepartmentHistory AS dh   
    ON dh.DepartmentID = d.DepartmentID  
INNER JOIN HumanResources.EmployeePayHistory AS ph  
    ON ph.BusinessEntityID = dh.BusinessEntityID  
WHERE dh.EndDate IS NULL;  
```  
  
 Set di risultati parziale:  
  
 ```
DepartmentName        MedianCont    MedianDisc
--------------------   ----------   ----------
Document Control       16.8269      16.8269
Engineering            34.375       32.6923
Executive              54.32695     48.5577
Human Resources        17.427850    16.5865
```  

### <a name="b-basic-syntax-example"></a>B. Esempio della sintassi di base  
 Nell'esempio seguente vengono utilizzate le funzioni PERCENTILE_CONT e PERCENTILE_DISC per trovare lo stipendio medio del dipendente in ogni reparto. È possibile che queste funzioni non restituiscano lo stesso valore. PERCENTILE_CONT esegue l'interpolazione del valore appropriato, che può esistere o meno nel set di dati, mentre PERCENTILE_DISC restituisce sempre un valore effettivo dal set.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT DISTINCT DepartmentName  
,PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY BaseRate)  
    OVER (PARTITION BY DepartmentName) AS MedianCont  
,PERCENTILE_DISC(0.5) WITHIN GROUP (ORDER BY BaseRate)  
    OVER (PARTITION BY DepartmentName) AS MedianDisc  
FROM dbo.DimEmployee; 
```  
  
 Set di risultati parziale:  
  
 ```
DepartmentName        MedianCont    MedianDisc
--------------------   ----------   ----------
Document Control       16.826900    16.8269
Engineering            34.375000    32.6923
Human Resources        17.427850    16.5865
Shipping and Receiving 9.250000      9.0000
```  
  
## <a name="see-also"></a>Vedere anche  
 [PERCENTILE_DISC &#40;Transact-SQL&#41;](../../t-sql/functions/percentile-disc-transact-sql.md)  
  
 
