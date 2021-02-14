---
title: Funzione Concat (XQuery) | Microsoft Docs
description: Informazioni sulla funzione XQuery Concat () che restituisce una stringa creata concatenando zero o più stringhe specificate come argomenti.
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
- fn:concat function
- concat function [XQuery]
ms.assetid: d50afd20-a297-445e-be9e-13b48017e7ca
author: rothja
ms.author: jroth
ms.openlocfilehash: 3ad92315e20ae314fb9a22ddea1241f379ecc90f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341926"
---
# <a name="functions-on-string-values---concat"></a>Funzioni su valori stringa - concat
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Accetta zero o più stringhe come argomenti e restituisce una stringa creata concatenando i valori di ognuno di questi argomenti.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
fn:concat ($string as xs:string?  
           ,$string as xs:string?  
           [, ...]) as xs:string  
```  
  
## <a name="arguments"></a>Argomenti  
 *$string*  
 Stringa facoltativa per la concatenazione.  
  
## <a name="remarks"></a>Commenti  
 La funzione richiede almeno due argomenti. Se un argomento è costituito da una sequenza vuota, viene considerato come stringa di lunghezza zero.  
  
## <a name="supplementary-characters-surrogate-pairs"></a>Caratteri supplementari (coppie di surrogati)  
 Il comportamento delle coppie di surrogati nelle funzioni XQuery dipende dal livello di compatibilità del database e, in alcuni casi, dall'URI dello spazio dei nomi predefinito per le funzioni. Per ulteriori informazioni, vedere la sezione "funzioni XQuery sono compatibili con i surrogati" nell'argomento [modifiche di rilievo apportate alle funzionalità di motore di database in SQL Server 2016](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md). Vedere anche [livello di compatibilità ALTER DATABASE &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md) e [regole di confronto e supporto Unicode](../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="examples"></a>Esempio  
 In questo argomento vengono forniti esempi di XQuery sulle istanze XML archiviate in diverse colonne di tipo **XML** nel database di esempio AdventureWorks.  
  
### <a name="a-using-the-concat-xquery-function-to-concatenate-strings"></a>R. Utilizzo della funzione XQuery concat() per la concatenazione di stringhe  
 Per un modello di prodotto specifico, questa query restituisce una stringa creata concatenando il periodo di validità e la descrizione della garanzia. Nel documento di descrizione del catalogo, l' `Warranty` elemento <> è costituito da <`WarrantyPeriod`> e <`Description`> elementi figlio.  
  
```  
WITH XMLNAMESPACES (  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd,  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
SELECT CatalogDescription.query('  
    <Product   
        ProductModelID= "{ (/pd:ProductDescription/@ProductModelID)[1] }"  
        ProductModelName = "{ sql:column("PD.Name") }" >  
        {   
          concat( string((/pd:ProductDescription/pd:Features/wm:Warranty/wm:WarrantyPeriod)[1]), "-",  
                  string((/pd:ProductDescription/pd:Features/wm:Warranty/wm:Description)[1]))   
         }   
     </Product>  
 ') as Result  
FROM Production.ProductModel PD  
WHERE  PD.ProductModelID=28  
  
```  
  
 Dalla query precedente si noti quanto segue:  
  
-   Nella clausola SELECT CatalogDescription è una colonna di tipo **XML** . Viene pertanto utilizzato il [metodo query () (tipo di dati XML)](../t-sql/xml/query-method-xml-data-type.md), instructions. query (). L'istruzione XQuery viene specificata come argomento per il metodo di query.  
  
-   Nel documento sul quale viene eseguita la query vengono utilizzati spazi dei nomi. Pertanto, la parola chiave **namespace** viene utilizzata per definire il prefisso per lo spazio dei nomi. Per ulteriori informazioni, vedere [prologo XQuery](../xquery/modules-and-prologs-xquery-prolog.md).  
  
 Risultato:  
  
```  
<Product ProductModelID="28" ProductModelName="Road-450">1 year-parts and labor</Product>  
```  
  
 La query precedente recupera le informazioni relative a un prodotto specifico. La query seguente recupera le stesse informazioni per tutti i prodotti per i quali vengono archiviate descrizioni di catalogo XML. Il metodo **exist ()** del tipo di dati **XML** nella clausola WHERE restituisce true se il documento XML nelle righe contiene un `ProductDescription` elemento <>.  
  
```  
WITH XMLNAMESPACES (  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd,  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
  
SELECT CatalogDescription.query('  
    <Product   
        ProductModelID= "{ (/pd:ProductDescription/@ProductModelID)[1] }"   
        ProductName = "{ sql:column("PD.Name") }" >  
        {   
          concat( string((/pd:ProductDescription/pd:Features/wm:Warranty/wm:WarrantyPeriod)[1]), "-",  
                  string((/pd:ProductDescription/pd:Features/wm:Warranty/wm:Description)[1]))   
         }   
     </Product>  
 ') as Result  
FROM Production.ProductModel PD  
WHERE CatalogDescription.exist('//pd:ProductDescription ') = 1  
  
```  
  
 Si noti che il valore booleano restituito dal metodo **exist ()** del tipo **XML** viene confrontato con 1.  
  
### <a name="implementation-limitations"></a>Limitazioni di implementazione  
 Limitazioni:  
  
-   La funzione **Concat ()** in SQL Server accetta solo valori di tipo xs: String. Per gli altri valori è necessario eseguire il cast esplicito al tipo xs:string o al tipo xdt:untypedAtomic.  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni XQuery per il tipo di dati XML](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
