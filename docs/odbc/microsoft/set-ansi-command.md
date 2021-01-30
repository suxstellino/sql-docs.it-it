---
description: SET ANSI (comando)
title: IMPOSTA comando ANSI | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- set ANSI command [ODBC]
ms.assetid: cf9a01b2-14bf-458c-a73c-2a58ddef32d8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bb9dbd2b73b4ff0f7f75442c42de31dbe389211a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208665"
---
# <a name="set-ansi-command"></a>SET ANSI (comando)
Determina il modo in cui i confronti tra stringhe di lunghezze diverse vengono eseguiti con l'operatore = nei comandi SQL di Visual FoxPro.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
SET ANSI ON | OFF  
```  
  
## <a name="arguments"></a>Argomenti  
 ON  
 (Impostazione predefinita per il driver; l'impostazione predefinita per Visual FoxPro è OFF). Consente di ricoprire la stringa più breve con gli spazi vuoti necessari per renderla uguale alla lunghezza della stringa più lunga. Le due stringhe vengono quindi confrontate per il carattere per le lunghezze intere. Considerare questo confronto:  
  
```  
'Tommy' = 'Tom'  
```  
  
 Il risultato è false (. F.) se SET ANSI è on, perché quando viene riempito,' Tom ' diventa ' Tom ' e le stringhe ' Tom ' è Tommy ' non corrispondono al carattere per il carattere.  
  
 L'operatore = = usa questo metodo per i confronti nei comandi SQL di Visual FoxPro.  
  
 OFF  
 Specifica che la stringa più breve non deve essere riempita con spazi vuoti. Le due stringhe vengono confrontate per il carattere fino a quando non viene raggiunta la fine della stringa più breve. Considerare questo confronto:  
  
```  
'Tommy' = 'Tom'  
```  
  
 Il risultato è true (. T.) quando SET ANSI è off, perché il confronto si interrompe dopo ' Tom '.  
  
## <a name="remarks"></a>Commenti  
 SET ANSI determina se le due stringhe più brevi vengono riempite con spazi vuoti quando viene eseguito un confronto tra stringhe SQL. SET ANSI non ha alcun effetto sull'operatore = =; Quando si usa l'operatore = =, la stringa più breve viene sempre riempita con spazi vuoti per il confronto.  
  
## <a name="string-order"></a>Ordine stringhe  
 Nei comandi SQL, l'ordine da sinistra a destra delle due stringhe in un confronto è irrelevantswitching una stringa da un lato dell'operatore = o = = all'altro non influisce sul risultato del confronto.  
  
## <a name="see-also"></a>Vedere anche  
 [Comando SELECT-SQL](../../odbc/microsoft/select-sql-command.md)   
 [SET EXACT (comando)](../../odbc/microsoft/set-exact-command.md)
