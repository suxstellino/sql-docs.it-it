---
description: sys.event_log (Database di SQL Azure)
title: sys.event_log (database SQL di Azure) | Microsoft Docs
ms.custom: ''
ms.date: 01/28/2019
ms.service: sql-database
ms.prod_service: sql-database
ms.reviewer: ''
ms.topic: language-reference
f1_keywords:
- event_log
- sys.event_log_TSQL
- event_log_TSQL
- sys.event_log
dev_langs:
- TSQL
helpviewer_keywords:
- event_log
- sys.event_log
ms.assetid: ad5496b5-e5c7-4a18-b5a0-3f985d7c4758
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.openlocfilehash: c3a358e23d9428440166dd98a0f29ca8e4ea11c5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093101"
---
# <a name="sysevent_log-azure-sql-database"></a>sys.event_log (Database di SQL Azure)

[!INCLUDE[Azure SQL Database](../../includes/applies-to-version/asdb.md)]

  Restituisce [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] le connessioni al database, gli errori di connessione e i deadlock. È possibile usare queste informazioni per tenere traccia dell'attività del database con il [!INCLUDE[ssSDS](../../includes/sssds-md.md)] o per risolvere i problemi relativi.  
  
> [!CAUTION]  
> Per le installazioni con un numero elevato di database o un numero elevato di account di accesso, le attività in sys.event_log possono causare limitazioni nelle prestazioni, un utilizzo elevato della CPU e probabilmente causare errori di accesso. Le query di sys.event_log possono contribuire al problema. Microsoft sta lavorando per risolvere il problema. Nel frattempo, per ridurre l'effetto di questo problema, limitare le query di sys.event_log. Per ulteriori informazioni sulla configurazione, gli utenti del plug-in NewRelic SQL Server devono visitare [database SQL di Microsoft Azure ottimizzazione del plug-in & modifiche delle prestazioni](https://discuss.newrelic.com/t/microsoft-azure-sql-database-plugin-tuning-performance-tweaks/30729) .  
  
 La vista `sys.event_log` contiene le colonne seguenti.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**database_name**|**sysname**|Nome del database. Se la connessione ha esito negativo e l'utente non ha specificato un nome di database, questa colonna è vuota.|  
|**start_time**|**datetime2**|Data e ora UTC dell'inizio dell'intervallo di aggregazione. In caso di eventi aggregati, l'ora è sempre un multiplo di 5 minuti. Esempio:<br /><br /> 28/09/2011 16:00:00<br />'29-09-2011 16:05:00'<br />'28-09-2011 16:10:00'|  
|**end_time**|**datetime2**|Data e ora UTC della fine dell'intervallo di aggregazione. Per gli eventi aggregati, **end_time** è sempre esattamente 5 minuti dopo rispetto al **start_time** corrispondente nella stessa riga. Per gli eventi non aggregati, **start_time** e **end_time** uguali alla data e all'ora UTC effettive dell'evento.|  
|**event_category**|**nvarchar (64)**|Componente di alto livello tramite cui è stato generato l'evento.<br /><br /> Per un elenco di valori possibili, vedere [tipi di evento](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) .|  
|**event_type**|**nvarchar (64)**|Tipo di evento.<br /><br /> Per un elenco di valori possibili, vedere [tipi di evento](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) .|  
|**event_subtype**|**int**|Sottotipo dell'evento in corso.<br /><br /> Per un elenco di valori possibili, vedere [tipi di evento](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) .|  
|**event_subtype_desc**|**nvarchar (64)**|Descrizione del sottotipo di evento.<br /><br /> Per un elenco di valori possibili, vedere [tipi di evento](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) .|  
|**severity**|**int**|Gravità dell'errore. I valori possibili sono:<br /><br /> 0 = Informazioni<br />1 = Avviso<br />2 = errore|  
|**event_count**|**int**|Il numero di volte in cui si è verificato questo evento per il database specificato nell'intervallo di tempo specificato (**start_time** e **end_time**).|  
|**description**|**nvarchar(max)**|Descrizione dettagliata dell'evento.<br /><br /> Per un elenco di valori possibili, vedere [tipi di evento](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) .|  
|**additional_data**|**XML**|*Nota: questo valore è sempre NULL per il database SQL di Azure V12. Vedere la sezione [esempi](#Deadlock) per recuperare gli eventi di deadlock per V12.*<br /><br /> Per gli eventi **deadlock** , questa colonna contiene il grafico deadlock. La colonna è NULL per altri tipi di evento. |  
  
##  <a name="event-types"></a><a name="EventTypes"></a> Tipi di evento

 Gli eventi registrati da ogni riga di questa vista vengono identificati da una categoria (**event_category**), da un tipo di evento (**event_type**) e da un sottotipo (**event_subtype**). Nella tabella seguente sono elencati i tipi di eventi raccolti in questa vista.  
  
 Per gli eventi nella categoria **connettività** , le informazioni di riepilogo sono disponibili nella vista sys.database_connection_stats.  
  
> [!NOTE]  
> La vista non include tutti gli eventi possibili del database [!INCLUDE[ssSDS](../../includes/sssds-md.md)], ma solo quelli elencati. Nelle versioni future del [!INCLUDE[ssSDS](../../includes/sssds-md.md)] potranno venire aggiunte categorie, tipi di evento e sottotipi.  
  
|**event_category**|**event_type**|**event_subtype**|**event_subtype_desc**|**severity**|**description**|  
|-------------------------|---------------------|------------------------|------------------------------|------------------|---------------------|  
|**connettività**|**connection_successful**|0|**connection_successful**|0|Connessione al database effettuata.|  
|**connettività**|**connection_failed**|0|**invalid_login_name**|2|Il nome dell'account di accesso non è valido in questa versione di SQL Server.|  
|**connettività**|**connection_failed**|1|**windows_auth_not_supported**|2|Account di accesso di Windows non supportati in questa versione di SQL Server.|  
|**connettività**|**connection_failed**|2|**attach_db_not_supported**|2|L'utente ha richiesto di allegare un file di database che non è supportato.|  
|**connettività**|**connection_failed**|3|**change_password_not_supported**|2|L'utente ha richiesto di modificare la password dell'accesso utente, operazione non supportata.|  
|**connettività**|**connection_failed**|4|**login_failed_for_user**|2|Accesso non riuscito per l'utente.|  
|**connettività**|**connection_failed**|5|**login_disabled**|2|L'account di accesso è stato disabilitato.|  
|**connettività**|**connection_failed**|6|**failed_to_open_db**|2|*Nota: si applica solo al database SQL di Azure V11.*<br /><br /> Impossibile aprire il database. È possibile che il database non esista o che non sia stata effettuata l'autenticazione per aprire il database.|  
|**connettività**|**connection_failed**|7|**blocked_by_firewall**|2|Indirizzo IP del client non consentito per l'accesso al server.|  
|**connettività**|**connection_failed**|8|**client_close**|2|*Nota: si applica solo al database SQL di Azure V11.*<br /><br /> È possibile che si sia verificato un timeout del client nel tentativo di stabilire la connessione. Provare ad aumentare il timeout della connessione.|  
|**connettività**|**connection_failed**|9|**riconfigurazione**|2|*Nota: si applica solo al database SQL di Azure V11.*<br /><br /> Impossibile stabilire la connessione poiché era in corso una riconfigurazione del database.|  
|**connettività**|**connection_terminated**|0|**idle_connection_timeout**|2|*Nota: si applica solo al database SQL di Azure V11.*<br /><br /> Il tempo di inattività della connessione è stato maggiore rispetto alla soglia definita dal sistema.|  
|**connettività**|**connection_terminated**|1|**riconfigurazione**|2|*Nota: si applica solo al database SQL di Azure V11.*<br /><br /> La sessione è stata terminata a causa di una riconfigurazione del database.|  
|**connettività**|**limitazione**|*\<reason code>*|**reason_code**|2|*Nota: si applica solo al database SQL di Azure V11.*<br /><br /> La richiesta è limitata.  Codice motivo limitazione: *\<reason code>* . Per altre informazioni, vedere [limitazione del motore](/previous-versions/azure/dn338079(v=azure.100)).|  
|**connettività**|**throttling_long_transaction**|40549|**long_transaction**|2|*Nota: si applica solo al database SQL di Azure V11.*<br /><br /> Sessione terminata perché è presente una transazione a lunga esecuzione. Provare ad abbreviare la transazione. Per altre informazioni, vedere [limiti delle risorse](/previous-versions/azure/dn338081(v=azure.100)).|  
|**connettività**|**throttling_long_transaction**|40550|**excessive_lock_usage**|2|*Nota: si applica solo al database SQL di Azure V11.*<br /><br /> Sessione terminata perché ha acquisito troppi blocchi. Provare a leggere o modificare meno righe in una sola transazione. Per altre informazioni, vedere [limiti delle risorse](/previous-versions/azure/dn338081(v=azure.100)).|  
|**connettività**|**throttling_long_transaction**|40551|**excessive_tempdb_usage**|2|*Nota: si applica solo al database SQL di Azure V11.*<br /><br /> Sessione terminata a causa di un utilizzo eccessivo di TEMPDB. Provare a modificare la query per ridurre l'utilizzo dello spazio delle tabelle temporanee. Per altre informazioni, vedere [limiti delle risorse](/previous-versions/azure/dn338081(v=azure.100)).|  
|**connettività**|**throttling_long_transaction**|40552|**excessive_log_space_usage**|2|*Nota: si applica solo al database SQL di Azure V11.*<br /><br /> Sessione terminata a causa di un utilizzo eccessivo dello spazio del log delle transazioni. Provare a modificare meno righe in una sola transazione. Per altre informazioni, vedere [limiti delle risorse](/previous-versions/azure/dn338081(v=azure.100)).|  
|**connettività**|**throttling_long_transaction**|40553|**excessive_memory_usage**|2|*Nota: si applica solo al database SQL di Azure V11.*<br /><br /> Sessione terminata a causa di un utilizzo eccessivo della memoria. Provare a modificare la query per elaborare meno righe. Per altre informazioni, vedere [limiti delle risorse](/previous-versions/azure/dn338081(v=azure.100)).|  
|**motore**|**deadlock**|0|**deadlock**|2|Si è verificato un deadlock.|  
  
## <a name="permissions"></a>Autorizzazioni

 Gli utenti con l'autorizzazione per accedere al database **Master** hanno accesso in sola lettura a questa vista.  
  
## <a name="remarks"></a>Osservazioni  
  
### <a name="event-aggregation"></a>Aggregazione evento

 Le informazioni sull'evento per questa vista vengono raccolte e aggregate in intervalli di 5 minuti. La colonna **event_count** rappresenta il numero di volte in cui un determinato **event_type** e **event_subtype** si sono verificati per un database specifico in un determinato intervallo di tempo.  
  
> [!NOTE]  
> Alcuni eventi, ad esempio i deadlock, non vengono aggregati. Per questi eventi, **event_count** sarà 1 e **start_time** e **end_time** saranno uguali alla data e all'ora UTC effettive in cui si è verificato l'evento.  
  
 Ad esempio, se un utente non è riuscito a connettersi al database Database1 per sette volte tra le 11:00 e le 11:05 del 2/5/2012 (UTC) a causa di un nome account di accesso non valido, queste informazioni sono disponibili in una singola riga di questa vista:  
  
|**database_name**|**start_time**|**end_time**|**event_category**|**event_type**|**event_subtype**|**event_subtype_desc**|**severity**|**event_count**|**description**|**additional_data**|  
|------------------------|---------------------|-------------------|-------------------------|---------------------|------------------------|------------------------------|------------------|----------------------|---------------------|--------------------------|  
|`Database1`|`2012-02-05 11:00:00`|`2012-02-05 11:05:00`|`connectivity`|`connection_failed`|`4`|`login_failed_for_user`|`2`|`7`|`Login failed for user.`|`NULL`|  
  
### <a name="interval-start_time-and-end_time"></a>start_time e end_time dell'intervallo  
 Un evento è incluso in un intervallo di aggregazione quando l'evento si verifica *in* o _dopo_**start_time** e _prima_**end_time** per tale intervallo. Ad esempio, un evento che si verifica esattamente il `2012-10-30 19:25:00.0000000` è incluso solo nel secondo intervallo indicato di seguito:  
  
```
start_time                    end_time  
2012-10-30 19:20:00.0000000   2012-10-30 19:25:00.0000000  
2012-10-30 19:25:00.0000000   2012-10-30 19:30:00.0000000  
```

### <a name="data-updates"></a>Aggiornamenti dei dati

 I dati in questa vista vengono accumulati nel tempo. In genere, vengono accumulati entro un'ora dall'inizio dell'intervallo di aggregazione, ma la visualizzazione di tutti i dati nella vista potrebbe richiedere fino a un massimo di 24 ore. Durante questo tempo, le informazioni contenute all'interno di una singola riga possono essere aggiornate periodicamente.  
  
### <a name="data-retention"></a>Conservazione dei dati

 I dati in questa vista vengono conservati per un massimo di 30 giorni o meno, a seconda del numero di database e del numero di eventi univoci generati da ogni database. Per prolungare il mantenimento di queste informazioni, copiare i dati in un database separato. Dopo aver creato una copia iniziale della vista, le relative righe possono essere aggiornate quando i dati vengono accumulati. Per mantenere aggiornata la copia dei dati, eseguire periodicamente un'analisi delle righe della tabella per cercare un eventuale aumento del numero di eventi di righe esistenti e per identificare le righe nuove (è possibile effettuare questa operazione per le righe univoche mediante le ore di inizio e di fine), quindi aggiornare la copia dei dati con queste modifiche.  
  
### <a name="errors-not-included"></a>Errori non inclusi

 In questa vista non possono essere incluse tutte le informazioni relative a connessioni ed errori:  
  
- Questa vista non include tutti gli [!INCLUDE[ssSDS](../../includes/sssds-md.md)] errori del database che possono verificarsi, ma solo quelli specificati nei [tipi di evento](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) in questo argomento.  
- Se si verifica un errore del computer nel [!INCLUDE[ssSDS](../../includes/sssds-md.md)] Data Center, è possibile che nella tabella eventi manchi una piccola quantità di dati.  
- Se un indirizzo IP è stato bloccato tramite DoSGuard, gli eventi di tentativi di connessione dall'indirizzo IP in questione non possono essere raccolti, né verranno visualizzati in questa vista.  
  
## <a name="examples"></a>Esempi  
  
### <a name="simple-examples"></a>Esempi semplici

 La query seguente restituisce tutti gli eventi che si sono verificati tra le ore 12:00 del 9/25/2011 e le ore 12:00 del 9/28/2011 (UTC). Per impostazione predefinita, i risultati della query vengono ordinati in base **start_time** (ordine crescente).  

```sql
SELECT * FROM sys.event_log
WHERE start_time >= '2011-09-25 12:00:00'
    AND end_time <= '2011-09-28 12:00:00';  
```

La query seguente restituisce tutti gli eventi deadlock per il database Database1 (si applica solo al database SQL di Azure V11).  

```sql
SELECT * FROM sys.event_log
WHERE event_type = 'deadlock'
    AND database_name = 'Database1';  
```

<a name="Deadlock"></a> La query seguente restituisce tutti gli eventi deadlock per il database Database1 (si applica solo alla versione 12 del database SQL di Azure).  

```sql
WITH CTE AS (  
       SELECT CAST(event_data AS XML)  AS [target_data_XML]  
   FROM sys.fn_xe_telemetry_blob_target_read_file('dl', null, null, null)  
)  
SELECT target_data_XML.value('(/event/@timestamp)[1]', 'DateTime2') AS Timestamp,  
target_data_XML.query('/event/data[@name=''xml_report'']/value/deadlock') AS deadlock_xml,  
target_data_XML.query('/event/data[@name=''database_name'']/value').value('(/value)[1]', 'nvarchar(100)') AS db_name  
FROM CTE  
```

La query seguente restituisce la limitazione a livello hardware per gli eventi dei thread di lavoro SQL che si sono verificati tra le 10:00 e le 11:00 del 9/25/2011 (UTC).  

```sql
SELECT * FROM sys.event_log
WHERE event_type = 'throttling'
    AND event_subtype = 4194307
    AND start_time >= '2011-09-25 10:00:00'
    AND end_time <= '2011-09-25 11:00:00';  
```

### <a name="db-scoped-extended-event"></a>Evento esteso DB-Scoped

 Usare il codice di esempio seguente per configurare la sessione di eventi estesi (XEvent) con ambito database:  

```sql
IF EXISTS  
    (SELECT * from sys.database_event_sessions  
        WHERE name = 'azure_monitor_deadlock_session')  
BEGIN  
    ALTER EVENT SESSION azure_monitor_deadlock_session  
        ON DATABASE  
        DROP TARGET package0.ring_buffer;  
  
    DROP EVENT SESSION azure_monitor_deadlock_session  
        ON DATABASE;  
END  
  
CREATE EVENT SESSION azure_monitor_deadlock_session  
    ON DATABASE  
    ADD EVENT sqlserver.database_xml_deadlock_report  
    ADD TARGET package0.ring_buffer  
    (  
        SET max_memory = 2048, max_events_limit = 10  
    )  
    WITH (STARTUP_STATE = ON,  
          EVENT_RETENTION_MODE = ALLOW_SINGLE_EVENT_LOSS);  
  
ALTER EVENT SESSION azure_monitor_deadlock_session  
    ON DATABASE  
    STATE = START;  
```

### <a name="check-for-deadlock"></a>Verifica deadlock

Utilizzare la query seguente per verificare la presenza di un deadlock.  

```sql
WITH CTE AS (  
    SELECT CAST(xet.target_data AS XML)  AS [target_data_XML]  
        FROM            sys.dm_xe_database_session_targets AS xet  
             INNER JOIN sys.dm_xe_database_sessions        AS xe  
                 ON (xe.address = xet.event_session_address)  
        WHERE xe.name = 'azure_monitor_deadlock_session'  
)  
, CTE2 AS (  
    SELECT  
            T2.EventData.query('.').value(  
                '(/event/@timestamp)[1]', 'DateTime2') AS Timestamp,  
            T2.EventData.query('.').query(  
                '(/event/data/value/deadlock)[1]')     AS deadlock_xml  
        FROM CTE  
            CROSS Apply [target_data_XML].nodes(  
                '/RingBufferTarget/event') AS T2(EventData)  
)  
SELECT * FROM CTE2;  
```

## <a name="see-also"></a>Vedere anche

 [Eventi estesi nel database SQL di Azure](/azure/azure-sql/database/xevent-db-diff-from-svr)  
