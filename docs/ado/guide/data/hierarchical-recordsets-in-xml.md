---
description: Recordset gerarchici in XML
title: Recordset gerarchici in XML | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- hierarchical Recordsets [ADO], in XML
ms.assetid: 5d4b11c4-c94f-4910-b99b-5b9abc50d791
author: rothja
ms.author: jroth
ms.openlocfilehash: 9919489df58853a3fa8eea3711583cdf57a0849f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033711"
---
# <a name="hierarchical-recordsets-in-xml"></a>Recordset gerarchici in XML
ADO consente la persistenza degli oggetti recordset gerarchici in XML. Con gli oggetti recordset gerarchici, il valore di un campo nel recordset padre è un altro recordset. Tali campi sono rappresentati come elementi figlio nel flusso XML anziché come attributo.  
  
## <a name="remarks"></a>Commenti  
 Nell'esempio seguente viene illustrato questo caso:  
  
```  
Rs.Open "SHAPE {select stor_id, stor_name, state from stores} APPEND ({select stor_id, ord_num, ord_date, qty from sales} AS rsSales RELATE stor_id TO stor_id)", "Provider=MSDataShape;DSN=pubs;Integrated Security=SSPI;"  
```  
  
 Di seguito è riportato il formato XML del Recordset permanente:  
  
```  
<xml xmlns:s="uuid:BDC6E3F0-6DA3-11d1-A2A3-00AA00C14882"     xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882"     xmlns:rs="urn:schemas-microsoft-com:rowset"   
    xmlns:z="#RowsetSchema">   
  <s:Schema id="RowsetSchema">   
    <s:ElementType name="row" content="eltOnly" rs:updatable="true">   
      <s:AttributeType name="stor_id" rs:number="1"   
        rs:writeunknown="true">   
        <s:datatype dt:type="string" dt:maxLength="4"   
          rs:fixedlength="true" rs:maybenull="false"/>   
      </s:AttributeType>   
      <s:AttributeType name="stor_name" rs:number="2" rs:nullable="true"   
        rs:writeunknown="true">   
          <s:datatype dt:type="string" dt:maxLength="40"/>   
      </s:AttributeType>   
      <s:AttributeType name="state" rs:number="3" rs:nullable="true"   
        rs:writeunknown="true">   
        <s:datatype dt:type="string" dt:maxLength="2"   
          rs:fixedlength="true"/>   
      </s:AttributeType>   
      <s:ElementType name="rsSales" content="eltOnly"   
        rs:updatable="true" rs:relation="010000000100000000000000">   
        <s:AttributeType name="stor_id" rs:number="1"   
          rs:writeunknown="true">   
          <s:datatype dt:type="string" dt:maxLength="4"   
            rs:fixedlength="true" rs:maybenull="false"/>   
        </s:AttributeType>   
        <s:AttributeType name="ord_num" rs:number="2"   
          rs:writeunknown="true">   
          <s:datatype dt:type="string" dt:maxLength="20"   
            rs:maybenull="false"/>   
        </s:AttributeType>   
        <s:AttributeType name="ord_date" rs:number="3"   
          rs:writeunknown="true">   
            <s:datatype dt:type="dateTime" dt:maxLength="16"   
              rs:scale="3" rs:precision="23" rs:fixedlength="true"   
              rs:maybenull="false"/>   
        </s:AttributeType>   
        <s:AttributeType name="qty" rs:number="4" rs:writeunknown="true">   
          <s:datatype dt:type="i2" dt:maxLength="2" rs:precision="5"   
            rs:fixedlength="true" rs:maybenull="false"/>   
        </s:AttributeType>   
        <s:extends type="rs:rowbase"/>   
      </s:ElementType>   
      <s:extends type="rs:rowbase"/>   
    </s:ElementType>   
  </s:Schema>   
  <rs:data>   
    <z:row stor_id="6380" stor_name="Eric the Read Books" state="WA">   
      <rsSales stor_id="6380" ord_num="6871"   
        ord_date="1994-09-14T00:00:00" qty="5"/>   
      <rsSales stor_id="6380" ord_num="722a"   
        ord_date="1994-09-13T00:00:00" qty="3"/>   
    </z:row>   
    <z:row stor_id="7066" stor_name="Barnum's" state="CA">   
      <rsSales stor_id="7066" ord_num="A2976"   
        ord_date="1993-05-24T00:00:00" qty="50"/>   
      <rsSales stor_id="7066" ord_num="QA7442.3"   
        ord_date="1994-09-13T00:00:00" qty="75"/>   
    </z:row>   
    <z:row stor_id="7067" stor_name="News & Brews" state="CA">   
      <rsSales stor_id="7067" ord_num="D4482"   
        ord_date="1994-09-14T00:00:00" qty="10"/>   
      <rsSales stor_id="7067" ord_num="P2121"   
        ord_date="1992-06-15T00:00:00" qty="40"/>   
      <rsSales stor_id="7067" ord_num="P2121"   
        ord_date="1992-06-15T00:00:00" qty="20"/>   
      <rsSales stor_id="7067" ord_num="P2121"   
        ord_date="1992-06-15T00:00:00" qty="20"/>   
    </z:row>   
  </rs:data>   
</xml>   
```  
  
 L'ordine esatto delle colonne nel recordset padre non è evidente quando viene reso permanente in questo modo. Qualsiasi campo nell'elemento padre può contenere un recordset figlio. Il provider di persistenza Salva in modo permanente tutte le colonne scalari come attributi e quindi Salva in modo permanente tutte le "colonne" del recordset figlio come elementi figlio della riga padre. La posizione ordinale del campo nel recordset padre può essere ottenuta esaminando la definizione dello schema del recordset. Ogni campo ha una proprietà OLE DB, RS: Number, definita nello spazio dei nomi dello schema recordset che contiene il numero ordinale per il campo.  
  
 I nomi di tutti i campi del recordset figlio sono concatenati con il nome del campo nel recordset padre che contiene questo elemento figlio. In questo modo si garantisce che non vi siano conflitti di nome nei casi in cui i recordset padre e figlio contengono entrambi un campo ottenuto da due tabelle diverse, ma è denominato singolarmente.  
  
 Quando si salvano recordset gerarchici in XML, è necessario tenere presenti le restrizioni seguenti in ADO:  
  
-   Un recordset gerarchico con aggiornamenti in sospeso non può essere reso permanente in XML.  
  
-   Un recordset gerarchico creato con un comando Shape con parametri non può essere reso permanente (in formato XML o ADTG).  
  
-   ADO salva attualmente la relazione tra i recordset padre e figlio come oggetto binario di grandi dimensioni (BLOB). I tag XML per descrivere questa relazione non sono stati ancora definiti nello spazio dei nomi dello schema del set di righe.  
  
-   Quando viene salvato un recordset gerarchico, tutti i recordset figlio vengono salvati insieme a esso. Se il recordset corrente è figlio di un altro recordset, il relativo elemento padre non viene salvato. Vengono salvati tutti i recordset figlio che formano il sottoalbero del recordset corrente.  
  
 Quando un recordset gerarchico viene riaperto dal formato XML-permanente, è necessario tenere presenti le limitazioni seguenti:  
  
-   Se il record figlio contiene record per i quali non sono presenti record padre corrispondenti, tali righe non verranno scritte nella rappresentazione XML del recordset gerarchico. In questo modo, tali righe andranno perse quando il recordset viene riaperto dalla relativa posizione permanente.  
  
-   Se un record figlio contiene riferimenti a più di un record padre, quando si riapre il recordset, il recordset figlio potrebbe contenere record duplicati. Tuttavia, questi duplicati saranno visibili solo se l'utente utilizza direttamente il set di righe figlio sottostante. Se viene utilizzato un capitolo per spostarsi nel recordset figlio (che è l'unico modo per spostarsi tra ADO), i duplicati non sono visibili.  
  
## <a name="see-also"></a>Vedere anche  
 [Persistenza di record in formato XML](./persisting-records-in-xml-format.md)