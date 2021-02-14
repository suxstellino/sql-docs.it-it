---
description: Attività Flusso di dati
title: Attività Flusso di dati | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.dataflowtask.f1
helpviewer_keywords:
- data flow task [Integration Services]
- performance [Integration Services]
- data flow [Integration Services], performance
- data flow [Integration Services], Data Flow task
- Integration Services, performance
ms.assetid: c27555c4-208c-43c8-b511-a4de2a8a3344
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 00270115679b886a0f4d657487a91e845c56da3f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352120"
---
# <a name="data-flow-task"></a>Attività Flusso di dati

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  L'attività Flusso di dati incapsula il motore flusso di dati che consente di spostare i dati dalle origini alle destinazioni e offre la possibilità di trasformare, pulire e modificare i dati durante lo spostamento. L'aggiunta di un'attività Flusso di dati al flusso di controllo di un pacchetto consente al pacchetto di estrarre, trasformare e caricare dati.  
  
 Un flusso di dati è costituito da almeno un componente dei flussi di dati, ma è in genere formato da un set di componenti dei flussi di dati opportunamente connessi: origini che estraggono i dati, trasformazioni che modificano, inviano o riepilogano i dati e destinazioni che caricano i dati.  
  
 In fase di esecuzione l'attività Flusso di dati compila un piano di esecuzione, basato sul flusso di dati, che viene eseguito dal motore flusso di dati. È possibile creare un'attività Flusso di dati priva di flusso di dati, ma l'attività verrà eseguita solo se include almeno un flusso di dati.  
  
 Per l'inserimento bulk dei dati di file di testo in un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è possibile utilizzare l'attività Inserimento bulk anziché un'attività Flusso di dati e un flusso di dati. L'attività Inserimento bulk non consente tuttavia la trasformazione dei dati. Per altre informazioni, vedere [Attività Inserimento bulk](../../integration-services/control-flow/bulk-insert-task.md).  
  
## <a name="multiple-flows"></a>Più flussi  
 Un'attività Flusso di dati può includere più flussi di dati. Se un'attività copia più set di dati e l'ordine in cui i dati vengono copiati non è significativo, è consigliabile includere più flussi di dati in un'unica attività Flusso di dati. È ad esempio possibile creare cinque flussi di dati, ognuno dei quali copia dati da un file flat a una tabella delle dimensioni diversa in uno schema star di data warehouse.  
  
 Se un'attività flusso di dati contiene più flussi di dati, l'ordine di esecuzione verrà automaticamente determinato dal motore flusso di dati. Se l'ordine di esecuzione è rilevante, sarà pertanto necessario includere nel pacchetto più attività Flusso di dati, ognuna contenente un solo flusso di dati. Sarà quindi possibile applicare vincoli di precedenza per controllare l'ordine di esecuzione delle attività.  
  
 Nella figura seguente viene illustrata un'attività Flusso di dati che include più flussi di dati.  
  
 ![Flussi di dati](../../integration-services/control-flow/media/mw-dts-09.gif "Flussi di dati")  
  
## <a name="log-entries"></a>Voci di log  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] include un set di eventi del log disponibili per tutte le attività. [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] fornisce anche voci di log personalizzate a molte attività. Per altre informazioni, vedere [registrazione di Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md). L'attività Flusso di dati include le voci di log personalizzate seguenti:  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**BufferSizeTuning**|Indica che l'attività Flusso di dati ha modificato le dimensioni del buffer. In questa voce di log vengono indicati i motivi della modifica delle dimensioni del buffer e le nuove dimensioni temporanee del buffer.|  
|**OnPipelinePostEndOfRowset**|Indica che a un componente è stato inviato il segnale di fine del set di righe, che viene impostato dall'ultima chiamata al metodo **ProcessInput** . Viene scritta una voce per ogni componente del flusso di dati che elabora dati di input. Tale voce include il nome del componente.|  
|**OnPipelinePostPrimeOutput**|Indica che il componente ha completato l'ultima chiamata al metodo **PrimeOutput** . A seconda del flusso di dati, è possibile che vengano scritte più voci di log. Se il componente è un'origine, questa voce di log indica che tale componente ha terminato l'elaborazione delle righe.|  
|**OnPipelinePreEndOfRowset**|Indica che un componente sta per ricevere il segnale di fine del set di righe, che viene impostato dall'ultima chiamata al metodo **ProcessInput** . Viene scritta una voce per ogni componente del flusso di dati che elabora dati di input. Tale voce include il nome del componente.|  
|**OnPipelinePrePrimeOutput**|Indica che il componente sta per ricevere una chiamata dal metodo **PrimeOutput** . A seconda del flusso di dati, è possibile che vengano scritte più voci di log.|  
|**OnPipelineRowsSent**|Specifica il numero delle righe inviate all'input di un componente da una chiamata al metodo **ProcessInput** . La voce di log include il nome del componente.|  
|**PipelineBufferLeak**|Fornisce informazioni su tutti i componenti che hanno mantenuto attivi i buffer dopo la chiusura di Gestione buffer. Se vi è ancora un buffer attivo, le risorse dei buffer non sono state rilasciate e potrebbero verificarsi perdite di memoria. Nella voce di log vengono indicati il nome del componente e l'ID del buffer.|  
|**PipelineComponentTime**|Indica la quantità di tempo (in millisecondi) impiegata dal componente per ognuno dei cinque passaggi principali dell'elaborazione, ovvero Validate, PreExecute, PostExecute, ProcessInput e ProcessOutput.|  
|**PipelineExecutionPlan**|Specifica il piano di esecuzione del flusso di dati. Il piano di esecuzione offre informazioni sulle modalità di invio dei buffer ai componenti. Insieme alla voce di log PipelineExecutionTrees, queste informazioni illustrano ciò che avviene nell'attività Flusso di dati.|  
|**PipelineExecutionTrees**|Specifica gli alberi di esecuzione del layout nel flusso di dati. L'utilità di pianificazione del motore flusso di dati utilizza tali alberi per compilare il piano di esecuzione del flusso di dati.|  
|**PipelineInitialization**|Fornisce le informazioni di inizializzazione relative all'attività, che includono le directory da utilizzare per l'archiviazione temporanea dei dati BLOB, le dimensioni predefinite del buffer e il numero di righe in un buffer. A seconda della configurazione dell'attività Flusso di dati, è possibile che vengano scritte più voci di log.|  
  
 Queste voci di log forniscono numerose informazioni sull'esecuzione dell'attività Flusso di dati ogni volta che si esegue un pacchetto. L'esecuzione ripetuta dei pacchetti consente nel tempo di acquisire importanti informazioni cronologiche sull'elaborazione eseguita dall'attività, gli eventuali problemi che possono influire sulle prestazioni e il volume di dati gestito dall'attività.  
  
 Per ulteriori informazioni sull'utilizzo di queste voci di log per monitorare e migliorare le prestazioni del flusso di dati, vedere uno degli argomenti seguenti:  
  
-   [Contatori delle prestazioni](../../integration-services/performance/performance-counters.md)  
  
-   [Funzionalità delle prestazioni del flusso di dati](../../integration-services/data-flow/data-flow-performance-features.md)  
  
### <a name="sample-messages-from-a-data-flow-task"></a>Messaggi di esempio di un'attività Flusso di dati  
 Nella tabella seguente vengono elencati messaggi di esempio per le voci di log relative a un pacchetto molto semplice. Il pacchetto utilizza un'origine OLE DB per estrarre dati da una tabella, una trasformazione Ordinamento per ordinare i dati e una destinazione OLE DB per scrivere i dati in una tabella diversa.  
  
|Voce di log|Messaggi|  
|---------------|--------------|  
|**BufferSizeTuning**|`Rows in buffer type 0 would cause a buffer size greater than the configured maximum. There will be only 9637 rows in buffers of this type.`<br /><br /> `Rows in buffer type 2 would cause a buffer size greater than the configured maximum. There will be only 9497 rows in buffers of this type.`<br /><br /> `Rows in buffer type 3 would cause a buffer size greater than the configured maximum. There will be only 9497 rows in buffers of this type.`|  
|**OnPipelinePostEndOfRowset**|`A component will be given the end of rowset signal. : 1180 : Sort : 1181 : Sort Input`<br /><br /> `A component will be given the end of rowset signal. : 1291 : OLE DB Destination : 1304 : OLE DB Destination Input`|  
|**OnPipelinePostPrimeOutput**|`A component has returned from its PrimeOutput call. : 1180 : Sort`<br /><br /> `A component has returned from its PrimeOutput call. : 1 : OLE DB Source`|  
|**OnPipelinePreEndOfRowset**|`A component has finished processing all of its rows. : 1180 : Sort : 1181 : Sort Input`<br /><br /> `A component has finished processing all of its rows. : 1291 : OLE DB Destination : 1304 : OLE DB Destination Input`|  
|**OnPipelinePrePrimeOutput**|`PrimeOutput will be called on a component. : 1180 : Sort`<br /><br /> `PrimeOutput will be called on a component. : 1 : OLE DB Source`|  
|**OnPipelineRowsSent**|`Rows were provided to a data flow component as input. :  : 1185 : OLE DB Source Output : 1180 : Sort : 1181 : Sort Input : 76`<br /><br /> `Rows were provided to a data flow component as input. :  : 1308 : Sort Output : 1291 : OLE DB Destination : 1304 : OLE DB Destination Input : 76`|  
|**PipelineComponentTime**|`The component "Calculate LineItemTotalCost" (3522) spent 356 milliseconds in ProcessInput.`<br /><br /> `The component "Sum Quantity and LineItemTotalCost" (3619) spent 79 milliseconds in ProcessInput.`<br /><br /> `The component "Calculate Average Cost" (3662) spent 16 milliseconds in ProcessInput.`<br /><br /> `The component "Sort by ProductID" (3717) spent 125 milliseconds in ProcessInput.`<br /><br /> `The component "Load Data" (3773) spent 0 milliseconds in ProcessInput.`<br /><br /> `The component "Extract Data" (3869) spent 688 milliseconds in PrimeOutput filling buffers on output "OLE DB Source Output" (3879).`<br /><br /> `The component "Sum Quantity and LineItemTotalCost" (3619) spent 141 milliseconds in PrimeOutput filling buffers on output "Aggregate Output 1" (3621).`<br /><br /> `The component "Sort by ProductID" (3717) spent 16 milliseconds in PrimeOutput filling buffers on output "Sort Output" (3719).`|  
|**PipelineExecutionPlan**|`SourceThread0`<br /><br /> `Drives: 1`<br /><br /> `Influences: 1180 1291`<br /><br /> `Output Work List`<br /><br /> `CreatePrimeBuffer of type 1 for output ID 11.`<br /><br /> `SetBufferListener: "WorkThread0" for input ID 1181`<br /><br /> `CreatePrimeBuffer of type 3 for output ID 12.`<br /><br /> `CallPrimeOutput on component "OLE DB Source" (1)`<br /><br /> `End Output Work List`<br /><br /> `End SourceThread0`<br /><br /> `WorkThread0`<br /><br /> `Drives: 1180`<br /><br /> `Influences: 1180 1291`<br /><br /> `Input Work list, input ID 1181 (1 EORs Expected)`<br /><br /> `CallProcessInput on input ID 1181 on component "Sort" (1180) for view type 2`<br /><br /> `End Input Work list for input 1181`<br /><br /> `Output Work List`<br /><br /> `CreatePrimeBuffer of type 4 for output ID 1182.`<br /><br /> `SetBufferListener: "WorkThread1" for input ID 1304`<br /><br /> `CallPrimeOutput on component "Sort" (1180)`<br /><br /> `End Output Work List`<br /><br /> `End WorkThread0`<br /><br /> `WorkThread1`<br /><br /> `Drives: 1291`<br /><br /> `Influences: 1291`<br /><br /> `Input Work list, input ID 1304 (1 EORs Expected)`<br /><br /> `CallProcessInput on input ID 1304 on component "OLE DB Destination" (1291) for view type 5`<br /><br /> `End Input Work list for input 1304`<br /><br /> `Output Work List`<br /><br /> `End Output Work List`<br /><br /> `End WorkThread1`|  
|**PipelineExecutionTrees**|`begin execution tree 0`<br /><br /> `output "OLE DB Source Output" (11)`<br /><br /> `input "Sort Input" (1181)`<br /><br /> `end execution tree 0`<br /><br /> `begin execution tree 1`<br /><br /> `output "OLE DB Source Error Output" (12)`<br /><br /> `end execution tree 1`<br /><br /> `begin execution tree 2`<br /><br /> `output "Sort Output" (1182)`<br /><br /> `input "OLE DB Destination Input" (1304)`<br /><br /> `output "OLE DB Destination Error Output" (1305)`<br /><br /> `end execution tree 2`|  
|**PipelineInitialization**|`No temporary BLOB data storage locations were provided. The buffer manager will consider the directories in the TEMP and TMP environment variables.`<br /><br /> `The default buffer size is 10485760 bytes.`<br /><br /> `Buffers will have 10000 rows by default`<br /><br /> `The data flow will not remove unused components because its RunInOptimizedMode property is set to false.`|  
  
 Per molti eventi vengono scritte più voci nel log e i messaggi relativi a numerose voci di log contengono dati complessi. Per semplificare la comprensione e la comunicazione del contenuto dei messaggi complessi, è possibile analizzare il testo dei messaggi. In base alla posizione dei log, è possibile utilizzare istruzioni Transact-SQL o un componente script per separare il testo complesso in colonne o altri formati che si ritengono più utili.  
  
 Nella tabella seguente viene ad esempio illustrato il messaggio "Sono state passate righe come input per un componente del flusso di dati. :  : 1185 : Output origine OLE DB : 1180 : Ordinamento : 1181 : Input ordinamento : 76", scomposto in colonne. Il messaggio è stato scritto dall'evento **OnPipelineRowsSent** quando le righe sono state inviate dall'origine OLE DB alla trasformazione Ordinamento.  
  
|Colonna|Descrizione|Valore|  
|------------|-----------------|-----------|  
|**PathID**|Valore della proprietà **ID** del percorso tra l'origine OLE DB e la trasformazione Ordinamento.|1185|  
|**PathName**|Valore della proprietà **Name** del percorso.|Output origine OLE DB|  
|**ComponentID**|Valore della proprietà **ID** della trasformazione Ordinamento.|1180|  
|**ComponentName**|Valore della proprietà **Name** della trasformazione Ordinamento.|Ordina|  
|**InputID**|Valore della proprietà **ID** dell'input della trasformazione Ordinamento.|1181|  
|**InputName**|Valore della proprietà **Name** dell'input della trasformazione Ordinamento.|Input ordinamento|  
|**RowsSent**|Numero di righe inviate all'input della trasformazione Ordinamento.|76|  
  
## <a name="configuration-of-the-data-flow-task"></a>Configurazione dell'attività Flusso di dati  
 È possibile impostare le proprietà a livello di codice o nella finestra **Proprietà** .  
  
 Per altre informazioni sull'impostazione di queste proprietà nella finestra **Proprietà** , fare clic sull'argomento seguente:  
  
-   [Impostazione delle proprietà di un'attività o di un contenitore](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="programmatic-configuration-of-the-data-flow-task"></a>Configurazione a livello di codice dell'attività Flusso di dati  
 Per ulteriori informazioni sull'aggiunta di un'attività Flusso di dati a un pacchetto a livello di codice e sulle impostazioni delle proprietà del flusso di dati, fare clic sull'argomento seguente:  
  
-   [Aggiunta dell'attività Flusso di dati a livello di programmazione](../../integration-services/building-packages-programmatically/adding-the-data-flow-task-programmatically.md)  
  
## <a name="related-tasks"></a>Attività correlate  
 [Impostazione delle proprietà di un'attività o di un contenitore](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="related-content"></a>Contenuto correlato  
 Video relativo al [server di distribuzione di dati bilanciati](/previous-versions/dn912438(v=msdn.10))sul sito technet.microsoft.com.  
  
