---
description: Raccolta Fields
title: Raccolta Fields | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Field object [ADO], fields collection
- Fields collection [ADO]
ms.assetid: 574cf36e-e5f5-403b-983c-749ef93c108f
author: rothja
ms.author: jroth
ms.openlocfilehash: 7dee12a0cdb274f58bb3dc79fddeb057e4913e81
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032413"
---
# <a name="the-fields-collection"></a>Raccolta Fields
La raccolta **Fields** è una delle raccolte intrinseche di ADO. Una raccolta è un set ordinato di elementi a cui è possibile fare riferimento come unità. Per ulteriori informazioni sulle raccolte ADO, vedere [il modello a oggetti ADO](../../../ado/guide/data/ado-objects-and-collections.md).  
  
 La raccolta **Fields** contiene un oggetto **Field** per ogni campo (colonna) nel **Recordset**. Analogamente a tutte le raccolte ADO, dispone di proprietà **count** e **Item** , oltre ai metodi **Append** e **Refresh** . Dispone inoltre di metodi **CancelUpdate**, **Delete**, **Resync** e **Update** , che non sono disponibili per altre raccolte ADO.  
  
## <a name="examining-the-fields-collection"></a>Esame della raccolta Fields  
 Si consideri la raccolta **Fields** del **Recordset** di esempio introdotto in questa sezione. Il **Recordset** di esempio è derivato dall'istruzione SQL  
  
```  
SELECT ProductID, ProductName, UnitPrice FROM Products WHERE CategoryID = 7  
```  
  
 Pertanto, si noterà che la raccolta dei **campi recordset** contiene tre campi.  
  
```  
'BeginWalkFields  
    Dim objFields As ADODB.Fields  
    Dim intLoop As Integer  
  
    objRs.Open strSQL, strConnStr, adOpenForwardOnly, adLockReadOnly, adCmdText  
  
    Set objFields = objRs.Fields  
  
    For intLoop = 0 To (objFields.Count - 1)  
        Debug.Print objFields.Item(intLoop).Name  
    Next  
'EndWalkFields  
```  
  
 Questo codice determina semplicemente il numero di oggetti **campo** nella raccolta **Fields** usando la proprietà **count** e scorre la raccolta, restituendo il valore della proprietà **Name** per ogni oggetto **campo** . Per ottenere informazioni su un campo, è possibile usare molte altre proprietà di **campo** . Per ulteriori informazioni sull'esecuzione di query su un **campo**, vedere [l'oggetto Field](../../../ado/guide/data/the-field-object.md).  
  
## <a name="counting-columns"></a>Conteggio delle colonne  
 Come si può immaginare, la proprietà **count** restituisce il numero effettivo di oggetti **Field** nella raccolta **Fields** . Poiché la numerazione per i membri di una raccolta inizia con zero, è consigliabile usare sempre i cicli di codice che iniziano con il membro zero e terminano con il valore della proprietà **count** meno 1. Se si utilizza Microsoft Visual Basic e si desidera scorrere i membri di una raccolta senza selezionare la proprietà **count** , utilizzare l'oggetto **for each... Comando successivo** .  
  
 Se la proprietà **count** è zero, nella raccolta non sono presenti oggetti.  
  
## <a name="getting-to-the-field"></a>Recupero del campo  
 Come per qualsiasi raccolta ADO, la proprietà **Item** è la proprietà predefinita della raccolta. Restituisce il singolo oggetto **Field** specificato dal nome o dall'indice passato. Pertanto, le istruzioni seguenti sono equivalenti per il **Recordset** di esempio:  
  
```  
objField = objRecordset.Fields.Item("ProductID")  
objField = objRecordset.Fields("ProductID")  
objField = objRecordset.Fields.Item(0)  
objField = objRecordset.Fields(0)  
```  
  
 Se questi metodi sono equivalenti, qual è la scelta migliore? Dipende. L'utilizzo di un indice per recuperare un **campo** dalla raccolta è più veloce perché accede direttamente al **campo** senza dover eseguire una ricerca di stringhe. D'altra parte, l'ordine dei **campi** all'interno della raccolta deve essere noto e, in caso di modifica dell'ordine, il riferimento all'indice del **campo** dovrà essere modificato ovunque si verifichi. Sebbene leggermente più lento, l'uso del nome del **campo** è più flessibile perché non dipende dall'ordine dei **campi** nella raccolta.  
  
## <a name="using-the-refresh-method"></a>Utilizzo del metodo Refresh  
 A differenza di altre raccolte ADO, l'utilizzo del metodo **Refresh** sulla raccolta **Fields** non ha alcun effetto visibile. Per recuperare le modifiche dalla struttura del database sottostante, è necessario utilizzare il metodo **Requery** oppure, se l'oggetto **Recordset** non supporta i segnalibri, il metodo **MoveFirst** , che comporterà l'esecuzione del comando sul provider.  
  
## <a name="adding-fields-to-a-recordset"></a>Aggiunta di campi a un recordset  
 Il metodo **Append** viene utilizzato per aggiungere campi a un **Recordset**.  
  
 È possibile utilizzare il metodo **Append** per costruire un **Recordset** a livello di codice senza aprire una connessione a un'origine dati. Si verificherà un errore di run-time se il metodo **Append** viene chiamato sulla raccolta **Fields** di un **Recordset** aperto o su un **Recordset** in cui è stata impostata la proprietà **ActiveConnection** . È possibile aggiungere campi solo a un **Recordset** che non è aperto e non è ancora stato connesso a un'origine dati. Tuttavia, per specificare i valori per i **campi** appena accodati, è necessario innanzitutto aprire il **Recordset** .  
  
 Gli sviluppatori spesso hanno bisogno di una posizione per archiviare temporaneamente alcuni dati o desiderano che alcuni dati fungano da un server, in modo che possa partecipare a data binding in un'interfaccia utente. ADO (insieme al [servizio Microsoft Cursor per OLE DB](../../../ado/guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md)) consente allo sviluppatore di compilare un oggetto **Recordset** vuoto specificando le informazioni sulle colonne e chiamando **Open**. Nell'esempio seguente vengono aggiunti tre nuovi campi a un nuovo oggetto **Recordset** . Il **Recordset** viene quindi aperto, vengono aggiunti due nuovi record e il **Recordset** viene salvato in modo permanente in un file. Per ulteriori informazioni sulla persistenza dei **Recordset** , vedere [aggiornamento e persistenza dei dati](../../../ado/guide/data/updating-and-persisting-data.md).  
  
```  
'BeginFabricate  
    Dim objRs As ADODB.Recordset  
    Set objRs = New ADODB.Recordset  
  
    With objRs.Fields  
        .Append "StudentID", adChar, 11, adFldUpdatable  
        .Append "FullName", adVarChar, 50, adFldUpdatable  
        .Append "PhoneNmbr", adVarChar, 20, adFldUpdatable  
    End With  
  
    With objRs  
        .Open  
  
        .AddNew  
        .Fields(0) = "123-45-6789"  
        .Fields(1) = "John Doe"  
        .Fields(2) = "(425) 555-5555"  
        .Update  
  
        .AddNew  
        .Fields(0) = "123-45-6780"  
        .Fields(1) = "Jane Doe"  
        .Fields(2) = "(615) 555-1212"  
        .Update  
    End With  
  
    objRs.Save App.Path & "FabriTest.adtg", adPersistADTG  
  
    objRs.Close  
'EndFabricate  
```  
  
 L'utilizzo del metodo **Append dei campi** è diverso tra l'oggetto **Recordset** e l'oggetto **record** . Per ulteriori informazioni sull'oggetto **record** , vedere [record e flussi](../../../ado/guide/data/records-and-streams.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione di recordset gerarchici](../../../ado/guide/data/fabricating-hierarchical-recordsets.md)
