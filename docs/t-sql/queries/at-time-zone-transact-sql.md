---
description: AT TIME ZONE (Transact-SQL)
title: AT TIME ZONE (Transact-SQL)
ms.date: 06/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.custom: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- AT TIME ZONE
- AT_TIME_ZONE_TSQL
helpviewer_keywords:
- AT TIME ZONE function
ms.assetid: 311f682f-7f1b-43b6-9ea0-24e36b64f73a
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current||=azure-sqldw-latest||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: 391e045d078b2735411c51db73948e416f9e4ea5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159683"
---
# <a name="at-time-zone-transact-sql"></a>AT TIME ZONE (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

Converte un elemento *inputdate* nel valore *datetimeoffset* corrispondente nel fuso orario di destinazione. Quando *inputdate* viene specificato senza informazioni relative alla differenza, la funzione applica la differenza del fuso orario presumendo che *inputdate* sia nel fuso orario di destinazione. Se *inputdate* viene specificato come un valore *datetimeoffset*, la clausola **AT TIME ZONE** lo converte nel fuso orario di destinazione usando le regole di conversione del fuso orario.  

L'implementazione di **AT TIME ZONE** si basa su un meccanismo di Windows per convertire i valori **datetime** tra fusi orari.  

![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md) 

## <a name="syntax"></a>Sintassi

```syntaxsql
inputdate AT TIME ZONE timezone  
```

## <a name="arguments"></a>Argomenti

*inputdate*  
Espressione che può essere risolta in un valore **smalldatetime**, **datetime**, **datetime2**, o **datetimeoffset**.  

*timezone* Nome del fuso orario di destinazione. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si basa sui fusi orari che vengono archiviati nel Registro di sistema di Windows. I fusi orari installati nel computer sono archiviati nell'hive del Registro di sistema seguente: **KEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Time Zones**. Un elenco dei fusi orari installati viene anche esposto attraverso la vista [sys.time_zone_info &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-time-zone-info-transact-sql.md).  

## <a name="return-types"></a>Tipi restituiti

Restituisce il tipo di dati di **datetimeoffset**.

## <a name="return-value"></a>Valore restituito

Il valore **datetimeoffset** nel fuso orario di destinazione.
  
## <a name="remarks"></a>Osservazioni

**AT TIME ZONE** applica regole specifiche per la conversione dei valori di input in tipi di dati **smalldatetime**, **datetime** e **datetime2** che rientrano in un intervallo interessato da un passaggio all'ora legale (DST):

- Quando l'orologio viene spostato in avanti, vi è uno scostamento nell'ora locale uguale alla durata della regolazione dell'orologio. Questa durata è in genere di 1 ora, ma può essere di 30 o 45 minuti, a seconda del fuso orario. I singoli momenti contenuti in questo scostamento vengono convertiti con la differenza *dopo* il passaggio all'ora legale.  

    ```sql
    /*  
        Moving to DST in "Central European Standard Time" zone: 
        offset changes from +01:00 -> +02:00   
        Change occurred on March 29th, 2015 at 02:00:00.   
        Adjusted local time became 2015-03-29 03:00:00.  
    */  

    --Time before DST change has standard time offset (+01:00)
    SELECT CONVERT(DATETIME2(0), '2015-03-29T01:01:00', 126)     
    AT TIME ZONE 'Central European Standard Time';  
    --Result: 2015-03-29 01:01:00 +01:00   
  
    /*
      Adjusted time from the "gap interval" (between 02:00 and 03:00)
      is moved 1 hour ahead and presented with the summer time offset
      (after the DST change) 
    */
    SELECT CONVERT(DATETIME2(0), '2015-03-29T02:01:00', 126)   
    AT TIME ZONE 'Central European Standard Time';  
    --Result: 2015-03-29 03:01:00 +02:00

    --Time after 03:00 is presented with the summer time offset (+02:00)
    SELECT CONVERT(DATETIME2(0), '2015-03-29T03:01:00', 126)   
    AT TIME ZONE 'Central European Standard Time';  
    --Result: 2015-03-29 03:01:00 +02:00  
  
    ```

- Quando l'ora viene riportata indietro, 2 ore dell'ora locale si sovrappongono a formarne una sola.  In tal caso, i singoli momenti che appartengono all'intervallo sovrapposto vengono presentati con la differenza *prima* del cambio dell'ora:  
  
    ```sql
    /*  
        Moving back from DST to standard time in
        "Central European Standard Time" zone:
        offset changes from +02:00 -> +01:00.
        Change occurred on October 25th, 2015 at 03:00:00.
        Adjusted local time became 2015-10-25 02:00:00
    */  

    --Time before the change has DST offset (+02:00)
    SELECT CONVERT(DATETIME2(0), '2015-10-25T01:01:00', 126)
    AT TIME ZONE 'Central European Standard Time';  
    --Result: 2015-10-25 01:01:00 +02:00  

    /*
      Time from the "overlapped interval" is presented with standard time 
      offset (before the change)
    */
    SELECT CONVERT(DATETIME2(0), '2015-10-25T02:00:00', 126)
    AT TIME ZONE 'Central European Standard Time';  
    --Result: 2015-10-25 02:00:00 +02:00  


    --Time after 03:00 is regularly presented with the standard time offset (+01:00)
    SELECT CONVERT(DATETIME2(0), '2015-10-25T03:01:00', 126)
    AT TIME ZONE 'Central European Standard Time';
    --Result: 2015-10-25 03:01:00 +01:00
  
    ```

Poiché alcune informazioni, come ad esempio le regole di fuso orario, vengono mantenute all'esterno di [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] e sono soggette a modifiche occasionali, la funzione **AT TIME ZONE** è classificata come non deterministica. 

## <a name="examples"></a>Esempi

### <a name="a-add-target-time-zone-offset-to-datetime-without-offset-information"></a>R. Aggiungere la differenza di fuso orario di destinazione a datetime senza informazioni relative alla differenza  

Usare **AT TIME ZONE** per aggiungere la differenza in base alle regole del fuso orario, quando si è certi che i valori di **datetime** originali siano specificati nello stesso fuso orario:  

```sql
USE AdventureWorks2016;
GO  
  
SELECT SalesOrderID, OrderDate,
    OrderDate AT TIME ZONE 'Pacific Standard Time' AS OrderDate_TimeZonePST  
FROM Sales.SalesOrderHeader;
```

### <a name="b-convert-values-between-different-time-zones"></a>B. Convertire i valori tra fusi orari diversi  

Nell'esempio seguente vengono convertiti valori tra fusi orari diversi:  

```sql
USE AdventureWorks2016;
GO

SELECT SalesOrderID, OrderDate,
    OrderDate AT TIME ZONE 'Pacific Standard Time' AS OrderDate_TimeZonePST,
    OrderDate AT TIME ZONE 'Central European Standard Time' AS OrderDate_TimeZoneCET
FROM Sales.SalesOrderHeader;
```

### <a name="c-query-temporal-tables-using-a-specific-time-zone"></a>C. Eseguire una query su tabelle temporali usando un fuso orario specifico

Nell'esempio seguente vengono selezionati i dati da una tabella temporale con l'ora solare Pacifico.  

```sql
USE AdventureWorks2016;
GO

DECLARE @ASOF datetimeoffset;  
SET @ASOF = DATEADD (month, -1, GETDATE()) AT TIME ZONE 'UTC';

-- Query state of the table a month ago projecting period
-- columns as Pacific Standard Time
SELECT BusinessEntityID, PersonType, NameStyle, Title,
    FirstName, MiddleName,
    ValidFrom AT TIME ZONE 'Pacific Standard Time'
FROM  Person.Person_Temporal
FOR SYSTEM_TIME AS OF @ASOF;
```

## <a name="next-steps"></a>Passaggi successivi

- [Tipi di data e ora](../../t-sql/data-types/date-and-time-types.md)
- [Funzioni e tipi di dati di data e ora &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md)
