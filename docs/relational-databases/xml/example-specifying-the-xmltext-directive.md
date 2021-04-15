---
title: 'Esempio: Specifica della direttiva XMLTEXT | Microsoft Docs'
description: Informazioni su come gestire la parte non usata di un documento XML specificando la direttiva XMLTEXT in un'istruzione SELECT usando la modalità EXPLICIT.
ms.custom: ''
ms.date: 04/05/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- XMLTEXT directive
ms.assetid: e78008ec-51e8-4fd1-b86f-1058a781de17
author: rothja
ms.author: jroth
ms.openlocfilehash: 9479680633ae2c46799b703f250333cbab844351
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490839"
---
# <a name="example-specifying-the-xmltext-directive"></a>Esempio: Specifica della direttiva XMLTEXT
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Nell'esempio seguente viene illustrata la gestione dei dati nella colonna di overflow utilizzando la direttiva **XMLTEXT** in un'istruzione `SELECT` in modalità EXPLICIT.  
  
 Si consideri la tabella `Person` . La tabella contiene una colonna `Overflow` in cui viene archiviata la parte non utilizzata del documento XML.  
  
```  
USE tempdb;  
GO  
CREATE TABLE Person(PersonID varchar(5), PersonName varchar(20), Overflow nvarchar(200));  
GO  
INSERT INTO Person VALUES   
    ('P1','Joe',N'<SomeTag attr1="data">content</SomeTag>')  
   ,('P2','Joe',N'<SomeTag attr2="data"/>')  
   ,('P3','Joe',N'<SomeTag attr3="data" PersonID="P">content</SomeTag>');  
```  
  
 Questa query recupera colonne dalla tabella `Person` . Per la colonna `Overflow` non è specificato *AttributeName* , ma la *direttiva* è impostata su `XMLTEXT` nell'ambito della specifica del nome della colonna della tabella universale.  
  
```  
SELECT 1 as Tag, NULL as parent,  
       PersonID as [Parent!1!PersonID],  
       PersonName as [Parent!1!PersonName],  
       Overflow as [Parent!1!!XMLTEXT] -- No AttributeName; XMLTEXT directive  
FROM Person  
FOR XML EXPLICIT;  
```  
  
 Nel documento XML risultante:  
  
-   Poiché *AttributeName* non è specificato per la colonna `Overflow` ed è specificata la direttiva `xmltext`, gli attributi nell'elemento <`overflow`> vengono accodati all'elenco attributi dell'elemento <`Parent`> che li racchiude.  
  
-   Poiché l'attributo `PersonID` nell'elemento <`xmltext`> è in conflitto con l'attributo `PersonID` recuperato allo stesso livello dell'elemento, l'attributo dell'elemento <`xmltext`> viene ignorato, anche se `PersonID` è NULL. In linea generale, nell'overflow un attributo prevarrà sull'attributo con lo stesso nome.  
  
 Risultato:  
  
 ```   
 <Parent PersonID="P1" PersonName="Joe" attr1="data">content</Parent>  
 <Parent PersonID="P2" PersonName="Joe" attr2="data"></Parent>  
 <Parent PersonID="P3" PersonName="Joe" attr3="data">content</Parent>
 ```  
  
 Se i dati di overflow includono sottoelementi e si specifica la stessa query, i sottoelementi nella colonna `Overflow` vengono aggiunti come sottoelementi dell'elemento <`Parent`> che li racchiude.  
  
 Modificare, ad esempio, i dati della tabella `Person` in modo che la colonna `Overflow` includa sottoelementi.  
  
```  
USE tempdb;  
GO  
TRUNCATE TABLE Person;  
GO  
INSERT INTO Person VALUES   
    ('P1','Joe',N'<SomeTag attr1="data">content</SomeTag>')  
   ,('P2','Joe',N'<SomeTag attr2="data"/>')  
    ,('P3','Joe',N'<SomeTag attr3="data" PersonID="P"><name>PersonName</name></SomeTag>');  
```  
  
 Se si esegue la stessa query, i sottoelementi dell'elemento <`xmltext`> vengono aggiunti come sottoelementi dell'elemento <`Parent`> che li racchiude:  
  
```  
SELECT 1 as Tag, NULL as parent,  
       PersonID as [Parent!1!PersonID],  
       PersonName as [Parent!1!PersonName],  
       Overflow as [Parent!1!!XMLTEXT] -- no AttributeName, XMLTEXT directive  
FROM Person  
FOR XML EXPLICIT;  
```  
  
 Risultato:  
  
 ```   
 <Parent PersonID="P1" PersonName="Joe" attr1="data">content</Parent>  
 <Parent PersonID="P2" PersonName="Joe" attr2="data"></Parent>  
 <Parent PersonID="P3" PersonName="Joe" attr3="data">  
 <name>PersonName</name>  
 </Parent>
 ```  
  
 Se si specifica *AttributeName* con la direttiva `xmltext`, gli attributi dell'elemento <`overflow`> vengono aggiunti come attributi dei sottoelementi dell'elemento <`Parent`> che li racchiude. Il nome specificato per *AttributeName* diventa il nome del sottoelemento.  
  
 Nella query seguente l'argomento *AttributeName*, <`overflow`>, viene specificato assieme alla `xmltext`direttiva *:*  
  
```  
SELECT 1 as Tag, NULL as parent,  
       PersonID as [Parent!1!PersonID],  
       PersonName as [Parent!1!PersonName],  
       Overflow as [Parent!1!overflow!XMLTEXT] -- Overflow is AttributeName  
                      -- XMLTEXT is a directive  
FROM Person  
FOR XML EXPLICIT  
```  
  
 Risultato:  
  
 ```   
 <Parent PersonID="P1" PersonName="Joe">  
 <overflow attr1="data">content</overflow>  
 </Parent>  
 <Parent PersonID="P2" PersonName="Joe">  
 <overflow attr2="data" />  
 </Parent>  
 <Parent PersonID="P3" PersonName="Joe">  
 <overflow attr3="data" PersonID="P">  
 <name>PersonName</name>  
 </overflow>  
 </Parent>
 ```  
  
 In questo elemento di query la *direttiva* viene specificata per l'attributo `PersonName` . `PersonName` verrà pertanto aggiunto come sottoelemento dell'elemento <`Parent`> che lo racchiude. Gli attributi dell'elemento <`xmltext`> vengono aggiunti all'elemento <`Parent`> che li racchiude. Il contenuto dell'elemento <`overflow`> e i sottoelementi vengono anteposti agli altri sottoelementi degli elementi <`Parent`> che li racchiudono.  
  
```  
SELECT 1      AS Tag, NULL as parent,  
       PersonID   AS [Parent!1!PersonID],  
       PersonName AS [Parent!1!PersonName!element], -- element directive  
       Overflow   AS [Parent!1!!XMLTEXT]  
FROM Person  
FOR XML EXPLICIT;  
```  
  
 Risultato:  
  
 ```   
 <Parent PersonID="P1" attr1="data">content<PersonName>Joe</PersonName>  
 </Parent>  
 <Parent PersonID="P2" attr2="data">  
 <PersonName>Joe</PersonName>  
 </Parent>  
 <Parent PersonID="P3" attr3="data">  
 <name>PersonName</name>  
 <PersonName>Joe</PersonName>  
 </Parent>
 ```  
  
 Se i dati della colonna `XMLTEXT` includono attributi per l'elemento radice, tali attributi non compaiono nello schema dei dati XML e il parser MSXML non convalida il frammento di documento XML risultante. Ad esempio:  
  
```  
SELECT 1 AS Tag,  
       0 ASParent,  
       N'<overflow a="1"/>' AS 'overflow!1!!xmltext'  
FOR XML EXPLICIT, xmldata;  
```  
  
 Di seguito è riportato il risultato. L'attributo di overflow `a` manca dallo schema restituito:  
  
 ```   
 <Schema name="Schema2"  
 xmlns="urn:schemas-microsoft-com:xml-data"  
 xmlns:dt="urn:schemas-microsoft-com:datatypes">  
 <ElementType name="overflow" content="mixed" model="open">`  
 </ElementType>`  
 </Schema>`  
 <overflow xmlns="x-schema:#Schema2" a="1">  
 </overflow>
 ```  
  
## <a name="see-also"></a>Vedere anche  
 [Usare la modalità EXPLICIT con FOR XML](../../relational-databases/xml/use-explicit-mode-with-for-xml.md)  
  
  
