---
description: Tipi di dati parametro
title: Tipi di dati dei parametri | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], parameters
- parameter data type [ODBC]
- minimum SQL syntax supported [ODBC]
- ODBC drivers [ODBC], minimum SQL syntax supported
ms.assetid: fd7e99d8-d26a-408c-9733-6ffccde99f75
author: David-Engel
ms.author: v-daenge
ms.reviewer: ''
ms.openlocfilehash: fbc01ee7f6c4e7cd3ca7dbe4deb6e78e5ff78449
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160832"
---
# <a name="parameter-data-types"></a>Tipi di dati parametro
Anche se ogni parametro specificato con **SQLBindParameter** viene definito usando un tipo di dati SQL, i parametri in un'istruzione SQL non hanno un tipo di dati intrinseco. Pertanto, i marcatori di parametro possono essere inclusi in un'istruzione SQL solo se i relativi tipi di dati possono essere dedotti da un altro operando nell'istruzione. Ad esempio, in un'espressione aritmetica come? + COLUMN1, il tipo di dati del parametro può essere dedotto dal tipo di dati della colonna denominata rappresentata da COLUMN1. Se non è possibile determinare il tipo di dati, un'applicazione non può utilizzare un marcatore di parametro.  
  
 Nella tabella seguente viene descritto il modo in cui un tipo di dati viene determinato per diversi tipi di parametri, in base a SQL-92. Per una specifica più completa sull'inferenza del tipo di parametro quando vengono utilizzate altre clausole SQL, vedere la specifica SQL-92.  
  
|Posizione del parametro|Tipo di dati assunto|  
|---------------------------|-----------------------|  
|Un operando di un operatore aritmetico o di confronto binario|Uguale all'altro operando|  
|Primo operando in una clausola **between**|Uguale al secondo operando|  
|Il secondo o il terzo operando in una clausola **between**|Uguale al primo operando|  
|Espressione utilizzata con **in**|Uguale al primo valore o alla colonna risultato della sottoquery|  
|Valore utilizzato con **in**|Uguale all'espressione o al primo valore se nell'espressione è presente un marcatore di parametro|  
|Valore del criterio utilizzato con **like**|VARCHAR|  
|Valore di aggiornamento utilizzato con **Update**|Uguale alla colonna di aggiornamento|
