---
description: 'Da C a SQL: ora'
title: 'Da C a SQL: ora | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data conversions from C to SQL types [ODBC], time
- time data type [ODBC]
- converting data from c to SQL types [ODBC], time
ms.assetid: a8da43c9-d9a5-45e5-bd9a-1dd633db2ee0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b53505a9f10d6f7039e0811e5f9fda8674092be1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195064"
---
# <a name="c-to-sql-time"></a>Da C a SQL: ora
Identificatore per il tipo di dati time ODBC C:  
  
 SQL_C_TYPE_TIME  
  
 Nella tabella seguente sono illustrati i tipi di dati ODBC SQL ai quali è possibile convertire i dati di C. Per una spiegazione delle colonne e dei termini della tabella, vedere [conversione di dati da C a tipi di dati SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md).  
  
|Identificatore del tipo SQL|Test|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR<br /><br /> SQL_VARCHAR<br /><br /> SQL_LONGVARCHAR|Lunghezza byte colonna >= 8<br /><br /> Lunghezza in byte colonna < 8<br /><br /> Il valore dei dati non è un'ora valida|n/d<br /><br /> 22001<br /><br /> 22008|  
|SQL_WCHAR<br /><br /> SQL_WVARCHAR<br /><br /> SQL_WLONGVARCHAR|Lunghezza carattere colonna >= 8<br /><br /> Lunghezza carattere colonna < 8<br /><br /> Il valore dei dati non è un'ora valida|n/d<br /><br /> 22001<br /><br /> 22008|  
|SQL_TYPE_TIME|Il valore dei dati è un'ora valida<br /><br /> Il valore dei dati non è un'ora valida|n/d<br /><br /> 22007|  
|SQL_TYPE_TIMESTAMP|Il valore dei dati è un tempo valido [a]<br /><br /> Il valore dei dati non è un'ora valida|n/d<br /><br /> 22007|  
  
 [a] la parte relativa alla data del timestamp è impostata sulla data corrente e la parte frazionaria dei secondi del timestamp è impostata su zero.  
  
 Per informazioni sui valori validi in una struttura SQL_C_TYPE_TIME, vedere tipi di [dati C](../../../odbc/reference/appendixes/c-data-types.md), più indietro in questa appendice.  
  
 Quando i dati dell'ora C vengono convertiti in dati di tipo carattere SQL, i dati di tipo carattere risultanti sono nel formato "*HH*:*mm*:*SS*".  
  
 Il driver ignora il valore di lunghezza/indicatore durante la conversione dei dati dal tipo di dati time C e presuppone che le dimensioni del buffer dei dati siano le dimensioni del tipo di dati time C. Il valore di lunghezza/indicatore viene passato nell'argomento *StrLen_Or_Ind* in **SQLPutData** e nel buffer specificato con l'argomento *StrLen_or_IndPtr* in **SQLBindParameter**. Il buffer di dati viene specificato con l'argomento *DataPtr* in **SQLPutData** e l'argomento *ParameterValuePtr* in **SQLBindParameter**.
