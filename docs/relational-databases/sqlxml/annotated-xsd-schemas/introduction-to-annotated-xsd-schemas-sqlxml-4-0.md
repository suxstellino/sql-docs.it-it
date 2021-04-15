---
title: Introduzione agli schemi XSD con annotazioni (SQLXML)
description: Informazioni sulla creazione di viste XML di dati relazionali usando il linguaggio XSD (XML Schema Definition) (SQLXML 4.0).
ms.date: 01/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- namespaces [SQLXML], annotated XSD schemas
- mapping schema [SQLXML], about mapping schema
- views [SQLXML]
- valid XSD schemas [SQLXML]
- annotations [SQLXML]
- XSD schemas [SQLXML], creating XML views
- annotated XSD schemas, creating XML views
- minimum XSD schema
- annotated XSD schemas, examples
- XML views [SQLXML]
ms.assetid: 15282db1-65c4-43be-bdb7-e9ef49cb33a2
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8c4dd2f6518a15b2b2022f2c30afa838ab484e44
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491428"
---
# <a name="introduction-to-annotated-xsd-schemas-sqlxml-40"></a>Introduzione agli schemi XSD con annotazioni (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  È possibile creare viste XML di dati relazionali mediante il linguaggio di definizione di XML Schema (XSD). In tali viste è possibile eseguire query utilizzando le query XPath (XML Path language). La procedura è simile a quella utilizzata per creare viste mediante le istruzioni CREATE VIEW e quindi specificare query SQL in tali viste.  
  
 Un elemento XML Schema descrive la struttura di un documento XML e i vari vincoli presenti sui dati del documento. Quando si specificano query XPath nello schema, la struttura del documento XML restituita è determinata dallo schema nel quale viene eseguita la query XPath.  
  
 In uno schema XSD l'elemento racchiude l'intero schema. Tutte le dichiarazioni di elemento **\<xsd:schema>** devono essere contenute all'interno dell'elemento. **\<xsd:schema>** È possibile descrivere gli attributi che definiscono lo spazio dei nomi in cui risiede lo schema e gli spazi dei nomi usati nello schema come proprietà **\<xsd:schema>** dell'elemento.  
  
 Uno schema XSD valido deve contenere **\<xsd:schema>** l'elemento definito come segue:  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<!-- additional schema definitions here -->  
</xsd:schema>  
```  
  
 **\<xsd:schema>** L'elemento deriva dalla specifica dello spazio dei nomi XML Schema in http://www.w3.org/2001/XMLSchema .  
  
## <a name="annotations-to-the-xsd-schema"></a>Annotazioni dello schema XSD  
 È possibile utilizzare uno schema XSD con annotazioni che descrivono il mapping a un database, eseguire query nel database e restituire i risultati nel formato di un documento XML. Le annotazioni vengono fornite per eseguire il mapping di uno schema XSD a colonne e tabelle di database. È possibile specificare le query XPath nella vista XML creata dallo schema XSD per eseguire query nel database e ottenere risultati in formato XML.  
  
> [!NOTE]  
>  In [!INCLUDE[msCoName](../../../includes/msconame-md.md)] SQLXML 4.0 il linguaggio dello schema XSD supporta le annotazioni introdotte con il linguaggio dello schema XDR (XML-Data Reduced) con annotazioni in [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)]. Lo schema XDR con annotazioni è deprecato in SQLXML 4.0.  
  
 Nel contesto del database relazionale risulta utile per eseguire il mapping dello schema XSD arbitrario a un archivio relazionale. Un modo per ottenere questo risultato è annotare lo schema XSD. Uno schema XSD con le annotazioni viene definito schema di *mapping*, che fornisce informazioni relative alla modalità di mapping dei dati XML all'archivio relazionale. Uno schema di mapping è, di fatto, una vista XML dei dati relazionali. I mapping possono essere utilizzati per recuperare dati relazionali come documento XML.  
  
## <a name="namespace-for-annotations"></a>Spazio dei nomi per le annotazioni  
 In uno schema XSD le annotazioni vengono specificate usando lo spazio dei nomi **urn:schemas-microsoft-com:mapping-schema**. Come illustrato nell'esempio seguente, il modo più semplice per specificare lo spazio dei nomi è specificarlo nel **\<xsd:schema>** tag .  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
...  
</xsd:schema>  
```  
  
 Il prefisso dello spazio dei nomi utilizzato è arbitrario. In questa documentazione il prefisso **sql** viene usato per indicare lo spazio dei nomi dell'annotazione e per distinguere le annotazioni in questo spazio dei nomi da quelle in altri spazi dei nomi.  
  
## <a name="example-of-an-annotated-xsd-schema"></a>Esempio di schema XSD con annotazioni  
 Nell'esempio seguente lo schema XSD è costituito da un **\<Person.Contact>** elemento . **\<Employee>** L'elemento ha un attributo **ContactID** e **\<FirstName>** gli elementi **\<LastName>** figlio:  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema">  
  <xsd:element name="Contact" >  
   <xsd:complexType>  
     <xsd:sequence>  
        <xsd:element name="FName"    
                     type="xsd:string" />   
        <xsd:element name="LName"  
                     type="xsd:string" />  
     </xsd:sequence>  
        <xsd:attribute name="ConID" type="xsd:integer" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 A questo schema XSD vengono aggiunte annotazioni per eseguire il mapping degli elementi e degli attributi alle colonne e alle tabelle di database:  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="Contact" sql:relation="Person.Contact" >  
   <xsd:complexType>  
     <xsd:sequence>  
        <xsd:element name="FName"  
                     sql:field="FirstName"   
                     type="xsd:string" />   
        <xsd:element name="LName"    
                     sql:field="LastName"    
                     type="xsd:string" />  
     </xsd:sequence>  
        <xsd:attribute name="ConID"   
                       sql:field="ContactID"   
                       type="xsd:integer" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 Nello schema di mapping l'elemento viene mappato alla tabella Person.Contact nel database AdventureWorks di esempio usando l'annotazione **\<Contact>** **sql:relation.** Gli attributi ConID, FName e LName vengono mappati alle colonne ContactID, FirstName e LastName della tabella Person.Contact usando le annotazioni **sql:field.**  
  
 Questo schema XSD con annotazioni fornisce la vista XML dei dati relazionali. In questa vista XML possono essere eseguite query mediante il linguaggio XPath. Una query XPath restituisce come risultato un documento XML anziché il set di righe restituito dalle query SQL.  
  
> [!NOTE]  
>  Nello schema di mapping la distinzione tra maiuscole e minuscole per i valori relazionali specificati (ad esempio il nome di tabella e il nome di colonna) dipende dall'eventuale utilizzo delle impostazioni delle regole di confronto con distinzione tra maiuscole e minuscole da parte di SQL Server. Per altre informazioni, vedere [Collation and Unicode Support](../../../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="other-resources"></a>Risorse aggiuntive  
 Ulteriori informazioni sul linguaggio di definizione di XML Schema (XSD), sul linguaggio XML Path (XPath) e su Extensible Stylesheet Language Transformations (XSLT) sono disponibili nei siti Web seguenti:  
  
-   XML Schema Part 0: Primer, W3C Recommendation (https://www.w3.org/TR/xmlschema-0/)  
  
-   XML Schema Part 1: Structures, W3C Recommendation (https://www.w3.org/TR/xmlschema-1/)  
  
-   XML Schema Part 2:Datatypes, W3C Recommendation (https://www.w3.org/TR/xmlschema-2/)  
  
-   XPath (XML Path Language) (https://www.w3.org/TR/xpath)  
  
-   Trasformazioni XSL (XSLT) (https://www.w3.org/TR/xslt)  
  
## <a name="see-also"></a>Vedere anche  
 [Considerazioni sulla sicurezza degli schemi &#40;sqlxml 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/annotated-schema-security-considerations-sqlxml-4-0.md)   
 [Gli schemi XDR &#40;deprecati in SQLXML 4.0&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/annotated-xdr-schemas-deprecated-in-sqlxml-4-0.md)  
  
  
