---
title: Ripartire il carico di lavoro alla replica secondaria di un gruppo di disponibilità
description: Informazioni sulla ripartizione di query e report di sola lettura in replica secondaria di un gruppo di disponibilità Always On in SQL Server.
ms.custom: seo-lt-2019
ms.date: 06/06/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- connection access to availability replicas
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], readable secondary replicas
- active secondary replicas [SQL Server], read-only access to
- readable secondary replicas
- Availability Groups [SQL Server], active secondary replicas
ms.assetid: 78f3f81a-066a-4fff-b023-7725ff874fdf
author: cawrites
ms.author: chadam
ms.openlocfilehash: 02e07cbb5b0fd6a917d6455c9d41a0fda3f03055
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343951"
---
# <a name="offload-read-only-workload-to-secondary-replica-of-an-always-on-availability-group"></a>Ripartire il carico di lavoro di sola lettura in una replica secondaria di un gruppo di disponibilità Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Le funzionalità secondarie attive di [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] includono il supporto per l'accesso in sola lettura a una o più repliche secondarie (*repliche secondarie leggibili*). Una replica secondaria leggibile può essere in modalità di disponibilità con commit sincrono o asincrono. Una replica secondaria leggibile consente l'accesso in sola lettura a tutti i relativi database secondari. Tuttavia, i database secondari leggibili non sono impostati per la sola lettura. Sono dinamici. Un database secondario viene modificato in base ai cambiamenti apportati al database primario corrispondente. Per una replica secondaria tipica, i dati presenti nel database secondario, comprese le tabelle durevoli con ottimizzazione per la memoria, sono quasi in tempo reale. Inoltre, gli indici full-text sono sincronizzati con i database secondari. In molte circostanze, la latenza dei dati tra un database primario e il database secondario corrispondente è in genere solo di pochi secondi.  
  
 Le impostazioni di sicurezza nei database primari vengono rese persistenti nei database secondari. Sono inclusi utenti, ruoli del database e delle applicazioni insieme alle rispettive autorizzazioni, nonché Transparent Data Encryption (TDE), se abilitato nel database primario.  
  
> [!NOTE]  
>  Nonostante non sia possibile scrivere dati nei database secondari, è possibile scrivere nei database di lettura-scrittura dell'istanza del server in cui è ospitata la replica secondaria, inclusi i database utente e quelli di sistema come **tempdb**.  
  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] supporta anche il reindirizzamento delle richieste di connessione con finalità di lettura a una replica secondaria leggibile (*routing di sola lettura*). Per informazioni sul routing di sola lettura, vedere [Uso di un listener per connettersi a una replica secondaria di sola lettura (routing di sola lettura)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md#ConnectToSecondary).  
  
##  <a name="benefits"></a><a name="bkmk_Benefits"></a> Vantaggi  
 L'indirizzamento di connessioni di sola lettura a repliche secondarie leggibili offre i seguenti vantaggi:  
  
-   Consente di scaricare i carichi di lavoro di sola lettura secondari dalla replica primaria, in cui sono conservate le relative risorse per i carichi di lavoro critici. In caso di carico di lavoro di lettura critico o di carico di lavoro per il quale non è possibile tollerare la latenza, si consiglia di effettuare la relativa esecuzione nella replica primaria.  
  
-   Consente di migliorare il rendimento dell'investimento per i sistemi in cui sono ospitate repliche secondarie leggibili.  
  
 Inoltre, le repliche secondarie leggibili forniscono un supporto affidabile per operazioni di sola lettura, come indicato di seguito:  
  
-   Le statistiche temporanee automatiche in un database secondario leggibile consentono di ottimizzare le query di sola lettura sulle tabelle basate su disco. Per le tabelle con ottimizzazione per la memoria, le statistiche mancanti vengono create automaticamente. Tuttavia, non è previsto alcun aggiornamento automatico delle statistiche non aggiornate. Sarà necessario aggiornare manualmente le statistiche nella replica primaria. Per altre informazioni, vedere [Statistiche](#Read-OnlyStats)per i database con accesso di sola lettura più avanti in questo argomento.  
  
-   Nei carichi di lavoro di sola lettura per le tabelle basate su disco viene usato il controllo delle versioni delle righe per rimuovere la contesa di blocco nei database secondari. Viene eseguito automaticamente il mapping a livello di transazioni di isolamento dello snapshot di tutte le query eseguite nei database secondari, anche quando gli altri livelli di isolamento delle transazioni sono impostati in modo esplicito. Tutti gli hint di blocco vengono ignorati. In questo modo si elimina la contesa lettore/writer.  
  
-   I carichi di lavoro di sola lettura per le tabelle ottimizzate per la memoria durevoli accedono ai dati esattamente nello stesso modo in cui si accede nel database primario, tramite le stored procedure native o l'interoperabilità SQL con le stesse limitazioni del livello di isolamento delle transazioni (vedere [Livelli di isolamento nel motore di database](/previous-versions/sql/sql-server-2008-r2/ms189122(v=sql.105))). Il carico di lavoro di report o le query di sola lettura in esecuzione nella replica primaria possono essere eseguiti nella replica secondaria senza richiedere alcuna modifica. Analogamente, un carico di lavoro di report o le query di sola lettura in esecuzione in una replica secondaria possono essere eseguiti nella replica primaria senza richiedere alcuna modifica.  In modo analogo alle tabelle basate su disco, viene eseguito automaticamente il mapping a livello di transazioni di isolamento dello snapshot di tutte le query eseguite nei database secondari, anche quando gli altri livelli di isolamento delle transazioni sono impostati in modo esplicito.  
  
-   Le operazioni DML sono consentite nelle variabili di tabella sia per i tipi di tabella basati su disco che per quelli ottimizzati per la memoria nella replica secondaria.  
  
##  <a name="prerequisites-for-the-availability-group"></a><a name="bkmk_Prerequisites"></a> Prerequisiti per il gruppo di disponibilità  
  
-   **Repliche secondarie leggibili (obbligatorio)**  
  
     L'amministratore del database deve configurare una o più repliche in modo tale da consentire tutte le connessioni (solo per l'accesso in lettura) o solo le connessioni con finalità di lettura quando vengono eseguite nel ruolo secondario.  
  
    > [!NOTE]  
    >  Facoltativamente, l'amministratore del database può configurare le repliche di disponibilità per escludere le connessioni in sola lettura quando l'esecuzione avviene nel ruolo primario.
  
     Per altre informazioni, vedere [Informazioni sull'accesso alla connessione client per le repliche di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md).  
  
    >[!WARNING]
    >  Solo le repliche che si trovano nella stessa build principale di SQL Server saranno leggibili. Per altre informazioni, vedere [Informazioni di base sull'aggiornamento in sequenza](upgrading-always-on-availability-group-replica-instances.md#rolling-upgrade-basics-for-always-on-ags).
  
  
-   **Listener del gruppo di disponibilità**  
  
     Per supportare il routing di sola lettura, un gruppo di disponibilità deve possedere un [listener del gruppo di disponibilità](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md). Il client in sola lettura deve indirizzare le richieste di connessione al listener e la stringa di connessione del client deve specificare la finalità dell'applicazione come in sola lettura, ovvero devono essere *richieste di connessione con finalità di lettura*.  
  
-   **Routing di sola lettura**  
  
     Con *routing di sola lettura* si intende la capacità di SQL Server di instradare le richieste di connessione in ingresso con finalità di lettura dirette a un listener del gruppo di disponibilità a una replica secondaria leggibile disponibile. I prerequisiti per il routing di sola lettura sono i seguenti:  
  
    -   Per supportare il routing di sola lettura una replica secondaria leggibile richiede un URL di routing di sola lettura. L'URL viene usato solo quando la replica locale viene eseguita nel ruolo secondario. L'URL di routing di sola lettura deve essere specificato per ogni singola replica in base alle esigenze. Ogni URL di routing di sola lettura viene usato per il routing delle richieste di connessione con finalità di lettura a una replica secondaria leggibile specifica. In genere, a ogni replica secondaria leggibile viene assegnato un URL di routing di sola lettura.  
  
    -   Ogni replica di disponibilità che deve supportare il routing di sola lettura quando viene eseguita come replica primaria richiede un elenco di routing di sola lettura. L'elenco di routing di sola lettura viene usato solo quando la replica locale viene eseguita nel ruolo primario. L'elenco deve essere specificato per ogni singola replica in base alle esigenze. In genere, ciascun elenco di routing di sola lettura deve contenere tutti gli URL di routing di sola lettura, con l'URL della replica locale alla fine dell'elenco.  
  
        > [!NOTE]  
        >  Il carico delle richieste di connessione con finalità di lettura può essere bilanciato tra le repliche. Per altre informazioni, vedere [Configurare il bilanciamento del carico tra le repliche di sola lettura](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md#loadbalancing).  
  
     Per altre informazioni, vedere [Configurare il routing di sola lettura per un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md).  
  
> [!NOTE]  
>  Per informazioni sui listener del gruppo di disponibilità e altre informazioni sul routing di sola lettura, vedere [Listener del gruppo di disponibilità, connettività client e failover dell'applicazione &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md).  
  
##  <a name="limitations-and-restrictions"></a><a name="bkmk_LimitationsRestrictions"></a> Limitazioni e restrizioni  
 Alcune operazioni non sono completamente supportate, come indicato di seguito:  
  
-   Non appena una replica leggibile viene abilitata per la lettura, può iniziare ad accettare connessioni ai relativi database secondari. Tuttavia, se in un database primario è presente una transazione attiva, le versioni di riga non saranno completamente disponibili nel database secondario corrispondente. È necessario eseguire il commit o il rollback di tutte le transazioni attive presenti nella replica primaria al momento della configurazione della replica secondaria. Fino a quando questo processo non viene completato, il mapping del livello di isolamento delle transazioni nel database secondario non è completo e le query sono temporaneamente bloccate.  
  
    > [!WARNING]  
    >  L'esecuzione di transazioni prolungate ha un impatto sul numero di righe con versione mantenute, sia per le tabelle basate su disco che per quelle ottimizzate per la memoria.  
  
-   In un database secondario con tabelle con ottimizzazione per la memoria, anche se le versioni di riga vengono generate sempre per le tabelle con ottimizzazione per la memoria, le query vengono bloccate finché non vengono completate tutte le transazioni attive presenti nella replica primaria quando la replica secondaria è stata abilitata per la lettura. In questo modo si garantisce che sia le tabelle basate su disco che quelle ottimizzate per la memoria siano disponibili contemporaneamente per il carico di lavoro di report e per le query di sola lettura.  
  
-   Le funzionalità di rilevamento delle modifiche e Change Data Capture non sono supportate nei database secondari che appartengono a una replica secondaria leggibile:  
  
    -   Il rilevamento delle modifiche è disabilitato in modo esplicito nei database secondari.  
  
    -   La funzionalità Change Data Capture non può essere abilitata solo in un database di replica secondaria. È possibile abilitare Change Data Capture nel database di replica primaria e leggere le modifiche dalle tabelle CDC usando le funzioni nel database di replica secondaria.  
  
-   Dal momento che viene eseguito il mapping delle operazioni di lettura al livello di transazioni di isolamento dello snapshot, la pulizia di record fantasma nella replica primaria può essere bloccata dalle transazioni in una o più repliche secondarie. L'attività di pulizia di record fantasma consentirà di pulire automaticamente i record fantasma per le tabelle basate su disco nella replica primaria se non più necessari per qualsiasi replica secondaria.  Questa operazione è simile a quella che viene effettuata quando si eseguono transazioni nella replica primaria. In caso estremo, nel database secondario sarà necessario terminare una query di lettura a esecuzione prolungata nella replica secondaria tramite cui si blocca la pulizia fantasma. Si noti che la pulizia fantasma può essere bloccata se la replica secondaria è disconnessa o quando lo spostamento dati è sospeso nel database secondario. Questo stato impedisce inoltre il troncamento del log, pertanto se lo stato persiste, si consiglia di rimuovere il database secondario dal gruppo di disponibilità. Non esiste alcun problema di pulizia di record fantasma con le tabelle con ottimizzazione per la memoria perché le versioni di riga vengono mantenute nella memoria e sono indipendenti dalle versioni di riga nella replica primaria.  
  
-   Potrebbe verificarsi un errore durante l'operazione DBCC SHRINKFILE nei file contenenti le tabelle basate su disco nella replica primaria se nel file sono contenuti record fantasma ancora necessari in una replica secondaria.  
  
-   A partire da [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)], le repliche secondarie leggibili possono rimanere online anche quando la replica primaria è offline a causa di un'azione dell'utente o di un errore, ad esempio se la sincronizzazione è stata sospesa a causa di un comando dell'utente o di un errore oppure se una replica sta risolvendo lo stato perché WSFC è offline. Tuttavia, il routing di sola lettura non funziona in questa situazione perché il listener del gruppo di disponibilità è offline. I client devono connettersi direttamente alle repliche secondarie di sola lettura per i carichi di lavoro in sola lettura.  
  
> [!NOTE]  
>  Se si esegue una query sulla DMV [sys.dm_db_index_physical_stats](../../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md) in un'istanza del server che ospita una replica secondaria leggibile, potrebbe verificarsi un problema di blocco della fase di rollforward. Questa condizione si verifica perché la DMV acquisisce un blocco IS nella vista o nella tabella utente specificata che può bloccare le richieste di una fase di rollforward per un blocco X presente in tale vista o tabella utente.  
  
##  <a name="performance-considerations"></a><a name="bkmk_Performance"></a> Considerazioni sulle prestazioni  
 In questa sezione si illustrano le diverse considerazioni sulle prestazioni relative ai database secondari leggibili.  
  
 **Contenuto della sezione**  
  
-   [Latenza dei dati](#DataLatency)  
  
-   [Impatto sui carichi di lavoro di sola lettura](#ReadOnlyWorkloadImpact)  
  
-   [Indicizzazione](#bkmk_Indexing)  
  
-   [Statistiche](#Read-OnlyStats)  
  
###  <a name="data-latency"></a><a name="DataLatency"></a> Latenza dei dati  
 L'implementazione dell'accesso di sola lettura alle repliche secondarie è utile qualora i carichi di lavoro di sola lettura possono tollerare una certa latenza dei dati. Nelle situazioni in cui la latenza dei dati non può essere accettata, si consideri la possibilità di eseguire i carichi di lavoro di sola lettura nella replica primaria.  
  
 I record di log delle modifiche sul database primario vengono inviati dalla replica primaria alle repliche secondarie. In ogni database secondario i record di log vengono applicati tramite un thread della fase di rollforward dedicato. In un database secondario di accesso in lettura, una modifica ai dati specificata non viene visualizzata nei risultati della query fino a quando il record di log, in cui è contenuta la modifica, non sarà stato applicato al database secondario e non è stato eseguito il commit della transazione nel database primario.  
  
 Ciò significa che si verifica della latenza, in genere solo pochi secondi, tra la replica primaria e quella secondaria. In rari casi, tuttavia, ad esempio se problemi di rete compromettono la velocità effettiva, la latenza può diventare significativa. La latenza aumenta quando si verificano colli di bottiglia I/O e quando viene sospeso lo spostamento dati. Per monitorare lo spostamento dati sospeso, è possibile usare il [dashboard Always On](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md) o la DMV [sys.dm_hadr_database_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) .  
  
####  <a name="data-latency-on-databases-with-memory-optimized-tables"></a><a name="bkmk_LatencyWithInMemOLTP"></a> Latenza dei dati nei database con tabelle ottimizzate per la memoria  
 [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] prevede alcune considerazioni speciali in relazione alla latenza dei dati per le repliche secondarie attive, vedere [[!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)]Repliche secondarie attive: repliche secondarie leggibili](https://technet.microsoft.com/library/ff878253(v=sql.120).aspx). A partire da [!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)], non esistono considerazioni speciali in relazione alla latenza dei dati per le tabelle ottimizzate per la memoria. La latenza dei dati prevista per le tabelle ottimizzate per la memoria è paragonabile a quella per le tabelle basate su disco.  
  
###  <a name="read-only-workload-impact"></a><a name="ReadOnlyWorkloadImpact"></a> Impatto sui carichi di lavoro di sola lettura  
 Quando si configura una replica secondaria per l'accesso di sola lettura, nei carichi di lavoro di sola lettura dei database secondari si usano le risorse di sistema, ad esempio CPU e I/O (per le tabella basate su disco) dai thread della fase di rollforward, soprattutto se i carichi di lavoro di sola lettura nelle tabelle basate su disco prevedono l'esecuzione di molte operazioni di I/O. Non esiste alcun impatto I/O quando si accede alle tabelle con ottimizzazione per la memoria perché tutte le righe si trovano in memoria.  
  
 Inoltre, i carichi di lavoro di sola lettura nelle repliche secondarie possono bloccare le modifiche DDL (Data Definition Language) applicate tramite record di log.  
  
-   Anche se nelle operazioni di lettura non si accettano i blocchi condivisi a causa del controllo delle versioni delle righe, in queste operazioni si accettano i blocchi di stabilità dello schema (Sch-S) che possono bloccare le operazioni di rollforward tramite cui si applicano le modifiche DDL. Le operazioni DDL includono operazioni ALTER/DROP di tabelle e viste ma non operazioni DROP o ALTER di stored procedure. Se, quindi, ad esempio si elimina una tabella basata su disco o con ottimizzazione per la memoria nel database primario, quando il thread della fase di rollforward elabora il record del log per eliminare la tabella, deve acquisire un blocco SCH_M nella tabella e può essere bloccato da una query in esecuzione che accede alla tabella.  Si tratta dello stesso comportamento della replica primaria con la differenza che l'eliminazione della tabella viene eseguita come parte di una sessione utente e non di un thread della fase di rollforward.  
  
-   Per le tabelle con ottimizzazione per la memoria sono previsti blocchi aggiuntivi. Eliminare una stored procedure nativa può comportare il blocco del thread della fase di rollforward se avviene l'esecuzione simultanea della stored procedure nativa nella replica secondaria. Si tratta dello stesso comportamento della replica primaria con la differenza che l'eliminazione della stored procedure viene eseguita come parte di una sessione utente e non di un thread della fase di rollforward.  
  
 Valutare le procedure consigliate relative alla compilazione delle query e applicarle ai database secondari. Pianificare, ad esempio, le query a lunga esecuzione come aggregazioni di dati durante i periodi di minore attività.  
  
> [!NOTE]  
>  Se un thread della fase di rollforward è bloccato da query in una replica secondaria, viene generato l'oggetto XEvent **sqlserver.lock_redo_blocked** .  
  
###  <a name="indexing"></a><a name="bkmk_Indexing"></a> Indicizzazione  
 Per ottimizzare i carichi di lavoro di sola lettura nelle repliche secondarie leggibili, è possibile creare indici nelle tabelle dei database secondari. Poiché non è possibile apportare modifiche allo schema o ai dati nei database secondari, creare indici nei database primari e consentire il trasferimento delle modifiche al database secondario attraverso il processo di rollforward.  
  
 Per monitorare l'attività di utilizzo dell'indice in una replica secondaria, eseguire una query sulle colonne **user_seeks**, **user_scans** e **user_lookups** della DMV [sys.dm_db_index_usage_stats](../../../relational-databases/system-dynamic-management-views/sys-dm-db-index-usage-stats-transact-sql.md) .  
  
###  <a name="statistics-for-read-only-access-databases"></a><a name="Read-OnlyStats"></a> Statistiche  
 Le statistiche sulle colonne di tabelle e viste indicizzate vengono usate per ottimizzare i piani di query. Per i gruppi di disponibilità, le statistiche create e gestite nei database primari vengono rese automaticamente persistenti nei database secondari come parte dell'applicazione dei record di log delle transazioni. Tuttavia, è possibile che per il carico di lavoro di sola lettura nei database secondari siano richieste statistiche diverse rispetto a quelle create nei database primari. Ad ogni modo, poiché i database secondari sono limitati all'accesso di sola lettura, non è possibile creare statistiche nei database secondari.  
  
 Per risolvere il problema, le statistiche temporanee per i database secondari vengono create e gestite dalla replica secondaria in **tempdb**. Il suffisso _readonly_database_statistic viene aggiunto al nome delle statistiche temporanee per distinguerle da quelle permanenti rese persistenti dal database primario.  
  
 Solo in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] è possibile creare e aggiornare le statistiche temporanee. È tuttavia possibile eliminare le statistiche temporanee e monitorare le relative proprietà usando gli stessi strumenti usati per le statistiche permanenti:  
  
-   Eliminare le statistiche temporanee usando l’istruzione [DROP STATISTICS](../../../t-sql/statements/drop-statistics-transact-sql.md) [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
-   Monitorare le statistiche usando le viste del catalogo **sys.stats** e **sys.stats_columns** . **sys_stats** include una colonna, **is_temporary**, che indica quali statistiche sono permanenti e quali invece temporanee.  
  
 L'aggiornamento automatico delle statistiche per le tabelle con ottimizzazione per la memoria nella replica primaria o secondaria non è supportato. È necessario monitorare i piani e le prestazioni delle query nella replica secondaria e aggiornare manualmente le statistiche nella replica primaria quando necessario. Tuttavia, le statistiche mancanti vengono create automaticamente sia nella replica primaria che in quella secondaria.  
  
 Per altre informazioni sulle statistiche di SQL Server, vedere [Statistiche](../../../relational-databases/statistics/statistics.md).  
  
 **Contenuto della sezione**  
  
-   [Statistiche permanenti non aggiornate nei database secondari](#StalePermStats)  
  
-   [Limitazioni e restrizioni](#StatsLimitationsRestrictions)  
  
####  <a name="stale-permanent-statistics-on-secondary-databases"></a><a name="StalePermStats"></a> Statistiche permanenti non aggiornate nei database secondari  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] è possibile rilevare situazioni in cui le statistiche permanenti in un database secondario non sono aggiornate. Tuttavia non è possibile apportare le modifiche alle statistiche permanenti se non modificando il database primario. Per l'ottimizzazione query, in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] è possibile creare statistiche temporanee per le tabelle basate su disco nel database secondario e usarle al posto di quelle permanenti non aggiornate.  
  
 Quando le statistiche permanenti vengono aggiornate nel database primario, vengono rese automaticamente persistenti nel database secondario. In [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] è quindi possibile usare le statistiche permanenti aggiornate che sono più recenti delle statistiche temporanee.  
  
 Se si esegue il failover del gruppo di disponibilità, le statistiche temporanee vengono eliminate in tutte le repliche secondarie.  
  
####  <a name="limitations-and-restrictions"></a><a name="StatsLimitationsRestrictions"></a> Limitazioni e restrizioni  
  
-   Poiché le statistiche temporanee sono archiviate in **tempdb**, un riavvio del servizio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] comporta l'indisponibilità di tutte le statistiche temporanee.  
  
-   Il suffisso _readonly_database_statistic è riservato alle statistiche generate da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Non è possibile usare questo suffisso quando si creano statistiche in un database primario. Per altre informazioni, vedere l'articolo relativo alle [statistiche](../../../relational-databases/statistics/statistics.md).  
  
##  <a name="accessing-memory-optimized-tables-on-a-secondary-replica"></a><a name="bkmk_AccessInMemTables"></a> Accesso alle tabelle ottimizzate per la memoria in una replica secondaria  
 Con le tabelle ottimizzate per la memoria in una replica secondaria possono essere usati gli stessi livelli di isolamento delle transazioni usati nella replica primaria. È consigliabile impostare il livello di isolamento a livello di sessione su READ COMMITTED e l'opzione a livello di database MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT su ON. Ad esempio:  
  
```sql  
ALTER DATABASE CURRENT SET MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT=ON  
GO  
SET TRANSACTION ISOLATION LEVEL READ COMMITTED  
GO  
SELECT SUM(UnitPrice*OrderQty)   
FROM Sales.SalesOrderDetail_inmem  
GO  
  
```  
  
##  <a name="capacity-planning-considerations"></a><a name="bkmk_CapacityPlanning"></a> Considerazioni sulla pianificazione della capacità  
  
-   In caso di tabelle basate su disco, le repliche secondarie leggibili possono richiedere spazio in **tempdb** per due motivi:  
  
    -   Le versioni di riga vengono copiate dal livello di isolamento dello snapshot in **tempdb**.  
  
    -   Le statistiche temporanee per i database secondari vengono create e mantenute in **tempdb**. Le statistiche temporanee possono provocare un lieve aumento delle dimensioni di **tempdb**. Per altre informazioni, vedere [Statistiche per i database con accesso di sola lettura](#Read-OnlyStats)più avanti in questa sezione.  
  
-   Quando si configura l'accesso in lettura per una o più repliche secondarie, nei database primari vengono aggiunti 14 byte di overhead sulle righe di dati eliminate, modificate o inserite per archiviare i puntatori alle versioni di riga nei database secondari per le tabelle basate su disco. L'overhead di 14 byte viene trasferito ai database secondari. Poiché l'overhead di 14 byte viene aggiunto alle righe di dati, è possibile che si verifichino divisioni di pagina.  
  
     I dati della versione di riga non sono generati dai database primari. Al contrario, i database secondari generano le versioni di riga. Tuttavia, il controllo delle versioni delle righe aumenta l'archiviazione dei dati nei database primari e secondari.  
  
     L'aggiunta dei dati della versione di riga dipende dall'impostazione del livello di isolamento dello snapshot o di isolamento dello snapshot Read committed sul database primario. Nella tabella seguente viene descritto il comportamento del controllo delle versioni in un database secondario leggibile con impostazioni diverse per le tabelle basate su disco.  
  
    |Replica secondaria leggibile?|L'isolamento dello snapshot o l'isolamento dello snapshot Read Committed è abilitato?|Database primario|Database secondario|  
    |---------------------------------|-----------------------------------------------|----------------------|------------------------|  
    |No|No|Nessuna versione di riga né overhead di 14 byte|Nessuna versione di riga né overhead di 14 byte|  
    |No|Sì|Versioni di riga e overhead di 14 byte|Nessuna versione di riga, ma overhead di 14 byte|  
    |Sì|No|Nessuna versione di riga, ma overhead di 14 byte|Versioni di riga e overhead di 14 byte|  
    |Sì|Sì|Versioni di riga e overhead di 14 byte|Versioni di riga e overhead di 14 byte|  
  
##  <a name="related-tasks"></a><a name="bkmk_RelatedTasks"></a> Attività correlate  
  
-   [Configurare l'accesso in sola lettura in una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
-   [Configurare il routing di sola lettura per un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)  
  
-   [Creare o configurare un listener del gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [Monitorare Gruppi di disponibilità &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
-   [Visualizzazione delle proprietà della replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-replica-properties-sql-server.md)  
  
-   [Utilizzare la finestra di dialogo Nuovo gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenuto correlato  
  
-   [Blog del team di SQL Server Always On: blog ufficiale del team di SQL Server Always On](/archive/blogs/sqlalwayson/)  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Informazioni sull'accesso alla connessione client per le repliche di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)   
 [Listener del gruppo di disponibilità, connettività client e failover dell'applicazione &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [Statistiche](../../../relational-databases/statistics/statistics.md)  
  
