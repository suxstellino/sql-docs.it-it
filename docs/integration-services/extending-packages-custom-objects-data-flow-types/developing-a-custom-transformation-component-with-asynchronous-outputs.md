---
description: Sviluppo di un componente di trasformazione personalizzato con output asincroni
title: Sviluppo di un componente di trasformazione personalizzato con output asincroni | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- custom data flow components [Integration Services], transformation components
- asynchronous outputs [Integration Services]
- ProcessInput method
- cache [Integration Services]
- buffer allocations [Integration Services]
- transformation components [Integration Services]
- output columns [Integration Services]
- PrimeOutput method
- data flow components [Integration Services], transformation components
ms.assetid: 1c3e92c7-a4fa-4fdd-b9ca-ac3069536274
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d6eeddfc5c4fff29b27462889d39748c8df2a01e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348251"
---
# <a name="developing-a-custom-transformation-component-with-asynchronous-outputs"></a>Sviluppo di un componente di trasformazione personalizzato con output asincroni

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Utilizzare un componente con output asincroni quando una trasformazione non può inviare righe all'output prima che il componente non abbia ricevuto tutte le righe di input oppure quando la trasformazione non produce esattamente una riga di output per ogni riga ricevuta come input. La trasformazione Aggregazione, ad esempio, non può calcolare una somma tra righe prima di aver letto tutte le righe. Viceversa, è possibile utilizzare un componente con output sincroni quando si modifica ogni riga di dati passata. È possibile modificare i dati per ogni riga sul posto oppure creare una o più nuove colonne aggiuntive, ognuna delle quali con un valore per ogni riga di input. Per altre informazioni sulla differenza tra componenti sincroni e asincroni, vedere [Informazioni sulle trasformazioni sincrone e asincrone](../../integration-services/understanding-synchronous-and-asynchronous-transformations.md).  
  
 I componenti di trasformazione con output asincroni sono univoci, perché agiscono contemporaneamente da componenti di destinazione e di origine. Questo tipo di componente riceve righe dai componenti a monte e aggiunge righe che vengono utilizzate dai componenti a valle. Nessun altro componente del flusso di dati esegue entrambe queste operazioni.  
  
 Le colonne dei componenti a monte che sono disponibili per un componente con output sincroni sono automaticamente disponibili per i componenti a valle del componente. Pertanto, un componente con output sincroni non deve necessariamente definire colonne di output per fornire colonne e righe al componente successivo. I componenti con output asincroni, al contrario, devono definire colonne di output e fornire righe ai componenti a valle. Pertanto, un componente con output asincroni deve eseguire più attività durante la fase di progettazione e di esecuzione, mentre lo sviluppatore deve implementare più codice.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] include diverse trasformazioni con output asincroni. La trasformazione Ordinamento, ad esempio, richiede che tutte le righe siano presenti al fine di poter essere ordinate e ottiene tale risultato tramite output asincroni. Dopo aver ricevuto tutte le righe, la trasformazione le ordina e le aggiunge all'output.  
  
 In questa sezione viene illustrato in dettaglio come sviluppare trasformazioni con output asincroni. Per altre informazioni sullo sviluppo di componenti di origine, vedere [Sviluppo di un componente di origine personalizzato](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-source-component.md).  
  
## <a name="design-time"></a>Fase di progettazione  
  
### <a name="creating-the-component"></a>Creazione del componente  
 La proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100.SynchronousInputID%2A> sull'oggetto <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100> identifica se un output è sincrono o asincrono. Per creare un output asincrono, aggiungere l'output al componente e impostare <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100.SynchronousInputID%2A> su zero. L'impostazione di questa proprietà determina anche se l'attività Flusso di dati alloca oggetti <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer> sia per l'input che per l'output del componente o se viene allocato un singolo buffer condiviso tra i due oggetti.  
  
 Nel codice di esempio seguente è illustrato un componente che crea un output asincrono nell'implementazione di <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A>.  
  
```csharp  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.Samples.SqlServer.Dts  
{  
    [DtsPipelineComponent(DisplayName = "AsyncComponent",ComponentType = ComponentType.Transform)]  
    public class AsyncComponent : PipelineComponent  
    {  
        public override void ProvideComponentProperties()  
        {  
            // Call the base class, which adds a synchronous input  
            // and output.  
            base.ProvideComponentProperties();  
  
            // Make the output asynchronous.  
            IDTSOutput100 output = ComponentMetaData.OutputCollection[0];  
            output.SynchronousInputID = 0;  
        }  
    }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
Imports Microsoft.SqlServer.Dts.Runtime  
  
<DtsPipelineComponent(DisplayName:="AsyncComponent", ComponentType:=ComponentType.Transform)> _  
Public Class AsyncComponent  
    Inherits PipelineComponent  
  
    Public Overrides Sub ProvideComponentProperties()  
  
        ' Call the base class, which adds a synchronous input  
        ' and output.  
        Me.ProvideComponentProperties()  
  
        ' Make the output asynchronous.  
        Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection(0)  
        output.SynchronousInputID = 0  
  
    End Sub  
  
End Class  
```  
  
### <a name="creating-and-configuring-output-columns"></a>Creazione e configurazione di colonne di output  
 Come accennato in precedenza, un componente asincrono aggiunge colonne alla propria raccolta di colonne di output per fornire colonne ai componenti a valle. Sono disponibili diversi metodi della fase di progettazione tra cui scegliere, a seconda dei requisiti del componente. Se ad esempio si desidera passare tutte le colonne dai componenti a monte ai componenti a valle, è necessario eseguire l'override del metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.OnInputPathAttached%2A> per aggiungere le colonne, perché si tratta del primo metodo in cui le colonne di input sono disponibili per il componente.  
  
 Se il componente crea colonne di output in base alle colonne selezionate per il proprio input, eseguire l'override del metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.SetUsageType%2A> per selezionare le colonne di output e indicare come verranno utilizzate.  
  
 Se un componente con output asincroni crea colonne di output in base alle colonne dei componenti a monte e le colonne disponibili a valle cambiano, il componente deve aggiornare la propria raccolta di colonne di output. Queste modifiche devono essere rilevate dal componente durante <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A> e corrette durante <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReinitializeMetaData%2A>.  
  
> [!NOTE]  
>  Quando una colonna di output viene rimossa dalla raccolta di colonne di output, i componenti a valle del flusso di dati che fanno riferimento a tale colonna possono essere influenzati negativamente. È necessario correggere la colonna di output senza rimuoverla e ricrearla per evitare l'interruzione dei componenti a valle. Se ad esempio il tipo di dati della colonna è stato modificato, è necessario aggiornarlo.  
  
 Nell'esempio di codice seguente è illustrato un componente che aggiunge una colonna di output alla propria raccolta di colonne di output per ogni colonna disponibile del componente a monte.  
  
```csharp  
public override void OnInputPathAttached(int inputID)  
{  
   IDTSInput100 input = ComponentMetaData.InputCollection.GetObjectByID(inputID);  
   IDTSOutput100 output = ComponentMetaData.OutputCollection[0];  
   IDTSVirtualInput100 vInput = input.GetVirtualInput();  
  
   foreach (IDTSVirtualInputColumn100 vCol in vInput.VirtualInputColumnCollection)  
   {  
      IDTSOutputColumn100 outCol = output.OutputColumnCollection.New();  
      outCol.Name = vCol.Name;  
      outCol.SetDataTypeProperties(vCol.DataType, vCol.Length, vCol.Precision, vCol.Scale, vCol.CodePage);  
   }  
}  
```  
  
```vb  
Public Overrides Sub OnInputPathAttached(ByVal inputID As Integer)  
  
    Dim input As IDTSInput100 = ComponentMetaData.InputCollection.GetObjectByID(inputID)  
    Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection(0)  
    Dim vInput As IDTSVirtualInput100 = input.GetVirtualInput()  
  
    For Each vCol As IDTSVirtualInputColumn100 In vInput.VirtualInputColumnCollection  
  
        Dim outCol As IDTSOutputColumn100 = output.OutputColumnCollection.New()  
        outCol.Name = vCol.Name  
        outCol.SetDataTypeProperties(vCol.DataType, vCol.Length, vCol.Precision, vCol.Scale, vCol.CodePage)  
  
    Next  
End Sub  
```  
  
## <a name="run-time"></a>Fase di esecuzione  
 I componenti con output asincroni eseguono anche una sequenza di metodi della fase di esecuzione diversa rispetto agli altri tipi di componenti. Innanzitutto, si tratta degli unici componenti che ricevono una chiamata ai metodi <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> e <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>. I componenti con output asincroni richiedono inoltre l'accesso a tutte le righe in ingresso prima che possano iniziare l'elaborazione. Pertanto, devono memorizzare le righe di input nella cache interna finché non sono state lette tutte le righe. Infine, a differenza di altri componenti, i componenti con output asincroni ricevono sia un buffer di input che un buffer di output.  
  
### <a name="understanding-the-buffers"></a>Informazioni sui buffer  
 Il componente riceve il buffer di input durante <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>. Questo buffer contiene le righe aggiunte dai componenti a monte. Contiene inoltre le colonne dell'input del componente, oltre alle colonne fornite nell'output di un componente a monte ma che non sono state aggiunte alla raccolta di input del componente asincrono.  
  
 Il buffer di output, fornito al componente in <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>, inizialmente non contiene righe. Il componente aggiunge righe a questo buffer e lo fornisce ai componenti a valle quando è pieno. Il buffer di output contiene le colonne definite nella raccolta di colonne di output del componente, oltre alle eventuali colonne aggiunte da altri componenti a valle ai propri output.  
  
 Questo comportamento è diverso da quello dei componenti con output sincroni, che ricevono un singolo buffer condiviso. Il buffer condiviso di un componente con output sincroni contiene sia le colonne di input che di output del componente, oltre alle colonne aggiunte agli output dei componenti a monte e a valle.  
  
### <a name="processing-rows"></a>Elaborazione di righe  
  
#### <a name="caching-input-rows"></a>Memorizzazione nella cache di righe di input  
 Quando si scrive un componente con output asincroni, è possibile scegliere tra tre diversi modi per aggiungere righe al buffer di output. È possibile aggiungerle non appena vengono ricevute righe di input, memorizzarle nella cache finché il componente non ha ricevuto tutte le righe dal componente a monte oppure aggiungerle quando è appropriato per il componente. Il metodo scelto dipende dai requisiti del componente. Ad esempio, il componente Ordinamento richiede che tutte le righe a monte vengano ricevute prima che sia possibile ordinarle. Pertanto, attende che vengano lette tutte le righe prima di aggiungere righe al buffer di output.  
  
 Il componente deve memorizzare nella cache interna le righe ricevute nel buffer di input finché non è pronto a elaborarle. Le righe del buffer in ingresso possono essere memorizzate nella cache in una tabella di dati, una matrice multidimensionale o qualsiasi altra struttura interna.  
  
#### <a name="adding-output-rows"></a>Aggiunta di righe di output  
 Sia che le righe vengano aggiunte al buffer di output non appena vengono ricevute o dopo la ricezione di tutte, questa operazione viene eseguita chiamando il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.AddRow%2A> sul buffer di output. Dopo aver aggiunto la nuova riga, impostare i valori di ogni colonna al suo interno.  
  
 Poiché a volte il buffer di output contiene più colonne rispetto alla raccolta di colonne di output del componente, è necessario individuare l'indice della colonna appropriata nel buffer prima di impostarne il valore. Il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSBufferManager100.FindColumnByLineageID%2A> della proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A> restituisce l'indice della colonna nella riga del buffer con l'ID di derivazione specificato, che viene quindi utilizzato per assegnare il valore alla colonna di buffer.  
  
 Il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A>, chiamato prima del metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> o del metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>, è il primo metodo in cui è disponibile la proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A> e rappresenta la prima opportunità per individuare gli indici delle colonne dei buffer di input e di output.  
  
## <a name="sample"></a>Esempio  
 Nell'esempio seguente è illustrato un componente di trasformazione semplice con output asincroni che aggiunge righe al buffer di output non appena vengono ricevute. Nell'esempio non vengono dimostrati tutti i metodi e le funzionalità descritti nell'argomento. Vengono descritti i metodi importanti di cui ogni componente di trasformazione personalizzato con output asincroni deve eseguire l'override, ma non è incluso codice per la convalida in fase di progettazione. Inoltre, nel codice del metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> si presuppone che nella raccolta di colonne di output sia inclusa un'unica colonna per ogni colonna presente nella raccolta di colonne di input.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
using Microsoft.SqlServer.Dts.Runtime.Wrapper;  
  
namespace Microsoft.Samples.SqlServer.Dts  
{  
   [DtsPipelineComponent(DisplayName = "AsynchronousOutput")]  
   public class AsynchronousOutput : PipelineComponent  
   {  
      PipelineBuffer outputBuffer;  
      int[] inputColumnBufferIndexes;  
      int[] outputColumnBufferIndexes;  
  
      public override void ProvideComponentProperties()  
      {  
         // Let the base class add the input and output objects.  
         base.ProvideComponentProperties();  
  
         // Name the input and output, and make the  
         // output asynchronous.  
         ComponentMetaData.InputCollection[0].Name = "Input";  
         ComponentMetaData.OutputCollection[0].Name = "AsyncOutput";  
         ComponentMetaData.OutputCollection[0].SynchronousInputID = 0;  
      }  
      public override void PreExecute()  
      {  
         IDTSInput100 input = ComponentMetaData.InputCollection[0];  
         IDTSOutput100 output = ComponentMetaData.OutputCollection[0];  
  
         inputColumnBufferIndexes = new int[input.InputColumnCollection.Count];  
         outputColumnBufferIndexes = new int[output.OutputColumnCollection.Count];  
  
         for (int col = 0; col < input.InputColumnCollection.Count; col++)  
            inputColumnBufferIndexes[col] = BufferManager.FindColumnByLineageID(input.Buffer, input.InputColumnCollection[col].LineageID);  
  
         for (int col = 0; col < output.OutputColumnCollection.Count; col++)  
            outputColumnBufferIndexes[col] = BufferManager.FindColumnByLineageID(output.Buffer, output.OutputColumnCollection[col].LineageID);  
  
      }  
  
      public override void PrimeOutput(int outputs, int[] outputIDs, PipelineBuffer[] buffers)  
      {  
         if (buffers.Length != 0)  
            outputBuffer = buffers[0];  
      }  
      public override void ProcessInput(int inputID, PipelineBuffer buffer)  
      {  
            // Advance the buffer to the next row.  
            while (buffer.NextRow())  
            {  
               // Add a row to the output buffer.  
               outputBuffer.AddRow();  
               for (int x = 0; x < inputColumnBufferIndexes.Length; x++)  
               {  
                  // Copy the data from the input buffer column to the output buffer column.  
                  outputBuffer[outputColumnBufferIndexes[x]] = buffer[inputColumnBufferIndexes[x]];  
               }  
            }  
         if (buffer.EndOfRowset)  
         {  
            // EndOfRowset on the input buffer is true.  
            // Set EndOfRowset on the output buffer.  
            outputBuffer.SetEndOfRowset();  
         }  
      }  
   }  
}  
```  
  
```vb  
Imports System  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
Imports Microsoft.SqlServer.Dts.Runtime.Wrapper  
  
Namespace Microsoft.Samples.SqlServer.Dts  
  
    <DtsPipelineComponent(DisplayName:="AsynchronousOutput")> _  
    Public Class AsynchronousOutput  
  
        Inherits PipelineComponent  
  
        Private outputBuffer As PipelineBuffer  
        Private inputColumnBufferIndexes As Integer()  
        Private outputColumnBufferIndexes As Integer()  
  
        Public Overrides Sub ProvideComponentProperties()  
  
            ' Let the base class add the input and output objects.  
            Me.ProvideComponentProperties()  
  
            ' Name the input and output, and make the  
            ' output asynchronous.  
            ComponentMetaData.InputCollection(0).Name = "Input"  
            ComponentMetaData.OutputCollection(0).Name = "AsyncOutput"  
            ComponentMetaData.OutputCollection(0).SynchronousInputID = 0  
        End Sub  
  
        Public Overrides Sub PreExecute()  
  
            Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)  
            Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection(0)  
  
            ReDim inputColumnBufferIndexes(input.InputColumnCollection.Count)  
            ReDim outputColumnBufferIndexes(output.OutputColumnCollection.Count)  
  
            For col As Integer = 0 To input.InputColumnCollection.Count  
                inputColumnBufferIndexes(col) = BufferManager.FindColumnByLineageID(input.Buffer, input.InputColumnCollection(col).LineageID)  
            Next  
  
            For col As Integer = 0 To output.OutputColumnCollection.Count  
                outputColumnBufferIndexes(col) = BufferManager.FindColumnByLineageID(output.Buffer, output.OutputColumnCollection(col).LineageID)  
            Next  
  
        End Sub  
        Public Overrides Sub PrimeOutput(ByVal outputs As Integer, ByVal outputIDs As Integer(), ByVal buffers As PipelineBuffer())  
  
            If buffers.Length <> 0 Then  
                outputBuffer = buffers(0)  
            End If  
  
        End Sub  
  
        Public Overrides Sub ProcessInput(ByVal inputID As Integer, ByVal buffer As PipelineBuffer)  
  
                ' Advance the buffer to the next row.  
                While (buffer.NextRow())  
  
                    ' Add a row to the output buffer.  
                    outputBuffer.AddRow()  
                    For x As Integer = 0 To inputColumnBufferIndexes.Length  
  
                        ' Copy the data from the input buffer column to the output buffer column.  
                        outputBuffer(outputColumnBufferIndexes(x)) = buffer(inputColumnBufferIndexes(x))  
  
                    Next  
                End While  
  
            If buffer.EndOfRowset = True Then  
                ' EndOfRowset on the input buffer is true.  
                ' Set the end of row set on the output buffer.  
                outputBuffer.SetEndOfRowset()  
            End If  
        End Sub  
    End Class  
End Namespace  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Sviluppo di un componente di trasformazione personalizzato con output sincroni](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-synchronous-outputs.md)   
 [Informazioni sulle trasformazioni sincrone e asincrone](../../integration-services/understanding-synchronous-and-asynchronous-transformations.md)   
 [Creazione di una trasformazione asincrona con il componente script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-an-asynchronous-transformation-with-the-script-component.md)  
  
  
