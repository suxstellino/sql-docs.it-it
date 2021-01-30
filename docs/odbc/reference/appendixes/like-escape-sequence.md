---
description: Sequenza di escape LIKE
title: Sequenza di escape LIKE | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC escape sequences [ODBC], LIKE
- LIKE escape sequence [ODBC]
- escape sequences [ODBC], LIKE
ms.assetid: 798d75ea-be9d-4bef-b297-318bc327f1ca
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d32b0470cb2adf0960e23dac17d8cb4942f296c0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184397"
---
# <a name="like-escape-sequence"></a>Sequenza di escape LIKE
ODBC utilizza sequenze di escape per la clausola LIKE. La sintassi di questa sequenza di escape è la seguente:  
  
```  
{'escape-character'}  
```  
  
## <a name="remarks"></a>Commenti  
 Nella notazione BNF la sintassi è la seguente:  
  
 *ODBC-like-Escape* :: =  
  
 *ODBC-ESC-initiator* escape '*Escape-Character*' *ODBC-ESC-Terminator*  
  
 carattere *di escape* :: = *carattere*  
  
 *ODBC-ESC-initiator* :: = {  
  
 *ODBC-ESC-Terminator* :: =}  
  
 Per determinare se il driver supporta la sequenza di escape LIKE, un'applicazione può chiamare **SQLGetInfo** con il tipo di informazioni SQL_LIKE_ESCAPE_CLAUSE.
