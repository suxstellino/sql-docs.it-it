---
title: Usare funzioni di conversione in query XPath (SQLXML)
description: Informazioni su come specificare le funzioni di conversione esplicita string() e number() nelle query XPath SQLXML 4.0.
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- explicit conversion functions [SQLXML]
- string function
- number function
- XPath queries [SQLXML], explicit conversion functions
ms.assetid: 1111cb5d-2bd9-4bdb-8de2-dc0e47452dd6
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 33e71474d1bee154028d78385d12ecbdac3ee69f
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490481"
---
# <a name="specifying-explicit-conversion-functions-in-xpath-queries-sqlxml-40"></a>Specifica di funzioni di conversione esplicita in query XPath (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Negli esempi seguenti viene illustrato come specificare le funzioni di conversione esplicita nelle query XPath. Le query XPath di questi esempi vengono specificate sullo schema di mapping contenuto in SampleSchema1.xml. Per informazioni su questo schema di esempio, vedere [Sample Annotated XSD Schema for XPath Examples &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/sample-annotated-xsd-schema-for-xpath-examples-sqlxml-4-0.md).  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-use-the-number-explicit-conversion-function"></a>R. Utilizzo della funzione di conversione esplicita number ()  
 La **funzione number()** converte un argomento in un numero.  
  
 Supponendo che il valore di **ContactID** non sia numerico, la query seguente converte **ContactID** in un numero e lo confronta con il valore 4. La query restituisce quindi tutti gli elementi figlio del nodo di contesto con **\<Employee>** **l'attributo ContactID** con valore numerico 4:  
  
```  
/child::Contact[number(attribute::ContactID)= 4]  
```  
  
 È possibile specificare un collegamento **all'asse** dell'attributo (@) e, poiché l'asse figlio è l'asse predefinito, può essere omesso dalla query:   
  
```  
/Contact[number(@ContactID) = 4]  
```  
  
 In termini relazionali, la query restituisce un dipendente con **contactID** 4.  
  
##### <a name="to-test-the-xpath-query-against-the-mapping-schema"></a>Per testare la query Xpath sullo schema di mapping  
  
1.  Copiare [il codice dello schema di](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/sample-annotated-xsd-schema-for-xpath-examples-sqlxml-4-0.md) esempio e incollarlo in un file di testo. Salvare il file con il nome SampleSchema1.xml.  
  
2.  Creare il modello seguente (ExplicitConversionA.xml) e salvarlo nella directory in cui è stato salvato il file SampleSchema1.xml.  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
      <sql:xpath-query mapping-schema="SampleSchema1.xml">  
        /Contact[number(@ContactID)=4]  
      </sql:xpath-query>  
    </ROOT>  
    ```  
  
     Il percorso di directory specificato per lo schema di mapping SampleSchema1.xml è relativo alla directory in cui è salvato il modello. È possibile specificare anche un percorso assoluto, ad esempio:  
  
    ```  
    mapping-schema="C:\MyDir\SampleSchema1.xml"  
    ```  
  
3.  Creare e utilizzare lo script di test SQLXML 4.0 (Sqlxml4test.vbs) per eseguire il modello.  
  
     Per altre informazioni, vedere [Utilizzo di ADO per l'esecuzione di query SQLXML 4.0.](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)  
  
 Di seguito è riportato il set di risultati relativo all'esecuzione di questo modello:  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <Contact ContactID="4" LastName="Acevedo" FirstName="Humberto" Title="Sr." />   
</ROOT>  
```  
  
### <a name="b-use-the-string-explicit-conversion-function"></a>B. Utilizzo della funzione di conversione esplicita string ()  
 La **funzione string()** converte un argomento in una stringa.  
  
 La query seguente converte **ContactID** in una stringa e lo confronta con il valore stringa "4". La query restituisce tutti **\<Employee>** gli elementi figlio del nodo di contesto con un **ContactID** con un valore stringa "4":  
  
```  
/child::Contact[string(attribute::ContactID)="4"]  
```  
  
 È possibile specificare un collegamento **all'asse** dell'attributo (@) e, poiché l'asse figlio è l'asse predefinito, può essere omesso dalla query:   
  
```  
/Contact[string(@ContactID)="4"]  
```  
  
 Dal punto di vista funzionale questa query restituisce gli stessi risultati della query di esempio precedente, sebbene la valutazione venga fatta rispetto a un valore stringa e non al valore numerico (ovvero il numero 4).  
  
##### <a name="to-test-the-xpath-query-against-the-mapping-schema"></a>Per testare la query Xpath sullo schema di mapping  
  
1.  Copiare [il codice dello schema di](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/sample-annotated-xsd-schema-for-xpath-examples-sqlxml-4-0.md) esempio e incollarlo in un file di testo. Salvare il file con il nome SampleSchema1.xml.  
  
2.  Creare il modello seguente (ExplicitConversionB.xml) e salvarlo nella directory in cui è stato salvato il file SampleSchema1.xml.  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
      <sql:xpath-query mapping-schema="SampleSchema1.xml">  
        Contact[string(@ContactID)="4"]  
      </sql:xpath-query>  
    </ROOT>  
    ```  
  
     Il percorso di directory specificato per lo schema di mapping SampleSchema1.xml è relativo alla directory in cui è salvato il modello. È possibile specificare anche un percorso assoluto, ad esempio:  
  
    ```  
    mapping-schema="C:\MyDir\SampleSchema1.xml"  
    ```  
  
3.  Creare e utilizzare lo script di test SQLXML 4.0 (Sqlxml4test.vbs) per eseguire il modello.  
  
     Per altre informazioni, vedere [Utilizzo di ADO per l'esecuzione di query SQLXML 4.0.](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)  
  
 Di seguito è riportato il set di risultati relativo all'esecuzione del modello:  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <Contact ContactID="4" LastName="Acevedo" FirstName="Humberto" Title="Sr." />   
</ROOT>  
```  
  
  
