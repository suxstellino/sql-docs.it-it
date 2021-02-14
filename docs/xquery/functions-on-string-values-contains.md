---
title: Funzione Contains (XQuery) | Microsoft Docs
description: Informazioni sull'utilizzo della funzione contains in un XQuery per determinare se un valore di stringa specificato contiene il valore della sottostringa specificato.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- contains function (XQuery)
- fn:contains function
ms.assetid: 2c88c015-04fc-429b-84b2-835596a28b65
author: rothja
ms.author: jroth
ms.openlocfilehash: 18f57d57e1a944a1be66a880231533e6cf34493e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344974"
---
# <a name="functions-on-string-values---contains"></a>Funzioni su valori stringa - contains
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Restituisce un valore di tipo xs: Boolean che indica se il valore di *$arg 1* contiene un valore stringa specificato da *$ARG 2*.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
fn:contains ($arg1 as xs:string?, $arg2 as xs:string?) as xs:boolean?  
```  
  
## <a name="arguments"></a>Argomenti  
 *$arg 1*  
 Valore stringa da testare.  
  
 *$arg 2*  
 Sottostringa da cercare.  
  
## <a name="remarks"></a>Commenti  
 Se il valore di *$ARG 2* è una stringa di lunghezza zero, la funzione restituisce **true**. Se il valore di *$arg 1* è una stringa di lunghezza zero e il valore di *$ARG 2* non è una stringa di lunghezza zero, la funzione restituisce **false**.  
  
 Se il valore di *$arg 1* o *$ARG 2* è la sequenza vuota, l'argomento viene trattato come stringa di lunghezza zero.  
  
 La funzione contains() utilizza le regole di confronto dei punti di codice Unicode predefinite di XQuery per il confronto delle stringhe.  
  
 Il valore della sottostringa specificato per *$ARG 2* deve essere minore o uguale a 4000 caratteri. Se il valore specificato è maggiore di 4000 caratteri, si verifica una condizione di errore dinamico e la funzione Contains () restituisce una sequenza vuota anziché un valore booleano **true** o **false**. In [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] non vengono generati errori dinamici nelle espressioni XQuery.  
  
 Per ottenere confronti senza distinzione tra maiuscole e minuscole, è possibile [usare le funzioni maiuscole](../xquery/functions-on-string-values-upper-case.md) o minuscole.  
  
## <a name="supplementary-characters-surrogate-pairs"></a>Caratteri supplementari (coppie di surrogati)  
 Il comportamento delle coppie di surrogati nelle funzioni XQuery dipende dal livello di compatibilità del database e, in alcuni casi, dall'URI dello spazio dei nomi predefinito per le funzioni. Per ulteriori informazioni, vedere la sezione "funzioni XQuery sono compatibili con i surrogati" nell'argomento [modifiche di rilievo apportate alle funzionalità di motore di database in SQL Server 2016](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md). Vedere anche [livello di compatibilità ALTER DATABASE &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md) e [regole di confronto e supporto Unicode](../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="examples"></a>Esempio  
 In questo argomento vengono forniti esempi di XQuery sulle istanze XML archiviate in diverse colonne di tipo XML nel database AdventureWorks.  
  
### <a name="a-using-the-contains-xquery-function-to-search-for-a-specific-character-string"></a>R. Utilizzo della funzione XQuery contains() per cercare una stringa di caratteri specifica  
 La query seguente trova i prodotti per i quali la descrizione di riepilogo contiene la parola Aerodynamic. La query restituisce il ProductID e l' `Summary` elemento <> per tali prodotti.  
  
```  
--The product model description document uses  
--namespaces. The WHERE clause uses the exit()  
--method of the xml data type. Inside the exit method,  
--the XQuery contains()function is used to  
--determine whether the <Summary> text contains the word  
--Aerodynamic.   
  
USE AdventureWorks  
GO  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd)  
SELECT ProductModelID, CatalogDescription.query('  
      <Prod>  
         { /pd:ProductDescription/@ProductModelID }  
         { /pd:ProductDescription/pd:Summary }  
      </Prod>  
 ') as Result  
FROM Production.ProductModel  
where CatalogDescription.exist('  
   /pd:ProductDescription/pd:Summary//text()  
    [contains(., "Aerodynamic")]') = 1  
```  
  
 Risultati  
  
 `ProductModelID Result`  
  
 `-------------- ---------`  
  
 `28     <Prod ProductModelID="28">`  
  
 `<pd:Summary xmlns:pd=`  
  
 `"https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">`  
  
 `<p1:p xmlns:p1="http://www.w3.org/1999/xhtml">`  
  
 `A TRUE multi-sport bike that offers streamlined riding and`  
  
 `a revolutionary design. Aerodynamic design lets you ride with`  
  
 `the pros, and the gearing will conquer hilly roads.</p1:p>`  
  
 `</pd:Summary>`  
  
 `</Prod>`  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni XQuery per il tipo di dati XML](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
