---
description: Sezione dello schema
title: Sezione dello schema | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Schema section [ADO]
ms.assetid: 4ac6e524-2c92-48e8-b871-0a4b5c8fda18
author: rothja
ms.author: jroth
ms.openlocfilehash: 346957b2b060bf990cf77bfe7784f67b13a37c37
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037011"
---
# <a name="schema-section"></a>Sezione dello schema
La sezione schema è obbligatoria. Come illustrato nell'esempio precedente, ADO scrive i metadati dettagliati su ogni colonna per mantenere la semantica dei valori dei dati il più possibile per l'aggiornamento. Tuttavia, per caricare nel codice XML, ADO richiede solo i nomi delle colonne e il set di righe a cui appartengono. Di seguito è riportato un esempio di uno schema minimo:  
  
```  
<xml xmlns:s="uuid:BDC6E3F0-6DA3-11d1-A2A3-00AA00C14882"  
    xmlns:rs="urn:schemas-microsoft-com:rowset"  
    xmlns:z="#RowsetSchema">  
  <s:Schema id="RowsetSchema">  
    <s:ElementType name="row" content="eltOnly">  
      <s:AttributeType name="ShipperID"/>  
      <s:AttributeType name="CompanyName"/>  
      <s:AttributeType name="Phone"/>  
      <s:Extends type="rs:rowbase"/>  
    </s:ElementType>  
  </s:Schema>  
  <rs:data>  
...  
  </rs:data>  
</xml>  
```  
  
 Nell'esempio precedente, ADO considererà i dati come stringhe a lunghezza variabile perché non sono incluse informazioni sul tipo nello schema.  
  
## <a name="creating-aliases-for-column-names"></a>Creazione di alias per i nomi di colonna  
 L'attributo RS: Name consente di creare un alias per un nome di colonna, in modo che un nome descrittivo venga visualizzato nelle informazioni sulla colonna esposte dal set di righe e che sia possibile utilizzare un nome più breve nella sezione dati. Ad esempio, è possibile modificare lo schema precedente per eseguire il mapping di IDCorriere a S1, da CompanyName a S2 e da telefono a S3 come indicato di seguito:  
  
```  
<s:Schema id="RowsetSchema">   
<s:ElementType name="row" content="eltOnly" rs:updatable="true">   
<s:AttributeType name="s1" rs:name="ShipperID" rs:number="1" ...>   
...  
</s:AttributeType>   
<s:AttributeType name="s2" rs:name="CompanyName" rs:number="2" ...>   
...  
</s:AttributeType>   
<s:AttributeType name="s3" rs:name="Phone" rs:number="3" ...>   
...  
</s:AttributeType>   
...  
</s:ElementType>   
</s:Schema>  
```  
  
 Quindi, nella sezione dati la riga utilizzerebbe l'attributo Name (non RS: Name) per fare riferimento a tale colonna:  
  
```  
"<row s1="1" s2="Speedy Express" s3="(503) 555-9831"/>  
```  
  
 La creazione di alias per i nomi di colonna è obbligatoria ogni volta che un nome di colonna non è un attributo o un nome di tag valido in XML. Ad esempio, "LastName" deve avere un alias perché i nomi con spazi incorporati sono attributi non validi. La riga seguente non verrà gestita correttamente dal parser XML, pertanto è necessario creare un alias per un altro nome che non disponga di uno spazio incorporato.  
  
```  
<row last name="Jones"/>  
```  
  
 Qualsiasi valore utilizzato per l'attributo Name deve essere utilizzato in modo coerente in ogni punto in cui viene fatto riferimento alla colonna sia nelle sezioni dello schema che dei dati del documento XML. Nell'esempio seguente viene illustrato l'uso coerente di S1:  
  
```  
<s:Schema id="RowsetSchema">  
  <s:ElementType name="row" content="eltOnly">  
    <s:attribute type="s1"/>  
    <s:attribute type="CompanyName"/>  
    <s:attribute type="s3"/>  
    <s:extends type="rs:rowbase"/>  
  </s:ElementType>  
  <s:AttributeType name="s1" rs:name="ShipperID" rs:number="1"   
    rs:maydefer="true" rs:writeunknown="true">  
    <s:datatype dt:type="i4" dt:maxLength="4" rs:precision="10"   
      rs:fixedlength="true" rs:maybenull="true"/>  
  </s:AttributeType>  
</s:Schema>  
<rs:data>  
  <z:row s1="1" CompanyName="Speedy Express" s3="(503) 555-9831"/>  
</rs:data>  
```  
  
 Analogamente, poiché nell'esempio precedente non è definito alcun alias `CompanyName` , è `CompanyName` necessario utilizzarlo in modo coerente in tutto il documento.  
  
## <a name="data-types"></a>Tipi di dati  
 È possibile applicare un tipo di dati a una colonna con l'attributo dt: Type. Per la guida definitiva ai tipi XML consentiti, vedere la sezione relativa ai tipi di dati della [specifica W3C XML-Data](http://www.w3.org/TR/1998/NOTE-XML-data/). È possibile specificare un tipo di dati in due modi: specificare l'attributo dt: Type direttamente nella definizione di colonna oppure utilizzare il costrutto s:DataType come elemento annidato della definizione di colonna. Ad esempio,  
  
```  
<s:AttributeType name="Phone" >  
  <s:datatype dt:type="string"/>  
</s:AttributeType>  
```  
  
 equivale a  
  
```  
<s:AttributeType name="Phone" dt:type="string"/>  
```  
  
 Se si omette l'attributo dt: Type interamente dalla definizione di riga, per impostazione predefinita il tipo della colonna sarà una stringa di lunghezza variabile.  
  
 Se si dispone di più informazioni sul tipo rispetto al nome del tipo (ad esempio, DT: maxLength), questo rende più leggibile l'utilizzo dell'elemento figlio s:DataType. Si tratta semplicemente di una convenzione, ma non di un requisito.  
  
 Negli esempi seguenti viene illustrato come includere informazioni sui tipi nello schema.  
  
```  
<!-- 1. String with no max length -->  
<s:AttributeType name="title_id"/>  
<!-or -->  
<s:AttributeType name="title_id" dt:type="string"/>  
  
<!-- 2. Fixed length string with max length of 6 -->  
<s:AttributeType name="title_id">  
    <s:datatype dt:type="string" dt:maxLength="6" rs:fixedlength="true" />  
</s:AttributeType>  
  
<!-- 3. Variable length string with max length of 6 -->  
<s:AttributeType name="title_id">  
    <s:datatype dt:type="string" dt:maxLength="6" />  
</s:AttributeType>  
  
<!-- 4. Integer -->  
<s:AttributeType name="title_id" dt:type="int"/>  
```  
  
 Nel secondo esempio esiste un lieve utilizzo dell'attributo RS: FixedLength. Una colonna con l'attributo RS: FixedLength impostata su true significa che i dati devono avere la lunghezza definita nello schema. In questo caso, un valore valido per title_id è "123456", così com'è "123". Tuttavia, "123" non sarebbe valido perché la sua lunghezza è 3, non 6. Per una descrizione più completa della proprietà FixedLength, vedere la guida per programmatori OLE DB.  
  
## <a name="handling-nulls"></a>Gestione dei valori null  
 I valori null vengono gestiti dall'attributo RS: maybenull. Se questo attributo è impostato su true, il contenuto della colonna può contenere un valore null. Inoltre, se la colonna non viene trovata in una riga di dati, l'utente che esegue la lettura dei dati dal set di righe otterrà uno stato null da IRowset:: GetData (). Si considerino le seguenti definizioni di colonna dalla tabella Shippers.  
  
```  
<s:AttributeType name="ShipperID">  
  <s:datatype dt:type="int" dt:maxLength="4"/>  
</s:AttributeType>  
<s:AttributeType name="CompanyName">  
  <s:datatype dt:type="string" dt:maxLength="40" rs:maybenull="true"/>  
</s:AttributeType>  
```  
  
 La definizione consente `CompanyName` di essere null, ma `ShipperID` non può contenere un valore null. Se la sezione dati contiene la riga seguente, il provider di persistenza imposterà lo stato dei dati della `CompanyName` colonna sulla costante di stato OLE DB DBSTATUS_S_ISNULL:  
  
```  
<z:row ShipperID="1"/>  
```  
  
 Se la riga è completamente vuota, come indicato di seguito, il provider di persistenza restituisce uno stato OLE DB di DBSTATUS_E_UNAVAILABLE per `ShipperID` e DBSTATUS_S_ISNULL per CompanyName.  
  
```  
<z:row/>   
```  
  
 Si noti che una stringa di lunghezza zero non corrisponde a null.  
  
```  
<z:row ShipperID="1" CompanyName=""/>  
```  
  
 Per la riga precedente, il provider di persistenza restituirà uno stato di OLE DB DBSTATUS_S_OK per entrambe le colonne. `CompanyName`In questo caso è semplicemente "" (una stringa di lunghezza zero).  
  
 Per ulteriori informazioni sui costrutti di OLE DB disponibili per l'utilizzo nello schema di un documento XML per OLE DB, vedere la definizione di "urn: schemas-microsoft-com: rowset" e la guida per programmatori OLE DB.  
  
## <a name="see-also"></a>Vedere anche  
 [Persistenza di record in formato XML](../../../ado/guide/data/persisting-records-in-xml-format.md)
