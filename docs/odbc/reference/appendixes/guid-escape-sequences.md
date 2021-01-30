---
description: Sequenze di escape GUID
title: Sequenze di escape GUID | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC escape sequences [ODBC], GUID
- escape sequences [ODBC], guid
- guid escape sequence [ODBC]
ms.assetid: 71d43ef9-4a31-493e-b9e0-f864e9ef3ce6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 535d24034c219691b192d409f72df15c83ab22c7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208581"
---
# <a name="guid-escape-sequences"></a>Sequenze di escape GUID
ODBC utilizza sequenze di escape per i valori letterali GUID. La sintassi di questa sequenza di escape è la seguente:  
  
```  
{guid 'nnnnnnnn-nnnn-nnnn-nnnn-nnnnnnnnnnnn'}  
```  
  
## <a name="remarks"></a>Commenti  
 Nella notazione BNF la sintassi è la seguente:  
  
 *ODBC-GUID-Escape* :: =  
     *ODBC-ESC-initiator GUID* '*Guid-value*' *ODBC-ESC-Terminator*  
  
 *ODBC-ESC-initiator* :: = {  
  
 *ODBC-ESC-Terminator* :: =}  
  
 *GUID-valore* :: = clock-valore GUID-basso-valore-orologio separatore GUID-valore medio-orologio-separatore GUID-valore-valore di clock- *Seq-value GUID-valore-nodo-separatore*  
  
 *separatore GUID* :: =-  
  
 *Clock-low-value* :: = *hex_digit hex_digit hex_digit hex_digit* hex_digit hex_digit hex_digit hex_digit  
  
 *clock-valore medio* :: = *hex_digit hex_digit hex_digit hex_digit*  
  
 *Clock-High-Value* :: = *hex_digit hex_digit hex_digit hex_digit*  
  
 *clock-seq-value* :: = *hex_digit hex_digit hex_digit hex_digit*  
  
 *Clock-node-value* :: = *hex_digit hex_digit hex_digit hex_digit hex_digit* hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit  
  
 *hex_digit* :: = 0 &#124; 1 &#124; 2 &#124; 3 &#124; 4 &#124; 5 &#124; 6 &#124; 7 &#124; 8 &#124; 9 &#124; A &#124; B &#124; C &#124; D &#124; E &#124; F  
  
 La sequenza di escape dei valori letterali GUID è supportata se il tipo di dati GUID è supportato dall'origine dati. Un'applicazione deve chiamare **SQLGetTypeInfo** per determinare se questo tipo di dati è supportato.
