---
description: Debug di un flusso di dati
title: Debug di un flusso di dati | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- progress reporting [Integration Services]
- data viewers [Integration Services]
- data flow [Integration Services], debugging
- debugging [Integration Services], data flow
- counting rows
ms.assetid: 1c574f1b-54f7-4c05-8e42-8620e2c1df0f
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c69bed5fbec76fab380f91515827630fa60938a5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342163"
---
# <a name="debugging-data-flow"></a>Debug di un flusso di dati

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] e Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] includono funzionalità e strumenti che è possibile usare per la risoluzione dei problemi dei flussi di dati in un pacchetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
-   [!INCLUDE[ssIS](../../includes/ssis-md.md)] Progettazione offre visualizzatori dati.  
  
-   [!INCLUDE[ssIS](../../includes/ssis-md.md)] Progettazione e le trasformazioni di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] offrono conteggi delle righe.  
  
-   [!INCLUDE[ssIS](../../includes/ssis-md.md)] Progettazione genera report di stato in fase di runtime.  
  
## <a name="data-viewers"></a>Visualizzatori dati  
 I visualizzatori dati visualizzano i dati scambiati tra due componenti in un flusso di dati. I dati possono essere visualizzati quando vengono estratti da un'origine dei dati e immessi per la prima volta in un flusso di dati, prima e dopo l'aggiornamento da parte di una trasformazione e prima che vengano caricati nella destinazione.  
  
 Per visualizzare i dati è necessario collegare visualizzatori dati al percorso che connette i due componenti flusso di dati. La possibilità di visualizzare i dati scambiati tra due componenti flusso di dati semplifica l'identificazione di valori di dati imprevisti, consente di vedere come vengono modificati i valori delle colonne da parte di una trasformazione e di individuare i motivi per cui una trasformazione non riesce. Nel caso in cui, ad esempio, una ricerca in una tabella di riferimento abbia esito negativo, per risolvere il problema potrebbe essere necessario aggiungere una trasformazione che fornisce dati predefiniti per le colonne vuote.  
  
 Un visualizzatore consente di visualizzare i dati in una griglia. Se si utilizza una griglia, è possibile selezionare le colonne da visualizzare. I valori per le colonne selezionate verranno visualizzati in formato tabulare.  
  
 È inoltre possibile collegare più visualizzatori dati in uno stesso percorso e visualizzare gli stessi dati in formati diversi, creando ad esempio una vista grafico e una visualizzazione griglia, oppure creare visualizzatori dati diversi per colonne di dati diverse.  
  
 Quando si aggiunge un visualizzatore dati a un percorso, Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] aggiunge un'icona di visualizzatore dati all'area di progettazione della scheda **Flusso di dati** , accanto al percorso. Le trasformazioni che possono avere più output, come la trasformazione Suddivisione condizionale, possono includere un visualizzatore dati per ogni percorso.  
  
 In fase di runtime viene visualizzata la finestra **Visualizzatore dati** in cui sono disponibili le informazioni specificate dal formato del visualizzatore dati. Un visualizzatore che utilizza un formato griglia, ad esempio, visualizza i dati per le colonne selezionate, il numero delle righe di output passate al componente flusso di dati e il numero delle righe visualizzate. Le informazioni vengono visualizzate un buffer dopo l'altro e il numero delle righe contenuto in ogni buffer dipende dalla larghezza delle righe nel flusso di dati.  
  
 Nella finestra di dialogo **Visualizzatore dati** è possibile copiare i dati negli Appunti, cancellare tutti i dati dalla tabella, riconfigurare il visualizzatore dati, riprendere il flusso di dati e collegare o scollegare il visualizzatore dati.  
  
#### <a name="to-add-a-data-viewer"></a>Per aggiungere un visualizzatore dati  
  
-   [Aggiunta di un visualizzatore dati a un flusso di dati](#add_viewer)  
  
## <a name="row-counts"></a>Conteggi delle righe  
 Il numero delle righe passate lungo un percorso viene visualizzato nell'area di progettazione della scheda **Flusso di dati** di Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] , accanto al percorso. Tale numero viene aggiornato periodicamente mentre i dati si spostano lungo il percorso.  
  
 Nel flusso di dati è inoltre possibile aggiungere una trasformazione Conteggio righe per l'acquisizione del conteggio di righe finale in una variabile. Per altre informazioni, vedere [Trasformazione Conteggio righe](../../integration-services/data-flow/transformations/row-count-transformation.md).  
  
## <a name="progress-reporting"></a>Report di stato  
 Quando si esegue un pacchetto, Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] delinea i progressi compiuti nell'area di progettazione della scheda **Flusso dati** visualizzando ogni componente del flusso dati in un colore che ne indica lo stato. Quando ogni componente, inizialmente privo di colore, comincia a eseguire le operazioni previste, assume il colore giallo e, al termine, assume il colore verde. Tramite il colore rosso viene indicato che l'esecuzione del componente non è riuscita.  
  
 Il significato dei colori è descritto nella tabella seguente.  
  
|Colore|Descrizione|  
|-----------|-----------------|  
|Nessuno|In attesa di essere chiamato dal motore flusso di dati.|  
|Giallo|È in corso una trasformazione, un'operazione di estrazione o un'operazione di caricamento dei dati.|  
|Green|L'esecuzione è stata completata.|  
|rosso|Il componente è stato eseguito con errori.|  

## <a name="analysis-of-data-flow"></a>Analisi del flusso di dati
  È possibile usare la vista [execution_data_statistics](../../integration-services/system-views/catalog-execution-data-statistics.md) del database **SSISDB** per analizzare il flusso di dati dei pacchetti. In questa vista viene visualizzata una riga ogni volta che un componente del flusso di dati invia dati a un componente a valle. Le informazioni possono essere utilizzate per acquisire una comprensione più approfondita delle righe inviate a ciascun componente.  
  
> [!NOTE]  
>  Il livello di registrazione deve essere impostato su **Dettagliato** per acquisire informazioni con la vista catalog.execution_data_statistics.  
  
 Nell'esempio seguente viene visualizzato il numero di righe inviate tra i componenti di un pacchetto.  
  
```sql
use SSISDB  
select package_name, task_name, source_component_name, destination_component_name, rows_sent  
from catalog.execution_data_statistics  
where execution_id = 132  
order by source_component_name, destination_component_name   
```  
  
 Nell'esempio seguente viene calcolato il numero di righe per millisecondi inviate da ogni componente per un'esecuzione specifica. I valori calcolati sono:  
  
-   **total_rows** : somma di tutte le righe inviate dal componente  
  
-   **wall_clock_time_ms**: tempo totale di esecuzione trascorso, in millisecondi, per ogni componente  
  
-   **num_rows_per_millisecond**: numero di righe per millisecondi inviate da ogni componente  
  
 La clausola **HAVING** viene usata per impedire un errore di divisione per zero nei calcoli.  
  
```sql  
use SSISDB  
select source_component_name, destination_component_name,  
    sum(rows_sent) as total_rows,  
    DATEDIFF(ms,min(created_time),max(created_time)) as wall_clock_time_ms,  
    ((0.0+sum(rows_sent)) / (datediff(ms,min(created_time),max(created_time)))) as [num_rows_per_millisecond]  
from [catalog].[execution_data_statistics]  
where execution_id = 132  
group by source_component_name, destination_component_name  
having (datediff(ms,min(created_time),max(created_time))) > 0  
order by source_component_name desc  
```  

## <a name="configure-an-error-output-in-a-data-flow-component"></a>Configurazione di un output degli errori in un componente del flusso di dati
  Molti componenti del flusso di dati supportano l'output degli errori. A seconda del componente, in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] sono disponibili strumenti diversi per la configurazione di un output degli errori. Oltre a configurare un output degli errori, è possibile configurare le relative colonne, tra cui le colonne **ErrorCode** e **ErrorColumn** aggiunte dal componente.  
  
### <a name="configuring-an-error-output"></a>Configurazione di un output degli errori  
 Per configurare un output degli errori sono disponibili due opzioni:  
  
-   Usare la finestra di dialogo **Configura output errori** . Questa finestra di dialogo consente di configurare un output degli errori in qualsiasi componente del flusso di dati che supporti l'output degli errori.  
  
-   Utilizzare la finestra di dialogo dell'editor per il componente. Alcuni componenti consentono di configurare gli output degli errori direttamente dalla finestra di dialogo dell'editor. Non è tuttavia possibile configurare output degli errori dalla finestra di dialogo dell'editor per l'origine ADO NET, la trasformazione Importa colonna, la trasformazione Comando OLE DB o la destinazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact.  
  
 Nelle procedure seguenti viene descritto come utilizzare queste finestre di dialogo per configurare gli output degli errori.  
  
#### <a name="to-configure-an-error-output-using-the-configure-error-output-dialog-box"></a>Per configurare un output degli errori tramite la finestra di dialogo Configura output errori  
  
1.  In [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]aprire il progetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] che contiene il pacchetto desiderato.  
  
2.  In Esplora soluzioni fare doppio clic sul pacchetto per aprirlo.  
  
3.  In Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] fare clic sulla scheda **Flusso di dati** .  
  
4.  Trascinare l'output degli errori, rappresentato da una freccia rossa, dal componente corrispondente all'origine degli errori a un altro componente del flusso di dati.  
  
5.  Nella finestra di dialogo **Configura output errori** selezionare un'azione nelle colonne **Errore** e **Troncamento** per ogni colonna dell'input del componente.  
  
6.  Per salvare il pacchetto aggiornato, dal menu **File** scegliere **Salva elementi selezionati**.  
  
#### <a name="to-add-an-error-output-using-the-editor-dialog-box-for-the-component"></a>Per aggiungere un output degli errori tramite la finestra di dialogo dell'editor per il componente  
  
1.  In [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]aprire il progetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] che contiene il pacchetto desiderato.  
  
2.  In Esplora soluzioni fare doppio clic sul pacchetto per aprirlo.  
  
3.  In Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] fare clic sulla scheda **Flusso di dati** .  
  
4.  Fare doppio clic sui componenti del flusso di dati in cui si desidera configurare un output degli errori e, a seconda del componente, eseguire una delle operazioni seguenti:  
  
    -   Fare clic su **Configura output errori**.  
  
    -   Fare clic su **Output degli errori**.  
  
5.  Impostare l'opzione **Errore** per ogni colonna.  
  
6.  Impostare l'opzione **Troncamento** per ogni colonna.  
  
7.  Fare clic su **OK**.  
  
8.  Per salvare il pacchetto aggiornato, dal menu **File** scegliere **Salva elementi selezionati**.  
  
### <a name="configuring-error-output-columns"></a>Configurazione delle colonne dell'output degli errori  
 Per configurare le colonne dell'output degli errori, è necessario usare la scheda **Proprietà input e output** della finestra di dialogo **Editor avanzato** .  
  
#### <a name="to-configure-error-output-columns"></a>Per configurare le colonne dell'output degli errori  
  
1.  In [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]aprire il progetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] che contiene il pacchetto desiderato.  
  
2.  In Esplora soluzioni fare doppio clic sul pacchetto per aprirlo.  
  
3.  In Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] fare clic sulla scheda **Flusso di dati** .  
  
4.  Fare clic con il pulsante destro del mouse sul componente di cui si desidera configurare le colonne dell'output degli errori, quindi scegliere **Visualizza editor avanzato**.  
  
5.  Fare clic sulla scheda **Proprietà input e output**. Espandere **Output errori \<component name>** , quindi **Colonne di output**.  
  
6.  Fare clic su una colonna e aggiornarne le proprietà.  
  
    > [!NOTE]  
    >  L'elenco di colonne include le colonne dell'input del componente, le colonne **ErrorCode** e **ErrorColumn** aggiunte dall'output degli errori precedente e le colonne **ErrorCode** e **ErrorColumn** aggiunte dal componente corrente.  
  
7.  Scegliere **OK.**  
  
8.  Per salvare il pacchetto aggiornato, dal menu **File** scegliere **Salva elementi selezionati**.  

## <a name="add-a-data-viewer-to-a-data-flow"></a><a name="add_viewer"></a> Aggiunta di un visualizzatore dati a un flusso di dati
  In questo argomento viene descritta la procedura per l'aggiunta e la configurazione di un visualizzatore dati in un flusso di dati. In un visualizzatore dati vengono visualizzati i dati in transito tra due componenti flusso di dati, ad esempio i dati estratti da un'origine dei dati prima che vengano modificati da una trasformazione del flusso di dati.  
  
 Un percorso collega i componenti in un flusso di dati connettendo l'output di un componente all'input dell'altro.  
  
 Per poter aggiungere visualizzatori dati in un pacchetto, è necessario che il pacchetto includa un'attività Flusso di dati e almeno due componenti flusso di dati connessi tra di loro.  
  
 Aggiungere un visualizzatore dati a un output di errore per visualizzare la descrizione dell'errore e il nome della colonna in cui si è verificato l'errore. Per impostazione predefinita l'output di errore include solo gli identificatori numerici per l'errore e la colonna.  
  
### <a name="to-add-a-data-viewer-to-a-data-flow"></a>Per aggiungere un visualizzatore dati in un flusso di dati  
  
1.  In [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]aprire il progetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] che contiene il pacchetto desiderato.  
  
2.  In Esplora soluzioni fare doppio clic sul pacchetto per aprirlo.  
  
3.  Fare clic sulla scheda **Flusso di controllo** se non è già visualizzata.  
  
4.  Fare clic sull'attività Flusso di dati nel cui flusso di dati si vuole aggiungere un visualizzatore dati e quindi dare clic sulla scheda **Flusso di dati** .  
  
5.  Fare clic con il pulsante destro del mouse su un percorso tra due componenti del flusso di dati e quindi scegliere **Modifica**.  
  
6.  Nella pagina **Generale** è possibile visualizzare e modificare le proprietà del percorso. Ad esempio, nell'elenco a discesa **PathAnnotation** è possibile selezionare l'annotazione visualizzata accanto al percorso.  
  
7.  Nella pagina **Metadati** è possibile visualizzare i metadati delle colonne e copiarli negli Appunti.  
  
8.  Nella pagina **Visualizzatore dati** fare clic su **Abilita visualizzatore dati**.  
  
9. Nell'area Colonne da visualizzare selezionare le colonne che si desidera visualizzare nel visualizzatore dati. Per impostazione predefinita, nell'elenco **Colonne visualizzate** sono elencate e selezionate tutte le colonne disponibili. Spostare le colonne da non usare nell'elenco **Colonne inutilizzate** selezionandole e facendo clic sulla freccia sinistra.  
  
    > [!NOTE]  
    >  I valori nella griglia che rappresentano i tipi di dati DT_DATE, DT_DBTIME2, DT_FILETIME, DT_DBTIMESTAMP, DT_DBTIMESTAMP2 e DT_DBTIMESTAMPOFFSET vengono visualizzati come stringhe in formato ISO 8601. Uno spazio separatore sostituisce il separatore **T** . I valori che rappresentano i tipi di dati DT_DATE e DT_FILETIME includono sette cifre per i secondi frazionari. Poiché il tipo di dati DT_FILETIME memorizza solo tre cifre dei secondi frazionari, nella griglia viene visualizzato zero per le quattro cifre rimanenti. I valori che rappresentano il tipo di dati DT_DBTIMESTAMP includono tre cifre per i secondi frazionari. Per i valori che rappresentano i tipi di dati DT_DBTIME2, DT_DBTIMESTAMP2 e DT_DBTIMESTAMPOFFSET, il numero di cifre dei secondi frazionari corrisponde alla scala specificata per il tipo di dati della colonna. Per altre informazioni sui formati ISO 8601, vedere [Formati di data e ora](../data-flow/parsing-data.md). Per altre informazioni sui tipi di dati, vedere [Tipi di dati di Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
10. Fare clic su **OK**.  

## <a name="data-flow-taps"></a>Scelte del flusso di dati
 È possibile aggiungere una scelta dei dati in un percorso del flusso di dati di un pacchetto in fase di esecuzione e indirizzare l'output della scelta dei dati a un file esterno. Per utilizzare questa funzionalità, è necessario distribuire il progetto SSIS utilizzando il modello di distribuzione del progetto in un server SSIS. Al termine della distribuzione del pacchetto nel server, è necessario eseguire script T-SQL nel database SSISDB per aggiungere le scelte dei dati prima dell'esecuzione del pacchetto. Di seguito è riportato uno scenario di esempio:  
  
1.  Creare un'istanza di esecuzione di un pacchetto usando la stored procedure [catalog.create_execution &#40;database SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md).  
  
2.  Aggiungere una scelta dei dati usando la stored procedure [catalog.add_data_tap](../../integration-services/system-stored-procedures/catalog-add-data-tap.md) o [catalog.add_data_tap_by_guid](../../integration-services/system-stored-procedures/catalog-add-data-tap-by-guid.md) .  
  
3.  Avviare l'istanza di esecuzione del pacchetto usando [catalog.start_execution &#40;databse SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-start-execution-ssisdb-database.md).  
  
 Di seguito è riportato uno script SQL di esempio tramite cui vengono eseguiti i passaggi descritti nello scenario sopra indicato:  
  
```sql
Declare @execid bigint  
EXEC [SSISDB].[catalog].[create_execution] @folder_name=N'ETL Folder', @project_name=N'ETL Project', @package_name=N'Package.dtsx', @execution_id=@execid OUTPUT  
EXEC [SSISDB].[catalog].add_data_tap @execution_id = @execid, @task_package_path = '\Package\Data Flow Task', @dataflow_path_id_string = 'Paths[Flat File Source.Flat File Source Output]', @data_filename = 'output.txt'  
EXEC [SSISDB].[catalog].[start_execution] @execid  
```  
  
 I parametri del nome della cartella, del nome del progetto e del nome del pacchetto della stored procedure create_execution corrispondono ai nomi della cartella, del progetto e del pacchetto nel catalogo di Integration Services. È possibile ottenere i nomi della cartella, del progetto e del pacchetto da utilizzare nella chiamata a create_execution da SQL Server Management Studio come illustrato nella figura riportata di seguito. Se non viene visualizzato il progetto SSIS in questo punto, non può ancora essere distribuito nel server SSIS. Fare clic con il pulsante destro del mouse sul progetto SSIS in Visual Studio e scegliere Distribuisci per distribuire il progetto nel server SSIS previsto.  
  
 Anziché digitare le istruzioni SQL, è possibile generare lo script di esecuzione del pacchetto effettuando i passaggi seguenti:  
  
1.  Fare clic con il pulsante destro del mouse su **Package.dtsx** , quindi scegliere **Esegui**.  
  
2.  Fare clic sul pulsante della barra degli strumenti **Script** per generare lo script.  
  
3.  A questo punto, aggiungere l'istruzione add_data_tap prima della chiamata a start_execution.  
  
 Il parametro task_package_path della stored procedure add_data_tap corrisponde alla proprietà PackagePath dell'attività Flusso di dati in Visual Studio. In Visual Studio fare clic con il pulsante destro del mouse su **Attività Flusso di dati** e scegliere **Proprietà** per avviare la relativa finestra.  Prendere nota del valore della proprietà **PackagePath** per usarlo come valore per il parametro task_package_path per la chiamata della stored procedure add_data_tap.  
  
 Il parametro dataflow_path_id_string della stored procedure add_data_tap corrisponde alla proprietà IdentificationString del percorso del flusso di dati a cui si desidera aggiungere una scelta dei dati. Per ottenere dataflow_path_id_string, fare clic sul percorso del flusso di dati (la freccia tra le attività nel flusso di dati) e prendere nota del valore della proprietà **IdentificationString** nella finestra Proprietà.  
  
 Quando si esegue lo script, il file di output viene archiviato in \<Program Files>\Microsoft SQL Server\110\DTS\DataDumps. Se esiste già un file con il nome, viene creato un nuovo file con un suffisso, ad esempio output[1].txt.  
  
 Come accennato in precedenza, è inoltre possibile usare la stored procedure [catalog.add_data_tap_by_guid](../../integration-services/system-stored-procedures/catalog-add-data-tap-by-guid.md)anziché la stored procedure add_data_tap. Questa stored procedure accetta l'ID dell'attività Flusso di dati come parametro anziché task_package_path. È possibile ottenere l'ID dell'attività Flusso di dati dalla finestra delle proprietà di Visual Studio.  
  
### <a name="removing-a-data-tap"></a>Rimozione di una scelta dei dati  
 È possibile rimuovere una scelta dei dati prima di avviare l'esecuzione usando la stored procedure [catalog.remove_data_tap](../../integration-services/system-stored-procedures/catalog-remove-data-tap.md) . Questa stored procedure accetta l'ID della scelta dei dati come parametro, che è possibile ottenere come output della stored procedure add_data_tap.  
  
```sql
DECLARE @tap_id bigint  
EXEC [SSISDB].[catalog].add_data_tap @execution_id = @execid, @task_package_path = '\Package\Data Flow Task', @dataflow_path_id_string = 'Paths[Flat File Source.Flat File Source Output]', @data_filename = 'output.txt' @data_tap_id=@tap_id OUTPUT  
EXEC [SSISDB].[catalog].remove_data_tap @tap_id  
```  
  
### <a name="listing-all-data-taps"></a>Elenco di tutte le scelte dei dati  
 È inoltre possibile elencare tutte le scelte dei dati tramite la vista catalog.execution_data_taps. Nell'esempio seguente vengono estratte le scelte dei dati per un'istanza di esecuzione specifica (ID: 54).  
  
```sql 
select * from [SSISDB].[catalog].execution_data_taps where execution_id=@execid  
```  
  
### <a name="performance-consideration"></a>Considerazione sulle prestazioni  
 L'abilitazione del livello di registrazione dettagliata e l'aggiunta di scelte dei dati determinano un aumento delle operazioni di I/O eseguite dalla soluzione di integrazione dei dati. Pertanto, è consigliabile aggiungere le scelte dei dati solo ai fini della risoluzione dei problemi.  
  
### <a name="video"></a>Video  
 In questo [video su TechNet](../../sql-server/index.yml) viene illustrato come aggiungere o usare le scelte dei dati nel catalogo di SQL Server 2012 SSISDB che facilita il debug dei pacchetti a livello di codice e l'acquisizione dei risultati parziali in fase di esecuzione. Inoltre vengono illustrate la modalità per elencare/rimuovere queste scelte dei dati e le procedure consigliate per l'utilizzo delle scelte dei dati nei pacchetti SSIS.  
 
## <a name="see-also"></a>Vedere anche  
 [Gestione degli errori nei dati](../../integration-services/data-flow/error-handling-in-data.md)  
  
