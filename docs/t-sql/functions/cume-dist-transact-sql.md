---
description: CUME_DIST (Transact-SQL)
title: CUME_DIST (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CUME_DIST
- CUME_DIST_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CUME_DIST function
- analytic functions, CUME_DIST
ms.assetid: 491b07f3-9ffd-4cdd-93e5-5abb636fc5ef
author: cawrites
ms.author: chadam
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2f28c7c8039602d5892430b1c59a4f39e026a954
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184196"
---
# <a name="cume_dist-transact-sql"></a>CUME_DIST (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], questa funzione calcola la distribuzione cumulativa di un valore in un gruppo di valori, ovvero `CUME_DIST` calcola la posizione relativa di un valore specificato in un gruppo di valori. Presumendo un ordine crescente, la funzione `CUME_DIST` di un valore nella riga _r_ viene definito come il numero di righe con valori inferiori o uguali a tale valore nella riga _r_, diviso per il numero di righe valutato nella partizione o nel set di risultati della query. `CUME_DIST` è simile alla funzione `PERCENT_RANK`.
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CUME_DIST( )  
    OVER ( [ partition_by_clause ] order_by_clause )  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
OVER **(** [ _partition\_by\_clause_ ] _order\_by\_clause_)  

_partition\_by\_clause_ divide il set di risultati della clausola FROM in partizioni, alle quali viene applicata la funzione. Se l'argomento _partition\_by\_clause_ non viene specificato, `CUME_DIST` considera tutte le righe del set di risultati della query come un unico gruppo. _order\_by\_clause_ determina l'ordine logico in cui viene eseguita l'operazione. `CUME_DIST` richiede _order\_by\_clause_. `CUME_DIST` non accetterà l'elemento \<rows or range clause> della sintassi OVER. Per altre informazioni, vedere [Clausola OVER - &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).
  
## <a name="return-types"></a>Tipi restituiti
**float(53)**
  
## <a name="remarks"></a>Osservazioni  
`CUME_DIST` restituisce un intervallo di valori superiori a 0 e inferiori o uguali a 1. I valori restituiscono sempre lo stesso valore di distribuzione cumulativo. `CUME_DIST` include valori NULL per impostazione predefinita e li tratta come i valori minori consentiti.
  
`CUME_DIST` è non deterministico. Per altre informazioni, vedere [Funzioni deterministiche e non deterministiche](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).
  
## <a name="examples"></a>Esempi  
Questo esempio usa la funzione `CUME_DIST` per calcolare il percentile del salario per ogni dipendente all'interno di un reparto specificato. `CUME_DIST` restituisce un valore che rappresenta la percentuale di dipendenti con un salario minore o uguale al dipendente corrente nello stesso reparto. La funzione `PERCENT_RANK` calcola il rango percentile dello stipendio del dipendente in un reparto. Per partizionare le righe del set di risultati in base al reparto, l'esempio specifica il valore _partition\_by\_clause_. La clausola ORDER BY della clausola OVER ordina le righe in ogni partizione in modo logico. La clausola ORDER BY dell'istruzione SELECT determina l'ordine di visualizzazione del set di risultati.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT Department, LastName, Rate,   
       CUME_DIST () OVER (PARTITION BY Department ORDER BY Rate) AS CumeDist,   
       PERCENT_RANK() OVER (PARTITION BY Department ORDER BY Rate ) AS PctRank  
FROM HumanResources.vEmployeeDepartmentHistory AS edh  
    INNER JOIN HumanResources.EmployeePayHistory AS e    
    ON e.BusinessEntityID = edh.BusinessEntityID  
WHERE Department IN (N'Information Services',N'Document Control')   
ORDER BY Department, Rate DESC;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Department             LastName               Rate                  CumeDist               PctRank  
---------------------- ---------------------- --------------------- ---------------------- ----------------------  
Document Control       Arifin                 17.7885               1                      1  
Document Control       Norred                 16.8269               0.8                    0.5  
Document Control       Kharatishvili          16.8269               0.8                    0.5  
Document Control       Chai                   10.25                 0.4                    0  
Document Control       Berge                  10.25                 0.4                    0  
Information Services   Trenary                50.4808               1                      1  
Information Services   Conroy                 39.6635               0.9                    0.888888888888889  
Information Services   Ajenstat               38.4615               0.8                    0.666666666666667  
Information Services   Wilson                 38.4615               0.8                    0.666666666666667  
Information Services   Sharma                 32.4519               0.6                    0.444444444444444  
Information Services   Connelly               32.4519               0.6                    0.444444444444444  
Information Services   Berg                   27.4038               0.4                    0  
Information Services   Meyyappan              27.4038               0.4                    0  
Information Services   Bacon                  27.4038               0.4                    0  
Information Services   Bueno                  27.4038               0.4                    0  
(15 row(s) affected)  
```  
  
## <a name="see-also"></a>Vedere anche
[PERCENT_RANK &#40;Transact-SQL&#41;](../../t-sql/functions/percent-rank-transact-sql.md)
  
  
