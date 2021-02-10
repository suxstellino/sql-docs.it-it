---
description: 'Invio di aggiornamenti: metodo UpdateBatch'
title: 'Invio degli aggiornamenti: metodo UpdateBatch | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 87123797-831f-48e0-94b5-f669f9ca194a
author: rothja
ms.author: jroth
ms.openlocfilehash: 88e441fd0ff58f63cf5ac701ec2620c29048b1e6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032493"
---
# <a name="sending-the-updates-updatebatch-method"></a>Invio di aggiornamenti: metodo UpdateBatch
Il codice seguente apre un recordset in modalità batch impostando la proprietà LockType su adLockBatchOptimistic e CursorLocation su adUseClient. Aggiunge due nuovi record e modifica il valore di un campo in un record esistente, salvando i valori originali, quindi chiama UpdateBatch per restituire le modifiche all'origine dati.  
  
## <a name="remarks"></a>Commenti  
  
```  
'BeginBatchUpdate  
    strConn = "Provider=SQLOLEDB;Initial Catalog=Northwind;" & _  
              "Data Source=MySQLServer;Integrated Security=SSPI;"  
  
    strSQL = "SELECT ShipperId, CompanyName, Phone FROM Shippers"  
  
    Set objRs1 = New ADODB.Recordset  
    objRs1.CursorLocation = adUseClient  
    objRs1.Open strSQL, strConn, adOpenStatic, adLockBatchOptimistic, adCmdText  
  
    ' Change value of Phone field for first record in Recordset, saving value  
    ' for later restoration.  
    intId = objRs1("ShipperId")  
    sPhone = objRs1("Phone")  
  
    objRs1("Phone") = "(111) 555-1111"  
  
    'Add two new records  
    For i = 0 To 1  
        objRs1.AddNew  
        objRs1(1) = "New Shipper #" & CStr((i + 1))  
        objRs1(2) = "(nnn) 555-" & i & i & i & i  
    Next i  
  
    ' Send the updates  
    objRs1.UpdateBatch  
'EndBatchUpdate  
```  
  
 Se si modifica il record corrente o si aggiunge un nuovo record quando si chiama il metodo UpdateBatch, ADO chiamerà automaticamente il metodo Update per salvare le modifiche in sospeso apportate al record corrente prima di trasmettere le modifiche in batch al provider.  
  
## <a name="see-also"></a>Vedere anche  
 [Modalità batch](../../../ado/guide/data/batch-mode.md)
