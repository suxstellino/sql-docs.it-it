---
title: 'Esempio: Specifica delle direttive ID e IDREF | Microsoft Docs'
description: Visualizzare un esempio di come specificare le direttive ID e IDREF in una query SQL.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- IDREF directive
- ID directive
ms.assetid: 7ff1ea73-71ca-4786-bd42-564f1b5de2d9
author: rothja
ms.author: jroth
ms.openlocfilehash: 98248798cc991c14017c3d0ceb6b5650f632ef9b
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490884"
---
# <a name="example-specifying-the-id-and-idref-directives"></a>Esempio: Specifica delle direttive ID e IDREF

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Questo esempio è pressoché lo stesso di [Specificare la direttiva ELEMENTXSINIL](../../relational-databases/xml/example-specifying-the-elementxsinil-directive.md) . L'unica differenza è che la query specifica le direttive **ID** e **IDREF** . Queste direttive sovrascrivono i tipi dell'attributo **SalesPersonID** negli elementi <`OrderHeader`> e <`OrderDetail`>. creano collegamenti tra più documenti. Per visualizzare i tipi sovrascritti è necessario lo schema. La query specifica pertanto l'opzione **XMLDATA** nella clausola FOR XML per ottenere il recupero dello schema.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT  1 as Tag,  
        0 as Parent,  
        SalesOrderID  as [OrderHeader!1!SalesOrderID!id],  
        OrderDate     as [OrderHeader!1!OrderDate],  
        CustomerID    as [OrderHeader!1!CustomerID],  
        NULL          as [SalesPerson!2!SalesPersonID],  
        NULL          as [OrderDetail!3!SalesOrderID!idref],
        NULL          as [OrderDetail!3!LineTotal],  
        NULL          as [OrderDetail!3!ProductID],  
        NULL          as [OrderDetail!3!OrderQty]  
FROM   Sales.SalesOrderHeader  
WHERE  SalesOrderID=43659 or SalesOrderID=43661  

UNION ALL   
SELECT 2 as Tag,  
       1 as Parent,  
        SalesOrderID,   
        NULL,  
        NULL,  
        SalesPersonID,    
        NULL,           
        NULL,           
        NULL,  
        NULL           
FROM   Sales.SalesOrderHeader  
WHERE  SalesOrderID=43659 or SalesOrderID=43661  

UNION ALL  
SELECT 3 as Tag,  
       1 as Parent,  
        SOD.SalesOrderID,  
        NULL,  
        NULL,  
        SalesPersonID,  
        SOH.SalesOrderID,  
        LineTotal,  
        ProductID,  
        OrderQty     
FROM    Sales.SalesOrderHeader SOH,
        Sales.SalesOrderDetail SOD  
WHERE   SOH.SalesOrderID = SOD.SalesOrderID  
AND     (SOH.SalesOrderID=43659 or SOH.SalesOrderID=43661)
ORDER BY [OrderHeader!1!SalesOrderID!id],
         [SalesPerson!2!SalesPersonID],  
         [OrderDetail!3!SalesOrderID!idref],
         [OrderDetail!3!LineTotal]

FOR XML EXPLICIT, XMLDATA;
```  
  
 Di seguito è riportato il risultato parziale. Nello schema le direttive **ID** e **IDREF** hanno sovrascritto i tipi di dati dell'attributo **SalesOrderID** negli elementi <`OrderHeader`> e <`OrderDetail`>. Se vengono rimosse le direttive, lo schema restituisce i tipi originali degli attributi.  
  
```xml
<Schema
       name="Schema1"
       xmlns="urn:schemas-microsoft-com:xml-data"
       xmlns:dt="urn:schemas-microsoft-com:datatypes">  
  <ElementType name="OrderHeader" content="mixed" model="open">  
    <AttributeType name="SalesOrderID" dt:type="id" />  
    <AttributeType name="OrderDate" dt:type="dateTime" />  
    <AttributeType name="CustomerID" dt:type="i4" />  
    <attribute type="SalesOrderID" />  
    <attribute type="OrderDate" />  
    <attribute type="CustomerID" />  
  </ElementType>  
  <ElementType name="SalesPerson" content="mixed" model="open">  
    <AttributeType name="SalesPersonID" dt:type="i4" />  
    <attribute type="SalesPersonID" />  
  </ElementType>  
  <ElementType name="OrderDetail" content="mixed" model="open">  
    <AttributeType name="SalesOrderID" dt:type="idref" />  
    <AttributeType name="LineTotal" dt:type="number" />  
    <AttributeType name="ProductID" dt:type="i4" />  
    <AttributeType name="OrderQty" dt:type="i2" />  
    <attribute type="SalesOrderID" />  
    <attribute type="LineTotal" />  
    <attribute type="ProductID" />  
    <attribute type="OrderQty" />  
  </ElementType>  
</Schema>  
<OrderHeader
       xmlns="x-schema:#Schema1"
       SalesOrderID="43659"
       OrderDate="2001-07-01T00:00:00"
       CustomerID="676">  
  <SalesPerson SalesPersonID="279" />  
  <OrderDetail
         SalesOrderID="43659"
         LineTotal="10.373000"
         ProductID="712"
         OrderQty="2" />  
  ...  
</OrderHeader>  
...  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Usare la modalità EXPLICIT con FOR XML](../../relational-databases/xml/use-explicit-mode-with-for-xml.md)  
  
  
