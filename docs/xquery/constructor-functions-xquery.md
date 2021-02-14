---
title: Funzioni costruttore (XQuery) | Microsoft Docs
description: Informazioni sulle funzioni del costruttore in XQuery che consentono di creare istanze dei tipi atomici definiti dall'utente o incorporati XSD.
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
- constructor functions [XQuery]
ms.assetid: 98562d0e-d0e0-4f62-b001-90acbac67277
author: rothja
ms.author: jroth
ms.openlocfilehash: 0cc1fe3f4f36191a04973c759474f55a8356abda
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349328"
---
# <a name="constructor-functions-xquery"></a>Funzioni costruttore (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Le funzioni costruttore creano istanze di un qualsiasi tipo atomico XSD predefinito o definito dall'utente a partire da un input specificato.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
TYP($atomicvalue as xdt:anyAtomicType?  
  
) as TYP?  
  
```  
  
## <a name="arguments"></a>Argomenti  
 *$strval*  
 Stringa che verrà convertita.  
  
 *TYP*  
 Qualsiasi tipo XSD predefinito.  
  
## <a name="remarks"></a>Commenti  
 Le funzioni costruttore sono supportate per i tipi atomici XSD di base e derivati. Tuttavia, i sottotipi di **xs: Duration**, che include **xdt: yearMonthDuration e xdt: dayTimeDuration** e **xs: QName**, **xs: NMTOKEN** e **xs: Notation** non sono supportati. Sono inoltre disponibili i tipi atomici definiti dall'utente contenuti nelle raccolte di schemi associate, a condizione che siano derivati direttamente o indirettamente dai tipi seguenti.  
  
#### <a name="supported-base-types"></a>Tipi di base supportati  
 Di seguito sono elencati i tipi di base supportati:  
  
-   xs:string  
  
-   xs:boolean  
  
-   xs:decimal  
  
-   xs:float  
  
-   xs:double  
  
-   xs:duration  
  
-   xs:dateTime  
  
-   xs:time  
  
-   xs:date  
  
-   xs:gYearMonth  
  
-   xs:gYear  
  
-   xs:gMonthDay  
  
-   xs:gDay  
  
-   xs:gMonth  
  
-   xs:hexBinary  
  
-   xs:base64Binary  
  
-   xs:anyURI  
  
#### <a name="supported-derived-types"></a>Tipi derivati supportati  
 Di seguito sono elencati i tipi derivati supportati:  
  
-   xs:normalizedString  
  
-   xs:token  
  
-   xs:language  
  
-   xs:Name  
  
-   xs:NCName  
  
-   xs:ID  
  
-   xs:IDREF  
  
-   xs:ENTITY  
  
-   xs:integer  
  
-   xs:nonPositiveInteger  
  
-   xs:negativeInteger  
  
-   xs:long  
  
-   xs:int  
  
-   xs:short  
  
-   xs:byte  
  
-   xs:nonNegativeInteger  
  
-   xs:unsignedLong  
  
-   xs:unsignedInt  
  
-   xs:unsignedShort  
  
-   xs:unsignedByte  
  
-   xs:positiveInteger  
  
 SQL Server supporta inoltre l'elaborazione delle costanti in fase di compilazione per le chiamate di funzioni costruttore, come indicato di seguito:  
  
-   Se l'argomento è un valore letterale stringa, l'espressione verrà valutata in fase di compilazione. Quando il valore non soddisfa i vincoli di tipo, viene generato un errore statico.  
  
-   Se l'argomento è un valore letterale di un altro tipo, l'espressione verrà valutata in fase di compilazione. Quando il valore non soddisfa i vincoli di tipo, viene restituita la sequenza vuota.  
  
## <a name="examples"></a>Esempio  
 In questo argomento vengono forniti esempi di XQuery sulle istanze XML archiviate in diverse colonne di tipo **XML** nel database AdventureWorks.  
  
### <a name="a-using-the-datetime-xquery-function-to-retrieve-older-product-descriptions"></a>R. Utilizzo della funzione XQuery dateTime() per recuperare descrizioni di prodotto non recenti  
 In questo esempio un documento XML di esempio viene inizialmente assegnato a una variabile di tipo **XML** . Questo documento contiene tre <`ProductDescription` di esempio> elementi, ognuno dei quali contiene un `DateCreated` elemento <> elemento figlio.  
  
 Viene quindi eseguita una query sulla variabile per recuperare le descrizioni di prodotto create prima di una data specifica. Ai fini del confronto, nella query viene utilizzata la funzione costruttore **xs: DateTime ()** per digitare le date.  
  
```  
declare @x xml  
set @x = '<root>  
<ProductDescription ProductID="1" >  
  <DateCreated DateValue="2000-01-01T00:00:00Z" />  
  <Summary>Some Summary description</Summary>  
</ProductDescription>  
<ProductDescription  ProductID="2" >  
  <DateCreated DateValue="2001-01-01T00:00:00Z" />  
  <Summary>Some Summary description</Summary>  
</ProductDescription>  
<ProductDescription ProductID="3" >  
  <DateCreated DateValue="2002-01-01T00:00:00Z" />  
  <Summary>Some Summary description</Summary>  
</ProductDescription>  
</root>'  
  
select @x.query('  
     for $PD in  /root/ProductDescription  
     where xs:dateTime(data( ($PD/DateCreated/@DateValue)[1] )) < xs:dateTime("2001-01-01T00:00:00Z")  
     return  
        element Product  
       {   
        ( attribute ProductID { data($PD/@ProductID ) },  
        attribute DateCreated { data( ($PD/DateCreated/@DateValue)[1] ) } )  
        }  
 ')  
```  
  
 Dalla query precedente si noti quanto segue:  
  
-   Oggetto per... La struttura del ciclo WHERE viene utilizzata per recuperare l' \<ProductDescription> elemento che soddisfa la condizione specificata nella clausola WHERE.  
  
-   La funzione del costruttore **DateTime ()** viene usata per costruire valori di tipo **DateTime** in modo che possano essere confrontati in modo appropriato.  
  
-   La query genera quindi il codice XML risultante. Poiché si sta creando una sequenza di attributi, nella costruzione di strutture XML vengono utilizzate virgole e parentesi.  
  
 Risultato:  
  
```  
<Product   
   ProductID="1"   
   DateCreated="2000-01-01T00:00:00Z"/>  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Costrutto XML &#40;XQuery&#41;](../xquery/xml-construction-xquery.md)   
 [Funzioni XQuery per il tipo di dati XML](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
