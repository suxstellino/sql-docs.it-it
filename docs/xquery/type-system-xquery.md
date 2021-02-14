---
title: Sistema di tipi (XQuery) | Microsoft Docs
description: Informazioni sul sistema di tipi XQuery che include tipi incorporati di XML Schema e tipi definiti nello spazio dei nomi XPath-DataTypes.
ms.custom: ''
ms.date: 08/10/2016
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- sequence [XQuery]
- type system [XQuery]
- typed values [XQuery]
- XQuery, sequence
- string values [XQuery]
- XQuery, XPath data type namespace
- xdt prefix [XML in SQL Server]
- XQuery, type system
- built-in XML schema types [SQL Server]
- xs prefix [XML in SQL Server]
ms.assetid: 22d6f861-d058-47ee-b550-cbe9092dcb12
author: rothja
ms.author: jroth
ms.openlocfilehash: a92b26642f5759044233d5d63b5a432ee5fe426c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352845"
---
# <a name="type-system-xquery"></a>Sistema di tipi (XQuery)
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  XQuery è un linguaggio fortemente tipizzato per i tipi di schema e tipizzato in modo debole per i dati non tipizzati. I tipi predefiniti di XQuery sono:  
  
-   Tipi incorporati di XML Schema nello **http://www.w3.org/2001/XMLSchema** spazio dei nomi.  
  
-   Tipi definiti nello **http://www.w3.org/2004/07/xpath-datatypes** spazio dei nomi.  
  
 Questo argomento descrive inoltre quanto segue:  
  
-   Valore tipizzato e valore stringa di un nodo.  
  
-   La [funzione dati &#40;&#41;XQuery ](../xquery/data-accessor-functions-data-xquery.md) e la [funzione stringa &#40;XQuery&#41;](../xquery/data-accessor-functions-string-xquery.md).  
  
-   Individuazione di una corrispondenza per il tipo di sequenza restituito da un'espressione.  
  
## <a name="built-in-types-of-xml-schema"></a>Tipi predefiniti di XML Schema  
 I tipi predefiniti di XML Schema hanno un prefisso predefinito xs Alcuni di questi tipi includono **xs: integer** e **xs: String**. Tutti questi tipi predefiniti sono supportati ed è possibile utilizzarli per la creazione di una raccolta di XML Schema.  
  
 Quando si esegue una query su codice XML tipizzato, il tipo statico e dinamico dei nodi è determinato dalla raccolta di XML Schema associata alla colonna o alla variabile su cui viene eseguita la query. Per ulteriori informazioni sui tipi statici e dinamici, vedere il [contesto dell'espressione e la valutazione delle Query &#40;XQuery&#41;](../xquery/expression-context-and-query-evaluation-xquery.md). Ad esempio, la query seguente viene specificata in base a una colonna **XML** tipizzata ( `Instructions` ). Nell'espressione viene utilizzato l'elemento `instance of` per verificare che il valore tipizzato dell'attributo `LotSize` restituito sia di tipo `xs:decimal`.  
  
```  
SELECT Instructions.query('  
   DECLARE namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
   data(/AWMI:root[1]/AWMI:Location[@LocationID=10][1]/@LotSize)[1] instance of xs:decimal  
') AS Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Queste informazioni relative alla tipizzazione vengono fornite dalla raccolta di XML Schema associata alla colonna.  
  
## <a name="types-defined-in-xpath-data-types-namespace"></a>Tipi definiti nello spazio dei nomi dei tipi di dati XPath  
 I tipi definiti nello **http://www.w3.org/2004/07/xpath-datatypes** spazio dei nomi hanno un prefisso predefinito di **xdt**. Per questi tipi sono valide le osservazioni seguenti:  
  
-   Non è possibile utilizzarli per la creazione di una raccolta di XML Schema. Questi tipi vengono utilizzati nel sistema di tipi XQuery e vengono utilizzati per [XQuery e tipizzazione statica](../xquery/xquery-and-static-typing.md). È possibile eseguire il cast ai tipi atomici, ad esempio **xdt: untypedAtomic**, nello spazio dei nomi **xdt** .  
  
-   Quando si esegue una query su codice XML non tipizzato, il tipo statico e dinamico dei nodi elemento è **xdt: non tipizzato** e il tipo di valori di attributo è **xdt: untypedAtomic**. Il risultato di un metodo **query ()** genera codice XML non tipizzato. Ciò significa che i nodi XML vengono restituiti rispettivamente come **xdt: untyped** e **xdt: untypedAtomic**.  
  
-   I tipi **xdt: dayTimeDuration** e **xdt: yearMonthDuration** non sono supportati.  
  
 Nell'esempio seguente, la query viene eseguita su una variabile XML non tipizzata. L'espressione `data(/a[1]`) restituisce una sequenza di un singolo valore atomico. La funzione `data()` restituisce il valore tipizzato dell'elemento `<a>`. Il codice XML su cui viene eseguita la query è non tipizzato e pertanto il tipo del valore restituito è `xdt:untypedAtomic`. Di conseguenza, `instance of` restituisce true.  
  
```  
DECLARE @x xml  
SET @x='<a>20</a>'  
SELECT @x.query( 'data(/a[1]) instance of xdt:untypedAtomic' )  
```  
  
 Anziché recuperare il valore tipizzato, l'espressione (`/a[1]`) dell'esempio seguente restituisce una sequenza di un singolo elemento, ovvero l'elemento `<a>`. Questo elemento viene utilizzato dall'espressione `instance of` per verificare che il valore restituito dall'espressione sia un nodo elemento di tipo `xdt:untyped type`.  
  
```  
DECLARE @x xml  
SET @x='<a>20</a>'  
-- Is this an element node whose name is "a" and type is xdt:untyped.  
SELECT @x.query( '/a[1] instance of element(a, xdt:untyped?)')  
-- Is this an element node of type xdt:untyped.  
SELECT @x.query( '/a[1] instance of element(*, xdt:untyped?)')  
-- Is this an element node?  
SELECT @x.query( '/a[1] instance of element()')  
```  
  
> [!NOTE]  
>  Quando si esegue una query su un'istanza XML tipizzata e l'espressione della query include l'asse padre, le informazioni relative al tipo statico dei nodi risultanti non sono più disponibili. Il tipo dinamico rimane tuttavia associato ai nodi.  
  
## <a name="typed-value-vs-string-value"></a>Valore tipizzato e valore stringa  
 A ogni nodo è associato un valore tipizzato e un valore stringa. Per i dati XML tipizzati, il tipo del valore tipizzato è fornito dalla raccolta di XML Schema associata alla colonna o alla variabile su cui viene eseguita la query. Per i dati XML non tipizzati, il tipo del valore tipizzato è **xdt: untypedAtomic**.  
  
 È possibile utilizzare la funzione **Data ()** o **String ()** per recuperare il valore di un nodo:  
  
-   La [funzione dati &#40;XQuery&#41;](../xquery/data-accessor-functions-data-xquery.md) restituisce il valore tipizzato di un nodo.  
  
-   La [funzione stringa &#40;XQuery&#41;](../xquery/data-accessor-functions-string-xquery.md) restituisce il valore stringa del nodo.  
  
 Nella raccolta di XML Schema seguente `root` viene definito l'elemento> <del tipo Integer:  
  
```  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema">  
      <element name="root" type="integer"/>  
</schema>'  
GO  
```  
  
 Nell'esempio seguente, l'espressione recupera innanzitutto il valore tipizzato di `/root[1]` e quindi vi aggiunge `3`.  
  
```  
DECLARE @x xml(SC)  
SET @x='<root>5</root>'  
SELECT @x.query('data(/root[1]) + 3')  
```  
  
 Nell'esempio seguente, l'espressione ha esito negativo perché `string(/root[1])` restituisce un valore di tipo string. Questo valore viene quindi passato a un operatore aritmetico che utilizza solo valori di tipo numeric come operandi.  
  
```  
-- Fails because the argument is string type (must be numeric primitive type).  
DECLARE @x xml(SC)  
SET @x='<root>5</root>'  
SELECT @x.query('string(/root[1]) + 3')  
```  
  
 Nell'esempio seguente viene calcolato il totale degli attributi `LaborHours`. La `data()` funzione recupera i valori tipizzati degli `LaborHours` attributi da tutti gli `Location` elementi <> per un modello di prodotto. In base alla XML Schema associata alla `Instruction` colonna, `LaborHours` è di tipo **xs: Decimal** .  
  
```  
SELECT Instructions.query('   
DECLARE namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";   
             sum(data(//AWMI:Location/@LaborHours))   
') AS Result   
FROM Production.ProductModel   
WHERE ProductModelID=7  
```  
  
 Il risultato restituito dalla query è 12.75.  
  
> [!NOTE]  
>  L'utilizzo esplicito della funzione **Data ()** in questo esempio è a solo illustrazione. Se non viene specificato, **Sum ()** applica in modo implicito la funzione **Data ()** per estrarre i valori tipizzati dei nodi.  
  
## <a name="see-also"></a>Vedere anche  
 [Modelli e autorizzazioni di SQL Server Profiler](../tools/sql-server-profiler/sql-server-profiler-templates-and-permissions.md)   
 [Nozioni fondamentali su XQuery](../xquery/xquery-basics.md)  
  
  
