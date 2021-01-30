---
description: Vincoli del calendario gregoriano
title: Vincoli del calendario gregoriano | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], Gregorian calendar
- Gregorian calendar [ODBC]
ms.assetid: 70667410-c582-4369-8e06-9d98e21cd2bf
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: dbab6e1ef26843807c67e3d3daba9a66e054d8f1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165410"
---
# <a name="constraints-of-the-gregorian-calendar"></a>Vincoli del calendario gregoriano
I tipi di dati date e DateTime e i campi finali dei tipi di dati interval devono essere conformi ai vincoli del calendario gregoriano. Questi vincoli sono i seguenti:  
  
-   Il valore del campo month deve essere compreso tra 1 e 12 inclusi.  
  
-   Il valore del campo Day deve essere compreso tra 1 e il numero di giorni del mese. Il numero di giorni del mese è determinato dai valori dei campi anno e mesi e può essere 28, 29, 30 o 31. Il numero di giorni del mese può anche variare a seconda che si tratti di un anno bisestile.  
  
-   Il valore del campo hour deve essere compreso tra 0 e 23, inclusi.  
  
-   Il valore del campo minuti deve essere compreso tra 0 e 59, inclusi.  
  
-   Per il campo dei secondi finali dei tipi di dati intervallo, il valore del campo secondi deve essere compreso tra 0 e 59,9 (*n*), inclusi, dove *n* è il numero di cifre nella precisione in secondi frazionari.  
  
-   Per il campo secondi finali dei tipi di dati DateTime, il valore del campo secondi deve essere compreso tra 0 e 61,9 (*n*), inclusi, dove *n* specifica il numero di cifre "9" e il valore di *n* è la precisione dei secondi frazionari. (L'intervallo di secondi consente un massimo di due secondi intercalari per mantenere la sincronizzazione del tempo siderale).
