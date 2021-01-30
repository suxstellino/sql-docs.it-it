---
description: sp_addmergepublication (Transact-SQL)
title: sp_addmergepublication (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addmergepublication
- sp_addmergepublication_TSQL
helpviewer_keywords:
- sp_addmergepublication
ms.assetid: 28a629a1-7374-4614-9b04-279d290a942a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f0c132ffc9315bafc28530afeef83f779c2975e7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209433"
---
# <a name="sp_addmergepublication-transact-sql"></a>sp_addmergepublication (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Viene creata una nuova pubblicazione di tipo merge. Questa stored procedure viene eseguita nel server di pubblicazione nel database in fase di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_addmergepublication [ @publication = ] 'publication'   
    [ , [ @description = ] 'description'   
    [ , [ @retention = ] retention ]   
    [ , [ @sync_mode = ] 'sync_mode' ]   
    [ , [ @allow_push = ] 'allow_push' ]   
    [ , [ @allow_pull = ] 'allow_pull' ]   
    [ , [ @allow_anonymous = ] 'allow_anonymous' ]   
    [ , [ @enabled_for_internet = ] 'enabled_for_internet' ]   
    [ , [ @centralized_conflicts = ] 'centralized_conflicts' ]   
    [ , [ @dynamic_filters = ] 'dynamic_filters' ]   
    [ , [ @snapshot_in_defaultfolder = ] 'snapshot_in_default_folder' ]   
    [ , [ @alt_snapshot_folder = ] 'alternate_snapshot_folder' ]   
    [ , [ @pre_snapshot_script = ] 'pre_snapshot_script' ]   
    [ , [ @post_snapshot_script = ] 'post_snapshot_script' ]   
    [ , [ @compress_snapshot = ] 'compress_snapshot' ]   
    [ , [ @ftp_address = ] 'ftp_address' ]   
    [ , [ @ftp_port = ] ftp_port ]   
    [ , [ @ftp_subdirectory = ] 'ftp_subdirectory' ]   
    [ , [ @ftp_login = ] 'ftp_login' ]   
    [ , [ @ftp_password = ] 'ftp_password' ]   
    [ , [ @conflict_retention = ] conflict_retention ]   
    [ , [ @keep_partition_changes = ] 'keep_partition_changes' ]   
    [ , [ @allow_subscription_copy = ] 'allow_subscription_copy' ]   
    [ , [ @allow_synctoalternate = ] 'allow_synctoalternate' ]   
    [ , [ @validate_subscriber_info = ] 'validate_subscriber_info' ]   
    [ , [ @add_to_active_directory = ] 'add_to_active_directory' ]   
    [ , [ @max_concurrent_merge = ] maximum_concurrent_merge ]   
    [ , [ @max_concurrent_dynamic_snapshots = ] max_concurrent_dynamic_snapshots ]  
    [ , [ @use_partition_groups = ] 'use_partition_groups' ]  
    [ , [ @publication_compatibility_level = ] 'backward_comp_level' ]  
    [ , [ @replicate_ddl = ] replicate_ddl ]  
    [ , [ @allow_subscriber_initiated_snapshot = ] 'allow_subscriber_initiated_snapshot' ]   
    [ , [ @allow_web_synchronization = ] 'allow_web_synchronization' ]   
    [ , [ @web_synchronization_url = ] 'web_synchronization_url' ]  
    [ , [ @allow_partition_realignment = ] 'allow_partition_realignment' ]  
    [ , [ @retention_period_unit = ] 'retention_period_unit' ]  
    [ , [ @generation_leveling_threshold = ] generation_leveling_threshold ]  
    [ , [ @automatic_reinitialization_policy = ] automatic_reinitialization_policy ]  
    [ , [ @conflict_logging = ] 'conflict_logging' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione di tipo merge da creare. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito e non deve essere la parola chiave all. Il nome della pubblicazione deve essere univoco all'interno del database.  
  
`[ @description = ] 'description'` Descrizione della pubblicazione. *Description* è di **tipo nvarchar (255)** e il valore predefinito è null.  
  
`[ @retention = ] retention` Periodo di memorizzazione, in unità del periodo di memorizzazione, per cui salvare le modifiche per la *pubblicazione* specificata. la *conservazione* è di **tipo int** e il valore predefinito è 14 unità. Le unità del periodo di memorizzazione sono definite da *retention_period_unit*. Se la sottoscrizione non viene sincronizzata entro il periodo di memorizzazione specificato e se le modifiche che tale sottoscrizione avrebbe dovuto ricevere sono state rimosse tramite un'operazione di rimozione nel server di distribuzione, la sottoscrizione scade e pertanto dovrà essere reinizializzata. Il periodo di memorizzazione massimo consentito equivale al numero di giorni tra il 31 dicembre 9999 e la data corrente.  
  
> [!NOTE]  
>  Il periodo di memorizzazione per le pubblicazioni di tipo merge è caratterizzato da un periodo di tolleranza di 24 ore per consentire l'adeguamento dei Sottoscrittori appartenenti a fusi orari diversi. Se, ad esempio, si imposta un periodo di memorizzazione di un giorno, il periodo di memorizzazione effettivo sarà di 48 ore.  
  
`[ @sync_mode = ] 'sync_mode'` Modalità della sincronizzazione iniziale dei sottoscrittori della pubblicazione. *sync_mode* è di **tipo nvarchar (10)**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**native** (impostazione predefinita)|Genera l'output in modalità nativa del programma per la copia bulk per tutte le tabelle.|  
|**character**|Genera l'output in modalità carattere del programma per la copia bulk per tutte le tabelle. Obbligatorio per supportare [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssEW](../../includes/ssew-md.md)] e non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sottoscrittori.|  
  
`[ @allow_push = ] 'allow_push'` Specifica se è possibile creare sottoscrizioni push per la pubblicazione specificata. *allow_push* è di **tipo nvarchar (5)** e il valore predefinito è true, che consente le sottoscrizioni push nella pubblicazione.  
  
`[ @allow_pull = ] 'allow_pull'` Specifica se è possibile creare sottoscrizioni pull per la pubblicazione specificata. *allow_pull* è di **tipo nvarchar (5)** e il valore predefinito è true, che consente le sottoscrizioni pull nella pubblicazione. È necessario specificare true per supportare i [!INCLUDE[ssEW](../../includes/ssew-md.md)] sottoscrittori.  
  
`[ @allow_anonymous = ] 'allow_anonymous'` Specifica se è possibile creare sottoscrizioni anonime per la pubblicazione specificata. *allow_anonymous* è di **tipo nvarchar (5)** e il valore predefinito è true, che consente sottoscrizioni anonime nella pubblicazione. Per supportare i [!INCLUDE[ssEW](../../includes/ssew-md.md)] sottoscrittori, è necessario specificare **true**.  
  
`[ @enabled_for_internet = ] 'enabled_for_internet'` Specifica se la pubblicazione è abilitata per Internet e determina se è possibile utilizzare il protocollo FTP (file Transfer Protocol) per trasferire i file di snapshot in un Sottoscrittore. *enabled_for_internet* è di **tipo nvarchar (5)** e il valore predefinito è false. Se **true**, i file di sincronizzazione della pubblicazione vengono inseriti nella directory C:\Programmi\Microsoft SQL Server\MSSQL\MSSQL.x\Repldata\Ftp. La directory Ftp deve essere creata dall'utente. Se **false**, la pubblicazione non è abilitata per l'accesso a Internet.  
  
`[ @centralized_conflicts = ] 'centralized_conflicts'` Questo parametro è deprecato ed è supportato solo per la compatibilità con le versioni precedenti degli script. Utilizzare *conflict_logging* per specificare il percorso in cui sono archiviati i record dei conflitti.  
  
`[ @dynamic_filters = ] 'dynamic_filters'` Consente alla pubblicazione di tipo merge di utilizzare i filtri di riga con parametri. *dynamic_filters* è di **tipo nvarchar (5)** e il valore predefinito è false.  
  
> [!NOTE]  
>  Si consiglia di non specificare questo parametro ma di consentire a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di determinare automaticamente se vengono utilizzati i filtri di riga con parametri. Se si specifica il valore **true** per *dynamic_filters*, è necessario definire un filtro di riga con parametri per l'articolo. Per altre informazioni, vedere [Definizione e modifica di un filtro di riga con parametri per un articolo di merge](../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md).  
  
`[ @snapshot_in_defaultfolder = ] 'snapshot_in_default_folder'` Specifica se i file di snapshot sono archiviati nella cartella predefinita. *snapshot_in_default_folder* è di **tipo nvarchar (5)** e il valore predefinito è true. Se **true**, i file di snapshot sono disponibili nella cartella predefinita. Se **false**, i file di snapshot verranno archiviati nella posizione alternativa specificata da *alternate_snapshot_folder*. Una posizione alternativa può essere un altro server, un'unità di rete oppure un supporto rimovibile, ad esempio un CD o un disco rimovibile. È inoltre possibile archiviare i file di snapshot in un sito FTP (File Transfer Protocol) in modo da poterli recuperare successivamente tramite il Sottoscrittore. Si noti che questo parametro può essere true e avere ancora un percorso specificato da *alt_snapshot_folder*. Tale combinazione indica che i file di snapshot vengono archiviati sia nella posizione predefinita che in posizioni alternative.  
  
`[ @alt_snapshot_folder = ] 'alternate_snapshot_folder'` Specifica la posizione della cartella alternativa per lo snapshot. *alternate_snapshot_folder* è di **tipo nvarchar (255)** e il valore predefinito è null.  
  
`[ @pre_snapshot_script = ] 'pre_snapshot_script'` Specifica un puntatore al percorso di un file con **estensione SQL** . *pre_snapshot_script* è di **tipo nvarchar (255)** e il valore predefinito è null. Durante l'applicazione dello snapshot in un Sottoscrittore, l'agente di merge esegue lo script pre-snapshot prima degli script degli oggetti replicati. Lo script viene eseguito nel contesto di sicurezza utilizzato dall'agente di merge durante la connessione al database di sottoscrizione. Gli script di pre-snapshot non vengono eseguiti nei [!INCLUDE[ssEW](../../includes/ssew-md.md)] sottoscrittori.  
  
`[ @post_snapshot_script = ] 'post_snapshot_script'` Specifica un puntatore al percorso di un file con **estensione SQL** . *post_snapshot_script* è di **tipo nvarchar (255)** e il valore predefinito è null. L'agente di merge esegue lo script post-snapshot dopo l'applicazione di tutti gli altri script di oggetti replicati e dei dati durante una sincronizzazione iniziale. Lo script viene eseguito nel contesto di sicurezza utilizzato dall'agente di merge durante la connessione al database di sottoscrizione. Gli script post-snapshot non vengono eseguiti nei [!INCLUDE[ssEW](../../includes/ssew-md.md)] sottoscrittori.  
  
`[ @compress_snapshot = ] 'compress_snapshot'`Specifica che lo snapshot scritto nella posizione **\@ alt_snapshot_folder** deve essere compresso nel [!INCLUDE[msCoName](../../includes/msconame-md.md)] formato CAB. *compress_snapshot* è di **tipo nvarchar (5)** e il valore predefinito è false. **false** specifica che lo snapshot non verrà compresso; **true** specifica che lo snapshot deve essere compresso. I file di snapshot di dimensioni superiori a 2GB non possono essere compressi. I file di snapshot compressi vengono decompressi nella posizione dove viene eseguito l'agente di merge; in genere le sottoscrizioni pull vengono utilizzate con gli snapshot compressi in modo che i file vengono decompressi nel Sottoscrittore. Non è possibile comprimere lo snapshot all'interno della cartella predefinita. Per supportare i [!INCLUDE[ssEW](../../includes/ssew-md.md)] sottoscrittori, è necessario specificare **false**.  
  
`[ @ftp_address = ] 'ftp_address'` È l'indirizzo di rete del servizio FTP per il server di distribuzione. *ftp_address* è di **tipo sysname** e il valore predefinito è null. Specifica la posizione dei file di snapshot della pubblicazione, dove i file possono essere prelevati dall'agente di merge di un Sottoscrittore. Poiché questa proprietà viene archiviata per ogni pubblicazione, ogni pubblicazione può avere un *ftp_address* diverso. La pubblicazione deve supportare la propagazione di snapshot tramite FTP.  
  
`[ @ftp_port = ] ftp_port` Numero di porta del servizio FTP per il server di distribuzione. *ftp_port* è di **tipo int** e il valore predefinito è 21. Specifica la posizione dei file di snapshot della pubblicazione, dove i file possono essere prelevati dall'agente di merge di un Sottoscrittore. Poiché questa proprietà viene archiviata per ogni pubblicazione, ogni pubblicazione può disporre di un proprio *ftp_port*.  
  
`[ @ftp_subdirectory = ] 'ftp_subdirectory'` Specifica la posizione in cui i file di snapshot saranno disponibili per l'agente di merge del Sottoscrittore se la pubblicazione supporta la propagazione di snapshot tramite FTP. *ftp_subdirectory* è di **tipo nvarchar (255)** e il valore predefinito è null. Poiché questa proprietà viene archiviata per ogni pubblicazione, ogni pubblicazione può disporre di un proprio *ftp_subdirctory* o scegliere di non avere sottodirectory, indicato con un valore null.  
  
 Durante la pregenerazione degli snapshot per le pubblicazioni con filtri con parametri, lo snapshot dei dati per ogni partizione del Sottoscrittore deve essere archiviato nella propria cartella. La struttura di directory per gli snapshot pregenerati tramite FTP deve rispettare la struttura seguente:  
  
 *alternate_snapshot_folder*\FTP \\ *publisher_publicationDB_publication* \\ *PartitionID*.  
  
> [!NOTE]  
>  I valori riportati sopra in corsivo dipendono dai dettagli della pubblicazione e dalla partizione del Sottoscrittore.  
  
`[ @ftp_login = ] 'ftp_login'` Nome utente utilizzato per la connessione al servizio FTP. *ftp_login* è di **tipo sysname** e il valore predefinito è "Anonymous".  
  
`[ @ftp_password = ] 'ftp_password'` Password utente utilizzata per la connessione al servizio FTP. *ftp_password* è di **tipo sysname** e il valore predefinito è null.  
  
> [!IMPORTANT]  
>  Non usare una password vuota. Usare una password complessa.  
  
`[ @conflict_retention = ] conflict_retention` Specifica il periodo di memorizzazione, espresso in giorni, per cui vengono conservati i conflitti. *conflict_retention* è di **tipo int** e il valore predefinito è 14 giorni prima che la riga con conflitti venga eliminata dalla tabella dei conflitti.  
  
`[ @keep_partition_changes = ] 'keep_partition_changes'` Specifica se abilitare le ottimizzazioni delle modifiche delle partizioni quando non è possibile utilizzare le partizioni pre-calcolate. *keep_partition_changes* è di **tipo nvarchar (5)** e il valore predefinito è true. **false** indica che le modifiche alle partizioni non sono ottimizzate e quando le partizioni pre-calcolate non vengono utilizzate, le partizioni inviate a tutti i sottoscrittori verranno verificate quando i dati vengono modificati in una partizione. **true** indica che le modifiche alle partizioni sono ottimizzate e che sono interessati solo i sottoscrittori con righe nelle partizioni modificate. Quando si utilizzano partizioni pre-calcolate, impostare *use_partition_groups* su **true** e impostare *keep_partition_changes* su **false**. Per altre informazioni, vedere [Ottimizzare le prestazioni dei filtri con parametri con le partizioni pre-calcolate](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md).  
  
> [!NOTE]  
>  Se si specifica il valore **true** per *keep_partition_changes*, specificare il valore **1** per il parametro **-MaxNetworkOptimization** di agente di snapshot. Per ulteriori informazioni su questo parametro, vedere [agente di snapshot di replica](../../relational-databases/replication/agents/replication-snapshot-agent.md). Per informazioni su come specificare i parametri degli agenti, vedere [Replication Agent Administration](../../relational-databases/replication/agents/replication-agent-administration.md).  
  
 Con i [!INCLUDE[ssEW](../../includes/ssew-md.md)] sottoscrittori, *keep_partition_changes* necessario impostare su true per assicurarsi che le eliminazioni vengano propagate correttamente. Se impostato su false, nel Sottoscrittore potrebbero essere presenti più righe rispetto al previsto.  
  
`[ @allow_subscription_copy = ] 'allow_subscription_copy'` Abilita o Disabilita la possibilità di copiare i database di sottoscrizione che sottoscrivono la pubblicazione. *allow_subscription_copy* è di **tipo nvarchar (5)** e il valore predefinito è false. Le dimensioni del database di sottoscrizione copiato devono essere inferiori a 2 gigabyte (GB)  
  
`[ @allow_synctoalternate = ] 'allow_synctoalternate'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @validate_subscriber_info = ] 'validate_subscriber_info'` Vengono elencate le funzioni utilizzate per definire una partizione del Sottoscrittore dei dati pubblicati quando vengono utilizzati i filtri di riga con parametri. *validate_subscriber_info* è di **tipo nvarchar (500)** e il valore predefinito è null. Queste informazioni vengono utilizzate dall'agente di merge per convalidare la partizione del Sottoscrittore. Se, ad esempio, [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) viene utilizzato nel filtro di riga con parametri, il parametro deve essere `@validate_subscriber_info=N'SUSER_SNAME()'` .  
  
> [!NOTE]  
>  Si consiglia di non specificare questo parametro e di consentire invece a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di determinare il criterio di filtro in modo automatico.  
  
`[ @add_to_active_directory = ] 'add_to_active_directory'` Questo parametro è deprecato ed è supportato solo per la compatibilità con le versioni precedenti degli script. Non è più possibile aggiungere informazioni di pubblicazione in [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory.  
  
`[ @max_concurrent_merge = ] maximum_concurrent_merge` Numero massimo di processi di merge simultanei. *maximum_concurrent_merge* è di **tipo int** e il valore predefinito è 0. Il valore **0** per questa proprietà significa che non esiste alcun limite al numero di processi di merge simultanei in esecuzione in un determinato momento. Questa proprietà imposta un limite per il numero di processi di merge che è possibile eseguire contemporaneamente in una pubblicazione di tipo merge. Se è stata pianificata l'esecuzione simultanea di un numero di processi maggiore del limite consentito, i processi in eccesso vengono inseriti in una coda dove rimangono in attesa fino al completamento del processo di merge attualmente in esecuzione.  
  
`[ @max_concurrent_dynamic_snapshots = ] max_concurrent_dynamic_snapshots` Numero massimo di sessioni di agente di snapshot che possono essere eseguite simultaneamente per generare snapshot dei dati filtrati per le partizioni del Sottoscrittore. *maximum_concurrent_dynamic_snapshots* è di **tipo int** e il valore predefinito è 0. Se è **0**, non esiste alcun limite per le sessioni di snapshot dei numeri. Se è stata pianificata l'esecuzione simultanea di un numero di processi di snapshot superiore al limite consentito, i processi in eccesso vengono inseriti in una coda in cui rimangono in attesa fino al completamento del processo di snapshot in esecuzione.  
  
`[ @use_partition_groups = ] 'use_partition_groups'` Specifica che le partizioni pre-calcolate devono essere utilizzate per ottimizzare il processo di sincronizzazione. *use_partition_groups* è di **tipo nvarchar (5)**. i possibili valori sono i seguenti:  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**true**|La pubblicazione utilizza partizioni pre-calcolate.|  
|**false**|La pubblicazione non utilizza partizioni pre-calcolate.|  
|NULL (impostazione predefinita)|Il sistema decide sulla strategia di partizionamento.|  
  
 Le partizioni pre-calcolate vengono utilizzate per impostazione predefinita. Per evitare l'utilizzo di partizioni pre-calcolate, *use_partition_groups* necessario impostare su **false**. Se è NULL, il sistema decide se è possibile utilizzare le partizioni pre-calcolate. Se non è possibile utilizzare le partizioni pre-calcolate, questo valore verrà effettivamente **impostato su false** senza generare errori. In questi casi, *keep_partition_changes* possibile impostare su **true** per fornire una certa ottimizzazione. Per ulteriori informazioni, vedere [filtri di riga con parametri](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md) e [ottimizzare le prestazioni dei filtri con parametri con le partizioni pre-calcolate](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md).  
  
`[ @publication_compatibility_level = ] backward_comp_level` Indica la compatibilità con le versioni precedenti della pubblicazione. *backward_comp_level* è di **tipo nvarchar (6)**. i possibili valori sono i seguenti:  
  
|Valore|Versione|  
|-----------|-------------|  
|**90RTM**|[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|  
|**100RTM**|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|  
  
`[ @replicate_ddl = ] replicate_ddl` Indica se per la pubblicazione è supportata la replica dello schema. *replicate_ddl* è di **tipo int** e il valore predefinito è 1. **1** indica che le istruzioni Data Definition Language (DDL) eseguite nel server di pubblicazione vengono replicate, mentre **0** indica che le istruzioni DDL non vengono replicate. Per altre informazioni, vedere [Apportare modifiche allo schema nei database di pubblicazione](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).  
  
 Il parametro *\@ replicate_ddl* viene rispettato quando un'istruzione DDL aggiunge una colonna. Il parametro *\@ replicate_ddl* viene ignorato quando un'istruzione DDL modifica o rilascia una colonna per i motivi seguenti.  
  
-   Quando viene rilasciata una colonna, è necessario aggiornare sysarticlecolumns per evitare l'inclusione della colonna eliminata da parte delle nuove istruzioni DML che determinerebbe la mancata esecuzione dell'agente di distribuzione. Il parametro *\@ replicate_ddl* viene ignorato perché la replica deve sempre replicare la modifica dello schema.  
  
-   Quando una colonna viene modificata, è probabile che venga modificato il tipo di dati di origine o il supporto dei valori Null, di conseguenza le istruzioni DML contengono un valore che potrebbe non essere compatibile con la tabella nel Sottoscrittore. Tali istruzioni DML potrebbero determinare la mancata esecuzione dell'agente di distribuzione. Il parametro *\@ replicate_ddl* viene ignorato perché la replica deve sempre replicare la modifica dello schema.  
  
-   Quando un'istruzione DDL aggiunge una nuova colonna, sysarticlecolumns non include la nuova colonna. Le istruzioni DML non tenteranno di replicare i dati per la nuova colonna. Il parametro viene rispettato perché la replica o la non replica di DDL è accettabile.  
  
`[ @allow_subscriber_initiated_snapshot = ] 'allow_subscriber_initiated_snapshot'` Indica se i sottoscrittori di questa pubblicazione possono avviare il processo di snapshot per generare lo snapshot filtrato per la relativa partizione di dati. *allow_subscriber_initiated_snapshot* è di **tipo nvarchar (5)** e il valore predefinito è false. **true** indica che i sottoscrittori possono avviare il processo di snapshot.  
  
`[ @allow_web_synchronization = ] 'allow_web_synchronization'` Specifica se la pubblicazione è abilitata per la sincronizzazione Web. *allow_web_synchronization* è di **tipo nvarchar (5)** e il valore predefinito è false. **true** specifica che le sottoscrizioni della pubblicazione possono essere sincronizzate tramite HTTPS. Per altre informazioni, vedere [Web Synchronization for Merge Replication](../../relational-databases/replication/web-synchronization-for-merge-replication.md). Per supportare i [!INCLUDE[ssEW](../../includes/ssew-md.md)] sottoscrittori, è necessario specificare **true**.  
  
`[ @web_synchronization_url = ] 'web_synchronization_url'` Specifica il valore predefinito dell'URL Internet utilizzato per la sincronizzazione Web. *web_synchronization_url* è di **tipo nvarchar (500)** e il valore predefinito è null. Definisce l'URL Internet predefinito se non ne è stato impostato uno in modo esplicito quando viene eseguito [sp_addmergepullsubscription_agent](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md) .  
  
`[ @allow_partition_realignment = ] 'allow_partition_realignment'` Determina se le eliminazioni vengono inviate al Sottoscrittore quando la modifica della riga nel server di pubblicazione comporta la modifica della partizione. *allow_partition_realignment* è di **tipo nvarchar (5)** e il valore predefinito è true. **true** Invia le eliminazioni al Sottoscrittore per riflettere i risultati di una modifica della partizione rimuovendo i dati che non fanno più parte della partizione del Sottoscrittore. **false** lascia i dati di una vecchia partizione nel Sottoscrittore, in cui le modifiche apportate a questi dati nel server di pubblicazione non vengono replicate nel Sottoscrittore, ma le modifiche apportate nel Sottoscrittore vengono replicate nel server di pubblicazione. L'impostazione di *allow_partition_realignment* su **false** viene utilizzata per mantenere i dati in una sottoscrizione da una partizione precedente quando i dati devono essere accessibili per motivi cronologici.  
  
> [!NOTE]  
>  I dati che rimangono nel Sottoscrittore come risultato dell'impostazione di *allow_partition_realignment* su **false** devono essere considerati come se fossero di sola lettura. Tuttavia, questo non viene applicato dal sistema di replica.  
  
`[ @retention_period_unit = ] 'retention_period_unit'` Specifica le unità per il periodo di memorizzazione impostato dalla *conservazione*. *retention_period_unit* è di **tipo nvarchar (10)**. i possibili valori sono i seguenti.  
  
|Valore|Versione|  
|-----------|-------------|  
|**giorno** (impostazione predefinita)|Il periodo di memorizzazione è specificato in giorni.|  
|**week**|Il periodo di memorizzazione è specificato in settimane.|  
|**month**|Il periodo di memorizzazione è specificato in mesi.|  
|**year**|Il periodo di memorizzazione è specificato in anni.|  
  
`[ @generation_leveling_threshold = ] generation_leveling_threshold` Specifica il numero di modifiche contenute in una generazione. Una generazione è una raccolta di modifiche recapitate a un server di pubblicazione o a un Sottoscrittore. *generation_leveling_threshold* è di **tipo int** e il valore predefinito è 1000.  
  
`[ @automatic_reinitialization_policy = ] automatic_reinitialization_policy`Specifica se le modifiche vengono caricate dal Sottoscrittore prima di una reinizializzazione automatica richiesta da una modifica alla pubblicazione, in cui è stato specificato il valore **1** per **\@ force_reinit_subscription**. *automatic_reinitialization_policy* è di bit e il valore predefinito è 0. **1** indica che le modifiche vengono caricate dal Sottoscrittore prima che si verifichi una reinizializzazione automatica.  
  
> [!IMPORTANT]  
>  Se si aggiunge, elimina o modifica un filtro con parametri, le modifiche in sospeso nel Sottoscrittore non possono essere caricate nel server di pubblicazione durante la reinizializzazione. Per caricare le modifiche in sospeso, sincronizzare tutte le sottoscrizioni prima di modificare il filtro.  
  
`[ @conflict_logging = ] 'conflict_logging'` Specifica la posizione di archiviazione dei record dei conflitti. *conflict_logging* è di **tipo nvarchar (15)**. i possibili valori sono i seguenti:  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**pubblicazione**|I record con conflitti vengono archiviati nel server di pubblicazione.|  
|**Sottoscrittore**|I record con conflitti vengono archiviati nel Sottoscrittore che ha causato il conflitto. Non supportato per i [!INCLUDE[ssEW](../../includes/ssew-md.md)] sottoscrittori.|  
|**sia**|I record con conflitti vengono archiviati nel server di pubblicazione e nel Sottoscrittore.|  
|NULL (predefinito)|La replica imposta  automaticamente conflict_logging **sia** quando il valore *backward_comp_level* è **90RTM** che nel **server di pubblicazione** in tutti gli altri casi.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 **sp_addmergepublication** viene utilizzata nella replica di tipo merge.  
  
 Per elencare gli oggetti di pubblicazione nel Active Directory utilizzando il parametro **\@ add_to_active_directory** , [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è necessario che l'oggetto sia già stato creato nella Active Directory.  
  
 Se sono presenti più pubblicazioni che pubblicano lo stesso oggetto di database, solo le pubblicazioni con un valore *replicate_ddl* **1** eseguiranno la replica delle istruzioni ALTER TABLE, ALTER VIEW, alter procedure, ALTER FUNCTION e alter trigger DDL. Una istruzione ALTER TABLE DROP COLUMN DDL verrà tuttavia replicata da tutte le pubblicazioni che stanno pubblicando la colonna eliminata.  
  
 Per i [!INCLUDE[ssEW](../../includes/ssew-md.md)] sottoscrittori, il valore di *alternate_snapshot_folder* viene utilizzato solo quando il valore di *snapshot_in_default_folder* è **false**.  
  
 Con la replica DDL abilitata (_replicate_ddl_**= 1**) per una pubblicazione, allo scopo di apportare modifiche DDL non di replica alla pubblicazione, è necessario prima eseguire [sp_changemergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md) per impostare *replicate_ddl* su **0**. Una volta rilasciate le istruzioni DDL non di replica, è possibile eseguire nuovamente **sp_changemergepublication** per riattivare la replica DDL.  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_AddMergePub](../../relational-databases/replication/codesnippet/tsql/sp-addmergepublication-t_1.sql)]  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_addmergepublication**.  
  
## <a name="see-also"></a>Vedere anche  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [Pubblicare dati e oggetti di database](../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [sp_changemergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md)   
 [sp_dropmergepublication &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropmergepublication-transact-sql.md)   
 [sp_helpmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md)   
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
