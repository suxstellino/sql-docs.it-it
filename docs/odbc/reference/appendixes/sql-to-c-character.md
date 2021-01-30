---
description: 'Da SQL a C: carattere'
title: 'Da SQL a C: carattere | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from SQL to C types [ODBC], character
- character data type [ODBC]
- data conversions from SQL to C types [ODBC], character
ms.assetid: 7fdb7f38-b64d-48f2-bcb4-1ca96b2bbdb6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 78bbf45362216f8fb2a0706977347219ac6ac28b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203065"
---
# <a name="sql-to-c-character"></a>Da SQL a C: carattere

Gli identificatori per i tipi di dati SQL ODBC character sono i seguenti:

- SQL_CHAR
- SQL_VARCHAR
- SQL_LONGVARCHAR
- SQL_WCHAR
- SQL_WVARCHAR
- SQL_WLONGVARCHAR

Nella tabella seguente sono illustrati i tipi di dati ODBC C in cui è possibile convertire i dati SQL di tipo carattere. Per una spiegazione delle colonne e dei termini della tabella, vedere [conversione di dati da SQL a tipi di dati C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md).  

|Identificatore di tipo C|Test|TargetValuePtr|StrLen_or_IndPtr|SQLSTATE|
|:----------------|:---|:-------------|:---------------|:-------|
|SQL_C_CHAR|Lunghezza in byte dei dati < *bufferLength*<br /><br /> Lunghezza in byte dei dati >= *bufferLength*|Data<br /><br /> Dati troncati|Lunghezza dei dati in byte<br /><br /> Lunghezza dei dati in byte|n/d<br /><br /> 01004|  
|SQL_C_WCHAR|Lunghezza in caratteri dei dati < *bufferLength*<br /><br /> Lunghezza in caratteri della >dati = *bufferLength*|Data<br /><br /> Dati troncati|Lunghezza dei dati in caratteri<br /><br /> Lunghezza dei dati in caratteri|n/d<br /><br /> 01004|  
|SQL_C_STINYINT SQL_C_UTINYINT SQL_C_TINYINT SQL_C_SBIGINT SQL_C_UBIGINT SQL_C_SSHORT SQL_C_USHORT SQL_C_SHORT SQL_C_SLONG SQL_C_ULONG SQL_C_LONG SQL_C_NUMERIC|Dati convertiti senza troncamento [b]<br /><br /> Dati convertiti con troncamento di cifre frazionarie [a]<br /><br /> La conversione dei dati comporterebbe la perdita di cifre intere (anziché frazionarie) [a]<br /><br /> I dati non sono *valori letterali numerici*[b]|Data<br /><br /> Dati troncati<br /><br /> Non definito<br /><br /> Non definito|Numero di byte del tipo di dati C<br /><br /> Numero di byte del tipo di dati C<br /><br /> Non definito<br /><br /> Non definito|n/d<br /><br /> 01S07<br /><br /> 22003<br /><br /> 22018|  
|SQL_C_FLOAT SQL_C_DOUBLE|I dati sono compresi nell'intervallo del tipo di dati in cui viene convertito il numero [a]<br /><br /> I dati non rientrano nell'intervallo del tipo di dati in cui viene convertito il numero [a]<br /><br /> I dati non sono *valori letterali numerici*[b]|Data<br /><br /> Non definito<br /><br /> Non definito|Dimensioni del tipo di dati C<br /><br /> Non definito<br /><br /> Non definito|n/d<br /><br /> 22003<br /><br /> 22018|  
|SQL_C_BIT|I dati sono 0 o 1<br /><br /> I dati sono maggiori di 0, minori di 2 e diversi da 1<br /><br /> I dati sono minori di 0 oppure maggiori o uguali a 2<br /><br /> I dati non sono *valori letterali numerici*|Data<br /><br /> Dati troncati<br /><br /> Non definito<br /><br /> Non definito|1 [b]<br /><br /> 1 [b]<br /><br /> Non definito<br /><br /> Non definito|n/d<br /><br /> 01S07<br /><br /> 22003<br /><br /> 22018|  
|SQL_C_BINARY|Lunghezza in byte dei dati <= *bufferLength*<br /><br /> Lunghezza in byte dei dati > *bufferLength*|Data<br /><br /> Dati troncati|Lunghezza dei dati in byte<br /><br /> Lunghezza dei dati|n/d<br /><br /> 01004|  
|SQL_C_TYPE_DATE|Il valore dei dati è un *valore di data* valido [a]<br /><br /> Il valore dei dati è un *valore timestamp* valido. la parte relativa all'ora è zero [a]<br /><br /> Il valore dei dati è un *valore timestamp* valido. la parte relativa all'ora è diversa da zero [a], [c]<br /><br /> Il valore dei dati non è un valore di *Data/ora* valido o un valore *timestamp*[a]|Data<br /><br /> Data<br /><br /> Dati troncati<br /><br /> Non definito|6 [b]<br /><br /> 6 [b]<br /><br /> 6 [b]<br /><br /> Non definito|n/d<br /><br /> n/d<br /><br /> 01S07<br /><br /> 22018|  
|SQL_C_TYPE_TIME|Il valore dei dati è un *valore di ora valido e il valore dei secondi frazionari è 0*[a]<br /><br /> Il valore dei dati è un valore *timestamp valido o un valore di ora valido*; la parte frazionaria di secondi è zero [a], [d]<br /><br /> Il valore dei dati è un *valore timestamp* valido. la parte relativa ai secondi frazionari è diversa da zero [a], [d], [e]<br /><br /> Il valore dei dati non è un valore *temporale* o un valore di *timestamp* valido [a]|Data<br /><br /> Data<br /><br /> Dati troncati<br /><br /> Non definito|6 [b]<br /><br /> 6 [b]<br /><br /> 6 [b]<br /><br /> Non definito|n/d<br /><br /> n/d<br /><br /> 01S07<br /><br /> 22018|  
|SQL_C_TYPE_TIMESTAMP|Il valore dei dati è un valore *timestamp valido o un valore di ora valido*; parte relativa ai secondi frazionari non troncata [a]<br /><br /> Il valore dei dati è un valore *timestamp valido o un valore di ora valido*; parte relativa ai secondi frazionari troncata [a]<br /><br /> Il valore dei dati è un *valore di data* valido [a]<br /><br /> Il valore dei dati è un *valore temporale* valido [a]<br /><br /> Il valore dei dati non è un valore di *Data*, valore *temporale* o *valore di timestamp* valido [a]|Data<br /><br /> Dati troncati<br /><br /> Dati [f]<br /><br /> Dati [g]<br /><br /> Non definito|16 [b]<br /><br /> 16 [b]<br /><br /> 16 [b]<br /><br /> 16 [b]<br /><br /> Non definito|n/d<br /><br /> 01S07<br /><br /> n/d<br /><br /> n/d<br /><br /> 22018|  
|Tutti i tipi di intervallo C|Il valore dei dati è un *valore di intervallo* valido. nessun troncamento<br /><br /> Il valore dei dati è un *valore di intervallo* valido. troncamento di uno o più campi finali<br /><br /> I dati sono di intervallo valido. precisione significativa campo principale persa<br /><br /> Il valore dei dati non è un valore di intervallo valido|Data<br /><br /> Dati troncati<br /><br /> Non definito<br /><br /> Non definito|Lunghezza dei dati in byte<br /><br /> Lunghezza dei dati in byte<br /><br /> Non definito<br /><br /> Non definito|n/d<br /><br /> 01S07<br /><br /> 22015<br /><br /> 22018|  
|&nbsp;|&nbsp;|&nbsp;|&nbsp;|&nbsp;|

 [a] il valore di *bufferLength* viene ignorato per la conversione. Il driver presuppone che le dimensioni di **TargetValuePtr* siano le dimensioni del tipo di dati C.  
  
 [b] corrisponde alla dimensione del tipo di dati C corrispondente.  
  
 [c] la parte relativa all'ora del *valore timestamp* viene troncata.  
  
 [d] la parte relativa alla data del *valore timestamp* viene ignorata.  
  
 [e] la parte relativa ai secondi frazionari del timestamp viene troncata.  
  
 [f] i campi di tempo della struttura del timestamp sono impostati su zero.  
  
 [g] i campi data della struttura timestamp vengono impostati sulla data corrente.  

**Spazi aggiuntivi**

Gli spazi iniziali e finali vengono ignorati quando i dati di tipo carattere SQL vengono convertiti in uno dei tipi seguenti:

- Data
- NUMERIC
- time
- timestamp
- dati intervallo C
