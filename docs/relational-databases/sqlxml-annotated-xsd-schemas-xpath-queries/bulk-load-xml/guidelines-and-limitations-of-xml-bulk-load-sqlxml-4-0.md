---
title: Linee guida e limitazioni del caricamento bulk XML (SQLXML)
description: Informazioni sulle linee guida e sulle limitazioni relative all'utilizzo del caricamento bulk XML in SQLXML 4.0.
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- XML Bulk Load [SQLXML], about XML Bulk Load
- bulk load [SQLXML], about bulk load
ms.assetid: c5885d14-c7c1-47b3-a389-455e99a7ece1
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5bb35c4a9302feb91d160c5f8ae105516c9afb4b
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490631"
---
# <a name="guidelines-and-limitations-of-xml-bulk-load-sqlxml-40"></a>Linee guida e limitazioni per il caricamento bulk XML (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Quando si utilizza il caricamento bulk XML, è consigliabile disporre di una certa familiarità con le linee guida e le limitazioni seguenti:  
  
-   Gli schemi inline non sono supportati.  
  
     Se nel documento XML di origine è presente uno schema inline, questo viene ignorato dal caricamento bulk XML. È necessario specificare lo schema di mapping per il caricamento bulk XML come esterno ai dati XML. Non è possibile specificare lo schema di mapping in un nodo usando l'attributo **xmlns="x:schema".**  
  
-   Un documento XML viene controllato per verificare che utilizzi un formato corretto, ma non viene convalidato.  
  
     Il caricamento bulk XML controlla il documento XML per determinare se il formato è corretto, per garantire che il codice XML sia conforme ai requisiti di sintassi della raccomandazione XML 1.0 di World Wide Web Consortium. Se il formato del documento non è corretto, il caricamento bulk XML annulla l'elaborazione e restituisce un errore. L'unica eccezione a questo comportamento è costituita dal caso in cui il documento è un frammento, ad esempio non dispone di un singolo elemento radice. In questo caso, il documento verrà caricato.  
  
     Il caricamento bulk XML non convalida il documento rispetto ad alcuno schema dati XML o DTD definito o a cui si fa riferimento all'interno del file di dati XML. Il caricamento bulk XML, inoltre, non convalida il file di dati XML rispetto allo schema di mapping fornito.  
  
-   Vengono ignorate tutte le informazioni sui prologhi XML.  
  
     Il caricamento bulk XML ignora tutte le informazioni prima e dopo \<root> l'elemento nel documento XML. Il caricamento bulk XML, ad esempio, ignora qualsiasi dichiarazione XML, qualsiasi definizione DTD interna e tutti i riferimenti DTD esterni, i commenti e così via.  
  
-   Se è presente uno schema di mapping che definisce una relazione di chiave primaria/chiave esterna tra due tabelle, ad esempio tra Customer e CustOrder, la tabella con la chiave primaria deve essere descritta per prima nello schema. La tabella con la colonna chiave esterna deve essere visualizzata come successiva nello schema. Il motivo è che l'ordine in cui le tabelle vengono identificate nello schema è l'ordine utilizzato per caricarle nel database. Ad esempio, lo schema XDR seguente genera un errore quando viene utilizzato nel caricamento bulk XML perché l'elemento viene **\<Order>** descritto prima **\<Customer>** dell'elemento . La colonna CustomerID in CustOrder è una colonna chiave esterna che fa riferimento alla colonna chiave primaria CustomerID nella tabella Cust.  
  
    ```  
    <?xml version="1.0" ?>  
    <Schema xmlns="urn:schemas-microsoft-com:xml-data"   
            xmlns:dt="urn:schemas-microsoft-com:xml:datatypes"    
            xmlns:sql="urn:schemas-microsoft-com:xml-sql" >  
  
        <ElementType name="Order" sql:relation="CustOrder" >  
          <AttributeType name="OrderID" />  
          <AttributeType name="CustomerID" />  
          <attribute type="OrderID" />  
          <attribute type="CustomerID" />  
        </ElementType>  
  
       <ElementType name="CustomerID" dt:type="int" />  
       <ElementType name="CompanyName" dt:type="string" />  
       <ElementType name="City" dt:type="string" />  
  
       <ElementType name="root" sql:is-constant="1">  
          <element type="Customers" />  
       </ElementType>  
       <ElementType name="Customers" sql:relation="Cust"   
                         sql:overflow-field="OverflowColumn"  >  
          <element type="CustomerID" sql:field="CustomerID" />  
          <element type="CompanyName" sql:field="CompanyName" />  
          <element type="City" sql:field="City" />  
          <element type="Order" >   
               <sql:relationship  
                   key-relation="Cust"  
                    key="CustomerID"  
                    foreign-key="CustomerID"  
                    foreign-relation="CustOrder" />  
          </element>  
       </ElementType>  
    </Schema>  
    ```  
  
-   Se lo schema non specifica colonne di overflow tramite l'annotazione **sql:overflow-field,** il caricamento bulk XML ignora tutti i dati presenti nel documento XML, ma non sono descritti nello schema di mapping.  
  
     Il caricamento bulk XML applica lo schema di mapping specificato ogni volta che rileva tag noti nel flusso di dati XML e ignora i dati presenti nel documento XML ma che non sono descritti nello schema. Si supponga, ad esempio, di avere uno schema di mapping che descrive un **\<Customer>** elemento . Il file di dati XML ha un tag radice (non descritto nello schema) che racchiude **\<AllCustomers>** tutti gli **\<Customer>** elementi:  
  
    ```  
    <AllCustomers>  
      <Customer>...</Customer>  
      <Customer>...</Customer>  
       ...  
    </AllCustomers>  
    ```  
  
     In questo caso, il caricamento bulk XML ignora **\<AllCustomers>** l'elemento e inizia il mapping in corrispondenza dell'elemento. **\<Customer>** Il caricamento bulk XML ignora gli elementi non descritti nello schema ma che sono presenti nel documento XML.  
  
     Si consideri un altro file di dati di origine XML che **\<Order>** contiene elementi . Tali elementi non vengono descritti nello schema di mapping:  
  
    ```  
    <AllCustomers>  
      <Customer>...</Customer>  
        <Order> ... </Order>  
        <Order> ... </Order>  
         ...  
      <Customer>...</Customer>  
        <Order> ... </Order>  
        <Order> ... </Order>  
         ...  
      ...  
    </AllCustomers>  
    ```  
  
     Il caricamento bulk XML ignora questi **\<Order>** elementi. Tuttavia, se si usa l'annotazione **sql:overflow-field** nello schema per identificare una colonna come colonna di overflow, il caricamento bulk XML archivia tutti i dati non utilizzati in questa colonna.  
  
-   Le sezioni CDATA e i riferimenti a entità vengono convertiti nei rispettivi equivalenti in formato stringa prima di essere archiviati nel database.  
  
     In questo esempio, una sezione CDATA esegue il wrapping del valore per **\<City>** l'elemento . Il caricamento bulk XML estrae il valore stringa ("NY") prima di inserire **\<City>** l'elemento nel database.  
  
    ```  
    <City><![CDATA[NY]]> </City>  
    ```  
  
     Il caricamento bulk XML non mantiene i riferimenti alle entità.  
  
-   Se lo schema di mapping specifica il valore predefinito per un attributo e i dati di origine XML non contengono tale attributo, il caricamento bulk XML utilizza il valore predefinito.  
  
     Lo schema XDR di esempio seguente assegna un valore predefinito **all'attributo HireDate:**  
  
    ```  
    <?xml version="1.0" ?>  
    <Schema xmlns="urn:schemas-microsoft-com:xml-data"   
            xmlns:dt="urn:schemas-microsoft-com:xml:datatypes"    
            xmlns:sql="urn:schemas-microsoft-com:xml-sql" >  
       <ElementType name="root" sql:is-constant="1">  
          <element type="Customers" />  
       </ElementType>  
  
       <ElementType name="Customers" sql:relation="Cust3" >  
          <AttributeType name="CustomerID" dt:type="int"  />  
          <AttributeType name="HireDate"  default="2000-01-01" />  
          <AttributeType name="Salary"   />  
  
          <attribute type="CustomerID" sql:field="CustomerID" />  
          <attribute type="HireDate"   sql:field="HireDate"  />  
          <attribute type="Salary"     sql:field="Salary"    />  
       </ElementType>  
    </Schema>  
    ```  
  
     In questi dati XML **l'attributo HireDate** non è presente nel secondo **\<Customers>** elemento. Quando il caricamento bulk XML inserisce il secondo elemento nel database, usa il **\<Customers>** valore predefinito specificato nello schema.  
  
    ```  
    <ROOT>  
      <Customers CustomerID="1" HireDate="1999-01-01" Salary="10000" />  
      <Customers CustomerID="2" Salary="10000" />  
    </ROOT>  
    ```  
  
-   **L'annotazione sql:url-encode** non è supportata:  
  
     Non è possibile specificare un URL nei dati XML di input e prevedere che il caricamento bulk legga i dati da tale posizione.  
  
     Vengono create le tabelle identificate nello schema di mapping (il database deve essere presente). Se una o più tabelle esistono già nel database, la proprietà SGDropTables determina se queste tabelle preesistenti devono essere eliminate e ri-create.  
  
-   Se si specifica la proprietà SchemaGen ,ad esempio SchemaGen = true, vengono create le tabelle identificate nello schema di mapping. SchemaGen, tuttavia, non crea vincoli (ad esempio i vincoli PRIMARY KEY/FOREIGN KEY) su queste tabelle con un'eccezione: se i nodi XML che costituiscono la chiave primaria in una relazione sono definiti come con un tipo XML di ID (ovvero **type="xsd:ID"** per XSD) e la proprietà SGUseID è impostata su True per SchemaGen, non vengono create solo chiavi primarie dai nodi tipi di ID, ma le relazioni chiave primaria/chiave esterna vengono create dalle relazioni tra schemi di mapping.  
  
-   SchemaGen non usa facet ed estensioni dello schema XSD per generare lo [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] schema relazionale.  
  
-   Se si specifica la proprietà SchemaGen (ad esempio SchemaGen = true) nel caricamento bulk, vengono aggiornate solo le tabelle (e non le viste del nome condiviso) specificate.  
  
-   SchemaGen fornisce solo funzionalità di base per la generazione dello schema relazionale da XSD con annotazioni. Se necessario, l'utente deve modificare manualmente le tabelle generate.  
  
-   Se tra le tabelle sono presenti più relazioni, SchemaGen tenta di creare una singola relazione che include tutte le chiavi coinvolte tra le due tabelle. Questa limitazione può essere la causa di un errore [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
-   Quando si esegue il caricamento bulk di dati XML in un database, deve essere presente almeno un attributo o un elemento figlio nello schema di mapping mappato a una colonna del database.  
  
-   Se si inseriscono valori di data tramite il caricamento bulk XML, i valori devono essere specificati nel formato (-)AAAA-MM-GG((+-)FO). Si tratta del formato XSD standard per la data.  
  
-   Alcuni flag della proprietà non sono compatibili con altri flag della proprietà stessa. Ad esempio, il caricamento bulk non supporta **Ignoreduplicatekeys=true** insieme **a Keepidentity=false.** Quando **Keepidentity=false,** il caricamento bulk prevede che il server generi i valori della chiave. Le tabelle devono avere **un vincolo IDENTITY** per la chiave. Il server non genererà chiavi duplicate, pertanto non è necessario impostare **Ignoreduplicatekeys** su **true.** **Ignoreduplicatekeys** deve essere impostato su **true** solo quando si caricano i valori di chiave primaria dai dati in ingresso in una tabella che contiene righe e potrebbe verificarsi un conflitto di valori di chiave primaria.  
  
  
