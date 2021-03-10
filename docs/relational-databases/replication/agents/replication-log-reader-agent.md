---
title: Agente lettura log repliche | Microsoft Docs
description: Agente di lettura log repliche monitora i database di SQL Server configurati per la replica transazionale e copia le transazioni nel database di distribuzione.
ms.custom: ''
ms.date: 10/29/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Log Reader Agent, executables
- Log Reader Agent, parameter reference
- agents [SQL Server replication], Log Reader Agent
- command prompt [SQL Server replication]
ms.assetid: 5487b645-d99b-454c-8bd2-aff470709a0e
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 58e9ed82d9518d423001342fa288e2c7aba1f5bf
ms.sourcegitcommit: 98acedd435aecfda1b3c4c23d3f0c3c1a12682a4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/10/2021
ms.locfileid: "102532343"
---
# <a name="replication-log-reader-agent"></a>Agente lettura log repliche
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  Agente lettura log repliche è un eseguibile che consente di monitorare il log delle transazioni di tutti i database configurati per la replica transazionale e di copiare le transazioni contrassegnate per la replica dal log delle transazioni al database di distribuzione.  
  
> [!NOTE]  
>  I parametri possono essere specificati in qualsiasi ordine. Quando i parametri facoltativi non vengono specificati, vengono utilizzati i valori predefiniti basati sul profilo agente predefinito.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
logread [-?]   
-Publisher server_name[\instance_name]   
-PublisherDB publisher_database   
[-Continuous]  
[-DefinitionFile def_path_and_file_name]  
[-Distributor server_name[\instance_name]]  
[-DistributorLogin distributor_login]  
[-DistributorPassword distributor_password]  
[-DistributorSecurityMode [0|1]]  
[-EncryptionLevel [0|1|2]]  
[-ExtendedEventConfigFile configuration_path_and_file_name]  
[-HistoryVerboseLevel [0|1|2]]  
[-KeepAliveMessageInterval keep_alive_message_interval_seconds]  
[-LoginTimeOut login_time_out_seconds]  
[-LogScanThreshold scan_threshold]  
[-MaxCmdsInTran number_of_commands]  
[-MessageInterval message_interval]
[-MultiSubnetFailover [0|1]]
[-Output output_path_and_file_name]  
[-OutputVerboseLevel [0|1|2|3|4]]  
[-PacketSize packet_size]  
[-PollingInterval polling_interval]  
[-ProfileName profile_name]   
[-PublisherFailoverPartner server_name[\instance_name] ]  
[-PublisherSecurityMode [0|1]]  
[-PublisherLogin publisher_login]  
[-PublisherPassword publisher_password]   
[-QueryTimeOut query_time_out_seconds]  
[-ReadBatchSize number_of_transactions]   
[-ReadBatchThreshold read_batch_threshold]  
[-RecoverFromDataErrors]  
```  
  
## <a name="arguments"></a>Argomenti  
 **-?**  
 Visualizza le informazioni sull'utilizzo.  
  
 **-Publisher** _server_name_[ **\\** _instance_name_]  
 Nome del server di pubblicazione. Specificare *server_name* per l'istanza predefinita di [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in tale server. Specificare _server_name_ **\\** _instance_name_ per un'istanza denominata di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in tale server.  
  
 **-PublisherDB** _publisher_database_  
 Nome del database del server di pubblicazione.  
  
 **-Continuous**  
 Specifica se l'agente tenta di eseguire continuamente il polling delle transazioni replicate. Se specificato, l'agente esegue il polling delle transazioni replicate dall'origine in base agli intervalli di polling, anche se non vi sono transazioni in sospeso.  
  
 **-DefinitionFile** _def_path_and_file_name_  
 Percorso del file di definizione dell'agente. Un file di definizione dell'agente contiene argomenti della riga di comando per l'agente. Il contenuto del file viene analizzato come file eseguibile. Utilizzare virgolette doppie (") per specificare valori dell'argomento contenenti caratteri arbitrari.  
  
 **-Distributor** _server_name_[ **\\** _instance_name_]  
 Nome del database di distribuzione. Specificare *server_name* per l'istanza predefinita di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in tale server. Specificare _server_name_ **\\** _instance_name_ per un'istanza denominata di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in tale server.  
  
 **-DistributorLogin** _distributor_login_  
 Nome dell'account di accesso del database di distribuzione.  
  
 **-DistributorPassword** _distributor_password_  
 Password del database di distribuzione.  
  
 **-DistributorSecurityMode** [ **0**\| **1**]  
 Specifica la modalità di sicurezza del database di distribuzione. Un valore **0** indica la modalità di autenticazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (impostazione predefinita), mentre un valore **1** indica la modalità di autenticazione di [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows.  
  
 **-EncryptionLevel** [ **0** \| **1** \| **2** ]  
 Livello di crittografia TLS (Transport Layer Security), noto in precedenza come SSL (Secure Sockets Layer), usato dall'agente di lettura log quando vengono stabilite le connessioni.  
  
|Valore di EncryptionLevel|Descrizione|  
|---------------------------|-----------------|  
|**0**|Specifica che TLS non viene usato.|  
|**1**|Specifica che TLS viene usato, ma l'agente non verifica che il certificato server TLS/SSL sia firmato da un'autorità di certificazione attendibile.|  
|**2**|Specifica che TLS viene usato e che il certificato viene verificato.|  

 > [!NOTE]  
 >  Un certificato TLS/SSL valido è definito con un nome di dominio completo di SQL Server. Affinché l'agente possa connettersi correttamente quando si imposta - EncryptionLevel su 2, creare un alias nel Server SQL locale. Il parametro ‘Nome Alias’ deve essere il nome del server e il parametro ‘Server’ deve essere impostato per il nome completo di SQL Server.
 
 Per altre informazioni, vedere [Visualizzare e modificare le impostazioni di sicurezza della replica](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md).  
  
 **-ExtendedEventConfigFile** _configuration_path_and_file_name_  
 Consente di specificare il percorso e il nome del file di configurazione XML di eventi estesi. Il file di configurazione di eventi estesi consente di configurare sessioni e abilitare eventi per la traccia.  
  
 **-HistoryVerboseLevel** [ **0**\| **1**\| **2**]  
 Consente di specificare la quantità di cronologia registrata durante un'operazione dell'agente di lettura log. Per ridurre al minimo l'effetto della registrazione della cronologia sulle prestazioni, selezionare **1**.  
  
|Valore di HistoryVerboseLevel|Descrizione|  
|-------------------------------|-----------------|  
|**0**||  
|**1**|Valore predefinito. Aggiorna sempre un messaggio di cronologia precedente con lo stesso stato (avvio, avanzamento, esito positivo e così via). Se non è presente un record precedente con lo stesso stato, inserisce un nuovo record.|  
|**2**|Inserisce nuovi record della cronologia, a meno che il record sia per eventi come messaggi inattivi o messaggi di processo con esecuzione prolungata, nel qual caso aggiorna i record precedenti.|  
  
 **-KeepAliveMessageInterval** _keep_alive_message_interval_seconds_  
 Numero di secondi prima che il thread per la cronologia controlli se una delle connessioni esistenti è in attesa di una risposta dal server. Questo valore può essere ridotto per evitare che l'agente di controllo contrassegni l'agente di lettura log come sospetto in caso di esecuzione di un batch con esecuzione prolungata. Il valore predefinito è 300 secondi.  
  
 **-LoginTimeOut** _login_time_out_seconds_  
 Numero di secondi prima del timeout di accesso. Il valore predefinito è 15 secondi.  
  
 **-LogScanThreshold** _scan_threshold_  
 Solo per uso interno.  
  
 **-MaxCmdsInTran** _number_of_commands_  
 Specifica il numero massimo di istruzioni raggruppate in una transazione durante la scrittura dei comandi nel database di distribuzione da parte dell'agente di lettura log. L'utilizzo di questo parametro consente all'agente di lettura log e all'agente di distribuzione di dividere le transazioni di grandi dimensioni, ovvero costituite da molti comandi, nel server di pubblicazione in diverse transazioni più piccole quando applicate al Sottoscrittore. Può inoltre ridurre la possibilità che si verifichino contese nel server di distribuzione e diminuire la latenza tra il server di pubblicazione e il Sottoscrittore. Dal momento che la transazione originale viene applicata in unità più piccole, il Sottoscrittore può accedere alle righe di una vasta transazione logica del server di pubblicazione prima della fine della transazione originale, violando la rigida atomicità transazionale. Il valore predefinito è **0**, che consente di mantenere i limiti delle transazioni del server di pubblicazione.  
  
> [!NOTE]
>  Questo parametro viene ignorato per le pubblicazioni non [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Per ulteriori informazioni, vedere la sezione "Configurazione del processo del set di transazioni" in [Performance Tuning for Oracle Publishers](../../../relational-databases/replication/non-sql/performance-tuning-for-oracle-publishers.md).  
  
 **-MessageInterval** _message_interval_  
 Intervallo di tempo utilizzato per la registrazione della cronologia. Un evento della cronologia viene registrato quando viene raggiunto il valore di **MessageInterval** dopo la registrazione dell'ultimo evento della cronologia.  
  
 Se nell'origine non vi sono transazioni replicate disponibili, tramite l'agente viene inviato al server di distribuzione un messaggio che segnala l'assenza di transazioni. Questa opzione specifica per quanto tempo l'agente aspetta prima di inviare un altro messaggio di assenza di transazioni. Gli agenti inviano sempre un messaggio di assenza di transazioni quando rilevano che nell'origine non vi sono transazioni disponibili dopo aver elaborato in precedenza transazioni replicate. Il valore predefinito è 60 secondi.  
 
 **-MultiSubnetFailover** [**0**\|**1**] specifica se la proprietà MultiSubnetFailover è abilitata o meno. Se l'applicazione si connette a un gruppo di disponibilità Always On in subnet diverse, l'impostazione di MultiSubnetFailover su 1 (True) garantisce una maggiore velocità di rilevamento e connessione al server attualmente attivo.   
   **Si applica a**: [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)] ).  
  
 **-Output** _output_path_and_file_name_  
 Percorso del file di output dell'agente. Se non viene specificato il nome file, l'output viene inviato alla console. Se il nome file specificato esiste già, l'output viene aggiunto al file.  
  
 **-OutputVerboseLevel** [ **0**\| **1**\| **2** \| **3** \| **4** ]  
 Specifica se l'output deve essere dettagliato.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**0**|Vengono stampati solo i messaggi di errore.|  
|**1**|Vengono stampati tutti i messaggi di report di stato dell'agente.|  
|**2** (impostazione predefinita)|Vengono stampati tutti i messaggi di errore e i messaggi di report di stato dell'agente.|  
|**3**|Vengono stampati i primi 100 byte di ogni comando replicato.|  
|**4**|Vengono stampati tutti i comandi replicati.|  
  
 I valori 2-4 sono utili quando si esegue il debug.  
  
 **-PacketSize** _packet_size_  
 Dimensioni del pacchetto, in byte. Il valore predefinito è 4096 byte.  
  
 **-PollingInterval** _polling_interval_  
 Frequenza, in secondi, di esecuzione di query sul log per le transazioni replicate. Il valore predefinito è 5 secondi.  
  
 **-ProfileName** _profile_name_  
 Specifica un profilo agente da utilizzare per i parametri dell'agente. Se **ProfileName** è NULL, il profilo agente è disabilitato. Se **ProfileName** non viene specificato, viene utilizzato il profilo predefinito per il tipo di agente. Per altre informazioni, vedere [Profili degli agenti di replica](../../../relational-databases/replication/agents/replication-agent-profiles.md).  
  
 **-PublisherFailoverPartner** _server_name_[ **\\** _instance_name_]  
 Specifica l'istanza del partner di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] che partecipa in una sessione di mirroring del database con il database di pubblicazione. Per altre informazioni, vedere [Mirroring e replica del database &#40;SQL Server&#41;](../../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md).  
  
 **-PublisherSecurityMode** [ **0**\| **1**]  
 Specifica la modalità di sicurezza del server di pubblicazione. Un valore **0** indica la modalità di autenticazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (impostazione predefinita), mentre un valore **1** indica la modalità di autenticazione di Windows.  
  
 **-PublisherLogin** _publisher_login_  
 Nome dell'account di accesso del server di pubblicazione.  
  
 **-PublisherPassword** _publisher_password_  
 Password del server di pubblicazione.  
  
 **-QueryTimeOut** _query_time_out_seconds_  
 Numero di secondi prima del timeout delle query. Il valore predefinito è 1800 secondi.  
  
 **-ReadBatchSize** _number_of_transactions_  
 Numero massimo di transazioni lette dal log delle transazioni del database di pubblicazione per ciclo di elaborazione, con un valore predefinito di 500 e un valore massimo di 10000. L'agente continuerà a leggere transazioni nei batch fino a quando viene completata la lettura di tutte le transazioni nel log. Questo parametro non è supportato per i server di pubblicazione Oracle.  
  
 **-ReadBatchThreshold** _number_of_commands_  
 Numero di comandi di replica da leggere dal log delle transazioni prima del rilascio al Sottoscrittore da parte dell'agente di distribuzione. Il valore predefinito è 0. Se questo parametro non è specificato, l'agente di lettura log leggerà fino alla fine del log o fino al numero specificato in **-ReadBatchSize** (numero di transazioni).  
  
 **-RecoverFromDataErrors**  
 Specifica che l'esecuzione dell'agente di lettura continua anche nel caso in cui vengano rilevati errori nei dati di colonna pubblicati da un server di pubblicazione non SQL Server. Per impostazione predefinita, tali errori comportano l'interruzione dell'agente di lettura log. Quando si utilizza **-RecoverFromDataErrors**, i dati di colonna erronei vengono replicati come NULL o come valore non Null appropriato e i messaggi di avviso vengono registrati nella tabella [MSlogreader_history](../../../relational-databases/system-tables/mslogreader-history-transact-sql.md) . Questo parametro è supportato solo per i server di pubblicazione Oracle.  
  
## <a name="remarks"></a>Osservazioni  
  
> [!IMPORTANT]  
>  Se [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent è stato installato per l'esecuzione con un account di sistema locale anziché un account utente di dominio (impostazione predefinita), il servizio può accedere solo al computer locale. Se l'agente di lettura log in esecuzione in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent è configurato per l'utilizzo della modalità di autenticazione di Windows durante l'accesso a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], l'agente di lettura log si interrompe. L'impostazione predefinita prevede l'autenticazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Per informazioni sulla modifica degli account di sicurezza, vedere [View and Modify Replication Security Settings](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md).  
  
 Per avviare l'agente di lettura log, eseguire **logread.exe** dal prompt dei comandi. Per informazioni, vedere [Concetti di base relativi ai file eseguibili dell'agente di replica](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md).  
  
## <a name="change-history"></a>Cronologia modifiche  
  
|Contenuto aggiornato|  
|---------------------|  
|Aggiunta del parametro **-ExtendedEventConfigFile** .|  
|Aggiunta del parametro **-MultiSubnetFailover**.|
  
## <a name="see-also"></a>Vedere anche  
 [Amministrazione dell'agente di replica](../../../relational-databases/replication/agents/replication-agent-administration.md)  
  
  
