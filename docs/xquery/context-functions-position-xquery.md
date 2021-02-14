---
title: Funzione position (XQuery) | Microsoft Docs
description: Informazioni sulla posizione della funzione XQuery () che restituisce un valore integer che indica la posizione di un elemento di contesto all'interno di una sequenza di elementi.
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
- position function
- fn:position function
ms.assetid: f1bab9e4-1715-4c06-9cb0-06c7e0c9c97f
author: rothja
ms.author: jroth
ms.openlocfilehash: dd2be5cf246bd625e63c31975effa59fb1b919d0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345036"
---
# <a name="context-functions---position-xquery"></a>Funzioni di contesto - position (XQuery)
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Restituisce un valore integer che indica la posizione dell'elemento di contesto all'interno della sequenza di elementi corrente da elaborare.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
fn:position() as xs:integer  
```  
  
## <a name="remarks"></a>Osservazioni  
 In [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , **FN: position ()** può essere usato solo nel contesto di un predicato dipendente dal contesto. In particolare, può essere utilizzata solo tra parentesi ([ ]). I confronti basati su tale funzione non riducono la cardinalità durante l'interferenza dei tipi statici.  
  
## <a name="examples"></a>Esempio  
 In questo argomento vengono forniti esempi di XQuery sulle istanze XML archiviate in diverse colonne di tipo **XML** nel [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] database.  
  
### <a name="a-using-the-position-xquery-function-to-retrieve-the-first-two-product-features"></a>R. Utilizzo della funzione XQuery position() per recuperare le prime due caratteristiche del prodotto  
 La query seguente recupera le prime due funzionalità, ovvero i primi due elementi figlio dell' `Features` elemento <>, dalla descrizione del catalogo del modello di prodotto. Se sono presenti più funzionalità, aggiunge un elemento <`there-is-more/`> al risultato.  
  
```  
SELECT CatalogDescription.query('  
     declare namespace pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     <Product>   
          { /pd:ProductDescription/@ProductModelID }  
          { /pd:ProductDescription/@ProductModelName }   
          {  
            for $f in /pd:ProductDescription/pd:Features/*[position()<=2]  
            return  
            $f   
          }  
          {  
            if (count(/pd:ProductDescription/pd:Features/*) > 2)  
            then <there-is-more/>  
            else ()  
          }   
     </Product>          
') as x  
FROM Production.ProductModel  
WHERE CatalogDescription is not null  
```  
  
 Dalla query precedente si noti quanto segue:  
  
-   La parola chiave **namespace** nel [prologo XQuery](../xquery/modules-and-prologs-xquery-prolog.md) definisce un prefisso dello spazio dei nomi utilizzato nel corpo della query.  
  
-   Il corpo della query costruisce il codice XML che include un \<Product> elemento con gli attributi **ProductModelID** e **ProductModelName** e le funzionalità del prodotto restituite come elementi figlio.  
  
-   La funzione **Position ()** viene utilizzata nel predicato per determinare la posizione dell' \<Features> elemento figlio nel contesto. Viene restituita se si tratta della prima o della seconda caratteristica.  
  
-   \<there-is-more/>Se nel catalogo dei prodotti sono presenti più di due funzionalità, l'istruzione If aggiunge un elemento al risultato.  
  
-   Poiché non tutte le descrizioni del catalogo dei modelli di prodotto vengono archiviate nella tabella, viene utilizzata la clausola WHERE per scartare le righe in cui CatalogDescriptions è NULL.  
  
 Risultato parziale:  
  
```  
<Product ProductModelID="19" ProductModelName="Mountain 100">  
  <p1:Warranty xmlns:p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    <p1:WarrantyPeriod>3 year</p1:WarrantyPeriod>  
    <p1:Description>parts and labor</p1:Description>  
  </p1:Warranty>  
  <p2:Maintenance xmlns:p2="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    <p2:NoOfYears>10</p2:NoOfYears>  
    <p2:Description>maintenance contact available through your dealer or  
                    any AdventureWorks retail store.</p2:Description>  
  </p2:Maintenance>  
  <there-is-more/>  
</Product>   
...  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni XQuery per il tipo di dati XML](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
