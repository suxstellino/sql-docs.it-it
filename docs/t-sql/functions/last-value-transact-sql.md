---
description: LAST_VALUE (Transact-SQL)
title: LAST_VALUE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/22/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- LAST_VALUE
- LAST_VALUE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- analytic functions, LAST_VALUE
- LAST_VALUE function
ms.assetid: fd833e34-8092-42b7-80fc-95ca6b0eab6b
author: cawrites
ms.author: chadam
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8121b8cb50ff42114196973af17efa42992e16b4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343570"
---
# <a name="last_value-transact-sql"></a>LAST_VALUE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Restituisce l'ultimo valore in un set ordinato di valori.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  

```syntaxsql 
LAST_VALUE ( [ scalar_expression ] )  [ IGNORE NULLS | RESPECT NULLS ]
    OVER ( [ partition_by_clause ] order_by_clause rows_range_clause )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *scalar_expression*  
 Valore da restituire. *scalar_expression* può essere una colonna, una sottoquery o un'altra espressione che restituisce un solo valore. Altre funzioni analitiche non sono consentite.  
  
 [ IGNORE NULLS | RESPECT NULLS ]     
 **Si applica a**: SQL Edge di Azure

 IGNORE NULLS: ignora i valori null nel set di dati quando si calcola l'ultimo valore in una partizione.     
 RESPECT NULLS: rispetta i valori null nel set di dati quando si calcola l'ultimo valore in una partizione.     
 
  Per altre informazioni, vedere [Inserimento di valori mancanti](/azure/azure-sql-edge/imputing-missing-values/).
  
 OVER **(** [ *partition_by_clause* ] *order_by_clause* [ *rows_range_clause* ] **)**  
 *partition_by_clause* suddivide il set di risultati generato dalla clausola FROM in partizioni alle quali viene applicata la funzione. Se non specificato, la funzione tratta tutte le righe del set di risultati della query come un unico gruppo.  
  
 *order_by_clause* determina l'ordine dei dati prima che venga applicata la funzione. *order_by_clause* è obbligatorio. *rows_range_clause* limita ulteriormente le righe all'interno della partizione specificando i punti iniziali e finali. Per altre informazioni, vedere [Clausola OVER - &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
## <a name="return-types"></a>Tipi restituiti  
 Tipo identico a *scalar_expression*.  
  
## <a name="general-remarks"></a>Osservazioni generali  
 LAST_VALUE è non deterministico. Per altre informazioni, vedere [Funzioni deterministiche e non deterministiche](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-last_value-over-partitions"></a>R. Utilizzo di LAST_VALUE su partizioni  
 Nell'esempio seguente viene restituita la data di assunzione dell'ultimo dipendente in ogni reparto per lo stipendio specificato (valore Rate). La clausola PARTITION BY suddivide i dipendenti in base al reparto e la funzione LAST_VALUE viene applicata indipendentemente a ogni partizione. La clausola ORDER BY specificata nella clausola OVER determina l'ordine logico in cui la funzione FIRST_VALUE viene applicata alle righe in ogni partizione.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Department, LastName, Rate, HireDate,   
    LAST_VALUE(HireDate) OVER (PARTITION BY Department ORDER BY Rate) AS LastValue  
FROM HumanResources.vEmployeeDepartmentHistory AS edh  
INNER JOIN HumanResources.EmployeePayHistory AS eph    
    ON eph.BusinessEntityID = edh.BusinessEntityID  
INNER JOIN HumanResources.Employee AS e  
    ON e.BusinessEntityID = edh.BusinessEntityID  
WHERE Department IN (N'Information Services',N'Document Control');   
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
  
Department                  LastName                Rate         HireDate     LastValue  
--------------------------- ----------------------- ------------ ----------   ----------  
Document Control            Chai                    10.25        2003-02-23   2003-03-13  
Document Control            Berge                   10.25        2003-03-13   2003-03-13  
Document Control            Norred                  16.8269      2003-04-07   2003-01-17  
Document Control            Kharatishvili           16.8269      2003-01-17   2003-01-17  
Document Control            Arifin                  17.7885      2003-02-05   2003-02-05  
Information Services        Berg                    27.4038      2003-03-20   2003-01-24  
Information Services        Meyyappan               27.4038      2003-03-07   2003-01-24  
Information Services        Bacon                   27.4038      2003-02-12   2003-01-24  
Information Services        Bueno                   27.4038      2003-01-24   2003-01-24  
Information Services        Sharma                  32.4519      2003-01-05   2003-03-27  
Information Services        Connelly                32.4519      2003-03-27   2003-03-27  
Information Services        Ajenstat                38.4615      2003-02-18   2003-02-23  
Information Services        Wilson                  38.4615      2003-02-23   2003-02-23  
Information Services        Conroy                  39.6635      2003-03-08   2003-03-08  
Information Services        Trenary                 50.4808      2003-01-12   2003-01-12  
  
```  
  
### <a name="b-using-first_value-and-last_value-in-a-computed-expression"></a>B. Utilizzo di FIRST_VALUE e LAST_VALUE in un'espressione calcolata  
 Nell'esempio seguente vengono utilizzate le funzioni FIRST_VALUE e LAST_VALUE nelle espressioni calcolate per mostrare la differenza tra il valore della quota vendite per il trimestre corrente e il primo e ultimo trimestre dell'anno rispettivamente per un numero specificato di dipendenti. La funzione FIRST_VALUE restituisce il valore delle quote vendite per il primo trimestre dell'anno e lo sottrae dal valore delle quote vendite per il trimestre corrente. Viene restituito nella colonna derivata denominata DifferenceFromFirstQuarter. Per il primo trimestre dell'anno il valore della colonna DifferenceFromFirstQuarter è 0. La funzione LAST_VALUE restituisce il valore delle quote vendite per l'ultimo trimestre dell'anno e lo sottrae dal valore delle quote vendite per il trimestre corrente. Viene restituito nella colonna derivata denominata DifferenceFromLastQuarter. Per l'ultimo trimestre dell'anno il valore della colonna DifferenceFromLastQuarter è 0.  
  
 In questo esempio è richiesta la clausola "RANGE BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING" per la restituzione di valori diversi da zero nella colonna DifferenceFromLastQuarter, come illustrato di seguito. L'intervallo predefinito è "RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW". Se nell'esempio si usa l'intervallo predefinito in questione, o se non si include alcun intervallo determinando quindi l'uso dell'intervallo predefinito, nella colonna DifferenceFromLastQuarter verranno restituiti valori pari a zero. Per altre informazioni, vedere [Clausola OVER - &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
```sql  
USE AdventureWorks2012;  
SELECT BusinessEntityID, DATEPART(QUARTER,QuotaDate)AS Quarter, YEAR(QuotaDate) AS SalesYear,   
    SalesQuota AS QuotaThisQuarter,   
    SalesQuota - FIRST_VALUE(SalesQuota)   
        OVER (PARTITION BY BusinessEntityID, YEAR(QuotaDate)   
              ORDER BY DATEPART(QUARTER,QuotaDate) ) AS DifferenceFromFirstQuarter,   
    SalesQuota - LAST_VALUE(SalesQuota)   
        OVER (PARTITION BY BusinessEntityID, YEAR(QuotaDate)   
              ORDER BY DATEPART(QUARTER,QuotaDate)   
              RANGE BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING ) AS DifferenceFromLastQuarter   
FROM Sales.SalesPersonQuotaHistory   
WHERE YEAR(QuotaDate) > 2005   
AND BusinessEntityID BETWEEN 274 AND 275   
ORDER BY BusinessEntityID, SalesYear, Quarter;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
BusinessEntityID Quarter     SalesYear   QuotaThisQuarter      DifferenceFromFirstQuarter DifferenceFromLastQuarter  
---------------- ----------- ----------- --------------------- --------------------------- -----------------------  
274              1           2006        91000.00              0.00                        -63000.00  
274              2           2006        140000.00             49000.00                    -14000.00  
274              3           2006        70000.00              -21000.00                   -84000.00  
274              4           2006        154000.00             63000.00                    0.00  
274              1           2007        107000.00             0.00                        -9000.00  
274              2           2007        58000.00              -49000.00                   -58000.00  
274              3           2007        263000.00             156000.00                   147000.00  
274              4           2007        116000.00             9000.00                     0.00  
274              1           2008        84000.00              0.00                        -103000.00  
274              2           2008        187000.00             103000.00                   0.00  
275              1           2006        502000.00             0.00                        -822000.00  
275              2           2006        550000.00             48000.00                    -774000.00  
275              3           2006        1429000.00            927000.00                   105000.00  
275              4           2006        1324000.00            822000.00                   0.00  
275              1           2007        729000.00             0.00                        -489000.00  
275              2           2007        1194000.00            465000.00                   -24000.00  
275              3           2007        1575000.00            846000.00                   357000.00  
275              4           2007        1218000.00            489000.00                   0.00  
275              1           2008        849000.00             0.00                        -20000.00  
275              2           2008        869000.00             20000.00                    0.00  
  
(20 row(s) affected)  
  
```  
  
## <a name="see-also"></a>Vedere anche  

 [First_Value &#40;Transact-SQL&#41;](first-value-transact-sql.md)  
