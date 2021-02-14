---
title: Analysis Services con i gruppi di disponibilità
description: Se i gruppi di disponibilità Always On vengono usati come soluzione a disponibilità elevata, è possibile usare un database di questo gruppo come origine dati in una soluzione multidimensionale o tabulare di Analysis Services.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: 14d16bfd-228c-4870-b463-a283facda965
author: cawrites
ms.author: chadam
manager: erikre
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 1bcb05458c8e8792975606382e7a1cd24de628a3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338497"
---
# <a name="analysis-services-with-always-on-availability-groups"></a>Analysis Services con i gruppi di disponibilità AlwaysOn

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

  Un gruppo di disponibilità AlwaysOn è una raccolta predefinita di database relazionali di SQL Server per cui è previsto un failover reciproco quando le condizioni attivano un failover in uno dei database, reindirizzando le richieste a un database con mirroring su un'altra istanza nello stesso gruppo di disponibilità. Se i gruppi di disponibilità vengono utilizzati come soluzione di disponibilità elevata, è possibile utilizzare un database di questo gruppo come origine dati in una soluzione multidimensionale o tabulare di Analysis Services. Tutte le operazioni di Analysis Services elencate di seguito funzionano nel modo previsto quando si utilizza un database di disponibilità: elaborazione o importazione di dati, query dirette su dati relazionali (utilizzando la modalità DirectQuery o l'archiviazione ROLAP) e writeback.  
  
 Le attività di elaborazione e query sono carichi di lavoro di sola lettura. È possibile migliorare le prestazioni ripartendo questi carichi di lavoro su una replica secondaria leggibile. Per questo scenario sono necessarie operazioni di configurazione aggiuntive. Utilizzare l'elenco di controllo contenuto in questo argomento per assicurarsi di eseguire tutti i passaggi.  
  
##  <a name="prerequisites"></a><a name="bkmk_prereq"></a> Prerequisiti  
 È necessario disporre dell'accesso di SQL Server su tutte le repliche. È necessario disporre dei privilegi di **sysadmin** per configurare i gruppi, i listener e i database di disponibilità, ma per gli utenti sono sufficienti le autorizzazioni di **db_datareader** per accedere al database da un client Analysis Services.  
  
 Utilizzare un provider di dati che supporti il protocollo TDS (Tabular Data Stream) versione 7.4 o successive, ad esempio SQL Server Native Client 11.0 o il provider di dati per SQL Server in .NET Framework 4.02.  
  
 **(Per i carichi di lavoro di sola lettura)** . Il ruolo della replica secondaria deve essere configurato per le connessioni di sola lettura, il gruppo di disponibilità deve presentare un elenco di routing e la connessione nell'origine dati di Analysis Services deve specificare il listener del gruppo di disponibilità. In questo argomento sono incluse le indicazioni necessarie per eseguire l'attivazione.  
  
##  <a name="checklist-use-a-secondary-replica-for-read-only-operations"></a><a name="bkmk_UseSecondary"></a> Elenco di controllo: utilizzare una replica secondaria per le operazioni di sola lettura  
 È possibile configurare una connessione all'origine dati per l'utilizzo di una replica secondaria leggibile se la soluzione di Analysis Services include il writeback. Se si dispone di una connessione di rete veloce, la replica secondaria genera una latenza dati molto bassa, fornendo dati pressoché identici a quelli della replica primaria. Utilizzando la replica secondaria per le operazioni di Analysis Services, è possibile ridurre le contese di lettura-scrittura sulla replica primaria e utilizzare in modo migliore le repliche secondarie nel gruppo di disponibilità.  
  
 Per impostazione predefinita, gli accessi in lettura e scrittura e l'accesso con finalità di lettura sono entrambi consentiti alla replica primaria, ma alle repliche secondarie non sono consentite connessioni. Per impostare una connessione client di sola lettura a una replica secondaria, è necessaria una configurazione aggiuntiva. La configurazione richiede l'impostazione di proprietà sulla replica secondaria e l'esecuzione di uno script T-SQL per la definizione di un elenco di routing di sola lettura. Effettuare le procedure seguenti per assicurarsi di avere eseguito entrambi i passaggi.  
  
> [!NOTE]  
>  I passaggi seguenti presumono l'esistenza di un gruppo di disponibilità AlwaysOn e dei relativi database. Se si configura un nuovo gruppo, utilizzare la Creazione guidata Gruppo di disponibilità per creare il gruppo e creare un join dei database. Tramite la procedura guidata vengono controllati i prerequisiti, vengono fornite istruzioni per ogni passaggio e viene eseguita la sincronizzazione iniziale. Per altre informazioni, vedere [Utilizzare la Creazione guidata Gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md).  
  
#### <a name="step-1-configure-access-on-an-availability-replica"></a>Passaggio 1: Configurare l'accesso su una replica di disponibilità  
  
1.  In Esplora oggetti connettersi all'istanza del server che ospita la replica primaria ed espandere l'albero del server.  
  
    > [!NOTE]  
    >  Questi passaggi sono descritti in [Configurare l'accesso in sola lettura in una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md) che include informazioni aggiuntive e istruzioni alternative per l'esecuzione di questa attività.  
  
2.  Espandere il nodo **Disponibilità elevata AlwaysOn** e il nodo **Gruppi di disponibilità**.  
  
3.  Fare clic sul gruppo di disponibilità di cui si desidera modificare la replica. Espandere **Repliche di disponibilità**.  
  
4.  Fare clic con il pulsante destro del mouse sulla replica secondaria e scegliere **Proprietà**.  
  
5.  Nella finestra di dialogo **Proprietà replica di disponibilità** modificare l'accesso alla connessione per il ruolo secondario, come segue:  
  
    -   Nell'elenco a discesa **Secondario leggibile** selezionare **Solo finalità di lettura**.  
  
    -   Nell'elenco a discesa **Connessioni nel ruolo primario** selezionare **Consenti tutte le connessioni**. Questa è la modalità predefinita.  
  
    -   Facoltativamente, nell'elenco a discesa **Modalità di disponibilità** selezionare **Commit sincrono**. Questo passaggio non è obbligatorio, ma assicura la parità dei dati tra la replica primaria e quella secondaria.  
  
         Questa proprietà è anche un requisito per il failover pianificato. Per eseguire un failover manuale pianificato a scopo di testing, impostare **Modalità di disponibilità** su **Commit sincrono** per le repliche primaria e secondaria.  
  
#### <a name="step-2-configure-read-only-routing"></a>Passaggio 2: Configurare il routing di sola lettura  
  
1.  Connettersi alla replica primaria.  
  
    > [!NOTE]  
    >  Questi passaggi sono descritti in [Configurare il routing di sola lettura per un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md) che include informazioni aggiuntive e istruzioni alternative per l'esecuzione di questa attività.  
  
2.  Aprire una finestra Query e incollare lo script seguente. Tramite questo script è possibile abilitare le connessioni leggibili a una replica secondaria (disabilitata per impostazione predefinita), impostare l'URL del routing di sola lettura e creare l'elenco di routing in cui viene stabilita la priorità di indirizzamento delle richieste di connessione.  La prima istruzione, ovvero quella che consente le connessioni leggibili, è ridondante se le proprietà sono già state impostate in [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)], ma è stata inclusa per motivi di completezza.  
  
    ```  
    ALTER AVAILABILITY GROUP [AG1]  
     MODIFY REPLICA ON  
    N'COMPUTER01' WITH   
    (SECONDARY_ROLE (ALLOW_CONNECTIONS = READ_ONLY));  
  
    ALTER AVAILABILITY GROUP [AG1]  
     MODIFY REPLICA ON  
    N'COMPUTER01' WITH   
    (SECONDARY_ROLE (READ_ONLY_ROUTING_URL = N'TCP://COMPUTER01.contoso.com:1433'));  
  
    ALTER AVAILABILITY GROUP [AG1]  
     MODIFY REPLICA ON  
    N'COMPUTER02' WITH   
    (SECONDARY_ROLE (ALLOW_CONNECTIONS = READ_ONLY));  
  
    ALTER AVAILABILITY GROUP [AG1]  
     MODIFY REPLICA ON  
    N'COMPUTER02' WITH   
    (SECONDARY_ROLE (READ_ONLY_ROUTING_URL = N'TCP://COMPUTER02.contoso.com:1433'));  
  
    ALTER AVAILABILITY GROUP [AG1]   
    MODIFY REPLICA ON  
    N'COMPUTER01' WITH   
    (PRIMARY_ROLE (READ_ONLY_ROUTING_LIST=('COMPUTER02','COMPUTER01')));  
  
    ALTER AVAILABILITY GROUP [AG1]   
    MODIFY REPLICA ON  
    N'COMPUTER02' WITH   
    (PRIMARY_ROLE (READ_ONLY_ROUTING_LIST=('COMPUTER01','COMPUTER02')));  
    GO  
    ```  
  
3.  Modificare lo script sostituendo i segnaposto con valori validi per la distribuzione:  
  
    -   Sostituire "Computer01" con il nome dell'istanza del server in cui è ospitata la replica primaria.  
  
    -   Sostituire "Computer02" con il nome dell'istanza del server in cui è ospitata la replica secondaria.  
  
    -   Sostituire "contoso.com" con il nome del dominio oppure ometterlo dallo script se tutti i computer si trovano nello stesso dominio. Mantenere il numero di porta se il listener utilizza la porta predefinita. La porta effettivamente utilizzata dal listener è elencata nella pagina delle proprietà di [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)].  
  
4.  Eseguire lo script.  
  
     A questo punto, creare un'origine dati in un modello di Analysis Services che utilizza un database dal gruppo appena configurato.  
  
##  <a name="create-an-analysis-services-data-source-using-an-always-on-availability-database"></a><a name="bkmk_ssasAODB"></a> Creare un'origine dati di Analysis Services utilizzando un database di disponibilità AlwaysOn  
 In questa sezione viene illustrato come creare un'origine dati di Analysis Services connessa a un database in un gruppo di disponibilità. È possibile utilizzare queste istruzioni per configurare una connessione a una replica primaria (impostazione predefinita) o a una replica secondaria leggibile configurata in base ai passaggi della sezione precedente. Le impostazioni di configurazione AlwaysOn e le proprietà di connessione impostate nel client determineranno l'uso della replica primaria o di quella secondaria.  
  
1.  In [!INCLUDE[ssBIDevStudio](../../../includes/ssbidevstudio-md.md)], in un progetto di modello di data mining e multidimensionale di Analysis Services, fare clic con il pulsante destro del mouse su **Origini dati** e selezionare **Nuova origine dati**. Per creare una nuova origine dati, fare clic su **Nuova** .  
  
     In alternativa, per un progetto di modello tabulare scegliere **Importa da origine dati** dal menu Modello.  
  
2.  In Gestione connessione, in Provider, scegliere un provider che supporti il protocollo TDS (Tabular Data Stream ). Questo protocollo è supportato da SQL Server Native Client 11.0.  
  
3.  In Gestione connessione, in Nome server, immettere il nome del *listener del gruppo di disponibilità*, quindi scegliere un database disponibile nel gruppo.  
  
     Il listener del gruppo di disponibilità reindirizza una connessione client a una replica primaria per le richieste di lettura-scrittura o a una replica secondaria se si specifica la finalità di lettura nella stringa di connessione. Poiché i ruoli delle repliche cambiano durante un failover (la replica primaria diventa secondaria e una secondaria diventa primaria), è sempre necessario specificare il listener in modo che la connessione client venga reindirizzata di conseguenza.  
  
     Per determinare il nome del listener del gruppo di disponibilità, è possibile rivolgersi a un amministratore di database o connettersi a un'istanza del gruppo di disponibilità e visualizzare la relativa configurazione della disponibilità AlwaysOn.   
  
4.  In Gestione connessione scegliere **Tutto** nel riquadro di navigazione sinistro per visualizzare la griglia delle proprietà del provider di dati.  
  
     Impostare **Finalità dell'applicazione** su **READONLY** per configurare una connessione client di sola lettura su una replica secondaria. In caso contrario, mantenere l'impostazione predefinita **READWRITE** per indirizzare la connessione alla replica primaria.  
  
5.  In Impostazioni di rappresentazione selezionare **Usa nome utente e password specifici di Windows**, quindi immettere un account utente di dominio di Windows che disponga di un numero minimo di autorizzazioni **db_datareader** per il database.  
  
     Non scegliere **Usa credenziali dell'utente corrente** o **Eredita**. È possibile scegliere **Usa account del servizio**, ma solo se tale account dispone delle autorizzazioni di lettura per il database.  
  
     Terminare l'origine dati e chiudere la Creazione guidata origine dati.  
  
6.  Aggiungere **MultiSubnetFailover=Yes** alla stringa di connessione per offrire un rilevamento e una connessione più rapidi al server attivo. Per informazioni su questa proprietà di connessione, vedere [Supporto di SQL Server Native Client per il ripristino di emergenza a disponibilità elevata](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md).  
  
     Questa proprietà non è visibile nella griglia delle proprietà. Per aggiungere la proprietà, fare clic con il pulsante destro del mouse sull'origine dati e scegliere **Visualizza codice**. Aggiungere `MultiSubnetFailover=Yes` alla stringa di connessione.  
  
 L'origine dati è ora definita. È ora possibile procedere alla compilazione di un modello, partendo dalla vista origine dati o, nel caso di modelli tabulari, dalla creazione di relazioni. Quando è il momento di recuperare dati dal database di disponibilità, ad esempio quando si è pronti a elaborare o distribuire la soluzione, è possibile testare la configurazione per verificare che i dati siano accessibili dalla replica secondaria.  
  
##  <a name="test-the-configuration"></a><a name="bkmk_test"></a> Testare la configurazione  
 Dopo avere configurato la replica secondaria e creato una connessione all'origine dati in Analysis Services, è possibile confermare che i comandi di query ed elaborazione vengano reindirizzati alla replica secondaria. È anche possibile eseguire un failover manuale pianificato per verificare il piano di recupero per questo scenario.  
  
#### <a name="step-1-confirm-the-data-source-connection-is-redirected-to-the-secondary-replica"></a>Passaggio 1: Confermare che la connessione all'origine dati venga reindirizzata alla replica secondaria  
  
1.  Avviare SQL Server Profiler e connettersi all'istanza di SQL Server che ospita la replica secondaria.  
  
     Durante l'esecuzione della traccia, tramite gli eventi **SQL:BatchStarting** e **SQL:BatchCompleting** verranno mostrate le query emesse da Analysis Services eseguite sull'istanza del motore di database. Questi eventi sono selezionati per impostazione predefinita, pertanto è sufficiente avviare la traccia.  
  
2.  In [!INCLUDE[ssBIDevStudio](../../../includes/ssbidevstudio-md.md)]aprire il progetto o la soluzione di Analysis Services contenente la connessione all'origine dati che si desidera testare. Assicurarsi che l'origine dati specifichi il listener del gruppo di disponibilità e non un'istanza nel gruppo.  
  
     Questo passaggio è importante. Il routing alla replica secondaria non avviene se si specifica un nome di istanza del server.  
  
3.  Disporre le finestre dell'applicazione per una visualizzazione side-by-side di SQL Server Profiler e [!INCLUDE[ssBIDevStudio](../../../includes/ssbidevstudio-md.md)] .  
  
4.  Distribuire la soluzione, e al termine, arrestare la traccia.  
  
     Nella finestra di traccia vengono visualizzati gli eventi correlati all'applicazione **Microsoft SQL Server Analysis Services**. Vengono anche visualizzate le istruzioni **SELECT** che consentono di recuperare dati da un database sull'istanza del server che ospita la replica secondaria, a prova che la connessione è stata effettuata attraverso il listener alla replica secondaria.  
  
#### <a name="step-2-perform-a-planned-failover-to-test-the-configuration"></a>Passaggio 2: Eseguire un failover pianificato per testare la configurazione  
  
1.  In [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] verificare le repliche primaria e secondaria per assicurarsi che entrambe siano configurate per la modalità commit sincrono e siano attualmente sincronizzate.  
  
     I passaggi seguenti presumono che una replica secondaria sia configurata per il commit sincrono.  
  
     Per verificare la sincronizzazione, aprire una connessione a ogni istanza che ospita le repliche primaria e secondaria, espandere la cartella Database e assicurarsi che al nome del database sia stato aggiunto **(Sincronizzato)** e **(Sincronizzazione in corso)** in ogni replica.  
  
    > [!NOTE]  
    >  Questi passaggi sono descritti in [Eseguire un failover manuale pianificato di un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md) che include informazioni aggiuntive e istruzioni alternative per l'esecuzione di questa attività.  
  
2.  In SQL Server Profiler avviare le tracce per ogni replica e visualizzarle side-by-side. Nei passaggi seguenti verranno confrontate le tracce per confermare che le query SQL utilizzate per l'elaborazione e le query da Analysis Services vegano passate da una replica all'altra.  
  
3.  Eseguire un comando di elaborazione o query da Analysis Services. Poiché l'origine dati è stata configurata per una connessione di sola lettura, il comando viene eseguito sulla replica secondaria.  
  
4.  In [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]connettersi alla replica secondaria.  
  
5.  Espandere il nodo **Disponibilità elevata AlwaysOn** e il nodo **Gruppi di disponibilità**.  
  
6.  Fare clic con il pulsante destro del mouse sul gruppo di disponibilità di cui eseguire il failover e selezionare il comando **Failover** . Verrà avviata la Creazione guidata Gruppo di disponibilità di failover. Utilizzare la procedura guidata per scegliere la replica da impostare come nuova replica primaria.  
  
7.  Confermare che il failover sia stato eseguito correttamente:  
  
    -   In [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]espandere i gruppi di disponibilità per visualizzare le designazioni (primaria) e (secondaria). L'istanza che in precedenza era una replica primaria dovrebbe ora essere una replica secondaria.  
  
    -   Visualizzare il dashboard per determinare se sono stati rilevati problemi di integrità. Fare clic con il pulsante destro del mouse sul gruppo di disponibilità e scegliere **Mostra dashboard**.  
  
8.  Attendere uno o due minuti che il failover venga completato sul back-end.  
  
9. Ripetere il comando di query o elaborazione nella soluzione di Analysis Services, quindi osservare le tracce side-by-side in SQL Server Profiler. Le prove dell'elaborazione saranno visibili sull'altra istanza, che è ora la nuova replica secondaria.  
  
##  <a name="what-happens-after-a-failover-occurs"></a><a name="bkmk_whathappens"></a> Cosa accade dopo un failover  
 Durante un failover, una replica secondaria assume il ruolo primario e la replica primaria precedente passa al ruolo secondario. Tutte le connessioni client vengono terminate, la proprietà del listener del gruppo di disponibilità viene trasferita insieme al ruolo di replica primaria alla nuova istanza di SQL Server e l'endpoint del listener viene associato alle porte TCP e agli indirizzi IP virtuali della nuova istanza. Per altre informazioni, vedere [Informazioni sull'accesso alla connessione client per le repliche di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md).  
  
 Se si verifica un failover durante l'elaborazione, si verifica l'errore seguente nel file di log o nella finestra di output di Analysis Services: "Errore OLE DB: errore OLE DB o ODBC: Errore di collegamento durante la comunicazione; 08S01; Provider TPC: Connessione in corso interrotta forzatamente dall'host remoto. ; 08S01."  
  
 Questo errore viene risolto se si attende un minuto e si riprova. Se il gruppo di disponibilità viene configurato correttamente per la replica secondaria leggibile, l'elaborazione verrà ripresa sulla nuova replica secondaria al successivo tentativo.  
  
 Gli errori persistenti sono con tutta probabilità dovuti a un problema di configurazione. È possibile provare a eseguire nuovamente lo script T-SQL per risolvere i problemi con l'elenco di routing, gli URL del routing di sola lettura e la finalità di lettura sulla replica secondaria. È inoltre necessario verificare che la replica primaria consenta tutte le connessioni.  
  
##  <a name="writeback-when-using-an-always-on-availability-database"></a><a name="bkmk_writeback"></a> Writeback quando si utilizza un database di disponibilità AlwaysOn  
 Il writeback è una funzionalità di Analysis Services che supporta l'analisi di simulazione in Excel. È comunemente utilizzato per attività di elaborazione budget e previsioni nelle applicazioni personalizzate.  
  
 Il supporto per il writeback richiede una connessione client READWRITE. In Excel se si prova a eseguire il writeback in una connessione di sola lettura, si verifica l'errore seguente: "Impossibile recuperare i dati dall'origine dati esterna." "Impossibile recuperare i dati dall'origine dati esterna."  
  
 Se una connessione è stata configurata per l'accesso continuo a una replica secondaria leggibile, è ora necessario configurare una nuova connessione che utilizzi una connessione READWRITE alla replica primaria.  
  
 A tale scopo, creare un'origine dati aggiuntiva in un modello di Analysis Services per supportare la connessione di lettura-scrittura. Quando si crea l'origine dati aggiuntiva, usare lo stesso nome di listener e lo stesso database specificati nella connessione di sola lettura, ma anziché modificare **Finalità dell'applicazione**, mantenere l'impostazione predefinita che supporta le connessioni READWRITE. È ora possibile aggiungere le nuove tabelle delle dimensioni e dei fatti alla vista origine dati basate sull'origine dati di lettura-scrittura, quindi abilitare il writeback sulle nuove tabelle.  
  
## <a name="see-also"></a>Vedere anche  
 [Listener del gruppo di disponibilità, connettività client e failover dell'applicazione &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [Repliche secondarie attive: Repliche secondarie leggibili &#40;Gruppi di disponibilità AlwaysOn&#41;](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)   
 [Criteri AlwaysOn per problemi operativi con gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-policies-for-operational-issues-always-on-availability.md)   
 [Creare un'origine dati &#40;SSAS multidimensionale&#41;](/analysis-services/multidimensional-models/create-a-data-source-ssas-multidimensional)   
 [Abilitare il writeback della dimensione](/analysis-services/multidimensional-models/bi-wizard-enable-dimension-writeback)  
  
