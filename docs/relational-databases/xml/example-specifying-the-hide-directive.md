---
title: 'Esempio: specifica della direttiva HIDE | Microsoft Docs'
description: Vedere un esempio di uso della direttiva HIDE in una query SQL.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- HIDE directive
ms.assetid: 87504d87-1cbd-412a-9041-47884b6efcec
author: rothja
ms.author: jroth
ms.openlocfilehash: 8e32d48d14a3a0cfabb054f92fb4536b59088734
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107488538"
---
# <a name="example-specifying-the-hide-directive"></a>Esempio: specifica della direttiva HIDE
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Nell'esempio seguente viene illustrato l'utilizzo della direttiva **HIDE** . Questa direttiva si rivela utile quando si desidera la restituzione di un attributo in base al quale ordinare le righe nella tabella universale restituita dalla query, ma non si desidera che tale attributo venga visualizzato nel documento XML finale.  
  
 La query genera il codice XML seguente:  
  
```  
<ProductModel ProdModelID="19" Name="Mountain-100">  
  <Summary>  
    <SummaryDescription>  
           <Summary> element from XML stored in CatalogDescription column  
    </SummaryDescription>  
  </Summary>  
</ProductModel>  
```  
  
 Questa query genera il codice XML desiderato. La query identifica due gruppi di colonne con valori di Tag 1 e 2 nei nomi delle colonne.  
  
 Questa query usa il metodo [query() (tipo di dati xml)](../../t-sql/xml/query-method-xml-data-type.md) del tipo di dati **xml** per eseguire query sulla colonna CatalogDescription di tipo **xml** e recuperare la descrizione di riepilogo. La query usa anche il [metodo value() (tipo di dati xml)](../../t-sql/xml/value-method-xml-data-type.md) del tipo di dati **xml** per il recupero del valore ProductModelID dalla colonna CatalogDescription. Questo valore non è necessario nel codice XML risultante, ma lo è per ordinare il set di righe risultante. Pertanto, il nome della colonna, `[Summary!2!ProductModelID!HIDE]`include la direttiva **HIDE** . Se questa colonna non viene inclusa nell'istruzione SELECT sarà necessario ordinare il set di righe per `[ProductModel!1!ProdModelID]` e `[Summary!2!SummaryDescription]` che sono di tipo **xml** e non sarà possibile utilizzare la colonna di tipo **xml** in ORDER BY. Viene pertanto aggiunta la colonna `[Summary!2!ProductModelID!HIDE]` , che viene poi specificata nella clausola ORDER BY.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT  1 as Tag,  
        0 as Parent,  
        ProductModelID     as [ProductModel!1!ProdModelID],  
        Name               as [ProductModel!1!Name],  
        NULL               as [Summary!2!ProductModelID!hide],  
        NULL               as [Summary!2!SummaryDescription]  
FROM    Production.ProductModel  
WHERE   CatalogDescription is not null  
UNION ALL  
SELECT  2 as Tag,  
        1 as Parent,  
        ProductModelID,  
        Name,  
        CatalogDescription.value('  
         declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
       (/PD:ProductDescription/@ProductModelID)[1]', 'int'),  
        CatalogDescription.query('  
         declare namespace pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
         /pd:ProductDescription/pd:Summary')  
FROM    Production.ProductModel  
WHERE   CatalogDescription is not null  
ORDER BY [ProductModel!1!ProdModelID],[Summary!2!ProductModelID!hide]  
FOR XML EXPLICIT  
go  
```  
  
 Risultato:  
  
```  
<ProductModel ProdModelID="19" Name="Mountain-100">  
  <Summary>  
    <SummaryDescription>  
      <pd:Summary xmlns:pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription" xmlns="">  
        <p1:p xmlns:p1="http://www.w3.org/1999/xhtml">Our top-of-the-line competition mountain bike. Performance-enhancing options include the innovative HL Frame, super-smooth front suspension, and traction for all terrain. </p1:p>  
      </pd:Summary>  
    </SummaryDescription>  
  </Summary>  
</ProductModel>  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Usare la modalità EXPLICIT con FOR XML](../../relational-databases/xml/use-explicit-mode-with-for-xml.md)  
  
  
