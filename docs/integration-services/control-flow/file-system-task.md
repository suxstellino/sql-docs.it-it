---
description: Attività File system
title: Attività File system | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.filesystemtask.f1
- sql13.dts.designer.filesystemtask.general.f1
helpviewer_keywords:
- File System task [Integration Services]
ms.assetid: 7dd79a6a-e066-4028-a385-1d40f31056f8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d75ea560e7e3d186013664ee89e66b956e0405fa
ms.sourcegitcommit: 0b37eb7aef2f358f80867cd13830dd6683da8d85
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/30/2021
ms.locfileid: "105980969"
---
# <a name="file-system-task"></a>Attività File system

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  L'attività File system consente di eseguire operazioni su file e directory nel file system. Tramite l'attività File system un pacchetto può ad esempio creare, spostare o eliminare file e directory. È inoltre possibile utilizzare l'attività File system per impostare attributi su file e directory, ad esempio per impostare file come nascosti o in sola lettura.  
  
 Tutte le operazioni dell'attività File system utilizzano un'origine, che può essere costituita da un file o una directory. Il file copiato o la directory eliminata tramite un'attività è un'origine. L'origine può essere specificata tramite una gestione connessione file che punta alla directory o al file oppure specificando il nome di una variabile che contiene il percorso dell'origine. Per altre informazioni, vedere [Gestione connessione File](../../integration-services/connection-manager/file-connection-manager.md) e [Variabili di Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md).  
  
 Le operazioni che prevedono la copia e lo spostamento di file e directory oppure la ridenominazione di file utilizzano un'origine e una destinazione. La destinazione viene specificata utilizzando una gestione connessione file o una variabile. Le operazioni dell'attività File system possono essere configurate in modo da consentire la sovrascrittura dei file e delle directory di destinazione. L'operazione con cui viene creata una nuova directory può essere configurata in modo da utilizzare una directory esistente con il nome specificato anziché avere esito negativo se la directory esiste già.  
  
## <a name="predefined-file-system-operations"></a>Operazioni predefinite dell'attività File system  
 L'attività File system include un set predefinito di operazioni, descritte nella tabella seguente.  
  
|Operazione|Descrizione|  
|---------------|-----------------|  
|Copia directory|Copia una cartella da un percorso a un altro.|  
|Copia file|Copia un file da un percorso a un altro.|  
|Creare la directory|Crea una cartella in un percorso specificato.|  
|Elimina directory|Elimina una cartella in un percorso specificato.|  
|Elimina contenuto directory|Elimina tutti i file e le cartelle contenute in una cartella.|  
|Elimina file|Elimina un file in un percorso specificato.|  
|Sposta directory|Sposta una cartella da un percorso a un altro.|  
|Sposta file|Sposta un file da un percorso a un altro.|  
|Rinomina file|Rinomina un file in un percorso specificato.|  
|Impostare gli attributi|Imposta attributi su file e cartelle. Tali attributi includono Archive, Hidden, Normal, ReadOnly e System. Normal indica la mancanza di attributi e non può essere combinato con altri attributi. Tutti gli altri attributi possono essere utilizzati in combinazione.|  
  
 L'attività File system opera su un singolo file o directory. Pertanto, questa attività non supporta l'utilizzo di caratteri jolly per eseguire la stessa operazione su più file. Affinché l'attività File system ripeta un'operazione su più file o directory, inserire l'attività File system in un contenitore Ciclo Foreach, come descritto nella procedura seguente:  
  
-   **Configurare il contenitore Ciclo Foreach** Nella pagina **Raccolta** dell'Editor ciclo Foreach, impostare l'enumeratore su **Enumeratore Foreach File** e immettere l'espressione con caratteri jolly come configurazione dell'enumeratore per **File**. Nella pagina **Mapping variabili** dell'Editor ciclo Foreach, eseguire il mapping di una variabile che si desidera usare per passare uno alla volta i nomi dei file all'attività File System.  
  
-   **Aggiungere e configurare un'attività File System** Aggiungere un'attività File System al contenitore Ciclo Foreach. Nella pagina **Generale** dell'Editor attività File system, impostare la proprietà **SourceVariable** o **DestinationVariable** sulla variabile definita nel contenitore Ciclo Foreach.  
  
## <a name="custom-log-entries-available-on-the-file-system-task"></a>Voci di log personalizzate disponibili nell'attività File System  
 Nella tabella seguente sono elencate le voci di log personalizzate disponibili per l'attività File system. Per altre informazioni, vedere [registrazione di Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**FileSystemOperation**|Indica l'operazione eseguita dall'attività. Questa voce di log viene scritta all'inizio dell'operazione sul file system e include informazioni sull'origine e sulla destinazione.|  
  
## <a name="configuring-the-file-system-task"></a>Configurazione dell'attività File system  
 È possibile impostare le proprietà tramite Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] o a livello di codice.  
  
 Per ulteriori informazioni sulle proprietà che è possibile impostare in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] , vedere gli argomenti seguenti:  
  
-   [Editor attività File system &#40;pagina Generale&#41;]()  
  
-   [Pagina Espressioni](../../integration-services/expressions/expressions-page.md)  
  
 Per altre informazioni sull'impostazione di queste proprietà in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] , vedere l'argomento seguente:  
  
-   [Impostazione delle proprietà di un'attività o di un contenitore](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
 Per ulteriori informazioni sull'impostazione di queste proprietà a livello di codice, vedere l'argomento seguente:  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask>  
  
## <a name="related-tasks"></a>Attività correlate  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] include un'attività che consente di eseguire il download e il caricamento di file di dati, nonché di gestire directory sui server. Per altre informazioni, vedere [Attività FTP](../../integration-services/control-flow/ftp-task.md).  
  
## <a name="file-system-task-editor-general-page"></a>Editor attività File system (pagina Generale)
  Usare la pagina **Generale** della finestra di dialogo **Editor attività File system** per configurare l'operazione di file system eseguita dall'attività.  
  
 È necessario specificare una gestione connessione di origine e di destinazione impostando le proprietà SourceConnection e DestinationConnection. È possibile specificare i nomi delle gestioni connessione file che puntano ai file utilizzati dall'attività come origine o come destinazione oppure, se i percorsi dei file sono archiviati in variabili, è possibile specificare i nomi delle variabili. Per usare variabili per l'archiviazione dei percorsi dei file, è innanzitutto necessario impostare su **True** l'opzione IsSourcePathVariable per la connessione di origine e l'opzione IsDestinationPathVariable per la connessione di destinazione. È quindi possibile scegliere quali variabili utilizzare tra le variabili di sistema o definite dall'utente esistenti, oppure creare nuove variabili. Nella finestra di dialogo **Aggiungi variabile** è possibile configurare e specificare l'ambito delle variabili. L'ambito deve essere l'attività File system o un contenitore padre. Per altre informazioni, vedere [Variabili di Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md) e [Utilizzo di variabili nei pacchetti](../integration-services-ssis-variables.md).  
  
> [!NOTE]  
>  Per eseguire l'override delle variabili selezionate per le proprietà **SourceConnection** e **DestinationConnection** , immettere un'espressione per le proprietà **Source** e **Destination** . Le espressioni devono essere immesse nella pagina **Espressioni** di **Editor attività File system**. Ad esempio, per impostare il percorso dei file utilizzati dall'attività come destinazione, potrebbe essere necessario utilizzare la variabile A in determinate condizione e la variabile B in altre.  
  
> [!NOTE]  
>  L'attività File system opera su un singolo file o directory. Pertanto, questa attività non supporta l'utilizzo di caratteri jolly per eseguire la stessa operazione su più file o directory. Affinché l'attività File system ripeta un'operazione su più file o directory, inserirla in un contenitore Ciclo Foreach. Per altre informazioni, vedere [Attività File system](../../integration-services/control-flow/file-system-task.md).  
  
 Con le espressioni è possibile utilizzare variabili differenti per  
  
### <a name="options"></a>Opzioni  
 **IsDestinationPathVariable**  
 Consente di specificare se il percorso di destinazione è archiviato in una variabile. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**True**|Il percorso di destinazione è archiviato in una variabile. Se si seleziona questo valore, viene visualizzata l'opzione dinamica **DestinationVariable**.|  
|**False**|Il percorso di destinazione è specificato in una gestione connessione file. Se si seleziona questo valore, viene visualizzata l'opzione dinamica **DestinationConnection**.|  
  
 **OverwriteDestination**  
 Consente di specificare se l'operazione può sovrascrivere i file nella directory di destinazione.  
  
 **Nome**  
 Consente di specificare un nome univoco per l'attività File system. Tale nome viene utilizzato come etichetta nell'icona dell'attività.  
  
> [!NOTE]  
>  I nomi delle attività devono essere univoci all'interno di un pacchetto.  
  
 **Descrizione**  
 Consente di digitare una descrizione dell'attività File system.  
  
 **Operazione**  
 Consente di selezionare l'operazione di file system da eseguire. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Copia directory**|Consente di copiare una directory. Quando si seleziona questo valore vengono visualizzate le opzioni dinamiche per l'origine e la destinazione.|  
|**Copia file**|Consente di copiare un file. Quando si seleziona questo valore vengono visualizzate le opzioni dinamiche per l'origine e la destinazione.|  
|**Crea directory**|Creare una directory. Quando si seleziona questo valore vengono visualizzate le opzioni dinamiche per una directory di origine e di destinazione.|  
|**Elimina directory**|Consente di eliminare una directory. Quando si seleziona questo valore vengono visualizzate le opzioni dinamiche per l'origine.|  
|**Elimina contenuto directory**|Consente di eliminare il contenuto di una directory. Quando si seleziona questo valore vengono visualizzate le opzioni dinamiche per l'origine.|  
|**Elimina file**|Consente di eliminare un file. Quando si seleziona questo valore vengono visualizzate le opzioni dinamiche per l'origine.|  
|**Sposta directory**|Consente di spostare una directory. Quando si seleziona questo valore vengono visualizzate le opzioni dinamiche per l'origine e la destinazione.|  
|**Sposta file**|Consente di spostare un file. Quando si seleziona questo valore vengono visualizzate le opzioni dinamiche per l'origine e la destinazione. Quando si sposta un file, non includere un nome file nel percorso della directory che si fornisce come destinazione.|  
|**Rinomina file**|Consente di rinominare un file. Quando si seleziona questo valore vengono visualizzate le opzioni dinamiche per l'origine e la destinazione. Quando si rinomina un file, includere il nuovo nome file nel percorso della directory che si fornisce come destinazione.|  
|**Imposta attributi**|Consente di impostare gli attributi di un file o di una directory. Quando si seleziona questo valore vengono visualizzate le opzioni dinamiche per l'origine e l'operazione.|  
  
 **IsSourcePathVariable**  
 Consente di specificare se il percorso di destinazione è archiviato in una variabile. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-|  
|**True**|Il percorso di destinazione è archiviato in una variabile. Se si seleziona questo valore, viene visualizzata l'opzione dinamica **SourceVariable**.|  
|**False**|Il percorso di destinazione è specificato in una gestione connessione file. Se si seleziona questo valore, viene visualizzata l'opzione dinamica **DestinationVariable**.|  
  
### <a name="isdestinationpathvariable-dynamic-options"></a>Opzioni dinamiche di IsDestinationPathVariable  
  
#### <a name="isdestinationpathvariable--true"></a>IsDestinationPathVariable = True  
 **DestinationVariable**  
 Selezionare il nome della variabile nell'elenco o fare clic su \<**New variable...**> per crearne una nuova.  
  
 **Argomenti correlati:** [Variabili di Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Aggiungere una variabile](../integration-services-ssis-variables.md)  
  
#### <a name="isdestinationpathvariable--false"></a>IsDestinationPathVariable = False  
 **DestinationConnection**  
 Selezionare una gestione connessione file nell'elenco oppure fare clic su \<**New connection...**> per creare una nuova gestione connessione.  
  
 **Argomenti correlati:** [Gestione connessione file](../../integration-services/connection-manager/file-connection-manager.md), [Editor gestione connessione file](../connection-manager/file-connection-manager.md)  
  
### <a name="issourcepathvariable-dynamic-options"></a>Opzioni dinamiche di IsSourcePathVariable  
  
#### <a name="issourcepathvariable--true"></a>IsSourcePathVariable = True  
 **SourceVariable**  
 Selezionare il nome della variabile nell'elenco o fare clic su \<**New variable...**> per crearne una nuova.  
  
 **Argomenti correlati:** [Variabili di Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Aggiungere una variabile](../integration-services-ssis-variables.md)  
  
#### <a name="issourcepathvariable--false"></a>IsSourcePathVariable = False  
 **SourceConnection**  
 Selezionare una gestione connessione file nell'elenco oppure fare clic su \<**New connection...**> per creare una nuova gestione connessione.  
  
 **Argomenti correlati:** [Gestione connessione file](../../integration-services/connection-manager/file-connection-manager.md)  
  
### <a name="operation-dynamic-options"></a>Opzioni dinamiche di Operation  
  
#### <a name="operation--set-attributes"></a>Operation = Imposta attributi  
 **Hidden**  
 Consente di specificare se il file o la directory è visibile.  
  
 **ReadOnly**  
 Consente di specificare se il file è di sola lettura.  
  
 **Archiviazione**  
 Consente di specificare se il file o la directory è pronta per l'archiviazione.  
  
 **Sistema**  
 Consente di specificare se il file è un file di sistema.  
  
#### <a name="operation--create-directory"></a>Operation = Crea directory  
 **UseDirectoryIfExists**  
 Indica se l'operazione **Crea directory** usa una directory esistente con il nome specificato anziché creare una nuova directory.  
  
## <a name="see-also"></a>Vedere anche  
 [Attività di Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Flusso di controllo](../../integration-services/control-flow/control-flow.md)  
  
