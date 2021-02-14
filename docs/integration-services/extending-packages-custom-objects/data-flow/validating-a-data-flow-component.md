---
description: Convalida di un componente del flusso di dati
title: Convalida di un componente flusso di dati | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- ReinitializeMetaData method
- Validate method
- component validation [Integration Services]
- custom data flow components [Integration Services], validating
- validation [Integration Services], components
- data flow components [Integration Services], validating
- validation [Integration Services]
ms.assetid: 1a7d5925-b387-4e31-af7f-c7f3c5151040
author: chugugrace
ms.author: chugu
ms.openlocfilehash: fc86f379cdbd324c47adcbdb9b610399ca2f28af
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354381"
---
# <a name="validating-a-data-flow-component"></a>Convalida di un componente del flusso di dati

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A> della classe di base <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent> viene fornito per impedire l'esecuzione di un componente che non è configurato correttamente. Utilizzare questo metodo per verificare che un componente includa il numero previsto di oggetti di input e output, che i valori delle proprietà personalizzate del componente siano accettabili e che siano state specificate le eventuali connessioni richieste. Utilizzare questo metodo anche per verificare che le colonne nelle raccolte di input e output includano i tipi di dati corretti e che l'oggetto <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSUsageType> di ogni colonna sia impostato correttamente per il componente. L'implementazione della classe di base assiste nel processo di convalida controllando la raccolta di colonne di input del componente e verificando che ogni colonna della raccolta faccia riferimento a una colonna presente in <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputCollection100> del componente a monte.  
  
## <a name="validate-method"></a>Metodo Validate  
 Il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A> viene chiamato ripetutamente quando un componente viene modificato in Progettazione [!INCLUDE[ssIS](../../../includes/ssis-md.md)]. È possibile fornire feedback alla finestra di progettazione e agli utenti del componente tramite il valore restituito dell'enumerazione <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus> e inviando avvisi ed errori. L'enumerazione <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus> contiene tre valori che indicano varie fasi di errore e un quarto valore, <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus.VS_ISVALID>, che indica se il componente è configurato correttamente e pronto per l'esecuzione.  
  
 Il valore <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus.VS_NEEDSNEWMETADATA> indica che esiste un errore in <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ComponentMetaData%2A> e che il componente può correggere gli errori. Se un componente rileva un errore di metadati che può correggere, l'errore non deve essere corretto nel metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A> e <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ComponentMetaData%2A> non deve essere modificato durante la convalida. Al contrario, il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A> deve restituire solo <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus.VS_NEEDSNEWMETADATA> e il componente deve correggere l'errore in una chiamata al metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReinitializeMetaData%2A>, come descritto più avanti in questa sezione.  
  
 Il valore <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus.VS_ISBROKEN> indica che il componente contiene un errore che può essere corretto modificando il componente nella finestra di progettazione. L'errore è in genere causato da una proprietà personalizzata o una connessione richiesta che non è specificata o non è impostata correttamente.  
  
 Il valore finale dell'errore è <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus.VS_ISCORRUPT>, che indica che il componente ha individuato errori che dovrebbero verificarsi solo se la proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ComponentMetaData%2A> è stata modificata direttamente, modificando il codice XML del pacchetto o utilizzando il modello a oggetti. Ad esempio, questo tipo di errore si verifica quando un componente ha aggiunto solo un input ma durante la convalida vengono individuati più input in <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ComponentMetaData%2A>. Gli errori che generano questo valore restituito possono essere corretti solo reimpostando il componente tramite il pulsante **Reimposta** della finestra di dialogo **Editor avanzato**.  
  
 Oltre a restituire valori di errori, i componenti forniscono feedback inviando avvisi o errori durante la convalida. I metodi <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireWarning%2A> e <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireError%2A> forniscono questo meccanismo. Quando questi metodi vengono chiamati, tali eventi vengono inviati nella finestra **Elenco errori** di [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)]. Gli sviluppatori di componenti possono quindi fornire feedback diretto agli utenti sugli errori che si sono verificati e, se necessario, su come correggerli.  
  
 Nell'esempio di codice seguente viene illustrata un'implementazione sottoposta a override di <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A>.  
  
```csharp  
public override DTSValidationStatus Validate()  
{  
    bool pbCancel = false;  
  
    // Validate that there is one input.  
    if (ComponentMetaData.InputCollection.Count != 1)  
    {  
    ComponentMetaData.FireError(0, ComponentMetaData.Name, "Incorrect number of inputs.", "", 0, out pbCancel);  
    return DTSValidationStatus.VS_ISCORRUPT;  
    }  
  
    // Validate that the UserName custom property is set.  
    if (ComponentMetaData.CustomPropertyCollection["UserName"].Value == null || ((string)ComponentMetaData.CustomPropertyCollection["UserName"].Value).Length == 0)  
    {  
        ComponentMetaData.FireError(0, ComponentMetaData.Name, "The UserName property must be set.", "", 0, out pbCancel);  
        return DTSValidationStatus.VS_ISBROKEN;  
    }  
  
    // Validate that there is one output.  
    if (ComponentMetaData.OutputCollection.Count != 1)  
    {  
        ComponentMetaData.FireError(0, ComponentMetaData.Name, "Incorrect number of outputs.", "", 0, out pbCancel);  
        return DTSValidationStatus.VS_ISCORRUPT;  
    }  
  
    // Let the base class verify that the input column reflects the output   
    // of the upstream component.  
    return base.Validate();  
}  
```  
  
```vb  
Public  Overrides Function Validate() As DTSValidationStatus   
  
 Dim pbCancel As Boolean = False  
  
 ' Validate that there is one input.  
 If Not (ComponentMetaData.InputCollection.Count = 1) Then   
   ComponentMetaData.FireError(0, ComponentMetaData.Name, "Incorrect number of inputs.", "", 0, pbCancel)   
   Return DTSValidationStatus.VS_ISCORRUPT   
 End If   
  
 ' Validate that the UserName custom property is set.  
 If ComponentMetaData.CustomPropertyCollection("UserName").Value Is Nothing OrElse CType(ComponentMetaData.CustomPropertyCollection("UserName").Value, String).Length = 0 Then   
   ComponentMetaData.FireError(0, ComponentMetaData.Name, "The UserName property must be set.", "", 0, pbCancel)   
   Return DTSValidationStatus.VS_ISBROKEN   
 End If   
  
 ' Validate that there is one output.  
 If Not (ComponentMetaData.OutputCollection.Count = 1) Then   
   ComponentMetaData.FireError(0, ComponentMetaData.Name, "Incorrect number of outputs.", "", 0, pbCancel)   
   Return DTSValidationStatus.VS_ISCORRUPT   
 End If   
  
 ' Let the base class verify that the input column reflects the output   
 ' of the upstream component.  
  
 Return MyBase.Validate   
  
End Function  
```  
  
## <a name="reinitializemetadata-method"></a>Metodo ReinitializeMetaData  
 Il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReinitializeMetaData%2A> viene chiamato da Progettazione [!INCLUDE[ssIS](../../../includes/ssis-md.md)] quando si modifica un componente che restituisce <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus.VS_NEEDSNEWMETADATA> dal metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A>. I componenti devono contenere codice che rileva e corregge i problemi identificati dal componente durante la convalida.  
  
 Nell'esempio seguente viene illustrato un componente che rileva problemi durante la convalida e li corregge nel metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReinitializeMetaData%2A>.  
  
```csharp  
private bool areInputColumnsValid = true;  
public override DTSValidationStatus Validate()  
{  
    IDTSInput100 input = ComponentMetaData.InputCollection[0];  
    IDTSVirtualInput100 vInput = input.GetVirtualInput();  
  
    bool Cancel = false;  
    foreach (IDTSInputColumn100 column in input.InputColumnCollection)  
    {  
        try  
        {  
            IDTSVirtualInputColumn100 vColumn = vInput.VirtualInputColumnCollection.GetVirtualInputColumnByLineageID(column.LineageID);  
        }  
        catch  
        {  
            ComponentMetaData.FireError(0, ComponentMetaData.Name, "The input column " + column.IdentificationString + " does not match a column in the upstream component.", "", 0, out Cancel);  
            areInputColumnsValid = false;  
            return DTSValidationStatus.VS_NEEDSNEWMETADATA;  
        }  
    }  
  
    return DTSValidationStatus.VS_ISVALID;  
}  
public override void ReinitializeMetaData()  
{  
    if (!areInputColumnsValid)  
    {  
        IDTSInput100 input = ComponentMetaData.InputCollection[0];  
        IDTSVirtualInput100 vInput = input.GetVirtualInput();  
  
        foreach (IDTSInputColumn100 column in input.InputColumnCollection)  
        {  
            IDTSVirtualInputColumn100 vColumn = vInput.VirtualInputColumnCollection.GetVirtualInputColumnByLineageID(column.LineageID);  
  
            if (vColumn == null)  
                input.InputColumnCollection.RemoveObjectByID(column.ID);  
        }  
        areInputColumnsValid = true;  
    }  
}  
```  
  
```vb  
Private areInputColumnsValid As Boolean = True   
  
Public  Overrides Function Validate() As DTSValidationStatus   
 Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)   
 Dim vInput As IDTSVirtualInput100 = input.GetVirtualInput   
 Dim Cancel As Boolean = False   
 For Each column As IDTSInputColumn100 In input.InputColumnCollection   
   Try   
     Dim vColumn As IDTSVirtualInputColumn100 = vInput.VirtualInputColumnCollection.GetVirtualInputColumnByLineageID(column.LineageID)   
   Catch   
     ComponentMetaData.FireError(0, ComponentMetaData.Name, "The input column " + column.IdentificationString + " does not match a column in the upstream component.", "", 0, Cancel)   
     areInputColumnsValid = False   
     Return DTSValidationStatus.VS_NEEDSNEWMETADATA   
   End Try   
 Next   
 Return DTSValidationStatus.VS_ISVALID   
End Function   
  
Public  Overrides Sub ReinitializeMetaData()   
 If Not areInputColumnsValid Then   
   Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)   
   Dim vInput As IDTSVirtualInput100 = input.GetVirtualInput   
   For Each column As IDTSInputColumn100 In input.InputColumnCollection   
     Dim vColumn As IDTSVirtualInputColumn100 = vInput.VirtualInputColumnCollection.GetVirtualInputColumnByLineageID(column.LineageID)   
     If vColumn Is Nothing Then   
       input.InputColumnCollection.RemoveObjectByID(column.ID)   
     End If   
   Next   
   areInputColumnsValid = True   
 End If   
End Sub  
```  
  
