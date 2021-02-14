---
title: expanded-QName (XQuery) | Microsoft Docs
description: Informazioni su come usare la funzione expanded-QName () per restituire l'URI dello spazio dei nomi e la parte del nome locale di un QName.
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
- expanded-QName function
- fn:expanded-QName function
ms.assetid: b8377042-95cc-467b-9ada-fe43cebf4bc3
author: rothja
ms.author: jroth
ms.openlocfilehash: 704cac94b48fe172eaa6a972cdd6c49bad1601fe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344929"
---
# <a name="functions-related-to-qnames---expanded-qname"></a>Funzioni correlate a elementi QName - expanded-QName
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Restituisce un valore del tipo xs: QName con l'URI dello spazio dei nomi specificato nell' *$paramURI* e il nome locale specificato nell' *$paramLocal*. Se *$paramURI* è la stringa vuota o la sequenza vuota, non rappresenta alcuno spazio dei nomi.  
  
## <a name="syntax"></a>Sintassi  
  
```  
fn:expanded-QName($paramURI as xs:string?, $paramLocal as xs:string?) as xs:QName?  
```  
  
## <a name="arguments"></a>Argomenti  
 *$paramURI*  
 URI dello spazio dei nomi dell'elemento QName.  
  
 *$paramLocal*  
 Parte dell'elemento QName che rappresenta il nome locale.  
  
## <a name="remarks"></a>Commenti  
 Per la funzione **expanded-QName ()** si applica quanto segue:  
  
-   Se il valore *$paramLocal* specificato non è nel formato lessicale corretto per il tipo xs: NCName, viene restituita la sequenza vuota e viene rappresentato un errore dinamico.  
  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] non supporta la conversione dal tipo xs:QName a un tipo diverso. Per questo motivo, non è possibile usare la funzione **expanded-QName ()** nella costruzione XML. Ad esempio, quando si costruisce un nodo come `<e> expanded-QName(...) </e>`, il valore deve essere non tipizzato. A questo scopo è necessario convertire il valore del tipo xs:QName restituito da `expanded-QName()` in xdt:untypedAtomic. La funzionalità non è tuttavia supportata. Una soluzione a questo problema è illustrata in un esempio di seguito in questo argomento.  
  
-   È possibile modificare o confrontare i valori di tipo QName esistenti. Confronta, ad esempio, `/root[1]/e[1] eq expanded-QName("http://nsURI" "myNS")` il valore dell'elemento <`e`> con il QName restituito dalla funzione **expanded-QName ()** .  
  
## <a name="examples"></a>Esempio  
 In questo argomento vengono forniti esempi di XQuery sulle istanze XML archiviate in diverse colonne di tipo **XML** nel [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] database.  
  
### <a name="a-replacing-a-qname-type-node-value"></a>R. Sostituzione di un valore di nodo di tipo QName  
 Nell'esempio seguente viene illustrata la procedura per modificare il valore di un nodo elemento di tipo QName. Nell'esempio vengono eseguite le operazioni seguenti:  
  
-   Crea una raccolta di XML Schema che definisce un elemento di tipo QName.  
  
-   Crea una tabella con una colonna di tipo **XML** tramite la raccolta di XML Schema.  
  
-   Salva un'istanza XML nella tabella.  
  
-   Usa il metodo **Modify ()** del tipo di dati XML per modificare il valore dell'elemento di tipo QName nell'istanza di. La funzione **expanded-QName ()** viene usata per generare il nuovo valore di tipo QName.  
  
```  
-- If XML schema collection (if exists)  
-- drop xml schema collection SC  
-- go  
-- Create XML schema collection  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  
    xmlns:xs="http://www.w3.org/2001/XMLSchema"   
    targetNamespace="QNameXSD"   
      xmlns:xqo="QNameXSD" elementFormDefault="qualified">  
      <element name="Root" type="xqo:rootType" />  
      <complexType name="rootType">  
            <sequence minOccurs="1" maxOccurs="1">  
                        <element name="ElemQN" type="xs:QName" />  
            </sequence>  
      </complexType>  
</schema>'  
go  
-- Create table.  
CREATE TABLE T( XmlCol xml(SC) )  
-- Insert sample XML instnace  
INSERT INTO T VALUES ('  
<Root xmlns="QNameXSD" xmlns:ns="https://myURI">  
      <ElemQN>ns:someName</ElemQN>  
</Root>')  
go  
-- Verify the insertion  
SELECT * from T  
go  
-- Result  
<Root xmlns="QNameXSD" xmlns:ns="https://myURI">  
  <ElemQN>ns:someName</ElemQN>  
</Root>   
```  
  
 Nella query seguente, il valore dell' `ElemQN` elemento <> viene sostituito utilizzando il metodo **Modify ()** del tipo di dati XML e il valore Replace di XML DML, come illustrato.  
  
```  
-- the value.  
UPDATE T   
SET XmlCol.modify('  
  declare default element namespace "QNameXSD";   
  replace value of /Root[1]/ElemQN   
  with expanded-QName("https://myURI", "myLocalName") ')  
go  
-- Verify the result  
SELECT * from T  
go  
```  
  
 Di seguito è riportato il risultato. Si noti che l'elemento <`ElemQN`> di tipo QName dispone ora di un nuovo valore:  
  
```  
<Root xmlns="QNameXSD" xmlns:ns="urn">  
  <ElemQN xmlns:p1="https://myURI">p1:myLocalName</ElemQN>  
</Root>  
```  
  
 Le istruzioni seguenti rimuovono gli oggetti utilizzati nell'esempio.  
  
```  
-- Cleanup  
DROP TABLE T  
go  
drop xml schema collection SC  
go  
```  
  
### <a name="b-dealing-with-the-limitations-when-using-the-expanded-qname-function"></a>B. Gestione delle limitazioni di utilizzo della funzione expanded-QName()  
 Impossibile utilizzare la funzione **expanded-QName** nella costruzione XML. Questa condizione è illustrata nell'esempio seguente. Per ovviare a questa limitazione, nell'esempio viene inserito per prima cosa un nodo, che verrà poi modificato.  
  
```  
-- if exists drop the table T  
--drop table T  
-- go  
-- Create XML schema collection  
-- DROP XML SCHEMA COLLECTION SC  
-- go  
CREATE XML SCHEMA COLLECTION SC AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema">  
      <element name="root" type="QName" nillable="true"/>  
</schema>'  
go  
 -- Create table T with a typed xml column (using the XML schema collection)  
CREATE TABLE T (xmlCol XML(SC))  
go  
-- Insert an XML instance.  
insert into T values ('<root xmlns:a="https://someURI">a:b</root>')  
 go  
-- Verify  
SELECT *   
FROM T  
```  
  
 Il tentativo seguente aggiunge un altro `root` elemento <> ma ha esito negativo perché la funzione expanded-QName () non è supportata nella costruzione XML.  
  
```  
update T SET xmlCol.modify('  
insert <root>{expanded-QName("http://ns","someLocalName")}</root> as last into / ')  
go  
```  
  
 Per risolvere il problema, inserire prima un'istanza con un valore per l'elemento <`root`> e quindi modificarlo. In questo esempio viene usato un valore iniziale nil quando `root` viene inserito l'elemento <>. La raccolta XML Schema in questo esempio consente un valore nil per l' `root` elemento> <.  
  
```  
update T SET xmlCol.modify('  
insert <root xsi:nil="true"/> as last into / ')  
go  
-- now replace the nil value with another QName.  
update T SET xmlCol.modify('  
replace value of /root[last()] with expanded-QName("http://ns","someLocalName") ')  
go  
 -- verify   
SELECT * FROM T  
go  
-- result  
<root>b</root>  
```  
  
 `<root xmlns:a="https://someURI">a:b</root>`  
  
 `<root xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p1="http://ns">p1:someLocalName</root>`  
  
 È possibile confrontare il valore dell'elemento QName, come illustrato nella query seguente. La query restituisce solo gli `root` elementi> <i cui valori corrispondono al valore di tipo QName restituito dalla funzione **expanded-QName ()** .  
  
```  
SELECT xmlCol.query('  
    for $i in /root  
    return  
       if ($i eq expanded-QName("http://ns","someLocalName") ) then  
          $i  
       else  
          ()')  
FROM T  
```  
  
### <a name="implementation-limitations"></a>Limitazioni di implementazione  
 Esiste una limitazione: la funzione **expanded-QName ()** accetta la sequenza vuota come secondo argomento e restituirà Empty anziché generare un errore di run-time quando il secondo argomento non è corretto.  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni correlate a QName &#40;XQuery&#41;]()  
  
