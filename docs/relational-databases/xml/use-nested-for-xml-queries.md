---
title: Usare query FOR XML annidate | Microsoft Docs
description: Informazioni sull'uso delle query FOR XML nidificate.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- FOR XML clause, nested FOR XML queries
- queries [XML in SQL Server], nested FOR XML
- nested FOR XML queries
ms.assetid: 7604161a-a958-446d-b102-7dee432979d0
author: rothja
ms.author: jroth
ms.openlocfilehash: ab01da1e5e3834caee4d8d0426b6d560a01c7bb2
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107487051"
---
# <a name="use-nested-for-xml-queries"></a>Utilizzo di query FOR XML nidificate
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Il tipo di dati **xml** e la [direttiva TYPE nelle query FOR XML](../../relational-databases/xml/type-directive-in-for-xml-queries.md) l'elaborazione dell'XML restituito dalle query FOR XML sia sul server che sul client.  
  
## <a name="processing-with-xml-type-variables"></a>Elaborazione con variabili di tipo XML  
 È possibile assegnare il risultato di una query FOR XML a una variabile di tipo **xml** oppure usare XQuery per eseguire query sul risultato e quindi assegnare tale risultato a una variabile di tipo **xml** per un'ulteriore elaborazione.  
  
```  
DECLARE @x xml  
SET @x=(SELECT ProductModelID, Name  
        FROM Production.ProductModel  
        WHERE ProductModelID=122 or ProductModelID=119  
        FOR XML RAW, TYPE)  
SELECT @x  
-- Result  
--<row ProductModelID="122" Name="All-Purpose Bike Stand" />  
--<row ProductModelID="119" Name="Bike Wash" />  
```  
  
 Il valore XML restituito nella variabile `@x`può essere ulteriormente elaborato usando uno dei metodi con tipo di dati **xml** . Ad esempio, è possibile recuperare il valore dell'attributo `ProductModelID` usando il [metodo value()](../../t-sql/xml/value-method-xml-data-type.md).  
  
```  
DECLARE @i int;  
SET @i = (SELECT @x.value('/row[1]/@ProductModelID[1]', 'int'));  
SELECT @i;  
```  
  
 Nell'esempio seguente, il risultato della query `FOR XML` viene restituito come tipo **xml** a causa della direttiva `TYPE` specificata nella clausola `FOR XML` .  
  
```  
SELECT ProductModelID, Name  
FROM Production.ProductModel  
WHERE ProductModelID=119 or ProductModelID=122  
FOR XML RAW, TYPE,ROOT('myRoot');  
  
```  
  
 Risultato:  
  
```  
<myRoot>  
  <row ProductModelID="122" Name="All-Purpose Bike Stand" />  
  <row ProductModelID="119" Name="Bike Wash" />  
</myRoot>  
```  
  
 Poiché il risultato è di tipo **xml** , è possibile applicare uno dei metodi con tipo di dati **xml** direttamente al valore XML, come illustrato nella query seguente. Nella query viene usato il [metodo query() con tipo di dati XML](../../t-sql/xml/query-method-xml-data-type.md) per recuperare il primo elemento <`row`> figlio dell'elemento <`myRoot`>.  
  
```  
SELECT  (SELECT ProductModelID, Name  
         FROM Production.ProductModel  
         WHERE ProductModelID=119 or ProductModelID=122  
         FOR XML RAW, TYPE,ROOT('myRoot')).query('/myRoot[1]/row[1]');  
  
```  
  
 Risultato:  
  
```  
<row ProductModelID="122" Name="All-Purpose Bike Stand" />  
```  
  
## <a name="returning-inner-for-xml-query-results-to-outer-queries-as-xml-type-instances"></a>Restituzione di risultati della query FOR XML interna a query esterne come istanze di tipo XML  
 È possibile scrivere query `FOR XML` nidificate in cui il risultato della query interna viene restituito alla query esterna come tipo di dati **xml** . Ad esempio:  
  
```  
SELECT Col1,   
       Col2,   
       ( SELECT Col3, Col4   
        FROM  T2  
        WHERE T2.Col = T1.Col  
        ...  
        FOR XML AUTO, TYPE )  
FROM T1  
WHERE ...  
FOR XML AUTO, TYPE;  
```  
  
 Dalla query precedente si noti quanto segue:  
  
-   Il valore XML generato dalla query `FOR XML` interna viene aggiunto al valore XML generato dalla query `FOR XML`esterna.  
  
-   Nella query interna viene specificata la direttiva `TYPE` . I dati restituiti dalla query interna sono quindi di tipo **xml** . Se non si specifica la direttiva TYPE, il risultato della query `FOR XML` interna viene restituito come **nvarchar(max)** e i dati XML vengono sostituiti con entità.  
  
## <a name="controlling-the-shape-of-resulting-xml-data"></a>Controllo della forma dei dati XML risultanti  
 Le query FOR XML nidificate consentono di avere un maggior controllo nella definizione della forma dei dati XML risultanti. È possibile utilizzare query FOR XML nidificate per costruire valori XML che siano parzialmente incentrati sugli attributi e parzialmente sugli elementi.  
  
 Per altre informazioni su come specificare valori XML incentrati sia sugli attributi sia sugli elementi con query FOR XML nidificate, vedere [Query FOR XML e query nidificata FOR XML a confronto](../../relational-databases/xml/for-xml-query-compared-to-nested-for-xml-query.md) e [Determinare la struttura dei valori XML tramite query nidificate FOR XML](../../relational-databases/xml/shape-xml-with-nested-for-xml-queries.md).  
  
 È possibile generare gerarchie XML che includono elementi di pari livello eseguendo query FOR XML nidificate in modalità AUTO. Per altre informazioni, vedere [Generare elementi di pari livello tramite query nidificate in modalità AUTO](../../relational-databases/xml/generate-siblings-with-a-nested-auto-mode-query.md).  
  
 Indipendentemente dalla modalità utilizzata, le query FOR XML nidificate consentono un maggior controllo nella definizione della forma dei valori XML risultanti e possono essere utilizzate in sostituzione delle query in modalità EXPLICIT.  
  
## <a name="examples"></a>Esempi  
 Negli argomenti seguenti vengono forniti esempi di query FOR XML nidificate.  
  
 [Query FOR XML e query FOR XML annidate a confronto](../../relational-databases/xml/for-xml-query-compared-to-nested-for-xml-query.md)  
 Confronto di una query FOR XML con un solo livello con una query FOR XML nidificata. In questo esempio è inclusa una dimostrazione di come specificare valori XML incentrati sia sugli attributi sia sugli elementi come risultato della query.  
  
 [Generare elementi di pari livello tramite query annidate in modalità AUTO](../../relational-databases/xml/generate-siblings-with-a-nested-auto-mode-query.md)  
 Procedura di generazione di elementi di pari livello tramite query nidificate in modalità AUTO  
  
 [Usare query FOR XML annidate in ASP.NET](../../relational-databases/xml/use-nested-for-xml-queries-in-asp-net.md)  
 Dimostrazione del modo in cui un'applicazione ASPX può utilizzare FOR XML per restituire XML da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [Determinare la struttura dei valori XML tramite query FOR XML annidate](../../relational-databases/xml/shape-xml-with-nested-for-xml-queries.md)  
 Utilizzo di query FOR XML nidificate per controllare la struttura di un documento XML creato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
  
