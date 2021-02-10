---
description: Metodo Save
title: Metodo Save | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Recordset::Save
- _Recordset::raw_Save
helpviewer_keywords:
- Save method [ADO]
ms.assetid: ed3d9678-5c28-4e61-8bb3-7dfb66d99cf5
author: rothja
ms.author: jroth
ms.openlocfilehash: a15cb79b3dc6a00e8096cec5289a5c8ce5304938
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051513"
---
# <a name="save-method"></a>Metodo Save
Salva il [Recordset](./recordset-object-ado.md) in un oggetto file o [flusso](./stream-object-ado.md) .  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
recordset.Save Destination, PersistFormat  
```  
  
#### <a name="parameters"></a>Parametri  
 *Destinazione*  
 facoltativo. **Variant** che rappresenta il nome percorso completo del file in cui deve essere salvato il **Recordset** o un riferimento a un oggetto **flusso** .  
  
 *PersistFormat*  
 facoltativo. Valore [PersistFormatEnum](./persistformatenum.md) che specifica il formato in cui deve essere salvato il **Recordset** (XML o ADTG). Il valore predefinito è **adPersistADTG**.  
  
## <a name="remarks"></a>Commenti  
 Il metodo **Save** può essere richiamato solo su un **Recordset** aperto. Utilizzare il metodo [Open (recordset ADO)](./open-method-ado-recordset.md) per ripristinare successivamente il **Recordset** dalla *destinazione*.  
  
 Se la proprietà [Filter Property](./filter-property.md) è attiva per il **Recordset**, verranno salvate solo le righe accessibili nel filtro. Se il **Recordset** è gerarchico, verranno salvati il **Recordset** figlio corrente e i relativi elementi figlio, incluso il **Recordset** padre. Se viene chiamato il metodo Save di un **Recordset** figlio, l'elemento figlio e tutti i relativi elementi figlio vengono salvati, ma l'elemento padre non lo è.  
  
 La prima volta che si salva il **Recordset**, è facoltativo specificare la *destinazione*. Se si omette la *destinazione*, verrà creato un nuovo file con un nome impostato sul valore della proprietà Source del **Recordset**.  
  
 Omettere la *destinazione* quando si chiama successivamente **Save** dopo il primo salvataggio o si verificherà un errore di run-time. Se successivamente si chiama **Save** con una nuova *destinazione*, il **Recordset** viene salvato nella nuova destinazione. Tuttavia, la nuova destinazione e la destinazione originale saranno entrambe aperte.  
  
 **Salva** non chiude il **Recordset** o la *destinazione*, pertanto è possibile continuare a lavorare con il **Recordset** e salvare le modifiche più recenti. La *destinazione* rimane aperta fino alla chiusura del **Recordset** .  
  
 Per motivi di sicurezza, il metodo **Save** consente solo l'utilizzo di impostazioni di protezione minime e personalizzate da uno script eseguito da Microsoft Internet Explorer.  
  
 Se il metodo **Save** viene chiamato mentre è in corso un'operazione di recupero, esecuzione o aggiornamento di un **Recordset** asincrono, **Salva** attende fino al completamento dell'operazione asincrona.  
  
 I record vengono salvati a partire dalla prima riga del **Recordset**. Al termine del metodo **Save** , la posizione della riga corrente viene spostata nella prima riga del **Recordset**.  
  
 Per ottenere risultati ottimali, impostare la proprietà [CursorLocation (ADO)](./cursorlocation-property-ado.md) su **adUseClient** con **Save**. Se il provider non supporta tutte le funzionalità necessarie per salvare gli oggetti **Recordset** , il servizio Cursor fornirà tale funzionalità.  
  
 Quando un **Recordset** viene reso permanente con la proprietà **CursorLocation** impostata su **adUseServer come**, la funzionalità di aggiornamento per il **Recordset** è limitata. In genere, sono consentiti solo aggiornamenti, inserimenti ed eliminazioni a tabella singola (dipendenti dalla funzionalità del provider). Anche il metodo di [Risincronizzazione](./resync-method.md) non è disponibile in questa configurazione.  
  
> [!NOTE]
>  Il salvataggio di un **Recordset** con **campi** di tipo **adVariant**, **ADIDISPATCH** o **adIUnknown** non è supportato da ADO e può causare risultati imprevedibili.  
  
 Solo i filtri sotto forma di stringhe di criteri (ad esempio, OrderDate >' 12/31/1999') influiscono sul contenuto di un **Recordset** permanente. I filtri creati con una matrice di **segnalibri** o con un valore di [FilterGroupEnum](./filtergroupenum.md) non influiscono sul contenuto del **Recordset** salvato in modo permanente. Queste regole si applicano ai **Recordset** creati con i cursori lato client o lato server.  
  
 Poiché il parametro di *destinazione* può accettare qualsiasi oggetto che supporti l'interfaccia IStream di OLE DB, è possibile salvare un **Recordset** direttamente nell'oggetto risposta ASP. Per altri dettagli, vedere lo **scenario di persistenza dei recordset XML**.  
  
 È inoltre possibile salvare un **Recordset** in formato XML in un'istanza di un oggetto DOM MSXML, come illustrato nel codice Visual Basic seguente:  
  
```  
Dim xDOM As New MSXML.DOMDocument  
Dim rsXML As New ADODB.Recordset  
Dim sSQL As String, sConn As String  
  
sSQL = "SELECT customerid, companyname, contactname FROM customers"  
sConn="Provider=Microsoft.Jet.OLEDB.4.0;Data Source=Northwind.mdb"  
rsXML.Open sSQL, sConn  
rsXML.Save xDOM, adPersistXML   'Save Recordset directly into a DOM tree.  
...  
```  
  
> [!NOTE]
>  Quando si salvano recordset gerarchici (forme dati) in formato XML, si applicano due limitazioni. Non è possibile salvare in XML se il **Recordset** gerarchico contiene aggiornamenti in sospeso e non è possibile salvare un **Recordset** gerarchico con parametri.  
  
 Un **Recordset** salvato in formato XML viene salvato utilizzando il formato UTF-8. Quando tale file viene caricato in un flusso ADO, l'oggetto flusso non tenterà di aprire un **Recordset** dal flusso, a meno che la proprietà charset del flusso non sia impostata sul valore appropriato per il formato UTF-8.  
  
## <a name="applies-to"></a>Si applica a  

:::row:::
    :::column:::
        [Oggetto Recordset (ADO)](./recordset-object-ado.md)  
    :::column-end:::
    :::column:::
        [Oggetto Stream (ADO)](./stream-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Vedere anche  
 [Esempio di metodi Save e Open (VB)](./save-and-open-methods-example-vb.md)   
 [Esempio di metodi Save e Open (VC + +)](./save-and-open-methods-example-vc.md)   
 [Metodo Open (recordset ADO)](./open-method-ado-recordset.md)   
 [Metodo Open (flusso ADO)](./open-method-ado-stream.md)   
 [Metodo SaveToFile](./savetofile-method.md)