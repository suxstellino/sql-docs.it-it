---
description: Origine CDC
title: Origine CDC | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.designer.cdcsource.f1
- sql13.ssis.designer.cdcsource.connection.f1
- sql13.ssis.designer.cdcsource.columns.f1
- sql13.ssis.designer.cdcsource.errorhandling.f1
ms.assetid: 99775608-e177-44ed-bb44-aaccb0f4f327
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9c10e3f1aa1460f63f3fcd3e95bb95bef8e63065
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049211"
---
# <a name="cdc-source"></a>Origine CDC

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Tramite l'origine CDC viene letto un intervallo di dati delle modifiche da tabelle delle modifiche di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] ; queste vengono poi recapitate a valle ad altri componenti SSIS.  
  
 L'intervallo di dati delle modifiche letti dall'origine CDC è denominato intervallo di elaborazione CDC ed è determinato dall'attività di controllo CDC eseguita prima dell'inizio del flusso di dati corrente. L'Intervallo di elaborazione CDC viene derivato dal valore di una variabile del pacchetto che gestisce lo stato dell'elaborazione CDC per un gruppo di tabelle.  
  
 L'origine CDC estrae dati da un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite una tabella di database, una vista o un'istruzione SQL.  
  
 Per l'origine CDC vengono utilizzate le configurazioni seguenti:  
  
-   Una gestione connessione ADO.NET di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per accedere al database CDC di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per altre informazioni sulla configurazione della connessione dell'origine CDC, vedere [Editor origine CDC &#40;pagina Gestione connessione&#41;]().  
  
-   Una tabella abilitata per CDC.  
  
-   Il nome dell'istanza di acquisizione della tabella selezionata (se ne è presente più di una).  
  
-   La modalità di elaborazione delle modifiche.  
  
-   Il nome della variabile del pacchetto dello stato CDC in base alla quale viene determinato l'intervallo di elaborazione CDC. L'origine CDC non modifica questa variabile.  
  
 I dati restituiti dall'origine CDC sono identici a quelli restituiti dalle funzioni CDC di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **cdc.fn_cdc_get_all_changes_\<capture-instance-name>** o **cdc.fn_cdc_get_net_changes_\<capture-instance-name>** (se disponibili). L'unica aggiunta facoltativa è la colonna **__$initial_processing** , che indica se l'intervallo di elaborazione corrente può essere sovrapposto a un caricamento iniziale della tabella. Per ulteriori informazioni sull'elaborazione iniziale, vedere [Attività di controllo CDC](../../integration-services/control-flow/cdc-control-task.md).  
  
 L'origine CDC include un output regolare e un output degli errori.  
  
## <a name="error-handling"></a>Gestione degli errori  
 L'origine CDC include un output degli errori. L'output degli errori del componente include le colonne di output seguenti:  
  
-   **Error Code**: il valore è sempre -1.  
  
-   **Error Column**(Colonna errore): colonna di origine che provoca l'errore (per gli errori di conversione).  
  
-   **Error Row Columns**: dati del record che provocano l'errore.  
  
 A seconda dell'impostazione del comportamento in seguito all'errore, l'origine CDC supporta la restituzione degli errori (conversione dei dati, troncamento) che si verificano durante il processo di estrazione nell'output degli errori.  
  
## <a name="data-type-support"></a>Supporto dei tipi di dati  
 Il componente di origine CDC per Microsoft supporta tutti i tipi di dati [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , di cui viene eseguito il mapping ai tipi di dati SSIS corretti.  
  
## <a name="troubleshooting-the-cdc-source"></a>Risoluzione dei problemi relativi all'origine CDC  
 Di seguito vengono fornite informazioni sulla risoluzione dei problemi relativi all'origine CDC.  
  
### <a name="use-this-script-to-isolate-problems-and-reproduce-them-in-sql-server-management-studio"></a>Utilizzare questo script per isolare problemi e riprodurli in SQL Server Management Studio  
 Il funzionamento dell'origine CDC è regolato dal funzionamento dell'attività di controllo CDC eseguita prima di richiamare l'origine CDC. Tramite l'attività di controllo CDC viene preparato il valore della variabile del pacchetto dello stato CDC in modo da contenere gli LSN iniziale e finale. Viene eseguita la funzione equivalente allo script seguente:  
  
```sql
use <cdc-enabled-database-name>  
               declare @start_lsn binary(10), @end_lsn binary(10)  
               set @start_lsn = sys.fn_cdc_increment_lsn(  
                       convert(binary(10),'0x' + '<value-from-state-cs>', 1))  
               set @end_lsn =   
                       convert(binary(10),'0x' + '<value-from-state-ce>', 1)  
               select * from cdc.fn_cdc_get_net_changes_dbo_Table1(@start_lsn,  
@end_lsn, '<mode>')  
```  
  
 dove:  
  
-   \<cdc-enabled-database-name> è il nome del database [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che contiene le tabelle delle modifiche.  
  
-   \<value-from-state-cs> è il valore visualizzato nella variabile di stato CDC come CS/\<value-from-state-cs>/ (CS indica l'inizio dell'intervallo di elaborazione corrente).  
  
-   \<value-from-state-ce> è il valore visualizzato nella variabile di stato CDC come CE/\<value-from-state-cs>/ (CE indica la fine dell'intervallo di elaborazione corrente).  
  
-   \<mode> corrisponde alle modalità di elaborazione CDC. Le modalità di elaborazione hanno uno dei seguenti valori: **All**, **All with Old Values**, **Net**, **Net with Update Mask**, **Net with Merge**.  
  
 Questo script consente di isolare i problemi riproducendoli in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], in cui è possibile riprodurre e identificare facilmente gli errori.  
  
#### <a name="sql-server-error-message"></a>Messaggio di errore di SQL Server  
 È possibile che in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]venga restituito il messaggio di errore seguente:  
  
 **Numero di argomenti insufficiente per la routine o funzione cdc.fn_cdc_get_net_changes_\<..>.**  
  
 Questo errore non indica che un argomento è mancante. Indica invece che i valori LSN iniziale o finale nella variabile di stato CDC non sono validi.  
  
## <a name="configuring-the-cdc-source"></a>Configurazione dell'origine CDC  
 È possibile configurare l'origine CDC a livello di codice o tramite Progettazione SSIS.  
  
 Per ulteriori informazioni, vedere uno degli argomenti seguenti:  
  
-   [Editor origine CDC &#40;pagina Gestione connessione&#41;]()  
  
-   [Editor origine CDC &#40;pagina Colonne&#41;]()  
  
-   [Editor origine CDC &#40;pagina Output degli errori&#41;]()  
  
 La finestra di dialogo **Editor avanzato** contiene le proprietà che è possibile impostare a livello di codice.  
  
 Per aprire la finestra di dialogo **Editor avanzato** :  
  
-   Nella schermata **Flusso di dati** del progetto di [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] fare clic con il pulsante destro del mouse sull'origine CDC e scegliere **Visualizza editor avanzato**.  
  
 Per altre informazioni sulle proprietà che è possibile impostare nella finestra di dialogo **Editor avanzato** , vedere [Proprietà personalizzate dell'origine CDC](../../integration-services/data-flow/cdc-source-custom-properties.md).  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Proprietà personalizzate dell'origine CDC](../../integration-services/data-flow/cdc-source-custom-properties.md)  
  
-   [Estrarre dati delle modifiche tramite l'origine CDC](../../integration-services/data-flow/extract-change-data-using-the-cdc-source.md)  
  
## <a name="cdc-source-editor-connection-manager-page"></a>Editor origine CDC (pagina Gestione connessione)
  Usare la pagina **Gestione connessione** della finestra di dialogo **Editor origine CDC** per selezionare la gestione connessione ADO.NET per il database di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] da cui l'origine CDC legge le righe delle modifiche (database CDC). Dopo aver selezionato il database CDC è necessario selezionare una tabella acquisita nel database.  
  
 Per altre informazioni sull'origine CDC, vedere [Origine CDC](../../integration-services/data-flow/cdc-source.md).  
  
### <a name="task-list"></a>Elenco attività  
 **Per aprire CDC Source Editor (pagina Gestione connessione)**  
  
1.  In [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], aprire il pacchetto [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] che contiene l'origine CDC.  
  
2.  Nella scheda **Flusso di dati** fare doppio clic sull'origine CDC.  
  
3.  Nell' **Editor origine CDC** fare clic su **Gestione connessione**.  
  
### <a name="options"></a>Opzioni  
 **Gestione connessione ADO.NET**  
 Selezionare una gestione connessione esistente nell'elenco o creare una nuova connessione facendo clic su **Nuova** . La connessione deve essere stabilita a un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] abilitato per CDC e in cui si trova la tabella delle modifiche selezionata.  
  
 **Nuovo**  
 Fare clic su **Nuovo**. Verrà visualizzata la finestra di dialogo **Editor della gestione connessione ADO.NET** in cui è possibile creare una nuova gestione connessione  
  
 **CDC Table**  
 Selezionare la tabella di origine CDC contenente le modifiche acquisite che si desidera leggere e inviare ai componenti SSIS a valle per l'elaborazione.  
  
 **Istanza di acquisizione**  
 Selezionare o digitare il nome dell'istanza di acquisizione CDC con la tabella CDC da leggere.  
  
 Una tabella di origine acquisita può contenere una o due istanze acquisite per gestire la transizione senza problemi della definizione di tabella mediante modifiche dello schema. Se per la tabella di origine in corso di acquisizione sono definite più istanze di acquisizione, selezionare l'istanza di acquisizione che si desidera utilizzare a questo punto. Il nome dell'istanza di acquisizione predefinito per una tabella [schema].[tabella] è \<schema>_\<table>, ma i nomi delle istanze di acquisizione effettivi in uso possono essere diversi. L'effettiva tabella da cui viene eseguita la lettura è la tabella CDC **cdc .\<capture-instance>_CT**.  
  
 **CDC Processing Mode**  
 Selezionare la modalità di elaborazione più adatta per le esigenze di elaborazione correnti. Di seguito sono elencate le opzioni possibili:  
  
-   **All**: restituisce le modifiche nell'intervallo CDC corrente senza i valori **Before Update** .  
  
-   **All with old values**: restituisce le modifiche nell'intervallo di elaborazione CDC corrente inclusi i valori precedenti (**Before Update**). Ogni operazione di aggiornamento prevede due righe: una con i valori prima dell'aggiornamento e una con i valori dopo l'aggiornamento.  
  
-   **Net**: restituisce una sola riga delle modifiche per ogni riga di origine modificata nell'intervallo di elaborazione CDC corrente. Se una riga di origine è stata aggiornata più volte, viene restituita la modifica combinata (ad esempio, inserimento+aggiornamento viene prodotto come un singolo aggiornamento e aggiornamento+eliminazione viene prodotto come una singola eliminazione). Quando si utilizza la modalità di elaborazione delle modifiche Net, è possibile suddividere le modifiche negli output Delete, Insert e Update e gestirli in parallelo, perché la singola riga di origine viene visualizzata in più output.  
  
-   **Net with update mask** (Net con maschera di aggiornamento): questa modalità è simile alla modalità Net standard, ma aggiunge anche colonne booleane con il modello di nome **__$\<column-name>\___Changed**, che indica la presenza di colonne modificate nella riga delle modifiche corrente.  
  
-   **Net with merge**: questa modalità è simile alla modalità Net standard, ma con le operazioni Insert e Update unite in una singola operazione Merge (UPSERT).  
  
> [!NOTE]  
>  Per tutte le opzioni di modifica Net, è necessario che la tabella di origine disponga di una chiave primaria o di un indice univoco. Per tabelle senza una chiave primaria o indici univoci, usare l'opzione **All** .  
  
 **Variabile contenente lo stato CDC**  
 Selezionare la variabile del pacchetto di stringhe SSIS che gestisce lo stato CDC per il contesto CDC corrente. Per altre informazioni sulla variabile di stato CDC, vedere [Definire una variabile di stato](../../integration-services/data-flow/define-a-state-variable.md).  
  
 **Include reprocessing indicator column**  
 Selezionare questa casella di controllo per creare una colonna di output speciale denominata **__$reprocessing**.  
  
 Questa colonna contiene un valore **true** quando l'intervallo di elaborazione CDC si sovrappone all'intervallo di elaborazione iniziale (l'intervallo di LSN che corrisponde al periodo di caricamento iniziale) o quando un intervallo di elaborazione CDC viene rielaborato a causa di un errore in un'esecuzione precedente. Questa colonna indicatore consente agli sviluppatori di SSIS di gestire gli errori in modo diverso durante la rielaborazione delle modifiche. Azioni quali l'eliminazione di una riga non esistente e l'inserimento non riuscito su una chiave duplicata, ad esempio, possono essere ignorate.  
  
 Per altre informazioni, vedere [Proprietà personalizzate dell'origine CDC](../../integration-services/data-flow/cdc-source-custom-properties.md).  
  
## <a name="cdc-source-editor-columns-page"></a>Editor origine CDC (pagina Colonne)
  Usare la pagina **Colonne** della finestra di dialogo **CDC Source Editor (Editor origine CDC)** per eseguire il mapping tra una colonna di output e ogni colonna esterna (di origine).  
  
### <a name="task-list"></a>Elenco attività  
 **Per aprire CDC Source Editor (pagina Colonne)**  
  
1.  In [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], aprire il pacchetto [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] che contiene l'origine CDC.  
  
2.  Nella scheda **Flusso di dati** fare doppio clic sull'origine CDC.  
  
3.  In **CDC Source Editor**, fare clic su **Colonne**.  
  
### <a name="options"></a>Opzioni  
 **Colonne esterne disponibili**  
 Elenco delle colonne esterne disponibili nell'origine dei dati. Non è possibile utilizzare questa tabella per l'aggiunta o l'eliminazione di colonne. Selezionare le colonne da utilizzare nell'origine. Le colonne scelte vengono aggiunte all'elenco **Colonna esterna** nell'ordine in cui vengono selezionate.  
  
 **Colonna esterna**  
 Una vista delle colonne esterne (di origine) nell'ordine in cui vengono presentate durante la configurazione di componenti che utilizzano i dati dell'origine CDC. Per modificare questo ordine, deselezionare innanzitutto le colonne selezionate nell'elenco **Colonne esterne disponibili** , quindi selezionare le colonne esterne nell'elenco secondo un ordine diverso. Le colonne scelte vengono aggiunte all'elenco **Colonna esterna** nell'ordine in cui vengono selezionate.  
  
 **Colonna di output**  
 Consente di immettere un nome univoco per ogni colonna di output. Per impostazione predefinita viene suggerito il nome della colonna esterna (di origine) selezionata. È comunque possibile scegliere qualsiasi nome descrittivo univoco. Il nome immesso viene visualizzato in Progettazione SSIS.  
  
## <a name="cdc-source-editor-error-output-page"></a>Editor origine CDC (pagina Output degli errori)
  Usare la pagina **Output degli errori** della finestra di dialogo **Editor origine CDC** per selezionare le opzioni di gestione degli errori.  
  
### <a name="task-list"></a>Elenco attività  
 **Per aprire CDC Source Editor (pagina Output degli errori)**  
  
1.  In [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], aprire il pacchetto [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] che contiene l'origine CDC.  
  
2.  Nella scheda **Flusso di dati** fare doppio clic sull'origine CDC.  
  
3.  In **Editor origine CDC** fare clic su **Output degli errori**.  
  
### <a name="options"></a>Opzioni  
 **Input/Output**  
 Consente di visualizzare il nome dell'origine dei dati.  
  
 **Colonna**  
 Consente di visualizzare le colonne esterne (di origine) selezionate nella pagina **Gestione connessione** della finestra di dialogo **Editor origine CDC** .  
  
 **Error (Errore) (Error (Errore)e)**  
 Consente di selezionare il modo in cui l'origine CDC deve gestire gli errori in un flusso: ignorare l'errore, reindirizzare la riga o interrompere il componente.  
  
 **Troncamento**  
 Consente di selezionare il modo in cui l'origine CDC deve gestire il troncamento in un flusso: ignorare l'errore, reindirizzare la riga o interrompere il componente.  
  
 **Descrizione**  
 Non usato.  
  
 **Imposta questo valore nelle celle selezionate**  
 Consente di selezionare il modo in cui l'origine CDC gestisce tutte le celle selezionate in caso di errore o troncamento: ignorare l'errore, reindirizzare la riga o interrompere il componente.  
  
 **Applica**  
 Consente di applicare le opzioni di gestione degli errori alle celle selezionate.  
  
### <a name="error-handling-options"></a>Opzioni di gestione degli errori  
 Utilizzare le opzioni seguenti per configurare il modo in cui l'origine CDC gestisce errori e troncamenti.  
  
 **Interrompi componente**  
 Quando si verifica un errore o un troncamento l'attività Flusso di dati viene interrotta. Questo è il comportamento predefinito.  
  
 **Ignora errore**  
 L'errore o il troncamento viene ignorato e la riga di dati viene indirizzata all'output dell'origine CDC.  
  
 **Reindirizza flusso**  
 La riga di dati contenente l'errore o il troncamento viene indirizzata all'output degli errori dell'origine CDC. In questo caso, viene utilizzata la gestione degli errori dell'origine CDC. Per altre informazioni, vedere [Origine CDC](../../integration-services/data-flow/cdc-source.md).  
  
## <a name="related-content"></a>Contenuto correlato  
  
-   Intervento nel blog sulle [modalità di elaborazione per l'origine CDC](https://www.mattmasson.com/2012/01/processing-modes-for-the-cdc-source/)sul sito Web mattmasson.com.  
  
