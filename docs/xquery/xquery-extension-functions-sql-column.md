---
title: 'Funzione SQL: Column () (XQuery) | Microsoft Docs'
description: "Informazioni sul modo in cui la funzione XQuery SQL: Column () può essere utilizzata per associare dati relazionali non XML all'interno di XML e per unire dati relazionali e XML."
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- sql:column function
- sql:column() function
ms.assetid: e8f67bdf-b489-49a9-9d0f-2069c1750467
author: rothja
ms.author: jroth
ms.openlocfilehash: 3d2f550e3516594a269257786e109df9c84a1082
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341874"
---
# <a name="xquery-extension-functions---sqlcolumn"></a>Funzioni per estensioni XQuery - sql:column()
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Come descritto nell'argomento relativo [all'associazione di dati relazionali all'interno di codice XML](../t-sql/xml/binding-relational-data-inside-xml-data.md), è possibile utilizzare la funzione **SQL: Column (()** quando si utilizzano i [metodi con tipo di dati XML](../t-sql/xml/xml-data-type-methods.md) per esporre un valore relazionale all'interno di XQuery.  
  
 Ad esempio, il [metodo query () (tipo di dati XML)](../t-sql/xml/query-method-xml-data-type.md) viene utilizzato per specificare una query su un'istanza XML archiviata in una variabile o una colonna di tipo **XML** . Talvolta, potrebbe inoltre essere necessario che la query utilizzi valori di un'altra colonna non XML per unire i dati relazionali e XML. A tale scopo, usare la funzione **SQL: Column ()** .  
  
 Verrà eseguito il mapping tra il valore SQL e un valore XQuery corrispondente e il relativo tipo sarà un tipo di base XQuery equivalente al tipo SQL corrispondente.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sql:column("columnName")  
```  
  
## <a name="remarks"></a>Osservazioni  
 Si noti che il riferimento a una colonna specificata nella funzione **SQL: Column ()** all'interno di un linguaggio XQuery fa riferimento a una colonna della riga in fase di elaborazione.  
  
 In [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] è possibile fare riferimento a un'istanza **XML** solo nel contesto dell'espressione di origine di un'istruzione XML-DML insert; in caso contrario, non è possibile fare riferimento a colonne di tipo **XML** o di tipo CLR definito dall'utente.  
  
 La funzione **SQL: Column ()** non è supportata nelle operazioni di join. Al suo posto, è possibile utilizzare l'operazione APPLY.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-sqlcolumn-to-retrieve-the-relational-value-inside-xml"></a>R. Utilizzo della funzione sql:column() per recuperare il valore relazionale nell'istanza XML  
 Nell'esempio seguente relativo alla costruzione di un'istanza XML viene illustrato il recupero di valori da una colonna relazionale non XML per associare dati XML e relazionali.  
  
 La query costruisce un'istanza XML con il formato seguente:  
  
```xml
<Product ProductID="771" ProductName="Mountain-100 Silver, 38" ProductPrice="3399.99" ProductModelID="19"   
  ProductModelName="Mountain 100" />  
```  
  
 Nell'istanza XML costruita si noti quanto segue:  
  
-   I valori di attributo **ProductID**, **ProductName** e **ProductPrice** vengono ottenuti dalla tabella **Product** .  
  
-   Il valore dell'attributo **ProductModelID** viene recuperato dalla tabella **ProductModel** .  
  
-   Per rendere la query più interessante, il valore dell'attributo **ProductModelName** viene ottenuto dalla colonna **CatalogDescription** di **tipo XML**. Poiché le informazioni XML del catalogo prodotti non vengono archiviate per tutti i modelli di prodotto, l'istruzione `if` viene utilizzata per recuperare il valore solo se esiste.  
  
    ```sql
    SELECT P.ProductID, CatalogDescription.query('  
    declare namespace pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
           <Product   
               ProductID=       "{ sql:column("P.ProductID") }"  
               ProductName=     "{ sql:column("P.Name") }"  
               ProductPrice=    "{ sql:column("P.ListPrice") }"  
               ProductModelID= "{ sql:column("PM.ProductModelID") }" >  
               { if (not(empty(/pd:ProductDescription))) then  
                 attribute ProductModelName { /pd:ProductDescription[1]/@ProductModelName }  
                else   
                   ()  
    }  
            </Product>  
    ') as Result  
    FROM Production.ProductModel PM, Production.Product P  
    WHERE PM.ProductModelID = P.ProductModelID  
    AND   CatalogDescription is not NULL  
    ORDER By PM.ProductModelID  
    ```  
  
 Dalla query precedente si noti quanto segue:  
  
-   Poiché i valori vengono recuperati da due tabelle diverse, la clausola FROM specifica due tabelle. La condizione della clausola WHERE filtra il risultato e recupera solo i prodotti con modelli che includono descrizioni del catalogo.  
  
-   La parola chiave **namespace** nel [prologo XQuery](../xquery/modules-and-prologs-xquery-prolog.md) definisce il prefisso dello spazio dei nomi XML, "PD", usato nel corpo della query. Si noti che gli alias di tabella "P" e "PM" vengono definiti nella clausola FROM della query stessa.  
  
-   La funzione **SQL: Column ()** viene utilizzata per fornire valori non XML all'interno di XML.  
  
 Risultato parziale:  
  
```  
ProductID               Result  
-----------------------------------------------------------------  
771         <Product ProductID="771"                   ProductName="Mountain-100 Silver, 38"   
                  ProductPrice="3399.99" ProductModelID="19"   
                  ProductModelName="Mountain 100" />  
...  
```  
  
 La query seguente costruisce un'istanza XML che contiene informazioni specifiche del prodotto, inclusi i valori degli attributi ProductID, ProductName, ProductPrice e, se disponibile, ProductModelName per tutti i prodotti appartenenti al modello di prodotto specifico ProductModelID=19. Il codice XML viene quindi assegnato alla @x variabile di tipo **XML** .  
  
```sql
declare @x xml  
SELECT @x = CatalogDescription.query('  
declare namespace pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
       <Product   
           ProductID=       "{ sql:column("P.ProductID") }"  
           ProductName=     "{ sql:column("P.Name") }"  
           ProductPrice=    "{ sql:column("P.ListPrice") }"  
           ProductModelID= "{ sql:column("PM.ProductModelID") }" >  
           { if (not(empty(/pd:ProductDescription))) then  
             attribute ProductModelName { /pd:ProductDescription[1]/@ProductModelName }  
            else   
               ()  
}  
        </Product>  
')   
FROM Production.ProductModel PM, Production.Product P  
WHERE PM.ProductModelID = P.ProductModelID  
And P.ProductModelID = 19  
select @x  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni di estensione di SQL Server XQuery]()   
 [Confronto dati XML tipizzati con dati XML non tipizzati](../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Dati XML &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)   
 [Creare istanze di dati XML](../relational-databases/xml/create-instances-of-xml-data.md)   
 [metodi con tipo di dati XML](../t-sql/xml/xml-data-type-methods.md)   
 [Linguaggio XML di manipolazione dei dati &#40;XML DML&#41;](../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
