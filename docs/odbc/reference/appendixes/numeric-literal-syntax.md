---
description: Sintassi dei valori letterali numerici
title: Sintassi di valori letterali numerici | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC literals [ODBC], numeric
- numeric literals [ODBC]
- literals [ODBC], numeric
ms.assetid: fb17498d-4f1d-4b3d-b33d-1e62c7d3c32d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 541f2fa53d6aeb9a387117b17f0917cb110dc630
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193301"
---
# <a name="numeric-literal-syntax"></a>Sintassi dei valori letterali numerici
Per i valori letterali numerici in ODBC viene utilizzata la sintassi seguente:  
  
 *numeric-Literal* :: = *signed-numeric-Literal &#124; unsigned-numeric-Literal*  
  
 *signed-numeric-Literal* :: = [*segno*] senza *segno-numeric-Literal*  
  
 *unsigned-numeric-Literal* :: = *exact-numeric-Literal &#124; approssimate-numeric-Literal*  
  
 *exact-numeric-Literal* :: = *unsigned-integer* [*periodo*[*senza segno-intero*]] *&#124;periodo senza segno-intero*  
  
 *Sign::* = *più-sign &#124; segno meno*  
  
 *approssimativo-numeric-Literal* :: = *mantissa ed esponente*  
  
 *mantissa* :: = *exact-numeric-Literal*  
  
 *esponente* :: = *Signed-Integer*  
  
 *Signed-Integer* :: = [*segno*] senza *segno-intero*  
  
 *unsigned-integer* :: = *digit...*  
  
 *segno più* :: = *+*  
  
 *segno meno* :: =-  
  
 *digit* :: = 1 &#124; 2 &#124; 3 &#124; 4 &#124; 5 &#124; 6 &#124; 7 &#124; 8 &#124; 9 &#124; 0  
  
 *periodo* :: =.
