---
description: 'Da C a SQL: data'
title: 'Da C a SQL: data | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- date data type [ODBC]
- converting data from c to SQL types [ODBC], date
- data conversions from C to SQL types [ODBC], date
ms.assetid: bea087d3-911f-418b-b483-d2b5b334da19
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a0724309642b8a6dc640b6159715927544d74733
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207843"
---
# <a name="c-to-sql-date"></a>Da C a SQL: data
Identificatore per il tipo di dati ODBC C Data:  
  
 SQL_C_TYPE_DATE  
  
 Nella tabella seguente sono illustrati i tipi di dati ODBC SQL in cui è possibile convertire i dati di data C. Per una spiegazione delle colonne e dei termini della tabella, vedere [conversione di dati da C a tipi di dati SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md).  
  
|Identificatore del tipo SQL|Test|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR<br /><br /> SQL_VARCHAR<br /><br /> SQL_LONGVARCHAR|Lunghezza byte colonna >= 10<br /><br /> Lunghezza in byte colonna < 10<br /><br /> Il valore dei dati non è una data valida|n/d<br /><br /> 22001<br /><br /> 22008|  
|SQL_WCHAR<br /><br /> SQL_WVARCHAR<br /><br /> SQL_WLONGVARCHAR|Lunghezza carattere colonna >= 10<br /><br /> Lunghezza carattere colonna < 10<br /><br /> Il valore dei dati non è una data valida|n/d<br /><br /> 22001<br /><br /> 22008|  
|SQL_TYPE_DATE|Il valore dei dati è una data valida<br /><br /> Il valore dei dati non è una data valida|n/d<br /><br /> 22007|  
|SQL_TYPE_TIMESTAMP|Il valore dei dati è una data valida [a]<br /><br /> Il valore dei dati non è una data valida|n/d<br /><br /> 22007|  
  
 [a] la parte relativa all'ora del timestamp è impostata su zero.  
  
 Per informazioni sui valori validi in una struttura SQL_C_TYPE_DATE, vedere tipi di [dati C](../../../odbc/reference/appendixes/c-data-types.md), più indietro in questa appendice.  
  
 Quando i dati di data C vengono convertiti in dati di tipo carattere SQL, i dati di tipo carattere risultanti sono nel formato "*aaaa* - *mm* - *GG*".  
  
 Il driver ignora il valore di lunghezza/indicatore durante la conversione dei dati dal tipo di dati Data C e presuppone che le dimensioni del buffer dei dati siano pari alla dimensione del tipo di dati Data C. Il valore di lunghezza/indicatore viene passato nell'argomento *StrLen_Or_Ind* in **SQLPutData** e nel buffer specificato con l'argomento *StrLen_or_IndPtr* in **SQLBindParameter**. Il buffer di dati viene specificato con l'argomento *DataPtr* in **SQLPutData** e l'argomento *ParameterValuePtr* in **SQLBindParameter**.
