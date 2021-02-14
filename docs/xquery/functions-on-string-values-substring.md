---
title: Funzione SUBSTRING (XQuery) | Microsoft Docs
description: Informazioni sulla funzione XQuery substring () che restituisce la parte specificata di una stringa di origine.
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
- substring function [XQuery]
- fn:substring function
ms.assetid: 2b3b8651-de51-46dc-af82-c86c45eac871
author: rothja
ms.author: jroth
ms.openlocfilehash: 7457341a5489485973869e46c2ba721b86352217
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345011"
---
# <a name="functions-on-string-values---substring"></a>Funzioni su valori stringa - substring
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Restituisce parte del valore di *$sourceString*, a partire dalla posizione indicata dal valore di *$startingLoc* e continua per il numero di caratteri indicato dal valore di *$length*.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
fn:substring($sourceString as xs:string?,  
                          $startingLoc as xs:decimal?) as xs:string?  
  
fn:substring($sourceString as xs:string?,  
                          $startingLoc as xs:decimal?,  
                          $length as xs:decimal?) as xs:string?  
```  
  
## <a name="arguments"></a>Argomenti  
 *$sourceString*  
 Stringa di origine.  
  
 *$startingLoc*  
 Punto della stringa di origine corrispondente al punto iniziale della sottostringa. Se il valore è negativo o è uguale a 0, vengono restituiti solo i caratteri nelle posizioni maggiori di zero. Se è maggiore della lunghezza della *$sourceString*, viene restituita la stringa di lunghezza zero.  
  
 *$length*  
 Numero di caratteri da recuperare (facoltativo). Se non è specificato, vengono restituiti tutti i caratteri dalla posizione specificata in *$startingLoc* fino alla fine della stringa.  
  
## <a name="remarks"></a>Commenti  
 La versione della funzione con tre argomenti restituisce i caratteri di `$sourceString` il cui operatore di posizione `$p` è soggetto alla regola seguente:  
  
 `fn:round($startingLoc) <= $p < fn:round($startingLoc) + fn:round($length)`  
  
 Il valore di *$length* può essere maggiore del numero di caratteri nel valore di *$sourceString* dopo la posizione iniziale. In questo caso, la sottostringa restituisce i caratteri fino alla fine del *$sourceString*.  
  
 Il primo carattere di una stringa si trova nella posizione 1.  
  
 Se il valore di *$sourceString* è la sequenza vuota, viene gestito come stringa di lunghezza zero. In caso contrario, se *$startingLoc* o *$length* è una sequenza vuota, viene restituita la sequenza vuota.  
  
## <a name="supplementary-characters-surrogate-pairs"></a>Caratteri supplementari (coppie di surrogati)  
 Il comportamento delle coppie di surrogati nelle funzioni XQuery dipende dal livello di compatibilità del database e, in alcuni casi, dall'URI dello spazio dei nomi predefinito per le funzioni. Per ulteriori informazioni, vedere la sezione "funzioni XQuery sono compatibili con i surrogati" nell'argomento [modifiche di rilievo apportate alle funzionalità di motore di database in SQL Server 2016](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md). Vedere anche [livello di compatibilità ALTER DATABASE &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md) e [regole di confronto e supporto Unicode](../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="implementation-limitations"></a>Limitazioni di implementazione  
 SQL Server richiede che i parametri *$startingLoc* e *$length* siano di tipo xs: Decimal anziché di tipo xs: Double.  
  
 SQL Server consente *$startingLoc* e *$length* la sequenza vuota, perché la sequenza vuota è un possibile valore in seguito a errori dinamici di cui è stato eseguito il mapping a ().  
  
## <a name="examples"></a>Esempio  
 In questo argomento vengono forniti esempi di XQuery sulle istanze XML archiviate in diverse colonne di tipo **XML** nel [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] database.  
  
### <a name="a-using-the-substring-xquery-function-to-retrieve-partial-summary-product-model-descriptions"></a>R. Utilizzo della funzione XQuery substring() per recuperare le descrizioni parziali del modello del prodotto disponibili nell'elemento Summary  
 La query recupera i primi 50 caratteri del testo che descrive il modello del prodotto, il <`Summary`> elemento nel documento.  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd)  
SELECT ProductModelID, CatalogDescription.query('  
    <Prod>{ substring(string((/pd:ProductDescription/pd:Summary)[1]), 1, 50) }</Prod>  
 ') as Result  
FROM Production.ProductModel  
where CatalogDescription.exist('/pd:ProductDescription')  = 1;  
```  
  
 Dalla query precedente si noti quanto segue:  
  
-   La funzione **String ()** restituisce il valore stringa dell'elemento<`Summary`>. Questa funzione viene usata, perché l' `Summary` elemento <> contiene sia il testo che i sottoelementi (elementi di formattazione HTML) e perché si ignoreranno questi elementi e si recupererà tutto il testo.  
  
-   La funzione **substring ()** recupera i primi 50 caratteri dal valore stringa recuperato dalla **stringa ()**.  
  
 Risultato parziale:  
  
```  
ProductModelID Result  
-------------- ----------------------------------------------------  
19      <Prod>Our top-of-the-line competition mountain bike.</Prod>   
23      <Prod>Suitable for any type of riding, on or off-roa</Prod>  
...  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni XQuery per il tipo di dati XML](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
