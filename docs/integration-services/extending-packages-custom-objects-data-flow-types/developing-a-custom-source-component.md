---
description: Sviluppo di un componente di origine personalizzato
title: Sviluppo di un componente di origine personalizzato | Microsoft Docs
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
- validation [Integration Services], components
- external data sources [Integration Services]
- data flow components [Integration Services], source components
- output columns [Integration Services]
- custom data flow components [Integration Services], source components
- custom sources [Integration Services]
- source components [Integration Services]
ms.assetid: 4dc0f631-8fd6-4007-b573-ca67f58ca068
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cc3a5fc5e867a7f8afabd8e4373e00926534ae3f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348265"
---
# <a name="developing-a-custom-source-component"></a>Sviluppo di un componente di origine personalizzato

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] offre agli sviluppatori la possibilità di scrivere componenti di origine in grado di connettersi a origini dati personalizzate e di fornire dati da tali origini ad altri componenti in un'attività Flusso di dati. La possibilità per creare origini personalizzate si rivela utile quando è necessario connettersi a origini dati non accessibili tramite una delle origini di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] esistenti.  
  
 I componenti di origine includono uno o più output e nessun input. In fase di progettazione tali componenti vengono utilizzati per creare e configurare connessioni, leggere metadati di colonne dall'origine dati esterna e configurare le colonne di output dell'origine in base all'origine dati esterna. Durante l'esecuzione, si connettono all'origine dati esterna e aggiungono righe a un buffer di output. L'attività Flusso di dati fornisce quindi questo buffer di righe di dati ai componenti a valle.  
  
 Per una panoramica generale sullo sviluppo di componenti flusso dati, vedere [Sviluppo di un componente flusso di dati personalizzato](../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md).  
  
## <a name="design-time"></a>Fase di progettazione  
 L'implementazione della funzionalità in fase di progettazione di un componente di origine implica la specifica di una connessione a un'origine dati esterna, l'aggiunta e la configurazione delle colonne di output che riflettono l'origine dati e la verifica che il componente sia pronto per l'esecuzione. Per definizione, un componente di origine include uno o più output asincroni e nessun input.  
  
### <a name="creating-the-component"></a>Creazione del componente  
 I componenti di origine si connettono a origini dati esterne tramite gli oggetti <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> definiti in un pacchetto. Indicano il requisito di una gestione connessione con l'aggiunta di un elemento alla raccolta <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.RuntimeConnectionCollection%2A> della proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ComponentMetaData%2A>. Questa raccolta ha due scopi: contenere i riferimenti alle gestioni connessioni nel pacchetto usato dal componente e segnalare la necessità di una gestione connessione alla finestra di progettazione. Dopo che un oggetto <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeConnection100> è stato aggiunto alla raccolta, in **Editor avanzato** viene visualizzata la scheda **Proprietà connessione**, che consente agli utenti di selezionare o creare una connessione nel pacchetto.  
  
 Nell'esempio di codice seguente è illustrata un'implementazione di <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A> che aggiunge un output, quindi aggiunge un oggetto <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeConnection100> a <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.RuntimeConnectionCollection%2A>.  
  
```csharp  
using System;  
using System.Collections;  
using System.Data;  
using System.Data.SqlClient;  
using System.Data.OleDb;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Runtime.Wrapper;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
  
namespace Microsoft.Samples.SqlServer.Dts  
{  
    [DtsPipelineComponent(DisplayName = "MySourceComponent",ComponentType = ComponentType.SourceAdapter)]  
    public class MyComponent : PipelineComponent  
    {  
        public override void ProvideComponentProperties()  
        {  
            // Reset the component.  
            base.RemoveAllInputsOutputsAndCustomProperties();  
            ComponentMetaData.RuntimeConnectionCollection.RemoveAll();  
  
            IDTSOutput100 output = ComponentMetaData.OutputCollection.New();  
            output.Name = "Output";  
  
            IDTSRuntimeConnection100 connection = ComponentMetaData.RuntimeConnectionCollection.New();  
            connection.Name = "ADO.NET";  
        }  
```  
  
```vb  
Imports System.Data  
Imports System.Data.SqlClient  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Runtime.Wrapper  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
  
<DtsPipelineComponent(DisplayName:="MySourceComponent", ComponentType:=ComponentType.SourceAdapter)> _  
Public Class MySourceComponent  
    Inherits PipelineComponent  
  
    Public Overrides Sub ProvideComponentProperties()  
  
        ' Allow for resetting the component.  
        RemoveAllInputsOutputsAndCustomProperties()  
        ComponentMetaData.RuntimeConnectionCollection.RemoveAll()  
  
        Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection.New()  
        output.Name = "Output"  
  
        Dim connection As IDTSRuntimeConnection100 = ComponentMetaData.RuntimeConnectionCollection.New()  
        connection.Name = "ADO.NET"  
  
    End Sub  
End Class  
```  
  
### <a name="connecting-to-an-external-data-source"></a>Connessione a un'origine dati esterna  
 Dopo che una connessione è stata aggiunta a <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.RuntimeConnectionCollection%2A>, eseguire l'override del metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.AcquireConnections%2A> per stabilire una connessione all'origine dati esterna. Questo metodo viene chiamato sia durante la fase di progettazione che di esecuzione. Il componente deve stabilire una connessione alla gestione connessione specificata dalla connessione di run-time e, successivamente, all'origine dati esterna.  
  
 Una volta stabilita, la connessione deve essere memorizzata nella cache interna dal componente e rilasciata quando viene chiamato il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReleaseConnections%2A>. Il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReleaseConnections%2A> viene chiamato in fase di progettazione e di esecuzione, analogamente al metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.AcquireConnections%2A>. Gli sviluppatori eseguono l'override di questo metodo e rilasciano la connessione stabilita dal componente durante <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.AcquireConnections%2A>.  
  
 Nell'esempio di codice seguente è illustrato un componente che si connette a una connessione ADO.NET nel metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.AcquireConnections%2A> e chiude la connessione nel metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReleaseConnections%2A>.  
  
```csharp  
private SqlConnection sqlConnection;  
  
public override void AcquireConnections(object transaction)  
{  
    if (ComponentMetaData.RuntimeConnectionCollection[0].ConnectionManager != null)  
    {  
        ConnectionManager cm = Microsoft.SqlServer.Dts.Runtime.DtsConvert.GetWrapper(ComponentMetaData.RuntimeConnectionCollection[0].ConnectionManager);  
        ConnectionManagerAdoNet cmado = cm.InnerObject as ConnectionManagerAdoNet;  
  
        if (cmado == null)  
            throw new Exception("The ConnectionManager " + cm.Name + " is not an ADO.NET connection.");  
  
        sqlConnection = cmado.AcquireConnection(transaction) as SqlConnection;  
        sqlConnection.Open();  
    }  
}  
  
public override void ReleaseConnections()  
{  
    if (sqlConnection != null && sqlConnection.State != ConnectionState.Closed)  
        sqlConnection.Close();  
}  
```  
  
```vb  
Private sqlConnection As SqlConnection  
  
Public Overrides Sub AcquireConnections(ByVal transaction As Object)  
  
    If Not IsNothing(ComponentMetaData.RuntimeConnectionCollection(0).ConnectionManager) Then  
  
        Dim cm As ConnectionManager = Microsoft.SqlServer.Dts.Runtime.DtsConvert.GetWrapper(ComponentMetaData.RuntimeConnectionCollection(0).ConnectionManager)  
        Dim cmado As ConnectionManagerAdoNet = CType(cm.InnerObject, ConnectionManagerAdoNet)  
  
        If IsNothing(cmado) Then  
            Throw New Exception("The ConnectionManager " + cm.Name + " is not an ADO.NET connection.")  
        End If  
  
        sqlConnection = CType(cmado.AcquireConnection(transaction), SqlConnection)  
        sqlConnection.Open()  
  
    End If  
End Sub  
  
Public Overrides Sub ReleaseConnections()  
  
    If Not IsNothing(sqlConnection) And sqlConnection.State <> ConnectionState.Closed Then  
        sqlConnection.Close()  
    End If  
  
End Sub  
```  
  
### <a name="creating-and-configuring-output-columns"></a>Creazione e configurazione di colonne di output  
 Le colonne di output di un componente di origine riflettono le colonne dell'origine dati esterna aggiunte dal componente al flusso di dati durante l'esecuzione. In fase di progettazione le colonne di output vengono aggiunte dopo che il componente è stato configurato per la connessione a un'origine dati esterna. Il metodo della fase di progettazione utilizzato da un componente per aggiungere le colonne alla propria raccolta di output varia in base ai requisiti del componente, anche se questa aggiunta non deve essere eseguita durante <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A> o <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.AcquireConnections%2A>. Ad esempio, un componente che contiene un'istruzione SQL in una proprietà personalizzata che controlla il set di dati per il componente può aggiungere le proprie colonne di output durante il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.SetComponentProperty%2A>. Il componente verifica se è disponibile una connessione memorizzata nella cache e, in caso affermativo, si connette all'origine dati e genera le proprie colonne di output.  
  
 Dopo la creazione di una colonna di output, impostare le relative proprietà del tipo di dati chiamando il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.SetDataTypeProperties%2A>. Questo metodo è necessario perché le proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.DataType%2A>, <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.Length%2A>, <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.Precision%2A> e <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.CodePage%2A> sono di sola lettura e ogni proprietà dipende dalle impostazioni dell'altra. Questo metodo impone la necessità di impostare questi valori in modo consistente e l'attività Flusso di dati verifica se sono stati impostati correttamente.  
  
 L'oggetto <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.DataType%2A> della colonna determina i valori impostati per le altre proprietà. Nella tabella seguente sono illustrati i requisiti delle proprietà dipendenti per ogni oggetto <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.DataType%2A>. Le proprietà dipendenti dei tipi di dati non elencati sono impostate su zero.  
  
|DataType|Length|Scalabilità|Precisione|CodePage|  
|--------------|------------|-----------|---------------|--------------|  
|DT_DECIMAL|0|Maggiore di 0 e minore o uguale a 28.|0|0|  
|DT_CY|0|0|0|0|  
|DT_NUMERIC|0|Maggiore di 0 e minore o uguale a 28, nonché minore di Precision.|Maggiore o uguale a 1 e minore o uguale a 38.|0|  
|DT_BYTES|Maggiore di 0.|0|0|0|  
|DT_STR|Maggiore di 0 e minore di 8000.|0|0|Diverso da 0 e tabella codici valida.|  
|DT_WSTR|Maggiore di 0 e minore di 4000.|0|0|0|  
  
 Poiché le restrizioni sulle proprietà del tipo di dati sono basate sul tipo di dati della colonna di output, è necessario scegliere il tipo di dati di [!INCLUDE[ssIS](../../includes/ssis-md.md)] corretto quando si utilizzano tipi gestiti. La classe di base fornisce tre metodi di supporto, <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ConvertBufferDataTypeToFitManaged%2A>, <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferTypeToDataRecordType%2A> e <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.DataRecordTypeToBufferType%2A>, per assistere gli sviluppatori di componenti gestiti nella selezione di un tipo di dati di [!INCLUDE[ssIS](../../includes/ssis-md.md)] dato un tipo gestito. Questi metodi convertono i tipi di dati gestiti nei tipi di dati di [!INCLUDE[ssIS](../../includes/ssis-md.md)] e viceversa.  
  
 Nell'esempio di codice seguente viene illustrato il popolamento della raccolta di colonne di output di un componente in base allo schema di una tabella. I metodi di supporto della classe di base vengono utilizzati per impostare il tipo di dati della colonna, mentre le proprietà dipendenti vengono impostate in base al tipo di dati.  
  
```csharp  
SqlCommand sqlCommand;  
  
private void CreateColumnsFromDataTable()  
{  
    // Get the output.  
    IDTSOutput100 output = ComponentMetaData.OutputCollection[0];  
  
    // Start clean, and remove the columns from both collections.  
    output.OutputColumnCollection.RemoveAll();  
    output.ExternalMetadataColumnCollection.RemoveAll();  
  
    this.sqlCommand = sqlConnection.CreateCommand();  
    this.sqlCommand.CommandType = CommandType.Text;  
    this.sqlCommand.CommandText = (string)ComponentMetaData.CustomPropertyCollection["SqlStatement"].Value;  
    SqlDataReader schemaReader = this.sqlCommand.ExecuteReader(CommandBehavior.SchemaOnly);  
    DataTable dataTable = schemaReader.GetSchemaTable();  
  
    // Walk the columns in the schema,   
    // and for each data column create an output column and an external metadata column.  
    foreach (DataRow row in dataTable.Rows)  
    {  
        IDTSOutputColumn100 outColumn = output.OutputColumnCollection.New();  
        IDTSExternalMetadataColumn100 exColumn = output.ExternalMetadataColumnCollection.New();  
  
        // Set column data type properties.  
        bool isLong = false;  
        DataType dt = DataRecordTypeToBufferType((Type)row["DataType"]);  
        dt = ConvertBufferDataTypeToFitManaged(dt, ref isLong);  
        int length = 0;  
        int precision = (short)row["NumericPrecision"];  
        int scale = (short)row["NumericScale"];  
        int codepage = dataTable.Locale.TextInfo.ANSICodePage;  
  
        switch (dt)  
        {  
            // The length cannot be zero, and the code page property must contain a valid code page.  
            case DataType.DT_STR:  
            case DataType.DT_TEXT:  
                length = precision;  
                precision = 0;  
                scale = 0;  
                break;  
  
            case DataType.DT_WSTR:  
                length = precision;  
                codepage = 0;  
                scale = 0;  
                precision = 0;  
                break;  
  
            case DataType.DT_BYTES:  
                precision = 0;  
                scale = 0;  
                codepage = 0;  
                break;  
  
            case DataType.DT_NUMERIC:  
                length = 0;  
                codepage = 0;  
  
                if (precision > 38)  
                    precision = 38;  
  
                if (scale > 6)  
                    scale = 6;  
                break;  
  
            case DataType.DT_DECIMAL:  
                length = 0;  
                precision = 0;  
                codepage = 0;  
                break;  
  
            default:  
                length = 0;  
                precision = 0;  
                codepage = 0;  
                scale = 0;  
                break;  
  
        }  
  
        // Set the properties of the output column.  
        outColumn.Name = (string)row["ColumnName"];  
        outColumn.SetDataTypeProperties(dt, length, precision, scale, codepage);  
    }  
}  
```  
  
```vb  
Private sqlCommand As SqlCommand  
  
Private Sub CreateColumnsFromDataTable()  
  
    ' Get the output.  
    Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection(0)  
  
    ' Start clean, and remove the columns from both collections.  
    output.OutputColumnCollection.RemoveAll()  
    output.ExternalMetadataColumnCollection.RemoveAll()  
  
    Me.sqlCommand = sqlConnection.CreateCommand()  
    Me.sqlCommand.CommandType = CommandType.Text  
    Me.sqlCommand.CommandText = CStr(ComponentMetaData.CustomPropertyCollection("SqlStatement").Value)  
  
    Dim schemaReader As SqlDataReader = Me.sqlCommand.ExecuteReader(CommandBehavior.SchemaOnly)  
    Dim dataTable As DataTable = schemaReader.GetSchemaTable()  
  
    ' Walk the columns in the schema,   
    ' and for each data column create an output column and an external metadata column.  
    For Each row As DataRow In dataTable.Rows  
  
        Dim outColumn As IDTSOutputColumn100 = output.OutputColumnCollection.New()  
        Dim exColumn As IDTSExternalMetadataColumn100 = output.ExternalMetadataColumnCollection.New()  
  
        ' Set column data type properties.  
        Dim isLong As Boolean = False  
        Dim dt As DataType = DataRecordTypeToBufferType(CType(row("DataType"), Type))  
        dt = ConvertBufferDataTypeToFitManaged(dt, isLong)  
        Dim length As Integer = 0  
        Dim precision As Integer = CType(row("NumericPrecision"), Short)  
        Dim scale As Integer = CType(row("NumericScale"), Short)  
        Dim codepage As Integer = dataTable.Locale.TextInfo.ANSICodePage  
  
        Select Case dt  
  
            ' The length cannot be zero, and the code page property must contain a valid code page.  
            Case DataType.DT_STR  
            Case DataType.DT_TEXT  
                length = precision  
                precision = 0  
                scale = 0  
  
            Case DataType.DT_WSTR  
                length = precision  
                codepage = 0  
                scale = 0  
                precision = 0  
  
            Case DataType.DT_BYTES  
                precision = 0  
                scale = 0  
                codepage = 0  
  
            Case DataType.DT_NUMERIC  
                length = 0  
                codepage = 0  
  
                If precision > 38 Then  
                    precision = 38  
                End If  
  
                If scale > 6 Then  
                    scale = 6  
                End If  
  
            Case DataType.DT_DECIMAL  
                length = 0  
                precision = 0  
                codepage = 0  
  
            Case Else  
                length = 0  
                precision = 0  
                codepage = 0  
                scale = 0  
        End Select  
  
        ' Set the properties of the output column.  
        outColumn.Name = CStr(row("ColumnName"))  
        outColumn.SetDataTypeProperties(dt, length, precision, scale, codepage)  
    Next  
End Sub  
```  
  
### <a name="validating-the-component"></a>Convalida del componente  
 È necessario convalidare un componente di origine e verificare che le colonne definite nelle relative raccolte di colonne di output corrispondano alle colonne nell'origine dati esterna. Talvolta, può essere impossibile verificare le colonne di output rispetto all'origine dati esterna, ad esempio in uno stato disconnesso oppure quando è preferibile evitare lunghi round trip al server. In queste situazioni, le colonne nell'output possono comunque essere convalidate tramite <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100.ExternalMetadataColumnCollection%2A> dell'oggetto di output. Per altre informazioni, vedere [Convalida di un componente del flusso di dati](../../integration-services/extending-packages-custom-objects/data-flow/validating-a-data-flow-component.md).  
  
 Questa raccolta esiste sia negli oggetti di input che di output ed è possibile popolarla con le colonne dell'origine dati esterna. È possibile usare questa raccolta per convalidare le colonne di output quando Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] è offline, quando il componente è disconnesso o quando la proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ValidateExternalMetadata%2A> è **false**. La raccolta deve essere popolata per la prima volta contemporaneamente alla creazione delle colonne di output. L'aggiunta di colonne di metadati esterne alla raccolta è relativamente semplice, perché la colonna di metadati esterna deve inizialmente corrispondere alla colonna di output. Le proprietà del tipo di dati della colonna devono essere già state impostate correttamente e possono essere copiate direttamente nell'oggetto <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSExternalMetadataColumn100>.  
  
 Nel codice di esempio seguente viene aggiunta una colonna di metadati esterna basata su una colonna di output appena creata. Si presuppone che la colonna di output sia già stata creata.  
  
```csharp  
private void CreateExternalMetaDataColumn(IDTSOutput100 output, IDTSOutputColumn100 outputColumn)  
{  
  
    // Set the properties of the external metadata column.  
    IDTSExternalMetadataColumn100 externalColumn = output.ExternalMetadataColumnCollection.New();  
    externalColumn.Name = outputColumn.Name;  
    externalColumn.Precision = outputColumn.Precision;  
    externalColumn.Length = outputColumn.Length;  
    externalColumn.DataType = outputColumn.DataType;  
    externalColumn.Scale = outputColumn.Scale;  
  
    // Map the external column to the output column.  
    outputColumn.ExternalMetadataColumnID = externalColumn.ID;  
  
}  
```  
  
```vb  
Private Sub CreateExternalMetaDataColumn(ByVal output As IDTSOutput100, ByVal outputColumn As IDTSOutputColumn100)  
  
        ' Set the properties of the external metadata column.  
        Dim externalColumn As IDTSExternalMetadataColumn100 = output.ExternalMetadataColumnCollection.New()  
        externalColumn.Name = outputColumn.Name  
        externalColumn.Precision = outputColumn.Precision  
        externalColumn.Length = outputColumn.Length  
        externalColumn.DataType = outputColumn.DataType  
        externalColumn.Scale = outputColumn.Scale  
  
        ' Map the external column to the output column.  
        outputColumn.ExternalMetadataColumnID = externalColumn.ID  
  
    End Sub  
```  
  
## <a name="run-time"></a>Fase di esecuzione  
 Durante l'esecuzione i componenti aggiungono righe ai buffer di output creati dall'attività Flusso di dati e forniti al componente in <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>. Chiamato una volta per i componenti di origine, il metodo riceve un buffer di output per ogni <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100> del componente connesso a un componente a valle.  
  
### <a name="locating-columns-in-the-buffer"></a>Individuazione di colonne nel buffer  
 Il buffer di output per un componente contiene le colonne definite dal componente ed eventuali altre colonne aggiunte all'output di un componente a valle. Se ad esempio un componente di origine prevede tre colonne nel relativo output e il componente successivo aggiunge una quarta colonna di output, il buffer di output fornito per l'utilizzo da parte del componente di origine contiene queste quattro colonne.  
  
 L'ordine delle colonne in una riga del buffer non è definito dall'indice della colonna di output nella raccolta di colonne di output. È possibile individuare in modo accurato una colonna di output in una riga del buffer solo tramite il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSBufferManagerClass.FindColumnByLineageID%2A> di <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A>. Questo metodo individua la colonna con l'ID di derivazione specificato nel buffer specificato, quindi restituisce la relativa posizione nella riga. Gli indici delle colonne di output vengono in genere individuati nel metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A> e vengono archiviati per l'utilizzo durante <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>.  
  
 Nell'esempio di codice seguente viene individuata la posizione delle colonne di output nel buffer di output durante una chiamata a <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A> e tali colonne vengono archiviate in una struttura interna. Anche il nome della colonna viene archiviato nella struttura e viene utilizzato nell'esempio di codice per il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> nella sezione successiva di questo argomento.  
  
```csharp  
ArrayList columnInformation;  
  
private struct ColumnInfo  
{  
    public int BufferColumnIndex;  
    public string ColumnName;  
}  
  
public override void PreExecute()  
{  
    this.columnInformation = new ArrayList();  
    IDTSOutput100 output = ComponentMetaData.OutputCollection[0];  
  
    foreach (IDTSOutputColumn100 col in output.OutputColumnCollection)  
    {  
        ColumnInfo ci = new ColumnInfo();  
        ci.BufferColumnIndex = BufferManager.FindColumnByLineageID(output.Buffer, col.LineageID);  
        ci.ColumnName = col.Name;  
        columnInformation.Add(ci);  
    }  
}  
```  
  
```vb  
Public Overrides Sub PreExecute()  
  
    Me.columnInformation = New ArrayList()  
    Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection(0)  
  
    For Each col As IDTSOutputColumn100 In output.OutputColumnCollection  
  
        Dim ci As ColumnInfo = New ColumnInfo()  
        ci.BufferColumnIndex = BufferManager.FindColumnByLineageID(output.Buffer, col.LineageID)  
        ci.ColumnName = col.Name  
        columnInformation.Add(ci)  
    Next  
End Sub  
```  
  
### <a name="processing-rows"></a>Elaborazione di righe  
 Le righe vengono aggiunte al buffer di output chiamando il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.AddRow%2A>, che crea una nuova riga del buffer con valori vuoti nelle colonne. Il componente assegna quindi i valori alle singole colonne. I buffer di output forniti a un componente vengono creati e monitorati dall'attività Flusso di dati. Quando diventano pieni, le righe nel buffer vengono spostate nel componente successivo. Non è possibile determinare quando un batch di righe è stato inviato al componente successivo, in quanto lo spostamento di righe da parte dell'attività Flusso di dati è trasparente per lo sviluppatore di componenti e la proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.RowCount%2A> è sempre zero sui buffer di output. Quando un componente di origine termina l'aggiunta di righe al proprio buffer di output, notifica l'attività Flusso di dati con una chiamata al metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetEndOfRowset%2A> di <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer>, quindi le righe rimanenti nel buffer vengono passate al componente successivo.  
  
 Mentre il componente di origine legge le righe dall'origine dati esterna, è possibile aggiornare i contatori delle prestazioni "Righe lette" o "Byte BLOB letti" chiamando il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.IncrementPipelinePerfCounter%2A>. Per altre informazioni, vedere [i contatori delle prestazioni](../../integration-services/performance/performance-counters.md).  
  
 Nell'esempio di codice seguente è illustrato un componente che aggiunge righe in un buffer di output in <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>. Gli indici delle colonne di output nel buffer sono stati individuati utilizzando <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A> nell'esempio di codice precedente.  
  
```csharp  
public override void PrimeOutput(int outputs, int[] outputIDs, PipelineBuffer[] buffers)  
{  
    IDTSOutput100 output = ComponentMetaData.OutputCollection[0];  
    PipelineBuffer buffer = buffers[0];  
  
    SqlDataReader dataReader = sqlCommand.ExecuteReader();  
  
    // Loop over the rows in the DataReader,   
    // and add them to the output buffer.  
    while (dataReader.Read())  
    {  
        // Add a row to the output buffer.  
        buffer.AddRow();  
  
        for (int x = 0; x < columnInformation.Count; x++)  
        {  
            ColumnInfo ci = (ColumnInfo)columnInformation[x];  
            int ordinal = dataReader.GetOrdinal(ci.ColumnName);  
  
            if (dataReader.IsDBNull(ordinal))  
                buffer.SetNull(ci.BufferColumnIndex);  
            else  
            {  
                buffer[ci.BufferColumnIndex] = dataReader[ci.ColumnName];  
            }  
        }  
    }  
    buffer.SetEndOfRowset();  
}  
```  
  
```vb  
Public Overrides Sub PrimeOutput(ByVal outputs As Integer, ByVal outputIDs As Integer(), ByVal buffers As PipelineBuffer())  
  
    Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection(0)  
    Dim buffer As PipelineBuffer = buffers(0)  
  
    Dim dataReader As SqlDataReader = sqlCommand.ExecuteReader()  
  
    ' Loop over the rows in the DataReader,   
    ' and add them to the output buffer.  
    While (dataReader.Read())  
  
        ' Add a row to the output buffer.  
        buffer.AddRow()  
  
        For x As Integer = 0 To columnInformation.Count  
  
            Dim ci As ColumnInfo = CType(columnInformation(x), ColumnInfo)  
  
            Dim ordinal As Integer = dataReader.GetOrdinal(ci.ColumnName)  
  
            If (dataReader.IsDBNull(ordinal)) Then  
                buffer.SetNull(ci.BufferColumnIndex)  
            Else  
                buffer(ci.BufferColumnIndex) = dataReader(ci.ColumnName)  
  
            End If  
        Next  
  
    End While  
  
    buffer.SetEndOfRowset()  
End Sub  
```  
  
## <a name="sample"></a>Esempio  
 Nell'esempio seguente è illustrato un componente di origine semplice che utilizza una gestione connessione file per caricare il contenuto binario dei file nel flusso di dati. Nell'esempio non vengono dimostrati tutti i metodi e le funzionalità descritti nell'argomento. Vengono descritti i metodi importanti di cui ogni componente di origine personalizzato deve eseguire l'override, ma non è incluso codice per la convalida in fase di progettazione.  
  
```csharp  
using System;  
using System.IO;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
using Microsoft.SqlServer.Dts.Runtime.Wrapper;  
  
namespace BlobSrc  
{  
  [DtsPipelineComponent(DisplayName = "BLOB Inserter Source", Description = "Inserts files into the data flow as BLOBs")]  
  public class BlobSrc : PipelineComponent  
  {  
    IDTSConnectionManager100 m_ConnMgr;  
    int m_FileNameColumnIndex = -1;  
    int m_FileBlobColumnIndex = -1;  
  
    public override void ProvideComponentProperties()  
    {  
      IDTSOutput100 output = ComponentMetaData.OutputCollection.New();  
      output.Name = "BLOB File Inserter Output";  
  
      IDTSOutputColumn100 column = output.OutputColumnCollection.New();  
      column.Name = "FileName";  
      column.SetDataTypeProperties(DataType.DT_WSTR, 256, 0, 0, 0);  
  
      column = output.OutputColumnCollection.New();  
      column.Name = "FileBLOB";  
      column.SetDataTypeProperties(DataType.DT_IMAGE, 0, 0, 0, 0);  
  
      IDTSRuntimeConnection100 conn = ComponentMetaData.RuntimeConnectionCollection.New();  
      conn.Name = "FileConnection";  
    }  
  
    public override void AcquireConnections(object transaction)  
    {  
      IDTSRuntimeConnection100 conn = ComponentMetaData.RuntimeConnectionCollection[0];  
      m_ConnMgr = conn.ConnectionManager;  
    }  
  
    public override void ReleaseConnections()  
    {  
      m_ConnMgr = null;  
    }  
  
    public override void PreExecute()  
    {  
      IDTSOutput100 output = ComponentMetaData.OutputCollection[0];  
  
      m_FileNameColumnIndex = (int)BufferManager.FindColumnByLineageID(output.Buffer, output.OutputColumnCollection[0].LineageID);  
      m_FileBlobColumnIndex = (int)BufferManager.FindColumnByLineageID(output.Buffer, output.OutputColumnCollection[1].LineageID);  
    }  
  
    public override void PrimeOutput(int outputs, int[] outputIDs, PipelineBuffer[] buffers)  
    {  
      string strFileName = (string)m_ConnMgr.AcquireConnection(null);  
  
      while (strFileName != null)  
      {  
        buffers[0].AddRow();  
  
        buffers[0].SetString(m_FileNameColumnIndex, strFileName);  
  
        FileInfo fileInfo = new FileInfo(strFileName);  
        byte[] fileData = new byte[fileInfo.Length];  
        FileStream fs = new FileStream(strFileName, FileMode.Open, FileAccess.Read, FileShare.Read);  
        fs.Read(fileData, 0, fileData.Length);  
  
        buffers[0].AddBlobData(m_FileBlobColumnIndex, fileData);  
  
        strFileName = (string)m_ConnMgr.AcquireConnection(null);  
      }  
  
      buffers[0].SetEndOfRowset();  
    }  
  }  
}  
```  
  
```vb  
Imports System   
Imports System.IO   
Imports Microsoft.SqlServer.Dts.Pipeline   
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper   
Imports Microsoft.SqlServer.Dts.Runtime.Wrapper   
Namespace BlobSrc   
  
 <DtsPipelineComponent(DisplayName="BLOB Inserter Source", Description="Inserts files into the data flow as BLOBs")> _   
 Public Class BlobSrc   
 Inherits PipelineComponent   
   Private m_ConnMgr As IDTSConnectionManager100   
   Private m_FileNameColumnIndex As Integer = -1   
   Private m_FileBlobColumnIndex As Integer = -1   
  
   Public  Overrides Sub ProvideComponentProperties()   
     Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection.New   
     output.Name = "BLOB File Inserter Output"   
     Dim column As IDTSOutputColumn100 = output.OutputColumnCollection.New   
     column.Name = "FileName"   
     column.SetDataTypeProperties(DataType.DT_WSTR, 256, 0, 0, 0)   
     column = output.OutputColumnCollection.New   
     column.Name = "FileBLOB"   
     column.SetDataTypeProperties(DataType.DT_IMAGE, 0, 0, 0, 0)   
     Dim conn As IDTSRuntimeConnection90 = ComponentMetaData.RuntimeConnectionCollection.New   
     conn.Name = "FileConnection"   
   End Sub   
  
   Public  Overrides Sub AcquireConnections(ByVal transaction As Object)   
     Dim conn As IDTSRuntimeConnection100 = ComponentMetaData.RuntimeConnectionCollection(0)   
     m_ConnMgr = conn.ConnectionManager   
   End Sub   
  
   Public  Overrides Sub ReleaseConnections()   
     m_ConnMgr = Nothing   
   End Sub   
  
   Public  Overrides Sub PreExecute()   
     Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection(0)   
     m_FileNameColumnIndex = CType(BufferManager.FindColumnByLineageID(output.Buffer, output.OutputColumnCollection(0).LineageID), Integer)   
     m_FileBlobColumnIndex = CType(BufferManager.FindColumnByLineageID(output.Buffer, output.OutputColumnCollection(1).LineageID), Integer)   
   End Sub   
  
   Public  Overrides Sub PrimeOutput(ByVal outputs As Integer, ByVal outputIDs As Integer(), ByVal buffers As PipelineBuffer())   
     Dim strFileName As String = CType(m_ConnMgr.AcquireConnection(Nothing), String)   
     While Not (strFileName Is Nothing)   
       buffers(0).AddRow   
       buffers(0).SetString(m_FileNameColumnIndex, strFileName)   
       Dim fileInfo As FileInfo = New FileInfo(strFileName)   
       Dim fileData(fileInfo.Length) As Byte   
       Dim fs As FileStream = New FileStream(strFileName, FileMode.Open, FileAccess.Read, FileShare.Read)   
       fs.Read(fileData, 0, fileData.Length)   
       buffers(0).AddBlobData(m_FileBlobColumnIndex, fileData)   
       strFileName = CType(m_ConnMgr.AcquireConnection(Nothing), String)   
     End While   
     buffers(0).SetEndOfRowset   
   End Sub   
 End Class   
End Namespace  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Sviluppo di un componente di destinazione personalizzato](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-destination-component.md)   
 [Creazione di un'origine con il componente script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-source-with-the-script-component.md)  
  
  
