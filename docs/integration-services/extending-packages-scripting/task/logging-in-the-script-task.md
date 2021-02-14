---
description: Registrazione nell'attività Script
title: Registrazione nell'attività Script | Microsoft Docs
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
- SQL Server Integration Services packages, logs
- SSIS packages, logs
- logs [Integration Services], scripts
- Integration Services packages, logs
- Log method
- SSIS Script task, logs
- Script task [Integration Services], logs
- packages [Integration Services], logs
ms.assetid: 2e11fc15-df18-4309-bd2d-fc58aa4b9b7a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2ea76691d3fb5cf6f3fcdd4ba5043d9af8b0484b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351548"
---
# <a name="logging-in-the-script-task"></a>Registrazione nell'attività Script

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  L'utilizzo della registrazione nei pacchetti di [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] consente di registrare informazioni dettagliate su stato di esecuzione, risultati e problemi, tramite la registrazione di eventi predefiniti o di messaggi definiti dall'utente da analizzare in un secondo momento. L'attività Script può usare il metodo <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Log%2A> dell'oggetto **Dts** per registrare dati definiti dall'utente. Se la registrazione è abilitata e l'evento **ScriptTaskLogEntry** è selezionato per la registrazione nella scheda **Dettagli** della finestra di dialogo **Configura log SSIS**, una singola chiamata al metodo <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Log%2A> archivia le informazioni dell'evento in tutti i provider di log configurati per l'attività.  
  
> [!NOTE]  
>  Anche se è possibile eseguire direttamente la registrazione dall'attività Script, è consigliabile implementare eventi anziché registrare. Quando si utilizzano gli eventi, oltre ad abilitare la registrazione dei messaggi di eventi, è anche possibile rispondere all'evento con gestori eventi predefiniti o definiti dall'utente.  
  
 Per altre informazioni sulla registrazione, vedere [Registrazione di Integration Services &#40;SSIS&#41;](../../../integration-services/performance/integration-services-ssis-logging.md).  
  
## <a name="logging-example"></a>Esempio di registrazione  
 Nell'esempio seguente è illustrata la registrazione dall'attività Script registrando un valore che rappresenta il numero di righe elaborate.  
  
```vb  
Public Sub Main()  
  
    Dim rowsProcessed As Integer = 100  
    Dim emptyBytes(0) As Byte  
  
    Try  
        Dts.Log("Rows processed: " & rowsProcessed.ToString, _  
            0, _  
            emptyBytes)  
        Dts.TaskResult = ScriptResults.Success  
    Catch ex As Exception  
        'An error occurred.  
        Dts.Events.FireError(0, "Script Task Example", _  
            ex.Message & ControlChars.CrLf & ex.StackTrace, _  
            String.Empty, 0)  
        Dts.TaskResult = ScriptResults.Failure  
    End Try  
  
End Sub  
```  
  
```csharp  
using System;  
using System.Data;  
using Microsoft.SqlServer.Dts.Runtime;  
  
public class ScriptMain  
{  
  
    public void Main()  
        {  
            //  
            int rowsProcessed = 100;  
            byte[] emptyBytes = new byte[0];  
  
            try  
            {  
                Dts.Log("Rows processed: " + rowsProcessed.ToString(), 0, emptyBytes);  
                Dts.TaskResult = (int)ScriptResults.Success;  
            }  
            catch (Exception ex)  
            {  
                //An error occurred.  
                Dts.Events.FireError(0, "Script Task Example", ex.Message + "\r" + ex.StackTrace, String.Empty, 0);  
                Dts.TaskResult = (int)ScriptResults.Failure;  
            }  
  
        }  
```  
  
 }  
  
## <a name="see-also"></a>Vedere anche  
 [Registrazione di Integration Services &#40;SSIS&#41;](../../../integration-services/performance/integration-services-ssis-logging.md)  
  
  
