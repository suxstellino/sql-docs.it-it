---
description: Message Queue Task
title: Attività Message Queue | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.messagequeuetask.f1
- sql13.dts.designer.msgqueuetask.general.f1
- sql13.dts.designer.msgqueuetask.send.f1
- sql13.dts.designer.msgqueuetask.receive.f1
- sql13.dts.designer.selectvariables.f1
helpviewer_keywords:
- Message Queue task [Integration Services]
- receiving messages
- messages [Integration Services]
- sending messages
ms.assetid: ae1d8fad-6649-4e93-b589-14a32d07da33
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e39187e32afbcf72bf1eb950f18d5f21cef38211
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348307"
---
# <a name="message-queue-task"></a>Message Queue Task

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  L'attività Message Queue consente di usare Microsoft Message Queuing (noto anche come MSMQ) per scambiare messaggi tra pacchetti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] o per inviare messaggi a una coda di applicazione elaborata da un'applicazione personalizzata. I messaggi possono essere in forma di testo semplice, di file o di variabili con i rispettivi valori.  
  
 Tramite l'attività Message Queue è possibile coordinare operazioni nell'intera azienda. Se la destinazione è occupata o non disponibile, i messaggi possono essere messi in coda e recapitati in un secondo tempo. L'attività può ad esempio mettere in coda i messaggi destinati ai computer portatili offline dei rappresentanti, che li riceveranno quando si connetteranno alla rete. È possibile utilizzare l'attività Message Queue per gli scopi seguenti:  
  
-   Ritardare l'esecuzione dell'attività fino all'archiviazione di altri pacchetti. Si supponga ad esempio che, al termine della manutenzione notturna in ogni filiale dell'azienda, un'attività Message Queue invii un messaggio al computer della sede centrale e che in tale computer sia in esecuzione un pacchetto contenente varie attività Message Queue, ognuna delle quali in attesa del messaggio di una filiale specifica. Alla ricezione di un messaggio dalla filiale corrispondente, l'attività carica i dati dal computer della filiale. Al termine dell'archiviazione da parte di tutte le filiali, il pacchetto calcola i totali riepilogativi.  
  
-   Inviare file di dati al computer responsabile della loro elaborazione. L'output del registratore di cassa di un ristorante, ad esempio, può essere inviato in un messaggio di tipo file di dati al sistema di paghe della sede centrale, che estrae i dati relativi alle mance dei singoli camerieri.  
  
-   Distribuire file in tutta l'azienda. È ad esempio possibile utilizzare l'attività Message Queue per inviare il file di un pacchetto a un altro computer, dove è presente un pacchetto in esecuzione che utilizza un'attività Message Queue per recuperare e salvare il pacchetto localmente.  
  
 Per inviare o ricevere messaggi, l'attività Message Queue utilizza uno dei tipi di messaggi seguenti: file di dati, stringa, messaggio stringa in variabile o variabile. I messaggi di tipo messaggio stringa in variabile possono essere utilizzati solo per la ricezione di messaggi.  
  
 Per connettersi a una coda di messaggi l'attività utilizza una gestione connessione MSMQ. Per altre informazioni, vedere [Gestione connessione MSMQ](../../integration-services/connection-manager/msmq-connection-manager.md). Per altre informazioni su Microsoft Message Queuing, vedere [MSDN Library](../../sql-server/index.yml).  
  
 Affinché sia possibile usare l'attività Message Queue, è necessario avere installato il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Alcuni dei componenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che è possibile selezionare per l'installazione nella pagina **Componenti da installare** o **Selezione funzionalità** dell'Installazione guidata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installano solo un subset parziale dei componenti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Tali componenti si rivelano utili per attività specifiche, ma le funzionalità di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] risulteranno limitate. L'opzione [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] , ad esempio, installa i componenti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] necessari per la progettazione dei pacchetti, ma il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] non viene installato e l'attività Message Queue non è disponibile. Per garantire l'installazione completa di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], è necessario selezionare [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] nella pagina **Componenti da installare** . Per altre informazioni sull'installazione e l'esecuzione dell'attività Message Queue, vedere [Installazione di Integration Services](../../integration-services/install-windows/install-integration-services.md).  
  
> [!NOTE]  
>  L'attività Message Queue ha esito negativo in conformità con lo standard FIPS (Federal Information Processing Standard) 140-2 se il sistema operativo del computer è configurato in modalità FIPS e l'attività utilizza la crittografia. Se non viene utilizzata la crittografia, l'attività Message Queue viene eseguita correttamente.  
  
## <a name="message-types"></a>Tipi di messaggi  
 Per configurare i tipi di messaggi disponibili per l'attività Message Queue, procedere nel modo seguente:  
  
-   Il tipo di messaggio **File di dati** specifica che il messaggio è contenuto in un file. Per quanto riguarda la ricezione dei messaggi, è possibile configurare l'attività in modo da salvare il file, sovrascrivere un file esistente e specificare il pacchetto da cui l'attività può ricevere messaggi.  
  
-   Il tipo di messaggio **Stringa** specifica che il messaggio è una stringa. Per quanto riguarda la ricezione dei messaggi, è possibile configurare l'attività in modo da confrontare la stringa ricevuta con una stringa definita dall'utente ed eseguire un'azione che dipende dal risultato del confronto. Il confronto tra le stringhe può essere esatto, con o senza distinzione tra maiuscole e minuscole oppure utilizzare una sottostringa.  
  
-   **Messaggio stringa in variabile** specifica che l'origine del messaggio è una stringa che viene inviata a una variabile di destinazione. È possibile configurare l'attività in modo da confrontare la stringa ricevuta con una stringa definita dall'utente, utilizzando un confronto esatto, senza distinzione tra maiuscole e minuscole o tra sottostringhe. Questo tipo di messaggio è disponibile solo quando l'attività riceve messaggi.  
  
-   **Variabile** specifica che il messaggio contiene una o più variabili. È possibile configurare l'attività in modo da specificare i nomi delle variabili incluse nel messaggio. Per quanto riguarda la ricezione dei messaggi, è possibile configurare l'attività in modo da specificare sia il pacchetto da cui può ricevere messaggi, sia la variabile che costituisce la destinazione del messaggio.  
  
## <a name="sending-messages"></a>sending messages  
 Quando si configura l'attività Message Queue per l'invio di messaggi, per crittografare i messaggi è possibile utilizzare uno degli algoritmi di crittografia attualmente supportati dalla tecnologia MSMQ, ovvero RC2 e RC4. Entrambi questi algoritmi di crittografia sono attualmente considerati vulnerabili rispetto ad algoritmi più recenti non ancora supportati dalla tecnologia MSMQ. È pertanto consigliabile valutare con attenzione le esigenze di crittografia ai fini dell'invio di messaggi tramite l'attività Message Queue.  
  
## <a name="receiving-messages"></a>ricezione di messaggi  
 Per quanto riguarda la ricezione di messaggi, è possibile configurare l'attività Message Queue in modo da:  
  
-   Ignorare il messaggio ricevuto oppure rimuoverlo dalla coda.  
  
-   Specificare un timeout.  
  
-   Generare un errore in caso di timeout.  
  
-   Sovrascrivere un file esistente, se il messaggio è archiviato in un **file di dati**.  
  
-   Salvare il file del messaggio con un nome di file diverso, se il messaggio è di tipo **Messaggio file di dati** .  
  
## <a name="custom-logging-messages-available-on-the-message-queue-task"></a>Messaggi di registrazione personalizzati disponibili nell'attività Message Queue  
 Nella tabella seguente sono elencate le voci di log personalizzate disponibili per l'attività Message Queue. Per altre informazioni, vedere [registrazione di Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**MSMQAfterOpen**|Indica che l'attività ha terminato l'apertura della coda di messaggi.|  
|**MSMQBeforeOpen**|Indica che l'attività ha iniziato ad aprire la coda di messaggi.|  
|**MSMQBeginReceive**|Indica che l'attività ha iniziato a ricevere un messaggio.|  
|**MSMQBeginSend**|Indica che l'attività ha iniziato a inviare un messaggio.|  
|**MSMQEndReceive**|Indica che l'attività ha terminato la ricezione di un messaggio.|  
|**MSMQEndSend**|Indica che l'attività ha terminato l'invio di un messaggio.|  
|**MSMQTaskInfo**|Offre informazioni descrittive sull'attività.|  
|**MSMQTaskTimeOut**|Indica che si è verificato il timeout dell'attività.|  
  
## <a name="configuration-of-the-message-queue-task"></a>Configurazione dell'attività Message Queue  
 È possibile impostare le proprietà tramite Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] o a livello di codice. Per informazioni sulle proprietà che è possibile impostare in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] , fare clic sull'argomento seguente:  
  
-   [Pagina Espressioni](../../integration-services/expressions/expressions-page.md)  
  
 Per informazioni sull'impostazione a livello di programmazione di queste proprietà, vedere la documentazione per la classe **Microsoft.SqlServer.Dts.Tasks.MessageQueueTask.MessageQueueTask** nella Guida per gli sviluppatori.  
  
## <a name="related-tasks"></a>Attività correlate  
 Per altre informazioni su come impostare queste proprietà nella finestra di Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] , vedere [Impostazione delle proprietà di un'attività o di un contenitore](./add-or-delete-a-task-or-a-container-in-a-control-flow.md).  
  
## <a name="message-queue-task-editor-general-page"></a>Editor attività Message Queue (pagina Generale)
  Utilizzare la pagina **Generale** della finestra di dialogo **Editor attività Message Queue** per assegnare un nome e una descrizione all'attività Message Queue, specificare il formato dei messaggi e impostare l'attività per l'invio o la ricezione dei messaggi.  
  
### <a name="options"></a>Opzioni  
 **Nome**  
 Consente di specificare un nome univoco per l'attività Message Queue. Tale nome viene utilizzato come etichetta nell'icona dell'attività.  
  
> [!NOTE]  
>  I nomi delle attività devono essere univoci all'interno di un pacchetto.  
  
 **Descrizione**  
 Consente di digitare una descrizione dell'attività Message Queue.  
  
 **Use2000Format**  
 Consente di indicare se utilizzare il formato 2000 di Microsoft Message Queuing (noto anche come MSMQ). Il valore predefinito è **False**.  
  
 **MSMQConnection**  
 Selezionare una gestione connessione MSMQ nell'elenco oppure fare clic su \<**New connection...**> per creare una nuova gestione connessione.  
  
 **Argomenti correlati**: [Gestione connessione MSMQ](../../integration-services/connection-manager/msmq-connection-manager.md), [Editor gestione connessione MSMQ](../connection-manager/msmq-connection-manager.md)  
  
 **Messaggio**  
 Consente di specificare se l'attività Message Queue invia o riceve messaggi. Se si seleziona **Invia messaggio**, nel riquadro sinistro della finestra di dialogo viene inserita la pagina Invia. Se invece si seleziona **Ricevi messaggio**, viene inserita la pagina Ricevi. Per impostazione predefinita, questo valore è impostato su **Invia messaggio**.  
  
## <a name="message-queue-task-editor-send-page"></a>Editor attività Message Queue (pagina Invio)
  Usare la pagina **Invio** della finestra di dialogo **Editor attività Message Queue** per configurare un'attività Message Queue per l'invio di messaggi da un pacchetto di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
### <a name="options"></a>Opzioni  
 **UseEncryption**  
 Consente di indicare se crittografare il messaggio. Il valore predefinito è **False**.  
  
 **EncryptionAlgorithm**  
 Se si sceglie di utilizzare la crittografia, consente di specificare il nome dell'algoritmo di crittografia da utilizzare. L'attività Message Queue può utilizzare gli algoritmi RC2 e RC4. L'impostazione predefinita è **RC2**.  
  
> [!NOTE]  
>  L'algoritmo RC4 è supportato solo per motivi di compatibilità con le versioni precedenti. È possibile crittografare il nuovo materiale usando RC4 o RC4_128 solo quando il livello di compatibilità del database è 90 o 100. (Non consigliato.) Usare un algoritmo più recente, ad esempio uno degli algoritmi AES. Nella versione corrente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]il materiale crittografato utilizzando RC4 o RC4_128 può essere decrittografato in qualsiasi livello di compatibilità.  
  
> [!IMPORTANT]  
>  Questi algoritmi di crittografia sono quelli supportati dalla tecnologia Microsoft Message Queuing (nota anche come MSMQ). Entrambi gli algoritmi di crittografia sono ormai considerati vulnerabili rispetto ad altri più recenti che tuttavia non sono ancora supportati da MSMQ. È pertanto consigliabile valutare con attenzione le esigenze di crittografia ai fini dell'invio di messaggi tramite l'attività Message Queue.  
  
 **MessageType**  
 Consente di selezionare il tipo di messaggio. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Messaggio file di dati**|Il messaggio viene archiviato in un file. La selezione del valore determina la visualizzazione dell'opzione dinamica **DataFileMessage**.|  
|**Messaggio variabili**|Il messaggio viene archiviato in una variabile. La selezione del valore determina la visualizzazione dell'opzione dinamica **VariableMessage**.|  
|**Messaggio stringa**|Il messaggio viene archiviato nell'attività Message Queue. La selezione del valore determina la visualizzazione dell'opzione dinamica **StringMessage**.|  
  
### <a name="messagetype-dynamic-options"></a>Opzioni dinamiche di MessageType  
  
#### <a name="messagetype--data-file-message"></a>MessageType = Messaggio file di dati  
 **DataFileMessage**  
 Digitare il percorso del file di dati o fare clic sui puntini di sospensione **(...)** e quindi individuare il file.  
  
#### <a name="messagetype--variable-message"></a>MessageType = Messaggio variabili  
 **VariableMessage**  
 Digitare i nomi delle variabili o fare clic sui puntini di sospensione **(...)** e quindi selezionare le variabili. Le variabili sono separate da virgole.  
  
 **Argomenti correlati:** Seleziona variabili  
  
#### <a name="messagetype--string-message"></a>MessageType = Messaggio stringa  
 **StringMessage**  
 Digitare il messaggio stringa o fare clic sui puntini di sospensione **(...)** e quindi digitare il messaggio nella finestra di dialogo **Immettere il messaggio stringa**.  
  
## <a name="message-queue-task-editor-receive-page"></a>Editor attività Message Queue (pagina Ricezione)
  Usare la pagina **Ricezione** della finestra di dialogo **Editor attività Message Queue** per configurare un'attività Message Queue che consenta di ricevere messaggi di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queuing (MSMQ).  
  
### <a name="options"></a>Opzioni  
 **RemoveFromMessageQueue**  
 Consente di indicare se rimuovere il messaggio dalla coda dopo averlo ricevuto. Per impostazione predefinita, questo valore è impostato su **False**.  
  
 **ErrorIfMessageTimeOut**  
 Consente di indicare se l'attività viene interrotta quando si verifica il timeout del messaggio, visualizzando un messaggio di errore. Il valore predefinito è **False**.  
  
 **TimeoutAfter**  
 Se si sceglie di visualizzare un messaggio di errore in caso di errore dell'attività, specificare il numero di secondi di attesa prima della visualizzazione del messaggio di timeout.  
  
 **MessageType**  
 Consente di selezionare il tipo di messaggio. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Messaggio file di dati**|Il messaggio viene archiviato in un file. La selezione del valore determina la visualizzazione dell'opzione dinamica **DataFileMessage**.|  
|**Messaggio variabili**|Il messaggio viene archiviato in una variabile. La selezione del valore determina la visualizzazione dell'opzione dinamica **VariableMessage**.|  
|**Messaggio stringa**|Il messaggio viene archiviato nell'attività Message Queue. La selezione del valore determina la visualizzazione dell'opzione dinamica **StringMessage**.|  
|**Messaggio stringa in variabile**|Il messaggio viene archiviato in una variabile.<br /><br /> La selezione del valore determina la visualizzazione dell'opzione dinamica **StringMessage**.|  
  
### <a name="messagetype-dynamic-options"></a>Opzioni dinamiche di MessageType  
  
#### <a name="messagetype--data-file-message"></a>MessageType = Messaggio file di dati  
 **SaveFileAs**  
 Digitare il percorso del file da usare o fare clic sul pulsante con i puntini di sospensione **(...)** e quindi individuare il file.  
  
 **Overwrite**  
 Consente di indicare se sovrascrivere i dati in un file esistente durante il salvataggio del contenuto di un messaggio con file di dati. Il valore predefinito è **False**.  
  
 **Filter**  
 Consente di specificare se applicare un filtro al messaggio. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Nessun filtro**|L'attività non filtra i messaggi. La selezione del valore determina la visualizzazione dell'opzione dinamica **IdentifierReadOnly**.|  
|**Dal pacchetto**|Il messaggio riceve solo messaggi dal pacchetto specificato. La selezione del valore determina la visualizzazione dell'opzione dinamica **Identifier**.|  
  
#### <a name="filter-dynamic-options"></a>Opzioni dinamiche di filtro  
  
##### <a name="filter--no-filter"></a>Filter = Nessun filtro  
 **IdentifierReadOnly**  
 Questa opzione è di sola lettura. Può essere vuota o contenere il GUID di un pacchetto se la proprietà Filter è stata impostata in precedenza.  
  
##### <a name="filter--from-package"></a>Filter = Dal pacchetto  
 **Identificatore**  
 Se si sceglie di applicare un filtro, digitare l'identificatore univoco del pacchetto da cui si possono ricevere i messaggi oppure fare clic sul pulsante con i puntini di sospensione **(...)** e specificare il pacchetto.  
  
 **Argomenti correlati:** [Seleziona pacchetto](../../integration-services/control-flow/select-a-package.md)  
  
#### <a name="messagetype--variable-message"></a>MessageType = Messaggio variabili  
 **Filter**  
 Consente di specificare se applicare un filtro ai messaggi. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Nessun filtro**|L'attività non filtra i messaggi. La selezione del valore determina la visualizzazione dell'opzione dinamica **IdentifierReadOnly**.|  
|**Dal pacchetto**|Il messaggio riceve solo messaggi dal pacchetto specificato. La selezione del valore determina la visualizzazione dell'opzione dinamica **Identifier**.|  
  
 **Variabile**  
 Digitare il nome della variabile oppure fare clic su \<**New variable...**> per configurare una nuova variabile.  
  
 **Argomenti correlati:** [Aggiungi variabile](../integration-services-ssis-variables.md)  
  
#### <a name="filter-dynamic-options"></a>Opzioni dinamiche di filtro  
  
##### <a name="filter--no-filter"></a>Filter = Nessun filtro  
 **IdentifierReadOnly**  
 Questa opzione è vuota.  
  
##### <a name="filter--from-package"></a>Filter = Dal pacchetto  
 **Identificatore**  
 Se si sceglie di applicare un filtro, digitare l'identificatore univoco del pacchetto da cui si possono ricevere i messaggi oppure fare clic sul pulsante con i puntini di sospensione **(...)** e specificare il pacchetto.  
  
 **Argomenti correlati:** [Seleziona pacchetto](../../integration-services/control-flow/select-a-package.md)  
  
#### <a name="messagetype--string-message"></a>MessageType = Messaggio stringa  
 **Confronta**  
 Consente di specificare se applicare un filtro ai messaggi. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Nessuno**|I messaggi non vengono confrontati.|  
|**Corrispondenza esatta**|I messaggi devono corrispondere esattamente alla stringa nell'opzione indicata **CompareString** .|  
|**Ignora maiuscole/minuscole**|I messaggi devono corrispondere alla stringa indicata nell'opzione **CompareString** , ma nel confronto non viene rilevata la distinzione tra maiuscole e minuscole.|  
|**Stringa contenente**|Il messaggio deve contenere la stringa indicata nell'opzione **CompareString** .|  
  
 **CompareString**  
 Specificare la stringa con cui confrontare il messaggio, a meno che l'opzione **Compare** sia impostata su **Nessuno**.  
  
#### <a name="messagetype--string-message-to-variable"></a>MessageType = Messaggio stringa in variabile  
 **Confronta**  
 Consente di specificare se applicare un filtro ai messaggi. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Nessuno**|I messaggi non vengono confrontati.|  
|**Corrispondenza esatta**|I messaggi devono corrispondere esattamente alla stringa indicata nell'opzione **CompareString** .|  
|**Ignora maiuscole/minuscole**|I messaggi devono corrispondere alla stringa indicata nell'opzione **CompareString** , ma nel confronto non viene rilevata la distinzione tra maiuscole e minuscole.|  
|**Stringa contenente**|Il messaggio deve contenere la stringa indicata nell'opzione **CompareString** .|  
  
 **CompareString**  
 Specificare la stringa con cui confrontare il messaggio, a meno che l'opzione **Compare** sia impostata su **Nessuno**.  
  
 **Variabile**  
 Digitare il nome della variabile per la memorizzazione del messaggio ricevuto oppure fare clic su \<**New variable...**> per configurare una nuova variabile.  
  
 **Argomenti correlati:** [Aggiungi variabile](../integration-services-ssis-variables.md)  
  
## <a name="select-variables"></a>Seleziona variabili
  Usare la finestra di dialogo **Seleziona variabili** per specificare le variabili da applicare in un'operazione di invio di messaggi nell'attività Message Queue. Nell'elenco **Variabili disponibili** sono incluse le variabili di sistema e definite dall'utente che si trovano nell'ambito dell'attività Message Queue o nel rispettivo contenitore padre. L'attività usa le variabili incluse dell'elenco **Variabili selezionate** .  
  
### <a name="options"></a>Opzioni  
 **Variabili disponibili**  
 Consente di selezionare una o più variabili.  
  
 **Variabili selezionate**  
 Consente di selezionare una o più variabili.  
  
 **Freccia DESTRA**  
 Consente di spostare le variabili selezionate nell'elenco **Variabili selezionate** .  
  
 **Freccia SINISTRA**  
 Consente di spostare nuovamente le variabili selezionate nell'elenco **Variabili disponibili** .  
  
 **Nuova variabile**  
 Consente di creare una nuova variabile.  
  
 **Argomenti correlati:** [Aggiungi variabile](../integration-services-ssis-variables.md)  
## <a name="see-also"></a>Vedere anche  
 [Attività di Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Flusso di controllo](../../integration-services/control-flow/control-flow.md)  
  
