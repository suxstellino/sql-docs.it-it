---
title: Negare le autorizzazioni per una raccolta di XML Schema | Microsoft Docs
description: Informazioni su come negare l'autorizzazione per la creazione di una nuova raccolta XML Schema o per l'uso di una raccolta esistente.
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- denying permissions [SQL Server], XML server collections
ms.assetid: e2b300b0-e734-4c43-a4da-c78e6e5d4fba
author: rothja
ms.author: jroth
ms.openlocfilehash: 0e428e81f45376bd81dd957812da35e313829c39
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490966"
---
# <a name="deny-permissions-on-an-xml-schema-collection"></a>Negazione delle autorizzazioni per una raccolta di XML Schema
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  È possibile negare l'autorizzazione per la creazione di una nuova raccolta di XML Schema o per l'utilizzo di una raccolta esistente.  
  
## <a name="denying-permission-to-create-an-xml-schema-collection"></a>Negazione dell'autorizzazione per la creazione di una raccolta di XML Schema  
 È possibile negare l'autorizzazione per la creazione di una raccolta di XML Schema nei modi seguenti:  
  
-   Negando l'autorizzazione ALTER per lo schema relazionale.  
  
-   Negando l'autorizzazione CONTROL per lo schema relazionale in modo da negare tutte le autorizzazioni per lo schema relazionale e per tutti gli oggetti contenuti.  
  
-   Negando l'autorizzazione ALTER ANY SCHEMA per il database. In tal caso, l'entità non può creare una raccolta di XML Schema in una posizione all'interno del database. Si noti inoltre che tramite la negazione dell'autorizzazione ALTER o CONTROL vengono negate tutte le autorizzazioni per tutti gli oggetti del database.  
  
## <a name="denying-permissions-on-an-xml-schema-collection-object"></a>Negazione di autorizzazioni per un oggetto di una raccolta di XML Schema  
 Per una raccolta di XML Schema esistente e i relativi risultati è possibile negare le autorizzazioni seguenti:  
  
-   Tramite la negazione dell'autorizzazione ALTER, all'entità viene negata la possibilità di modificare il contenuto di una raccolta di XML Schema.  
  
-   Tramite la negazione dell'autorizzazione CONTROL, all'entità viene negata la possibilità di eseguire qualsiasi operazione sulla raccolta di XML Schema.  
  
-   Tramite la negazione dell'autorizzazione REFERENCES, all'entità viene negata la possibilità di tipizzare o vincolare parametri e colonne di tipo XML mediante la raccolta di XML Schema, nonché la possibilità di fare riferimento a tale raccolta da altre raccolte di XML Schema.  
  
-   Tramite la negazione dell'autorizzazione VIEW DEFINITION, all'entità viene negata la possibilità di visualizzare il contenuto di una raccolta di XML Schema.  
  
-   Tramite la negazione dell'autorizzazione EXECUTE, all'entità viene negata la possibilità di inserire o aggiornare i valori in colonne, variabili e parametri tipizzati o vincolati dalla raccolta di XML Schema, nonché la possibilità di eseguire query sui valori nelle stesse variabili e colonne di tipo XML.  
  
## <a name="examples"></a>Esempi  
 Gli scenari degli esempi seguenti illustrano il funzionamento delle autorizzazioni per XML Schema. In ogni esempio vengono creati il database di prova, gli schemi relazionali e gli account di accesso necessari. A tali account di accesso vengono concesse le autorizzazioni necessarie per la raccolta di XML Schema. Alla fine di ogni esempio viene eseguito il processo di pulizia necessario.  
  
### <a name="a-preventing-a-user-from-creating-an-xml-schema-collection"></a>R. Procedura per impedire a un utente di creare una raccolta di XML Schema  
 Uno dei modi per impedire a un utente di creare una raccolta di XML Schema consiste nel negare l'autorizzazione ALTER per uno schema relazionale, come illustrato nell'esempio seguente.  
  
 Nell'esempio vengono creati l'account utente `TestLogin1`e un database, nonché uno schema relazionale, in aggiunta allo schema `dbo` , nel database. Inizialmente, l'autorizzazione `CREATE XML SCHEMA` consente all'utente di creare una raccolta di schemi in una posizione qualsiasi all'interno del database. Nell'esempio viene negata all'utente l'autorizzazione `ALTER` per uno degli schemi relazionali. In questo modo, viene impedito all'utente di creare una raccolta di XML Schema in tale schema relazionale.  
  
```  
CREATE LOGIN TestLogin1 WITH password='SQLSvrPwd1'  
GO  
CREATE DATABASE SampleDBForSchemaPermissions  
GO  
USE SampleDBForSchemaPermissions  
GO  
-- Create another relational schema in the database.  
CREATE SCHEMA myOtherDBSchema  
GO  
CREATE USER TestLogin1  
GO  
-- For TestLogin1 to create/import XML schema collection, following  
-- permission needed.  
-- Database-level permissions  
GRANT CREATE XML SCHEMA COLLECTION TO TestLogin1  
GO  
GRANT ALTER ANY SCHEMA TO TestLogin1  
GO  
-- Now TestLogin1 can import an XML schema collection.  
SETUSER 'TestLogin1'  
GO  
CREATE XML SCHEMA COLLECTION myOtherDBSchema.myTestSchemaCollection AS '<?xml version="1.0" encoding="UTF-8" ?>  
<xsd:schema targetNamespace="https://schemas.adventure-works.com/Additional/ContactInfo"   
            xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
elementFormDefault="qualified">  
<xsd:element name="telephone" type="xsd:string" />  
</xsd:schema>'  
GO  
DROP XML SCHEMA COLLECTION myOtherDBSchema.myTestSchemaCollection  
GO  
-- Now deny permission from TestLogin1 to alter myOtherDBSchema.  
setuser  
GO  
DENY ALTER ON SCHEMA::myOtherDBSchema TO TestLogin1  
GO  
-- Now TestLogin1 cannot create xml schema collection.  
SETUSER 'TestLogin1'  
GO  
CREATE XML SCHEMA COLLECTION myOtherDBSchema.myTestSchemaCollection AS '<?xml version="1.0" encoding="UTF-8" ?>  
<xsd:schema targetNamespace="https://schemas.adventure-works.com/Additional/ContactInfo"   
            xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
elementFormDefault="qualified">  
<xsd:element name="telephone" type="xsd:string" />  
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
  
### <a name="b-denying-permissions-on-an-xml-schema-collection"></a>B. Procedura per negare autorizzazioni per una raccolta di XML Schema  
 Nell'esempio seguente viene illustrato come negare a un account di accesso un'autorizzazione specifica per una raccolta di XML Schema esistente. In questo esempio, a un account di accesso di prova viene negata l'autorizzazione REFERENCES per una raccolta di XML Schema esistente.  
  
 Nell'esempio vengono creati l'account utente `TestLogin1`e un database, nonché uno schema relazionale, in aggiunta allo schema `dbo` , nel database. Inizialmente, l'autorizzazione `CREATE XML SCHEMA` consente all'utente di creare una raccolta di schemi in una posizione qualsiasi all'interno del database.  
  
 L'autorizzazione `REFERENCES` per la raccolta di XML Schema consente a `TestLogin1` di utilizzare lo schema nella creazione di una colonna `xml` tipizzata in una tabella. Se viene negata l'autorizzazione `REFERENCES` per la raccolta di XML Schema, `TestLogin1` non potrà utilizzare tale raccolta.  
  
```  
CREATE LOGIN TestLogin1 WITH password='SQLSvrPwd1'  
GO  
CREATE DATABASE SampleDBForSchemaPermissions  
GO  
USE SampleDBForSchemaPermissions  
GO  
-- Create another relational schema in the database.  
CREATE SCHEMA myOtherDBSchema  
GO  
CREATE USER TestLogin1  
GO  
-- For TestLogin1 to create/import XML schema collection, the following  
-- permission is required.  
-- Database-level permissions  
GRANT CREATE XML SCHEMA COLLECTION TO TestLogin1  
GO  
GRANT ALTER ANY SCHEMA TO TestLogin1  
GO  
-- Now TestLogin1 can import an XML schema collection.  
SETUSER 'TestLogin1'  
GO  
CREATE XML SCHEMA COLLECTION myOtherDBSchema.myTestSchemaCollection AS '<?xml version="1.0" encoding="UTF-8" ?>  
<xsd:schema targetNamespace="https://schemas.adventure-works.com/Additional/ContactInfo"   
            xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
elementFormDefault="qualified">  
<xsd:element name="telephone" type="xsd:string" />  
</xsd:schema>'  
GO  
-- Grant permission to TestLogin1 to create a table and reference the XML schema collection.  
SETUSER  
GO  
GRANT CREATE TABLE TO TestLogin1  
GO  
-- The user also needs REFERENCES permission to use the XML schema collection  
-- to create a typed XML column (REFERENCES permission on the schema   
-- collection is not needed).  
GRANT REFERENCES ON XML SCHEMA COLLECTION::myOtherDBSchema.myTestSchemaCollection   
TO TestLogin1  
GO  
  
--TestLogin1 can use the schema.  
CREATE TABLE T(i int, x xml (myOtherDBSchema.myTestSchemaCollection))  
GO  
-- Drop the table.  
DROP TABLE T  
GO  
-- Now deny REFERENCES permission to TestLogin1 on the schema created previously.  
SETUSER  
GO  
DENY REFERENCES ON XML SCHEMA COLLECTION::myOtherDBSchema.myTestSchemaCollection TO TestLogin1  
  
GO  
-- Now TestLogin1 cannot create xml schema collection  
SETUSER 'TestLogin1'  
GO  
-- Following statement fails. TestLogin1 does not have REFERENCES   
-- permission on the XML schema collection.  
CREATE TABLE T(i int, x xml (myOtherDBSchema.myTestSchemaCollection))  
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
  
## <a name="see-also"></a>Vedere anche  
 [Confronto dati XML tipizzati con dati XML non tipizzati](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Raccolte di XML Schema &#40;SQL Server&#41;](../../relational-databases/xml/xml-schema-collections-sql-server.md)   
 [Requisiti e limitazioni per l'utilizzo di raccolte di XML Schema nel server](../../relational-databases/xml/requirements-and-limitations-for-xml-schema-collections-on-the-server.md)   
 [DENY - autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md)   
 [GRANT - autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)   
 [Dati XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-sql-server.md)  
  
  
