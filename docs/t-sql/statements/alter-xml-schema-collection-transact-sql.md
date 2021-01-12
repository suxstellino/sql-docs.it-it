---
description: ALTER XML SCHEMA COLLECTION (Transact-SQL)
title: ALTER XML SCHEMA COLLECTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_XML_SCHEMA_COLLECTION_TSQL
- ALTER XML SCHEMA COLLECTION
dev_langs:
- TSQL
helpviewer_keywords:
- schema collections [SQL Server], altering
- xml_schema_namespace function
- adding schema components
- ALTER XML SCHEMA COLLECTION statement
- XML schemas [SQL Server], adding
- XML schema collections [SQL Server], modifying
- schema collections [SQL Server], adding components
- XML schema collections [SQL Server], adding components
- importing schemas
- XML schema collections [SQL Server], altering
- schema collections [SQL Server], modifying
- multiple schema namespaces
ms.assetid: e311c425-742a-4b0d-b847-8b974bf66d53
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f10572e2ceec916d930d8efd9cd2f2402294c926
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098497"
---
# <a name="alter-xml-schema-collection-transact-sql"></a>ALTER XML SCHEMA COLLECTION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Aggiunge nuovi componenti di schema a una raccolta di XML Schema esistente.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
ALTER XML SCHEMA COLLECTION [ relational_schema. ]sql_identifier ADD 'Schema Component'  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *relational_schema*  
 Identifica il nome dello schema relazionale. Se viene omesso, viene utilizzato lo schema relazionale predefinito.  
  
 *sql_identifier*  
 Identificatore SQL per la raccolta di XML Schema.  
  
 **'** *Schema Component* **'**  
 Componente dello schema da inserire.  
  
## <a name="remarks"></a>Osservazioni  
 Utilizzare ALTER XML SCHEMA COLLECTION per aggiungere nuovi XML Schema i cui spazi dei nomi non sono già nella raccolta di XML Schema, oppure per aggiungere nuovi componenti a spazi dei nomi esistenti nella raccolta.  
  
 Nell'esempio seguente viene aggiunto un nuovo \<element> allo spazio dei nomi esistente `https://MySchema/test_xml_schema` nella raccolta `MyColl`.  
  
```sql  
-- First create an XML schema collection.  
CREATE XML SCHEMA COLLECTION MyColl AS '  
   <schema   
    xmlns="http://www.w3.org/2001/XMLSchema"   
    targetNamespace="https://MySchema/test_xml_schema">  
      <element name="root" type="string"/>   
  </schema>'  
-- Modify the collection.   
ALTER XML SCHEMA COLLECTION MyColl ADD '  
  <schema xmlns="http://www.w3.org/2001/XMLSchema"   
         targetNamespace="https://MySchema/test_xml_schema">   
     <element name="anotherElement" type="byte"/>   
 </schema>';  
```  
  
 `ALTER XML SCHEMA` aggiunge l'elemento specificato in `<anotherElement>` allo spazio dei nomi definito precedentemente `https://MySchema/test_xml_schema`.  
  
 Si noti che se alcuni dei componenti che si desidera aggiungere alla raccolta fanno riferimento a componenti che sono già nella stessa raccolta, è necessario utilizzare `<import namespace="referenced_component_namespace" />`. Tuttavia, non è consentito utilizzare lo spazio dei nomi dello schema corrente in `<xsd:import>` e pertanto i componenti dello spazio dei nomi di destinazione corrispondente allo spazio dei nomi dello schema corrente vengono importati automaticamente.  
  
 Per rimuovere raccolte, usare [DROP XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-xml-schema-collection-transact-sql.md).  
  
 Se la raccolta di schemi contiene già una carattere jolly di convalida lax o un elemento di tipo **xs:anyType**, l'aggiunta di una nuova dichiarazione di elemento globale, tipo o attributo alla raccolta di schemi comporterà una riconvalida di tutti i dati archiviati vincolati dalla raccolta di schemi.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per modificare XML SCHEMA COLLECTION è richiesta l'autorizzazione ALTER per la raccolta.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-creating-xml-schema-collection-in-the-database"></a>R. Creazione di una raccolta di XML Schema nel database  
 Nell'esempio corrente viene creata la raccolta di XML Schema `ManuInstructionsSchemaCollection`. La raccolta ha solo uno spazio dei nomi di schema.  
  
```sql  
-- Create a sample database in which to load the XML schema collection.  
CREATE DATABASE SampleDB;  
GO  
USE SampleDB;  
GO  
CREATE XML SCHEMA COLLECTION ManuInstructionsSchemaCollection AS  
N'<?xml version="1.0" encoding="UTF-16"?>  
<xsd:schema targetNamespace="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"   
   xmlns          ="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"   
   elementFormDefault="qualified"   
   attributeFormDefault="unqualified"  
   xmlns:xsd="http://www.w3.org/2001/XMLSchema" >  
  
    <xsd:complexType name="StepType" mixed="true" >  
        <xsd:choice  minOccurs="0" maxOccurs="unbounded" >   
            <xsd:element name="tool" type="xsd:string" />  
            <xsd:element name="material" type="xsd:string" />  
            <xsd:element name="blueprint" type="xsd:string" />  
            <xsd:element name="specs" type="xsd:string" />  
            <xsd:element name="diag" type="xsd:string" />  
        </xsd:choice>   
    </xsd:complexType>  
  
    <xsd:element  name="root">  
        <xsd:complexType mixed="true">  
            <xsd:sequence>  
                <xsd:element name="Location" minOccurs="1" maxOccurs="unbounded">  
                    <xsd:complexType mixed="true">  
                        <xsd:sequence>  
                            <xsd:element name="step" type="StepType" minOccurs="1" maxOccurs="unbounded" />  
                        </xsd:sequence>  
                        <xsd:attribute name="LocationID" type="xsd:integer" use="required"/>  
                        <xsd:attribute name="SetupHours" type="xsd:decimal" use="optional"/>  
                        <xsd:attribute name="MachineHours" type="xsd:decimal" use="optional"/>  
                        <xsd:attribute name="LaborHours" type="xsd:decimal" use="optional"/>  
                        <xsd:attribute name="LotSize" type="xsd:decimal" use="optional"/>  
                    </xsd:complexType>  
                </xsd:element>  
            </xsd:sequence>  
        </xsd:complexType>  
    </xsd:element>  
</xsd:schema>' ;  
GO  
-- Verify - list of collections in the database.  
SELECT *  
FROM sys.xml_schema_collections;  
-- Verify - list of namespaces in the database.  
SELECT name  
FROM sys.xml_schema_namespaces;  
  
-- Use it. Create a typed xml variable. Note the collection name   
-- that is specified.  
DECLARE @x xml (ManuInstructionsSchemaCollection);  
GO  
--Or create a typed xml column.  
CREATE TABLE T (  
        i int primary key,   
        x xml (ManuInstructionsSchemaCollection));  
GO  
-- Clean up.  
DROP TABLE T;  
GO  
DROP XML SCHEMA COLLECTION ManuInstructionsSchemaCollection;  
Go  
USE master;  
GO  
DROP DATABASE SampleDB;  
```  
  
 In alternativa, è possibile assegnare la raccolta di schemi a una variabile e specificare la variabile nell'istruzione `CREATE XML SCHEMA COLLECTION` nel modo descritto di seguito:  
  
```sql  
DECLARE @MySchemaCollection nvarchar(max);  
SET @MySchemaCollection  = N' copy the schema collection here';  
CREATE XML SCHEMA COLLECTION AS @MySchemaCollection;   
```  
  
 La variabile utilizzata nell'esempio è di tipo `nvarchar(max)`. È anche possibile usare una variabile di tipo **xml**, che viene convertita implicitamente in una stringa.  
  
 Per altre informazioni, vedere [Visualizzare una raccolta di XML Schema archiviata](../../relational-databases/xml/view-a-stored-xml-schema-collection.md).  
  
 È possibile archiviare le raccolte di schemi in una colonna di tipo **xml**. In questo caso, per creare una raccolta di XML Schema, eseguire la procedura seguente:  
  
1.  Recuperare la raccolta di schemi dalla colonna tramite un'istruzione SELECT e assegnarla a una variabile di tipo **xml** o **varchar**.  
  
2.  Specificare il nome della variabile nell'istruzione CREATE XML SCHEMA COLLECTION.  
  
 L'istruzione CREATE XML SCHEMA COLLECTION archivia solo i componenti dello schema riconosciuti da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Non tutti gli elementi contenuti in XML Schema vengono archiviati nel database. Pertanto, se si desidera una copia esatta della raccolta di XML Schema, è consigliabile salvare gli XML Schema in una colonna di database o in un'altra cartella nel computer.  
  
### <a name="b-specifying-multiple-schema-namespaces-in-a-schema-collection"></a>B. Specifica di spazi dei nomi relativi a più schemi in una raccolta di schemi  
 È possibile specificare più XML Schema quando si crea una raccolta di XML Schema. Ad esempio:  
  
```sql  
CREATE XML SCHEMA COLLECTION N'  
<xsd:schema>....</xsd:schema>  
<xsd:schema>...</xsd:schema>';  
```  
  
 Nell'esempio seguente viene creata la raccolta di XML Schema `ProductDescriptionSchemaCollection` che include spazi dei nomi relativi a due XML Schema.  
  
```sql  
CREATE XML SCHEMA COLLECTION ProductDescriptionSchemaCollection AS   
'<xsd:schema targetNamespace="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain"  
    xmlns="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain"   
    elementFormDefault="qualified"   
    xmlns:xsd="http://www.w3.org/2001/XMLSchema" >  
    <xsd:element name="Warranty"  >  
        <xsd:complexType>  
            <xsd:sequence>  
                <xsd:element name="WarrantyPeriod" type="xsd:string"  />  
                <xsd:element name="Description" type="xsd:string"  />  
            </xsd:sequence>  
        </xsd:complexType>  
    </xsd:element>  
</xsd:schema>  
 <xs:schema targetNamespace="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription"   
    xmlns="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription"   
    elementFormDefault="qualified"   
    xmlns:mstns="https://tempuri.org/XMLSchema.xsd"   
    xmlns:xs="http://www.w3.org/2001/XMLSchema"  
    xmlns:wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain" >  
    <xs:import   
namespace="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain" />  
    <xs:element name="ProductDescription" type="ProductDescription" />  
        <xs:complexType name="ProductDescription">  
            <xs:sequence>  
                <xs:element name="Summary" type="Summary" minOccurs="0" />  
            </xs:sequence>  
            <xs:attribute name="ProductModelID" type="xs:string" />  
            <xs:attribute name="ProductModelName" type="xs:string" />  
        </xs:complexType>  
        <xs:complexType name="Summary" mixed="true" >  
            <xs:sequence>  
                <xs:any processContents="skip" namespace="http://www.w3.org/1999/xhtml" minOccurs="0" maxOccurs="unbounded" />  
            </xs:sequence>  
        </xs:complexType>  
</xs:schema>'  
;  
GO   
-- Clean up  
DROP XML SCHEMA COLLECTION ProductDescriptionSchemaCollection;  
GO  
```  
  
### <a name="c-importing-a-schema-that-does-not-specify-a-target-namespace"></a>C. Importazione di uno schema che non consente di specificare uno spazio dei nomi di destinazione  
 Se uno schema che non contiene un attributo **targetNamespace** viene importato in una raccolta, i relativi componenti vengono associati allo spazio dei nomi di destinazione della stringa vuota come illustrato nell'esempio seguente. Si noti che la mancata associazione di uno o più schemi importati nella raccolta risulta nell'associazione di più componenti di schema (potenzialmente non correlati) allo spazio dei nomi predefinito della stringa vuota.  
  
```sql  
-- Create a collection that contains a schema with no target namespace.  
CREATE XML SCHEMA COLLECTION MySampleCollection AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  xmlns:ns="http://ns">  
<element name="e" type="dateTime"/>  
</schema>';  
GO  
-- query will return the names of all the collections that   
--contain a schema with no target namespace  
SELECT sys.xml_schema_collections.name   
FROM   sys.xml_schema_collections   
JOIN   sys.xml_schema_namespaces   
ON     sys.xml_schema_collections.xml_collection_id =   
       sys.xml_schema_namespaces.xml_collection_id   
WHERE  sys.xml_schema_namespaces.name='';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-xml-schema-collection-transact-sql.md)   
 [DROP XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-xml-schema-collection-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [Confronto dati XML tipizzati con dati XML non tipizzati](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Requisiti e limitazioni per le raccolte di XML Schema nel server](../../relational-databases/xml/requirements-and-limitations-for-xml-schema-collections-on-the-server.md)  
  
  
