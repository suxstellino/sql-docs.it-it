---
description: Valori letterali intervallo
title: Valori letterali intervallo | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], interval data types
- interval literals [ODBC]
- interval data type [ODBC], literals
ms.assetid: f9e6c3c7-4f98-483f-89d8-ebc5680f021b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1e100fcc54f9ca6cc165eece7637b7fae45f65b4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184417"
---
# <a name="interval-literals"></a>Valori letterali intervallo
ODBC richiede che tutti i driver supportino la conversione del tipo di dati SQL_CHAR o SQL_VARCHAR a tutti i tipi di dati intervallo C. Se l'origine dati sottostante non supporta i tipi di dati interval, tuttavia, il driver deve essere in grado di individuare il formato corretto del valore nel campo SQL_CHAR per supportare tali conversioni. In modo analogo, ODBC richiede che qualsiasi tipo ODBC C sia convertibile in SQL_CHAR o SQL_VARCHAR, quindi un driver deve conoscere il formato di un intervallo archiviato nel campo carattere. In questa sezione viene descritta la sintassi dei valori letterali di intervallo, che il writer del driver deve utilizzare per convalidare i campi SQL_CHAR durante la conversione in o da tipi di dati intervallo C.  
  
> [!NOTE]  
>  La sintassi BNF completa per i valori letterali di intervallo è illustrata nella sezione relativa alla [sintassi dei valori letterali di intervallo](../../../odbc/reference/appendixes/interval-literal-syntax.md) nell'Appendice C: grammatica SQL.  
  
 Per passare valori letterali intervallo come parte di un'istruzione SQL, viene definita una sintassi della clausola di escape per i valori letterali di intervallo. Per altre informazioni, vedere [valori letterali data, ora e timestamp](../../../odbc/reference/develop-app/date-time-and-timestamp-literals.md).  
  
 Un valore letterale di intervallo è nel formato seguente:  
  
```  
INTERVAL[<sign>] 'value' <interval qualifier>  
```  
  
 dove "INTERVAL" indica che il valore letterale del carattere è un intervallo. Il segno può essere più o meno; si trova al di fuori della stringa dell'intervallo ed è facoltativa.  
  
 Il qualificatore Interval può essere un singolo campo DateTime o essere composto da due campi DateTime, nel formato: \<*leading field*> a \<*trailing field*> .  
  
-   Quando l'intervallo è costituito da un singolo campo, il campo singolo può essere un campo diverso da un secondo che può essere accompagnato da una precisione leader facoltativa tra parentesi. Il singolo campo DateTime può anche essere un secondo campo che può essere accompagnato dalla precisione leader facoltativa, la precisione frazionaria facoltativa dei secondi tra parentesi o entrambe. Se per un campo di secondi sono presenti sia una precisione iniziali che una precisione in secondi frazionari, queste sono separate da virgole. Se il campo secondi ha una precisione in secondi frazionari, deve avere anche una precisione principale.  
  
-   Quando l'intervallo è costituito da campi iniziali e finali, il campo principale è un campo diverso da un secondo che può essere accompagnato dalla precisione dei campi iniziali dell'intervallo racchiusa tra parentesi. Il campo finale può essere un campo diverso da un secondo o un secondo campo che può essere accompagnato da una precisione frazionaria intervallo-secondi tra parentesi.  
  
 La stringa di intervallo in *valore* è racchiusa tra virgolette singole. Può essere un valore letterale di anno/mese o un valore letterale giorno-ora. Il formato della stringa in *valore* è determinato dalle regole seguenti:  
  
-   La stringa contiene un valore decimale per ogni campo che è implicito da \<*interval* *qualifier*> .  
  
-   Se la precisione dell'intervallo include i campi anno e mese, i valori di questi campi sono separati da un segno meno.  
  
-   Se la precisione dell'intervallo include il giorno e l'ora dei campi, i valori di questi campi sono separati da uno spazio.  
  
-   Se la precisione dell'intervallo include l'ora del campo e i campi dell'ordine inferiore (minuto e secondo), i valori di questi campi sono separati da due punti.  
  
-   Se la precisione dell'intervallo include un secondo campo e la precisione espressa o implicita in secondi è diversa da zero, il carattere immediatamente prima della prima cifra della parte frazionaria del secondo è un punto.  
  
-   Nessun campo può avere una lunghezza superiore a due cifre, ad eccezione di:  
  
    -   Il valore del campo principale può essere fino a quando la precisione di intervallo espressa o implicita.  
  
    -   La parte frazionaria del secondo campo può essere purché la precisione espressa o implicita in secondi.  
  
    -   I campi finali seguono i vincoli usuali del calendario gregoriano. Vedere [vincoli del calendario gregoriano](../../../odbc/reference/appendixes/constraints-of-the-gregorian-calendar.md).  
  
 Nella tabella seguente sono elencati esempi di valori letterali di intervallo validi inclusi nella clausola di escape ODBC per gli intervalli. La sintassi della clausola escape è la seguente:  
  
> [!NOTE]  
>  *{Intervallo di segno intervallo-intervallo stringa-qualificatore}*  
  
|Clausola escape letterale|Intervallo specificato|  
|---------------------------|------------------------|  
|{INTERVALLO ' 326' ANNO (4)}|Specifica un intervallo di 326 anni. La precisione principale dell'intervallo è 4.|  
|{INTERVALLO ' 326' MESE (3)}|Specifica un intervallo di 326 mesi. La precisione principale dell'intervallo è 3.|  
|{INTERVALLO ' 3261' GIORNO (4)}|Specifica un intervallo di 3261 giorni. La precisione principale dell'intervallo è 4.|  
|{INTERVALLO ' 163' ORA (3)}|Specifica un intervallo di 163 giorni. La precisione principale dell'intervallo è 3.|  
|{INTERVALLO ' 163' MINUTO (3)}|Specifica un intervallo di 163 minuti. La precisione principale dell'intervallo è 3.|  
|{INTERVALLO ' 223,16' SECONDO (3, 2)}|Specifica un intervallo di 223,16 secondi. La precisione principale dell'intervallo è 3 e la precisione dei secondi è 2.|  
|{INTERVALLO ' 163-11' ANNO (3) AL MESE}|Specifica un intervallo di 163 anni e 11 mesi. La precisione principale dell'intervallo è 3.|  
|{INTERVALLO ' 163 12' GIORNO (3) PER ORA}|Specifica un intervallo di 163 giorni e 12 ore. La precisione principale dell'intervallo è 3.|  
|{INTERVALLO ' 163 12:39' GIORNO (3) AL MINUTO}|Specifica un intervallo di 163 giorni, 12 ore e 39 minuti. La precisione principale dell'intervallo è 3.|  
|{INTERVALLO ' 163 12:39:59.163' GIORNO (3) AL SECONDO (3)}|Specifica un intervallo di 163 giorni, 12 ore, 39 minuti e 59,163 secondi. La precisione principale dell'intervallo è 3 e la precisione dei secondi è 3.|  
|{INTERVALLO ' 163:39' ORA (3) AL MINUTO}|Specifica un intervallo di 163 ore e 39 minuti. La precisione principale dell'intervallo è 3.|  
|{INTERVAL ' 163:39:59.163' HOUR (3) TO SECOND (4)}|Specifica un intervallo di 163 ore, 39 minuti e 59,163 secondi. La precisione principale dell'intervallo è 3 e la precisione dei secondi è 4.|  
|{INTERVAL ' 163:59.163' MINUTE (3) AL SECONDO (5)}|Specifica un intervallo di 163 minuti e 59,163 secondi. La precisione principale dell'intervallo è 3 e la precisione dei secondi è 5.|  
|{INTERVAL-'16 23:39:56.23' GIORNI AL SECONDO}|Specifica un intervallo di meno 16 giorni, 23 ore, 39 minuti e 56,23 secondi. La precisione principale implicita è 2 e la precisione dei secondi impliciti è 6.|  
  
 Nella tabella seguente sono elencati esempi di valori letterali di intervallo non validi:  
  
|Clausola escape letterale|Motivo per cui non valido|  
|---------------------------|------------------------|  
|{INTERVALLO ' 163' ORA (2)}|La precisione principale dell'intervallo è 2, ma il valore del campo principale è 163.|  
|{INTERVALLO ' 223,16' SECONDO (2, 2)}<br /><br /> {INTERVALLO ' 223,16' SECONDO (3, 1)}|Nel primo esempio la precisione iniziale è troppo piccola e nel secondo esempio la precisione dei secondi è troppo piccola.|  
|{INTERVALLO ' 223,16' SECONDO}<br /><br /> {INTERVALLO ' 223' ANNO}|Poiché la precisione principale non è specificata, il valore predefinito è 2, che è troppo piccolo per mantenere il valore letterale specificato.|  
|{INTERVALLO ' 22,1234567' SECONDO}|La precisione dei secondi non è specificata, quindi il valore predefinito è 6. Il valore letterale ha sette cifre dopo il separatore decimale.|  
|{INTERVALLO ' 163-13' ANNO (3) AL MESE}<br /><br /> {INTERVALLO ' 163 65' GIORNO (3) PER ORA}<br /><br /> {INTERVALLO ' 163 62:39' GIORNO (3) AL MINUTO}<br /><br /> {INTERVAL ' 163 12:125:59.163' GIORNO (3) AL SECONDO (3)}<br /><br /> {INTERVALLO ' 163:144' ORA (3) AL MINUTO}<br /><br /> {INTERVAL ' 163:567:234.163' HOUR (3) TO SECOND (4)}<br /><br /> {INTERVAL ' 163:591.163' MINUTE (3) AL SECONDO (5)}|Il campo finale non segue le regole del calendario gregoriano.|
