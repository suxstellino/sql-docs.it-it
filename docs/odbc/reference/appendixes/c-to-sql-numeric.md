---
description: 'Da C a SQL: dati numerici'
title: 'Da C a SQL: Numeric | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- numeric data type [ODBC], converting
- data conversions from C to SQL types [ODBC], numeric
- converting data from c to SQL types [ODBC], numeric
ms.assetid: af4095ff-06c3-4b04-83bf-19f9ee098dc2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b825725ada434a72e14f9631504ee343951d6a4a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207817"
---
# <a name="c-to-sql-numeric"></a>Da C a SQL: dati numerici
Gli identificatori per i tipi di dati ODBC C numerici sono:  
  
 SQL_C_STINYINT  
  
 SQL_C_SLONG  
  
 SQL_C_UTINYINT  
  
 SQL_C_ULONG  
  
 SQL_C_TINYINT  
  
 SQL_C_LONG  
  
 SQL_C_SSHORT  
  
 SQL_C_FLOAT  
  
 SQL_C_USHORT  
  
 SQL_C_DOUBLE  
  
 SQL_C_SHORT  
  
 SQL_C_NUMERIC  
  
 SQL_C_SBIGINT  
  
 SQL_C_UBIGINT  
  
 Nella tabella seguente sono illustrati i tipi di dati ODBC SQL in cui è possibile convertire i dati numerici C. Per una spiegazione delle colonne e dei termini della tabella, vedere [conversione di dati da C a tipi di dati SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md).  
  
|Identificatore del tipo SQL|Test|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR<br /><br /> SQL_VARCHAR<br /><br /> SQL_LONGVARCHAR|Numero di cifre <= lunghezza in byte colonna<br /><br /> Numero di cifre > lunghezza in byte della colonna|n/d<br /><br /> 22001|  
|SQL_WCHAR<br /><br /> SQL_WVARCHAR<br /><br /> SQL_WLONGVARCHAR|Numero di caratteri <= Lunghezza carattere colonna<br /><br /> Numero di caratteri > Lunghezza carattere colonna|n/d<br /><br /> 22001|  
|SQL_DECIMAL [b]<br /><br /> SQL_NUMERIC [b]<br /><br /> SQL_TINYINT [b]<br /><br /> SQL_SMALLINT [b]<br /><br /> SQL_INTEGER [b]<br /><br /> SQL_BIGINT [b]|Dati convertiti senza troncamento o con troncamento di cifre frazionarie<br /><br /> Dati convertiti con troncamento di cifre intere|n/d<br /><br /> 22003|  
|SQL_REAL<br /><br /> SQL_FLOAT<br /><br /> SQL_DOUBLE|I dati sono compresi nell'intervallo del tipo di dati in cui viene convertito il numero<br /><br /> I dati non rientrano nell'intervallo del tipo di dati in cui viene convertito il numero|n/d<br /><br /> 22003|  
|SQL_BIT|I dati sono 0 o 1<br /><br /> I dati sono maggiori di 0, minori di 2 e diversi da 1<br /><br /> I dati sono minori di 0 oppure maggiori o uguali a 2|n/d<br /><br /> 22001<br /><br /> 22003|  
|SQL_INTERVAL_YEAR [a]<br /><br /> SQL_INTERVAL_MONTH [a]<br /><br /> SQL_INTERVAL_DAY [a]<br /><br /> SQL_INTERVAL_HOUR [a]<br /><br /> SQL_INTERVAL_MINUTE [a]<br /><br /> SQL_INTERVAL_SECOND [a]|I dati non sono troncati.<br /><br /> Dati troncati.|n/d<br /><br /> 22015|  
  
 [a] queste conversioni sono supportate solo per i tipi di dati numerici esatti (SQL_C_STINYINT, SQL_C_UTINYINT, SQL_C_SSHORT, SQL_C_USHORT, SQL_C_SLONG, SQL_C_ULONG o SQL_C_NUMERIC). Non sono supportate per i tipi di dati numerici approssimati (SQL_C_FLOAT o SQL_C_DOUBLE). I tipi di dati C numerici esatti non possono essere convertiti in un tipo SQL intervallo il cui valore di precisione intervallo non è un singolo campo.  
  
 [b] per il caso "n/a", un driver può restituire facoltativamente SQL_SUCCESS_WITH_INFO e 01S07 quando si verifica un troncamento frazionario.  
  
 Il driver ignora il valore di lunghezza/indicatore durante la conversione dei dati dai tipi di dati C numerici e presuppone che le dimensioni del buffer dei dati siano le dimensioni del tipo di dati C numerico. Il valore di lunghezza/indicatore viene passato nell'argomento *StrLen_Or_Ind* in **SQLPutData** e nel buffer specificato con l'argomento *StrLen_or_IndPtr* in **SQLBindParameter**. Il buffer di dati viene specificato con l'argomento *DataPtr* in **SQLPutData** e l'argomento *ParameterValuePtr* in **SQLBindParameter**.
