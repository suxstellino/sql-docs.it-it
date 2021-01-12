---
description: ISNUMERIC (Transact-SQL)
title: ISNUMERIC (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ISNUMERIC
- ISNUMERIC_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- expressions [SQL Server], valid numeric type
- numeric data
- ISNUMERIC function
- verifying valid numeric type
- valid numeric type [SQL Server]
- checking valid numeric type
ms.assetid: 7aa816de-529a-4f6c-a99f-4d5a9ef599eb
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cc5cee521a7a22429986bc7e6b71d96d729c03ef
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100437"
---
# <a name="isnumeric-transact-sql"></a>ISNUMERIC (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Determina se il tipo di un'espressione è un tipo numerico valido.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql 
ISNUMERIC ( expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *expression*  
 [Espressione](../../t-sql/language-elements/expressions-transact-sql.md) da valutare.  
  
## <a name="return-types"></a>Tipi restituiti  
 **int**  
  
## <a name="remarks"></a>Osservazioni  
 ISNUMERIC restituisce 1 quando l'espressione di input restituisce un tipo di dati numerico valido. In caso contrario, restituisce 0. I [tipi di dati numerici validi](../../t-sql/data-types/numeric-types.md) sono:  

| Area | Tipi di dati numerici |
|-|-|
| [Valori numerici esatti](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md) | **bigint**, **int**, **smallint**, **tinyint**, **bit** |
| [Precisione fissa](../../t-sql/data-types/decimal-and-numeric-transact-sql.md) | **decimal**, **numeric** |
| [Con approssimazione](../../t-sql/data-types/float-and-real-transact-sql.md) | **float**, **real** |
| [Valori monetari](../../t-sql/data-types/money-and-smallmoney-transact-sql.md) | **money**, **smallmoney** |

> [!NOTE]  
> ISNUMERIC restituisce 1 per alcuni caratteri non numerici, ad esempio i segni più (+) e meno (-) e i simboli di valuta validi come il segno di dollaro ($). Per un elenco completo di simboli di valuta, vedere [money e smallmoney &#40;Transact-SQL&#41;](../../t-sql/data-types/money-and-smallmoney-transact-sql.md).  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente la funzione `ISNUMERIC` viene utilizzata per restituire tutti i codici postali che non sono valori numerici.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT City, PostalCode  
FROM Person.Address   
WHERE ISNUMERIC(PostalCode) <> 1;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 Nell'esempio seguente la funzione `ISNUMERIC` viene utilizzata per restituire tutti i codici postali che non sono valori numerici.  
  
```sql
USE master;  
GO  
SELECT name, ISNUMERIC(name) AS IsNameANumber, database_id, ISNUMERIC(database_id) AS IsIdANumber   
FROM sys.databases;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche

- [Espressioni &#40; Transact-SQL &#41;](../../t-sql/language-elements/expressions-transact-sql.md)
- [Funzioni di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)
- [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)
