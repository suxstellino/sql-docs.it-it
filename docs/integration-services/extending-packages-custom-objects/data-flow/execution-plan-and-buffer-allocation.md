---
description: Piano di esecuzione e allocazione di buffer
title: Piano di esecuzione e allocazione di buffer | Microsoft Docs
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
- custom data flow components [Integration Services], buffer allocations
- custom data flow components [Integration Services], execution plans
- buffer allocations [Integration Services]
- data flow components [Integration Services], buffer allocations
- data flow components [Integration Services], execution plans
- execution plans [Integration Services]
ms.assetid: 679d9ff0-641e-47c3-abb8-d1a7dcb279dd
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5b9827f4943e21b117353af4aaa4ea825cef19d5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343690"
---
# <a name="execution-plan-and-buffer-allocation"></a>Piano di esecuzione e allocazione di buffer

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Prima dell'esecuzione, l'attività Flusso di dati esamina i propri componenti e genera un piano di esecuzione per ogni sequenza di componenti. In questa sezione vengono fornite informazioni sul piano di esecuzione, su come visualizzarlo e su come influisce sull'allocazione dei buffer di input e output.  
  
## <a name="understanding-the-execution-plan"></a>Informazioni sul piano di esecuzione  
 Un piano di esecuzione contiene thread di origine e thread di lavoro. Ogni thread contiene elenchi di operazioni che specificano elenchi di operazioni di output per i thread di origine oppure elenchi di operazioni di input e output per i thread di lavoro. I thread di origine in un piano di esecuzione rappresentano i componenti di origine nel flusso di dati e sono identificati nel piano di esecuzione da *SourceThreadn*, dove *n* è il numero in base zero del thread di origine.  
  
 Ogni thread di origine crea un buffer, imposta un listener e chiama il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> sul componente di origine. Si tratta del punto in cui viene avviata l'esecuzione e hanno origine i dati, quando il componente di origine inizia ad aggiungere righe nei buffer di output forniti dall'attività Flusso di dati. Dopo l'inizio dell'esecuzione dei thread di origine, il lavoro viene bilanciato tra thread di lavoro.  
  
 Un thread di lavoro può contenere elenchi di lavoro di input e di output e viene identificato nel piano di esecuzione come *WorkThreadn*, dove *n* è il numero in base zero del thread di lavoro. Questi thread contengono elenchi di operazioni di output quando il grafico contiene un componente con output asincroni.  
  
 Nel piano di esecuzione di esempio seguente viene rappresentato un flusso di dati che contiene un componente di origine connesso a una trasformazione con un output asincrono connesso a un componente di destinazione. Nell'esempio WorkThread0 contiene un elenco di operazioni di output perché il componente di trasformazione ha un output asincrono.  
  
```  
SourceThread0   
    Influences: 72 158   
    Output Work List   
        CreatePrimeBuffer of type 1 for output id 10   
        SetBufferListener: "WorkThread0" for input ID 73   
        CallPrimeOutput on component "OLE DB Source" (1)   
    End Output Work List   
    This thread drives 0 distributors   
End SourceThread0   
WorkThread0   
    Influences: 72 158   
    Input Work list, input ID 73   
        CallProcessInput on input ID 73 on component "Sort" (72) for view type 2   
    End Input Work list for input 73   
    Output Work List   
        CreatePrimeBuffer of type 3 for output id 74   
        SetBufferListener: "WorkThread1" for input ID 171with internal handoff   
        CallPrimeOutput on component "Sort" (72)   
    End Output Work List   
    This thread drives 0 distributors   
End WorkThread0   
WorkThread1   
    Influences: 158   
    Input Work list, input ID 171  
        CallProcessInput on input ID 171 on component "OLE DB Destination" (158) for view type 4  
    End Input Work list for input 171   
    Output Work List   
    End Output Work List   
    This thread drives 0 distributors   
End WorkThread1  
```  
  
> [!NOTE]  
>  Il piano di esecuzione viene generato ogni volta che un pacchetto viene eseguito e può essere acquisito con l'aggiunta di un provider di log al pacchetto, l'abilitazione della registrazione e la selezione dell'evento **PipelineExecutionPlan**.  
  
## <a name="understanding-buffer-allocation"></a>Informazioni sull'allocazione di buffer  
 In base al piano di esecuzione, l'attività Flusso di dati crea buffer che contengono le colonne definite negli output dei componenti del flusso di dati. Il buffer viene riutilizzato durante il flusso dei dati attraverso la sequenza dei componenti, finché non viene rilevato un componente con output asincroni. A questo punto viene creato un nuovo buffer, che contiene le colonne di output dell'output asincrono e le colonne di output dei componenti a valle.  
  
 Durante l'esecuzione, i componenti hanno accesso al buffer nell'origine o nel thread di lavoro corrente. Il buffer è un buffer di input, fornito dal metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>, o un buffer di output, fornito dal metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>. Anche la proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.Mode%2A> di <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer> identifica ogni buffer come buffer di input o di output.  
  
 I componenti di trasformazione con output asincroni ricevono il buffer di input esistente dal metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> e il nuovo buffer di output dal metodo <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>. Un componente di trasformazione con output asincroni è il solo tipo di componente del flusso di dati che riceve un buffer sia di input che di output.  
  
 Poiché il buffer fornito a un componente contiene probabilmente più colonne di quelle presenti nelle raccolte di colonne di output o di input del componente, gli sviluppatori di componenti possono chiamare il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSBufferManager100.FindColumnByLineageID%2A> per individuare una colonna nel buffer specificando il relativo **LineageID**.  
  
