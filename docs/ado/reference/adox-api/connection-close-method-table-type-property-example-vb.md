---
description: Esempio del metodo Close di Connection e della proprietà Type di Table (VB)
title: Metodo di chiusura della connessione, esempio di proprietà del tipo di tabella (VB) | Microsoft Docs
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
- Close method [ADOX], Visual Basic example
- Type property [ADOX], Visual Basic example
ms.assetid: f88e7a3b-19ed-46e2-b2ce-3b611d9b8166
author: rothja
ms.author: jroth
ms.openlocfilehash: d5049e6d1e60a2a54633c1fa0fe7adacb0685690
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172214"
---
# <a name="connection-close-method-table-type-property-example-vb"></a>Esempio del metodo Close di Connection e della proprietà Type di Table (VB)
Se si imposta la proprietà [ActiveConnection](./activeconnection-property-adox.md) su **Nothing** , la connessione al catalogo verrà chiusa. Le raccolte associate saranno vuote. Tutti gli oggetti creati dagli oggetti dello schema nel catalogo saranno orfani. Eventuali proprietà degli oggetti memorizzati nella cache saranno comunque disponibili, ma un tentativo di leggere le proprietà che richiedono una chiamata al provider avrà esito negativo.  
  
```  
' BeginCloseConnectionVB  
Sub Main()  
    On Error GoTo CloseConnectionByNothingError  
  
    Dim cnn As New ADODB.Connection  
    Dim cat As New ADOX.Catalog  
    Dim tbl As ADOX.Table  
  
    cnn.Open "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
        "Data Source= 'Northwind.mdb';"  
    Set cat.ActiveConnection = cnn  
    Set tbl = cat.Tables(0)  
    Debug.Print tbl.Type    ' Cache tbl.Type info  
    Set cat.ActiveConnection = Nothing  
    Debug.Print tbl.Type    ' tbl is orphaned  
    ' Previous line will succeed if this info was cached.  
    Debug.Print tbl.Columns(0).DefinedSize  
    ' Previous line will fail if this info has not been cached.  
  
    'Clean up.  
    cnn.Close  
    Set cat = Nothing  
    Set cnn = Nothing  
    Exit Sub  
  
CloseConnectionByNothingError:  
    Set cat = Nothing  
  
    If Not cnn Is Nothing Then  
        If cnn.State = adStateOpen Then cnn.Close  
    End If  
    Set cnn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
' EndCloseConnectionVB  
```  
  
 La chiusura di un oggetto [Connection](../ado-api/connection-object-ado.md) utilizzato per aprire il catalogo deve avere lo stesso effetto dell'impostazione della proprietà **ActiveConnection** su **Nothing**.  
  
```  
Attribute VB_Name = "Connection"  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Proprietà ActiveConnection (ADOX)](./activeconnection-property-adox.md)   
 [Oggetto Catalog (ADOX)](./catalog-object-adox.md)   
 [Oggetto Column (ADOX)](./column-object-adox.md)   
 [Raccolta Columns (ADOX)](./columns-collection-adox.md)   
 [Oggetto Table (ADOX)](./table-object-adox.md)   
 [Raccolta Tables (ADOX)](./tables-collection-adox.md)   
 [Proprietà Type (Table) (ADOX)](./type-property-table-adox.md)