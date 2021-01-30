---
description: 'Da C a SQL: carattere'
title: 'Da C a SQL: carattere | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- character data type [ODBC]
- data conversions from C to SQL types [ODBC], character
- converting data from c to SQL types [ODBC], character
ms.assetid: be66188a-ebdb-4c9e-af72-c379886766fa
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c387c2063bea8f16c7ec5ec90aa0271830f3e5f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212359"
---
# <a name="c-to-sql-character"></a>Da C a SQL: carattere
Gli identificatori per il tipo di dati C ODBC di tipo carattere sono:  
  
 SQL_C_CHAR  
  
 SQL_C_WCHAR  
  
 La tabella seguente illustra i tipi di dati ODBC SQL in cui è possibile convertire i dati di tipo carattere C. Per una spiegazione delle colonne e dei termini della tabella, vedere [conversione di dati da C a tipi di dati SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md).  
  
> [!NOTE]  
>  Quando i dati di tipo carattere C vengono convertiti in dati SQL Unicode, la lunghezza dei dati Unicode deve essere un numero pari.  
  
|Identificatore del tipo SQL|Test|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR<br /><br /> SQL_VARCHAR<br /><br /> SQL_LONGVARCHAR|Lunghezza in byte dei dati <= lunghezza della colonna.<br /><br /> Lunghezza in byte dei dati > lunghezza della colonna.|n/d<br /><br /> 22001|  
|SQL_WCHAR<br /><br /> SQL_WVARCHAR<br /><br /> SQL_WLONGVARCHAR|Lunghezza in caratteri della <dati = lunghezza della colonna.<br /><br /> Lunghezza in caratteri dei dati > lunghezza della colonna.|n/d<br /><br /> 22001|  
|SQL_DECIMAL<br /><br /> SQL_NUMERIC<br /><br /> SQL_TINYINT<br /><br /> SQL_SMALLINT<br /><br /> SQL_INTEGER SQL_BIGINT|Dati convertiti senza troncamento<br /><br /> Dati convertiti con troncamento di cifre frazionarie [e]<br /><br /> La conversione dei dati comporterebbe la perdita di cifre intere (anziché frazionarie) [e]<br /><br /> Il valore dei dati non è un valore *letterale numerico*|n/d<br /><br /> 22001<br /><br /> 22001<br /><br /> 22018|  
|SQL_REAL<br /><br /> SQL_FLOAT<br /><br /> SQL_DOUBLE|I dati sono compresi nell'intervallo del tipo di dati in cui viene convertito il numero<br /><br /> I dati non rientrano nell'intervallo del tipo di dati in cui viene convertito il numero<br /><br /> Il valore dei dati non è un valore *letterale numerico*|n/d<br /><br /> 22003<br /><br /> 22018|  
|SQL_BIT|I dati sono 0 o 1<br /><br /> I dati sono maggiori di 0, minori di 2 e diversi da 1<br /><br /> I dati sono minori di 0 oppure maggiori o uguali a 2<br /><br /> I dati non sono *valori letterali numerici*|n/d<br /><br /> 22001<br /><br /> 22003<br /><br /> 22018|  
|SQL_BINARY<br /><br /> SQL_VARBINARY<br /><br /> SQL_LONGVARBINARY|(Lunghezza in byte dei dati)/2 <= lunghezza in byte della colonna<br /><br /> (Lunghezza in byte dei dati)/2 > lunghezza in byte della colonna<br /><br /> Il valore dei dati non è un valore esadecimale|n/d<br /><br /> 22001<br /><br /> 22018|  
|SQL_TYPE_DATE|Il valore dei dati è un valore *ODBC-date-Literal* valido<br /><br /> Il valore dei dati è un valore *letterale ODBC-timestamp-valore* valido; la parte relativa all'ora è zero<br /><br /> Il valore dei dati è un valore *letterale ODBC-timestamp-valore* valido; la parte relativa all'ora è diversa da zero [a]<br /><br /> Il valore dei dati non è un valore *letterale di data/ora ODBC* valido o *ODBC-timestamp-Literal*|n/d<br /><br /> n/d<br /><br /> 22008<br /><br /> 22018|  
|SQL_TYPE_TIME|Il valore dei dati è un valore *letterale di ora ODBC* valido<br /><br /> Il valore dei dati è un valore *letterale ODBC-timestamp-valore* valido; la parte relativa ai secondi frazionari è zero [b]<br /><br /> Il valore dei dati è un valore *letterale ODBC-timestamp-valore* valido; la parte relativa ai secondi frazionari è diversa da zero [b]<br /><br /> Il valore dei dati non è un valore *letterale ODBC-time-literal* valido o *ODBC-timestamp-Literal*|n/d<br /><br /> n/d<br /><br /> 22008<br /><br /> 22018|  
|SQL_TYPE_TIMESTAMP|Il valore dei dati è un valore *letterale ODBC-timestamp-valore* valido; parte relativa ai secondi frazionari non troncata<br /><br /> Il valore dei dati è un valore *letterale ODBC-timestamp-valore* valido; parte relativa ai secondi frazionari troncata<br /><br /> Il valore dei dati è un valore *ODBC-date-Literal* valido [c]<br /><br /> Il valore dei dati è un valore *letterale di ora ODBC* valido [d]<br /><br /> Il valore dei dati non è un valore *ODBC-date-* Literal, un valore letterale di tipo ODBC- *Time-* Literal o un valore *letterale ODBC-timestamp*|n/d<br /><br /> 22008<br /><br /> n/d<br /><br /> n/d<br /><br /> 22018|  
|Tutti i tipi di intervallo SQL|Il valore dei dati è un *valore di intervallo* valido. nessun troncamento<br /><br /> Il valore dei dati è un *valore di intervallo* valido. il valore in uno dei campi è troncato<br /><br /> Il valore dei dati non è un valore letterale di intervallo valido|n/d<br /><br /> 22015<br /><br /> 22018|  
  
 [a] la parte relativa all'ora del timestamp è troncata.  
  
 [b] la parte relativa alla data del timestamp viene ignorata.  
  
 [c] la parte relativa all'ora del timestamp è impostata su zero.  
  
 [d] la parte relativa alla data del timestamp è impostata sulla data corrente.  
  
 [e] l'origine dati/driver attende in modo efficace fino a quando non viene ricevuta l'intera stringa (anche se i dati di tipo carattere vengono inviati in parti dalle chiamate a **SQLPutData**) prima di tentare di eseguire la conversione.  
  
 Quando i dati di tipo C vengono convertiti in dati numerici, data, ora o timestamp SQL, gli spazi vuoti iniziali e finali vengono ignorati.  
  
 Quando i dati di tipo carattere C vengono convertiti in dati SQL binari, ogni due byte di dati di tipo carattere viene convertito in un singolo byte (8 bit) di dati binari. Ogni due byte di dati di tipo carattere rappresenta un numero in formato esadecimale. Ad esempio, "01" viene convertito in un 00000001 binario e "FF" viene convertito in un 11111111 binario.  
  
 Il driver converte sempre le coppie di cifre esadecimali in byte singoli e ignora il byte di terminazione null. Per questo motivo, se la lunghezza della stringa di caratteri è dispari, l'ultimo byte della stringa (escluso il byte di terminazione null, se presente) non viene convertito.  
  
> [!NOTE]  
>  Gli sviluppatori di applicazioni si sconsigliano di associare i dati di tipo C a un tipo di dati binary SQL. Questa conversione è in genere inefficiente e lenta.
