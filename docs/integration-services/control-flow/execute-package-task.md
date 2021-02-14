---
description: Attività Esegui pacchetto
title: Attività Esegui pacchetto | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.executepackagetask.f1
- sql13.dts.designer.executepackagetask.package.f1
- sql13.dts.designer.executepackagetask.parameter.F1
- sql13.dts.designer.executepackagetask.general.f1
helpviewer_keywords:
- Execution Package task [Integration Services]
- child packages
- parent packages [Integration Services]
ms.assetid: 042d4ec0-0668-401c-bb3a-a25fe2602eac
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8251f2ff30fafff584b4f56fe93a9b7423ea412b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352081"
---
# <a name="execute-package-task"></a>Attività Esegui pacchetto

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  L'attività Esegui pacchetto permette di estendere le funzionalità aziendali di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] consentendo ai pacchetti di eseguire altri pacchetti nell'ambito di un flusso di lavoro.  
  
 È possibile utilizzare l'attività Esegui pacchetto per gli scopi seguenti:  
  
-   Suddivisione del flusso di lavoro di pacchetti complessi. Questa attività consente di suddividere il flusso di lavoro in più pacchetti, più semplici da leggere, testare e gestire. Se ad esempio si caricano dati in uno schema star, sarà possibile compilare un pacchetto a parte per il popolamento delle singole dimensioni e della tabella dei fatti.  
  
-   Riutilizzo di parti di pacchetti. È possibile riutilizzare parti del flusso di lavoro di un pacchetto in altri pacchetti. È ad esempio possibile compilare un modulo di estrazione dati che può essere chiamato da pacchetti diversi. Ogni pacchetto che chiama il modulo di estrazione può quindi eseguire operazioni diverse di ripulitura, filtraggio o aggregazione dei dati.  
  
-   Raggruppamento di unità di lavoro. Le unità di lavoro possono essere incapsulate in pacchetti separati ed è possibile crearne un join come componenti transazionali al flusso di lavoro di un pacchetto padre. Il pacchetto padre esegue ad esempio i pacchetti accessori e, in base all'esito positivo o negativo dell'esecuzione di questi ultimi, esegue il commit o il rollback della transazione.  
  
-   Controllo della sicurezza dei pacchetti. Gli autori dei pacchetti hanno l'esigenza di accedere solo a una parte di una soluzione composta da più pacchetti. Suddividendo un pacchetto in più pacchetti è possibile offrire un livello di sicurezza superiore, perché a ogni autore è possibile concedere l'accesso ai soli pacchetti interessati.  
  
 Un pacchetto che esegue altri pacchetti è detto in genere pacchetto padre, mentre i pacchetti eseguiti dal flusso di lavoro di un pacchetto padre sono detti pacchetti figlio.  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] include attività per l'esecuzione delle operazioni del flusso di lavoro, ad esempio l'esecuzione di eseguibili e file batch. Per altre informazioni, vedere [Attività Esegui processo](../../integration-services/control-flow/execute-process-task.md).  
  
## <a name="running-packages"></a>Esecuzione di pacchetti  
 L'attività Esegui pacchetto consente di eseguire i pacchetti figlio contenuti nello stesso progetto in cui è contenuto il pacchetto padre. Per selezionare un pacchetto figlio dal progetto, impostare la proprietà **ReferenceType** su **Riferimento al progetto** e quindi impostare la proprietà **PackageNameFromProjectReference** .  
  
> [!NOTE]  
>  L'opzione **ReferenceType** è di sola lettura e viene impostata su **Riferimento esterno** se il progetto in cui è contenuto il pacchetto non è stato convertito nel modello di distribuzione del progetto. [Distribuire progetti e pacchetti di Integration Services (SSIS)](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).  
  
 L'attività Esegui pacchetto può eseguire sia pacchetti archiviati nel database msdb di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che pacchetti archiviati nel file system. L'attività usa una gestione connessione OLE DB per connettersi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o una gestione connessione file per l'accesso al file system. Per altre informazioni, vedere [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md) e [File Connection Manager](../../integration-services/connection-manager/flat-file-connection-manager.md).  
  
 Poiché l'attività Esegui pacchetto consente anche di eseguire un piano di manutenzione database, è possibile gestire sia pacchetti [!INCLUDE[ssIS](../../includes/ssis-md.md)] che piani di manutenzione database nella stessa soluzione di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Un piano di manutenzione database è simile a un pacchetto di [!INCLUDE[ssIS](../../includes/ssis-md.md)] , ma può includere solo attività di manutenzione del database e viene sempre archiviato nel database msdb.  
  
 Se si sceglie un pacchetto archiviato nel file system, sarà necessario specificare il nome e il percorso del pacchetto. Il pacchetto può risiedere in qualunque posizione del file system e non deve necessariamente trovarsi nella stessa cartella del pacchetto padre.  
  
 Il pacchetto figlio può essere eseguito nel processo del pacchetto padre o in un processo a parte. L'esecuzione del pacchetto figlio in un processo a parte richiede più memoria, ma offre maggiore flessibilità. Se ad esempio il processo figlio non riesce, il processo padre potrà continuare l'esecuzione.  
  
 Talvolta può essere tuttavia necessario che l'esito dei pacchetti padre e figlio venga determinato come per una singola unità oppure si desidera evitare l'overhead di un processo aggiuntivo. Se ad esempio un processo figlio non riesce e nel processo padre la fase successiva dell'elaborazione dipende dal completamento del processo figlio, è preferibile eseguire il pacchetto figlio nello stesso processo del pacchetto padre.  
  
 Per impostazione predefinita, la proprietà ExecuteOutOfProcess dell'attività Esegui pacchetto è impostata su **False** e il pacchetto figlio viene eseguito nello stesso processo del pacchetto padre. Se si imposta questa proprietà su **True**, il pacchetto figlio viene eseguito in un processo separato. In questo modo è possibile che l'avvio del pacchetto figlio sia rallentato. Inoltre, se si imposta la proprietà su **True**, non è possibile eseguire il debug del pacchetto in un'installazione di soli strumenti. È necessario installare [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Per altre informazioni, vedere [Installazione di Integration Services](../../integration-services/install-windows/install-integration-services.md).  
  
## <a name="extending-transactions"></a>Estensione delle transazioni  
 Poiché la transazione utilizzata dal pacchetto padre può essere estesa al pacchetto figlio, è possibile eseguire in un'unica operazione il commit o il rollback di tutte le operazioni eseguite dai due pacchetti. È ad esempio possibile eseguire il commit o il rollback degli inserimenti nel database eseguiti dal pacchetto padre a seconda dell'esito degli inserimenti nel database eseguiti dal pacchetto figlio e viceversa. Per altre informazioni, vedere [Transazioni ereditate](../integration-services-transactions.md).  
  
## <a name="propagating-logging-details"></a>Propagazione dei dettagli di registrazione  
 Il pacchetto figlio eseguito dall'attività Esegui pacchetto invia sempre i dettagli di registrazione al pacchetto padre, anche se non è configurato per l'utilizzo della registrazione. I dettagli ricevuti dal pacchetto figlio verranno tuttavia registrati solo se l'attività Esegui pacchetto è configurata per l'utilizzo della registrazione. Per altre informazioni, vedere [registrazione di Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
## <a name="passing-values-to-child-packages"></a>Passaggio di valori ai pacchetti figlio  
 I pacchetti figlio utilizzano in genere valori ricevuti dal pacchetto chiamante, che normalmente è il pacchetto padre. L'utilizzo di valori ricevuti da un pacchetto padre può essere utile negli scenari seguenti:  
  
-   Parti di un flusso di lavoro più grande sono assegnate a pacchetti diversi. È ad esempio possibile creare un pacchetto che scarica dati durante la notte, li riepiloga, assegna i valori di riepilogo alle variabili appropriate e quindi le passa a un altro pacchetto per un'ulteriore elaborazione dei dati.  
  
-   Il pacchetto padre coordina dinamicamente le attività in un pacchetto figlio. Il pacchetto padre determina ad esempio il numero dei giorni del mese corrente e lo assegna a una variabile, di modo che il pacchetto figlio esegua una determinata attività per il numero di volte indicato da tale valore.  
  
-   Un pacchetto figlio deve accedere ai dati derivati dinamicamente dal pacchetto padre. Ad esempio, il pacchetto padre estrae dati da una tabella e carica il set di righe in una variabile, quindi il pacchetto figlio esegue ulteriori elaborazioni su tali dati.  
  
 È possibile utilizzare i metodi seguenti per passare valori a un pacchetto figlio:  
  
-   **SSIS**  
  
     [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] offre un tipo di configurazione, cioè Variabile pacchetto padre, per il passaggio dei valori dal pacchetto padre a quello figlio. Tale configurazione è compilata in base al pacchetto figlio e utilizza una variabile del pacchetto padre. Viene quindi eseguito il mapping della configurazione a una variabile nel pacchetto figlio o alla proprietà di un oggetto nel pacchetto figlio. La variabile può anche essere utilizzata negli script eseguiti dall'attività Script o dal componente script.  
  
-   **Parameters**  
  
     È possibile configurare l'attività Esegui pacchetto per eseguire il mapping delle variabili o dei parametri del pacchetto padre o dei parametri del progetto ai parametri del pacchetto figlio. Il progetto deve utilizzare il modello di distribuzione del progetto e il pacchetto figlio deve essere contenuto nello stesso progetto in cui è contenuto il pacchetto padre. Per altre informazioni, vedere [Editor attività Esegui pacchetto]().  
  
    > [!NOTE]  
    >  Se il parametro del pacchetto figlio non è sensibile e ne viene eseguito il mapping a un parametro padre sensibile, non sarà possibile completare l'esecuzione del pacchetto figlio.  
    >   
    >  Sono supportate le seguenti condizioni di mapping:  
    >   
    >  Viene eseguito il mapping del parametro del pacchetto figlio sensibile a un parametro padre sensibile  
    >   
    >  Viene eseguito il mapping del parametro del pacchetto figlio sensibile a un parametro padre non sensibile  
    >   
    >  Viene eseguito il mapping del parametro del pacchetto figlio non sensibile a un parametro padre non sensibile  
  
 La variabile del pacchetto padre può essere definita nell'ambito dell'attività Esegui pacchetto o in un contenitore padre, ad esempio il pacchetto. Se sono presenti più variabili con lo stesso nome, verrà utilizzata quella definita nell'ambito dell'attività Esegui pacchetto oppure quella con ambito più vicino all'attività.  
  
 Per altre informazioni, vedere [Utilizzare i valori di variabili e parametri in un pacchetto figlio](../../integration-services/packages/legacy-package-deployment-ssis.md#child).  
  
### <a name="accessing-parent-package-variables"></a>Accesso alle variabili del pacchetto padre  
 Utilizzando l'attività Script è possibile consentire ai pacchetti figlio di accedere alle variabili del pacchetto padre. Quando si immette il nome della variabile del pacchetto padre nella pagina **Script** di **Editor attività Script**, non includere **User:** nel nome della variabile. In caso contrario, il pacchetto figlio non individua la variabile quando si esegue il pacchetto padre.  
  
## <a name="configuring-the-execute-package-task"></a>Configurazione dell'attività Esegui pacchetto  
 È possibile impostare le proprietà tramite Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] o a livello di codice.  
  
 Per ulteriori informazioni sulle proprietà che è possibile impostare in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] , fare clic sull'argomento seguente:  
  
-   [Pagina Espressioni](../../integration-services/expressions/expressions-page.md)  
  
 Per altre informazioni sull'impostazione di queste proprietà in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] , fare clic sull'argomento seguente:  
  
-   [Impostazione delle proprietà di un'attività o di un contenitore](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="configuring-the-execute-package-task-programmatically"></a>Configurazione dell'attività Esegui pacchetto a livello di codice  
 Per ulteriori informazioni sull'impostazione di queste proprietà a livello di codice, fare clic sull'argomento seguente:  
  
-   [N:Microsoft.SqlServer.Dts.Tasks.ExecutePackageTask](https://technet.microsoft.com/library/microsoft.sqlserver.dts.tasks.executepackagetask\(v=sql.110\).aspx)  
  
## <a name="execute-package-task-editor"></a>Editor attività Esegui pacchetto
  Utilizzare l'editor attività Esegui pacchetto per configurare la relativa attività. L'attività Esegui pacchetto permette di estendere le funzionalità aziendali di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] consentendo ai pacchetti di eseguire altri pacchetti nell'ambito di un flusso di lavoro.  
  
 **Per saperne di più**  
  
-   [Aprire l'editor attività Esegui pacchetto](#open)  
  
-   [Impostare le opzioni nella pagina Generale](#general)  
  
-   [Impostare le opzioni nella pagina Pacchetto](#package)  
  
-   [Impostare le opzioni nella pagina Associazioni di parametro](#parameter)  
  
###  <a name="open-the-execute-package-task-editor"></a><a name="open"></a> Aprire l'editor attività Esegui pacchetto  
  
1.  Aprire un progetto [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] in [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] contenente un'attività Esegui pacchetto.  
  
2.  Fare clic con il pulsante destro del mouse sull'attività disponibile in Progettazione SSIS, quindi scegliere **Modifica**.  
  
###  <a name="set-the-options-on-the-general-page"></a><a name="general"></a> Impostare le opzioni nella pagina Generale  
 **Nome**  
 Specificare un nome univoco per l'attività Esegui pacchetto. Tale nome viene utilizzato come etichetta nell'icona dell'attività.  
  
> [!NOTE]  
>  I nomi delle attività devono essere univoci all'interno di un pacchetto.  
  
 **Descrizione**  
 Digitare una descrizione dell'attività Esegui pacchetto.  
  
###  <a name="set-the-options-on-the-package-page"></a><a name="package"></a> Impostare le opzioni nella pagina Pacchetto  
 **ReferenceType**  
 Selezionare **Riferimento al progetto** per pacchetti figlio inclusi nel progetto. Selezionare **Riferimento esterno** per pacchetti figlio posizionati esternamente al pacchetto  
  
> [!NOTE]  
>  L'opzione **ReferenceType** è di sola lettura e viene impostata su **Riferimento esterno** se il progetto in cui è contenuto il pacchetto non è stato convertito nel modello di distribuzione del progetto. [Distribuire progetti e pacchetti di Integration Services (SSIS)](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).  
  
 **Password**  
 Se il pacchetto figlio è protetto con password, specificare la password del pacchetto figlio oppure fare clic sul pulsante con i puntini di sospensione (...) per creare una nuova password per il pacchetto figlio.  
  
 **ExecuteOutOfProcess**  
 Specificare se il pacchetto figlio viene eseguito nel processo del pacchetto padre o in un processo a parte. Per impostazione predefinita, la proprietà ExecuteOutOfProcess dell'attività Esegui pacchetto è impostata su **False** e il pacchetto figlio viene eseguito nello stesso processo del pacchetto padre. Se si imposta questa proprietà su **True**, il pacchetto figlio viene eseguito in un processo separato. In questo modo è possibile che l'avvio del pacchetto figlio sia rallentato. Inoltre, se la proprietà viene impostata su **True**, non è possibile eseguire il debug del pacchetto in un'installazione di soli strumenti, ma è necessario installare il prodotto [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Per altre informazioni, vedere [Installazione di Integration Services](../../integration-services/install-windows/install-integration-services.md).  
  
#### <a name="referencetype-dynamic-options"></a>Opzioni dinamiche relative a ReferenceType  
  
##### <a name="referencetype--external-reference"></a>ReferenceType = Riferimento esterno  
 **Posizione**  
 Selezionare il percorso del pacchetto figlio. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**SQL Server**|Impostare il percorso su un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**File system**|Impostare il percorso sul file system.|  
  
 **Connection**  
 Selezionare il tipo di posizione di archiviazione del pacchetto figlio.  
  
 **PackageNameReadOnly**  
 Viene visualizzato il nome del pacchetto.  
  
##### <a name="referencetype--project-reference"></a>ReferenceType = Riferimento al progetto  
 **PackageNameFromProjectReference**  
 Selezionare un pacchetto contenuto nel progetto affinché sia considerato il pacchetto figlio.  
  
#### <a name="location-dynamic-options"></a>Opzioni dinamiche relative al percorso  
  
##### <a name="location--sql-server"></a>Percorso = SQL Server  
 **Connection**  
 Selezionare una gestione connessione OLE DB nell'elenco oppure fare clic su \<**New connection...**> per creare una nuova gestione connessione.  
  
 **Argomenti correlati:** [Gestione connessione OLE DB](../../integration-services/connection-manager/ole-db-connection-manager.md)  
  
 **PackageName**  
 Digitare il nome del pacchetto figlio oppure fare clic sul pulsante con i puntini di sospensione (...) e quindi individuare il pacchetto.  
  
##### <a name="location--file-system"></a>Percorso = File system  
 **Connection**  
 Selezionare una gestione connessione file nell'elenco oppure fare clic su \<**New connection...**> per creare una nuova gestione connessione.  
  
 **Argomenti correlati:** [Gestione connessione file](../../integration-services/connection-manager/file-connection-manager.md)  
  
 **PackageNameReadOnly**  
 Viene visualizzato il nome del pacchetto.  
  
###  <a name="set-the-options-on-the-parameter-bindings-page"></a><a name="parameter"></a> Impostare le opzioni nella pagina Associazioni di parametro  
 È possibile passare i valori del pacchetto padre o del progetto al pacchetto figlio. Il progetto deve utilizzare il modello di distribuzione del progetto e il pacchetto figlio deve essere contenuto nello stesso progetto in cui è contenuto il pacchetto padre.  
  
 Per informazioni sulla conversione dei progetti nel modello di distribuzione del progetto, vedere [Distribuire progetti e pacchetti di Integration Services (SSIS)](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).  
  
 **Parametro del pacchetto figlio**  
 Immettere o selezionare un nome per il parametro del pacchetto figlio.  
  
 **Parametro o variabile di associazione**  
 Selezionare il parametro o la variabile contenente il valore che si desidera passare al pacchetto figlio.  
  
 **Aggiungere**  
 Fare clic su questa opzione per eseguire il mapping di un parametro o una variabile a un parametro del pacchetto figlio.  
  
 **Rimuovi**  
 Fare clic su questa opzione per rimuovere un mapping tra un parametro o una variabile e un parametro del pacchetto figlio.  
  
