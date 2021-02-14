---
description: Generazione di eventi nell'attività Script
title: Generazione di eventi nell'attività Script | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- events [Integration Services], scripts
- warnings [Integration Services]
- SSIS events, scripts
- errors [Integration Services], events
- SSIS Script task, events
- Events property
- Script task [Integration Services], events
ms.assetid: 21ea07d1-e267-4fb1-a6cc-82c95a39beae
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2dddd04120c80ddd2f80151b2dba22c7079f8790
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351544"
---
# <a name="raising-events-in-the-script-task"></a>Generazione di eventi nell'attività Script

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Gli eventi consentono di segnalare errori, avvisi e altre informazioni, ad esempio l'avanzamento o lo stato delle attività, al pacchetto contenitore. Il pacchetto fornisce gestori eventi per la gestione di notifiche degli eventi. L'attività Script può generare eventi chiamando metodi sulla proprietà <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Events%2A> dell'oggetto **Dts**. Per altre informazioni sulla gestione degli eventi da parte dei pacchetti di [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)], vedere [Gestori eventi di Integration Services &#40;SSIS&#41;](../../../integration-services/integration-services-ssis-event-handlers.md).  
  
 Gli eventi possono essere registrati in qualsiasi provider di log abilitato nel pacchetto. I provider di log archiviano informazioni sugli eventi in un archivio dati. L'attività Script può anche utilizzare il metodo <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Log%2A> per registrare informazioni in un provider di log senza generare un evento. Per altre informazioni sull'uso del metodo <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Log%2A>, vedere [Registrazione nell'attività Script](../../../integration-services/extending-packages-scripting/task/logging-in-the-script-task.md).  
  
 Per generare un evento, l'attività Script chiama uno dei metodi esposti dalla proprietà <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Events%2A>. Nella tabella seguente sono elencati i metodi esposti dalla proprietà <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Events%2A>.  
  
|Event|Descrizione|  
|-----------|-----------------|  
|<xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireCustomEvent%2A>|Genera un evento personalizzato definito dall'utente nel pacchetto.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireError%2A>|Informa il pacchetto di una condizione di errore.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireInformation%2A>|Fornisce informazioni all'utente.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireProgress%2A>|Informa il pacchetto dello stato dell'attività.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireQueryCancel%2A>|Restituisce un valore che indica se il pacchetto richiede che l'attività venga chiusa in modo anomalo.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireWarning%2A>|Informa il pacchetto che l'attività è in uno stato che garantisce la notifica all'utente, ma non è una condizione di errore.|  
  
## <a name="events-example"></a>Esempio di eventi  
 Nell'esempio seguente viene illustrato come generare eventi dall'attività Script. Viene utilizzata una funzione dell'API di Windows nativa per determinare se è disponibile una connessione Internet. Se non sono disponibili connessioni, viene generato un errore. Se è in uso una connessione modem potenzialmente volatile, viene generato un avviso. In caso contrario, viene restituito un messaggio informativo indicante che è stata rilevata una connessione Internet.  
  
```vb  
Private Declare Function InternetGetConnectedState Lib "wininet" _  
    (ByRef dwFlags As Long, ByVal dwReserved As Long) As Long  
  
Private Enum ConnectedStates  
    LAN = &H2  
    Modem = &H1  
    Proxy = &H4  
    Offline = &H20  
    Configured = &H40  
    RasInstalled = &H10  
End Enum  
  
Public Sub Main()  
  
    Dim dwFlags As Long  
    Dim connectedState As Long  
    Dim fireAgain as Boolean  
  
    connectedState = InternetGetConnectedState(dwFlags, 0)  
  
    If connectedState <> 0 Then  
        If (dwFlags And ConnectedStates.Modem) = ConnectedStates.Modem Then  
            Dts.Events.FireWarning(0, "Script Task Example", _  
                "Volatile Internet connection detected.", String.Empty, 0)  
        Else  
            Dts.Events.FireInformation(0, "Script Task Example", _  
                "Internet connection detected.", String.Empty, 0, fireAgain)  
        End If  
    Else  
        ' If not connected to the Internet, raise an error.  
        Dts.Events.FireError(0, "Script Task Example", _  
            "Internet connection not available.", String.Empty, 0)  
    End If  
  
    Dts.TaskResult = ScriptResults.Success  
  
End Sub  
```  
  
```csharp  
using System;  
using System.Data;  
using Microsoft.SqlServer.Dts.Runtime;  
using System.Windows.Forms;  
using System.Runtime.InteropServices;  
  
public class ScriptMain  
{  
  
[DllImport("wininet")]  
        private extern static long InternetGetConnectedState(ref long dwFlags, long dwReserved);  
  
        private enum ConnectedStates  
        {  
            LAN = 0x2,  
            Modem = 0x1,  
            Proxy = 0x4,  
            Offline = 0x20,  
            Configured = 0x40,  
            RasInstalled = 0x10  
        };  
  
        public void Main()  
        {  
            //  
            long dwFlags = 0;  
            long connectedState;  
            bool fireAgain = true;  
            int state;  
  
            connectedState = InternetGetConnectedState(ref dwFlags, 0);  
            state = (int)ConnectedStates.Modem;  
            if (connectedState != 0)  
            {  
                if ((dwFlags & state) == state)  
                {  
                    Dts.Events.FireWarning(0, "Script Task Example", "Volatile Internet connection detected.", String.Empty, 0);  
                }  
                else  
                {  
                    Dts.Events.FireInformation(0, "Script Task Example", "Internet connection detected.", String.Empty, 0, ref fireAgain);  
                }  
            }  
            else  
            {  
                // If not connected to the Internet, raise an error.  
                Dts.Events.FireError(0, "Script Task Example", "Internet connection not available.", String.Empty, 0);  
            }  
  
            Dts.TaskResult = (int)ScriptResults.Success;  
  
        }  
  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Gestori eventi di Integration Services &#40;SSIS&#41;](../../../integration-services/integration-services-ssis-event-handlers.md)   
 [Aggiunta di un gestore eventi a un pacchetto](../../integration-services-ssis-event-handlers.md)  
  
