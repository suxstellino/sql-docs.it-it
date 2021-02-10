---
description: Limiti di un recordset
title: Limiti di un recordset | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- EOF property [ADO], boundaries of a Recordset
- Recordset object [ADO], boundaries of a Recordset
- BOF property [ADO], boundaries of a Recordset
ms.assetid: c0dd4a0f-478d-4c5e-b5d5-7535f211d064
author: rothja
ms.author: jroth
ms.openlocfilehash: f737ad11af65045a1d923f427c7487e4b751aaa8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028016"
---
# <a name="boundaries-of-a-recordset"></a>Limiti di un recordset
**Recordset** supporta le proprietà **BOF** e **EOF** per delineare rispettivamente l'inizio e la fine del set di dati. È possibile pensare a **BOF** e **EOF** come record "fantasma" posizionati all'inizio e alla fine del **Recordset**. Il conteggio di **BOF** e **EOF**, il **Recordset** di esempio è ora simile al seguente:  
  
|ProductID|ProductName|UnitPrice|  
|---------------|-----------------|---------------|  
|BOF|||  
|7|Pere organiche secche di zio Bob|30,0000|  
|14|Tofu|23,2500|  
|28|Crauti Rssle|45,6000|  
|51|Mele Manjimup secche|53,0000|  
|74|Tofu Longlife|10,0000|  
|EOF|||  
  
 Quando un cursore si sposta oltre l'ultimo record, **EOF** viene impostato su **true**; in caso contrario, il valore è **false**. Analogamente, quando il cursore si sposta prima del primo record, **BOF** è impostato su **true**; in caso contrario, il valore è **false**. Queste proprietà vengono comunemente usate per enumerare i record nel set di dati, come illustrato nel frammento di codice JScript seguente.  
  
```  
while (objRecordset.EOF != true)   
{  
   // Work on the current record.  
   ...  
   // Advance the cursor forward to the next record.  
   objRecordset.MoveNext();  
}  
or  
while (objRecordset.BOF != true)   
{  
   // Work on the current record.  
   ...  
   // Move the cursor to the previous record.  
   objRecordset.MovePrevious();  
}  
```  
  
 Se il valore di **BOF** e **EOF** è **true**, l'oggetto **Recordset** è vuoto. Entrambe le proprietà saranno **false** per un oggetto **Recordset** appena aperto e non vuoto. È possibile utilizzare le proprietà **BOF** e **EOF** insieme per determinare se un oggetto **Recordset** è vuoto o meno, come illustrato nel frammento di codice JScript seguente.  
  
```  
if (objRecordset.EOF == true && objRecordset.BOF == true)  
{  
   WScript.Echo("we got an empty dataset.");  
}  
else  
{  
   WScript.Echo("we got a full dataset.");  
}  
```  
  
 Questo schema funziona per tutti i tipi di cursore ed è indipendente dai provider sottostanti. Se si tenta di determinare il vuoto di un oggetto **Recordset** controllando se il valore della proprietà **RecordCount** è zero (0) o meno, è necessario adottare le precauzioni per usare un cursore e un provider appropriati che supportano la restituzione del numero di record nel risultato.  
  
 Se si elimina l'ultimo record rimanente nell'oggetto **Recordset** , il cursore viene lasciato in uno stato indeterminato. Le proprietà **BOF** e **EOF** possono rimanere **false** fino a quando non si tenta di riposizionare il record corrente, a seconda del provider. Per ulteriori informazioni, vedere [eliminazione di record tramite il metodo Delete](./deleting-records-using-the-delete-method.md).