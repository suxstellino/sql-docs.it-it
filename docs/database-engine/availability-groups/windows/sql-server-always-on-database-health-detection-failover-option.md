---
title: Rilevamento dell'integrità a livello di database
description: Informazioni sulla funzionalità di rilevamento dell'integrità a livello di database per i gruppi di disponibilità Always On di SQL Server.
ms.custom: seo-lt-2019
ms.date: 01/19/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- AlwaysOn
- DB_FAILOVER
- Always On
- High Availability
- SQL Server
ms.assetid: d74afd28-25c3-48a1-bc3f-e353bee615c2
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1d0abb2a078c9dd2e7f540ee05b70b60dd756a34
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97639154"
---
# <a name="availability-group-database-level-health-detection-failover-option"></a>Opzione di failover di rilevamento dell'integrità a livello di database di un gruppo di disponibilità
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
A partire da SQL Server 2016, quando si configura un gruppo di disponibilità Always On è disponibile l'opzione di rilevamento dell'integrità a livello di database (DB_FAILOVER). Il rilevamento dell'integrità a livello di database avvisa quando un database non è più nello stato online, quando si verifica un errore, e attiverà il failover automatico del gruppo di disponibilità.

Il rilevamento dell'integrità a livello di database è abilitato per il gruppo di disponibilità nel suo complesso, pertanto il rilevamento dell'integrità a livello database consente di monitorare ogni database nel gruppo di disponibilità. Non può essere abilitato in modo selettivo per database specifici nel gruppo di disponibilità.

## <a name="benefits-of-database-level-health-detection-option"></a>Vantaggi dell'opzione di rilevamento dell'integrità a livello di database
---
L'opzione di rilevamento dell'integrità a livello di database di un gruppo di disponibilità è ampiamente consigliata come un'opzione valida per garantire la disponibilità elevata dei database. Considerare la possibilità di attivarla per tutti i gruppi di disponibilità. Se l'applicazione dipende da diversi database per garantire una disponibilità elevata, raggrupparli in un gruppo di disponibilità con l'opzione di integrità database attivata.

Ad esempio, con l'opzione di rilevamento dell'integrità a livello di database attivata, se SQL Server non è riuscito a scrivere nel file di log delle transazioni per uno dei database, lo stato di quel database cambia per indicare l'errore e il gruppo di disponibilità ne effettuerà subito il failover in modo tale che l'applicazione possa riconnettersi e continuare a lavorare con un'interruzione minima quando i database sono di nuovo online.

### <a name="enabling-database-level-health-detection"></a>Abilitazione del rilevamento dell'integrità a livello di database

Sebbene sia generalmente consigliata, l'opzione Integrità database è **disattivata per impostazione predefinita**, allo scopo di mantenere la compatibilità con le impostazioni predefinite nelle versioni precedenti.

Esistono diversi modi semplici per abilitare l'impostazione di rilevamento dell'integrità a livello di database:

1. In SQL Server Management Studio connettersi al motore di database di SQL Server. Nella finestra Esplora oggetti fare clic con il pulsante destro del mouse sul nodo Disponibilità elevata AlwaysOn ed eseguire la **Creazione guidata gruppo di disponibilità**. Selezionare la casella di controllo **Rilevamento dell'integrità a livello di database** nella pagina Specifica nome. Completare quindi il resto delle pagine della procedura guidata.

   ![Casella di controllo Rilevamento dell'integrità a livello di database](../../../database-engine/availability-groups/windows/media/always-on-enable-database-health-checkbox.png)

2. Visualizzare le **proprietà** di un gruppo di disponibilità esistente in SQL Server Management Studio. Connettersi a SQL Server. Nella finestra Esplora oggetti espandere il nodo Disponibilità elevata AlwaysOn. Espandere i gruppi di disponibilità. Fare clic con il pulsante destro del mouse sul gruppo di disponibilità e scegliere Proprietà. Selezionare l'opzione **Rilevamento dell'integrità a livello di database**, quindi fare clic su OK o eseguire lo script della modifica.

   ![Proprietà gruppo di disponibilità - Rilevamento dell'integrità a livello di database](../../../database-engine/availability-groups/windows/media/always-on-ag-properties-database-level-health-detection.png)


3. Sintassi Transact-SQL per **CREATE AVAILABILITY GROUP**. Il parametro DB_FAILOVER accetta i valori ON o OFF.

   ```sql
   CREATE AVAILABILITY GROUP [Contoso-ag]
   WITH (DB_FAILOVER=ON)
   FOR DATABASE [AutoHa-Sample]
   REPLICA ON
       N'SQLSERVER-0' WITH (ENDPOINT_URL = N'TCP://SQLSERVER-0.DOMAIN.COM:5022',
         FAILOVER_MODE = AUTOMATIC, AVAILABILITY_MODE = SYNCHRONOUS_COMMIT),
       N'SQLSERVER-1' WITH (ENDPOINT_URL = N'TCP://SQLSERVER-1.DOMAIN.COM:5022',
        FAILOVER_MODE = AUTOMATIC, AVAILABILITY_MODE = SYNCHRONOUS_COMMIT);
    ```

4. Sintassi Transact-SQL per **ALTER AVAILABILITY GROUP**. Il parametro DB_FAILOVER accetta i valori ON o OFF.

   ```sql
   ALTER AVAILABILITY GROUP [Contoso-ag] SET (DB_FAILOVER = ON);

   ALTER AVAILABILITY GROUP [Contoso-ag] SET (DB_FAILOVER = OFF);
   ```

### <a name="caveats"></a>Precisazioni

È importante notare che l'opzione Rilevamento dell'integrità a livello di database attualmente non prevede il monitoraggio dei tempi di attività dei dischi da parte di SQL Server e SQL Server non esegue direttamente il monitoraggio della disponibilità dei file di database. Se un'unità disco ha esito negativo o non è più disponibile, quell'unità da sola non attiva necessariamente il gruppo di disponibilità per il failover automatico.

Ad esempio, quando un database è inattivo senza transazioni attive e senza scritture fisiche in corso, se alcuni dei file di database diventano inaccessibili, SQL Server non potrà eseguire alcuna operazione I/O di lettura o scrittura nei file e non potrà modificare immediatamente lo stato di quel database, pertanto non verrà attivato alcun failover. In seguito, quando si verifica un checkpoint del database o un'operazione di lettura o scrittura fisica per soddisfare una query, SQL Server potrà notare il problema del file e reagire modificando lo stato del database e successivamente il gruppo di disponibilità con il rilevamento dell'integrità a livello di database attivato ne effettuerà il failover a causa della modifica di integrità del database.

Sempre a titolo di esempio, quando il motore di database di SQL Server deve leggere una pagina di dati per soddisfare una query, se la pagina di dati è memorizzata nella cache nella memoria del pool di buffer, è possibile che non sia necessaria alcuna lettura da disco con accesso fisico per soddisfare la richiesta di query. Pertanto, un file di dati mancanti o non disponibile potrebbe non attivare immediatamente un failover automatico anche quando è abilitata l'opzione di integrità del database, poiché lo stato del database non viene rilevato immediatamente.


## <a name="database-failover-is-separate-from-flexible-failover-policy"></a>Il failover del database è separato dai criteri di failover flessibili
Il rilevamento dell'integrità a livello di database implementa criteri di failover flessibili che consentono di configurare le soglie dell'integrità del processo di SQL Server per i criteri di failover. Il rilevamento dell'integrità a livello di database è configurato con il parametro DB_FAILOVER, mentre l'opzione del gruppo di disponibilità FAILURE_CONDITION_LEVEL è separata per la configurazione del rilevamento dell'integrità del processo di SQL Server. Le due opzioni sono indipendenti.

## <a name="managing-and-monitoring-database-level-health-detection"></a>Gestione e monitoraggio del rilevamento dell'integrità a livello di database

### <a name="dynamic-management-views"></a>DMV (Dynamic Management View)

La DMV di sistema sys.availability_groups mostra una colonna db_failover che indica se l'opzione di rilevamento dell'integrità a livello di database è disattivata (0) o attivata (1).

```sql
select name, db_failover from sys.availability_groups
```


Output di esempio della dmv:

|name  |  db_failover|
|---------|---------|
| Contoso-ag | 1  |

### <a name="errorlog"></a>ErrorLog
Il log degli errori di SQL Server (o il testo da sp_readerrorlog) mostrerà il messaggio di errore 41653 dopo il failover di un gruppo di disponibilità a causa dei controlli di rilevamento dell'integrità a livello di database.

Ad esempio, questo estratto del log degli errori mostra che la scrittura del log delle transazioni non è riuscita a causa di un problema del disco e successivamente il database denominato AutoHa-Sample è stato arrestato. Tale condizione ha attivato il rilevamento dell'integrità a livello di database per il failover del gruppo di disponibilità.

>2016-04-25 12:20:21.08 spid1s      Errore: 17053, gravità: 16, stato: 1.
>
>2016-04-25 12:20:21.08 spid1s      SQLServerLogMgr::LogWriter: è stato rilevato l'errore del sistema operativo 21(Il dispositivo non è pronto.).
>2016-04-25 12:20:21.08 spid1s      Errore di scrittura durante lo scaricamento del log.
>
>2016-04-25 12:20:21.08 spid79      Errore: 9001, gravità: 21, stato: 4.
>
>2016-04-25 12:20:21.08 spid79      Il log per il database 'AutoHa-Sample' non è disponibile. Controllare il registro eventi per i messaggi di errore correlati. Risolvere eventuali errori e riavviare il database.
>
>**2016-04-25 12:20:21.15 spid79      Errore: 41653, gravità: 21, stato: 1.**
>
>**2016-04-25 12:20:21.15 spid79      Si è verificato un errore del database 'AutoHa-Sampl' (tipo di errore: 2 ' DB_SHUTDOWN') causando un errore del gruppo di disponibilità 'Contoso-ag'.  Per informazioni sugli errori rilevati, vedere il log degli errori di SQL Server.  Se questa condizione persiste, contattare l'amministratore di sistema.**
>
>2016-04-25 12:20:21.17 spid79      Informazioni sullo stato del database 'AutoHa-Sample' -  LSN finalizzato: '(34:664:1)'    LSN di commit: '(34:656:1)'    Tempo di commit: 'Apr 25 2016 12:19PM'
>
>2016-04-25 12:20:21.19 spid15s     È stata terminata la connessione di Gruppi di disponibilità Always On con il database secondario per il database primario 'AutoHa-Sample' nella replica di disponibilità 'SQLServer-0' con ID replica: {c4ad5ea4-8a99-41fa-893e-189154c24b49}. Questo è un messaggio informativo. Non è richiesta alcuna azione da parte dell'utente.
>
>2016-04-25 12:20:21.21 spid75      Always On: la replica locale del gruppo di disponibilità 'Contoso-ag' sta per passare al ruolo primario in risposta a una richiesta del cluster WSFC (Windows Server Failover Clustering). Questo è un messaggio informativo. Non è richiesta alcuna azione da parte dell'utente.
>
>2016-04-25 12:20:21.21 spid75      Lo stato della replica di disponibilità locale nel gruppo di disponibilità 'ag' è cambiato da 'PRIMARY_NORMAL' a 'RESOLVING_NORMAL'.  Lo stato è cambiato perché il gruppo di disponibilità sta per passare offline.  La replica passerà offline perché il gruppo di disponibilità associato è stato eliminato, l'utente ha portato offline il gruppo di disponibilità associato nella console di gestione di Windows Server Failover Clustering (WSFC) o il gruppo di disponibilità sta effettuando il failover in un'altra istanza di SQL Server.  Per altre informazioni, vedere il log degli errori di SQL Server, la console di gestione di Windows Server Failover Clustering (WSFC) o il log WSFC.

### <a name="extended-event-sqlserveravailability_replica_database_fault_reporting"></a>Evento esteso sqlserver.availability_replica_database_fault_reporting

È stato definito un nuovo evento esteso a partire da SQL Server 2016 che viene attivato dal rilevamento dell'integrità a livello di database.  Il nome dell'evento è **sqlserver.availability_replica_database_fault_reporting**

Questo XEvent viene attivato solo nella replica primaria. L'XEvent viene attivato quando viene rilevato un problema di integrità a livello di database per un database ospitato in un gruppo di disponibilità.

Di seguito è riportato un esempio per creare una sessione XEvent che acquisisce questo evento. Poiché non è stato specificato alcun percorso, il file di output XEvent deve trovarsi nel percorso del log degli errori di SQL Server predefinito. Eseguire questa operazione nella replica primaria del gruppo di disponibilità:

Script di esempio per una sessione di eventi estesi

```sql
CREATE EVENT SESSION [AlwaysOn_dbfault] ON SERVER
ADD EVENT sqlserver.availability_replica_database_fault_reporting
ADD TARGET package0.event_file(SET filename=N'dbfault.xel',max_file_size=(5),max_rollover_files=(4))
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,
    MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)
GO
ALTER EVENT SESSION AlwaysOn_dbfault ON SERVER STATE=START
GO
```

#### <a name="extended-event-output"></a>Output dell'evento esteso
Con SQL Server Management Studio connettersi all'istanza primaria di Server SQL, espandere il nodo Gestione, quindi espandere Eventi estesi. Individuare la sessione (AlwaysOn_dbfault è il nome nell'esempio precedente) ed espanderlo per visualizzare i file di output. Fare clic sul file di output per aprire il file dell'evento in una nuova scheda.

Descrizione dei campi:

|Dati della colonna | Descrizione|
|---------|---------|
|availability_group_id |ID del gruppo di disponibilità.|
|availability_group_name |Nome del gruppo di disponibilità.|
|availability_replica_id |ID della replica di disponibilità.|
|availability_replica_name |Nome della replica di disponibilità.|
|database_name |Nome del database che segnala l'errore.|
|database_replica_id |ID del database della replica di disponibilità.|
|failover_ready_replicas |Numero di repliche secondarie per il failover automatico sincronizzate.|
|fault_type  | ID errore segnalato. Valori possibili:  <br/> 0: nessuno <br/>1: sconosciuto<br/>2: arresto|
|is_critical | Questo valore deve sempre restituire true per l'XEvent a partire da SQL Server 2016.|


In questo output di esempio fault_type indica che si è verificato un evento critico nel gruppo di disponibilità Contoso-ag nella replica denominata SQLSERVER-1, a causa del nome di database AutoHa-Sample2, con tipo di errore 2: arresto.

|Campo  | valore|
|---------|---------|
|availability_group_id | 24E6FE58-5EE8-4C4E-9746-491CFBB208C1|
|availability_group_name | Contoso-ag|
|availability_replica_id | 3EAE74D1-A22F-4D9F-8E9A-DEFF99B1F4D1|
|availability_replica_name | SQLSERVER-1|
|database_name | AutoHa-Sample2|
|database_replica_id | 39971379-8161-4607-82E7-098590E5AE00|
|failover_ready_replicas | 1|
|fault_type | 2|
|is_critical | True|


### <a name="related-references"></a>Riferimenti correlati

* [CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md)

* [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md)

* [Criteri di failover flessibili per failover automatico di un gruppo di disponibilità (SQL Server)](./configure-flexible-automatic-failover-policy.md)

* [Enhance AlwaysOn Failover Policy to Test SQL Server Database Data and Log Drives (Migliorare i criteri di failover AlwaysOn per testare le unità di log e i dati di database di SQL Server)](/archive/blogs/alwaysonpro/enhance-alwayson-failover-policy-to-test-sql-server-database-data-and-log-drives)

* [Eventi estesi](../../../relational-databases/extended-events/extended-events.md)