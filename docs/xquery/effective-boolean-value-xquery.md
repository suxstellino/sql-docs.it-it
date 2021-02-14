---
title: Valore booleano effettivo (XQuery) | Microsoft Docs
description: Informazioni sui valori booleani effettivi in XQuery.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- effective Boolean value [XQuery]
- Boolean values
- XQuery, effective Boolean values
- EBV
ms.assetid: 506682b1-b6c9-45e2-aa54-7abd5844c3f1
author: rothja
ms.author: jroth
ms.openlocfilehash: c9e390546feb848f965df2bfd60a2cc68c8fb3ac
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341555"
---
# <a name="effective-boolean-value-xquery"></a>Valore booleano effettivo (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  I valori booleani effettivi sono:  
  
-   False se l'operando è una sequenza vuota o un valore booleano False.  
  
-   In caso contrario, il valore è True.  
  
 È possibile calcolare il valore booleano effettivo per le espressioni che restituiscono un valore booleano singolo, una sequenza di nodi o una sequenza vuota. Si noti che il valore booleano viene calcolato in modo implicito quando vengono elaborati i tipi seguenti di espressioni:  
  
-   Espressioni logiche  
  
-   [Funzione not](../xquery/functions-on-boolean-values-not-function.md)  
  
-   Clausola WHERE di un'espressione FLWOR  
  
-   [Espressioni condizionali](../xquery/conditional-expressions-xquery.md)  
  
-   [Espressioni quantificate](../xquery/quantified-expressions-xquery.md)  
  
 Di seguito è riportato un esempio di un valore booleano effettivo. Quando l'espressione **if** viene elaborata, viene determinato il valore booleano effettivo della condizione. Poiché `/a[1]` restituisce una sequenza vuota, il valore booleano effettivo è False. Il risultato viene restituito in formato XML con un nodo di testo (false).  
  
```  
value is false  
DECLARE @x XML  
SET @x = '<b/>'  
SELECT @x.query('if (/a[1]) then "true" else "false"')  
go  
```  
  
 Nell'esempio seguente, il valore booleano effettivo è True, perché l'espressione restituisce una sequenza non vuota.  
  
```  
DECLARE @x XML  
SET @x = '<a/>'  
SELECT @x.query('if (/a[1]) then "true" else "false"')  
go  
```  
  
 Quando si eseguono query su colonne o variabili **XML** tipizzate, è possibile avere nodi di tipo booleano. I **dati ()** in questo caso restituiscono un valore booleano. Se l'espressione della query restituisce un valore booleano True, il valore booleano effettivo è True, come illustrato nell'esempio seguente. Nell'esempio viene inoltre illustrato quanto segue:  
  
-   Viene creata una raccolta di XML Schema. L'elemento \<b> nella raccolta è di tipo booleano.  
  
-   Viene creata e sottoposta a query una variabile **XML** tipizzata.  
  
-   L'espressione `data(/b[1])` restituisce un valore booleano True e pertanto in questo caso il valore booleano effettivo è True.  
  
-   L'espressione `data(/b[2])` restituisce un valore booleano false. e pertanto in questo caso il valore booleano effettivo è False.  
  
```  
CREATE XML SCHEMA COLLECTION SC AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema">  
      <element name="s" type="string"/>  
      <element name="b" type="boolean"/>  
</schema>'  
go  
DECLARE @x XML(SC)  
SET @x = '<b>true</b><b>false</b>'  
SELECT @x.query('if (data(/b[1])) then "true" else "false"')  
SELECT @x.query('if (data(/b[2])) then "true" else "false"')  
go  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Nozioni fondamentali su XQuery](../xquery/xquery-basics.md)   
 [Istruzione FLWOR e iterazione &#40;XQuery&#41;](../xquery/flwor-statement-and-iteration-xquery.md)  
  
  
