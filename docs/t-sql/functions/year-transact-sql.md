---
description: YEAR (Transact-SQL)
title: YEAR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- YEAR
- YEAR_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- dates [SQL Server], years
- date and time [SQL Server], YEAR
- functions [SQL Server], date and time
- YEAR function [SQL Server]
- dateparts [SQL Server], year
ms.assetid: 74aa7ccc-8575-4018-80cf-14aeca379687
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e54ab38d0b5564cbedff0c1d3d3509ed1f4e2e43
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755781"
---
# <a name="year-transact-sql"></a>YEAR (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce un valore integer che rappresenta l'anno nel valore *date* specificato.  
  
 Per una panoramica di tutti i tipi di dati e delle funzioni di data e ora [!INCLUDE[tsql](../../includes/tsql-md.md)], vedere [Funzioni e tipi di dati di data e ora &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
YEAR ( date )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *date*  
 Espressione che può essere risolta in un valore **time**, **date**, **smalldatetime**, **datetime**, **datetime2** o **datetimeoffset**. L'argomento *date* può essere costituito da un'espressione, un'espressione di colonna, una variabile definita dall'utente o un valore letterale stringa.  
  
## <a name="return-types"></a>Tipi restituiti  
 **int**  
  
## <a name="return-value"></a>Valore restituito  
 YEAR restituisce lo stesso valore di [DATEPART](../../t-sql/functions/datepart-transact-sql.md) (**year**, *date*).  
  
 Se *date* contiene solo la parte dell'ora, il valore restituito è 1900, l'anno di base.  
  
## <a name="examples"></a>Esempi  
 L'istruzione seguente restituisce `2010`. Si tratta del numero dell'anno.  
  
```sql  
SELECT YEAR('2010-04-30T01:01:01.1234567-07:00');  
```  
  
 L'istruzione seguente restituisce `1900, 1, 1`. L'argomento di *date* è il numero `0`. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], `0` viene interpretato come 1 gennaio 1900.  
  
```sql 
SELECT YEAR(0), MONTH(0), DAY(0);  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 L'istruzione seguente restituisce `1900, 1, 1`. L'argomento di *date* è il numero `0`. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], `0` viene interpretato come 1 gennaio 1900.  
  
```sql  
SELECT TOP 1 YEAR(0), MONTH(0), DAY(0);  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
  
  

