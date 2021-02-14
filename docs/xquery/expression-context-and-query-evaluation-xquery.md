---
title: Contesto delle espressioni e valutazione delle query (XQuery) | Microsoft Docs
description: Informazioni sul modo in cui le informazioni del contesto statico e dinamico di un'espressione XQuery vengono usate per analizzarle e valutarle.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- expression context [XQuery]
- XQuery, expression context
- XQuery, query evaluation
- static context
- dynamic context [XQuery]
ms.assetid: 5059f858-086a-40d4-811e-81fedaa18b06
author: rothja
ms.author: jroth
ms.openlocfilehash: f4fd85737d1a7cdc981576f624a2a403f0eb19ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341950"
---
# <a name="expression-context-and-query-evaluation-xquery"></a>Contesto delle espressioni e valutazione delle query (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Il contesto di un'espressione è rappresentato dalle informazioni che ne consentono l'analisi e la valutazione. Di seguito sono illustrate le due fasi della valutazione di XQuery:  
  
-   **Contesto statico** : si tratta della fase di compilazione della query. A seconda delle informazioni disponibili, durante l'analisi statica della query a volte vengono generati errori.  
  
-   **Contesto dinamico** : questa è la fase di esecuzione della query. Anche se in una query non si verificano errori statici, ad esempio durante la compilazione, è possibile che vengano generati errori durante l'esecuzione della query.  
  
## <a name="static-context"></a>Contesto statico  
 L'inizializzazione del contesto statico prevede la raccolta di tutte le informazioni necessarie per l'analisi statica dell'espressione. Nella fase di inizializzazione vengono eseguite le operazioni seguenti:  
  
-   Il criterio di **spazi vuoti limite** è impostato su Strip. Gli spazi vuoti limite non vengono pertanto conservati dai costruttori di **elementi** e **attributi** nella query. Ad esempio:  
  
    ```  
    declare @x xml  
    set @x=''  
    select @x.query('<a>  {"Hello"}  </a>,  
  
        <b> {"Hello2"}  </b>')  
    ```  
  
     Questa query restituisce il risultato seguente, perché lo spazio limite viene escluso durante l'analisi dell'espressione XQuery:  
  
    ```  
    <a>Hello</a><b>Hello2</b>  
    ```  
  
-   Il prefisso e l'associazione allo spazio dei nomi vengono inizializzati per gli elementi seguenti:  
  
    -   Un set di spazi dei nomi predefiniti.  
  
    -   Gli spazi dei nomi definiti utilizzando WITH XMLNAMESPACES. Per altre informazioni, vedere [aggiungere spazi dei nomi alle query con with XMLnamespaces](../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)).  
  
    -   Gli spazi dei nomi definiti nel prologo della query. Si noti che le dichiarazioni degli spazi dei nomi nel prologo possono avere la priorità sulla dichiarazione dello spazio dei nomi di WITH XMLNAMESPACES. Nella query seguente, ad esempio, con XmlNamespaces dichiara un prefisso (PD) che lo associa allo spazio dei nomi ( `https://someURI` ). Nella clausola WHERE, tuttavia, il prologo della query ha la priorità su tale associazione.  
  
        ```  
        WITH XMLNAMESPACES ('https://someURI' AS pd)  
        SELECT ProductModelID, CatalogDescription.query('  
            <Product   
                ProductModelID= "{ sql:column("ProductModelID") }"   
                />  
        ') AS Result  
        FROM Production.ProductModel  
        WHERE CatalogDescription.exist('  
            declare namespace  pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
             /pd:ProductDescription[(pd:Specifications)]'  
            ) = 1  
        ```  
  
     Tutte le associazioni di spazi dei nomi vengono risolte durante l'inizializzazione del contesto statico.  
  
-   Se si esegue una query su una colonna o una variabile **XML** tipizzata, i componenti della raccolta XML Schema associata alla colonna o alla variabile vengono importati nel contesto statico. Per altre informazioni, vedere [Confrontare dati XML tipizzati con dati XML non tipizzati](../relational-databases/xml/compare-typed-xml-to-untyped-xml.md).  
  
-   Per ogni tipo atomico presente negli schemi importati, nel contesto statico viene inoltre resa disponibile una funzione di cast, Questo è illustrato nell'esempio seguente. In questo esempio viene specificata una query su una variabile **XML** tipizzata. La raccolta di XML Schema associata alla variabile definisce un tipo atomico, myType. Corrispondente a questo tipo, una funzione di cast, **MyType ()**, è disponibile durante l'analisi statica. L'espressione della query (`ns:myType(0)`) restituisce un valore di tipo myType.  
  
    ```  
    -- DROP XML SCHEMA COLLECTION SC  
    -- go  
    CREATE XML SCHEMA COLLECTION SC AS '<schema xmlns="http://www.w3.org/2001/XMLSchema"   
    targetNamespace="myNS" xmlns:ns="myNS"  
    xmlns:s="https://schemas.microsoft.com/sqlserver/2004/sqltypes">  
          <import namespace="https://schemas.microsoft.com/sqlserver/2004/sqltypes"/>  
          <simpleType name="myType">  
                <restriction base="int">  
                 <enumeration value="0" />  
                  <enumeration value="1"/>  
                </restriction>  
          </simpleType>  
          <element name="root" type="ns:myType"/>  
    </schema>'  
    go  
  
    DECLARE @var XML(SC)  
    SET @var = '<root xmlns="myNS">0</root>'  
    -- specify myType() casting function in the query  
    SELECT @var.query('declare namespace ns="myNS"; ns:myType(0)')  
    ```  
  
     Nell'esempio seguente la funzione di cast per il tipo XML incorporato **int** è specificata nell'espressione.  
  
    ```  
    declare @x xml  
    set @x = ''  
    select @x.query('xs:int(5)')  
    go  
    ```  
  
 Dopo che il contesto statico è stato inizializzato, viene analizzata (compilata) l'espressione della query. L'analisi statica include le operazioni seguenti:  
  
1.  Analisi della query.  
  
2.  Risoluzione dei nomi della funzione e del tipo specificati nell'espressione.  
  
3.  Tipizzazione statica della query, che consente di verificare che la query sia indipendente dai tipi. Ad esempio, la query seguente restituisce un errore statico, perché l' **+** operatore richiede argomenti di tipo primitivi numerici:  
  
    ```  
    declare @x xml  
    set @x=''  
    SELECT @x.query('"x" + 4')  
    ```  
  
     Nell'esempio seguente l'operatore **value ()** richiede un singleton. Come specificato nella XML Schema, possono essere presenti più \<Elem> elementi. L'analisi statica dell'espressione determina che non è indipendente dai tipi e viene restituito un errore statico. Per risolvere l'errore, è necessario riformulare l'espressione in modo da specificare in modo esplicito un singleton (`data(/x:Elem)[1]`).  
  
    ```  
    DROP XML SCHEMA COLLECTION SC  
    go  
    CREATE XML SCHEMA COLLECTION SC AS '<schema xmlns="http://www.w3.org/2001/XMLSchema"   
    targetNamespace="myNS" xmlns:ns="myNS"  
    xmlns:s="https://schemas.microsoft.com/sqlserver/2004/sqltypes">  
          <import namespace="https://schemas.microsoft.com/sqlserver/2004/sqltypes"/>  
          <element name="Elem" type="string"/>  
    </schema>'  
    go  
  
    declare @x xml (SC)  
    set @x='<Elem xmlns="myNS">test</Elem><Elem xmlns="myNS">test2</Elem>'  
    SELECT @x.value('declare namespace x="myNS"; data(/x:Elem)[1]','varchar(20)')  
    ```  
  
     Per ulteriori informazioni, vedere [XQuery e tipizzazione statica](../xquery/xquery-and-static-typing.md).  
  
### <a name="implementation-restrictions"></a>Limitazioni di implementazione  
 Di seguito sono elencate le limitazioni correlate al contesto statico:  
  
-   La modalità di compatibilità XPath non è supportata.  
  
-   Per la costruzione del codice XML, è supportata solo la modalità di costruzione con rimozione. Si tratta dell'impostazione predefinita. Pertanto, il tipo del nodo dell'elemento costruito è **xdt: tipo non tipizzato** e gli attributi sono di tipo **xdt: untypedAtomic** .  
  
-   È supportata solo la modalità ordinata.  
  
-   Sono supportati solo i criteri di rimozione dello spazio XML.  
  
-   La funzionalità URI di base non è supportata.  
  
-   **FN: doc ()** non supportato.  
  
-   **FN: Collection ()** non è supportato.  
  
-   Il flagger statico XQuery non è disponibile.  
  
-   Vengono utilizzate le regole di confronto associate al tipo di dati **XML** . Vengono sempre impostate le regole di confronto dei punti di codice Unicode.  
  
## <a name="dynamic-context"></a>Contesto dinamico  
 Il contesto dinamico indica le informazioni che devono essere disponibili durante l'esecuzione dell'espressione. Oltre al contesto statico, le informazioni seguenti vengono inizializzate come parte del contesto dinamico:  
  
-   L'elemento di contesto, la posizione del contesto e le dimensioni del contesto vengono inizializzate come illustrato di seguito. Si noti che tutti questi valori possono essere sostituiti dal [metodo nodes ()](../t-sql/xml/nodes-method-xml-data-type.md).  
  
    -   Il tipo di dati **XML** imposta l'elemento di contesto, ovvero il nodo in fase di elaborazione, sul nodo del documento.  
  
    -   La posizione del contesto, ovvero la posizione dell'elemento di contesto rispetto ai nodi da elaborare, viene innanzitutto impostata su 1.  
  
    -   Le dimensioni del contesto, ovvero il numero di elementi della sequenza da elaborare, vengono innanzitutto impostate su 1, perché esiste sempre un nodo del documento.  
  
### <a name="implementation-restrictions"></a>Limitazioni di implementazione  
 Di seguito sono elencate le limitazioni correlate al contesto dinamico:  
  
-   Le funzioni di contesto di **data e ora correnti** , **FN: Current-date**, **FN: Current-time** e **FN: Current-DateTime**, non sono supportate.  
  
-   Il **fuso orario implicito** è fisso a UTC + 0 e non può essere modificato.  
  
-   La funzione **FN: doc ()** non è supportata. Tutte le query vengono eseguite su variabili o colonne di tipo **XML** .  
  
-   La funzione **FN: Collection ()** non è supportata.  
  
## <a name="see-also"></a>Vedere anche  
 [Nozioni fondamentali su XQuery](../xquery/xquery-basics.md)   
 [Confronto dati XML tipizzati con dati XML non tipizzati](../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Raccolte di XML Schema &#40;SQL Server&#41;](../relational-databases/xml/xml-schema-collections-sql-server.md)  
  
  
