---
title: Risoluzione dei problemi relativi ai cluster di failover | Microsoft Docs
description: Informazioni sulla risoluzione dei problemi relativi ai cluster di failover, tra cui il ripristino in seguito a un errore, la risoluzione del problemi comuni e l'uso di stored procedure estese o oggetti COM.
ms.custom: ''
ms.date: 10/21/2015
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- troublshooting, failover clustering
- failover clustering, troubleshooting
- cluster troubleshooting
ms.assetid: 84012320-5a7b-45b0-8feb-325bf0e21324
author: cawrites
ms.author: chadam
ms.openlocfilehash: 34fe360248235da3c9a647799a178d9c734ad027
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350046"
---
# <a name="failover-cluster-troubleshooting"></a>Risoluzione dei problemi relativi al clustering di failover
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In questo argomento sono disponibili informazioni sugli argomenti seguenti:  
  
-   Procedura di base per la risoluzione dei problemi.  
  
-   Recupero da un errore del clustering di failover.  
  
-   Risoluzione dei problemi più comuni relativi al clustering di failover.  
  
-   Utilizzo di stored procedure estese e di oggetti COM.  
  
## <a name="basic-troubleshooting-steps"></a>Procedura di base per la risoluzione dei problemi  
 La prima attività di diagnostica è l'esecuzione del controllo di convalida sul cluster aggiornato. Per informazioni dettagliate sulla convalida, vedere [Guida dettagliata ai cluster di failover: convalida dell'hardware per un cluster di failover](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732035(v=ws.10)).  Il controllo può essere completato senza interruzione del servizio, poiché non ha effetto sulle risorse cluster in linea. La convalida può essere eseguita in qualsiasi momento dopo aver installato la funzionalità Clustering di failover, ovvero prima della distribuzione del cluster, durante la sua creazione e mentre il cluster è in esecuzione. In effetti mentre il cluster è in uso vengono eseguiti altri test, che verificano la corretta adozione delle procedure consigliate per i carichi di lavoro a disponibilità elevata. Delle decine di test eseguiti, solo alcuni hanno impatto sui carichi di lavoro del cluster in esecuzione. e rientrano tutti nella categoria di archiviazione, pertanto ignorare l'intera categoria è un modo pratico per evitare test che possono causare interruzioni.  
Il clustering di failover prevede una misura di protezione integrata che evita tempi di inattività imprevisti quando si eseguono test di archiviazione durante la convalida. Se il cluster presenta gruppi online all'avvio della convalida e i test di archiviazione restano selezionati, all'utente verrà chiesto di confermare l'esecuzione di tutti i test o di ignorare il test dei dischi di tutti i gruppi in linea, per evitare tempi di inattività. Se l'intera categoria di archiviazione è stata esclusa dal test, il messaggio non viene visualizzato. In questo modo la convalida del cluster avviene senza tempi di inattività.  
  
#### <a name="how-to-revalidate-your-cluster"></a>Procedura di riconvalida del cluster  
  
1.  Nell'albero della console dello snap-in Cluster di Failover, assicurarsi che l'opzione **Gestione Cluster di failover** sia selezionata e quindi selezionare **Convalida una configurazione** in **Gestione**.  
  
2.  Seguire le istruzioni della procedura guidata per specificare i server e i test ed eseguire i test. Al termine dei test verrà visualizzata la pagina **Riepilogo** .  
  
3.  Nella pagina **Riepilogo** fare clic su **Visualizza report** per visualizzare i risultati del test.  
  
     Per visualizzare i risultati dopo aver chiuso la procedura guidata, vedere **%SystemRoot%\Cluster\Reports\Validation Report Data e ora.html** dove **% SystemRoot %** rappresenta la cartella in cui è installato il sistema operativo, ad esempio **C:\Windows**.  
  
4.  Per visualizzare gli argomenti della Guida che consentono di interpretare i risultati, fare clic su **Ulteriori informazioni sui test di convalida dei cluster**.  
  
 Per visualizzare gli argomenti della Guida relativi alla convalida dei cluster dopo aver chiuso la procedura guidata, nello snap-in Cluster di failover fare clic su **?**, **Guida**, selezionare la scheda **Contenuto** , espandere il contenuto della Guida per il cluster di failover e quindi fare clic su **Convalida della configurazione di un cluster di failover**.  Al termine della procedura guidata di convalida, i risultati verranno visualizzati nel **Report di riepilogo** . Tutti i test devono essere superati con un segno di spunta verde o, in alcuni casi, un triangolo giallo di avviso. Quando si esaminano aree relative a problemi (X rosse o punti interrogativi gialli) nella parte del report che riepiloga i risultati del test, fare clic su un singolo test per esaminare i dettagli. Tutti i problemi segnalati con una X rossa devono essere risolti prima di passare alla risoluzione dei problemi di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
 **Installare gli aggiornamenti**  
  
 L'installazione degli aggiornamenti è importante per prevenire i problemi del sistema. Collegamenti utili:  
  
-   [Hotfix e aggiornamenti consigliati per i cluster di failover di Windows Server 2012 R2](https://support.microsoft.com/kb/2920151)  
  
-   [Hotfix e aggiornamenti consigliati per i cluster di failover di Windows Server 2012](https://support.microsoft.com/kb/2784261)  
  
-   [Hotfix e aggiornamenti consigliati per i cluster di failover di Windows Server 2008 R2](https://support.microsoft.com/kb/980054)  
  
-   [Hotfix e aggiornamenti consigliati per i cluster di failover di Windows Server 2008](https://support.microsoft.com/kb/957311)  
  
## <a name="recovering-from-failover-cluster-failure"></a>Recupero da un errore del clustering di failover  
 Sono in genere due le cause di un errore del clustering di failover:  
  
-   Errore hardware in uno dei nodi di un cluster a due nodi. L'errore hardware può essere causato dal danneggiamento della scheda SCSI o da un errore del sistema operativo.  
  
     Per eseguire il recupero in presenza di questo errore, utilizzare il programma di installazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per rimuovere il nodo bloccato dal cluster di failover, risolvere il problema hardware con il computer offline, ripristinare il computer, quindi aggiungere nuovamente il nodo riparato all'istanza del cluster di failover.  
  
     Per altre informazioni, vedere [Creare un nuovo cluster di failover di SQL Server &#40;programma di installazione&#41;](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md) e [Recuperare da un errore dell'istanza del cluster di failover](../../../sql-server/failover-clusters/windows/recover-from-failover-cluster-instance-failure.md).  
  
-   Errore del sistema operativo. In questo caso, il nodo è offline ma è irrimediabilmente danneggiato.  
  
     Per eseguire il recupero in presenza di un errore del sistema operativo, recuperare il nodo e testare il cluster di failover. Se il failover dell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] non viene eseguito correttamente, è necessario utilizzare il programma di installazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per rimuovere [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dal cluster di failover, apportare le necessarie correzioni, ripristinare il computer, quindi aggiungere nuovamente il nodo riparato all'istanza del cluster di failover.  
  
     L'utilizzo di questa procedura per eseguire il recupero in presenza di un errore del sistema operativo può richiedere tempo. Se è possibile eseguire il recupero in modo più agevole, evitare di utilizzare questa tecnica.  
  
     Per altre informazioni, vedere [Creare un nuovo cluster di failover di SQL Server &#40;programma di installazione&#41;](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md) e [Procedura: Recuperare da un errore dell'istanza del cluster di failover nello scenario 2](recover-from-failover-cluster-instance-failure.md).  
  
## <a name="resolving-common-problems"></a>Risoluzione di problemi comuni  
 Di seguito vengono elencati alcuni problemi comuni relativi all'utilizzo e le rispettive modalità di risoluzione.  
  
### <a name="problem-incorrect-use-of-command-prompt-syntax-to-install-sql-server"></a>Problema: uso non corretto della sintassi del prompt dei comandi per installare SQL Server  
 **Problema 1** : è difficile diagnosticare problemi di installazione quando si usa l'opzione **/qn** dal prompt dei comandi, perché **tale opzione** determina l'eliminazione di tutte le finestre di dialogo dell'interfaccia utente e dei messaggi di errore del programma di installazione. Se l'opzione **/qn** viene specificata, tutti i messaggi del programma di installazione, compresi i messaggi di errore, vengono inseriti nei file di log del programma. Per altre informazioni sui file di log, vedere [Visualizzare e leggere i file di log del programma di installazione di SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
 **Soluzione 1**: usare l'opzione **/qb** anziché l'opzione **/qn**. Se si usa l'opzione **/qb** , per ogni passaggio verrà visualizzata l'interfaccia utente di base, inclusi i messaggi di errore.  
  
### <a name="problem-sql-server-cannot-log-on-to-the-network-after-it-migrates-to-another-node"></a>Problema: SQL Server non riesce ad accedere alla rete dopo la migrazione in un altro nodo  
 **Problema 1:** gli account del servizio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] non riescono a contattare un controller di dominio.  
  
 **Soluzione 1**: controllare la presenza di problemi di rete, ad esempio errori della scheda o problemi DNS, nei log eventi. Verificare che sia possibile effettuare il ping del controller di dominio.  
  
 **Problema 2: le password degli account del servizio** [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] non sono identiche in tutti i nodi del cluster oppure il nodo non riavvia un servizio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] di cui è stata eseguita la migrazione da un nodo con errori.  
  
 **Soluzione 2:** cambiare le password dell'account del servizio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] tramite Gestione configurazione [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. In caso contrario, se si modificano le password dell'account del servizio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in un nodo, sarà necessario modificare le password anche in tutti gli altri nodi. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
### <a name="problem-sql-server-cannot-access-the-cluster-disks"></a>Problema: SQL Server non riesce ad accedere ai dischi del cluster  
 **Problema 1:** il firmware o i driver non sono aggiornati in tutti i nodi.  
  
 **Soluzione 1:** verificare che tutti i nodi usino versioni corrette del firmware e le stesse versioni dei driver.  
  
 **Problema 2:** un nodo non è in grado di recuperare i dischi del cluster di cui è stata eseguita la migrazione da un nodo bloccato in un disco cluster condiviso che utilizza una diversa lettera di unità.  
  
 **Soluzione 2:** le lettere di unità disco dei dischi del cluster devono essere identiche in entrambi i server. In caso contrario, modificare l'installazione originale del sistema operativo e del servizio cluster (MSCS, [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Cluster Service).  
  
### <a name="problem-failure-of-a-sql-server-service-causes-failover"></a>Problema: failover causato da un errore di un servizio SQL Server  
 **Risoluzione:** per impedire che un errore in servizi specifici determini il failover del gruppo [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , configurare i servizi tramite Amministrazione cluster in Windows, come descritto di seguito:  
  
-   Deselezionare la casella di controllo **Influisce sul gruppo** nella scheda **Avanzate** della finestra di dialogo **Proprietà full-text** . Se tuttavia [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] causa un failover, il servizio Ricerca full-text verrà riavviato.  
  
### <a name="problem-sql-server-does-not-start-automatically"></a>Problema: SQL Server non si avvia automaticamente  
 **Risoluzione:** per avviare automaticamente un cluster di failover, usare Amministrazione cluster in MSCS. Impostare il servizio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per l'avvio manuale. Configurare inoltre Amministrazione cluster in MSCS per l'avvio del servizio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Per altre informazioni, vedere [Gestione dei servizi](https://msdn.microsoft.com/library/ms178096\(v=sql.105\).aspx).  
  
### <a name="problem-the-network-name-is-offline-and-you-cannot-connect-to-sql-server-using-tcpip"></a>Problema: il nome rete è offline e non è possibile connettersi a SQL Server tramite TCP/IP  
 **Problema 1:** DNS non è disponibile e la risorsa cluster è impostata in modo da richiedere DNS.  
  
 **Soluzione 1:** risolvere i problemi di DNS.  
  
 **Problema 2:** nella rete è presente un nome duplicato.  
  
 **Soluzione 2:** usare NBTSTAT per individuare il nome duplicato e quindi risolvere il problema.  
  
 **Problema 3:** [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] non esegue la connessione tramite named pipe.  
  
 **Soluzione 3:** usando Gestione configurazione SQL Server, creare un alias per connettersi al computer appropriato tramite named pipe. Se, ad esempio, si usa un cluster con due nodi (**Nodo A** e **Nodo B**) e un'istanza del cluster di failover (**Virtsql**) con un'istanza predefinita, è possibile seguire la procedura seguente per connettersi al server la cui risorsa Nome di rete è offline:  
  
1.  Utilizzare Amministrazione cluster per determinare il nodo in cui è esecuzione il gruppo che contiene l'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . In questo esempio si tratta del nodo **Node A**.  
  
2.  Avviare il servizio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nel computer tramite **net start**. Per altre informazioni sull'uso di **net start**, vedere [Avvio manuale di SQL Server](https://msdn.microsoft.com/library/ms191193\(v=sql.105\).aspx).  
  
3.  Avviare Gestione configurazione SQL Server di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] su **Nodo A**. Visualizzare il nome della pipe sulla quale il server è in attesa. Il nome dovrebbe avere il formato seguente: \\\\.\\$$\VIRTSQL\pipe\sql\query.  
  
4.  Sul computer client, avviare Gestione configurazione SQL Server.  
  
5.  Creare l'alias SQLTEST1 per connettersi alla pipe tramite named pipe. A tale scopo, immettere **Nodo A** come nome del server e modificare il nome della pipe in \\\\.\pipe\\$$\VIRTSQL\sql\query.  
  
6.  Connettersi all'istanza utilizzando l'alias SQLTEST1 come nome del server.  
  
### <a name="problem-sql-server-setup-fails-on-a-cluster-with-error-11001"></a>Problema: impossibile installare SQL Server in un cluster. Errore 11001  
 **Problema:** chiave orfana del Registro di sistema in [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\MSSQL.X\Cluster]  
  
 **Risoluzione:** assicurarsi che l'hive del Registro di sistema MSSQL.X non sia in uso e quindi eliminare la chiave relativa al cluster.  
  
### <a name="problem-cluster-setup-error-the-installer-has-insufficient-privileges-to-access-this-directory-drivemicrosoft-sql-server-the-installation-cannot-continue-log-on-as-an-administrator-or-contact-your-system-administrator"></a>Problema: errore di installazione del cluster: "Privilegi insufficienti per accedere alla directory: \<drive>\Microsoft SQL Server. Accedere come amministratore oppure contattare l'amministratore del sistema"  
 **Problema:** questo errore è causato da un'unità condivisa SCSI non partizionata correttamente.  
  
 **Risoluzione:** usare la procedura seguente per ricreare un'unica partizione nel disco condiviso:  
  
1.  Eliminare la risorsa disco dal cluster.  
  
2.  Eliminare tutte le partizioni presenti nel disco.  
  
3.  Nelle proprietà del disco verificare che si tratta di un disco di base.  
  
4.  Creare un'unica partizione nel disco condiviso, formattare il disco e assegnarvi una lettera di unità.  
  
5.  Aggiungere il disco al cluster tramite Amministrazione cluster (cluadmin).  
  
6.  Eseguire il programma di installazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
### <a name="problem-applications-fail-to-enlist-sql-server-resources-in-a-distributed-transaction"></a>Problema: le applicazioni non riescono a integrare le risorse di SQL Server in una transazione distribuita  
 **Problema:**[!INCLUDE[msCoName](../../../includes/msconame-md.md)] Distributed Transaction Coordinator (MS DTC) non è completamente configurato in Windows, quindi è possibile che le applicazioni non possano visualizzare l'elenco delle risorse [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in una transazione distribuita. Questo problema può interessare server collegati, query distribuite e stored procedure remote che utilizzano transazioni distribuite. Per ulteriori informazioni sulla configurazione di MS DTC, vedere [Before Installing Failover Clustering](../../../sql-server/failover-clusters/install/before-installing-failover-clustering.md).  
  
 **Risoluzione:** per evitare problemi di questo tipo, è necessario abilitare completamente i servizi MS DTC nei server in cui è installato [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e MS DTC è configurato.  
  
 Per abilitare completamente MS DTC, effettuare le operazioni seguenti:  
  
1.  Nel Pannello di controllo aprire **Strumenti di amministrazione** e quindi **Gestione computer**.  
  
2.  Nel riquadro sinistro di Gestione computer espandere **Servizi e applicazioni** e quindi fare clic su **Servizi**.  
  
3.  Nel riquadro destro di Gestione computer fare clic con il pulsante destro del mouse su **Distributed Transaction Coordinator** e quindi scegliere **Proprietà**.  
  
4.  Nella finestra **Distributed Transaction Coordinator** fare clic sulla scheda **Generale** e quindi su **Arresta** per arrestare il servizio.  
  
5.  Nella finestra **Distributed Transaction Coordinator** fare clic sulla scheda **Connessione** e impostare l'account di accesso NT AUTHORITY\NetworkService.  
  
6.  Fare clic su **Applica** e quindi su **OK** per chiudere la finestra **Distributed Transaction Coordinator** . Chiudere la finestra **Gestione computer** . Chiudere la finestra **Strumenti di amministrazione** .  
  
## <a name="using-extended-stored-procedures-and-com-objects"></a>Utilizzo delle stored procedure estese e degli oggetti COM  
 Le stored procedure estese con un clustering di failover devono essere installate in un disco del cluster dipendente da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. In tal modo quando un nodo cede il controllo a un altro nodo mediante il failover, sarà comunque possibile utilizzare le stored procedure estese.  
  
 Se le stored procedure estese utilizzano componenti COM, l'amministratore dovrà registrare i componenti COM in ogni nodo del cluster. Per consentire la creazione dei componenti COM, le informazioni per il caricamento e l'esecuzione dei componenti stessi devono trovarsi nel Registro di sistema del nodo attivo. In caso contrario, le informazioni resteranno nel Registro di sistema del computer in cui i componenti COM sono stati registrati per la prima volta.  
  
## <a name="see-also"></a>Vedere anche  
 [Visualizzare e leggere i file di log del programma di installazione di SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)   
 [Funzionamento delle stored procedure estese](../../../relational-databases/extended-stored-procedures-programming/how-extended-stored-procedures-work.md)   
 [Caratteristiche dell'esecuzione di stored procedure estese](../../../relational-databases/extended-stored-procedures-programming/execution-characteristics-of-extended-stored-procedures.md)  
  
