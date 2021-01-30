---
description: Esempio della proprietà DeleteRule (VB)
title: Esempio di proprietà DeleteRule (VB) | Microsoft Docs
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
- DeleteRule property [ADOX], Visual Basic example
ms.assetid: 9ba00118-a80d-4a6d-a7d6-4f5492fb7ded
author: rothja
ms.author: jroth
ms.openlocfilehash: 5e06fe258c1baaa01eff25678c312cdb087f9b6c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172094"
---
# <a name="deleterule-property-example-vb"></a>Esempio della proprietà DeleteRule (VB)
In questo esempio viene illustrata la proprietà [DeleteRule](./deleterule-property-adox.md) di un oggetto [Key](./key-object-adox.md) . Il codice aggiunge una nuova [tabella](./table-object-adox.md) e quindi definisce una nuova chiave primaria, impostando **DeleteRule** su **adRICascade**.  
  
```  
' BeginDeleteRuleVB  
Sub Main()  
    On Error GoTo DeleteRuleXError  
  
    Dim kyPrimary As New ADOX.Key  
    Dim cat As New ADOX.Catalog  
    Dim tblNew As New ADOX.Table  
  
    ' Connect the catalog  
    cat.ActiveConnection = "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
                     "data source='Northwind.mdb';"  
  
    ' Name new table  
    tblNew.Name = "NewTable"  
  
    ' Append a numeric and a text field to new table.  
    tblNew.Columns.Append "NumField", adInteger, 20  
    tblNew.Columns.Append "TextField", adVarWChar, 20  
  
    ' Append the new table  
    cat.Tables.Append tblNew  
  
    ' Define the Primary key  
    kyPrimary.Name = "NumField"  
    kyPrimary.Type = adKeyPrimary  
    kyPrimary.RelatedTable = "Customers"  
    kyPrimary.Columns.Append "NumField"  
    kyPrimary.Columns("NumField").RelatedColumn = "CustomerId"  
    kyPrimary.DeleteRule = adRICascade  
  
    ' Append the primary key  
    cat.Tables("NewTable").Keys.Append kyPrimary  
    Debug.Print "The primary key is appended."  
  
    'Delete the table as this is a demonstration.  
    cat.Tables.Delete tblNew.Name  
    Debug.Print "The primary key is deleted."  
  
    'Clean up  
    Set cat.ActiveConnection = Nothing  
    Set cat = Nothing  
    Set kyPrimary = Nothing  
    Set tblNew = Nothing  
    Exit Sub  
  
DeleteRuleXError:  
  
    Set cat = Nothing  
    Set kyPrimary = Nothing  
    Set tblNew = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
  
End Sub  
' EndDeleteRuleVB  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Proprietà DeleteRule (ADOX)](./deleterule-property-adox.md)   
 [Oggetto Key (ADOX)](./key-object-adox.md)