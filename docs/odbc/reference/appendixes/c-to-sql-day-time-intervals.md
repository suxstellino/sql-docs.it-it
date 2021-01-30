---
description: 'Da C a SQL: intervalli di data/ora'
title: 'Da C a SQL: intervalli di Day-Time | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- day-time intervals [ODBC]
- data conversions from C to SQL types [ODBC], day-time intervals
- converting data from c to SQL types [ODBC], day-time intervals
- intervals [ODBC], converting
ms.assetid: f9ee1ddb-dec7-4f78-b6e2-5ba34e7d6f59
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e207a7533a0f09360925f6123e8797bdabecc21b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207830"
---
# <a name="c-to-sql-day-time-intervals"></a>Da C a SQL: intervalli di data/ora
Gli identificatori per i tipi di dati ODBC C per l'intervallo di tempo del giorno sono:  
  
 SQL_C_INTERVAL_DAY  
  
 SQL_C_INTERVAL_HOUR  
  
 SQL_C_INTERVAL_MINUTE  
  
 SQL_C_INTERVAL_SECOND  
  
 SQL_C_INTERVAL_DAY_TO_HOUR  
  
 SQL_C_INTERVAL_DAY_TO_MINUTE  
  
 SQL_C_INTERVAL_DAY_TO_SECOND  
  
 SQL_C_INTERVAL_HOUR_TO_MINUTE  
  
 SQL_C_INTERVAL_HOUR_TO_SECOND  
  
 SQL_C_INTERVAL_MINUTE_TO_SECOND  
  
 Nella tabella seguente sono illustrati i tipi di dati ODBC SQL in cui è possibile convertire i dati intervallo C. Per una spiegazione delle colonne e dei termini della tabella, vedere [conversione di dati da C a tipi di dati SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md).  
  
|Identificatore del tipo SQL|Test|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR [a]<br /><br /> SQL_VARCHAR [a]<br /><br /> SQL_LONGVARCHAR [a]|Lunghezza byte colonna >= lunghezza byte carattere<br /><br /> Lunghezza in byte di colonna < lunghezza in byte carattere [a]<br /><br /> Il valore dei dati non è un valore letterale di intervallo valido|n/d<br /><br /> 22001<br /><br /> 22015|  
|SQL_WCHAR [a]<br /><br /> SQL_WVARCHAR [a]<br /><br /> SQL_WLONGVARCHAR [a]|Lunghezza carattere colonna >= Lunghezza caratteri dei dati<br /><br /> Lunghezza carattere colonna < lunghezza di caratteri dei dati [a]<br /><br /> Il valore dei dati non è un valore letterale di intervallo valido|n/d<br /><br /> 22001<br /><br /> 22015|  
|SQL_TINYINT [b]<br /><br /> SQL_SMALLINT [b] SQL_INTEGER [b]<br /><br /> SQL_BIGINT [b] SQL_NUMERIC [b]<br /><br /> SQL_DECIMAL [b]|La conversione di un intervallo a campo singolo non ha causato il troncamento di cifre intere<br /><br /> La conversione ha causato il troncamento di cifre intere|n/d<br /><br /> 22003|  
|SQL_INTERVAL_DAY<br /><br /> SQL_INTERVAL_HOUR<br /><br /> SQL_INTERVAL_MINUTE<br /><br /> SQL_INTERVAL_SECOND<br /><br /> SQL_INTERVAL_DAY_TO_HOUR<br /><br /> SQL_INTERVAL_DAY_TO_MINUTE<br /><br /> SQL_INTERVAL_DAY_TO_SECOND<br /><br /> SQL_INTERVAL_HOUR_TO_MINUTE<br /><br /> SQL_INTERVAL_HOUR_TO_SECOND<br /><br /> SQL_INTERVAL_MINUTE_TO_SECOND|Il valore dei dati è stato convertito senza troncamento dei campi<br /><br /> Uno o più campi del valore di dati sono stati troncati durante la conversione|n/d<br /><br /> 22015|  
  
 [a] tutti i tipi di dati intervallo C possono essere convertiti in un tipo di dati character.  
  
 [b] se il campo tipo nella struttura intervallo è tale che l'intervallo è un singolo campo (SQL_DAY, SQL_HOUR, SQL_MINUTE o SQL_SECOND), il tipo intervallo C può essere convertito in un valore numerico esatto (SQL_TINYINT, SQL_SMALLINT, SQL_INTEGER, SQL_BIGINT, SQL_DECIMAL o SQL_NUMERIC).  
  
 La conversione predefinita di un tipo intervallo C corrisponde al tipo SQL intervallo di tempo del giorno corrispondente.  
  
 Il driver ignora il valore di lunghezza/indicatore durante la conversione dei dati dal tipo di dati interval C e presuppone che le dimensioni del buffer dei dati siano le dimensioni del tipo di dati interval C. Il valore di lunghezza/indicatore viene passato nell'argomento *StrLen_Or_Ind* in **SQLPutData** e nel buffer specificato con l'argomento *StrLen_or_IndPtr* in **SQLBindParameter**. Il buffer di dati viene specificato con l'argomento *DataPtr* in **SQLPutData** e l'argomento *ParameterValuePtr* in **SQLBindParameter**.  
  
 Nell'esempio seguente viene illustrato come inviare i dati intervallo C archiviati nella struttura SQL_INTERVAL_STRUCT in una colonna del database. La struttura di intervallo contiene un intervallo di DAY_TO_SECOND; verrà archiviato in una colonna di database di tipo SQL_INTERVAL_DAY_TO_MINUTE.  
  
```  
SQL_INTERVAL_STRUCT is;  
SQLINTEGER cbValue;  
  
// Initialize the interval struct to contain the DAY_TO_SECOND  
// interval "154 days, 22 hours, 44 minutes, and 10 seconds"  
is.intval.day_second.day      = 154;  
is.intval.day_second.hour     = 22;  
is.intval.day_second.minute   = 44;  
is.intval.day_second.second   = 10;  
is.interval_sign              = SQL_FALSE;  
  
// Bind the dynamic parameter  
SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_INTERVAL_DAY_TO_SECOND,  
                  SQL_INTERVAL_DAY_TO_MINUTE, 0, 0, &is,  
                  sizeof(SQL_INTERVAL_STRUCT), &cbValue);  
  
// Execute an insert statement; "interval_column" is a column  
// whose data type is SQL_INTERVAL_DAY_TO_MINUTE.  
SQLExecDirect(hstmt,"INSERT INTO table(interval_column) VALUES (?)",SQL_NTS);  
```
