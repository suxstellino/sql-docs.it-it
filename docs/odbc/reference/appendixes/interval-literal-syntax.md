---
description: Sintassi dei valori letterali intervallo
title: Sintassi valore letterale intervallo | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- literals [ODBC], interval
- interval literals [ODBC]
- ODBC literals [ODBC], interval
ms.assetid: 2f2d22c1-51d6-4055-9f5a-53bc31e9fea0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5a58ddbde1be09f16fec79fcff13147a805febfd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208532"
---
# <a name="interval-literal-syntax"></a>Sintassi dei valori letterali intervallo
La sintassi seguente viene utilizzata per i valori letterali di intervallo in ODBC.  
  
 *intervallo-valore letterale:: = intervallo* [+*&#124;*-] intervallo *-stringa di intervallo-qualificatore*  
  
 *intervallo-stringa* :: = *virgolette* { *year-month-Literal* &#124; *giorno-ora-valore letterale* *}*  
  
 *year-month-Literal* :: = *years-value* &#124; [*years-value* -] *months-value*  
  
 *giorno-ora-valore letterale* :: = data/ora &#124; *intervallo* di *tempo*  
  
 *giorno-ora-intervallo* :: = *giorni-valore* [*ore-valore* [:*minuti-valore*[:*secondi-valore*]]]  
  
 *time-interval* :: = *hours-valore* [:*minuti-valore* [:*secondi-valore* ]]  
  
 &#124; *minuti-valore* [:*secondi-valore* ]  
  
 &#124; *secondi-valore*  
  
 *years-valore* :: = *DateTime-value*  
  
 *months-valore* :: = *DateTime-value*  
  
 *giorni-valore* :: = *DateTime-value*  
  
 *hours-valore* :: = *DateTime-value*  
  
 *minuti-valore* :: = *DateTime-value*  
  
 *secondi-valore* :: = *secondi-valore intero* [. [ *secondi-frazione*] ]  
  
 *secondi-Integer-valore* :: = *senza segno-intero*  
  
 *secondi-frazione* :: = *senza segno-intero*  
  
 *DateTime-valore* :: = *senza segno-intero*  
  
 *Interval-Qualifier* :: = *Start-* Field per *End-Field* &#124; *Single-DateTime-field*  
  
 *Start-Field* :: = *non-Second-DateTime-field* [(*intervallo-iniziale-precisione campo* )]  
  
 *End-Field* :: = *non-Second-datetime-Field* &#124; Second [(*intervallo-frazioni-secondi-precisione*)]  
  
 *Single-DateTime-field* :: = *non-Second-DateTime-field* [(*Intervallo-primo-campo-precisione*)] &#124; secondo [(intervallo-*primo-campo-* precisione [, (*intervallo-frazione-secondi-precisione*)]  
  
 *DateTime-field* :: = *non-Second-datetime-Field* &#124; secondo  
  
 *non secondo-DateTime-field* :: = anno &#124; mese &#124; giorno &#124; ora &#124; minuto  
  
 *intervallo-frazionario-secondi-precisione* :: = *senza segno-intero*  
  
 *intervallo-leader di campo-precisione* :: = *senza segno-intero*  
  
 *quote* :: ='  
  
 *unsigned-integer* :: = *digit...*
