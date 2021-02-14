---
description: Scripting
title: Scripting | Microsoft Docs
ms.custom: ''
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- dependencies [SMO]
- scripts [SMO]
ms.assetid: 13a35511-3987-426b-a3b7-3b2e83900dc7
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3aff8bbdc42ca0ecd5ae7e8139ec4ec78f7b3fd8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352703"
---
# <a name="scripting"></a>Scripting
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Gli script in SMO sono controllati dall' <xref:Microsoft.SqlServer.Management.Smo.Scripter> oggetto e dai relativi oggetti figlio oppure dal metodo **script** per i singoli oggetti. L' <xref:Microsoft.SqlServer.Management.Smo.Scripter> oggetto controlla il mapping tra le relazioni di dipendenza per gli oggetti in un'istanza di [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
 La generazione di script avanzata tramite l'oggetto <xref:Microsoft.SqlServer.Management.Smo.Scripter> e i relativi oggetti figlio è un processo a tre fasi:  
  
1.  Individuazione  
  
2.  Generazione dell'elenco  
  
3.  Generazione dello script  

 Nella fase di individuazione viene utilizzato l'oggetto <xref:Microsoft.SqlServer.Management.Smo.DependencyWalker>. A partire da un elenco URN di oggetti, il metodo <xref:Microsoft.SqlServer.Management.Smo.DependencyWalker.DiscoverDependencies%2A> dell'oggetto <xref:Microsoft.SqlServer.Management.Smo.DependencyWalker> restituisce un oggetto <xref:Microsoft.SqlServer.Management.Smo.DependencyTree> per gli oggetti presenti nell'elenco URN. Il parametro Boolean *fParents* viene utilizzato per indicare se gli elementi padre o gli elementi figlio dell'oggetto specificato devono essere individuati. In questa fase è possibile modificare l'albero delle dipendenze.  
  
 Nella fase di generazione dell'elenco viene passato l'albero e viene restituito l'elenco risultante. Questo elenco di oggetti è ordinato secondo la generazione di script e può essere modificato.  
  
 Nelle fasi di generazione dell'elenco viene utilizzato il metodo <xref:Microsoft.SqlServer.Management.Smo.DependencyWalker.WalkDependencies%2A> per restituire un oggetto <xref:Microsoft.SqlServer.Management.Smo.DependencyTree>. In questa fase è possibile modificare <xref:Microsoft.SqlServer.Management.Smo.DependencyTree>.  
  
 Nella terza e ultima fase viene generato uno script con l'elenco e le opzioni di generazione script specificate. Il risultato viene restituito come un oggetto di sistema <xref:System.Collections.Specialized.StringCollection>. In questa fase i nomi degli oggetti dipendenti vengono estratti dalla raccolta Elementi dell'oggetto <xref:Microsoft.SqlServer.Management.Smo.DependencyTree> e da proprietà come <xref:Microsoft.SqlServer.Management.Smo.DependencyTree.NumberOfSiblings%2A> e <xref:Microsoft.SqlServer.Management.Smo.DependencyTree.FirstChild%2A>.  
  
## <a name="example"></a>Esempio  
 Per usare qualsiasi esempio di codice fornito, è necessario scegliere l'ambiente di programmazione, il modello di programmazione e il linguaggio di programmazione per la creazione dell'applicazione. Per altre informazioni, vedere [creare un progetto Visual C&#35; SMO in Visual Studio .NET](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md).  
  
 Questo esempio di codice richiede un'istruzione **Imports** per lo spazio dei nomi System. Collections. Specialized. Inserire questa istruzione con le altre istruzioni Imports prima di qualsiasi dichiarazione nell'applicazione.  
  
```  
Imports Microsoft.SqlServer.Management.Smo  
Imports Microsoft.SqlServer.Management.Common  
Imports System.Collections.Specialized  
```  
  
## <a name="scripting-out-the-dependencies-for-a-database-in-visual-basic"></a>Generazione di script delle dipendenze per un database in Visual Basic  
 In questo esempio di codice viene illustrato come individuare le dipendenze e scorrere l'elenco per visualizzare i risultati.  
  
```  
' compile with:   
' /r:Microsoft.SqlServer.Smo.dll   
' /r:Microsoft.SqlServer.ConnectionInfo.dll   
' /r:Microsoft.SqlServer.Management.Sdk.Sfc.dll   
  
Imports Microsoft.SqlServer.Management.Smo  
Imports Microsoft.SqlServer.Management.Sdk.Sfc  
  
Public Class A  
   Public Shared Sub Main()  
      ' database name  
      Dim dbName As [String] = "AdventureWorksLT2012"   ' database name  
  
      ' Connect to the local, default instance of SQL Server.   
      Dim srv As New Server()  
  
      ' Reference the database.    
      Dim db As Database = srv.Databases(dbName)  
  
      ' Define a Scripter object and set the required scripting options.   
      Dim scrp As New Scripter(srv)  
      scrp.Options.ScriptDrops = False  
      scrp.Options.WithDependencies = True  
      scrp.Options.Indexes = True   ' To include indexes  
      scrp.Options.DriAllConstraints = True   ' to include referential constraints in the script  
  
      ' Iterate through the tables in database and script each one. Display the script.  
      For Each tb As Table In db.Tables  
         ' check if the table is not a system table  
         If tb.IsSystemObject = False Then  
            Console.WriteLine("-- Scripting for table " + tb.Name)  
  
            ' Generating script for table tb  
            Dim sc As System.Collections.Specialized.StringCollection = scrp.Script(New Urn() {tb.Urn})  
            For Each st As String In sc  
               Console.WriteLine(st)  
            Next  
            Console.WriteLine("--")  
         End If  
      Next  
   End Sub  
End Class  
```  
  
## <a name="scripting-out-the-dependencies-for-a-database-in-visual-c"></a>Generazione di script delle dipendenze per un database in Visual C#  
 In questo esempio di codice viene illustrato come individuare le dipendenze e scorrere l'elenco per visualizzare i risultati.  
  
```  
// compile with:   
// /r:Microsoft.SqlServer.Smo.dll   
// /r:Microsoft.SqlServer.ConnectionInfo.dll   
// /r:Microsoft.SqlServer.Management.Sdk.Sfc.dll   
  
using System;  
using Microsoft.SqlServer.Management.Smo;  
using Microsoft.SqlServer.Management.Sdk.Sfc;  
  
public class A {  
   public static void Main() {   
      String dbName = "AdventureWorksLT2012"; // database name  
  
      // Connect to the local, default instance of SQL Server.   
      Server srv = new Server();  
  
      // Reference the database.    
      Database db = srv.Databases[dbName];  
  
      // Define a Scripter object and set the required scripting options.   
      Scripter scrp = new Scripter(srv);  
      scrp.Options.ScriptDrops = false;  
      scrp.Options.WithDependencies = true;  
      scrp.Options.Indexes = true;   // To include indexes  
      scrp.Options.DriAllConstraints = true;   // to include referential constraints in the script  
  
      // Iterate through the tables in database and script each one. Display the script.     
      foreach (Table tb in db.Tables) {   
         // check if the table is not a system table  
         if (tb.IsSystemObject == false) {  
            Console.WriteLine("-- Scripting for table " + tb.Name);  
  
            // Generating script for table tb  
            System.Collections.Specialized.StringCollection sc = scrp.Script(new Urn[]{tb.Urn});  
            foreach (string st in sc) {  
               Console.WriteLine(st);  
            }  
            Console.WriteLine("--");  
         }  
      }   
   }  
}  
```  
  
## <a name="scripting-out-the-dependencies-for-a-database-in-powershell"></a>Generazione di script delle dipendenze per un database in PowerShell  
 In questo esempio di codice viene illustrato come individuare le dipendenze e scorrere l'elenco per visualizzare i risultati.  
  
```  
# Set the path context to the local, default instance of SQL Server.  
CD \sql\localhost\default  
  
# Create a Scripter object and set the required scripting options.  
$scrp = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Scripter -ArgumentList (Get-Item .)  
$scrp.Options.ScriptDrops = $false  
$scrp.Options.WithDependencies = $true  
$scrp.Options.IncludeIfNotExists = $true  
  
# Set the path context to the tables in AdventureWorks2012.  
  
CD Databases\AdventureWorks2012\Tables  
  
foreach ($Item in Get-ChildItem)  
 {    
 $scrp.Script($Item)  
 }  
```  
  
  
