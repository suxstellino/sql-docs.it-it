---
description: Eliminazione di record con il metodo Delete
title: Eliminazione di record tramite il metodo Delete | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- ADO, deleting records
- deleting records [ADO]
- editing data [ADO], Delete method
- Delete method [ADO]
ms.assetid: bfed5cfa-7f57-463b-9da2-0c612a079d30
author: rothja
ms.author: jroth
ms.openlocfilehash: 43695feb469483bfe1b3a7d9e629adb7d2f7d4e5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033261"
---
# <a name="deleting-records-using-the-delete-method"></a>Eliminazione di record con il metodo Delete
L'utilizzo del metodo **Delete** contrassegna il record corrente o un gruppo di record in un oggetto **Recordset** per l'eliminazione. Se l'oggetto **Recordset** non consente l'eliminazione dei record, si verificherà un errore. Se si è in modalità di aggiornamento immediato, le eliminazioni vengono eseguite immediatamente nel database. Se un record non può essere eliminato correttamente (ad esempio, a causa di violazioni di integrità del database), il record rimarrà in modalità di modifica dopo la chiamata a **Update.** Ciò significa che è necessario annullare l'aggiornamento usando [CancelUpdate](../../reference/ado-api/cancelupdate-method-ado.md) prima di spostare il record corrente (ad esempio, con [Close](../../reference/ado-api/close-method-ado.md), [Move](../../reference/ado-api/move-method-ado.md)o [NextRecordset](../../reference/ado-api/nextrecordset-method-ado.md)).  
  
 Se si è in modalità di aggiornamento batch, i record vengono contrassegnati per l'eliminazione dalla cache e l'effettiva eliminazione viene eseguita quando si chiama il metodo **UpdateBatch** . Per visualizzare i record eliminati, impostare la proprietà **Filter** su **adFilterAffectedRecords** dopo la chiamata a **Delete** .  
  
 Il tentativo di recuperare i valori dei campi dal record eliminato genera un errore. Dopo l'eliminazione del record corrente, il record eliminato rimane aggiornato fino a quando non si passa a un record diverso. Quando ci si allontana dal record eliminato, non è più accessibile.  
  
 Se le eliminazioni vengono annidate in una transazione, è possibile recuperare i record eliminati utilizzando il metodo **RollbackTrans** . Se si è in modalità di aggiornamento batch, è possibile annullare un'eliminazione in sospeso o un gruppo di eliminazioni in sospeso usando il metodo **CancelBatch** .  
  
 Se il tentativo di eliminare i record ha esito negativo a causa di un conflitto con i dati sottostanti (ad esempio, un record è già stato eliminato da un altro utente), il provider restituisce gli avvisi alla raccolta degli **errori** , ma non interrompe l'esecuzione del programma. Si verifica un errore di run-time solo se sono presenti conflitti in tutti i record richiesti.  
  
 Se la proprietà dinamica **tabella univoca** è impostata e il **Recordset** è il risultato dell'esecuzione di un'operazione di join su più tabelle, il metodo **Delete** eliminerà le righe solo dalla tabella denominata nella proprietà **Unique Table** .  
  
 Il metodo **Delete** accetta un argomento facoltativo che consente di specificare quali record sono interessati dall'operazione di **eliminazione** . Gli unici valori validi per questo argomento sono le seguenti costanti enumerate ADO **AffectEnum** :  
  
-   **adAffectCurrent** Influiscono solo sul record corrente.  
  
-   **adAffectGroup** Interessa solo i record che soddisfano l'impostazione della proprietà **filtro** corrente. Per usare questa opzione, è necessario impostare la proprietà **Filter** su un valore **FilterGroupEnum** o su una matrice di **segnalibri** .  
  
 Il codice seguente illustra un esempio di come specificare **adAffectGroup** quando si chiama il metodo **Delete** . In questo esempio vengono aggiunti alcuni record al **Recordset** di esempio e viene aggiornato il database. Quindi filtra il **Recordset** usando la costante enumerata del filtro **adFilterAffectedRecords** , che lascia solo i record appena aggiunti visibili nel **Recordset.** Infine, viene chiamato il metodo **Delete** e viene specificato che tutti i record che soddisfano l'impostazione della proprietà **Filter** corrente (i nuovi record) devono essere eliminati.  
  
```  
'BeginDeleteGroup  
    'add some bogus records  
    With objRs  
        For i = 0 To 8  
            .AddNew  
            .Fields("CompanyName") = "Shipper Number " & i + 1  
            .Fields("Phone") = "(425) 555-000" & (i + 1)  
            .Update  
        Next i  
  
        ' update  
        .UpdateBatch  
  
        'filter on newly added records  
        .Filter = adFilterAffectedRecords  
        Debug.Print "Deleting the " & .RecordCount & _  
                    " records you just added."  
  
        'delete the newly added bogus records  
        .Delete adAffectGroup  
        .Filter = adFilterNone  
        Debug.Print .RecordCount & " records remain."  
    End With  
'EndDeleteGroup  
```