---
description: Informazioni su Change Data Capture (SQL Server)
title: Informazioni su Change Data Capture
ms.custom: seo-dt-2019
ms.date: 01/14/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: vanto
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- change data capture [SQL Server], about
- change data capture [SQL Server]
- 22832 (Database Engine error)
ms.assetid: 7d8c4684-9eb1-4791-8c3b-0f0bb15d9634
author: rothja
ms.author: jroth
ms.openlocfilehash: f90e59f3e54b69a98e7ca058da971677faaafd0d
ms.sourcegitcommit: e5664d20ed507a6f1b5e8ae7429a172a427b066c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/19/2020
ms.locfileid: "97697113"
---
# <a name="about-change-data-capture-sql-server"></a>Informazioni su Change Data Capture (SQL Server)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]



  La funzionalità Change Data Capture registra le attività di inserimento, aggiornamento ed eliminazione che si applicano a una tabella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rendendo disponibili i dettagli delle modifiche in un formato relazionale facilmente utilizzabile. Le informazioni sulla colonna e i metadati necessari per applicare le modifiche a un ambiente di destinazione vengono acquisiti per le righe modificate e archiviati in tabelle delle modifiche che riflettono la struttura della colonna delle tabelle di origine con rilevamento. Per consentire ai consumer di accedere in modo sistematico ai dati delle modifiche, sono disponibili funzioni con valori di tabella.  
  
 Un ottimo esempio di consumer di dati a cui è rivolta questa tecnologia è un'applicazione di estrazione, trasformazione e caricamento (ETL). Un'applicazione di questo tipo carica incrementalmente dati delle modifiche dalle tabelle di origine di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un data warehouse oppure in un data mart. Anche se la rappresentazione delle tabelle di origine all'interno del data warehouse deve riflettere le modifiche in tali tabelle, una tecnologia end-to-end che aggiorna una replica dell'origine non è appropriata. È necessario invece un flusso affidabile di dati delle modifiche strutturato in modo che i consumer possano applicarlo a rappresentazioni di destinazione dei dati diverse. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Change Data Capture fornisce questa tecnologia.  
  
## <a name="change-data-capture-data-flow"></a>Flusso di dati di Change Data Capture  
 Nella figura seguente viene illustrato il flusso di dati principale per Change Data Capture.  
  
 ![Flusso di dati di Change Data Capture](../../relational-databases/track-changes/media/cdcdataflow.gif "Flusso di dati di Change Data Capture")  
  
 L'origine dei dati delle modifiche per la funzionalità Change Data Capture è data dal log delle transazioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Man mano che alle tabelle di origine con rilevamento vengono applicati inserimenti, aggiornamenti ed eliminazioni, le voci che descrivono tali modifiche vengono aggiunte al log. Il log viene utilizzato come input per il processo di acquisizione Il log viene letto e le informazioni relative alle modifiche vengono aggiunte alla tabella delle modifiche associata alla tabella con rilevamento. Per enumerare le modifiche visualizzate nelle tabelle delle modifiche in un intervallo specificato, sono disponibili diverse funzioni che restituiscono le informazioni in un set di risultati filtrato. Tale set di risultati viene utilizzato in genere da un processo dell'applicazione per aggiornare una rappresentazione dell'origine in alcuni ambienti esterni.  
  
## <a name="understanding-change-data-capture-and-the-capture-instance"></a>Informazioni su Change Data Capture e l'istanza di acquisizione  
 Affinché sia possibile rilevare le modifiche apportate a qualsiasi tabella singola di un database, è necessario che Change Data Capture venga abilitato in modo esplicito per il database. Per eseguire questa operazione, usare la stored procedure [sys.sp_cdc_enable_db](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-db-transact-sql.md). Quando il database è abilitato, le tabelle di origine possono essere identificate come tabelle con rilevamento usando la stored procedure [sys.sp_cdc_enable_table](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md). Quando la funzionalità Change Data Capture viene abilitata per una tabella, per supportare la distribuzione dei dati delle modifiche nella tabella di origine viene creata un'istanza di acquisizione associata costituita da una tabella delle modifiche e da una o due funzioni della query. I metadati che descrivono i dettagli di configurazione dell'istanza di acquisizione vengono mantenuti nelle tabelle di metadati di Change Data Capture **cdc.change_tables**, **cdc.index_columns** e **cdc.captured_columns**. È possibile recuperare queste informazioni usando la stored procedure [sys.sp_cdc_help_change_data_capture](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md).  
  
 Tutti gli oggetti associati a un'istanza di acquisizione vengono creati nello schema relativo a Change Data Capture del database abilitato. Il nome dell'istanza di acquisizione deve essere un nome di oggetto valido e univoco in tutte le istanze di acquisizione del database. Per impostazione predefinita, il nome è \<*schema name*\_*table name*> della tabella di origine. Il nome assegnato alla tabella delle modifiche associata viene creato aggiungendo **_CT** al nome dell'istanza di acquisizione. Il nome assegnato alla funzione usata per eseguire una query relativa a tutte le modifiche viene creato anteponendo **fn_cdc_get_all_changes_** al nome dell'istanza di acquisizione. Se l'istanza di acquisizione è configurata per supportare **net changes**, viene creata anche la funzione della query **net_changes**, cui viene assegnato un nome creato anteponendo **fn_cdc_get_net_changes\_** al nome dell'istanza di acquisizione.  
  
## <a name="change-table"></a>Tabella delle modifiche  
 Le prime cinque colonne di una tabella delle modifiche di Change Data Capture sono costituite da metadati che forniscono informazioni aggiuntive attinenti alla modifica registrata. Le colonne rimanenti rispecchiano le colonne acquisite identificate dalla tabella di origine nel nome e, generalmente, nel tipo. Tali colonne contengono i dati delle colonne acquisite raccolti dalla tabella di origine.  
  
 Ogni operazione di inserimento o eliminazione applicata a una tabella di origine viene visualizzata in un'unica riga all'interno della tabella delle modifiche. Le colonne di dati della riga che costituisce il risultato di un'operazione di inserimento contengono i valori della colonna dopo l'inserimento, mentre le colonne di dati della riga che costituisce il risultato di un'operazione di eliminazione contengono i valori della colonna prima dell'eliminazione. Per eseguire un'operazione di aggiornamento sono necessarie due voci di riga, una per identificare i valori della colonna prima dell'aggiornamento e l'altra per identificare i valori della colonna dopo l'aggiornamento.  
  
 Ogni riga di una tabella delle modifiche contiene inoltre metadati aggiuntivi che consentono di interpretare l'attività di modifica. La colonna __$start_lsn identifica il numero di sequenza del file di log (LSN) del commit assegnato alla modifica. Il valore LSN di commit identifica le modifiche di cui è stato eseguito il commit all'interno della stessa transazione e ordina inoltre tali transazioni. La colonna \_\_$seqval può essere usata per ordinare più modifiche che si verificano all'interno di una stessa transazione, La colonna \_\_$operation registra l'operazione associata alla modifica: 1 = eliminazione, 2 = inserimento, 3 = aggiornamento (prima dell'immagine) e 4 = aggiornamento (dopo l'immagine). La colonna \_\_$update_mask, infine, è una maschera di bit variabile con un bit definito per ogni colonna acquisita. Per le voci relative all'inserimento e all'eliminazione dei dati, tutti i bit della maschera di aggiornamento verranno sempre impostati. Per le righe aggiornate, tuttavia, saranno impostati solo i bit corrispondenti alle colonne modificate.  
  
## <a name="change-data-capture-validity-interval-for-a-database"></a>Intervallo di validità di Change Data Capture per un database  
 L'intervallo di validità di Change Data Capture per un database è rappresentato dal periodo di tempo durante il quale i dati delle modifiche sono disponibili per le istanze di acquisizione. L'intervallo di validità ha inizio nel momento in cui viene creata la prima istanza di acquisizione e si estende fino al momento corrente.  
  
 Se non vengono eliminati in modo periodico e sistematico, i dati inseriti nelle tabelle delle modifiche aumenteranno notevolmente e non sarà più possibile gestirli. Il processo di pulizia di Change Data Capture è responsabile dell'applicazione dei criteri di pulizia basati sulla memorizzazione. Tale processo sposta innanzitutto l'endpoint inferiore dell'intervallo di validità in modo da soddisfare la restrizione relative al tempo e successivamente rimuove le voci della tabella delle modifiche scadute. Per impostazione predefinita, i dati vengono conservati per tre giorni.  
  
 Considerato che il processo di acquisizione esegue il commit di ogni nuovo batch di dati delle modifiche, a livello dell'endpoint superiore le nuove voci vengono aggiunte a **cdc.lsn_time_mapping** per ogni transazione cui sono associate voci della tabella delle modifiche. Nella tabella di mapping vengono mantenuti sia un numero di sequenza del file di log (LSN) del commit che l'ora in cui è stato eseguito il commit della transazione (colonne start_lsn e tran_end_time, rispettivamente). Il valore LSN massimo presente in **cdc.lsn_time_mapping** rappresenta il limite superiore della finestra di validità del database. L'ora corrispondente in cui viene eseguito il commit viene utilizzata come base per il calcolo del nuovo limite inferiore da parte del criterio di pulizia basato sulla memorizzazione.  
  
 Poiché il processo di acquisizione estrae i dati delle modifiche dal log delle transazioni, è presente una latenza predefinita tra il momento in cui viene eseguito il commit di una modifica in una tabella di origine e quello in cui la modifica viene visualizzata nella tabella delle modifiche associate. Mentre tale latenza è in genere bassa, è tuttavia importante tenere presente che i dati delle modifiche non sono disponibili fino a quando il processo di acquisizione non ha elaborato le voci di log correlate.  
  
## <a name="change-data-capture-validity-interval-for-a-capture-instance"></a>Intervallo di validità di Change Data Capture per un'istanza di acquisizione  
 Sebbene l'intervallo di validità per il database e quello per l'istanza di acquisizione singola in genere coincidano, questa situazione non si verifica sempre. L'intervallo di validità dell'istanza di acquisizione viene avviato quando il processo di acquisizione riconosce l'istanza e avvia la registrazione delle modifiche associate nella relativa tabella. Di conseguenza, se le istanze di acquisizione vengono create in momenti diversi, a ciascuna sarà associato inizialmente un endpoint inferiore diverso. La colonna start_lsn del set di risultati restituito da [sys.sp_cdc_help_change_data_capture](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md) visualizza l'endpoint basso corrente per ogni istanza di acquisizione definita. Quando il processo di pulizia elimina le voci della tabella delle modifiche, modifica anche i valori di start_lsn affinché tutte le istanze di acquisizione riflettano il nuovo limite inferiore per i dati delle modifiche disponibili. Vengono modificate solo le istanze di acquisizione per cui i valori di start_lsn sono attualmente minori rispetto al nuovo limite inferiore. Con il tempo, se non viene creata alcuna nuova istanza di acquisizione, gli intervalli di validità per tutte le istanze singole tenderanno a coincidere con l'intervallo di validità per il database.  
  
 L'intervallo di validità è importante per i consumer dei dati delle modifiche perché l'intervallo di estrazione per una richiesta deve essere interamente compreso nell'intervallo di validità di Change Data Capture per l'istanza di acquisizione corrente. Se l'endpoint inferiore dell'intervallo di estrazione è minore dell'endpoint inferiore dell'intervallo di validità, alcuni dati delle modifiche potrebbero non essere disponibili a causa di una pulizia eccessiva. Se invece l'endpoint superiore dell'intervallo di estrazione è maggiore dell'endpoint superiore dell'intervallo di validità, il processo di acquisizione non ha ancora eseguito alcuna elaborazione durante il periodo di tempo rappresentato dall'intervallo di estrazione, provocando anche in questo caso la non disponibilità dei dati delle modifiche.  
  
 La funzione [sys.fn_cdc_get_min_lsn](../../relational-databases/system-functions/sys-fn-cdc-get-min-lsn-transact-sql.md) consente di recuperare il valore LSN minimo corrente per un'istanza di acquisizione, mentre [sys.fn_cdc_get_max_lsn](../../relational-databases/system-functions/sys-fn-cdc-get-max-lsn-transact-sql.md) consente di recuperare il valore LSN massimo corrente. In caso di esecuzione di una query sui dati delle modifiche, se l'intervallo LSN specificato non è compreso tra questi due valori LSN, non sarà possibile eseguire le funzioni della query relative a Change Data Capture.  
  
## <a name="handling-changes-to-source-tables"></a>Gestione delle modifiche nelle tabelle di origine  
 L'adattamento delle modifiche delle colonne nelle tabelle di origine per cui deve essere eseguito il rilevamento delle modifiche costituisce un problema di difficile risoluzione per i consumer a valle. Sebbene l'abilitazione di Change Data Capture in una tabella di origine non impedisca il verificarsi di tali modifiche DDL, tale funzionalità consente tuttavia di mitigare l'effetto sui consumer consentendo ai set di risultati recapitati restituiti tramite l'API di rimanere invariati anche se la struttura della colonna della tabella di origine sottostante subisce modifiche. Tale struttura fissa viene riflessa anche nella tabella delle modifiche sottostante alla quale accedono le funzioni della query definite.  
  
 Per adattare una tabella delle modifiche con struttura della colonna fissa, il processo di acquisizione responsabile del popolamento della tabella delle modifiche ignorerà qualsiasi nuova colonna non identificata per l'acquisizione al momento dell'abilitazione di Change Data Capture per la tabella di origine. Se una colonna con rilevamento viene eliminata, per tale colonna verranno specificati valori Null nelle voci di modifica successive. Tuttavia, se una colonna esistente subisce una modifica relativa al tipo di dati, la modifica viene propagata alla tabella delle modifiche per garantire che il meccanismo di acquisizione non provochi una perdita di dati nelle colonne con rilevamento. Il processo di acquisizione invia inoltre tutte le modifiche rilevate nella struttura della colonna di tabelle con rilevamento alla tabella cdc.ddl_history. Gli utenti che vogliono ricevere un avviso relativo alle modifiche eventualmente apportate ad applicazioni a valle possono usare la stored procedure [sys.sp_cdc_get_ddl_history](../../relational-databases/system-stored-procedures/sys-sp-cdc-get-ddl-history-transact-sql.md).  
  
 Quando alla tabella di origine associata vengono applicate modifiche DDL, l'istanza di acquisizione corrente continuerà in genere a mantenere la propria forma. È tuttavia possibile creare una seconda istanza di acquisizione per la tabella che rifletta la nuova struttura della colonna. In questo modo il processo di acquisizione è in grado di trasferire le modifiche apportate alla stessa tabella di origine in due tabelle delle modifiche distinte con strutture della colonna diverse. Di conseguenza, mentre una tabella delle modifiche può continuare a essere utilizzata da programmi operativi correnti, la seconda può risultare utile in un ambiente di sviluppo in cui devono essere incorporati i nuovi dati della colonna. Se si consente al meccanismo di acquisizione di popolare entrambe le tabelle delle modifiche contemporaneamente, una transizione da una tabella all'altra può essere eseguita senza perdita di dati delle modifiche. Questa situazione può verificarsi tutte le volte che due cronologie di Change Data Capture si sovrappongono. Quando viene interessata la transizione, l'istanza di acquisizione obsoleta può essere rimossa.  
  
> [!NOTE]  
>  Il numero massimo di istanze di acquisizione che possono essere associate contemporaneamente a un'unica tabella di origine è due.  
  
## <a name="relationship-between-the-capture-job-and-the-transactional-replication-logreader"></a>Relazione tra il processo di acquisizione e l'agente di lettura log della replica transazionale  
 La logica del processo Change Data Capture è incorporata nella stored procedure [sp_replcmds](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md), una funzione del server interna compilata come parte del processo sqlservr.exe e usata anche dalla replica transazionale per raccogliere modifiche dal log delle transazioni. Quando per un database è abilitata solo la funzionalità Change Data Capture, per richiamare sp_replcmds viene creato il processo di acquisizione di SQL Server Agent relativo a Change Data Capture. Se è presente anche la replica, l'agente di lettura log transazionale viene utilizzato da solo per soddisfare le esigenze relative ai dati delle modifiche per entrambi i consumer. Questa strategia consente di ridurre in modo significativo le contese relative al log quando per lo stesso database sono abilitati sia la replica che Change Data Capture.  
  
 Il passaggio tra queste due modalità operative per l'acquisizione dei dati delle modifiche viene eseguito automaticamente tutte le volte che lo stato della replica di un database abilitato per Change Data Capture subisce una modifica.  
  
> [!IMPORTANT]  
>  Per entrambe le istanze della logica di acquisizione è necessario che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent sia in esecuzione affinché il processo venga eseguito.  
  
 L'attività principale del processo di acquisizione consiste nell'analisi del log e nella scrittura dei dati delle colonne e delle informazioni relative alla transazione nelle tabelle delle modifiche di Change Data Capture. Per garantire un limite coerente a livello di transazione in tutte le tabelle delle modifiche di Change Data Capture che popola, il processo di acquisizione apre ed esegue il commit della propria transazione a ogni ciclo di analisi. Il processo rileva il momento in cui per le tabelle viene abilitato Change Data Capture e include automaticamente tali tabelle nel set di tabelle per le quali viene eseguito il monitoraggio delle voci di modifica nel log. Analogamente, verrà rilevata anche la disabilitazione di Change Data Capture, provocando la rimozione della tabella di origine dal set di tabelle per le quali viene eseguito il monitoraggio dei dati delle modifiche. Quando l'elaborazione relativa a una sezione del log è completata, il processo di acquisizione segnala la logica di troncamento del log al server, che utilizza queste informazioni per identificare le voci di log idonee per il troncamento.  
  
> [!NOTE]  
>  Quando un database è abilitato per Change Data Capture, anche se la modalità di recupero è impostata sul recupero semplice, il punto di troncamento del log non avanzerà finché tutte le modifiche contrassegnate per l'acquisizione non sono state raccolte dal processo di acquisizione. Se il processo di acquisizione non è in esecuzione e sono disponibili modifiche per la raccolta, l'esecuzione di CHECKPOINT non comporterà il troncamento del log.  
  
 Il processo di acquisizione viene inoltre utilizzato per gestire la cronologia relativa alle modifiche DDL apportate alle tabelle con rilevamento. Le istruzioni DDL associate a Change Data Capture inseriscono voci nel log delle transazioni del database ogni volta che una tabella o un database per cui la funzionalità Change Data Capture è abilitata viene eliminato o ogni volta che vengono aggiunte, modificate o eliminate colonne di una tabella per cui tale funzionalità è abilitata. Tali voci di log vengono elaborate dal processo di acquisizione, che successivamente invia gli eventi DDL associati alla tabella cdc.ddl_history. È possibile ottenere informazioni sugli eventi DDL che interessano le tabelle con rilevamento usando la stored procedure [sys.sp_cdc_get_ddl_history](../../relational-databases/system-stored-procedures/sys-sp-cdc-get-ddl-history-transact-sql.md).  
  
## <a name="change-data-capture-agent-jobs"></a>Processi dell'agente di Change Data Capture  
 A un database abilitato per la funzionalità Change Data Capture sono in genere associati due processi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, uno usato per popolare le tabelle delle modifiche del database e uno responsabile della pulizia di tali tabelle. Entrambi i processi sono costituiti da un solo passaggio che esegue il comando [!INCLUDE[tsql](../../includes/tsql-md.md)] . Il comando [!INCLUDE[tsql](../../includes/tsql-md.md)] richiamato è una stored procedure definita di Change Data Capture che implementa la logica del processo. I processi vengono creati nel momento in cui per la prima tabella del database viene abilitato Change Data Capture. Mentre il processo di pulizia viene sempre creato, il processo di acquisizione verrà creato solo se non sono state definite pubblicazioni transazionali per il database. Il processo di acquisizione viene creato anche quando per un database sono abilitati sia Change Data Capture che la replica transazionale e il processo di lettura log transazionale viene rimosso perché al database non sono più associate pubblicazioni definite.  
  
 Sia il processo di acquisizione che quello di pulizia vengono creati utilizzando parametri predefiniti. Il processo di acquisizione viene avviato immediatamente. L'esecuzione è continua e la capacità di elaborazione prevede un massimo di 1.000 transazioni per ciclo di analisi con un'attesa di 5 secondi tra un ciclo e l'altro. Il processo di pulizia viene eseguito quotidianamente alle 02.00. Tale processo consente di memorizzare le voci della tabella delle modifiche per 4320 minuti o 3 giorni, rimuovendo un massimo di 5000 voci con una singola istruzione Delete.  
  
 I processi dell'agente di Change Data Capture vengono rimossi quando la funzionalità viene disabilitata per il database. Il processo di acquisizione può essere rimosso anche quando la prima pubblicazione viene aggiunta a un database e le funzionalità Change Data Capture e replica transazionale sono entrambe abilitate.  
  
 Internamente, i processi dell'agente Change Data Capture vengono creati e rilasciati usando rispettivamente le stored procedure [sys.sp_cdc_add_job](../../relational-databases/system-stored-procedures/sys-sp-cdc-add-job-transact-sql.md) e [sys.sp_cdc_drop_job](../../relational-databases/system-stored-procedures/sys-sp-cdc-drop-job-transact-sql.md). Tali stored procedure vengono inoltre esposte in modo che gli amministratori possano controllare la creazione e la rimozione dei processi.  
  
 Un amministratore non dispone di alcun controllo esplicito sulla configurazione predefinita dei processi dell'agente di Change Data Capture. A tale scopo viene fornita la stored procedure [sys.sp_cdc_change_job](../../relational-databases/system-stored-procedures/sys-sp-cdc-change-job-transact-sql.md) che consente di modificare i parametri di configurazione predefiniti. Inoltre, la stored procedure [sys.sp_cdc_help_jobs](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-jobs-transact-sql.md) consente di visualizzare i parametri di configurazione correnti. Sia il processo di acquisizione che quello di pulizia estraggono parametri di configurazione dalla tabella msdb.dbo.cdc_jobs all'avvio. Qualsiasi modifica apportata a tali valori usando [sys.sp_cdc_change_job](../../relational-databases/system-stored-procedures/sys-sp-cdc-change-job-transact-sql.md) non avrà effetto finché il processo non viene arrestato e riavviato.  
  
 Per avviare e arrestare i processi dell'agente di Change Data Capture, sono disponibili due stored procedure aggiuntive, [sys.sp_cdc_start_job](../../relational-databases/system-stored-procedures/sys-sp-cdc-start-job-transact-sql.md) e [sys.sp_cdc_stop_job](../../relational-databases/system-stored-procedures/sys-sp-cdc-stop-job-transact-sql.md).  
  
> [!NOTE]  
>  L'avvio e l'arresto del processo di acquisizione non comportano alcuna perdita dei dati delle modifiche, ma consentono solo di evitare che il processo esegua l'analisi del log per inserire voci di modifica nelle tabelle delle modifiche. Una strategia ragionevole per impedire che l'analisi del log aumenti il carico durante i periodi in cui la richiesta è massima consiste nell'arresto del processo e nel successivo riavvio quando la richiesta è diminuita.  
  
 Entrambi i processi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent sono stati progettati per essere sufficientemente flessibili e configurabili in modo da soddisfare le esigenze di base degli ambienti in cui viene usata la funzionalità Change Data Capture. In entrambi casi, tuttavia, le stored procedure sottostanti che forniscono le funzionalità di base sono state esposte per poter applicare ulteriore personalizzazione.  
  
 Change Data Capture non funziona correttamente quando il servizio Motore di database o SQL Server Agent è in esecuzione con l'account NETWORK SERVICE. Questa situazione può generare l'errore 22832.  
 
## <a name="working-with-database-and-table-collation-differences"></a>Uso di regole di confronto diverse tra database e tabella

È importante sapere che può accadere che alcune regole di confronto siano diverse tra il database e le colonne di una tabella configurata per Change Data Capture. CDC usa l'archiviazione provvisoria per popolare tabelle laterali. Se una tabella contiene colonne CHAR o VARCHAR con regole di confronto diverse dalle regole di confronto del database e se tali colonne archiviano caratteri non ASCII (ad esempio caratteri DBCS), è possibile che CDC non salvi in modo permanente i dati modificati coerentemente con i dati nelle tabelle di base. Ciò è dovuto al fatto che alle variabili dell'archiviazione provvisoria non possono essere associate regole di confronto.

Considerare uno degli approcci seguenti per verificare che i dati modificati acquisiti siano coerenti con le tabelle di base:

- Usare il tipo di dati NCHAR o NVARCHAR per colonne contenenti dati non ASCII.

- In alternativa, usare le stesse regole di confronto per le colonne e per il database.

Ad esempio, se un database usa le regole di confronto di SQL_Latin1_General_CP1_CI_AS, considerare la tabella seguente:

```sql
CREATE TABLE T1( 
     C1 INT PRIMARY KEY, 
     C2 VARCHAR(10) collate Chinese_PRC_CI_AI)
```

CDC potrebbe non acquisire i dati binari per la colonna C2, perché le regole di confronto sono diverse (Chinese_PRC_CI_AI). Per evitare questo problema, usare NVARCHAR:

```sql
CREATE TABLE T1( 
     C1 INT PRIMARY KEY, 
     C2 NVARCHAR(10) collate Chinese_PRC_CI_AI --Unicode data type, CDC works well with this data type)
```

## <a name="limitations"></a>Limitazioni

Change Data Capture presenta le limitazioni seguenti: 

**Linux**   
CDC è ora supportato per SQL Server 2017 in Linux a partire da CU18 e SQL Server 2019 in Linux.

**Indici columnstore**   
Change Data Capture non può essere abilitato per le tabelle con un indice columnstore cluster. A partire da SQL Server 2016, può essere abilitato per le tabelle con un indice columnstore non cluster.

**Cambio della partizione con variabili**   
L'uso di variabili con il cambio della partizione nei database o nelle tabelle con Change Data Capture (CDC) non è supportato per l'istruzione `ALTER TABLE ... SWITCH TO ... PARTITION ...`. Per altre informazioni, vedere [Limitazioni del cambio della partizione](../replication/publish/replicate-partitioned-tables-and-indexes.md#replication-support-for-partition-switching). 



## <a name="see-also"></a>Vedere anche  
 [Tenere traccia delle modifiche ai dati &#40;SQL Server&#41;](../../relational-databases/track-changes/track-data-changes-sql-server.md)   
 [Abilitare e disabilitare Change Data Capture &#40;SQL Server&#41;](../../relational-databases/track-changes/enable-and-disable-change-data-capture-sql-server.md)   
 [Usare Change Data &#40;SQL Server&#41;](../../relational-databases/track-changes/work-with-change-data-sql-server.md)   
 [Amministrare e monitorare Change Data Capture &#40;SQL Server&#41;](../../relational-databases/track-changes/administer-and-monitor-change-data-capture-sql-server.md)  
  
  
