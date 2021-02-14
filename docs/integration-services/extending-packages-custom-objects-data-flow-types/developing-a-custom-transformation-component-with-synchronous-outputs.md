---
description: Sviluppo di un componente di trasformazione personalizzato con output sincroni
title: Sviluppo di un componente di trasformazione personalizzato con output sincroni | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
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
- ProcessInput method
- buffer allocations [Integration Services]
- synchronous outputs [Integration Services]
- transformation components [Integration Services]
- output columns [Integration Services]
- data flow components [Integration Services], transformation components
ms.assetid: b694d21f-9919-402d-9192-666c6449b0b7
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 87a53ff91f4ae7d6237fb3f9640420dd964b607f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348232"
---
# <a name="developing-a-custom-transformation-component-with-synchronous-outputs"></a>Sviluppo di un componente di trasformazione personalizzato con output sincroni

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  I componenti di trasformazione con output sincroni ricevono righe dai componenti a monte, quindi leggono o modificano i valori nelle colonne di queste righe mentre le passano ai componenti a valle. Possono anche definire colonne di output aggiuntive derivate dalle colonne fornite dai componenti a monte, ma non aggiungono righe al flusso di dati. Per altre informazioni sulla differenza tra componenti sincroni e asincroni, vedere [Informazioni sulle trasformazioni sincrone e asincrone](../../integration-services/understanding-synchronous-and-asynchronous-transformations.md).  
  
 Questo tipo di componente è indicato per attività in cui i dati vengono modificati in linea quando vengono forniti al componente e in cui il componente non deve necessariamente vedere tutte le righe prima di elaborarle. Si tratta del componente più facile da sviluppare, perché le trasformazioni con output sincroni solitamente non si connettono a origini dati esterne, non gestiscono colonne di metadati esterne né aggiungono righe ai buffer di output.  
  
 La creazione di un componente di trasformazione con output sincroni richiede l'aggiunta di un oggetto <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSInput100> che conterrà le colonne a monte selezionate per il componente e di un oggetto <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100> che può contenere colonne derivate create dal componente. Include anche l'implementazione dei metodi della fase di progettazione e la specifica di codice che legge o modifica le colonne nelle righe del buffer in ingresso durante l'esecuzione.  
  
 In questa sezione vengono fornite le informazioni necessarie per implementare un componente di trasformazione personalizzato, con esempi di codice che consentono di comprenderne meglio i concetti.  
  
## <a name="design-time"></a>Fase di progettazione  
 Il codice della fase di progettazione per questo componente richiede la creazione degli input e degli output, l'aggiunta di eventuali colonne di output aggiuntive generate dal componente e la convalida della configurazione del componente.  
  
### <a name="creating-the-component"></a>Creazione del componente  
 Gli input, gli output e le proprietà personalizzate del componente vengono in genere creati durante il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A>. Sono disponibili due modi per aggiungere l'input e l'output di un componente di trasformazione con output sincroni. È possibile utilizzare l'implementazione della classe di base del metodo e quindi modificare l'input e output predefiniti creati oppure aggiungere manualmente l'input e l'output in modo esplicito.  
  
 Nell'esempio di codice seguente è illustrata un'implementazione di <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A> che aggiunge in modo esplicito gli oggetti di input e di output. In un commento è inclusa la chiamata alla classe di base che consente di ottenere lo stesso risultato.  
  
```csharp  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.Samples.SqlServer.Dts  
{  
    [DtsPipelineComponent(DisplayName = "SynchronousComponent", ComponentType = ComponentType.Transform)]  
    public class SyncComponent : PipelineComponent  
    {  
  
        public override void ProvideComponentProperties()  
        {  
            // Add the input.  
            IDTSInput100 input = ComponentMetaData.InputCollection.New();  
            input.Name = "Input";  
  
            // Add the output.  
            IDTSOutput100 output = ComponentMetaData.OutputCollection.New();  
            output.Name = "Output";  
            output.SynchronousInputID = input.ID;  
  
            // Alternatively, you can let the base class add the input and output  
            // and set the SynchronousInputID of the output to the ID of the input.  
            // base.ProvideComponentProperties();  
        }  
    }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
Imports Microsoft.SqlServer.Dts.Runtime  
  
<DtsPipelineComponent(DisplayName:="SynchronousComponent", ComponentType:=ComponentType.Transform)> _  
Public Class SyncComponent  
    Inherits PipelineComponent  
  
    Public Overrides Sub ProvideComponentProperties()  
  
        ' Add the input.  
        Dim input As IDTSInput100 = ComponentMetaData.InputCollection.New()  
        input.Name = "Input"  
  
        ' Add the output.  
        Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection.New()  
        output.Name = "Output"  
        output.SynchronousInputID = Input.ID  
  
        ' Alternatively, you can let the base class add the input and output  
        ' and set the SynchronousInputID of the output to the ID of the input.  
        ' base.ProvideComponentProperties();  
  
    End Sub  
  
End Class  
```  
  
### <a name="creating-and-configuring-output-columns"></a>Creazione e configurazione di colonne di output  
 Anche se i componenti di trasformazione con output sincroni non aggiungono righe ai buffer, possono aggiungere colonne di output aggiuntive all'output. In genere, quando un componente aggiunge una colonna di output, i relativi valori vengono derivati in fase di esecuzione dai dati contenuti in una o più colonne fornite al componente da un componente a monte.  
  
 Dopo la creazione di una colonna di output, è necessario impostare le relative proprietà del tipo di dati. L'impostazione delle proprietà del tipo di dati di una colonna di output richiede una gestione speciale e viene eseguita chiamando il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.SetDataTypeProperties%2A>. Questo metodo è necessario perché le proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.DataType%2A>, <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.Length%2A>, <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.Precision%2A> e <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.CodePage%2A> sono singolarmente di sola lettura, in quanto ognuna dipende dalle impostazioni dell'altra. Questo metodo garantisce che i valori delle proprietà siano impostati in modo coerente e l'attività Flusso di dati verifica se sono stati impostati correttamente.  
  
 L'oggetto <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.DataType%2A> della colonna determina i valori impostati per le altre proprietà. Nella tabella seguente sono illustrati i requisiti delle proprietà dipendenti per ogni oggetto <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.DataType%2A>. Le proprietà dipendenti dei tipi di dati non elencati sono impostate su zero.  
  
|DataType|Length|Scalabilità|Precisione|CodePage|  
|--------------|------------|-----------|---------------|--------------|  
|DT_DECIMAL|0|Maggiore di 0 e minore o uguale a 28.|0|0|  
|DT_CY|0|0|0|0|  
|DT_NUMERIC|0|Maggiore di 0 e minore o uguale a 28, nonché minore di Precision.|Maggiore o uguale a 1 e minore o uguale a 38.|0|  
|DT_BYTES|Maggiore di 0.|0|0|0|  
|DT_STR|Maggiore di 0 e minore di 8000.|0|0|Diverso da 0 e tabella codici valida.|  
|DT_WSTR|Maggiore di 0 e minore di 4000.|0|0|0|  
  
 Poiché le restrizioni sulle proprietà del tipo di dati sono basate sul tipo di dati della colonna di output, è necessario scegliere il tipo di dati di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] corretto quando si utilizzano tipi gestiti. La classe di base fornisce tre metodi di supporto, <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ConvertBufferDataTypeToFitManaged%2A>, <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferTypeToDataRecordType%2A> e <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.DataRecordTypeToBufferType%2A>, per assistere gli sviluppatori di componenti gestiti nella selezione di un tipo di dati di [!INCLUDE[ssIS](../../includes/ssis-md.md)] dato un tipo gestito. Questi metodi convertono i tipi di dati gestiti nei tipi di dati di [!INCLUDE[ssIS](../../includes/ssis-md.md)] e viceversa.  
  
## <a name="run-time"></a>Fase di esecuzione  
 Generalmente, l'implementazione della parte runtime del componente è suddivisa in due attività: individuazione delle colonne di input e output del componente nel buffer e lettura o scrittura dei valori di tali colonne nelle righe del buffer in ingresso.  
  
### <a name="locating-columns-in-the-buffer"></a>Individuazione di colonne nel buffer  
 Il numero di colonne nei buffer forniti a un componente durante l'esecuzione supera in genere il numero di colonne presenti nelle raccolte di input o output del componente, perché ogni buffer contiene tutte le colonne di output definite nei componenti in un flusso di dati. Per assicurarsi che le colonne del buffer corrispondano correttamente alle colonne dell'input o dell'output, gli sviluppatori di componenti devono utilizzare il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSBufferManager100.FindColumnByLineageID%2A> di <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A>. Questo metodo individua una colonna nel buffer specificato in base al relativo ID di derivazione. In genere, le colonne vengono individuate durante <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A> perché si tratta del primo metodo di runtime in cui la proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A> diventa disponibile.  
  
 Nell'esempio di codice seguente è illustrato un componente che individua gli indici delle colonne nella propria raccolta di colonne di input e output durante <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A>. Gli indici delle colonne vengono archiviati in una matrice di valori integer e il componente può accedervi durante <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>.  
  
```csharp  
int []inputColumns;  
int []outputColumns;  
  
public override void PreExecute()  
{  
    IDTSInput100 input = ComponentMetaData.InputCollection[0];  
    IDTSOutput100 output = ComponentMetaData.OutputCollection[0];  
  
    inputColumns = new int[input.InputColumnCollection.Count];  
    outputColumns = new int[output.OutputColumnCollection.Count];  
  
    for(int col=0; col < input.InputColumnCollection.Count; col++)  
    {  
        IDTSInputColumn100 inputColumn = input.InputColumnCollection[col];  
        inputColumns[col] = BufferManager.FindColumnByLineageID(input.Buffer, inputColumn.LineageID);  
    }  
  
    for(int col=0; col < output.OutputColumnCollection.Count; col++)  
    {  
        IDTSOutputColumn100 outputColumn = output.OutputColumnCollection[col];  
        outputColumns[col] = BufferManager.FindColumnByLineageID(input.Buffer, outputColumn.LineageID);  
    }  
  
}  
```  
  
```vb  
Public Overrides Sub PreExecute()  
  
    Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)  
    Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection(0)  
  
    ReDim inputColumns(input.InputColumnCollection.Count)  
    ReDim outputColumns(output.OutputColumnCollection.Count)  
  
    For col As Integer = 0 To input.InputColumnCollection.Count  
  
        Dim inputColumn As IDTSInputColumn100 = input.InputColumnCollection(col)  
        inputColumns(col) = BufferManager.FindColumnByLineageID(input.Buffer, inputColumn.LineageID)  
    Next  
  
    For col As Integer = 0 To output.OutputColumnCollection.Count  
  
        Dim outputColumn As IDTSOutputColumn100 = output.OutputColumnCollection(col)  
        outputColumns(col) = BufferManager.FindColumnByLineageID(input.Buffer, outputColumn.LineageID)  
    Next  
  
End Sub  
```  
  
### <a name="processing-rows"></a>Elaborazione di righe  
 I componenti ricevono oggetti <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer> che contengono righe e colonne nel metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>. Durante questo metodo vengono scorse le righe nel buffer e vengono lette e modificate le colonne identificate durante <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A> . Il metodo viene chiamato ripetutamente dall'attività Flusso di dati finché non vengono più fornite righe dal componente a monte.  
  
 Una singola colonna nel buffer viene letta o scritta tramite il metodo di accesso dell'indicizzatore di matrici oppure con il metodo **Get** o **Set**. I metodi **Get** e **Set** sono più efficienti e devono essere usati quando il tipo di dati della colonna nel buffer è noto.  
  
 Nell'esempio di codice seguente è illustrata un'implementazione del metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> che elabora le righe in ingresso.  
  
```csharp  
public override void ProcessInput( int InputID, PipelineBuffer buffer)  
{  
       while( buffer.NextRow())  
       {  
            for(int x=0; x < inputColumns.Length;x++)  
            {  
                if(!buffer.IsNull(inputColumns[x]))  
                {  
                    object columnData = buffer[inputColumns[x]];  
                    // TODO: Modify the column data.  
                    buffer[inputColumns[x]] = columnData;  
                }  
            }  
  
      }  
}  
```  
  
```vb  
Public Overrides Sub ProcessInput(ByVal InputID As Integer, ByVal buffer As PipelineBuffer)  
  
        While (buffer.NextRow())  
  
            For x As Integer = 0 To inputColumns.Length  
  
                if buffer.IsNull(inputColumns(x)) = false then  
  
                    Dim columnData As Object = buffer(inputColumns(x))  
                    ' TODO: Modify the column data.  
                    buffer(inputColumns(x)) = columnData  
  
                End If  
            Next  
  
        End While  
End Sub  
```  
  
## <a name="sample"></a>Esempio  
 Nell'esempio seguente è illustrato un componente di trasformazione semplice con output sincroni che converte i valori di tutte le colonne di tipo stringa in lettere maiuscole. Nell'esempio non vengono dimostrati tutti i metodi e le funzionalità descritti nell'argomento. Vengono descritti i metodi importanti di cui ogni componente di trasformazione personalizzato con output sincroni deve eseguire l'override, ma non è incluso codice per la convalida in fase di progettazione.  
  
```csharp  
using System;  
using System.Collections;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
using Microsoft.SqlServer.Dts.Runtime.Wrapper;  
  
namespace Uppercase  
{  
  [DtsPipelineComponent(DisplayName = "Uppercase")]  
  public class Uppercase : PipelineComponent  
  {  
    ArrayList m_ColumnIndexList = new ArrayList();  
  
    public override void ProvideComponentProperties()  
    {  
      base.ProvideComponentProperties();  
      ComponentMetaData.InputCollection[0].Name = "Uppercase Input";  
      ComponentMetaData.OutputCollection[0].Name = "Uppercase Output";  
    }  
  
    public override void PreExecute()  
    {  
      IDTSInput100 input = ComponentMetaData.InputCollection[0];  
      IDTSInputColumnCollection100 inputColumns = input.InputColumnCollection;  
  
      foreach (IDTSInputColumn100 column in inputColumns)  
      {  
        if (column.DataType == DataType.DT_STR || column.DataType == DataType.DT_WSTR)  
        {  
          m_ColumnIndexList.Add((int)BufferManager.FindColumnByLineageID(input.Buffer, column.LineageID));  
        }  
      }  
    }  
  
    public override void ProcessInput(int inputID, PipelineBuffer buffer)  
    {  
      while (buffer.NextRow())  
      {  
        foreach (int columnIndex in m_ColumnIndexList)  
        {  
          string str = buffer.GetString(columnIndex);  
          buffer.SetString(columnIndex, str.ToUpper());  
        }  
      }  
    }  
  }  
}  
```  
  
```vb  
Imports System   
Imports System.Collections   
Imports Microsoft.SqlServer.Dts.Pipeline   
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper   
Imports Microsoft.SqlServer.Dts.Runtime.Wrapper   
Namespace Uppercase   
  
 <DtsPipelineComponent(DisplayName="Uppercase")> _   
 Public Class Uppercase   
 Inherits PipelineComponent   
   Private m_ColumnIndexList As ArrayList = New ArrayList   
  
   Public  Overrides Sub ProvideComponentProperties()   
     MyBase.ProvideComponentProperties   
     ComponentMetaData.InputCollection(0).Name = "Uppercase Input"   
     ComponentMetaData.OutputCollection(0).Name = "Uppercase Output"   
   End Sub   
  
   Public  Overrides Sub PreExecute()   
     Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)   
     Dim inputColumns As IDTSInputColumnCollection100 = input.InputColumnCollection   
     For Each column As IDTSInputColumn100 In inputColumns   
       If column.DataType = DataType.DT_STR OrElse column.DataType = DataType.DT_WSTR Then   
         m_ColumnIndexList.Add(CType(BufferManager.FindColumnByLineageID(input.Buffer, column.LineageID), Integer))   
       End If   
     Next   
   End Sub   
  
   Public  Overrides Sub ProcessInput(ByVal inputID As Integer, ByVal buffer As PipelineBuffer)   
     While buffer.NextRow   
       For Each columnIndex As Integer In m_ColumnIndexList   
         Dim str As String = buffer.GetString(columnIndex)   
         buffer.SetString(columnIndex, str.ToUpper)   
       Next   
     End While   
   End Sub   
 End Class   
End Namespace  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Sviluppo di un componente di trasformazione personalizzato con output asincroni](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-asynchronous-outputs.md)   
 [Informazioni sulle trasformazioni sincrone e asincrone](../../integration-services/understanding-synchronous-and-asynchronous-transformations.md)   
 [Creazione di una trasformazione sincrona con il componente script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-synchronous-transformation-with-the-script-component.md)  
  
  
