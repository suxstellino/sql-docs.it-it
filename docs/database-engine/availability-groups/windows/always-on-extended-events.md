---
title: Configurare eventi estesi per i gruppi di disponibilità
description: SQL Server definisce eventi estesi specifici per i gruppi di disponibilità Always On. È possibile monitorare questi eventi estesi all'interno di una sessione per facilitare la diagnosi della causa principale durante le attività di risoluzione dei problemi che interessano un gruppo di disponibilità.
ms.custom: ag-guide, seodec18
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: 5950f98a-3950-473d-95fd-cde3557b8fc2
author: rothja
ms.author: jroth
ms.openlocfilehash: 7ba062db14f9cb3d2ba262350c1ff5b93a079884
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091821"
---
# <a name="configure-extended-events-for-always-on-availability-groups"></a>Configurare eventi estesi per i gruppi di disponibilità Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  SQL Server definisce eventi estesi specifici per i gruppi di disponibilità Always On. È possibile monitorare questi eventi estesi all'interno di una sessione per facilitare la diagnosi della causa principale durante le attività di risoluzione dei problemi che interessano un gruppo di disponibilità. Per visualizzare gli eventi estesi di un gruppo di disponibilità è possibile usare la seguente query:  
  
```sql  
SELECT * FROM sys.dm_xe_objects WHERE name LIKE '%hadr%'  
```  
   
##  <a name="alwayson_health-session"></a><a name="BKMK_alwayson_health"></a>Sessione Alwayson_health  
 La sessione di eventi estesi alwayson_health viene creata automaticamente quando si crea il gruppo di disponibilità e acquisisce un subset degli eventi correlati al gruppo di disponibilità. Questa sessione è preconfigurata e rappresenta uno strumento semplice e rapido per consentire di iniziare rapidamente la risoluzione dei problemi di un gruppo di disponibilità. La Creazione guidata Gruppo di disponibilità avvia automaticamente la sessione in ogni replica di disponibilità partecipante configurata nella procedura guidata.  
  
> [!IMPORTANT]  
>  Se il gruppo di disponibilità non è stato creato tramite **Creazione guidata Gruppo di disponibilità**, la sessione alwayson_health non può essere avviata automaticamente. Se non è avviata, la sessione non può acquisire dati sugli eventi quando si verifica un problema imprevisto. Avviare la sessione manualmente e configurarla per l'avvio automatico configurando le proprietà della sessione.  
  
 Per visualizzare la definizione della sessione alwayson_health:  
  
1.  In **Esplora oggetti** espandere **Gestione**, **Eventi estesi**, quindi **Sessioni**.  
  
2.  Fare clic con il pulsante destro su **Alwayson_health**, scegliere **Crea script per sessione**, scegliere **CREATE in** e quindi fare clic su **Nuova finestra editor di query**.  

Per informazioni su alcuni eventi coperti da alwayson_health, vedere il [Riferimento degli eventi estesi](always-on-extended-events.md#BKMK_Reference).  


##  <a name="extended-events-for-debugging"></a><a name="BKMK_Debugging"></a>Eventi estesi per il debug  
 Oltre agli eventi estesi coperti dalla sessione Alwayson_health, SQL Server consente di definire un set completo di eventi di debug per i gruppi di disponibilità. Per sfruttare questi altri eventi estesi in una sessione, seguire le procedure seguenti:  
  
1.  In **Esplora oggetti** espandere **Gestione**, **Eventi estesi**, quindi **Sessioni**.  
  
2.  Fare clic con il pulsante destro del mouse su **Sessioni** e selezionare **Nuova sessione**. Oppure, fare clic con il pulsante destro su **Alwayson_health** e selezionare **Proprietà**.  
  
3.  Nel riquadro **Seleziona una pagina** fare clic su **Eventi**.  
  
4.  Nella libreria di eventi, colonna **Categoria**, selezionare **alwayson** e deselezionare tutte le altre categorie.  
  
5.  Nella colonna **Canale** selezionare **Debug**. Tutti gli eventi correlati al gruppo di disponibilità che non sono già selezionati sono ora visibili nella libreria di eventi.  
  
6.  Evidenziare un evento nella libreria di eventi e fare clic sul pulsante **>** per selezionarlo per la sessione.  
  
7.  Al termine della sessione, fare clic su **OK** per chiuderla. Assicurarsi che la sessione sia avviata in modo che acquisisca gli eventi selezionati.  
  
##  <a name="always-on-availability-groups-extended-events-reference"></a><a name="BKMK_Reference"></a> Riferimento degli eventi estesi dei gruppi di disponibilità Always On  
 Questa sezione descrive alcuni eventi estesi che vengono utilizzati per monitorare i gruppi di disponibilità.  
  
 [availability_replica_state_change](#BKMK_availability_replica_state_change)  
  
 [availability_group_lease_expired](#BKMK_availability_group_lease_expired)  
  
 [availability_replica_automatic_failover_validation](#BKMK_availability_replica_automatic_failover_validation)  
  
 [error_reported (vari numeri di errore): per i problemi di connessione o di trasporto](#BKMK_error_reported)  
  
 [data_movement_suspend_resume](#BKMK_data_movement_suspend_resume)  
  
 [alwayson_ddl_executed](#BKMK_alwayson_ddl_executed)  
  
 [availability_replica_manager_state](#BKMK_availability_replica_manager_state)  
  
 [error_reported (1480): Ruolo di replica di database modificato](#BKMK_error_reported_1480)  
  
###  <a name="availability_replica_state_change"></a><a name="BKMK_availability_replica_state_change"></a> availability_replica_state_change  
 Si verifica quando lo stato di una replica di disponibilità viene modificato. La creazione di un gruppo di disponibilità o l'aggiunta di una replica di disponibilità può attivare questo evento. È utile per la diagnostica dei failover automatici non riusciti. Può anche essere utilizzato per tracciare i passaggi del failover.  
  
#### <a name="event-information"></a>Informazioni sull'evento  
  
|Colonna|Descrizione|  
|------------|-----------------|  
|Nome|availability_replica_state_change|  
|Category|always on|  
|Channel|Operativo|  
  
#### <a name="event-fields"></a>Campi dell'evento  
  
|Nome|Type_name|Descrizione|  
|----------|----------------|-----------------|  
|availability_group_id|guid|ID del gruppo di disponibilità.|  
|availability_group_name|unicode_string|Nome del gruppo di disponibilità.|  
|availability_replica_id|guid|ID della replica di disponibilità.|  
|previous_state|availability_replica_state|Ruolo della replica prima della modifica.<br /><br /> **I valori possibili sono:**<br /><br /> Primary_Normal<br /><br /> Secondary_Normal<br /><br /> Resolving_Pending_Failover<br /><br /> Resolving_Normal<br /><br /> Primary_Pending<br /><br /> Not_Available|  
|current_state|availability_replica_state|Ruolo della replica dopo la modifica.<br /><br /> **I valori possibili sono:**<br /><br /> Primary_Normal<br /><br /> Secondary_Normal<br /><br /> Resolving_Pending_Failover<br /><br /> Resolving_Normal<br /><br /> Primary_Pending<br /><br /> Not_Available|  
  
#### <a name="alwayson_health-session-definition"></a>Definizione della sessione alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
ADD EVENT availability_replica_state_change  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
###  <a name="availability_group_lease_expired"></a><a name="BKMK_availability_group_lease_expired"></a> availability_group_lease_expired  
 Si verifica quando il gruppo di disponibilità e il cluster hanno un problema di connettività e il lease è scaduto. L'evento indica che la connettività tra il gruppo di disponibilità e il cluster WSFC sottostante è interrotta. Se il problema di connettività si verifica nella replica primaria, l'evento potrebbe causare un failover automatico o portare offline il gruppo di disponibilità.  
  
#### <a name="event-information"></a>Informazioni sull'evento  
  
|Colonna|Descrizione|  
|------------|-----------------|  
|Nome|availability_group_lease_expired|  
|Category|always on|  
|Channel|Operativo|  
  
#### <a name="event-fields"></a>Campi dell'evento  
  
|Nome|Type_name|Descrizione|  
|----------|----------------|-----------------|  
|availability_group_id|guid|ID del gruppo di disponibilità.|  
|availability_group_name|unicode_string|Nome del gruppo di disponibilità.|  
  
#### <a name="alwayson_health-session-definition"></a>Definizione della sessione alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
ADD EVENT availability_group_lease_expired  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
###  <a name="availability_replica_automatic_failover_validation"></a><a name="BKMK_availability_replica_automatic_failover_validation"></a> availability_replica_automatic_failover_validation  
 Si verifica alla convalida di failover della conformità della replica primaria e indica se la replica di disponibilità di destinazione è pronta per diventare la nuova replica primaria. Ad esempio, la convalida di failover restituisce il valore false quando non tutti i database sono sincronizzati o uniti. Questo evento è progettato per rappresentare un punto di errore durante i failover. Le informazioni sono di particolare interesse per l'amministratore del database specialmente per i failover automatici perché si tratta di operazioni automatiche. L'amministratore del database può esaminare l'evento per vedere perché un failover automatico ha causato un errore.  
  
#### <a name="event-information"></a>Informazioni sull'evento  
  
|Nome|Descrizione|  
|----------|-----------------|  
|availability_replica_automatic _failover_validation||  
|Category|always on|  
|Channel|Analitici|  
  
#### <a name="event-fields"></a>Campi dell'evento  
  
|Nome|Type_name|Descrizione|  
|----------|----------------|-----------------|  
|availability_group_id|guid|ID del gruppo di disponibilità.|  
|availability_group_name|unicode_string|Nome del gruppo di disponibilità.|  
|availability_replica_id|guid|ID della replica di disponibilità.|  
|forced_quorum|validation_result_type|Se ha valore TRUE, il failover automatico viene invalidato per la replica di disponibilità.<br /><br /> TRUE<br /><br /> FALSE|  
|joined_and_synchronized|validation_result_type|Se ha valore FALSE, il failover automatico viene invalidato per la replica di disponibilità.<br /><br /> TRUE<br /><br /> FALSE|  
|previous_primary_or_automatic_failover_target|validation_result_type|Se ha valore FALSE, il failover automatico viene invalidato per la replica di disponibilità.<br /><br /> TRUE<br /><br /> FALSE|  
  
#### <a name="alwayson_health-session-definition"></a>Definizione della sessione alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
  
ADD EVENT availability_replica_automatic_failover_validation (  
WHERE (  
[forced_quorum]=(TRUE) OR [joined_and_synchronized]=(FALSE) OR [previous_primary_or_automatic_failover_target]=(TRUE)  
)  
)  
  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
  
```  
  
###  <a name="error_reported-multiple-error-numbers-for-transport-or-connection-issues"></a><a name="BKMK_error_reported"></a> error_reported (vari numeri di errore): per i problemi di connessione o di trasporto  
 Ogni evento filtrato indica che si è verificato un problema di connettività nell'endpoint di mirroring del database o di trasporto da cui dipende il gruppo di disponibilità.  
  
|Colonna|Descrizione|  
|------------|-----------------|  
|Nome|error_reported<br /><br /> numeri da usare come filtro: 35201, 35202, 35206, 35204, 35207, 9642, 9666, 9691, 9692, 9693, 28034, 28036, 28080, 28091, 33309|  
|Category|errori|  
|Channel|Amministrativi|  
  
#### <a name="error-numbers-to-filter"></a>Numeri di errore da usare come filtro  
  
|Numero di errore|Descrizione|  
|------------------|-----------------|  
|35201|Si è verificato un timeout della connessione durante il tentativo di stabilire una connessione alla replica di disponibilità '%ls'.|  
|35202|La connessione per il gruppo di disponibilità '%ls' dalla replica di disponibilità '%ls' con ID [%ls] a '%ls' con ID [%ls] è stata stabilita correttamente.  Questo è un messaggio informativo. Non è richiesta alcuna azione da parte dell'utente.|  
|35206|Timeout di una connessione stabilita in precedenza con la replica di disponibilità '%ls'.|  
|35204|La connessione tra l'istanza '%ls' e '%ls' è stata disabilitata a causa dell'arresto dell'endpoint.|  
|Timeout + connessione|  
|35207|Impossibile stabilire la connessione sull'ID del gruppo di disponibilità '%ls' dall'ID replica '%ls' all'ID replica '%ls' a causa dell'errore %d, gravità %d, stato %d.  gravità %d, stato %d. (Potrebbe non avere un utilizzo DBA corretto. In questo caso verificare e rimuovere più avanti)|  
|9642|Errore in un endpoint della connessione del trasporto Service Broker/mirroring del database Errore: %i  Stato: %i. (Ruolo endpoint vicino: %S_MSG, indirizzo endpoint distante: '%.*hs') Errore: %i Stato: %i. (Ruolo endpoint vicino: %S_MSG, indirizzo endpoint distante: '%.\*hs')|  
|9666|L'endpoint %S_MSG si trova nello stato disabilitato o arrestato.|  
|9691|L'endpoint %S_MSG non è più in attesa delle connessioni.|  
|9692|L'endpoint %S_MSG non può restare in attesa sulla porta %d perché è utilizzato da un altro processo.|  
|9693|L'endpoint %S_MSG non può restare in attesa di connessioni a causa dell'errore seguente: '%.*ls'.|  
|28034|Handshake connessione non riuscito. L'account di accesso '%.*ls' non dispone dell'autorizzazione CONNECT per l'endpoint. Stato %d.|  
|28036|Handshake connessione non riuscito. Impossibile trovare il certificato utilizzato da questo endpoint: %S_MSG. Eseguire DBCC CHECKDB nel database master per verificare l'integrità dei metadati negli endpoint. Stato %d.|  
|28080|Handshake connessione non riuscito. Endpoint %S_MSG non configurato. Stato %d.|  
|28091|Avvio endpoint per %S_MSG senza autenticazione non supportato.|  
|33309|Impossibile avviare l'endpoint del cluster. Configurazione predefinita dell'endpoint %S_MSG non ancora caricata.|  
  
#### <a name="alwayson_health-session-definition"></a>Definizione della sessione alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
ADD EVENT sqlserver.error_reported(  
    WHERE   
(  
--Connectivity Error Messages  
[error_number]=(35201)   
OR [error_number]=(35202)   
OR [error_number]=(35204)   
OR [error_number]=(35206)   
OR [error_number]=(35207)   
OR [error_number]=(9642)   
--OR [error_number]=(9666)   
OR [error_number]=(9691)   
OR [error_number]=(9692)   
OR [error_number]=(9693)   
OR [error_number]=(28034)   
OR [error_number]=(28036)   
OR [error_number]=(28080)   
OR [error_number]=(28091)   
OR [error_number]=(33309)  
)  
)  
  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
###  <a name="data_movement_suspend_resume"></a><a name="BKMK_data_movement_suspend_resume"></a> data_movement_suspend_resume  
 Si verifica quando lo spostamento nel database di una replica di database viene sospeso o ripreso.  
  
#### <a name="event-information"></a>Informazioni sull'evento  
  
|Colonna|Descrizione|  
|------------|-----------------|  
|Nome|data_movement_suspend_resume|  
|Category|Sempre attiva|  
|Channel|Operativo|  
  
#### <a name="event-fields"></a>Campi dell'evento  
  
|Nome|Type_name|Descrizione|  
|-|-|-|  
|availability_group_id|guid|ID del gruppo di disponibilità.|  
|availability_group_name|unicode_string|Nome del gruppo di disponibilità, se disponibile.|  
|availability_replica_id|guid|ID della replica di disponibilità.|  
|database_replica_id|guid|ID del database di disponibilità.|  
|database_replica_name|unicode_string|Nome del database di disponibilità.|  
|database_id|uint32|ID del database di disponibilità.|  
|suspend_status|suspend_status_type|Valori dello stato di sospensione.<br /><br /> SUSPEND_NULL<br /><br /> RESUMED<br /><br /> SUSPENDED<br /><br /> SUSPENDED_INVALID|  
|suspend_source|suspend_source_type|Origine dell'azione di sospensione o ripresa.|  
|suspend_reason|unicode_string|Ragione sospensione acquisita nella gestione delle repliche di database.|  
  
#### <a name="alwayson_health-session-definition"></a>Definizione della sessione alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
  
ADD EVENT data_movement_suspend_resume (  
WHERE (  
[suspend_status]=(SUSPENDED)or [suspend_status]=(SUSPENDED_INVALID) or   
[suspend_status]=(SUSPEND_NULL)  
)  
)  
  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
###  <a name="alwayson_ddl_executed"></a><a name="BKMK_alwayson_ddl_executed"></a> alwayson_ddl_executed  
 Si verifica quando viene eseguita un'istruzione DDL del gruppo di disponibilità che include CREATE, ALTER o DROP. Lo scopo principale dell'evento è indicare un problema relativo a un'azione utente su una replica di disponibilità o di indicare il punto iniziale di un'azione operativa, seguita da un problema di runtime, ad esempio failover manuale, failover forzato, spostamento dei dati sospeso o spostamento dei dati ripreso.  
  
#### <a name="event-information"></a>Informazioni sull'evento  
  
|Colonna|Descrizione|  
|------------|-----------------|  
|Nome|alwayson_ddl_execution|  
|Category|always on|  
|Channel|Analitici|  
  
#### <a name="event-fields"></a>Campi dell'evento  
  
|Nome|Type_name|Descrizione|  
|----------|----------------|-----------------|  
|availability_group_id|Guid|ID del gruppo di disponibilità.|  
|availability_group_name|unicode_string|Nome del gruppo di disponibilità.|  
|ddl_action|alwayson_ddl_action|Indica il tipo di azione DDL: CREATE, ALTER o DROP.|  
|ddl_phase|ddl_opcode|Indica la fase dell'operazione DDL: BEGIN, COMMIT o ROLLBACK.|  
|.|unicode_string|Testo dell'istruzione eseguita.|  
  
#### <a name="alwayson_health-session-definition"></a>Definizione della sessione alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
  
ADD EVENT alwayson_ddl_executed  
  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
###  <a name="availability_replica_manager_state"></a><a name="BKMK_availability_replica_manager_state"></a> availability_replica_manager_state  
 Si verifica alla modifica dello stato di gestione repliche di disponibilità. Questo evento indica l'heartbeat della gestione repliche di disponibilità. Quando la gestione repliche di disponibilità non ha uno stato integro, tutte le repliche di disponibilità nell'istanza di SQL Server non saranno attive.  
  
#### <a name="event-information"></a>Informazioni sull'evento  
  
|Colonna|Descrizione|  
|------------|-----------------|  
|Nome|availability_replica_manager_state_change|  
|Category|always on|  
|Channel|Operativo|  
  
#### <a name="event-fields"></a>Campi dell'evento  
  
|Nome|Type_name|Descrizione|  
|----------|----------------|-----------------|  
|current_state|manager_state|Stato corrente di gestione repliche di disponibilità.<br /><br /> Online<br /><br /> Offline<br /><br /> WaitingForClusterCommunication|  
  
#### <a name="alwayson_health-session-definition"></a>Definizione della sessione alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
  
ADD EVENT availability_replica_manager_state (  
WHERE ([current_state]=(OFFLINE))  
)  
  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
###  <a name="error_reported-1480-database-replica-role-change"></a><a name="BKMK_error_reported_1480"></a> error_reported (1480): Ruolo di replica di database modificato  
 Questo evento error_reported filtrato avviene in modo asincrono dopo la modifica di un ruolo di replica di disponibilità. Indica in quali database di disponibilità non avviene la modifica del ruolo previsto durante il processo di failover.  
  
#### <a name="event-information"></a>Informazioni sull'evento  
  
|Colonna|Descrizione|  
|------------|-----------------|  
|Nome|error_reported<br /><br /> Numero di errore 1480: è in corso la modifica nel database REPLICATION_TYPE_MSG "DATABASE_NAME" dei ruoli da "OLD_ROLE" a "NEW_ROLE" a causa di REASON_MSG|  
|Category|errori|  
|Channel|Amministrativi|  
  
#### <a name="alwayson_health-session-definition"></a>Definizione della sessione alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
ADD EVENT sqlserver.error_reported(  
    WHERE   
(  
--database replica role change message  
OR [error_number] = (1480)  
  
--database replica runtime error messages  
OR [error_number]=(823)   
OR [error_number]=(824)   
OR [error_number]=(829)  
)  
)  
  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
## <a name="next-steps"></a>Passaggi successivi  
 [Visualizzare i dati della sessione eventi](/previous-versions/sql/sql-server-2012/hh710068(v=sql.110))