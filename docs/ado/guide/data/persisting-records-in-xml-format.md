---
description: Persistenza di record in formato XML
title: Salvataggio permanente di record in formato XML | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- persisting data [ADO]
- data updates [ADO], persisting data
- data persistence [ADO]
- XML persistence [ADO]
- updating data [ADO], persisting data
ms.assetid: f3113ec4-ae31-428f-89c6-bc1024f128ea
author: rothja
ms.author: jroth
ms.openlocfilehash: 6f29f624fe9f55287dcd8944fd04da0426d9552c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032623"
---
# <a name="persisting-records-in-xml-format"></a>Persistenza di record in formato XML
Analogamente al formato ADTG, la persistenza del **Recordset** in formato XML viene implementata con il provider di persistenza di Microsoft OLE DB. Questo provider genera un set di righe di sola lettura e di sola lettura da un file o un flusso XML salvato che contiene le informazioni sullo schema generate da ADO. Analogamente, può assumere un **Recordset** ADO, generare codice XML e salvarlo in un file o in qualsiasi oggetto che implementi l'interfaccia **IStream** com. In realtà, un file è semplicemente un altro esempio di oggetto che supporta **IStream**. Per le versioni 2,5 e successive, ADO si basa su Microsoft XML Parser (MSXML) per caricare il codice XML nel **Recordset**; pertanto msxml.dll è obbligatorio.  
  
> [!NOTE]
>  Alcune limitazioni si applicano quando si salvano **Recordset** gerarchici (forme dati) in formato XML. Non è possibile salvare in XML se il **Recordset** gerarchico contiene aggiornamenti in sospeso e non è possibile salvare un **Recordset** gerarchico con parametri. Per ulteriori informazioni, vedere la pagina relativa alla [permanenza dei recordset filtrati e gerarchici](../../../ado/guide/data/persisting-filtered-and-hierarchical-recordsets.md).  
  
 Il modo più semplice per salvare in modo permanente i dati in XML e caricarlo di nuovo tramite ADO è con i metodi **Save** e **Open** , rispettivamente. Nell'esempio di codice ADO seguente viene illustrato il salvataggio dei dati nella tabella **titles** in un file denominato titles. sav.  
  
```  
Dim rs as new Recordset  
Dim rs2 as new Recordset  
Dim c as new Connection  
Dim s as new Stream  
  
' Query the Titles table.  
c.Open "provider=sqloledb;data source=MySQLServer;initial catalog=pubs;Integrated Security='SSPI'"  
rs.cursorlocation = adUseClient  
rs.Open "select * from titles", c, adOpenStatic  
  
' Save to the file in the XML format. Note that if you don't specify   
' adPersistXML, a binary format (ADTG) will be used by default.  
rs.Save "titles.sav", adPersistXML  
  
' Save the recordset into the ADO Stream object.  
rs.save s, adPersistXML  
rs.Close  
c.Close  
  
set rs = nothing  
  
' Reopen the file.  
rs.Open "titles.sav",,,,adCmdFile  
' Open the Stream back into a Recordset.  
rs2.open s  
```  
  
 ADO rende sempre permanente l'intero oggetto **Recordset** . Se si desidera rendere permanente un subset di righe dell'oggetto **Recordset** , utilizzare il metodo **Filter** per limitare le righe o modificare la clausola di selezione. Tuttavia, è necessario aprire un oggetto **Recordset** con un cursore sul lato client (**CursorLocation**  =  **adUseClient**) per utilizzare il metodo **Filter** per il salvataggio di un subset di righe. Ad esempio, per recuperare i titoli che iniziano con la lettera "b", è possibile applicare un filtro a un oggetto **Recordset** aperto:  
  
```  
rs.Filter "title_id like 'B*'"  
rs.Save "btitles.sav", adPersistXML  
```  
  
 ADO utilizza sempre il set di righe del motore di cursore client per produrre un oggetto **Recordset** scorrevole e con segnalibro sopra i dati di sola trasmissione generati dal provider di persistenza.  
  
 In questa sezione vengono trattati gli argomenti seguenti.  
  
-   [Formato di persistenza XML](../../../ado/guide/data/xml-persistence-format.md)  
  
-   [Namespaces](../../../ado/guide/data/namespaces.md) (Spazi dei nomi)  
  
-   [Sezione dello schema](../../../ado/guide/data/schema-section.md)  
  
-   [Sezione di dati](../../../ado/guide/data/data-section.md)  
  
-   [Recordset gerarchici in XML](../../../ado/guide/data/hierarchical-recordsets-in-xml.md)  
  
-   [Proprietà dinamiche dei recordset in XML](../../../ado/guide/data/recordset-dynamic-properties-in-xml.md)  
  
-   [Trasformazioni XSLT](../../../ado/guide/data/xslt-transformations.md)  
  
-   [Salvataggio nell'oggetto DOM XML](../../../ado/guide/data/saving-to-the-xml-dom-object.md)  
  
-   [Considerazioni sulla sicurezza per XML](../../../ado/guide/data/xml-security-considerations.md)  
  
-   [Scenario di persistenza di recordset XML](../../../ado/guide/data/xml-recordset-persistence-scenario.md)
