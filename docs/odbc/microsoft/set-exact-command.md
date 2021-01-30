---
description: SET EXACT (comando)
title: IMPOSTA comando esatto | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SET EXACT command [ODBC]
ms.assetid: 9533d3e0-e7c1-49de-a3a3-0cc4373a91cb
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 34a22d320dbd099bd26db1b5396cc55a15a4b288
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203954"
---
# <a name="set-exact-command"></a>SET EXACT (comando)
Specifica le regole per il confronto di due stringhe di lunghezze diverse.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
SET EXACT ON | OFF  
```  
  
## <a name="arguments"></a>Argomenti  
 ON  
 Specifica che le espressioni devono corrispondere a un carattere per essere equivalente. Eventuali spazi vuoti finali nelle espressioni vengono ignorati per il confronto. Per il confronto, la più breve delle due espressioni viene riempita a destra con spazi vuoti in modo da corrispondere alla lunghezza dell'espressione più lunga.  
  
 OFF  
 (Impostazione predefinita). Specifica che, per essere equivalente, le espressioni devono corrispondere al carattere per il carattere fino a quando non viene raggiunta la fine dell'espressione sul lato destro.  
  
## <a name="remarks"></a>Commenti  
 L'impostazione esatta SET non ha effetto se entrambe le stringhe hanno la stessa lunghezza.  
  
## <a name="string-comparisons"></a>Confronti tra stringhe  
 In Visual FoxPro sono presenti due operatori relazionali che verificano l'uguaglianza.  
  
 L'operatore = esegue un confronto tra due valori dello stesso tipo. Questo operatore è adatto per confrontare dati di tipo carattere, numerici, di data e logici.  
  
 Tuttavia, quando si confrontano le espressioni di caratteri con l'operatore =, i risultati potrebbero non essere esattamente quelli previsti. Le espressioni di caratteri vengono confrontate con il carattere da sinistra a destra fino a quando una delle espressioni non è uguale all'altra, fino a quando non viene raggiunta la fine dell'espressione sul lato destro dell'operatore = (impostazione esatta OFF) o fino a quando non vengono raggiunte le estremità di entrambe le espressioni (impostazione esatta in).  
  
 L'operatore = = può essere utilizzato quando è necessario un confronto esatto dei dati di tipo carattere. Se due espressioni di caratteri vengono confrontate con l'operatore = =, le espressioni su entrambi i lati dell'operatore = = devono contenere esattamente gli stessi caratteri, inclusi gli spazi vuoti, da considerare uguali. L'impostazione esatta SET viene ignorata quando le stringhe di caratteri vengono confrontate con = =.  
  
 Nella tabella seguente viene illustrato il modo in cui la scelta dell'operatore e l'impostazione esatta hanno effetto sui confronti. Un carattere di sottolineatura rappresenta uno spazio vuoto.  
  
|Confronto|= EXACT OFF|= EXACT ON|= = ESATTA ON o OFF|  
|----------------|------------------|-----------------|--------------------------|  
|"ABC" = "ABC"|Corrispondenza|Corrispondenza|Corrispondenza|  
|"AB" = "ABC"|Nessuna corrispondenza.|Nessuna corrispondenza.|Nessuna corrispondenza.|  
|"ABC" = "AB"|Corrispondenza|Nessuna corrispondenza.|Nessuna corrispondenza.|  
|"ABC" = "ab_"|Nessuna corrispondenza.|Nessuna corrispondenza.|Nessuna corrispondenza.|  
|"AB" = "ab_"|Nessuna corrispondenza.|Corrispondenza|Nessuna corrispondenza.|  
|"ab_" = "AB"|Corrispondenza|Corrispondenza|Nessuna corrispondenza.|  
|"" = "AB"|Nessuna corrispondenza.|Nessuna corrispondenza.|Nessuna corrispondenza.|  
|"AB" = ""|Corrispondenza|Nessuna corrispondenza.|Nessuna corrispondenza.|  
|"__" = ""|Corrispondenza|Corrispondenza|Nessuna corrispondenza.|  
|"" = "___"|Nessuna corrispondenza.|Corrispondenza|Nessuna corrispondenza.|  
|TRIM ("_ _") = ""|Corrispondenza|Corrispondenza|Corrispondenza|  
|"" = TRIM ("_ _")|Corrispondenza|Corrispondenza|Corrispondenza|  
  
## <a name="see-also"></a>Vedere anche  
 [SET ANSI (comando)](../../odbc/microsoft/set-ansi-command.md)
