---
title: Generare elementi di pari livello tramite query annidate in modalità AUTO | Microsoft Docs
description: Informazioni su come generare elementi di pari livello XML utilizzando una query nidificata in modalità AUTO come alternativa all'utilizzo della modalità EXPLICIT.
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- queries [XML in SQL Server], nested AUTO mode
- nested AUTO mode query
ms.assetid: 748d9899-589d-4420-8048-1258e9e67c20
author: rothja
ms.author: jroth
ms.openlocfilehash: 8edc13548f6a07785cf3523b8cc5c09f5d84bcb5
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107488184"
---
# <a name="generate-siblings-with-a-nested-auto-mode-query"></a>Generare elementi di pari livello tramite query nidificate in modalità AUTO
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Nell'esempio seguente viene descritta la procedura per generare elementi di pari livello tramite una query nidificata in modalità AUTO. L'unico metodo alternativo per generare un valore XML di questo tipo è utilizzare la modalità EXPLICIT, ma ciò può rivelarsi eccessivamente complesso.  
  
## <a name="example"></a>Esempio  
 Questa query costruisce un valore XML che contiene informazioni sugli ordini di vendita, Sono inclusi gli elementi seguenti:  
  
-   Informazioni contenute nell'intestazione degli ordini di vendita, `SalesOrderID`, `SalesPersonID`e `OrderDate`. [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] archivia queste informazioni nella tabella `SalesOrderHeader` .  
  
-   Informazioni dettagliate sugli ordini di vendita, che includono i prodotti ordinati, il prezzo unitario e la quantità ordinata. Tali informazioni sono archiviate nella tabella `SalesOrderDetail` .  
  
-   Informazioni sul venditore, ovvero la persona che ha ricevuto l'ordine. Il codice `SalesPerson` è archiviato nella tabella `SalesPersonID`. Per questa query è necessario unire in join tale tabella con la tabella `Employee` , per trovare il nome del venditore.  
  
 Le due query `SELECT` che seguono generano valori XML con struttura lievemente diversa.  
  
 La prima query genera un valore XML in cui <`SalesPerson`> e <`SalesOrderHeader`> sono elementi di pari livello figli di <`SalesOrder`>:  
  
```  
SELECT   
      (SELECT top 2 SalesOrderID, SalesPersonID, CustomerID,  
         (select top 3 SalesOrderID, ProductID, OrderQty, UnitPrice  
           from Sales.SalesOrderDetail  
            WHERE  SalesOrderDetail.SalesOrderID =   
                   SalesOrderHeader.SalesOrderID  
            FOR XML AUTO, TYPE)  
        FROM  Sales.SalesOrderHeader  
        WHERE SalesOrderHeader.SalesOrderID = SalesOrder.SalesOrderID  
        for xml auto, type),  
        (SELECT *   
         FROM  (SELECT SalesPersonID, EmployeeID  
              FROM Sales.SalesPerson, HumanResources.Employee  
              WHERE SalesPerson.SalesPersonID = Employee.EmployeeID) As   
                     SalesPerson  
         WHERE  SalesPerson.SalesPersonID = SalesOrder.SalesPersonID  
       FOR XML AUTO, TYPE)  
FROM (SELECT SalesOrderHeader.SalesOrderID, SalesOrderHeader.SalesPersonID  
      FROM Sales.SalesOrderHeader, Sales.SalesPerson  
      WHERE SalesOrderHeader.SalesPersonID = SalesPerson.SalesPersonID  
     ) as SalesOrder  
ORDER BY SalesOrder.SalesOrderID  
FOR XML AUTO, TYPE  
```  
  
 Nella query precedente l'istruzione `SELECT` più esterna esegue le operazioni seguenti:  
  
-   Esegue una query sul set di righe `SalesOrder`, specificato nella clausola `FROM`. Viene restituito un valore XML con uno o più elementi <`SalesOrder`>.  
  
-   Specifica la modalità `AUTO` e la direttiva `TYPE` . `AUTO` trasforma il risultato della query in XML e la direttiva `TYPE` restituisce il risultato come tipo **xml** .  
  
-   Include due istruzioni `SELECT` nidificate, separate da una virgola (,). La prima istruzione nidificata `SELECT` recupera le informazioni relative all'ordine di vendita, l'intestazione ed i dettagli, mentre la seconda istruzione `SELECT` recupera le informazioni relative al venditore.  
  
    -   L'istruzione `SELECT` che recupera `SalesOrderID`, `SalesPersonID`e `CustomerID` include a sua volta un'altra istruzione nidificata `SELECT ... FOR XML` (con modalità `AUTO` e direttiva `TYPE` ), che restituisce i dettagli dell'ordine di vendita.  
  
 L'istruzione `SELECT` che recupera le informazioni relative al venditore esegue una query sul set di righe `SalesPerson`, creato nella clausola `FROM` . Per consentire l'esecuzione delle query `FOR XML` , è necessario specificare un nome per il set di righe anonimo generato nella clausola `FROM` . In tal caso viene specificato il nome `SalesPerson`.  
  
 Risultato parziale:  
  
```  
<SalesOrder>  
  <Sales.SalesOrderHeader SalesOrderID="43659" SalesPersonID="279" CustomerID="676">  
    <Sales.SalesOrderDetail SalesOrderID="43659" ProductID="776" OrderQty="1" UnitPrice="2024.9940" />  
    <Sales.SalesOrderDetail SalesOrderID="43659" ProductID="777" OrderQty="3" UnitPrice="2024.9940" />  
    <Sales.SalesOrderDetail SalesOrderID="43659" ProductID="778" OrderQty="1" UnitPrice="2024.9940" />  
  </Sales.SalesOrderHeader>  
  <SalesPerson SalesPersonID="279" EmployeeID="279" />  
</SalesOrder>  
...  
```  
  
 La query seguente genera le stesse informazioni relative all'ordine di vendita, con la differenza che nel valore XML risultante <`SalesPerson`> appare un elemento di pari livello di <`SalesOrderDetail`>:  
  
```  
<SalesOrder>  
    <SalesOrderHeader ...>  
          <SalesOrderDetail .../>  
          <SalesOrderDetail .../>  
          ...  
          <SalesPerson .../>  
    </SalesOrderHeader>  
  
</SalesOrder>  
<SalesOrder>  
  ...  
</SalesOrder>  
```  
  
 Query:  
  
```  
SELECT SalesOrderID, SalesPersonID, CustomerID,  
             (select top 3 SalesOrderID, ProductID, OrderQty, UnitPrice  
              from Sales.SalesOrderDetail  
              WHERE SalesOrderDetail.SalesOrderID = SalesOrderHeader.SalesOrderID  
              FOR XML AUTO, TYPE),  
              (SELECT *   
               FROM  (SELECT SalesPersonID, EmployeeID  
                    FROM Sales.SalesPerson, HumanResources.Employee  
                    WHERE SalesPerson.SalesPersonID = Employee.EmployeeID) As SalesPerson  
               WHERE  SalesPerson.SalesPersonID = SalesOrderHeader.SalesPersonID  
         FOR XML AUTO, TYPE)  
FROM Sales.SalesOrderHeader  
WHERE SalesOrderID=43659 or SalesOrderID=43660  
FOR XML AUTO, TYPE  
```  
  
 Risultato:  
  
```  
<Sales.SalesOrderHeader SalesOrderID="43659" SalesPersonID="279" CustomerID="676">  
  <Sales.SalesOrderDetail SalesOrderID="43659" ProductID="776" OrderQty="1" UnitPrice="2024.9940" />  
  <Sales.SalesOrderDetail SalesOrderID="43659" ProductID="777" OrderQty="3" UnitPrice="2024.9940" />  
  <Sales.SalesOrderDetail SalesOrderID="43659" ProductID="778" OrderQty="1" UnitPrice="2024.9940" />  
  <SalesPerson SalesPersonID="279" EmployeeID="279" />  
</Sales.SalesOrderHeader>  
<Sales.SalesOrderHeader SalesOrderID="43660" SalesPersonID="279" CustomerID="117">  
  <Sales.SalesOrderDetail SalesOrderID="43660" ProductID="762" OrderQty="1" UnitPrice="419.4589" />  
  <Sales.SalesOrderDetail SalesOrderID="43660" ProductID="758" OrderQty="1" UnitPrice="874.7940" />  
  <SalesPerson SalesPersonID="279" EmployeeID="279" />  
</Sales.SalesOrderHeader>  
```  
  
 Poiché la direttiva `TYPE` restituisce il risultato della query come tipo **xml** , è possibile eseguire una query sul valore XML risultante usando vari metodi con tipo di dati **xml** . Per altre informazioni, vedere [Metodi con tipo di dati XML](../../t-sql/xml/xml-data-type-methods.md). In questa query di esempio, si noti quanto segue:  
  
-   La query precedente viene aggiunta nella clausola `FROM` . Il risultato della query viene restituito sotto forma di tabella. Viene aggiunto l'alias `XmlCol` .  
  
-   La clausola `SELECT` specifica una query XQuery sul valore `XmlCol` restituito nella clausola `FROM` . Per specificare l'espressione XQuery viene usato il metodo **query()** del tipo di dati **xml**. Per altre informazioni, vedere [Metodo query&#40;&#41; con &#40;tipo di dati XML&#41;](../../t-sql/xml/query-method-xml-data-type.md).  
  
    ```  
    SELECT XmlCol.query('<Root> { /* } </Root>')  
    FROM (  
    SELECT SalesOrderID, SalesPersonID, CustomerID,  
                 (select top 3 SalesOrderID, ProductID, OrderQty, UnitPrice  
                  from Sales.SalesOrderDetail  
                  WHERE SalesOrderDetail.SalesOrderID = SalesOrderHeader.SalesOrderID  
                  FOR XML AUTO, TYPE),  
                  (SELECT *   
                   FROM  (SELECT SalesPersonID, EmployeeID  
                        FROM Sales.SalesPerson, HumanResources.Employee  
                        WHERE SalesPerson.SalesPersonID = Employee.EmployeeID) As SalesPerson  
                   WHERE  SalesPerson.SalesPersonID = SalesOrderHeader.SalesPersonID  
             FOR XML AUTO, TYPE)  
    FROM Sales.SalesOrderHeader  
    WHERE SalesOrderID='43659' or SalesOrderID='43660'  
    FOR XML AUTO, TYPE ) as T(XmlCol)  
    ```  
  
## <a name="see-also"></a>Vedere anche  
 [Utilizzo di query FOR XML nidificate](../../relational-databases/xml/use-nested-for-xml-queries.md)  
  
  
