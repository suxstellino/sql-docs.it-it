---
title: Funzione AVG (XQuery) | Microsoft Docs
description: Informazioni sulla funzione XQuery avg () che restituisce la media di una sequenza di numeri specificata.
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:avg function
- avg function [XQuery]
ms.assetid: 0cc60267-3c56-4a88-8ad7-bb07f0255d56
author: rothja
ms.author: jroth
ms.openlocfilehash: ee09ab89e1acb0fc8ddad23d002e2cd0e136bc59
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340490"
---
# <a name="aggregate-functions---avg"></a>Funzioni di aggregazione - avg
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Restituisce la media di una sequenza di numeri.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
fn:avg($arg as xdt:anyAtomicType*) as xdt:anyAtomicType?  
```  
  
## <a name="arguments"></a>Argomenti  
 *$arg*  
 Sequenza di valori atomici per la quale viene calcolata la media.  
  
## <a name="remarks"></a>Commenti  
 Tutti i tipi di valori atomizzati passati a **AVG ()** devono essere un sottotipo di uno dei tre tipi di base numerici predefiniti o xdt: untypedAtomic. e non possono essere una combinazione di tipi. I valori del tipo xdt:untypedAtomic vengono considerati come xs:double. Il risultato di **AVG ()** riceve il tipo di base dei tipi passati, ad esempio xs: Double nel caso di xdt: untypedAtomic.  
  
 Se l'input è una sequenza vuota calcolata in modo statico, la sequenza vuota è implicita e viene restituito un errore statico.  
  
 La funzione **AVG ()** restituisce la media dei numeri calcolati. Ad esempio:  
  
 conteggio div **Sum (** *$arg* **) (** *$arg* **)**  
  
 Se *$arg* è una sequenza vuota, viene restituita la sequenza vuota.  
  
 Se non è possibile eseguire il cast di un valore xdt: untypedAtomic a xs: Double, il valore viene ignorato nella sequenza di input *$arg*.  
  
 In tutti gli altri casi, la funzione restituisce un errore statico.  
  
## <a name="examples"></a>Esempio  
 In questo argomento vengono forniti esempi di XQuery sulle istanze XML archiviate in diverse colonne di tipo **XML** nel database AdventureWorks.  
  
### <a name="a-using-the-avg-xquery-function-to-find-work-center-locations-in-the-manufacturing-process-in-which-labor-hours-are-greater-than-the-average-for-all-work-center-locations"></a>R. Utilizzo della funzione XQuery avg() per trovare i centri di lavorazione del processo di produzione con ore di manodopera superiori alla media di tutti i centri.  
 È possibile riscrivere la query fornita in [funzione min (XQuery)](../xquery/aggregate-functions-min.md) per usare la funzione **AVG ()** .  
  
## <a name="implementation-limitations"></a>Limitazioni di implementazione  
 Limitazioni:  
  
-   La funzione **AVG ()** esegue il mapping di tutti i numeri interi a xs: Decimal.  
  
-   La funzione **AVG ()** su valori di tipo xs: Duration non è supportata.  
  
-   Non sono supportate le sequenze con combinazioni di tipi che non rispettano i limiti del tipo di base.  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni XQuery per il tipo di dati XML](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
