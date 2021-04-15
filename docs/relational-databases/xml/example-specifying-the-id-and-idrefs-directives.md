---
title: 'Esempio: Specifica delle direttive ID e IDREFS | Microsoft Docs'
description: Informazioni su come la specifica delle direttive ID e IDREFS in una query SQL può abilitare i collegamenti all'interno del documento.
ms.custom: fresh2019may
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- IDREFS directive
- ID directive
ms.assetid: 99b9f0d8-ecbb-4225-859f-881066c09785
author: rothja
ms.author: jroth
ms.openlocfilehash: 6da50deb87e9fa4b06119b06ca1f701c1075ddc7
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107488494"
---
# <a name="example-specifying-the-id-and-idrefs-directives"></a>Esempio: Specifica delle direttive ID e IDREFS

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Un attributo dell'elemento può essere specificato come attributo di tipo **ID** e l'attributo **IDREFS** può quindi essere utilizzato per fare riferimento a tale attributo. In questo modo è possibile creare collegamenti tra più documenti, in modo analogo alla relazione esistente tra chiave primaria e chiave esterna nei database relazionali.  
  
 Nell'esempio seguente viene illustrato l'utilizzo delle direttive **ID** e **IDREFS** per la creazione di attributi dei tipi **ID** e **IDREFS** . Poiché gli ID non possono essere valori Integer, nell'esempio i valori di ID vengono convertiti, ovvero ne viene eseguito il cast di tipo. Vengono utilizzati prefissi per i valori degli ID.  
  
 Si supponga di voler generare codice XML come illustrato di seguito:  
  
```xml
<Customer CustomerID="C1" SalesOrderIDList=" O11 O22 O33..." >
    <SalesOrder SalesOrderID="O11" OrderDate="..." />  
    <SalesOrder SalesOrderID="O22" OrderDate="..." />  
    <SalesOrder SalesOrderID="O33" OrderDate="..." />  
    ...  
</Customer>  
```  
  
L'attributo `SalesOrderIDList` dell'elemento `<Customer>` è un attributo multivalore che fa riferimento all'attributo `SalesOrderID` dell'elemento `<SalesOrder>`. Per stabilire il collegamento, è necessario dichiarare l'attributo `SalesOrderID` con il tipo di dati `ID` e l'attributo `SalesOrderIDList` dell'elemento `<Customer>` con il tipo di dati `IDREFS`. Poiché un cliente può inviare più ordini, viene utilizzato il tipo `IDREFS` .
  
 Anche gli attributi **IDREFS** hanno più di un valore. È pertanto necessario utilizzare una clausola SELECT separata che riutilizzerà le stesse informazioni di Tag, Parent e colonna chiave. È poi necessario usare `ORDER BY` per garantire che le sequenze di righe che compongono i valori **IDREFS** vengano visualizzate raggruppate sotto il rispettivo elemento padre.  
  
 Di seguito è riportata la query che genera il codice XML desiderato. La query utilizza le direttive `ID` e `IDREFS` per sovrascrivere i tipi nei nomi di colonna (`SalesOrder!2!SalesOrderID!ID`, `Customer!1!SalesOrderIDList!IDREFS`).  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT  1 as Tag,  
        0 as Parent,  
        C.CustomerID   [Customer!1!CustomerID],  
        NULL           [Customer!1!SalesOrderIDList!IDREFS],
        NULL           [SalesOrder!2!SalesOrderID!ID],  
        NULL           [SalesOrder!2!OrderDate]  
FROM   Sales.Customer C   

UNION ALL   
SELECT  1 as Tag,  
        0 as Parent,  
        C.CustomerID,  
        'O-'+CAST(SalesOrderID as varchar(10)),   
        NULL,  
        NULL  
FROM   Sales.Customer AS C  
INNER JOIN Sales.SalesOrderHeader AS SOH  
    ON  C.CustomerID = SOH.CustomerID  

UNION ALL  
SELECT 2 as Tag,  
       1 as Parent,  
        C.CustomerID,  
        NULL,  
        'O-'+CAST(SalesOrderID as varchar(10)),  
        OrderDate  
FROM   Sales.Customer AS C  
INNER JOIN Sales.SalesOrderHeader AS SOH  
    ON  C.CustomerID = SOH.CustomerID
ORDER BY [Customer!1!CustomerID] ,
         [SalesOrder!2!SalesOrderID!ID],  
         [Customer!1!SalesOrderIDList!IDREFS]  
FOR XML EXPLICIT;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Usare la modalità EXPLICIT con FOR XML](../../relational-databases/xml/use-explicit-mode-with-for-xml.md)  
  
  
