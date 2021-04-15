---
title: Supporto del tipo di dati xml in FOR XML| Microsoft Docs
description: Informazioni sull'uso delle query FOR XML sulle colonne del database SQL con tipo di dati xml.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- user-defined functions [SQL Server], XML
- xml data type [SQL Server], FOR XML clause
ms.assetid: 365de07d-694c-4c8b-b671-8825be27f87c
author: rothja
ms.author: jroth
ms.openlocfilehash: 588771aa272ec10c9b2654e40bd71fe6e0ccbd4b
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490862"
---
# <a name="for-xml-support-for-the-xml-data-type"></a>Supporto del tipo di dati xml in FOR XML
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Se nella clausola SELECT di una query FOR XML è specificata una colonna di tipo **xml** , viene eseguito il mapping dei valori della colonna come elementi nel codice XML risultante, indipendentemente dal fatto che sia stata specificata o meno la direttiva ELEMENTS. Le dichiarazioni XML nella colonna di tipo **xml** non sono serializzate.  
  
 Ad esempio, la query seguente recupera le informazioni di contatto del cliente, quali le colonne `BusinessEntityID`, `FirstName`e `LastName` , e i numeri di telefono dalla colonna `AdditionalContactInfo` di tipo **xml** .  
  
```  
USE AdventureWorks2012;  
GO  
SELECT BusinessEntityID, FirstName, LastName, AdditionalContactInfo.query('  
declare namespace act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes";  
 //act:telephoneNumber/act:number  
') AS PhoneNumber  
FROM Person.Person  
WHERE AdditionalContactInfo.query('  
declare namespace act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes";  
 //act:telephoneNumber/act:number  
')IS NOT NULL  
FOR XML AUTO, TYPE;  
```  
  
 Poiché nella query non è specificata la direttiva ELEMENTS, i valori di colonna vengono restituiti come attributi, ad eccezione delle informazioni aggiuntive sui contatti recuperate dalla colonna di tipo **xml** , che vengono restituite come elementi.  
  
 Risultato parziale:  
  
 `<Person.Person BusinessEntityID="291" FirstName="Gustavo" LastName="Achong">`  
  
 `<PhoneNumber>`  
  
 `<act:number xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">425-555-1112</act:number>`  
  
 `<act:number xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">425-555-1111</act:number>`  
  
 `</PhoneNumber>`  
  
 `</Person.Person>`  
  
 `<Person.Person BusinessEntityID="293" FirstName="Catherine" LastName="Abel">`  
  
 `<PhoneNumber>`  
  
 `<act:number xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">206-555-2222</act:number>`  
  
 `<act:number xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">206-555-1234</act:number>`  
  
 `</PhoneNumber>`  
  
```  
</Person.Person>  
...  
```  
  
 Se si specifica un alias di colonna per la colonna XML generata da XQuery, tale alias consente di aggiungere un elemento wrapper intorno al codice XML generato da XQuery. Ad esempio, la query seguente specifica `MorePhoneNumbers` come alias di colonna:  
  
```  
SELECT BusinessEntityID, FirstName, LastName, AdditionalContactInfo.query('  
declare namespace act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes";  
 //act:telephoneNumber/act:number  
') AS PhoneNumber  
FROM Person.Person  
WHERE AdditionalContactInfo.query('  
declare namespace act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes";  
 //act:telephoneNumber/act:number  
')IS NOT NULL  
FOR XML AUTO, TYPE;  
```  
  
 Il codice XML restituito da XQuery viene inserito nell'elemento <`MorePhoneNumbers`>, come illustrato nel risultato parziale seguente:  
  
 `<Person.Person BusinessEntityID="291" FirstName="Gustavo" LastName="Achong">`  
  
 `<MorePhoneNumbers>`  
  
 `<act:number xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">425-555-1112</act:number>`  
  
 `<act:number xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">425-555-1111</act:number>`  
  
 `</MorePhoneNumbers>`  
  
 `</Person.Person>`  
  
 `<Person.Person BusinessEntityID="293" FirstName="Catherine" LastName="Abel">`  
  
 `<MorePhoneNumbers>`  
  
 `<act:number xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">206-555-2222</act:number>`  
  
 `<act:number xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">206-555-1234</act:number>`  
  
 `</MorePhoneNumbers>`  
  
```  
</Person.Person>  
...  
```  
  
 Se si specifica la direttiva ELEMENTS nella query, BusinessEntityID, LastName e FirstName verranno restituiti come elementi nel codice XML risultante.  
  
 Nell'esempio seguente viene dimostrato che, in base alla logica di elaborazione di FOR XML, le dichiarazioni XML contenute nei dati XML di una colonna di tipo **xml** non vengono serializzate:  
  
```  
create table t(i int, x xml)  
go  
insert into t values(1, '<?xml version="1.0" encoding="UTF-8" ?>  
                             <Root SomeID="10" />')  
select i, x  
from   t  
for xml auto;  
```  
  
 Di seguito è riportato il risultato. Nel risultato la dichiarazione XML <`?xml version="1.0" encoding="UTF-8" ?`> non è serializzata.  
  
```  
<root>  
  <t i="1">  
    <x>  
      <Root SomeID="10" />  
    </x>  
  </t>  
</root>  
```  
  
## <a name="returning-xml-from-a-user-defined-function"></a>Restituzione di codice XML da una funzione definita dall'utente  
 Le query FOR XML consentono di restituire codice XML da una funzione definita dall'utente che restituisce quanto segue:  
  
-   Una tabella con una singola colonna di tipo **xml**  
  
-   Un'istanza di tipo **xml**  
  
 Ad esempio, la funzione definita dall'utente seguente restituisce una tabella con una singola colonna di tipo **xml**:  
  
```  
USE AdventureWorks2012;  
GO  
CREATE FUNCTION dbo.MyUDF (@ProudctModelID int)  
RETURNS @T TABLE  
  (  
     ProductDescription xml  
  )  
AS  
BEGIN  
  INSERT @T  
     SELECT CatalogDescription.query('  
declare namespace PD="https://www.adventure-works.com/schemas/products/description";  
                    //PD:ProductDescription  ')  
     FROM Production.ProductModel  
     WHERE ProductModelID = @ProudctModelID  
  RETURN  
END;  
```  
  
 È possibile eseguire la funzione definita dall'utente ed eseguire una query sulla tabella restituita dalla funzione. In questo esempio, il codice XML restituito dalla query eseguita sulla tabella viene assegnato a una variabile di tipo **xml** .  
  
```  
declare @x xml;  
set @x = (SELECT * FROM MyUDF(19));  
select @x;  
```  
  
 Di seguito è riportato un altro esempio di una funzione definita dall'utente, che restituisce un'istanza del tipo **xml** . Nell'esempio la funzione definita dall'utente restituisce un'istanza di tipo XML perché è specificato lo spazio dei nomi dello schema.  
  
```  
DROP FUNCTION dbo.MyUDF;  
GO  
CREATE FUNCTION MyUDF (@ProductModelID int)   
RETURNS xml ([Production].[ProductDescriptionSchemaCollection])  
AS  
BEGIN  
  declare @x xml  
  set @x =   ( SELECT CatalogDescription  
          FROM Production.ProductModel  
          WHERE ProductModelID = @ProductModelID )  
  return @x  
END;  
```  
  
 Il codice XML restituito dalla funzione definita dall'utente può quindi essere assegnato a una variabile di tipo **xml** , come illustrato nell'esempio seguente:  
  
```  
declare @x xml;  
SELECT @x= dbo.MyUDF4 (19) ;  
select @x;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Supporto di FOR XML per vari tipi di dati di SQL Server](../../relational-databases/xml/for-xml-support-for-various-sql-server-data-types.md)  
  
  
