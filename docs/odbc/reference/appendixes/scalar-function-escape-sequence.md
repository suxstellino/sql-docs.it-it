---
description: Sequenza di escape per funzioni scalari
title: Sequenza di escape di funzioni scalari | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- escape sequences [ODBC], scalar function
- scalar functions [ODBC], escape sequences
- ODBC escape sequences [ODBC], scalar function
ms.assetid: aaf5d516-e090-445f-8839-9e39581c69c7
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f17323281ba666d9657b8c1f7e555b61ef24e19f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187122"
---
# <a name="scalar-function-escape-sequence"></a>Sequenza di escape per funzioni scalari
ODBC utilizza sequenze di escape per le funzioni scalari. La sintassi di questa sequenza di escape è la seguente:  
  
```  
{fn scalar-function}  
```  
  
## <a name="remarks"></a>Commenti  
 Nella notazione BNF la sintassi è la seguente:  
  
 *ODBC-Scalar-Function-Escape* :: =  
  
 *ODBC-ESC-initiator* Fn *Scalar-Function ODBC-ESC-Terminator*  
  
 *Scalar-Function* :: = *nome-funzione* (*elenco di argomenti*)  
  
 Le definizioni per il nome di funzione non *-* Terminal e il *nome di funzione* (*elenco di argomenti*) sono derivate dall'elenco di funzioni scalari nelle [funzioni scalari e:](../../../odbc/reference/appendixes/appendix-e-scalar-functions.md).  
  
 *ODBC-ESC-initiator* :: = {  
  
 *ODBC-ESC-Terminator* :: =}  
  
 Per determinare se l'origine dati supporta le procedure e se il driver supporta la sintassi di chiamata della procedura ODBC, un'applicazione può chiamare **SQLGetInfo**. Per ulteriori informazioni, vedere [Appendice E: funzioni scalari](../../../odbc/reference/appendixes/appendix-e-scalar-functions.md).
