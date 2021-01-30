---
description: SYSDATETIMEOFFSET (Transact-SQL)
title: SYSDATETIMEOFFSET (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SYSDATETIMEOFFSET_TSQL
- SYSDATETIMEOFFSET
dev_langs:
- TSQL
helpviewer_keywords:
- date and time [SQL Server], SYSDATETIMEOFFSET
- dates [SQL Server], functions
- current date and time [SQL Server]
- functions [SQL Server], time
- system date and time [SQL Server]
- system time [SQL Server]
- SYSDATETIMEOFFSET function [SQL Server]
- functions [SQL Server], date and time
- time [SQL Server], functions
- dates [SQL Server], current date and time
- dates [SQL Server], system date and time
- time zones [SQL Server]
- time [SQL Server], system
ms.assetid: 8423c753-cebe-4edd-871d-0138e092199f
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d7b207ed3e6a3784fe78481be7ac2f1082bad8c3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205870"
---
# <a name="sysdatetimeoffset-transact-sql"></a>SYSDATETIMEOFFSET (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce il valore **datetimeoffset(7)** che contiene la data e l'ora del computer in cui è in esecuzione l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La differenza di fuso orario è inclusa.  
  
 Per una panoramica di tutti i tipi di dati e delle funzioni di data e ora [!INCLUDE[tsql](../../includes/tsql-md.md)], vedere [Funzioni e tipi di dati di data e ora &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
SYSDATETIMEOFFSET ( )  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-type"></a>Tipo restituito  
 **datetimeoffset(7)**  
  
## <a name="remarks"></a>Osservazioni  
 Le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] possono fare riferimento a SYSDATETIMEOFFSET ovunque possano fare riferimento a un'espressione **datetimeoffset**.  
  
 SYSDATETIMEOFFSET è una funzione non deterministica. Le viste e le espressioni che fanno riferimento a questa funzione in una colonna non sono indicizzabili.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ottiene i valori di data e ora tramite l'API Windows GetSystemTimeAsFileTime(). L'accuratezza dipende dall'hardware e dalla versione di Windows del computer in cui è in esecuzione l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La precisione di tale API è fissata sul valore di 100 nanosecondi. L'accuratezza può essere determinata usando l'API Windows GetSystemTimeAdjustment().  
  
## <a name="examples"></a>Esempi  
 Negli esempi seguenti sono utilizzate le sei funzioni di sistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che restituiscono data e ora corrente per restituire la data, l'ora o entrambe. I valori sono restituiti in serie. Pertanto, i secondi frazionari potrebbero essere diversi.  
  
### <a name="a-showing-the-formats-that-are-returned-by-the-date-and-time-functions"></a>R. Visualizzazione dei formati restituiti dalle funzioni di data e ora  
 Nell'esempio seguente vengono illustrati i diversi formati restituiti dalle funzioni di data e ora.  
  
```sql
SELECT SYSDATETIME() AS [SYSDATETIME()]  
    ,SYSDATETIMEOFFSET() AS [SYSDATETIMEOFFSET()]  
    ,SYSUTCDATETIME() AS [SYSUTCDATETIME()]  
    ,CURRENT_TIMESTAMP AS [CURRENT_TIMESTAMP]  
    ,GETDATE() AS [GETDATE()]  
    ,GETUTCDATE() AS [GETUTCDATE()];  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
SYSDATETIME()      2007-04-30 13:10:02.0474381
SYSDATETIMEOFFSET()2007-04-30 13:10:02.0474381 -07:00
SYSUTCDATETIME()   2007-04-30 20:10:02.0474381
CURRENT_TIMESTAMP  2007-04-30 13:10:02.047
GETDATE()          2007-04-30 13:10:02.047
GETUTCDATE()       2007-04-30 20:10:02.047
```  
  
### <a name="b-converting-date-and-time-to-date"></a>B. Conversione di data e ora in una data  
 Nell'esempio seguente viene illustrato come convertire i valori di data e ora in un tipo di dati `date`.  
  
```sql
SELECT CONVERT (date, SYSDATETIME())  
    ,CONVERT (date, SYSDATETIMEOFFSET())  
    ,CONVERT (date, SYSUTCDATETIME())  
    ,CONVERT (date, CURRENT_TIMESTAMP)  
    ,CONVERT (date, GETDATE())  
    ,CONVERT (date, GETUTCDATE());  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
2007-04-30
2007-04-30
2007-04-30
2007-04-30
2007-04-30
2007-04-30
```  
  
### <a name="c-converting-date-and-time-to-times"></a>C. Conversione di data e ora in ore  
 Nell'esempio seguente viene illustrato come convertire i valori di data e ora in un tipo di dati `time`.  
  
```sql
SELECT CONVERT (time, SYSDATETIME()) AS [SYSDATETIME()]  
    ,CONVERT (time, SYSDATETIMEOFFSET()) AS [SYSDATETIMEOFFSET()]  
    ,CONVERT (time, SYSUTCDATETIME()) AS [SYSUTCDATETIME()]  
    ,CONVERT (time, CURRENT_TIMESTAMP) AS [CURRENT_TIMESTAMP]  
    ,CONVERT (time, GETDATE()) AS [GETDATE()]  
    ,CONVERT (time, GETUTCDATE()) AS [GETUTCDATE()];  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
SYSDATETIME()      13:18:45.3490361
SYSDATETIMEOFFSET()13:18:45.3490361
SYSUTCDATETIME()   20:18:45.3490361
CURRENT_TIMESTAMP  13:18:45.3470000
GETDATE()          13:18:45.3470000
GETUTCDATE()       20:18:45.3470000
```  
  
## <a name="see-also"></a>Vedere anche  
 [CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [Funzioni e tipi di dati di data e ora &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md)  
  
  

