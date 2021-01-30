---
description: Dimensioni della colonna
title: Dimensioni colonna | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], column size
- size of data types [ODBC]
- SQL data types [ODBC], column characteristics
- column size of data types [ODBC]
ms.assetid: 541b83ab-b16d-4714-bcb2-3c3daa9a963b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bae1a1814ae89b2fe4b91ba9c2936df601c03301
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183660"
---
# <a name="column-size"></a>Dimensioni della colonna
La dimensione della colonna (o del parametro) dei tipi di dati numerici è definita come il numero massimo di cifre utilizzate dal tipo di dati della colonna o del parametro o dalla precisione dei dati. Per i tipi di carattere, si tratta della lunghezza in caratteri dei dati. per i tipi di dati binari, le dimensioni della colonna sono definite come lunghezza in byte dei dati. Per i tipi di dati time, timestamp e all Interval, questo è il numero di caratteri nella rappresentazione dei caratteri di questi dati. La dimensione della colonna definita per ogni tipo di dati SQL conciso è illustrata nella tabella seguente.  
  
|Identificatore del tipo SQL|Dimensioni colonne|  
|-------------------------|-----------------|  
|Tutti i tipi di carattere [a], [b]|Dimensioni di colonna definite o massime in caratteri della colonna o del parametro (come contenuto nel campo del descrittore di SQL_DESC_LENGTH). Ad esempio, le dimensioni della colonna di una colonna di tipo carattere a un byte definito come CHAR (10) sono 10.|  
|SQL_DECIMAL SQL_NUMERIC|Numero di cifre definito. La precisione di una colonna definita come NUMERIC (10, 3), ad esempio, è 10.|  
|SQL_BIT [c]|1|  
|SQL_TINYINT [c]|3|  
|SQL_SMALLINT [c]|5|  
|SQL_INTEGER [c]|10|  
|SQL_BIGINT [c]|19 (se firmato) o 20 (se non firmato)|  
|SQL_REAL [c]|7|  
|SQL_FLOAT [c]|15|  
|SQL_DOUBLE [c]|15|  
|Tutti i tipi binari [a], [b]|Lunghezza definita o massima, in byte, della colonna o del parametro. La lunghezza di una colonna definita come BINARY (10), ad esempio, è 10.|  
|SQL_TYPE_DATE [c]|10 (numero di caratteri nel formato *aaaa-mm-gg* ).|  
|SQL_TYPE_TIME [c]|8 (il numero di caratteri nel formato *hh-mm-SS* ) o 9 + *s* (il numero di caratteri nel formato *hh: mm: SS*[. fff...], dove *s* è la precisione dei secondi).|  
|SQL_TYPE_TIMESTAMP|16 (il numero di caratteri nel formato *aaaa-mm-gg hh: mm* )<br /><br /> 19 (il numero di caratteri nel formato *aaaa-mm-gg* *hh: mm: SS* )<br /><br /> oppure<br /><br /> 20 + *s* (il numero di caratteri nel formato *aaaa-mm-gg hh: mm: SS*[. fff...], dove *s* è la precisione dei secondi).|  
|SQL_INTERVAL_SECOND|Dove *p* è la precisione iniziali dell'intervallo e *s* è la precisione dei secondi, *p* (se *s*= 0) o *p* + *s*+ 1 (se *s*>0). [ d|  
|SQL_INTERVAL_DAY_TO_SECOND|Dove *p* è la precisione massima dell'intervallo e *s* è la precisione dei secondi, 9 +*p* (se *s*= 0) o 10 +*p* + *s* (se *s*>0). [ d|  
|SQL_INTERVAL_HOUR_TO_SECOND|Dove *p* è la precisione massima dell'intervallo e *s* è la precisione dei secondi, 6 +*p* (if *s*= 0) o 7 +*p* + *s* (se *s*>0). [ d|  
|SQL_INTERVAL_MINUTE_TO_SECOND|Dove *p* è la precisione iniziali dell'intervallo e *s* è la precisione dei secondi, 3 +*p* (se *s*= 0) o 4 +*p* + *s* (se *s*>0). [ d|  
|SQL_INTERVAL_YEAR SQL_INTERVAL_MONTH SQL_INTERVAL_DAY SQL_INTERVAL_HOUR SQL_INTERVAL_MINUTE|*p*, dove *p* è la precisione principale dell'intervallo. d|  
|SQL_INTERVAL_YEAR_TO_MONTH SQL_INTERVAL_DAY_TO_HOUR|3 +*p*, dove *p* è la precisione principale dell'intervallo. d|  
|SQL_INTERVAL_DAY_TO_MINUTE|6 +*p*, dove *p* è la precisione principale dell'intervallo. d|  
|SQL_INTERVAL_HOUR_TO_MINUTE|3 +*p*, dove *p* è la precisione principale dell'intervallo. d|  
|SQL_GUID|36 (numero di caratteri nel formato *aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee* )|  
  
 [a] per un'applicazione ODBC 1,0 che chiama **SQLSetParam** in un driver ODBC 2,0 e per un'applicazione ODBC 2,0 che chiama **SQLBindParameter** in un driver odbc 1,0, quando \* *StrLen_or_IndPtr* viene SQL_DATA_AT_EXEC per un tipo di SQL_LONGVARCHAR o SQL_LONGVARBINARY, *ColumnSize* deve essere impostato sulla lunghezza totale dei dati da inviare, non sulla precisione definita in questa tabella.  
  
 [b] se il driver non è in grado di determinare la lunghezza della colonna o del parametro per un tipo di variabile, restituisce SQL_NO_TOTAL.  
  
 [c] l'argomento *ColumnSize* di **SQLBindParameter** viene ignorato per questo tipo di dati.  
  
 [d] per le regole generali sulla lunghezza di colonna nei tipi di dati interval, vedere [lunghezza dei tipi di dati interval](../../../odbc/reference/appendixes/interval-data-type-length.md), più indietro in questa appendice.  
  
 I valori restituiti per la dimensione della colonna o del parametro non corrispondono ai valori in un campo di descrizione. I valori possono provenire dal campo SQL_DESC_PRECISION o SQL_DESC_LENGTH, a seconda del tipo di dati, come illustrato nella tabella seguente.  
  
|Tipo SQL|Campo del descrittore corrispondente a<br /><br /> dimensioni della colonna o del parametro|  
|--------------|--------------------------------------------------------------------|  
|Tutti i tipi di carattere e binari|LENGTH|  
|Tutti i tipi numerici|PRECISION|  
|Tutti i tipi DateTime e Interval|LENGTH|  
|SQL_BIT|LENGTH|
