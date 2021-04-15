---
title: 'Esempi: Utilizzo della modalità AUTO | Microsoft Docs'
description: Visualizzare esempi di query che usano la modalità FOR XML AUTO.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- AUTO FOR XML mode, examples
ms.assetid: 11e8d0e4-df8a-46f8-aa21-9602d4f26cad
author: rothja
ms.author: jroth
ms.openlocfilehash: e9224e6c4b64161edbebafb2b2a5ee073dcbcdee
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490828"
---
# <a name="examples-using-auto-mode"></a>Esempi: Uso della modalità AUTO
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Negli esempi seguenti viene illustrato l'utilizzo della modalità AUTO. Molte di queste query vengono eseguite sui documenti XML con istruzioni per la produzione di biciclette, archiviati nella colonna Instructions della tabella ProductModel del database di esempio [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] .  
  
## <a name="example-retrieving-customer-order-and-order-detail-information"></a>Esempio: Recupero di informazioni sul cliente, l'ordine e i dettagli dell'ordine  
 Questa query recupera informazioni sul cliente, sull'ordine e sui dettagli dell'ordine per un cliente specifico.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT Cust.CustomerID,   
       OrderHeader.CustomerID,  
       OrderHeader.SalesOrderID,   
       Detail.SalesOrderID, Detail.LineTotal, Detail.ProductID,   
       Product.Name,  
       Detail.OrderQty  
FROM Sales.Customer AS Cust  
INNER JOIN Sales.SalesOrderHeader AS OrderHeader   
    ON Cust.CustomerID = OrderHeader.CustomerID  
INNER JOIN Sales.SalesOrderDetail AS Detail  
    ON OrderHeader.SalesOrderID = Detail.SalesOrderID  
INNER JOIN Production.Product AS Product  
    ON Product.ProductID = Detail.ProductID  
WHERE Cust.CustomerID IN (29672, 29734)  
ORDER BY OrderHeader.CustomerID,  
         OrderHeader.SalesOrderID  
FOR XML AUTO;  
```  
  
 Poiché la query identifica gli alias di tabella `Cust`, `OrderHeader`, `Detail`e `Product` , la modalità `AUTO` genera gli elementi corrispondenti. Anche in questo caso, la gerarchia di tali elementi è determinata dall'ordine in cui le tabelle vengono identificate dalle colonne specificate nella clausola `SELECT` .  
  
 Di seguito è riportato il risultato parziale.  
  
 `<Cust CustomerID="29672">`  
  
 `<OrderHeader CustomerID="29672" SalesOrderID="43660">`  
  
 `<Detail SalesOrderID="43660" LineTotal="874.794000" ProductID="758" OrderQty="1">`  
  
 `<Product Name="Road-450 Red, 52" />`  
  
 `</Detail>`  
  
 `<Detail SalesOrderID="43660" LineTotal="419.458900" ProductID="762" OrderQty="1">`  
  
 `<Product Name="Road-650 Red, 44" />`  
  
 `</Detail>`  
  
 `</OrderHeader>`  
  
 `<OrderHeader CustomerID="29672" SalesOrderID="47660">`  
  
 `<Detail SalesOrderID="47660" LineTotal="469.794000" ProductID="765" OrderQty="1">`  
  
 `<Product Name="Road-650 Black, 58" />`  
  
 `</Detail>`  
  
 `</OrderHeader>`  
  
 `<OrderHeader CustomerID="29672" SalesOrderID="49857">`  
  
 `<Detail SalesOrderID="49857" LineTotal="44.994000" ProductID="852" OrderQty="1">`  
  
 `<Product Name="Women's Tights, S" />`  
  
 `</Detail>`  
  
 `</OrderHeader>`  
  
 `...`  
  
 `</Cust>`  
  
## <a name="example-specifying-group-by-and-aggregate-functions"></a>Esempio: Specifica della clausola GROUP BY e di funzioni di aggregazione  
 La query seguente restituisce gli ID di singoli clienti e il numero degli ordini effettuati da tali clienti.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT C.CustomerID, COUNT(*) AS NoOfOrders  
FROM Sales.Customer AS C  
INNER JOIN Sales.SalesOrderHeader AS SOH  
On C.CustomerID = SOH.CustomerID  
GROUP BY C.CustomerID  
FOR XML AUTO;This is the partial result:  
```  
  
 `<I CustomerID="11000" NoOfOrders="3" />`  
  
 `<I CustomerID="11001" NoOfOrders="3" />`  
  
 `...`  
  
## <a name="example-specifying-computed-columns-in-auto-mode"></a>Esempio: Specifica di colonne calcolate in modalità AUTO  
 Questa query restituisce i nomi concatenati dei singoli clienti e le informazioni sugli ordini. Poiché la colonna calcolata viene assegnata al livello più interno rilevato fino a quel punto, che in questo esempio è l'elemento <`SOH`>, nel risultato i nomi concatenati dei clienti vengono aggiunti come attributi dell'elemento <`SOH`>.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT P.FirstName + ' ' + P.LastName AS Name,  
       SOH.SalesOrderID  
FROM Sales.Customer AS C  
INNER JOIN Sales.SalesOrderHeader AS SOH  
    ON  C.CustomerID = SOH.CustomerID  
INNER JOIN Person.Person AS P  
    ON P.BusinessEntityID = C.PersonID  
FOR XML AUTO;  
```  
  
 Risultato parziale:  
  
```  
<SOH Name="Jon Yang" SalesOrderID="43793" />  
<SOH Name="Eugene Huang" SalesOrderID="43767" />  
```  
  
 Per recuperare gli elementi <`IndividualCustomer`> che hanno come sottoelemento un attributo `Name` contenente le informazioni di intestazione di ogni ordine di vendita, la query viene riscritta utilizzando un'istruzione sub-SELECT. L'istruzione SELECT interna crea una tabella `IndividualCustomer` temporanea con la colonna calcolata contenente i nomi dei singoli clienti. Tale tabella viene quindi unita in join alla tabella `SalesOrderHeader` per ottenere il risultato.  
  
 Nella tabella `Sales.Customer` sono archiviate informazioni sui singoli clienti, incluso il valore `PersonID` di ogni cliente. Tale valore `PersonID` viene quindi utilizzato per recuperare il nome del contatto dalla tabella `Person.Person` .  
  
```  
SELECT IndividualCustomer.Name, SOH.SalesOrderID  
FROM (SELECT FirstName+ ' '+LastName AS Name, C.PersonID, C.CustomerID  
      FROM Sales.Customer AS C, Person.Person AS P  
      WHERE C.PersonID = P.BusinessEntityID) AS IndividualCustomer  
LEFT OUTER JOIN  Sales.SalesOrderHeader AS SOH  
   ON IndividualCustomer.CustomerID = SOH.CustomerID  
ORDER BY IndividualCustomer.CustomerID, SOH.CustomerIDFOR XML AUTO;  
```  
  
 Risultato parziale:  
  
 `<IndividualCustomer Name="Jon Yang">`  
  
 `<SOH SalesOrderID="43793" />`  
  
 `<SOH SalesOrderID="51522" />`  
  
 `<SOH SalesOrderID="57418" />`  
  
 `</IndividualCustomer>`  
  
 `...`  
  
 `...`  
  
## <a name="example-returning-binary-data"></a>Esempio: Recupero di dati binari  
 Questa query restituisce una foto del prodotto dalla tabella `ProductPhoto` . `ThumbNailPhoto` è una colonna **varbinary(max)** nella tabella `ProductPhoto` . Per impostazione predefinita, la modalità `AUTO` restituisce ai dati binari un riferimento costituito da un URL relativo alla radice virtuale del database in cui viene eseguita la query. Per identificare l'immagine, è necessario specificare l'attributo chiave `ProductPhotoID` . Per recuperare un riferimento a un'immagine come illustrato nell'esempio seguente, è inoltre necessario specificare la chiave primaria della tabella nella clausola `SELECT` , per identificare una riga in modo univoco.  
  
```  
SELECT ProductPhotoID, ThumbNailPhoto  
FROM   Production.ProductPhoto   
WHERE ProductPhotoID=70  
FOR XML AUTO;  
```  
  
 Risultato:  
  
 `-- result`  
  
 `<Production.ProductPhoto`  
  
 `ProductPhotoID="70"`  
  
 `ThumbNailPhoto= "dbobject/Production.ProductPhoto[@ProductPhotoID='70']/@ThumbNailPhoto" />`  
  
 Se si esegue la stessa query specificando l'opzione `BINARY BASE64` , i dati binari verranno restituiti in formato con codifica Base64.  
  
```  
SELECT ProductPhotoID, ThumbNailPhoto  
FROM   Production.ProductPhoto   
WHERE ProductPhotoID=70  
FOR XML AUTO, BINARY BASE64;  
```  
  
 Risultato:  
  
 `-- result`  
  
 `<Production.ProductPhoto ProductPhotoID="70" ThumbNailPhoto="Base64 encoded photo" />`  
  
 Per impostazione predefinita, quando si utilizza la modalità AUTO per recuperare dati binari, al posto dei dati binari viene restituito un riferimento a un URL relativo alla radice virtuale del database in cui è stata eseguita la query. Questa situazione si presenta se non è specificata l'opzione BINARY BASE64.  
  
 Quando la modalità AUTO restituisce un riferimento URL a dati binari in database senza distinzione tra maiuscole e minuscole, in cui un nome di tabella o colonna specificato nella query non corrisponde al nome della tabella o della colonna nel database, la query viene eseguita, ma la combinazione di maiuscole e minuscole nel riferimento non sarà consistente. Ad esempio:  
  
```  
SELECT ProductPhotoID, ThumbnailPhoto  
FROM   Production.ProductPhoto   
WHERE  ProductPhotoID=70  
FOR XML AUTO;  
```  
  
 Risultato:  
  
 `<Production.PRODUCTPHOTO`  
  
 `PRODUCTPHOTOID="70"`  
  
 `THUMBNAILPHOTO= "dbobject/Production.PRODUCTPHOTO[@ProductPhotoID='70']/@ThumbNailPhoto" />`  
  
 Questo costituisce un problema soprattutto quando si eseguono query dbobject su un database con distinzione tra maiuscole e minuscole. Per evitarlo, è necessario che la combinazione di maiuscole e minuscole nel nome di tabella o colonna specificato nelle query corrisponda a quella del nome della tabella o colonna nel database.  
  
## <a name="example-understanding-the-encoding"></a>Esempio: Informazioni sulla codifica  
 In questo esempio vengono illustrate le varie operazioni di codifica eseguite sul risultato.  
  
 Creare la tabella seguente:  
  
```  
CREATE TABLE [Special Chars] (Col1 char(1) primary key, [Col#&2] varbinary(50));  
```  
  
 Aggiungere i dati seguenti alla tabella:  
  
```  
INSERT INTO [Special Chars] VALUES ('&', 0x20), ('#', 0x20);  
```  
  
 La query restituisce i dati dalla tabella. È specificata la modalità AUTO della clausola FOR XML. I dati binari vengono restituiti come riferimento.  
  
```  
SELECT * FROM [Special Chars] FOR XML AUTO;  
```  
  
 Risultato:  
  
 `<Special_x0020_Chars`  
  
 `Col1="#"`  
  
 `Col_x0023__x0026_2="dbobject/Special_x0020_Chars[@Col1='#']/@Col_x0023__x0026_2"`  
  
 `/>`  
  
 `<Special_x0020_Chars`  
  
 `Col1="&"`  
  
 `Col_x0023__x0026_2="dbobject/Special_x0020_Chars[@Col1='&']/@Col_x0023__x0026_2"`  
  
 `/>`  
  
 Di seguito viene descritto il processo di codifica dei caratteri speciali nel risultato:  
  
-   I caratteri speciali XML e URL inclusi nei nomi degli elementi e degli attributi restituiti nel risultato della query vengono codificati utilizzando il valore esadecimale del carattere Unicode corrispondente. Nell'esempio precedente il nome di elemento <`Special Chars`> viene restituito come <`Special_x0020_Chars`>. Il nome di attributo <`Col#&2`> viene restituito come <`Col_x0023__x0026_2`>. Vengono codificati sia i caratteri XML speciali che i caratteri URL speciali.  
  
-   Se i valori degli elementi o degli attributi includono una delle cinque entità carattere XML standard (', "", \<, > e &), tali caratteri XML speciali verranno sempre codificati tramite la codifica dei caratteri XML. Nel risultato precedente il valore `&` nel valore dell'attributo <`Col1`> viene codificato come `&`. Il carattere #, tuttavia, rimane invariato perché è un carattere XML valido e non un carattere XML speciale.  
  
-   Se i valori degli elementi o degli attributi contengono eventuali caratteri URL speciali che hanno un significato particolare negli URL, tali caratteri verranno codificati solo nel valore URL DBOBJECT e unicamente se il carattere speciale fa parte del nome di una tabella o di una colonna. Nel risultato carattere `#` , che fa parte del nome di tabella `Col#&2` , viene codificato come `_x0023_ in the DBOJBECT URL`.  
  
## <a name="see-also"></a>Vedere anche  
 [Usare la modalità AUTO con FOR XML](../../relational-databases/xml/use-auto-mode-with-for-xml.md)  
  
  
