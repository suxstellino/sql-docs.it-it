---
description: Aggiunta di supporto per il debug in un'attività personalizzata
title: Aggiunta di supporto per il debug in un'attività personalizzata | Microsoft Docs
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
- BreakpointManager class
- breakpoints [Integration Services]
- custom tasks [Integration Services], debugging
- IDTSSuspend interface
- IDTSBreakpointSite interface
- SSIS custom tasks, debugging
- debugging [Integration Services], custom tasks
ms.assetid: 7f06e49b-0b60-4e81-97da-d32dc248264a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 28ba35dc2aecd6c25da114493b8c6e424b77dccf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346537"
---
# <a name="adding-support-for-debugging-in-a-custom-task"></a>Aggiunta di supporto per il debug in un'attività personalizzata

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Il motore di runtime di [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] consente la sospensione di pacchetti, attività e altri tipi di contenitori durante l'esecuzione tramite l'utilizzo di punti di interruzione. L'utilizzo di punti di interruzione consente di rivedere e correggere gli errori che impediscono la corretta esecuzione dell'applicazione o delle attività. L'architettura dei punti di interruzione consente al client di valutare il valore di runtime degli oggetti nel pacchetto in determinati punti dell'esecuzione mentre l'elaborazione dell'attività è sospesa.  
  
 Gli sviluppatori di attività personalizzate possono utilizzare questa architettura per creare destinazioni di punti di interruzione personalizzati utilizzando l'interfaccia <xref:Microsoft.SqlServer.Dts.Runtime.IDTSBreakpointSite> e la relativa interfaccia padre <xref:Microsoft.SqlServer.Dts.Runtime.IDTSSuspend>. L'interfaccia <xref:Microsoft.SqlServer.Dts.Runtime.IDTSBreakpointSite> definisce l'interazione tra il motore di runtime e l'attività per la creazione e la gestione di siti o destinazioni di punti di interruzione personalizzati. L'interfaccia <xref:Microsoft.SqlServer.Dts.Runtime.IDTSSuspend> fornisce metodi e proprietà che vengono chiamati dal motore di runtime per notificare all'attività di sospendere o riprendere l'esecuzione.  
  
 Un sito o una destinazione di punti di interruzione è un punto nell'esecuzione dell'attività in cui l'elaborazione può essere sospesa. Gli utenti selezionano uno dei siti di punti di interruzione disponibili nella finestra di dialogo **Imposta punti di interruzione**. Ad esempio, oltre alle opzioni predefinite dei punti di interruzione, il contenitore Ciclo Foreach prevede l'opzione "Interrompi all'inizio di ogni iterazione del ciclo".  
  
 Quando un'attività raggiunge la destinazione di un punto di interruzione durante l'esecuzione, la valuta per determinare se il punto di interruzione è abilitato. Ciò indica che l'utente desidera arrestare l'esecuzione in corrispondenza di tale punto di interruzione. Se il punto di interruzione è abilitato, l'attività genera l'evento <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents.OnBreakpointHit%2A> per il motore di runtime. Il motore di runtime risponde all'evento chiamando il metodo **Suspend** di ogni attività attualmente in esecuzione nel pacchetto. L'esecuzione dell'attività riprende quando il runtime chiama il metodo **ResumeExecution** dell'attività sospesa.  
  
 Le attività che non utilizzano punti di interruzione devono comunque implementare le interfacce <xref:Microsoft.SqlServer.Dts.Runtime.IDTSBreakpointSite> e <xref:Microsoft.SqlServer.Dts.Runtime.IDTSSuspend>. Ciò assicura che l'attività venga sospesa correttamente quando altri oggetti del pacchetto generano eventi <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents.OnBreakpointHit%2A>.  
  
## <a name="idtsbreakpointsite-interface-and-breakpointmanager"></a>Interfaccia IDTSBreakpointSite e BreakpointManager  
 Le attività creano destinazioni di punti di interruzione chiamando il metodo <xref:Microsoft.SqlServer.Dts.Runtime.BreakpointManager.CreateBreakpointTarget%2A> di <xref:Microsoft.SqlServer.Dts.Runtime.BreakpointManager>, fornendo un ID integer e una stringa descrittiva come parametri. Quando l'attività raggiunge il punto del codice che contiene la destinazione del punto di interruzione, la valuta utilizzando il metodo <xref:Microsoft.SqlServer.Dts.Runtime.BreakpointManager.IsBreakpointTargetEnabled%2A> per determinare se tale punto di interruzione è abilitato. Se **true**, l'attività invia una notifica al motore di runtime generando l'evento <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents.OnBreakpointHit%2A>.  
  
 L'interfaccia <xref:Microsoft.SqlServer.Dts.Runtime.IDTSBreakpointSite> definisce un singolo metodo, <xref:Microsoft.SqlServer.Dts.Runtime.IDTSBreakpointSite.AcceptBreakpointManager%2A>, che viene chiamato dal motore di runtime durante la creazione di attività. Questo metodo fornisce come parametro l'oggetto <xref:Microsoft.SqlServer.Dts.Runtime.BreakpointManager>, che viene quindi utilizzato dall'attività per creare e gestire i propri punti di interruzione. Le attività devono archiviare l'oggetto <xref:Microsoft.SqlServer.Dts.Runtime.BreakpointManager> in locale per usarlo durante l'esecuzione dei metodi **Validate** ed **Execute**.  
  
 Nell'esempio di codice seguente viene illustrato come creare una destinazione di punti di interruzione tramite <xref:Microsoft.SqlServer.Dts.Runtime.BreakpointManager>. Viene chiamato il metodo <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents.OnBreakpointHit%2A> per generare l'evento.  
  
```csharp  
public void AcceptBreakpointManager( BreakpointManager breakPointManager )  
{  
   //   Store the breakpoint manager locally.  
   this.bpm  = breakPointManager;  
}  
public override DTSExecResult Execute( Connections connections,  
  Variables variables, IDTSComponentEvents events,  
  IDTSLogging log, DtsTransaction txn)  
{  
   //   Create a breakpoint.  
   this.bpm.CreateBreakPointTarget( 1 , "A sample breakpoint target." );  
...  
   if( this.bpm.IsBreakpointTargetEnabled( 1 ) == true )  
      events.OnBreakpointHit( this.bpm.GetBreakpointTarget( 1 ) );  
}  
```  
  
```vb  
Public Sub AcceptBreakpointManager(ByVal breakPointManager As BreakpointManager)  
  
   ' Store the breakpoint manager locally.  
   Me.bpm  = breakPointManager  
  
End Sub  
  
Public Overrides Function Execute(ByVal connections As Connections, _  
  ByVal variables As Variables, ByVal events As IDTSComponentEvents, _  
  ByVal log As IDTSLogging, ByVal txn As DtsTransaction) As DTSExecResult  
  
   ' Create a breakpoint.  
   Me.bpm.CreateBreakPointTarget(1 , "A sample breakpoint target.")  
  
   If Me.bpm.IsBreakpointTargetEnabled(1) = True Then  
      events.OnBreakpointHit(Me.bpm.GetBreakpointTarget(1))  
   End If  
  
End Function  
```  
  
## <a name="idtssuspend-interface"></a>Interfaccia IDTSSuspend  
 L'interfaccia <xref:Microsoft.SqlServer.Dts.Runtime.IDTSSuspend> definisce i metodi che vengono chiamati dal motore di runtime quando sospende o riprende l'esecuzione di un'attività. L'interfaccia <xref:Microsoft.SqlServer.Dts.Runtime.IDTSSuspend> viene implementata dall'interfaccia <xref:Microsoft.SqlServer.Dts.Runtime.IDTSBreakpointSite> e i relativi metodi **Suspend** e **ResumeExecution** vengono in genere sottoposti a override dall'attività personalizzata. Quando il motore di runtime riceve un evento **OnBreakpointHit** da un'attività, chiama il metodo **Suspend** di ogni attività in esecuzione, notificando la pausa alle attività. Quando il client riprende l'esecuzione, il motore di runtime chiama il metodo **ResumeExecution** delle attività sospese.  
  
 La sospensione e la ripresa dell'esecuzione di un'attività implica la sospensione e la ripresa del thread di esecuzione dell'attività. Nel codice gestito questo risultato si ottiene usando la classe **ManualResetEvent** nello spazio dei nomi **System.Threading** di .NET Framework.  
  
 Nell'esempio di codice seguente vengono illustrate la sospensione e la ripresa dell'esecuzione di un'attività. Si noti che il metodo **Execute** è diverso rispetto all'esempio di codice precedente e che il thread di esecuzione è in pausa quando viene attivato il punto di interruzione.  
  
```csharp  
private ManualResetEvent m_suspended = new ManualResetEvent( true );  
private ManualResetEvent m_canExecute = new ManualResetEvent( true );  
private int   m_suspendRequired = 0;  
private int   m_debugMode = 0;  
  
public override DTSExecResult Execute( Connections connections, Variables variables, IDTSComponentEvents events, IDTSLogging log, DtsTransaction txn)  
{  
   // While a task is not executing, it is suspended.    
   // Now that we are executing,  
   // change to not suspended.  
   ChangeEvent(m_suspended, false);  
  
   // Check for a suspend before doing any work,   
   // in case the suspend and execute calls  
   // were initiated at virtually the same time.  
   CheckAndSuspend();  
   CheckAndFireBreakpoint( componentEvents, 1);  
}  
private void CheckAndSuspend()  
{  
   // Loop until we can execute.    
   // The loop is required rather than a simple If  
   // because there is a time between the return from WaitOne and the  
   // reset that we might receive another Suspend call.    
   // Suspend() will see that we are suspended  
   // and return.  So we need to rewait.  
   while (!m_canExecute.WaitOne(0, false))  
   {  
      ChangeEvent(m_suspended, true);  
      m_canExecute.WaitOne();  
      ChangeEvent(m_suspended, false);  
   }  
}  
private void CheckAndFireBreakpoint(IDTSComponentEvents events, int breakpointID)  
{  
   // If the breakpoint is enabled, fire it.  
   if (m_debugMode != 0 &&    this.bpm.IsBreakpointTargetEnabled(breakpointID))  
   {  
      //   Enter a suspend mode before firing the breakpoint.    
      //   Firing the breakpoint will cause the runtime   
      //   to call Suspend on this task.    
      //   Because we are blocked on the breakpoint,   
      //   we are suspended.  
      ChangeEvent(m_suspended, true);  
      events.OnBreakpointHit(this.bpm.GetBreakpointTarget(breakpointID));  
      ChangeEvent(m_suspended, false);  
   }  
   // Check for a suspension for two reasons:   
   //   1. If we are at a point where we could fire a breakpoint,   
   //      we are at a valid suspend point.  Even if we didn't hit a  
   //      breakpoint, the runtime may have called suspend,   
   //      so check for it.       
   //   2. Between the return from OnBreakpointHit   
   //      and the reset of the event, it is possible to have  
   //      received a suspend call from which we returned because   
   //      we were already suspended.  We need to be sure it is okay  
   //      to continue executing now.  
   CheckAndSuspend();  
}  
static void ChangeEvent(ManualResetEvent e, bool shouldSet)  
{  
   bool succeeded;  
   if (shouldSet)  
      succeeded = e.Set();  
   else  
      succeeded = e.Reset();  
  
   if (!succeeded)  
      throw new Exception("Synchronization object failed.");  
  
}  
public bool SuspendRequired  
{  
   get   {return m_suspendRequired != 0;}  
   set  
   {  
      // This lock is also taken by Suspend().    
      // Because it is possible for the package to be  
      // suspended and resumed in quick succession,   
      // this property might be set before  
      // the actual Suspend() call.    
      // Without the lock, the Suspend() might reset the canExecute  
      // event after we set it to abort the suspension.  
      lock (this)  
      {  
         Interlocked.Exchange(ref m_suspendRequired, value ? 1 : 0);  
         if (!value)  
            ResumeExecution();  
      }  
   }  
}  
public void ResumeExecution()  
{  
   ChangeEvent( m_canExecute,true );  
}  
public void Suspend()  
{  
   // This lock is also taken by the set SuspendRequired method.    
   // It prevents this call from overriding an   
   // aborted suspension.  See comments in set SuspendRequired.  
   lock (this)  
   {  
      // If a Suspend is required, do it.  
      if (m_suspendRequired != 0)  
         ChangeEvent(m_canExecute, false);  
   }  
   // We can't return from Suspend until the task is "suspended".  
   // This can happen one of two ways:   
   // the m_suspended event occurs, indicating that the execute thread  
   // has suspended, or the canExecute flag is set,   
   // indicating that a suspend is no longer required.  
   WaitHandle [] suspendOperationComplete = {m_suspended, m_canExecute};  
   WaitHandle.WaitAny(suspendOperationComplete);  
}  
```  
  
```vb  
Private m_suspended As ManualResetEvent = New ManualResetEvent(True)  
Private m_canExecute As ManualResetEvent = New ManualResetEvent(True)  
Private m_suspendRequired As Integer = 0  
Private m_debugMode As Integer = 0  
  
Public Overrides Function Execute(ByVal connections As Connections, _  
ByVal variables As Variables, ByVal events As IDTSComponentEvents, _  
ByVal log As IDTSLogging, ByVal txn As DtsTransaction) As DTSExecResult  
  
   ' While a task is not executing it is suspended.    
   ' Now that we are executing,  
   ' change to not suspended.  
   ChangeEvent(m_suspended, False)  
  
   ' Check for a suspend before doing any work,   
   ' in case the suspend and execute calls  
   ' were initiated at virtually the same time.  
   CheckAndSuspend()  
   CheckAndFireBreakpoint(componentEvents, 1)  
  
End Function  
  
Private Sub CheckAndSuspend()  
  
   ' Loop until we can execute.    
   ' The loop is required rather than a simple if  
   ' because there is a time between the return from WaitOne and the  
   ' reset that we might receive another Suspend call.    
   ' Suspend() will see that we are suspended  
   ' and return.  So we need to rewait.  
   Do While Not m_canExecute.WaitOne(0, False)  
              ChangeEvent(m_suspended, True)  
              m_canExecute.WaitOne()  
              ChangeEvent(m_suspended, False)  
   Loop  
  
End Sub  
  
Private Sub CheckAndFireBreakpoint(ByVal events As IDTSComponentEvents, _  
ByVal breakpointID As Integer)  
  
   ' If the breakpoint is enabled, fire it.  
   If m_debugMode <> 0 AndAlso Me.bpm.IsBreakpointTargetEnabled(breakpointID) Then  
              '   Enter a suspend mode before firing the breakpoint.    
              '   Firing the breakpoint will cause the runtime   
              '   to call Suspend on this task.    
              '   Because we are blocked on the breakpoint,   
              '   we are suspended.  
              ChangeEvent(m_suspended, True)  
              events.OnBreakpointHit(Me.bpm.GetBreakpointTarget(breakpointID))  
              ChangeEvent(m_suspended, False)  
   End If  
  
   ' Check for a suspension for two reasons:   
   '   1. If we are at a point where we could fire a breakpoint,   
   '         we are at a valid suspend point.  Even if we didn't hit a  
   '         breakpoint, the runtime may have called suspend,   
   '         so check for it.     
   '   2. Between the return from OnBreakpointHit   
   '         and the reset of the event, it is possible to have  
   '         received a suspend call from which we returned because   
   '         we were already suspended.  We need to be sure it is okay  
   '         to continue executing now.  
   CheckAndSuspend()  
  
End Sub  
  
Shared Sub ChangeEvent(ByVal e As ManualResetEvent, ByVal shouldSet As Boolean)  
  
   Dim succeeded As Boolean  
   If shouldSet Then  
              succeeded = e.Set()  
   Else  
              succeeded = e.Reset()  
   End If  
  
   If (Not succeeded) Then  
              Throw New Exception("Synchronization object failed.")  
   End If  
  
End Sub  
  
Public Property SuspendRequired() As Boolean  
   Get  
               Return m_suspendRequired <> 0  
  End Get  
  Set  
    ' This lock is also taken by Suspend().    
     '   Because it is possible for the package to be  
     '   suspended and resumed in quick succession,   
     '   this property might be set before  
     '   the actual Suspend() call.    
     '   Without the lock, the Suspend() might reset the canExecute  
     '   event after we set it to abort the suspension.  
              SyncLock Me  
                         Interlocked.Exchange(m_suspendRequired,IIf(Value, 1, 0))  
                         If (Not Value) Then  
                                    ResumeExecution()  
                         End If  
              End SyncLock  
   End Set  
End Property  
  
Public Sub ResumeExecution()  
   ChangeEvent(m_canExecute,True)  
End Sub  
  
Public Sub Suspend()  
  
   ' This lock is also taken by the set SuspendRequired method.    
   ' It prevents this call from overriding an   
   ' aborted suspension.  See comments in set SuspendRequired.  
   SyncLock Me  
   ' If a Suspend is required, do it.  
              If m_suspendRequired <> 0 Then  
                         ChangeEvent(m_canExecute, False)  
              End If  
   End SyncLock  
   ' We can't return from Suspend until the task is "suspended".  
   ' This can happen one of two ways:   
   ' the m_suspended event occurs, indicating that the execute thread  
   ' has suspended, or the canExecute flag is set,   
   ' indicating that a suspend is no longer required.  
   Dim suspendOperationComplete As WaitHandle() = {m_suspended, m_canExecute}  
   WaitHandle.WaitAny(suspendOperationComplete)  
  
End Sub  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Debug del flusso di controllo](../../../integration-services/troubleshooting/debugging-control-flow.md)  
  
  
