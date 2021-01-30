---
description: 'Da SQL a C: dati numerici'
title: 'Da SQL a C: Numeric | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data conversions from SQL to C types [ODBC], numeric
- numeric data type [ODBC], converting
- converting data from SQL to C types [ODBC], numeric
ms.assetid: 76f8b5d5-4bd0-4dcb-a90a-698340e0d36e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7f18cc2ecff847b2d7d7716e2c302de2b7bb3ff2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203021"
---
# <a name="sql-to-c-numeric"></a>Da SQL a C: dati numerici

Gli identificatori per i tipi di dati ODBC SQL numerici sono i seguenti:

- SQL_DECIMAL  
- SQL_BIGINT  
- SQL_NUMERIC  
- SQL_REAL  
- SQL_TINYINT  
- SQL_FLOAT  
- SQL_SMALLINT  
- SQL_DOUBLE SQL_INTEGER  

Nella tabella seguente sono illustrati i tipi di dati ODBC C in cui è possibile convertire i dati SQL numerici. Per una spiegazione delle colonne e dei termini della tabella, vedere [conversione di dati da SQL a tipi di dati C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md).  

|Identificatore di tipo C|Test|**TargetValuePtr*|**StrLen_or_IndPtr*|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|SQL_C_CHAR|Lunghezza in byte di caratteri < *bufferLength*<br /><br /> Numero di cifre intere (anziché frazionarie) < *bufferLength*<br /><br /> Numero di cifre intere (anziché frazionarie) >= *bufferLength*|Data<br /><br /> Dati troncati<br /><br /> Non definito|Lunghezza dei dati in byte<br /><br /> Lunghezza dei dati in byte<br /><br /> Non definito|n/d<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_WCHAR|Lunghezza in caratteri < *bufferLength*<br /><br /> Numero di cifre intere (anziché frazionarie) < *bufferLength*<br /><br /> Numero di cifre intere (anziché frazionarie) >= *bufferLength*|Data<br /><br /> Dati troncati<br /><br /> Non definito|Lunghezza dei dati in caratteri<br /><br /> Lunghezza dei dati in caratteri<br /><br /> Non definito|n/d<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_STINYINT<br /><br /> SQL_C_UTINYINT<br /><br /> SQL_C_TINYINT<br /><br /> SQL_C_SBIGINT<br /><br /> SQL_C_UBIGINT<br /><br /> SQL_C_SSHORT<br /><br /> SQL_C_USHORT<br /><br /> SQL_C_SHORT<br /><br /> SQL_C_SLONG<br /><br /> SQL_C_ULONG<br /><br /> SQL_C_LONG<br /><br /> SQL_C_NUMERIC|Dati convertiti senza troncamento [a]<br /><br /> Dati convertiti con troncamento di cifre frazionarie [a]<br /><br /> La conversione dei dati comporterebbe la perdita di cifre intere (anziché frazionarie) [a]|Data<br /><br /> Dati troncati<br /><br /> Non definito|Dimensioni del tipo di dati C<br /><br /> Dimensioni del tipo di dati C<br /><br /> Non definito|n/d<br /><br /> 01S07<br /><br /> 22003|  
|SQL_C_FLOAT<br /><br /> SQL_C_DOUBLE|I dati sono compresi nell'intervallo del tipo di dati in cui viene convertito il numero [a]<br /><br /> I dati non rientrano nell'intervallo del tipo di dati in cui viene convertito il numero [a]|Data<br /><br /> Non definito|Dimensioni del tipo di dati C<br /><br /> Non definito|n/d<br /><br /> 22003|  
|SQL_C_BIT|I dati sono 0 o 1 [a]<br /><br /> I dati sono maggiori di 0, minori di 2 e diversi da 1 [a]<br /><br /> I dati sono minori di 0 oppure maggiori o uguali a 2 [a]|Data<br /><br /> Dati troncati<br /><br /> Non definito|1 [b]<br /><br /> 1 [b]<br /><br /> Non definito|n/d<br /><br /> 01S07<br /><br /> 22003|  
|SQL_C_BINARY|Lunghezza in byte dei dati <= *bufferLength*<br /><br /> Lunghezza in byte dei dati > *bufferLength*|Data<br /><br /> Non definito|Lunghezza dei dati<br /><br /> Non definito|n/d<br /><br /> 22003|  
|SQL_C_INTERVAL_MONTH [c] SQL_C_INTERVAL_YEAR [c] SQL_C_INTERVAL_DAY [c] SQL_C_INTERVAL_HOUR [c] SQL_C_INTERVAL_MINUTE [c] SQL_C_INTERVAL_SECOND [c]|Dati non troncati<br /><br /> Parte relativa ai secondi frazionari troncata<br /><br /> Parte intera del numero troncato|Data<br /><br /> Dati troncati<br /><br /> Non definito|Lunghezza dei dati in byte<br /><br /> Lunghezza dei dati in byte<br /><br /> Non definito|n/d<br /><br /> 01S07<br /><br /> 22015|  
|SQL_C_INTERVAL_YEAR_TO_MONTH SQL_C_INTERVAL_DAY_TO_HOUR SQL_C_INTERVAL_DAY_TO_MINUTE SQL_C_INTERVAL_DAY_TO_SECOND SQL_C_INTERVAL_HOUR_TO_MINUTE SQL_C_INTERVAL_HOUR_TO_SECOND|Parte intera del numero troncato|Non definito|Non definito|22015|  
  
 [a] il valore di *bufferLength* viene ignorato per la conversione. Il driver presuppone che le dimensioni di **TargetValuePtr* siano le dimensioni del tipo di dati C.  
  
 [b] corrisponde alla dimensione del tipo di dati C corrispondente.  
  
 [c] Questa conversione è supportata solo per i tipi di dati numerici esatti (SQL_DECIMAL, SQL_NUMERIC, SQL_TINYINT, SQL_SMALLINT, SQL_INTEGER e SQL_BIGINT). Non è supportata per i tipi di dati numerici approssimati (SQL_REAL, SQL_FLOAT o SQL_DOUBLE).  

## <a name="sql_c_numeric-and-sqlsetdescfield"></a>SQL_C_NUMERIC e SQLSetDescField

 La [funzione SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md) è necessaria per eseguire un'associazione manuale con valori SQL_C_NUMERIC. (Si noti che SQLSetDescField è stato aggiunto in ODBC 3,0). Per eseguire l'associazione manuale, è innanzitutto necessario ottenere l'handle del descrittore.  

```cpp
if (fCType == SQL_C_NUMERIC) {   
   // special processing required for NUMERIC to get right scale & precision  
   // Modify the fields in the implicit application parameter descriptor  
   SQLHDESC hdesc=NULL;  
  
   // Use SQL_ATTR_APP_ROW_DESC for calls to SQLBindCol()  
   // Use SQL_ATTR_APP_PARAM_DESC for calls to SQLBindParameter()  
   //  
   // retcode = SQLGetStmtAttr(hstmt, SQL_ATTR_APP_ROW_DESC, &hdesc, 0, NULL);  
   retcode = SQLGetStmtAttr(hstmt, SQL_ATTR_APP_PARAM_DESC, &hdesc, 0, NULL);  
   if (!ODBC_CALL_SUCCESS(retcode)) {  
      printf ("\nSQLGetStmtAttr failed");  
      i = 1;  
      sqlstate[7] = '\0';  
      while (SQLGetDiagRec(SQL_HANDLE_STMT, hstmt, i, sqlstate, &NativeError, wrkbuf, sizeof(wrkbuf), &len) != SQL_NO_DATA) {  
         printf("\niTestCase = %d Failed...Precision = %d, Scale = %d\nNativeError=%d, State=%s, \n  Message=%s",   
            iTestCase, Precision, Scale, NativeError, sqlstate, wrkbuf);  
         i++;  
      }  
      continue;  
   }  
   retcode = SQLSetDescField(hdesc, iCol, SQL_DESC_TYPE, (SQLPOINTER) SQL_C_NUMERIC, 0);  
   if (!ODBC_CALL_SUCCESS(retcode))  
      goto error;  
   retcode = SQLSetDescField(hdesc, iCol, SQL_DESC_PRECISION, (SQLPOINTER)num.precision, 0);  
   if (!ODBC_CALL_SUCCESS(retcode))  
      goto error;  
   retcode = SQLSetDescField(hdesc, iCol, SQL_DESC_SCALE, (SQLPOINTER)num.scale, 0);  
   if (!ODBC_CALL_SUCCESS(retcode))  
      goto error;  
   retcode = SQLSetDescField(hdesc, iCol, SQL_DESC_DATA_PTR, (SQLPOINTER) &(num), sizeof(num));  
```
