---
description: SQL_ARD_TYPE
title: SQL_ARD_TYPE | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], pseudo-type identifiers
- pseudo-type identifiers [ODBC], SQL_ARD_TYPE
- SQL_ARD_TYPE [ODBC]
ms.assetid: 8d87ca10-f955-4284-8689-e9f4cc31e7ae
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 43d8ce804fc66fd8d9a305868cf1a8b22cf8ff7d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187069"
---
# <a name="sql_ard_type"></a>SQL_ARD_TYPE
L'identificatore del tipo di SQL_ARD_TYPE viene usato per indicare che i dati in un buffer saranno del tipo specificato nel campo SQL_DESC_CONCISE_TYPE di ARD. SQL_ARD_TYPE viene immesso nell'argomento *targetType* di una chiamata a **SQLGetData** anziché un tipo di dati specifico e consente a un'applicazione di modificare il tipo di dati del buffer modificando il campo del descrittore. Questo valore associa il tipo di dati del buffer *\* TargetValuePtr* al campo del descrittore. (SQL_ARD_TYPE non viene immesso in una chiamata a **SQLBindCol** o **SQLBindParameter** perché il tipo del buffer associato è già associato ai campi SQL_DESC_TYPE e SQL_DESC_CONCISE_TYPE e può essere modificato in qualsiasi momento modificando uno di questi campi).  
  
 L'identificatore del tipo di SQL_ARD_TYPE può essere utilizzato per specificare valori non predefiniti per la precisione e i secondi di precisione iniziali dei tipi di dati intervallo e per i valori di precisione e scala per il tipo di dati SQL_C_NUMERIC. Per ulteriori informazioni, vedere [override della precisione iniziali e dei secondi predefinita per i tipi di dati intervallo](../../../odbc/reference/appendixes/overriding-default-leading-and-seconds-precision-for-interval-data-types.md) e [override della precisione e della scala predefinite per i tipi di dati numerici](../../../odbc/reference/appendixes/overriding-default-precision-and-scale-for-numeric-data-types.md), più avanti in questa appendice.
