---
description: Funzioni di aggregazione - count
title: Funzione Count (XQuery) | Microsoft Docs
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:count function
- count function [XQuery]
ms.assetid: a9f7131f-23e1-4d4d-a36c-180447543926
author: rothja
ms.author: jroth
ms.openlocfilehash: eb3fa2943ead0cb4b4000dbaa534f9b4b232dd3d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341977"
---
# <a name="aggregate-functions---count"></a>Funzioni di aggregazione - count
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Restituisce il numero di elementi contenuti nella sequenza specificata da *$arg*.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
fn:count($arg as item()*) as xs:integer  
```  
  
## <a name="arguments"></a>Argomenti  
 *$arg*  
 Elementi da contare.  
  
## <a name="remarks"></a>Commenti  
 Restituisce 0 se *$arg* è una sequenza vuota.  
  
## <a name="examples"></a>Esempio  
 In questo argomento vengono forniti esempi di XQuery sulle istanze XML archiviate in diverse colonne di tipo **XML** nel database AdventureWorks.  
  
### <a name="a-using-the-count-xquery-function-to-count-the-number-of-work-center-locations-in-the-manufacturing-of-a-product-model"></a>R. Utilizzo della funzione XQuery count() per il conteggio del numero di centri di lavorazione nella produzione di un modello di prodotto  
 La query seguente conta il numero di centri di lavorazione nel processo di produzione di un modello di prodotto (ProductModelID=7).  
  
```  
SELECT Production.ProductModel.ProductModelID,   
       Production.ProductModel.Name,   
       Instructions.query('  
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
       <NoOfWorkStations>  
          { count(/AWMI:root/AWMI:Location) }  
       </NoOfWorkStations>  
') as WorkCtrCount  
FROM Production.ProductModel  
WHERE Production.ProductModel.ProductModelID=7  
```  
  
 Dalla query precedente si noti quanto segue:  
  
-   La parola chiave **namespace** nel [prologo XQuery](../xquery/modules-and-prologs-xquery-prolog.md) definisce un prefisso dello spazio dei nomi. Il prefisso viene quindi utilizzato nel corpo della query XQuery.  
  
-   La query costruisce il codice XML che include l' `NoOfWorkStations` elemento <>.  
  
-   La funzione **Count ()** nel corpo della query XQuery conta il numero di `Location` elementi <>.  
  
 Risultato:  
  
```  
ProductModelID   Name                 WorkCtrCount       
-------------- ---------------------------------------------------  
7             HL Touring Frame  <NoOfWorkStations>6</NoOfWorkStations>     
```  
  
 È inoltre possibile costruire il codice XML in modo che includa ID e nome del modello di prodotto, come illustrato nella query seguente:  
  
```  
SELECT Instructions.query('  
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
       <NoOfWorkStations  
             ProductModelID= "{ sql:column("Production.ProductModel.ProductModelID") }"   
             ProductModelName = "{ sql:column("Production.ProductModel.Name") }" >  
          { count(/AWMI:root/AWMI:Location) }  
       </NoOfWorkStations>  
') as WorkCtrCount  
FROM Production.ProductModel  
WHERE Production.ProductModel.ProductModelID= 7  
```  
  
 Risultato:  
  
```  
<NoOfWorkStations ProductModelID="7"   
                  ProductModelName="HL Touring Frame">6</NoOfWorkStations>  
```  
  
 È possibile ottenere la restituzione dei valori in un tipo non xml, come illustrato nella query seguente. La query usa il [metodo value () (tipo di dati XML)](../t-sql/xml/value-method-xml-data-type.md) per recuperare il conteggio delle posizioni del centro di lavorazione.  
  
```  
SELECT  ProductModelID,   
        Name,   
        Instructions.value('declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
           count(/AWMI:root/AWMI:Location)', 'int' ) as WorkCtrCount  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Risultato:  
  
```  
ProductModelID    Name            WorkCtrCount  
-------------- ---------------------------------  
7              HL Touring Frame        6     
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni XQuery per il tipo di dati XML](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
