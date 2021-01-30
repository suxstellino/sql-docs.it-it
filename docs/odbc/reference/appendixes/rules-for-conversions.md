---
description: Regole per le conversioni
title: Regole per le conversioni | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- numeric data type [ODBC], literals
- conversions with numeric literals [ODBC]
- numeric literals [ODBC]
- literals [ODBC], numeric
ms.assetid: 89f846a3-001d-496a-9843-ac9c38dc1762
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b015a60e376e47a4c3c931d4739a884db23fc98b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187132"
---
# <a name="rules-for-conversions"></a>Regole per le conversioni
Le regole in questa sezione sono valide per le conversioni che coinvolgono valori letterali numerici. Ai fini di queste regole, vengono definiti i termini seguenti:  
  
-   *Assegnazione archivio:* Quando si inviano dati a una colonna di una tabella in un database. Questo errore si verifica durante le chiamate a **SQLExecute**, **SQLExecDirect** e **SQLSetPos**. Durante l'assegnazione del negozio, "destinazione" si riferisce a una colonna del database e "Source" si riferisce ai dati nei buffer dell'applicazione.  
  
-   *Assegnazione di recupero:* Durante il recupero dei dati dal database ai buffer dell'applicazione. Questo errore si verifica durante le chiamate a **SQLFetch**, **SQLGetData**, **SQLFetchScroll** e **SQLSetPos**. Durante l'assegnazione del recupero, "destinazione" si riferisce ai buffer dell'applicazione e "Source" si riferisce alla colonna di database.  
  
-   *CS:* Valore nell'origine del carattere.  
  
-   *NT:* Valore nella destinazione numerica.  
  
-   *NS:* Valore dell'origine numerica.  
  
-   *CT:* Valore nella destinazione del carattere.  
  
-   Precisione di un valore letterale numerico esatto: numero di cifre in esso contenute.  
  
-   La scala di un valore letterale numerico esatto: il numero di cifre a destra del periodo espresso o implicito.  
  
-   Precisione di un valore letterale numerico approssimato: la precisione del relativo mantissa.  
  
## <a name="character-source-to-numeric-target"></a>Da origine carattere a destinazione numerica  
 Di seguito sono riportate le regole per la conversione da un'origine carattere (CS) a una destinazione numerica (NT):  
  
1.  Sostituire CS con il valore ottenuto rimuovendo gli spazi iniziali o finali in CS. Se CS non è un valore *letterale numerico* valido, viene restituito SQLSTATE 22018 (valore di carattere non valido per la specifica del cast).  
  
2.  Sostituire CS con il valore ottenuto rimuovendo gli zeri iniziali prima del separatore decimale, gli zeri finali dopo la virgola decimale o entrambi.  
  
3.  Convertire CS in NT. Se la conversione comporta la perdita di cifre significative, viene restituito l'identificativo SQLSTATE 22003 (valore numerico non compreso nell'intervallo). Se la conversione comporta la perdita di cifre non significative, viene restituito SQLSTATE 01S07 (troncamento frazionario).  
  
## <a name="numeric-source-to-character-target"></a>Da origine numerica a destinazione carattere  
 Di seguito sono riportate le regole per la conversione da un'origine numerica (NS) a una destinazione di caratteri (CT):  
  
1.  Lasciare che LT sia la lunghezza in caratteri di CT. Per l'assegnazione di recupero, LT è uguale alla lunghezza del buffer in caratteri meno il numero di byte nel carattere di terminazione null per questo set di caratteri.  
  
2.  Casi  
  
    -   Se NS è un tipo numerico esatto, è possibile lasciare che l'oggetto YP sia uguale alla stringa di caratteri più breve conforme alla definizione del valore *letterale exact-numeric* , in modo che la scala di YP corrisponda alla scala di NS e il valore interpretato di YP sia il valore assoluto di NS.  
  
    -   Se NS è un tipo numerico approssimativo, lasciare che YP sia una stringa di caratteri come indicato di seguito:  
  
         Maiuscole/minuscole:  
  
         Se NS è uguale a 0, YP è 0.  
  
         Consentire a YSN di essere la stringa di caratteri più breve conforme alla definizione di valore *letterale* exact-numeric e il cui valore interpretato è il valore assoluto di NS. Se la lunghezza di YSN è minore di (*precisione* + 1) del tipo di dati NS, consentire a YP uguale a YSN.  
  
         In caso contrario, YP è la stringa di caratteri più breve conforme alla definizione di valore *letterale approssimativo* il cui valore interpretato è il valore assoluto di NS e il cui *mantissa* è costituito da una singola *cifra* che non è' 0', seguita da un *punto* e un *intero senza segno*.  
  
3.  Maiuscole/minuscole:  
  
    -   Se NS è minore di 0, consentire a Y di essere il risultato di:  
  
         '-'  &#124;&#124; YP  
  
         dove ' &#124;&#124;' è l'operatore di concatenazione di stringhe.  
  
         In caso contrario, lasciare Y uguale a YP.  
  
4.  Lasciare che sia la lunghezza in caratteri di Y.  
  
5.  Maiuscole/minuscole:  
  
    -   Se LY è uguale a LT, CT viene impostato su Y.  
  
    -   Se LY è minore di LT, CT viene impostato su Y esteso a destra del numero di spazi appropriato.  
  
         In caso contrario (LY > LT), copiare i primi caratteri LT di Y in CT.  
  
         Maiuscole/minuscole:  
  
         Se si tratta di un'assegnazione di archivio, restituire l'errore SQLSTATE 22001 (dati stringa, troncati a destra).  
  
         Se si tratta di un'assegnazione di recupero, restituire l'avviso SQLSTATE 01004 (dati stringa, troncati a destra). Quando la copia comporta la perdita di cifre frazionarie (ad eccezione degli zeri finali), è definito dal driver se si verifica una delle condizioni seguenti:  
  
         (1) il driver tronca la stringa in Y a una scala appropriata (che può essere anche zero) e scrive il risultato in CT.  
  
         (2) il driver arrotonda la stringa in Y a una scala appropriata (che può essere anche zero) e scrive il risultato in CT.  
  
         (3) il driver non tronca né arrotonda, ma copia solo i primi caratteri LT di Y in CT.
