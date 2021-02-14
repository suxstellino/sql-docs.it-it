---
description: Aggiunta di attività a livello di programmazione
title: Aggiunta di attività a livello di programmazione | Microsoft Docs
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
- tasks [Integration Services], packages
- adding package tasks
ms.assetid: 5d4652d5-228c-4238-905c-346dd8503fdf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1650323cede5f91401faa97e820917b2549d5050
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355113"
---
# <a name="adding-tasks-programmatically"></a>Aggiunta di attività a livello di programmazione

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  È possibile aggiungere attività ai tipi di oggetti seguenti nel motore di runtime:  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.Package>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.Sequence>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.ForLoop>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.ForEachLoop>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler>  
  
 Queste classi vengono considerate contenitori ed ereditano tutte la proprietà <xref:Microsoft.SqlServer.Dts.Runtime.IDTSSequence.Executables%2A>. I contenitori possono contenere una raccolta di attività, ovvero oggetti eseguibili elaborati dal runtime durante l'esecuzione del contenitore. L'ordine di esecuzione degli oggetti nella raccolta è determinato da qualsiasi oggetto <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint> impostato su ogni attività nei contenitori. I vincoli di precedenza rendono possibile la diramazione dell'esecuzione in base all'esito positivo, all'esito negativo o al completamento di un oggetto <xref:Microsoft.SqlServer.Dts.Runtime.Executable> nella raccolta.  
  
 Ogni contenitore include una raccolta <xref:Microsoft.SqlServer.Dts.Runtime.Executables> che contiene i singoli oggetti <xref:Microsoft.SqlServer.Dts.Runtime.Executable>. Ogni attività eseguibile eredita e implementa il metodo <xref:Microsoft.SqlServer.Dts.Runtime.Executable.Execute%2A> e il metodo <xref:Microsoft.SqlServer.Dts.Runtime.Executable.Validate%2A>. Questi due metodi vengono chiamati dal motore di runtime per elaborare ogni oggetto <xref:Microsoft.SqlServer.Dts.Runtime.Executable>.  
  
 Per aggiungere un'attività a un pacchetto, è necessario un contenitore con una raccolta <xref:Microsoft.SqlServer.Dts.Runtime.Executables> esistente. Nella maggior parte dei casi, l'attività che verrà aggiunta alla raccolta è un pacchetto. Per aggiungere l'eseguibile della nuova attività nella raccolta per tale contenitore, chiamare il metodo <xref:Microsoft.SqlServer.Dts.Runtime.Executables.Add%2A>. Il metodo include un solo parametro, ovvero una stringa che contiene il CLSID, il PROGID, il moniker STOCK o l'oggetto <xref:Microsoft.SqlServer.Dts.Runtime.TaskInfo.CreationName%2A> dell'attività da aggiungere.  
  
## <a name="task-names"></a>Nomi delle attività  
 Anche se è possibile specificare un'attività in base al nome o all'ID, il moniker **STOCK** è il parametro usato più di frequente nel metodo <xref:Microsoft.SqlServer.Dts.Runtime.Executables.Add%2A>. Per aggiungere un'attività a un eseguibile identificato dal moniker **STOCK**, usare la sintassi seguente:  
  
```csharp  
Executable exec = package.Executables.Add("STOCK:BulkInsertTask");  
  
```  
  
```vb  
Dim exec As Executable = package.Executables.Add("STOCK:BulkInsertTask")  
  
```  
  
 L'elenco seguente illustra i nomi di ogni attività usati dopo il moniker **STOCK**.  
  
-   ActiveXScriptTask  
  
-   BulkInsertTask  
  
-   ExecuteProcessTask  
  
-   ExecutePackageTask  
  
-   Exec80PackageTask  
  
-   FileSystemTask  
  
-   FTPTask  
  
-   MSMQTask  
  
-   PipelineTask  
  
-   ScriptTask  
  
-   SendMailTask  
  
-   SQLTask  
  
-   TransferStoredProceduresTask  
  
-   TransferLoginsTask  
  
-   TransferErrorMessagesTask  
  
-   TransferJobsTask  
  
-   TransferObjectsTask  
  
-   TransferDatabaseTask  
  
-   WebServiceTask  
  
-   WmiDataReaderTask  
  
-   WmiEventWatcherTask  
  
-   XMLTask  
  
 Se si preferisce una sintassi più esplicita o se l'attività che si desidera aggiungere non include un moniker STOCK, è possibile aggiungere l'attività all'eseguibile utilizzandone il nome lungo. Questa sintassi richiede anche la specifica del numero di versione dell'attività.  
  
```csharp  
Executable exec = package.Executables.Add(  
  "Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptTask, " +  
  "Microsoft.SqlServer.ScriptTask, Version=10.0.000.0, " +  
  "Culture=neutral, PublicKeyToken=89845dcd8080cc91");  
```  
  
```vb  
Dim exec As Executable = package.Executables.Add( _  
  "Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptTask, " & _  
  "Microsoft.SqlServer.ScriptTask, Version=10.0.000.0, " & _  
  "Culture=neutral, PublicKeyToken=89845dcd8080cc91")  
```  
  
 È possibile ottenere il nome lungo dell'attività a livello di programmazione, senza la necessità di specificare la versione dell'attività, usando la proprietà **AssemblyQualifiedName** della classe, come illustrato nell'esempio seguente. Per questo esempio è richiesto un riferimento all'assembly Microsoft.SqlServer.SQLTask.  
  
```csharp  
using Microsoft.SqlServer.Dts.Tasks.ExecuteSQLTask;  
...  
      Executable exec = package.Executables.Add(  
        typeof(Microsoft.SqlServer.Dts.Tasks.ExecuteSQLTask.ExecuteSQLTask).AssemblyQualifiedName);  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Tasks.ExecuteSQLTask  
...  
    Dim exec As Executable = package.Executables.Add( _  
      GetType(Microsoft.SqlServer.Dts.Tasks.ExecuteSQLTask.ExecuteSQLTask).AssemblyQualifiedName)  
```  
  
 L'esempio di codice seguente illustra come creare una raccolta <xref:Microsoft.SqlServer.Dts.Runtime.Executables> da un nuovo pacchetto, quindi aggiungere un'attività File system e un'attività Inserimento bulk alla raccolta tramite i relativi moniker **STOCK**. Per questo esempio è richiesto un riferimento agli assembly Microsoft.SqlServer.FileSystemTask e Microsoft.SqlServer.BulkInsertTask.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Tasks.FileSystemTask;  
using Microsoft.SqlServer.Dts.Tasks.BulkInsertTask;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Package p = new Package();  
      // Add a File System task to the package.  
      Executable exec1 = p.Executables.Add("STOCK:FileSystemTask");  
      TaskHost thFileSystemTask = exec1 as TaskHost;  
      // Add a Bulk Insert task to the package.  
      Executable exec2 = p.Executables.Add("STOCK:BulkInsertTask");  
      TaskHost thBulkInsertTask = exec2 as TaskHost;  
  
      // Iterate through the package Executables collection.  
      Executables pExecs = p.Executables;  
      foreach (Executable pExec in pExecs)  
      {  
        TaskHost taskHost = (TaskHost)pExec;  
        Console.WriteLine("Type {0}", taskHost.InnerObject.ToString());  
      }  
      Console.Read();  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Tasks.FileSystemTask  
Imports Microsoft.SqlServer.Dts.Tasks.BulkInsertTask  
  
Module Module1  
  
  Sub Main()  
  
    Dim p As Package = New Package()  
    ' Add a File System task to the package.  
    Dim exec1 As Executable = p.Executables.Add("STOCK:FileSystemTask")  
    Dim thFileSystemTask As TaskHost = CType(exec1, TaskHost)  
    ' Add a Bulk Insert task to the package.  
    Dim exec2 As Executable = p.Executables.Add("STOCK:BulkInsertTask")  
    Dim thBulkInsertTask As TaskHost = CType(exec2, TaskHost)  
  
    ' Iterate through the package Executables collection.  
    Dim pExecs As Executables = p.Executables  
    Dim pExec As Executable  
    For Each pExec In pExecs  
      Dim taskHost As TaskHost = CType(pExec, TaskHost)  
      Console.WriteLine("Type {0}", taskHost.InnerObject.ToString())  
    Next  
    Console.Read()  
  
  End Sub  
  
End Module  
```  
  
 **Esempio di output**  
  
 `Type Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask`  
  
 `Type Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask`  
  
## <a name="taskhost-container"></a>Contenitore TaskHost  
 La classe <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> è un contenitore che non viene visualizzato nell'interfaccia utente grafica, ma è molto importante nella programmazione. Questa classe è un wrapper per ogni attività. Le attività aggiunte al pacchetto utilizzando il metodo <xref:Microsoft.SqlServer.Dts.Runtime.Executables.Add%2A> come oggetto <xref:Microsoft.SqlServer.Dts.Runtime.Executable> possono essere sottoposte a cast come oggetto <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>. Quando si esegue il cast di un'attività come <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>, è possibile utilizzare proprietà e metodi aggiuntivi sull'attività. È inoltre possibile accedere all'attività stessa tramite la proprietà <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.InnerObject%2A> di <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>. A seconda delle esigenze, è possibile decidere di mantenere l'attività come oggetto <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> in modo da poterne utilizzare le proprietà tramite la raccolta <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.Properties%2A>. Il vantaggio dell'utilizzo di <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.Properties%2A> è la possibilità di scrivere codice più generico. Se è necessario codice molto specifico per un'attività, è consigliabile eseguire il cast dell'attività nell'oggetto appropriato.  
  
 Nell'esempio di codice seguente è illustrato come eseguire il cast di un oggetto <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>, thBulkInsertTask, che contiene <xref:Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask>, in un oggetto <xref:Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask>.  
  
```csharp  
BulkInsertTask myTask = thBulkInsertTask.InnerObject as BulkInsertTask;  
```  
  
```vb  
Dim myTask As BulkInsertTask = CType(thBulkInsertTask.InnerObject, BulkInsertTask)  
```  
  
 Nell'esempio di codice seguente è illustrato come eseguire il cast dell'eseguibile in <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>, quindi utilizzare la proprietà <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.InnerObject%2A> per determinare quale tipo di eseguibile è contenuto nell'host.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Tasks.FileSystemTask;  
using Microsoft.SqlServer.Dts.Tasks.BulkInsertTask;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Package p = new Package();  
      // Add a File System task to the package.  
      Executable exec1 = p.Executables.Add("STOCK:FileSystemTask");  
      TaskHost thFileSystemTask1 = exec1 as TaskHost;  
      // Add a Bulk Insert task to the package.  
      Executable exec2 = p.Executables.Add("STOCK:BulkInsertTask");  
      TaskHost thFileSystemTask2 = exec2 as TaskHost;  
  
      // Iterate through the package Executables collection.  
      Executables pExecs = p.Executables;  
      foreach (Executable pExec in pExecs)  
      {  
        TaskHost taskHost = (TaskHost)pExec;  
        if (taskHost.InnerObject is Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask)  
        {  
          // Do work with FileSystemTask here.  
          Console.WriteLine("Found task of type {0}", taskHost.InnerObject.ToString());  
        }  
        else if (taskHost.InnerObject is Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask)  
        {  
          // Do work with BulkInsertTask here.  
          Console.WriteLine("Found task of type {0}", taskHost.InnerObject.ToString());  
        }  
        // Add additional statements to check InnerObject, if desired.  
      }  
      Console.Read();  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Tasks.FileSystemTask  
Imports Microsoft.SqlServer.Dts.Tasks.BulkInsertTask  
  
Module Module1  
  
  Sub Main()  
  
    Dim p As Package = New Package()  
    ' Add a File System task to the package.  
    Dim exec1 As Executable = p.Executables.Add("STOCK:FileSystemTask")  
    Dim thFileSystemTask1 As TaskHost = CType(exec1, TaskHost)  
    ' Add a Bulk Insert task to the package.  
    Dim exec2 As Executable = p.Executables.Add("STOCK:BulkInsertTask")  
    Dim thFileSystemTask2 As TaskHost = CType(exec2, TaskHost)  
  
    ' Iterate through the package Executables collection.  
    Dim pExecs As Executables = p.Executables  
    Dim pExec As Executable  
    For Each pExec In pExecs  
      Dim taskHost As TaskHost = CType(pExec, TaskHost)  
      If TypeOf taskHost.InnerObject Is Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask Then  
        ' Do work with FileSystemTask here.  
        Console.WriteLine("Found task of type {0}", taskHost.InnerObject.ToString())  
      ElseIf TypeOf taskHost.InnerObject Is Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask Then  
        ' Do work with BulkInsertTask here.  
        Console.WriteLine("Found task of type {0}", taskHost.InnerObject.ToString())  
      End If  
      ' Add additional statements to check InnerObject, if desired.  
    Next  
    Console.Read()  
  
  End Sub  
  
End Module  
```  
  
 **Esempio di output**  
  
 `Found task of type Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask`  
  
 `Found task of type Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask`  
  
 L'istruzione <xref:Microsoft.SqlServer.Dts.Runtime.Executables.Add%2A> restituisce un eseguibile di cui viene eseguito il cast in un oggetto <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> dall'oggetto <xref:Microsoft.SqlServer.Dts.Runtime.Executable> appena creato.  
  
 Per impostare proprietà o chiamare metodi sul nuovo oggetto, sono disponibili due opzioni:  
  
1.  Utilizzare la raccolta <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.Properties%2A> di <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>. Ad esempio, per ottenere una proprietà dall'oggetto, utilizzare `th.Properties["propertyname"].GetValue(th))`. Per impostare una proprietà, utilizzare `th.Properties["propertyname"].SetValue(th, <value>);`.  
  
2.  Eseguire il cast di <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.InnerObject%2A> di <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> nella classe dell'attività. Ad esempio, per eseguire il cast dell'attività Inserimento bulk in <xref:Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask> dopo che è stata aggiunta a un pacchetto come <xref:Microsoft.SqlServer.Dts.Runtime.Executable> e successivamente ne è stato eseguito il cast in <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>, utilizzare `BulkInsertTask myTask = th.InnerObject as BulkInsertTask;`.  
  
 L'utilizzo della classe <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> nel codice, invece dell'esecuzione del cast nella classe specifica dell'attività, presenta i vantaggi seguenti:  
  
-   Il provider <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost><xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.Properties%2A> non richiede un riferimento all'assembly nel codice.  
  
-   È possibile progettare routine generiche che funzionano per qualsiasi attività, perché non è necessario conoscere il nome dell'attività in fase di compilazione. Tali routine generiche includono i metodi in cui si passa il nome dell'attività al metodo e il codice del metodo funziona per tutte le attività. Si tratta di un metodo efficace per la scrittura di codice di test.  
  
 L'esecuzione del cast da <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> nella classe specifica dell'attività presenta i vantaggi seguenti:  
  
-   Il progetto di Visual Studio rende disponibile il completamento delle istruzioni (IntelliSense).  
  
-   È possibile che il codice venga eseguito più velocemente.  
  
-   Gli oggetti specifici dell'attività consentono l'associazione anticipata e le risultanti ottimizzazioni. Per ulteriori informazioni sull'associazione anticipata e tardiva, vedere l'argomento corrispondente in Concetti sul linguaggio Visual Basic.  
  
 L'esempio di codice seguente si basa sul concetto di riutilizzo del codice dell'attività. Anziché eseguire il cast delle attività negli equivalenti specifici della classe, nell'esempio di codice viene illustrato come eseguire il cast dell'eseguibile in un oggetto <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>, quindi viene utilizzato <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.Properties%2A> per scrivere codice generico per tutte le attività.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Package package = new Package();  
  
      string[] tasks = { "STOCK:SQLTask", "STOCK:ScriptTask",   
        "STOCK:ExecuteProcessTask", "STOCK:PipelineTask",   
        "STOCK:FTPTask", "STOCK:SendMailTask", "STOCK:MSMQTask" };  
  
      foreach (string s in tasks)  
      {  
        TaskHost taskhost = package.Executables.Add(s) as TaskHost;  
        DtsProperties props = taskhost.Properties;  
        Console.WriteLine("Enumerating properties on " + taskhost.Name);  
        Console.WriteLine(" TaskHost.InnerObject is " + taskhost.InnerObject.ToString());  
        Console.WriteLine();  
  
        foreach (DtsProperty prop in props)  
        {  
          Console.WriteLine("Properties for " + prop.Name);  
          Console.WriteLine("Name : " + prop.Name);  
          Console.WriteLine("Type : " + prop.Type.ToString());  
          Console.WriteLine("Readable : " + prop.Get.ToString());  
          Console.WriteLine("Writable : " + prop.Set.ToString());  
          Console.WriteLine();  
        }  
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
  
    Dim package As Package = New Package()  
  
    Dim tasks() As String = New String() {"STOCK:SQLTask", "STOCK:ScriptTask", _  
              "STOCK:ExecuteProcessTask", "STOCK:PipelineTask", _  
              "STOCK:FTPTask", "STOCK:SendMailTask", "STOCK:MSMQTask"}  
  
    For Each s As String In tasks  
  
      Dim taskhost As TaskHost = CType(package.Executables.Add(s), TaskHost)  
      Dim props As DtsProperties = taskhost.Properties  
      Console.WriteLine("Enumerating properties on " & taskhost.Name)  
      Console.WriteLine(" TaskHost.InnerObject is " & taskhost.InnerObject.ToString())  
      Console.WriteLine()  
  
      For Each prop As DtsProperty In props  
        Console.WriteLine("Properties for " + prop.Name)  
        Console.WriteLine(" Name : " + prop.Name)  
        Console.WriteLine(" Type : " + prop.Type.ToString())  
        Console.WriteLine(" Readable : " + prop.Get.ToString())  
        Console.WriteLine(" Writable : " + prop.Set.ToString())  
        Console.WriteLine()  
      Next  
  
    Next  
    Console.Read()  
  
  End Sub  
  
End Module  
```  
  
## <a name="external-resources"></a>Risorse esterne  
 Intervento nel blog sull'[aggiornamento di EzAPI per SQL Server 2012](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ezapi-updated-for-sql-server-2012/ba-p/388042) sul sito Web blogs.msdn.com.  

## <a name="see-also"></a>Vedere anche  
 [Connessione di attività a livello di programmazione](../../integration-services/building-packages-programmatically/connecting-tasks-programmatically.md)  
  
  
