---
description: Tipi di dati Microsoft Excel
title: Tipi di dati di Microsoft Excel | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], Excel driver
- Excel data types [ODBC]
- Jet-based ODBC drivers [ODBC], Excel driver
- desktop database drivers [ODBC], Excel driver
- ODBC desktop database drivers [ODBC], Excel driver
- Excel driver [ODBC], data types
ms.assetid: 7b44c8e5-0bc3-4912-8a5d-56f4d5562fe6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a6342706e2ab9b66db9d0410b31573d89bdf1d7c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212126"
---
# <a name="microsoft-excel-data-types"></a>Tipi di dati Microsoft Excel
Nella tabella seguente viene illustrato come viene eseguito il mapping dei tipi di dati del driver Microsoft Excel ai tipi di dati SQL ODBC. Il driver Microsoft Excel assegna questi tipi di dati alle colonne nelle tabelle di Microsoft Excel in base ai dati nella colonna.  
  
|Tipo di dati di Microsoft Excel|Tipo di dati ODBC|  
|-------------------------------|--------------------|  
|CURRENCY|SQL_NUMERIC|  
|DATETIME|SQL_TIMESTAMP|  
|LOGICO|SQL_BIT|  
|NUMBER|SQL_DOUBLE|  
|TEXT|SQL_VARCHAR|  
  
> [!NOTE]  
>  **SQLGetTypeInfo** restituisce tipi di dati SQL ODBC. Tutte le conversioni nell'Appendice D di *ODBC Programmer ' s Reference* sono supportate per i tipi di dati ODBC SQL elencati in precedenza in questo argomento.  
  
 Nella tabella seguente vengono illustrate le limitazioni relative ai tipi di dati di Microsoft Excel.  
  
|Tipo di dati|Descrizione|  
|---------------|-----------------|  
|Dati crittografati|Il driver Microsoft Excel non è in grado di leggere i dati crittografati.|  
|Stringhe di errore|Il driver Microsoft Excel non può restituire una stringa di caratteri per i valori di errore di Microsoft Excel (#N/A!, #VALUE!, #REF!, #DIV/0!, #NUM!, #NAME? e #NULL!), ma restituisce invece un valore NULL.|  
|LOGICO|Il valore in una colonna logica viene restituito in un buffer SQL_C_CHAR come 0 o 1.|  
|NUMBER|Se viene creata una colonna Integer, è possibile immettere numeri troppo grandi per il tipo di dati integer e i dati contenenti valori non integer possono essere inseriti, a causa del risultato che la colonna può essere convertita in SQL_DOUBLE.|  
|TEXT|Quando le righe di una colonna contengono più di un tipo di dati di Microsoft Excel, il driver ODBC Microsoft Excel assegna il tipo di dati SQL_VARCHAR alla colonna. Si verifica un'eccezione: se la colonna contiene solo due o tre tipi di dati DateTime (DATE, TIME e DATETIME), il driver ODBC di Microsoft Excel assegna il tipo di dati SQL_TIMESTAMP alla colonna.<br /><br /> La creazione di una colonna di testo di zero o di lunghezza non specificata restituisce effettivamente una colonna a 255 byte.<br /><br /> Un valore letterale stringa di caratteri può contenere qualsiasi carattere ANSI (1-255 decimale). Utilizzare due virgolette singole consecutive (") per rappresentare una virgoletta singola (').<br /><br /> L'inserimento di un valore NULL in una colonna con un tipo di dati diverso da SQL_VARCHAR provocherà la modifica del tipo di dati della colonna SQL_VARCHAR.|  
  
 Per altre limitazioni sui tipi di dati, vedere [limitazioni del tipo di dati](../../odbc/microsoft/data-type-limitations.md).
