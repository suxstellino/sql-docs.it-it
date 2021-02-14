---
description: Attività Esegui processo
title: Attività Esegui processo | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.executeprocesstask.f1
- sql13.dts.designer.executeprocesstask.general.f1
- sql13.dts.designer.executeprocesstask.process.f1
helpviewer_keywords:
- Execute Process task [Integration Services]
ms.assetid: aca5a0b5-34a9-45bc-a234-8e63ea51a1ee
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 98409c2a75cc76589a705579ff02f1be34c9c7b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352109"
---
# <a name="execute-process-task"></a>Attività Esegui processo

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  L'attività Esegui processo consente di eseguire un'applicazione o un file batch come parte del flusso di lavoro di un pacchetto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Sebbene sia possibile usarla per aprire qualsiasi applicazione standard, ad esempio [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] o [!INCLUDE[ofprword](../../includes/ofprword-md.md)], l'attività Esegui processo viene in genere usata per eseguire applicazioni aziendali o file batch che usano un'origine dei dati. È ad esempio possibile utilizzare l'attività Esegui processo per espandere un file di testo compresso. Il pacchetto può quindi utilizzare il file di testo come origine dei dati per il proprio flusso di dati. Sempre a titolo di esempio, è anche possibile utilizzare tale attività per eseguire un'applicazione [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] personalizzata che genera un report giornaliero sulle vendite, che può essere allegato a un'attività Invia messaggi e inoltrato a una lista di distribuzione.  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] include altre attività che consentono l'esecuzione di operazioni di flusso di lavoro, quale l'esecuzione di pacchetti. Per altre informazioni, vedere [Attività Esegui pacchetto](../../integration-services/control-flow/execute-package-task.md)  
  
## <a name="custom-log-entries-available-on-the-execute-process-task"></a>Voci di log personalizzate disponibili nell'attività Esegui processo  
 Nella tabella seguente sono elencate le voci di log personalizzate disponibili per l'attività Esegui processo. Per altre informazioni, vedere [registrazione di Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**ExecuteProcessExecutingProcess**|Fornisce informazioni sui processi che l'attività dovrà eseguire.<br /><br /> Vengono scritte due voci di log. Una contiene informazioni sul nome e sulla posizione del file eseguibile eseguito dall'attività, l'altra registra l'uscita dal file eseguibile.|  
|**ExecuteProcessVariableRouting**|Fornisce informazioni sulle variabili indirizzate all'input e agli output del file eseguibile. Vengono scritte voci di log per stdin (l'input), stdout (l'output) e stderr (l'output degli errori).|  
  
## <a name="configuration-of-the-execute-process-task"></a>Configurazione dell'attività Esegui processo  
 È possibile impostare le proprietà tramite Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] o a livello di codice.  
  
 Per altre informazioni sull'impostazione di queste proprietà in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] , fare clic sull'argomento seguente:  
  
-   [Impostazione delle proprietà di un'attività o di un contenitore](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
### <a name="property-settings"></a>Impostazioni delle proprietà  
 Quando l'attività Esegui processo esegue un'applicazione personalizzata, fornisce dati di input all'applicazione tramite uno o entrambi i metodi seguenti:  
  
-   Una variabile specificata nell'impostazione della proprietà **StandardInputVariable** . Per altre informazioni sulle variabili, vedere [Variabili di Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md) e [Utilizzo di variabili nei pacchetti](../integration-services-ssis-variables.md).  
  
-   Un argomento specificato nell'impostazione della proprietà **Arguments** . Se, ad esempio, l'attività apre un documento in Word, l'argomento può assegnare un nome al file con estensione doc.  
  
 Per passare più argomenti a un'applicazione personalizzata in un'attività Esegui processo, utilizzare gli spazi per delimitarli. Un argomento non può includere uno spazio. In caso contrario, l'attività non verrà eseguita. È possibile utilizzare un'espressione per passare il valore di una variabile come argomento. Nell'esempio seguente l'espressione passa due valori di variabili come argomenti e utilizza uno spazio per delimitare gli argomenti:  
  
 `@variable1 + " " + @variable2`  
  
 È possibile utilizzare un'espressione per impostare varie proprietà dell'attività Esegui processo.  
  
 Quando si usa la proprietà **StandardInputVariable** per configurare l'attività Esegui processo in modo che fornisca dati di input, chiamare il metodo **Console.ReadLine** dall'applicazione per leggere l'input. Per altre informazioni, vedere l'argomento [Metodo Console.ReadLine](/dotnet/api/system.console.readline) nella libreria di classi di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)].  
  
 Quando si usa la proprietà **Arguments** per configurare l'attività Esegui processo in modo che fornisca dati di input, effettuare uno dei passaggi seguenti per ottenere gli argomenti:  
  
-   Se si usa Microsoft Visual Basic per scrivere l'applicazione, impostare la proprietà **My.Application.CommandLineArgs** . Nell'esempio seguente viene impostata la proprietà **My.Application.CommandLineArgs** per recuperare due argomenti:  
  
    ```vb  
    Dim variable1 As String = My.Application.CommandLineArgs.Item(0)  
    Dim variable2 As String = My.Application.CommandLineArgs.Item(1)   
    ```  
  
     Per altre informazioni, vedere l'argomento [Proprietà My.Application.CommandLineArgs](https://go.microsoft.com/fwlink/?LinkId=129200)nei riferimenti di [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] .  
  
-   Se si usa Microsoft Visual C# per scrivere l'applicazione, usare il metodo **Main** .  
  
     Per altre informazioni, vedere l'argomento [Argomenti della riga di comando (Guida per programmatori C#)](/dotnet/csharp/programming-guide/main-and-command-args/command-line-arguments)nella Guida per programmatori C#.  
  
 L'attività Esegui processo include inoltre le proprietà **StandardOutputVariable** e **StandardErrorVariable** che consentono di specificare le variabili che usano rispettivamente l'output standard e l'output degli errori dell'applicazione.  
  
 È inoltre possibile configurare l'attività Esegui processo specificando una directory di lavoro, un periodo di timeout o un valore che indica se l'esecuzione del file eseguibile è stata completata. L'attività può essere configurata anche in modo da non riuscire se il codice restituito dal file eseguibile non corrisponde al valore che indica il completamento dell'esecuzione oppure se il file eseguibile non viene trovato nel percorso specificato.  
  
## <a name="programmatic-configuration-of-the-execute-process-task"></a>Configurazione a livello di codice dell'attività Esegui processo  
 Per ulteriori informazioni sull'impostazione di queste proprietà a livello di codice, fare clic sull'argomento seguente:  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.ExecuteProcess.ExecuteProcess>  
  
## <a name="execute-process-task-editor-general-page"></a>Editor attività Esegui processo (pagina Generale)
  Usare la pagina **Generale** della finestra di dialogo **Editor attività Esegui processo** per assegnare un nome e una descrizione all'attività Esegui processo.  
  
### <a name="options"></a>Opzioni  
 **Nome**  
 Consente di specificare un nome univoco per l'attività Esegui processo. Tale nome viene utilizzato come etichetta nell'icona dell'attività.  
  
> [!NOTE]  
>  I nomi delle attività devono essere univoci all'interno di un pacchetto.  
  
 **Descrizione**  
 Consente di digitare una descrizione dell'attività Esegui processo.  
  
## <a name="execute-process-task-editor-process-page"></a>Execute Process Task Editor (Process Page)
  Utilizzare la pagina **Processo** della finestra di dialogo **Editor attività Esegui processo** per configurare le opzioni di esecuzione del processo. Tali opzioni includono il file eseguibile da avviare, il relativo percorso, gli argomenti del prompt dei comandi, nonché le variabili per la generazione dell'input e l'acquisizione dell'output.  
  
### <a name="options"></a>Opzioni  
 **RequireFullFileName**  
 Consente di indicare se l'attività deve avere esito negativo nel caso in cui il file eseguibile non venga trovato nel percorso specificato.  
  
 **File eseguibile**  
 Consente di digitare il nome del file eseguibile da avviare.  
  
 **Argomenti**  
 Consente di specificare gli argomenti del prompt dei comandi.  
  
 **WorkingDirectory**  
 Digitare il percorso della cartella contenente il file eseguibile oppure fare clic sul pulsante sfoglia **(...)** per individuare la cartella.  
  
 **StandardInputVariable**  
 Consente di selezionare una variabile per l'invio dell'input al processo. È anche possibile fare clic su \<**New variable...**> per crearne una nuova:  
  
 **Argomenti correlati:** [Aggiungi variabile](../integration-services-ssis-variables.md)  
  
 **StandardOutputVariable**  
 Consente di selezionare una variabile per l'acquisizione dell'output del processo. È anche possibile fare clic su \<**New variable...**> per crearne una nuova.  
  
 **StandardErrorVariable**  
 Consente di selezionare una variabile per l'acquisizione dell'output degli errori del processore. È anche possibile fare clic su \<**New variable...**> per crearne una nuova.  
  
 **FailTaskIfReturnCodeIsNotSuccessValue**  
 Consente di indicare se l'attività deve avere esito negativo nel caso in cui il codice di uscita del processo non corrisponda al valore specificato in **SuccessValue**.  
  
 **SuccessValue**  
 Consente di specificare il valore restituito dal file eseguibile per segnalare l'esito positivo. Per impostazione predefinita, questo valore è impostato su **0**.  
  
 **TimeOut**  
 Consente di specificare il numero di secondi di esecuzione del processo. Il valore **0** indica che non è previsto alcun timeout e il processo viene eseguito fino al completamento o finché non si verifica un errore.  
  
 **TerminateProcessAfterTimeOut**  
 Consente di indicare se il processo deve essere terminato allo scadere del periodo di timeout specificato nell'opzione **TimeOut** . Questa opzione è disponibile solo se **TimeOut** non è impostata su **0**.  
  
 **WindowStyle**  
 Consente di specificare lo stile della finestra in cui viene eseguito il processo.  
  
## <a name="see-also"></a>Vedere anche  
 [Attività di Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Flusso di controllo](../../integration-services/control-flow/control-flow.md)  
  
