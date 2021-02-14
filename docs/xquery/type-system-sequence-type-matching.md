---
title: Corrispondenza del tipo di sequenza | Microsoft Docs
description: Informazioni su come abbinare il tipo di sequenza restituito da un'espressione XQuery con un tipo specifico.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- sequence type matching [XQuery]
- XQuery, sequence type matching
ms.assetid: 8c56fb69-ca04-4aba-b55a-64ae216c492d
author: rothja
ms.author: jroth
ms.openlocfilehash: 11c0eb693468b980db88995713dc6af99180c373
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352818"
---
# <a name="type-system---sequence-type-matching"></a>Sistema di tipi - Corrispondenza per il tipo di sequenza
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Il valore di un'espressione XQuery è sempre rappresentato da una sequenza di zero o più elementi, che possono essere valori atomici o nodi. Il tipo di sequenza fa riferimento alla capacità di stabilire una corrispondenza tra il tipo di sequenza restituito da un'espressione di query e un tipo specifico. Ad esempio:  
  
-   Se il valore dell'espressione è atomico, è possibile stabilire se è di tipo integer, decimal o string.  
  
-   Se il valore dell'espressione è un nodo XML, è possibile stabilire se è un nodo di commento, un nodo istruzione di elaborazione o un nodo di testo.  
  
-   È possibile stabilire se l'espressione restituisce un elemento XML o un nodo di attributo con un nome e un tipo specifico.  
  
 Per individuare una corrispondenza per il tipo di sequenza, è possibile utilizzare l'operatore booleano `instance of`. Per ulteriori informazioni sull' `instance of` espressione, vedere [espressioni SequenceType &#40;&#41;XQuery ](../xquery/sequencetype-expressions-xquery.md).  
  
## <a name="comparing-the-atomic-value-type-returned-by-an-expression"></a>Confronto del tipo del valore atomico restituito da un'espressione  
 Se un'espressione restituisce una sequenza di valori atomici, potrebbe essere necessario trovare il tipo del valore nella sequenza. Negli esempi seguenti viene illustrato l'utilizzo della sintassi del tipo di sequenza per valutare il tipo di valore atomico restituito da un'espressione.  
  
### <a name="example-determining-whether-a-sequence-is-empty"></a>Esempio: determinazione di una sequenza vuota  
 Il tipo di sequenza **Empty ()** può essere utilizzato in un'espressione di tipo sequenza per determinare se la sequenza restituita dall'espressione specificata è una sequenza vuota.  
  
 Nell'esempio seguente, il XML Schema consente `root` di supporta <elemento>:  
  
```  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema">  
      <element name="root" nillable="true" type="byte"/>  
</schema>'  
GO  
```  
  
 A questo punto, se un'istanza XML tipizzata specifica un valore per l' `root` elemento <>, `instance of empty()` restituisce false.  
  
```  
DECLARE @var XML(SC1)  
SET @var = '<root>1</root>'  
-- The following returns False  
SELECT @var.query('data(/root[1]) instance of  empty() ')  
GO  
```  
  
 Se l' `root` elemento <> è supporta nell'istanza, il relativo valore è una sequenza vuota e `instance of empty()` restituisce true.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root xsi:nil="true" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" />'  
SELECT @var.query('data(/root[1]) instance of  empty() ')  
GO  
```  
  
### <a name="example-determining-the-type-of-an-attribute-value"></a>Esempio: determinazione del tipo di valore di un attributo  
 A volte è possibile valutare il tipo di sequenza restituito da un'espressione prima dell'elaborazione. Ad esempio, si consideri uno XML Schema nel quale un nodo è definito come un tipo unione. Nell'esempio seguente, l'XML Schema della raccolta definisce l'attributo `a` come un tipo unione il cui valore può essere di tipo decimal o string.  
  
```  
-- Drop schema collection if it exists.  
-- DROP XML SCHEMA COLLECTION SC.  
-- GO  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema">  
  <element name="root">  
    <complexType>  
       <sequence/>  
         <attribute name="a">  
            <simpleType>  
               <union memberTypes="decimal string"/>  
            </simpleType>  
         </attribute>  
     </complexType>  
  </element>  
</schema>'  
GO  
```  
  
 Prima di elaborare un'istanza XML tipizzata, è possibile determinare il tipo del valore dell'attributo `a`. Nell'esempio seguente, il valore dell'attributo `a` è di tipo decimal e pertanto `, instance of xs:decimal` restituisce True.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root a="2.5"/>'  
SELECT @var.query('data((/root/@a)[1]) instance of xs:decimal')  
GO  
```  
  
 L'attributo `a` viene modificato in un tipo string e pertanto `instance of xs:string` restituirà True.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root a="Hello"/>'  
SELECT @var.query('data((/root/@a)[1]) instance of xs:string')  
GO  
```  
  
### <a name="example-cardinality-in-sequence-expressions"></a>Esempio: cardinalità nelle espressioni di sequenza  
 Nell'esempio seguente viene illustrato l'effetto della cardinalità in un'espressione di sequenza. Nell'XML Schema seguente viene definito un `root` elemento <> che è di tipo byte ed è nillable.  
  
```  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema">  
      <element name="root" nillable="true" type="byte"/>  
</schema>'  
GO  
```  
  
 Nella query seguente, dato che l'espressione restituisce un singleton di tipo byte, `instance of` restituisce True.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root>111</root>'  
SELECT @var.query('data(/root[1]) instance of  xs:byte ')   
GO  
```  
  
 Se si imposta il <`root`> elemento Nil, il relativo valore è una sequenza vuota. ovvero l'espressione `/root[1]` restituisce una sequenza vuota e pertanto `instance of xs:byte` restituisce False. Si noti che in questo caso la cardinalità predefinita è 1.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root xsi:nil="true" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"></root>'  
SELECT @var.query('data(/root[1]) instance of  xs:byte ')   
GO  
-- result = false  
```  
  
 Se si specifica la cardinalità aggiungendo l'indicatore di occorrenza (`?`), l'espressione della sequenza restituisce True.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root xsi:nil="true" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"></root>'  
SELECT @var.query('data(/root[1]) instance of  xs:byte? ')   
GO  
-- result = true  
```  
  
 Si noti che il test di un'espressione SequenceType include due fasi:  
  
1.  Il test determina innanzitutto se il tipo di espressione corrisponde al tipo specificato.  
  
2.  In caso affermativo, il test determina se il numero di elementi restituiti dall'espressione corrisponde all'indicatore di occorrenza specificato.  
  
 Se entrambe le condizioni sono vere, l'espressione `instance of` restituisce True.  
  
### <a name="example-querying-against-an-xml-type-column"></a>Esempio: esecuzione di una query su una colonna di tipo xml  
 Nell'esempio seguente viene specificata una query su una colonna Instructions di tipo **XML** nel [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] database. Si tratta di una colonna XML tipizzata, perché è associata a uno schema. Tramite XML Schema viene definito l'attributo `LocationID` di tipo integer. Pertanto, nell'espressione Sequence, `instance of xs:integer?` restituisce true.  
  
```  
SELECT Instructions.query('   
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";   
data(/AWMI:root[1]/AWMI:Location[1]/@LocationID) instance of xs:integer?') as Result   
FROM Production.ProductModel   
WHERE ProductModelID = 7  
```  
  
## <a name="comparing-the-node-type-returned-by-an-expression"></a>Confronto del tipo di nodo restituito da un'espressione  
 Se un'espressione restituisce una sequenza di nodi, è possibile che sia necessario individuare il tipo di nodo nella sequenza. Nell'esempio seguente viene illustrata la sintassi del tipo che è possibile utilizzare per valutare il tipo di nodo restituito da un'espressione. È possibile utilizzare i tipi di sequenza seguenti:  
  
-   **Item ()** -corrisponde a qualsiasi elemento nella sequenza.  
  
-   **node ()** : determina se la sequenza è un nodo.  
  
-   **processing-instruction ()** : determina se l'espressione restituisce un'istruzione di elaborazione.  
  
-   **Comment ()** : determina se l'espressione restituisce un commento.  
  
-   **document-node ()** : determina se l'espressione restituisce un nodo del documento.  
  
 Nell'esempio seguente vengono illustrati questi tipi di sequenza.  
  
### <a name="example-using-sequence-types"></a>Esempio: utilizzo di tipi di sequenza  
 In questo esempio vengono eseguite numerose query su una variabile XML non tipizzata. Le query illustrano l'utilizzo dei tipi di sequenza.  
  
```  
DECLARE @var XML  
SET @var = '<?xml-stylesheet href="someValue" type="text/xsl" ?>  
<root>text node  
  <!-- comment 1 -->   
  <a>Data a</a>  
  <!-- comment  2 -->  
</root>'  
```  
  
 Nella prima query, l'espressione restituisce il valore tipizzato dell'elemento <`a`>. Nella seconda query, l'espressione restituisce l'elemento <`a`>. In entrambi i casi si tratta di elementi e pertanto entrambe le query restituiscono True.  
  
```  
SELECT @var.query('data(/root[1]/a[1]) instance of item()')  
SELECT @var.query('/root[1]/a[1] instance of item()')  
```  
  
 Tutte le espressioni XQuery nelle tre query seguenti restituiscono il nodo elemento figlio dell'elemento <`root`>. L'espressione SequenceType `instance of node()` restituisce pertanto True e le altre due espressioni, `instance of text()` e `instance of document-node()`, restituiscono False.  
  
```  
SELECT @var.query('(/root/*)[1] instance of node()')  
SELECT @var.query('(/root/*)[1] instance of text()')  
SELECT @var.query('(/root/*)[1] instance of document-node()')   
```  
  
 Nella query seguente, l' `instance of document-node()` espressione restituisce true perché l'elemento padre dell' `root` elemento <> è un nodo del documento.  
  
```  
SELECT @var.query('(/root/..)[1] instance of document-node()') -- true  
```  
  
 Nella query seguente, l'espressione recupera il primo nodo dall'istanza XML. Poiché è un nodo istruzione di elaborazione, l'espressione `instance of processing-instruction()` restituisce True.  
  
```  
SELECT @var.query('(/node())[1] instance of processing-instruction()')  
```  
  
### <a name="implementation-limitations"></a>Limitazioni di implementazione  
 Sono valide le limitazioni seguenti:  
  
-   il **documento-node ()** con sintassi del tipo di contenuto non è supportato.  
  
-   la sintassi **di elaborazione-istruzione (nome)** non è supportata.  
  
## <a name="element-tests"></a>Test dell'elemento  
 Un test dell'elemento consente di stabilire una corrispondenza tra il nodo elemento restituito da un'espressione e un nodo elemento con un nome e un tipo specifico. Sono disponibili i test dell'elemento seguenti:  
  
```  
element ()  
element(ElementName)  
element(ElementName, ElementType?)   
element(*, ElementType?)  
```  
  
## <a name="attribute-tests"></a>Test dell'attributo  
 Il test dell'attributo consente di stabilire se l'attributo restituito da un'espressione è un nodo di attributo. Sono disponibili i test dell'attributo seguenti:  
  
 `attribute()`  
  
 `attribute(AttributeName)`  
  
 `attribute(AttributeName, AttributeType)`  
  
## <a name="test-examples"></a>Esempi di test  
 Negli esempi seguenti vengono illustrati scenari nei quali i test dell'elemento e dell'attributo possono risultare utili.  
  
### <a name="example-a"></a>Esempio A  
 Nell'XML Schema seguente viene definito il `CustomerType` tipo complesso in cui gli `firstName` elementi <> e <`lastName`> sono facoltativi. Per un'istanza XML specificata, potrebbe essere necessario stabilire se esiste il nome di un cliente specifico.  
  
```  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  
targetNamespace="myNS" xmlns:ns="myNS">  
  <complexType name="CustomerType">  
     <sequence>  
        <element name="firstName" type="string" minOccurs="0"   
                  nillable="true" />  
        <element name="lastName" type="string" minOccurs="0"/>  
     </sequence>  
  </complexType>  
  <element name="customer" type="ns:CustomerType"/>  
</schema>  
'  
GO  
DECLARE @var XML(SC)  
SET @var = '<x:customer xmlns:x="myNS">  
<firstName>SomeFirstName</firstName>  
<lastName>SomeLastName</lastName>  
</x:customer>'  
```  
  
 Nella query seguente viene utilizzata un' `instance of element (firstName)` espressione per determinare se il primo elemento figlio di <`customer`> è un elemento il cui nome è <`firstName`>. In caso affermativo, restituisce True.  
  
```  
SELECT @var.query('declare namespace x="myNS";   
     (/x:customer/*)[1] instance of element (firstName)')  
GO  
```  
  
 Se si rimuove l' `firstName` elemento <> dall'istanza, la query restituirà false.  
  
 È inoltre possibile utilizzare:  
  
-   La sintassi del tipo di sequenza `element(ElementName, ElementType?)`, come illustrato nella query seguente. Tale sintassi stabilisce una corrispondenza per un nodo elemento denominato `firstName` e di tipo `xs:string` che supporta o non supporta i valori Null.  
  
    ```  
    SELECT @var.query('declare namespace x="myNS";   
    (/x:customer/*)[1] instance of element (firstName, xs:string?)')  
    ```  
  
-   La sintassi del tipo di sequenza `element(*, type?)`, come illustrato nella query seguente. Tale sintassi stabilisce una corrispondenza per il nodo elemento se è di tipo `xs:string`, indipendentemente dal nome.  
  
    ```  
    SELECT @var.query('declare namespace x="myNS"; (/x:customer/*)[1] instance of element (*, xs:string?)')  
    GO  
    ```  
  
### <a name="example-b"></a>Esempio B  
 Nell'esempio seguente viene illustrato come determinare se il nodo restituito da un'espressione è un nodo elemento con un nome specifico, Usa il test **Element ()** .  
  
 Nell'esempio seguente, i due <`Customer`> elementi nell'istanza XML su cui viene eseguita la query sono di due tipi diversi, `CustomerType` e `SpecialCustomerType` . Si supponga di voler ottenere informazioni sul tipo di <`Customer` elemento> restituito dall'espressione. La raccolta di XML Schema seguente definisce i tipi `CustomerType` e `SpecialCustomerType`.  
  
```  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  
          targetNamespace="myNS"  xmlns:ns="myNS">  
  <complexType name="CustomerType">  
    <sequence>  
      <element name="firstName" type="string"/>  
      <element name="lastName" type="string"/>  
    </sequence>  
  </complexType>  
  <complexType name="SpecialCustomerType">  
     <complexContent>  
       <extension base="ns:CustomerType">  
        <sequence>  
            <element name="Age" type="int"/>  
        </sequence>  
       </extension>  
     </complexContent>  
    </complexType>  
   <element name="customer" type="ns:CustomerType"/>  
</schema>  
'  
GO  
```  
  
 Questa raccolta di XML Schema viene utilizzata per creare una variabile **XML** tipizzata. L'istanza XML assegnata a questa variabile presenta due <`customer`> elementi di due tipi diversi. Il primo elemento è di tipo `CustomerType` e il secondo elemento è di tipo `SpecialCustomerType`.  
  
```  
DECLARE @var XML(SC)  
SET @var = '  
<x:customer xmlns:x="myNS">  
   <firstName>FirstName1</firstName>  
   <lastName>LastName1</lastName>  
</x:customer>  
<x:customer xsi:type="x:SpecialCustomerType" xmlns:x="myNS" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
   <firstName> FirstName2</firstName>  
   <lastName> LastName2</lastName>  
   <Age>21</Age>  
</x:customer>'  
```  
  
 Nella query seguente, l'espressione `instance of element (*, x:SpecialCustomerType ?)` restituisce False, perché restituisce il primo elemento Customer che non è di tipo `SpecialCustomerType`.  
  
```  
SELECT @var.query('declare namespace x="myNS";   
    (/x:customer)[1] instance of element (*, x:SpecialCustomerType ?)')  
```  
  
 Se si modifica l'espressione della query precedente e si recupera il secondo <`customer` elemento> ( `/x:customer)[2]` ), il `instance of` restituisce true.  
  
### <a name="example-c"></a>Esempio C  
 In questo esempio viene utilizzato il test dell'attributo. L'XML Schema seguente definisce il tipo complesso CustomerType con gli attributi CustomerID e Age. L'attributo Age è facoltativo. Per un'istanza XML specifica, può essere utile determinare se l'attributo Age è presente nell' `customer` elemento <>.  
  
```  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  
       targetNamespace="myNS" xmlns:ns="myNS">  
<complexType name="CustomerType">  
  <sequence>  
     <element name="firstName" type="string" minOccurs="0"   
               nillable="true" />  
     <element name="lastName" type="string" minOccurs="0"/>  
  </sequence>  
  <attribute name="CustomerID" type="integer" use="required" />  
  <attribute name="Age" type="integer" use="optional" />  
 </complexType>  
 <element name="customer" type="ns:CustomerType"/>  
</schema>  
'  
GO  
```  
  
 La query seguente restituisce True perché nell'istanza XML su cui viene eseguita la query esiste un nodo di attributo al quale è assegnato il nome `Age`. In questa espressione viene utilizzato il test dell'attributo `attribute(Age)`. Gli attributi non sono ordinati, pertanto la query utilizza l'espressione FLWOR per recuperare tutti gli attributi per poi testare ognuno di essi tramite l'espressione `instance of`. Nell'esempio viene innanzitutto creata una raccolta di XML Schema per creare una variabile **XML** tipizzata.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<x:customer xmlns:x="myNS" CustomerID="1" Age="22" >  
<firstName>SomeFName</firstName>  
<lastName>SomeLName</lastName>  
</x:customer>'  
SELECT @var.query('declare namespace x="myNS";   
FOR $i in /x:customer/@*  
RETURN  
    IF ($i instance of attribute (Age)) THEN  
        "true"  
        ELSE  
        ()')     
GO  
  
```  
  
 Se si rimuove dall'istanza l'attributo facoltativo `Age`, la query precedente restituirà False.  
  
 Nel test dell'attributo è possibile specificare il nome e il tipo dell'attributo (`attribute(name,type)`).  
  
```  
SELECT @var.query('declare namespace x="myNS";   
FOR $i in /x:customer/@*  
RETURN  
    IF ($i instance of attribute (Age, xs:integer)) THEN  
        "true"  
        ELSE  
        ()')  
```  
  
 In alternativa, è possibile specificare la `attribute(*, type)` sintassi del tipo di sequenza. Se il tipo di attributo corrisponde al tipo specificato, in questo modo viene stabilita una corrispondenza per il nodo di attributo, indipendentemente dal nome.  
  
### <a name="implementation-limitations"></a>Limitazioni di implementazione  
 Sono valide le limitazioni seguenti:  
  
-   Nel test dell'elemento il nome del tipo deve essere seguito dall'indicatore di occorrenza (**?**).  
  
-   l' **elemento (ElementName, typeName)** non è supportato.  
  
-   **elemento ( \* , typeName)** non supportato.  
  
-   **Schema-Element ()** non è supportato.  
  
-   l' **attributo schema (attributeName)** non è supportato.  
  
-   L'esecuzione di query in modo esplicito per **xsi: Type** o **xsi: nil** non è supportata.  
  
## <a name="see-also"></a>Vedere anche  
 [Sistema di tipi &#40;XQuery&#41;](../xquery/type-system-xquery.md)  
  
  
