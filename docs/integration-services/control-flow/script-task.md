---
description: Attività Script
title: Attività Script | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.scripttask.f1
- sql13.dts.designer.scripttask.general.f1
- sql13.dts.designer.scripttask.script.f1
helpviewer_keywords:
- scripts [Integration Services], tasks
- Script task [Integration Services], about Script task
- Script task [Integration Services]
ms.assetid: f6cce7df-4bd6-4b75-9f89-6c37b4bb5558
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 10be8411e2de29c514c3d9aed50c550a0e86ba91
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348277"
---
# <a name="script-task"></a>Attività Script

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  L'attività Script fornisce il codice necessario per eseguire le funzioni non disponibili nelle trasformazioni e nelle attività predefinite di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Tale attività consente inoltre di combinare più funzioni in un unico script, anziché utilizzare più attività e trasformazioni. L'attività Script può essere utilizzata per operazioni che devono essere eseguite una sola volta in un pacchetto o una sola volta per oggetto enumerato, anziché una volta per ogni riga di dati.  
  
 È possibile utilizzare l'attività Script per gli scopi seguenti:  
  
-   Accedere ai dati tramite tecnologie non supportate dai tipi di connessioni predefiniti. Negli script è ad esempio possibile utilizzare ADSI (Active Directory Service Interfaces) per accedere ad Active Directory ed estrarre i nomi utente.  
  
-   Creare contatori delle prestazioni specifici dei pacchetti. Uno script può ad esempio creare un contatore delle prestazioni che viene aggiornato durante l'esecuzione di un'attività complessa o con prestazioni insufficienti.  
  
-   Determinare il numero delle righe contenute nei file specificati o stabilire se sono vuoti e quindi modificare il flusso di controllo di un pacchetto in base alle informazioni ottenute. Se ad esempio un file non contiene righe, il valore di una determinata variabile verrà impostato su 0 e un vincolo di precedenza che valuta tale valore impedirà all'attività File system di copiare il file.  
  
 Se è necessario utilizzare lo script per eseguire le stesse operazioni per ogni riga di dati in una set, è consigliabile utilizzare il componente script anziché l'attività Script. Se ad esempio si desidera stabilire se l'importo delle spese postali è ragionevole e ignorare le righe di dati che includono importi troppo alti o troppo bassi, è consigliabile utilizzare il componente script. Per altre informazioni, vedere [Componente script](../../integration-services/data-flow/transformations/script-component.md).  
  
 Per gli script utilizzati da più di un pacchetto è preferibile creare un'attività personalizzata, anziché utilizzare l'attività Script. Per altre informazioni, vedere [Sviluppo di un'attività personalizzata](../../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md).  
  
 Una volta stabilito che l'attività Script è la scelta più appropriata per il pacchetto, è necessario sia sviluppare lo script utilizzato dall'attività sia configurare l'attività stessa.  
  
## <a name="writing-and-running-the-script-that-the-task-uses"></a>Scrittura ed esecuzione di script utilizzati dall'attività  
 L'attività Script usa [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA) come ambiente in cui scrivere gli script e come motore in cui eseguirli.  
  
 In VSTA sono disponibili tutte le funzionalità standard dell'ambiente [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], come l'editor di [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] contraddistinto dal colore, la tecnologia IntelliSense ed **Esplora oggetti**. VSTA utilizza anche lo stesso debugger usato da altri strumenti di sviluppo di [!INCLUDE[msCoName](../../includes/msconame-md.md)] . I punti di interruzione degli script si integrano completamente con quelli delle attività e dei contenitori di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . VSTA supporta i linguaggi di programmazione [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic e [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C#.  
  
 Per eseguire uno script, VSTA deve essere pertanto installato nel computer in cui viene eseguito il pacchetto. Durante l'esecuzione di un pacchetto l'attività carica il motore di scripting ed esegue lo script. È possibile accedere ad assembly .NET esterni dagli script aggiungendo i riferimenti a tali assembly nel progetto. Attualmente non sono supportati riferimenti ad assembly .NET Core e .NET Standard.  
  
> [!NOTE]  
>  A differenza delle versioni precedenti in cui era possibile indicare se gli script erano precompilati o meno, in [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] e versioni successive tutti gli script sono precompilati. Se uno script è precompilato, il motore del linguaggio non verrà caricato in fase di esecuzione e il pacchetto verrà eseguito molto più rapidamente. I file binari precompilati occupano tuttavia una notevole quantità di spazio su disco.  
  
## <a name="configuring-the-script-task"></a>Configurazione dell'attività Script  
 Per configurare l'attività Script, procedere nel modo seguente:  
  
-   Specificare lo script personalizzato che deve essere eseguito dall'attività.  
  
-   Nel progetto VSTA, specificare il metodo chiamato dal runtime [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] come punto di ingresso nel codice dell'attività Script.  
  
-   Specificare il linguaggio di scripting.  
  
-   È facoltativamente possibile specificare gli elenchi delle variabili in sola lettura e in lettura e scrittura da utilizzare nello script.  
  
 È possibile impostare queste proprietà tramite Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] o a livello di codice.  
  
### <a name="configuring-the-script-task-in-the-designer"></a>Configurazione dell'attività Script in Progettazione  
 La tabella seguente illustra l'evento **ScriptTaskLogEntry** che può essere registrato per l'attività Script. L'evento **ScriptTaskLogEntry** viene selezionato per essere registrato sulla scheda **Dettagli** della finestra di dialogo **Configura log SSIS** . Per altre informazioni, vedere [registrazione di Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**ScriptTaskLogEntry**|Restituisce i risultati dell'implementazione della registrazione nell'ambito dello script. L'attività scrive una voce di log per ogni chiamata al metodo **Log** dell'oggetto **Dts** . Tali voci vengono scritte dall'attività al momento dell'esecuzione del codice. Per altre informazioni, vedere [Registrazione nell'attività Script](../../integration-services/extending-packages-scripting/task/logging-in-the-script-task.md).|  
  
 Per ulteriori informazioni sulle proprietà che è possibile impostare in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] , vedere gli argomenti seguenti:  
  
-   [Editor attività Script &#40;pagina Generale&#41;]()  
  
-   [Editor attività Script &#40;pagina Script&#41;]()  
  
-   [Pagina Espressioni](../../integration-services/expressions/expressions-page.md)  
  
 Per altre informazioni sull'impostazione di queste proprietà in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] , vedere l'argomento seguente:  
  
-   [Impostazione delle proprietà di un'attività o di un contenitore](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
### <a name="configuring-the-script-task-programmatically"></a>Configurazione dell'attività Script a livello di codice  
 Per ulteriori informazioni sull'impostazione di queste proprietà a livello di codice, vedere l'argomento seguente:  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptTask>  
  
## <a name="script-task-editor-general-page"></a>Editor attività Script (pagina Generale)
  Utilizzare la pagina **Generale** della finestra di dialogo **Editor attività Script** per assegnare un nome e una descrizione all'attività Script.  
  
 Per ulteriori informazioni sull'attività Script, vedere [Script Task](../../integration-services/control-flow/script-task.md) e [Configurazione dell'attività Script nell'editor attività Script](../../integration-services/extending-packages-scripting/task/configuring-the-script-task-in-the-script-task-editor.md). Per informazioni sulla programmazione dell'attività Script, vedere [Extending the Package with the Script Task](../../integration-services/extending-packages-scripting/task/extending-the-package-with-the-script-task.md).  
  
### <a name="options"></a>Opzioni  
 **Nome**  
 Consente di digitare un nome univoco per l'attività Script. Tale nome viene utilizzato come etichetta nell'icona dell'attività.  
  
> [!NOTE]  
>  I nomi delle attività devono essere univoci all'interno di un pacchetto.  
  
 **Descrizione**  
 Consente di digitare una descrizione dell'attività Script.  
  
## <a name="script-task-editor-script-page"></a>Editor attività Script (pagina Script)
  Utilizzare la pagina **Script** della finestra di dialogo **Editor attività Script** per impostare le proprietà dello script e specificare le variabili accessibili per lo script.  
  
> [!NOTE]  
>  In [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] e versioni successive, tutti gli script sono precompilati. Nelle versioni precedenti, si imposta una proprietà **PrecompileScriptIntoBinaryCode** per specificare che lo script è stato precompilato.  
  
 Per ulteriori informazioni sull'attività Script, vedere [Script Task](../../integration-services/control-flow/script-task.md) e [Configurazione dell'attività Script nell'editor attività Script](../../integration-services/extending-packages-scripting/task/configuring-the-script-task-in-the-script-task-editor.md). Per informazioni sulla programmazione dell'attività Script, vedere [Extending the Package with the Script Task](../../integration-services/extending-packages-scripting/task/extending-the-package-with-the-script-task.md).  
  
### <a name="options"></a>Opzioni  
 **ScriptLanguage**  
 Selezionare il linguaggio di scripting per l'attività, [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic o [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C#.  
  
 Dopo avere creato uno script per l'attività, non è possibile modificare il valore della proprietà **ScriptLanguage** .  
  
 Per impostare il linguaggio di scripting predefinito per l'attività Script, utilizzare l'opzione **Linguaggio di scripting** nella pagina **Generale** della finestra di dialogo **Opzioni** . Per ulteriori informazioni, vedere [General Page]().  
  
 **EntryPoint**  
 Specificare il metodo chiamato dal runtime [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] come punto di ingresso nel codice dell'attività Script. Il metodo specificato deve essere nella classe ScriptMain del progetto [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA). ScriptMain è la classe predefinita generata dai modelli di script.  
  
 Se si modifica il nome del metodo nel progetto VSTA, è necessario modificare il valore della proprietà **EntryPoint** .  
  
 **ReadOnlyVariables**  
 Digitare un elenco delimitato da virgole di variabili di sola lettura disponibili per lo script oppure fare clic sul pulsante con i puntini di sospensione (**...**) e selezionare le variabili nella finestra di dialogo **Seleziona variabili**.  
  
> [!NOTE]  
>  Per i nomi delle variabili viene fatta distinzione tra maiuscole e minuscole.  
  
 **ReadWriteVariables**  
 Digitare un elenco delimitato da virgole di variabili di lettura/scrittura disponibili per lo script oppure fare clic sul pulsante con i puntini di sospensione (**...**) e selezionare le variabili nella finestra di dialogo **Seleziona variabili**.  
  
> [!NOTE]  
>  Per i nomi delle variabili viene fatta distinzione tra maiuscole e minuscole.  
  
 **Modifica script**  
 Apre VSTA IDE, dove è possibile creare o modificare lo script.  
  
## <a name="related-content"></a>Contenuto correlato  
  
-   Articolo tecnico relativo all' [invio della posta elettronica con notifica di recapito in C#](https://go.microsoft.com/fwlink/?LinkId=237625)su shareourideas.com  
  
