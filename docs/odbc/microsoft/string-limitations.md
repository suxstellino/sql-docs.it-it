---
description: Limitazioni delle stringhe
title: Limitazioni di stringa | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC desktop database drivers [ODBC]
- desktop database drivers [ODBC]
ms.assetid: ec1da65f-c69d-415d-bf75-8fda8aa2b39f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 12d02a6e69b34c13219e4bb5f00aeacfa9945cfb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188924"
---
# <a name="string-limitations"></a>Limitazioni delle stringhe
La lunghezza massima di una stringa di istruzione SQL è di 65.000 caratteri.  
  
 Quando si utilizza il driver Microsoft Access, sono supportate solo le costanti di stringa SQL-92 (con virgolette singole, non virgolette doppie).  
  
 Il carattere barra verticale (&#124;) non può essere usato in una stringa, indipendentemente dal fatto che il carattere sia racchiuso tra virgolette.  
  
 Per garantire la massima interoperabilità, le applicazioni devono passare stringhe nei parametri, anziché passare stringhe tra virgolette.
