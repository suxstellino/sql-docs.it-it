---
description: Oggetto Field
title: Oggetto Field | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Field object [ADO]
ms.assetid: 7d1c4ad5-4be3-42ab-b516-e7133ca300bc
author: rothja
ms.author: jroth
ms.openlocfilehash: bf3a704b26e35c77d9ba7ba059978091002cc424
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036940"
---
# <a name="the-field-object"></a>Oggetto Field
Ogni oggetto **campo** corrisponde in genere a una colonna in una tabella di database. Tuttavia, un **campo** può anche rappresentare un puntatore a un altro **Recordset**, denominato capitolo. Le eccezioni, ad esempio le colonne del capitolo, verranno descritte più avanti in questa guida.  
  
 Utilizzare la proprietà **value** degli oggetti **Field** per impostare o restituire i dati per il record corrente. A seconda delle funzionalità esposte dal provider, alcune raccolte, metodi o proprietà di un oggetto **campo** potrebbero non essere disponibili.  
  
 Con le raccolte, i metodi e le proprietà di un oggetto **campo** , è possibile eseguire le operazioni seguenti:  
  
-   Restituisce il nome di un campo utilizzando la proprietà **Name** .  
  
-   Consente di visualizzare o modificare i dati nel campo utilizzando la proprietà **value** . **Value** è la proprietà predefinita dell'oggetto **Field** .  
  
-   Restituire le caratteristiche di base di un campo utilizzando le proprietà **Type**, **Precision** e **NumericScale** .  
  
-   Restituisce la dimensione dichiarata di un campo tramite la proprietà **DefinedSize** .  
  
-   Restituisce le dimensioni effettive dei dati in un campo specifico tramite la proprietà **ActualSize** .  
  
-   Determinare quali tipi di funzionalità sono supportati per un determinato campo usando la proprietà **Attributes** e la raccolta **Properties** .  
  
-   Modificare i valori dei campi contenenti dati di tipo carattere lungo o binario lungo usando i metodi **AppendChunk** e **GetChunk** .  
  
 Risolvere le discrepanze nei valori di campo durante l'aggiornamento batch usando le proprietà **OriginalValue** e **UnderlyingValue** , se il provider supporta gli aggiornamenti in batch.  
  
## <a name="describing-a-field"></a>Descrizione di un campo  
 Gli argomenti seguenti illustrano le proprietà dell'oggetto [campo](../../../ado/reference/ado-api/field-object.md) che rappresentano le informazioni che descrivono l'oggetto **campo** stesso, ovvero i metadati relativi al campo. Queste informazioni possono essere utilizzate per determinare gran parte dello schema del **Recordset**. Queste proprietà includono **Type**, **DefinedSize** e **ActualSize**, **Name** e **NumericScale** e **Precision**.  
  
### <a name="discovering-the-data-type"></a>Individuazione del tipo di dati  
 La proprietà **Type** indica il tipo di dati del campo. Le costanti enumerate del tipo di dati supportate da ADO sono descritte in [DataTypeEnum](../../../ado/reference/ado-api/datatypeenum.md) nella Guida *di riferimento per programmatori ADO*.  
  
 Per i tipi numerici a virgola mobile, ad esempio **adNumeric**, è possibile ottenere altre informazioni. La proprietà **NumericScale** indica il numero di cifre a destra del separatore decimale che verranno utilizzate per rappresentare i valori per il **campo**. La proprietà **Precision** specifica il numero massimo di cifre utilizzate per rappresentare i valori per il **campo**.  
  
### <a name="determining-field-size"></a>Determinazione delle dimensioni del campo  
 Utilizzare la proprietà **DefinedSize** per determinare la capacità dei dati di un oggetto **campo** .  
  
 Utilizzare la proprietà **ActualSize** per restituire la lunghezza effettiva del valore di un oggetto **campo** . Per tutti i campi, la proprietà **ActualSize** è di sola lettura. Se ADO non è in grado di determinare la lunghezza del valore dell'oggetto **campo** , la proprietà **ActualSize** restituisce **adUnknown**.  
  
 Le proprietà **DefinedSize** e **ActualSize** hanno scopi diversi. Si consideri, ad esempio, un oggetto **campo** con un tipo dichiarato di **adVarChar** e un valore della proprietà **DefinedSize** di 50, contenente un singolo carattere. Il valore della proprietà **ActualSize** restituito è la lunghezza in byte del singolo carattere.  
  
### <a name="determining-field-contents"></a>Determinazione del contenuto del campo  
 L'identificatore della colonna dall'origine dati è rappresentato dalla proprietà **Name** del **campo**. La proprietà **value** dell'oggetto **Field** restituisce o imposta il contenuto effettivo dei dati del campo. Si tratta della proprietà predefinita.  
  
 Per modificare i dati in un campo, impostare la proprietà **value** su un nuovo valore del tipo corretto. Il tipo di cursore deve supportare gli aggiornamenti per modificare il contenuto di un campo. La convalida del database non viene eseguita in modalità batch, quindi è necessario verificare la presenza di errori quando si chiama **UpdateBatch** in un caso di questo tipo. Alcuni provider supportano anche le proprietà **UnderlyingValue** e **OriginalValue** dell'oggetto **campo** ADO per facilitare la risoluzione dei conflitti quando si tenta di eseguire gli aggiornamenti in batch. Per informazioni dettagliate su come risolvere tali conflitti, vedere [modifica di dati](../../../ado/guide/data/editing-data.md).  
  
> [!NOTE]
>  Impossibile impostare i valori dei **campi del recordset** quando si aggiungono nuovi **campi** a un **Recordset**. È invece possibile aggiungere nuovi **campi** a un **Recordset** chiuso. Il **Recordset** deve quindi essere aperto e solo i valori possono essere assegnati a questi **campi**.  
  
### <a name="getting-more-field-information"></a>Recupero di altre informazioni sui campi  
 Gli oggetti ADO hanno due tipi di proprietà: incorporata e dinamica. Fino a questo punto, sono state illustrate solo le proprietà predefinite dell'oggetto **Field** .  
  
 Le proprietà predefinite sono quelle implementate in ADO e immediatamente disponibili per qualsiasi nuovo oggetto, usando la `MyObject.Property` sintassi. Non vengono visualizzati come oggetti **Proprietà** nella raccolta delle **Proprietà** di un oggetto.  
  
 Le proprietà dinamiche sono definite dal provider di dati sottostante e vengono visualizzate nella raccolta **Properties** per l'oggetto ADO appropriato. Una proprietà specifica del provider, ad esempio, può indicare se un oggetto **Recordset** supporta le transazioni o l'aggiornamento di. Queste proprietà aggiuntive verranno visualizzate come oggetti **Proprietà** nella raccolta delle **proprietà** dell'oggetto **Recordset** . È possibile fare riferimento alle proprietà dinamiche solo tramite la raccolta, usando la sintassi `MyObject.Properties(0)` o `MyObject.Properties("Name")` .  
  
 Non è possibile eliminare entrambi i tipi di proprietà.  
  
 Un oggetto **Proprietà** dinamica dispone di quattro proprietà predefinite:  
  
-   La proprietà **Name** è una stringa che identifica la proprietà.  
  
-   La proprietà **Type** è un numero intero che specifica il tipo di dati della proprietà.  
  
-   La proprietà **value** è una variante che contiene l'impostazione della proprietà. **Value** è la proprietà predefinita per un oggetto **Property** .  
  
-   La proprietà **Attributes** è un valore **Long** che indica le caratteristiche della proprietà specifica del provider.  
  
 La raccolta **Properties** per l'oggetto **Field** contiene metadati aggiuntivi sul campo. Il contenuto di questa raccolta varia a seconda del provider. Nell'esempio di codice seguente viene esaminata la raccolta **Properties** del **Recordset** di esempio introdotto all'inizio di questa sezione. Esamina prima di tutto il contenuto della raccolta. Questo codice usa il [provider di OLE DB per SQL Server](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-sql-server.md), quindi la raccolta **Properties** contiene informazioni rilevanti per tale provider.  
  
```  
'BeginFieldProps  
    Dim objProp As ADODB.Property  
  
    For intLoop = 0 To (objFields.Count - 1)  
        Debug.Print objFields.Item(intLoop).Name  
  
        For Each objProp In objFields(intLoop).Properties  
            Debug.Print vbTab & objProp.Name & " = " & objProp.Value  
        Next objProp  
    Next intLoop  
'EndFieldProps  
```  
  
### <a name="dealing-with-binary-data"></a>Gestione dei dati binari  
 Usare il metodo [AppendChunk](../../../ado/reference/ado-api/appendchunk-method-ado.md) su un oggetto **campo** per inserire dati di tipo binary o character lunghi. Nelle situazioni in cui la memoria di sistema è limitata, è possibile usare il metodo **AppendChunk** per modificare i valori Long in parti piuttosto che interamente.  
  
 Se il bit **adFldLong** nella proprietà **Attributes** di un oggetto **Field** è impostato su **true**, è possibile usare il metodo **AppendChunk** per quel campo.  
  
 La prima chiamata di **AppendChunk** su un oggetto **campo** scrive i dati nel campo, sovrascrivendo eventuali dati esistenti. Le chiamate successive a **AppendChunk** vengono aggiunte ai dati esistenti. Se si aggiungono dati a un campo e si imposta o si legge il valore di un altro campo nel record corrente, ADO presuppone che sia stata completata l'aggiunta dei dati al primo campo. Se si chiama nuovamente il metodo **AppendChunk** sul primo campo, ADO interpreta la chiamata come nuova operazione **AppendChunk** e sovrascrive i dati esistenti. L'accesso ai campi di altri oggetti **Recordset** che non sono cloni del primo oggetto **Recordset** non interferisce con le operazioni **AppendChunk** .  
  
 Usare il metodo **GetChunk** su un oggetto **campo** per recuperare parte o tutti i dati di tipo carattere o binario lungo. In situazioni in cui la memoria di sistema è limitata, è possibile usare il metodo **GetChunk** per modificare i valori Long in parti, anziché nel loro complesso.  
  
 I dati restituiti da una chiamata **GetChunk** sono assegnati alla *variabile*. Se le *dimensioni* sono maggiori dei dati rimanenti, il metodo **GetChunk** restituisce solo i dati rimanenti senza *variabile* di riempimento con spazi vuoti. Se il campo è vuoto, il metodo **GetChunk** restituisce un valore null.  
  
 Ogni chiamata a **GetChunk** successiva recupera i dati a partire dal punto in cui è stata interrotta la chiamata **GetChunk** precedente. Tuttavia, se si recuperano dati da un campo e quindi si imposta o si legge il valore di un altro campo nel record corrente, ADO presuppone che sia stato completato il recupero dei dati dal primo campo. Se si chiama nuovamente il metodo **GetChunk** sul primo campo, ADO interpreta la chiamata come nuova operazione **GetChunk** e inizia la lettura dall'inizio dei dati. L'accesso ai campi di altri oggetti **Recordset** che non sono cloni del primo oggetto **Recordset** non interferisce con le operazioni **GetChunk** .  
  
 Se il bit **adFldLong** nella proprietà **Attributes** di un oggetto **Field** è impostato su **true**, è possibile usare il metodo **GetChunk** per quel campo.  
  
 Se non è presente alcun record corrente quando si usa il metodo **GetChunk** o **AppendChunk** su un oggetto **Field** , viene generato l'errore 3021 (nessun record corrente).  
  
 Per un esempio relativo all'uso di questi metodi per modificare i dati binari, vedere gli esempi di metodo [AppendChunk](../../../ado/reference/ado-api/appendchunk-method-ado.md) e [GetChunk](../../../ado/reference/ado-api/getchunk-method-ado.md) nella Guida *di riferimento per programmatori ADO*.
