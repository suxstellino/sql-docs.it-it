---
title: Impostare i predicati di selezione nel percorso (SQLXML)
description: Informazioni su come la specifica dei predicati di selezione nell'espressione del percorso di una query XPath (SQLXML 4.0) filtra il set di nodi su cui viene eseguita una query.
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- XPath queries [SQLXML], predicates
- predicates [SQLXML]
- position-based filtering [SQLXML]
- XPath queries [SQLXML], location paths
- filtering [SQLXML]
- location path for XPath query
ms.assetid: dbef4cf4-a89b-4d7e-b72b-4062f7b29a80
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 06bcd85b4f3b25d0a01e5b82e1f6858049772b2e
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491838"
---
# <a name="specifying-selection-predicates-in-the-location-path-sqlxml-40"></a>Definizione di predicati di selezione nel percorso (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Un predicato filtra un set di nodi rispetto a un asse (simile a una clausola WHERE in un'istruzione SELECT). Il predicato viene specificato tra parentesi. Per filtrare ogni nodo nel set di nodi, l'espressione del predicato viene valutata con il nodo come nodo di contesto e con il numero di nodi nel set di nodi come dimensioni del contesto. Se l'espressione del predicato restituisce TRUE per il nodo, il nodo viene incluso nel set di nodi risultante.  
  
 XPath consente inoltre l'applicazione di filtri basata sulla posizione. Un'espressione del predicato valutata in numero seleziona il nodo dell'ordinale. Il percorso `Customer[3]`, ad esempio, restituisce il terzo cliente. Predicati numerici di questo tipo non sono supportati. Sono supportate solo le espressioni del predicato che restituiscono un risultato booleano.  
  
> [!NOTE]  
>  Per informazioni sulle limitazioni di questa implementazione XPath di XPath e sulle differenze tra di esso e la specifica W3C, vedere Introduzione all'uso di query [XPath &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/introduction-to-using-xpath-queries-sqlxml-4-0.md).  
  
## <a name="selection-predicate-example-1"></a>Predicato di selezione: esempio 1  
 L'espressione XPath seguente (percorso) seleziona dal nodo di contesto corrente tutti gli elementi figlio dell'elemento con l'attributo **\<Customer>** **CustomerID** con valore ALFKI:  
  
```  
/child::Customer[attribute::CustomerID="ALFKI"]  
```  
  
 In questa query XPath `child` e `attribute` sono nomi di asse. `Customer` è il test del nodo (TRUE se `Customer` è , perché è il tipo di nodo principale per **\<element node>** **\<element>** `child` l'asse). `attribute::CustomerID="ALFKI"` è il predicato. Nel predicato è l'asse ed è il test del nodo (TRUE se CustomerID è un attributo del nodo di contesto, perché è il tipo di nodo `attribute` principale `CustomerID`  **\<attribute>** dell'asse **dell'attributo).**  
  
 Utilizzando la sintassi abbreviata, la query XPath può essere specificata anche nel modo seguente:  
  
```  
/Customer[@CustomerID="ALFKI"]  
```  
  
## <a name="selection-predicate-example-2"></a>Predicato di selezione: esempio 2  
 L'espressione XPath seguente (percorso) seleziona dal nodo di contesto corrente tutti i nonni con l'attributo **\<Order>** **SalesOrderID** con valore 1:  
  
```  
/child::Customer/child::Order[attribute::SalesOrderID="1"]  
```  
  
 In questa espressione XPath `child` e `attribute` sono nomi di asse. `Customer`, `Order` e `SalesOrderID` sono i test di nodo. `attribute::OrderID="1"` è il predicato.  
  
 Utilizzando la sintassi abbreviata, la query XPath può essere specificata anche nel modo seguente:  
  
```  
/Customer/Order[@SalesOrderID="1"]  
```  
  
## <a name="selection-predicate-example-3"></a>Predicato di selezione: esempio 3  
 L'espressione XPath seguente (percorso) seleziona dal nodo di contesto corrente tutti gli elementi figlio **\<Customer>** che hanno uno o più elementi **\<ContactName>** figlio:  
  
```  
child::Customer[child::ContactName]  
```  
  
 In questo esempio si presuppone che sia un elemento figlio dell'elemento nel documento XML, definito mapping incentrato sugli elementi in uno **\<ContactName>** **\<Customer>** schema XSD con annotazioni.   
  
 In questa espressione XPath `child` è il nome dell'asse. `Customer` è il test del nodo (TRUE se `Customer` è un **\<element>** nodo, perché è il tipo di **\<element>** nodo principale per `child` l'asse). `child::ContactName` è il predicato. Nel predicato è `child` l'asse e `ContactName` è il test del nodo (TRUE se è un `ContactName` **\<element>** nodo).  
  
 Questa espressione restituisce solo gli **\<Customer>** elementi figlio del nodo di contesto che hanno elementi **\<ContactName>** figlio.  
  
 Utilizzando la sintassi abbreviata, la query XPath può essere specificata anche nel modo seguente:  
  
```  
Customer[ContactName]  
```  
  
## <a name="selection-predicate-example-4"></a>Predicato di selezione: esempio 4  
 L'espressione XPath seguente seleziona **\<Customer>** gli elementi figlio del nodo di contesto che non dispongono di elementi **\<ContactName>** figlio:  
  
```  
child::Customer[not(child::ContactName)]  
```  
  
 In questo esempio si presuppone che sia un elemento figlio dell'elemento nel documento XML e che il campo ContactName non **\<ContactName>** **\<Customer>** sia obbligatorio nel database .  
  
 In questo esempio, `child` è l'asse. `Customer` è il test del nodo (TRUE se `Customer` è un \<element> nodo). `not(child::ContactName)` è il predicato. Nel predicato è `child` l'asse e `ContactName` è il test del nodo (TRUE se è un `ContactName` \<element> nodo).  
  
 Utilizzando la sintassi abbreviata, la query XPath può essere specificata anche nel modo seguente:  
  
```  
Customer[not(ContactName)]  
```  
  
## <a name="selection-predicate-example-5"></a>Predicato di selezione: esempio 5  
 L'espressione XPath seguente seleziona dal nodo di contesto corrente tutti gli elementi **\<Customer>** figlio con **l'attributo CustomerID:**  
  
```  
child::Customer[attribute::CustomerID]  
```  
  
 In questo esempio è `child` l'asse e `Customer` è il test del nodo (TRUE se è un `Customer` \<element> nodo). `attribute::CustomerID` è il predicato. Nel predicato è `attribute` l'asse e `CustomerID` è il predicato (TRUE se `CustomerID` è un **\<attribute>** nodo).  
  
 Utilizzando la sintassi abbreviata, la query XPath può essere specificata anche nel modo seguente:  
  
```  
Customer[@CustomerID]  
```  
  
## <a name="selection-predicate-example-6"></a>Predicato di selezione: esempio 6  
 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] SQLXML 4.0 include il supporto per query XPath che contengono un prodotto incrociato nel predicato, come illustrato nell'esempio seguente:  
  
```  
Customer[Order/@OrderDate=Order/@ShipDate]  
```  
  
 La query seleziona tutti i clienti con un elemento `Order` per cui `OrderDate` è uguale a `ShipDate` di qualsiasi `Order`.  
  
## <a name="see-also"></a>Vedere anche  
 [Introduzione agli schemi XSD con annotazioni &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/introduction-to-annotated-xsd-schemas-sqlxml-4-0.md)   
 [Formattazione XML lato client &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml/formatting/client-side-xml-formatting-sqlxml-4-0.md)  
  
  
