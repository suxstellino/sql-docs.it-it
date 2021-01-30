---
description: 'Da C a SQL: timestamp'
title: 'Da C a SQL: timestamp | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data conversions from C to SQL types [ODBC], timestamp
- timestamp data type [ODBC]
- converting data from c to SQL types [ODBC], timestamp
ms.assetid: 0e08bfff-68f9-4648-9558-09b57fea08ad
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 54632816fb7013639e1cdb761fbc8df53c461a0d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158592"
---
# <a name="c-to-sql-timestamp"></a>Da C a SQL: timestamp
Identificatore per il tipo di dati timestamp ODBC C:  
  
 SQL_C_TYPE_TIMESTAMP  
  
 Nella tabella seguente sono illustrati i tipi di dati ODBC SQL in cui è possibile convertire i dati timestamp C. Per una spiegazione delle colonne e dei termini della tabella, vedere [conversione di dati da C a tipi di dati SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md).  
  
|Identificatore del tipo SQL|Test|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR<br /><br /> SQL_VARCHAR<br /><br /> SQL_LONGVARCHAR|Lunghezza byte colonna >= lunghezza byte carattere<br /><br /> 19 <= lunghezza in byte colonna < lunghezza in byte carattere<br /><br /> Lunghezza in byte colonna < 19<br /><br /> Il valore dei dati non è un timestamp valido|n/d<br /><br /> 22001<br /><br /> 22001<br /><br /> 22008|  
|SQL_WCHAR<br /><br /> SQL_WVARCHAR<br /><br /> SQL_WLONGVARCHAR|Lunghezza carattere colonna >= Lunghezza caratteri dei dati<br /><br /> 19 <= Lunghezza carattere colonna < Lunghezza caratteri dei dati<br /><br /> Lunghezza carattere colonna < 19<br /><br /> Il valore dei dati non è un timestamp valido|n/d<br /><br /> 22001<br /><br /> 22001<br /><br /> 22008|  
|SQL_TYPE_DATE|I campi di tempo sono pari a zero<br /><br /> I campi di tempo sono diversi da zero<br /><br /> Il valore dei dati non contiene una data valida|n/d<br /><br /> 22008<br /><br /> 22007|  
|SQL_TYPE_TIME|I campi di secondi frazionari sono pari a zero [a]<br /><br /> I campi di secondi frazionari sono diversi da zero [a]<br /><br /> Il valore dei dati non contiene un tempo valido|n/d<br /><br /> 22008<br /><br /> 22007|  
|SQL_TYPE_TIMESTAMP|I campi di secondi frazionari non sono troncati<br /><br /> I campi di secondi frazionari vengono troncati<br /><br /> Il valore dei dati non è un timestamp valido|n/d<br /><br /> 22008<br /><br /> 22007|  
  
 [a] i campi relativi alla data della struttura del timestamp vengono ignorati.  
  
 Per informazioni sui valori validi in una struttura SQL_C_TIMESTAMP, vedere tipi di [dati C](../../../odbc/reference/appendixes/c-data-types.md), più indietro in questa appendice.  
  
 Quando i dati timestamp C vengono convertiti in dati di tipo carattere SQL, i dati di tipo carattere risultanti sono in "*aaaa* - *mm* - *GG* *HH*:*mm*:*SS*[.*f...*] "Format.  
  
 Il driver ignora il valore di lunghezza/indicatore durante la conversione dei dati dal tipo di dati timestamp C e presuppone che le dimensioni del buffer dei dati siano le dimensioni del tipo di dati timestamp C. Il valore di lunghezza/indicatore viene passato nell'argomento *StrLen_Or_Ind* in **SQLPutData** e nel buffer specificato con l'argomento *StrLen_or_IndPtr* in **SQLBindParameter**. Il buffer di dati viene specificato con l'argomento *DataPtr* in **SQLPutData** e l'argomento *ParameterValuePtr* in **SQLBindParameter**.
