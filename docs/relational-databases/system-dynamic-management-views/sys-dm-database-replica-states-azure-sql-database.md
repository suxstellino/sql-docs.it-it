---
description: sys.dm_database_replica_states (database SQL di Azure)
title: sys.dm_database_replica_states (database SQL di Azure) | Microsoft Docs
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_database_replica_states_TSQL
- sys.dm_database_replica_states
- dm_database_replica_states
- dm_database_replica_states_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.dm_database_replica_states dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4ce500e9a149844ee6516aac82975e0bbe1b6e7c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097778"
---
# <a name="sysdm_database_replica_states-azure-sql-database"></a>sys.dm_database_replica_states (database SQL di Azure)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Restituisce una riga per il database, esponendo lo stato per la replica locale.  
  
> [!IMPORTANT]
> A seconda dell'azione e degli stati di livello superiore, è possibile che le informazioni sullo stato del database non siano disponibili o non aggiornate. I valori hanno inoltre pertinenza esclusivamente locale. 
   
|Nome colonna|Tipo di dati|Descrizione (sulla replica primaria)|  
|-----------------|---------------|----------------------------------------|  
|**database_id**|**int**|Identificatore del database.|  
|**group_id**|**uniqueidentifier**|Identificatore del gruppo di disponibilità a cui appartiene il database.|  
|**replica_id**|**uniqueidentifier**|Identificatore della replica di disponibilità all'interno del gruppo di disponibilità.|  
|**group_database_id**|**uniqueidentifier**|Identificatore del database nel gruppo di disponibilità. L'identificatore è identico su ogni replica a cui è stato aggiunto questo database.|  
|**is_local**|**bit**|Se il database di disponibilità è locale, uno di:<br /><br /> 0 = Il database non è locale rispetto all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> 1 = Il database è locale rispetto all'istanza del server.|  
|**is_primary_replica**|**bit**|Restituisce 1 se la replica è primaria, o 0 se si tratta di una replica secondaria nel gruppo di disponibilità a cui appartiene il database. Questa operazione non fa riferimento al database primario o secondario in un gruppo di disponibilità distribuito o a una relazione di replica geografica attiva.<br /><br />**Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.|  
|**synchronization_state**|**tinyint**|Stato di spostamento dei dati, uno dei valori seguenti.<br /><br /> 0 = sincronizzazione non in corso. Per un database primario, indica che il database non è pronto per la sincronizzazione del log delle transazioni con i database secondari corrispondenti. Per un database secondario, indica che il database non ha ancora avviato la sincronizzazione del log a causa di un problema di connessione, è stato sospeso o si trova in stati di transizione durante l'avvio o un cambio di ruolo.<br /><br /> 1 = sincronizzazione in corso. Per un database primario, indica che il database è pronto ad accettare una richiesta di analisi da un database secondario. Per un database secondario, indica che è in corso uno spostamento dati attivo per il database.<br /><br /> 2 = sincronizzato. Un database primario risulta essere nello stato SYNCHRONIZED anziché SYNCHRONIZING. Un database secondario con commit sincrono risulta essere nello stato sincronizzato se nella cache locale il database è pronto per il failover ed è in corso la sincronizzazione.<br /><br /> 3 = ripristino. Indica la fase del processo di rollback in cui un database secondario ottiene attivamente le pagine dal database primario.<br />**Attenzione:** Quando un database in una replica secondaria si trova nello stato REVERTING, il failover forzato sulla replica secondaria lascia il database in uno stato in cui non può essere avviato come database primario. Il database dovrà essere riconnesso come un database secondario oppure sarà necessario applicare i nuovi record del log da un backup del log.<br /><br /> 4 = inizializzazione in corso. Indica la fase di rollback in cui il log delle transazioni necessario a un database secondario per l'intercettazione dell'LSN di rollback viene fornito e finalizzato su una replica secondaria.<br />**Attenzione:** Quando un database in una replica secondaria si trova nello stato di INIZIALIZZAzione, il failover forzato sulla replica secondaria lascia il database in uno stato in cui non può essere avviato come database primario. Il database dovrà essere riconnesso come un database secondario oppure sarà necessario applicare i nuovi record del log da un backup del log.|  
|**synchronization_state_desc**|**nvarchar(60)**|Descrizione dello stato di spostamento dei dati, uno di:<br /><br /> NOT SYNCHRONIZING<br /><br /> SYNCHRONIZING<br /><br /> SYNCHRONIZED<br /><br /> REVERTING<br /><br /> INITIALIZING|  
|**is_commit_participant**|**bit**|0 = il commit della transazione non è sincronizzato rispetto a questo database.<br /><br /> 1 = il commit della transazione è sincronizzato rispetto a questo database.<br /><br /> Per un database in una replica di disponibilità con commit asincrono, questo valore è sempre 0.<br /><br /> Per un database in una replica di disponibilità con commit sincrono, questo valore è preciso solo nel database primario.|  
|**synchronization_health**|**tinyint**|Riflette l'intersezione dello stato di sincronizzazione di un database aggiunto al gruppo di disponibilità nella replica di disponibilità e la modalità di disponibilità della replica di disponibilità (modalità con commit sincrono o asincrono), uno dei valori seguenti.<br /><br /> 0 = non integro. Il **synchronization_state** del database è 0 (non in sincronizzazione).<br /><br /> 1 = parzialmente integro. Un database in una replica di disponibilità con commit sincrono viene considerato parzialmente integro se **synchronization_state** è 1 (sincronizzazione).<br /><br /> 2 = integro. Un database in una replica di disponibilità con commit sincrono viene considerato integro se **synchronization_state** è 2 (sincronizzato) e un database in una replica di disponibilità con commit asincrono viene considerato integro se **synchronization_state** è 1 (sincronizzazione).|  
|**synchronization_health_desc**|**nvarchar(60)**|Descrizione del **synchronization_health** del database di disponibilità.<br /><br /> NOT_HEALTHY<br /><br /> PARTIALLY_HEALTHY<br /><br /> HEALTHY|  
|**database_state**|**tinyint**|0 = Online<br /><br /> 1 = Ripristino in corso<br /><br /> 2 = Recupero in corso<br /><br /> 3 = Recupero in sospeso<br /><br /> 4 = Sospetto<br /><br /> 5 = Emergenza<br /><br /> 6 = Offline<br /><br /> **Nota:** Uguale alla colonna **stato** in sys. databases.|  
|**database_state_desc**|**nvarchar(60)**|Descrizione della **database_state** della replica di disponibilità.<br /><br /> ONLINE<br /><br /> RESTORING<br /><br /> RECOVERING<br /><br /> RECOVERY_PENDING<br /><br /> SUSPECT<br /><br /> EMERGENCY<br /><br /> OFFLINE<br /><br /> **Nota:** Uguale a **state_desc** colonna in sys. databases.|  
|**is_suspended**|**bit**|Stato del database, uno di:<br /><br /> 0 = Ripreso<br /><br /> 1 = sospeso|  
|**suspend_reason**|**tinyint**|Se il database è sospeso, il motivo dello stato sospeso, uno di:<br /><br /> 0 = Azione utente<br /><br /> 1 = Sospensione da parte del partner<br /><br /> 2 = Rollforward<br /><br /> 3 = Acquisizione<br /><br /> 4 = Applicazione<br /><br /> 5 = Riavvio<br /><br /> 6 = Rollback<br /><br /> 7 = Riconvalida<br /><br /> 8 = Errore nel calcolo del punto di sincronizzazione della replica secondaria|  
|**suspend_reason_desc**|**nvarchar(60)**|Descrizione del motivo dello stato sospeso del database, uno di:<br /><br /> SUSPEND_FROM_USER = Movimento di dati sospeso manualmente dall'utente<br /><br /> SUSPEND_FROM_PARTNER = La replica di database viene sospesa dopo un failover forzato<br /><br /> SUSPEND_FROM_REDO = Si è verificato un errore durante la fase di rollforward<br /><br /> SUSPEND_FROM_APPLY = Si è verificato un errore durante la scrittura del log nel file (vedere il log degli errori)<br /><br /> SUSPEND_FROM_CAPTURE = Si è verificato un errore durante l'acquisizione del log sulla replica primaria<br /><br /> SUSPEND_FROM_RESTART = La replica di database è stata sospesa prima che il database venisse riavviato (vedere il log degli errori)<br /><br /> SUSPEND_FROM_UNDO = Si è verificato un errore durante la fase di annullamento (vedere il log degli errori)<br /><br /> SUSPEND_FROM_REVALIDATION = Rilevata la mancata corrispondenza con le modifiche al log alla riconnessione (vedere il log degli errori)<br /><br /> SUSPEND_FROM_XRF_UPDATE = Impossibile trovare il punto del log comune (vedere il log degli errori)|  
|**recovery_lsn**|**numeric(25,0)**|Nella replica primaria, la fine del log delle transazioni prima della scrittura di qualsiasi nuovo record di log da parte del database primario dopo il failover o il recupero. Se per un database secondario specificato questo valore è minore del valore LSN finale corrente (last_hardened_lsn), recovery_lsn rappresenta il valore in base al quale è necessario risincronizzare il database secondario (ripristino e reinizializzazione). Se questo valore è maggiore o uguale al valore LSN finale corrente, la risincronizzazione non è necessaria e non viene eseguita.<br /><br /> **recovery_lsn** riflette un ID del blocco di log riempito con zeri. Non si tratta di un numero di sequenza del file di log (LSN).|  
|**truncation_lsn**|**numeric(25,0)**|Nella replica primaria, per il database primario, riflette l'LSN di troncamento del log minimo in tutti i database secondari corrispondenti. Se il troncamento del log locale è bloccato, ad esempio da un'operazione di backup, questo LSN potrebbe essere maggiore di quello di troncamento locale.<br /><br /> Per un database secondario specificato, riflette il punto di troncamento del database in questione.<br /><br /> **truncation_lsn** riflette un ID del blocco di log riempito con zeri. Non si tratta di un numero di sequenza del file di log (LSN).|  
|**last_sent_lsn**|**numeric(25,0)**|Identificatore del blocco di log che indica il punto fino a cui tutti i blocchi di log sono stati inviati dal database primario. ID del blocco di log successivo che verrà inviato, anziché l'ID del blocco di log inviato più di recente.<br /><br /> **last_sent_lsn** riflette un ID del blocco di log riempito con zeri, non è un numero di sequenza del log effettivo.|  
|**last_sent_time**|**datetime**|Ora di invio dell'ultimo blocco di log.|  
|**last_received_lsn**|**numeric(25,0)**|ID del blocco di log che identifica il punto fino a cui tutti i blocchi di log sono stati ricevuti dalla replica secondaria che ospita questo database secondario.<br /><br /> **last_received_lsn** riflette un ID del blocco di log riempito con zeri. Non si tratta di un numero di sequenza del file di log (LSN).|  
|**last_received_time**|**datetime**|Ora in cui è stato letto l'ID del blocco di log nell'ultimo messaggio ricevuto sulla replica secondaria.|  
|**last_hardened_lsn**|**numeric(25,0)**|Avvio del blocco del log in cui sono contenuti i record di log dell'LSN ultima finalizzazione in un database secondario.<br /><br /> In un database primario con commit asincrono o in un database con commit sincrono i cui criteri correnti si riferiscono a un ritardo, il valore è NULL. Per altri database primari con commit sincrono, **last_hardened_lsn** indica il valore minimo dell'LSN finalizzato in tutti i database secondari.<br /><br /> **Nota: last_hardened_lsn** riflette un ID del blocco di log riempito con zeri. Non si tratta di un numero di sequenza del file di log (LSN).|  
|**last_hardened_time**|**datetime**|In un database secondario, ora dell'identificatore del blocco di log per l'LSN dell'ultima finalizzazione (**last_hardened_lsn**). In un database primario, riflette l'ora corrispondente all'LSN minimo finalizzato.|  
|**last_redone_lsn**|**numeric(25,0)**|Numero di sequenza del file di log (LSN) effettivo dell'ultimo record di log di cui è stato eseguito il rollforward nel database secondario. **last_redone_lsn** è sempre minore di **last_hardened_lsn**.|  
|**last_redone_time**|**datetime**|Ora del rollforward dell'ultimo record di log sul database secondario.|  
|**log_send_queue_size**|**bigint**|Quantità di record di log del database primario che non è stata inviata ai database secondari, espressa in KB.|  
|**log_send_rate**|**bigint**|Frequenza media con cui l'istanza della replica primaria ha inviato i dati durante l'ultimo periodo attivo, in kilobyte (KB).|  
|**redo_queue_size**|**bigint**|Quantità di record di log nei file di log della replica secondaria che non sono ancora stati sottoposti a rollforward (in KB).|  
|**redo_rate**|**bigint**|Velocità media in base alla quale viene eseguito il riesecuzione dei record di log in un determinato database secondario, in kilobyte (KB).|  
|**filestream_send_rate**|**bigint**|Velocità alla quale i file FILESTREAM vengono inviati alla replica secondaria (in KB/secondo).|  
|**end_of_log_lsn**|**numeric(25,0)**|Fine locale dell'LSN del log. LSN effettivo che corrisponde all'ultimo record di log nella cache di log sui database primario e secondari. Nella replica primaria, le righe secondarie riflettono la fine dell'LSN del log degli ultimi messaggi di stato inviati dalle repliche secondarie alla replica primaria.<br /><br /> **end_of_log_lsn** riflette un ID del blocco di log riempito con zeri. Non si tratta di un numero di sequenza del file di log (LSN).|  
|**last_commit_lsn**|**numeric(25,0)**|Numero di sequenza del file di log effettivo che corrisponde all'ultimo record di commit nel log delle transazioni.<br /><br /> Sul database primario corrisponde all'ultimo record di commit elaborato. Nelle righe per i database secondari viene mostrato il numero di sequenza del file di log (LSN) inviato dalla replica secondaria alla replica primaria.<br /><br /> Sulla replica secondaria si tratta dell'ultimo record di commit di cui è stato eseguito il rollforward.|  
|**last_commit_time**|**datetime**|Ora che corrisponde all'ultimo record di commit.<br /><br /> Sul database secondario, l'ora equivale a quella sul database primario.<br /><br /> Sulla replica primaria ogni riga del database secondario contiene l'ora in cui la replica secondaria che ospita il database secondario ha riferito alla replica primaria. La differenza di tempo tra la riga database primario e una determinata riga del database secondario rappresenta approssimativamente l'obiettivo del punto di ripristino (RPO), presupponendo che il processo di rollforward venga aggiornato e che lo stato di avanzamento sia stato segnalato alla replica primaria dalla replica secondaria.|  
|**low_water_mark_for_ghosts**|**bigint**|Numero a incremento progressivo costante per il database che indica un limite minimo usato dall'attività di pulizia dei record fantasma sul database primario. Se questo numero non aumenta nel tempo, implica che la pulizia dei record fantasma potrebbe non avvenire. Per decidere quali righe fantasma pulire, la replica primaria utilizza il valore minimo di questa colonna per questo database in tutte le repliche di disponibilità, inclusa quella primaria.|  
|**secondary_lag_seconds**|**bigint**|Numero di secondi durante i quali la replica secondaria è dietro la replica primaria durante la sincronizzazione.<br /><br />**Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] e versioni successive.|  
|**quorum_commit_lsn**|**numeric(25,0)**|Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.|
|**quorum_commit_time**|**datetime**|Identificato solo a scopo informativo. Non supportata. Non è garantita la compatibilità con le versioni future.|


## <a name="permissions"></a>Autorizzazioni

È richiesta l'autorizzazione VIEW DATABASE STATE per il database.


## <a name="see-also"></a>Vedere anche

- [Gruppi di disponibilità AlwaysOn di &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)
- [Monitorare Gruppi di disponibilità &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)
