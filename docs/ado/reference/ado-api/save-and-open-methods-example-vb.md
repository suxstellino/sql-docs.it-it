---
description: Esempio dei metodi Save e Open (VB)
title: Esempio di metodi Save e Open (VB) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- Save method [ADO], Visual Basic example
- Open method [ADO]
ms.assetid: ddccdf58-9c57-4c9b-8b7f-0cf193f955fb
author: rothja
ms.author: jroth
ms.openlocfilehash: 0eafd067cdad9c107c591d09b5b5f50fdc6bc501
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040691"
---
# <a name="save-and-open-methods-example-vb"></a>Esempio dei metodi Save e Open (VB)
Questi tre esempi illustrano il modo in cui i metodi [Save](./save-method.md) e [Open](./open-method-ado-recordset.md) possono essere usati insieme.  
  
 Si supponga che si stia procedendo a un viaggio di lavoro e che si voglia eseguire una tabella da un database. Prima di procedere, è possibile accedere ai dati come [Recordset](./recordset-object-ado.md) e salvarli in un modulo trasportabile. Quando si arriva alla destinazione, si accede al **Recordset** come **Recordset** locale disconnesso. Apportare modifiche al **Recordset** e quindi salvarlo di nuovo. Infine, quando si torna a casa, si esegue nuovamente la connessione al database e la si aggiorna con le modifiche apportate in viaggio.  
  
 Per prima cosa, accedere alla tabella ***authors*** e salvarla.  
  
```  
'BeginSaveVB  
  
    'To integrate this code  
    'replace the data source and initial catalog values  
    'in the connection string  
  
Public Sub Main()  
    On Error GoTo ErrorHandler  
  
    'recordset and connection variables  
    Dim rstAuthors As ADODB.Recordset  
    Dim Cnxn As ADODB.Connection  
    Dim strCnxn As String  
    Dim strSQLAuthors As String  
  
    ' Open connection  
    Set Cnxn = New ADODB.Connection  
    strCnxn = "Provider='sqloledb';Data Source='MySqlServer';" & _  
        "Initial Catalog='Pubs';Integrated Security='SSPI';"  
    Cnxn.Open strCnxn  
  
    Set rstAuthors = New ADODB.Recordset  
    strSQLAuthors = "SELECT au_id, au_lname, au_fname, city, phone FROM Authors"  
    rstAuthors.Open strSQLAuthors, Cnxn, adOpenDynamic, adLockOptimistic, adCmdText  
  
    'For sake of illustration, save the Recordset to a diskette in XML format  
    rstAuthors.Save "c:\Pubs.xml", adPersistXML  
  
    ' clean up  
    rstAuthors.Close  
    Cnxn.Close  
    Set rstAuthors = Nothing  
    Set Cnxn = Nothing  
    Exit Sub  
  
ErrorHandler:  
    'clean up  
    If Not rstAuthors Is Nothing Then  
        If rstAuthors.State = adStateOpen Then rstAuthors.Close  
    End If  
    Set rstAuthors = Nothing  
  
    If Not Cnxn Is Nothing Then  
        If Cnxn.State = adStateOpen Then Cnxn.Close  
    End If  
    Set Cnxn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
'EndSaveVB  
```  
  
 A questo punto, si è arrivati alla destinazione. Si accederà alla tabella ***authors** _ come un recordset * * locale e disconnesso * *. È necessario disporre del provider **MSPersist** nel computer in uso per accedere al file salvato a:\Pubs.xml.  
  
```  
Attribute VB_Name = "Save"  
```  
  
 Infine, viene restituito Home. A questo punto, aggiornare il database con le modifiche apportate.  
  
```  
Attribute VB_Name = "Save"  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo Open (recordset ADO)](./open-method-ado-recordset.md)   
 [Oggetto recordset (ADO)](./recordset-object-ado.md)   
 [Ulteriori informazioni sulla persistenza dei recordset](../../guide/data/more-about-recordset-persistence.md)   
 [Metodo Save](./save-method.md)