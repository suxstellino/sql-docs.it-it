---
description: Individuazione dei componenti del flusso di dati a livello di programmazione
title: Individuazione dei componenti del flusso di dati a livello di programmazione | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- PipelineComponentInfos collection
- data flow task [Integration Services], components
- discovering data flow components
- components [Integration Services], data flow
- data flow [Integration Services], components
ms.assetid: ff92a96a-8af6-4532-82cc-c0bbff92401b
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f575dda54bd0c6ce21debd4f283f77a3eb1fe569
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355042"
---
# <a name="discovering-data-flow-components-programmatically"></a>Individuazione dei componenti del flusso di dati a livello di programmazione

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Dopo avere aggiunto un'attività Flusso di dati a un pacchetto, il passaggio successivo consiste nel determinare quali componenti flusso di dati sono disponibili per l'utilizzo. È possibile individuare a livello di programmazione le origini, le trasformazioni e le destinazioni del flusso di dati installate e disponibili nel computer locale. Per altre informazioni sull'aggiunta di un'attività Flusso di dati al pacchetto, vedere [Aggiunta dell'attività Flusso di dati a livello di programmazione](../../integration-services/building-packages-programmatically/adding-the-data-flow-task-programmatically.md).  
  
## <a name="discovering-components"></a>Individuazione di componenti  
 La classe <xref:Microsoft.SqlServer.Dts.Runtime.Application> fornisce la raccolta <xref:Microsoft.SqlServer.Dts.Runtime.Application.PipelineComponentInfos%2A>, che contiene un oggetto <xref:Microsoft.SqlServer.Dts.Runtime.PipelineComponentInfo> per ogni componente installato correttamente nel computer locale. Ogni oggetto <xref:Microsoft.SqlServer.Dts.Runtime.PipelineComponentInfo> contiene informazioni su un componente, ad esempio il nome, la descrizione e il nome di creazione. È possibile utilizzare il valore restituito nella proprietà <xref:Microsoft.SqlServer.Dts.Runtime.PipelineComponentInfo.CreationName%2A> per impostare la proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ComponentClassID%2A> di <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> quando si aggiunge un componente a un pacchetto.  
  
## <a name="next-step"></a>passaggio successivo  
 Dopo aver individuato i componenti disponibili, il passaggio successivo consiste nell'aggiunta e nella configurazione dei componenti, come descritto nell'argomento seguente, [Aggiunta di componenti del flusso di dati a livello di programmazione](../../integration-services/building-packages-programmatically/adding-data-flow-components-programmatically.md).  
  
## <a name="sample"></a>Esempio  
 Nell'esempio di codice seguente è illustrato come enumerare la raccolta <xref:Microsoft.SqlServer.Dts.Runtime.PipelineComponentInfos> dell'oggetto <xref:Microsoft.SqlServer.Dts.Runtime.Application> per individuare a livello di programmazione i componenti del flusso di dati disponibili nel computer locale. Questo esempio richiede un riferimento all'assembly Microsoft.SqlServer.ManagedDTS.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Application application = new Application();  
      PipelineComponentInfos componentInfos = application.PipelineComponentInfos;  
  
      foreach (PipelineComponentInfo componentInfo in componentInfos)  
      {  
        Console.WriteLine("Name: " + componentInfo.Name + "\n" +  
          " CreationName: " + componentInfo.CreationName + "\n");  
      }  
      Console.Read();  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    Dim application As Application = New Application()  
  
    Dim componentInfos As PipelineComponentInfos = application.PipelineComponentInfos  
  
    For Each componentInfo As PipelineComponentInfo In componentInfos  
      Console.WriteLine("Name: " & componentInfo.Name & vbCrLf & _  
        " CreationName: " & componentInfo.CreationName & vbCrLf)  
    Next  
  
    Console.Read()  
  
  End Sub  
  
End Module  
```
  
## <a name="see-also"></a>Vedere anche  
 [Aggiunta di componenti del flusso di dati a livello di programmazione](../../integration-services/building-packages-programmatically/adding-data-flow-components-programmatically.md)  
  
  
