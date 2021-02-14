---
title: Espressioni SequenceType (XQuery) | Microsoft Docs
description: Informazioni sull'istanza XQuery SequenceType Expressions di e cast as.
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
- SequenceType expressions
- instance of operator [XQuery]
- expressions [XQuery], SequenceType
- cast as operator
ms.assetid: ad3573da-d820-4d1c-81c4-a83c4640ce22
author: rothja
ms.author: jroth
ms.openlocfilehash: 0627d892733f84cb4a8d1b5cf80ad65d9c09f824
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335847"
---
# <a name="sequencetype-expressions-xquery"></a>Espressioni SequenceType (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  In XQuery un valore è sempre una sequenza. Il tipo del valore viene definito tipo di sequenza. Il tipo di sequenza può essere utilizzato in un' **istanza dell'** espressione XQuery. La sintassi SequenceType descritta nella specifica XQuery viene utilizzata quando è necessario fare riferimento a un tipo in un'espressione XQuery.  
  
 Il nome del tipo atomico può essere utilizzato anche nel **cast come** espressione XQuery. In [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , l' **istanza di** e il **cast come** espressioni XQuery in tipi sono parzialmente supportati.  
  
## <a name="instance-of-operator"></a>Operatore instance of  
 L' **istanza dell'** operatore può essere utilizzata per determinare il tipo dinamico, o in fase di esecuzione, del valore dell'espressione specificata. Ad esempio:  
  
```  
  
Expression instance of SequenceType[Occurrence indicator]  
```  
  
 Si noti che l'operatore,, `instance of` `Occurrence indicator` specifica la cardinalità, il numero di elementi nella sequenza risultante. Se non viene specificato, si presuppone una cardinalità 1. In [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] è supportato solo l'indicatore di occorrenza punto interrogativo (**?)** . Il quantificatore **?** l'indicatore di occorrenza indica che `Expression` può restituire zero o un elemento. Se il **?** l'indicatore di occorrenza è specificato `instance of` . restituisce true se il `Expression` tipo corrisponde a quello specificato `SequenceType` , indipendentemente dal fatto che `Expression` restituisca una sequenza Singleton o vuota.  
  
 Se il **?** l'indicatore di occorrenza non è specificato `sequence of` . restituisce true solo quando il `Expression` tipo corrisponde all' `Type` oggetto specificato e `Expression` restituisce un singleton.  
  
 **Nota** Gli **+** indicatori di occorrenza segno più () e asterisco (**&#42;**) non sono supportati in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
 Negli esempi seguenti viene illustrato l'utilizzo dell'**istanza dell'** operatore XQuery.  
  
### <a name="example-a"></a>Esempio A  
 Nell'esempio seguente viene creata una variabile di tipo **XML** e viene specificata una query su di essa. L'espressione di query specifica un operatore `instance of` per determinare se il tipo dinamico del valore restituito dal primo operando corrisponde al tipo specificato nel secondo operando.  
  
 La query seguente restituisce true, perché il valore 125 è un'istanza del tipo specificato, **xs: integer**:  
  
```  
declare @x xml  
set @x=''  
select @x.query('125 instance of xs:integer')  
go  
```  
  
 La query seguente restituisce True, poiché il valore restituito dall'espressione /a[1] nel primo operando è un elemento:  
  
```  
declare @x xml  
set @x='<a>1</a>'  
select @x.query('/a[1] instance of element()')  
go  
```  
  
 Allo stesso modo, `instance of` restituisce True nella query seguente poiché il tipo di valore dell'espressione nella prima espressione è un attributo:  
  
```  
declare @x xml  
set @x='<a attr1="x">1</a>'  
select @x.query('/a[1]/@attr1 instance of attribute()')  
go  
```  
  
 Nell'esempio seguente, l'espressione `data(/a[1]` restituisce un valore atomico tipizzato come xdt:untypedAtomic. Tramite l'operatore `instance of` viene pertanto restituito True.  
  
```  
declare @x xml  
set @x='<a>1</a>'  
select @x.query('data(/a[1]) instance of xdt:untypedAtomic')  
go  
```  
  
 Nella query seguente, l'espressione `data(/a[1]/@attrA` restituisce un valore atomico non tipizzato. Tramite l'operatore `instance of` viene pertanto restituito True.  
  
```  
declare @x xml  
set @x='<a attrA="X">1</a>'  
select @x.query('data(/a[1]/@attrA) instance of xdt:untypedAtomic')  
go  
```  
  
### <a name="example-b"></a>Esempio B  
 In questo esempio viene eseguita una query su una colonna XML tipizzata del database di esempio AdventureWorks. La raccolta di XML Schema associata alla colonna sulla quale viene eseguita la query fornisce le informazioni di tipizzazione.  
  
 Nell'espressione, **Data ()** restituisce il valore tipizzato dell'attributo ProductModelID il cui tipo è xs: String in base allo schema associato alla colonna. Tramite l'operatore `instance of` viene pertanto restituito True.  
  
```  
SELECT CatalogDescription.query('  
   declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
   data(/PD:ProductDescription[1]/@ProductModelID) instance of xs:string  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID = 19  
```  
  
 Per altre informazioni, vedere [Confrontare dati XML tipizzati con dati XML non tipizzati](../relational-databases/xml/compare-typed-xml-to-untyped-xml.md).  
  
 L'espressione usetheBoolean seguente esegue una query `instance of` per determinare se l'attributo LocationID è di tipo xs: integer:  
  
```  
SELECT Instructions.query('  
   declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
   /AWMI:root[1]/AWMI:Location[1]/@LocationID instance of attribute(LocationID,xs:integer)  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 La query seguente viene specificata sulla colonna XML tipizzata CatalogDescription. Le informazioni di tipizzazione sono fornite dalla raccolta di XML Schema associata alla colonna.  
  
 Nella query viene utilizzato il test `element(ElementName, ElementType?)` nell'espressione `instance of` per verificare che tramite `/PD:ProductDescription[1]` venga restituito un nodo di elemento con un nome e un tipo specifici.  
  
```  
SELECT CatalogDescription.query('  
     declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     /PD:ProductDescription[1] instance of element(PD:ProductDescription, PD:ProductDescription?)  
    ') as Result  
FROM  Production.ProductModel  
where ProductModelID=19  
```  
  
 La query restituisce True.  
  
### <a name="example-c"></a>Esempio C  
 Quando si utilizzano i tipi unione, l'espressione `instance of` in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] presenta una limitazione. In particolare, quando un elemento o un attributo è di un tipo unione, è possibile che `instance of` non riesca a determinare il tipo esatto. Di conseguenza, una query restituirà False, a meno che i tipi atomici utilizzati in SequenceType siano l'elemento padre di livello più alto del tipo effettivo dell'espressione nella gerarchia simpleType, ovvero che i tipi atomici specificati in SequenceType siano figli diretti di anySimpleType. Per informazioni sulla gerarchia dei tipi, vedere [regole di cast del tipo in XQuery](../xquery/type-casting-rules-in-xquery.md).  
  
 La prossima query esegue le operazioni seguenti:  
  
-   Crea a una raccolta di XML Schema contenente la definizione di un tipo unione, ad esempio un tipo integer o stringa.  
  
-   Dichiarare una variabile **XML** tipizzata utilizzando la raccolta di XML Schema.  
  
-   Assegna un'istanza XML di esempio alla variabile.  
  
-   Esegue una query sulla variabile per illustrare il funzionamento di `instance of` quando è applicato a un tipo unione.  
  
 Query:  
  
```  
CREATE XML SCHEMA COLLECTION MyTestSchema AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema" targetNamespace="http://ns" xmlns:ns="http://ns">  
<simpleType name="MyUnionType">  
<union memberTypes="integer string"/>  
</simpleType>  
<element name="TestElement" type="ns:MyUnionType"/>  
</schema>'  
Go  
```  
  
 La query seguente restituisce False, perché il SequenceType specificato nell'espressione `instance of` non è l'elemento padre di livello più alto del tipo effettivo dell'espressione specificata. Ovvero, il valore di <`TestElement`> è un tipo Integer. L'elemento padre di livello più alto è xs:decimal, ma non è specificato come secondo operando dell'operatore `instance of`.  
  
```  
SET QUOTED_IDENTIFIER ON  
DECLARE @var XML(MyTestSchema)  
  
SET @var = '<TestElement xmlns="http://ns">123</TestElement>'  
  
SELECT @var.query('declare namespace ns="http://ns"   
   data(/ns:TestElement[1]) instance of xs:integer')  
go  
```  
  
 Poiché l'elemento padre di livello più alto di xs:integer è xs:decimal, se viene modificata specificando xs:decimal come SequenceType la query restituirà True.  
  
```  
SET QUOTED_IDENTIFIER ON  
DECLARE @var XML(MyTestSchema)  
SET @var = '<TestElement xmlns="http://ns">123</TestElement>'  
SELECT @var.query('declare namespace ns="http://ns"     
   data(/ns:TestElement[1]) instance of xs:decimal')  
go  
```  
  
### <a name="example-d"></a>Esempio D  
 In questo esempio si crea prima una raccolta di XML Schema e la si usa per digitare una variabile **XML** . Viene quindi eseguita una query sulla variabile **XML** tipizzata per illustrare la `instance of` funzionalità.  
  
 La raccolta di XML Schema seguente definisce un tipo semplice, myType e un elemento, <`root`>, di tipo MyType:  
  
```  
drop xml schema collection SC  
go  
CREATE XML SCHEMA COLLECTION SC AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema" targetNamespace="myNS" xmlns:ns="myNS"  
xmlns:s="https://schemas.microsoft.com/sqlserver/2004/sqltypes">  
      <import namespace="https://schemas.microsoft.com/sqlserver/2004/sqltypes"/>  
      <simpleType name="myType">  
           <restriction base="s:varchar">  
                  <maxLength value="20"/>  
            </restriction>  
      </simpleType>  
      <element name="root" type="ns:myType"/>  
</schema>'  
Go  
```  
  
 A questo punto, creare una variabile **XML** tipizzata ed eseguire una query:  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root xmlns="myNS">My data</root>'  
SELECT @var.query('declare namespace sqltypes = "https://schemas.microsoft.com/sqlserver/2004/sqltypes";  
declare namespace ns="myNS";   
   data(/ns:root[1]) instance of ns:myType')  
go  
```  
  
 Poiché il tipo myType deriva per restrizione da un tipo varchar definito nello schema sqltypes, anche `instance of` restituirà True.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root xmlns="myNS">My data</root>'  
SELECT @var.query('declare namespace sqltypes = "https://schemas.microsoft.com/sqlserver/2004/sqltypes";  
declare namespace ns="myNS";   
data(/ns:root[1]) instance of sqltypes:varchar?')  
go  
```  
  
### <a name="example-e"></a>Esempio E  
 Nell'esempio seguente, l'espressione recupera uno dei valori dell'attributo IDREFS e utilizza `instance of` per determinare se il valore sia di tipo IDREF. Nell'esempio vengono eseguite le operazioni seguenti:  
  
-   Crea una raccolta di XML Schema in cui l' `Customer` elemento di> <dispone di un attributo di tipo di tipo IDREFS **ordery** e l' `Order` elemento di> <ha un attributo **OrderID** di tipo ID.  
  
-   Crea una variabile **XML** tipizzata e vi assegna un'istanza XML di esempio.  
  
-   Specifica una query sulla variabile. L'espressione di query recupera il valore dell'ID del primo ordine dall'attributo di tipo Ordery ORDERLIST del primo <`Customer`>. Il valore recuperato è di tipo IDREF. L'operatore `instance of` pertanto restituisce True.  
  
```  
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
  
select @x.query(' declare namespace CustOrders="Customers";   
 data(CustOrders:Customers/Customer[1]/@OrderList)[1] instance of xs:IDREF ? ') as XML_result  
```  
  
### <a name="implementation-limitations"></a>Limitazioni di implementazione  
 Limitazioni:  
  
-   I tipi di sequenza **elemento schema ()** e **attributo schema ()** non sono supportati per il confronto con l' `instance of` operatore.  
  
-   Le sequenze complete, ad esempio `(1,2) instance of xs:integer*`, non sono supportate.  
  
-   Quando si utilizza un form del tipo di sequenza **Element ()** che specifica un nome di tipo, ad esempio `element(ElementName, TypeName)` , il tipo deve essere qualificato con un punto interrogativo (?). Ad esempio, `element(Title, xs:string?)` indica che l'elemento potrebbe essere null. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] non supporta il rilevamento della fase di esecuzione della proprietà **xsi: nil** tramite `instance of` .  
  
-   Se il valore in `Expression` viene da un elemento o un attributo tipizzato come unione, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] può identificare solo il tipo primitivo dal quale deriva il tipo di valore, non il tipo derivato. Se, ad esempio, <`e1`> viene definito con un tipo statico (XS: integer | xs: String), il codice seguente restituirà false.  
  
    ```  
    data(<e1>123</e1>) instance of xs:integer  
    ```  
  
     `data(<e1>123</e1>) instance of xs:decimal` tuttavia restituirà True.  
  
-   Per i tipi di sequenza **processing-instruction ()** e **document-node ()** , sono consentiti solo moduli senza argomenti. Ad esempio, `processing-instruction()` è consentito, ma `processing-instruction('abc')` non è consentito.  
  
## <a name="cast-as-operator"></a>Operatore cast as  
 L'espressione **cast as** può essere utilizzata per convertire un valore in un tipo di dati specifico. Ad esempio:  
  
```  
  
Expression cast as  AtomicType?  
```  
  
 In [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], dopo `AtomicType` è necessario il punto interrogativo (?). Ad esempio, come illustrato nella query seguente, `"2" cast as xs:integer?` converte il valore stringa in un numero intero:  
  
```  
declare @x xml  
set @x=''  
select @x.query('"2" cast as xs:integer?')  
```  
  
 Nella query seguente, **Data ()** restituisce il valore tipizzato dell'attributo ProductModelID, un tipo stringa. L' `cast as` operatore converte il valore in xs: integer.  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS PD)  
SELECT CatalogDescription.query('  
   data(/PD:ProductDescription[1]/@ProductModelID) cast as xs:integer?  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID = 19  
```  
  
 L'utilizzo esplicito dei **dati ()** non è obbligatorio in questa query. L'espressione `cast as` esegue l'atomizzazione implicita sull'espressione di input.  
  
### <a name="constructor-functions"></a>Funzioni costruttore  
 È possibile utilizzare le funzioni costruttore del tipo atomico. Ad esempio, invece di usare l' `cast as` operatore, `"2" cast as xs:integer?` è possibile usare la funzione costruttore **xs: Integer ()** , come nell'esempio seguente:  
  
```  
declare @x xml  
set @x=''  
select @x.query('xs:integer("2")')  
```  
  
 L'esempio seguente restituisce un valore xs:date uguale a 2000-01-01Z.  
  
```  
declare @x xml  
set @x=''  
select @x.query('xs:date("2000-01-01Z")')  
```  
  
 È inoltre possibile utilizzare i costruttori per i tipi atomici definiti dall'utente. Se, ad esempio, la raccolta XML Schema associata al tipo di dati XML definisce un tipo semplice, è possibile utilizzare un costruttore **MyType ()** per restituire un valore di tale tipo.  
  
#### <a name="implementation-limitations"></a>Limitazioni di implementazione  
  
-   Le espressioni XQuery **typeswitch**, **castable** e **Treat** non sono supportate.  
  
-   **cast as** richiede un punto interrogativo (?) dopo il tipo atomico.  
  
-   **xs: QName** non è supportato come tipo per il cast. In alternativa **, usare expanded-QName** .  
  
-   **xs: date**, **xs: Time** e **xs: DateTime** richiedono un fuso orario, indicato da Z.  
  
     La query seguente ha esito negativo, perché il fuso orario non è specificato.  
  
    ```  
    DECLARE @var XML  
    SET @var = ''  
    SELECT @var.query(' <a>{xs:date("2002-05-25")}</a>')  
    go  
    ```  
  
     Aggiungendo l'indicatore di fuso orario Z al valore, la query riesce.  
  
    ```  
    DECLARE @var XML  
    SET @var = ''  
    SELECT @var.query(' <a>{xs:date("2002-05-25Z")}</a>')  
    go  
    ```  
  
     Risultato:  
  
    ```  
    <a>2002-05-25Z</a>  
    ```  
  
## <a name="see-also"></a>Vedere anche  
 [Espressioni XQuery](../xquery/xquery-expressions.md)   
 [Sistema di tipi &#40;XQuery&#41;](../xquery/type-system-xquery.md)  
  
  
