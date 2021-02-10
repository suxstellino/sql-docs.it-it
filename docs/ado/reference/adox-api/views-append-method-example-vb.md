---
description: Esempio del metodo Append di Views (VB)
title: Esempio di metodo Append views (VB) | Microsoft Docs
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
- Append method [ADOX]
ms.assetid: b5b4c082-ac29-4f49-a8b8-e21b554c9b0d
author: rothja
ms.author: jroth
ms.openlocfilehash: 1cc2df470b2b51176cd44433359cf74d31c36863
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100053382"
---
# <a name="views-append-method-example-vb"></a>Esempio del metodo Append di Views (VB)
Nel codice seguente viene illustrato come utilizzare un oggetto [Command](../ado-api/command-object-ado.md) e il metodo [Append](./append-method-adox-views.md) della raccolta [views](./views-collection-adox.md) per creare una nuova visualizzazione nell'origine dati sottostante.  
  
```  
' BeginCreateViewVB  
Sub Main()  
    On Error GoTo CreateViewError  
  
    Dim cmd As New ADODB.Command  
    Dim cat As New ADOX.Catalog  
  
    ' Open the Catalog  
    cat.ActiveConnection = _  
        "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
        "Data Source='Northwind.mdb';"  
  
    ' Create the command representing the view.  
    cmd.CommandText = "Select * From Customers"  
  
    ' Create the new View  
    cat.Views.Append "AllCustomers", cmd  
  
    'Clean up  
    Set cat.ActiveConnection = Nothing  
    Set cat = Nothing  
    Set cmd = Nothing  
    Exit Sub  
  
CreateViewError:  
  
    Set cat = Nothing  
    Set cmd = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
' EndCreateViewVB  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Proprietà ActiveConnection (ADOX)](./activeconnection-property-adox.md)   
 [Metodo Append (viste ADOX)](./append-method-adox-views.md)   
 [Oggetto Catalog (ADOX)](./catalog-object-adox.md)   
 [Oggetto View (ADOX)](./view-object-adox.md)   
 [Raccolta Views (ADOX)](./views-collection-adox.md)