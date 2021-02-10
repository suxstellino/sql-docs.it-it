---
description: Formato di persistenza XML
title: Formato di persistenza XML | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- XML persistence [ADO], persistence format
ms.assetid: 6e146738-ac4d-47bb-b6cd-d87b2260aead
author: rothja
ms.author: jroth
ms.openlocfilehash: 3a1a1b832f9e34cbf99e5fd01c424bf54c50c3bc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036721"
---
# <a name="xml-persistence-format"></a>Formato di persistenza XML
ADO usa la codifica UTF-8 per il flusso XML che rende permanente.  
  
 Il formato XML ADO è suddiviso in due sezioni, una sezione dello schema seguita dalla sezione Data. Di seguito è riportato un esempio di file XML per la tabella Shippers del database Northwind. Le varie parti del codice XML sono illustrate dopo l'esempio.  
  
## <a name="remarks"></a>Commenti  
  
```  
<xml xmlns:s="uuid:BDC6E3F0-6DA3-11d1-A2A3-00AA00C14882"   
xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882"   
xmlns:rs="urn:schemas-microsoft-com:rowset"   
xmlns:z="#RowsetSchema">   
  <s:Schema id="RowsetSchema">   
    <s:ElementType name="row" content="eltOnly" rs:updatable="true">   
      <s:AttributeType name="ShipperID" rs:number="1"   
        rs:basetable="shippers" rs:basecolumn="ShipperID"  
        rs:keycolumn="true">   
        <s:datatype dt:type="int" dt:maxLength="4" rs:precision="10"   
          rs:fixedlength="true" rs:maybenull="false"/>   
      </s:AttributeType>   
      <s:AttributeType name="CompanyName" rs:number="2"   
        rs:nullable="true" rs:write="true" rs:basetable="shippers"   
        rs:basecolumn="CompanyName">   
        <s:datatype dt:type="string" dt:maxLength="40" />   
      </s:AttributeType>   
      <s:AttributeType name="Phone" rs:number="3" rs:nullable="true"   
        rs:write="true" rs:basetable="shippers"   
        rs:basecolumn="Phone">   
        <s:datatype dt:type="string" dt:maxLength="24"/>   
      </s:AttributeType>   
      <s:extends type="rs:rowbase"/>   
    </s:ElementType>   
  </s:Schema>   
  
  <rs:data>   
    <z:row ShipperID="1" CompanyName="Speedy Express"   
      Phone="(503) 555-9831"/>   
    <z:row ShipperID="2" CompanyName="United Package"   
      Phone="(503) 555-3199"/>   
    <z:row ShipperID="3" CompanyName="Federal Shipping"   
      Phone="(503) 555-9931"/>   
  </rs:data>   
</xml>  
```  
  
 Nello schema sono illustrate le dichiarazioni degli spazi dei nomi, la sezione dello schema e la sezione dei dati. Nella sezione dello schema sono contenute le definizioni per Row, IDCorriere, CompanyName e Phone.  
  
 Le definizioni dello schema sono conformi alla [specifica W3C XML-Data](http://www.w3.org/TR/1998/NOTE-XML-data/) e possono essere completamente convalidate (sebbene la convalida non venga eseguita in Internet Explorer 5). XML-Data è attualmente l'unico formato di schema supportato per la persistenza dei recordset.  
  
 La sezione Data contiene tre righe contenenti informazioni sui mittenti. Per un set di righe vuoto, la sezione dei dati può essere vuota, ma i \<rs:data> tag devono essere presenti. Senza dati, è possibile scrivere l'abbreviazione di tag semplicemente \<rs:data/> . Qualsiasi tag con prefisso "RS" indica che si trova nello spazio dei nomi definito da urn: schemas-microsoft-com: rowset.  
  
## <a name="see-also"></a>Vedere anche  
 [Persistenza di record in formato XML](./persisting-records-in-xml-format.md)