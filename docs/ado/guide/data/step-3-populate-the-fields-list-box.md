---
description: 'Passaggio 3: Popolare la casella di riepilogo Fields'
title: 'Passaggio 3: popolare la casella di riepilogo campi | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 315c32dc-aeb1-4629-b30e-87b44e8f84d1
author: rothja
ms.author: jroth
ms.openlocfilehash: 6555f71d210524622e936026c8945168635de140
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032433"
---
# <a name="step-3-populate-the-fields-list-box"></a>Passaggio 3: Popolare la casella di riepilogo Fields
Per popolare la casella di riepilogo campi, inserire il codice seguente nel gestore eventi Click di `lstMain` :  
  
```  
Private Sub lstMain_Click()  
    Dim rec As Record  
    Dim rs As Recordset  
    Set rec = New Record  
    Set rs = New Recordset  
    grs.MoveFirst  
    grs.Move lstMain.ListIndex  
    lstDetails.Clear  
    rec.Open grs  
    Select Case rec.RecordType  
        Case adCollectionRecord:  
            Set rs = rec.GetChildren  
            While Not rs.EOF  
                lstDetails.AddItem rs(0)  
                rs.MoveNext  
            Wend  
        Case adSimpleRecord:  
            recFields rec, lstDetails, txtDetails  
  
        Case adStructDoc:  
    End Select  
  
End Sub  
```  
  
 Questo codice dichiara e crea istanze degli oggetti record e recordset locali `rec` rispettivamente e `rs` .  
  
 La riga corrispondente alla risorsa selezionata in `lstMain` viene resa la riga corrente di `grs` . La casella di riepilogo dettagli viene quindi deselezionata e `rec` viene aperta con la riga corrente di `grs` come origine.  
  
 Se la risorsa è un record di raccolta, come specificato da [RecordType](../../../ado/reference/ado-api/recordtype-property-ado.md), il recordset locale `rs` viene aperto sugli elementi figlio di REC. `lstDetails` Viene quindi compilato con i valori delle righe di `rs` .  
  
 Se la risorsa è un record semplice, `recFields` viene chiamato il metodo. Per ulteriori informazioni su `recFields` , vedere il passaggio successivo.  
  
 Non viene implementato codice se la risorsa è un documento strutturato.  
  
## <a name="see-also"></a>Vedere anche  
 [Scenario di pubblicazione Internet](../../../ado/guide/data/internet-publishing-scenario.md)   
 [Passaggio 2: inizializzare la casella di riepilogo principale](../../../ado/guide/data/step-2-initialize-the-main-list-box.md)   
 [Passaggio 4: Popolare la casella di riepilogo Details](../../../ado/guide/data/step-4-populate-the-details-text-box.md)
