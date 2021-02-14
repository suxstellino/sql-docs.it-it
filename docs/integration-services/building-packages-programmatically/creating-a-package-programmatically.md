---
description: Creazione di un pacchetto a livello di programmazione
title: Creazione di un pacchetto a livello di programmazione | Microsoft Docs
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
- SSIS packages, creating
- Integration Services packages, creating
- packages [Integration Services], creating
- SQL Server Integration Services packages, creating
ms.assetid: e44bcc70-32d3-43e8-a84b-29aef819d5d3
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6f7d955978a089a3a9c68420658dcc27ae3f12dd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355080"
---
# <a name="creating-a-package-programmatically"></a>Creazione di un pacchetto a livello di programmazione

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  L'oggetto <xref:Microsoft.SqlServer.Dts.Runtime.Package> rappresenta il contenitore di livello principale per tutti gli altri oggetti di una soluzione di progetto di [!INCLUDE[ssIS](../../includes/ssis-md.md)]. Essendo il contenitore di livello principale, il pacchetto è il primo oggetto creato. Gli oggetti successivi vengono aggiunti e quindi eseguiti nel contesto del pacchetto. Il pacchetto non sposta né trasforma dati, ma si basa sulle attività che contiene per eseguire questa operazione. Le attività eseguono la maggior parte delle operazioni del pacchetto e ne definiscono la funzionalità. Per creare un pacchetto sono sufficienti tre righe di codice, ma vengono aggiunti vari oggetti <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> e attività per fornire funzionalità aggiuntive. In questa sezione viene descritto come creare un pacchetto a livello di programmazione. Non vengono fornite informazioni sulla creazione di attività o di oggetti <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager>, che verranno descritti nelle sezioni successive.  
  
## <a name="example"></a>Esempio  
 Per scrivere codice utilizzando l'IDE di Visual Studio, è necessario un riferimento a Microsoft.SqlServer.ManagedDTS.dll per creare un'istruzione `using` (`Imports` in Visual Basic .NET) in Microsoft.SqlServer.Dts.Runtime. Nell'esempio di codice seguente è illustrata la creazione di un pacchetto vuoto.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Package package;  
      package = new Package();  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    Dim package As Package  
    package = New Package  
  
  End Sub  
  
End Module  
```  
  
 Per compilare ed eseguire l'esempio, premere F5 in Visual Studio. Per compilare il codice usando il compilatore C#, **csc.exe**, dal prompt dei comandi per la compilazione usare il comando e i riferimenti di file seguenti, sostituendo *\<filename>* con il nome del file con estensione cs o vb e assegnando un *\<outputfilename>* a scelta.  
  
 **csc /target:library /out: \<outputfilename>.dll \<filename>.cs /r:Microsoft.SqlServer.Managed DTS.dll" /r:System.dll**  
  
 Per compilare il codice usando il compilatore Visual Basic .NET, **vbc.exe**, dal prompt dei comandi per la compilazione usare il comando e i riferimenti di file seguenti.  
  
 **vbc /target:library /out: \<outputfilename>.dll \<filename>.vb /r:Microsoft.SqlServer.Managed DTS.dll" /r:System.dll**  
  
 È anche possibile creare un pacchetto caricando un pacchetto esistente salvato su disco, nel file system o in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La differenza è che viene dapprima creato l'oggetto <xref:Microsoft.SqlServer.Dts.Runtime.Application> e quindi nell'oggetto del pacchetto viene inserito uno dei metodi di overload dell'applicazione: **LoadPackage** per i file flat, **LoadFromSQLServer** per i pacchetti salvati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o <xref:Microsoft.SqlServer.Dts.Runtime.Application.LoadFromDtsServer%2A> per i pacchetti salvati nel file system. Nell'esempio seguente viene caricato un pacchetto esistente da disco, quindi vengono visualizzate diverse proprietà del pacchetto.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class ApplicationTests  
  {  
    static void Main(string[] args)  
    {  
      // The variable pkg points to the location of the  
      // ExecuteProcess package sample that was installed with  
      // the SSIS samples.  
      string pkg = @"C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services" +  
        @"\Package Samples\ExecuteProcess Sample\ExecuteProcess\UsingExecuteProcess.dtsx";  
  
      Application app = new Application();  
      Package p = app.LoadPackage(pkg, null);  
  
      // Now that the package is loaded, we can query on  
      // its properties.  
      int n = p.Configurations.Count;  
      DtsProperty p2 = p.Properties["VersionGUID"];  
      DTSProtectionLevel pl = p.ProtectionLevel;  
  
      Console.WriteLine("Number of configurations = " + n.ToString());  
      Console.WriteLine("VersionGUID = " + (string)p2.GetValue(p));  
      Console.WriteLine("ProtectionLevel = " + pl.ToString());  
      Console.Read();  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module ApplicationTests  
  
  Sub Main()  
  
    ' The variable pkg points to the location of the  
    ' ExecuteProcess package sample that was installed with  
    ' the SSIS samples.  
    Dim pkg As String = _  
      "C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services" & _  
      "\Package Samples\ExecuteProcess Sample\ExecuteProcess\UsingExecuteProcess.dtsx"  
  
    Dim app As Application = New Application()  
    Dim p As Package = app.LoadPackage(pkg, Nothing)  
  
    ' Now that the package is loaded, we can query on  
    ' its properties.  
    Dim n As Integer = p.Configurations.Count  
    Dim p2 As DtsProperty = p.Properties("VersionGUID")  
    Dim pl As DTSProtectionLevel = p.ProtectionLevel  
  
    Console.WriteLine("Number of configurations = " & n.ToString())  
    Console.WriteLine("VersionGUID = " & CType(p2.GetValue(p), String))  
    Console.WriteLine("ProtectionLevel = " & pl.ToString())  
    Console.Read()  
  
  End Sub  
  
End Module  
```  
  
 **Esempio di output**  
  
 `Number of configurations = 2`  
  
 `VersionGUID = {09016682-89B8-4406-AAC9-AF1E527FF50F}`  
  
 `ProtectionLevel = DontSaveSensitive`  
  
## <a name="external-resources"></a>Risorse esterne  
  
-   Intervento nel blog su un [esempio di API relativo all'origine e alla destinazione OleDB](https://techcommunity.microsoft.com/t5/sql-server-integration-services/api-sample-oledb-source-and-oledb-destination/ba-p/387553) sul sito Web blogs.msdn.com.  
  
-   Intervento nel blog sull'[aggiornamento di EzAPI per SQL Server 2012](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ezapi-updated-for-sql-server-2012/ba-p/388042) sul sito Web blogs.msdn.com.  
  
## <a name="see-also"></a>Vedere anche  
 [Aggiunta di attività a livello di programmazione](../../integration-services/building-packages-programmatically/adding-tasks-programmatically.md)  
  
  
