---
title: Processo di generazione di record (SQLXML)
description: Informazioni dettagliate sul processo di generazione di record di caricamento bulk XML in SQLXML 4.0.
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- XML Bulk Load [SQLXML], record generation process
- node scopes [SQLXML]
- record subsets [SQLXML]
- scope [SQLXML]
- key ordering rules [SQLXML]
- record generation process for bulk loads [SQLXML]
- entering node scope [SQLXML]
- bulk load [SQLXML], record generation process
- leaving node scope [SQLXML]
- schema mapping [SQLXML]
ms.assetid: d8885bbe-6f15-4fb9-9684-ca7883cfe9ac
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: cd81acac3c40baea509b46032316ffd36e502c64
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490615"
---
# <a name="record-generation-process-sqlxml-40"></a>Processo di generazione di record (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Il caricamento bulk XML elabora i dati di input XML e prepara record per le tabelle appropriate in Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. La logica nel caricamento bulk XML determina il momento in cui generare un nuovo record, i valori di elemento o attributo figlio da copiare nei campi del record e il momento in cui il record è completo e pronto per essere inviato a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per l'inserimento.  
  
 Il caricamento bulk XML non carica tutti i dati di input XML in memoria e non produce set di record completi prima di inviare dati a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Ciò è dovuto al fatto che i dati di input XML possono essere costituiti da un documento di grandi dimensioni, il cui caricamento in memoria può risultare dispendioso. Al contrario, il caricamento bulk XML esegue le operazioni seguenti:  
  
1.  Analizza lo schema di mapping e prepara il piano di esecuzione necessario.  
  
2.  Applica il piano di esecuzione ai dati nel flusso di input.  
  
 Questa elaborazione sequenziale rende essenziale fornire i dati di input XML in un modo specifico. È necessario comprendere il modo in cui il caricamento bulk XML analizza lo schema di mapping e la modalità di esecuzione del processo di generazione di record. Una volta compresi questi elementi, è possibile fornire uno schema di mapping al caricamento bulk XML che produca i risultati desiderati.  
  
 Il caricamento bulk XML gestisce annotazioni dello schema di mapping, inclusi i mapping di colonne e tabelle (specificati in modo esplicito utilizzando annotazioni o in modo implicito tramite il mapping predefinito), e relazioni di join comuni.  
  
> [!NOTE]  
>  Si presuppone una certa familiarità con gli schemi di mapping XSD o XDR con annotazioni. Per altre informazioni sugli schemi, vedere Introduzione agli schemi XSD con annotazioni [&#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/introduction-to-annotated-xsd-schemas-sqlxml-4-0.md) o Schemi XDR con annotazioni [&#40;deprecati in SQLXML 4.0&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/annotated-xdr-schemas-deprecated-in-sqlxml-4-0.md).  
  
 La comprensione della generazione di record richiede una certa familiarità con i concetti seguenti:  
  
-   Ambito di un nodo  
  
-   Regola di generazione di record  
  
-   Subset di record e regola di ordinamento delle chiavi  
  
-   Eccezioni alla regola di generazione di record  
  
## <a name="scope-of-a-node"></a>Ambito di un nodo  
 Un nodo (un elemento o un attributo)  in un documento XML entra nell'ambito quando viene rilevato dal caricamento bulk XML nel flusso di dati di input XML. Per un nodo elemento, il tag di inizio dell'elemento inserisce l'elemento nell'ambito. Per un nodo attributo, il nome di attributo inserisce l'attributo nell'ambito.  
  
 Un nodo abbandona l'ambito quando non include più dati in corrispondenza del tag di fine (nel caso di un nodo elemento) o alla fine di un valore di attributo (nel caso di un nodo attributo).  
  
## <a name="record-generation-rule"></a>Regola di generazione di record  
 Quando un nodo (elemento o attributo) entra nell'ambito, è possibile che venga generato un record dal nodo. Il record dura fino a quando il nodo associato resta nell'ambito. Quando il nodo abbandona l'ambito, il caricamento bulk XML considera completo il record generato (con i dati) e lo invia a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per l'inserimento.  
  
 Si consideri ad esempio il seguente frammento di schema XSD:  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="Customer" sql:relation="Customers" >  
   <xsd:complexType>  
     <xsd:attribute name="CustomerID" type="xsd:string" />  
     <xsd:attribute name="CompanyName" type="xsd:string" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 Lo schema specifica un elemento **\<Customer>** con **gli attributi CustomerID** **e CompanyName.** **L'annotazione sql:relation** esegue il **\<Customer>** mapping dell'elemento alla tabella Customers.  
  
 Si consideri il frammento seguente di un documento XML:  
  
```  
<Customer CustomerID="1" CompanyName="xyz" />  
<Customer CustomerID="2" CompanyName="abc" />  
...  
```  
  
 Quando il caricamento bulk XML riceve lo schema descritto nei paragrafi precedenti e i dati XML come input, elabora i nodi (elementi e attributi) nei dati di origine nel modo seguente:  
  
-   Il tag di inizio del primo **\<Customer>** elemento porta tale elemento nell'ambito. Questo nodo viene mappato alla tabella Customers. Il caricamento bulk XML genera pertanto un record per la tabella Customers.  
  
-   Nello schema tutti gli attributi **\<Customer>** dell'elemento vengono mappati alle colonne della tabella Customers. Poiché questi attributi entrano nell'ambito, il caricamento bulk XML ne copia i valori nel record del cliente già generato dall'ambito padre.  
  
-   Quando il caricamento bulk XML raggiunge il tag di fine per **\<Customer>** l'elemento, l'elemento esce dall'ambito. Di conseguenza, il caricamento bulk XML considera il record completo e lo invia a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Il caricamento bulk XML segue questo processo per ogni elemento **\<Customer>** successivo.  
  
> [!IMPORTANT]  
>  In questo modello, poiché viene inserito un record quando viene raggiunto il tag di fine (o il nodo è esterno all'ambito), è necessario definire tutti i dati associati al record all'interno dell'ambito del nodo.  
  
## <a name="record-subset-and-the-key-ordering-rule"></a>Subset di record e regola di ordinamento delle chiavi  
 Quando si specifica uno schema di mapping che utilizza , il termine subset si riferisce al set di record generato sul **\<sql:relationship>** lato esterna della relazione. Nell'esempio seguente i record CustOrder sono sul lato foreign, **\<sql:relationship>** .  
  
 Si supponga, ad esempio, un database contenente le tabelle seguenti:  
  
-   Cust (CustomerID, CompanyName, City)  
  
-   CustOrder (CustomerID, OrderID)  
  
 CustomerID nella tabella CustOrder è una chiave esterna che fa riferimento alla chiave primaria CustomerID nella tabella Cust.  
  
 Si consideri a questo punto la vista XML specificata nello schema XSD con annotazioni seguente. Questo schema usa **\<sql:relationship>** per specificare la relazione tra le tabelle Cust e CustOrder.  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:annotation>  
  <xsd:appinfo>  
    <sql:relationship name="CustCustOrder"  
          parent="Cust"  
          parent-key="CustomerID"  
          child="CustOrder"  
          child-key="CustomerID" />  
  </xsd:appinfo>  
</xsd:annotation>  
  
  <xsd:element name="Customers" sql:relation="Cust" >  
   <xsd:complexType>  
     <xsd:sequence>  
       <xsd:element name="CustomerID"  type="xsd:integer" />  
       <xsd:element name="CompanyName" type="xsd:string" />  
       <xsd:element name="City"        type="xsd:string" />  
       <xsd:element name="Order"   
                          sql:relation="CustOrder"  
                          sql:relationship="CustCustOrder" >  
         <xsd:complexType>  
          <xsd:attribute name="OrderID" type="xsd:integer" />  
         </xsd:complexType>  
       </xsd:element>  
     </xsd:sequence>  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 I dati XML di esempio e la procedura per creare un esempio reale vengono forniti di seguito.  
  
-   Quando un nodo elemento nel file di dati XML entra nell'ambito, il caricamento bulk XML genera un **\<Customer>** record per la tabella Cust. Il caricamento bulk XML copia quindi i valori di colonna necessari (CustomerID, CompanyName e City) dagli elementi , e figlio quando questi elementi entrano **\<CustomerID>** **\<CompanyName>** **\<City>** nell'ambito.  
  
-   Quando un **\<Order>** nodo elemento entra nell'ambito, il caricamento bulk XML genera un record per la tabella CustOrder. Il caricamento bulk XML copia il valore **dell'attributo OrderID** in questo record. Il valore necessario per la colonna CustomerID viene ottenuto **\<CustomerID>** dall'elemento figlio **\<Customer>** dell'elemento . Il caricamento bulk XML usa le informazioni specificate in per ottenere il valore della chiave esterna CustomerID per questo record, a meno che non sia stato specificato **\<sql:relationship>** **l'attributo CustomerID** nell'elemento **\<Order>** . La regola generale è che se l'elemento figlio specifica in modo esplicito un valore per l'attributo di chiave esterna, il caricamento bulk XML usa tale valore e non ottiene il valore dall'elemento padre utilizzando l'oggetto **\<sql:relationship>** specificato. Quando questo nodo elemento esce dall'ambito, il caricamento bulk XML invia il record a e quindi elabora tutti i nodi elemento successivi **\<Order>** [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nello stesso **\<Order>** modo.  
  
-   Infine, il **\<Customer>** nodo dell'elemento esce dall'ambito. A questo punto, il caricamento bulk XML invia il record del cliente a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Il caricamento bulk XML segue questo processo per tutti i clienti successivi nel flusso di dati XML.  
  
 Di seguito sono riportate due osservazioni sullo schema di mapping:  
  
-   Quando lo schema soddisfa la regola di "contenimento", ad esempio tutti i dati associati al cliente e l'ordine viene definito nell'ambito dei nodi elemento e associati, il caricamento bulk ha esito **\<Customer>** **\<Order>** positivo.  
  
-   Nella descrizione **\<Customer>** dell'elemento, i relativi elementi figlio vengono specificati nell'ordine appropriato. In questo caso, **\<CustomerID>** l'elemento figlio viene specificato prima **\<Order>** dell'elemento figlio. Ciò significa che nel file di dati XML di input il valore dell'elemento è disponibile come valore di chiave esterna quando l'elemento **\<CustomerID>** **\<Order>** entra nell'ambito. Gli attributi chiave vengono specificati per primi. Questo comportamento è denominato "regola di ordinamento delle chiavi".  
  
     Se si specifica l'elemento figlio dopo l'elemento figlio, il valore non **\<CustomerID>** è disponibile quando l'elemento entra **\<Order>** **\<Order>** nell'ambito. Quando il tag di fine viene letto, il record per la tabella CustOrder viene considerato completo e viene inserito nella tabella CustOrder con un valore NULL per la colonna CustomerID, che non è il risultato **\</Order>** desiderato.  
  
#### <a name="to-create-a-working-sample"></a>Per creare un esempio reale  
  
1.  Salvare lo schema fornito in questo esempio come SampleSchema.xml.  
  
2.  Creare le tabelle seguenti:  
  
    ```  
    CREATE TABLE Cust (  
                  CustomerID     int         PRIMARY KEY,  
                  CompanyName    varchar(20) NOT NULL,  
                  City           varchar(20) DEFAULT 'Seattle')  
    GO  
    CREATE TABLE CustOrder (  
                 OrderID        int         PRIMARY KEY,  
                 CustomerID     int         FOREIGN KEY REFERENCES                                          Cust(CustomerID))  
    GO  
    ```  
  
3.  Salvare i dati di input XML di esempio seguenti come file SampleXMLData.xml:  
  
    ```  
    <ROOT>  
      <Customers>  
        <CustomerID>1111</CustomerID>  
        <CompanyName>Hanari Carnes</CompanyName>  
        <City>NY</City>   
        <Order OrderID="1" />  
        <Order OrderID="2" />  
      </Customers>  
  
      <Customers>  
        <CustomerID>1112</CustomerID>  
        <CompanyName>Toms Spezialitten</CompanyName>  
        <City>LA</City>  
        <Order OrderID="3" />  
      </Customers>  
      <Customers>  
        <CustomerID>1113</CustomerID>  
        <CompanyName>Victuailles en stock</CompanyName>  
        <Order OrderID="4" />  
    </Customers>  
    </ROOT>  
    ```  
  
4.  Per eseguire il caricamento bulk XML, salvare ed eseguire l'esempio (BulkLoad.vbs) di [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic, Scripting Edition (VBScript) seguente:  
  
    ```  
    set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
    objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
    objBL.ErrorLogFile = "c:\error.log"  
    objBL.CheckConstraints = True  
    objBL.Execute "c:\SampleSchema.xml", "c:\SampleXMLData.xml"  
    set objBL=Nothing  
    ```  
  
## <a name="exceptions-to-the-record-generation-rule"></a>Eccezioni alla regola di generazione di record  
 Il caricamento bulk XML non genera un record per un nodo quando entra nell'ambito se il nodo è un tipo IDREF o IDREFS. È necessario verificare che sia presente una descrizione completa del record in un punto dello schema. Le **annotazioni dt:type="nmtokens"** vengono ignorate così come il tipo IDREFS viene ignorato.  
  
 Si consideri ad esempio lo schema XSD seguente che descrive **\<Customer>** gli elementi **\<Order>** e . **\<Customer>** L'elemento include un attributo **OrderList** di tipo IDREFS. Il **\<sql:relationship>** tag specifica la relazione uno-a-molti tra il cliente e l'elenco di ordini.  
  
 Lo schema è il seguente:  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:annotation>  
  <xsd:appinfo>  
    <sql:relationship name="CustCustOrder"  
                 parent="Cust"  
                 parent-key="CustomerID"  
                 child="CustOrder"  
                 child-key="CustomerID" />  
  </xsd:appinfo>  
</xsd:annotation>  
  
  <xsd:element name="Customers" sql:relation="Cust" >  
   <xsd:complexType>  
    <xsd:attribute name="CustomerID" type="xsd:integer" />  
    <xsd:attribute name="CompanyName" type="xsd:string" />  
    <xsd:attribute name="City" type="xsd:string" />  
    <xsd:attribute name="OrderList"   
                       type="xsd:IDREFS"   
                       sql:relation="CustOrder"   
                       sql:field="OrderID"  
                       sql:relationship="CustCustOrder" >  
    </xsd:attribute>  
  </xsd:complexType>  
 </xsd:element>  
  
  <xsd:element name="Order" sql:relation="CustOrder" >  
   <xsd:complexType>  
    <xsd:attribute name="OrderID" type="xsd:string" />  
    <xsd:attribute name="CustomerID" type="xsd:integer" />  
    <xsd:attribute name="OrderDate" type="xsd:date" />  
  </xsd:complexType>  
 </xsd:element>  
</xsd:schema>  
```  
  
 Poiché il caricamento bulk ignora i nodi di tipo IDREFS, non esiste alcuna generazione di record quando il nodo **dell'attributo OrderList** entra nell'ambito. Se pertanto si desidera che i record relativi agli ordini vengano aggiunti alla tabella Orders, è necessario descrivere tali ordini in un punto dello schema. In questo schema, se si specifica l'elemento , il caricamento bulk XML aggiunge i record **\<Order>** degli ordini alla tabella Orders. **\<Order>** L'elemento descrive tutti gli attributi necessari per compilare il record per la tabella CustOrder.  
  
 È necessario assicurarsi che i **valori CustomerID** **e OrderID** nell'elemento **\<Customer>** corrispondano ai valori **\<Order>** nell'elemento . L'utente è responsabile del mantenimento dell'integrità referenziale.  
  
#### <a name="to-test-a-working-sample"></a>Per testare un esempio reale  
  
1.  Creare le tabelle seguenti:  
  
    ```  
    CREATE TABLE Cust (  
                  CustomerID     int          PRIMARY KEY,  
                  CompanyName    varchar(20)  NOT NULL,  
                  City           varchar(20)  DEFAULT 'Seattle')  
    GO  
    CREATE TABLE CustOrder (  
                  OrderID        varchar(10) PRIMARY KEY,  
                  CustomerID     int         FOREIGN KEY REFERENCES                                          Cust(CustomerID),  
                  OrderDate      datetime DEFAULT '2000-01-01')  
    GO  
    ```  
  
2.  Salvare lo schema di mapping fornito in questo esempio come file SampleSchema.xml.  
  
3.  Salvare i dati XML di esempio seguenti come file SampleXMLData.xml:  
  
    ```  
    <ROOT>  
      <Customers CustomerID="1111" CompanyName="Sean Chai" City="NY"  
                 OrderList="Ord1 Ord2" />  
      <Customers CustomerID="1112" CompanyName="Dont Know" City="LA"  
                 OrderList="Ord3 Ord4" />  
      <Order OrderID="Ord1" CustomerID="1111" OrderDate="1999-01-01" />  
      <Order OrderID="Ord2" CustomerID="1111" OrderDate="1999-02-01" />  
      <Order OrderID="Ord3" CustomerID="1112" OrderDate="1999-03-01" />  
      <Order OrderID="Ord4" CustomerID="1112" OrderDate="1999-04-01" />  
    </ROOT>  
    ```  
  
4.  Per eseguire il caricamento bulk XML, salvare ed eseguire l'esempio (SampleVB.vbs) di VBScript seguente:  
  
    ```  
    set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
    objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
    objBL.ErrorLogFile = "c:\error.log"  
    objBL.CheckConstraints=True  
    objBL.Execute "c:\SampleSchema.xml", "c:\SampleXMLData.xml"  
    set objBL=Nothing  
    ```  
  
  
