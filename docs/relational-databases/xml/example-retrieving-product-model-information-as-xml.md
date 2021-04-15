---
title: 'Esempio: Recupero delle informazioni relative al modello del prodotto in formato XML | Microsoft Docs'
description: Visualizzare un esempio di come recuperare le informazioni sul modello di prodotto in formato XML usando la modalità RAW con la clausola FOR XML.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- RAW mode, retrieving XML information example
ms.assetid: 3828b4ca-3ab2-444f-9c58-8be6e7f064a6
author: rothja
ms.author: jroth
ms.openlocfilehash: f55cecbb0437bd9d5ac523600803a0aa8086d4b8
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107488607"
---
# <a name="example-retrieving-product-model-information-as-xml"></a>Esempio: Recupero delle informazioni relative al modello del prodotto in formato XML
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  La query seguente restituisce le informazioni relative al modello del prodotto. `RAW` la modalità è specificata nella clausola `FOR XML` .  
  
## <a name="example"></a>Esempio  
  
```  
USE AdventureWorks2012;  
GO  
SELECT ProductModelID, Name  
FROM Production.ProductModel  
WHERE ProductModelID=122 or ProductModelID=119  
FOR XML RAW;  
GO  
```  
  
 Risultato parziale:  
  
 `<row ProductModelID="122" Name="All-Purpose Bike Stand" />`  
  
 `<row ProductModelID="119" Name="Bike Wash" />`  
  
 Per recuperare il codice XML incentrato sugli elementi, è necessario specificare la direttiva `ELEMENTS` .  
  
```  
USE AdventureWorks2012;  
GO  
SELECT ProductModelID, Name  
FROM Production.ProductModel  
WHERE ProductModelID=122 or ProductModelID=119  
FOR XML RAW, ELEMENTS;  
GO  
```  
  
 Risultato:  
  
```  
<row>  
  <ProductModelID>122</ProductModelID>  
  <Name>All-Purpose Bike Stand</Name>  
</row>  
<row>  
  <ProductModelID>119</ProductModelID>  
  <Name>Bike Wash</Name>  
</row>  
```  
  
 È possibile specificare facoltativamente la direttiva `TYPE` per recuperare risultati di tipo **xml** . La direttiva `TYPE` non modifica il contenuto dei risultati. Solo il tipo di dati dei risultati viene modificato.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT ProductModelID, Name  
FROM Production.ProductModel  
WHERE ProductModelID=122 or ProductModelID=119  
FOR XML RAW, TYPE ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Usare la modalità RAW con FOR XML](../../relational-databases/xml/use-raw-mode-with-for-xml.md)  
  
  
