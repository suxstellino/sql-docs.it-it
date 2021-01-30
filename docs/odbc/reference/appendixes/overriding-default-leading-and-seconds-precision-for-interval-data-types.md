---
description: Override della precisione iniziale e in secondi predefinita per i tipi di dati intervallo
title: Eseguire l'override della precisione iniziali e secondi per i tipi di dati intervallo | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], interval data types
- precision [ODBC], interval data types
- seconds precision [ODBC]
- interval data type [ODBC], precision
- interval leading precision [ODBC]
- interval precision [ODBC]
ms.assetid: 3d65493f-dce7-4d29-9f59-c63a4e47918c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a79bf67c0a8ccc988d9e171be9760be98c1dc5fa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160834"
---
# <a name="overriding-default-leading-and-seconds-precision-for-interval-data-types"></a>Override della precisione iniziale e in secondi predefinita per i tipi di dati intervallo
Quando il campo SQL_DESC_TYPE di un ARD è impostato su un tipo DateTime o Interval C, chiamando **SQLBindCol** o **SQLSetDescField**, il campo SQL_DESC_PRECISION, che contiene la precisione intervallo secondi, viene impostato sui valori predefiniti seguenti:  
  
-   6 per timestamp e tutti i tipi di dati intervallo con un secondo componente.  
  
-   0 per tutti gli altri tipi di dati.  
  
 Per tutti i tipi di dati intervallo, il campo del descrittore SQL_DESC_DATETIME_INTERVAL_PRECISION, che contiene la precisione del campo iniziali dell'intervallo, è impostato su un valore predefinito di 2.  
  
 Quando il campo SQL_DESC_TYPE in un oggetto APD è impostato su un tipo DateTime o Interval C, chiamando **SQLBindParameter** o **SQLSetDescField**, i campi SQL_DESC_PRECISION e SQL_DESC_DATETIME_INTERVAL_PRECISION di APD vengono impostati sul valore predefinito specificato in precedenza. Questo vale per i parametri di input ma non per i parametri di input/output o output.  
  
 Una chiamata a **SQLSetDescRec** imposta la precisione principale dell'intervallo sull'impostazione predefinita, ma imposta la precisione in secondi intervallo (nel campo SQL_DESC_PRECISION) sul valore dell'argomento *Precision* .  
  
 Se una delle impostazioni predefinite fornite in precedenza non è accettabile per un'applicazione, l'applicazione deve impostare il SQL_DESC_PRECISION o SQL_DESC_DATETIME_INTERVAL_PRECISION campo chiamando **SQLSetDescField**.  
  
 Se l'applicazione chiama **SQLGetData** per restituire dati in un tipo DateTime o Interval C, vengono utilizzate la precisione iniziali dell'intervallo e i secondi di intervallo predefiniti. Se il valore predefinito non è accettabile, l'applicazione deve chiamare **SQLSetDescField** per impostare il campo del descrittore o **SQLSetDescRec** per impostare SQL_DESC_PRECISION. La chiamata a **SQLGetData** deve avere un *targetType* di SQL_ARD_TYPE per usare i valori nei campi del descrittore.  
  
 Quando viene chiamato **SQLPutData** , viene letta la precisione del tempo di precisione e di intervallo per i secondi di intervallo, dai campi del record del descrittore che corrispondono al parametro o alla colonna data-at-execution, ovvero i campi APD per le chiamate a **SQLExecute** o **SQLExecDirect** o i campi ARD per le chiamate a **SQLBulkOperations** o **SQLSetPos**.
