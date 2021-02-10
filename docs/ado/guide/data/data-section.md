---
description: Sezione di dati
title: Sezione Data | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data section [ADO]
ms.assetid: 43dc42a8-7057-48e6-93d6-880d5c5c51a4
author: rothja
ms.author: jroth
ms.openlocfilehash: 09b79b0001ff448ecd333a4ec601c4ff42febf6d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037661"
---
# <a name="data-section"></a>Sezione di dati
La sezione Data definisce i dati del set di righe insieme a tutti gli aggiornamenti, gli inserimenti o le eliminazioni in sospeso. La sezione dei dati può contenere zero o più righe. Può contenere solo dati di un set di righe in cui la riga è definita dallo schema. Inoltre, come indicato in precedenza, le colonne senza dati possono essere omesse. Se nella sezione dati viene utilizzato un attributo o un sottoelemento e tale costrutto non è stato definito nella sezione schema, viene ignorato automaticamente.  
  
## <a name="string"></a>string  
 I caratteri XML riservati nei dati di testo devono essere sostituiti con le entità carattere appropriate. Ad esempio, nel nome della società "garage di Joe", la virgoletta singola deve essere sostituita da un'entità. La riga effettiva sarà simile alla seguente:  
  
```  
<z:row CompanyName="Joe's Garage"/>  
```  
  
 I seguenti caratteri sono riservati in XML e devono essere sostituiti da entità carattere: {', ", &, \<,> }.  
  
## <a name="binary"></a>Binary  
 I dati binari sono con codifica bin. Hex, ovvero un byte viene mappato a due caratteri, un carattere per ogni bocconcino.  
  
## <a name="datetime"></a>Datetime  
 Il formato di VT_DATE Variant non è supportato direttamente da XML-Data tipi di dati. Il formato corretto per le date con un componente dati e ora è aaaa-mm-ggThh: mm: SS.  
  
 Per ulteriori informazioni sui formati di data specificati da XML, vedere la [specifica W3C XML-Data](https://go.microsoft.com/fwlink/?LinkId=5692).  
  
 Quando la specifica XML-Data definisce due tipi di dati equivalenti (ad esempio, I4 = = int), ADO scriverà il nome descrittivo, ma si leggerà in entrambi.  
  
## <a name="managing-pending-changes"></a>Gestione delle modifiche in sospeso  
 Un recordset può essere aperto in modalità di aggiornamento immediato o batch. Quando vengono aperti in modalità di aggiornamento batch con cursori sul lato client, tutte le modifiche apportate al recordset sono in uno stato in sospeso fino a quando non viene chiamato il metodo UpdateBatch. Anche le modifiche in sospeso vengono rese permanente quando il recordset viene salvato. In XML sono rappresentati dall'uso degli elementi "Update" definiti in urn: schemas-microsoft-com: rowset. Inoltre, se è possibile aggiornare un set di righe, è necessario impostare la proprietà aggiornabile su true nella definizione della riga. Ad esempio, per definire che la tabella Shippers contiene modifiche in sospeso, la definizione di riga avrà un aspetto simile al seguente.  
  
```  
<s:ElementType name="row" content="eltOnly" updatable="true">  
  <s:attribute type="ShipperID"/>  
  <s:attribute type="CompanyName"/>  
  <s:attribute type="Phone"/>  
  <s:extends type="rs:rowbase"/>  
</s:ElementType>  
```  
  
 In questo modo, il provider di persistenza può esporre i dati in modo che ADO possa costruire un oggetto recordset aggiornabile.  
  
 I dati di esempio seguenti illustrano la modalità di ricerca di inserimenti, modifiche ed eliminazioni nel file salvato in modo permanente.  
  
```  
<rs:data>  
  <z:row ShipperID="2" CompanyName="United Package"   
    Phone="(503) 555-3199"/>  
<rs:update>  
  <rs:original>  
    <z:row ShipperID="3" CompanyName="Federal Shipping"   
      Phone="(503) 555-9931"/>  
  </rs:original>  
  <z:row Phone="(503) 552-7134"/>  
</rs:update>  
<rs:insert>  
  <z:row ShipperID="12" CompanyName="Lightning Shipping"   
    Phone="(505) 111-2222"/>  
  <z:row ShipperID="13" CompanyName="Thunder Overnight"   
    Phone="(505) 111-2222"/>  
  <z:row ShipperID="14" CompanyName="Blue Angel Air Delivery"   
    Phone="(505) 111-2222"/>  
</rs:insert>  
<rs:delete>  
  <z:row ShipperID="1" CompanyName="Speedy Express" Phone="(503) 555-9831"/>  
</rs:delete>  
</rs:data>  
```  
  
 Un aggiornamento contiene sempre tutti i dati di riga originali seguiti dai dati delle righe modificati. La riga modificata può contenere tutte le colonne o solo le colonne effettivamente modificate. Nell'esempio precedente, la riga per spedizioniere 2 non viene modificata e solo la colonna Phone ha modificato i valori per spedizioniere 3 ed è pertanto l'unica colonna inclusa nella riga modificata. Le righe inserite per gli spedizionieri 12, 13 e 14 vengono raggruppate in un tag RS: Insert. Si noti che le righe eliminate possono anche essere raggruppate in batch, sebbene non sia illustrato nell'esempio precedente.  
  
## <a name="see-also"></a>Vedere anche  
 [Persistenza di record in formato XML](./persisting-records-in-xml-format.md)