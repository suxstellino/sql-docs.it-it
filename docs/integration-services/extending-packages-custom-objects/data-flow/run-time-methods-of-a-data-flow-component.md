---
description: Metodi di runtime di un componente del flusso di dati
title: Metodi di runtime di un componente flusso di dati | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- run-time [Integration Services]
- data flow components [Integration Services], run-time methods
ms.assetid: fd9e4317-18dd-43af-bbdc-79db32183ac4
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8fa0106c903a509eb2b4f734b667323b6d71fd74
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343635"
---
# <a name="run-time-methods-of-a-data-flow-component"></a>Metodi di runtime di un componente del flusso di dati

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  In fase di esecuzione l'attività Flusso di dati esamina la sequenza di componenti, prepara un piano di esecuzione e gestisce un pool di thread di lavoro che eseguono il piano di lavoro. L'attività carica righe di dati dalle origini, li elabora tramite trasformazioni, quindi li salva nelle destinazioni.  
  
## <a name="sequence-of-method-execution"></a>Sequenza di esecuzione dei metodi  
 Durante l'esecuzione di un componente del flusso di dati, viene chiamato un subset dei metodi nella classe di base <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent>. I metodi, così come la sequenza con cui vengono chiamati, sono sempre gli stessi, ad eccezione dei metodi <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> e <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>. Questi due metodi vengono chiamati in base all'esistenza e alla configurazione degli oggetti <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSInput100> e <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100> di un componente.  
  
 Nell'elenco seguente sono illustrati i metodi nell'ordine in cui vengono chiamati durante l'esecuzione del componente. Si noti che <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>, se chiamato, viene sempre chiamato prima di <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>.  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.AcquireConnections%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReleaseConnections%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrepareForExecute%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.AcquireConnections%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PostExecute%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReleaseConnections%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Cleanup%2A>  
  
### <a name="primeoutput-method"></a>Metodo PrimeOutput  
 Il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeComponent100.PrimeOutput%2A> viene chiamato quando un componente include almeno un output, connesso a un componente a valle tramite un oggetto <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSPath100>, la cui proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100.SynchronousInputID%2A> è zero. Il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeComponent100.PrimeOutput%2A> viene chiamato per i componenti di origine e per le trasformazioni con output asincroni. A differenza del metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> descritto di seguito, il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> viene chiamato una sola volta per ogni componente che lo richiede.  
  
### <a name="processinput-method"></a>Metodo ProcessInput   
 Il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeComponent100.ProcessInput%2A> viene chiamato per i componenti che includono almeno un input connesso a un componente a monte tramite un oggetto <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSPath100>. Il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeComponent100.ProcessInput%2A> viene chiamato per i componenti di destinazione e per le trasformazioni con output sincroni. <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> viene chiamato ripetutamente finché non sono più disponibili righe da elaborare dei componenti a monte.  
  
## <a name="working-with-inputs-and-outputs"></a>Utilizzo di input e output  
 In fase di esecuzione i componenti del flusso di dati eseguono le attività seguenti:  
  
-   I componenti di origine aggiungono righe.  
  
-   I componenti di trasformazione con output sincroni ricevono le righe fornite dai componenti di origine.  
  
-   I componenti di trasformazione con output asincroni ricevono e aggiungono righe.  
  
-   I componenti di destinazione ricevono righe e quindi le caricano in una destinazione.  
  
 Durante l'esecuzione, l'attività Flusso di dati alloca oggetti <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer> che contengono tutte le colonne definite nelle raccolte di colonne di output di una sequenza di componenti. Ad esempio, se ognuno dei quattro componenti in una sequenza del flusso di dati aggiunge una colonna di output alla raccolta di colonne di output, il buffer fornito a ogni componente contiene quattro colonne, una per ogni colonna di output per componente. A causa di questo comportamento, un componente a volte riceve buffer contenenti colonne che non utilizza.  
  
 Poiché i buffer ricevuti dal componente possono contenere colonne che il componente non utilizzerà, è necessario individuare le colonne che si desidera utilizzare nelle raccolte di colonne di input e output del componente nel buffer fornito al componente dall'attività Flusso di dati. A tale scopo, utilizzare il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSBufferManager100.FindColumnByLineageID%2A> della proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A>. Per motivi di prestazioni, questa attività viene in genere eseguita durante il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A>, anziché in <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> o <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>.  
  
 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A> viene chiamato prima dei metodi <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> e <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> e rappresenta la prima opportunità per un componente di eseguire questa operazione dopo che <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A> diventa disponibile per il componente. Durante questo metodo, il componente deve individuare le proprie colonne nei buffer e archiviare questa informazione internamente affinché le colonne possano essere utilizzate nei metodi <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> o <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>.  
  
 Nell'esempio di codice seguente viene illustrata la modalità con cui un componente di trasformazione con output sincroni individua le proprie colonne di input nel buffer durante <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A>.  
  
```csharp  
private int []bufferColumnIndex;  
public override void PreExecute()  
{  
    IDTSInput100 input = ComponentMetaData.InputCollection[0];  
    bufferColumnIndex = new int[input.InputColumnCollection.Count];  
  
    for( int x=0; x < input.InputColumnCollection.Count; x++)  
    {  
        IDTSInputColumn100 column = input.InputColumnCollection[x];  
        bufferColumnIndex[x] = BufferManager.FindColumnByLineageID( input.Buffer, column.LineageID);  
    }  
}  
```  
  
```vb  
Dim bufferColumnIndex As Integer()  
  
    Public Overrides Sub PreExecute()  
  
        Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)  
  
        ReDim bufferColumnIndex(input.InputColumnCollection.Count)  
  
        For x As Integer = 0 To input.InputColumnCollection.Count  
  
            Dim column As IDTSInputColumn100 = input.InputColumnCollection(x)  
            bufferColumnIndex(x) = BufferManager.FindColumnByLineageID(input.Buffer, column.LineageID)  
  
        Next  
  
    End Sub  
```  
  
### <a name="adding-rows"></a>Aggiunta di righe  
 I componenti forniscono righe ai componenti a valle tramite l'aggiunta di righe agli oggetti <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer>. L'attività Flusso di dati fornisce una matrice di buffer di output, uno per ogni oggetto <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100> connesso a un componente a valle, come parametro per il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>. I componenti di origine e i componenti di trasformazione con output asincroni aggiungono righe ai buffer e quando hanno terminato chiamano il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetEndOfRowset%2A>. L'attività Flusso di dati gestisce i buffer di output che fornisce ai componenti e, quando un buffer diventa pieno, sposta automaticamente le righe presenti al suo interno nel componente successivo. Il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> viene chiamato una sola volta per ogni componente, a differenza del metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> che viene chiamato ripetutamente.  
  
 Nell'esempio di codice seguente viene illustrata la modalità con cui un componente aggiunge righe ai propri buffer di output durante il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>, quindi chiama il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetEndOfRowset%2A>.  
  
```csharp  
public override void PrimeOutput( int outputs, int []outputIDs,PipelineBuffer []buffers)  
{  
    for( int x=0; x < outputs; x++ )  
    {  
        IDTSOutput100 output = ComponentMetaData.OutputCollection.GetObjectByID( outputIDs[x]);  
        PipelineBuffer buffer = buffers[x];  
  
        // TODO: Add rows to the output buffer.  
    }  
    foreach( PipelineBuffer buffer in buffers )  
    {  
        /// Notify the data flow task that no more rows are coming.  
        buffer.SetEndOfRowset();  
    }  
}  
```  
  
```vb  
public overrides sub PrimeOutput( outputs as Integer , outputIDs() as Integer ,buffers() as PipelineBuffer buffers)  
  
    For x As Integer = 0 To outputs.MaxValue  
  
        Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection.GetObjectByID(outputIDs(x))  
        Dim buffer As PipelineBuffer = buffers(x)  
  
        ' TODO: Add rows to the output buffer.  
  
    Next  
  
    For Each buffer As PipelineBuffer In buffers  
  
        ' Notify the data flow task that no more rows are coming.  
        buffer.SetEndOfRowset()  
  
    Next  
  
End Sub  
```  
  
 Per altre informazioni sullo sviluppo di componenti che aggiungono righe ai buffer di output, vedere [Sviluppo di un componente di origine personalizzato](../../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-source-component.md) e [Sviluppo di un componente di trasformazione personalizzato con output asincroni](../../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-asynchronous-outputs.md).  
  
### <a name="receiving-rows"></a>Ricezione di righe  
 I componenti ricevono righe dai componenti a monte in oggetti <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer>. L'attività Flusso di dati fornisce un oggetto <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer> che contiene le righe aggiunte al flusso di dati dai componenti a monte come parametro per il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>. Questo buffer di input può essere utilizzato per esaminare e modificare le righe e le colonne nel buffer, ma non può essere utilizzato per aggiungere o rimuovere righe. Il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> viene chiamato ripetutamente finché non sono più disponibili buffer. L'ultima volta che viene chiamato, la proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.EndOfRowset%2A> è **true**. È possibile scorrere la raccolta di righe nel buffer tramite il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.NextRow%2A>, che fa avanzare il buffer alla riga successiva. Questo metodo restituisce **false** quando il buffer si trova nell'ultima riga della raccolta. Non è necessario controllare la proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.EndOfRowset%2A> a meno che non occorra eseguire un'azione aggiuntiva dopo l'elaborazione delle ultime righe di dati.  
  
 Nel testo riportato di seguito viene illustrato l'utilizzo corretto del metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.NextRow%2A> e della proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.EndOfRowset%2A>:  
  
 `while (buffer.NextRow())`  
  
 `{`  
  
 `// Do something with each row.`  
  
 `}`  
  
 `if (buffer.EndOfRowset)`  
  
 `{`  
  
 `// Optionally, do something after all rows have been processed.`  
  
 `}`  
  
 Nell'esempio di codice seguente viene illustrata la modalità con cui un componente elabora le righe nei buffer di input durante il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>.  
  
```csharp  
public override void ProcessInput( int inputID, PipelineBuffer buffer )  
{  
    {  
        IDTSInput100 input = ComponentMetaData.InputCollection.GetObjectByID(inputID);  
        while( buffer.NextRow())  
        {  
            // TODO: Examine the columns in the current row.  
        }  
}  
```  
  
```vb  
Public Overrides Sub ProcessInput(ByVal inputID As Integer, ByVal buffer As PipelineBuffer)  
  
        Dim input As IDTSInput100 = ComponentMetaData.InputCollection.GetObjectByID(inputID)  
        While buffer.NextRow() = True  
  
            ' TODO: Examine the columns in the current row.  
        End While  
  
End Sub  
```  
  
 Per altre informazioni sullo sviluppo di componenti che ricevono righe nei buffer di input, vedere [Sviluppo di un componente di destinazione personalizzato](../../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-destination-component.md) e [Sviluppo di un componente di trasformazione personalizzato con output sincroni](../../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-synchronous-outputs.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi della fase di progettazione di un componente del flusso di dati](../../../integration-services/extending-packages-custom-objects/data-flow/design-time-methods-of-a-data-flow-component.md)  
  
  
