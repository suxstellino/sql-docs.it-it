---
description: Monitoraggio dei contatori delle prestazioni con l'attività Script
title: Monitoraggio dei contatori delle prestazioni con l'attività Script | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- performance counters [Integration Services]
- SSIS Script task, performance counters
- custom performance counters [Integration Services]
- Script task [Integration Services], examples
- Script task [Integration Services], performance counters
- counters [Integration Services]
ms.assetid: 86609bf1-cae6-435e-a58d-41bdfc521e94
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b05cfaa612a9e08fb503ee9d4448f871d9cd7904
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344844"
---
# <a name="monitoring-performance-counters-with-the-script-task"></a>Monitoraggio dei contatori delle prestazioni con l'attività Script

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  È possibile che gli amministratori abbiano l'esigenza di monitorare le prestazioni dei pacchetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] che eseguono trasformazioni complesse su grandi quantità di dati. Lo spazio dei nomi **System.Diagnostics** delle classi [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] fornisce le classi per l'uso di contatori delle prestazioni esistenti e per la creazione di contatori delle prestazioni personalizzati.  
  
 I contatori delle prestazioni archiviano informazioni sulle prestazioni dell'applicazione che è possibile utilizzare per analizzare le prestazioni del software nel corso del tempo. Possono essere monitorati in locale o in remoto tramite lo strumento **Performance Monitor**. È possibile archiviare i valori dei contatori delle prestazioni in variabili per una successiva diramazione del flusso di controllo nel pacchetto.  
  
 In alternativa all'uso dei contatori delle prestazioni, è possibile generare l'evento <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireProgress%2A> tramite la proprietà <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Events%2A> dell'oggetto **Dts**. L'evento <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireProgress%2A> restituisce al runtime di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] informazioni relative sia allo stato incrementale che alla percentuale di completamento.  
  
> [!NOTE]  
>  Se si desidera creare un'attività da riutilizzare più facilmente con più pacchetti, è possibile utilizzare il codice di questo esempio di attività Script come punto iniziale per un'attività personalizzata. Per altre informazioni, vedere [Sviluppo di un'attività personalizzata](../../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md).  
  
## <a name="description"></a>Descrizione  
 Nell'esempio seguente viene creato un contatore delle prestazioni personalizzato che viene incrementato. Viene innanzitutto verificato se il contatore delle prestazioni esiste già. Se non è stato creato, lo script chiama il metodo **Create** dell'oggetto **PerformanceCounterCategory** per crearlo. Dopo la creazione del contatore delle prestazioni, lo script lo incrementa. Infine, viene seguita la procedura consigliata di chiamare il metodo **Close** sul contatore delle prestazioni quando non è più necessario.  
  
> [!NOTE]  
>  Per creare una nuova categoria di contatori delle prestazioni e un nuovo contatore delle prestazioni, è necessario disporre di diritti amministrativi. Inoltre, la nuova categoria e il nuovo contatore diventano persistenti nel computer dopo la creazione.  
  
#### <a name="to-configure-this-script-task-example"></a>Per configurare l'esempio di attività Script  
  
-   Usare un'istruzione **Imports** nel codice per importare lo spazio dei nomi **System.Diagnostics**.  
  
### <a name="example-code"></a>Codice di esempio  
  
```vb  
Public Sub Main()  
  
    Dim myCounter As PerformanceCounter  
  
    Try  
        'Create the performance counter if it does not already exist.  
        If Not _  
        PerformanceCounterCategory.Exists("TaskExample") Then  
            PerformanceCounterCategory.Create("TaskExample", _  
                "Task Performance Counter Example", "Iterations", _  
                "Number of times this task has been called.")  
        End If  
  
        'Initialize the performance counter.  
        myCounter = New PerformanceCounter("TaskExample", _  
            "Iterations", String.Empty, False)  
  
        'Increment the performance counter.  
        myCounter.Increment()  
  
         myCounter.Close()  
        Dts.TaskResult = ScriptResults.Success  
    Catch ex As Exception  
        Dts.Events.FireError(0, _  
            "Task Performance Counter Example", _  
            ex.Message & ControlChars.CrLf & ex.StackTrace, _  
            String.Empty, 0)  
        Dts.TaskResult = ScriptResults.Failure  
    End Try  
  
End Sub  
```  
  
```csharp  
  
public class ScriptMain  
{  
  
public void Main()  
        {  
  
            PerformanceCounter myCounter;  
  
            try  
            {  
                //Create the performance counter if it does not already exist.  
                if (!PerformanceCounterCategory.Exists("TaskExample"))  
                {  
                    PerformanceCounterCategory.Create("TaskExample", "Task Performance Counter Example", "Iterations", "Number of times this task has been called.");  
                }  
  
                //Initialize the performance counter.  
                myCounter = new PerformanceCounter("TaskExample", "Iterations", String.Empty, false);  
  
                //Increment the performance counter.  
                myCounter.Increment();  
  
                myCounter.Close();  
                Dts.TaskResult = (int)ScriptResults.Success;  
            }  
            catch (Exception ex)  
            {  
                Dts.Events.FireError(0, "Task Performance Counter Example", ex.Message + "\r" + ex.StackTrace, String.Empty, 0);  
                Dts.TaskResult = (int)ScriptResults.Failure;  
            }  
  
            Dts.TaskResult = (int)ScriptResults.Success;  
        }  
  
```  
