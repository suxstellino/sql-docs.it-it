---
description: Commenti su HelloData
title: Commenti su HelloData | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- hellodata sample application [ADO]
ms.assetid: a2831d77-7040-4b73-bbae-fe0bf78107ed
author: rothja
ms.author: jroth
ms.openlocfilehash: e0510d75c69caadd52a595d58900662cc8ef7ad4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037711"
---
# <a name="comments-on-hellodata"></a>Commenti su HelloData
L'applicazione HelloData è in grado di eseguire le operazioni di base di una tipica applicazione ADO, ovvero ottenere, esaminare, modificare e aggiornare i dati. Quando si avvia l'applicazione, fare clic sul primo pulsante, **recuperare i dati**. Verrà eseguita la subroutine **GetData** .  
  
## <a name="getdata"></a>GetData  
 **GetData** inserisce una stringa di connessione valida in una variabile a livello di modulo, *m_sConnStr*. Per ulteriori informazioni sulle stringhe di connessione, vedere [creazione della stringa di connessione](./creating-a-connection-string.md).  
  
 Assegnare un gestore errori usando un'istruzione Visual Basic **OnError** . Per ulteriori informazioni sulla gestione degli errori in ADO, vedere [gestione degli errori](./error-handling.md). Viene creato un nuovo oggetto **connessione** e la proprietà **CursorLocation** è impostata su **adUseClient** perché nell'esempio HelloData viene creato un *Recordset disconnesso*. Ciò significa che non appena i dati sono stati recuperati dall'origine dati, la connessione fisica con l'origine dati è interruppe, ma è comunque possibile utilizzare i dati memorizzati nella cache in locale nell'oggetto **Recordset** .  
  
 Una volta aperta la connessione, assegnare una stringa SQL a una variabile (crediti). Quindi, creare un'istanza di un nuovo oggetto **Recordset** , `m_oRecordset1` . Nella riga di codice successiva aprire il **Recordset** sulla **connessione** esistente, passando `sSQL` come origine del **Recordset**. Si aiuta ADO a determinare che la stringa SQL passata come origine per il **Recordset** è una definizione testuale di un comando passando **adCmdText** nell'argomento finale al metodo **Open del recordset** . Questa riga imposta anche **LockType** e **CursorType** associati al **Recordset**.  
  
 La riga di codice successiva imposta la proprietà **MarshalOptions** su **adMarshalModifiedOnly**. **MarshalOptions** indica i record di cui deve essere effettuato il marshalling al livello intermedio (o al server Web). Per ulteriori informazioni sul marshalling, vedere la documentazione COM. Quando si usa **adMarshalModifiedOnly** con un cursore sul lato client ([CursorLocation](../../reference/ado-api/cursorlocation-property-ado.md)  =  **adUseClient**), solo i record modificati sul client vengono riscritti nel livello intermedio. L'impostazione di **MarshalOptions** su **adMarshalModifiedOnly** può migliorare le prestazioni perché viene effettuato il marshalling di un numero minore di righe  
  
 Successivamente, disconnettere il **Recordset** impostando la relativa proprietà **ActiveConnection** su **Nothing**. Per ulteriori informazioni, vedere la sezione relativa alla disconnessione e riconnessione del recordset in [aggiornamento e salvataggio permanente dei dati](./updating-and-persisting-data.md).  
  
 Chiudere la connessione all'origine dati ed eliminare l'oggetto **connessione** esistente. Questa operazione rilascia le risorse utilizzate.  
  
 Il passaggio finale consiste nell'impostare il **Recordset** come **origine** dati per il controllo Microsoft DataGrid sul modulo, in modo da poter visualizzare facilmente i dati del **Recordset** nel form.  
  
 Fare clic sul secondo pulsante, **esaminare i dati**. Viene eseguita la subroutine **ExamineData** .  
  
## <a name="examinedata"></a>ExamineData  
 ExamineData utilizza diversi metodi e proprietà dell'oggetto **Recordset** per visualizzare informazioni sui dati nel **Recordset**. Indica il numero di record utilizzando la proprietà **RecordCount** . Esegue il ciclo del **Recordset** e stampa il valore della proprietà **AbsolutePosition** nella casella di testo di visualizzazione del form. Anche durante il ciclo, il valore della proprietà **Bookmark** per il terzo record viene inserito in una variabile Variant, *vBookmark*, per un uso successivo.  
  
 La routine passa direttamente al terzo record usando la variabile di segnalibro archiviata in precedenza. La routine chiama la subroutine **WalkFields** , che esegue il ciclo della raccolta **Fields** del **Recordset** e Visualizza i dettagli relativi a ogni **campo** nella raccolta.  
  
 Infine, **ExamineData** usa la proprietà **Filter** del **Recordset** per schermare solo i record con un **CategoryID** uguale a 2. Il risultato dell'applicazione di questo filtro è immediatamente visibile nella griglia di visualizzazione del form.  
  
 Per ulteriori informazioni sulle funzionalità illustrate nella subroutine **ExamineData** , vedere [analisi dei dati](./examining-data.md).  
  
 Quindi, fare clic sul terzo pulsante, **Edit data**. Verrà eseguita la subroutine **EditData** .  
  
## <a name="editdata"></a>EditData  
 Quando il codice immette la subroutine **EditData** , il **Recordset** viene comunque filtrato in base a **CategoryID** uguale a 2, in modo che siano visibili solo gli elementi che soddisfano i criteri di filtro. Esegue prima il ciclo del **Recordset** e aumenta il 10% del prezzo di ogni elemento visibile nel **Recordset** . Il valore del campo **Price** viene modificato impostando la proprietà **value** per il campo su un valore nuovo e valido.  
  
 Tenere presente che il **Recordset** è disconnesso dall'origine dati. Le modifiche apportate in **EditData** vengono apportate solo alla copia locale dei dati memorizzata nella cache. Per ulteriori informazioni, vedere [modifica di dati](./editing-data.md).  
  
 Le modifiche non verranno apportate all'origine dati fino a quando non si fa clic sul quarto pulsante **Aggiorna dati**. Verrà eseguita la subroutine **UpdateData** .  
  
## <a name="updatedata"></a>UpdateData  
 UpdateData rimuove innanzitutto il filtro che è stato applicato al **Recordset**. Il codice rimuove e Reimposta `m_oRecordset1` come **origine dati** per il DataGrid associato Microsoft sul form, in modo che il **Recordset** non filtrato venga visualizzato nella griglia.  
  
 Il codice verifica quindi se è possibile spostarsi all'indietro nel **Recordset** usando il metodo **Supports** con l'argomento **adMovePrevious** .  
  
 La routine passa al primo record usando il metodo **MoveFirst** e Visualizza i valori originali e correnti del campo, usando le proprietà **OriginalValue** e **value** dell'oggetto **Field** . Queste proprietà, insieme alla proprietà **UnderlyingValue** (non usato in questo caso), sono descritte in [aggiornamento e salvataggio permanente dei dati](./updating-and-persisting-data.md).  
  
 Successivamente, viene creato un nuovo oggetto **connessione** che viene utilizzato per ristabilire una connessione all'origine dati. Per riconnettere il **Recordset** all'origine dati, impostare la nuova **connessione** come **ActiveConnection** per il **Recordset**. Per inviare gli aggiornamenti al server, il codice chiama **UpdateBatch** sul **Recordset**.  
  
 Se l'aggiornamento del batch riesce, una variabile di flag a livello di modulo, `m_flgPriceUpdated` , viene impostata su true. In questo modo verrà visualizzato un avviso in seguito per pulire tutte le modifiche apportate al database.  
  
 Infine, il codice si sposta nuovamente sul primo record del **Recordset** e Visualizza i valori originali e quelli correnti. I valori sono gli stessi dopo la chiamata a **UpdateBatch**.  
  
 Per informazioni dettagliate su come aggiornare i dati, incluse le operazioni da eseguire quando vengono modificati i dati nel server mentre il **Recordset** è disconnesso, vedere [aggiornamento e salvataggio permanente dei dati](./updating-and-persisting-data.md).  
  
## <a name="form_unload"></a>Form_Unload  
 La subroutine **Form_Unload** è importante per diversi motivi. Innanzitutto, poiché si tratta di un'applicazione di esempio, Form_Unload pulisce le modifiche apportate al database prima che l'applicazione venga chiusa. In secondo luogo, il codice Mostra come un comando può essere eseguito direttamente da un oggetto **connessione** aperto tramite il metodo **Execute** . Infine, viene illustrato un esempio di esecuzione di una query che non restituisce righe (una query di aggiornamento) rispetto all'origine dati.