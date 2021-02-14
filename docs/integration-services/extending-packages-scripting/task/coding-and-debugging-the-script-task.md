---
description: Scrittura di codice e debug dell'attività Script
title: Codifica e debug dell'attività Script | Microsoft Docs
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
- Script task [Integration Services], debugging
- SSIS Script task, development environment
- SSIS Script task, debugging
- Script task [Integration Services], development environment
- Script task [Integration Services], coding
- debugging [Integration Services], scripts
- VSTA
- SSIS Script task, coding
ms.assetid: 687c262f-fcab-42e8-92ae-e956f3d92d69
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6f5cc9132e7a8f67d2c4636eeaeaa0cb7db5ca0c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351586"
---
# <a name="coding-and-debugging-the-script-task"></a>Scrittura di codice e debug dell'attività Script

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Dopo avere configurato l'attività Script in **Editor attività Script**, scrivere il codice personalizzato nell'ambiente di sviluppo corrispondente.  
  
## <a name="script-task-development-environment"></a>Ambiente di sviluppo dell'attività Script  
 Per l'attività Script viene usato [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] Tools for Applications (VSTA) come ambiente di sviluppo per lo script stesso.  
  
 Il codice di script viene scritto in [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic o [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual C#. Specificare il linguaggio di script impostando la proprietà **ScriptLanguage** in **Editor attività Script**. Se si preferisce utilizzare un altro linguaggio di programmazione, è possibile sviluppare un assembly personalizzato nel linguaggio desiderato e chiamarne la funzionalità dal codice nell'attività Script.  
  
 Lo script creato nell'attività Script viene archiviato nella definizione del pacchetto. Non viene creato un file script separato. Pertanto, l'utilizzo dell'attività Script non ha effetto sulla distribuzione del pacchetto.  
  
> [!NOTE]  
>  Quando si progetta il pacchetto e si esegue il debug dello script, il codice di script viene scritto temporaneamente in un file di progetto. Poiché l'archiviazione di informazioni riservate in un file costituisce un potenziale rischio per la sicurezza, è consigliabile non inserire nel codice di script informazioni di questo tipo, ad esempio le password.  
  
 Per impostazione predefinita, nell'ambiente IDE **Option Strict** è disabilitato.  
  
## <a name="script-task-project-structure"></a>Struttura di progetto dell'attività Script  
 Quando si crea o si modifica lo script contenuto in un'attività Script, VSTA apre un nuovo progetto vuoto o riapre il progetto esistente. La creazione di questo progetto VSTA non influisce sulla distribuzione del pacchetto, perché il progetto viene salvato nel file del pacchetto; l'attività Script non crea file aggiuntivi.  
  
### <a name="project-items-and-classes-in-the-script-task-project"></a>Elementi e classi del progetto dell'attività Script  
 Per impostazione predefinita, il progetto dell'attività Script visualizzato nella finestra Esplora progetti in VSTA contiene un unico elemento, **ScriptMain**. L'elemento **ScriptMain** contiene a sua volta un'unica classe, anch'essa denominata **ScriptMain**. Gli elementi di codice nella classe variano a seconda del linguaggio di programmazione selezionato per l'attività Script:  
  
-   Se l'attività Script è configurata per il linguaggio di programmazione [!INCLUDE[vb_orcas_long](../../../includes/vb-orcas-long-md.md)], la classe **ScriptMain** include una subroutine pubblica, **Main**. La subroutine **ScriptMain.Main** è il metodo chiamato dal runtime quando si esegue l'attività Script.  
  
     Per impostazione predefinita, nella subroutine **Main** di un nuovo script è presente solo la riga `Dts.TaskResult = ScriptResults.Success`. Questa riga indica al runtime che l'operazione dell'attività è riuscita. La proprietà **Dts.TaskResult** viene trattata in [Risultati restituiti dall'attività Script](../../../integration-services/extending-packages-scripting/task/returning-results-from-the-script-task.md).  
  
-   Se l'attività Script è configurata per il linguaggio di programmazione Visual C#, la classe **ScriptMain** include un metodo pubblico, **Main**. Il metodo viene chiamato durante l'esecuzione dell'attività Script.  
  
     Per impostazione predefinita, il metodo **Main** include la riga `Dts.TaskResult = (int)ScriptResults.Success`. Questa riga indica al runtime che l'operazione dell'attività è riuscita.  
  
 L'elemento **ScriptMain** può contenere classi diverse dalla classe **ScriptMain**. Le classi sono disponibili solo per l'attività Script in cui risiedono.  
  
 Per impostazione predefinita, l'elemento di progetto **ScriptMain** contiene il codice seguente, generato automaticamente. Nel modello del codice sono inoltre disponibili una panoramica dell'attività Script, nonché informazioni aggiuntive su come recuperare e modificare oggetti SSIS, quali variabili, eventi e connessioni.  
  
```vb  
' Microsoft SQL Server Integration Services Script Task  
' Write scripts using Microsoft Visual Basic 2008.  
' The ScriptMain is the entry point class of the script.  
  
Imports System  
Imports System.Data  
Imports System.Math  
Imports Microsoft.SqlServer.Dts.Runtime.VSTAProxy  
  
<System.AddIn.AddIn("ScriptMain", Version:="1.0", Publisher:="", Description:="")> _  
Partial Class ScriptMain  
  
Private Sub ScriptMain_Startup(ByVal sender As Object, ByVal e As System.EventArgs) Handles Me.Startup  
  
End Sub  
  
Private Sub ScriptMain_Shutdown(ByVal sender As Object, ByVal e As System.EventArgs) Handles Me.Shutdown  
Try  
' Unlock variables from the read-only and read-write variable collection properties  
If (Dts.Variables.Count <> 0) Then  
Dts.Variables.Unlock()  
End If  
Catch ex As Exception  
        End Try  
End Sub  
  
Enum ScriptResults  
Success = DTSExecResult.Success  
Failure = DTSExecResult.Failure  
End Enum  
  
' The execution engine calls this method when the task executes.  
' To access the object model, use the Dts property. Connections, variables, events,  
' and logging features are available as members of the Dts property as shown in the following examples.  
'  
' To reference a variable, call Dts.Variables("MyCaseSensitiveVariableName").Value  
' To post a log entry, call Dts.Log("This is my log text", 999, Nothing)  
' To fire an event, call Dts.Events.FireInformation(99, "test", "hit the help message", "", 0, True)  
'  
' To use the connections collection use something like the following:  
' ConnectionManager cm = Dts.Connections.Add("OLEDB")  
' cm.ConnectionString = "Data Source=localhost;Initial Catalog=AdventureWorks;Provider=SQLNCLI10;Integrated Security=SSPI;Auto Translate=False;"  
'  
' Before returning from this method, set the value of Dts.TaskResult to indicate success or failure.  
'   
' To open Help, press F1.  
  
Public Sub Main()  
'  
' Add your code here  
'  
Dts.TaskResult = ScriptResults.Success  
End Sub  
  
End Class  
```  
  
```csharp  
/*  
   Microsoft SQL Server Integration Services Script Task  
   Write scripts using Microsoft Visual C# 2008.  
   The ScriptMain is the entry point class of the script.  
*/  
  
using System;  
using System.Data;  
using Microsoft.SqlServer.Dts.Runtime.VSTAProxy;  
using System.Windows.Forms;  
  
namespace ST_1bcfdbad36d94f8ba9f23a10375abe53.csproj  
{  
    [System.AddIn.AddIn("ScriptMain", Version = "1.0", Publisher = "", Description = "")]  
    public partial class ScriptMain  
    {  
        private void ScriptMain_Startup(object sender, EventArgs e)  
        {  
  
        }  
  
        private void ScriptMain_Shutdown(object sender, EventArgs e)  
        {  
            try  
            {  
                // Unlock variables from the read-only and read-write variable collection properties  
                if (Dts.Variables.Count != 0)  
                {  
                    Dts.Variables.Unlock();  
                }  
            }  
            catch  
            {  
            }  
        }  
  
        #region VSTA generated code  
        private void InternalStartup()  
        {  
            this.Startup += new System.EventHandler(ScriptMain_Startup);  
            this.Shutdown += new System.EventHandler(ScriptMain_Shutdown);  
        }  
        enum ScriptResults  
        {  
            Success = DTSExecResult.Success,  
            Failure = DTSExecResult.Failure  
        };  
  
        #endregion  
  
        /*  
The execution engine calls this method when the task executes.  
To access the object model, use the Dts property. Connections, variables, events,  
and logging features are available as members of the Dts property as shown in the following examples.  
  
To reference a variable, call Dts.Variables["MyCaseSensitiveVariableName"].Value;  
To post a log entry, call Dts.Log("This is my log text", 999, null);  
To fire an event, call Dts.Events.FireInformation(99, "test", "hit the help message", "", 0, true);  
  
To use the connections collection use something like the following:  
ConnectionManager cm = Dts.Connections.Add("OLEDB");  
cm.ConnectionString = "Data Source=localhost;Initial Catalog=AdventureWorks;Provider=SQLNCLI10;Integrated Security=SSPI;Auto Translate=False;";  
  
Before returning from this method, set the value of Dts.TaskResult to indicate success or failure.  
  
To open Help, press F1.  
*/  
  
        public void Main()  
        {  
            // TODO: Add your code here  
            Dts.TaskResult = (int)ScriptResults.Success;  
        }  
    }  
```  
  
### <a name="additional-project-items-in-the-script-task-project"></a>Altri elementi di progetto dell'attività Script  
 Il progetto dell'attività Script può includere elementi diversi dall'elemento **ScriptMain** predefinito. È possibile aggiungere classi, moduli e file di codice al progetto. È anche possibile utilizzare cartelle per organizzare gruppi di elementi. Tutti gli elementi aggiunti vengono salvati in modo permanente nel pacchetto.  
  
### <a name="references-in-the-script-task-project"></a>Riferimenti nel progetto dell'attività Script  
 Per aggiungere riferimenti agli assembly gestiti, fare clic con il pulsante destro del mouse sul progetto di attività script in **Esplora progetti** e quindi scegliere **Aggiungi riferimento**. Per altre informazioni, vedere [Riferimenti ad altri assembly nelle soluzioni di scripting](../../../integration-services/extending-packages-scripting/referencing-other-assemblies-in-scripting-solutions.md).  
  
> [!NOTE]  
>  È possibile visualizzare i riferimenti al progetto nell'ambiente IDE di VSTA in **Visualizzazione classi** o in **Esplora progetti**. Queste finestre possono essere aperte dal menu **Visualizza**. È possibile aggiungere un nuovo riferimento dal menu **Progetto**, da **Esplora progetti** o da **Visualizzazione classi**.  
  
## <a name="interacting-with-the-package-in-the-script-task"></a>Interazione con il pacchetto nell'attività Script  
 L'attività Script usa l'oggetto **Dts** globale, che è un'istanza della classe <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel>, e i relativi membri per interagire con il pacchetto contenitore e con il runtime di [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)].  
  
 La tabella seguente elenca i principali membri pubblici della classe <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel>, che viene esposta al codice di attività Script tramite l'oggetto **Dts** globale. Negli argomenti di questa sezione verrà descritto in maggior dettaglio l'utilizzo di questi membri.  
  
|Membro|Scopo|  
|------------|-------------|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Connections%2A>|Fornisce accesso alle gestioni connessioni definite nel pacchetto.|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Events%2A>|Fornisce un'interfaccia degli eventi per consentire all'attività Script di generare errori, avvisi e messaggi informativi.|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.ExecutionValue%2A>|Fornisce un modo semplice per restituire al runtime un singolo oggetto (in aggiunta a **TaskResult**) che può anche essere usato per la diramazione del flusso di lavoro.|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Log%2A>|Registra informazioni quali lo stato e i risultati dell'attività nei provider di log abilitati.|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.TaskResult%2A>|Indica l'esito positivo o negativo dell'attività.|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Transaction%2A>|Fornisce la transazione, se presente, in cui è in esecuzione il contenitore dell'attività.|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Variables%2A>|Fornisce accesso alle variabili elencate nelle proprietà **ReadOnlyVariables** e **ReadWriteVariables** dell'attività per l'uso all'interno dello script.|  
  
 La classe <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel> contiene anche alcuni membri pubblici che probabilmente non verranno utilizzati.  
  
|Membro|Descrizione|  
|------------|-----------------|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.VariableDispenser%2A>|La proprietà <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Variables%2A> fornisce un accesso più semplice alle variabili. Anche se è possibile utilizzare <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.VariableDispenser%2A>, è necessario chiamare in modo esplicito i metodi per bloccare e sbloccare le variabili per la lettura e la scrittura. L'attività Script gestisce automaticamente la semantica di blocco quando si utilizza la proprietà <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Variables%2A>.|  
  
## <a name="debugging-the-script-task"></a>Debug dell'attività Script  
 Per eseguire il debug del codice nell'attività Script, impostare almeno un punto di interruzione nel codice, quindi chiudere l'IDE di VSTA per eseguire il pacchetto in [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)]. Quando l'esecuzione del pacchetto entra nell'attività Script, l'IDE di VSTA viene riaperto e visualizza il codice in modalità di sola lettura. Quando l'esecuzione raggiunge il punto di interruzione, è possibile esaminare i valori delle variabili e scorrere il codice rimanente.  
  
> [!WARNING]  
>  È possibile eseguire il debug dell'attività Script quando si esegue il pacchetto in modalità a 64 bit.  
  
> [!NOTE]  
>  È necessario eseguire il pacchetto da sottoporre a debug nell'attività Script. Se si esegue solo la singola attività, i punti di interruzione nel codice dell'attività Script vengono ignorati.  
  
> [!NOTE]  
>  Non è possibile eseguire il debug di un'attività Script se l'attività viene eseguita nell'ambito di un pacchetto figlio eseguito da un'attività Esegui pacchetto. In tali circostanze, i punti di interruzione impostati all'interno dell'attività Script del pacchetto figlio verranno ignorati. È possibile eseguire il debug del pacchetto figlio normalmente eseguendolo separatamente.  
  
> [!NOTE]  
>  Quando si esegue il debug di un pacchetto che contiene più attività Script, il debugger è in grado di elaborarne solo una. Il sistema può eseguire il debug di un'altra attività Script se il debugger termina l'elaborazione, come nel caso di un contenitore Ciclo ForEach o Ciclo For.  
  
## <a name="external-resources"></a>Risorse esterne  
  
-   Intervento del blog, [VSTA setup and configuration troubles for SSIS 2008 and R2 installations](/archive/blogs/jason_howell/vsta-setup-and-configuration-troubles-for-ssis-2008-and-r2-installations) (Problemi di installazione e configurazione di VSTA per le installazioni SSIS 2008 e R2), in blogs.msdn.com.  
  
## <a name="see-also"></a>Vedere anche  
 [Riferimenti ad altri assembly nelle soluzioni di scripting](../../../integration-services/extending-packages-scripting/referencing-other-assemblies-in-scripting-solutions.md)   
 [Configurazione dell'attività Script nell'editor attività Script](../../../integration-services/extending-packages-scripting/task/configuring-the-script-task-in-the-script-task-editor.md)  
  
