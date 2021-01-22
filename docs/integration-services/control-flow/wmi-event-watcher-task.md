---
description: Attività Monitoraggio eventi WMI
title: Attività Monitoraggio eventi WMI | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.wmieventwatchertask.f1
- sql13.dts.designer.wmieventwatcher.general.f1
- sql13.dts.designer.wmieventwatcher.wmiquery.f1
helpviewer_keywords:
- WQL [Integration Services]
- WMI Event Watcher task [Integration Services]
ms.assetid: b5bb52e9-a77e-41e1-93f9-d4c3bc6b2c9a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8b4e04871bf0264c9e9c723365f2a29d95265ffa
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98594930"
---
# <a name="wmi-event-watcher-task"></a>Attività Monitoraggio eventi WMI

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  L'attività Monitoraggio eventi WMI consente di monitorare gli eventi di WMI (Windows Management Instrumentation, Strumentazione gestione Windows) utilizzando una query WQL (Management Instrumentation Query Language) per specificare gli eventi desiderati. È possibile utilizzare l'attività Monitoraggio eventi WMI per gli scopi seguenti:  
  
-   Attendere la notifica dell'aggiunta di file a una cartella e quindi avviare l'elaborazione dei file.  
  
-   Eseguire un pacchetto che elimina file quando la quantità di memoria disponibile in un server è inferiore a una percentuale specificata.  
  
-   Monitorare l'installazione di un'applicazione e quindi eseguire un pacchetto che la utilizza.  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] include un'attività che consente di leggere le informazioni di WMI.  
  
 Per ulteriori informazioni su questa attività, fare clic sull'argomento seguente:  
  
-   [Attività Lettore di dati WMI](../../integration-services/control-flow/wmi-data-reader-task.md)  
  
## <a name="wql-queries"></a>Query WQL  
 WQL è un sottolinguaggio di SQL che include estensioni per supportare la notifica degli eventi WMI e altre caratteristiche specifiche di WMI. Per altre informazioni su WQL, vedere la documentazione di Strumentazione gestione Windows in [MSDN Library](/documentation/).  
  
> [!NOTE]  
>  Le classi WMI variano a seconda della versione di Windows.  
  
 La query seguente esegue il monitoraggio di una notifica che segnala che l'utilizzo della CPU è superiore al 40%.  
  
```  
SELECT * from __InstanceModificationEvent WITHIN 2 WHERE TargetInstance ISA 'Win32_Processor' and TargetInstance.LoadPercentage > 40  
```  
  
 La query seguente esegue il monitoraggio di una notifica che segnala che un file è stato aggiunto a una cartella.  
  
```  
SELECT * FROM __InstanceCreationEvent WITHIN 10 WHERE TargetInstance ISA "CIM_DirectoryContainsFile" and TargetInstance.GroupComponent= "Win32_Directory.Name=\"c:\\\\WMIFileWatcher\""   
```  
  
## <a name="custom-logging-messages-available-on-the-wmi-event-watcher-task"></a>Messaggi di registrazione personalizzati disponibili nell'attività Monitoraggio eventi WMI  
 Nella tabella seguente sono elencate le voci di log personalizzate disponibili per l'attività Monitoraggio eventi WMI. Per altre informazioni, vedere [registrazione di Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**WMIEventWatcherEventOccurred**|Indica che si è verificato un evento monitorato dall'attività.|  
|**WMIEventWatcherTimedout**|Indica che si è verificato il timeout dell'attività.|  
|**WMIEventWatcherWatchingForWMIEvents**|Indica che l'attività ha iniziato a eseguire la query WQL. La voce include la query.|  
  
## <a name="configuration-of-the-wmi-event-watcher-task"></a>Configurazione dell'attività Monitoraggio eventi WMI  
 Per configurare l'attività Lettore di dati WMI, procedere nel modo seguente:  
  
-   Specificare la gestione connessione WMI da utilizzare.  
  
-   Specificare l'origine della query WQL. È possibile utilizzare query con origine esterna all'attività, una variabile o un file, oppure archiviate in una proprietà dell'attività.  
  
-   Specificare l'operazione che deve essere eseguita dall'attività quando si verifica l'evento WMI. È possibile registrare la notifica dell'evento e lo stato dopo l'evento oppure generare eventi di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] personalizzati che forniscono informazioni associate all'evento WMI, la notifica e lo stato dopo l'evento.  
  
-   Definire la risposta dell'attività all'evento. È possibile configurare l'attività in modo da riuscire o non riuscire, a seconda dell'evento, oppure da riprendere semplicemente il monitoraggio dell'evento.  
  
-   Specificare l'operazione che deve essere eseguita dall'attività al timeout della query WQL. È possibile registrare il timeout e lo stato dopo il timeout oppure generare un evento di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] personalizzato che indica che si è verificato il timeout dell'evento WMI e registra il timeout e lo stato dopo il timeout.  
  
-   Definire la risposta dell'attività al timeout. È possibile configurare l'attività in modo da riuscire o non riuscire oppure da riprendere semplicemente il monitoraggio dell'evento.  
  
-   Specificare il numero di volte per cui monitorare l'evento.  
  
-   Specificare il timeout.  
  
 Se l'origine è un file, l'attività Monitoraggio eventi WMI utilizzerà una gestione connessione file per connettersi al file. Per ulteriori informazioni, vedere [Flat File Connection Manager](../../integration-services/connection-manager/flat-file-connection-manager.md).  
  
 L'attività Monitoraggio eventi WMI utilizza una gestione connessione WMI per connettersi al server da cui legge le informazioni di WMI. Per altre informazioni, vedere [Gestione connessione WMI](../../integration-services/connection-manager/wmi-connection-manager.md).  
  
 È possibile impostare le proprietà tramite Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] o a livello di codice.  
  
 Per ulteriori informazioni sulle proprietà che è possibile impostare in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] , fare clic sull'argomento seguente:  
  
-   [Pagina Espressioni](../../integration-services/expressions/expressions-page.md)  
  
 Per altre informazioni sull'impostazione di queste proprietà in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] , fare clic sull'argomento seguente:  
  
-   [Impostazione delle proprietà di un'attività o di un contenitore](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="programmatic-configuration-of-the-wmi-event-watcher-task"></a>Configurazione a livello di codice dell'attività Monitoraggio eventi WMI  
 Per ulteriori informazioni sull'impostazione di queste proprietà a livello di codice, fare clic sull'argomento seguente:  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.WmiEventWatcherTask.WmiEventWatcherTask>  
  
## <a name="wmi-event-watcher-task-editor-general-page"></a>Editor attività Monitoraggio eventi WMI (pagina Generale)
  Utilizzare la pagina **Generale** della finestra di dialogo **Editor attività Monitoraggio eventi WMI** per specificare un nome e una descrizione per l'attività Monitoraggio eventi WMI.  
  
 Per altre informazioni su WQL (WMI Query Language), vedere l'argomento relativo a WMI (Windows Management Instrumentation) [Query con WQL](/windows/win32/wmisdk/querying-with-wql)in MSDN Library.  
  
### <a name="options"></a>Opzioni  
 **Nome**  
 Consente di specificare un nome univoco per l'attività Monitoraggio eventi WMI. Tale nome viene utilizzato come etichetta nell'icona dell'attività.  
  
> [!NOTE]  
>  I nomi delle attività devono essere univoci all'interno di un pacchetto.  
  
 **Descrizione**  
 Consente di digitare una descrizione dell'attività Monitoraggio eventi WMI.  
  
## <a name="wmi-event-watcher-task-editor-wmi-options-page"></a>Editor attività Monitoraggio eventi WMI (pagina Opzioni WMI)
  Usare la pagina **Opzioni WMI** della finestra di dialogo **Editor attività Monitoraggio eventi WM** per specificare l'origine della query WQL e la modalità di risposta dell'attività Monitoraggio eventi WMI agli eventi del servizio Strumentazione Gestione Windows (WMI).  
  
 Per altre informazioni su WQL (WMI Query Language), vedere l'argomento relativo a WMI (Windows Management Instrumentation) [Query con WQL](/windows/win32/wmisdk/querying-with-wql)in MSDN Library.  
  
### <a name="static-options"></a>Opzioni statiche  
 **WMIConnectionName**  
 Selezionare una gestione connessione WMI nell'elenco oppure fare clic su \<**New WMI Connection...**> per creare una nuova gestione connessione.  
  
 **Argomenti correlati:** [Gestione connessione WMI](../../integration-services/connection-manager/wmi-connection-manager.md), [Editor gestione connessione WMI](../connection-manager/wmi-connection-manager.md)  
  
 **WQLQuerySourceType**  
 Consente di selezionare il tipo di origine della query WQL eseguita dall'attività. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Input diretto**|Consente di impostare l'origine su una query WQL. Selezionando questo valore viene visualizzata l'opzione dinamica **WQLQuerySource**.|  
|**Connessione file**|Consente di selezionare un file contenente la query WQL. Selezionando questo valore viene visualizzata l'opzione dinamica **WQLQuerySource**.|  
|**Variabile**|Consente di impostare l'origine su una variabile che definisce la query WQL. Selezionando questo valore viene visualizzata l'opzione dinamica **WQLQuerySource**.|  
  
 **ActionAtEvent**  
 Consente di specificare se l'evento WMI registra l'evento e inizia un'azione [!INCLUDE[ssIS](../../includes/ssis-md.md)] oppure se si limita a registrare l'evento.  
  
 **AfterEvent**  
 Consente di specificare se l'attività ha esito positivo o negativo dopo avere ricevuto l'evento WMI oppure se deve continuare a monitorare l'evento per controllare se viene generato nuovamente.  
  
 **ActionAtTimeout**  
 Consente di specificare se l'attività registra un timeout della query WMI e inizia un evento [!INCLUDE[ssIS](../../includes/ssis-md.md)] in risposta oppure se si limita a registrare il timeout.  
  
 **AfterTimeout**  
 Consente di specificare se l'attività ha esito positivo o negativo in risposta a un timeout oppure se deve continuare il monitoraggio per controllare se viene generato un altro timeout.  
  
 **NumberOfEvents**  
 Consente di specificare il numero di eventi da monitorare.  
  
 **Timeout**  
 Consente di specificare il numero di secondi di attesa per la generazione dell'evento. Un valore pari a 0 significa che non è attivo alcun timeout.  
  
### <a name="wqlquerysource-dynamic-options"></a>Opzioni dinamiche di WQLQuerySource  
  
#### <a name="wqlquerysource--direct-input"></a>WQLQuerySource = Input diretto  
 **WQLQuerySource**  
 Consente di specificare una query o di immettere una query nella finestra di dialogo **Query WQL** visualizzata facendo clic sul pulsante (...).  
  
#### <a name="wqlquerysource--file-connection"></a>WQLQuerySource = Connessione file  
 **WQLQuerySource**  
 Selezionare una gestione connessione file nell'elenco oppure fare clic su \<**New connection...**> per creare una nuova gestione connessione.  
  
 **Argomenti correlati:** [Gestione connessione file](../../integration-services/connection-manager/file-connection-manager.md), [Editor gestione connessione file](../connection-manager/file-connection-manager.md)  
  
#### <a name="wqlquerysource--variable"></a>WQLQuerySource = Variabile  
 **WQLQuerySource**  
 Selezionare una variabile nell'elenco oppure fare clic su \<**New variable...**> per crearne una nuova.  
  
 **Argomenti correlati:** [Variabili di Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Aggiungere una variabile](../integration-services-ssis-variables.md)