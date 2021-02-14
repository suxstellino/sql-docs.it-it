---
description: Connessione alle origini dati in un'attività personalizzata
title: Connessione alle origini dati in un'attività personalizzata | Microsoft Docs
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
- ConnectionManager objects
- connection managers [Integration Services], external data sources
- data sources [Integration Services], external
- custom tasks [Integration Services], external data sources
- external data sources [Integration Services]
- connections [Integration Services], external data sources
- SSIS custom tasks, external data sources
ms.assetid: 9f0b3a43-3eaa-4b3c-bb08-29b630c11306
author: chugugrace
ms.author: chugu
ms.openlocfilehash: eb2b6d7c5d1ad85006da66af2ffcc783d853f3d2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348247"
---
# <a name="connecting-to-data-sources-in-a-custom-task"></a>Connessione alle origini dati in un'attività personalizzata

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Le attività si connettono alle origini dati esterne per recuperare o salvare i dati tramite una gestione connessione. In fase di progettazione una gestione connessione rappresenta una connessione logica, in cui sono descritte informazioni chiave come il nome del server e le eventuali proprietà di autenticazione. In fase di esecuzione le attività chiamano il metodo <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.AcquireConnection%2A> della gestione connessione per stabilire la connessione fisica all'origine dati.  
  
 Poiché un pacchetto può contenere molte attività, ognuna delle quali con connessioni a origini dati diverse, tutte le gestioni connessioni vengono registrate in una raccolta, <xref:Microsoft.SqlServer.Dts.Runtime.Connections>. Le attività utilizzano la raccolta presente nel relativo pacchetto per individuare la gestione connessione che utilizzeranno durante la convalida e l'esecuzione. La raccolta <xref:Microsoft.SqlServer.Dts.Runtime.Connections> costituisce il primo parametro per i metodi <xref:Microsoft.SqlServer.Dts.Runtime.Task.Validate%2A> e <xref:Microsoft.SqlServer.Dts.Runtime.Task.Execute%2A>.  
  
 È possibile impedire all'attività di utilizzare la gestione connessione errata rendendo gli oggetti <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> della raccolta visualizzabili dall'utente tramite una finestra di dialogo o un elenco a discesa nell'interfaccia utente grafica. In questo modo l'utente ha la possibilità di effettuare una selezione solo tra gli oggetti <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> del tipo appropriato contenuti nel pacchetto.  
  
 Le attività chiamano il metodo <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.AcquireConnection%2A> per stabilire la connessione fisica all'origine dati. Il metodo restituisce l'oggetto connessione sottostante che può essere utilizzato dall'attività. Poiché la gestione connessione isola i dettagli di implementazione dell'oggetto connessione sottostante dall'attività, quest'ultima deve limitarsi a chiamare il metodo <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.AcquireConnection%2A> per stabilire la connessione, senza intervenire in altri aspetti della connessione.  
  
## <a name="example"></a>Esempio  
 Nel codice di esempio seguente è illustrata la convalida del nome di <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> nei metodi Validate ed Execute e viene mostrato come utilizzare il metodo <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.AcquireConnection%2A> per stabilire la connessione fisica nel metodo Execute.  
  
```csharp  
private string connectionManagerName = "";  
  
public string ConnectionManagerName  
{  
  get { return this.connectionManagerName; }  
  set { this.connectionManagerName = value; }  
}  
  
public override DTSExecResult Validate(  
  Connections connections, VariableDispenser variableDispenser,  
  IDTSComponentEvents componentEvents, IDTSLogging log)  
{  
  // If the connection manager exists, validation is successful;  
  // otherwise, fail validation.  
  try  
  {  
    ConnectionManager cm = connections[this.connectionManagerName];  
    return DTSExecResult.Success;  
  }  
  catch (System.Exception e)  
  {  
    componentEvents.FireError(0, "SampleTask", "Invalid connection manager.", "", 0);  
    return DTSExecResult.Failure;  
  }  
}  
  
public override DTSExecResult Execute(Connections connections,   
  VariableDispenser variableDispenser, IDTSComponentEvents componentEvents,   
  IDTSLogging log, object transaction)  
{  
  try  
  {  
    ConnectionManager cm = connections[this.connectionManagerName];  
    object connection = cm.AcquireConnection(transaction);  
    return DTSExecResult.Success;  
  }  
  catch (System.Exception exception)  
  {  
    componentEvents.FireError(0, "SampleTask", exception.Message, "", 0);  
    return DTSExecResult.Failure;  
  }  
}  
```  
  
```vb  
Private _connectionManagerName As String = ""  
  
Public Property ConnectionManagerName() As String  
  Get  
    Return Me._connectionManagerName  
  End Get  
  Set(ByVal Value As String)  
    Me._connectionManagerName = value  
  End Set  
End Property  
  
Public Overrides Function Validate( _  
  ByVal connections As Microsoft.SqlServer.Dts.Runtime.Connections, _  
  ByVal variableDispenser As Microsoft.SqlServer.Dts.Runtime.VariableDispenser, _  
  ByVal componentEvents As Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents, _  
  ByVal log As Microsoft.SqlServer.Dts.Runtime.IDTSLogging) _  
  As Microsoft.SqlServer.Dts.Runtime.DTSExecResult  
  
  ' If the connection manager exists, validation is successful;  
  ' otherwise fail validation.  
  Try  
    Dim cm As ConnectionManager = connections(Me._connectionManagerName)  
    Return DTSExecResult.Success  
  Catch e As System.Exception  
    componentEvents.FireError(0, "SampleTask", "Invalid connection manager.", "", 0)  
    Return DTSExecResult.Failure  
  End Try  
  
End Function  
  
Public Overrides Function Execute( _  
  ByVal connections As Microsoft.SqlServer.Dts.Runtime.Connections, _  
  ByVal variableDispenser As Microsoft.SqlServer.Dts.Runtime.VariableDispenser, _  
  ByVal componentEvents As Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents, _  
  ByVal log As Microsoft.SqlServer.Dts.Runtime.IDTSLogging, ByVal transaction As Object) _  
  As Microsoft.SqlServer.Dts.Runtime.DTSExecResult  
  
  Try  
    Dim cm As ConnectionManager = connections(Me._connectionManagerName)  
    Dim connection As Object = cm.AcquireConnection(transaction)  
    Return DTSExecResult.Success  
  Catch exception As System.Exception  
    componentEvents.FireError(0, "SampleTask", exception.Message, "", 0)  
    Return DTSExecResult.Failure  
  End Try  
  
End Function  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Connessioni in Integration Services &#40;SSIS&#41;](../../../integration-services/connection-manager/integration-services-ssis-connections.md)   
 [Creare gestioni connessioni](../../connection-manager/integration-services-ssis-connections.md)  
  
