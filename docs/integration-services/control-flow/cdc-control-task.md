---
description: Attività di controllo CDC
title: Attività di controllo CDC | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.designer.cdccontroltask.f1
- sql13.ssis.designer.cdccontroltask.config.f1
ms.assetid: 6404dc7f-550c-47cc-b901-c072742f430a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 492a0cd4b510b91d1f5f17ad04de54b005502bba
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049115"
---
# <a name="cdc-control-task"></a>Attività di controllo CDC

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  L'attività di controllo CDC viene utilizzata per controllare il ciclo di vita di pacchetti Change Data Capture (CDC). Questa attività consente di gestire la sincronizzazione del pacchetto CDC con il pacchetto di caricamento iniziale e la gestione di intervalli di numeri di sequenza del file di log (LSN) elaborati in un'esecuzione di un pacchetto CDC. L'attività di controllo CDC, inoltre, consente di gestire gli scenari di errore e il recupero da errori.  
  
 L'attività di controllo CDC consente di gestire lo stato del pacchetto CDC in una variabile del pacchetto SSIS e di renderlo persistente in una tabella di database, in modo che lo stato venga mantenuto tra attivazioni del pacchetto e tra più pacchetti che eseguono insieme un processo CDC comune. Un'attività, ad esempio, può essere responsabile del caricamento iniziale e l'altra degli aggiornamenti trickle-feed.  
  
 L'attività di controllo CDC supporta due gruppi di operazioni. Mediante un gruppo viene gestita la sincronizzazione del caricamento iniziale e dell'elaborazione delle modifiche; mediante l'altro gruppo viene gestito l'intervallo di elaborazione delle modifiche degli LSN per un'esecuzione di un pacchetto CDC e di tenere traccia degli elementi elaborati correttamente.  
  
 Nelle operazioni seguenti viene gestita la sincronizzazione del caricamento iniziale e dell'elaborazione delle modifiche:  
  
|Operazione|Descrizione|  
|---------------|-----------------|  
|ResetCdcState|Questa operazione viene utilizzata per reimpostare lo stato CDC persistente associato al contesto CDC corrente. Dopo l'esecuzione di questa operazione, l'LSN massimo corrente della tabella LSN-timestamp `sys.fn_cdc_get_max_lsn` diventa l'inizio dell'intervallo di elaborazione successivo. Per questa operazione è necessaria una connessione al database di origine.|  
|MarkInitialLoadStart|Questa operazione viene utilizzata all'inizio di un pacchetto di caricamento iniziale per registrare l'LSN corrente nel database di origine prima che tramite il pacchetto di caricamento iniziale venga avviata la lettura delle tabelle di origine. Per questa operazione è necessaria una connessione al database di origine per chiamare `sys.fn_cdc_get_max_lsn`.<br /><br /> Se si seleziona MarkInitialLoadStart quando si usa CDC di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] , ovvero non di Oracle, l'utente specificato nella gestione connessione deve essere db_owner o sysadmin.|  
|MarkInitialLoadEnd|Questa operazione viene utilizzata alla fine di un pacchetto di caricamento iniziale per registrare l'LSN corrente nel database di origine dopo che il pacchetto di caricamento iniziale completa la lettura delle tabelle di origine. Questo LSN viene determinato registrando la data e l'ora correnti in cui si è verificata l'operazione ed eseguendo quindi una query sulla tabella di mapping `cdc.lsn_time_`nel database CDC per trovare una modifica successiva alla data e all'ora.<br /><br /> Se si seleziona MarkInitialLoadEnd quando si usa CDC di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] , ovvero non di Oracle, l'utente specificato nella gestione connessione deve essere db_owner o sysadmin.|  
|MarkCdcStart|Questa operazione viene utilizzata quando il caricamento iniziale viene eseguito da un database snapshot. In questo caso, l'elaborazione delle modifiche deve iniziare immediatamente dopo l'LSN dello snapshot. È possibile specificare il nome del database snapshot da utilizzare e l'attività di controllo CDC esegue una query su [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] per trovare l'LSN dello snapshot. È inoltre possibile scegliere di specificare direttamente l'LSN dello snapshot.<br /><br /> Se si seleziona MarkCdcStart quando si usa CDC di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] , ovvero non di Oracle, l'utente specificato nella gestione connessione deve essere db_owner o sysadmin.|  
  
 Le operazioni seguenti vengono utilizzate per gestire l'intervallo di elaborazione:  
  
|Operazione|Descrizione|  
|---------------|-----------------|  
|GetProcessingRange|Questa operazione viene utilizzata prima di richiamare il flusso di dati che utilizza il flusso di dati dell'origine CDC. L'operazione consente di stabilire un intervallo di LSN letti dal flusso di dati dell'origine CDC quando il flusso di dati viene richiamato. L'intervallo viene archiviato in una variabile del pacchetto SSIS utilizzata dall'origine CDC durante l'elaborazione del flusso di dati.<br /><br /> Per altre informazioni sugli stati che vengono archiviati, vedere [Definire una variabile di stato](../../integration-services/data-flow/define-a-state-variable.md).|  
|MarkProcessedRange|Questa operazione avviene dopo ogni esecuzione CDC (una volta completato il flusso di dati CDC) per registrare l'ultimo LSN elaborato completamente nell'esecuzione CDC. Alla successiva esecuzione di GetProcessingRange, questa posizione corrisponde all'inizio dell'intervallo di elaborazione.|  
  
## <a name="handling-cdc-state-persistency"></a>Gestione della persistenza dello stato CDC  
 L'attività di controllo CDC consente di gestire uno stato persistente tra attivazioni. Le informazioni archiviate nello stato CDC vengono utilizzate per determinare e gestire l'intervallo di elaborazione per il pacchetto CDC e per il rilevamento di eventuali condizioni di errore. Lo stato persistente viene archiviato come stringa. Per altre informazioni, vedere [Definire una variabile di stato](../../integration-services/data-flow/define-a-state-variable.md).  
  
 L'attività di controllo CDC supporta due tipi di persistenza dello stato  
  
-   Persistenza dello stato manuale: in questo caso, l'attività di controllo CDC gestisce lo stato archiviato in una variabile del pacchetto, ma lo sviluppatore del pacchetto deve leggere la variabile da un archivio persistente prima di chiamare il controllo CDC e quindi riscriverla nell'archivio persistente dopo l'ultima chiamata del controllo CDC e il completamento dell'esecuzione CDC.  
  
-   Persistenza dello stato automatica: lo stato CDC viene archiviato in una tabella di un database. Lo stato viene archiviato con un nome specificato nella proprietà **StateName** in una tabella denominata nella proprietà **Table to Use for Storing State** , inclusa in una gestione connessione selezionata per l'archiviazione dello stato. Il valore predefinito è la gestione connessione di origine, ma secondo la pratica comune è consigliabile impostare il valore sulla gestione connessione di destinazione. Tramite l'attività di controllo CDC viene aggiornato il valore di stato nella tabella di stato e viene eseguito il commit di tale valore come parte della transazione di ambiente.  
  
## <a name="error-handling"></a>Gestione degli errori  
 È possibile che l'attività di controllo CDC restituisca un errore nei casi seguenti:  
  
-   Non è possibile leggere lo stato CDC persistente o l'aggiornamento dello stato persistente non riesce.  
  
-   Non è possibile leggere le informazioni LSN correnti dal database di origine.  
  
-   Lo stato CDC letto non è coerente.  
  
 In tutti questi casi, l'attività di controllo CDC restituisce un errore che può essere gestito tramite la modalità standard di gestione degli errori del flusso di controllo in SSIS.  
  
 L'attività di controllo CDC può inoltre restituire un avviso quando l'operazione GetProcessingRange viene richiamata direttamente dopo un'altra operazione GetProcessingRange senza che venga chiamata l'operazione MarkProcessedRange. Ciò indica che l'esecuzione precedente non è riuscita o che è possibile che un altro pacchetto CDC con lo stesso nome di stato CDC sia in esecuzione.  
  
## <a name="configuring-the-cdc-control-task"></a>Configurazione dell'attività di controllo CDC  
 È possibile impostare le proprietà tramite Progettazione SSIS o a livello di codice.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Proprietà personalizzate dell'attività di controllo CDC](../../integration-services/control-flow/cdc-control-task-custom-properties.md)  
  
## <a name="related-tasks"></a>Attività correlate  
 [Definire una variabile di stato](../../integration-services/data-flow/define-a-state-variable.md)  
  
## <a name="related-content"></a>Contenuto correlato  
  
-   Articolo tecnico [Installing Microsoft SQL Server 2012 Change Data Capture for Oracle by Attunity](https://go.microsoft.com/fwlink/?LinkId=252958)(Installazione di Microsoft SQL Server 2012 Change Data Capture per Oracle di Attunity) nel sito Web social.technet.microsoft.com.  
  
-   Articolo tecnico [Troubleshoot Configuration Issues in Microsoft Change Data Capture for Oracle by Attunity](https://go.microsoft.com/fwlink/?LinkId=252960)(Risoluzione dei problemi di configurazione in Microsoft Change Data Capture per Oracle di Attunity) nel sito Web social.technet.microsoft.com.  
  
-   Articolo tecnico [Troubleshoot CDC Instance Errors in Microsoft Change Data Capture for Oracle by Attunity](https://go.microsoft.com/fwlink/?LinkId=252961)(Risoluzione degli errori di istanze di CDC in Microsoft Change Data Capture per Oracle di Attunity) nel sito Web social.technet.microsoft.com.  
  
## <a name="cdc-control-task-editor"></a>Editor attività Controllo CDC
  Utilizzare la finestra di dialogo **CDC Control Task Editor** per configurare l'attività di controllo CDC. La configurazione dell'attività di controllo CDC include la definizione di una connessione al database CDC, l'operazione dell'attività CDC e le informazioni sulla gestione dello stato.  
  
 Per ulteriori informazioni sull'attività di controllo CDC, vedere [CDC Control Task](../../integration-services/control-flow/cdc-control-task.md).  
  
 **Per aprire CDC Control Task Editor**  
  
1.  In [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]aprire il pacchetto [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] contenente l'attività di controllo CDC.  
  
2.  Nella scheda **Flusso di controllo** fare doppio clic sull'attività di controllo CDC.  
  
### <a name="options"></a>Opzioni  
 **Gestione connessione ADO.NET per database CDC di SQL Server**  
 Selezionare una gestione connessione esistente nell'elenco o creare una nuova connessione facendo clic su **Nuova** . La connessione deve essere stabilita a un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] abilitato per CDC e in cui si trova la tabella delle modifiche selezionata.  
  
 **Operazione di controllo CDC**  
 Selezionare l'operazione da eseguire per questa attività. Per tutte le operazioni viene utilizzata la variabile di stato archiviata in una variabile del pacchetto SSIS utilizzata per archiviare lo stato e passarlo tra i diversi componenti nel pacchetto.  
  
-   **Contrassegna avvio caricamento iniziale**: questa operazione viene utilizzata durante l'esecuzione di un caricamento iniziale da un database attivo senza uno snapshot. Viene richiamata all'inizio di un pacchetto di caricamento iniziale per registrare il valore LSN corrente nel database di origine prima che venga avviata la lettura delle tabelle di origine. Questa operazione richiede una connessione al database di origine.  
  
     Se si seleziona **Contrassegna avvio caricamento iniziale** quando si usa CDC di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] (ovvero, non Oracle), l'utente specificato nella gestione connessione deve essere  **db_owner** o **sysadmin**.  
  
-   **Contrassegna fine caricamento iniziale**: questa operazione viene utilizzata durante l'esecuzione di un caricamento iniziale da un database attivo senza uno snapshot. Viene richiamata alla fine di un pacchetto di caricamento iniziale per registrare l'LSN corrente nel database di origine al termine della lettura delle tabelle di origine. Questo LSN viene determinato registrando l'ora corrente in cui l'operazione si è verificata ed eseguendo quindi una query sulla tabella `cdc.lsn_time_`mapping nel database CDC per ricercare una modifica successiva a tale ora  
  
     Se si seleziona **Contrassegna fine caricamento iniziale** quando si usa CDC di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] (ovvero, non Oracle), l'utente specificato nella gestione connessione deve essere  **db_owner** o **sysadmin**.  
  
-   **Contrassegna avvio CDC**: questa operazione viene usata quando il caricamento iniziale è costituito da un database snapshot o da un database disattivato. Viene richiamata in qualsiasi punto all'interno del pacchetto di caricamento iniziale. L'operazione accetta un parametro che può essere un LSN snapshot, un nome di un database snapshot (da cui l'LSN snapshot viene automaticamente derivato) o può essere lasciato vuoto, nel qual caso l'LSN del database corrente viene utilizzato come LSN iniziale per il pacchetto di elaborazione delle modifiche.  
  
     Questa operazione viene utilizzata al posto delle operazioni Contrassegna avvio caricamento iniziale/Contrassegna fine caricamento iniziale.  
  
     Se si seleziona **Contrassegna avvio CDC** quando si usa CDC di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] (ovvero, non Oracle), l'utente specificato nella gestione connessione deve essere  **db_owner** o **sysadmin**.  
  
-   **Ottieni intervallo di elaborazione**: questa operazione viene utilizzata in un pacchetto di elaborazione delle modifiche prima di richiamare il flusso di dati che utilizza il flusso di dati dell'origine CDC. L'operazione consente di stabilire un intervallo di LSN letti dal flusso di dati dell'origine CDC quando il flusso di dati viene richiamato. L'intervallo viene archiviato in una variabile del pacchetto SSIS utilizzata dall'origine CDC durante l'elaborazione del flusso di dati.  
  
     Per altre informazioni sui possibili stati CDC che vengono archiviati, vedere [Definire una variabile di stato](../../integration-services/data-flow/define-a-state-variable.md).  
  
-   **Contrassegna intervallo elaborato**: questa operazione viene usata in un pacchetto di elaborazione delle modifiche alla fine di un'esecuzione CDC (dopo il corretto completamento del flusso di dati CDC) per registrare l'ultimo LSN elaborato completamente nell'esecuzione CDC. Alla prossima esecuzione di `GetProcessingRange` , questa posizione determina l'inizio dell'intervallo di elaborazione successivo.  
  
-   **Reimposta stato CDC**: questa operazione viene utilizzata per reimpostare lo stato CDC persistente associato al contesto CDC corrente. Dopo l'esecuzione di questa operazione, l'LSN massimo corrente della tabella LSN-timestamp `sys.fn_cdc_get_max_lsn` diventa l'inizio dell'intervallo di elaborazione successivo. Per questa operazione è necessaria una connessione al database di origine.  
  
     Questa operazione viene ad esempio utilizzata quando si desidera elaborare solo i record di modifica appena creati e ignorare tutti i record di modifica obsoleti.  
  
 **Variabile contenente lo stato CDC**  
 Selezionare la variabile del pacchetto SSIS utilizzata per archiviare le informazioni di stato relative all'operazione dell'attività. Prima di iniziare, è necessario definire una variabile. Se si seleziona **AutomaticStatePersistence**, la variabile di stato viene caricata e salvata automaticamente.  
  
 Per altre informazioni sulla definizione della variabile di stato, vedere [Definire una variabile di stato](../../integration-services/data-flow/define-a-state-variable.md).  
  
 **LSN SQL Server per avviare il nome snapshot/CDC:**  
 Digitare l'LSN del database di origine corrente o il nome del database snapshot da cui viene eseguito il caricamento iniziale per determinare l'inizio di CDC. Questa opzione è disponibile solo se si imposta **Operazione di controllo CDC** su **Contrassegna avvio CDC**.  
  
 Per ulteriori informazioni su queste operazioni, vedere [CDC Control Task](../../integration-services/control-flow/cdc-control-task.md)  
  
 **Archivia automaticamente lo stato in una tabella di database**  
 Selezionare questa casella di controllo se si desidera che tramite l'attività di controllo CDC vengano gestiti automaticamente il caricamento e l'archiviazione dello stato CDC in una tabella di stato contenuta nel database specificato. Se la casella di controllo è deselezionata, lo sviluppatore deve caricare lo stato CDC quando il pacchetto viene avviato e salvarlo ogni volta che lo stato CDC cambia.  
  
 **Gestione connessione per il database in cui è archiviato lo stato**  
 Selezionare una gestione connessione ADO.NET esistente dall'elenco o creare una nuova connessione facendo clic su Nuova. Questa connessione è a un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che contiene la tabella Stato. La tabella Stato contiene le informazioni sullo stato.  
  
 È disponibile solo se si seleziona **AutomaticStatePersistence** ed è un parametro obbligatorio.  
  
 **Tabella da utilizzare per l'archiviazione dello stato**  
 Digitare il nome della tabella di stato da utilizzare per l'archiviazione dello stato CDC. La tabella specificata deve disporre di due colonne denominate **name** e **state** , entrambe dello stesso tipo di dati **varchar (256)**.  
  
 Facoltativamente, è possibile selezionare **Nuova** per ottenere uno script SQL che compila una nuova tabella Stato con le colonne obbligatorie. Se **AutomaticStatePersistence** è selezionato, lo sviluppatore deve creare una tabella di stato in base ai requisiti elencati in precedenza.  
  
 È disponibile solo se si seleziona **AutomaticStatePersistence** ed è un parametro obbligatorio.  
  
 **Nome dello stato**  
 Digitare un nome da associare allo stato CDC persistente. Un nome dello stato comune verrà specificato dal caricamento completo e dai pacchetti CDC che utilizzano lo stesso contesto. Questo nome viene utilizzato per cercare la riga di stato nella tabella di stato  
  
