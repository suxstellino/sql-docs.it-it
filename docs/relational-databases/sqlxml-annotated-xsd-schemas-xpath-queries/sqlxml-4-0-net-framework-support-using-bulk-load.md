---
title: Utilizzo del caricamento bulk di SQLXML nell'ambiente .NET
description: Informazioni su come usare l'oggetto COM per il caricamento bulk di SQLXML 4.0 nell'ambiente .NET per il caricamento bulk di dati XML in un database.
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- SQLXML, XML Bulk Load
- XML Bulk Load [SQLXML], .NET environment
- .NET Framework [SQLXML], XML Bulk Load
- bulk load [SQLXML], .NET environment
ms.assetid: b85df83b-ba56-43bf-bcdf-b2a6fca43276
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3a914261a2bcb04974a22bdb70579827e94af771
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491558"
---
# <a name="sqlxml-40-net-framework-support---using-bulk-load"></a>Supporto SQLXML 4.0 per .NET Framework - Uso del caricamento bulk
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  In questo argomento viene illustrato l'utilizzo delle funzionalità di caricamento bulk XML nell'ambiente .NET. Per informazioni dettagliate sul caricamento bulk XML, vedere Esecuzione del caricamento bulk di dati [XML &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/performing-bulk-load-of-xml-data-sqlxml-4-0.md).  
  
 Per utilizzare l'oggetto COM del caricamento bulk di SQLXML da un ambiente gestito, è necessario aggiungere un riferimento a un progetto a questo oggetto. In questo modo, viene generata un'interfaccia con wrapper gestito intorno all'oggetto COM del caricamento bulk.  
  
> [!NOTE]  
>  Il caricamento bulk XML gestito non può essere eseguito con flussi gestiti e richiede un wrapper intorno ai flussi nativi. Il componente di caricamento bulk di SQLXML non viene eseguito in un ambiente a thread multipli (attributo '[MTAThread]'). Se si tenta di eseguire il componente di caricamento bulk in un ambiente multi-thread, si ottiene un'eccezione InvalidCastException con le informazioni aggiuntive seguenti: "QueryInterface per l'interfaccia SQLXMLBULKLOADLib.ISQLXMLBulkLoad non riuscita". La soluzione alternativa consiste nell'rendere accessibile l'oggetto che contiene l'oggetto caricamento bulk a thread singolo, ad esempio usando l'attributo **[STAThread],** come illustrato nell'esempio.  
  
 In questo argomento viene fornita un'applicazione C# di esempio reale per eseguire il caricamento bulk dei dati nel database. Per creare un esempio reale, eseguire la procedura seguente:  
  
1.  Creare le tabelle seguenti:  
  
    ```  
    CREATE TABLE Ord (  
             OrderID     int identity(1,1)  PRIMARY KEY,  
             CustomerID  varchar(5))  
    GO  
    CREATE TABLE Product (  
             ProductID   int identity(1,1) PRIMARY KEY,  
             ProductName varchar(20))  
    GO  
    CREATE TABLE OrderDetail (  
           OrderID     int FOREIGN KEY REFERENCES Ord(OrderID),  
           ProductID   int FOREIGN KEY REFERENCES Product(ProductID),  
                       CONSTRAINT OD_key PRIMARY KEY (OrderID, ProductID))  
    GO  
    ```  
  
2.  Salvare lo schema seguente in un file (schema.xml):  
  
    ```  
    <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
                xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
    <xsd:annotation>  
      <xsd:appinfo>  
        <sql:relationship name="OrderOD"  
              parent="Ord"  
              parent-key="OrderID"  
              child="OrderDetail"  
              child-key="OrderID" />  
  
        <sql:relationship name="ODProduct"  
              parent="OrderDetail"  
              parent-key="ProductID"  
              child="Product"  
              child-key="ProductID"   
              inverse="true"/>  
      </xsd:appinfo>  
    </xsd:annotation>  
  
      <xsd:element name="Order" sql:relation="Ord"   
                                sql:key-fields="OrderID" >  
       <xsd:complexType>  
         <xsd:sequence>  
            <xsd:element name="Product" sql:relation="Product"   
                         sql:key-fields="ProductID"  
                         sql:relationship="OrderOD ODProduct">  
              <xsd:complexType>  
                 <xsd:attribute name="ProductID" type="xsd:int" />  
                 <xsd:attribute name="ProductName" type="xsd:string" />  
              </xsd:complexType>  
            </xsd:element>  
         </xsd:sequence>  
            <xsd:attribute name="OrderID"   type="xsd:integer" />   
            <xsd:attribute name="CustomerID"   type="xsd:string" />  
        </xsd:complexType>  
      </xsd:element>  
    </xsd:schema>  
    ```  
  
3.  Salvare il documento XML di esempio seguente in un file (data.xml):  
  
    ```  
    <ROOT>    
      <Order OrderID="11" CustomerID="ALFKI">  
        <Product ProductID="11" ProductName="Chai" />  
        <Product ProductID="22" ProductName="Chang" />  
      </Order>  
      <Order OrderID="22" CustomerID="ANATR">  
         <Product ProductID="33" ProductName="Aniseed Syrup" />  
        <Product ProductID="44" ProductName="Gumbo Mix" />  
      </Order>  
    </ROOT>  
    ```  
  
4.  Avviare Visual Studio.  
  
5.  Creare un'applicazione console C#.  
  
6.  Scegliere **Aggiungi riferimento** dal menu **Progetto.**  
  
7.  Nella scheda **COM** selezionare Libreria dei tipi **Microsoft SQLXML Bulkload 4.0** (xblkld4.dll) e fare clic su **OK.** Verrà visualizzato **l'assembly Interop.SQLXMLBULKLOADLib** creato nel progetto.  
  
8.  Sostituire il metodo Main() con il codice seguente. Aggiornare la **proprietà ConnectionString** e il percorso del file dello schema e dei file di dati.  
  
    ```  
    [STAThread]  
       static void Main(string[] args)  
       {     
             try  
             {  
                SQLXMLBULKLOADLib.SQLXMLBulkLoad4Class objBL = new SQLXMLBULKLOADLib.SQLXMLBulkLoad4Class();  
                objBL.ConnectionString = "Provider=sqloledb;server=server;database=databaseName;integrated security=SSPI";  
                objBL.ErrorLogFile = "error.xml";  
                objBL.KeepIdentity = false;  
                objBL.Execute ("schema.xml","data.xml");  
             }  
             catch(Exception e)  
             {  
             Console.WriteLine(e.ToString());  
             }  
       }  
    ```  
  
9. Per caricare i dati XML nella tabella creata, compilare ed eseguire il progetto.  
  
    > [!NOTE]  
    >  Il riferimento al componente di caricamento bulk (xblkld4.dll) può essere aggiunto anche utilizzando lo strumento tlbimp.exe, disponibile come parte di .NET Framework. Questo strumento crea un wrapper gestito per la DLL nativa (xblkld4.dll) che può quindi essere utilizzato in qualsiasi progetto .NET. Ad esempio:  
  
    ```  
    c:\>tlbimp xblkld4.dll  
    ```  
  
     Questa operazione crea la DLL del wrapper gestito (SQLXMLBULKLOADLib.dll) che è possibile utilizzare nel progetto .NET Framework. In .NET Framework aggiungere il riferimento al progetto alla nuova DLL creata.  
  
## <a name="see-also"></a>Vedere anche  
 [Esecuzione del caricamento bulk di dati XML &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/performing-bulk-load-of-xml-data-sqlxml-4-0.md)  
  
  
