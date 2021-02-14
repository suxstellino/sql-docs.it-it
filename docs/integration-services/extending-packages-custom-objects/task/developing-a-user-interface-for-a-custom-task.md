---
description: Sviluppo di un'interfaccia utente per un'attività personalizzata
title: Sviluppo di un'interfaccia utente per un'attività personalizzata | Microsoft Docs
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
- custom user interfaces [Integration Services]
- IDtsTaskUI interface
- DtsTaskAttribute attribute
- custom tasks [Integration Services], user interface
- custom user interface [Integration Services], custom tasks
- user interface [Integration Services]
- SSIS custom tasks, user interface
ms.assetid: 1e940cd1-c5f8-4527-b678-e89ba5dc398a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f0536913b55a3e798b7306f5bd2fa36d115e5f84
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337699"
---
# <a name="developing-a-user-interface-for-a-custom-task"></a>Sviluppo di un'interfaccia utente per un'attività personalizzata

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Il modello a oggetti di [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] offre agli sviluppatori di attività la possibilità di creare facilmente un'interfaccia utente personalizzata per un'attività da integrare e visualizzare in [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)]. L'interfaccia utente può fornire informazioni utili all'utente in Progettazione [!INCLUDE[ssIS](../../../includes/ssis-md.md)] oltre a indicazioni su come configurare correttamente le proprietà e le impostazioni dell'attività personalizzata.  
  
 Per sviluppare un'interfaccia utente personalizzata per un'attività, è necessario utilizzare due classi importanti, descritte nella tabella seguente.  
  
|Classe|Descrizione|  
|-----------|-----------------|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute>|Attributo che identifica un'attività gestita e fornisce informazioni della fase di progettazione tramite le proprietà per controllare le modalità di visualizzazione e di interazione di Progettazione [!INCLUDE[ssIS](../../../includes/ssis-md.md)] con l'oggetto.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI>|Interfaccia utilizzata dall'attività per associare l'attività alla relativa interfaccia utente personalizzata.|  
  
 In questa sezione viene descritto il ruolo dell'attributo <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute> e dell'interfaccia <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI> durante lo sviluppo di un'interfaccia utente per un'attività personalizzata e vengono fornite informazioni su come creare, integrare, distribuire e sottoporre a debug l'attività all'interno di Progettazione [!INCLUDE[ssIS](../../../includes/ssis-md.md)].  
  
 Progettazione [!INCLUDE[ssIS](../../../includes/ssis-md.md)] fornisce più punti di ingresso all'interfaccia utente dell'attività. L'utente può scegliere **Modifica** dal menu di scelta rapida, fare doppio clic sull'attività o fare clic sul collegamento **Visualizza editor** nella parte inferiore della finestra delle proprietà. Quando l'utente accede a uno di questi punti di ingresso, Progettazione [!INCLUDE[ssIS](../../../includes/ssis-md.md)] trova e carica l'assembly che contiene l'interfaccia utente per l'attività. L'interfaccia utente per l'attività è responsabile della creazione della finestra di dialogo delle proprietà che l'utente visualizza in [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)].  
  
 L'attività e la relativa interfaccia utente sono entità separate che devono essere implementate in assembly distinti per ridurre gli interventi di localizzazione, distribuzione e manutenzione. La DLL dell'attività non carica, chiama né in genere contiene informazioni sulla relativa interfaccia utente, ad eccezione delle informazioni contenute nei valori dell'attributo <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute> codificati nell'attività. Questo è l'unico modo in cui un'attività è associata alla propria interfaccia utente.  
  
## <a name="the-dtstask-attribute"></a>Attributo DtsTask  
 L'attributo <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute> è incluso nel codice della classe dell'attività per associare un'attività alla relativa interfaccia utente. Progettazione [!INCLUDE[ssIS](../../../includes/ssis-md.md)] utilizza le proprietà dell'attributo per determinare la modalità di visualizzazione dell'attività nella finestra di progettazione. Queste proprietà includono il nome da visualizzare e l'icona, se presente.  
  
 Nella tabella seguente sono descritte le proprietà dell'attributo <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute>.  
  
|Proprietà|Descrizione|  
|--------------|-----------------|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Localization.DtsLocalizableAttribute.DisplayName%2A>|Visualizza il nome dell'attività nella casella degli strumenti del Flusso di controllo.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Localization.DtsLocalizableAttribute.Description%2A>|Descrizione dell'attività (ereditata da <xref:Microsoft.SqlServer.Dts.Runtime.Localization.DtsLocalizableAttribute>). Questa proprietà è visualizzata nelle descrizioni comandi.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.IconResource%2A>|Icona visualizzata in Progettazione [!INCLUDE[ssIS](../../../includes/ssis-md.md)].|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.RequiredProductLevel%2A>|Se utilizzata, impostarla su uno dei valori dell'enumerazione <xref:Microsoft.SqlServer.Dts.Runtime.DTSProductLevel>. Ad esempio: `RequiredProductLevel = DTSProductLevel.None`.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.TaskContact%2A>|Contiene informazioni di contatto per i casi in cui l'attività richiede supporto tecnico.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.TaskType%2A>|Assegna un tipo all'attività.|  
|Attribute.TypeId|Se implementata in una classe derivata, ottiene un identificatore univoco per questo attributo. Per altre informazioni, vedere la proprietà **Attribute.TypeID** nella libreria di classi .NET Framework.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.UITypeName%2A>|Nome di tipo dell'assembly utilizzato da Progettazione [!INCLUDE[ssIS](../../../includes/ssis-md.md)] per caricare l'assembly. Questa proprietà viene utilizzata per trovare l'assembly dell'interfaccia utente per l'attività.|  
  
 Nell'esempio di codice seguente è illustrato l'aspetto di <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute> codificato sopra la definizione della classe.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
namespace Microsoft.SSIS.Samples  
{  
  [DtsTask  
  (  
   DisplayName = "MyTask",  
   IconResource = "MyTask.MyTaskIcon.ico",  
   UITypeName = "My Custom Task," +  
   "Version=1.0.0.0," +  
   "Culture = Neutral," +  
   "PublicKeyToken = 12345abc6789de01",  
   TaskType = "PackageMaintenance",  
   TaskContact = "MyTask; company name; any other information",  
   RequiredProductLevel = DTSProductLevel.None  
   )]  
  public class MyTask : Task  
  {  
    // Your code here.  
  }  
}  
```  
  
```vb  
Imports System  
Imports Microsoft.SqlServer.Dts.Runtime  
  
<DtsTask(DisplayName:="MyTask", _  
 IconResource:="MyTask.MyTaskIcon.ico", _  
 UITypeName:="My Custom Task," & _  
 "Version=1.0.0.0,Culture=Neutral," & _  
 "PublicKeyToken=12345abc6789de01", _  
 TaskType:="PackageMaintenance", _  
 TaskContact:="MyTask; company name; any other information", _  
 RequiredProductLevel:=DTSProductLevel.None)> _  
Public Class MyTask  
  Inherits Task  
  
  ' Your code here.  
  
End Class 'MyTask  
```  
  
 Progettazione [!INCLUDE[ssIS](../../../includes/ssis-md.md)] utilizza la proprietà <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.UITypeName%2A> dell'attributo che include il nome dell'assembly, il nome del tipo, la versione, la lingua e il token di chiave pubblica per individuare l'assembly nella Global Assembly Cache (GAC) e caricarlo per l'utilizzo nella finestra di progettazione.  
  
 Dopo l'individuazione dell'assembly, Progettazione [!INCLUDE[ssIS](../../../includes/ssis-md.md)] utilizza le altre proprietà dell'attributo per visualizzare informazioni aggiuntive sull'attività in Progettazione [!INCLUDE[ssIS](../../../includes/ssis-md.md)], ad esempio il nome, l'icona e la descrizione dell'attività.  
  
 Le proprietà <xref:Microsoft.SqlServer.Dts.Runtime.Localization.DtsLocalizableAttribute.DisplayName%2A>, <xref:Microsoft.SqlServer.Dts.Runtime.Localization.DtsLocalizableAttribute.Description%2A> e <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.IconResource%2A> specificano la modalità con cui l'attività viene presentata all'utente. La proprietà <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.IconResource%2A> contiene l'ID di risorsa dell'icona incorporata nell'assembly dell'interfaccia utente. La finestra di progettazione carica la risorsa dell'icona in base all'ID dall'assembly e la visualizza accanto al nome dell'attività nella casella degli strumenti e nell'area di progettazione quando l'attività viene aggiunta al pacchetto. Se un'attività non prevede una risorsa di icona, la finestra di progettazione utilizza un'icona predefinita per l'attività.  
  
## <a name="the-idtstaskui-interface"></a>Interfaccia IDTSTaskUI  
 L'interfaccia <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI> definisce la raccolta di metodi e proprietà chiamati da Progettazione [!INCLUDE[ssIS](../../../includes/ssis-md.md)] per inizializzare e visualizzare l'interfaccia utente associata all'attività. Quando viene richiamata l'interfaccia utente per un'attività, la finestra di progettazione chiama il metodo <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI.Initialize%2A>, implementato dall'interfaccia utente dell'attività quando è stata scritta, quindi fornisce le raccolte <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> e <xref:Microsoft.SqlServer.Dts.Runtime.Connections> rispettivamente dell'attività e del pacchetto come parametri. Queste raccolte vengono archiviate in locale e utilizzate successivamente nel metodo <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI.GetView%2A>.  
  
 La finestra di progettazione chiama il metodo <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI.GetView%2A> per richiedere la finestra visualizzata in Progettazione [!INCLUDE[ssIS](../../../includes/ssis-md.md)]. L'attività crea un'istanza della finestra che contiene l'interfaccia utente per l'attività e restituisce l'interfaccia utente alla finestra di progettazione per la visualizzazione. In genere, gli oggetti <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> e <xref:Microsoft.SqlServer.Dts.Runtime.Connections> vengono forniti alla finestra tramite un costruttore di overload, in modo che possano essere utilizzati per configurare l'attività.  
  
 Progettazione [!INCLUDE[ssIS](../../../includes/ssis-md.md)] chiama il metodo <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI.GetView%2A> dell'interfaccia utente dell'attività per visualizzare l'interfaccia utente per l'attività. L'interfaccia utente dell'attività restituisce il Windows Form da questo metodo e Progettazione [!INCLUDE[ssIS](../../../includes/ssis-md.md)] visualizza questo form come finestra di dialogo modale. Quando il form viene chiuso, Progettazione [!INCLUDE[ssIS](../../../includes/ssis-md.md)] esamina il valore della proprietà **DialogResult** del form per determinare se l'attività è stata modificata e se queste modifiche devono essere salvate. Se il valore della proprietà **DialogResult** è **OK**, Progettazione [!INCLUDE[ssIS](../../../includes/ssis-md.md)] chiama i metodi di persistenza dell'attività per salvare le modifiche; in caso contrario, le modifiche vengono ignorate.  
  
 Nell'esempio di codice seguente viene implementata l'interfaccia <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI> e si presuppone l'esistenza di una classe di Windows Form denominata SampleTaskForm.  
  
```csharp  
using System;  
using System.Windows.Forms;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Runtime.Design;  
  
namespace Sample  
{  
   public class HelloWorldTaskUI : IDtsTaskUI  
   {  
      TaskHost   taskHost;  
      Connections connections;  
      public void Initialize(TaskHost taskHost, IServiceProvider serviceProvider)  
      {  
         this.taskHost = taskHost;  
         IDtsConnectionService cs = serviceProvider.GetService  
         ( typeof( IDtsConnectionService ) ) as   IDtsConnectionService;   
         this.connections = cs.GetConnections();  
      }  
      public ContainerControl GetView()  
      {  
        return new HelloWorldTaskForm(this.taskHost, this.connections);  
      }  
     public void Delete(IWin32Window parentWindow)  
     {  
     }  
     public void New(IWin32Window parentWindow)  
     {  
     }  
   }  
}  
```  
  
```vb  
Imports System  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Runtime.Design  
Imports System.Windows.Forms  
  
Public Class HelloWorldTaskUI  
  Implements IDtsTaskUI  
  
  Dim taskHost As TaskHost  
  Dim connections As Connections  
  
  Public Sub Initialize(ByVal taskHost As TaskHost, ByVal serviceProvider As IServiceProvider) _  
    Implements IDtsTaskUI.Initialize  
  
    Dim cs As IDtsConnectionService  
  
    Me.taskHost = taskHost  
    cs = DirectCast(serviceProvider.GetService(GetType(IDtsConnectionService)), IDtsConnectionService)  
    Me.connections = cs.GetConnections()  
  
  End Sub  
  
  Public Function GetView() As ContainerControl _  
    Implements IDtsTaskUI.GetView  
  
    Return New HelloWorldTaskForm(Me.taskHost, Me.connections)  
  
  End Function  
  
  Public Sub Delete(ByVal parentWindow As IWin32Window) _  
    Implements IDtsTaskUI.Delete  
  
  End Sub  
  
  Public Sub [New](ByVal parentWindow As IWin32Window) _  
    Implements IDtsTaskUI.[New]  
  
  End Sub  
  
End Class  
```  
 
## <a name="see-also"></a>Vedere anche  
 [Creazione di un'attività personalizzata](../../../integration-services/extending-packages-custom-objects/task/creating-a-custom-task.md)   
 [Scrittura del codice di un'attività personalizzata](../../../integration-services/extending-packages-custom-objects/task/coding-a-custom-task.md)   
 [Sviluppo di un'interfaccia utente per un'attività personalizzata](../../../integration-services/extending-packages-custom-objects/task/developing-a-user-interface-for-a-custom-task.md)  
  
  
