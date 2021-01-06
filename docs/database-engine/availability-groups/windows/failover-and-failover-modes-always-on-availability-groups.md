---
title: Modalità di failover per i gruppi di disponibilità
description: Descrizione delle diverse modalità di failover disponibili per i database che fanno parte di un gruppo di disponibilità Always On.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], failover
- Availability Groups [SQL Server], failover modes
- failover [SQL Server], AlwaysOn Availability Groups
ms.assetid: 378d2d63-50b9-420b-bafb-d375543fda17
author: cawrites
ms.author: chadam
ms.openlocfilehash: f6ea40fdde107aedf5a76f7419dfd096e167bd53
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644150"
---
# <a name="failover-and-failover-modes-always-on-availability-groups"></a>Failover e modalità di failover (gruppi di disponibilità AlwaysOn)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Nel contesto di un gruppo di disponibilità, il ruolo primario e il ruolo secondario delle repliche di disponibilità sono generalmente intercambiabili in un processo noto come *failover*. Sono disponibili tre tipi di failover: failover automatico (senza perdita di dati), failover manuale pianificato (senza perdita di dati) e failover manuale forzato (con possibile perdita di dati), in genere chiamato *failover forzato*. Con il failover automatico e il failover manuale pianificato vengono conservati tutti i dati. Per un gruppo di disponibilità il failover si verifica a livello di replica di disponibilità. In pratica, il failover di un gruppo di disponibilità viene eseguito in una delle relative repliche secondarie, la *destinazione di failover* corrente.  
  
> [!NOTE]  
>   Se non è configurato [Rilevamento dell'integrità a livello di database](../../../database-engine/availability-groups/windows/sql-server-always-on-database-health-detection-failover-option.md), gli errori a livello di database, ad esempio un database ritenuto sospetto in seguito alla perdita di un file di dati, all'eliminazione di un database o al danneggiamento di un log delle transazioni, non causano il failover di un gruppo di disponibilità.  
  
 Durante il failover, la destinazione del failover subentra al ruolo primario, recupera i database e li porta online come nuovi database primari. La replica primaria precedente, se disponibile, assume il ruolo secondario e i relativi database diventano database secondari. Potenzialmente, questi ruoli possono essere scambiati vicendevolmente o destinati a una destinazione del failover differente, in seguito a più errori o per scopi amministrativi.  
  
 Le forme di failover supportate da una replica di disponibilità sono specificate dalla proprietà della *modalità di failover* . Per una replica di disponibilità specificata le modalità di failover possibili dipendono dalla [modalità di disponibilità](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md) della replica, come segue:  
  
-   Le **repliche con commit sincrono** supportano due impostazioni: automatica e manuale. L'impostazione automatica supporta sia il failover automatico sia quello manuale. Per impedire la perdita di dati, il failover automatico e il failover pianificato richiedono che la destinazione del failover sia una replica secondaria con commit sincrono con uno stato di sincronizzazione integro ad indicare che ogni database secondario sulla destinazione del failover è sincronizzato con il database primario corrispondente. Quando una replica secondaria non soddisfa entrambe queste condizioni, supporta solo il failover forzato. Si noti che il failover forzato è anche supportato da una replica il cui ruolo si trova nello stato RESOLVING.  
  
-   Le **repliche con commit asincrono** supportano solo la modalità di failover manuale. Inoltre, poiché non sono mai sincronizzate, supportano solo il failover forzato.  
  
> [!NOTE]  
>  Dopo un failover, le applicazioni client che devono accedere ai database primari devono connettersi alla nuova replica primaria. Inoltre, se la nuova replica secondaria è configurata per consentire l'accesso in sola lettura, le applicazioni client in sola lettura possono connettersi ad essa. Per informazioni sulla connessione dei client a un gruppo di disponibilità, vedere [Listener del gruppo di disponibilità, connettività client e failover dell'applicazione &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md).  
  
 **Sezioni dell'argomento:**  
  
-   [Termini e definizioni](#TermsAndDefinitions)  
  
-   [Panoramica del failover](#Overview)  
  
-   [Failover automatico](#AutomaticFailover)  
  
-   [Failover manuale pianificato (senza perdita di dati)](#ManualFailover)  
  
-   [Failover forzato (con possibile perdita di dati)](#ForcedFailover)  
  
-   [Attività correlate](#RelatedTasks)  
  
-   [Contenuto correlato](#RelatedContent)  
  
##  <a name="terms-and-definitions"></a><a name="TermsAndDefinitions"></a> Termini e definizioni  
 Failover automatico  
 Failover che si verifica automaticamente alla perdita della replica primaria. Il failover automatico è supportato solo quando la replica primaria corrente e una replica secondaria sono entrambe configurate con la modalità di failover impostata su AUTOMATIC e la replica secondaria è attualmente sincronizzata.  Se la modalità di failover della replica primaria o di quella secondaria è MANUAL, il failover automatico non è supportato.  
  
 Failover manuale pianificato (senza perdita di dati)  
 Il failover manuale pianificato o *failover manuale* è un failover avviato da un amministratore del database, in genere per scopi amministrativi. Un failover manuale pianificato è supportato solo se la replica primaria e la replica secondaria sono configurate per la modalità commit sincrono e sia la replica primaria che la replica secondaria sono attualmente sincronizzate (nello stato SYNCHRONIZED). Quando la replica secondaria di destinazione è sincronizzata, il failover manuale (senza perdita di dati) è possibile anche se la replica primaria è stata arrestata in modo anomalo perché i database secondari sono pronti per il failover. Un amministratore di database avvia manualmente un failover manuale.  
  
 Failover forzato (con possibile perdita di dati)  
 Failover che può essere avviato da un amministratore del database quando nessuna replica secondaria è sincronizzata (SYNCHRONIZED) con la replica primaria o la replica primaria non è in esecuzione e nessuna replica secondaria è pronta per il failover. Il failover forzato implica il rischio della perdita di dati ed è rigorosamente consigliato per il ripristino di emergenza. Il failover forzato è anche noto come failover manuale forzato, in quanto può essere avviato solo manualmente. Si tratta dell'unica forma di failover supportata nella modalità di disponibilità commit asincrono.  
  
 [!INCLUDE[ssFosAutoC](../../../includes/ssfosautoc-md.md)]  
 In un determinato gruppo di disponibilità, coppia di repliche di disponibilità, inclusa la replica primaria corrente, configurata per la modalità commit sincrono con failover automatico. Un [!INCLUDE[ssFosAuto](../../../includes/ssfosauto-md.md)] diventa effettivo solo se la replica secondaria si trova attualmente nello stato SINCRONIZZATO con la replica primaria.  
  
 [!INCLUDE[ssFosSyncC](../../../includes/ssfossyncc-md.md)]  
 In un determinato gruppo di disponibilità, set di due o tre repliche di disponibilità, inclusa la replica primaria corrente, configurato per la modalità commit sincrono, se presente. Un [!INCLUDE[ssFosSync](../../../includes/ssfossync-md.md)]diventa effettivo solo se le repliche secondarie sono configurate per la modalità di failover manuale e almeno una replica secondaria si trova attualmente nello stato SINCRONIZZATO con la replica primaria.  
  
 [!INCLUDE[ssFosEntireC](../../../includes/ssfosentirec-md.md)]  
 In un determinato gruppo di disponibilità, il set di tutte le repliche di disponibilità il cui stato operativo è attualmente ONLINE, indipendentemente dalle modalità di disponibilità e di failover. Un [!INCLUDE[ssFosEntire](../../../includes/ssfosentire-md.md)]diventa rilevante quando nessuna replica secondaria si trova attualmente nello stato SINCRONIZZATO con la replica primaria.  
  
##  <a name="overview-of-failover"></a><a name="Overview"></a> Panoramica del failover  
 Nella tabella seguente sono riportate le forme di failover supportate nelle diverse modalità di disponibilità e di failover. Per ogni associazione, la modalità di disponibilità e la modalità di failover effettive vengono determinate dall'intersezione delle modalità della replica primaria in aggiunta alle modalità di una o più repliche secondarie.  
  
|Modulo di failover|Modalità commit asincrono|Modalità commit sincrono con modalità di failover manuale|Modalità commit sincrono con modalità di failover automatico|  
|-------------------------------|---------------------------------------------------------|------------------------------------------------------------|  
|Failover automatico|No|No|Sì|  
|Failover manuale pianificato|No|Sì|sì|  
|failover forzato|Sì|Sì|Sì **&#42;**|  
  
 **&#42;** Se si esegue un comando di failover forzato su una replica secondaria sincronizzata, la replica secondaria si comporta come per un failover manuale.  
  
 Il periodo di tempo di non disponibilità del database durante un failover dipende dal tipo di failover e da ciò che lo ha causato.  
  
> [!IMPORTANT]  
>  Per supportare le connessioni client dopo il failover, a eccezione dei database indipendenti, è necessario ricreare manualmente sul nuovo database primario gli account di accesso e i processi definiti sui database primari precedenti. Per altre informazioni, vedere [Gestione di account di accesso e processi per i database di un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/logins-and-jobs-for-availability-group-databases.md).  
  
### <a name="failover-sets"></a>Set di failover  
 Le forme di failover possibili per un gruppo di disponibilità specificato possono essere comprese in termini di set di failover. Un set di failover è composto dalla replica primaria e dalle repliche secondarie che supportano una forma specificata di failover, come riportato di seguito.  
  
-   **[!INCLUDE[ssFosAutoC](../../../includes/ssfosautoc-md.md)] (facoltativo):**  In un determinato gruppo di disponibilità, coppia di repliche di disponibilità, inclusa la replica primaria corrente, configurata per la modalità commit sincrono con failover automatico. Un failover automatico impostato diventa effettivo solo se la replica secondaria si trova attualmente nello stato SYNCHRONIZED con la replica primaria.  
  
-   **[!INCLUDE[ssFosSyncC](../../../includes/ssfossyncc-md.md)] (facoltativo):**  In un determinato gruppo di disponibilità, set di due o tre repliche di disponibilità, inclusa la replica primaria corrente, configurato per la modalità commit sincrono, se presente. Un failover con commit sincrono impostato diventa effettivo solo se le repliche secondarie sono configurate per la modalità di failover manuale e almeno una replica secondaria si trova nello stato SYNCHRONIZED con la replica primaria.  
  
-   **[!INCLUDE[ssFosEntireC](../../../includes/ssfosentirec-md.md)] :**  In un determinato gruppo di disponibilità, il set di tutte le repliche di disponibilità il cui stato operativo è attualmente ONLINE, indipendentemente dalle modalità di disponibilità e di failover. L'intero set di failover diventa rilevante quando nessuna replica secondaria si trova attualmente nello stato SYNCHRONIZED con la replica primaria.  
  
 Quando si configura una replica di disponibilità come commit sincrono con failover automatico, la replica di disponibilità diventa parte del [!INCLUDE[ssFosAuto](../../../includes/ssfosauto-md.md)]. Il fatto che un set diventi effettivo o meno dipende tuttavia dal database primario corrente. Le forme di failover effettivamente possibili in un determinato momento dipendono dai set di failover attualmente effettivi.  
  
 Si consideri ad esempio un gruppo di disponibilità con quattro repliche di disponibilità:  
  
|Replica|Impostazioni delle modalità di disponibilità e di failover|  
|-------------|--------------------------------------------------|  
|Una|Commit sincrono con failover automatico|  
|b|Commit sincrono con failover automatico|  
|C|Commit sincrono solo con failover manuale pianificato|  
|D|Commit asincrono (solo con failover forzato)|  
  
 Il comportamento di failover di ogni replica secondaria dipende dalla replica di disponibilità che è attualmente la replica primaria. Fondamentalmente, per una determinata replica secondaria, il comportamento di failover è il caso peggiore data la replica primaria corrente. Nell'illustrazione seguente viene descritto come il comportamento del failover di repliche secondarie varia a seconda della replica primaria corrente e se è configurato per la modalità con commit asincrono (solo con failover forzato) o la modalità con commit sincrono (con o senza failover automatico).  
  
 ![Effetto del failover sulla configurazione della replica primaria](../../../database-engine/availability-groups/windows/media/aoag-failoversetexample.gif "Effetto del failover sulla configurazione della replica primaria")  
  
##  <a name="automatic-failover"></a><a name="AutomaticFailover"></a> Automatic Failover  
 In seguito a un failover automatico, una replica secondaria qualificata assume automaticamente il ruolo primario quando la replica primaria non è più disponibile. Il failover automatico è particolarmente appropriato quando il nodo WSFC che ospita la replica primaria è locale rispetto al nodo che ospita la replica secondaria. Ciò dipende dal fatto che la sincronizzazione dei dati funziona meglio con una latenza del messaggio bassa tra i computer e le connessioni client possono rimanere locali.  
  
 **Contenuto della sezione**  
  
-   [Condizioni necessarie per un failover automatico](#RequiredConditions)  
  
-   [Funzionamento del failover automatico](#HowAutoFoWorks)  
  
-   [Per abilitare il failover automatico](#EnableAutoFo)  
  
###  <a name="conditions-required-for-an-automatic-failover"></a><a name="RequiredConditions"></a> Condizioni necessarie per un failover automatico  
 Il failover automatico si verifica solo se sussistono le condizioni seguenti:  
  
-   Presenza di un set di failover automatico. Questo set è composto da una replica primaria e da una secondaria (la *destinazione del failover automatico*) entrambe configurate per la modalità con commit sincrono e impostate su AUTOMATIC per il failover. Se la replica primaria è impostata su MANUAL per il failover, il failover automatico non è supportato, anche se una replica secondaria è impostata su AUTOMATIC per il failover.  
  
     Per altre informazioni, vedere [Modalità di disponibilità &#40;gruppi di disponibilità Always On&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md).  
  
-   La destinazione del failover automatico deve avere uno stato di sincronizzazione integro ad indicare che ogni database secondario sulla destinazione del failover è sincronizzato con il database primario corrispondente.  
  
    > [!TIP]  
    >  Gruppi di disponibilità AlwaysOn esegue il monitoraggio dell'integrità di entrambe le repliche in un set di failover automatico. In caso di errore di una delle repliche, lo stato di integrità del gruppo di disponibilità è impostato su CRITICAL. In caso di errore della replica secondaria, il failover automatico non è possibile dal momento che la relativa destinazione non è disponibile. In caso di errore della replica primaria, verrà eseguito il failover del gruppo di disponibilità sulla replica secondaria. Fino a quando la replica primaria precedente non torna online, non sarà disponibile alcuna destinazione di failover automatico. In entrambi i casi, per garantire la disponibilità in caso di un errore sequenziale, è consigliabile configurare una replica secondaria diversa come destinazione del failover automatico.  
    >   
    >  Per altre informazioni, vedere [Utilizzare i criteri AlwaysOn per visualizzare l'integrità di un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/use-always-on-policies-to-view-the-health-of-an-availability-group-sql-server.md) e [Modificare la modalità di failover di una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md).  
  
-   Il cluster WSFC (Windows Server Failover Clustering) dispone del quorum. Per altre informazioni, vedere [Modalità quorum WSFC e configurazione del voto &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-quorum-modes-and-voting-configuration-sql-server.md).  
  
-   La replica primaria non è più disponibile e i livelli delle condizioni di failover definiti dai criteri di failover flessibili sono stati soddisfatti. Per informazioni sui livelli di condizione del failover, vedere [Criteri di failover flessibili per failover automatico di un gruppo di disponibilità &#40;SQL Server&#41;](./configure-flexible-automatic-failover-policy.md).  
  
###  <a name="how-automatic-failover-works"></a><a name="HowAutoFoWorks"></a> Funzionamento del failover automatico  
 Un failover automatico dà inizio alla sequenza di azioni seguente:  
  
1.  Se l'istanza del server che ospita la replica primaria corrente è ancora in esecuzione, lo stato dei database primari diventa DISCONNECTED e tutti i client vengono disconnessi.  
  
2.  Se alcuni record del log sono in attesa in code di recupero sulla replica secondaria di destinazione, la replica secondaria applica i rimanenti record del log per completare il rollforward dei database secondari.  
  
    > [!NOTE]  
    >  La quantità di tempo necessaria per applicare il log a un determinato database dipende dalla velocità del sistema, dal carico di lavoro recente e dal numero di record del log presenti nella coda di recupero.  
  
3.  Le replica secondaria precedente assume il ruolo primario. I relativi database diventano i database primari. La nuova replica primaria esegue il più rapidamente possibile il rollback di eventuali transazioni di cui non è ancora stato eseguito il commit (fase di rollback del recupero). I blocchi isolano queste transazioni in modo che il rollback venga eseguito in background mentre i client utilizzano il database. Questo processo non prevede il rollback delle transazioni di cui è già stato eseguito il commit.  
  
     Un database secondario finché non viene connesso viene brevemente contrassegnato come NOT_SYNCHRONIZED. Prima dell'avvio del recupero di rollback, i database secondari si possono connettere ai nuovi database primari e rapidamente passare allo stato SYNCHRONIZED. La situazione migliore generalmente è per una terza replica con commit sincrono che rimane nel ruolo secondario dopo il failover.  
  
4.  Al riavvio, l'istanza del server che ospita la replica primaria precedente riconosce che il ruolo primario è stato assunto da un'altra replica di disponibilità. La replica primaria precedente passa quindi al ruolo secondario e i relativi database diventano database secondari. La nuova replica secondaria si connette alla replica primaria corrente e intercetta il più rapidamente possibile il relativo database fino ai database primari correnti. Non appena la nuova replica secondaria completa la risincronizzazione dei relativi database, il failover è nuovamente possibile nella direzione inversa.  
  
###  <a name="to-configure-automatic-failover"></a><a name="EnableAutoFo"></a> Per configurare il failover automatico  
 Una replica di disponibilità può essere configurata per il supporto del failover automatico in qualsiasi momento.  
  
 **To configure automatic failover**  
  
1.  Assicurarsi che la replica secondaria sia configurata per l'utilizzo della modalità di disponibilità commit sincrono. Per altre informazioni, vedere [Modificare la modalità di disponibilità di una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md).  
  
2.  Impostare la modalità di failover su automatico. Per altre informazioni, vedere [Modificare la modalità di failover di una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md).  
  
3.  Facoltativamente, modificare i criteri di failover flessibili del gruppo di disponibilità per specificare gli ordinamenti di errori che possono fare in modo che si verifichi un failover automatico. Per altre informazioni, vedere [Configurare i criteri di failover flessibili per controllare le condizioni per il failover automatico &#40;Gruppi di disponibilità AlwaysOn&#41;](../../../database-engine/availability-groups/windows/configure-flexible-automatic-failover-policy.md) e [Criteri di failover per istanze del cluster di failover](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md).  
  
##  <a name="planned-manual-failover-without-data-loss"></a><a name="ManualFailover"></a> Failover manuale pianificato (senza perdita di dati)  
 Il failover manuale comporta il passaggio al ruolo primario di una replica secondaria sincronizzata dopo che un amministratore di database esegue un comando di failover manuale sull'istanza del server che ospita la replica secondaria di destinazione. Per supportare il failover manuale, la replica secondaria e la replica primaria corrente devono essere entrambe configurate per la modalità commit sincrono, se presente. Di ogni database secondario sulla replica di disponibilità deve essere creato un join al gruppo di disponibilità e sincronizzato con il database primario corrispondente, vale a dire la replica secondaria deve essere sincronizzata. In questo modo si assicura che ogni transazione sottoposta a commit sul database primario precedente lo sia anche sul nuovo database primario. I nuovi database primari risulteranno pertanto identici ai database primari precedenti.  
  
 Nella figura seguente vengono illustrate le fasi di un failover pianificato:  
  
1.  Prima del failover, la replica primaria è ospitata dall'istanza del server in `Node01`.  
  
2.  Un amministratore di database avvia un failover pianificato. La destinazione del failover è la replica di disponibilità ospitata dall'istanza del server in `Node02`.  
  
3.  La destinazione del failover (in `Node02`) diventa la nuova replica primaria. Poiché questo è un failover pianificato, la prima replica primaria passa al ruolo secondario durante il failover e porta immediatamente i database online come database secondari.  
  
 ![Illustrazione di un failover manuale pianificato](../../../database-engine/availability-groups/windows/media/aoag-plannedmanualfailover.gif "Illustrazione di un failover manuale pianificato")  
  
 **Contenuto della sezione**  
  
-   [Condizioni necessarie per un failover manuale](#ManualFailoverConditions)  
  
-   [Funzionamento del failover manuale](#ManualFailoverHowWorks)  
  
-   [Mantenimento della disponibilità durante gli aggiornamenti](#ManualFailoverDuringUpgrades)  
  
###  <a name="conditions-required-for-a-manual-failover"></a><a name="ManualFailoverConditions"></a> Condizioni necessarie per un failover manuale  
 Per supportare un failover manuale, la replica primaria corrente deve essere impostata sulla modalità commit sincrono e una replica secondaria deve essere:  
  
-   Configurata per la modalità commit sincrono.  
  
-   Attualmente sincronizzata con la replica primaria.  
  
 Per eseguire manualmente il failover su un gruppo di disponibilità, è necessario connettersi alla replica secondaria che deve diventare la nuova replica primaria.  
  
###  <a name="how-a-planned-manual-failover-works"></a><a name="ManualFailoverHowWorks"></a> Funzionamento del failover manuale pianificato  
 Il failover manuale pianificato, che deve essere avviato sulla replica secondaria di destinazione, dà inizio alla sequenza di azioni seguente:  
  
1.  Per assicurarsi che nessuna nuova transazione utente si verifichi sui database primari originali, il cluster WSFC invia una richiesta alla replica primaria per passare alla modalità offline.  
  
2.  Se alcuni log sono in attesa nella coda di recupero di un database secondario qualsiasi, la replica secondaria completa il rollforward di quel database secondario. La quantità di tempo necessaria dipende dalla velocità del sistema, dal carico di lavoro recente e dal numero di record del log nella coda di recupero. Per conoscere le dimensioni correnti della coda di recupero, usare il contatore delle prestazioni **Coda di recupero** . Per altre informazioni, vedere [SQL Server, replica di database](../../../relational-databases/performance-monitor/sql-server-database-replica.md).  
  
    > [!NOTE]  
    >  Il tempo di failover può essere regolato limitando le dimensioni della coda di recupero. Questa operazione, tuttavia, può causare un rallentamento della replica primaria a vantaggio della replica secondaria.  
  
3.  La replica secondaria diventa la nuova replica primaria e la replica primaria precedente diventa la nuova replica secondaria.  
  
4.  La nuova replica primaria esegue il rollback di eventuali transazioni di cui non è stato eseguito il commit e porta online i database come database primari. Tutti i database secondari vengono brevemente contrassegnati come NOT_SYNCHRONIZED finché non vengono connessi e risincronizzati con i nuovi database primari. Questo processo non prevede il rollback delle transazioni di cui è già stato eseguito il commit.  
  
5.  Quando la replica primaria precedente viene riportata online, assume il ruolo secondario e il database primario diventa il database secondario. La nuova replica secondaria risincronizza rapidamente i nuovi database secondari con i database primari corrispondenti.  
  
    > [!NOTE]  
    >  Non appena la nuova replica secondaria completa la risincronizzazione dei database, il failover è nuovamente possibile nella direzione inversa.  
  
 Dopo il failover, è necessario riconnettere i client al database primario corrente. Per altre informazioni, vedere [Listener del gruppo di disponibilità, connettività client e failover dell'applicazione &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md).  
  
###  <a name="maintaining-availability-during-upgrades"></a><a name="ManualFailoverDuringUpgrades"></a> Mantenimento della disponibilità durante gli aggiornamenti  
 L'amministratore di database per i gruppi di disponibilità può utilizzare i failover manuali per gestire la disponibilità dei database durante aggiornamenti hardware o software. Per utilizzare un gruppo di disponibilità per aggiornamenti software, l'istanza del server e/o il nodo computer che ospita la replica secondaria di destinazione deve avere già ricevuto gli aggiornamenti. Per altre informazioni, vedere [Aggiornamento delle istanze di replica dei gruppi di disponibilità AlwaysOn](../../../database-engine/availability-groups/windows/upgrading-always-on-availability-group-replica-instances.md).  
  
##  <a name="forced-failover-with-possible-data-loss"></a><a name="ForcedFailover"></a> Failover forzato (con possibile perdita di dati)  
 Il failover forzato di un gruppo di disponibilità (con possibile perdita di dati) è un metodo di ripristino di emergenza che consente di usare una replica secondaria come server warm standby. Dal momento che il failover forzato implica il rischio di possibili perdite di dati, è consigliabile usarlo con cautela e quando strettamente necessario. È consigliabile forzare il failover solo se è necessario ripristinare immediatamente il servizio sui database di disponibilità e si è disposti a rischiare la perdita di dati. Per altre informazioni sui prerequisiti, consigli per il failover forzato e uno scenario di esempio che usa il failover forzato per il ripristino da un errore irreversibile, vedere [Eseguire un failover manuale forzato di un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md).  
  
> [!WARNING]  
>  Il failover forzato richiede che il cluster WSFC disponga di quorum. Per informazioni sulla configurazione e la forzatura del quorum, vedere [WSFC &#40;Windows Server Failover Clustering&#41; con SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md).  
  
 **Contenuto della sezione**  
  
-   [Funzionamento del failover forzato](#ForcedFailoverHowWorks)  
  
-   [Rischi correlati al failover forzato](#ForcedFailoverRisks)  
  
-   [Motivi per cui è necessario il failover forzato dopo aver forzato il quorum](#WhyFFoPostForcedQuorum)  
  
-   [Come tenere traccia della potenziale perdita di dati](#TrackPotentialDataLoss)  
  
-   [Gestione della potenziale perdita di dati](#ForcedFailoverManagingDataLoss)  
  
###  <a name="how-forced-failover-works"></a><a name="ForcedFailoverHowWorks"></a> Funzionamento del failover forzato  
 Tramite il failover forzato viene avviata una transizione del ruolo primario a una replica di destinazione il cui ruolo si trova nello stato SECONDARY o RESOLVING. La destinazione del failover diventa la nuova replica primaria e tramite essa le copie dei database diventano immediatamente disponibili ai client. Quando la replica primaria precedente diventa disponibile, assume il ruolo secondario e i relativi database diventano database secondari.  
  
 Tutti i database secondari (compresi i database primari precedenti, una volta diventati disponibili) si trovano nello stato SUSPENDED. A seconda dello stato della sincronizzazione dei dati precedente di un database secondario sospeso, potrebbe essere adatto per recuperare i dati mancanti di cui è stato eseguito il commit per il database primario. Su una replica secondaria configurata per l'accesso in sola lettura, è possibile eseguire query sui database secondari per individuare manualmente i dati mancanti. È quindi possibile eseguire istruzioni [!INCLUDE[tsql](../../../includes/tsql-md.md)] per apportare le modifiche necessarie sui nuovi database primari.  
  
###  <a name="risks-of-forcing-failover"></a><a name="ForcedFailoverRisks"></a> Rischi correlati al failover forzato  
 È fondamentale comprendere che il failover forzato può causare la perdita di dati. La perdita di dati è possibile in quanto tramite la replica di destinazione non è possibile comunicare con la replica primaria e pertanto non è possibile garantire che i database siano sincronizzati. Il failover forzato comporta l'avvio di un nuovo fork di recupero. Poiché i database primari originali e i database secondari si trovano su fork di recupero diversi, ognuno di essi contiene dati che l'altro database non contiene: ogni database primario originale contiene tutte le modifiche non inviate al database secondario precedente dalla sua coda di invio (log non inviato). I database secondari precedenti contengono tutte le modifiche apportate dopo il failover forzato.  
  
 Se il failover viene forzato a causa di un errore della replica primaria, la potenziale perdita di dati dipende dal fatto che uno o più log delle transazioni siano stati inviati o meno alla replica secondaria prima dell'errore. Nella modalità commit asincrono, la presenza di log non inviati accumulati è sempre una possibilità. Nella modalità commit sincrono, questo è possibile solo fino a quando i database secondari non diventano sincronizzati.  
  
 Nella tabella seguente è riepilogata la possibilità di perdita di dati per un particolare database sulla replica a cui viene applicato un failover forzato.  
  
|Modalità di disponibilità della replica secondaria|Il database è sincronizzato?|È possibile che si verifichi una perdita dei dati?|  
|--------------------------------------------|-------------------------------|----------------------------|  
|Synchronous-commit|Sì|No|  
|Synchronous-commit|No|sì|  
|Asynchronous-commit|No|Sì|  
  
 I database secondari registrano solo due fork di recupero, pertanto se si eseguono più failover forzati, qualsiasi database secondario che abbia dato inizio alla sincronizzazione dati con il failover forzato precedente non potrà essere ripreso. In questo caso, i database secondari che non possono essere ripresi dovranno essere rimossi dal gruppo di disponibilità, ripristinati fino al momento corretto e nuovamente aggiunti al gruppo di disponibilità. È possibile osservare l'errore 1408 con stato 103 in questo scenario (errore: 1408, gravità: 16, stato: 103). Un ripristino non funziona tra più fork di recupero, pertanto, assicurarsi di eseguire un backup del log dopo avere eseguito più failover forzati.  
  
###  <a name="why-forced-failover-is-required-after-forcing-quorum"></a><a name="WhyFFoPostForcedQuorum"></a> Motivi per cui è necessario il failover forzato dopo aver forzato il quorum  
 Dopo aver forzato il quorum sul cluster WSFC (*quorum forzato*), sarà necessario effettuare un failover forzato (con possibile perdita di dati) su ogni gruppo di disponibilità. Il failover forzato è necessario poiché lo stato effettivo dei valori del cluster WSFC potrebbe essere andato perso. Dopo un quorum forzato è necessario evitare i failover normali poiché una replica secondaria non sincronizzata potrebbe essere visualizzata come sincronizzata nel cluster WSFC riconfigurato.  
  
 Si consideri ad esempio un cluster WSFC che ospita un gruppo di disponibilità in tre nodi:  il nodo A ospita la replica primaria e sia il nodo B che il nodo C ospitano una replica secondaria. Il nodo C viene disconnesso dal cluster WSFC mentre la replica secondaria locale è sincronizzata (SYNCHRONIZED).  Tuttavia, nei nodi A e B viene mantenuto un quorum integro e il gruppo di disponibilità rimane online. Nel nodo A gli aggiornamenti continuano ad essere accettati dalla replica primaria, mentre nel nodo B prosegue la sincronizzazione della replica secondaria con quella primaria. La replica secondaria nel nodo C diventa non sincronizzata e viene persa la sincronizzazione con la replica primaria. Tuttavia, poiché il nodo C è disconnesso, la replica rimane, in modo non corretto, nello stato SYNCHRONIZED.  
  
 Se il quorum viene perso e, successivamente, viene forzato nel nodo A, lo stato di sincronizzazione del gruppo di disponibilità nel cluster WSFC deve essere corretto, con la replica secondaria nel nodo C visualizzata come UNSYNCHRONIZED. Tuttavia, se il quorum viene forzato nel nodo C, la sincronizzazione del gruppo di disponibilità non sarà corretta. Nel cluster verrà ripristinato lo stato di sincronizzazione attivo quando il nodo C era disconnesso, con la replica secondaria nel nodo C visualizzata *erroneamente* come SYNCHRONIZED. Poiché tramite i failover manuali pianificati viene garantita la sicurezza dei dati, una volta forzato il quorum non è consentito riportare un gruppo di disponibilità online.  
  
###  <a name="tracking-potential-data-loss"></a><a name="TrackPotentialDataLoss"></a> Come tenere traccia della potenziale perdita di dati  
 Quando nel cluster WSFC è disponibile un quorum integro, è possibile stimare il potenziale rischio corrente di perdita di dati nei database. Per una replica secondaria specifica, il potenziale rischio corrente di perdita di dati dipende dal ritardo dei database secondari locali rispetto ai database primari corrispondenti. Poiché la quantità di ritardo varia nel tempo, è consigliabile tenere traccia periodicamente della perdita di dati potenziale per i database secondari non sincronizzati. Ai fini del rilevamento del ritardo è necessario eseguire il confronto tra LSN ultimo commit e Ora ultimo commit per ogni database primario e i relativi database secondari, come riportato di seguito:  
  
1.  Connettersi alla replica primaria.  
  
2.  Eseguire una query nelle colonne **last_commit_lsn** (LSN dell'ultima transazione sottoposta a commit) e **last_commit_time** (ora dell'ultimo commit) della DMV [sys.dm_hadr_database_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) .  
  
3.  Confrontare i valori restituiti per ogni database primario e tutti i relativi database secondari. La differenza tra gli LSN ultimo commit indica la quantità di ritardo.  
  
4.  È possibile attivare un avviso quando la quantità di ritardo in un database o set di database supera il ritardo massimo desiderato per un periodo di tempo specificato. Ad esempio, la query può essere eseguita da un processo che viene eseguito ogni minuto in ciascun database primario. Se la differenza tra **last_commit_time** di un database primario e uno dei relativi database secondari ha superato l'obiettivo del punto di recupero (RPO, Recovery Point Objective), ad esempio 5 minuti, dall'ultima esecuzione del processo, quest'ultimo può generare un avviso.  
  
> [!IMPORTANT]  
>  Quando nel cluster WSFC manca il quorum o quest'ultimo è stato forzato, **last_commit_lsn** e **last_commit_time** sono NULL. Per informazioni su come evitare la perdita di dati dopo aver forzato un quorum, vedere "Metodi possibili per evitare la perdita di dati dopo aver forzato il quorum" in [Eseguire un failover manuale forzato di un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md).  
  
###  <a name="managing-the-potential-data-loss"></a><a name="ForcedFailoverManagingDataLoss"></a> Gestione della potenziale perdita di dati  
 Dopo aver forzato il failover, tutti i database secondari vengono sospesi. compresi i database primari precedenti. Ciò avviene dopo che la replica primaria precedente torna online e viene ora individuata come replica secondaria. È necessario riprendere manualmente ogni database sospeso singolarmente in ogni replica secondaria.  
  
 Quando la replica primaria precedente diventa disponibile, supponendo che i relativi database non siano danneggiati, è possibile tentare di gestire la potenziale perdita di dati. L'approccio disponibile per la gestione della potenziale perdita di dati dipende dal fatto che la replica primaria originale sia connessa o meno alla nuova replica primaria. Supponendo che la replica primaria originale possa accedere alla nuova istanza primaria, la riconnessione avviene in modo automatico e trasparente.  
  
#### <a name="the-original-primary-replica-has-reconnected"></a>La replica primaria originale è stata riconnessa  
 In genere, dopo un errore, al riavvio la replica primaria originale si riconnette rapidamente al proprio partner. Alla riconnessione, la replica primaria originale diventa la replica secondaria. I relativi database diventano i database secondari e vengono impostati sullo stato SUSPENDED. Per i nuovi database secondari non verrà eseguito il rollback a meno che non vengano ripresi.  
  
 I database sospesi, tuttavia, sono inaccessibili, pertanto non è possibile verificarli per capire quali dati andrebbero persi in caso di ripresa di un determinato database. Pertanto, la decisione relativa alla ripresa o alla rimozione di un database secondario dipende dal fatto che si sia disposti o meno ad accettare eventuali perdite di dati:  
  
-   Se la perdita di dati non è accettabile, rimuovere i database dal gruppo di disponibilità per recuperarli.  
  
     L'amministratore di database potrà recuperare i database primari precedenti e tentare di recuperare i dati che sarebbero andati persi. Tuttavia, quando un database primario precedente viene portato online, è diverso dal database primario corrente, pertanto l'amministratore di database deve rendere inaccessibile ai client il database rimosso o il database primario corrente in modo da evitare ulteriori divergenze tra i database e impedire problemi di failover al livello dei client.  
  
-   Se la perdita di dati fosse accettabile, è possibile riprendere i database secondari.  
  
     Se si riprende un nuovo database secondario, il rollback diventa il primo passaggio della sincronizzazione del database. Se nella coda di invio erano in attesa record di log al momento dell'errore, le transazioni corrispondenti vengono perdute, anche se con commit.  
  
#### <a name="the-original-primary-replica-has-not-reconnected"></a>La replica primaria originale non è stata riconnessa  
 Se è possibile impedire temporaneamente alla replica primaria originale di riconnettersi in rete alla nuova replica primaria, è possibile esaminare i database primari originali per capire quali dati andrebbero persi se venissero ripresi.  
  
-   Contesti in cui la perdita di dati è accettabile  
  
     Consentire alla replica primaria originale di riconnettersi alla nuova replica primaria. La riconnessione comporta la sospensione dei nuovi database secondari. Per avviare la sincronizzazione dati su un database, riprenderlo. La nuova replica secondaria elimina il fork di recupero originale per quel database, perdendo eventuali transazioni mai inviate o ricevute dalla replica secondaria precedente.  
  
-   Contesti in cui la perdita di dati non è ammissibile  
  
     Se il database primario originale contiene dati critici che andrebbero persi in caso di ripresa del database sospeso, è possibile conservare i dati sul database primario originale rimuovendolo dal gruppo di disponibilità. In seguito a questa operazione il database viene impostato sullo stato RESTORING. A questo punto, è consigliabile tentare di eseguire il backup della parte finale del log del database rimosso. È quindi possibile aggiornare il database primario corrente (il database secondario precedente) esportando i dati che si desidera recuperare dal database primario originale e importandoli nel database primario corrente. È consigliabile eseguire un backup completo del database primario aggiornato il più rapidamente possibile.  
  
     Nell'istanza del server che ospita la nuova replica secondaria, è possibile eliminare il database secondario sospeso e creare un nuovo database secondario ripristinando questo backup (e almeno un backup del log successivo) utilizzando RESTORE WITH NORECOVERY. È consigliabile rimandare backup del log aggiuntivi dei database primari correnti finché non vengono ripresi i database secondari corrispondenti.  
  
> [!WARNING]  
>  Il troncamento del log delle transazioni viene ritardato nel database primario, mentre gli eventuali database secondari vengono sospesi. Inoltre, l'integrità di sincronizzazione di una replica secondaria con commit sincrono non potrà passare allo stato HEALTHY finché un database locale qualsiasi rimane sospeso.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
 **Per configurare il comportamento del failover**  
  
-   [Modificare la modalità di disponibilità di una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md)  
  
-   [Modificare la modalità di failover di una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md)  
  
-   [Configurare i criteri di failover flessibili per controllare le condizioni per il failover automatico &#40;Gruppi di disponibilità AlwaysOn&#41;](../../../database-engine/availability-groups/windows/configure-flexible-automatic-failover-policy.md)  
  
 **Per eseguire un failover manuale**  
  
-   [Eseguire un failover manuale pianificato di un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md)  
  
-   [Eseguire un failover manuale forzato di un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)  
  
-   [Usare la Procedura guidata Failover del gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-fail-over-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Gestione di account di accesso e processi per i database di un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/logins-and-jobs-for-availability-group-databases.md)  
  
 **Per configurare la configurazione del quorum WSFC**  
  
-   [Configurare le impostazioni NodeWeight per il quorum del cluster](../../../sql-server/failover-clusters/windows/configure-cluster-quorum-nodeweight-settings.md)  
  
-   [Visualizzare le impostazioni NodeWeight per il quorum del cluster](../../../sql-server/failover-clusters/windows/view-cluster-quorum-nodeweight-settings.md)  
  
-   [Forzare l'avvio di un cluster WSFC senza un quorum](../../../sql-server/failover-clusters/windows/force-a-wsfc-cluster-to-start-without-a-quorum.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenuto correlato  
  
-   [Microsoft SQL Server Always On Solutions Guide for High Availability and Disaster Recovery (Guida alle soluzioni AlwaysOn di Microsoft SQL Server per la disponibilità elevata e il ripristino di emergenza)](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
-   [Blog del team di SQL Server Always On: blog ufficiale del team di SQL Server Always On](/archive/blogs/sqlalwayson/)  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Modalità di disponibilità &#40;gruppi di disponibilità AlwaysOn&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)   
 [Windows Server Failover Clustering &#40;WSFC&#41; con SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)   
 [Transazioni tra database non supportate per il mirroring del database o i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md)   
 [Criteri di failover per istanze del cluster di failover](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)   
 [Criteri di failover flessibili per failover automatico di un gruppo di disponibilità &#40;SQL Server&#41;](./configure-flexible-automatic-failover-policy.md)  
  
