---
description: 'Creazione di istanze di eventi ADO: Visual Basic'
title: 'Creazione di istanze evento ADO: Visual Basic | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- VB
ms.assetid: dce0a2a3-326f-4aaf-a822-6c5549833afa
author: rothja
ms.author: jroth
ms.openlocfilehash: 764c1d06be70c808881d74c8e458b74a432ddfe3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033351"
---
# <a name="ado-event-instantiation-visual-basic"></a>Creazione di istanze di eventi ADO: Visual Basic
Per gestire gli eventi ADO in Microsoft® Visual Basic®, è necessario dichiarare una variabile a livello di modulo usando la parola chiave **WithEvents** . La variabile può essere dichiarata solo come parte di un modulo di classe e deve essere dichiarata a livello di modulo. Questo non è altrettanto restrittivo come sembra, tuttavia, perché gli oggetti **Form** Visual Basic sono anche classi. Il modo più semplice per gestire gli eventi ADO consiste nel dichiarare una variabile mediante **WithEvents**. Nell'esempio seguente viene gestito l'evento **ConnectComplete** per un oggetto **Connection** :  
  
```  
' BeginEventExampleVB02  
Dim WithEvents connEvent As Connection  
Attribute connEvent.VB_VarHelpID = -1  
  
Private Sub Form_Load()  
Dim strConn As String  
  
   ' Create a new object with event  
   ' handling enabled.  
   strConn = "Provider=sqloledb;" & _  
      "Data Source=MyServer;" & _  
      "Initial Catalog=Northwind;" & _  
      "Integrated Security=SSPI;"  
   Set connEvent = New ADODB.Connection  
   connEvent.Open strConn  
End Sub  
  
Private Sub connEvent_ConnectComplete(ByVal pError As ADODB.Error, _  
    adStatus As ADODB.EventStatusEnum, _  
    ByVal pConnection As ADODB.Connection)  
Dim strMsg As String  
  
   If adStatus = adStatusErrorsOccurred Then  
      Select Case pError.Number  
         Case adErrOperationCancelled  
            ' The operation was cancelled in the  
            ' Will event. Notify the user and exit.  
            strMsg = "I'm sorry you can't connect right now." & vbCrLf  
            strMsg = strMsg & "Click OK to exit."  
            Unload Me  
         Case Else  
            strMsg = "Error " & Format(pError.Number) & vbCrLf  
            strMsg = strMsg & pError.Description  
            strMsg = strMsg & "Click OK to exit."  
            Unload Me  
      End Select  
   End If  
   frmWait.btnOK.Enabled = True  
End Sub  
' EndEventExampleVB02  
```  
  
 L'oggetto **Connection** viene dichiarato a livello di **form** utilizzando la parola chiave **WithEvents** per abilitare la gestione degli eventi. Il gestore dell'evento Form_Load crea effettivamente l'oggetto assegnando un nuovo oggetto **connessione** a *connEvent* , quindi apre la connessione. Naturalmente, un'applicazione reale esegue una maggiore elaborazione nel gestore dell'evento Form_Load rispetto a quanto illustrato di seguito.
