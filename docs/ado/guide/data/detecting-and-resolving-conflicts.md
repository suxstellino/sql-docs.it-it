---
description: Rilevamento e risoluzione di conflitti
title: Rilevamento e risoluzione dei conflitti | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- conflicts [ADO], detecting and resolving
- ADO, detecting and resolving conflicts
ms.assetid: b28fdd26-c1a4-40ce-a700-2b0c9d201514
author: rothja
ms.author: jroth
ms.openlocfilehash: 4c92b847b95aacd5cf642c9646f4618dc5986abf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037631"
---
# <a name="detecting-and-resolving-conflicts"></a>Rilevamento e risoluzione di conflitti
Se si sta lavorando con il recordset in modalità immediata, è possibile che si verifichino problemi di concorrenza. D'altra parte, se l'applicazione usa l'aggiornamento in modalità batch, è possibile che un utente possa modificare un record prima che vengano salvate le modifiche apportate da un altro utente che modifica lo stesso record. In tal caso, si desidera che l'applicazione gestisca normalmente il conflitto. Potrebbe essere necessario che l'ultima persona invii un aggiornamento al server "WINS". In alternativa, è possibile consentire all'utente più recente di decidere quale aggiornamento deve avere la precedenza, offrendogli una scelta tra i due valori in conflitto.  
  
 In ogni caso, ADO fornisce le proprietà UnderlyingValue e OriginalValue dell'oggetto Field per gestire questi tipi di conflitti. Utilizzare queste proprietà in combinazione con il metodo di risincronizzazione e la proprietà Filter del recordset.  
  
## <a name="remarks"></a>Commenti  
 Quando ADO rileva un conflitto durante un aggiornamento batch, viene aggiunto un avviso alla raccolta Errors. Pertanto, è consigliabile controllare sempre gli errori immediatamente dopo la chiamata a BatchUpdate e, se li si trova, iniziare a testare il presupposto che si sia verificato un conflitto. Il primo passaggio consiste nell'impostare la proprietà Filter sul recordset uguale a adFilterConflictingRecords. Questo limita la visualizzazione del recordset solo ai record in conflitto. Se la proprietà RecordCount è uguale a zero dopo questo passaggio, si sa che l'errore è stato generato da qualcosa di diverso da un conflitto.  
  
 Quando si chiama BatchUpdate, ADO e il provider generano istruzioni SQL per eseguire aggiornamenti sull'origine dati. Tenere presente che alcune origini dati presentano limitazioni sui tipi di colonne che è possibile utilizzare in una clausola WHERE.  
  
 Chiamare quindi il metodo Resync sul recordset con il set di argomenti AffectRecords uguale a adAffectGroup e il set di argomenti ResyncValues uguale a adResyncUnderlyingValues. Il metodo Resync aggiorna i dati nell'oggetto recordset corrente dal database sottostante. Utilizzando adAffectGroup, si garantisce che solo i record visibili con l'impostazione del filtro corrente, ovvero solo i record in conflitto, vengano risincronizzati con il database. Questo potrebbe rendere una differenza significativa nelle prestazioni se si sta utilizzando un recordset di grandi dimensioni. Impostando l'argomento ResyncValues su adResyncUnderlyingValues quando si chiama la risincronizzazione, si garantisce che la proprietà UnderlyingValue conterrà il valore (in conflitto) del database, che la proprietà Value manterrà il valore immesso dall'utente e che la proprietà OriginalValue conterrà il valore originale per il campo (il valore che aveva prima dell'ultima chiamata UpdateBatch riuscita). È quindi possibile usare questi valori per risolvere il conflitto a livello di codice o richiedere all'utente di selezionare il valore che verrà usato.  
  
 Questa tecnica è illustrata nell'esempio di codice seguente. Nell'esempio viene creato in modo artificiale un conflitto utilizzando un recordset separato per modificare un valore nella tabella sottostante prima della chiamata a UpdateBatch.  
  
```  
'BeginConflicts  
    strConn = "Provider=SQLOLEDB;Initial Catalog=Northwind;" & _  
              "Data Source=MySQLServer;Integrated Security=SSPI;"  
  
    strSQL = "SELECT * FROM Shippers WHERE ShipperID = 2"  
  
    'Open Rs and change a value  
    Set objRs1 = New ADODB.Recordset  
    Set objRs2 = New ADODB.Recordset  
    objRs1.CursorLocation = adUseClient  
    objRs1.Open strSQL, strConn, adOpenStatic, adLockBatchOptimistic, adCmdText  
    objRs1("Phone") = "(111) 555-1111"  
  
    'Introduce a conflict at the db...  
    objRs2.Open strSQL, strConn, adOpenKeyset, adLockOptimistic, adCmdText  
    objRs2("Phone") = "(999) 555-9999"  
    objRs2.Update  
    objRs2.Close  
    Set objRs2 = Nothing  
  
    On Error Resume Next  
    objRs1.UpdateBatch  
  
    If objRs1.ActiveConnection.Errors.Count <> 0 Then  
        Dim intConflicts As Integer  
  
        intConflicts = 0  
  
        objRs1.Filter = adFilterConflictingRecords  
  
        intConflicts = objRs1.RecordCount  
  
        'Resync so we can see the UnderlyingValue and offer user a choice.  
        'This sample only displays all three values and resets to original.  
        objRs1.Resync adAffectGroup, adResyncUnderlyingValues  
  
        If intConflicts > 0 Then  
            strMsg = "A conflict occurred with updates for " & intConflicts & _  
                     " record(s)." & vbCrLf & "The values will be restored" & _  
                     " to their original values." & vbCrLf & vbCrLf  
  
            objRs1.MoveFirst  
            While Not objRs1.EOF  
              strMsg = strMsg & "SHIPPER = " & objRs1("CompanyName") & vbCrLf  
              strMsg = strMsg & "Value = " & objRs1("Phone").Value & vbCrLf  
              strMsg = strMsg & "UnderlyingValue = " & _  
                                 objRs1("Phone").UnderlyingValue & vbCrLf  
              strMsg = strMsg & "OriginalValue = " & _  
                                 objRs1("Phone").OriginalValue & vbCrLf  
              strMsg = strMsg & vbCrLf & "Original value has been restored."  
  
              MsgBox strMsg, vbOKOnly, _  
                    "Conflict " & objRs1.AbsolutePosition & _  
                    " of " & intConflicts  
  
              objRs1("Phone").Value = objRs1("Phone").OriginalValue  
              objRs1.MoveNext  
            Wend  
  
            objRs1.UpdateBatch adAffectGroup  
        Else  
            'Other error occurred. Minimal handling in this example.  
             strMsg = "Errors occurred during the update. " & _  
                        objRs1.ActiveConnection.Errors(0).Number & " " & _  
                        objRs1.ActiveConnection.Errors(0).Description  
        End If  
  
        On Error GoTo 0  
    End If  
  
    objRs1.MoveFirst  
    objRs1.Close  
    Set objRs1 = Nothing  
'EndConflicts  
```  
  
 È possibile utilizzare la proprietà Status del record corrente o di un campo specifico per determinare il tipo di conflitto che si è verificato.  
  
 Per informazioni dettagliate sulla gestione degli errori, vedere [gestione degli errori](./error-handling.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Modalità batch](./batch-mode.md)