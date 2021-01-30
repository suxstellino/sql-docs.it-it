---
description: Struttura C Interval
title: Struttura intervallo C | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], interval data types
- interval data type [ODBC], structure
- C data types [ODBC], interval
ms.assetid: 52b42b56-50aa-4ce6-8d79-0963c7a71437
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e470d0e30864c953fa3937725bcd5597bf542f41
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212442"
---
# <a name="c-interval-structure"></a>Struttura C Interval
Ogni tipo di dati intervallo C elencato nella sezione [tipi di dati c](../../../odbc/reference/appendixes/c-data-types.md) utilizza la stessa struttura per contenere i dati intervallo. Quando si chiama **SQLFetch**, **SQLFetchScroll** o **SQLGetData** , il driver restituisce i dati nella struttura di SQL_INTERVAL_STRUCT, usa il valore specificato dall'applicazione per i tipi di dati c (nella chiamata a **SQLBindCol**, **SQLGetData** o **SQLBindParameter**) per interpretare il contenuto di SQL_INTERVAL_STRUCT e popola il campo *interval_type* della struttura con il valore *enum* corrispondente al tipo c. Si noti che i driver non leggono il campo *interval_type* per determinare il tipo di intervallo; recuperano il valore del campo del descrittore SQL_DESC_CONCISE_TYPE. Quando si usa la struttura per i dati dei parametri, il driver usa il valore specificato dall'applicazione nel campo SQL_DESC_CONCISE_TYPE di APD per interpretare il contenuto di SQL_INTERVAL_STRUCT, anche se l'applicazione imposta il valore del campo *interval_type* su un valore diverso.  
  
 Questa struttura viene definita nel modo seguente:  
  
```  
typedef struct tagSQL_INTERVAL_STRUCT  
{  
   SQLINTERVAL interval_type;   
   SQLSMALLINT interval_sign;  
   union {  
         SQL_YEAR_MONTH_STRUCT   year_month;  
         SQL_DAY_SECOND_STRUCT   day_second;  
         } intval;  
} SQL_INTERVAL_STRUCT;  
typedef enum   
{  
   SQL_IS_YEAR = 1,  
   SQL_IS_MONTH = 2,  
   SQL_IS_DAY = 3,  
   SQL_IS_HOUR = 4,  
   SQL_IS_MINUTE = 5,  
   SQL_IS_SECOND = 6,  
   SQL_IS_YEAR_TO_MONTH = 7,  
   SQL_IS_DAY_TO_HOUR = 8,  
   SQL_IS_DAY_TO_MINUTE = 9,  
   SQL_IS_DAY_TO_SECOND = 10,  
   SQL_IS_HOUR_TO_MINUTE = 11,  
   SQL_IS_HOUR_TO_SECOND = 12,  
   SQL_IS_MINUTE_TO_SECOND = 13  
} SQLINTERVAL;  
  
typedef struct tagSQL_YEAR_MONTH  
{  
   SQLUINTEGER year;  
   SQLUINTEGER month;   
} SQL_YEAR_MONTH_STRUCT;  
  
typedef struct tagSQL_DAY_SECOND  
{  
   SQLUINTEGER day;  
   SQLUINTEGER hour;  
   SQLUINTEGER minute;  
   SQLUINTEGER second;  
   SQLUINTEGER fraction;  
} SQL_DAY_SECOND_STRUCT;  
```  
  
 Il campo *interval_type* della SQL_INTERVAL_STRUCT indica all'applicazione la struttura che viene mantenuta nell'Unione e i membri della struttura sono rilevanti. Il campo *interval_sign* dispone del valore SQL_FALSE se il campo con l'intervallo è senza segno; Se è SQL_TRUE, il campo principale è negativo. Il valore nel campo principale è sempre senza segno, indipendentemente dal valore di *interval_sign*. Il campo *interval_sign* funge da bit di segno.
