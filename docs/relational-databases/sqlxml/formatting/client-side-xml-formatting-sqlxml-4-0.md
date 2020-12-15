---
title: Formattazione XML sul lato client (SQLXML)
description: Informazioni sulla formattazione XML sul lato client in SQLXML 4,0 usando la clausola FOR XML.
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- FOR XML clause, formatting
- middle tier XML formatting [SQLXML]
- client-side XML formatting
- client-side-xml attribute
ms.assetid: 9630a21d-a93b-4d3b-8a25-c4b32399f993
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9d13f5252fade9a05db1b9d31cc62f1ed9292aa4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97430106"
---
# <a name="client-side-xml-formatting-sqlxml-40"></a>Formattazione XML sul lato client (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Questo argomento fornisce informazioni sulla formattazione XML sul lato client. Per formattazione sul lato client si intende la formattazione di dati XML nel livello intermedio.  
  
> [!NOTE]  
>  In questo argomento vengono fornite informazioni aggiuntive sull'utilizzo della clausola FOR XML sul lato client, con cui si suppone che l'utente disponga già di una certa familiarità. Per ulteriori informazioni su FOR XML, vedere [costruzione di codice XML mediante for XML](../../../relational-databases/xml/for-xml-sql-server.md).  
  
 **Importante** Per utilizzare la funzionalità FOR XML sul lato client con il nuovo tipo di dati **XML** , i client devono utilizzare sempre il [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] provider di dati native client (SQLNCLI11) anziché il provider SQLOLEDB. SQLNCLI11 rappresenta la versione più recente del provider di SQL Server ed è in grado di riconoscere completamente i tipi di dati introdotti in [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]. Il comportamento per il lato client FOR XML con il provider SQLOLEDB considererà i tipi di dati **XML** come stringhe.  
  
## <a name="formatting-xml-documents-on-the-client-side"></a>Formattazione di documenti XML sul lato client  
 Quando un'applicazione client esegue la query  
  
```  
SELECT FirstName, LastName  
FROM   Person.Contact  
FOR XML RAW  
```  
  
 Solo la parte seguente della query viene inviata al server:  
  
```  
SELECT FirstName, LastName  
FROM   Person.Contact  
```  
  
 Il server esegue la query e restituisce un set di righe, che contiene FirstName e LastName, al client. Il livello intermedio applica quindi la trasformazione FOR XML al set di righe e restituisce la formattazione XML al client.  
  
 Analogamente, quando si esegue una query XPath il server restituisce il set di righe al client e la trasformazione FOR XML EXPLICIT viene applicata al set di righe nel client, generando la formattazione XML desiderata.  
  
 Nella tabella seguente vengono illustrate le modalità che è possibile specificare con la clausola FOR XML sul lato client.  
  
|Modalità FOR XML sul lato client|Commento|  
|-------------------------------|-------------|  
|RAW|Produce risultati identici se specificata in FOR XML sul lato client o sul lato server.|  
|NESTED|È simile alla modalità FOR XML AUTO sul lato server.|  
|EXPLICIT|È simile alla modalità FOR XML EXPLICIT sul lato server.|  
  
> [!NOTE]  
>  Se si specifica la modalità AUTO e si richiede la formattazione XML sul lato client, l'intera query viene inviata al server, ovvero la formattazione XML viene eseguita nel server. Benché ciò avvenga per motivi di praticità, si noti che la modalità NESTED restituisce nomi di tabella di base nel documento XML generato. È possibile che alcune delle applicazioni scritte richiedano nomi di tabella di base. È possibile, ad esempio, eseguire una stored procedure e caricare i dati risultanti in un set di dati (in [!INCLUDE[msCoName](../../../includes/msconame-md.md)] .NET Framework) e quindi generare in seguito un Diffgram per aggiornare i dati nelle tabelle. In tal caso, sarà necessario disporre delle informazioni sulla tabella di base e utilizzare la modalità NESTED.  
  
## <a name="benefits-of-client-side-xml-formatting"></a>Vantaggi della formattazione XML sul lato client  
 Gli elementi seguenti costituiscono alcuni dei vantaggi della formattazione XML nel client.  
  
### <a name="if-you-have-stored-procedures-on-the-server-that-return-a-single-rowset-you-can-request-client-side-for-xml-transformation-to-generate-an-xml"></a>Se nel server sono disponibili stored procedure che restituiscono un singolo set di righe, è possibile richiedere la trasformazione FOR XML sul lato client per generare un documento XML.  
 Si consideri, ad esempio, la stored procedure seguente. La procedura restituisce i nomi e i cognomi dei dipendenti inclusi nella tabella Person.Contact del database AdventureWorks.  
  
```  
IF EXISTS (SELECT name FROM sysobjects  
   WHERE name = 'GetContacts' AND type = 'P')  
   DROP PROCEDURE GetContacts  
GO  
CREATE PROCEDURE GetContacts  
AS  
    SELECT   FirstName, LastName  
    FROM     Person.Contact  
```  
  
 Il modello XML di esempio seguente esegue la stored procedure. La clausola FOR XML viene specificata dopo il nome della stored procedure.  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <sql:query client-side-xml="1">  
    EXEC GetContacts FOR XML NESTED  
  </sql:query>  
</ROOT>  
```  
  
 Poiché l'attributo **client-side-xml** è impostato su 1 (true) nel modello, il stored procedure viene eseguito sul server e il set di righe a due colonne restituito dal server viene trasformato in XML nel livello intermedio e restituito al client. Di seguito viene fornito solo un risultato parziale.  
  
```  
 <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <Person.Contact FirstName="Gustavo" LastName="Achong" />   
  <Person.Contact FirstName="Catherine" LastName="Abel" />  
</ROOT>  
```  
  
> [!NOTE]  
>  Quando si utilizzano le classi gestite SQLXML o del provider SQLXMLOLEDB, è possibile utilizzare la proprietà **ClientSideXML** per richiedere la formattazione XML sul lato client.  
  
### <a name="the-workload-is-more-balanced"></a>Il carico di lavoro risulta più bilanciato.  
 Poiché il client esegue la formattazione XML, il carico di lavoro viene bilanciato tra il server e il client, liberando il server per l'esecuzione di altre operazioni.  
  
## <a name="supporting-client-side-xml-formatting"></a>Supporto della formattazione XML sul lato client  
 Per supportare la funzionalità di formattazione XML sul lato client, SQLXML fornisce gli elementi seguenti:  
  
-   provider SQLXMLOLEDB  
  
-   classi gestite SQLXML  
  
-   Supporto avanzato per modelli XML  
  
-   Proprietà SqlXmlCommand. ClientSideXml  
  
     È possibile specificare formattazione sul lato client impostando su true questa proprietà per le classi gestite SQLXML.  
  
## <a name="enhanced-xml-template-support"></a>Supporto avanzato per modelli XML  
 A partire da [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] , il modello XML in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] è stato migliorato con l'aggiunta dell'attributo **client-side-xml** . Se questo attributo è impostato su true, i dati XML vengono formattati nel client. Si noti che questo attributo di modello è identico in funzionalità alla proprietà ClientSideXML specifica del provider SQLXMLOLEDB.  
  
> [!NOTE]  
>  Se si esegue un modello XML in un'applicazione ADO che utilizza il provider SQLXMLOLEDB e si specifica sia l'attributo **client-side-xml** nel modello che la proprietà ClientSideXML del provider, il valore specificato nel modello avrà la precedenza.  
  
## <a name="see-also"></a>Vedere anche  
 [Architettura della formattazione XML sul lato client e sul lato server &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml/formatting/architecture-of-client-side-and-server-side-xml-formatting-sqlxml-4-0.md)   
 [FOR XML &#40;SQL Server&#41;](../../../relational-databases/xml/for-xml-sql-server.md)   
 [Considerazioni sulla sicurezza per XML &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/for-xml-security-considerations-sqlxml-4-0.md)   
 [Supporto del tipo di dati XML in SQLXML 4,0](../../../relational-databases/sqlxml/xml-data-type-support-in-sqlxml-4-0.md)   
 [Classi gestite SQLXML](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/sqlxml-4-0-net-framework-support-managed-classes.md)   
 [Formattazione XML sul lato client e sul lato server &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml/formatting/client-side-vs-server-side-xml-formatting-sqlxml-4-0.md)   
 [Oggetto SqlXmlCommand &#40;classi gestite SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/sqlxml-managed-classes-sqlxmlcommand-object.md)   
 [Dati XML &#40;SQL Server&#41;](../../../relational-databases/xml/xml-data-sql-server.md)  
  
  
