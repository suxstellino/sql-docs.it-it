---
description: Generazione e definizione di eventi in un'attività personalizzata
title: Generazione e definizione di eventi in un'attività personalizzata | Microsoft Docs
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
- SSIS events, custom
- status information [SQL Server], task events
- custom tasks [Integration Services], events
- SSIS custom tasks, events
- IDTSComponentEvents interface
- events [Integration Services], custom
- events [Integration Services], runtime
- custom events [Integration Services]
- SSIS events, runtime
- IDTSEvents interface
ms.assetid: e0898aa1-e90c-4c4e-99d4-708a76efddfd
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1fcafce23542f1d5d1ea1756cdb361d5197286b6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091612"
---
# <a name="raising-and-defining-events-in-a-custom-task"></a>Generazione e definizione di eventi in un'attività personalizzata

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Il motore di runtime di [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] include una raccolta di eventi che forniscono lo stato di avanzamento durante la convalida e l'esecuzione di un'attività. L'interfaccia <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents> definisce questi eventi e viene fornita alle attività come parametro per i metodi <xref:Microsoft.SqlServer.Dts.Runtime.Executable.Validate%2A> e <xref:Microsoft.SqlServer.Dts.Runtime.Executable.Execute%2A>.  
  
 È disponibile un altro set di eventi, definiti nell'interfaccia <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents>, che vengono generati per conto dell'attività da <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>. L'oggetto <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> genera gli eventi che si verificano prima e dopo la convalida e l'esecuzione, mentre l'attività genera gli eventi che si verificano durante l'esecuzione e la convalida.  
  
## <a name="creating-custom-events"></a>Creazione di eventi personalizzati  
 Gli sviluppatori di attività personalizzate possono definire nuovi eventi personalizzati creando un nuovo oggetto <xref:Microsoft.SqlServer.Dts.Runtime.EventInfo> nell'implementazione sottoposta a override del metodo <xref:Microsoft.SqlServer.Dts.Runtime.Task.InitializeTask%2A>. L'oggetto <xref:Microsoft.SqlServer.Dts.Runtime.EventInfo>, una volta creato, viene aggiunto alla raccolta **EventInfos** tramite il metodo <xref:Microsoft.SqlServer.Dts.Runtime.EventInfos.Add%2A>. La firma del metodo <xref:Microsoft.SqlServer.Dts.Runtime.EventInfos.Add%2A> è la seguente:  
  
 `public void Add(string eventName, string description, bool allowEventHandlers, string[] parameterNames, TypeCode[] parameterTypes, string[] parameterDescriptions);`  
  
 L'esempio di codice seguente illustra il metodo **InitializeTask** di un'attività personalizzata, in cui vengono creati due eventi e vengono impostate le relative proprietà. I nuovi eventi vengono quindi aggiunti alla raccolta <xref:Microsoft.SqlServer.Dts.Runtime.EventInfos>.  
  
 Il primo evento personalizzato presenta un elemento *eventName* di "**OnBeforeIncrement**" e un elemento *description* di "**Fires after the initial value is updated.**" Il parametro successivo, il valore **true**, indica che questo evento deve consentire la creazione di un contenitore del gestore eventi per gestire l’evento. Il gestore eventi è un contenitore che fornisce la struttura in un pacchetto e servizi alle attività, analogamente ad altri contenitori come Sequence, ForLoop e ForEachLoop. Quando il parametro *allowEventHandlers* è **true**, gli oggetti <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler> vengono creati per l'evento. I parametri definiti per l'evento sono ora disponibili per <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler> nella raccolta di variabili di <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler>.  
  
```csharp  
public override void InitializeTask(Connections connections,  
   VariableDispenser variables, IDTSInfoEvents events,  
   IDTSLogging log, EventInfos eventInfos,  
   LogEntryInfos logEntryInfos, ObjectReferenceTracker refTracker)  
{  
    this.eventInfos = eventInfos;  
    string[] paramNames = new string[1];  
    TypeCode[] paramTypes = new TypeCode[1]{TypeCode.Int32};  
    string[] paramDescriptions = new string[1];  
  
    paramNames[0] = "InitialValue";  
    paramDescriptions[0] = "The value before it is incremented.";  
  
    this.eventInfos.Add("OnBeforeIncrement",   
      "Fires before the task increments the value.",  
      true,paramNames,paramTypes,paramDescriptions);  
    this.onBeforeIncrement = this.eventInfos["OnBeforeIncrement"];  
  
    paramDescriptions[0] = "The value after it has been incremented.";  
    this.eventInfos.Add("OnAfterIncrement",  
      "Fires after the initial value is updated.",  
      true,paramNames, paramTypes,paramDescriptions);  
    this.onAfterIncrement = this.eventInfos["OnAfterIncrement"];  
}  
```  
  
```vb  
Public Overrides Sub InitializeTask(ByVal connections As Connections, _  
ByVal variables As VariableDispenser, ByVal events As IDTSInfoEvents, _  
ByVal log As IDTSLogging, ByVal eventInfos As EventInfos, _  
ByVal logEntryInfos As LogEntryInfos, ByVal refTracker As ObjectReferenceTracker)   
  
    Dim paramNames(0) As String  
    Dim paramTypes(0) As TypeCode = {TypeCode.Int32}  
    Dim paramDescriptions(0) As String  
  
    Me.eventInfos = eventInfos  
  
    paramNames(0) = "InitialValue"  
    paramDescriptions(0) = "The value before it is incremented."  
  
    Me.eventInfos.Add("OnBeforeIncrement", _  
      "Fires before the task increments the value.", _  
      True, paramNames, paramTypes, paramDescriptions)  
    Me.onBeforeIncrement = Me.eventInfos("OnBeforeIncrement")  
  
    paramDescriptions(0) = "The value after it has been incremented."  
    Me.eventInfos.Add("OnAfterIncrement", _  
      "Fires after the initial value is updated.", True, _  
      paramNames, paramTypes, paramDescriptions)  
    Me.onAfterIncrement = Me.eventInfos("OnAfterIncrement")  
  
End Sub  
```  
  
## <a name="raising-custom-events"></a>Generazione di eventi personalizzati  
 Gli eventi personalizzati vengono generati con una chiamata al metodo <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireCustomEvent%2A>. La riga di codice seguente genera un evento personalizzato.  
  
```csharp  
componentEvents.FireCustomEvent(this.onBeforeIncrement.Name,  
   this.onBeforeIncrement.Description, ref arguments,  
   null, ref bFireOnBeforeIncrement);  
```  
  
```vb  
componentEvents.FireCustomEvent(Me.onBeforeIncrement.Name, _  
Me.onBeforeIncrement.Description, arguments, _  
Nothing,  bFireOnBeforeIncrement)  
```  
  
## <a name="sample"></a>Esempio  
 L'esempio di codice seguente mostra un'attività che definisce un evento personalizzato nel metodo **InitializeTask**, aggiunge l'evento personalizzato alla raccolta <xref:Microsoft.SqlServer.Dts.Runtime.EventInfos>, quindi genera l'evento personalizzato durante il relativo metodo **Execute** chiamando il metodo <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireCustomEvent%2A>.  
  
```csharp  
[DtsTask(DisplayName = "CustomEventTask")]  
    public class CustomEventTask : Task  
    {  
        public override DTSExecResult Execute(Connections connections,   
          VariableDispenser variableDispenser, IDTSComponentEvents componentEvents,  
           IDTSLogging log, object transaction)  
        {  
            bool fireAgain;  
            object[] args = new object[1] { "The value of the parameter." };  
            componentEvents.FireCustomEvent( "MyCustomEvent",   
              "Firing the custom event.", ref args,  
              "CustomEventTask" , ref fireAgain );  
            return DTSExecResult.Success;  
        }  
  
        public override void InitializeTask(Connections connections,  
          VariableDispenser variableDispenser, IDTSInfoEvents events,  
          IDTSLogging log, EventInfos eventInfos,  
          LogEntryInfos logEntryInfos, ObjectReferenceTracker refTracker)  
        {  
            string[] names = new string[1] {"Parameter1"};  
            TypeCode[] types = new TypeCode[1] {TypeCode.String};  
            string[] descriptions = new string[1] {"Parameter description." };  
  
            eventInfos.Add("MyCustomEvent",  
             "Fires when my interesting event happens.",  
             true, names, types, descriptions);  
  
        }  
   }  
```  
  
```vb  
<DtsTask(DisplayName = "CustomEventTask")> _   
    Public Class CustomEventTask  
     Inherits Task  
        Public Overrides Function Execute(ByVal connections As Connections, _  
          ByVal variableDispenser As VariableDispenser, _  
          ByVal componentEvents As IDTSComponentEvents, _  
          ByVal log As IDTSLogging, ByVal transaction As Object) _  
          As DTSExecResult  
  
            Dim fireAgain As Boolean  
            Dim args() As Object =  New Object(1) {"The value of the parameter."}  
  
            componentEvents.FireCustomEvent("MyCustomEvent", _  
              "Firing the custom event.", args, _  
              "CustomEventTask" ,  fireAgain)  
            Return DTSExecResult.Success  
        End Function  
  
        Public Overrides  Sub InitializeTask(ByVal connections As Connections, _  
          ByVal variableDispenser As VariableDispenser,  
          ByVal events As IDTSInfoEvents,  
          ByVal log As IDTSLogging, ByVal eventInfos As EventInfos, ByVal logEnTryInfos As LogEnTryInfos, ByVal refTracker As ObjectReferenceTracker)  
  
            Dim names() As String =  New String(1) {"Parameter1"}  
            Dim types() As TypeCode =  New TypeCode(1) {TypeCode.String}  
            Dim descriptions() As String =  New String(1) {"Parameter description."}  
  
            eventInfos.Add("MyCustomEvent", _  
              "Fires when my interesting event happens.", _  
              True, names, types, descriptions)  
  
        End Sub  
  
    End Class  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Gestori eventi di Integration Services &#40;SSIS&#41;](../../../integration-services/integration-services-ssis-event-handlers.md)   
 [Aggiunta di un gestore eventi a un pacchetto](../../integration-services-ssis-event-handlers.md)  
  
