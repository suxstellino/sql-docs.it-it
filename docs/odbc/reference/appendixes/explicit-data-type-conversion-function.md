---
description: Funzione di conversione esplicita del tipo di dati
title: Funzione di conversione del tipo di dati esplicita | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- explicit data type conversion functions [ODBC]
- data type conversion functions [ODBC]
- functions [ODBC], explicit data type conversion functions
ms.assetid: d5789450-b668-4753-96c8-6789e955e7ed
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 927c911b839e7aa07b087edb0fb3b457d0825b6c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194816"
---
# <a name="explicit-data-type-conversion-function"></a>Funzione di conversione esplicita del tipo di dati
La conversione esplicita del tipo di dati viene specificata in termini di definizioni del tipo di dati SQL.  
  
 La sintassi ODBC per la funzione di conversione esplicita del tipo di dati non limita le conversioni. La validità delle conversioni specifiche di un tipo di dati in un altro tipo di dati sarà determinata da ogni implementazione specifica del driver. Poiché il driver converte la sintassi ODBC nella sintassi nativa, rifiuterà le conversioni che, sebbene valide nella sintassi ODBC, non sono supportate dall'origine dati. La funzione ODBC **SQLGetInfo**, con le opzioni di conversione (ad esempio SQL_CONVERT_BIGINT, SQL_CONVERT_BINARY, SQL_CONVERT_INTERVAL_YEAR_MONTH e così via), fornisce un modo per richiedere informazioni sulle conversioni supportate dall'origine dati.  
  
 Il formato della funzione **Convert** è:  
  
 **Convert (** _value_exp_, _data_type_**)**  
  
 La funzione restituisce il valore specificato da *value_exp* convertito nel *data_type* specificato, dove *data_type* è una delle parole chiave seguenti:  

:::row:::
    :::column:::
        SQL_BIGINT  
        SQL_BINARY  
        SQL_BIT  
        SQL_CHAR  
        SQL_DATE  
        SQL_DECIMAL  
        SQL_DOUBLE  
        SQL_FLOAT  
        SQL_GUID  
        SQL_INTEGER  
        SQL_INTERVAL_DAY  
        SQL_INTERVAL_DAY_TO_HOUR  
    :::column-end:::
    :::column:::
        SQL_INTERVAL_DAY_TO_MINUTE  
        SQL_INTERVAL_DAY_TO_SECOND  
        SQL_INTERVAL_HOUR  
        SQL_INTERVAL_HOUR_TO_MINUTE  
        SQL_INTERVAL_HOUR_TO_SECOND  
        SQL_INTERVAL_MINUTE  
        SQL_INTERVAL_MINUTE_TO_SECOND  
        SQL_INTERVAL_MONTH  
        SQL_INTERVAL_SECOND  
        SQL_INTERVAL_YEAR  
        SQL_INTERVAL_YEAR_TO_MONTH  
        SQL_LONGVARBINARY  
    :::column-end:::
    :::column:::
        SQL_LONGVARCHAR  
        SQL_NUMERIC  
        SQL_REAL  
        SQL_SMALLINT  
        SQL_TIME  
        SQL_TIMESTAMP  
        SQL_TINYINT  
        SQL_VARBINARY  
        SQL_VARCHAR  
        SQL_WCHAR  
        SQL_WLONGVARCHAR  
        SQL_WVARCHAR  
    :::column-end:::
:::row-end:::

 La sintassi ODBC per la funzione di conversione esplicita del tipo di dati non supporta la specifica del formato di conversione. Se la specifica dei formati espliciti è supportata dall'origine dati sottostante, è necessario che un driver specifichi un valore predefinito o implementi la specifica di formato.  
  
 L'argomento *value_exp* può essere un nome di colonna, il risultato di un'altra funzione scalare o un valore letterale stringa o numerico. Ad esempio:  
  
```  
{ fn CONVERT( { fn CURDATE() }, SQL_CHAR ) }  
```  
  
 Converte l'output della funzione scalare CURDe in una stringa di caratteri.  
  
 Poiché ODBC non impone un tipo di dati per i valori restituiti dalle funzioni scalari (poiché le funzioni sono spesso specifiche dell'origine dati), le applicazioni devono utilizzare la funzione CONVERT Scalar, quando possibile, per forzare la conversione del tipo di dati.  
  
 Nei due esempi seguenti viene illustrato l'utilizzo della funzione **Convert** . In questi esempi si presuppone l'esistenza di una tabella denominata EMPLOYEEs, con una colonna EMPNO di tipo SQL_SMALLINT e una colonna EMPNAME di tipo SQL_CHAR.  
  
 Se un'applicazione specifica l'istruzione SQL seguente:  
  
```  
SELECT EMPNO FROM EMPLOYEES WHERE {fn CONVERT(EMPNO,SQL_CHAR)} LIKE '1%'  
```  
  
-   Un driver per ORACLE converte l'istruzione SQL in:  
  
    ```  
    SELECT EMPNO FROM EMPLOYEES WHERE to_char(EMPNO) LIKE '1%'  
    ```  
  
-   Un driver per SQL Server converte l'istruzione SQL in:  
  
    ```  
    SELECT EMPNO FROM EMPLOYEES WHERE convert(char,EMPNO) LIKE '1%'  
    ```  
  
 Se un'applicazione specifica l'istruzione SQL seguente:  
  
```  
SELECT {fn ABS(EMPNO)}, {fn CONVERT(EMPNAME,SQL_SMALLINT)}  
   FROM EMPLOYEES WHERE EMPNO <> 0  
```  
  
-   Un driver per ORACLE converte l'istruzione SQL in:  
  
    ```  
    SELECT abs(EMPNO), to_number(EMPNAME) FROM EMPLOYEES WHERE EMPNO <> 0  
    ```  
  
-   Un driver per SQL Server converte l'istruzione SQL in:  
  
    ```  
    SELECT abs(EMPNO), convert(smallint, EMPNAME) FROM EMPLOYEES  
       WHERE EMPNO <> 0  
    ```  
  
-   Un driver per Ingres converte l'istruzione SQL in:  
  
    ```  
    SELECT abs(EMPNO), int2(EMPNAME) FROM EMPLOYEES WHERE EMPNO <> 0  
    ```
