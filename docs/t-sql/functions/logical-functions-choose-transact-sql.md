---
description: Funzioni logiche - CHOOSE (Transact-SQL)
title: CHOOSE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CHOOSE
- CHOOSE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CHOOSE function
ms.assetid: 1c382c83-7500-4bae-bbdc-c1dbebd3d83f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6aad317f1b8677b2c9d1c5de1013df7beb303d7f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208836"
---
# <a name="logical-functions---choose-transact-sql"></a>Funzioni logiche - CHOOSE (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Viene restituito l'elemento in corrispondenza dell'indice specificato di un elenco di valori in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CHOOSE ( index, val_1, val_2 [, val_n ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *index*  
 Espressione intera che rappresenta un indice in base 1 nell'elenco degli elementi che lo seguono.  
  
 Se il tipo di dati numerico del valore dell'indice specificato è diverso da **int**, il valore viene convertito in modo implicito in un tipo di dati integer. Se il valore di indice supera i limiti della matrice di valori, tramite CHOOSE viene restituito Null.  
  
 *val_1...val_n*  
 Elenco di valori delimitati da virgole di qualsiasi tipo di dati.  
  
## <a name="return-types"></a>Tipi restituiti  
 Restituisce il tipo di dati con precedenza maggiore nel set di tipi passato alla funzione. Per altre informazioni, vedere [Precedenza dei tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
  
## <a name="remarks"></a>Commenti  
 CHOOSE funziona come un indice in una matrice, dove la matrice è costituita dagli argomenti che seguono l'argomento dell'indice. L'argomento dell'indice determina quali tra i valori seguenti saranno restituiti.  
  
## <a name="examples"></a>Esempi  

### <a name="a-simple-choose-example"></a>R. Esempio di CHOOSE semplice

 Nell'esempio seguente viene restituito il terzo elemento dell'elenco di valori forniti.  
 
```sql 
SELECT CHOOSE ( 3, 'Manager', 'Director', 'Developer', 'Tester' ) AS Result;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
-------------  
Developer  
  
(1 row(s) affected)  
```  

### <a name="b-simple-choose-example-based-on-column"></a>B. Esempio di CHOOSE semplice basato su una colonna

 Nell'esempio seguente viene restituita una stringa di caratteri semplice in base al valore della colonna `ProductCategoryID`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT ProductCategoryID, CHOOSE (ProductCategoryID, 'A','B','C','D','E') AS Expression1  
FROM Production.ProductCategory;  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
ProductCategoryID Expression1  
----------------- -----------  
3                 C  
1                 A  
2                 B  
4                 D  
  
(4 row(s) affected)  
  
```  

### <a name="c-choose-in-combination-with-month"></a>C. CHOOSE in combinazione con MONTH
  
 Nell'esempio seguente viene restituita la stagione in cui un dipendente è stato assunto. La funzione MONTH viene utilizzata per restituire il valore del mese della colonna `HireDate`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT JobTitle, HireDate, CHOOSE(MONTH(HireDate),'Winter','Winter', 'Spring','Spring','Spring','Summer','Summer',   
                                                  'Summer','Autumn','Autumn','Autumn','Winter') AS Quarter_Hired  
FROM HumanResources.Employee  
WHERE  YEAR(HireDate) > 2005  
ORDER BY YEAR(HireDate);  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
JobTitle                                           HireDate   Quarter_Hired  
-------------------------------------------------- ---------- -------------  
Sales Representative                               2006-11-01 Autumn  
European Sales Manager                             2006-05-18 Spring  
Sales Representative                               2006-07-01 Summer  
Sales Representative                               2006-07-01 Summer  
Sales Representative                               2007-07-01 Summer  
Pacific Sales Manager                              2007-04-15 Spring  
Sales Representative                               2007-07-01 Summer  
  
```  
  
## <a name="see-also"></a>Vedere anche  
 [IIF &#40;Transact-SQL&#41;](../../t-sql/functions/logical-functions-iif-transact-sql.md)  
  
  
