---
title: Concedere le autorizzazioni per una raccolta di XML Schema | Microsoft Docs
description: Informazioni su come concedere autorizzazioni per la creazione di una raccolta di XML Schema, nonché per un oggetto di una raccolta di XML Schema.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- granting permissions [SQL Server], XML schema collections
- ALTER permission
ms.assetid: ffbb829c-3b8f-4e5d-97d9-ab4059aab0db
author: rothja
ms.author: jroth
ms.openlocfilehash: ecc7905afada0fd53cec2a99630963e1189741a5
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107488116"
---
# <a name="grant-permissions-on-an-xml-schema-collection"></a>Concedere autorizzazioni per una raccolta di XML Schema
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  È possibile concedere autorizzazioni per la creazione di una raccolta di XML Schema, nonché per un oggetto di una raccolta di XML Schema.  
  
## <a name="granting-permission-to-create-an-xml-schema-collection"></a>Concessione dell'autorizzazione per la creazione di una raccolta di XML Schema  
 Per creare una raccolta di XML Schema, sono necessarie le autorizzazioni seguenti:  
  
-   Per l'entità è necessaria l'autorizzazione CREATE XML SCHEMA COLLECTION a livello di database.  
  
-   Poiché le raccolte di XML Schema rientrano nell'ambito dello schema relazionale, l'entità deve disporre inoltre dell'autorizzazione ALTER per lo schema relazionale.  
  
 Le autorizzazioni seguenti consentono a un'entità di creare una raccolta XML Schema in uno schema relazionale all'interno di un database in un server:  
  
-   Autorizzazione CONTROL nel server  
  
-   Autorizzazione ALTER ANY DATABASE nel server  
  
-   Autorizzazione ALTER per il database  
  
-   Autorizzazione CONTROL per il database  
  
-   Autorizzazioni ALTER ANY SCHEMA e CREATE XML SCHEMA COLLECTION per il database  
  
-   Autorizzazione ALTER o CONTROL nello schema relazionale e autorizzazione CREATE XML SCHEMA COLLECTION nel database  
  
 L'ultimo metodo di autorizzazione viene utilizzato nell'esempio seguente.  
  
 Il proprietario dello schema relazionale diventa il proprietario della raccolta di XML Schema creata in tale schema e dispone quindi del controllo completo sulla raccolta di XML Schema. Pertanto, questo proprietario può modificare la raccolta di XML Schema, tipizzare una colonna XML o eliminare la raccolta di XML Schema.  
  
## <a name="granting-permissions-on-an-xml-schema-collection-object"></a>Concessione di autorizzazioni per un oggetto della raccolta di XML Schema  
 Per la raccolta di XML Schema sono consentite le autorizzazioni seguenti:  
  
-   L'autorizzazione ALTER è necessaria per la modifica del contenuto di una raccolta di XML Schema esistente tramite l'istruzione ALTER XML SCHEMA COLLECTION.  
  
-   L'autorizzazione CONTROL consente a un utente di eseguire qualsiasi operazione sulla raccolta di XML Schema.  
  
-   L'autorizzazione TAKE OWNERSHIP è necessaria per trasferire la proprietà della raccolta di XML Schema da un'entità a un'altra.  
  
-   L'autorizzazione REFERENCES consente all'entità di utilizzare la raccolta di XML Schema per tipizzare o vincolare colonne di tipo **xml** in tabelle, viste e parametri. L'autorizzazione REFERENCES è necessaria inoltre quando una raccolta di XML Schema fa riferimento a un'altra.  
  
-   L'autorizzazione VIEW DEFINITION consente all'entità di eseguire query sul contenuto di una raccolta di XML Schema tramite XML_SCHEMA_NAMESPACE o mediante le viste del catalogo, a condizione che l'entità disponga inoltre di una delle autorizzazioni ALTER, REFERENCES o CONTROL per la raccolta.  
  
-   L'autorizzazione EXECUTE è necessaria per convalidare i valori inseriti o aggiornati dall'entità in base alla raccolta di XML Schema utilizzata per tipizzare o vincolare le colonne di tipo **xml** , le variabili e i parametri. Questa autorizzazione è necessaria inoltre quando si eseguono query sui dati XML archiviati in tali colonne e variabili.  
  
## <a name="examples"></a>Esempi  
 Gli scenari degli esempi seguenti illustrano il funzionamento delle autorizzazioni per XML Schema. In ogni esempio vengono creati il database di prova, gli schemi relazionali e gli account di accesso necessari. A tali account di accesso vengono concesse le autorizzazioni necessarie per la raccolta di XML Schema. Alla fine di ogni esempio viene eseguito il processo di eliminazione necessario.  
  
### <a name="a-granting-permissions-to-create-an-xml-schema-collection"></a>R. Concessione di autorizzazioni per la creazione di una raccolta di XML Schema  
 Nell'esempio seguente viene illustrato come concedere autorizzazioni per consentire a un'entità di creare una raccolta di XML Schema. Nell'esempio vengono creati un database di esempio e un utente di prova, `TestLogin1`. `TestLogin1` viene fornita quindi l'autorizzazione `ALTER` per lo schema relazionale e l'autorizzazione `CREATE XML SCHEMA COLLECTION` per il database. Con tali autorizzazioni, `TestLogin1` può creare una raccolta di XML Schema di esempio.  
  
```  
SETUSER  
GO  
USE master  
GO  
CREATE LOGIN TestLogin1 WITH password='SQLSvrPwd1'  
GO  
CREATE DATABASE SampleDBForSchemaPermissions  
GO  
USE SampleDBForSchemaPermissions  
GO  
CREATE USER TestLogin1  
GO  
-- User must have ALTER permission on the relational schema in the database.  
GRANT ALTER ON SCHEMA::dbo TO TestLogin1  
GO  
-- User also must have permission to create XML schema collections in the database.  
GRANT CREATE XML SCHEMA COLLECTION   
TO TestLogin1  
GO  
-- Execute CREATE XML SCHEMA COLLECTION.  
SETUSER 'TestLogin1'  
GO  
CREATE XML SCHEMA COLLECTION myTestSchemaCollection AS '<?xml version="1.0" encoding="UTF-8" ?>  
<xsd:schema targetNamespace="https://schemas.adventure-works.com/Additional/ContactInfo"   
            xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
elementFormDefault="qualified">  
<xsd:element name="AdditionalContactInfo" >  
  <xsd:complexType mixed="true" >  
    <xsd:sequence>  
      <xsd:any processContents="strict"    
               namespace="https://schemas.adventure-works.com/Contact/Record   
                          https://schemas.adventure-works.com/AdditionalContactTypes"  
               minOccurs="0" maxOccurs="unbounded" />  
    </xsd:sequence>  
  </xsd:complexType>  
</xsd:element>  
<xsd:element name="root" type="xsd:byte"/>  
</xsd:schema>'  
GO  
-- Final cleanup  
SETUSER  
GO  
USE master  
GO  
DROP DATABASE SampleDBForSchemaPermissions  
GO  
DROP LOGIN TestLogin1  
GO  
```  
  
### <a name="b-granting-permission-to-use-an-existing-xml-schema-collection"></a>B. Concessione dell'autorizzazione per l'utilizzo di una raccolta di XML Schema esistente  
 Nell'esempio seguente viene illustrato ulteriormente il modello di autorizzazione per la raccolta di XML Schema, con le diverse autorizzazioni necessarie per creare e utilizzare tale raccolta.  
  
 Nell'esempio vengono creati un database di prova e l'account di accesso `TestLogin1`. `TestLogin1` crea una raccolta di XML Schema nel database. L'account di accesso crea quindi una tabella e utilizza la raccolta di XML Schema per creare una colonna XML tipizzata. Successivamente, l'utente inserisce i dati ed esegue le relative query. Tutti questi passaggi richiedono le autorizzazioni necessarie per lo schema, come illustrato nel codice seguente.  
  
```  
SETUSER  
GO  
USE master  
GO  
CREATE LOGIN TestLogin1 WITH password='SQLSvrPwd1'  
GO  
CREATE DATABASE SampleDBForSchemaPermissions  
GO  
USE SampleDBForSchemaPermissions  
GO  
CREATE USER TestLogin1  
GO  
-- Grant permission to the user.  
SETUSER  
GO  
-- User must have ALTER permission on the relational schema in the database.  
GRANT ALTER ON SCHEMA::dbo TO TestLogin1  
GO  
-- User also must have permission to create XML schema collections in the database.  
GRANT CREATE XML SCHEMA COLLECTION   
TO TestLogin1  
GO  
-- Now user can execute the previous CREATE XML SCHEMA COLLECTION statement.  
SETUSER 'TestLogin1'  
GO  
CREATE XML SCHEMA COLLECTION myTestSchemaCollection AS '<?xml version="1.0" encoding="UTF-8" ?>  
<xsd:schema targetNamespace="https://schemas.adventure-works.com/Additional/ContactInfo"   
            xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
elementFormDefault="qualified">  
  
<xsd:element name="AdditionalContactInfo" >  
  <xsd:complexType mixed="true" >  
    <xsd:sequence>  
      <xsd:any processContents="strict"    
               namespace="https://schemas.adventure-works.com/Contact/Record   
                          https://schemas.adventure-works.com/AdditionalContactTypes"  
               minOccurs="0" maxOccurs="unbounded" />  
    </xsd:sequence>  
  </xsd:complexType>  
</xsd:element>  
<xsd:element name="telephone" type="xsd:string" />  
</xsd:schema>'  
GO  
  
-- Create a table by using the collection to type an XML column.   
--TestLogin1 must have permission to create a table.  
SETUSER  
GO  
GRANT CREATE TABLE TO TestLogin1  
GO  
-- The user also must have REFERENCES permission to use the XML schema collection  
-- to create a typed XML column (REFERENCES permission on schema   
-- collection is not needed).  
GRANT REFERENCES ON XML SCHEMA COLLECTION::myTestSchemaCollection   
TO TestLogin1  
GO  
-- Now user can create a table and use the XML schema collection to create   
-- a typed XML column.  
SETUSER 'TestLogin1'  
GO  
CREATE TABLE MyTestTable (xmlCol xml (dbo.myTestSchemaCollection))  
GO  
-- To insert data in the table, the user needs EXECUTE permission on the XML schema collection.  
-- GRANT EXECUTE permission to TestLogin2 on the xml schema collection.  
SETUSER  
GO  
GRANT EXECUTE ON XML SCHEMA COLLECTION::myTestSchemaCollection   
TO TestLogin1  
GO  
-- TestLogin1 does not own the dbo schema. This user must have INSERT permission.  
GRANT INSERT TO TestLogin1  
GO  
-- Now the user can insert data into the table.  
SETUSER 'TestLogin1'  
GO  
INSERT INTO MyTestTable VALUES('  
<telephone xmlns="https://schemas.adventure-works.com/Additional/ContactInfo">111-1111</telephone>  
')  
GO  
-- To query the table, TestLogin1 must have permissions: SELECT on the table and EXECUTE on the XML schema collection.  
SETUSER  
GO  
GRANT SELECT TO TestLogin1  
GO  
-- TestLogin1 already has EXECUTE permission on the schema (granted before inserting a record in the table).  
SELECT xmlCol.query('declare default element namespace "https://schemas.adventure-works.com/Additional/ContactInfo" /telephone[1]')  
FROM MyTestTable  
GO  
-- To show that the user must have EXECUTE permission to query, revoke the  
-- previously granted permission and return the query.  
SETUSER  
GO  
REVOKE EXECUTE ON XML SCHEMA COLLECTION::myTestSchemaCollection to TestLogin1  
Go  
-- Now TestLogin1 cannot execute the query.  
SETUSER 'TestLogin1'  
GO  
SELECT xmlCol.query('declare default element namespace "https://schemas.adventure-works.com/Additional/ContactInfo" /telephone[1]')  
FROM MyTestTable  
GO  
-- Final cleanup   
SETUSER  
GO  
USE master  
GO  
DROP DATABASE SampleDBForSchemaPermissions  
GO  
DROP LOGIN TestLogin1  
GO  
```  
  
### <a name="c-granting-alter-permission-on-an-xml-schema-collection"></a>C. Concessione dell'autorizzazione ALTER per una raccolta di XML Schema  
 Un utente deve disporre dell'autorizzazione ALTER per modificare una raccolta di XML Schema esistente nel database. Nell'esempio seguente viene illustrato come concedere l'autorizzazione `ALTER` .  
  
```  
SETUSER  
GO  
USE master  
GO  
CREATE LOGIN TestLogin1 WITH password='SQLSvrPwd1'  
GO  
CREATE DATABASE SampleDBForSchemaPermissions  
GO  
USE SampleDBForSchemaPermissions  
GO  
CREATE USER TestLogin1  
GO  
-- Grant permission to the user.  
SETUSER  
GO  
-- User must have ALTER permission on the relational schema in the database.  
GRANT ALTER ON SCHEMA::dbo TO TestLogin1  
GO  
-- User also must have permission to create XML schema collections in the database.  
GRANT CREATE XML SCHEMA COLLECTION   
TO TestLogin1  
GO  
-- Now user can execute the previous CREATE XML SCHEMA COLLECTION statement.  
SETUSER 'TestLogin1'  
GO  
CREATE XML SCHEMA COLLECTION myTestSchemaCollection AS '<?xml version="1.0" encoding="UTF-8" ?>  
<xsd:schema targetNamespace="https://schemas.adventure-works.com/Additional/ContactInfo"   
            xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
elementFormDefault="qualified">  
  
<xsd:element name="AdditionalContactInfo" >  
  <xsd:complexType mixed="true" >  
    <xsd:sequence>  
      <xsd:any processContents="strict"    
               namespace="https://schemas.adventure-works.com/Contact/Record   
                          https://schemas.adventure-works.com/AdditionalContactTypes"  
               minOccurs="0" maxOccurs="unbounded" />  
    </xsd:sequence>  
  </xsd:complexType>  
</xsd:element>  
<xsd:element name="telephone" type="xsd:string" />  
</xsd:schema>'  
GO  
-- Grant ALTER permission to TestLogin1.  
setuser  
GO  
GRANT ALTER ON XML SCHEMA COLLECTION::myTestSchemaCollection TO TestLogin1  
GO  
-- TestLogin1 should be able to add components to the collection.  
SETUSER 'TestLogin1'  
GO  
ALTER XML SCHEMA COLLECTION myTestSchemaCollection ADD '  
<xsd:schema targetNamespace="https://schemas.adventure-works.com/Additional/ContactInfo"   
            xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
            xmlns="https://schemas.adventure-works.com/Additional/ContactInfo"   
elementFormDefault="qualified">  
 <xsd:element name="pager" type="xsd:string"/>  
</xsd:schema>  
'  
Go  
-- Final cleanup   
SETUSER  
GO  
USE master  
GO  
DROP DATABASE SampleDBForSchemaPermissions  
GO  
DROP LOGIN TestLogin1  
GO  
```  
  
### <a name="d-granting-take-ownership-permission-on-an-xml-schema-collection"></a>D. Concessione dell'autorizzazione TAKE OWNERSHIP per una raccolta di XML Schema  
 Nell'esempio seguente viene illustrato il trasferimento della proprietà di XML Schema da un utente a un altro. Per rendere più interessante l'esempio, si supponga che gli utenti utilizzino schemi relazionali diversi.  
  
 In questo esempio vengono eseguite le operazioni seguenti:  
  
-   Viene creato un database con due schemi relazionali, `dbo` e `myOtherDBSchema`.  
  
-   Vengono creati due account utente, `TestLogin1` e `TestLogin2`. `TestLogin2` viene definito come proprietario dello schema relazionale `myOtherDBSchema` .  
  
-   `TestLogin1` crea una raccolta di XML Schema nello schema relazionale `dbo` .  
  
-   `TestLogin1` concede quindi l'autorizzazione `TAKE OWNERSHIP` per la raccolta di XML Schema a `TestLogin2`.  
  
-   `TestLogin2` diventa il proprietario della raccolta di XML Schema in `myOtherDBSchema`senza modificarne lo schema relazionale.  
  
```  
CREATE LOGIN TestLogin1 with password='SQLSvrPwd1'  
GO  
CREATE LOGIN TestLogin2 with password='SQLSvrPwd2'  
GO  
CREATE DATABASE SampleDBForSchemaPermissions  
GO  
USE SampleDBForSchemaPermissions  
GO  
-- Create another relational schema in the database.  
CREATE SCHEMA myOtherDBSchema  
GO  
-- Create users in the database. Note TestLogin2's default schema is  
-- myOtherDBSchema.  
CREATE USER TestLogin1  
GO  
CREATE USER TestLogin2 WITH DEFAULT_SCHEMA=myOtherDBSchema  
GO  
-- TestLogin2 will own myOtherDBSchema relational schema.  
ALTER AUTHORIZATION ON SCHEMA::myOtherDBSchema TO TestLogin2  
GO  
  
-- For TestLogin1 to create XML schema collection, the following  
-- permission is required.  
GRANT CREATE XML SCHEMA COLLECTION   
TO TestLogin1  
GO  
GRANT ALTER ON SCHEMA::dbo TO TestLogin1  
GO  
-- Now TestLogin1 can create an XML schema collection.  
setuser 'TestLogin1'  
GO  
CREATE XML SCHEMA COLLECTION myTestSchemaCollection AS '<?xml version="1.0" encoding="UTF-8" ?>  
<xsd:schema targetNamespace="https://schemas.adventure-works.com/Additional/ContactInfo"   
            xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
elementFormDefault="qualified">  
  
<xsd:element name="AdditionalContactInfo" >  
 <xsd:complexType mixed="true" >  
    <xsd:sequence>  
      <xsd:any processContents="strict"   
               namespace="https://schemas.adventure-works.com/Contact/Record   
                          https://schemas.adventure-works.com/AdditionalContactTypes"  
               minOccurs="0" maxOccurs="unbounded" />  
    </xsd:sequence>  
 </xsd:complexType>  
</xsd:element>  
<xsd:element name="telephone" type="xsd:string" />  
</xsd:schema>'  
GO  
  
-- Grant TAKE OWNERSHIP to TestLogin2.  
SETUSER  
GO  
GRANT TAKE OWNERSHIP ON XML SCHEMA COLLECTION::dbo.myTestSchemaCollection   
TO TestLogin2  
GO  
-- Verify the owner. Note the UserName and Principal_id is null.   
SELECT user_name(sys.xml_schema_collections.principal_id) as UserName,   
       sys.schemas.name as RelSchemaName,*   
FROM   sys.xml_schema_collections   
      JOIN sys.schemas   
      ON sys.schemas.schema_id=sys.xml_schema_collections.schema_id  
GO  
-- TestLogin2 can take ownership now.  
setuser 'TestLogin2'  
GO  
ALTER AUTHORIZATION ON XML SCHEMA COLLECTION::dbo.myTestSchemaCollection   
TO TestLogin2  
GO  
-- Note that although TestLogin2 is the owner,the XML schema collection   
-- is still in dbo.  
SELECT user_name(sys.xml_schema_collections.principal_id) as UserName,   
      sys.schemas.name as RelSchemaName,*   
FROM sys.xml_schema_collections JOIN sys.schemas   
     ON sys.schemas.schema_id=sys.xml_schema_collections.schema_id  
GO  
  
-- TestLogin2 moves the collection from dbo to myOtherDBSchema relational schema.  
-- TestLogin2 already has all necessary permissions.  
-- 1) TestLogin2 owns the destination relational schema so he can alter it.  
-- 2) TestLogin2 owns the XML schema collection (therefore, has CONTROL permission).  
ALTER SCHEMA myOtherDBSchema  
TRANSFER XML SCHEMA COLLECTION::dbo.myTestSchemaCollection  
GO  
  
SELECT user_name(sys.xml_schema_collections.principal_id) as UserName,   
       sys.schemas.name as RelSchemaName,*   
FROM   sys.xml_schema_collections JOIN sys.schemas   
       ON sys.schemas.schema_id=sys.xml_schema_collections.schema_id  
GO  
-- Final cleanup   
SETUSER  
GO  
USE master  
GO  
DROP DATABASE SampleDBForSchemaPermissions  
GO  
DROP LOGIN TestLogin1  
DROP LOGIN TestLogin2  
go   
```  
  
### <a name="e-granting-view-definition-permission-on-an-xml-schema-collection"></a>E. Concessione dell'autorizzazione VIEW DEFINITION per una raccolta di XML Schema  
 Nell'esempio seguente viene illustrato come concedere autorizzazioni VIEW DEFINITION per una raccolta di XML Schema.  
  
```  
SETUSER  
GO  
USE master  
GO  
IF EXISTS( SELECT * FROM sysdatabases WHERE name='permissionsDB' )  
   DROP DATABASE permissionsDB  
GO  
IF EXISTS( SELECT * FROM sys.sql_logins WHERE name='schemaUser' )  
   DROP LOGIN schemaUser  
GO  
CREATE DATABASE permissionsDB  
GO  
CREATE LOGIN schemaUser WITH PASSWORD='Pass#123',DEFAULT_DATABASE=permissionsDB  
GO  
GRANT CONNECT SQL TO schemaUser  
GO  
USE permissionsDB  
GO  
CREATE USER schemaUser WITH DEFAULT_SCHEMA=dbo  
GO  
CREATE XML SCHEMA COLLECTION MySC AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema" targetNamespace="http://ns"  
xmlns:ns="http://ns">  
  
   <simpleType name="ListOfIntegers">  
      <list itemType="integer"/>  
   </simpleType>  
  
   <element name="root" type="ns:ListOfIntegers"/>  
  
   <element name="gRoot" type="gMonth"/>  
  
</schema>  
'  
GO  
-- schemaUser cannot see the contents of the collection.  
SETUSER 'schemaUser'  
GO  
SELECT XML_SCHEMA_NAMESPACE(N'dbo',N'MySC')  
GO  
  
-- Grant schemaUser VIEW DEFINITION and REFERENCES permissions  
-- on the XML schema collection.  
SETUSER  
GO  
GRANT VIEW DEFINITION ON XML SCHEMA COLLECTION::dbo.MySC TO schemaUser  
GO  
GRANT REFERENCES ON XML SCHEMA COLLECTION::dbo.MySC TO schemaUser  
GO  
-- Now schemaUser can see the content of the collection.  
SETUSER 'schemaUser'  
GO  
SELECT XML_SCHEMA_NAMESPACE(N'dbo',N'MySC')  
GO  
-- Revoke schemaUser VIEW DEFINITION permissions  
-- on the XML schema collection.  
SETUSER  
GO  
REVOKE VIEW DEFINITION ON XML SCHEMA COLLECTION::dbo.MySC FROM schemaUser  
GO  
-- Now schemaUser cannot see the contents of   
-- the collection.  
setuser 'schemaUser'  
GO  
SELECT XML_SCHEMA_NAMESPACE(N'dbo',N'MySC')  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Dati XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-sql-server.md)   
 [Confronto dati XML tipizzati con dati XML non tipizzati](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Raccolte di XML Schema &#40;SQL Server&#41;](../../relational-databases/xml/xml-schema-collections-sql-server.md)   
 [Requisiti e limitazioni per le raccolte di XML Schema nel server](../../relational-databases/xml/requirements-and-limitations-for-xml-schema-collections-on-the-server.md)  
  
  
