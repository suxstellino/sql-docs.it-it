---
title: sp_addsubscription (Transact-SQL) | Microsoft Docs
description: Aggiunge una sottoscrizione a una pubblicazione e imposta lo stato del Sottoscrittore. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.
ms.date: 06/09/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.custom: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addsubscription
- sp_addsubscription_TSQL
helpviewer_keywords:
- sp_addsubscription
ms.assetid: 61ddf287-1fa0-4c1a-8657-ced50cebf0e0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a4bb033d4817fcf08a7816f8c6598802e9ac3f8c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211956"
---
# <a name="sp_addsubscription-transact-sql"></a>sp_addsubscription (Transact-SQL)
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]

  Aggiunge una sottoscrizione a una pubblicazione e imposta lo stato del Sottoscrittore. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_addsubscription [ @publication = ] 'publication'  
    [ , [ @article = ] 'article']  
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @destination_db = ] 'destination_db' ]  
        [ , [ @sync_type = ] 'sync_type' ]  
    [ , [ @status = ] 'status'  
        [ , [ @subscription_type = ] 'subscription_type' ]  
    [ , [ @update_mode = ] 'update_mode' ]  
    [ , [ @loopback_detection = ] 'loopback_detection' ]  
    [ , [ @frequency_type = ] frequency_type ]  
    [ , [ @frequency_interval = ] frequency_interval ]  
    [ , [ @frequency_relative_interval = ] frequency_relative_interval ]  
    [ , [ @frequency_recurrence_factor = ] frequency_recurrence_factor ]  
    [ , [ @frequency_subday = ] frequency_subday ]  
    [ , [ @frequency_subday_interval = ] frequency_subday_interval ]  
    [ , [ @active_start_time_of_day = ] active_start_time_of_day ]  
    [ , [ @active_end_time_of_day = ] active_end_time_of_day ]  
    [ , [ @active_start_date = ] active_start_date ]  
    [ , [ @active_end_date = ] active_end_date ]  
    [ , [ @optional_command_line = ] 'optional_command_line' ]  
    [ , [ @reserved = ] 'reserved' ]  
    [ , [ @enabled_for_syncmgr= ] 'enabled_for_syncmgr' ]  
    [ , [ @offloadagent= ] remote_agent_activation]  
    [ , [ @offloadserver= ] 'remote_agent_server_name' ]  
    [ , [ @dts_package_name= ] 'dts_package_name' ]  
    [ , [ @dts_package_password= ] 'dts_package_password' ]  
    [ , [ @dts_package_location= ] 'dts_package_location' ]  
    [ , [ @distribution_job_name= ] 'distribution_job_name' ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @backupdevicetype = ] 'backupdevicetype' ]  
    [ , [ @backupdevicename = ] 'backupdevicename' ]  
    [ , [ @mediapassword = ] 'mediapassword' ]  
    [ , [ @password = ] 'password' ]  
    [ , [ @fileidhint = ] fileidhint ]  
    [ , [ @unload = ] unload ]  
    [ , [ @subscriptionlsn = ] subscriptionlsn ]  
    [ , [ @subscriptionstreams = ] subscriptionstreams ]  
    [ , [ @subscriber_type = ] subscriber_type ]  
    [ , [ @memory_optimized = ] memory_optimized ]  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @publication =]'*pubblicazione*'  
 Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
 [ @article =]'*article*'  
 Articolo in cui viene sottoscritta la pubblicazione. *article* è di **tipo sysname** e il valore predefinito è all. Se si specifica all, viene aggiunta una sottoscrizione a tutti gli articoli della pubblicazione specificata. Per i server di pubblicazione Oracle sono supportati solo i valori all o NULL.  
  
 [ @subscriber =]'*Subscriber*'  
 Nome del Sottoscrittore. *Subscriber* è di **tipo sysname** e il valore predefinito è null.  

> [!NOTE]
> Il nome del server può essere specificato come `<Hostname>,<PortNumber>` . Potrebbe essere necessario specificare il numero di porta per la connessione quando SQL Server viene distribuito in Linux o Windows con una porta personalizzata e il servizio browser è disabilitato.
  
 [ @destination_db =]'*destination_db*'  
 Nome del database di destinazione in cui verranno inseriti i dati replicati. *destination_db* è di **tipo sysname** e il valore predefinito è null. Se è NULL, *destination_db* viene impostato sul nome del database di pubblicazione. Per i Publisher Oracle, è necessario specificare *destination_db* . Per un Sottoscrittore non SQL Server, specificare il valore (destinazione predefinita) per *destination_db*.  
  
 [ @sync_type =]'*sync_type*'  
 Tipo di sincronizzazione per la sottoscrizione. *sync_type* è di **tipo nvarchar (255)**. i possibili valori sono i seguenti:  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|Nessuno|Il Sottoscrittore dispone già dello schema e dei dati iniziali per le tabelle pubblicate.<br /><br /> Nota: questa opzione è stata deprecata. In alternativa, utilizzare il valore replication support only.|  
|automatic (predefinito)|Lo schema e i dati iniziali per le tabelle pubblicate vengono trasferiti per primi nel Sottoscrittore.|  
|replication support only|Esegue nel Sottoscrittore la generazione automatica di stored procedure e trigger personalizzati degli articoli che supportano l'aggiornamento delle sottoscrizioni, se appropriato. Presuppone che nel Sottoscrittore siano già disponibili lo schema e i dati iniziali per le tabelle pubblicate. Durante la configurazione di una topologia di replica transazionale peer-to-peer, assicurarsi che i dati in tutti i nodi della topologia siano identici. Per altre informazioni, vedere [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).<br /><br /> *Non supportato per le sottoscrizioni a pubblicazioni non SQL Server.*|  
|initialize with backup|Lo schema e i dati iniziali per le tabelle pubblicate vengono recuperati da un backup del database di pubblicazione. Si presuppone che il Sottoscrittore abbia accesso a un backup del database di pubblicazione. Il percorso del backup e il tipo di supporto per il backup sono specificati da *backupdevicename* e *backupdevicetype*. Se si utilizza questa opzione, non sarà necessario mettere in stato di inattività la topologia di replica transazionale peer-to-peer durante la configurazione.<br /><br /> *Non supportato per le sottoscrizioni a pubblicazioni non SQL Server.*|  
|initialize from lsn|Utilizzato quando si aggiunge un nodo a una topologia di replica transazionale peer-to-peer. Utilizzato con @subscriptionlsn perché tutte le transazioni pertinenti vengano replicate nel nuovo nodo. Presuppone che nel Sottoscrittore siano già disponibili lo schema e i dati iniziali per le tabelle pubblicate. Per altre informazioni, vedere [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).|  
  
> [!NOTE]  
>  Le tabelle e i dati di sistema vengono sempre trasferiti.  
  
 [ @status =]'*status*'  
 Stato della sottoscrizione. *status* è di **tipo sysname** e il valore predefinito è null. Se non è impostato in modo esplicito, questo parametro viene impostato automaticamente dalla replica su uno dei valori seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|active|La sottoscrizione è inizializzata e in grado di accettare modifiche. Questa opzione viene impostata quando il valore di *sync_type* è None, Initialize with backup o replication support only.|  
|subscribed|La sottoscrizione deve essere inizializzata. Questa opzione viene impostata quando il valore di *sync_type* è automatico.|  
  
 [ @subscription_type =]'*subscription_type*'  
 Tipo di sottoscrizione. *subscription_type* è di **tipo nvarchar (4)** e il valore predefinito è push. I possibili valori sono push o pull. Gli agenti di distribuzione delle sottoscrizioni push si trovano nel server di distribuzione, mentre gli agenti di distribuzione delle sottoscrizioni pull si trovano nel Sottoscrittore. *subscription_type* possibile eseguire il pull per creare una sottoscrizione pull denominata nota al server di pubblicazione. Per altre informazioni, vedere [Sottoscrivere le pubblicazioni](../../relational-databases/replication/subscribe-to-publications.md).  
  
> [!NOTE]  
>  Con le sottoscrizioni anonime non è necessario utilizzare questa stored procedure.  
  
 [ @update_mode =]'*update_mode*'  
 Tipo di aggiornamento. *update_mode* è di **tipo nvarchar (30)**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|read only (predefinito)|La sottoscrizione è di sola lettura. Le modifiche apportate nel Sottoscrittore non vengono ritrasmesse al server di pubblicazione.|  
|sync tran|Abilita il supporto per sottoscrizioni ad aggiornamento immediato. Questa proprietà non è supportata per server di pubblicazione Oracle.|  
|queued tran|Abilita la sottoscrizione per l'aggiornamento in coda. Le modifiche dei dati possono essere apportate nel Sottoscrittore, archiviate in una coda e quindi distribuite al server di pubblicazione. Questa proprietà non è supportata per server di pubblicazione Oracle.|  
|failover|Abilita la sottoscrizione per l'aggiornamento immediato sostituito dall'aggiornamento in coda in caso di failover. Le modifiche dei dati possono essere apportate nel Sottoscrittore e distribuite immediatamente al server di pubblicazione. Se il server di pubblicazione e il Sottoscrittore non sono connessi, la modalità di aggiornamento può essere modificata in modo tale che le modifiche apportate ai dati nel Sottoscrittore vengano archiviate in una coda fino alla riconnessione del Sottoscrittore e del server di pubblicazione. Questa proprietà non è supportata per server di pubblicazione Oracle.|  
|queued failover|Abilita la sottoscrizione come sottoscrizione con aggiornamento in coda con la possibilità di passare alla modalità di aggiornamento immediato. Le modifiche ai dati possono essere apportate nel Sottoscrittore e archiviate in una coda fino alla riconnessione del Sottoscrittore e del server di pubblicazione. Quando viene ristabilita una connessione continua, la modalità di aggiornamento può essere modificata nella modalità di aggiornamento immediato. Questa proprietà non è supportata per server di pubblicazione Oracle.|  
  
 Si noti che i valori synctran e queued tran non sono consentiti se la pubblicazione da sottoscrivere supporta DTS.  
  
 [ @loopback_detection =]'*loopback_detection*'  
 Indica se l'agente di distribuzione reinvia al Sottoscrittore le transazioni generate nel Sottoscrittore stesso. *loopback_detection* è di **tipo nvarchar (5)**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|true|L'agente di distribuzione non reinvia le transazioni generate nel Sottoscrittore al Sottoscrittore. Utilizzato con la replica transazionale bidirezionale. Per altre informazioni, vedere [Bidirectional Transactional Replication](../../relational-databases/replication/transactional/bidirectional-transactional-replication.md).|  
|false|L'agente di distribuzione reinvia le transazioni generate nel Sottoscrittore al Sottoscrittore.|  
|NULL (predefinito)|Impostato automaticamente su true per un Sottoscrittore [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e su false per un Sottoscrittore non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
 [ @frequency_type =] *frequency_type*  
 Frequenza di pianificazione dell'attività di distribuzione. *frequency_type* è di tipo int. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|1|Singola occorrenza|  
|2|On demand|  
|4|Ogni giorno|  
|8|Settimanale|  
|16|Ogni mese|  
|32|Mensile relativa|  
|64 (impostazione predefinita)|Avvio automatico|  
|128|Periodica|  
  
 [ @frequency_interval =] *frequency_interval*  
 Valore da applicare alla frequenza impostata da *frequency_type*. *frequency_interval* è di **tipo int** e il valore predefinito è null.  
  
 [ @frequency_relative_interval =] *frequency_relative_interval*  
 Data dell'agente di distribuzione. Questo parametro viene usato quando *frequency_type* è impostato su 32 (mensile relativo). *frequency_relative_interval* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|1|First (Primo)|  
|2|Second|  
|4|Terzo|  
|8|Quarto|  
|16|Last (Ultimo)|  
|NULL (predefinito)||  
  
 [ @frequency_recurrence_factor =] *frequency_recurrence_factor*  
 Fattore di occorrenza utilizzato da *frequency_type*. *frequency_recurrence_factor* è di **tipo int** e il valore predefinito è null.  
  
 [ @frequency_subday =] *frequency_subday*  
 Frequenza di ripianificazione in minuti durante il periodo definito. *frequency_subday* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|1|Una sola volta|  
|2|Second|  
|4|Minuto|  
|8|Ora|  
|NULL||  
  
 [ @frequency_subday_interval =] *frequency_subday_interval*  
 Intervallo per *frequency_subday*. *frequency_subday_interval* è di **tipo int** e il valore predefinito è null.  
  
 [ @active_start_time_of_day =] *active_start_time_of_day*  
 Ora del giorno della prima esecuzione pianificata dell'agente di distribuzione nel formato HHMMSS. *active_start_time_of_day* è di **tipo int** e il valore predefinito è null.  
  
 [ @active_end_time_of_day =] *active_end_time_of_day*  
 Ora del giorno dell'ultima esecuzione pianificata dell'agente di distribuzione, nel formato HHMMSS. *active_end_time_of_day* è di **tipo int** e il valore predefinito è null.  
  
 [ @active_start_date =] *active_start_date*  
 Data della prima esecuzione pianificata dell'agente di distribuzione nel formato AAAAMMGG. *active_start_date* è di **tipo int** e il valore predefinito è null.  
  
 [ @active_end_date =] *active_end_date*  
 Data dell'ultima esecuzione pianificata dell'agente di distribuzione, nel formato AAAAMMGG. *active_end_date* è di **tipo int** e il valore predefinito è null.  
  
 [ @optional_command_line =]'*optional_command_line*'  
 Prompt dei comandi facoltativo da eseguire. *optional_command_line* è di **tipo nvarchar (4000)** e il valore predefinito è null.  
  
 [ @reserved =]'*riservato*'  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
 [ @enabled_for_syncmgr =]'*enabled_for_syncmgr*'  
 Indica se la sottoscrizione può essere sincronizzata tramite [!INCLUDE[msCoName](../../includes/msconame-md.md)] Gestione sincronizzazione Microsoft Windows. *enabled_for_syncmgr* è di **tipo nvarchar (5)** e il valore predefinito è false. Se il valore è false, la sottoscrizione non viene registrata con Gestione sincronizzazione Microsoft Windows. Se il valore è true, la sottoscrizione viene registrata con Gestione sincronizzazione Microsoft Windows e può essere sincronizzata senza avviare [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Questa proprietà non è supportata per server di pubblicazione Oracle.  
  
 [ @offloadagent =]'*remote_agent_activation*'  
 Specifica se è possibile o meno attivare l'agente in remoto. *remote_agent_activation* è di **bit** e il valore predefinito è 0.  
  
> [!NOTE]  
>  Questo parametro è deprecato ed è ancora disponibile per compatibilità con gli script di versioni precedenti.  
  
 [ @offloadserver =]'*remote_agent_server_name*'  
 Specifica il nome di rete del server da utilizzare per l'attivazione remota. *remote_agent_server_name* è di **tipo sysname** e il valore predefinito è null.  
  
 [ @dts_package_name =]'*dts_package_name*'  
 Specifica il nome del pacchetto Data Transformation Services (DTS). *dts_package_name* è di **tipo sysname** e il valore predefinito è null. Ad esempio, per specificare il pacchetto DTSPub_Package, il parametro deve essere `@dts_package_name = N'DTSPub_Package'`. Questo parametro è disponibile per sottoscrizioni push. Per aggiungere informazioni sul pacchetto DTS a una sottoscrizione pull, utilizzare sp_addpullsubscription_agent.  
  
 [ @dts_package_password =]'*dts_package_password*'  
 Password del pacchetto, se è disponibile. *dts_package_password* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  Se *dts_package_name* è specificato, è necessario specificare una password.  
  
 [ @dts_package_location =]'*dts_package_location*'  
 Specifica la posizione del pacchetto. *dts_package_location* è di **tipo nvarchar (12)** e il valore predefinito è Distributor. Per la posizione del pacchetto è possibile specificare distributor o subscriber.  
  
 [ @distribution_job_name =]'*distribution_job_name*'  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
 [ @publisher =]'*Publisher*'  
 Specifica un server di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazione non. *Publisher* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  il *Server* di pubblicazione non deve essere specificato per un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] server di pubblicazione.  
  
 [ @backupdevicetype =]'*backupdevicetype*'  
 Specifica il tipo di dispositivo di backup utilizzato durante l'inizializzazione di un Sottoscrittore da un backup. *backupdevicetype* è di **tipo nvarchar (20)**. i possibili valori sono i seguenti:  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|logical (predefinito)|Il dispositivo di backup è un dispositivo logico.|  
|disk|Il dispositivo di backup è l'unità disco.|  
|tape|Il dispositivo di backup è l'unità nastro.|  
  
 *backupdevicetype* viene utilizzato solo quando *sync_method* è impostato su initialize_with_backup.  
  
 [ @backupdevicename =]'*backupdevicename*'  
 Specifica il nome del dispositivo utilizzato durante l'inizializzazione di un Sottoscrittore da un backup. *backupdevicename* è di **tipo nvarchar (1000)** e il valore predefinito è null.  
  
 [ @mediapassword =]'*MEDIAPASSWORD*'  
 Specifica una password per il set di supporti se durante la formattazione dei supporti è stata impostata una password. *MEDIAPASSWORD* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 [ @password =]'*password*'  
 Specifica una password per il backup se durante la creazione del backup è stata impostata una password. *password* è di **tipo sysname** e il valore predefinito è null.  
  
 [ @fileidhint =] *fileidhint*  
 Identifica un valore ordinale del set di backup da ripristinare. *fileidhint* è di **tipo int** e il valore predefinito è null.  
  
 [ @unload =] *scaricamento*  
 Specifica se è necessario scaricare un dispositivo di backup su nastro dopo il completamento dell'inizializzazione dal backup. *unload* è di **bit** e il valore predefinito è 1. 1 specifica che il nastro deve essere scaricato. *unload* viene usato solo quando *backupdevicetype* è Tape.  
  
 [ @subscriptionlsn =] *SubscriptionLSN*  
 Specifica il numero di sequenza del file di log (LSN) al quale una sottoscrizione deve iniziare a recapitare le modifiche a un nodo in una topologia di replica transazionale peer-to-peer. Usato con il @sync_type valore initialize from LSN per assicurarsi che tutte le transazioni rilevanti vengano replicate in un nuovo nodo. Per altre informazioni, vedere [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).  
  
 [ @subscriptionstreams =] *SubscriptionStreams*  
 Numero di connessioni consentite per agente di distribuzione per l'applicazione di batch di modifiche in parallelo a un Sottoscrittore, conservando molte delle caratteristiche transazionali disponibili quando si utilizza un singolo thread. *SubscriptionStreams* è di **tinyint** e il valore predefinito è null. È supportato un intervallo di valori compreso tra 1 e 64. Questo parametro non è supportato per i [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sottoscrittori non, i Publisher Oracle o le sottoscrizioni peer-to-peer. Ogni volta che vengono utilizzati flussi di sottoscrizioni, vengono aggiunte nuove righe nella tabella msreplication_subscriptions (1 per stream) con agent_id impostato su NULL.  
  
> [!NOTE]  
>  Subscriptionstreams non funziona per gli articoli configurati per fornire [!INCLUDE[tsql](../../includes/tsql-md.md)]. Per utilizzare subscriptionstreams, configurare gli articoli per fornire invece chiamate di stored procedure.  
  
 [ @subscriber_type =] *subscriber_type*  
 Tipo di Sottoscrittore. *subscriber_type* è di **tinyint**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|0 (predefinito)|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sottoscrittore|  
|1|Server dell'origine dei dati ODBC.|  
|2|Database [!INCLUDE[msCoName](../../includes/msconame-md.md)] Jet|  
|3|Provider OLE DB|  
  
 [ @memory_optimized =] *memory_optimized*  
 Indica che la sottoscrizione supporta le tabelle con ottimizzazione per la memoria. *memory_optimized* è di **bit**, dove 1 è uguale a true (la sottoscrizione supporta le tabelle con ottimizzazione per la memoria).  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 La stored procedure sp_addsubscription viene utilizzata per la replica snapshot e transazionale.  
  
 Se la stored procedure sp_addsubscription viene eseguita da un membro del ruolo predefinito del server sysadmin per creare una sottoscrizione push, il processo dell'agente di distribuzione viene creato in modo implicito e viene eseguito utilizzando l'account del servizio SQL Server Agent. Si consiglia di eseguire [sp_addpushsubscription_agent](../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md) e specificare le credenziali di un account di Windows diverso da quello specifico dell'agente per @job_login e @job_password . Per altre informazioni, vedere [Modello di sicurezza dell'agente di replica](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
 La stored procedure sp_addsubscription impedisce ai Sottoscrittori ODBC e OLE DB di accedere ai seguenti tipi di pubblicazione:  
  
-   Sono stati creati con la *sync_method* nativa nella chiamata a [sp_addpublication](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md).  
  
-   Contengono articoli aggiunti alla pubblicazione con la [sp_addarticle](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) stored procedure con un valore di parametro di *pre_creation_cmd* pari a 3 (troncamento).  
  
-   Tentativo di impostare *update_mode* per la sincronizzazione Tran.  
  
-   Pubblicazioni che includono un articolo configurato per l'utilizzo di istruzioni con parametri.  
  
 Inoltre, se per una pubblicazione l'opzione *allow_queued_tran* è impostata su true (che consente l'accodamento delle modifiche nel Sottoscrittore fino a quando non è possibile applicarle nel server di pubblicazione), la colonna timestamp di un articolo viene inviata come **timestamp** e le modifiche apportate a tale colonna vengono inviate al Sottoscrittore. Nel Sottoscrittore viene quindi generato e aggiornato il valore della colonna timestamp. Per un Sottoscrittore ODBC o OLE DB, sp_addsubscription ha esito negativo se viene effettuato un tentativo di sottoscrivere una pubblicazione con *allow_queued_tran* impostata su true e gli articoli con colonne timestamp.  
  
 Se una sottoscrizione non utilizza un pacchetto DTS, non è in grado di sottoscrivere una pubblicazione impostata su *allow_transformable_subscriptions*. Se la tabella della pubblicazione deve essere replicata sia come sottoscrizione DTS che come sottoscrizione non DTS, è necessario creare due pubblicazioni distinte, una per ogni tipo di sottoscrizione.  
  
 In caso di selezione delle opzioni **sync_type** , i parametri *replication support only*, *initialize with backup* o *initialize from lsn* e l'agente di lettura log devono essere in esecuzione dopo aver eseguito **sp_addsubscription**, in modo che gli script impostati vengano scritti nel database di distribuzione. L'agente di lettura log deve essere in esecuzione con un account membro del ruolo predefinito del server **sysadmin** . Quando l'opzione **sync_type** è impostata su *Automatic*, non sono richieste azioni dell'agente di lettura log speciali.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server sysadmin o del ruolo predefinito del database db_owner possono eseguire sp_addsubscription. Per le sottoscrizioni pull, gli utenti che dispongono di un account di accesso nell'elenco di accesso della pubblicazione possono eseguire sp_addsubscription.  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_addtranpushsubscription_agent](../../relational-databases/replication/codesnippet/tsql/sp-addsubscription-trans_1.sql)]  
  
## <a name="see-also"></a>Vedere anche  
 [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md)   
 [Creare una sottoscrizione per un Sottoscrittore non SQL Server](../../relational-databases/replication/create-a-subscription-for-a-non-sql-server-subscriber.md)   
 [Subscribe to Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [sp_addpushsubscription_agent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md)   
 [sp_changesubstatus &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changesubstatus-transact-sql.md)   
 [sp_dropsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md)   
 [sp_helpsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
