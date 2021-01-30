---
description: SYSUTCDATETIME (Transact-SQL)
title: SYSUTCDATETIME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/01/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SYSUTCDATETIME
- SYSUTCDATETIME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- system time [SQL Server]
- functions [SQL Server], date and time
- time [SQL Server], functions
- date and time [SQL Server], SYSUTCDATETIME
- SYSUTCDATETIME function [SQL Server]
- time [SQL Server], system
ms.assetid: f14fc2cd-9ea8-4daf-88f4-418cf523ab55
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fce96cdf1b28d72a7be47cbdb4dd8293431589a9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99101001"
---
# <a name="sysutcdatetime-transact-sql"></a>SYSUTCDATETIME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce il valore **datetime2** che contiene la data e l'ora del computer in cui è in esecuzione l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La data e l'ora vengono restituite in formato ora UTC (Coordinated Universal Time). La precisione frazionaria dei secondi può essere specificata in un intervallo da 1 a 7 cifre. La precisione predefinita è 7 cifre.  
  
> [!NOTE]  
>  La precisione in secondi frazionari di SYSDATETIME e SYSUTCDATETIME è maggiore di quella di GETDATE e GETUTCDATE. SYSDATETIMEOFFSET include la differenza di fuso orario di sistema. SYSDATETIME, SYSUTCDATETIME e SYSDATETIMEOFFSET possono essere assegnate a una variabile di uno qualsiasi dei tipi di data e ora.  
  
 Per una panoramica di tutti i tipi di dati e funzioni di data e ora [!INCLUDE[tsql](../../includes/tsql-md.md)], vedere [Funzioni e tipi di dati di data e ora](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
SYSUTCDATETIME ( )  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-type"></a>Tipo restituito  
 **datetime2**  
  
## <a name="remarks"></a>Commenti  
 Le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] possono fare riferimento a SYSUTCDATETIME in qualsiasi punto in cui possono fare riferimento a un'espressione **datetime2**.  
  
 SYSUTCDATETIME è una funzione non deterministica. Le viste e le espressioni che fanno riferimento a questa funzione in una colonna non sono indicizzabili.  
  
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
  
### <a name="c-converting-date-and-time-values-to-time"></a>C. Conversione dei valori di data e ora in ore  
 Nell'esempio seguente viene illustrato come convertire i valori di data e ora in un tipo di dati `time`.  
  
 ```sql
DECLARE @DATETIME DATETIME = GetDate();
DECLARE @TIME TIME
SELECT @TIME = CONVERT(time, @DATETIME)
SELECT @TIME AS 'Time', @DATETIME AS 'Date Time'
```
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
Time             Date Time  
13:49:33.6330000 2009-04-22 13:49:33.633
```  
  
## <a name="see-also"></a>Vedere anche  
 [CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [Funzioni e tipi di dati di data e ora &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md)   
 [AT TIME ZONE &#40;Transact-SQL&#41;](../../t-sql/queries/at-time-zone-transact-sql.md)  
  
  


