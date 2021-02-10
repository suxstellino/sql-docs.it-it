---
description: Esempio della proprietà ParentCatalog (VB)
title: Esempio di proprietà ParentCatalog (VB) | Microsoft Docs
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
- ParentCatalog property [ADOX], Visual Basic example
ms.assetid: 448bc850-7584-4c5f-89f3-5f4fee88b259
author: rothja
ms.author: jroth
ms.openlocfilehash: 5f728bb18f57d8f30cf4d576e651f8a20bc385ad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049871"
---
# <a name="parentcatalog-property-example-vb"></a>Esempio della proprietà ParentCatalog (VB)
Nel codice seguente viene illustrato come utilizzare la proprietà [ParentCatalog](./parentcatalog-property-adox.md) per accedere a una proprietà specifica del provider prima di aggiungere una tabella a un catalogo. La proprietà è **AutoIncrement**, che consente di creare un campo di incremento automatico in un database Microsoft Jet.  
  
```  
' BeginCreateAutoIncrColumnVB  
Sub Main()  
    On Error GoTo CreateAutoIncrColumnError  
  
    Dim cnn As New ADODB.Connection  
    Dim cat As New ADOX.Catalog  
    Dim tbl As New ADOX.Table  
  
    cnn.Open "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
        "Data Source='Northwind.mdb';"  
    Set cat.ActiveConnection = cnn  
  
    With tbl  
        .Name = "MyContacts"  
        Set .ParentCatalog = cat  
        ' Create fields and append them to the new Table object.  
        .Columns.Append "ContactId", adInteger  
        ' Make the ContactId column and auto incrementing column  
        .Columns("ContactId").Properties("AutoIncrement") = True  
        .Columns.Append "CustomerID", adVarWChar  
        .Columns.Append "FirstName", adVarWChar  
        .Columns.Append "LastName", adVarWChar  
        .Columns.Append "Phone", adVarWChar, 20  
        .Columns.Append "Notes", adLongVarWChar  
    End With  
  
    cat.Tables.Append tbl  
    Debug.Print "Table 'MyContacts' is added."  
  
    ' Delete the table as this is a demonstration.  
    cat.Tables.Delete tbl.Name  
    Debug.Print "Table 'MyContacts' is delete."  
  
    'Clean up  
    cnn.Close  
    Set cat = Nothing  
    Set tbl = Nothing  
    Set cnn = Nothing  
    Exit Sub  
  
CreateAutoIncrColumnError:  
  
    Set cat = Nothing  
    Set tbl = Nothing  
  
    If Not cnn Is Nothing Then  
        If cnn.State = adStateOpen Then cnn.Close  
    End If  
    Set cnn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
  
End Sub  
' EndCreateAutoIncrColumnVB  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo Append (colonne ADOX)](./append-method-adox-columns.md)   
 [Metodo Append (tabelle ADOX)](./append-method-adox-tables.md)   
 [Oggetto Catalog (ADOX)](./catalog-object-adox.md)   
 [Oggetto Column (ADOX)](./column-object-adox.md)   
 [Raccolta Columns (ADOX)](./columns-collection-adox.md)   
 [Proprietà Name (ADOX)](./name-property-adox.md)   
 [Proprietà ParentCatalog (ADOX)](./parentcatalog-property-adox.md)   
 [Oggetto Table (ADOX)](./table-object-adox.md)   
 [Proprietà Type (Column) (ADOX)](./type-property-column-adox.md)