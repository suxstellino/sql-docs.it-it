---
title: Uso del dashboard del gruppo di disponibilità in SSMS
description: Descrive come usare il dashboard del gruppo di disponibilità Always On per monitorare l'integrità di un gruppo di disponibilità in SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 08/09/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.agdashboard.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
- Availability Groups [SQL Server], dashboard
ms.assetid: c9ba2589-139e-42bc-99e1-94546717c64d
author: cawrites
ms.author: chadam
ms.openlocfilehash: 044b156f6f548150769fa9d172f652d584ff5a9a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355156"
---
# <a name="use-the-always-on-availability-group-dashboard-sql-server-management-studio"></a>Usare il dashboard del gruppo di disponibilità Always On in SQL Server Management Studio
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Gli amministratori del database usano il dashboard del gruppo di disponibilità Always On per visualizzare immediatamente l'integrità di un gruppo di disponibilità e delle relative repliche di disponibilità e database in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]. Alcuni degli utilizzi tipici del dashboard del gruppo di disponibilità sono i seguenti:  
  
-   Scelta di una replica per un failover manuale.    
-   Stima della perdita di dati in caso di failover forzato.  
-   Valutazione delle prestazioni della sincronizzazione dei dati.   
-   Valutazione dell'impatto sulle prestazioni di una replica secondaria con commit sincrono 
  -  Il dashboard fornisce indicatori delle prestazioni e stati dei gruppi di disponibilità principali che facilitano le decisioni operative relative alla disponibilità elevata usando i tipi di informazioni seguenti.  
-   Stato di rollup della replica    
-   Modalità e stato di sincronizzazione   
-   Perdita di dati stimata    
-   Tempo di recupero stimato (ripetere aggiornamento)    
-   Dettagli della replica di database    
-   Modalità e stato di sincronizzazione    
-   Tempo per il ripristino del log  
  
  
## <a name="prerequisites"></a>Prerequisites  
 È necessario essere connessi all'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (istanza del server) che ospita la replica primaria o una replica secondaria di un gruppo di disponibilità.  
  
 
### <a name="permissions"></a>Autorizzazioni  
 È necessario disporre delle autorizzazioni CONNECT, VIEW SERVER STATE e VIEW ANY DEFINITION.  
  
##  <a name="to-start-the-always-on-dashboard"></a>Per avviare il dashboard Always On  
  
1.  In Esplora oggetti connettersi all'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in cui si vuole eseguire il dashboard Always On.  
  
2.  Espandere il nodo **Disponibilità elevata Always On**, fare clic con il pulsante destro del mouse sul nodo **Gruppi di disponibilità** **Mostra dashboard**.  
  
##  <a name="change-always-on-dashboard-options"></a>Modificare le opzioni del dashboard Always On  
 È possibile usare la finestra di dialogo [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]**Opzioni** per configurare il comportamento del dashboard Always On di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
1.  Dal menu **Strumenti** scegliere **Opzioni**.  
  
2.  Per aggiornare automaticamente il dashboard, nella finestra di dialogo **Opzioni** selezionare **Abilita aggiornamento automatico**, immettere l'intervallo di aggiornamento in secondi, quindi immettere per quante volte si desidera tentare di stabilire la connessione.  
  
3.  Per abilitare criteri definiti dall'utente, selezionare **Abilita criteri Always On definiti dall'utente**.  
  
##  <a name="availability-group-summary"></a>Riepilogo del gruppo di disponibilità  
 Nella schermata del gruppo di disponibilità viene visualizzata una riga di riepilogo per ogni gruppo di disponibilità per il quale l'istanza del server connesso ospita una replica. In questo riquadro sono visualizzate le colonne seguenti:  
  
 **Nome gruppo di disponibilità**  
 Nome di un gruppo di disponibilità per il quale l'istanza del server connesso ospita una replica.  
  
 **Istanza primaria**  
 Nome dell'istanza del server che ospita la replica primaria del gruppo di disponibilità.  
  
 **Modalità di failover**  
 Visualizza la modalità di failover per cui è configurata la replica. I possibili valori per le modalità di failover sono:  
  
-   **Automatico** Indica che una o più repliche sono in modalità di failover automatico.  
  
-   **Manuale**. Indica che non vi sono repliche in modalità di failover automatico.  
  
 **Problemi**  
 Fare clic sul collegamento **Problemi** per aprire la documentazione di risoluzione dei problemi relativa a un determinato problema. Per un elenco dei problemi relativi ai criteri di Always On, vedere [Criteri Always On per problemi operativi con gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-policies-for-operational-issues-always-on-availability.md).  
  
> [!TIP]  
>  Fare clic sulle intestazioni di colonna per ordinare le informazioni sul gruppo di disponibilità in base al nome del gruppo di disponibilità, all'istanza primaria, alla modalità di failover o al problema.  
  
##  <a name="availability-group-details"></a><a name="AvGroupDetails"></a> Dettagli del gruppo di disponibilità  
 Le informazioni dettagliate riportate di seguito vengono visualizzate per il gruppo di disponibilità selezionato nello schermata di riepilogo:  
  
 **Stato gruppo di disponibilità**  
 Visualizza lo stato di integrità del gruppo di disponibilità.  
  
 **Primary instance**  
 Nome dell'istanza del server che ospita la replica primaria del gruppo di disponibilità.  
  
 **Failover mode**  
 Visualizza la modalità di failover per cui è configurata la replica. I possibili valori per le modalità di failover sono:  
  
-   **Automatico** Indica che una o più repliche sono in modalità di failover automatico.  
  
-   **Manuale**. Indica che non vi sono repliche in modalità di failover automatico.  
  
 **Stato cluster**  
 Nome e stato del cluster in cui l'istanza del server connesso e del gruppo di disponibilità è un nodo membro.  
  
##  <a name="availability-replica-details"></a><a name="AvReplicaDetails"></a> Dettagli replica di disponibilità  

Quando si è connessi alla replica principale, **Dettagli replica di disponibilità** visualizza le informazioni da tutte le repliche nel gruppo di disponibilità. Quando si è connessi a una replica secondaria, vengono visualizzate solo le informazioni della replica connessa.  

Nel riquadro **Replica di disponibilità** vengono visualizzate le colonne seguenti:  
  
 **Nome**  
 Nome dell'istanza del server che ospita la replica di disponibilità. Questa colonna viene visualizzata per impostazione predefinita.  
  
 **Ruolo**  
 Indica il ruolo corrente della replica di disponibilità, ovvero **Primario** o **Secondario**. Per informazioni sui ruoli di [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], vedere [Panoramica di Gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md). Questa colonna viene visualizzata per impostazione predefinita.  
  
 **Modalità di failover**  
 Visualizza la modalità di failover per cui è configurata la replica. I possibili valori per le modalità di failover sono:  
  
-   **Automatico** Indica che una o più repliche sono in modalità di failover automatico.  
  
-   **Manuale**. Indica che non vi sono repliche in modalità di failover automatico.  
  
 **Stato di sincronizzazione**  
 Indica se una replica secondaria è attualmente sincronizzata con la replica primaria. Questa colonna viene visualizzata per impostazione predefinita. I valori possibili sono:  
  
-   **Non sincronizzato**. Uno o più database nella replica non sono sincronizzati o non ne è ancora stato creato un join al gruppo di disponibilità.  
  
-   **Sincronizzazione in corso**. Uno o più database nella replica vengono sincronizzati.  
  
-   **Sincronizzato**. Tutti i database nella replica secondaria sono sincronizzati con i database primari corrispondenti nella replica primaria corrente o nell'ultima replica primaria.  
  
    > [!NOTE]  
    >  Nella modalità prestazioni, il database non si trova mai nello stato sincronizzato.  
  
-   **NULL**. Stato sconosciuto. Si ottiene questo valore quando l'istanza del server locale non è in grado di comunicare con il cluster di failover WSFC, ovvero il nodo locale non fa parte del quorum WSFC.  
  
 **Problemi**  
 Indica il nome del problema. Questo valore è visualizzato per impostazione predefinita. Per un elenco dei problemi relativi ai criteri di Always On, vedere [Criteri Always On per problemi operativi con gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-policies-for-operational-issues-always-on-availability.md).  
  
 **Modalità di disponibilità**  
 Indica la proprietà della replica impostata separatamente per ogni replica di disponibilità. Questo valore è nascosto per impostazione predefinita. I valori possibili sono:  
  
-   **Asincrona**. La replica secondaria non è mai sincronizzata con la replica primaria.  
  
-   **Synchronous**. Quando si esegue l'aggiornamento al database primario, un database secondario entra in questo stato e rimane aggiornato finché continua la sincronizzazione dati per il database.  
  
 **Modalità connessione primaria**  
 Indica la modalità utilizzata per connettersi alla replica primaria.  Questo valore è nascosto per impostazione predefinita.  
  
 **Modalità connessione secondaria**  
 Indica la modalità utilizzata per connettersi alla replica secondaria.  Questo valore è nascosto per impostazione predefinita.  
  
 **Stato connessione**  
 Indica se una replica secondaria è attualmente connessa alla replica primaria. Questa colonna è nascosta per impostazione predefinita. I valori possibili sono:  
  
-   **Disconnesso**. Per una replica di disponibilità remota, indica che è disconnessa dalla replica di disponibilità locale. La risposta della replica locale allo stato Disconnesso dipende dal relativo ruolo:  
  
    -   Sulla replica primaria, se una replica secondaria è disconnessa, i database secondari sono contrassegnati come **Non sincronizzato** sulla replica primaria e la replica primaria attende che la replica secondaria venga riconnessa.  
  
    -   Sulla replica secondaria, dopo avere rilevato che è disconnessa, tenta di riconnettersi alla replica primaria.  
  
-   **Connected**: Una replica di disponibilità remota attualmente connessa alla replica locale.  
  
 **Stato operativo**  
 Indica lo stato operativo corrente della replica secondaria. Questo valore è nascosto per impostazione predefinita. I valori possibili sono:  
  
 **0**. Failover in sospeso    
 **1**. In sospeso    
 **2**. Online    
 **3**. Offline   
 **4**. Non riuscito    
 **5**. Errore, nessun quorum  
  
 **NULL**. La replica non è locale.  
  
 **Ultimo errore di connessione.**  
 Numero dell'ultimo errore di connessione.  Questo valore è nascosto per impostazione predefinita.  
  
 **Descrizione ultimo errore di connessione**  
 Descrizione dell'ultimo errore di connessione.  Questo valore è nascosto per impostazione predefinita.  
  
 **Ora dell'ultimo errore di connessione**  
 Timestamp dell'ultimo errore di connessione. Questo valore è nascosto per impostazione predefinita.  
  
> [!NOTE]  
>  Per informazioni sui contatori delle prestazioni per le repliche di disponibilità, vedere [SQL Server, replica di disponibilità](../../../relational-databases/performance-monitor/sql-server-availability-replica.md).  
  
##  <a name="group-by-availability-group-information"></a>Raggruppare le informazioni in base a un gruppo di disponibilità  
 Per raggruppare le informazioni, fare clic su **Raggruppa per**, quindi selezionare una delle opzioni seguenti:  
  
-   **Repliche di disponibilità**    
-   **Database di disponibilità** 
-   **Synchronization state**  
-   **Conformità del failover**   
-   **Problemi**  
  
 Nel riquadro in cui sono visualizzate le informazioni raggruppate sono presenti le colonne seguenti:  
  
 **Nome**  
 Nome del database di disponibilità. Questo valore è visualizzato per impostazione predefinita.  
  
 **Replica**  
 Nome dell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] che ospita la replica di disponibilità. Questo valore è visualizzato per impostazione predefinita.  
  
 **Stato di sincronizzazione**  
 Indica se il database di disponibilità è attualmente sincronizzato con la replica primaria. Questo valore è visualizzato per impostazione predefinita. I possibili stati di sincronizzazione sono i seguenti:  
  
-   **Non in sincronizzazione**:  
-   
    -   Per il ruolo primario, indica che il database non è pronto per la sincronizzazione del log delle transazioni con i database secondari corrispondenti.   
    -   Per un database secondario, indica che il database non ha ancora avviato la sincronizzazione del log a causa di un problema di connessione, è stato sospeso o si trova in stati di transizione durante l'avvio o un cambio di ruolo.  
  
-   **Sincronizzazione in corso**:
- Su una replica primaria:   
    - In un database primario, indica che questo database è pronto ad accettare una richiesta di analisi da un database secondario.  
    - Su una replica secondaria, indica che è in corso uno spostamento dati attivo per quel database secondario. 
  
  
-   **Sincronizzato**: 
  
    - Per un database primario, indica che almeno un database secondario è sincronizzato.
    - Per un database secondario, indica che il database è sincronizzato con il database primario corrispondente.  
  
-   **Ripristino in corso**.  
  
     Indica la fase del processo di rollback in cui un database secondario ottiene attivamente le pagine dal database primario.  
  
    > [!CAUTION]  
    >  Quando un database si trova nello stato REVERTING, il failover forzato sulla replica secondaria può lasciare il database in uno stato in cui non può essere avviato.  
  
-   **Inizializzazione**.  
  
     Indica la fase di rollback in cui il log delle transazioni necessario a un database secondario per l'intercettazione dell'LSN di rollback viene fornito e finalizzato su una replica secondaria.  
  
    > [!CAUTION]  
    >  Quando un database si trova nello stato INITIALIZING, il failover forzato sulla replica secondaria lascerà sempre il database in uno stato in cui non può essere avviato.  
  
 **Conformità failover**  
 Indica per quale replica di disponibilità è possibile eseguire il failover con o senza la potenziale perdita di dati. Questa colonna viene visualizzata per impostazione predefinita. I valori possibili sono:  
  
-   **Perdita di dati**   
-   **Nessuna perdita di dati**  
  
 **Problemi**  
 Indica il nome del problema. Questa colonna viene visualizzata per impostazione predefinita. I valori possibili sono:  
  
-   **Avvisi**. Fare clic per visualizzare le soglie e gli avvisi.   
-   **Critico**. Fare clic per visualizzare i problemi critici.  
  
 Per un elenco dei problemi relativi ai criteri di Always On, vedere [Criteri Always On per problemi operativi con gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-policies-for-operational-issues-always-on-availability.md).  
  
 **Sospeso**  
 Indica se il database è **Sospeso** o **Ripreso**. Questo valore è nascosto per impostazione predefinita.  
  
 **Motivo di sospensione**  
 Indica il motivo dello stato di sospensione. Questo valore è nascosto per impostazione predefinita.  
  
 **Perdita di dati stimata (secondi)**  
 Indica la differenza di orario dell'ultimo record del log delle transazioni nelle repliche primaria e secondaria. In caso di errore della replica primaria, andranno persi tutti i record del log delle transazioni entro il tempo specificato. Questo valore è nascosto per impostazione predefinita.  
  
 **Tempo di recupero stimato (secondi)**  
 Indica il tempo in secondi necessario per ripetere l'aggiornamento. Il *tempo di aggiornamento* è il tempo richiesto dalla replica secondaria per l'aggiornamento alla replica primaria. Questo valore è nascosto per impostazione predefinita.  
  
 **Prestazioni di sincronizzazione (secondi)**  
 Indica il tempo in secondi necessario per la sincronizzazione tra le repliche primaria e secondaria. Questo valore è nascosto per impostazione predefinita.  
  
 **Dimensione coda di invio log (KB)**  
 Indica il numero di record del log nei file di log del database primario che non sono stati inviati alla replica secondaria. Questo valore è nascosto per impostazione predefinita.  
  
 **Velocità di invio log (KB/sec)**  
 Indica la velocità in KB al secondo alla quale i record di log vengono inviati alla replica secondaria. Questo valore è nascosto per impostazione predefinita.  
  
 **Dimensione coda di rollforward (KB)**  
 Indica il numero di record di log nei file di log della replica secondaria di cui non è stato ancora eseguito il rollforward. Questo valore è nascosto per impostazione predefinita.  
  
 **Velocità fase di rollforward (KB/sec)**  
 Indica la velocità in KB al secondo alla quale viene eseguito il rollforward dei record di log. Questo valore è nascosto per impostazione predefinita.  
  
 **Velocità di invio flusso di file (KB/sec)**  
 Indica la velocità del flusso di file in KB al secondo con cui avviene l'invio delle transazioni alla replica. Questo valore è nascosto per impostazione predefinita.  
  
 **LSN di fine log**  
 Indica il numero di sequenza del file di log (LSN) effettivo corrispondente all'ultimo record di log nella cache log nelle repliche primarie e secondarie. Questo valore è nascosto per impostazione predefinita.  
  
 **LSN recupero**  
 Indica la fine del log delle transazioni prima che la replica scriva qualsiasi nuovo record del log dopo il failover o il recupero nella replica primaria. Questo valore è nascosto per impostazione predefinita.  
  
 **LSN troncamento**  
 Indica il valore minimo di troncamento del log per la replica primaria. Questo valore è nascosto per impostazione predefinita.  
  
 **LSN ultimo commit**  
 Indica l'LSN effettivo corrispondente all'ultimo record di commit nel log delle transazioni. Questo valore è nascosto per impostazione predefinita.  
  
 **Ora ultimo commit**  
 Indica l'ora che corrisponde all'ultimo record di commit. Questo valore è nascosto per impostazione predefinita.  
  
 **LSN ultimo invio**  
 Indica il punto fino a cui tutti i blocchi di log sono stati inviati dalla replica primaria. Questo valore è nascosto per impostazione predefinita.  
  
 **Ora ultimo invio**  
 Indica l'ora di invio dell'ultimo blocco del log. Questo valore è nascosto per impostazione predefinita.  
  
 **LSN ultima ricezione**  
 Indica il punto fino a cui tutti i blocchi di log sono stati ricevuti dalla replica secondaria che ospita il database secondario. Questo valore è nascosto per impostazione predefinita.  
  
 **Ora ultima ricezione**  
 Indica l'ora in cui è stato letto l'identificatore del blocco di log nell'ultimo messaggio ricevuto sulla replica secondaria. Questo valore è nascosto per impostazione predefinita.  
  
 **LSN ultima finalizzazione**  
 Indica il punto fino a cui tutti i record di log sono stati scaricati su disco sulla replica secondaria. Questo valore è nascosto per impostazione predefinita.  
  
 **Ora ultima finalizzazione**  
 Indica l'ora di ricezione dell'identificatore del blocco di log per il valore LSN finale sulla replica secondaria. Questo valore è nascosto per impostazione predefinita.  
  
 **LSN ultimo rollforward**  
 Indica l'LSN effettivo del record di log di cui è stato eseguito il rollforward per ultimo nella replica secondaria. Questo valore è nascosto per impostazione predefinita.  
  
 **Ora ultimo rollforward**  
 Indica l'ora di rollforward dell'ultimo record del log nel database secondario. Questo valore è nascosto per impostazione predefinita.  
 

   > [!NOTE]  
   >  Poiché la maggior parte dei dati si basa su sys.dm_hadr_database_replica_states, potrebbero essere applicate alcune restrizioni. Per altre informazioni, vedere [sys.dm_hadr_database_replica_states (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md).


## <a name="always-on-availability-group-latency-reports"></a>Report di latenza del gruppo di disponibilità Always On
Il report di latenza del gruppo di disponibilità è un strumento di reporting incorporato nel dashboard del gruppo di disponibilità e disponibile in [SQL Server Management Studio 17.4](../../../ssms/download-sql-server-management-studio-ssms.md). Questa funzionalità comprende un report di facile comprensione che riporta in dettaglio il tempo dedicato alle diverse fasi del processo di trasporto del log. Ciò rappresenta un modo per limitare le cause potenziali della latenza durante il processo di sincronizzazione. 

SQL Agent esegue la raccolta dei dati e deve essere abilitato nella replica primaria e in almeno una delle repliche secondarie. Visualizzare il report facendo clic con il pulsante destro del mouse sul gruppo di disponibilità > Report > Report standard in **Esplora oggetti** di SQL Server Management Studio.  

Per altre informazioni, vedere [Report di latenza del gruppo di disponibilità Always On](/archive/blogs/sql_server_team/new-in-ssms-always-on-availability-group-latency-reports).

## <a name="related-tasks"></a>Attività correlate  
  
-   [Utilizzare i criteri AlwaysOn per visualizzare l'integrità di un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/use-always-on-policies-to-view-the-health-of-an-availability-group-sql-server.md)  
  
## <a name="see-also"></a>Vedere anche  
 [sys.dm_os_performance_counters &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)   
 [Monitoraggio di Gruppi di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)  
  
