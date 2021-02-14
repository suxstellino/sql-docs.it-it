---
description: Sviluppo di un'interfaccia utente per un componente del flusso di dati
title: Sviluppo di un'interfaccia utente per un componente flusso di dati | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- data flow components [Integration Services], custom editors
- user interfaces [Integration Services]
- custom data flow components [Integration Services], custom editors
- custom component editors [Integration Services]
- IDtsComponentUI interface
- UITypeName property
- custom user interface [Integration Services], custom data flow component
- editors [Integration Services]
ms.assetid: 10b829a1-609b-42e3-9070-cfe5a2bb698c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cfe2ebf4e63237f9e5039188c5f9fec883fd797e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343698"
---
# <a name="developing-a-user-interface-for-a-data-flow-component"></a>Sviluppo di un'interfaccia utente per un componente del flusso di dati

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Gli sviluppatori di componenti possono fornire un'interfaccia utente personalizzata per un componente, che viene visualizzata in [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] quando il componente viene modificato. L'implementazione di un'interfaccia utente personalizzata fornisce notifiche quando il componente viene aggiunto o eliminato da un'attività Flusso di dati e quando per il componente è richiesta la Guida.  
  
 Se non si fornisce un'interfaccia utente personalizzata per il componente, gli utenti possono comunque configurare il componente e le relative proprietà personalizzate utilizzando l'editor avanzato. È possibile assicurarsi che l'editor avanzato consenta agli utenti di modificare in modo appropriato i valori delle proprietà personalizzate tramite le proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100.TypeConverter%2A> e <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100.UITypeEditor%2A> di <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100>, a seconda dei casi. Per altre informazioni, vedere la sezione relativa alla creazione di proprietà personalizzate in [Metodi della fase di progettazione di un componente flusso di dati](../../../integration-services/extending-packages-custom-objects/data-flow/design-time-methods-of-a-data-flow-component.md).  
  
## <a name="setting-the-uitypename-property"></a>Impostazione della proprietà UITypeName  
 Per fornire un'interfaccia utente personalizzata, lo sviluppatore deve impostare la proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.DtsPipelineComponentAttribute.UITypeName%2A> di <xref:Microsoft.SqlServer.Dts.Pipeline.DtsPipelineComponentAttribute> sul nome di una classe che implementa l'interfaccia <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI>. Quando questa proprietà viene impostata dal componente, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] carica e chiama l'interfaccia utente personalizzata quando il componente viene modificato in Progettazione [!INCLUDE[ssIS](../../../includes/ssis-md.md)].  
  
 La proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.DtsPipelineComponentAttribute.UITypeName%2A> è una stringa delimitata da virgole che identifica il nome completo del tipo. Nell'elenco seguente sono illustrati, nell'ordine, gli elementi che identificano il tipo:  
  
-   Nome tipo  
  
-   Nome assembly  
  
-   Versione file  
  
-   Impostazioni cultura  
  
-   Token di chiave pubblica  
  
 Nell'esempio di codice seguente è illustrata una classe che deriva dalla classe di base <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent> e specifica la proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.DtsPipelineComponentAttribute.UITypeName%2A>.  
  
```csharp  
[DtsPipelineComponent(  
DisplayName="SampleComponent",  
UITypeName="MyNamespace.MyComponentUIClassName,MyAssemblyName,Version=1.0.0.0,Culture=neutral,PublicKeyToken=abcd...",  
ComponentType = ComponentType.Transform)]  
public class SampleComponent : PipelineComponent  
{  
//TODO: Implement the component here.  
}  
```  
  
```vb  
<DtsPipelineComponent(DisplayName="SampleComponent", _  
UITypeName="MyNamespace.MyComponentUIClassName,MyAssemblyName,Version=1.0.0.0,Culture=neutral,PublicKeyToken=abcd...", ComponentType=ComponentType.Transform)> _   
Public Class SampleComponent   
 Inherits PipelineComponent   
End Class  
```  
  
## <a name="implementing-the-idtscomponentui-interface"></a>Implementazione dell'interfaccia IDtsComponentUI   
 L'interfaccia <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI> contiene i metodi chiamati da Progettazione [!INCLUDE[ssIS](../../../includes/ssis-md.md)] quando un componente viene aggiunto, eliminato e modificato. Gli sviluppatori di componenti possono fornire codice nella loro implementazione di questi metodi per interagire con gli utenti del componente.  
  
 Questa classe viene in genere implementata in un assembly distinto dal componente stesso. Anche se non è obbligatorio utilizzare un assembly distinto, in questo modo lo sviluppatore può compilare e distribuire il componente e l'interfaccia utente indipendentemente l'uno dall'altra e mantenere piccola l'impronta binaria del componente.  
  
 L'implementazione di un'interfaccia utente personalizzata fornisce allo sviluppatore di componenti un maggior controllo sul componente quando viene modificato in Progettazione [!INCLUDE[ssIS](../../../includes/ssis-md.md)]. Ad esempio, un componente può aggiungere codice al metodo <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.New%2A>, che viene chiamato quando il componente viene inizialmente aggiunto a un'attività Flusso di dati, e visualizzare una procedura guidata che consente all'utente di completare la configurazione iniziale del componente.  
  
 Dopo aver creato una classe che implementa l'interfaccia <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI>, è necessario aggiungere codice per rispondere all'interazione dell'utente con il componente. Il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Initialize%2A> fornisce l'interfaccia <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> del componente e viene chiamato prima dei metodi <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.New%2A> e <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Edit%2A>. Questo riferimento deve essere archiviato in una variabile membro privata e utilizzato in seguito per modificare i metadati del componente.  
  
## <a name="modifying-a-component-and-persisting-changes"></a>Come effettuare modifiche in un componente e renderle persistenti  
 L'interfaccia <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> viene fornita come parametro al metodo <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Initialize%2A>. Questo riferimento deve essere memorizzato nella cache in una variabile membro dal codice dell'interfaccia utente e quindi utilizzato per modificare il componente in risposta all'interazione dell'utente con l'interfaccia utente.  
  
 Anche se è possibile modificare direttamente il componente tramite l'interfaccia <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100>, è preferibile creare un'istanza di <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapper> tramite il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.Instantiate%2A>. Quando si modifica direttamente il componente tramite l'interfaccia, le misure di sicurezza di convalida del componente vengono ignorate. Il vantaggio dell'utilizzo dell'istanza della fase di progettazione del componente tramite <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapper> è che ci si assicura che il componente controlli le modifiche apportate.  
  
 Il valore restituito del metodo <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Edit%2A> determina se le modifiche apportate a un componente vengono rese persistenti o annullate. Quando questo metodo restituisce **false**, tutte le modifiche vengono annullate; se restituisce **true**, le modifiche al componente vengono rese persistenti e il pacchetto viene contrassegnato in modo da richiederne il salvataggio.  
  
### <a name="using-the-services-of-the-ssis-designer"></a>Utilizzo dei servizi di Progettazione SSIS  
 Il parametro **IServiceProvider** del metodo <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Initialize%2A> fornisce l'accesso ai servizi seguenti di Progettazione [!INCLUDE[ssIS](../../../includes/ssis-md.md)]:  
  
|Servizio|Descrizione|  
|-------------|-----------------|  
|<xref:Microsoft.SqlServer.Dts.Design.IDtsClipboardService>|Viene utilizzato per determinare se il componente è stato generato come parte di un'operazione Copia/Incolla o Taglia/Incolla.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsConnectionService>|Viene utilizzato per accedere alle connessioni esistenti o per creare nuove connessioni nel pacchetto.|  
|<xref:Microsoft.SqlServer.Dts.Design.IErrorCollectionService>|Viene utilizzato per acquisire eventi dai componenti del flusso di dati quando è necessario acquisire tutti gli errori e gli avvisi generati dal componente anziché ricevere solo quelli più recenti.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsVariableService>|Viene utilizzato per accedere alle variabili esistenti o per creare nuove variabili nel pacchetto.|  
|<xref:Microsoft.SqlServer.Dts.Design.IDtsPipelineEnvironmentService>|Viene utilizzato dai componenti del flusso di dati per accedere all'attività Flusso di dati padre e ad altri componenti del flusso di dati. Questa caratteristica può essere utilizzata per sviluppare un componente come la Creazione guidata dimensioni a modifica lenta, che crea e connette componenti del flusso di dati aggiuntivi se necessario.|  
  
 Questi servizi offrono agli sviluppatori di componenti la possibilità di accedere e creare oggetti nel pacchetto in cui viene caricato il componente.  
  
## <a name="sample"></a>Esempio  
 Nell'esempio di codice seguente è illustrata l'integrazione di una classe di interfaccia utente personalizzata che implementa l'interfaccia <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI> e un Windows Form che funge da editor per un componente.  
  
### <a name="custom-user-interface-class"></a>Classe dell'interfaccia utente personalizzata  
 Nell'esempio di codice seguente è illustrata la classe che implementa l'interfaccia <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI>. Il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Edit%2A> crea l'editor del componente, quindi visualizza il form. Il valore restituito del form determina se le modifiche apportate al componente sono persistenti.  
  
```csharp  
using System;  
using System.Windows.Forms;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Pipeline.Design;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
  
namespace Microsoft.Samples.SqlServer.Dts  
{  
    public class SampleComponentUI : IDtsComponentUI  
    {  
        IDTSComponentMetaData100 md;  
        IServiceProvider sp;  
  
        public void Help(System.Windows.Forms.IWin32Window parentWindow)  
        {  
        }  
        public void New(System.Windows.Forms.IWin32Window parentWindow)  
        {  
        }  
        public void Delete(System.Windows.Forms.IWin32Window parentWindow)  
        {  
        }  
        public bool Edit(System.Windows.Forms.IWin32Window parentWindow, Variables vars, Connections cons)  
        {  
            // Create and display the form for the user interface.  
            SampleComponentUIForm componentEditor = new SampleComponentUIForm(cons, vars, md);  
  
            DialogResult result  = componentEditor.ShowDialog(parentWindow);  
  
            if (result == DialogResult.OK)  
                return true;  
  
            return false;  
        }  
        public void Initialize(IDTSComponentMetaData100 dtsComponentMetadata, IServiceProvider serviceProvider)  
        {  
            // Store the component metadata.  
            this.md = dtsComponentMetadata;  
        }  
    }  
}  
```  
  
```vb  
Imports System   
Imports System.Windows.Forms   
Imports Microsoft.SqlServer.Dts.Runtime   
Imports Microsoft.SqlServer.Dts.Pipeline.Design   
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper   
  
Namespace Microsoft.Samples.SqlServer.Dts   
  
 Public Class SampleComponentUI   
 Implements IDtsComponentUI   
   Private md As IDTSComponentMetaData100   
   Private sp As IServiceProvider   
  
   Public Sub Help(ByVal parentWindow As System.Windows.Forms.IWin32Window)   
   End Sub   
  
   Public Sub New(ByVal parentWindow As System.Windows.Forms.IWin32Window)   
   End Sub   
  
   Public Sub Delete(ByVal parentWindow As System.Windows.Forms.IWin32Window)   
   End Sub   
  
   Public Function Edit(ByVal parentWindow As System.Windows.Forms.IWin32Window, ByVal vars As Variables, ByVal cons As Connections) As Boolean   
     ' Create and display the form for the user interface.  
     Dim componentEditor As SampleComponentUIForm = New SampleComponentUIForm(cons, vars, md)   
     Dim result As DialogResult = componentEditor.ShowDialog(parentWindow)   
     If result = DialogResult.OK Then   
       Return True   
     End If   
     Return False   
   End Function   
  
   Public Sub Initialize(ByVal dtsComponentMetadata As IDTSComponentMetaData100, ByVal serviceProvider As IServiceProvider)   
     Me.md = dtsComponentMetadata   
   End Sub   
 End Class   
  
End Namespace  
```  
  
### <a name="custom-editor"></a>Editor personalizzato  
 Nel codice seguente è illustrata l'implementazione del Windows Form visualizzato durante la chiamata al metodo <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Edit%2A>.  
  
```csharp  
using System;  
using System.Drawing;  
using System.Collections;  
using System.ComponentModel;  
using System.Windows.Forms;  
using System.Data;  
  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.Samples.SqlServer.Dts  
{  
    public partial class SampleComponentUIForm : System.Windows.Forms.Form  
    {  
        private Connections connections;  
        private Variables variables;  
        private IDTSComponentMetaData100 metaData;  
        private CManagedComponentWrapper designTimeInstance;  
        private System.ComponentModel.IContainer components = null;  
  
        public SampleComponentUIForm( Connections cons, Variables vars, IDTSComponentMetaData100 md)  
        {  
            variables = vars;  
            connections = cons;  
            metaData = md;  
        }  
  
        private void btnOk_Click(object sender, System.EventArgs e)  
        {  
            if (designTimeInstance == null)  
                designTimeInstance = metaData.Instantiate();  
  
            designTimeInstance.SetComponentProperty( "CustomProperty", txtCustomPropertyValue.Text);  
  
            this.Close();  
        }  
  
        private void btnCancel_Click(object sender, System.EventArgs e)  
        {  
            this.Close();  
        }  
    }  
}  
```  
  
```vb  
Imports System   
Imports System.Drawing   
Imports System.Collections   
Imports System.ComponentModel   
Imports System.Windows.Forms   
Imports System.Data   
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper   
Imports Microsoft.SqlServer.Dts.Runtime   
  
Namespace Microsoft.Samples.SqlServer.Dts   
  
 Public Partial Class SampleComponentUIForm   
  Inherits System.Windows.Forms.Form   
   Private connections As Connections   
   Private variables As Variables   
   Private metaData As IDTSComponentMetaData100   
   Private designTimeInstance As CManagedComponentWrapper   
   Private components As System.ComponentModel.IContainer = Nothing   
  
   Public Sub New(ByVal cons As Connections, ByVal vars As Variables, ByVal md As IDTSComponentMetaData100)   
     variables = vars   
     connections = cons   
     metaData = md   
   End Sub   
  
   Private Sub btnOk_Click(ByVal sender As Object, ByVal e As System.EventArgs)   
     If designTimeInstance Is Nothing Then   
       designTimeInstance = metaData.Instantiate   
     End If   
     designTimeInstance.SetComponentProperty("CustomProperty", txtCustomPropertyValue.Text)   
     Me.Close   
   End Sub   
  
   Private Sub btnCancel_Click(ByVal sender As Object, ByVal e As System.EventArgs)   
     Me.Close   
   End Sub   
 End Class   
  
End Namespace  
```
  
## <a name="see-also"></a>Vedere anche  
 [Creazione di un componente flusso di dati personalizzato](../../../integration-services/extending-packages-custom-objects/data-flow/creating-a-custom-data-flow-component.md)  
  
  
