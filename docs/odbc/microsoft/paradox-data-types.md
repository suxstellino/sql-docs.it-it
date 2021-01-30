---
description: Tipi di dati Paradox
title: Tipi di dati Paradox | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC desktop database drivers [ODBC], Paradox driver
- desktop database drivers [ODBC], Paradox driver
- Paradox data types [ODBC]
- Jet-based ODBC drivers [ODBC], Paradox driver
- data types [ODBC], Paradox driver
- Paradox driver [ODBC], data types
ms.assetid: 0c9e5d21-9321-49f8-a055-69459e1c9c85
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ee49ff2df24a57daa481ea650f57c09a7712400e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203512"
---
# <a name="paradox-data-types"></a>Tipi di dati Paradox
Il driver ODBC Paradox esegue il mapping dei tipi di dati Paradox ai tipi di dati SQL ODBC. La tabella seguente elenca tutti i tipi di dati Paradox e Mostra i tipi di dati ODBC SQL a cui è stato eseguito il mapping.  
  
|Tipo di dati Paradox|Tipo di dati ODBC|  
|-----------------------|--------------------|  
|ALFANUMERICI|SQL_VARCHAR|  
|INCREMENTO AUTOMATICO [1]|SQL_INTEGER|  
|BCD [1]|SQL_DOUBLE|  
|BYTE [1]|SQL_BINARY|  
|DATE|SQL_DATE|  
|IMMAGINE [2]|SQL_LONGVARBINARY|  
|LOGICA [1]|SQL_BIT|  
|LONG [1]|SQL_INTEGER|  
|MEMO [2]|SQL_LONGVARCHAR|  
|DENARO [1]|SQL_DOUBLE|  
|NUMBER|SQL_DOUBLE|  
|SHORT|SQL_SMALLINT|  
|TEMPO [1]|SQL_TIMESTAMP|  
|TIMESTAMP [1]|SQL_TIMESTAMP|  
  
 [1] valido solo per le versioni di Paradox 5. *x*.  
  
 [2] valido solo per Paradox versione 4. *x* e 5. *x*.  
  
> [!NOTE]  
>  **SQLGetTypeInfo** restituisce tipi di dati SQL ODBC. Tutte le conversioni nell'Appendice D di *ODBC Programmer ' s Reference* sono supportate per i tipi di dati ODBC SQL elencati in precedenza in questo argomento.  
  
 Nella tabella seguente vengono illustrate le limitazioni relative ai tipi di dati Paradox.  
  
|Tipo di dati|Descrizione|  
|---------------|-----------------|  
|ALFANUMERICI|La creazione di una colonna ALFANUMERICa di lunghezza zero o non specificata restituisce effettivamente una colonna a 255 byte.|  
|BYTES|Se si inserisce NULL in una colonna binaria con il driver Paradox5, questo viene modificato in 0.|  
|LONG|Valore negativo massimo supportato dal driver Paradox per il tipo di dati Long in Paradox 5. *x* non è-2 ^ 31 (-2147483648), perché è necessario eseguire il mapping lungo al tipo di dati ODBC SQL_INTEGER. Il valore massimo negativo supportato per Long è in realtà-2 ^ 31 + 1 (-2147483647).|  
|timestamp|Quando un valore viene inserito in una colonna TIMESTAMP dal driver Paradox, successivamente recuperato dalla colonna, il valore recuperato può essere diverso dal valore inserito per un valore pari a 1 secondo a causa dell'arrotondamento.|  
  
 Per altre limitazioni sui tipi di dati, vedere [limitazioni del tipo di dati](../../odbc/microsoft/data-type-limitations.md).
