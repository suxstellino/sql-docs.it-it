---
description: Accesso alle righe in un recordset gerarchico (esempio)
title: Accesso alle righe in un recordset gerarchico | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- hierarchical Recordsets [ADO]
- data shaping [ADO], hierarchical Recordsets
ms.assetid: 25f1d2a1-6d5e-4457-aa07-5db5c75dee18
author: rothja
ms.author: jroth
ms.openlocfilehash: 369d9ef5548a0ff30b08dc27c9df38b0f3c1fa1b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028636"
---
# <a name="accessing-rows-in-a-hierarchical-recordset-example"></a>Accesso alle righe in un recordset gerarchico (esempio)
Nell'esempio seguente vengono illustrati i passaggi necessari per accedere alle righe in un [Recordset](../../reference/ado-api/recordset-object-ado.md)gerarchico:

1.  Gli oggetti **Recordset** dalle tabelle **authors** e **titleauthor** sono correlati dall'ID autore.

2.  Il ciclo esterno Visualizza il nome, lo stato e l'identificazione di ogni autore.

3.  Il **Recordset** aggiunto per ogni riga viene recuperato dalla raccolta [Fields](../../reference/ado-api/fields-collection-ado.md) e assegnato a *rstTitleAuthor*.

4.  Il ciclo interno Visualizza quattro campi da ogni riga del **Recordset** accodato.

 La proprietà [StayInSync](../../reference/ado-api/stayinsync-property.md) è impostata su **false** a scopo illustrativo, in modo che sia possibile visualizzare la modifica del capitolo in modo esplicito in ogni iterazione del ciclo esterno. Per rendere più efficiente l'esempio di codice, è possibile spostare l'assegnazione nel passaggio 3 prima della prima riga nel passaggio 2, in modo che l'assegnazione venga eseguita una sola volta. Impostare quindi la proprietà [StayInSync](../../reference/ado-api/stayinsync-property.md) su **true**, in modo che *rstTitleAuthor* venga modificato in modo implicito e automatico nel capitolo corrispondente ogni volta che *RST* si sposta in una nuova riga.

## <a name="example"></a>Esempio

```
Sub datashape()
   Dim cnn As New ADODB.Connection
   Dim rst As New ADODB.Recordset
   Dim rstTitleAuthor As New ADODB.Recordset

   cnn.Provider = "MSDataShape"
   cnn.Open    "Data Provider=MSDASQL;" & _
               "Data Source=SRV;Integrated Security=SSPI;Database=Pubs"
' STEP 1
   rst.StayInSync = FALSE
   rst.Open    "SHAPE  {select * from authors} "  & _
               "APPEND ({select * from titleauthor} " & _
               "RELATE au_id TO au_id) AS chapTitleAuthor", _
               cnn
' STEP 2
   While Not rst.EOF
      Debug.Print    rst("au_fname"), rst("au_lname"), _
                     rst("state"), rst("au_id")
' STEP 3
      Set rstTitleAuthor = rst("chapTitleAuthor").Value
' STEP 4
      While Not rstTitleAuthor.EOF
         Debug.Print rstTitleAuthor(0), rstTitleAuthor(1), _
                     rstTitleAuthor(2), rstTitleAuthor(3)
         rstTitleAuthor.MoveNext
      Wend
      rst.MoveNext
   Wend
End Sub
```

## <a name="see-also"></a>Vedere anche
 [Cenni preliminari sulla data shaping (ADO) Cenni preliminari sulla](./data-shaping-overview.md) [](../../reference/ado-api/field-object.md) [](../../reference/ado-api/fields-collection-ado.md) [grammatica di forma formale](./formal-shape-grammar.md) [Microsoft Data Shaping Service per OLE DB (provider di servizi ADO](../appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md) ) [Recordset oggetti (ADO)](../../reference/ado-api/recordset-object-ado.md) [provider necessari per i comandi di](./required-providers-for-data-shaping.md) forma della [clausola APPEND](./shape-append-clause.md) [della](./shape-commands-in-general.md) forma data shaping nella clausola di [calcolo di forma](./shape-compute-clause.md) generale [Visual Basic, Applications Edition funzioni](./visual-basic-for-applications-functions.md)