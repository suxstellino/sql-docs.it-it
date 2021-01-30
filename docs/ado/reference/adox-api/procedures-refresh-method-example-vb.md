---
description: Esempio del metodo Refresh di Procedures (VB)
title: Esempio di metodo Refresh di Procedures (VB) | Microsoft Docs
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
- Refresh method [ADOX], Visual Basic example
ms.assetid: 499679bd-287b-487d-bdfb-3803abffec1c
author: rothja
ms.author: jroth
ms.openlocfilehash: ebe7301fe71d90033aeee5ec72b9a23bcaa922be
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169233"
---
# <a name="procedures-refresh-method-example-vb"></a>Esempio del metodo Refresh di Procedures (VB)
Nel codice seguente viene illustrato come aggiornare la raccolta [Procedures](./procedures-collection-adox.md) di un [Catalogo](./catalog-object-adox.md). Questa operazione è necessaria prima che sia possibile accedere agli oggetti [procedure](./procedure-object-adox.md) del **Catalogo** .  
  
```  
' BeginProceduresRefreshVB  
Sub Main()  
    On Error GoTo ProcedureRefreshError  
  
    Dim cat As New ADOX.Catalog  
  
    ' Open the Catalog  
    cat.ActiveConnection = _  
        "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
        "Data Source='Northwind.mdb';"  
  
    ' Refresh the Procedures collection  
    cat.Procedures.Refresh  
  
    'Clean up  
    Set cat.ActiveConnection = Nothing  
    Set cat = Nothing  
    Exit Sub  
  
ProcedureRefreshError:  
    Set cat = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
' EndProceduresRefreshVB  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Oggetto Catalog (ADOX)](./catalog-object-adox.md)   
 [Raccolta Procedures (ADOX)](./procedures-collection-adox.md)   
 [Metodo Refresh (ADO)](../ado-api/refresh-method-ado.md)