---
description: Cifre decimali
title: Cifre decimali | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- size of data types [ODBC]
- decimal digits of data types [ODBC]
- data types [ODBC], decimal digits
- SQL data types [ODBC], column characteristics
ms.assetid: 07f3d1fc-b4ee-4693-b342-330b2231b6d0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f7a4d37601891c26ce4d59a9bfd788ce1c702ce5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194906"
---
# <a name="decimal-digits"></a>Cifre decimali
Le *cifre decimali* dei tipi di dati Decimal e numeric sono definite come il numero massimo di cifre a destra del separatore decimale o la scala dei dati. Per le colonne o i parametri con numero a virgola mobile approssimativo, la scala non è definita perché il numero di cifre a destra del separatore decimale non è fisso. Per i dati DateTime o Interval contenenti un componente secondi, le cifre decimali sono definite come il numero di cifre a destra del separatore decimale nel componente secondi dei dati.  
  
 Per i tipi di dati SQL_DECIMAL e SQL_NUMERIC, la scala massima corrisponde in genere alla precisione massima. Tuttavia, alcune origini dati impongono un limite separato per la scalabilità massima. Per determinare le scale minime e massime consentite per un tipo di dati, un'applicazione chiama **SQLGetTypeInfo**.  
  
 Le cifre decimali definite per ogni tipo di dati SQL conciso sono illustrate nella tabella seguente.  
  
|Tipo SQL|Cifre decimali|  
|--------------|--------------------|  
|Tutti i tipi di carattere e binari [a]|n/d|  
|SQL_DECIMAL<br />SQL_NUMERIC|Numero definito di cifre a destra della virgola decimale. La scala di una colonna definita come NUMERIC (10, 3), ad esempio, è 3. Può trattarsi di un numero negativo per supportare l'archiviazione di numeri molto grandi senza usare la notazione esponenziale. ad esempio, "12000" può essere archiviato come "12" con una scala di-3.|  
|Tutti i tipi numerici esatti diversi da SQL_DECIMAL e SQL_NUMERIC [a]|0|  
|Tutti i tipi di dati approssimati [a]|n/d|  
|SQL_TYPE_DATE e tutti i tipi di intervallo senza componente secondi [a]|n/d|  
|Tutti i tipi DateTime eccetto SQL_TYPE_DATE e tutti i tipi di intervallo con un componente secondi|Numero di cifre a destra del separatore decimale nella parte relativa ai secondi del valore (secondi frazionari). Questo numero non può essere negativo.|  
|SQL_GUID|n/d|  
  
 [a] l'argomento *DecimalDigits* di **SQLBindParameter** viene ignorato per questo tipo di dati.  
  
 I valori restituiti per le cifre decimali non corrispondono ai valori in un campo di descrizione. I valori possono provenire dal campo SQL_DESC_SCALE o SQL_DESC_PRECISION, a seconda del tipo di dati, come illustrato nella tabella seguente.  
  
|Tipo SQL|Campo del descrittore corrispondente a<br /><br /> cifre decimali|  
|--------------|----------------------------------------------------------|  
|Tutti i tipi di carattere e binari|n/d|  
|Tutti i tipi numerici esatti|SCALE|  
|SQL_BIT|n/d|  
|Tutti i tipi numerici approssimati|n/d|  
|Tutti i tipi DateTime|PRECISION|  
|Tutti i tipi di intervallo con un componente secondi|PRECISION|  
|Tutti i tipi di intervallo senza componente secondi|n/d|
