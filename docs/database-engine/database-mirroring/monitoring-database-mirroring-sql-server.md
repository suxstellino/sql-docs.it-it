---
title: Monitoraggio del mirroring del database (SQL Server) | Microsoft Docs
description: Informazioni su Monitoraggio mirroring del database, sulle stored procedure di sistema e sul funzionamento del monitoraggio del mirroring del database, incluso il processo Monitoraggio mirroring del database.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- monitoring [SQL Server], database mirroring
- database mirroring [SQL Server], monitoring
ms.assetid: a7b1b9b0-7c19-4acc-9de3-3a7c5e70694d
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: ece89606626172d50ba656a6545237af2ec761fe
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783409"
---
# <a name="monitoring-database-mirroring-sql-server"></a>Monitoraggio del mirroring del database (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Questa sezione presenta il monitoraggio del mirroring del database e le stored procedure di sistema **sp_dbmmonitor** , descrive il funzionamento del monitoraggio del mirroring del database, incluso il funzionamento del processo **Monitoraggio mirroring del database**, e fornisce un riepilogo delle informazioni che è possibile monitorare sulle sessioni di mirroring del database. Vengono inoltre fornite informazioni generali sulla definizione di valori soglia degli avvisi per un set di eventi di mirroring del database predefiniti e sull'impostazione di avvisi per qualsiasi evento di mirroring del database.  
  
 È possibile monitorare un database con mirroring durante una sessione di mirroring per verificare se i dati fluiscono correttamente. Per impostare e gestire il monitoraggio per uno o più database con mirroring in un'istanza del server, è possibile usare Monitoraggio mirroring del database oppure le stored procedure di sistema **sp_dbmmonitor** .  
  
 Un processo di **Monitoraggio mirroring del database** agisce in background in modo indipendente da Monitoraggio mirroring del database. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent chiama a intervalli regolari (per impostazione predefinita ogni minuto) il processo di **Monitoraggio mirroring del database** , il quale chiama a sua volta una stored procedure che aggiorna lo stato di mirroring. Se si utilizza [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] per avviare una sessione di mirroring, il **Processo di Monitoraggio mirroring del database** viene creato automaticamente. Se, invece, si usa solo ALTER DATABASE *<nome_database>* SET PARTNER per avviare il mirroring, è necessario creare il processo eseguendo una stored procedure.  
  
 **Contenuto dell'argomento**  
  
-   [Monitoraggio dello stato di mirroring](#MonitoringStatus)  
  
-   [Fonti di informazioni aggiuntive su un database con mirroring](#AdditionalSources)  
  
-   [Attività correlate](#RelatedTasks)  
  
##  <a name="monitoring-mirroring-status"></a><a name="MonitoringStatus"></a> Monitoraggio dello stato di mirroring  
 Per impostare e gestire il monitoraggio per uno o più database con mirroring in un'istanza del server, è possibile utilizzare Monitoraggio mirroring del database o le stored procedure di sistema **dbmmonitor** . È possibile monitorare un database con mirroring durante una sessione di mirroring per verificare se i dati fluiscono correttamente.  
  
 In particolare, il monitoraggio di un database con mirroring consente di:  
  
-   Verificare il funzionamento del mirroring.  
  
     Nello stato di base sono incluse informazioni che indicano se le due istanze del server sono attive, se i server sono connessi e se il log viene spostato dal database principale al database mirror.  
  
-   Determinare se il database mirror è in grado di mantenersi aggiornato rispetto al database principale.  
  
     Durante la modalità a prestazioni elevate, è possibile che il server principale sviluppi un backlog di record di log non inviati che devono essere inviati dal server principale al server mirror. In qualsiasi modalità operativa, inoltre, è possibile che il server mirror sviluppi un backlog di record di log non ripristinati scritti nel file di log, ma ancora da ripristinare nel database mirror.  
  
-   Determinare quanti dati sono stati persi quando l'istanza del server principale si è resa non disponibile durante la modalità a prestazioni elevate.  
  
     È possibile determinare la perdita di dati osservando la quantità di log di transazioni non inviate, se presenti, e l'intervallo di tempo in cui è stato eseguito il commit delle transazioni perse nel server principale.  
  
-   Confrontare le prestazioni correnti con quelle passate.  
  
     In caso di problemi, un amministratore di database può visualizzare una cronologia delle prestazioni del mirroring utile per comprendere lo stato corrente. La consultazione della cronologia consente all'utente di rilevare le tendenze nelle prestazioni, individuare modelli dei problemi relativi alle prestazioni, ad esempio gli orari in cui la rete è lenta o il numero di comandi immessi nel log è eccessivo.  
  
-   Risolvere i problemi che causano la riduzione del flusso di dati tra i partner per il mirroring.  
  
-   Impostare le soglie di avviso in base alle misurazioni chiave delle prestazioni.  
  
     Se una nuova riga di stato contiene un valore superiore a una soglia, viene inviato un evento informativo al registro eventi di Windows. Un amministratore di sistema può quindi configurare manualmente gli avvisi in base a questi eventi. Per altre informazioni, vedere [Usare valori di soglia avvisi e avvisi sulle metriche delle prestazioni di mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server.md).  
  
###  <a name="tools-for-monitoring-database-mirroring-status"></a><a name="tools_for_monitoring_dbm_status"></a> Strumenti per il monitoraggio dello stato di mirroring del database  
 Lo stato di mirroring può essere monitorato usando Monitoraggio mirroring del database o la stored procedure di sistema **sp_dbmmonitorresults** . Questi strumenti possono essere usati per monitorare il mirroring del database in qualsiasi database con mirroring nell'istanza del server locale sia parte degli amministratori di sistema, ovvero i membri del ruolo predefinito del server **sysadmin** , sia da parte dell'utente aggiunto al ruolo predefinito del database **dbm_monitor** nel database **msdb** da un amministratore di sistema. Utilizzando questi strumenti, un amministratore di sistema può anche aggiornare manualmente lo stato di mirroring.  
  
> [!NOTE]  
>  Gli amministratori di sistema possono anche configurare e visualizzare le soglie di avviso per le misurazioni chiave delle prestazioni. Per altre informazioni, vedere [Usare valori di soglia avvisi e avvisi sulle metriche delle prestazioni di mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server.md).  
  
-   Monitoraggio mirroring del database  
  
     Monitoraggio mirroring del database è uno strumento con interfaccia utente grafica che consente agli amministratori di sistema di visualizzare e aggiornare lo stato e di configurare le soglie di avviso per diverse misurazioni chiave delle prestazioni. Questo strumento può essere anche usato dai membri del ruolo predefinito del database **dbm_monitor** per visualizzare la riga più recente della tabella dello stato di mirroring, benché non possano aggiornare la tabella stessa.  
  
     Questo strumento visualizza lo stato di un database selezionato nella pagina a schede **Stato** , inclusa la metrica relativa alle prestazioni. Il contenuto di questa pagina deriva dalle istanze del server principale e del server mirror. La pagina viene compilata in modo asincrono man mano che lo stato viene raccolto attraverso connessioni separate alle istanze del server principale e del server mirror. Lo strumento tenta di aggiornare la tabella dello stato a intervalli di 30 secondi. L'aggiornamento ha esito positivo solo se la tabella non è stata aggiornata entro 15 secondi e l'utente è un membro del ruolo predefinito del server **sysadmin** . Per un riepilogo delle informazioni contenute nella pagina **Stato** , vedere [Stato visualizzato da Monitoraggio mirroring del database](#perf_metrics_of_dbm_monitor), di seguito in questo argomento.  
  
     Per un'introduzione all'interfaccia di Monitoraggio mirroring del database, vedere [Database Mirroring Monitor Overview](../../database-engine/database-mirroring/database-mirroring-monitor-overview.md). Per informazioni sull'avvio del Monitoraggio mirroring del database, vedere [Avviare Monitoraggio mirroring del database &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md).  
  
-   Stored procedure di sistema  
  
     È anche possibile recuperare o aggiornare lo stato corrente eseguendo la stored procedure di sistema **sp_dbmmonitorresults** . Altre stored procedure dbmmonitor consentono di impostare il monitoraggio, modificare i parametri di monitoraggio, visualizzare il periodo di aggiornamento corrente, nonché rimuovere il monitoraggio nell'istanza del server.  
  
     Nella tabella seguente vengono illustrate le stored procedure per la gestione e l'utilizzo del monitoraggio del mirroring del database in modo indipendente da Monitoraggio mirroring del database.  
  
    |Procedura|Descrizione|  
    |---------------|-----------------|  
    |[sp_dbmmonitoraddmonitoring](../../relational-databases/system-stored-procedures/sp-dbmmonitoraddmonitoring-transact-sql.md)|Crea un processo che aggiorna periodicamente le informazioni relative allo stato per ogni database con mirroring nell'istanza del server.|  
    |[sp_dbmmonitorchangemonitoring](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangemonitoring-transact-sql.md)|Cambia il valore di un parametro del monitoraggio di mirroring del database.|  
    |[sp_dbmmonitorhelpmonitoring](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpmonitoring-transact-sql.md)|Restituisce il periodo di aggiornamento corrente.|  
    |[sp_dbmmonitorresults](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md)|Restituisce le righe relative allo stato per un database monitorato e consente di scegliere se la stored procedure ottiene l'ultimo stato preliminarmente.|  
    |[sp_dbmmonitordropmonitoring](../../relational-databases/system-stored-procedures/sp-dbmmonitordropmonitoring-transact-sql.md)|Arresta ed elimina il processo di monitoraggio di mirroring per tutti i database nell'istanza del server.|  
  
     Le stored procedure di sistema **dbmmonitor** possono essere utilizzate a complemento di Monitoraggio mirroring del database. Anche se il monitoraggio è stato configurato usando **sp_dbmmonitoraddmonitoring**, ad esempio, è possibile usare Monitoraggio mirroring del database per visualizzare lo stato.  
  
### <a name="how-monitoring-works"></a>Funzionamento del monitoraggio  
 Questa sezione contiene informazioni sulla tabella relativa allo stato del mirroring del database, sul processo di monitoraggio del mirroring del database, sullo strumento di monitoraggio, sulla modalità di monitoraggio dello stato del mirroring del database, nonché sulla modalità di eliminazione del processo di monitoraggio.  
  
#### <a name="database-mirroring-status-table"></a>Tabella dello stato di mirroring del database  
 Lo stato di mirroring del database viene archiviato in una tabella dello stato di mirroring del database interna non documentata contenuta nel database **msdb** . Questa tabella viene automaticamente creata al primo aggiornamento dello stato di mirroring nell'istanza del server.  
  
 La tabella dello stato può essere aggiornata in modo automatico o manuale da un amministratore di sistema, con un intervallo di aggiornamento minimo di 15 secondi. Il valore minimo di 15 secondi impedisce l'overload delle istanze del server con richieste di stato.  
  
 La tabella dello stato viene aggiornata automaticamente sia da Monitoraggio mirroring del database sia dal processo di Monitoraggio di mirroring del database, se in esecuzione. Il **Processo di Monitoraggio mirroring del database** aggiorna la tabella una volta ogni minuto per impostazione predefinita. Un amministratore di sistema può specificare un periodo di aggiornamento compreso tra 1 e 120 minuti. Monitoraggio mirroring del database, invece, aggiorna automaticamente la tabella ogni 30 secondi. Per questi aggiornamenti, il **Processo di Monitoraggio mirroring del database** e Monitoraggio mirroring del database chiamano **sp_dbmmonitorupdate**.  
  
 Alla prima esecuzione di **sp_dbmmonitorupdate** , vengono creati la tabella di **stato di mirroring del database** e il ruolo predefinito del database **dbm_monitor** nel database **msdb** . **sp_dbmmonitorupdate** di solito aggiorna lo stato del mirroring inserendo una nuova riga nella tabella di stato per ogni database con mirroring sull'istanza del server. Per altre informazioni, vedere "Tabella dello stato di mirroring del database" più avanti in questo argomento. Questa stored procedure restituisce inoltre le misurazioni delle prestazioni nelle nuove righe e tronca le righe antecedenti il periodo di memorizzazione corrente (il valore predefinito è 7 giorni). Per altre informazioni, vedere [sp_dbmmonitorupdate &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorupdate-transact-sql.md).  
  
> [!NOTE]  
>  A meno che Monitoraggio mirroring del database non sia attualmente in uso da parte di un membro del ruolo predefinito del server **sysadmin** , la tabella dello stato viene automaticamente aggiornata solo se il **Processo di Monitoraggio mirroring del database** esiste e se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent è in esecuzione.  
  
#### <a name="database-mirroring-monitor-job"></a>Monitoraggio mirroring del database  
 **Processo di Monitoraggio mirroring del database** funziona in modo indipendente da **Monitoraggio mirroring del database** e viene creato automaticamente solo se si utilizza [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] per avviare una sessione di mirroring. Se per avviare il mirroring vengono sempre usati i comandi ALTER DATABASE *nome_database* SET PARTNER, il processo esiste solo se l'amministratore di sistema esegue la stored procedure **sp_dbmmonitoraddmonitoring** .  
  
 Dopo la creazione di **Processo di monitoraggio mirroring del database** , supponendo che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent sia in esecuzione, il processo viene chiamato una volta al minuto, per impostazione predefinita. Il processo chiama quindi la stored procedure di sistema **sp_dbmmonitorupdate** .  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent chiama il **Processo di Monitoraggio mirroring del database** una volta al minuto, per impostazione predefinita, e il processo chiama **sp_dbmmonitorupdate** per aggiornare la tabella dello stato. Gli amministratori di sistema possono modificare il periodo di aggiornamento usando la stored procedure di sistema **sp_dbmmonitorchangemonitoring** e possono visualizzare il periodo di aggiornamento corrente usando la stored procedure di sistema **sp_dbmmonitorchangemonitoring** . Per altre informazioni, vedere [sp_dbmmonitoraddmonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitoraddmonitoring-transact-sql.md) e [sp_dbmmonitorchangemonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangemonitoring-transact-sql.md).  
  
#### <a name="monitoring-database-mirroring-status-by-system-administrators"></a>Monitoraggio dello stato di mirroring del database (amministratori di sistema)  
 I membri del ruolo predefinito del server **sysadmin** possono visualizzare e aggiornare la tabella dello stato  
  
-   Utilizzo di Monitoraggio mirroring del database  
  
     L'utilizzo di Monitoraggio mirroring del database consente a un amministratore di sistema di aggiornare manualmente la pagina **Stato** , l'albero di navigazione o la pagina **Cronologia** . Questa operazione consente inoltre di aggiornare la tabella dello stato, a meno che non sia già stata aggiornata entro i 15 secondi precedenti.  
  
     Per visualizzare la cronologia dello stato di mirroring in una determinata istanza del server, l'amministratore di sistema può fare clic sul pulsante **Cronologia** relativo a tale istanza del server nella pagina **Stato** . La cronologia viene visualizzata nella finestra di dialogo **Cronologia mirroring del database** . In questa finestra di dialogo l'amministratore di sistema può inoltre visualizzare alcune o tutte le righe della tabella dello stato dell'istanza del server.  
  
     Per informazioni sulle misurazioni della pagina **Stato** , vedere Misurazioni delle prestazioni visualizzate da Monitoraggio mirroring del database di seguito in questo argomento.  
  
-   Uso di **sp_dbmmonitorresults**  
  
     Gli amministratori di sistema possono usare la stored procedure di sistema **sp_dbmmonitorresults** per visualizzare e, facoltativamente, aggiornare la tabella dello stato, se non è stata aggiornata entro i 15 secondi precedenti. Questa stored procedure chiama la stored procedure **sp_dbmmonitorupdate** e restituisce una o più righe di cronologia, in base a quanto richiesto nella chiamata di procedura. Per informazioni sullo stato nel set di risultati, vedere [sp_dbmmonitorresults &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md).  
  
#### <a name="monitoring-database-mirroring-status-by-dbm_monitor-members"></a>Monitoraggio dello stato di mirroring del database (membri di dbm_monitor)  
 Come indicato in precedenza, alla prima esecuzione di **sp_dbmmonitorupdate** viene creato il ruolo predefinito del database **dbm_monitor** nel database **msdb** . I membri del ruolo predefinito del database **dbm_monitor** possono visualizzare lo stato di mirroring esistente usando Monitoraggio mirroring del database o la stored procedure **sp_dbmmonitorresults** . Questi utenti non possono tuttavia aggiornare la tabella dello stato. Per ottenere la durata dello stato visualizzato l'utente può consultare i tempi nelle etichette **Log principale (** _\<time>_ **)** e **Log mirror (** _\<time>_ **)** nella pagina **Stato**.  
  
 I membri del ruolo predefinito del database **dbm_monitor** dipendono dal **Processo di Monitoraggio mirroring del database** per l'aggiornamento della tabella dello stato a intervalli regolari. Se il processo non esiste o se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent è stato arrestato, lo stato non è più aggiornato e potrebbe non riflettere più la configurazione della sessione di mirroring. Dopo un failover, ad esempio, può sembrare che i partner condividano lo stesso ruolo, principale o mirror, oppure il server principale corrente può essere indicato come mirror e, viceversa, il server mirror corrente come principale.  
  
#### <a name="dropping-the-database-mirroring-monitor-job"></a>Eliminazione di Processo di Monitoraggio mirroring del database  
 **Processo di Monitoraggio mirroring del database** rimane presente finché non viene eliminato. Il processo di monitoraggio deve essere gestito dall'amministratore di sistema. Per eliminare il **Processo di Monitoraggio mirroring del database**, usare **sp_dbmmonitordropmonitoring**. Per altre informazioni, vedere [sp_dbmmonitordropmonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitordropmonitoring-transact-sql.md).  
  
###  <a name="status-displayed-by-the-database-mirroring-monitor"></a><a name="perf_metrics_of_dbm_monitor"></a> Stato visualizzato da Monitoraggio mirroring del database  
 Nella pagina **Stato** di Monitoraggio mirroring del database vengono descritti i partner e lo stato della sessione di mirroring. Lo stato include la metrica relativa alle prestazioni, ad esempio lo stato del log delle transazioni, e altre informazioni utili per consentire la valutazione effettiva del tempo necessario per completare un failover, nonché della potenziale perdita di dati, se la sessione non è sincronizzata. In questa pagina, inoltre, vengono visualizzati lo **stato** e informazioni generali relative alla sessione di mirroring.  
  
> [!NOTE]  
>  Per un'introduzione a Monitoraggio mirroring del database e alla pagina **Stato** , vedere [Strumenti per il monitoraggio dello stato del mirroring del database](#tools_for_monitoring_dbm_status)più indietro in questo argomento.  
  
 Le informazioni disponibili per ogni strumento vengono riepilogate nelle sezioni seguenti.  
  
#### <a name="partners"></a>Partner  
 Nella pagina **Stato** vengono visualizzate le informazioni seguenti per ogni partner:  
  
-   Istanza del server  
  
     Nome dell'istanza del server il cui stato è visualizzato nella riga **Stato** .  
  
-   Ruolo corrente  
  
     Ruolo corrente dell'istanza del server. Gli stati possibili sono elencati di seguito:  
  
    -   Server principale  
  
    -   Mirror  
  
-   Stato mirroring  
  
     Gli stati possibili sono elencati di seguito:  
  
    -   Unknown  
  
    -   Sincronizzazione in corso  
  
    -   Sincronizzato  
  
    -   Suspended  
  
    -   Disconnesso  
  
-   Connessione server di controllo del mirroring del database  
  
     Stato di connessione del server di controllo del mirroring del database. Gli stati possibili sono elencati di seguito:  
  
    -   Unknown  
  
    -   Connesso  
  
    -   Disconnesso  
  
#### <a name="log-on-the-principal-server"></a>Log del server principale  
 La pagina **Stato** consente di visualizzare le informazioni seguenti relative allo stato del log nel server principale al momento indicato:  
  
-   Log non inviato  
  
     Quantità di log in attesa nella coda di invio espressa in kilobyte (KB).  
  
-   Transazione non inviata meno recente  
  
     Tempo di memorizzazione della transazione non inviata meno recente nella coda di invio. Il tempo di memorizzazione di questa transazione indica la quantità di transazioni, espressa in minuti, non ancora inviata all'istanza del server mirror. Questo valore consente di misurare la potenziale perdita di dati in termini di tempo.  
  
-   Tempo stimato per l'invio del log  
  
     Tempo stimato, espresso in minuti, richiesto dall'istanza del server principale per inviare il log attualmente presente nella coda di invio all'istanza del server mirror in base alla velocità di invio corrente. Il tempo effettivo richiesto per l'invio del log è soggetto alla velocità delle transazioni in entrata, che può subire notevoli variazioni. Il valore **Tempo stimato per l'invio del log** , tuttavia, può essere utile per valutare approssimativamente il tempo richiesto per un failover manuale.  
  
-   Velocità di invio corrente  
  
     Velocità alla quale le transazioni vengono inviate all'istanza del server mirror, espressa in KB al secondo.  
  
-   Frequenza corrente nuove transazioni  
  
     Frequenza alla quale le transazioni in entrata vengono immesse nel log del server principale, espressa in KB al secondo. Per stabilire se il mirroring è in ritardo, procede secondo le previsioni o sta recuperando, confrontare questo valore con il valore **Tempo stimato per l'invio del log** .  
  
#### <a name="log-on-the-mirror-server"></a>Log del server mirror  
 La pagina **Stato** consente di visualizzare le informazioni seguenti relative allo stato del log nel server mirror al momento indicato:  
  
-   Log non ripristinato  
  
     Quantità di log in attesa nella coda di rollforward espressa in kilobyte (KB).  
  
-   Tempo stimato per il ripristino del log  
  
     Numero approssimativo di minuti necessari per l'applicazione del log presente nella coda di rollforward al database mirror.  
  
-   Velocità di ripristino corrente  
  
     Frequenza alla quale le transazioni vengono ripristinate nel database mirror, espressa in KB al secondo.  
  
#### <a name="mirroring-session"></a>Sessione di mirroring  
 Nella pagina **Stato** , inoltre, vengono visualizzate le informazioni seguenti relative alla sessione di mirroring:  
  
-   Overhead commit mirror  
  
     Ritardo medio per transazione, espresso in millisecondi, rilevanti solo nella modalità a sicurezza elevata. Questo ritardo rappresenta la quantità di overhead generato mentre l'istanza del server principale è in attesa che l'istanza del server mirror scriva il record di log della transazione nella coda di rollforward.  
  
-   Tempo stimato per l'invio e il ripristino dell'intero log corrente  
  
     Tempo stimato necessario per inviare l'intero log non inviato di cui è stato eseguito il commit al server principale e di ripristinare l'intero log attualmente presente nella coda di rollforward. Questo valore può essere inferiore alla somma dei valori dei campi **Tempo stimato per l'invio del log** e **Tempo stimato per il ripristino del log** , perché l'invio e il ripristino possono avvenire in parallelo.  
  
-   Indirizzo server di controllo del mirroring  
  
     Indirizzo di rete dell'istanza del server di controllo del mirroring. Per informazioni sul formato di questo indirizzo, vedere [Specificare un indirizzo di rete del server &#40;Mirroring del database&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).  
  
-   Modalità operativa  
  
     La modalità operativa della sessione di mirroring del database:  
  
    -   Prestazioni elevate (asincrona)  
  
    -   Protezione elevata senza failover automatico (sincrona)  
  
    -   Protezione elevata con failover automatico (sincrona)  
  
##  <a name="additional-sources-of-information-about-a-mirrored-database"></a><a name="AdditionalSources"></a> Fonti di informazioni aggiuntive su un database con mirroring  
 Oltre all'utilizzo di Monitoraggio mirroring del database e delle stored procedure dbmmonitor per monitorare un database con mirroring e impostare avvisi sulle variabili delle prestazioni monitorate, in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] sono disponibili viste del catalogo, contatori delle prestazioni e notifiche di eventi per il mirroring del database.  
  
 **Contenuto della sezione**  
  
-   [Metadati di mirroring del database](#DbmMetadata)  
  
-   [Contatori delle prestazioni di mirroring del database](#DbmPerfCounters)  
  
-   [Notifiche degli eventi di mirroring del database](#DbmEventNotif)  
  
###  <a name="database-mirroring-metadata"></a><a name="DbmMetadata"></a> Metadati di mirroring del database  
 Ogni sessione di mirroring del database viene descritta nei metadati esposti tramite le viste a gestione dinamica o del catalogo seguenti:  
  
-   **sys.database_mirroring**  
  
     In questa vista vengono visualizzati i metadati di mirroring del database per ogni database con mirroring in un'istanza del server. Per altre informazioni, vedere [sys.database_mirroring &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md).  
  
-   **sys.database_mirroring_endpoints**  
  
     La vista del catalogo **sys.database_mirroring_endpoints** mostra informazioni relative all'endpoint del mirroring del database dell'istanza del server. Per altre informazioni, vedere [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md).  
  
-   **sys.database_mirroring_witnesses**  
  
     In questa vista vengono visualizzati i metadati di mirroring del database per ogni sessione in cui un'istanza del server è il server di controllo del mirroring. Per altre informazioni, vedere [sys.database_mirroring_witnesses &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md).  
  
-   **sys.dm_db_mirroring_connections**  
  
     Questa vista a gestione dinamica restituisce una riga per ogni connessione di rete di mirroring del database.  
  
     Per altre informazioni, vedere [sys.dm_db_mirroring_connections &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/database-mirroring-sys-dm-db-mirroring-connections.md).  
  
###  <a name="database-mirroring-performance-counters"></a><a name="DbmPerfCounters"></a> Contatori delle prestazioni di mirroring del database  
 I contatori delle prestazioni consentono di monitorare le prestazioni di mirroring del database. Ad esempio, è possibile esaminare il contatore **Ritardo transazioni** per verificare se il mirroring del database sta influenzando le prestazioni sul server principale. È possibile esaminare i contatori **Coda rollforward** e **Coda invii log** per verificare se il database mirror è in grado di mantenersi aggiornato rispetto al database principale. Il contatore **Byte log inviati/sec** consente di monitorare il numero di eventi di log inviati al secondo.  
  
 In Performance Monitor su ogni partner, i contatori delle prestazioni sono disponibili nell'oggetto prestazione del mirroring del database (**SQLServer:Database Mirroring**). Per altre informazioni, vedere [Oggetto Database Mirroring di SQL Server](../../relational-databases/performance-monitor/sql-server-database-mirroring-object.md).  
  
 **Per avviare Performance Monitor**  
  
-   [Avviare il Monitoraggio di sistema &#40;Windows&#41;](../../relational-databases/performance/start-system-monitor-windows.md)  
  
###  <a name="database-mirroring-event-notifications"></a><a name="DbmEventNotif"></a> Notifiche degli eventi di mirroring del database  
 Le notifiche degli eventi sono un tipo speciale di oggetto di database. Le notifiche degli eventi vengono eseguite in risposta a una serie di istruzioni DDL (Data Definition Language) Transact-SQL ed eventi di Traccia SQL e inviano informazioni su eventi di server e database a un servizio di [!INCLUDE[ssSB](../../includes/sssb-md.md)] .  
  
 Per il mirroring del database sono disponibili gli eventi seguenti:  
  
-   Classe di evento **Database Mirroring State Change**  
  
     Indica quando lo stato di mirroring di un database con mirroring cambia. Per altre informazioni, vedere [Database Mirroring State Change Event Class](../../relational-databases/event-classes/database-mirroring-state-change-event-class.md).  
  
-   Classe di evento **Audit Database Mirroring Login**  
  
     Segnala i messaggi di controllo correlati alla sicurezza di trasporto per il mirroring del database. Per altre informazioni, vedere [Audit Database Mirroring Login Event Class](../../relational-databases/event-classes/audit-database-mirroring-login-event-class.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Usare valori di soglia avvisi e avvisi sulle metriche delle prestazioni di mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server.md)  
  
-   [Avviare il monitoraggio mirroring del database &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
-   [Visualizzazione dello stato di un database con mirroring &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/view-the-state-of-a-mirrored-database-sql-server-management-studio.md)  
  
 **Stored procedure**  
  
-   [sp_dbmmonitoraddmonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitoraddmonitoring-transact-sql.md)  
  
-   [sp_dbmmonitorchangealert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangealert-transact-sql.md)  
  
-   [sp_dbmmonitorchangemonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangemonitoring-transact-sql.md)  
  
-   [sp_dbmmonitordropalert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitordropalert-transact-sql.md)  
  
-   [sp_dbmmonitordropmonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitordropmonitoring-transact-sql.md)  
  
-   [sp_dbmmonitorhelpalert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpalert-transact-sql.md)  
  
-   [sp_dbmmonitorhelpmonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpmonitoring-transact-sql.md)  
  
-   [sp_dbmmonitorresults &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md)  
  
-   [sp_dbmmonitorupdate &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorupdate-transact-sql.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Concetti relativi al provider WMI per eventi del server](../../relational-databases/wmi-provider-server-events/wmi-provider-for-server-events-concepts.md)  
  
  
