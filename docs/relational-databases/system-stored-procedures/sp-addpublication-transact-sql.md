---
description: sp_addpublication (Transact-SQL)
title: sp_addpublication (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_addpublication_TSQL
- sp_addpublication
helpviewer_keywords:
- sp_addpublication
ms.assetid: c7167ed1-2b7e-4824-b82b-65f4667c4407
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9aa65ff9b5b9c14441ac555d40788e92082f01c5
ms.sourcegitcommit: 7791bd2ba339edc5cd2078a6537c8f6bfe72a19b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2021
ms.locfileid: "98564447"
---
# <a name="sp_addpublication-transact-sql"></a>sp_addpublication (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea una pubblicazione snapshot o transazionale. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_addpublication [ @publication = ] 'publication'  
    [ , [ @taskid = ] tasked ]  
    [ , [ @restricted = ] 'restricted' ]  
    [ , [ @sync_method = ] 'sync_method' ]  
    [ , [ @repl_freq = ] 'repl_freq' ]  
    [ , [ @description = ] 'description' ]  
    [ , [ @status = ] 'status' ]  
    [ , [ @independent_agent = ] 'independent_agent' ]  
    [ , [ @immediate_sync = ] 'immediate_sync' ]  
    [ , [ @enabled_for_internet = ] 'enabled_for_internet' ]  
    [ , [ @allow_push = ] 'allow_push'  
    [ , [ @allow_pull = ] 'allow_pull' ]  
    [ , [ @allow_anonymous = ] 'allow_anonymous' ]  
    [ , [ @allow_sync_tran = ] 'allow_sync_tran' ]  
    [ , [ @autogen_sync_procs = ] 'autogen_sync_procs' ]  
    [ , [ @retention = ] retention ]  
    [ , [ @allow_queued_tran= ] 'allow_queued_updating' ]  
    [ , [ @snapshot_in_defaultfolder= ] 'snapshot_in_default_folder' ]  
    [ , [ @alt_snapshot_folder= ] 'alternate_snapshot_folder' ]  
    [ , [ @pre_snapshot_script= ] 'pre_snapshot_script' ]  
    [ , [ @post_snapshot_script= ] 'post_snapshot_script' ]  
    [ , [ @compress_snapshot= ] 'compress_snapshot' ]  
    [ , [ @ftp_address = ] 'ftp_address' ]  
    [ , [ @ftp_port= ] ftp_port ]  
    [ , [ @ftp_subdirectory = ] 'ftp_subdirectory' ]  
    [ , [ @ftp_login = ] 'ftp_login' ]  
    [ , [ @ftp_password = ] 'ftp_password' ]  
    [ , [ @allow_dts = ] 'allow_dts' ]  
    [ , [ @allow_subscription_copy = ] 'allow_subscription_copy' ]  
    [ , [ @conflict_policy = ] 'conflict_policy' ]  
    [ , [ @centralized_conflicts = ] 'centralized_conflicts' ]   
    [ , [ @conflict_retention = ] conflict_retention ]  
    [ , [ @queue_type = ] 'queue_type' ]  
    [ , [ @add_to_active_directory = ] 'add_to_active_directory' ]  
    [ , [ @logreader_job_name = ] 'logreader_agent_name' ]  
    [ , [ @qreader_job_name = ] 'queue_reader_agent_name' ]  
    [ , [ @publisher = ] 'publisher' ]   
    [ , [ @allow_initialize_from_backup = ] 'allow_initialize_from_backup' ]  
    [ , [ @replicate_ddl = ] replicate_ddl ]  
    [ , [ @enabled_for_p2p = ] 'enabled_for_p2p' ]  
    [ , [ @publish_local_changes_only = ] 'publish_local_changes_only' ]  
    [ , [ @enabled_for_het_sub = ] 'enabled_for_het_sub' ]  
    [ , [ @p2p_conflictdetection = ] 'p2p_conflictdetection' ]  
    [ , [ @p2p_originator_id = ] p2p_originator_id  
    [ , [ @p2p_continue_onconflict = ] 'p2p_continue_onconflict'  
    [ , [ @allow_partition_switch = ] 'allow_partition_switch'  
    [ , [ @replicate_partition_switch = ]'replicate_partition_switch'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ \@publication = ] 'publication'` Nome della pubblicazione da creare. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito. Il nome deve essere univoco all'interno del database.  
  
`[ \@taskid = ] taskid` Supportato solo per compatibilità con le versioni precedenti. utilizzare [sp_addpublication_snapshot &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md).  
  
`[ \@restricted = ] 'restricted'` Supportato solo per compatibilità con le versioni precedenti. usare *default_access*.  
  
`[ \@sync_method = ] _'sync_method'` Modalità di sincronizzazione. *sync_method* è di **tipo nvarchar (13)**. i possibili valori sono i seguenti.  
  
|Valore|Description|  
|-----------|-----------------|  
|**native**|Genera l'output in modalità nativa del programma per la copia bulk per tutte le tabelle. *Non supportato per i Publisher Oracle*.|  
|**character**|Genera l'output in modalità carattere del programma per la copia bulk per tutte le tabelle. _Per un server di pubblicazione Oracle,_ il **carattere** _è valido solo per la replica snapshot_.|  
|**simultanee**|Genera l'output del programma per la copia bulk in modalità nativa per tutte le tabelle, senza tuttavia bloccare le tabelle durante lo snapshot. Questo valore è supportato solo per pubblicazioni transazionali. *Non supportato per i Publisher Oracle*.|  
|**concurrent_c**|Genera l'output del programma per la copia bulk in modalità carattere per tutte le tabelle, senza tuttavia bloccare le tabelle durante lo snapshot. Questo valore è supportato solo per pubblicazioni transazionali.|  
|**snapshot del database**|Genera output del programma in modalità nativa per la copia bulk di tutte le tabelle da uno snapshot del database. Gli snapshot del database non sono disponibili in ogni edizione di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Funzionalità supportate dalle edizioni di SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).|  
|**database snapshot character**|Genera output del programma in modalità carattere per la copia bulk di tutte le tabelle da uno snapshot del database. Gli snapshot del database non sono disponibili in ogni edizione di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Funzionalità supportate dalle edizioni di SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).|  
|NULL (predefinito)|Il valore predefinito è **nativo** per i [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Publisher. Per i [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Publisher non, il valore predefinito è **character** quando il valore di *repl_freq* è **snapshot** e **concurrent_c** per tutti gli altri casi.|  
  
`[ \@repl_freq = ] 'repl_freq'` Tipo di frequenza di replica. *repl_freq* è di tipo **nvarchar (10)**. i possibili valori sono i seguenti.  
  
|Valore|Description|  
|-----------|-----------------|  
|**Continuous** (impostazione predefinita)|Il server di pubblicazione genera l'output per tutte le transazioni basate su log. Per gli [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] editori non è necessario che *sync_method* sia impostato su **concurrent_c**.|  
|**snapshot**|Il server di pubblicazione genera solo gli eventi di sincronizzazione pianificati. Per gli [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] editori non è necessario che *sync_method* sia impostato su **carattere**.|  
  
`[ \@description = ] 'description'` Descrizione facoltativa della pubblicazione. *Description* è di **tipo nvarchar (255)** e il valore predefinito è null.  
  
`[ \@status = ] 'status'` Specifica se i dati della pubblicazione sono disponibili. *status* è di **tipo nvarchar (8)**. i possibili valori sono i seguenti.  
  
|Valore|Description|  
|-----------|-----------------|  
|**active**|I dati della pubblicazione risultano immediatamente disponibili per i Sottoscrittori.|  
|**inattivo** (impostazione predefinita)|I dati della pubblicazione non sono disponibili per i Sottoscrittori quando viene creata la pubblicazione (è possibile creare una sottoscrizione, che tuttavia non viene elaborata).|  
  
 *Non supportato per i Publisher Oracle*.  
  
`[ \@independent_agent = ] 'independent_agent'` Specifica se è presente un agente di distribuzione autonomo per la pubblicazione. *independent_agent* è di **tipo nvarchar (5)** e il valore predefinito è false. Se **true**, esiste una agente di distribuzione autonoma per questa pubblicazione. Se **false**, la pubblicazione utilizza un agente di distribuzione condiviso e ogni coppia database server di pubblicazione/database Sottoscrittore dispone di un solo agente condiviso.  
  
`[ \@immediate_sync = ] 'immediate_synchronization'` Specifica se i file di sincronizzazione per la pubblicazione vengono creati a ogni esecuzione del agente di snapshot. *immediate_synchronization* è di **tipo nvarchar (5)** e il valore predefinito è false. Se **true**, i file di sincronizzazione vengono creati o ricreati a ogni esecuzione del agente di snapshot. Se l'esecuzione dell'agente snapshot viene completata prima della creazione della sottoscrizione, i Sottoscrittori possono ricevere i file di sincronizzazione immediatamente. Le nuove sottoscrizioni ricevono i file di sottoscrizione più recenti generati dall'ultima esecuzione dell'agente snapshot. *independent_agent* deve essere **true** per *immediate_synchronization* essere **true**. Se **false**, i file di sincronizzazione vengono creati solo se sono presenti nuove sottoscrizioni. È necessario chiamare [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md) per ogni sottoscrizione quando si aggiunge un nuovo articolo in modo incrementale a una pubblicazione esistente. I Sottoscrittori ricevono i file di sincronizzazione dopo la sottoscrizione solo se gli agenti snapshot sono stati avviati e completati.  
  
`[ \@enabled_for_internet = ] 'enabled_for_internet'` Specifica se la pubblicazione è abilitata per Internet e determina se è possibile utilizzare il protocollo FTP (file Transfer Protocol) per trasferire i file di snapshot in un Sottoscrittore. *enabled_for_internet* è di **tipo nvarchar (5)** e il valore predefinito è false. Se **true**, i file di sincronizzazione della pubblicazione vengono inseriti nella directory C:\Programmi\Microsoft SQL Server\MSSQL\MSSQL.x\Repldata\Ftp. La directory Ftp deve essere creata dall'utente.  
  
`[ \@allow_push = ] 'allow_push'` Specifica se è possibile creare sottoscrizioni push per la pubblicazione specificata. *allow_push* è di **tipo nvarchar (5)** e il valore predefinito è true, che consente le sottoscrizioni push nella pubblicazione.  
  
`[ \@allow_pull = ] 'allow_pull'` Specifica se è possibile creare sottoscrizioni pull per la pubblicazione specificata. *allow_pull* è di **tipo nvarchar (5)** e il valore predefinito è false. Se **false**, le sottoscrizioni pull non sono consentite nella pubblicazione.  
  
`[ \@allow_anonymous = ] 'allow_anonymous'` Specifica se è possibile creare sottoscrizioni anonime per la pubblicazione specificata. *allow_anonymous* è di **tipo nvarchar (5)** e il valore predefinito è false. Se **true**, è necessario impostare anche *immediate_synchronization* su **true**. Se **false**, le sottoscrizioni anonime non sono consentite nella pubblicazione.  
  
`[ \@allow_sync_tran = ] 'allow_sync_tran'` Specifica se per la pubblicazione sono consentite sottoscrizioni ad aggiornamento immediato. *allow_sync_tran* è di **tipo nvarchar (5)** e il valore predefinito è false. **true** *non è supportato per i Publisher Oracle*.  
  
`[ \@autogen_sync_procs = ] 'autogen_sync_procs'` Specifica se la stored procedure di sincronizzazione per le sottoscrizioni aggiornabili viene generata nel server di pubblicazione. *autogen_sync_procs* è di **tipo nvarchar (5)**. i possibili valori sono i seguenti.  
  
|Valore|Description|  
|-----------|-----------------|  
|**true**|Questo valore viene impostato automaticamente quando sono abilitate le sottoscrizioni aggiornabili.|  
|**false**|Questo valore viene impostato automaticamente quando non sono abilitate le sottoscrizioni aggiornabili oppure per server di pubblicazione Oracle.|  
|NULL (predefinito)|Il valore predefinito è **true** quando le sottoscrizioni aggiornabili sono abilitate e **false** quando le sottoscrizioni aggiornabili non sono abilitate.|  
  
> [!NOTE]  
>  Il valore fornito dall'utente per *autogen_sync_procs* verrà sostituito a seconda dei valori specificati per *allow_queued_tran* e *allow_sync_tran*.  
  
`[ \@retention = ] retention` Periodo di memorizzazione in ore per l'attività della sottoscrizione. la *conservazione* è di **tipo int** e il valore predefinito è 336 ore. Se una sottoscrizione non viene attivata entro il periodo di memorizzazione, scade e viene rimossa. Il valore di questo parametro può essere maggiore del periodo di memorizzazione massimo del database di distribuzione utilizzato dal server di pubblicazione. Se è **0**, le sottoscrizioni note alla pubblicazione non scadranno mai e verranno rimosse dall'agente di pulizia delle sottoscrizioni scaduto.  
  
`[ \@allow_queued_tran = ] 'allow_queued_updating'` Abilita o Disabilita l'accodamento delle modifiche nel Sottoscrittore finché non è possibile applicarle al server di pubblicazione. *allow_queued_updating* è di **tipo nvarchar (5)** e il valore predefinito è false. Se **false**, le modifiche nel Sottoscrittore non vengono accodate. **true** *non è supportato per i Publisher Oracle*.  
  
`[ \@snapshot_in_defaultfolder = ] 'snapshot_in_default_folder'` Specifica se i file di snapshot sono archiviati nella cartella predefinita. *snapshot_in_default_folder* è di **tipo nvarchar (5)** e il valore predefinito è true. Se **true**, i file di snapshot sono disponibili nella cartella predefinita. Se **false**, i file di snapshot sono stati archiviati nel percorso alternativo specificato da *alternate_snapshot_folder*. Le posizioni alternative possono essere un altro server, un'unità di rete oppure un supporto rimovibile, ad esempio un CD o un disco rimovibile. È inoltre possibile archiviare i file di snapshot in un sito FTP in modo che possano essere successivamente recuperati dal Sottoscrittore. Si noti che questo parametro può essere true e avere ancora una posizione nel parametro **\@ alt_snapshot_folder** . Tale combinazione indica che i file di snapshot vengono archiviati sia nella posizione predefinita che in posizioni alternative.  
  
`[ \@alt_snapshot_folder = ] 'alternate_snapshot_folder'` Specifica la posizione della cartella alternativa per lo snapshot. *alternate_snapshot_folder* è di **tipo nvarchar (255)** e il valore predefinito è null.  
  
`[ \@pre_snapshot_script = ] 'pre_snapshot_script'` Specifica un puntatore al percorso di un file con **estensione SQL** . *pre_snapshot_script* è di **tipo nvarchar (255)** e il valore predefinito è null. L'agente di distribuzione esegue lo script pre-snapshot prima dell'esecuzione di tutti gli script di oggetti replicati durante l'applicazione di uno snapshot in un Sottoscrittore. Lo script viene eseguito nel contesto di sicurezza utilizzato dall'agente di distribuzione per la connessione al database di sottoscrizione.  
  
`[ \@post_snapshot_script = ] 'post_snapshot_script'` Specifica un puntatore al percorso di un file con **estensione SQL** . *post_snapshot_script* è di **tipo nvarchar (255)** e il valore predefinito è null. L'agente di distribuzione esegue lo script post-snapshot dopo l'applicazione di tutti gli altri dati e script di oggetti replicati durante una sincronizzazione iniziale. Lo script viene eseguito nel contesto di sicurezza utilizzato dall'agente di distribuzione per la connessione al database di sottoscrizione.  
  
`[ \@compress_snapshot = ] 'compress_snapshot'`Specifica che lo snapshot scritto nella posizione **\@ alt_snapshot_folder** deve essere compresso nel [!INCLUDE[msCoName](../../includes/msconame-md.md)] formato CAB. *compress_snapshot* è di **tipo nvarchar (5)** e il valore predefinito è false. **false** specifica che lo snapshot non verrà compresso; **true** specifica che lo snapshot verrà compresso. I file di snapshot con una dimensione superiore a 2 gigabyte (GB) non possono essere compressi. I file di snapshot compressi vengono decompressi nella posizione in cui viene eseguito l'agente di distribuzione. Per le sottoscrizioni pull in genere vengono utilizzati snapshot compressi in modo che i file vengano decompressi nel Sottoscrittore. Non è possibile comprimere lo snapshot all'interno della cartella predefinita.  
  
`[ \@ftp_address = ] 'ftp_address'` È l'indirizzo di rete del servizio FTP per il server di distribuzione. *ftp_address* è di **tipo sysname** e il valore predefinito è null. Specifica la posizione in cui i file di snapshot della pubblicazione possono essere prelevati dall'agente di distribuzione o di merge di un Sottoscrittore. Poiché questa proprietà viene archiviata per ogni pubblicazione, ogni pubblicazione può avere un *ftp_address* diverso. La pubblicazione deve supportare la propagazione di snapshot tramite FTP.  
  
`[ \@ftp_port = ] ftp_port` Numero di porta del servizio FTP per il server di distribuzione. *ftp_port* è di **tipo int** e il valore predefinito è 21. Specifica la posizione dei file di snapshot della pubblicazione, dove i file possono essere prelevati dall'agente di distribuzione o di merge di un Sottoscrittore. Poiché questa proprietà viene archiviata per ogni pubblicazione, ogni pubblicazione può disporre di un proprio *ftp_port*.  
  
`[ \@ftp_subdirectory = ] 'ftp_subdirectory'` Specifica la posizione in cui i file di snapshot saranno disponibili per il agente di distribuzione o agente di merge del Sottoscrittore se la pubblicazione supporta la propagazione di snapshot tramite FTP. *ftp_subdirectory* è di **tipo nvarchar (255)** e il valore predefinito è null. Poiché questa proprietà viene archiviata per ogni pubblicazione, ogni pubblicazione può disporre di un proprio *ftp_subdirctory* o scegliere di non avere sottodirectory, indicato con un valore null.  
  
`[ \@ftp_login = ] 'ftp_login'` Nome utente utilizzato per la connessione al servizio FTP. *ftp_login* è di **tipo sysname** e il valore predefinito è Anonymous.  
  
`[ \@ftp_password = ] 'ftp_password'` Password utente utilizzata per la connessione al servizio FTP. *ftp_password* è di **tipo sysname** e il valore predefinito è null.  
  
`[ \@allow_dts = ] 'allow_dts'` Specifica che la pubblicazione consente le trasformazioni dei dati. Quando si crea una sottoscrizione, è possibile specificare un pacchetto DTS. *allow_transformable_subscriptions* è di **tipo nvarchar (5)** e il valore predefinito è false, che non consente le trasformazioni DTS. Quando *allow_dts* è true, è necessario che *sync_method* sia impostato su **character** o **concurrent_c**.  
  
 **true** *non è supportato per i Publisher Oracle*.  
  
`[ \@allow_subscription_copy = ] 'allow_subscription_copy'` Abilita o Disabilita la possibilità di copiare i database di sottoscrizione che sottoscrivono la pubblicazione. *allow_subscription_copy* è di **tipo nvarchar (5)** e il valore predefinito è false.  
  
`[ \@conflict_policy = ] 'conflict_policy'` Specifica i criteri di risoluzione dei conflitti seguiti quando viene utilizzata l'opzione del Sottoscrittore ad aggiornamento in coda. *conflict_policy* è di **tipo nvarchar (100)** e il valore predefinito è null. i possibili valori sono i seguenti.  
  
|Valore|Description|  
|-----------|-----------------|  
|**pub wins**|Prevale il server di pubblicazione.|  
|**sub reinit**|Reinizializzare la sottoscrizione.|  
|**sub wins**|Prevale il Sottoscrittore.|  
|NULL (predefinito)|Se il valore è NULL e la pubblicazione è di tipo snapshot, i criteri predefiniti diventeranno **sub reinit**. Se è NULL e la pubblicazione non è una pubblicazione snapshot, il valore predefinito sarà **WINS**.|  
  
 *Non supportato per i Publisher Oracle*.  
  
`[ \@centralized_conflicts = ] 'centralized_conflicts'` Specifica se i record dei conflitti vengono archiviati nel server di pubblicazione. *centralized_conflicts* è di **tipo nvarchar (5)** e il valore predefinito è true. Se **true**, i record dei conflitti vengono archiviati nel server di pubblicazione. Se **false**, i record dei conflitti vengono archiviati sia nel server di pubblicazione che nel Sottoscrittore che ha causato il conflitto. *Non supportato per i Publisher Oracle*.  
  
`[ \@conflict_retention = ] conflict_retention` Specifica il periodo di memorizzazione dei conflitti, in giorni. Si tratta del periodo di tempo durante il quale i metadati dei conflitti rimangono archiviati per la replica transazionale peer-to-peer e le sottoscrizioni ad aggiornamento in coda. *conflict_retention* è di **tipo int** e il valore predefinito è 14. *Non supportato per i Publisher Oracle*.  
  
`[ \@queue_type = ] 'queue_type'` Specifica il tipo di coda utilizzato. *queue_type* è di **tipo nvarchar (10)** e il valore predefinito è null. i possibili valori sono i seguenti.  
  
|Valore|Description|  
|-----------|-----------------|  
|**sql**|Consente di utilizzare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per l'archiviazione delle transazioni.|  
|NULL (predefinito)|Il valore predefinito è **SQL**, che specifica di utilizzare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per archiviare le transazioni.|  
  
> [!NOTE]  
>  L'utilizzo di MSMQ ([!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queuing) non è più supportato. Se si specifica il valore **MSMQ** , verrà generato un avviso e la replica imposterà automaticamente il valore su **SQL**.  
  
 *Non supportato per i Publisher Oracle*.  
  
`[ \@add_to_active_directory = ] 'add\to_active_directory'` Questo parametro è deprecato ed è supportato solo per la compatibilità con le versioni precedenti degli script. Non è più possibile aggiungere informazioni di pubblicazione in [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory.  
  
`[ \@logreader_job_name = ] 'logreader_agent_name'` Nome di un processo di Agent esistente. *logreader_agent_name* è di **tipo sysname** e il valore predefinito è null. Questo parametro viene specificato solo quando l'agente di lettura log utilizzerà un processo esistente anziché uno nuovo.  
  
`[ \@qreader_job_name = ] 'queue_reader_agent_name'` Nome di un processo di Agent esistente. *queue_reader_agent_name* è di **tipo sysname** e il valore predefinito è null. Questo parametro viene specificato solo quando l'agente di lettura coda utilizzerà un processo esistente anziché uno nuovo.  
  
`[ \@publisher = ] 'publisher'` Specifica un server di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazione non. *Publisher* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  non utilizzare *Publisher* quando si aggiunge una pubblicazione a un server di pubblicazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
`[ \@allow_initialize_from_backup = ] 'allow_initialize_from_backup'` Indica se i sottoscrittori possono inizializzare una sottoscrizione di questa pubblicazione da un backup anziché da uno snapshot iniziale. *allow_initialize_from_backup* è di **tipo nvarchar (5)**. i possibili valori sono i seguenti:  
  
|Valore|Description|  
|-----------|-----------------|  
|**true**|Abilita l'inizializzazione da un backup.|  
|**false**|Disabilita l'inizializzazione da un backup.|  
|NULL (predefinito)|Il valore predefinito è **true** per una pubblicazione in una topologia di replica peer-to-peer e **false** per tutte le altre pubblicazioni.|  
  
 Per altre informazioni, vedere [Inizializzazione di una sottoscrizione transazionale senza uno snapshot](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md).  
  
> [!WARNING]  
>  Per evitare la mancanza di dati del Sottoscrittore, quando si utilizza **sp_addpublication** con `@allow_initialize_from_backup = N'true'`, utilizzare sempre `@immediate_sync = N'true'`.  
  
`[ \@replicate_ddl = ] replicate_ddl` Indica se per la pubblicazione è supportata la replica dello schema. *replicate_ddl* è di **tipo int** e il valore predefinito è **1** per i [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Publisher e **0** per i [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Publisher non. **1** indica che le istruzioni Data Definition Language (DDL) eseguite nel server di pubblicazione vengono replicate, mentre **0** indica che le istruzioni DDL non vengono replicate. *La replica dello schema non è supportata per server di pubblicazione Oracle.* Per altre informazioni, vedere [Apportare modifiche allo schema nei database di pubblicazione](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).  
  
 Il parametro *\@ replicate_ddl* viene rispettato quando un'istruzione DDL aggiunge una colonna. Il parametro *\@ replicate_ddl* viene ignorato quando un'istruzione DDL modifica o rilascia una colonna per i motivi seguenti.  
  
-   Quando viene rilasciata una colonna, è necessario aggiornare sysarticlecolumns per evitare l'inclusione della colonna eliminata da parte delle nuove istruzioni DML che determinerebbe la mancata esecuzione dell'agente di distribuzione. Il parametro *\@ replicate_ddl* viene ignorato perché la replica deve sempre replicare la modifica dello schema.  
  
-   Quando una colonna viene modificata, è probabile che venga modificato il tipo di dati di origine o il supporto dei valori Null, di conseguenza le istruzioni DML contengono un valore che potrebbe non essere compatibile con la tabella nel Sottoscrittore. Tali istruzioni DML potrebbero determinare la mancata esecuzione dell'agente di distribuzione. Il parametro *\@ replicate_ddl* viene ignorato perché la replica deve sempre replicare la modifica dello schema.  
  
-   Quando un'istruzione DDL aggiunge una nuova colonna, sysarticlecolumns non include la nuova colonna. Le istruzioni DML non tenteranno di replicare i dati per la nuova colonna. Il parametro viene rispettato perché la replica o la non replica di DDL è accettabile.  
  
`[ \@enabled_for_p2p = ] 'enabled_for_p2p'` Consente di utilizzare la pubblicazione in una topologia di replica peer-to-peer. *enabled_for_p2p* è di **tipo nvarchar (5)** e il valore predefinito è false. **true** indica che la pubblicazione supporta la replica peer-to-peer. Quando si imposta *enabled_for_p2p* su **true**, si applicano le restrizioni seguenti:  
  
-   *allow_anonymous* deve essere **false**.  
  
-   *allow_dts* deve essere **false**.  
  
-   *allow_initialize_from_backup* deve essere **true**.  
  
-   *allow_queued_tran* deve essere **false**.  
  
-   *allow_sync_tran* deve essere **false**.  
  
-   *conflict_policy* deve essere **false**.  
  
-   *independent_agent* deve essere **true**.  
  
-   *repl_freq* deve essere **continuo**.  
  
-   *replicate_ddl* deve essere **1**.  
  
 Per altre informazioni, vedere [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).  
  
`[ \@publish_local_changes_only = ] 'publish_local_changes_only'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ \@enabled_for_het_sub = ] 'enabled_for_het_sub'` Consente alla pubblicazione di supportare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sottoscrittori non. *enabled_for_het_sub* è di **tipo nvarchar (5)** e il valore predefinito è false. Il valore **true** indica che la pubblicazione supporta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sottoscrittori non. Quando *enabled_for_het_sub* è **true**, vengono applicate le restrizioni seguenti:  
  
-   *allow_initialize_from_backup* deve essere **false**.  
  
-   *allow_push* deve essere **true**.  
  
-   *allow_queued_tran* deve essere **false**.  
  
-   *allow_subscription_copy* deve essere **false**.  
  
-   *allow_sync_tran* deve essere **false**.  
  
-   *autogen_sync_procs* deve essere **false**.  
  
-   *conflict_policy* deve essere null.  
  
-   *enabled_for_internet* deve essere **false**.  
  
-   *enabled_for_p2p* deve essere **false**.  
  
-   *ftp_address* deve essere null.  
  
-   *ftp_subdirectory* deve essere null.  
  
-   *ftp_password* deve essere null.  
  
-   *pre_snapshot_script* deve essere null.  
  
-   *post_snapshot_script* deve essere null.  
  
-   *replicate_ddl* deve essere 0.  
  
-   *qreader_job_name* deve essere null.  
  
-   *queue_type* deve essere null.  
  
-   *sync_method* non può essere **nativo** o **simultaneo**.  
  
 Per altre informazioni, vedere [Non-SQL Server Subscribers](../../relational-databases/replication/non-sql/non-sql-server-subscribers.md).  
  
`[ \@p2p_conflictdetection = ] 'p2p_conflictdetection'` Consente al agente di distribuzione di rilevare i conflitti se la pubblicazione è abilitata per la replica peer-to-peer. *p2p_conflictdetection* è di **tipo nvarchar (5)** e il valore predefinito è true. Per altre informazioni, vedere [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).  
  
`[ \@p2p_originator_id = ] p2p_originator_id` Specifica un ID per un nodo in una topologia peer-to-peer. *p2p_originator_id* è di **tipo int** e il valore predefinito è null. Questo ID viene utilizzato per il rilevamento dei conflitti se *p2p_conflictdetection* è impostato su true. Specificare un ID positivo diverso da zero che non sia mai stato utilizzato nella topologia. Per un elenco di ID già utilizzati, eseguire [sp_help_peerconflictdetection](../../relational-databases/system-stored-procedures/sp-help-peerconflictdetection-transact-sql.md).  
  
`[ \@p2p_continue_onconflict = ] 'p2p_continue_onconflict'` Determina se l'agente di distribuzione continua a elaborare le modifiche dopo che è stato rilevato un conflitto. *p2p_continue_onconflict* è di **tipo nvarchar (5)** e il valore predefinito è false.  
  
> [!CAUTION]  
>  È consigliabile utilizzare il valore predefinito FALSE. Quando questa opzione è impostata su TRUE, l'agente di distribuzione tenta di garantire la convergenza dei dati nella topologia applicando la riga in conflitto dal nodo con ID di origine maggiore. Questo metodo non garantisce la convergenza. Dopo il rilevamento di un conflitto, è necessario assicurarsi che la topologia sia coerente. Per ulteriori informazioni, vedere la sezione relativa alla gestione dei conflitti in [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).  
  
  
`[ \@allow_partition_switch = ] 'allow_partition_switch'` Specifica se ALTER TABLE... Le istruzioni SWITCH possono essere eseguite sul database pubblicato. *allow_partition_switch* è di **tipo nvarchar (5)** e il valore predefinito è false. Per altre informazioni, vedere [Replicare tabelle e indici partizionati](../../relational-databases/replication/publish/replicate-partitioned-tables-and-indexes.md).  
  
`[ \@replicate_partition_switch = ] 'replicate_partition_switch'` Specifica se ALTER TABLE... Le istruzioni SWITCH eseguite sul database pubblicato devono essere replicate nei Sottoscrittori. *replicate_partition_switch* è di **tipo nvarchar (5)** e il valore predefinito è false. Questa opzione è valida solo se *allow_partition_switch* è impostata su true.  

## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_addpublication** viene utilizzata nella replica snapshot e nella replica transazionale.  
  
 Se sono presenti più pubblicazioni che pubblicano lo stesso oggetto di database, solo le pubblicazioni con un valore *replicate_ddl* **1** eseguiranno la replica delle istruzioni ALTER TABLE, ALTER VIEW, alter procedure, ALTER FUNCTION e alter trigger DDL. Una istruzione ALTER TABLE DROP COLUMN DDL verrà tuttavia replicata da tutte le pubblicazioni che stanno pubblicando la colonna eliminata.  
  
 Con la replica DDL abilitata (*replicate_ddl*  =  **1**) per una pubblicazione, per apportare modifiche DDL non di replica alla pubblicazione, è necessario prima eseguire [sp_changepublication](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md) per impostare *replicate_ddl* su **0**. Una volta rilasciate le istruzioni DDL non di replica, è possibile eseguire nuovamente [sp_changepublication](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md) per riattivare la replica DDL.  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_AddTranPub](../../relational-databases/replication/codesnippet/tsql/sp-addpublication-transa_1.sql)]  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_addpublication**. Gli account di accesso con autenticazione di Windows devono disporre di un account utente nel database che rappresenta il proprio account utente di Windows. Un account utente che rappresenta un gruppo di Windows non è sufficiente.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_addlogreader_agent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md)   
 [sp_addpublication_snapshot &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md)   
 [sp_changepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)   
 [sp_droppublication &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-droppublication-transact-sql.md)   
 [sp_helppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md)   
 [sp_replicationdboption &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md)   
 [Pubblicare dati e oggetti di database](../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
