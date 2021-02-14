---
title: local-name-from-QName (XQuery) | Microsoft Docs
description: Informazioni su come usare la funzione local-name-from-QName () per restituire la parte del nome locale di un QName.
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:local-name-from-QName function
- local-name-from-QName function
ms.assetid: fafed718-8c3c-403f-93ee-ec51fc157a6e
author: rothja
ms.author: jroth
ms.openlocfilehash: 97852b8479df926a933e34f5a8dddff7eb240e65
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344999"
---
# <a name="functions-related-to-qnames---local-name-from-qname"></a>Funzioni correlate a elementi QName - local-name-from-QName
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Restituisce XS: NCNAME che rappresenta la parte locale di QName specificata da *$arg*. Se *$arg* è una sequenza vuota, il risultato è una sequenza vuota.  
  
## <a name="syntax"></a>Sintassi  
  
```  
fn:local-name-from-QName($arg as xs:QName?) as xs:NCName?  
```  
  
## <a name="arguments"></a>Argomenti  
 *$arg*  
 QName da cui deve essere estratto il nome locale.  
  
## <a name="examples"></a>Esempio  
 In questo argomento vengono forniti esempi di XQuery sulle istanze XML archiviate in diverse colonne di tipo **XML** nel [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] database.  
  
 Nell'esempio seguente viene usata la funzione **local-name-from-QName ()** per recuperare le parti del nome locale e dell'URI dello spazio dei nomi da un valore di tipo QName. Nell'esempio vengono eseguite le operazioni seguenti:  
  
-   Viene creata una raccolta di XML Schema.  
  
-   Viene creata una tabella con una colonna di tipo XML. Il tipo XML viene tipizzato tramite la raccolta di XML Schema.  
  
-   Viene archiviata un'istanza XML di esempio nella tabella. Utilizzando il metodo **query ()** del tipo di dati XML, viene eseguita l'espressione di query per recuperare la parte del nome locale del valore del tipo QName dall'istanza di.  
  
```sql
DROP TABLE T  
go  
DROP XML SCHEMA COLLECTION SC  
go  
CREATE XML SCHEMA COLLECTION SC AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  
targetNamespace="QNameXSD" >  
      <element name="root" type="QName" nillable="true"/>  
</schema>'  
go  
  
CREATE TABLE T (xmlCol XML(SC))  
go  
-- following OK  
insert into T values ('<root xmlns="QNameXSD" xmlns:a="https://someURI">a:someLocalName</root>')  
 go  
-- Retrieve the local name.   
SELECT xmlCol.query('declare default element namespace "QNameXSD"; local-name-from-QName(/root[1])')  
FROM T  
-- Result = someLocalName  
-- You can retrieve namespace URI part from the QName using the namespace-uri-from-QName() function  
SELECT xmlCol.query('declare default element namespace "QNameXSD"; namespace-uri-from-QName(/root[1])')  
FROM T  
-- Result = https://someURI  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni correlate a QName &#40;XQuery&#41;](./functions-related-to-qnames-expanded-qname.md)  
  
