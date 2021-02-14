---
title: Funzione ceiling (XQuery) | Microsoft Docs
description: Viene illustrato come utilizzare la funzione XQuery ceiling () per restituire il numero più piccolo senza una parte frazionaria inferiore al valore dell'argomento della funzione.
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:ceiling function
- ceiling function [XQuery]
ms.assetid: 594f1dd0-3c27-41b3-b809-9ce6714c5a97
author: rothja
ms.author: jroth
ms.openlocfilehash: 1ea2dbaab164c57acf9e9bc166ee431e80783648
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341587"
---
# <a name="numeric-values-functions---ceiling"></a>Funzioni per valori numerici - ceiling 
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Restituisce il più piccolo numero senza una parte frazionaria e non inferiore al valore del relativo argomento. Se l'argomento è una sequenza vuota, restituisce la sequenza vuota.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
fn:ceiling ( $arg as numeric?) as numeric?  
```  
  
## <a name="arguments"></a>Argomenti  
 *$arg*  
 Numero al quale viene applicata la funzione.  
  
## <a name="remarks"></a>Commenti  
 Se il tipo di *$arg* è uno dei tre tipi numerici di base, **xs: float**, **xs: Double** o **xs: Decimal**, il tipo restituito è uguale al tipo di *$arg* .  
  
 Se il tipo di *$arg* è un tipo derivato da uno dei tipi numerici, il tipo restituito è il tipo numerico di base.  
  
 Se l'input per le funzioni FN: floor, FN: Ceiling o FN: round è **xdt: untypedAtomic**, viene eseguito il cast implicito a **xs: Double**.  
  
 Qualsiasi altro tipo di dati genera un errore statico.  
  
## <a name="examples"></a>Esempio  
 In questo argomento vengono forniti esempi di XQuery sulle istanze XML archiviate in diverse colonne di tipo **XML** nel database AdventureWorks.  
  
### <a name="a-using-the-ceiling-xquery-function"></a>R. Utilizzo della funzione booleana XQuery ceiling()  
 Per il modello di prodotto 7, la query restituisce un elenco dei centri di lavorazione coinvolti nel processo di produzione del modello. Per ogni centro di lavorazione, la query restituisce l'ID, le ore di manodopera e la dimensione del lotto, se documentati. La query usa la funzione **Ceiling** per restituire le ore lavorative come valori di tipo **Decimal**.  
  
```  
SELECT ProductModelID, Instructions.query('  
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";   
     for $i in /AWMI:root/AWMI:Location  
     return   
       <Location LocationID="{ $i/@LocationID }"   
                   LaborHrs="{ ceiling($i/@LaborHours) }" >  
                    {   
                      $i/@LotSize  
                    }    
       </Location>  
') AS Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Dalla query precedente si noti quanto segue:  
  
-   Il prefisso AWMI è l'acronimo di Adventure Works Manufacturing Instructions e fa riferimento allo stesso spazio dei nomi utilizzato nel documento sul quale viene eseguita la query.  
  
-   **Le istruzioni** sono una colonna di tipo **XML** . Per specificare XQuery, viene pertanto utilizzato il [metodo query () (tipo di dati XML)](../t-sql/xml/query-method-xml-data-type.md) . L'istruzione XQuery viene specificata come argomento per il metodo di query.  
  
-   **per... Return** è un costrutto di ciclo. Nella query il ciclo **for** identifica un elenco di \<Location> elementi. Per ogni centro di lavorazione, l'istruzione **return** nel ciclo **for** descrive il codice XML da generare:  
  
    -   \<Location>Elemento con attributi LocationID e LaborHrs. L'espressione corrispondente racchiusa tra parentesi graffe ({ }) recupera i valori necessari dal documento.  
  
    -   L'espressione {$ i/@LotSize } Recupera l'attributo LotSize dal documento, se presente.  
  
    -   Risultato:  
  
```  
ProductModelID Result    
-------------- ------------------------------------------------------  
7      <Location LocationID="10" LaborHrs="3" LotSize="100"/>  
       <Location LocationID="20" LaborHrs="2" LotSize="1"/>     
       <Location LocationID="30" LaborHrs="1" LotSize="1"/>     
       <Location LocationID="45" LaborHrs="1" LotSize="20"/>  
       <Location LocationID="60" LaborHrs="3" LotSize="1"/>     
       <Location LocationID="60" LaborHrs="4" LotSize="1"/>  
```  
  
### <a name="implementation-limitations"></a>Limitazioni di implementazione  
 Limitazioni:  
  
-   La funzione **Ceiling ()** esegue il mapping di tutti i valori interi a xs: Decimal.  
  
## <a name="see-also"></a>Vedere anche  
 [Funzione floor &#40;XQuery&#41;](../xquery/numeric-values-functions-floor.md)   
 [Funzione round &#40;XQuery&#41;](../xquery/numeric-values-functions-round.md)  
  
  
