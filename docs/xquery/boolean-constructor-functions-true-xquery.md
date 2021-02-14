---
title: Funzione true (XQuery) | Microsoft Docs
description: Informazioni sulla funzione XQuery true () che restituisce il valore booleano true.
ms.custom: ''
ms.date: 08/10/2016
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:true function
- true function
ms.assetid: 318e370d-0444-4812-afe4-307df7ef9f3b
author: rothja
ms.author: jroth
ms.openlocfilehash: 779e3fa093d851e99e2869053f155fb70570f67c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340507"
---
# <a name="boolean-constructor-functions---true-xquery"></a>Funzioni costruttore booleane - true (XQuery)
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Restituisce il valore di tipo xs:boolean True, che equivale a `xs:boolean("1")`.  
  
## <a name="syntax"></a>Sintassi  
  
```  
fn:true() as xs:boolean  
```  
  
## <a name="examples"></a>Esempio  
 In questo argomento vengono forniti esempi di XQuery sulle istanze XML archiviate in diverse colonne di tipo **XML** nel database AdventureWorks.  
  
### <a name="a-using-the-true-xquery-boolean-function"></a>R. Utilizzo della funzione booleana true() di XQuery  
 Nell'esempio seguente viene eseguita una query su una variabile **XML** non tipizzata. L'espressione nel metodo **value ()** restituisce un valore booleano **true ()** se "AAA" è il valore dell'attributo. Il metodo **value ()** del tipo di dati **XML** converte il valore booleano in un bit e lo restituisce.  
  
```  
DECLARE @x XML  
SET @x= '<ROOT><elem attr="aaa">bbb</elem></ROOT>'  
select @x.value(' if ( (/ROOT/elem/@attr)[1] eq "aaa" ) then fn:true() else fn:false() ', 'bit')  
go  
-- result = 1  
```  
  
 Nell'esempio seguente la query viene specificata in base a una colonna **XML** tipizzata. L' `if` espressione verifica il valore booleano tipizzato dell' `ROOT` elemento <> e restituisce il codice XML costruito, di conseguenza. Nell'esempio vengono eseguite le operazioni seguenti:  
  
-   Crea una raccolta di XML Schema che definisce l' `ROOT` elemento di> <del tipo xs: Boolean.  
  
-   Crea una tabella con una colonna **XML** tipizzata utilizzando la raccolta di XML Schema.  
  
-   Viene salvata un'istanza XML della colonna, sulla quale viene eseguita una query.  
  
```  
-- Drop table if exist  
--DROP TABLE T  
--go  
DROP XML SCHEMA COLLECTION SC  
go  
CREATE XML SCHEMA COLLECTION SC AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  
targetNamespace="QNameXSD" >  
      <element name="ROOT" type="boolean" nillable="true"/>  
</schema>'  
go  
CREATE TABLE T (xmlCol XML(SC))  
go  
-- following OK  
insert into T values ('<ROOT xmlns="QNameXSD">true</ROOT>')  
 go  
-- Retrieve the local name.   
SELECT xmlCol.query('declare namespace a="QNameXSD";   
   if (/a:ROOT[1] eq true()) then  
       <result>Found boolean true</result>  
   else  
       <result>Found boolean false</result>')  
  
FROM T  
-- result = <result>Found boolean true</result>  
-- Clean up  
DROP TABLE T  
go  
DROP XML SCHEMA COLLECTION SC  
go  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni costruttore booleane &#40;XQuery&#41;](./xquery-functions-against-the-xml-data-type.md)  
  
