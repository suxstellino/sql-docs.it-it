---
title: Funzione min (XQuery) | Microsoft Docs
description: Informazioni sulla funzione XQuery min () che restituisce un elemento in una sequenza il cui valore è minore di quello di tutti gli altri.
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
- fn:min function
- min function [XQuery]
ms.assetid: db0b7d94-3fa6-488f-96d6-6a9a7d6eda23
author: rothja
ms.author: jroth
ms.openlocfilehash: 19c4204e84759bab022d0f0765b9d6154d23d003
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341942"
---
# <a name="aggregate-functions---min"></a>Funzioni di aggregazione - min
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Restituisce da una sequenza di valori atomici, *$arg*, un elemento il cui valore è minore di quello di tutti gli altri.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
fn:min($arg as xdt:anyAtomicType*) as xdt:anyAtomicType?  
```  
  
## <a name="arguments"></a>Argomenti  
 *$arg*  
 Sequenza di elementi da cui viene restituito il valore minimo.  
  
## <a name="remarks"></a>Commenti  
 Tutti i tipi di valori atomizzati passati a **min ()** devono essere sottotipi dello stesso tipo di base. I tipi di base accettati sono i tipi che supportano l'operazione **gt** . Tra questi tipi sono inclusi i tre tipi di base numerici predefiniti, ovvero i tipi di base di data/ora xs:string, xs:boolean e xdt:untypedAtomic. Per i valori di tipo xdt:untypedAtomic viene eseguito il cast a xs:double. Se è presente una combinazione di questi tipi o se vengono passati altri valori di altri tipi, viene generato un errore statico.  
  
 Il risultato di **min ()** riceve il tipo di base dei tipi passati, ad esempio xs: Double nel caso di xdt: untypedAtomic. Se l'input è una sequenza vuota calcolata in modo statico, la sequenza vuota è implicita e viene restituito un errore statico.  
  
 La funzione **min ()** restituisce un valore nella sequenza minore di qualsiasi altro oggetto nella sequenza di input. Per i valori xs:string, vengono utilizzate le regole di confronto predefinite dei punti di codice Unicode. Se non è possibile eseguire il cast di un valore xdt: untypedAtomic a xs: Double, il valore viene ignorato nella sequenza di input *$arg*. Se l'input è una sequenza vuota calcolata in modo dinamico, viene restituita la sequenza vuota.  
  
## <a name="examples"></a>Esempio  
 In questo argomento vengono forniti esempi di XQuery sulle istanze XML archiviate in diverse colonne di tipo **XML** nel database AdventureWorks.  
  
### <a name="a-using-the-min-xquery-function-to-find-the-work-center-location-that-has-the-fewest-labor-hours"></a>R. Utilizzo della funzione XQuery min() per l'individuazione del centro di lavorazione con il numero minimo di ore di manodopera  
 La query seguente recupera tutti i centri di lavorazione con il numero minimo di ore di manodopera inclusi nel processo di produzione del modello del prodotto (ProductModelID=7). In genere, come illustrato nell'esempio seguente, viene restituito un singolo centro. Nel caso in cui esistano più centri con lo stesso numero minimo di ore di manodopera, vengono restituiti tutti questi centri.  
  
```  
select ProductModelID, Name, Instructions.query('  
  declare namespace AWMI=  
    "https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
  for   $Location in /AWMI:root/AWMI:Location  
  where $Location/@LaborHours =  
          min( /AWMI:root/AWMI:Location/@LaborHours )  
return  
  <Location WCID=     "{ $Location/@LocationID }"   
              LaborHrs= "{ $Location/@LaborHours }" />  
  ') as Result   
FROM  Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Dalla query precedente si noti quanto segue:  
  
-   La parola chiave **namespace** nel prologo XQuery definisce un prefisso dello spazio dei nomi. In seguito, tale prefisso viene utilizzato nel corpo della query XQuery.  
  
 Il corpo XQuery costruisce il codice XML che include un \<Location> elemento con gli attributi wcid e **LaborHrs** .  
  
-   La query recupera inoltre i valori del modello del prodotto ProductModelID e dei nomi.  
  
 Risultato:  
  
```  
ProductModelID   Name              Result  
---------------  ----------------  ---------------------------------  
7                HL Touring Frame  <Location WCID="45" LaborHrs="0.5"/>   
```  
  
## <a name="implementation-limitations"></a>Limitazioni di implementazione  
 Limitazioni:  
  
-   La funzione **min ()** esegue il mapping di tutti i numeri interi a xs: Decimal.  
  
-   La funzione **min ()** su valori di tipo xs: Duration non è supportata.  
  
-   Non sono supportate le sequenze con combinazioni di tipi che non rispettano i limiti del tipo di base.  
  
-   Non è supportata l'opzione sintattica che fornisce le regole di confronto.  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni XQuery per il tipo di dati XML](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
