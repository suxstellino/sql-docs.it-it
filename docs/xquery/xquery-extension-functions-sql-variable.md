---
title: 'Funzione SQL: Variable () (XQuery) | Microsoft Docs'
description: "Informazioni su come utilizzare la funzione di estensione XQuery SQL: Variable () per esporre una variabile che contiene un valore relazionale SQL all'interno di un'espressione XQuery."
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- sql:variable() function
- sql:variable function
ms.assetid: 6e2e5063-c1cf-4b5a-b642-234921e3f4f7
author: rothja
ms.author: jroth
ms.openlocfilehash: d879088a98bfa7e826c53e4b90822bc65caff57a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350386"
---
# <a name="xquery-extension-functions---sqlvariable"></a>Funzioni per estensioni XQuery - sql:variable()
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Espone in un'espressione XQuery una variabile che contiene un valore SQL relazionale.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sql:variable("variableName") as xdt:anyAtomicType?  
```  
  
## <a name="remarks"></a>Osservazioni  
 Come descritto nell'argomento [associazione di dati relazionali all'interno di codice XML](../t-sql/xml/binding-relational-data-inside-xml-data.md), è possibile utilizzare questa funzione quando si utilizzano [i metodi con tipo di dati XML](../t-sql/xml/xml-data-type-methods.md) per esporre un valore relazionale all'interno di XQuery.  
  
 Il [metodo query ()](../t-sql/xml/query-method-xml-data-type.md) , ad esempio, viene utilizzato per specificare una query su un'istanza XML archiviata in una variabile o in una colonna con tipo di dati **XML** . A volte è necessario creare query in grado di utilizzare anche valori contenuti in un parametro o in una variabile [!INCLUDE[tsql](../includes/tsql-md.md)], per mettere insieme dati relazionali e XML. A tale scopo, usare la funzione **SQL: Variable** .  
  
 Il valore SQL verrà mappato a un valore XQuery corrispondente e il relativo tipo sarà un tipo di base XQuery equivalente al tipo SQL corrispondente.  
  
 È possibile fare riferimento a un'istanza **XML** solo nel contesto dell'espressione di origine di un'istruzione XML-DML insert. in caso contrario, non è possibile fare riferimento a valori di tipo **XML** o di un tipo di Common Language Runtime (CLR) definito dall'utente.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-the-sqlvariable-function-to-bring-a-transact-sql-variable-value-into-xml"></a>R. Utilizzo della funzione sql:variable() per convertire in XML il valore di una variabile Transact-SQL  
 Nell'esempio seguente viene costruita un'istanza XML costituita da:  
  
-   Un valore (`ProductID`) ottenuto da una colonna non XML. La [funzione SQL: Column ()](../xquery/xquery-extension-functions-sql-column.md) viene utilizzata per associare questo valore nel codice XML.  
  
-   Un valore (`ListPrice`) ottenuto da una colonna non XML di un'altra tabella. La funzione `sql:column()` viene utilizzata anche in questo caso per associare tale valore nell'istanza XML.  
  
-   Un valore (`DiscountPrice`) ottenuto da una variabile [!INCLUDE[tsql](../includes/tsql-md.md)]. Il metodo `sql:variable()` viene utilizzato per associare tale valore nell'istanza XML.  
  
-   Valore ( `ProductModelName` ) da una colonna di tipo **XML** , per rendere la query più interessante.  
  
 Query:  
  
```sql
DECLARE @price money  
  
SET @price=2500.00  
SELECT ProductID, Production.ProductModel.ProductModelID,CatalogDescription.query('  
declare namespace pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
  
       <Product   
           ProductID="{ sql:column("Production.Product.ProductID") }"  
           ProductModelID= "{ sql:column("Production.Product.ProductModelID") }"  
           ProductModelName="{/pd:ProductDescription[1]/@ProductModelName }"  
           ListPrice="{ sql:column("Production.Product.ListPrice") }"  
           DiscountPrice="{ sql:variable("@price") }"  
        />')   
FROM Production.Product   
JOIN Production.ProductModel  
ON Production.Product.ProductModelID = Production.ProductModel.ProductModelID  
WHERE ProductID=771  
```  
  
 Dalla query precedente si noti quanto segue:  
  
-   Gli elementi XQuery utilizzati nel metodo `query()` costruiscono l'istanza XML.  
  
-   La `namespace` parola chiave viene usata per definire un prefisso dello spazio dei nomi nel [prologo XQuery](../xquery/modules-and-prologs-xquery-prolog.md). Tale prefisso viene creato perché il valore dell'attributo `ProductModelName` viene recuperato dalla colonna di tipo xml `CatalogDescription xml`, a cui è associato uno schema.  
  
 Risultato:  
  
```xml
<Product ProductID="771" ProductModelID="19"   
         ProductModelName="Mountain 100"   
         ListPrice="3399.99" DiscountPrice="2500" />  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni di estensione di SQL Server XQuery](./xquery-extension-functions-sql-column.md)   
 [Confronto dati XML tipizzati con dati XML non tipizzati](../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Dati XML &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)   
 [Creare istanze di dati XML](../relational-databases/xml/create-instances-of-xml-data.md)   
 [metodi con tipo di dati XML](../t-sql/xml/xml-data-type-methods.md)   
 [Linguaggio XML di manipolazione dei dati &#40;XML DML&#41;](../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
