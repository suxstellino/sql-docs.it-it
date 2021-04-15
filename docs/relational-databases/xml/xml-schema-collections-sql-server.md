---
title: Raccolte di XML Schema (SQL Server) | Microsoft Docs
description: Informazioni sul modo in cui la raccolta di XML Schema archivia gli XML Schema importati per convalidare le istanze XML e tipizzare i dati XML archiviati in un database di SQL Server.
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- XSD schemas [SQL Server]
- xml_schema_namespace function
- XML schema collections [SQL Server], about XML schema collections
- metadata [SQL Server], XML schema collections
- queries [XML in SQL Server], XML schema collections
- schema collections [SQL Server]
- schemas [SQL Server], XML
- XML [SQL Server], schema collections
- XML schema collections [SQL Server]
- schema collections [SQL Server], about XML schema collections
ms.assetid: 659d41aa-ccec-4554-804a-722a96ef25c2
author: rothja
ms.author: jroth
ms.openlocfilehash: 15173fe3de1f3f82c5e70d0e5d8b302e380016d9
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107486700"
---
# <a name="xml-schema-collections-sql-server"></a>Raccolte di XML Schema (SQL Server)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Come descritto nell'argomento [xml &#40;Transact-SQL&#41;](../../t-sql/xml/xml-transact-sql.md), SQL Server offre funzionalità per l'archiviazione nativa dei dati XML tramite il tipo di dati **xml**. È facoltativamente possibile associare schemi XSD a una variabile o colonna di tipo **xml** tramite una raccolta di XML Schema. Una raccolta di XML Schema archivia gli elementi XML Schema importati e può essere quindi utilizzata per eseguire le operazioni seguenti:  
  
-   Convalidare istanze XML.  
  
-   Tipizzare i dati XML a mano a mano che vengono archiviati nel database.  
  
 Si noti che una raccolta di XML Schema è un'entità di metadati analoga a una tabella in un database. Le raccolte di schemi XML possono essere create, modificate ed eliminate. Gli schemi specificati in un'istruzione [CREATE XML SCHEMA COLLECTION (Transact-SQL)](../../t-sql/statements/create-xml-schema-collection-transact-sql.md) vengono automaticamente importati nell'oggetto raccolta di XML Schema appena creato. È possibile importare altri schemi o componenti di schema in un oggetto raccolta esistente nel database, usando l'istruzione [ALTER XML SCHEMA COLLECTION (Transact-SQL)](../../t-sql/statements/alter-xml-schema-collection-transact-sql.md) .  
  
 Come descritto nell'argomento [Confronto dati XML tipizzati con dati XML non tipizzati](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md), i dati XML archiviati in una colonna o in una variabile a cui è associato uno schema sono detti dati XML **tipizzati** perché lo schema fornisce le informazioni sul tipo di dati necessarie per i dati dell'istanza. SQL Server utilizza tali informazioni sul tipo per ottimizzare l'archiviazione dei dati.  
  
 Lo schema viene inoltre utilizzato dal motore di elaborazione delle query per la verifica dei tipi, per l'ottimizzazione delle query e per la modifica dei dati.  
  
 Per i valori **xml** tipizzati, SQL Server usa la raccolta di XML Schema associata per convalidare l'istanza XML. Se l'istanza XML è conforme allo schema, il database consente di archiviarla nel sistema insieme alle relative informazioni sul tipo, in caso contrario la rifiuta.  
  
 Per recuperare la raccolta di schemi archiviata nel database è possibile utilizzare la funzione intrinseca XML_SCHEMA_NAMESPACE. Per altre informazioni, vedere [Visualizzare una raccolta di XML Schema archiviata](../../relational-databases/xml/view-a-stored-xml-schema-collection.md).  
  
 La raccolta di XML Schema può essere utilizzata anche per tipizzare variabili, parametri e colonne XML.  
  
##  <a name="ddl-for-managing-schema-collections"></a><a name="ddl"></a> Istruzioni DDL per la gestione di raccolte di XML Schema  
 È possibile creare raccolte di XML Schema nel database e associarle a variabili e colonne di tipo **xml** . Per gestire le raccolte di schemi nel database, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono disponibili le istruzioni DDL seguenti:  
  
-   [CREATE XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-xml-schema-collection-transact-sql.md) Importa i componenti di schema in un database.  
  
-   [ALTER XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-xml-schema-collection-transact-sql.md) Modifica i componenti di schema in una raccolta XML Schema esistente.  
  
-   [DROP XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-xml-schema-collection-transact-sql.md) Elimina l'intera raccolta XML Schema e tutti i relativi componenti.  
  
 Per utilizzare una raccolta XML Schema e i relativi schemi, è necessario innanzitutto creare la raccolta e gli schemi utilizzando l'istruzione CREATE XML SCHEMA COLLECTION. Dopo aver creato la raccolta di schemi è possibile creare le variabili e le colonne di tipo **xml** e associare a esse la raccolta di schemi. Si noti che dopo aver creato la raccolta, nei metadati verranno archiviati diversi componenti degli schemi. È inoltre possibile utilizzare l'istruzione ALTER XML SCHEMA COLLECTION per aggiungere altri componenti agli schemi o nuovi schemi alla raccolta.  
  
 Per eliminare la raccolta di schemi, utilizzare l'istruzione DROP XML SCHEMA COLLECTION, che consente di eliminare tutti gli schemi contenuti nella raccolta e di rimuovere l'oggetto raccolta. Si noti che prima di eliminare una raccolta di schemi è necessario soddisfare le condizioni descritte in [DROP XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-xml-schema-collection-transact-sql.md).  
  
##  <a name="understanding-schema-components"></a><a name="components"></a> Informazioni sui componenti dello schema  
 Quando si utilizza l'istruzione CREATE XML SCHEMA COLLECTION, vengono importati nel database diversi componenti dello schema, ad esempio elementi, attributi e definizioni di tipi dello schema. Se si utilizza l'istruzione DROP XML SCHEMA COLLECTION, verrà rimossa l'intera raccolta.  
  
 L'istruzione CREATE XML SCHEMA COLLECTION salva i componenti dello schema in diverse tabelle di sistema.  
  
 Si consideri ad esempio lo schema seguente:  
  
```xml
<?xml version="1.0"?>  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            targetNamespace="uri:Cust_Orders2"  
            xmlns="uri:Cust_Orders2" >  
  <xsd:attribute name="SomeAttribute" type="xsd:int" />  
  <xsd:complexType name="SomeType" />  
  <xsd:complexType name="OrderType" >  
    <xsd:sequence>  
      <xsd:element name="OrderDate" type="xsd:date" />  
      <xsd:element name="RequiredDate" type="xsd:date" />  
      <xsd:element name="ShippedDate" type="xsd:date" />  
    </xsd:sequence>  
    <xsd:attribute name="OrderID" type="xsd:ID" />  
    <xsd:attribute name="CustomerID"  />  
    <xsd:attribute name="EmployeeID"  />  
  </xsd:complexType>  
  <xsd:complexType name="CustomerType" >  
     <xsd:sequence>  
        <xsd:element name="Order" type="OrderType"  
                     maxOccurs="unbounded" />  
       </xsd:sequence>  
      <xsd:attribute name="CustomerID" type="xsd:string" />  
      <xsd:attribute name="OrderIDList" type="xsd:IDREFS" />  
  </xsd:complexType>  
  <xsd:element name="Customer" type="CustomerType" />  
</xsd:schema>  
```  
  
 Questo schema mostra i diversi tipi di componenti che possono essere archiviati nel database, tra cui `SomeAttribute`, `SomeType`, `OrderType`, `CustomerType`, `Customer`, `Order`, `CustomerID`, `OrderID`, `OrderDate`, `RequiredDate`e `ShippedDate`.  
  
### <a name="component-categories"></a>Categorie di componenti  
 I componenti dello schema archiviati nel database rientrano nelle categorie seguenti:  
  
-   ELEMENT  
  
-   ATTRIBUTE  
  
-   TYPE (per tipi semplici o complessi)  
  
-   ATTRIBUTEGROUP  
  
-   MODELGROUP  
  
 Ad esempio:  
  
-   **SomeAttribute** è un componente di tipo ATTRIBUTE.  
  
-   **SomeType**, **OrderType** e **CustomerType** sono componenti di tipo TYPE.  
  
-   **Customer** è un componente ELEMENT.  
  
 Quando si importa uno schema nel database, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non archivia direttamente lo schema, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] archivia invece i singoli componenti. Ciò significa che il tag \<Schema> non viene archiviato, ma vengono mantenuti i componenti definiti al suo interno. Non vengono mantenuti tutti gli elementi dello schema. Se il tag \<Schema> contiene attributi che specificano il comportamento predefinito dei relativi componenti, tali attributi vengono spostati nei componenti dello schema durante il processo di importazione, come illustrato nella tabella seguente.  
  
|Nome attributo|Comportamento|  
|--------------------|--------------|  
|**attributeFormDefault**|Attributo **form** applicato a tutte le dichiarazioni di attributo nello schema nelle quali non è già presente e dove il valore viene impostato sul valore dell'attributo **attributeFormDefault** .|  
|**elementFormDefault**|Attributo **form** applicato a tutte le dichiarazioni di elemento nello schema nelle quali non è già presente e dove il valore viene impostato sul valore dell'attributo **elementFormDefault** .|  
|**blockDefault**|Attributo **block** applicato a tutte le dichiarazioni di elemento e definizioni di tipo nelle quali non è già presente e dove il valore viene impostato sul valore dell'attributo **blockDefault** .|  
|**finalDefault**|Attributo **final** applicato a tutte le dichiarazioni di elemento e definizioni di tipo nelle quali non è già presente e dove il valore viene impostato sul valore dell'attributo **finalDefault** .|  
|**targetNamespace**|Le informazioni sui componenti appartenenti allo spazio dei nomi di destinazione vengono archiviate nei metadati.|  
| &nbsp; | &nbsp; |
  
##  <a name="permissions-on-an-xml-schema-collection"></a><a name="perms"></a> Autorizzazioni per una raccolta di XML Schema  
 È necessario disporre delle autorizzazioni necessarie per eseguire le operazioni seguenti:  
  
-   Creare o caricare la raccolta XML Schema.  
  
-   Modificare la raccolta XML Schema.  
  
-   Eliminare la raccolta XML Schema.  
  
-   Usare la raccolta XML Schema per colonne, variabili e parametri di tipo **xml** oppure nei vincoli di tabella o di colonna  
  
 Il modello di sicurezza di SQL Server consente l'autorizzazione CONTROL per tutti gli oggetti. L'utente che dispone di questa autorizzazione ottiene tutte le altre autorizzazioni per l'oggetto. Il proprietario dell'oggetto dispone anch'esso di tutte le autorizzazioni per l'oggetto.  
  
 Il proprietario e l'utente che dispongono dell'autorizzazione CONTROL per un oggetto possono concedere qualsiasi autorizzazione per l'oggetto specifico. Un utente che non è il proprietario e che non dispone dell'autorizzazione CONTROL può comunque concedere l'autorizzazione per un oggetto se è specificata WITH GRANT OPTION. Ad esempio, si supponga che Utente A disponga tramite WITH GRANT OPTION dell'autorizzazione REFERENCES per la raccolta XML Schema denominata S, ma di nessun'altra autorizzazione per la raccolta. Utente A può concedere a Utente B l'autorizzazione REFERENCES per la raccolta di schemi S.  
  
 Il modello di sicurezza consente inoltre le autorizzazioni per la creazione e l'utilizzo di raccolte XML Schema o per il trasferimento della proprietà da un utente a un altro. Negli argomenti seguenti vengono illustrate le autorizzazioni per le raccolte XML Schema.  
  
-   [Concedere autorizzazioni per una raccolta di XML Schema](../../relational-databases/xml/grant-permissions-on-an-xml-schema-collection.md)  
  
     Questo argomento presenta informazioni sulla concessione delle autorizzazioni per creare una raccolta XML Schema e delle autorizzazioni per un oggetto raccolta di schemi XML.  
  
-   [Revocare le autorizzazioni per una raccolta di XML Schema](../../relational-databases/xml/revoke-permissions-on-an-xml-schema-collection.md)  
  
     Questo argomento presenta informazioni sulla revoca delle autorizzazioni per impedire la creazione di una raccolta XML Schema e illustra la revoca delle autorizzazioni per un oggetto raccolta XML Schema.  
  
-   [Negare le autorizzazioni per una raccolta di XML Schema](../../relational-databases/xml/deny-permissions-on-an-xml-schema-collection.md)  
  
     In questo argomento vengono fornite informazioni sulla negazione delle autorizzazioni per creare una raccolta XML Schema e delle autorizzazioni per un oggetto raccolta XML Schema.  
  
##  <a name="getting-information-about-xml-schemas-and-schema-collections"></a><a name="info"></a> Acquisizione di Informazioni su XML Schema e Raccolte di schemi  
 Le raccolte di XML Schema sono enumerate nella vista del catalogo sys.xml_schema_collections. La raccolta di XML Schema "sys" è definita dal sistema e contiene gli spazi dei nomi predefiniti che è possibile utilizzare in tutte le raccolte di XML Schema definite dall'utente senza doverli caricare in modo esplicito. Tale elenco contiene gli spazi dei nomi per xml, xs, xsi, fn e xdt. Sono disponibili altre due viste del catalogo: sys.xml_schema_namespaces, che enumera tutti gli spazi dei nomi in ogni raccolta di XML Schema, e sys.xml_components, che enumera tutti i componenti degli elementi XML Schema presenti in ognuno.  
  
 La funzione predefinita **XML_SCHEMA_NAMESPACE**, *schemaName, XmlSchemacollectionName, namespace-uri* restituisce un'istanza del tipo di dati **xml**. Tale istanza contiene frammenti di XML Schema per gli schemi inclusi in una raccolta di XML Schema, ad eccezione degli elementi XML Schema predefiniti.  
  
 Per enumerare il contenuto di una raccolta di XML Schema è possibile:  
  
-   Scrivere query Transact-SQL sulle viste del catalogo appropriate per le raccolte di XML Schema.  
  
-   Usare la funzione predefinita **XML_SCHEMA_NAMESPACE()** . È possibile applicare i metodi per il tipo di dati **xml** all'output di questa funzione, ma non è possibile modificare gli elementi XML Schema sottostanti.  
  
 Queste tecniche di enumerazione sono illustrate negli esempi seguenti.  
  
### <a name="example-enumerate-the-xml-namespaces-in-an-xml-schema-collection"></a>Esempio: Enumerazione degli spazi dei nomi XML in una raccolta di XML Schema  
 Per la raccolta di XML Schema "myCollection" utilizzare la query seguente:  
  
```sql
SELECT XSN.name  
FROM    sys.xml_schema_collections XSC JOIN sys.xml_schema_namespaces XSN  
    ON (XSC.xml_collection_id = XSN.xml_collection_id)  
WHERE    XSC.name = 'myCollection'     
```  
  
### <a name="example-enumerate-the-contents-of-an-xml-schema-collection"></a>Esempio: Enumerazione del contenuto di una raccolta di XML Schema  
 L'istruzione seguente enumera il contenuto della raccolta di XML Schema "myCollection" nell'ambito dello schema relazionale dbo.  
  
```sql
SELECT XML_SCHEMA_NAMESPACE (N'dbo', N'myCollection')  
```  
  
 I singoli elementi XML Schema inclusi nella raccolta possono essere ottenuti come istanze del tipo di dati **xml** , specificando lo spazio dei nomi di destinazione come terzo argomento della funzione **XML_SCHEMA_NAMESPACE()** , come illustrato nell'esempio seguente.  
  
### <a name="example-output-a-specified-schema-from-an-xml-schema-collection"></a>Esempio: Restituzione di uno schema specifico da una raccolta di XML Schema  
 L'istruzione seguente restituisce l'elemento XML Schema con spazio dei nomi di destinazione _pretend_ https/\/www.microsoft.com/was-books dalla raccolta di XML Schema "myCollection" all'interno dello schema relazionale dbo.  
  
```sql
SELECT XML_SCHEMA_NAMESPACE (N'dbo', N'myCollection',   
N'https://www.microsoft.com/was-books')  
```  
  
### <a name="querying-xml-schemas"></a>Esecuzione di query su elementi XML Schema  
 Per eseguire query sugli elementi XML Schema caricati in raccolte di XML Schema è possibile:  
  
-   Scrivere query Transact-SQL sulle viste del catalogo per gli spazi dei nomi degli elementi XML Schema.  
  
-   Creare una tabella contenente una colonna con tipo di dati **xml** per archiviare gli elementi XML Schema e quindi caricarli nel sistema di tipi XML. Per eseguire query sulla colonna XML, è possibile usare i metodi per il tipo di dati **xml** . È inoltre possibile compilare un indice XML su questa colonna. Questo approccio richiede tuttavia che l'applicazione mantenga la consistenza tra gli elementi XML Schema archiviati nella colonna XML e il sistema di tipi XML. Se ad esempio si elimina lo spazio dei nomi di un elemento XML Schema dal sistema di tipi XML, per mantenere la consistenza sarà necessario eliminarlo anche dalla tabella.  
  
## <a name="see-also"></a>Vedere anche  
 [Visualizzare una raccolta di XML Schema archiviata](../../relational-databases/xml/view-a-stored-xml-schema-collection.md)   
 [Pre-elaborazione di uno schema per unire schemi inclusi](../../relational-databases/xml/preprocess-a-schema-to-merge-included-schemas.md)   
 [Requisiti e limitazioni per le raccolte di XML Schema nel server](../../relational-databases/xml/requirements-and-limitations-for-xml-schema-collections-on-the-server.md)  
  
  
