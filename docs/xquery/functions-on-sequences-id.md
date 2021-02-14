---
title: Funzione ID (XQuery) | Microsoft Docs
description: "Viene illustrato come utilizzare la funzione ID XQuery per restituire una sequenza di elementi nell'istanza XML, in base all'ordine dei documenti, con i valori xs: IDREF forniti."
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:id function
- id function
ms.assetid: de99fc60-d0ad-4117-a17d-02bdde6512b4
author: rothja
ms.author: jroth
ms.openlocfilehash: c6ae0aedd4971d6345811e6ee0bfff5afa395c15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344981"
---
# <a name="functions-on-sequences---id"></a>Funzioni su sequenze - id
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Restituisce la sequenza di nodi elemento con valori xs: ID che corrispondono ai valori di uno o più valori xs: IDREF specificati in *$arg*.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
fn:id($arg as xs:IDREF*) as element()*  
```  
  
## <a name="arguments"></a>Argomenti  
 *$arg*  
 Uno o più valori xs:IDREF.  
  
## <a name="remarks"></a>Commenti  
 Il risultato della funzione è una sequenza di elementi dell'istanza XML, nell'ordine in cui ricorrono nel documento, con un valore xs:ID uguale a uno o più valori xs:IDREF nell'elenco di valori xs:IDREF candidati.  
  
 Se il valore xs:IDREF non corrisponde ad alcun elemento, la funzione restituisce la sequenza vuota.  
  
## <a name="examples"></a>Esempio  
 In questo argomento vengono forniti esempi di XQuery sulle istanze XML archiviate in diverse colonne di tipo **XML** nel [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] database.  
  
### <a name="a-retrieving-elements-based-on-the-idref-attribute-value"></a>R. Recupero di elementi basati sul valore dell'attributo IDREF  
 Nell'esempio seguente viene usato FN: ID per recuperare gli `employee` elementi> <, in base all'attributo IDREF Manager. In questo esempio, l'attributo manager è un attributo di tipo IDREF, mentre l'attributo eid è un attributo di tipo ID.  
  
 Per un valore di attributo manager specifico, la funzione **ID ()** trova l' `employee` elemento> <il cui valore dell'attributo del tipo di ID corrisponde al valore IDREF di input. In altre parole, per un dipendente specifico, la funzione **ID ()** restituisce Employee Manager.  
  
 Tale caso è illustrato nell'esempio seguente:  
  
-   Viene creata una raccolta di XML Schema.  
  
-   Una variabile **XML** tipizzata viene creata utilizzando la raccolta XML Schema.  
  
-   La query recupera l'elemento con un valore di attributo ID a cui fa riferimento l'attributo IDREF **Manager** dell' `employee` elemento <>.  
  
```  
-- If exists, drop the XML schema collection (SC).  
-- drop xml schema collection SC  
-- go  
  
create xml schema collection SC as  
'<schema xmlns="http://www.w3.org/2001/XMLSchema" xmlns:e="emp" targetNamespace="emp">  
            <element name="employees" type="e:EmployeesType"/>  
            <complexType name="EmployeesType">  
                 <sequence>  
                      <element name="employee" type="e:EmployeeType" minOccurs="0" maxOccurs="unbounded" />  
                 </sequence>  
            </complexType>    
  
            <complexType name="EmployeeType">  
                        <attribute name="eid" type="ID" />  
                        <attribute name="name" type="string" />  
                        <attribute name="manager" type="IDREF" />  
            </complexType>         
</schema>'  
go  
```  
  
```  
declare @x xml(SC)  
set @x='<e:employees xmlns:e="emp">  
<employee eid="e1" name="Joe" manager="e10" />  
<employee eid="e2" name="Bob" manager="e10" />  
<employee eid="e10" name="Dave" manager="e10" />  
</e:employees>'  
  
select @x.value(' declare namespace e="emp";   
 (fn:id(e:employees/employee[@name="Joe"]/@manager)/@name)[1]', 'varchar(50)')   
Go  
```  
  
 La query restituisce il valore "Dave", che indica che Dave è il responsabile di Joe.  
  
### <a name="b-retrieving-elements-based-on-the-orderlist-idrefs-attribute-value"></a>B. Recupero di elementi basati sul valore dell'attributo IDREFS OrderList  
 Nell'esempio seguente l'attributo Ordername dell' `Customer` elemento <> è un attributo di tipo IDREFS. che elenca gli ID degli ordini del cliente specifico. Per ogni ID dell'ordine, è presente un <`Order` elemento figlio> sotto il <`Customer`> specificando il valore dell'ordine.  
  
 L'espressione di query, `data(CustOrders:Customers/Customer[1]/@OrderList)[1]`, recupera il primo valore dall'elenco IDRES relativo al primo cliente. Questo valore viene quindi passato alla funzione **ID ()** . La funzione trova quindi il <`Order` elemento> il cui valore dell'attributo OrderID corrisponde all'input della funzione **ID ()** .  
  
```  
drop xml schema collection SC  
go  
create xml schema collection SC as  
'<schema xmlns="http://www.w3.org/2001/XMLSchema" xmlns:Customers="Customers" targetNamespace="Customers">  
            <element name="Customers" type="Customers:CustomersType"/>  
            <complexType name="CustomersType">  
                        <sequence>  
                            <element name="Customer" type="Customers:CustomerType" minOccurs="0" maxOccurs="unbounded" />  
                        </sequence>  
            </complexType>  
             <complexType name="OrderType">  
                <sequence minOccurs="0" maxOccurs="unbounded">  
                            <choice>  
                                <element name="OrderValue" type="integer" minOccurs="0" maxOccurs="unbounded"/>  
                            </choice>  
                </sequence>                                             
                <attribute name="OrderID" type="ID" />  
            </complexType>  
  
            <complexType name="CustomerType">  
                <sequence minOccurs="0" maxOccurs="unbounded">  
                            <choice>  
                                <element name="spouse" type="string" minOccurs="0" maxOccurs="unbounded"/>  
                                <element name="Order" type="Customers:OrderType" minOccurs="0" maxOccurs="unbounded"/>  
                            </choice>  
                </sequence>                                             
                <attribute name="CustomerID" type="string" />  
                <attribute name="OrderList" type="IDREFS" />  
            </complexType>  
 </schema>'  
go  
declare @x xml(SC)  
set @x='<CustOrders:Customers xmlns:CustOrders="Customers">  
                <Customer CustomerID="C1" OrderList="OrderA OrderB"  >  
                              <spouse>Jenny</spouse>  
                                <Order OrderID="OrderA"><OrderValue>11</OrderValue></Order>  
                                <Order OrderID="OrderB"><OrderValue>22</OrderValue></Order>  
  
                </Customer>  
                <Customer CustomerID="C2" OrderList="OrderC OrderD" >  
                                <spouse>John</spouse>  
                                <Order OrderID="OrderC"><OrderValue>33</OrderValue></Order>  
                                <Order OrderID="OrderD"><OrderValue>44</OrderValue></Order>  
  
                        </Customer>  
                <Customer CustomerID="C3"  OrderList="OrderE OrderF" >  
                                <spouse>Jane</spouse>  
                                <Order OrderID="OrderE"><OrderValue>55</OrderValue></Order>  
                                <Order OrderID="OrderF"><OrderValue>55</OrderValue></Order>  
                </Customer>  
                <Customer CustomerID="C4"  OrderList="OrderG"  >  
                                <spouse>Tim</spouse>  
                                <Order OrderID="OrderG"><OrderValue>66</OrderValue></Order>  
                        </Customer>  
                <Customer CustomerID="C5"  >  
                </Customer>  
                <Customer CustomerID="C6" >  
                </Customer>  
                <Customer CustomerID="C7"  >  
                </Customer>  
</CustOrders:Customers>'  
select @x.query('declare namespace CustOrders="Customers";  
  id(data(CustOrders:Customers/Customer[1]/@OrderList)[1])')  
  
-- result  
<Order OrderID="OrderA">  
  <OrderValue>11</OrderValue>  
</Order>  
```  
  
### <a name="implementation-limitations"></a>Limitazioni di implementazione  
 Limitazioni:  
  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] non supporta la versione a due argomenti di **ID ()**.  
  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] richiede che il tipo di argomento di **ID ()** sia un sottotipo di XS: IDREF *.  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni per le sequenze](./xquery-functions-against-the-xml-data-type.md)  
  
