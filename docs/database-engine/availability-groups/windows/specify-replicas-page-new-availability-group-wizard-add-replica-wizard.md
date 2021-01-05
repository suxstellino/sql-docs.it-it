---
title: 'Creazione guidata del gruppo di disponibilità: Pagina Specifica repliche'
description: Descrive le opzioni della pagina Specifica repliche della Creazione guidata gruppo di disponibilità in SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.newagwizard.listeners.f1
- sql13.swb.addreplicawizard.specifyreplicas.f1
- sql13.swb.newagwizard.specifyreplicas.f1
ms.assetid: 2d90fc12-a67b-4bd0-b0ab-899b73017196
author: cawrites
ms.author: chadam
ms.openlocfilehash: fc0fd2d74ceea77b0c577f7bf3315695385274eb
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97639277"
---
# <a name="specify-replicas-page-new-availability-group-wizard-add-replica-wizard"></a>Pagina Specifica repliche (Creazione guidata Gruppo di disponibilità: Procedura guidata Aggiungi replica)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In questo argomento vengono descritte le opzioni della pagina **Specifica repliche** . Questa pagina si trova nella **[!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)]** e nella **[!INCLUDE[ssAoAddRepWiz](../../../includes/ssaoaddrepwiz-md.md)]** . Tramite la pagina **Specifica repliche** è possibile specificare e configurare una o più repliche di disponibilità da aggiungere al gruppo di disponibilità. Nella pagina sono presenti quattro schede, presentate nella tabella seguente. Fare clic sul nome di una scheda nella tabella per accedere alla sezione corrispondente, più avanti in questo argomento.  
  
|Scheda|Breve descrizione|  
|---------|-----------------------|  
|[Repliche](#ReplicasTab)|Utilizzare questa scheda per specificare ogni istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] che ospiterà o ospita attualmente una replica secondaria. Si noti che la replica primaria sarà ospitata nell'istanza del server a cui si è attualmente connessi.<br /><br /> Completare specificando tutte le repliche nella scheda **Repliche** prima di iniziare le altre schede.<br/><br/> Nota: il **failover automatico** è disabilitato se il tipo di cluster è **NONE**. SQL Server supporta solo il failover manuale quando un gruppo di disponibilità non è presente in un cluster. <br/><br/> Quando il tipo di cluster è EXTERNAL, la modalità di failover è **Esterno**. <br/><br/> Quando si aggiunge una replica, tutte le nuove repliche devono trovarsi nello stesso tipo di sistema operativo delle repliche esistenti. <br/><br/>Quando si aggiunge una replica, se la replica primaria è in un cluster WSFC, le repliche secondarie devono essere nello stesso cluster.|
|[Endpoints](#EndpointsTab)|Usare questa scheda per verificare eventuali endpoint del mirroring di database esistenti e, inoltre, se tale endpoint risulta mancante in un'istanza del server i cui account del servizio usano l'autenticazione di Windows, per creare l'endpoint automaticamente.|  
|[Preferenze di backup](#BackupPreferencesTab)|Usare questa scheda per specificare le preferenze di backup per il gruppo di disponibilità nel suo complesso e le priorità di backup per le singole repliche di disponibilità.|  
|[Listener](#Listener)|Utilizzare questa scheda, se disponibile, per creare un listener del gruppo di disponibilità. Per impostazione predefinita, non viene creato un listener.<br /><br /> Questa scheda è disponibile solo se si esegue la [!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)].<br/><br/>DHCP è disabilitato quando il tipo di cluster è EXTERNAL o NESSUNO. |  
  
##  <a name="replicas-tab"></a><a name="ReplicasTab"></a> Scheda Repliche  
 **Istanza del server**  
 Consente di visualizzare il nome dell'istanza del server che ospiterà la replica di disponibilità.  
  
 Se un'istanza del server che si pensa di usare per ospitare una replica secondaria non è elencata nella griglia **Repliche di disponibilità** , fare clic sul pulsante **Aggiungi replica** . Se si configura un gruppo di disponibilità in un ambiente IT ibrido (vedere [Disponibilità elevata e ripristino di emergenza di SQL Server in Macchine virtuali di Azure](/previous-versions/azure/jj870962(v=azure.100))), è possibile fare clic sul pulsante **Aggiungi replica Azure** per creare macchine virtuali con repliche secondarie in Azure.  
  
 **Ruolo iniziale**  
 Indica il ruolo che la nuova replica eseguirà inizialmente: **Primaria** o **Secondaria**.  
  
 **Failover automatico (fino a 3)**  
 Selezionare questa casella di controllo solo se si desidera che questa replica di disponibilità sia un partner di failover automatico. Per configurare il failover automatico, è necessario scegliere questa opzione per la replica primaria iniziale e per una replica secondaria. Per entrambe le repliche verrà utilizzata la modalità di disponibilità con commit sincrono. Il failover automatico è supportato solo da tre repliche.  
  
 Per informazioni sulla modalità di disponibilità con commit sincrono, vedere [Modalità di disponibilità &#40;gruppi di disponibilità AlwaysOn&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md). Per informazioni sul failover automatico, vedere [Failover e modalità di failover &#40;gruppi di disponibilità AlwaysOn&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md).  
  
 **Commit sincrono (fino a 3)**  
 Se è stata selezionata l'opzione **Failover automatico (fino a 3)** per la replica, verrà anche selezionata l'opzione **Commit sincrono (fino a 3)** . Se la casella di controllo è vuota, selezionarla solo se si desidera che la modalità con commit sincrono venga utilizzata dalla replica solo con il failover manuale pianificato. La modalità con commit sincrono può essere utilizzata solo da tre repliche.  
  
 Se si desidera utilizzare la modalità di disponibilità con commit asincrono per questa replica, lasciare vuota questa casella di controllo. La replica supporterà solo il failover manuale forzato (con possibile perdita di dati). Per informazioni sulla modalità di disponibilità con commit asincrono, vedere [Modalità di disponibilità &#40;gruppi di disponibilità AlwaysOn&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md). Per informazioni sul failover manuale pianificato e sul failover manuale forzato, vedere [Failover e modalità di failover &#40;gruppi di disponibilità AlwaysOn&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md).  
  
 **Ruolo secondario leggibile**  
 Selezionare un valore dall'elenco a discesa **Secondario leggibile** , come indicato di seguito:  
  
 **No**  
 Non sono consentite connessioni dirette ai database secondari di questa replica. I database non sono disponibili per l'accesso in lettura. Si tratta dell'impostazione predefinita.  
  
 **Solo con finalità di lettura**  
 Sono consentite solo connessioni dirette di sola lettura ai database secondari di questa replica. Il database o i database secondari sono tutti disponibili per l'accesso in lettura.  
  
 **Sì**  
 Sono consentite tutte le connessioni ai database secondari di questa replica, ma solo per l'accesso in lettura. Il database o i database secondari sono tutti disponibili per l'accesso in lettura.  
  
 **Aggiungi replica**  
 Fare clic per aggiungere una replica secondaria al gruppo di disponibilità.  
  
 **Aggiungi replica Azure**  
 Fare clic per creare una macchina virtuale di Azure che esegue una replica secondaria nel gruppo di disponibilità. Questa opzione può essere applicata solo a un gruppo di disponibilità in un ambiente IT ibrido che contiene repliche locali. Per altre informazioni, vedere [Disponibilità elevata e ripristino di emergenza di SQL Server in Macchine virtuali di Azure](/previous-versions/azure/jj870962(v=azure.100)).  
  
 **Rimuovi replica**  
 Fare clic per rimuovere la replica secondaria selezionata dal gruppo di disponibilità.  
  
##  <a name="endpoints-tab"></a><a name="EndpointsTab"></a> Scheda Endpoint  
 Per ogni istanza del server in cui verrà ospitata una replica di disponibilità, nella scheda **Endpoint** vengono visualizzati i valori effettivi dell'endpoint del mirroring del database esistente, se presente, oppure i valori suggeriti per un nuovo endpoint potenziale in cui verrebbe utilizzata l'autenticazione di Windows. Sia per gli endpoint esistenti che per quelli potenziali, nella griglia Valori endpoint vengono visualizzate le informazioni seguenti:  
  
 **Nome server**  
 Visualizza il nome di un'istanza del server che ospiterà una replica di disponibilità.  
  
 **URL endpoint**  
 Visualizza l'URL effettivo o proposto dell'endpoint del mirroring del database. Per un nuovo endpoint proposto, è possibile modificare questo valore. Per informazioni sul formato di questi URL, vedere [Specificare l'URL dell'endpoint quando si aggiunge o si modifica una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md).  
  
 **Numero di porta**  
 Visualizza il numero di porta effettivo o proposto dell'endpoint. Per un nuovo endpoint proposto, è possibile modificare questo valore.  
  
 **Nome endpoint**  
 Visualizza il nome effettivo o proposto dell'endpoint. Per un nuovo endpoint proposto, è possibile modificare questo valore.  
  
 **Crittografa i dati**  
 Indica se i dati inviati su questo endpoint sono crittografati. Per un nuovo endpoint proposto, è possibile modificare questa impostazione.  
  
 **Account del servizio SQL Server**  
 Nome utente dell'account del servizio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
 Affinché in un'istanza del server si utilizzi un endpoint che prevede l'autenticazione di Windows, l'account del servizio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] deve essere un account di dominio.  
  
 Questo requisito determina il passaggio di configurazione successivo, come indicato di seguito:  
  
-   Se ogni istanza del server è in esecuzione in un account del servizio di dominio, ovvero se nella colonna **Account del servizio SQL Server** è visualizzato un account del servizio di dominio per ogni istanza del server, fare clic su **Avanti**.  
  
-   Se un'istanza del server è in esecuzione in un account del servizio non di dominio, è necessario apportare una modifica manuale all'istanza del server prima di continuare con la procedura guidata. In questo caso, facendo clic su **Avanti** viene visualizzata una finestra di dialogo di avviso. Fare clic su **No** per tornare alla scheda **Endpoint** . Lasciando aperta la pagina **Specifica repliche** della procedura guidata, apportare una delle modifiche seguenti a ogni istanza del server per la quale nella colonna **Account del servizio SQL Server** viene visualizzato un account del servizio non di dominio:  
  
    -   Utilizzare Gestione configurazione [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per modificare l'account del servizio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in un account di dominio. Per altre informazioni, vedere [Modificare l'account di avvio del servizio di SQL Server &#40;Gestione configurazione SQL Server&#41;](../../../database-engine/configure-windows/scm-services-change-the-service-startup-account.md).  
  
    -   Utilizzare [!INCLUDE[tsql](../../../includes/tsql-md.md)] o PowerShell per creare manualmente un endpoint del mirroring del database in cui venga utilizzato un certificato. Per altre informazioni, vedere [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../../t-sql/statements/create-endpoint-transact-sql.md) o [Creare un endpoint del mirroring del database per i gruppi di disponibilità AlwaysOn &#40;PowerShell SQL Server&#41;](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md).  
  
     Se si lascia aperta la pagina **Specifica repliche** durante la configurazione degli endpoint, tornare alla scheda **Endpoint** e fare clic su **Aggiorna** per aggiornare la griglia **Valori endpoint** .  
  
##  <a name="backup-preferences-tab"></a><a name="BackupPreferencesTab"></a> Scheda Preferenze di backup  
 Per specificare dove eseguire i backup, scegliere una delle opzioni seguenti:  
  
 **Preferisco secondario**  
 Specifica che i backup devono essere eseguiti su una replica secondaria tranne quando la replica primaria è l'unica replica online. In tal caso il backup deve essere eseguito sulla replica primaria. Questa è l'opzione predefinita.  
  
 **Solo secondaria**  
 Specifica che i backup non devono mai essere eseguiti sulla replica primaria. Se la replica primaria è l'unica replica online, il backup non viene eseguito.  
  
 **Server/istanza primaria**  
 Specifica che i backup devono essere sempre eseguiti sulla replica primaria. Questa opzione è utile se sono necessarie funzionalità di backup, ad esempio la creazione di backup differenziali, che non sono supportate quando il backup viene eseguito su una replica secondaria.  
  
 **Qualsiasi replica**  
 Specifica che si preferisce che i processi di backup ignorino il ruolo delle repliche di disponibilità nella scelta della replica per l'esecuzione dei backup. Si noti che i processi di backup potrebbero valutare altri fattori, ad esempio la priorità di backup di ogni replica di disponibilità in combinazione con lo stato operativo e lo stato connesso.  
  
> [!IMPORTANT]  
>  L'impostazione della preferenza di backup non viene forzata. L'interpretazione di questa preferenza dipende dall'eventuale logica su cui si basano gli script dei processi di backup per i database in un determinato gruppo di disponibilità. Per altre informazioni, vedere [Repliche secondarie attive: Backup su repliche secondarie &#40;Gruppi di disponibilità Always On&#41;](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md).  
  
### <a name="replica-backup-priorities-grid"></a>Griglia Priorità di backup replica  
 Utilizzare la griglia **Priorità di backup replica** per specificare le priorità di backup per ciascuna replica del gruppo di disponibilità. La griglia include le colonne seguenti:  
  
 **Istanza del server**  
 Visualizza il nome dell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] che ospita la replica di disponibilità.  
  
 **Priorità di backup (Più bassa=1, Più alta=100)**  
 Assegna la priorità di esecuzione dei backup nella replica rispetto alle altre repliche nello stesso gruppo di disponibilità. Il valore predefinito è 50. È possibile selezionare qualsiasi altro numero intero nell'intervallo 0..100. 1 indica la priorità più bassa e 100 indica la priorità più alta. Se si imposta **Priorità di backup** su 1, la replica di disponibilità verrà scelta per l'esecuzione dei backup solo se non sono attualmente disponibili repliche di disponibilità con priorità più alta.  
  
 **Escludi replica**  
 Evita che questa replica di disponibilità venga scelta per l'esecuzione di backup. Ciò si rivela utile, ad esempio, per una replica di disponibilità remota in cui non si desidera eseguire mai il failover dei backup.  
  
##  <a name="listener-tab"></a><a name="Listener"></a> Scheda Listener  
 Specificare la preferenza per un[listener del gruppo di disponibilità](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)che fornirà un punto di connessione client. I valori possibili sono:  
  
 **Non creare ora un listener del gruppo di disponibilità.**  
 Selezionare questa opzione per ignorare questo passaggio. È possibile creare un listener in un secondo momento. Per altre informazioni, vedere [Creare o configurare un listener del gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).  
  
 **Crea un listener del gruppo di disponibilità.**  
 Specificare le preferenze del listener per questo gruppo di disponibilità, nel modo seguente:  
  
 **Nome DNS del listener**  
 Specificare il nome di rete del listener. Questo nome deve essere univoco nel dominio e può contenere solo caratteri alfanumerici, trattini ( **-** ) e caratteri di sottolineatura ( **_** ), in qualsiasi ordine. Se viene specificato tramite la scheda **Listener** , il nome DNS può avere una lunghezza massima di 15 caratteri.  
  
> [!IMPORTANT]  
>  Se si immette un nome del listener DNS (o numero di porta) non valido nella scheda **Listener** , il pulsante **Avanti** è disabilitato nella pagina **Specifica repliche** .  
  
 **Porta**  
 Specificare la porta TCP utilizzata dal listener.  
  
> [!NOTE]  
>  Se si immette un numero di porta (o nome del listener DNS) non valido nella scheda **Listener** , il pulsante **Avanti** è disabilitato nella pagina **Specifica repliche** .  
  
 **Modalità di rete**  
 Utilizzare l'elenco a discesa per selezionare la modalità di rete che verrà utilizzata da questo listener. I valori possibili sono:  
  
 **Indirizzo IP statico**  
 Selezionare questa opzione se si desidera che il listener sia in ascolto su più subnet. Per utilizzare la modalità di rete dell'indirizzo IP statico, un listener del gruppo di disponibilità deve essere in ascolto su ogni subnet che ospita una replica di disponibilità per il gruppo di disponibilità. Per ogni subnet, fare clic su **Aggiungi** per selezionare un indirizzo subnet e specificare un indirizzo IP.  
  
 Se si seleziona **Indirizzo IP statico** come modalità di rete (selezione predefinita), viene visualizzata una griglia con le colonne **Subnet** e **Indirizzo IP** e i pulsanti **Aggiungi** e **Rimuovi** associati. La griglia è vuota finché non si aggiunge la prima subnet.  
  
 Colonna **Subnet**  
 Viene visualizzato l'indirizzo subnet selezionato per ogni subnet aggiunta per il listener.  
  
 Colonna **Indirizzo IP**  
 Viene visualizzato l'indirizzo IPv4 o IPv6 specificato per una determinata subnet.  
  
 **Aggiungere**  
 Fare clic per aggiungere una subnet al listener. Verrà aperta la finestra di dialogo **Aggiungi indirizzo IP** . Per altre informazioni, vedere l'argomento della Guida [Finestra di dialogo Aggiungi indirizzo IP &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/add-ip-address-dialog-box-sql-server-management-studio.md).  
  
 **Rimuovi**  
 Fare clic per rimuovere la subnet attualmente selezionata nella griglia.  
  
 **DHCP**  
 Selezionare questa opzione se si desidera che il listener sia in ascolto su una sola subnet e utilizzi un indirizzo IPv4 dinamico assegnato da un server in cui viene eseguito il protocollo DHCP (Dynamic Host Configuration Protocol). DHCP è limitato a una sola subnet comune a ogni istanza del server che ospita una replica di disponibilità per il gruppo di disponibilità. DHCP non è disponibile per il tipo di cluster EXTERNAL o NONE.  
  
> [!IMPORTANT]  
>  Non è consigliabile utilizzare DHCP negli ambienti di produzione. Se si verifica un periodo di inattività e il lease IP DHCP scade, è necessario del tempo aggiuntivo per registrare il nuovo indirizzo IP della rete DHCP che è associato al nome DNS del listener e influisce sulla connettività client. DHCP può essere tranquillamente usato per la configurazione dell'ambiente di sviluppo e test per verificare le funzioni di base di gruppi di disponibilità e per l'integrazione con le applicazioni.  
  
 Quando si seleziona **DHCP** , viene visualizzato il campo **Subnet** .  
  
 **Subnet**  
 Se si seleziona **DHCP** come modalità di rete, utilizzare l'elenco a discesa **Subnet** per selezionare un indirizzo per la subnet che ospita le repliche di disponibilità del gruppo di disponibilità.  
  
> [!IMPORTANT]
>  Dopo avere definito un listener del gruppo di disponibilità, è consigliabile effettuare le operazioni seguenti:  
> 
>  -   Chiedere all'amministratore di rete di riservare l'indirizzo IP del listener per un uso esclusivo. Fornire il nome host DNS del listener agli sviluppatori dell'applicazione in modo da essere usato nelle stringhe di connessione per la richiesta di connessioni client al gruppo di disponibilità.  
> -   Fornire il nome host DNS del listener agli sviluppatori dell'applicazione in modo da essere usato nelle stringhe di connessione per la richiesta di connessioni client al gruppo di disponibilità.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Usare la Creazione guidata Gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Usare la procedura guidata Aggiungi replica a gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Creare o configurare un listener del gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [Utilizzare certificati per un endpoint del mirroring del database &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
-   [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../../t-sql/statements/create-endpoint-transact-sql.md)  
  
-   [Creare un endpoint del mirroring del database per i gruppi di disponibilità AlwaysOn &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/create-availability-group-transact-sql.md)   
 [Prerequisiti, restrizioni e consigli per i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
  
