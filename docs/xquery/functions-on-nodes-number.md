---
title: Funzione Number (XQuery) | Microsoft Docs
description: Informazioni sul numero di funzione XQuery () che restituisce il valore numerico di un argomento specificato.
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- number function
- fn:number function
ms.assetid: dff6d19b-765c-4df9-afff-9a0e7be9b91b
author: rothja
ms.author: jroth
ms.openlocfilehash: 6aa035be000fbbda12a7205c33925daf656c3da0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345019"
---
# <a name="functions-on-nodes---number"></a>Funzioni su nodi - number
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Restituisce il valore numerico del nodo indicato dal *$arg*.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
fn:number() as xs:double?   
fn:number($arg as node()?) as xs:double?  
```  
  
## <a name="arguments"></a>Argomenti  
 *$arg*  
 Nodo per il quale verrà restituito un valore numerico.  
  
## <a name="remarks"></a>Commenti  
 Se *$arg* viene omesso, viene restituito il valore numerico del nodo di contesto, convertito in un valore Double. In SQL Server, **FN: Number ()** senza un argomento può essere usato solo nel contesto di un predicato dipendente dal contesto. In particolare, può essere utilizzata solo tra parentesi ([ ]). Ad esempio, l'espressione seguente restituisce l' `ROOT` elemento <>.  
  
```  
declare @x xml  
set @x='<ROOT>111</ROOT>'  
select @x.query('/ROOT[number()=111]')  
```  
  
 Se il valore del nodo non è una rappresentazione lessicale valida di un tipo semplice numerico, come definito in **XML Schema Part 2: Datatypes, W3C Recommendation**, la funzione restituisce una sequenza vuota. NaN non è supportato.  
  
## <a name="examples"></a>Esempio  
 In questo argomento vengono forniti esempi di XQuery sulle istanze XML archiviate in diverse colonne di tipo **XML** nel database AdventureWorks.  
  
### <a name="a-using-the-number-xquery-function-to-retrieve-the-numeric-value-of-an-attribute"></a>R. Utilizzo della funzione XQuery number() per recuperare il valore numerico di un attributo  
 La query seguente recupera il valore numerico dell'attributo lotsize dal primo centro di lavorazione nel processo di produzione del modello di prodotto 7.  
  
```  
SELECT ProductModelID, Instructions.query('  
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions" ;  
     for $i in (//AWMI:root//AWMI:Location)[1]  
     return   
       <Location LocationID="{ ($i/@LocationID) }"   
                   LotSizeA="{  $i/@LotSize }"  
                   LotSizeB="{  number($i/@LotSize) }"  
                   LotSizeC="{ number($i/@LotSize) + 1 }" >  
  
       </Location>  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Dalla query precedente si noti quanto segue:  
  
-   La funzione **Number ()** non è obbligatoria, come illustrato dalla query per l'attributo **LotSizeA** . Si tratta infatti di una funzione XPath della versione 1.0 ed è stata inclusa principalmente per motivi di compatibilità con le versioni precedenti.  
  
-   XQuery per **LotSizeB** specifica la funzione Number ed è ridondante.  
  
-   La query per l'utilizzo **di un numero eccessivo illustra l'** uso di un valore numerico in un'operazione aritmetica.  
  
 Risultato:  
  
```  
ProductModelID   Result  
----------------------------------------------  
7              <Location LocationID="10"   
                         LotSizeA="100"   
                         LotSizeB="100"   
                         LotSizeC="101" />  
```  
  
### <a name="implementation-limitations"></a>Limitazioni di implementazione  
 Limitazioni:  
  
-   La funzione **Number ()** accetta solo i nodi. Non accetta valori atomici.  
  
-   Quando non è possibile restituire i valori come numero, la funzione **Number ()** restituisce la sequenza vuota anziché Nan.  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni XQuery per il tipo di dati XML](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
