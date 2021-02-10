---
description: Record corrente e dimensioni del recordset
title: Record e dimensioni correnti del recordset | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- record location [ADO]
- current record [ADO]
ms.assetid: e63ff331-8655-4be7-82c6-e6cd6cc9d16d
author: rothja
ms.author: jroth
ms.openlocfilehash: 791ca36a7f5a61a0ad0208753386fba826d6d06e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033341"
---
# <a name="current-record-and-size-of-recordset"></a>Record corrente e dimensioni del recordset
In questa sezione viene descritto come individuare la posizione corrente del cursore nel **Recordset** di esempio nell' [esempio di codice JScript per restituire un recordset](./jscript-code-example-to-return-a-recordset.md).  
  
## <a name="current-record"></a>Record corrente  
 Il record corrente nel set di dati corrisponde a quello indicato dalla posizione del cursore dell'oggetto **Recordset** . Quando un oggetto **Recordset** viene restituito dall'origine dati come risultato della chiamata a **Recordset. Open**, **Command.Execute** o **Connection.Execute** (compresi **Connection. NamedCommand** e **Connection. StoredProcedure**), il cursore è impostato in modo da puntare al primo record. Nel set di dati di esempio, il record corrente iniziale è l'elemento "pera" biologico a secco "di Uncle Bob.  
  
## <a name="size-of-recordset"></a>Dimensioni del recordset  
 Per individuare le dimensioni di un oggetto **Recordset** , ottenere il valore della proprietà **Recordset. RecordCount** . Questo valore è un valore long integer che indica il numero di record nel **Recordset**. Se il set di dati viene restituito dal provider OLEDB per Microsoft SQL Server, questo valore restituisce il numero di righe restituite. La lettura della proprietà **RecordCount** in un **Recordset** chiuso causa un errore.  
  
 Se non è possibile determinare il numero di record, il valore della proprietà è-1.  
  
 Il valore della proprietà **RecordCount** dipende anche dalle funzionalità del provider e dal tipo di cursore utilizzato. Per un cursore di sola trasmissione, il valore è-1. Per un cursore statico o keyset, il valore è il numero effettivo di record restituiti nell'oggetto **Recordset** . Per un cursore dinamico, il valore è-1 o il numero effettivo di record, a seconda dell'origine dati.  
  
 Un cursore che supporta **RecordCount** deve funzionare più duramente e pertanto richiede una potenza di elaborazione maggiore di quella di un cursore che non supporta **RecordCount**. Se non è necessario ottenere informazioni sul numero di record, l'utilizzo di un tipo di cursore diverso potrebbe contribuire a migliorare le prestazioni dell'applicazione, soprattutto se è necessario gestire un set di dati di grandi dimensioni.  
  
 In alcuni casi, un provider o un cursore non è in grado di determinare il valore **RecordCount** senza prima recuperare tutti i record dall'origine dati. Per garantire un conteggio accurato, chiamare il **Recordset**. Metodo **MoveLast** prima della chiamata a **Recordset. RecordCount**.  
  
 L'oggetto **Recordset** di esempio ottenuto utilizzando l' [esempio di codice JScript](./jscript-code-example-to-return-a-recordset.md) utilizza un cursore di sola trasmissione. Pertanto, la chiamata di **RecordCount** su questo oggetto restituisce sempre-1. Se si modifica la riga di codice che chiama il **Recordset**. Metodo **Open** , come illustrato nell'esempio seguente, la proprietà **RecordCount** restituirà il numero effettivo di record recuperati.  
  
```  
oRs.Open sSQL, sCnStr, adOpenStatic, adLockOptimistic, adCmdText   
```  
  
 Ciò è dovuto al fatto che i cursori statici con [provider Microsoft OLE DB per SQL Server](../appendixes/microsoft-ole-db-provider-for-sql-server.md) supportano **RecordCount**. In questo esempio sono presenti cinque record, quindi **RecordCount** deve restituire il valore 5.  
  
 Questa sezione contiene l'argomento seguente.  
  
 [Limiti di un recordset](./boundaries-of-a-recordset.md)