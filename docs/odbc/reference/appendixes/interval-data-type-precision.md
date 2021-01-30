---
description: Precisione del tipo di dati intervallo
title: Precisione del tipo di dati interval | Microsoft Docs
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
- precision [ODBC]
- interval leading precision [ODBC]
- interval precision [ODBC]
ms.assetid: eb73bd77-2e7e-4498-a266-4d7c990a0d56
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 103b19e43f4ba0cea160bfd53aa178404adfe781
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208533"
---
# <a name="interval-data-type-precision"></a>Precisione del tipo di dati intervallo
La precisione per un tipo di dati intervallo include precisione iniziali, precisione intervallo e precisione secondi.  
  
 Il campo principale di un intervallo è un valore numerico con segno. Il numero massimo di cifre per il campo principale è determinato da una quantità denominata *intervallo di precisione,* che fa parte della dichiarazione del tipo di dati. Ad esempio, la dichiarazione: intervallo ora (5) al minuto ha una precisione leader di intervallo di 5. il campo HOUR può assumere valori compresi tra-99999 e 99999. La precisione principale dell'intervallo è contenuta nel campo SQL_DESC_DATETIME_INTERVAL_PRECISION del record del descrittore.  
  
 L'elenco di campi di cui un tipo di dati interval è costituito è denominato *precisione intervallo*. Non è un valore numerico, come può implicare il termine "precisione". Ad esempio, la precisione dell'intervallo dell'intervallo di tipo giorno a secondo è l'elenco giorno, ora, minuto, secondo. Nessun campo del descrittore che contiene questo valore. la precisione dell'intervallo può essere sempre determinata dal tipo di dati intervallo.  
  
 Qualsiasi tipo di dati intervallo con un secondo campo ha una *precisione in secondi*. Il numero di cifre decimali consentite nella parte frazionaria del valore dei secondi. Questa operazione è diversa rispetto ad altri tipi di dati, dove Precision indica il numero di cifre prima del separatore decimale. La precisione dei secondi di un tipo di dati intervallo è il numero di cifre dopo il separatore decimale. Ad esempio, se la precisione dei secondi è impostata su 6, il numero 123456 nel campo della frazione verrebbe interpretato come. 123456 e il numero 1230 verrebbe interpretato come. 001230. Per gli altri tipi di dati, questa operazione viene definita scala. La precisione in secondi intervallo è contenuta nel campo SQL_DESC_PRECISION del descrittore. Se la precisione del componente per i secondi frazionari del valore dell'intervallo SQL è maggiore di quello che può essere mantenuto nella struttura dell'intervallo C, viene definito dal driver se il valore dei secondi frazionari nell'intervallo SQL viene arrotondato o troncato quando viene convertito nella struttura di intervallo C.  
  
 Quando il campo SQL_DESC_CONCISE_TYPE è impostato su un tipo di dati interval, il campo SQL_DESC_TYPE viene impostato su SQL_INTERVAL e la SQL_DESC_DATETIME_INTERVAL_CODE viene impostata sul codice per il tipo di dati interval. Il campo SQL_DESC_DATETIME_INTERVAL_PRECISION viene impostato automaticamente sulla precisione principale dell'intervallo predefinito di 2 e il campo SQL_DESC_PRECISION viene impostato automaticamente sulla precisione dei secondi di intervallo predefinita di 6. Se uno di questi valori non è appropriato, l'applicazione deve impostare in modo esplicito il campo del descrittore tramite una chiamata a **SQLSetDescField**.
