---
description: Esempio del metodo Refresh di Views (VB)
title: Esempio di metodo Refresh views (VB) | Microsoft Docs
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
- Refresh method [ADOX]
ms.assetid: cdad2d66-6ade-40dc-9e74-e40cfa9bc127
author: rothja
ms.author: jroth
ms.openlocfilehash: 0d68a27408a6066ec98851c20df3f03b77381e68
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049561"
---
# <a name="views-refresh-method-example-vb"></a>Esempio del metodo Refresh di Views (VB)
Nel codice seguente viene illustrato come aggiornare la raccolta [views](./views-collection-adox.md) di un [Catalogo](./catalog-object-adox.md). Questa operazione è necessaria prima che sia possibile accedere agli oggetti di [visualizzazione](./view-object-adox.md) del **Catalogo** .  
  
```  
' BeginViewsRefreshVB  
Sub Main()  
    On Error GoTo ProcedureViewsRefreshError  
  
    Dim cat As New ADOX.Catalog  
  
    ' Open the catalog.  
    cat.ActiveConnection = "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
        "Data Source='Northwind.mdb';"  
  
    ' Refresh the Procedures collection.  
    cat.Views.Refresh  
  
    'Clean up  
    Set cat = Nothing  
    Exit Sub  
  
ProcedureViewsRefreshError:  
  
    If Not cat Is Nothing Then  
        Set cat = Nothing  
    End If  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
' EndViewsRefreshVB  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo Refresh (ADO)](../ado-api/refresh-method-ado.md)   
 [Raccolta Views (ADOX)](./views-collection-adox.md)