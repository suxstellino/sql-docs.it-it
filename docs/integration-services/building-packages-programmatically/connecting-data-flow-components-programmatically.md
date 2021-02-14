---
description: Connessione dei componenti del flusso di dati a livello di programmazione
title: Connessione dei componenti del flusso di dati a livello di programmazione | Microsoft Docs
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
- data flow task [Integration Services], components
- paths [Integration Services], components
- components [Integration Services], data flow
- data flow [Integration Services], components
ms.assetid: 404ecab7-7698-447b-93d6-dd256beb11ff
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 73e8eb7b506050489581ad6b4d00b02a51049f48
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355069"
---
# <a name="connecting-data-flow-components-programmatically"></a>Connessione dei componenti del flusso di dati a livello di programmazione

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Dopo avere aggiunto componenti all'attività Flusso di dati, connetterli per creare un albero di esecuzione che rappresenti il flusso di dati dalle origini attraverso le trasformazioni alle destinazioni. Utilizzare oggetti <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSPath100> per connettere i componenti nel flusso di dati.  
  
## <a name="creating-a-path"></a>Creazione di un percorso  
 Chiamare il metodo New della proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.MainPipeClass.PathCollection%2A> dell'interfaccia <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.MainPipe> per creare un nuovo percorso e aggiungerlo alla raccolta di percorsi nell'attività Flusso di dati. Questo metodo restituisce un nuovo oggetto <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSPath100> disconnesso, che verrà quindi utilizzato per connettere due componenti.  
  
 Chiamare il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSPath100.AttachPathAndPropagateNotifications%2A> per connettere il percorso e notificare ai componenti partecipanti nel percorso che sono stati connessi. Questo metodo accetta <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100> del componente a monte e <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSInput100> del componente a valle come parametri. Per impostazione predefinita, la chiamata al metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A> del componente crea un singolo input per i componenti che includono input e un singolo output per i componenti che includono output. Nell'esempio seguente vengono utilizzati l'output predefinito dell'origine e l'input predefinito della destinazione.  
  
## <a name="next-step"></a>passaggio successivo  
 Dopo aver stabilito un percorso tra due componenti, il passaggio successivo consiste nell'eseguire il mapping delle colonne di input nel componente a valle, descritto nell'argomento successivo [Selezione delle colonne di input a livello di programmazione](../../integration-services/building-packages-programmatically/selecting-input-columns-programmatically.md).  
  
## <a name="sample"></a>Esempio  
 Nell'esempio di codice seguente è illustrato come stabilire un percorso tra due componenti.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Package package = new Package();  
      Executable e = package.Executables.Add("STOCK:PipelineTask");  
      TaskHost thMainPipe = e as TaskHost;  
      MainPipe dataFlowTask = thMainPipe.InnerObject as MainPipe;  
  
      // Create the source component.    
      IDTSComponentMetaData100 source =  
        dataFlowTask.ComponentMetaDataCollection.New();  
      source.ComponentClassID = "DTSAdapter.OleDbSource";  
      CManagedComponentWrapper srcDesignTime = source.Instantiate();  
      srcDesignTime.ProvideComponentProperties();  
  
      // Create the destination component.  
      IDTSComponentMetaData100 destination =  
        dataFlowTask.ComponentMetaDataCollection.New();  
      destination.ComponentClassID = "DTSAdapter.OleDbDestination";  
      CManagedComponentWrapper destDesignTime = destination.Instantiate();  
      destDesignTime.ProvideComponentProperties();  
  
      // Create the path.  
      IDTSPath100 path = dataFlowTask.PathCollection.New();  
      path.AttachPathAndPropagateNotifications(source.OutputCollection[0],  
        destination.InputCollection[0]);  
    }  
  }  
```  
  
 }  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
  
Module Module1  
  
  Sub Main()  
  
    Dim package As Microsoft.SqlServer.Dts.Runtime.Package = _  
      New Microsoft.SqlServer.Dts.Runtime.Package()  
    Dim e As Executable = package.Executables.Add("STOCK:PipelineTask")  
    Dim thMainPipe As Microsoft.SqlServer.Dts.Runtime.TaskHost = _  
      CType(e, Microsoft.SqlServer.Dts.Runtime.TaskHost)  
    Dim dataFlowTask As MainPipe = CType(thMainPipe.InnerObject, MainPipe)  
  
    ' Create the source component.    
    Dim source As IDTSComponentMetaData100 = _  
      dataFlowTask.ComponentMetaDataCollection.New()  
    source.ComponentClassID = "DTSAdapter.OleDbSource"  
    Dim srcDesignTime As CManagedComponentWrapper = source.Instantiate()  
    srcDesignTime.ProvideComponentProperties()  
  
    ' Create the destination component.  
    Dim destination As IDTSComponentMetaData100 = _  
      dataFlowTask.ComponentMetaDataCollection.New()  
    destination.ComponentClassID = "DTSAdapter.OleDbDestination"  
    Dim destDesignTime As CManagedComponentWrapper = destination.Instantiate()  
    destDesignTime.ProvideComponentProperties()  
  
    ' Create the path.  
    Dim path As IDTSPath100 = dataFlowTask.PathCollection.New()  
    path.AttachPathAndPropagateNotifications(source.OutputCollection(0), _  
      destination.InputCollection(0))  
  
  End Sub  
  
End Module  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Selezione delle colonne di input a livello di programmazione](../../integration-services/building-packages-programmatically/selecting-input-columns-programmatically.md)  
  
  
