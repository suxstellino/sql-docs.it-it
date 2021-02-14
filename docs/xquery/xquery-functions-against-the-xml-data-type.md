---
title: Funzioni XQuery per il tipo di dati XML | Microsoft Docs
description: Informazioni sulle funzioni XQuery supportate per l'utilizzo con il tipo di dati XML.
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
- XQuery, functions
- xml data type [SQL Server], XQuery
- functions [SQL Server], XQuery
ms.assetid: 8df0877d-a03f-4ca9-b84e-908c4bb42b5e
author: rothja
ms.author: jroth
ms.openlocfilehash: 82f3eae7098b3ac5e86355aed1e13bdd5c137015
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352816"
---
# <a name="xquery-functions-against-the-xml-data-type"></a>Funzioni XQuery per il tipo di dati XML
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  In questo argomento e nei relativi argomenti secondari vengono descritte le funzioni che è possibile utilizzare quando si specifica XQuery sul tipo di dati **XML** . Per le specifiche W3C, vedere [http://www.w3.org/TR/2004/WD-xpath-functions-20040723](https://go.microsoft.com/fwlink/?LinkId=4873) .  
  
 Le funzioni XQuery appartengono allo http://www.w3.org/2004/07/xpath-functions spazio dei nomi. Le specifiche W3C utilizzano il prefisso "fn:" dello spazio dei nomi per la descrizione di queste funzioni. Quando si utilizzano le funzioni non è necessario specificare esplicitamente il prefisso "fn:". Per questo motivo e per migliorare la leggibilità, in questa documentazione i prefissi dello spazio dei nomi sono in genere omessi.  
  
 Nella tabella seguente sono elencate le funzioni XQuery supportate per il tipo di dati **XML**.  
  
|Category|Nome funzione|  
|--------------|-------------------|  
|[Funzioni su valori numerici]()|[Ceiling](../xquery/numeric-values-functions-ceiling.md)|  
||[Floor](../xquery/numeric-values-functions-floor.md)|  
||[turno](../xquery/numeric-values-functions-round.md)|  
|[Funzioni XQuery su valori stringa]()|[concat](../xquery/functions-on-string-values-concat.md)|  
||[contains](../xquery/functions-on-string-values-contains.md)|  
||[substring](../xquery/functions-on-string-values-substring.md)|  
||[Funzione lower-case &#40;XQuery&#41;](../xquery/functions-on-string-values-lower-case.md)|  
||[Lunghezza stringa](../xquery/functions-on-string-values-string-length.md)|  
||[Funzione Upper-Case &#40;XQuery&#41;](../xquery/functions-on-string-values-upper-case.md)|  
|Funzioni su valori booleani|[not](../xquery/functions-on-boolean-values-not-function.md)|  
|[Funzioni sui nodi]()|[number](../xquery/functions-on-nodes-number.md)|  
||[Funzione local-name (XQuery)](../xquery/functions-on-nodes-local-name.md)|  
||[Funzione namespace-uri (XQuery)](../xquery/functions-on-nodes-namespace-uri.md)|  
|[Funzioni di contesto]()|[last](../xquery/context-functions-last-xquery.md)|  
||[position](../xquery/context-functions-position-xquery.md)|  
|[Funzioni per le sequenze]()|[empty](../xquery/functions-on-sequences-empty.md)|  
||[distinct-values](../xquery/functions-on-sequences-distinct-values.md)|  
||[Funzione id (XQuery)](../xquery/functions-on-sequences-id.md)|  
|[Funzioni di aggregazione &#40;XQuery&#41;]()|[count](../xquery/aggregate-functions-count.md)|  
||[AVG](../xquery/aggregate-functions-avg.md)|  
||[min](../xquery/aggregate-functions-min.md)|  
||[max](../xquery/aggregate-functions-max.md)|  
||[sum](../xquery/aggregate-functions-sum.md)|  
|[Funzioni del costruttore &#40;XQuery&#41;](../xquery/constructor-functions-xquery.md)|[Funzioni costruttore](../xquery/constructor-functions-xquery.md)|  
|[Funzioni di accesso ai dati](../xquery/data-accessor-functions.md)|[string](../xquery/data-accessor-functions-string-xquery.md)|  
||[data](../xquery/data-accessor-functions-data-xquery.md)|  
|[Funzioni costruttore booleane &#40;XQuery&#41;]()|[Funzione true (XQuery)](../xquery/boolean-constructor-functions-true-xquery.md)|  
||[Funzione false (XQuery)](../xquery/boolean-constructor-functions-false-xquery.md)|  
|[Funzioni correlate a QName &#40;XQuery&#41;](./functions-related-to-qnames-expanded-qname.md)|[expanded-QName (XQuery)](../xquery/functions-related-to-qnames-expanded-qname.md)|  
||[local-name-from-QName (XQuery)](../xquery/functions-related-to-qnames-local-name-from-qname.md)|  
||[namespace-uri-from-QName (XQuery)](../xquery/functions-related-to-qnames-namespace-uri-from-qname.md)|  
|[Funzioni per le estensioni XQuery di SQL Server](./xquery-extension-functions-sql-column.md)|[Funzione sql:column() (XQuery)](../xquery/xquery-extension-functions-sql-column.md)|  
||[Funzione sql:variable() (XQuery)](../xquery/xquery-extension-functions-sql-variable.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [metodi con tipo di dati XML](../t-sql/xml/xml-data-type-methods.md)   
 [Riferimento al linguaggio XQuery &#40;SQL Server&#41;](../xquery/xquery-language-reference-sql-server.md)   
 [Dati XML &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)  
  
