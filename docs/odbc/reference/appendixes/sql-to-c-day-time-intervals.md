---
description: 'Da SQL a C: intervalli di tempo'
title: 'Da SQL a C: intervalli di Day-Time | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from SQL to C types [ODBC], day-time intervals
- day-time intervals [ODBC]
- data conversions from SQL to C types [ODBC], day-time intervals
- intervals [ODBC], converting
ms.assetid: 8ea84d69-2292-4128-89a0-f184f68abb98
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ae81fdf91c59457a6453958cb1c906abdd9d53fa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203041"
---
# <a name="sql-to-c-day-time-intervals"></a>Da SQL a C: intervalli di tempo

Gli identificatori per i tipi di dati ODBC SQL per l'intervallo di tempo del giorno sono i seguenti:

- SQL_INTERVAL_DAY
- SQL_INTERVAL_DAY_TO_MINUTE
- SQL_INTERVAL_HOUR
- SQL_INTERVAL_DAY_TO_SECOND
- SQL_INTERVAL_MINUTE
- SQL_INTERVAL_HOUR_TO_MINUTE
- SQL_INTERVAL_SECOND
- SQL_INTERVAL_HOUR_TO_SECOND
- SQL_INTERVAL_DAY_TO_HOUR
- SQL_INTERVAL_MINUTE_TO_SECOND

Nella tabella seguente sono illustrati i tipi di dati ODBC C in cui è possibile convertire i dati SQL dell'intervallo di tempo del giorno. Per una spiegazione delle colonne e dei termini della tabella, vedere [conversione di dati da SQL a tipi di dati C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md).

|Identificatore di tipo C|Test|**TargetValuePtr*|**StrLen_or_IndPtr*|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|Tutti i tipi di intervallo C giornalieri|Porzione campi finali non troncata<br /><br /> Porzione campi finali troncata<br /><br /> La precisione principale della destinazione non è abbastanza grande da poter conservare i dati dall'origine|Data<br /><br /> Dati troncati<br /><br /> Non definito|Lunghezza dei dati<br /><br /> Lunghezza dei dati<br /><br /> Non definito|n/d<br /><br /> 01S07<br /><br /> 22015|  
|SQL_C_STINYINT [b] SQL_C_UTINYINT [b] SQL_C_USHORT [b] SQL_C_SHORT [b] SQL_C_SLONG [b] SQL_C_ULONG [b] SQL_C_NUMERIC [b] SQL_C_BIGINT [b]|La precisione dell'intervallo è un singolo campo e i dati sono stati convertiti senza troncamento<br /><br /> La precisione dell'intervallo è un singolo campo e frazionario troncato<br /><br /> La precisione dell'intervallo è un singolo campo e l'intero troncato<br /><br /> La precisione dell'intervallo non è un campo singolo|Data<br /><br /> Dati troncati<br /><br /> Dati troncati<br /><br /> Non definito|Dimensioni del tipo di dati C<br /><br /> Lunghezza dei dati<br /><br /> Lunghezza dei dati<br /><br /> Dimensioni del tipo di dati C|n/d<br /><br /> 01S07<br /><br /> 22003<br /><br /> 07006|  
|SQL_C_BINARY|Lunghezza in byte dei dati <= *bufferLength*<br /><br /> Lunghezza in byte dei dati > *bufferLength*|Data<br /><br /> Non definito|Lunghezza dei dati<br /><br /> Non definito|n/d<br /><br /> 22003|  
|SQL_C_CHAR|Lunghezza in byte di caratteri < *bufferLength*<br /><br /> Numero di cifre intere (anziché frazionarie) < *bufferLength*<br /><br /> Numero di cifre intere (anziché frazionarie) >= *bufferLength*|Data<br /><br /> Dati troncati<br /><br /> Non definito|Dimensioni del tipo di dati C<br /><br /> Dimensioni del tipo di dati C<br /><br /> Non definito|n/d<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_WCHAR|Lunghezza in caratteri < *bufferLength*<br /><br /> Numero di cifre intere (anziché frazionarie) < *bufferLength*<br /><br /> Numero di cifre intere (anziché frazionarie) >= *bufferLength*|Data<br /><br /> Dati troncati<br /><br /> Non definito|Dimensioni del tipo di dati C<br /><br /> Dimensioni del tipo di dati C<br /><br /> Non definito|n/d<br /><br /> 01004<br /><br /> 22003|  
  
 [a] un tipo SQL di intervallo di giorno può essere convertito in un tipo C con intervallo di tempo.  
  
 [b] se la precisione dell'intervallo è un singolo campo (un giorno, un'ora, un minuto o un secondo), il tipo di intervallo SQL può essere convertito in un valore numerico esatto (SQL_C_STINYINT, SQL_C_UTINYINT, SQL_C_USHORT, SQL_C_SHORT, SQL_C_SLONG, SQL_C_ULONG o SQL_C_NUMERIC).  
  
La conversione predefinita di un tipo SQL intervallo corrisponde al tipo di dati intervallo C corrispondente. L'applicazione associa quindi la colonna o il parametro (oppure imposta il campo SQL_DESC_DATA_PTR nel record appropriato del sistema ARD) in modo che punti alla struttura di SQL_INTERVAL_STRUCT inizializzata (o passa un puntatore alla struttura SQL_ INTERVAL_STRUCT come argomento *TargetValuePtr* in una chiamata a **SQLGetData**).  
  
Nell'esempio seguente viene illustrato come trasferire i dati da una colonna di tipo SQL_INTERVAL_DAY_TO_MINUTE nella struttura SQL_INTERVAL_STRUCT in modo che ritorni come intervallo di DAY_TO_HOUR.  

```cpp
SQL_INTERVAL_STRUCT is;  
SQLINTEGER    cbValue;  
SQLUINTEGER   days, hours;  
  
// Execute a select statement; "interval_column" is a column  
// whose data type is SQL_INTERVAL_DAY_TO_MINUTE.  
SQLExecDirect(hstmt, "SELECT interval_column FROM table", SQL_NTS);  
  
// Bind  
SQLBindCol(hstmt, 1, SQL_C_INTERVAL_DAY_TO_MINUTE, &is, sizeof(SQL_INTERVAL_STRUCT), &cbValue);  
  
// Fetch  
SQLFetch(hstmt);  
  
// Process data  
days = is.intval.day_second.day;  
hours = is.intval.day_second.hour;  
```
