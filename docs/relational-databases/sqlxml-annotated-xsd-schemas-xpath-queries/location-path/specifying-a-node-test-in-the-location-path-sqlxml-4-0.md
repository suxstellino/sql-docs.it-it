---
title: Specifica di un test del nodo nel percorso (SQLXML)
description: Informazioni su come specificare un test del nodo nel percorso di una query XPath SQLXML 4.0.
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- XPath queries [SQLXML], location paths
- principal node types [SQLXML]
- node tests [SQLXML]
- location path for XPath query
ms.assetid: f46c30bf-1e24-4435-9ac2-f8ba43a8ff94
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 365441f7ebd67bc1083149de9bb994910fdef180
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491816"
---
# <a name="specifying-a-node-test-in-the-location-path-sqlxml-40"></a>Specifica di un test di nodo nel percorso (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Un test di nodo specifica il tipo di nodo selezionato dal passo. Ogni asse (**figlio**, **padre,** **attributo** o **self**) ha un tipo di nodo principale. Per **l'asse** dell'attributo, il tipo di nodo principale è **\<attribute>** . Per gli **assi** padre , **figlio** **e** self, il tipo di nodo principale è **\<element>** .  
  
> [!NOTE]  
>  Il test di nodo con carattere jolly *, ad esempio `child::*`, non è supportato.  
  
## <a name="node-test-example-1"></a>Test del nodo: esempio 1  
 Il percorso seleziona `child::Customer` gli elementi figlio del nodo di **\<Customer>** contesto.  
  
 In questo esempio `child` è l'asse e `Customer` è il test di nodo. Il tipo di nodo principale per **l'asse** figlio è **\<element>** . Pertanto, il test del nodo è TRUE se **\<Customer>** il nodo è un **\<element>** nodo. Se il nodo di contesto non **\<Customer>** ha elementi figlio, viene restituito un set vuoto di nodi.  
  
## <a name="node-test-example-2"></a>Test di nodo: esempio 2  
 Il percorso seleziona `attribute::CustomerID` **l'attributo CustomerID** del nodo di contesto.  
  
 Nell'esempio `attribute` è l'asse e `CustomerID` è il test di nodo. Il tipo di nodo principale **dell'asse dell'attributo** è **\<attribute>** . Pertanto, il test del nodo è TRUE **se CustomerID** è un **\<attribute>** nodo. Se il nodo di contesto non **ha CustomerID,** viene restituito un set vuoto di nodi.  
  
> [!NOTE]  
>  In questa implementazione di XPath, se un passaggio di percorso fa riferimento a un o a un tipo non dichiarato nello schema, viene **\<element>** **\<attribute>** generato un errore. a differenza di quanto avviene con l'implementazione di XPath in MSXML, che restituisce un set di nodi vuoto.  
  
## <a name="abbreviated-syntax-for-the-axes"></a>Sintassi abbreviata per gli assi  
 Per il percorso è supportata la sintassi abbreviata seguente:  
  
-   `attribute::` può essere abbreviato utilizzando `@`.  
  
     Il percorso `Customer[@CustomerID="ALFKI"]` è uguale a `child::Customer[attribute::CustomerID="ALFKI"]`.  
  
-   `child::` può essere omesso da un passo.  
  
     Pertanto, **child** è l'asse predefinito. Il percorso `Customer/Order` è uguale a `child::Customer/child::Order`.  
  
-   `self::node()` può essere abbreviato utilizzando un punto (.), mentre `parent::node()` può essere abbreviato utilizzando due punti (..).  
  
  
