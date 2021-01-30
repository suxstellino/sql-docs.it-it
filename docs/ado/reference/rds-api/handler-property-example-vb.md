---
description: Esempio della proprietà Handler (VB)
title: Esempio di proprietà Handler (VB) | Microsoft Docs
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
- Handler property [ADO], Visual Basic example
ms.assetid: 9664f9a6-65fc-4e7f-be3d-3e4b501b558a
author: rothja
ms.author: jroth
ms.openlocfilehash: d4d97d9aae8117ac7d10362da0ab5f666fc329f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168932"
---
# <a name="handler-property-example-vb"></a>Esempio della proprietà Handler (VB)
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
 In questo esempio viene illustrata la proprietà del [gestore](./handler-property-rds.md) dell'oggetto [DataControl di RDS](./datacontrol-object-rds.md) . Per altri dettagli, vedere [personalizzazione di datafactory](../../guide/remote-data-service/datafactory-customization.md) .  
  
 Si supponga che le sezioni seguenti nel file di parametri, Msdfmap.ini, si trovino nel server:  
  
```  
[connect AuthorDataBase]  
Access=ReadWrite  
Connect="DSN=Pubs"  
[sql AuthorById]  
SQL="SELECT * FROM Authors WHERE au_id = ?"  
```  
  
 Il codice è simile al seguente. Il comando assegnato alla proprietà [SQL](./sql-property.md) corrisponderà all'identificatore ***AuthorById** _ e recupererà una riga per l'autore Michael Leary. La proprietà **Recordset** oggetto _ *DataControl** viene assegnata a un oggetto [Recordset](../ado-api/recordset-object-ado.md) disconnesso esclusivamente come praticità di codifica.  
  
```  
'BeginHandlerVB  
Public Sub Main()  
    On Error GoTo ErrorHandler  
  
    Dim dc As New DataControl  
    Dim rst As ADODB.Recordset  
  
    dc.Handler = "MSDFMAP.Handler"  
    dc.ExecuteOptions = 1  
    dc.FetchOptions = 1  
    dc.Server = "https://MyServer"  
    dc.Connect = "Data Source=AuthorDataBase"  
    dc.SQL = "AuthorById('267-41-2394')"  
    dc.Refresh                  'Retrieve the record  
    Set rst = dc.Recordset      'Use another Recordset as a convenience  
    Debug.Print "Author is '" & rst!au_fname & " " & rst!au_lname & "'"  
  
    ' clean up  
    If rst.State = adStateOpen Then rst.Close  
    Set rst = Nothing  
    Set dc = Nothing  
    Exit Sub  
  
ErrorHandler:  
    ' clean up  
    If Not rst Is Nothing Then  
        If rst.State = adStateOpen Then rst.Close  
    End If  
    Set rst = Nothing  
    Set dc = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
'EndHandlerVB  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Oggetto DataControl (RDS)](./datacontrol-object-rds.md)   
 [Proprietà Handler (Servizi Desktop remoto)](./handler-property-rds.md)