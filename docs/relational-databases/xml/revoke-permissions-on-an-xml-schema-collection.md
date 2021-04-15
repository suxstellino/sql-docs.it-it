---
title: Revocare le autorizzazioni per una raccolta di XML Schema | Microsoft Docs
description: Informazioni sulla revoca delle autorizzazioni per una raccolta di XML Schema.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- revoking permissions [SQL Server]
ms.assetid: 4e542b70-2d56-4a65-8a39-96a1ed477ca6
author: rothja
ms.author: jroth
ms.openlocfilehash: a6be58ed505e5816c0c7dc3dfeaf589c3248e21c
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107487542"
---
# <a name="revoke-permissions-on-an-xml-schema-collection"></a>Revoca delle autorizzazioni per una raccolta di XML Schema
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Per revocare l'autorizzazione per creare una raccolta XML Schema, è possibile eseguire una delle operazioni seguenti:  
  
-   Revocare l'autorizzazione ALTER per lo schema relazionale. L'entità non sarà quindi in grado di creare una raccolta XML Schema nello schema relazionale, ma potrà comunque eseguire questa operazione negli altri schemi relazionali dello stesso database.  
  
-   Revocare l'autorizzazione ALTER ANY SCHEMA dell'entità per il database. L'entità non sarà quindi in grado di creare una raccolta XML Schema all'interno del database.  
  
-   Revocare l'autorizzazione CREATE XML SCHEMA COLLECTION o ALTER XML SCHEMA COLLECTION dell'entità per il database. Ciò impedisce all'entità di importare una raccolta XML Schema nel database. La revoca dell'autorizzazione ALTER o CONTROL per il database produce gli stessi effetti.  
  
## <a name="revoking-permissions-on-an-existing-xml-schema-collection-object"></a>Revoca delle autorizzazioni per un oggetto raccolta XML Schema esistente  
 Di seguito sono riportate le autorizzazioni che è possibile revocare per una raccolta XML Schema e i relativi risultati:  
  
-   La revoca dell'autorizzazione ALTER impedisce all'entità di modificare il contenuto della raccolta XML Schema.  
  
-   La revoca dell'autorizzazione TAKE OWNERSHIP impedisce all'entità di trasferire la proprietà della raccolta XML Schema.  
  
-   La revoca dell'autorizzazione REFERENCES impedisce all'entità di utilizzare la raccolta XML Schema per tipizzare o vincolare le colonne di tipo xml di tabelle e viste nonché i parametri. Comporta inoltre la revoca dell'autorizzazione che consente di fare riferimento alla raccolta di schemi da altre raccolte XML Schema.  
  
-   La revoca dell'autorizzazione VIEW DEFINITION impedisce all'entità di visualizzare il contenuto della raccolta XML Schema.  
  
-   La revoca dell'autorizzazione EXECUTE impedisce all'entità di inserire o aggiornare valori nelle colonne, variabili e parametri tipizzati o vincolati dalla raccolta XML. Comporta inoltre la revoca della possibilità di eseguire query su tali colonne, variabili o parametri di tipo **xml** .  
  
## <a name="examples"></a>Esempi  
 Gli scenari degli esempi seguenti illustrano il funzionamento delle autorizzazioni per XML Schema. In ogni esempio vengono creati il database di prova, gli schemi relazionali e gli account di accesso necessari. A tali account di accesso vengono concesse le autorizzazioni necessarie per la raccolta di XML Schema. Alla fine di ogni esempio viene eseguito il processo di pulizia necessario.  
  
### <a name="a-revoking-permissions-to-create-an-xml-schema-collection"></a>R. Revoca delle autorizzazioni per creare una raccolta XML Schema  
 In questo esempio vengono creati un account di accesso e un database di esempio e nel database viene inoltre aggiunto uno schema relazionale. Innanzitutto, all'account di accesso vengono concesse l'autorizzazione ALTER per entrambi gli schemi relazionali e le altre autorizzazioni necessarie per creare raccolte XML Schema. Viene quindi revocata l'autorizzazione ALTER per uno degli schemi relazionali del database, in modo tale da impedire all'account di accesso di creare una raccolta XML Schema.  
  
```  
setuser  
go  
create login TestLogin1 with password='SQLSvrPwd1'  
go  
create database SampleDBForSchemaPermissions  
go  
use SampleDBForSchemaPermissions  
go  
-- Create another relational schema in the db (in addition to dbo schema)  
CREATE SCHEMA myOtherDBSchema  
go  
CREATE USER TestLogin1  
go  
-- For TestLogin1 to create/import XML schema collection, following  
-- permission needed  
-- CREATE XML SCHEMA is a database level permission  
GRANT CREATE XML SCHEMA COLLECTION TO TestLogin1  
go  
GRANT ALTER ON SCHEMA::myOtherDBSchema TO TestLogin1  
go  
GRANT ALTER ON SCHEMA::dbo TO TestLogin1  
go  
-- Now TestLogin1 can import an XML schema collection in both relational schemas.  
setuser 'TestLogin1'  
go  
CREATE XML SCHEMA COLLECTION dbo.myTestSchemaCollection AS '<?xml version="1.0" encoding="UTF-8" ?>  
  
<xsd:schema targetNamespace="https://schemas.adventure-works.com/Additional/ContactInfo"   
            xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
elementFormDefault="qualified">  
<xsd:element name="telephone" type="xsd:string" />  
</xsd:schema>'  
go  
-- TestLogin1 can create XML schema collection in myOtherDBSchema relational schema  
CREATE XML SCHEMA COLLECTION myOtherDBSchema.myTestSchemaCollection AS '<?xml version="1.0" encoding="UTF-8" ?>  
  
<xsd:schema targetNamespace="https://schemas.adventure-works.com/Additional/ContactInfo"   
            xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
elementFormDefault="qualified">  
<xsd:element name="telephone" type="xsd:string" />  
</xsd:schema>'  
go  
-- Let us drop XML schema collections from both relational schemas  
DROP XML SCHEMA COLLECTION myOtherDBSchema.myTestSchemaCollection  
go  
DROP XML SCHEMA COLLECTION dbo.myTestSchemaCollection  
go  
-- now REVOKE permission from TestLogin1 to alter myOtherDBSchema  
setuser  
go  
REVOKE ALTER ON SCHEMA::myOtherDBSchema FROM TestLogin1  
go  
-- now TestLogin1 cannot create xml schema collection in myOtherDBSchema  
setuser 'TestLogin1'  
go  
CREATE XML SCHEMA COLLECTION myOtherDBSchema.myTestSchemaCollection AS '<?xml version="1.0" encoding="UTF-8" ?>  
<xsd:schema targetNamespace="https://schemas.adventure-works.com/Additional/ContactInfo"   
            xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
elementFormDefault="qualified">  
<xsd:element name="telephone" type="xsd:string" />  
</xsd:schema>'  
go  
  
-- TestLogin1 can still create XML schema collections in dbo  
-- It cannot create XML schema collections anywhere in the database  
-- if we REVOKE CREATE XML SCHEMA COLLECTION permission  
SETUSER  
go  
REVOKE CREATE XML SCHEMA COLLECTION FROM TestLogin1  
go  
  
setuser 'TestLogin1'  
go  
-- the following now should fail  
CREATE XML SCHEMA COLLECTION dbo.myTestSchemaCollection AS '<?xml version="1.0" encoding="UTF-8" ?>  
<xsd:schema targetNamespace="https://schemas.adventure-works.com/Additional/ContactInfo"   
            xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
elementFormDefault="qualified">  
<xsd:element name="telephone" type="xsd:string" />  
</xsd:schema>'  
go  
  
-- Final cleanup  
SETUSER  
go  
USE master  
go  
DROP DATABASE SampleDBForSchemaPermissions  
go  
DROP LOGIN TestLogin1  
Go  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Dati XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-sql-server.md)   
 [Confronto dati XML tipizzati con dati XML non tipizzati](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Raccolte di XML Schema &#40;SQL Server&#41;](../../relational-databases/xml/xml-schema-collections-sql-server.md)   
 [Requisiti e limitazioni per le raccolte di XML Schema nel server](../../relational-databases/xml/requirements-and-limitations-for-xml-schema-collections-on-the-server.md)  
  
  
