---
description: Gestione degli eventi a livello di programmazione
title: Gestione degli eventi a livello di programmazione | Microsoft Docs
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
- Integration Services packages, events
- SQL Server Integration Services packages, events
- SSIS events, programmatically handling
- packages [Integration Services], events
- DtsEventHandler object
- SSIS packages, events
- SSIS events
- events [Integration Services], programmatically
- tasks [Integration Services], events
- IDTSEvents interface
ms.assetid: 0f00bd66-efd5-4f12-9e1c-36195f739332
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a2800cbfc74bf87225f93d480d54777134be8f37
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355057"
---
# <a name="handling-events-programmatically"></a>Gestione degli eventi a livello di programmazione

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Il runtime di [!INCLUDE[ssIS](../../includes/ssis-md.md)] fornisce una raccolta di eventi che si verificano prima, durante e dopo la convalida e l'esecuzione di un pacchetto. Tali eventi possono essere acquisiti in due modi. Il primo metodo prevede di implementare l'interfaccia <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents> in una classe e specificare la classe come parametro per i metodi **Execute** e **Validate** del pacchetto. Il secondo metodo consiste nella creazione di oggetti <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler> che possono contenere altri oggetti di [!INCLUDE[ssIS](../../includes/ssis-md.md)], ad esempio attività e cicli, eseguiti quando si verifica un evento in <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents>. In questa sezione vengono descritti questi due metodi e vengono forniti esempi di codice per dimostrarne l'utilizzo.  
  
## <a name="receiving-idtsevents-callbacks"></a>Ricezione di callback IDTSEvents  
 Gli sviluppatori che compilano ed eseguono pacchetti a livello di programmazione possono ricevere notifiche di eventi durante il processo di convalida ed esecuzione tramite l'interfaccia <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents>. A tale scopo è possibile creare una classe che implementa l'interfaccia <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents> e specificare tale classe come parametro per i metodi **Execute** e **Validate** di un pacchetto. I metodi della classe vengono quindi chiamati dal motore di run-time quando si verificano gli eventi.  
  
 La classe <xref:Microsoft.SqlServer.Dts.Runtime.DefaultEvents> è una classe che implementa già l'interfaccia <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents>; pertanto, un'alternativa all'implementazione diretta di <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents> consiste nel derivare da <xref:Microsoft.SqlServer.Dts.Runtime.DefaultEvents> ed eseguire l'override degli eventi specifici ai quali si desidera rispondere. Specificare quindi la classe come parametro per i metodi **Validate** ed **Execute** di <xref:Microsoft.SqlServer.Dts.Runtime.Package> per ricevere i callback degli eventi.  
  
 Nell'esempio di codice seguente viene illustrata una classe che deriva da <xref:Microsoft.SqlServer.Dts.Runtime.DefaultEvents> ed esegue l'override del metodo <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents.OnPreExecute%2A>. La classe viene quindi specificata come parametro per i metodi **Validate** ed **Execute** del pacchetto.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Package p = new Package();  
      MyEventsClass eventsClass = new MyEventsClass();  
  
      p.Validate(null, null, eventsClass, null);  
      p.Execute(null, null, eventsClass, null, null);  
  
      Console.Read();  
    }  
  }  
  class MyEventsClass : DefaultEvents  
  {  
    public override void OnPreExecute(Executable exec, ref bool fireAgain)  
    {  
      // TODO: Add custom code to handle the event.  
      Console.WriteLine("The PreExecute event of the " +  
        exec.ToString() + " has been raised.");  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    Dim p As Package = New Package()  
    Dim eventsClass As MyEventsClass = New MyEventsClass()  
  
    p.Validate(Nothing, Nothing, eventsClass, Nothing)  
    p.Execute(Nothing, Nothing, eventsClass, Nothing, Nothing)  
  
    Console.Read()  
  
  End Sub  
  
End Module  
  
Class MyEventsClass  
  Inherits DefaultEvents  
  
  Public Overrides Sub OnPreExecute(ByVal exec As Executable, ByRef fireAgain As Boolean)  
  
    ' TODO: Add custom code to handle the event.  
    Console.WriteLine("The PreExecute event of the " & _  
      exec.ToString() & " has been raised.")  
  
  End Sub  
  
End Class  
```  
  
## <a name="creating-dtseventhandler-objects"></a>Creazione di oggetti DtsEventHandler  
 Il motore di run-time fornisce un sistema di gestione e notifica degli eventi stabile e ampiamente flessibile tramite l'oggetto <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler>. Questi oggetti consentono di progettare interi flussi di lavoro dall'interno del gestore eventi, che vengono eseguiti solo quando si verifica l'evento cui appartiene il gestore. L'oggetto <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler> è un contenitore eseguito quando viene generato l'evento corrispondente nel relativo oggetto padre. Questa architettura consente di creare flussi di lavoro isolati eseguiti in risposta a eventi che si verificano in un contenitore. Poiché gli oggetti <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler> sono sincroni, l'esecuzione riprende solo dopo che sono stati restituiti i gestori eventi collegati all'evento.  
  
 Nell'esempio di codice seguente viene illustrato come creare un oggetto <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler>. Nel codice viene aggiunto <xref:Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask> alla raccolta <xref:Microsoft.SqlServer.Dts.Runtime.Package.Executables%2A> del pacchetto, quindi viene creato un oggetto <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler> per l'evento <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents.OnError%2A> dell'attività. Un oggetto <xref:Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask> viene aggiunto al gestore eventi, che viene eseguito quando si verifica l'evento <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents.OnError%2A> per il primo oggetto <xref:Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask>. In questo esempio si presuppone che si disponga di un file denominato C:\Windows\Temp\DemoFile.txt per il test. La prima volta che si esegue l'esempio, il file viene copiato correttamente e il gestore eventi non viene chiamato. La seconda volta che si esegue l'esempio, il primo oggetto <xref:Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask> non riesce a copiare il file (perché il valore di <xref:Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask.OverwriteDestinationFile%2A> è **false**), viene chiamato il gestore eventi, il secondo oggetto <xref:Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask> elimina il file di origine, quindi il pacchetto segnala un problema a causa dell'errore che si è verificato.  
  
## <a name="example"></a>Esempio  
  
```csharp  
using System;  
using System.IO;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Tasks.FileSystemTask;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      string f = "DemoFile.txt";  
      Executable e;  
      TaskHost th;  
      FileSystemTask fst;  
  
      Package p = new Package();  
  
      p.Variables.Add("sourceFile", true, String.Empty,  
        @"C:\Windows\Temp\" + f);  
      p.Variables.Add("destinationFile", true, String.Empty,  
        Path.Combine(Directory.GetCurrentDirectory(), f));  
  
      // Create a first File System task and add it to the package.  
      // This task tries to copy a file from one folder to another.  
      e = p.Executables.Add("STOCK:FileSystemTask");  
      th = e as TaskHost;  
      th.Name = "FileSystemTask1";  
      fst = th.InnerObject as FileSystemTask;  
  
      fst.Operation = DTSFileSystemOperation.CopyFile;  
      fst.OverwriteDestinationFile = false;  
      fst.Source = "sourceFile";  
      fst.IsSourcePathVariable = true;  
      fst.Destination = "destinationFile";  
      fst.IsDestinationPathVariable = true;  
  
      // Add an event handler for the FileSystemTask's OnError event.  
      DtsEventHandler ehOnError = (DtsEventHandler)th.EventHandlers.Add("OnError");  
  
      // Create a second File System task and add it to the event handler.  
      // This task deletes the source file if the event handler is called.  
      e = ehOnError.Executables.Add("STOCK:FileSystemTask");  
      th = e as TaskHost;  
      th.Name = "FileSystemTask2";  
      fst = th.InnerObject as FileSystemTask;  
  
      fst.Operation = DTSFileSystemOperation.DeleteFile;  
      fst.Source = "sourceFile";  
      fst.IsSourcePathVariable = true;  
  
      DTSExecResult vr = p.Validate(null, null, null, null);  
      Console.WriteLine("ValidationResult = " + vr.ToString());  
      if (vr == DTSExecResult.Success)  
      {  
        DTSExecResult er = p.Execute(null, null, null, null, null);  
        Console.WriteLine("ExecutionResult = " + er.ToString());  
        if (er == DTSExecResult.Failure)  
          if (!File.Exists(@"C:\Windows\Temp\" + f))  
            Console.WriteLine("Source file has been deleted by the event handler.");  
      }  
      Console.Read();  
    }  
  }  
}  
```  
  
```vb  
Imports System.IO  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Tasks.FileSystemTask  
  
Module Module1  
  
  Sub Main()  
  
    Dim f As String = "DemoFile.txt"  
    Dim e As Executable  
    Dim th As TaskHost  
    Dim fst As FileSystemTask  
  
    Dim p As Package = New Package()  
  
    p.Variables.Add("sourceFile", True, String.Empty, _  
      "C:\Windows\Temp\" & f)  
    p.Variables.Add("destinationFile", True, String.Empty, _  
      Path.Combine(Directory.GetCurrentDirectory(), f))  
  
    ' Create a first File System task and add it to the package.  
    ' This task tries to copy a file from one folder to another.  
    e = p.Executables.Add("STOCK:FileSystemTask")  
    th = CType(e, TaskHost)  
    th.Name = "FileSystemTask1"  
    fst = CType(th.InnerObject, FileSystemTask)  
  
    fst.Operation = DTSFileSystemOperation.CopyFile  
    fst.OverwriteDestinationFile = False  
    fst.Source = "sourceFile"  
    fst.IsSourcePathVariable = True  
    fst.Destination = "destinationFile"  
    fst.IsDestinationPathVariable = True  
  
    ' Add an event handler for the FileSystemTask's OnError event.  
    Dim ehOnError As DtsEventHandler = CType(th.EventHandlers.Add("OnError"), DtsEventHandler)  
  
    ' Create a second File System task and add it to the event handler.  
    ' This task deletes the source file if the event handler is called.  
    e = ehOnError.Executables.Add("STOCK:FileSystemTask")  
    th = CType(e, TaskHost)  
    th.Name = "FileSystemTask1"  
    fst = CType(th.InnerObject, FileSystemTask)  
  
    fst.Operation = DTSFileSystemOperation.DeleteFile  
    fst.Source = "sourceFile"  
    fst.IsSourcePathVariable = True  
  
    Dim vr As DTSExecResult = p.Validate(Nothing, Nothing, Nothing, Nothing)  
    Console.WriteLine("ValidationResult = " + vr.ToString())  
    If vr = DTSExecResult.Success Then  
      Dim er As DTSExecResult = p.Execute(Nothing, Nothing, Nothing, Nothing, Nothing)  
      Console.WriteLine("ExecutionResult = " + er.ToString())  
      If er = DTSExecResult.Failure Then  
        If Not File.Exists("C:\Windows\Temp\" + f) Then  
          Console.WriteLine("Source file has been deleted by the event handler.")  
        End If  
      End If  
    End If  
    Console.Read()  
  
  End Sub  
  
End Module  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Gestori eventi di Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-event-handlers.md)   
 [Aggiunta di un gestore eventi a un pacchetto](../integration-services-ssis-event-handlers.md)  
  
