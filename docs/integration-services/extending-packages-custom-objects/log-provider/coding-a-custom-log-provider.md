---
description: Scrittura del codice di un provider di log personalizzato
title: Scrittura del codice di un provider di log personalizzato | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- custom log providers [Integration Services], coding
ms.assetid: 979a29ca-956e-4fdd-ab47-f06e84cead7a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 93ddf518405b44b5abb6956ee5da063b98ece1e8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348890"
---
# <a name="coding-a-custom-log-provider"></a>Scrittura del codice di un provider di log personalizzato

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Dopo avere creato una classe che eredita dalla classe di base <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase> e avere applicato l'attributo <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute> alla classe, è necessario eseguire l'override dell'implementazione delle proprietà e dei metodi della classe di base per fornire la funzionalità personalizzata.  
  
 Per esempi reali dei provider di log personalizzati, vedere [Sviluppo di un'interfaccia utente per un provider di log personalizzato](../../../integration-services/extending-packages-custom-objects/log-provider/developing-a-user-interface-for-a-custom-log-provider.md).  
  
## <a name="configuring-the-log-provider"></a>Configurazione del provider di log  
  
### <a name="initializing-the-log-provider"></a>Inizializzazione del provider di log  
 Eseguire l'override del metodo <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.InitializeLogProvider%2A> per memorizzare nella cache i riferimenti alla raccolta di connessioni e all'interfaccia degli eventi. È possibile utilizzare questi riferimenti memorizzati nella cache in seguito in altri metodi del provider di log.  
  
### <a name="using-the-configstring-property"></a>Utilizzo della proprietà ConfigString  
 In fase di progettazione un provider di log riceve le informazioni di configurazione dalla colonna **Configurazione**. Tali informazioni corrispondono alla proprietà <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A> del provider di log. Per impostazione predefinita, questa colonna contiene una casella di testo da cui è possibile recuperare qualsiasi informazione in formato stringa. La maggior parte dei provider di log inclusi in [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] utilizza questa proprietà per archiviare il nome della gestione connessione utilizzata per connettersi a un'origine dati esterna. Se il provider di log usa la proprietà <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A>, usare il metodo <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Validate%2A> per convalidarla e verificare che sia impostata correttamente.  
  
### <a name="validating-the-log-provider"></a>Convalida del provider di log  
 Eseguire l'override del metodo <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Validate%2A> per verificare che il provider sia stato configurato correttamente e sia pronto per l'esecuzione. In genere, è richiesto un livello minimo di convalida per verificare che <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A> sia impostato correttamente. L'esecuzione non può continuare finché il provider di log non restituisce <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult.Success> dal metodo <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Validate%2A>.  
  
 Nell'esempio di codice seguente è illustrata un'implementazione di <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Validate%2A> che verifica che sia specificato il nome di una gestione connessione, che la gestione connessione esista nel pacchetto e che restituisca un nome di file nella proprietà <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A>.  
  
```csharp  
public override DTSExecResult Validate(IDTSInfoEvents infoEvents)  
{  
    if (this.ConfigString.Length == 0 || connections.Contains(ConfigString) == false)  
    {  
        infoEvents.FireError(0, "MyTextLogProvider", "The ConnectionManager " + ConfigString + " specified in the ConfigString property cannot be found in the collection.", "", 0);  
        return DTSExecResult.Failure;  
    }  
    else  
    {  
        string fileName = connections[ConfigString].AcquireConnection(null) as string;  
  
        if (fileName == null || fileName.Length == 0)  
        {  
            infoEvents.FireError(0, "MyTextLogProvider", "The ConnectionManager " + ConfigString + " specified in the ConfigString property cannot be found in the collection.", "", 0);  
            return DTSExecResult.Failure;  
        }  
    }  
    return DTSExecResult.Success;  
}  
```  
  
```vb  
Public Overrides Function Validate(ByVal infoEvents As IDTSInfoEvents) As DTSExecResult  
    If Me.ConfigString.Length = 0 Or connections.Contains(ConfigString) = False Then  
        infoEvents.FireError(0, "MyTextLogProvider", "The ConnectionManager " + ConfigString + " specified in the ConfigString property cannot be found in the collection.", "", 0)  
        Return DTSExecResult.Failure  
    Else   
        Dim fileName As String =  connections(ConfigString).AcquireConnectionCType(as string, Nothing)  
  
        If fileName = Nothing Or fileName.Length = 0 Then  
            infoEvents.FireError(0, "MyTextLogProvider", "The ConnectionManager " + ConfigString + " specified in the ConfigString property cannot be found in the collection.", "", 0)  
            Return DTSExecResult.Failure  
        End If  
    End If  
    Return DTSExecResult.Success  
End Function  
```  
  
### <a name="persisting-the-log-provider"></a>Persistenza del provider di log  
 In genere non è necessario implementare la persistenza personalizzata per una gestione connessione. La persistenza personalizzata è richiesta solo quando le proprietà di un oggetto utilizzano tipi di dati complessi. Per altre informazioni, vedere [Sviluppo di oggetti personalizzati per Integration Services](../../../integration-services/extending-packages-custom-objects/developing-custom-objects-for-integration-services.md).  
  
## <a name="logging-with-the-log-provider"></a>Registrazione con il provider di log  
 Tutti i provider di log devono eseguire l'override di tre metodi di runtime: <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A>, <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Log%2A> e <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.CloseLog%2A>.  
  
> [!IMPORTANT]  
>  Durante la convalida e l'esecuzione di un singolo pacchetto, i metodi <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A> e <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.CloseLog%2A> vengono chiamati più di una volta. Verificare che il codice personalizzato non produca la sovrascrittura delle voci precedenti del log le volte successive che il log viene aperto e chiuso. Se è stato selezionato di registrare gli eventi di convalida nel pacchetto di test, il primo evento registrato che dovrebbe apparire è OnPreValidate; se invece il primo evento registrato visualizzato è PackageStart, significa che gli eventi di convalida iniziali sono stati sovrascritti.  
  
### <a name="opening-the-log"></a>Apertura del log  
 La maggior parte dei provider di log si connette a un'origine dati esterna, ad esempio un file o un database, per archiviare le informazioni degli eventi raccolte durante l'esecuzione del pacchetto. Come per qualsiasi altro oggetto nel runtime, la connessione all'origine dati esterna viene in genere stabilita tramite oggetti gestione connessione.  
  
 Il metodo <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A> viene chiamato all'inizio dell'esecuzione del pacchetto. Eseguire l'override di questo metodo per stabilire una connessione con l'origine dati esterna.  
  
 Nel codice di esempio seguente è illustrato un provider di log che apre un file di testo per la scrittura durante <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A>. Il file viene aperto tramite una chiamata al metodo <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.AcquireConnection%2A> della gestione connessione specificata nella proprietà <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A>.  
  
```csharp  
public override void OpenLog()  
{  
    if(!this.connections.Contains(this.ConfigString))  
        throw new Exception("The ConnectionManager " + this.ConfigString + " does not exist in the Connections collection.");  
  
    this.connectionManager = connections[ConfigString];  
    string filePath = this.connectionManager.AcquireConnection(null) as string;  
  
    if(filePath == null || filePath.Length == 0)  
        throw new Exception("The ConnectionManager " + this.ConfigString + " is not a valid FILE ConnectionManager");  
  
    //  Create a StreamWriter to append to.  
    sw = new StreamWriter(filePath,true);  
  
    sw.WriteLine("Open log" + System.DateTime.Now.ToShortTimeString());  
}  
```  
  
```vb  
Public Overrides  Sub OpenLog()  
    If Not Me.connections.Contains(Me.ConfigString) Then  
        Throw New Exception("The ConnectionManager " + Me.ConfigString + " does not exist in the Connections collection.")  
    End If  
  
    Me.connectionManager = connections(ConfigString)  
    Dim filePath As String =  Me.connectionManager.AcquireConnectionCType(as string, Nothing)  
  
    If filePath = Nothing Or filePath.Length = 0 Then  
        Throw New Exception("The ConnectionManager " + Me.ConfigString + " is not a valid FILE ConnectionManager")  
    End If  
  
    '  Create a StreamWriter to append to.  
    sw = New StreamWriter(filePath,True)  
  
    sw.WriteLine("Open log" + System.DateTime.Now.ToShortTimeString())  
End Sub  
```  
  
### <a name="writing-log-entries"></a>Scrittura di voci di log  
 Il metodo <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Log%2A> viene chiamato ogni volta che un oggetto del pacchetto genera un evento chiamando un metodo Fire\<event> in una delle interfacce degli eventi. Ogni evento viene generato con informazioni sul relativo contesto e in genere con un messaggio descrittivo. Tuttavia, non tutte le chiamate al metodo <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Log%2A> includono informazioni per ogni parametro del metodo. Ad esempio, alcuni eventi standard i cui nomi sono autodescrittivi non forniscono MessageText, mentre DataCode e DataBytes vengono utilizzati per fornire informazioni supplementari facoltative.  
  
 Nell'esempio di codice seguente viene implementato il metodo <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Log%2A> e vengono scritti gli eventi nel flusso aperto nella sezione precedente.  
  
```csharp  
public override void Log(string logEntryName, string computerName, string operatorName, string sourceName, string sourceID, string executionID, string messageText, DateTime startTime, DateTime endTime, int dataCode, byte[] dataBytes)  
{  
    sw.Write(logEntryName + ",");  
    sw.Write(computerName + ",");  
    sw.Write(operatorName + ",");  
    sw.Write(sourceName + ",");  
    sw.Write(sourceID + ",");  
    sw.Write(messageText + ",");  
    sw.Write(dataBytes + ",");  
    sw.WriteLine("");  
}  
```  
  
```vb  
Public Overrides  Sub Log(ByVal logEnTryName As String, ByVal computerName As String, ByVal operatorName As String, ByVal sourceName As String, ByVal sourceID As String, ByVal executionID As String, ByVal messageText As String, ByVal startTime As DateTime, ByVal endTime As DateTime, ByVal dataCode As Integer, ByVal dataBytes() As Byte)  
    sw.Write(logEnTryName + ",")  
    sw.Write(computerName + ",")  
    sw.Write(operatorName + ",")  
    sw.Write(sourceName + ",")  
    sw.Write(sourceID + ",")  
    sw.Write(messageText + ",")  
    sw.Write(dataBytes + ",")  
    sw.WriteLine("")  
End Sub  
```  
  
### <a name="closing-the-log"></a>Chiusura del log  
 Il metodo <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.CloseLog%2A> viene chiamato alla fine dell'esecuzione del pacchetto, dopo il completamento dell'esecuzione di tutti i relativi oggetti oppure quando il pacchetto viene arrestato a causa di errori.  
  
 Nell'esempio di codice seguente è illustrata un'implementazione del metodo <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.CloseLog%2A> che chiude il flusso di file aperto durante il metodo <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A>.  
  
```csharp  
public override void CloseLog()  
{  
    if (sw != null)  
    {  
        sw.WriteLine("Close log" + System.DateTime.Now.ToShortTimeString());  
        sw.Close();  
    }  
}  
```  
  
```vb  
Public Overrides  Sub CloseLog()  
    If Not sw Is Nothing Then  
        sw.WriteLine("Close log" + System.DateTime.Now.ToShortTimeString())  
        sw.Close()  
    End If  
End Sub  
```  
 
## <a name="see-also"></a>Vedere anche  
 [Creazione di un provider di log personalizzato](../../../integration-services/extending-packages-custom-objects/log-provider/creating-a-custom-log-provider.md)   
 [Sviluppo di un'interfaccia utente per un provider di log personalizzato](../../../integration-services/extending-packages-custom-objects/log-provider/developing-a-user-interface-for-a-custom-log-provider.md)  
  
  
