---
title: Configurare Windows Firewall
description: Informazioni su come configurare Windows Firewall per consentire l'accesso a un'istanza di SQL Server attraverso il firewall.
ms.custom: contperf-fy21q3
ms.date: 04/07/2021
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- Windows Firewall ports
- WMI firewall ports
- Windows Firewall [Database Engine]
- firewall systems, configuring
- advfirewall
- firewall systems
- rules firewall
- firewall systems, overview and port list
- 1433 TCP port
- portopening using netsh
- ports [SQL Server], TCP
- netsh to open firewall ports
ms.assetid: f55c6a0e-b6bd-4803-b51a-f3a419803024
author: cawrites
ms.author: chadam
ms.openlocfilehash: ca3e3ae4255fc1815e690ef633350b33e4725ed9
ms.sourcegitcommit: 09122d02fc3d86c6028366653337c083da8a3f4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2021
ms.locfileid: "107072433"
---
# <a name="configure-the-windows-firewall-to-allow-sql-server-access"></a>Configure the Windows Firewall to Allow SQL Server Access
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

I sistemi firewall contribuiscono a impedire l'accesso non autorizzato alle risorse del computer. Se un firewall viene abilitato ma non configurato correttamente, i tentativi di connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] potrebbero essere bloccati.  
  
Per accedere a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] attraverso un firewall, è necessario configurare il firewall nel computer in cui viene eseguito [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il firewall è un componente di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows. È possibile anche installare un firewall di un altro produttore. Questo articolo illustra come configurare Windows Firewall, ma i principi di base si applicano anche ad altri programmi firewall.  
  
> [!NOTE]  
>  Questo articolo offre una panoramica della configurazione del firewall e riepiloga le informazioni utili a un amministratore di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni sul firewall e per informazioni sul firewall autorevole, vedere la documentazione di riferimento, ad esempio la [Guida alla distribuzione di sicurezza di Windows Firewall](/windows/security/threat-protection/windows-firewall/windows-firewall-with-advanced-security-deployment-guide).  
  
 Gli utenti esperti nella gestione di **Windows Firewall** e che sanno quali impostazioni del firewall vogliono configurare possono passare direttamente agli articoli più avanzati:  
  
-   [Configurare Windows Firewall per l'accesso al motore di database](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)    
-   [Configurare Windows Firewall per consentire l'accesso ad Analysis Services](/analysis-services/instances/configure-the-windows-firewall-to-allow-analysis-services-access)    
-   [Configurare un firewall per l'accesso al server di report](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md)  
  
##  <a name="basic-firewall-information"></a><a name="BKMK_basic"></a> Informazioni di base sul firewall  
 I firewall funzionano controllando i pacchetti in ingresso e confrontandolo con il set di regole seguente:
- Il pacchetto soddisfa gli standard stabiliti dalle regole, quindi il firewall passa il pacchetto al protocollo TCP/IP per un'ulteriore elaborazione. 
- Il pacchetto non soddisfa gli standard specificati dalle regole.
    - Il firewall elimina quindi il pacchetto.-se la registrazione è abilitata, viene creata una voce nel file di registrazione del firewall.  
  
 L'elenco del traffico consentito viene creato in uno dei modi seguenti:  

- **Automaticamente**: quando un computer con un firewall abilitato avvia la comunicazione, il firewall crea una voce nell'elenco in modo che la risposta sia consentita. La risposta è considerata traffico richiesto e non è necessario configurare alcun elemento. 
  
-   **Manuale**: Le eccezioni per il firewall vengono configurate dall'amministratore. Consente di accedere a programmi o porte specifici nel computer. In questo caso, il computer accetta il traffico in ingresso non richiesto quando svolge le funzioni di server, listener o peer. Per la connessione a è necessario completare la configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 La scelta del tipo di firewall più adatto alle proprie esigenze non si limita alla decisione relativa alla necessità di aprire o chiudere una determinata porta Quando si progetta una strategia di firewall per l'azienda, assicurarsi di prendere in considerazione tutte le regole e le opzioni di configurazione disponibili. In questo articolo non vengono esaminate tutte le possibili opzioni del firewall. Si consiglia di consultare i documenti seguenti:  
  
 [Guida alla distribuzione di Windows Firewall](/windows/security/threat-protection/windows-firewall/windows-firewall-with-advanced-security-deployment-guide)    
 [Guida alla progettazione di Windows Firewall](/windows/security/threat-protection/windows-firewall/windows-firewall-with-advanced-security-design-guide)    
 [Pagina concernente l'introduzione all'isolamento di dominio e server](/windows/security/threat-protection/windows-firewall/domain-isolation-policy-design)  
  
##  <a name="default-firewall-settings"></a><a name="BKMK_default"></a> Impostazioni del firewall predefinite  
 Il primo passaggio della pianificazione della configurazione del firewall consiste nel determinare lo stato corrente del firewall per il sistema operativo in uso. Se quest'ultimo è stato aggiornato da una versione precedente, è possibile che le impostazioni del firewall siano state conservate o Il Criteri di gruppo o l'amministratore può modificare le impostazioni del firewall nel dominio.
  
> [!NOTE]  
>  L'attivazione del firewall influisce su altri programmi a cui è consentito l'accesso al computer, ad esempio la condivisione di file e stampanti e le connessioni desktop remoto. Prima di modificare le impostazioni del firewall, gli amministratori devono tener conto di tutte le applicazioni in esecuzione nel computer.  
  
##  <a name="programs-to-configure-the-firewall"></a><a name="BKMK_programs"></a> Programmi di configurazione del firewall  
Configurare le impostazioni di Windows Firewall con **Microsoft Management Console** o **netsh**.  

-  **Microsoft Management Console (MMC)**  
  
     Lo snap-in MMC Windows Firewall con sicurezza avanzata consente di configurare impostazioni del firewall più avanzate. Questo snap-in presenta la maggior parte delle opzioni di firewall in una modalità di facile utilizzo e presenta tutti i profili firewall. Per altre informazioni, vedere [Utilizzo dello snap-in Windows Firewall con sicurezza avanzata](#BKMK_WF_msc) più avanti in questo articolo.  
  
-   **netsh**  
  
     Il **netsh.exe** è uno strumento di amministrazione per configurare e monitorare i computer basati su Windows da un prompt dei comandi o tramite un file batch **.** Tramite lo strumento **netsh** è possibile indirizzare i comandi di contesto immessi all'helper appropriato e l'helper esegue il comando. Un helper è un file dll (Dynamic Link Library) che estende la funzionalità. L'helper fornisce: configurazione, monitoraggio e supporto per uno o più servizi, utilità o protocolli per lo strumento **netsh** .

     Tutti i sistemi operativi che supportano [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dispongono di un helper del firewall. [!INCLUDE[firstref_longhorn](../../includes/firstref-longhorn-md.md)] ha anche un helper del firewall avanzato denominato **advfirewall**. Molte delle opzioni di configurazione descritte possono essere configurate con **netsh**. Ad esempio, eseguire lo script seguente al prompt dei comandi per aprire la porta TCP 1433:  
  
    ```  
    netsh firewall set portopening protocol = TCP port = 1433 name = SQLPort mode = ENABLE scope = SUBNET profile = CURRENT  
    ```  
  
     Esempio simile utilizzando l'helper Windows Firewall con sicurezza avanzata:  
  
    ```  
    netsh advfirewall firewall add rule name = SQLPort dir = in protocol = tcp action = allow localport = 1433 remoteip = localsubnet profile = DOMAIN  
    ```  
  
     Per altre informazioni su **netsh**, vedere i collegamenti seguenti:  
  
    -   [Sintassi, contesti e formattazione dei comandi Netsh](/windows-server/networking/technologies/netsh/netsh-contexts)    
    -   [Utilizzo del contesto "netsh advfirewall firewall" anziché del contesto "netsh firewall" per controllare il comportamento di Windows Firewall in Windows Server 2008 e in Windows Vista](https://support.microsoft.com/kb/947709)    

-   **PowerShell**

    Vedere l'esempio seguente per aprire la porta TCP 1433 e la porta UDP 1434 per SQL Server istanza predefinita e SQL Server Browser servizio:
    
    ```powershell
    New-NetFirewallRule -DisplayName "SQLServer default instance" -Direction Inbound -LocalPort 1433 -Protocol TCP -Action Allow
    New-NetFirewallRule -DisplayName "SQLServer Browser service" -Direction Inbound -LocalPort 1434 -Protocol UDP -Action Allow
    ```
    
    Per altri esempi, vedere [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule).
    
- **Per Linux**: in Linux è anche necessario aprire le porte associate ai servizi a cui è necessario accedere. Diverse distribuzioni di Linux e firewall diversi hanno procedure proprie. Per due esempi, vedere [SQL Server in Red Hat](../../linux/quickstart-install-connect-red-hat.md) e [SQL Server in SUSE](../../linux/quickstart-install-connect-suse.md). 
  
## <a name="ports-used-by-ssnoversion"></a>Porte utilizzate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Le tabelle seguenti consentono di identificare le porte utilizzate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
###  <a name="ports-used-by-the-database-engine"></a><a name="BKMK_ssde"></a> Ports Used By the Database Engine  
 

Per impostazione predefinita, le tipiche porte usate da SQL Server e dai servizi del motore di database associati sono: TCP **1433**, **4022**, **135**, **1434**, UDP **1434**. La tabella seguente illustra queste porte in maggior dettaglio. Un'istanza denominata usa [porte dinamiche](#BKMK_dynamic_ports).
 
 Nella tabella seguente sono indicate le porte più utilizzate dal [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
|Scenario|Porta|Commenti|  
|--------------|----------|--------------|  
|Istanza predefinita in esecuzione su TCP|Porta TCP 1433|Porta più comune consentita attraverso il firewall. Viene utilizzata nelle connessioni di routine all'installazione predefinita del [!INCLUDE[ssDE](../../includes/ssde-md.md)]o a un'istanza denominata che rappresenta l'unica istanza in esecuzione nel computer. Le istanze denominate presuppongo alcune considerazioni speciali. Vedere [Porte dinamiche](#BKMK_dynamic_ports) più avanti in questo articolo.|  
|Istanze denominate con porta predefinita|La porta TCP è una porta dinamica determinata all'avvio del [!INCLUDE[ssDE](../../includes/ssde-md.md)] .|Vedere la discussione che segue nella sezione [Porte dinamiche](#BKMK_dynamic_ports). La porta UDP 1434 potrebbe essere necessaria per il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servizio browser quando si usano istanze denominate.|  
| Istanze denominate con porta fissa |Il numero di porta configurato dall'amministratore.|Vedere la discussione che segue nella sezione [Porte dinamiche](#BKMK_dynamic_ports).|  
|Dedicated Admin Connection|Porta TCP 1434 per l'istanza predefinita. Per le istanze denominate vengono utilizzate altre porte. Per informazioni sul numero di porta, controllare il log degli errori.|Per impostazione predefinita, le connessioni remote alla connessione amministrativa dedicata (DAC) non sono abilitate. Per abilitare le connessioni DAC remote, utilizzare il facet Configurazione superficie di attacco. Per ulteriori informazioni, vedere [Surface Area Configuration](../../relational-databases/security/surface-area-configuration.md).|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser|Porta UDP 1434|Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servizio browser è in attesa delle connessioni in ingresso a un'istanza denominata. 
Il servizio fornisce al client il numero di porta TCP corrispondente a tale istanza denominata. In genere il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser viene avviato ogni volta che si usano istanze denominate del [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servizio browser non è necessario se il client è configurato per la connessione alla porta specifica dell'istanza denominata.|  
|Istanza con endpoint HTTP.|Può essere specificata quando viene creato un endpoint HTTP. Le impostazioni predefinite sono la porta TCP 80 per il traffico CLEAR_PORT e la porta 443 per il traffico SSL_PORT.|Utilizzata per una connessione HTTP tramite un URL.|  
|Istanza predefinita con endpoint HTTPS |Porta TCP 443|Utilizzata per una connessione HTTPS tramite un URL. HTTPS è una connessione HTTP che usa Transport Layer Security (TLS), denominato in precedenza Secure Sockets Layer (SSL).|  
|[!INCLUDE[ssSB](../../includes/sssb-md.md)]|Porta TCP 4022. Per verificare la porta utilizzata, eseguire la query seguente:<br /><br /> `SELECT name, protocol_desc, port, state_desc`<br /><br /> `FROM sys.tcp_endpoints`<br /><br /> `WHERE type_desc = 'SERVICE_BROKER'`|Non esiste una porta predefinita per, mentre gli [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssSB](../../includes/sssb-md.md)] esempi della documentazione online utilizzano la configurazione convenzionale.|  
|Mirroring del database|Porta scelta dall'amministratore. Per determinare la porta, eseguire la query seguente:<br /><br /> `SELECT name, protocol_desc, port, state_desc FROM sys.tcp_endpoints`<br /><br /> `WHERE type_desc = 'DATABASE_MIRRORING'`|Non esiste una porta predefinita per il mirroring del database, tuttavia negli esempi della documentazione online viene utilizzata la porta TCP 5022 o 7022. È importante evitare di interrompere un endpoint del mirroring in uso, soprattutto in modalità a protezione elevata con failover automatico. La configurazione del firewall deve evitare di interrompere il quorum. Per altre informazioni, vedere [Specificare un indirizzo di rete del server &#40;Mirroring del database&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).|  
|Replica|Connessioni di replica per l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzo delle [!INCLUDE[ssDE](../../includes/ssde-md.md)] porte normali tipiche (la porta TCP 1433 è l'istanza predefinita)<br /><br /> La sincronizzazione Web e l'accesso FTP/UNC per snapshot di replica richiedono l'apertura di più porte sul firewall. Per trasferire lo schema e i dati iniziali da una posizione all'altra, per la replica possono essere utilizzati il protocollo FTP (porta TCP 21), la sincronizzazione su HTTP (porta TCP 80) o la condivisione di file. La condivisione file utilizza la porta UDP 137 e 138 e la porta TCP 139 se utilizzata insieme a NetBIOS. La condivisione file utilizza la porta TCP 445.|Per la sincronizzazione su HTTP, la replica utilizza l'endpoint IIS (configurabile, la porta 80 per impostazione predefinita), ma il processo IIS si connette al back-end [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite le porte standard (1433 per l'istanza predefinita.<br /><br /> Durante la sincronizzazione Web tramite FTP, il trasferimento FTP avviene tra IIS e il server di pubblicazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , non tra il sottoscrittore e IIS.|  
|[!INCLUDE[tsql](../../includes/tsql-md.md)] debugger|Porta TCP 135<br /><br /> Vedere [Considerazioni speciali per la porta 135](#BKMK_port_135)<br /><br /> Potrebbe essere necessaria anche l'eccezione [IPsec](#BKMK_IPsec) .|Se si utilizza [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], nel computer host di [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] , è necessario aggiungere anche **Devenv.exe** all'elenco di eccezioni e aprire la porta TCP 135.<br /><br /> Se si utilizza [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], nel computer host di [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] , è necessario aggiungere anche **ssms.exe** all'elenco di eccezioni e aprire la porta TCP 135. Per altre informazioni, vedere [Configurare le regole del firewall prima di eseguire il debugger TSQL](../../ssms/scripting/configure-firewall-rules-before-running-the-tsql-debugger.md).|  
  
 Per istruzioni dettagliate su come configurare il Windows Firewall per [!INCLUDE[ssDE](../../includes/ssde-md.md)] , vedere [configurare un Windows Firewall per l'accesso motore di database](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md).  
  
####  <a name="dynamic-ports"></a><a name="BKMK_dynamic_ports"></a> Porte dinamiche  
 Per impostazione predefinita, per le istanze denominate, inclusa [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)], vengono utilizzate porte dinamiche.  indica che ogni volta [!INCLUDE[ssDE](../../includes/ssde-md.md)] viene avviato, identifica una porta disponibile e utilizza tale numero di porta. Se l'istanza denominata è l'unica istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] installata, è probabile che venga utilizzata la porta TCP 1433. Se sono installate altre istanze del [!INCLUDE[ssDE](../../includes/ssde-md.md)] , è probabile che venga utilizzata una porta TCP diversa. Poiché la porta selezionata potrebbe cambiare ogni volta che [!INCLUDE[ssDE](../../includes/ssde-md.md)] viene avviato, è difficile configurare il firewall per consentire l'accesso al numero di porta corretto. Se viene utilizzato un firewall, è consigliabile riconfigurare [!INCLUDE[ssDE](../../includes/ssde-md.md)] per utilizzare ogni volta lo stesso numero di porta. È consigliabile usare una porta fissa o una porta statica. Per altre informazioni, vedere [Configurazione di un server per l'attesa su una porta TCP specifica &#40;Gestione configurazione SQL Server&#41;](../../database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port.md).  
  
 Un'alternativa alla configurazione di un'istanza denominata in modo che si metta in attesa su una porta fissa consiste nel creare un'eccezione nel firewall per un programma di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ad esempio **sqlservr.exe** per il [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Il numero di porta non verrà visualizzato nella colonna **porta locale** della pagina **regole in ingresso** quando si utilizza lo snap-in MMC Windows Firewall con sicurezza avanzata. Può essere difficile controllare le porte aperte. Un'altra considerazione è che un Service Pack o un aggiornamento cumulativo può modificare il percorso del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] file eseguibile e invalidare la regola del firewall.  
  
##### <a name="to-add-a-program-exception-to-the-firewall-using-windows-defender-firewall-with-advanced-security"></a>Per aggiungere un'eccezione del programma al firewall usando Windows Defender Firewall con sicurezza avanzata
  
1. Dal menu Start digitare *wf.msc*. Premere INVIO o selezionare il risultato della ricerca wf.msc per aprire **Windows Defender Firewall con sicurezza avanzata**.
1. Nel riquadro sinistro selezionare **Regole connessioni in entrata**.
1. Nel riquadro destro in **Azioni** selezionare **Nuova regola**. Verrà aperta la **Creazione guidata nuova regola connessioni in entrata**.
1. In **Tipo di regola** selezionare **Programma**. Selezionare **Avanti**.
1. In **Programma** selezionare **Percorso programma**. Selezionare **Sfoglia** per individuare l'istanza di SQL Server. Il programma è chiamato sqlservr.exe Si trova in genere in:

   `C:\Program Files\Microsoft SQL Server\MSSQL15.<InstanceName>\MSSQL\Binn\sqlservr.exe`

   Selezionare **Avanti**.

1. In **Azione** selezionare **Consenti la connessione**. Selezionare **Avanti**.
1. In **Profilo** includere tutti e tre i profili. Selezionare **Avanti**.
1. In **Nome** digitare un nome per la regola. Selezionare **Fine**.

Per altre informazioni sugli endpoint, vedere [Configurazione del Motore di database per l'attesa su più porte TCP](../../database-engine/configure-windows/configure-the-database-engine-to-listen-on-multiple-tcp-ports.md) e [Viste del catalogo degli endpoint &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md). 
  
###  <a name="ports-used-by-analysis-services"></a><a name="BKMK_ssas"></a> Porte utilizzate da Analysis Services  
 
Per impostazione predefinita le porte tipiche usate da SQL Server Analysis Services e dai servizi associati sono: TCP **2382**, **2383**, **80**, **443**. La tabella seguente illustra queste porte in maggior dettaglio.  
 
 Nella tabella seguente sono indicate le porte più utilizzate da [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
|Funzionalità|Porta|Commenti|  
|-------------|----------|--------------|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|Porta TCP 2383 per l'istanza predefinita|La porta standard per l'istanza predefinita di [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser|Porta TCP 2382 necessaria solo per un'istanza denominata di [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|Le richieste di connessione client per un'istanza denominata di [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] che non specificano un numero di porta vengono indirizzate alla porta 2382, ovvero la porta su cui è in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ascolto il browser. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser la richiesta viene quindi reindirizzata alla porta utilizzata dall'istanza denominata.|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] configurato per l'uso tramite IIS/HTTP<br /><br /> Il Servizio PivotTable® utilizza HTTP o HTTPS|Porta TCP 80|Utilizzata per una connessione HTTP tramite un URL.|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] configurato per l'uso tramite IIS/HTTPS<br /><br /> Il Servizio PivotTable® utilizza HTTP o HTTPS|Porta TCP 443|Utilizzata per una connessione HTTPS tramite un URL. HTTPS è una connessione HTTP che usa TLS.|  
  
 Se gli utenti accedono [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] tramite IIS e Internet, è necessario aprire la porta su cui IIS è in ascolto. Specificare quindi la porta nella stringa di connessione client. In questo caso, non è necessario aprire alcuna porta per l'accesso diretto ad [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. È consigliabile limitare la porta predefinita 2389 e la porta 2382 insieme a tutte le altre porte non necessarie.  
  
 Per istruzioni dettagliate su come configurare il Windows Firewall per [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , vedere [configurare il Windows Firewall per consentire l'accesso Analysis Services](/analysis-services/instances/configure-the-windows-firewall-to-allow-analysis-services-access).  
  
###  <a name="ports-used-by-reporting-services"></a><a name="BKMK_ssrs"></a> Porte utilizzate da Reporting Services  

Per impostazione predefinita, le porte tipiche utilizzate da SQL Server Reporting Services e i servizi associati sono: TCP **80**, **443**. La tabella seguente illustra queste porte in maggior dettaglio. 


Nella tabella seguente sono indicate le porte più utilizzate da [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
|Funzionalità|Porta|Commenti|  
|-------------|----------|--------------|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Servizi Web|Porta TCP 80|Utilizzata per una connessione HTTP a [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] tramite un URL. Si consiglia di non utilizzare la regola preconfigurata **World Wide Web Services (http)**. Per ulteriori informazioni, vedere la sezione [Interazione con altre regole del firewall](#BKMK_other_rules) più avanti.|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] configurato per l'utilizzo tramite HTTPS|Porta TCP 443|Utilizzata per una connessione HTTPS tramite un URL. HTTPS è una connessione HTTP che usa TLS. Si consiglia di non utilizzare la regola preconfigurata **Secure World Wide Web Services (HTTPS)**. Per ulteriori informazioni, vedere la sezione [Interazione con altre regole del firewall](#BKMK_other_rules) più avanti.|  
  
Quando [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] si connette a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] o di [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], è necessario aprire anche le porte adatte per quei servizi. Per istruzioni dettagliate su come configurare Windows Firewall per [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vedere [Configurare un firewall per l'accesso al server di report](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md).  
  
###  <a name="ports-used-by-integration-services"></a><a name="BKMK_ssis"></a> Porte utilizzate da Integration Services  
 Nella tabella seguente sono indicate le porte utilizzate dal servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
|Funzionalità|Porta|Commenti|  
|-------------|----------|--------------|  
|[!INCLUDE[msCoName](../../includes/msconame-md.md)] Remote Procedure Call (MS RPC)<br /><br /> Utilizzate dal runtime di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .|Porta TCP 135<br /><br /> Vedere [Considerazioni speciali per la porta 135](#BKMK_port_135)|Il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] utilizza il protocollo DCOM sulla porta 135. Gestione controllo servizi utilizza la porta 135 per eseguire attività quali l'avvio e l'arresto del [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] servizio e la trasmissione di richieste di controllo al servizio in esecuzione. Il numero di porta non può essere modificato.<br /><br /> Questa porta deve essere aperta solo se ci si connette a un'istanza remota del [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] servizio da [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] o da un'applicazione personalizzata.|  
  
Per istruzioni dettagliate su come configurare Windows Firewall per [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], vedere [Integration Services Service &#40;SSIS Service&#41;](/previous-versions/sql/sql-server-2012/ms137861(v=sql.110)) (Servizio Integration Services - Servizio SSIS).  
  
###  <a name="another-ports-and-services"></a><a name="BKMK_additional_ports"></a> Altre porte e servizi  
Nella tabella riportata di seguito sono elencati i servizi e le porte da cui potrebbe dipendere [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  

|Scenario|Porta|Commenti|  
|--------------|----------|--------------|  
|Strumentazione gestione Windows (WMI)<br /><br /> Per ulteriori informazioni su **Strumentazione gestione Windows (WMI)**, vedere [concetti relativi al provider WMI per la gestione della configurazione](../../relational-databases/wmi-provider-configuration/wmi-provider-for-configuration-management.md)|WMI viene eseguito come parte di un host del servizio condiviso con porte assegnate tramite DCOM e potrebbe utilizzare la porta TCP 135.<br /><br /> Vedere [Considerazioni speciali per la porta 135](#BKMK_port_135)|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizza WMI per elencare e gestire servizi. È consigliabile usare il gruppo di regole preconfigurate **Strumentazione gestione Windows (WMI)** . Per ulteriori informazioni, vedere la sezione [Interazione con altre regole del firewall](#BKMK_other_rules) più avanti.|  
|[!INCLUDE[msCoName](../../includes/msconame-md.md)] Distributed Transaction Coordinator (MS DTC)|Porta TCP 135<br /><br /> Vedere [Considerazioni speciali per la porta 135](#BKMK_port_135)|Se l'applicazione utilizza transazioni distribuite, può essere necessario configurare il firewall in modo da consentire il flusso del traffico di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Distributed Transaction Coordinator (MS DTC) tra istanze MS DTC separate e tra MS DTC e strumenti di gestione delle risorse come [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. È consigliabile utilizzare il gruppo di regole preconfigurato **Distributed Transaction Coordinator** .<br /><br /> Quando è configurato un solo oggetto MS DTC condiviso per l'intero cluster in un gruppo di risorse distinto, è necessario aggiungere sqlservr.exe come eccezione al firewall.|  
|Il pulsante Sfoglia in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] utilizza il protocollo UDP per connettersi al servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser. Per altre informazioni, vedere [Servizio SQL Server Browser &#40;Motore database e SSAS&#41;](../../database-engine/configure-windows/sql-server-browser-service-database-engine-and-ssas.md).|Porta UDP 1434|UDP è un protocollo senza connessione.<br /><br /> Il firewall dispone di un'impostazione ([Proprietà UnicastResponsesToMulticastBroadcastDisabled dell'interfaccia INetFwProfile](/windows/win32/api/netfw/nf-netfw-inetfwprofile-get_unicastresponsestomulticastbroadcastdisabled)) che controlla il comportamento del firewall e delle risposte unicast a una richiesta UDP broadcast (o multicast).  Sono possibili due comportamenti:<br /><br /> Se l'impostazione è TRUE, non sono consentite risposte unicast a una trasmissione. I servizi di enumerazione non possono essere eseguiti correttamente.<br /><br /> Se l'impostazione è FALSE (impostazione predefinita), le risposte unicast sono consentite per 3 secondi. L'intervallo di tempo non è configurabile. In una rete congestionata o ad alta latenza o nei server con carico elevato i tentativi di enumerazione delle istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] potrebbero restituire un elenco parziale e fuorviante per gli utenti.|  
|<a name="BKMK_IPsec"></a> Traffico IPsec|Porta UDP 500 e porta UDP 4500|Se i criteri di dominio richiedono che le comunicazioni di rete vengano eseguite tramite IPsec, è necessario aggiungere anche le porte UDP 4500 e UDP 500 all'elenco delle eccezioni. IPsec è un'opzione che usa la **Creazione guidata nuova regola connessioni in entrata** nello snap-in Windows Firewall. Per altre informazioni, vedere [Utilizzo dello snap-in Windows Firewall con sicurezza avanzata](#BKMK_WF_msc) più avanti.|  
|Utilizzo dell'autenticazione di Windows con domini trusted|Per consentire le richieste di autenticazione, è necessario configurare i firewall.|Per ulteriori informazioni, vedere [Configurazione di un firewall per domini e trust](https://support.microsoft.com/kb/179442/).|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e servizio cluster di Windows|Il clustering richiede porte aggiuntive non direttamente correlate a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|Per ulteriori informazioni, vedere [Enable a network for cluster use](/previous-versions/windows/it-pro/windows-server-2003/cc728293(v=ws.10)).|  
|Spazi dei nomi URL riservati in HTTP Server API (HTTP.SYS)|Probabilmente la porta TCP 80, ma può essere configurata su altre porte. Per informazioni generali, vedere [Configuring HTTP and HTTPS](/dotnet/framework/wcf/feature-details/configuring-http-and-https).|Per informazioni specifiche [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sulla prenotazione di un endpoint HTTP.SYS usando HttpCfg.exe, vedere [Informazioni su prenotazioni e registrazione URL &#40;Gestione configurazione SSRS&#41;](../../reporting-services/install-windows/about-url-reservations-and-registration-ssrs-configuration-manager.md).|  
  
##  <a name="special-considerations-for-port-135"></a><a name="BKMK_port_135"></a> Considerazioni speciali per la porta 135  
 Quando si utilizza RPC con TCP/IP o UDP/IP come trasporto, le porte in ingresso vengono assegnate dinamicamente ai servizi di sistema come richiesto. Vengono utilizzate le porte TCP/IP e UDP/IP più grandi della porta 1024. Le porte sono denominate "porte RPC casuali". In questi casi, i client RPC si basano sul mapper di endpoint RPC per indicare le porte dinamiche assegnate al server. Per alcuni servizi basati su RPC è possibile configurare una porta specifica anziché consentire a RPC un'assegnazione dinamica. È inoltre possibile limitare l'intervallo di porte assegnate dinamicamente da RPC a un piccolo intervallo, indipendentemente dal servizio. Poiché la porta 135 viene usata per molti servizi, viene spesso aggredita da utenti malintenzionati. Quando si apre la porta 135, limitare l'ambito della regola del firewall.  
  
 Per ulteriori informazioni sulla porta 135, consultare i riferimenti seguenti:  
  
-   [Panoramica dei servizi e requisiti per le porte di rete per il sistema server Windows](https://support.microsoft.com/kb/832017)   
-   [Troubleshooting RPC Endpoint Mapper errors using the Windows Server 2003 Support Tools from the product CD](https://mskb.pkisolutions.com/kb/839880)  
-   [Remote procedure call (RPC)](/previous-versions/ms950395(v=msdn.10))    
-   [Configurazione dell'allocazione delle porte dinamiche RPC perché funzionino con i firewall](https://support.microsoft.com/kb/154596/)  
  
##  <a name="interaction-with-other-firewall-rules"></a><a name="BKMK_other_rules"></a> Interazione con altre regole del firewall  
 Per effettuare la configurazione di Windows Firewall, è necessario utilizzare regole e gruppi di regole. Ogni regola o gruppo di regole è associato a un particolare programma o servizio e tale programma o servizio potrebbe modificare o eliminare la regola senza alcuna conoscenza. I gruppi di regole **Servizi Web (HTTP)** e **Servizi Web (HTTPS)** , ad esempio, sono associati a IIS. L'abilitazione di queste due regole comporterà l'apertura delle porte 80 e 443 e l'attivazione delle funzionalità di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che dipendono da tali porte. È tuttavia possibile che gli amministratori che configurano IIS le modifichino o disabilitino. Se si usa la porta 80 o la porta 443 per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è necessario creare una regola personalizzata o un gruppo di regole che gestisce la configurazione della porta preferita indipendentemente dalle altre regole IIS.  
  
 Lo snap-in MMC Windows Firewall con sicurezza avanzata consente tutto il traffico che corrisponde alle regole di concessione applicabili. Quindi, se sono presenti due regole valide per la porta 80 (con parametri diversi). Il traffico che corrisponde a una delle regole sarà consentito. Se una regola consente il traffico sulla porta 80 dalla subnet locale e una regola consente il traffico da qualsiasi indirizzo, il risultato finale è che tutto il traffico verso la porta 80 è indipendente dall'origine. Per gestire efficacemente l'accesso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], gli amministratori devono verificare periodicamente tutte le regole del firewall abilitate sul server.  
  
##  <a name="overview-of-firewall-profiles"></a><a name="BKMK_profiles"></a> Panoramica sui profili del firewall  
 I profili del firewall vengono usati dai sistemi operativi per identificare e ricordare ognuna delle reti: connettività, connessioni e categoria.  
  
 Sono disponibili tre tipi di percorsi di rete in Windows Firewall con sicurezza avanzata:  
  
-   **Dominio**: Windows può autenticare l'accesso al controller di dominio per il dominio al quale il computer viene unito.
-   **Pubblico**: Ad eccezione di quelle del dominio, tutte le reti sono inizialmente classificate come pubbliche. Le reti che rappresentano connessioni dirette a Internet o che si trovano in luoghi pubblici, ad esempio aeroporti e Internet caffè, dovrebbero essere sempre pubbliche.
-   **Privato**: Una rete identificata da un utente o da un'applicazione come privata. Solo le reti attendibili devono essere identificate come reti private. In genere gli utenti desiderano identificare come private le reti domestiche o di piccole aziende.  
  
 L'amministratore può creare un profilo per ogni tipo di percorso di rete, contenente diversi criteri del firewall. È possibile applicare un solo profilo alla volta. L'ordine di applicazione è il seguente:  
  
1.  Il profilo di dominio viene applicato se tutte le interfacce vengono autenticate nel controller di dominio in cui il computer è membro.
2.  Se tutte le interfacce sono autenticate nel controller di dominio o sono connesse a reti classificate come percorsi di rete privati, viene applicato il profilo privato.    
3.  In caso contrario, viene applicato il profilo pubblico.  
  
 Utilizzare lo snap-in MMC Windows Firewall con sicurezza avanzata per visualizzare e configurare tutti i profili del firewall. L'elemento **Windows Firewall** del Pannello di controllo configura solo il profilo corrente.  
  
##  <a name="additional-firewall-settings-using-the-windows-firewall-item-in-control-panel"></a><a name="BKMK_additional_settings"></a> Impostazioni aggiuntive del firewall mediante l'elemento Windows Firewall del Pannello di controllo  
 Il firewall aggiunto può limitare l'apertura della porta alle connessioni in ingresso da computer specifici o subnet locale. Limitare l'ambito dell'apertura della porta per ridurre la quantità di esposizione del computer agli utenti malintenzionati.  
  
> [!NOTE]  
>  L'uso dell'elemento **Windows Firewall** del Pannello di controllo configura solo il profilo del firewall corrente.  
  
### <a name="to-change-the-scope-of-a-firewall-exception-using-the-windows-firewall-item-in-control-panel"></a>Per modificare l'ambito dell'eccezione di un firewall mediante l'elemento Windows Firewall del Pannello di controllo  
  
1.  Nell'elemento **Windows Firewall** del pannello di controllo selezionare un programma o una porta nella scheda **eccezioni** , quindi selezionare **proprietà** o **modifica**.  
  
2.  Nella finestra di dialogo **modifica programma** o **modifica porta** Selezionare **Cambia ambito**.  
  
3.  Scegliere una delle seguenti opzioni:  
  
    -   **Qualsiasi computer (inclusi i computer su Internet)**: non consigliato. Qualsiasi computer in grado di gestire il computer per connettersi al programma o alla porta specificata. Questa impostazione potrebbe essere necessaria per consentire la presentazione delle informazioni a utenti anonimi su Internet, ma aumenta l'esposizione a utenti malintenzionati. Se si abilita questa impostazione, l'attraversamento di Consenti NAT (Network Address Translation), ad esempio l'opzione Consenti attraversamento confini, aumenterà l'esposizione.  
  
    -   **Solo la mia rete (subnet)**: un'impostazione più sicura rispetto A **qualsiasi computer**. Solo i computer che si trovano nella subnet locale della rete possono connettersi al programma o alla porta.  
  
    -   **Elenco personalizzato**: Solo i computer che dispongono degli indirizzi IP elencati possono connettersi. Un'impostazione sicura può essere più sicura rispetto alla **mia rete (subnet)**. Tuttavia, i computer client che usano DHCP possono modificare occasionalmente il proprio indirizzo IP; Disabilita la possibilità di connessione. Un altro computer, che non è stato progettato per l'autorizzazione, potrebbe accettare l'indirizzo IP elencato e connettersi ad esso. L' **elenco personalizzato** è adatto per elencare altri server configurati per l'utilizzo di un indirizzo IP fisso.
    Gli indirizzi IP possono essere falsificati da un intruso. L'efficacia delle regole di limitazione del firewall equivale solo a quella dell'infrastruttura di rete.  
  
##  <a name="using-the-windows-firewall-with-advanced-security-snap-in"></a><a name="BKMK_WF_msc"></a> Utilizzo dello snap-in Windows Firewall con sicurezza avanzata  
 Per configurare le impostazioni avanzate del firewall, è possibile utilizzare lo snap-in MMC Windows Firewall con sicurezza avanzata. Lo snap-in include una procedura guidata per le regole e le impostazioni che non sono disponibili nella **Windows Firewall** elemento del pannello di controllo. Le impostazioni includono:  
  
-   Impostazioni di crittografia  
-   Restrizioni dei servizi   
-   Restrizione delle connessioni per i computer in base al nome    
-   Restrizione delle connessioni a utenti o profili specifici  
-   Attraversamento dei confini che consente al traffico di ignorare i router NAT (Network Address Translation)    
-   Configurazione di regole in uscita    
-   Configurazione di regole di sicurezza    
-   Richiesta di IPsec per le connessioni in ingresso  
  
### <a name="to-create-a-new-firewall-rule-using-the-new-rule-wizard"></a>Per creare una nuova regola del firewall utilizzando la Creazione guidata nuova regola connessioni in entrata  
  
1.  Dal menu Start scegliere **Esegui**, digitare **WF.msc** e quindi fare clic su **OK**.    
2.  In **Windows Firewall con sicurezza avanzata** nel riquadro sinistro fare clic con il pulsante destro del mouse su **Regole in entrata** e quindi selezionare **Nuova regola**.   
3.  Completare la **Creazione guidata nuova regola connessioni in entrata** utilizzando le impostazioni desiderate.  
  
##  <a name="troubleshooting-firewall-settings"></a><a name="BKMK_troubleshooting"></a> Risoluzione dei problemi relativi alle impostazioni del firewall  
 Le tecniche e gli strumenti illustrati di seguito possono risultare utili per la risoluzione dei problemi del firewall.  
  
-   Lo stato della porta più efficace è l'unione di tutte le regole relative alla porta. Può essere utile esaminare tutte le regole che citano il numero di porta, quando si tenta di bloccare l'accesso a una porta. Esaminare le regole con lo snap-in MMC Windows Firewall con sicurezza avanzata e ordinare le regole in ingresso e in uscita in base al numero di porta.  
  
-   Verificare le porte attive nel computer su cui è in esecuzione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Il processo di revisione include la verifica delle porte TCP/IP in ascolto e anche la verifica dello stato delle porte.  
  
     Per verificare quali porte sono in ascolto, visualizzare le connessioni TCP attive e le statistiche IP usare l'utilità della riga di comando **netstat** .  
  
    #### <a name="to-list-which-tcpip-ports-are-listening"></a>Per elencare le porte TCP/IP in attesa  
  
    1.  Aprire una finestra del prompt dei comandi.  
  
    2.  Al prompt dei comandi digitare **netstat -n -a**.  
  
         L'opzione **-n** indica a **netstat** di visualizzare in valori numerici l'indirizzo e il numero di porta delle connessioni TCP attive. L'opzione **-a** indica a **netstat** di visualizzare le porte TCP e UDP su cui è in attesa il computer.  
  
-   L'utilità **PortQry** può essere usata per indicare lo stato delle porte TCP/IP come in attesa, non in attesa o filtrato. (L'utilità potrebbe non ricevere risposta dalla porta se presenta uno stato filtrato). L'utilità **PortQry** è disponibile per il download dall' [area download Microsoft](https://www.microsoft.com/download/details.aspx?id=17148).  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica dei servizi e requisiti per le porte di rete per il sistema server Windows](https://support.microsoft.com/kb/832017)   
 [Procedura: Configurare le impostazioni del firewall (database SQL di Azure)](/azure/azure-sql/database/firewall-configure)  
  
