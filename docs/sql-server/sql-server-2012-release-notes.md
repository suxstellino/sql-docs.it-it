---
title: Note sulla versione di SQL Server 2012 | Microsoft Docs
description: Questo documento descrive i problemi noti di cui è necessario essere a conoscenza prima di installare o risolvere i problemi relativi a Microsoft SQL Server 2012.
ms.prod: sql
ms.technology: release-landing
ms.custom: ''
ms.date: 07/22/2020
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Release Notes, SQL Server
ms.assetid: 9ccb390a-67a9-4593-85ea-2b4c41c4620f
author: rothja
ms.author: jroth
monikerRange: = sql-server-2016
ms.openlocfilehash: e75c713919a53a06c7b54c31f910810f055fac9e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336713"
---
# <a name="sql-server-2012-release-notes"></a>Note sulla versione di SQL Server 2012
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]
Questo documento descrive i problemi noti di cui è necessario essere a conoscenza prima di installare o risolvere i problemi relativi a [Microsoft SQL Server 2012](https://go.microsoft.com/fwlink/?LinkId=238647). Questo documento è disponibile solo online, non nei supporti di installazione, e viene aggiornato periodicamente.  
  
Per informazioni su come installare SQL Server 2012, vedere il file Leggimi di SQL Server 2012 disponibile nei supporti di installazione e nella pagina di download del [file Leggimi](https://download.microsoft.com/download/3/B/D/3BD9DD65-D3E3-43C3-BB50-0ED850A82AD5/ENU/Readme.htm) . Ulteriori informazioni sono disponibili nella [documentazione online di SQL Server](/previous-versions/sql/sql-server-2012/ms130214(v=sql.110)) e nel [forum su SQL Server](https://go.microsoft.com/fwlink/?LinkId=213599).  
  
## <a name="10-before-you-install"></a><a name="Install"></a>1.0 Prima di installare  
Prima di installare [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)], si considerino le informazioni riportate di seguito.  
  
### <a name="11-rules-documentation-for-sql-server-2012-setup"></a>1.1 Documentazione sulle regole relative al programma di installazione di SQL Server 2012  
**Problema:** la configurazione del computer viene convalidata dall'installazione di SQL Server prima del completamento dell'operazione di installazione. Le diverse regole in esecuzione durante l'operazione del programma di installazione di SQL Server vengono acquisite tramite il report Controllo configurazione sistema (SCC, System Configuration Checker). La documentazione relativa a queste regole del programma di installazione non è più disponibile in MSDN Library.  
  
**Soluzione alternativa:** per altre informazioni su queste regole di installazione è possibile fare riferimento al report di controllo della configurazione di sistema. Tramite il controllo della configurazione di sistema viene generato un report contenente una breve descrizione di ogni regola eseguita, nonché lo stato dell'esecuzione. Il report di controllo della configurazione di sistema si trova nel percorso %Programmi%\Microsoft SQL Server\110\Setup Bootstrap\Log\\\<AAAAMMGG_HHMM>\\\.  
  
### <a name="12-adding-a-local-user-account-for-the-distributed-replay-controller-service-might-terminate-setup-unexpectedly"></a>1.2 L'aggiunta di un account utente locale per il servizio Distributed Replay Controller potrebbe causare l'interruzione imprevista dell'installazione  
**Problema:** nella pagina **Distributed Replay Controller** del programma di installazione di SQL Server, quando si tenta di aggiungere un account utente locale per il servizio Distributed Replay Controller, viene visualizzato il messaggio di errore "Errore durante l'installazione di SQL Server" e l'installazione viene interrotta in modo imprevisto.  
  
**Soluzione alternativa:** durante l'installazione di SQL Server, non aggiungere account utente locali tramite "Aggiungi utente corrente" o "Aggiungi...". Dopo l'installazione, aggiungere manualmente un account utente locale effettuando i passaggi seguenti:  
  
1.  Arrestare il servizio SQL Server Distributed Replay Controller  
  
2.  Nel computer controller in cui è installato il servizio relativo, digitare dcomcnfg al prompt dei comandi.  
  
3.  Nella finestra Servizi componenti spostarsi in **Radice console** -> **Servizi componenti** -> **Computer** -> **Risorse del computer** -> **Dconfig** ->**DReplayController**.  
  
4.  Fare clic con il pulsante destro del mouse su **DReplayController**, quindi scegliere **Proprietà**.  
  
5.  Nella scheda **Sicurezza** nella finestra **Proprietà di DReplayController** fare clic su **Modifica** nella sezione **Autorizzazioni di esecuzione e attivazione** .  
  
6.  Concedere all'account utente locale le autorizzazioni di **attivazione locale e remota** , quindi scegliere **OK**.  
  
7.  Fare clic su **Modifica** nella sezione Autorizzazioni di accesso e concedere all'account utente locale le **autorizzazioni di accesso locale e remoto** , quindi scegliere **OK**.  
  
8.  Fare clic su **OK** per chiudere la finestra **Proprietà di DReplayController** .  
  
9. Nel computer controller aggiungere l'account utente locale al gruppo **Distributed COM Users** .  
  
10. Avviare il servizio SQL Server Distributed Replay Controller.  
  
### <a name="13-sql-server-setup-might-fail-while-trying-to-start-the-sql-server-browser-service"></a>1.3 L'installazione di SQL Server potrebbe non venir completata nel tentativo di avviare il servizio SQL Server Browser  
**Problema:** l'installazione di SQL Server potrebbe non riuscire quando si tenta di avviare il servizio SQL Server Browser, con errori simili al seguente:  
  
```
The following error has occurred:  
Service 'SQLBrowser' start request failed. Click 'Retry' to retry the failed action, or click 'Cancel' to cancel this action and continue setup.
```
  
o  
  
```
The following error has occurred:  
SQL Server Browser configuration for feature 'SQL_Browser_Redist_SqlBrowser_Cpu32' was cancelled by user after a previous installation failure. The last attempted step: Starting the SQL Server Browser service 'SQLBrowser', and waiting for up to '900' seconds for the process to complete.
```
  
**Soluzione alternativa:** può verificarsi quando l'installazione del motore di SQL Server o di Analysis Services non riesce. Per risolvere il problema, vedere i log del programma di installazione di SQL Server e risolvere i problemi relativi al motore di SQL Server e ad Analysis Services. Per altre informazioni, vedere la pagina relativa alla visualizzazione e alla lettura dei file di log del programma di installazione di SQL Server. Per altre informazioni, vedere [Visualizzare e leggere i file di log del programma di installazione di SQL Server](../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
### <a name="14-sql-server-2008-2008-r2-analysis-services-failover-cluster-upgrade-to-sql-server-2012-might-fail-after-renaming-the-network-name"></a>1.4 L'aggiornamento del cluster di failover di SQL Server 2008/2008 R2 Analysis Services a SQL Server 2012 potrebbe non venir completato dopo la ridenominazione del nome di rete  
**Problema:** dopo la modifica del nome di rete di un'istanza del cluster di failover di Microsoft SQL Server 2008 o 2008 R2 Analysis Services tramite lo strumento di amministrazione cluster di Windows, l'operazione di aggiornamento potrebbe non riuscire.  
  
**Soluzione alternativa:** per risolvere questo problema, aggiornare la voce del Registro di sistema ClusterName seguendo le istruzioni nella sezione sulla risoluzione di [questo articolo della Knowledge Base](https://support.microsoft.com/kb/955784).  
  
### <a name="15-installing-sql-server-2012-on-windows-server-2008-r2-server-core-service-pack-1"></a>1.5 Installazione di SQL Server 2012 in Windows Server 2008 R2 Server Core Service Pack 1  
È possibile installare SQL Server in Windows Server 2008 R2 Server Core SP1 con le limitazioni seguenti:  
  
-   Microsoft SQL Server 2012 non supporta l'installazione eseguita tramite l'Installazione guidata nel sistema operativo Server Core. In caso di installazione in Server Core, il programma di installazione di SQL Server prevede il supporto della modalità non interattiva completa tramite il parametro /Q o della modalità non interattiva semplice tramite il parametro /QS.  
  
-   In un computer con il sistema operativo Windows Server 2008 R2 Server Core SP1 l'aggiornamento di una versione precedente di SQL Server a Microsoft SQL Server 2012 non è supportato.  
  
-   In un computer con il sistema operativo Windows Server 2008 R2 Server Core SP1 l'installazione di una versione a 32 bit di Microsoft SQL Server 2012 non è supportata.  
  
-   In un computer con il sistema operativo Windows Server 2008 R2 Server Core SP1 non è possibile installare Microsoft SQL Server 2012 side-by-side con versioni precedenti di SQL Server.  
  
-   Non tutte le funzionalità di SQL Server 2012 sono supportate nel sistema operativo Server Core. Per altre informazioni sulle funzionalità supportate e sull'installazione di SQL Server 2012 in Server Core, vedere [Installare SQL Server 2012 in Server Core](/previous-versions/sql/sql-server-2012/hh231669(v=sql.110)).  
  
### <a name="16-semantic-search-requires-you-to-install-an-additional-dependency"></a>1.6 È necessario installare un'ulteriore dipendenza per la Ricerca semantica  
**Problema:** la ricerca semantica statistica ha un altro prerequisito, il database di statistiche lingua semantica, che non viene installato dal programma di installazione di SQL Server.  
  
**Soluzione alternativa:** per configurare il database di statistiche lingua semantica come prerequisito per l'indicizzazione semantica, eseguire la procedura seguente:  
  
1.  Individuare ed eseguire il pacchetto di Windows Installer denominato SemanticLanguageDatabase.msi sui supporti di installazione di SQL Server al fine di estrarre il database. Per SQL Server 2012 Express, scaricare il database di Semantic Language Statistics dall'[Area download Microsoft](https://www.microsoft.com/download/details.aspx?id=52681) (https://www.microsoft.com/download/details.aspx?id=52681) ) ed eseguire il pacchetto Windows Installer.  
  
2.  Spostare il database in una cartella di dati adeguata. Se si decide di lasciare il database nella posizione predefinita, è necessario modificare le relative autorizzazioni prima di poter collegarlo correttamente.  
  
3.  Collegare il database estratto.  
  
4.  Registrare il database chiamando la stored procedure **sp_fulltext_semantic_register_language_statistics_db** e specificando il nome assegnato al database al momento del collegamento.  
  
Una volta completate le attività, verrà visualizzato il seguente messaggio di errore quando si tenta di creare un indice semantico.  
  
```
Msg 41209, Level 16, State 3, Line 1  
A semantic language statistics database is not registered. Full-text indexes using 'STATISTICAL_SEMANTICS' cannot be created or populated.
```
  
### <a name="17-installation-prerequisite-handling-during-sql-server-2012-setup"></a>1.7 Gestione dei prerequisiti dell'installazione durante l'installazione di SQL Server 2012  
Di seguito viene descritto il comportamento dell'installazione dei prerequisiti durante l'installazione di SQL Server 2012:  
  
-   L'installazione di SQL Server 2012 è supportata solo in Windows 7 SP1 o Windows Server 2008 R2 SP1. Tramite il programma di installazione, tuttavia, non viene impedita l'installazione di SQL Server 2012 in Windows 7 o Windows Server 2008 R2.  
  
-   .NET Framework 3.5 SP1 è un requisito necessario per SQL Server 2012 quando vengono selezionati il motore di database, la replica, Master Data Services, Reporting Services, Data Quality Services (DQS) o SQL Server Management Studio e non viene più installato durante l'installazione di SQL Server.  
  
    -   Se si esegue il programma di installazione in un computer con sistema operativo Windows Vista SP2 o Windows Server 2008 SP2 e .NET Framework 3.5 SP1 non è installato, per installare SQL Server è necessario scaricare e installare .NET Framework 3.5 SP1 prima di procedere con l'installazione di SQL Server. È possibile scaricare .NET Framework 3.5 SP1 da Windows Update o direttamente da [qui](https://www.microsoft.com/download/details.aspx?id=25150). Prima di installare SQL Server è necessario scaricare e installare .NET Framework 3.5 SP1 per evitare l'interruzione dell'operazione.  
  
    -   Se si esegue il programma di installazione in un computer con sistema operativo Windows 7 SP1 o Windows Server 2008 R2 SP1, è necessario abilitare .NET Framework 3.5 SP1 prima di installare SQL Server 2012.  
  
        **Per abilitare .NET Framework 3.5 SP1 in Windows Server 2008 R2 SP1, usare uno dei metodi seguenti:**  
  
        Metodo 1: usare Server Manager  
  
        1.  In Server Manager fare clic su **Aggiungi funzionalità** per visualizzare un elenco di funzionalità disponibili.  
  
        2.  Nell'interfaccia **Seleziona funzionalità** espandere la voce **Funzionalità di .NET Framework 3.5.1** .  
  
        3.  Dopo aver espanso la voce **Funzionalità di .NET Framework 3.5.1**, vengono visualizzate due caselle di controllo. Una riguarda .NET Framework 3.5.1, l'altra l'attivazione di WCF. Selezionare **.NET Framework 3.5.1** e quindi fare clic su **Avanti**. È possibile installare le funzionalità di .NET Framework 3.5.1 solo se vengono installati anche i servizi ruolo e le funzionalità necessari.  
  
        4.  In **Conferma selezioni per l'installazione** rivedere le selezioni e scegliere Installa.  
  
        5.  Attendere il completamento del processo di installazione, quindi scegliere **Chiudi**.  
  
        Metodo 2: uso di Windows PowerShell  
  
        1.  Fare clic su **Start** | **Tutti i programmi** | **Accessori**.  
  
        2.  Espandere **Windows PowerShell**, fare clic con il pulsante destro del mouse su **Windows PowerShell** e quindi scegliere **Esegui come amministratore**. Scegliere **Sì** nella casella **Controllo account utente** .  
  
        3.  Al prompt dei comandi di PowerShell digitare i comandi seguenti e premere INVIO dopo ogni comando:  
  
            ```  
            Import-Module ServerManager  
            Add-WindowsFeature as-net-framework  
            ```  
  
        **Per abilitare .NET Framework 3.5 SP1 in Windows 7 SP1, usare il metodo seguente:**  
  
        1.  Fare clic sul pulsante **Start** | **Pannello di controllo** | **Programmi e funzionalità** e quindi scegliere **Attivazione o disattivazione delle funzionalità Windows**. Se viene richiesta una password di amministratore o una conferma, digitare la password o fornire la conferma.  
  
        2.  Per abilitare **Microsoft .NET Framework 3.5.1**, selezionare la casella di controllo accanto alla funzionalità. Per disattivare una funzionalità di Windows, deselezionare la casella di controllo.  
  
        3.  Fare clic su **OK**.  
  
        **Per abilitare .NET Framework 3.5 SP1 usare Gestione e manutenzione immagini distribuzione (DISM.exe):**  
  
        È possibile abilitare .NET Framework 3.5 SP1 usando Gestione e manutenzione immagini distribuzione (DISM.exe). Per altre informazioni sull'abilitazione delle funzionalità di Windows online, vedere [Abilitare o disabilitare funzionalità di Windows online](/previous-versions/windows/it-pro/windows-7/dd744582(v=ws.10)). Per abilitare .NET Framework 3.5 SP1, seguire le istruzioni riportate di seguito:  
  
        1.  Al prompt dei comandi digitare il comando riportato di seguito per un elenco delle funzionalità disponibili nel sistema operativo.  
  
            ```  
            sm /online /Get-Features  
            ```  
  
        2.  Facoltativo: al prompt dei comandi digitare il comando riportato di seguito per un elenco delle informazioni sulla funzionalità specifica cui si è interessati.  
  
            ```  
            Dism /online /Get-FeatureInfo /FeatureName:NetFx3  
            ```  
  
        3.  Digitare il comando riportato di seguito per abilitare Microsoft .NET Framework 3.5.1.  
  
            ```  
            Dism /online /Enable-Feature /FeatureName:NetFx3  
            ```  
  
-   .NET Framework 4 è un requisito necessario per SQL Server 2012. L'installazione di SQL Server prevede l'installazione di .NET Framework 4 durante la fase di installazione delle funzionalità.  
  
    SQL Server 2012 Express non prevede l'installazione di .NET Framework 4 quando si esegue l'installazione in un sistema operativo Windows Server 2008 R2 SP1 Server Core. Quando si installa SQL Server 2012 Express (solo database), .NET Framework 4 non è necessario se è già installato .NET Framework 3.5 SP1. Quando .NET Framework 3.5 SP1 non è installato o quando si installa SQL Server 2012 Management Studio Express, SQL Server 2012 Express with Tools o SQL Server 2012 Express with Advanced Services, è necessario installare .NET Framework 4 prima di installare SQL Server2012 Express in un sistema operativo Windows Server 2008 R2 SP1 Server Core.  
  
-   Per garantire la corretta installazione del componente Visual Studio, per SQL Server è necessaria l'installazione di un aggiornamento. Tramite il programma di installazione di SQL Server viene verificata la presenza di questo aggiornamento, quindi viene richiesto di scaricarlo e installarlo prima che sia possibile procedere all'installazione di SQL Server. Per evitare l'interruzione dell'installazione di SQL Server, è possibile scaricare e installare l'aggiornamento prima di avviare il programma di installazione, come illustrato di seguito (o installare tutti gli aggiornamenti per .NET Framework 3.5 SP1 disponibili in Windows Update):  
  
    -   Se si installa SQL Server 2012 in un computer con il sistema operativo Windows Vista SP2 o Windows Server 2008 SP2, è possibile ottenere l'aggiornamento necessario da [qui](https://support.microsoft.com/?kbid=956250).  
  
    -   Se si installa SQL Server 2012 in un computer con il sistema operativo Windows 7 SP1 o Windows Server 2008 R2 SP1, questo aggiornamento è già installato nel computer.  
  
-   Windows PowerShell 2.0 è un prerequisito per l'installazione dei componenti del motore di database di SQL Server 2012 e SQL Server Management Studio, tuttavia il programma di installazione di SQL Server non ne prevede più l'installazione. Se PowerShell 2.0 non è già installato nel computer, è possibile abilitarlo seguendo le istruzioni disponibili nella pagina [Framework di gestione Windows](https://mskb.pkisolutions.com/kb/968929) . La modalità per ottenere Windows PowerShell 2.0 dipende dal sistema operativo in esecuzione:  
  
    -   Windows Server 2008: Windows PowerShell 1.0 è una funzionalità e può essere aggiunta. Vengono scaricate e installate le versioni di Windows PowerShell 2.0 (di fatto come patch del sistema operativo).  
  
    -   Windows 7/Windows Server 2008 R2: Windows PowerShell 2.0 viene installato per impostazione predefinita.  
  
-   Se si prevede di usare le funzionalità di SQL Server 2012 in un ambiente SharePoint, è necessario disporre di SharePoint Server 2010 Service Pack 1 (SP1) e dell'aggiornamento cumulativo di agosto. È necessario installare SP1, l' [aggiornamento cumulativo di agosto](https://blogs.technet.com/b/stefan_gossner/archive/2010/09/02/august-2010-cumulative-update-for-sharepoint-has-been-released.aspx)di SharePoint e applicare patch alla server farm prima di aggiungere le funzionalità di SQL Server 2012 alla farm. Questo requisito si applica alle funzionalità di SQL Server 2012 seguenti: utilizzo di un'istanza del motore di database come server di database della farm, configurazione di PowerPivot per SharePoint o distribuzione di Reporting Services in modalità SharePoint.  
  
### <a name="18-supported-operating-systems-for-sql-server-2012"></a>1.8 Sistemi operativi supportati per SQL Server 2012  
SQL Server 2012 è supportato nei sistemi operativi Windows Vista SP2, Windows Server 2008 SP2, Windows 2008 R2 SP1 e Windows 7 SP1.  
  
### <a name="19-sync-framework-is-not-included-in-the-installation-package"></a>1.9 Sync Framework non è incluso nel pacchetto di installazione.  
**Problema:** Sync Framework non è incluso nel pacchetto di installazione di SQL Server 2012.  
  
**Soluzione alternativa:** scaricare la versione appropriata di Sync Framework da [questa pagina dell'Area download Microsoft](https://www.microsoft.com/download/en/details.aspx?displaylang=en&id=23217).  
  
### <a name="110-if-visual-studio-2010-service-pack-1-is-uninstalled-the-sql-server-2012-instance-must-be-repaired-to-restore-certain-components"></a>1.10 Se Visual Studio 2010 Service Pack 1 viene disinstallato, è necessario ripristinare l'istanza di SQL Server 2012 per ripristinare alcuni componenti  
**Issue:** l'installazione di [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] dipende da alcuni componenti di Visual Studio 2010 Service Pack 1. Se si disinstalla Service Pack 1, alcuni dei componenti condivisi vengono riportati alle rispettive versioni originali, mentre altri vengono completamente rimossi dal computer.  
  
**Soluzione alternativa:** ripristinare l'istanza di [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] dal percorso di installazione di rete o dai supporti originali.  
  
1.  Avviare il programma di installazione di SQL Server (setup.exe) dai supporti di installazione di SQL Server.  
  
2.  Dopo la verifica del sistema e dei prerequisiti, viene visualizzata la pagina **Centro installazione SQL Server** .  
  
3.  Fare clic su **Manutenzione** nell'area di navigazione a sinistra, quindi fare clic su **Ripristina** per avviare l'operazione di ripristino. Se Centro installazione è stato avviato usando il menu **Start** , a questo punto sarà necessario fornire il percorso del supporto di installazione.  
  
4.  Verranno eseguite la regola di supporto dell'installazione e le routine dei file per garantire che nel sistema siano installati i prerequisiti e che il computer passi le regole di convalida dell'installazione. Fare clic su **OK** o **Installa** per continuare.  
  
5.  Nella pagina **Seleziona istanza** selezionare l'istanza da ripristinare e quindi fare clic su **Avanti** per continuare.  
  
6.  Verranno eseguite le regole di ripristino per convalidare l'operazione. Scegliere **Avanti** per continuare.  
  
7.  La pagina **Ripristino** indica che l'operazione può essere eseguita. Per continuare fare clic su **Ripristina**.  
  
8.  Nella pagina **Stato del ripristino** viene mostrato lo stato dell'operazione di ripristino. Nella pagina **Operazione completata** è indicato che l'operazione è stata completata.  
  
Per altre informazioni su come ripristinare un'istanza di SQL Server, vedere [Ripristino di un'installazione non riuscita di SQL Server 2012](../database-engine/install-windows/repair-a-failed-sql-server-installation.md).  
  
### <a name="111-an-instance-of-sql-server-2012-might-fail-after-an-os-upgrade"></a>1.11 Un'istanza di SQL Server 2012 potrebbe non riuscire dopo un aggiornamento del sistema operativo  
**Problema:** un'istanza di SQL Server 2012 potrebbe non riuscire con l'errore seguente dopo l'aggiornamento del sistema operativo a Windows 7 SP1 da Windows Vista.  
  
`Setup has detected that the .NET Framework version 4 needs to be repaired. Do not restart your computer until Setup is complete.`  
  
**Soluzione alternativa**: ripristinare l'installazione di .NET Framework 4 dopo l'aggiornamento del sistema operativo. Per altre informazioni, vedere [Come ripristinare un'installazione esistente di .NET Framework](https://support.microsoft.com/kb/306160).  
  
### <a name="112-sql-server-edition-upgrade-requires-a-restart"></a>1.12 Per l'aggiornamento dell'edizione di SQL Server è richiesto un riavvio  
**Problema**: quando si esegue l'aggiornamento dell'edizione di un'istanza di SQL Server 2012, alcune delle funzionalità associate alla nuova edizione potrebbero non essere attivate immediatamente.  
  
**Soluzione alternativa**: riavviare il computer dopo l'aggiornamento dell'edizione di un'istanza di SQL Server 2012. Per altre informazioni sugli aggiornamenti supportati in SQL Server 2012, vedere [Aggiornamenti di versione ed edizione supportati](../database-engine/install-windows/supported-version-and-edition-upgrades-2017.md).  
  
### <a name="113-database-with-read-only-filegroup-or-files-cannot-be-upgraded"></a>1.13 Impossibile aggiornare un database con file o filegroup di sola lettura  
**Problema**:  non è possibile aggiornare un database collegandolo o ripristinandolo dal backup se il database o i relativi file/filegroup sono di sola lettura.  Viene restituito l'errore 3415.  Questo problema si verifica anche quando si esegue un aggiornamento sul posto di un'istanza di SQL Server, cioè quando si tenta di sostituire un'istanza esistente di SQL Server installando SQL Server 2012 e uno o più database esistenti sono di sola lettura.  
  
**Soluzione alternativa:** prima dell'aggiornamento, assicurarsi che il database e i relativi file/filegroup siano impostati sulla modalità di lettura/scrittura.  
  
### <a name="114-reinstalling-an-instance-of-sql-server-failover-custer-fails-if-you-use-the-same-ip-address"></a>1.14 La reinstallazione di un'istanza del cluster di failover di SQL Server non riesce se si usano lo stesso indirizzo IP  
**Problema:** se si specifica un indirizzo IP non corretto durante l'installazione di un'istanza del cluster di failover di SQL Server, l'installazione non riesce. Dopo la disinstallazione dell'istanza con errori, e in caso di tentativo di reinstallazione dell'istanza del cluster di failover di SQL Server con lo stesso nome istanza, e indirizzo IP corretto, l'installazione non viene completata. L'errore è dovuto al gruppo di risorse duplicate lasciato dall'installazione precedente.  
  
**Soluzione alternativa:** per risolvere il problema, usare un nome istanza diverso durante la reinstallazione oppure eliminare manualmente il gruppo di risorse prima della reinstallazione. Per altre informazioni, vedere la pagina relativa all' [aggiunta o alla rimozione di nodi in un cluster di failover di SQL Server](failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md).  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="20-analysis-services"></a><a name="AS"></a>2.0 Analysis Services  
  
### <a name="21-sql-editor-and-as-editor-cannot-connect-to-their-respective-server-instances-in-the-same-ssms-instance"></a>2.1 Non è possibile effettuare la connessione dell'editor SQL e di quello AS alle rispettive istanze del server nella stessa istanza di SSMS  
**Problema:** non è possibile eseguire la connessione a un server Analysis Services usando l'editor MDX/DMX quando l'editor SQL è già connesso.  
  
Quando si usano SQL Server Management Studio 2012 (SSMS), se un file con estensione sql viene aperto nell'editor e viene connesso a un'istanza di SQL Server, un file MDX o DMX aperto nella stessa istanza di SSMS non può essere connesso a un'istanza del server AS. Analogamente, se un file MDX o DMX è già aperto nell'editor in SSMS ed è connesso a un'istanza del server AS, un file con estensione sql aperto nella stessa istanza di SSMS non può essere connesso a un'istanza di SQL Server.  
  
**Soluzione alternativa**:  usare una delle opzioni seguenti per risolvere questo problema.  
  
-   Avviare un'altra istanza di SSMS per aprire il file MDX/DMX.  
  
-   Disconnettere l'editor SQL, quindi connettere l'editor MDX/DMX a un server AS.  
  
### <a name="22-cannot-create-or-open-tabular-projects-when-builtinadministrators-group-name-cannot-be-resolved"></a>2.2 Non è possibile creare o aprire progetti tabulari se il nome del gruppo BUILTIN\Administrators non può essere risolto  
**Problema:** per poter creare o aprire progetti tabulari, è necessario essere amministratore del server di database dell'area di lavoro. È possibile aggiungere un utente al gruppo di amministratori del server, aggiungendo il nome utente o il gruppo di utenti. Se si fa parte del gruppo BUILTIN\Administrator, non è possibile creare o modificare i file BIM a meno che non si crei un join del server di database dell'area di lavoro al dominio dal quale è stato inizialmente reso disponibile. Se si apre o crea il file BIM, l'operazione non riuscirà e verrà visualizzato il seguente messaggio di errore:  
  
`"The BIM file cannot be opened. The server connected to is not valid. Reason: You are not an administrator of server [server name]."`  
  
**Soluzioni alternative:**  
  
-   Creare nuovamente un join del server di database dell'area di lavoro e del computer SQL Server Data Tools (SSDT) al dominio.  
  
-   Se il server di database dell'area di lavoro e/o i computer SSDT non condividono sempre lo stesso dominio, aggiungere nomi utente singoli anziché il gruppo BUILTIN\Administrators come amministratori del server di database dell'area di lavoro.  
  
### <a name="23-ssis-components-for-as-tabular-models-do-not-work-as-expected"></a>2.3 I componenti SSIS per i modelli tabulari AS non funzionano come previsto  
I componenti SQL Server Integration Services (SSIS) per Analysis Services (AS) non funzionano come previsto per i modelli tabulari. Di seguito sono riportati i problemi noti che possono verificarsi quando si tenta di scrivere un pacchetto SSIS perché funzioni con modelli tabulari.  
  
**Problema:** la gestione connessione AS non può usare un modello tabulare nella stessa soluzione come origine dati.  
  
**Soluzione alternativa:** è necessario connettersi esplicitamente al server AS prima di configurare l'attività Elaborazione Analysis Services o Esegui DDL Analysis Services.  
  
Si verificano problemi con l'attività Elaborazione Analysis Services quando si lavora con modelli tabulari:  
  
**Problema:** invece di database, tabelle e partizioni, vengono visualizzati cubi, gruppi di misure e dimensioni. Si tratta di un limite dell'attività.  
  
**Soluzione alternativa:** è comunque possibile elaborare un modello tabulare usando la struttura a cubi/gruppi di misure/dimensioni.  
  
**Problema:** alcune opzioni di elaborazione supportate da AS in esecuzione in modalità tabulare non vengono esposte nell'attività Elaborazione Analysis Services, ad esempio Elabora deframmentazione.  
  
**Soluzione alternativa:** usare l'attività Esegui DDL Analysis Services invece di eseguire uno script XMLA in cui è contenuto il comando ProcessDefrag.  
  
**Problema:** alcune opzioni di configurazione nello strumento non sono applicabili. Evitare, ad esempio, di usare "Oggetti relativi a processi" quando si elaborano partizioni. Nell'opzione di configurazione "Elaborazione parallela" è incluso un messaggio di errore non valido in cui viene indicato che l'elaborazione parallela non è supportata nella SKU standard.  
  
**Soluzione alternativa:** nessuno  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="30-books-online"></a><a name="BOL"></a>3.0 Documentazione online  
  
### <a name="31-help-viewer-for-sql-server-crashes-in-environments-configured-to-run-only-ipv6"></a>3.1 Il visualizzatore della Guida di SQL Server si arresta in modo anomalo in ambienti configurati per eseguire solo IPv6  
**Problema**: se l'ambiente è configurato per eseguire solo IPv6, il visualizzatore della Guida di SQL Server 2012 verrà arrestato in modo anomalo e verrà visualizzato il messaggio di errore seguente:  
  
`HelpLibAgent.exe has stopped working.`  
  
> [!IMPORTANT]  
> La stessa operazione si applica a tutti gli ambienti in esecuzione con solo IPv6 abilitato. Gli ambienti in cui IPv4 (e IPv4 con IPv6) è abilitato non sono interessati.  
  
**Soluzione alternativa**: per evitare questo problema, abilitare IPv4 o usare la procedura riportata di seguito per aggiungere una voce di registro e creare un ACL per abilitare il visualizzatore della Guida per IPv6:  
  
1.  Creare una chiave del Registro di sistema con il nome "IPv6" e un valore pari a "1 (DWORD(32 bit))" in HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Help\v1.0.  
  
2.  Impostare gli ACL di sicurezza per la porta per IPv6 eseguendo il comando riportato di seguito da una finestra CMD in modalità amministratore:  
  
    ```  
    netsh http add urlacl url=https://[::1]:47873/help/ sddl=D:(A;;GX;;;WD)  
    ```  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="40-data-quality-services"></a><a name="DQS"></a>4.0 Data Quality Services  
  
### <a name="41-dqs-not-supported-in-a-cluster"></a>4.1. DQS non è supportato in un cluster  
**Problema:** DQS non è supportato nell'installazione di un cluster di SQL Server. Se si sta installando un'istanza cluster di SQL Server, è necessario non selezionare le caselle di controllo **Data Quality Services** e **Data Quality Client** nella pagina **Selezione funzionalità** . Se queste caselle sono state selezionate durante l'installazione dell'istanza cluster ed è stata completata l'installazione di Data Quality Server eseguendo il file DQSInstaller.exe, DQS verrà installato in questo nodo, ma non sarà disponibile in nodi aggiuntivi quando si aggiungeranno ulteriori nodi al cluster e quindi non funzionerà nei nodi aggiuntivi.  
  
**Soluzione alternativa:** installare l'aggiornamento cumulativo 1 di SQL Server 2012 per risolvere questo problema. Per istruzioni, vedere [https://support.microsoft.com/kb/2674817](https://support.microsoft.com/kb/2674817).  
  
### <a name="42-to-reinstall-data-quality-server-delete-the-dqs-objects-after-uninstalling-data-quality-server"></a>4.2 Per installare di nuovo Data Quality Server, eliminare gli oggetti DQS dopo aver disinstallato Data Quality Server  
**Problema:** se si disinstalla Data Quality Server, gli oggetti DQS (database DQS, account di accesso DQS e una stored procedure DQS) non vengono eliminati dall'istanza di SQL Server.  
  
**Soluzione alternativa:** per reinstallare Data Quality Server nello stesso computer e nella stessa istanza di SQL Server, è necessario eliminare manualmente gli oggetti DQS dall'istanza di SQL Server. È inoltre necessario eliminare i file dei database DQS (DQS_MAIN, DQS_PROJECTS e DQS_STAGING_DATA) dalla cartella C:\Programmi\Microsoft SQL Server\MSSQL11.<Istanza_SQL_Server>\MSSQL\DATA sul computer, prima di installare nuovamente Data Quality Server. In caso contrario, l'installazione del server Data Quality non verrà completata correttamente. Spostare i file dei database invece di eliminarli se si desidera mantenere dati, quali Knowledge Base o progetti Data Quality. Per altre informazioni sulla rimozione di oggetti DQS dopo il completamento del processo di disinstallazione, vedere [Rimuovere oggetti server Data Quality Services](./install/remove-data-quality-server-objects.md).  
  
### <a name="43-indication-of-a-terminated-knowledge-discovery-or-interactive-cleansing-activity-is-delayed"></a>4.3 Indicazione di individuazione informazioni terminata o ritardo nell'attività di pulizia interattiva  
**Problema:** se un amministratore termina un'attività nella schermata Monitoraggio attività, un utente interattivo che sta eseguendo operazioni di individuazione informazioni, gestione del dominio o un'attività di pulizia interattiva non riceverà alcuna indicazione che la propria attività è stata terminata finché non procede all'operazione successiva.  
  
**Soluzione alternativa:** nessuno  
  
### <a name="44-a-cancel-operation-discards-work-from-multiple-activities"></a>4.4 Le operazioni provenienti da più attività vengono ignorate da un'operazione di annullamento  
**Problema:** se si fa clic su **Annulla** quando si esegue un'attività di individuazione informazioni o gestione di dominio e vi sono anche altre attività in esecuzione, che sono state completate precedentemente ma senza che sia stata eseguita un'operazione di pubblicazione, tutto ciò che è stato eseguito sin dall'ultima pubblicazione di tutte le attività, non solo quelle attuali, verrà annullato.  
  
**Soluzione alternativa:** per evitare questo problema, pubblicare tutto ciò che si vuole mantenere nella knowledge base prima di iniziare una nuova attività.  
  
### <a name="45-controls-do-not-scale-properly-on-large-font-sizes"></a>4.5 I controlli non scalano correttamente con dimensioni del carattere grandi  
**Problema:** se si modifica la dimensione del testo in "Grande - 150%" (in Windows Server 2008 o Windows 7) o si modifica l'impostazione personalizzata DPI in 200% (in Windows 7), i pulsanti **Annulla** e **Crea** nella pagina **Nuova Knowledge Base** non saranno accessibili.  
  
**Soluzione alternativa:** Per risolvere il problema, impostare dimensioni inferiori per il tipo di carattere.  
  
### <a name="46-screen-resolution-of-800x600-is-not-supported"></a>4.6 La risoluzione dello schermo di 800x600 non è supportata  
**Problema:** l'applicazione Data Quality Client non viene visualizzata correttamente se la risoluzione dello schermo è impostata su 800x600.  
  
**Soluzione alternativa:** per risolvere il problema, impostare la risoluzione dello schermo su un valore più alto.  
  
### <a name="47-map-bigint-column-in-the-source-data-to-a-decimal-domain-to-prevent-data-loss"></a>4.7 Eseguire il mapping della colonna Bigint nei dati di origine a un dominio decimale per evitare la perdita dei dati  
**Problema:** se una colonna nei dati di origine è del tipo di dati **bigint**, è necessario eseguire il mapping della colonna a un dominio del tipo di dati **decimal** anziché **integer** in DQS. Questo perché il tipo di dati **decimal** rappresenta un intervallo di valori più ampio rispetto al tipo di dati **int** , pertanto può contenere valori più grandi.  
  
### <a name="48-nvarcharmax-and-varcharmax-data-types-are-not-supported-in-the-dqs-cleansing-component-in-integration-services"></a>4.8 I tipi di dati NVARCHAR(MAX) e VARCHAR(MAX) non sono supportati nel componente DQS Cleansing in Integration Services.  
**Problema:** le colonne di dati dei tipi di dati **nvarchar(max)** e **varchar(max)** non sono supportate nel componente DQS Cleansing in Integration Services. Queste colonne di dati non sono quindi disponibili per il mapping nella scheda Mapping dell'Editor trasformazione DQS Cleansing e non possono pertanto essere pulite.  
  
**Soluzione alternativa:** prima di elaborare queste colonne di dati con il componente DQS Cleansing, è necessario convertirle nel tipo di dati **DT_STR** o **DT_WSTR** usando la trasformazione Conversione dati.  
  
### <a name="49-the-item-to-run-dqsinstallerexe-on-the-start-menu-is-overwritten-on-new-sql-server-instance-installation"></a>4.9 La voce necessaria per eseguire DQSInstaller.exe nel menu Start viene sovrascritta nella nuova istallazione dell'istanza di SQL Server  
**Problema:** se si sceglie di installare Data Quality Services in un'istanza di SQL Server, viene creata una voce nel menu **Start** nel gruppo di programmi **Data Quality Services** denominata **Programma di installazione di Data Quality Server** dopo aver completato l'installazione di SQL Server. Tuttavia, se si installano più istanze di SQL Server nello stesso computer, nel menu **Start** è ancora presente una sola voce **Programma di installazione di Data Quality Server** . Fare clic su questa voce per eseguire il file DQSInstaller.exe nell'istanza di SQL Server installata più di recente.  
  
### <a name="410-activity-monitoring-displays-incorrect-status-for-failed-integration-services-cleansing-activities"></a>4.10 Nella schermata Monitoraggio attività viene visualizzato uno stato non corretto per le attività di pulizia non riuscite di Integration Services  
Nella colonna **Stato corrente** della schermata Monitoraggio attività viene erroneamente visualizzato lo stato **Operazione completata** anche per le attività di pulizia non riuscite di Integration Services.  
  
### <a name="411-schema-name-is-not-displayed-as-part-of-tableview-name"></a>4.11 Il nome dello schema non viene visualizzato come parte del nome della tabella/vista  
Quando si seleziona un'origine dati di SQL Server in una delle attività DQS durante la fase di mapping nel client Data Quality, l'elenco di tabelle e viste viene visualizzato senza il nome dello schema. Di conseguenza, se sono presenti più tabelle o viste con lo stesso nome, ma con schema diverso, sarà possibile distinguerle solo visualizzando l'anteprima dei dati oppure selezionandole e facendo riferimento ai campi disponibili per il mapping.  
  
### <a name="412-issue-with-cleansing-output-and-export-if-data-source-is-mapped-to-a-composite-domain-containing-a-child-domain-of-date-type"></a>4.12 Problema con l'output di DQS Cleansing e con l'esportazione se per l'origine dati viene eseguito il mapping a un dominio composito contenente un dominio figlio del tipo Date  
In un progetto DQS se è stato eseguito il mapping di un campo nei dati di origine a un dominio composito contenente un dominio figlio del tipo di dati Date, l'output del dominio figlio nel risultato della pulizia presenta un formato data non corretto e l'esportazione nel database non viene completata.  
  
### <a name="413-error-when-mapping-to-an-excel-sheet-that-contains-a--semicolon-in-its-name"></a>4.13 Errore durante il mapping a un foglio di Excel contenente un punto e virgola (;) nel relativo nome  
**Problema:** nella pagina **Mappa** di tutte le attività DQS in Data Quality Client, se si esegue il mapping al foglio di Excel di origine contenente un punto e virgola (;) nel nome, viene visualizzato un messaggio di eccezione non gestita quando si sceglie **Avanti** nella pagina **Mappa**.  
  
**Soluzione alternativa:** rimuovere il punto e virgola (;) dal nome del foglio nel file di Excel contenente i dati di origine di cui eseguire il mapping e riprovare.  
  
### <a name="414-issue-with-date-or-datetime-values-in-unmapped-source-fields-in-excel-during-cleansing-and-matching"></a>4.14 Problema con i valori Date o DateTime nei campi di origine di cui non è stato eseguito il mapping in Excel durante la pulizia e la corrispondenza  
**Problema**: se i dati di origine sono in formato Excel e per i campi di origine contenenti valori di tipo **Date** o **DateTime** non è stato eseguito il mapping, durante le attività di pulizia e corrispondenza si verifica quanto segue:  
  
-   I valori **Date** di cui non è stato eseguito il mapping vengono visualizzati ed esportati nel formato aaaa-mm-dd.  
  
-   Il valore time va perso per i valori **DateTime** di cui non è stato eseguito il mapping e che vengono visualizzati ed esportati nel formato aaaa-mm-gg.  
  
**Soluzione alternativa:** è possibile visualizzare i valori dei campi di cui non è stato eseguito il mapping nel riquadro inferiore destro della pagina **Gestisci e visualizza risultati** nell'attività di pulizia e nella pagina **Corrispondenza** nell'attività di corrispondenza.  
  
### <a name="415-cannot-import-domain-values-from-an-excel-file-xls-containing-more-than-255-columns-of-data"></a>4.15 Non è possibile importare valori di dominio da un file di Excel (xls) contenente più di 255 colonne di dati  
**Problema:** se si importano valori in un dominio da un file di Excel 97-2003 (con estensione xls) contenente più di 255 colonne di dati, viene visualizzato un messaggio di eccezione e l'importazione non riesce.  
  
**Soluzione alternativa:** per risolvere il problema, è possibile eseguire una delle operazioni seguenti:  
  
-   Salvare il file con estensione xls come file con estensione xlsx, quindi importare i valori dal file con estensione xlsx in un dominio.  
  
-   Rimuovere i dati in tutte le colonne successive alla 255 nel file con estensione xls, salvare il file, quindi importare i valori dal file con estensione xls in un dominio.  
  
### <a name="416-activity-monitoring-feature-is-unavailable-for-roles-other-than-dqs_administrator"></a>4.16 La funzionalità Monitoraggio attività non è disponibile per ruoli diversi da dqs_administrator  
La funzionalità Monitoraggio attività è disponibile solo per gli utenti che dispongono del ruolo dqs_administrator. Se all'account utente è associato il ruolo dqs_kb_editor o dqs_kb_operator, la funzionalità Monitoraggio attività non sarà disponibile nell'applicazione client Data Quality.  
  
### <a name="417-error-on-opening-a-knowledge-base-in-the-recent-knowledge-base-list-for-domain-management"></a>4.17 Errore durante l'apertura di una Knowledge Base nel relativo elenco recente per la gestione del dominio  
Problema: potrebbe essere visualizzato l'errore riportato di seguito se si apre una Knowledge Base nell'elenco **Knowledge Base recente** per l'attività di gestione del dominio nella schermata principale di Data Quality Client:  
  
`"A configuration with name 'RecentList:KB:<domain>\<username>' already exists in the database."`  
  
Questo errore è causato dalla differenza della modalità di confronto di stringhe nel database SQL Server e in C# da parte di DQS. Per il confronto di stringhe non viene applicata la distinzione tra maiuscole e minuscole nel database SQL Server, mentre viene applicata in C#.  
  
Di seguito viene fornito un esempio per illustrare questo comportamento. Si consideri un utente, Domain\user1. L'utente accede al computer Data Quality Client usando l'account "user1" e lavora in una Knowledge Base. Tramite DQS la Knowledge Base recente viene archiviata per ogni utente come un record nella tabella A_CONFIGURATION del database DQS_MAIN. In questo caso, il record sarà archiviato con il nome seguente: RecentList:KB:Domain\user1. Successivamente, l'utente accede al computer Data Quality Client come "User1" (si noti la U maiuscola) e cerca di aprire la Knowledge Base nell'elenco **Knowledge Base recente** per l'attività di gestione del dominio. Tramite il codice sottostante in DQS vengono confrontate le due stringhe RecentList:KB:DOMAIN\user1 e DOMAIN\User1 e, considerando il confronto di stringhe a cui viene applicata la distinzione tra maiuscole e minuscole in C#, le stringhe non corrisponderanno. Di conseguenza, mediante DQS verrà effettuato il tentativo di inserimento di un nuovo record per l'utente (User1) nella tabella A_CONFIGURATION del database DQS_MAIN. Tuttavia, a causa del confronto di stringhe a cui non viene applicata la distinzione tra maiuscole e minuscole nel database SQL, la stringa è già presente nella tabella A_CONFIGURATION del database DQS_MAIN e l'operazione di inserimento non verrà completata correttamente.  
  
**Soluzione alternativa:** per risolvere il problema, è possibile eseguire una delle operazioni seguenti:  
  
-   Verificare l'eventuale presenza di voci duplicate eseguendo l'istruzione riportata di seguito:  
  
    ```  
    SELECT * FROM DQS_MAIN.dbo.A_CONFIGURATION WHERE NAME like 'RecentList%'  
    ```  
  
    Successivamente, è possibile eseguire l'istruzione riportata di seguito per eliminare il record solo per l'utente interessato modificando il valore nella clausola WHERE in modo che corrisponda al dominio e al nome utente interessati.  
  
    ```  
    DELETE DQS_MAIN.dbo.A_Configuration WHERE NAME LIKE 'RecentList%<domain>\<username>'  
    ```  
  
    In alternativa, è possibile rimuovere tutte le voci recenti per tutti gli utenti in DQS:  
  
    ```  
    DELETE DQS_MAIN.dbo.A_Configuration WHERE NAME LIKE 'RecentList%'  
    ```  
  
-   Usare lo stesso uso delle maiuscole del caso precedente per specificare l'account utente mentre si accede al computer client Data Quality.  
  
> [!NOTE]  
> Per evitare questo problema, usare regole di applicazione delle maiuscole coerenti per specificare l'account utente mentre si accede al computer client Data Quality.  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="50-database-engine"></a><a name="DE"></a>5.0 Motore di database  
  
### <a name="51-use-of-distributed-replay-controller-and-distributed-replay-client-features"></a>5.1 Utilizzo delle funzionalità Distributed Replay Controller e Distributed Replay Client  
**Problema:** le funzionalità Controller di Riesecuzione distribuita e Client Riesecuzione distribuita sono rese disponibili nella SKU Server Core di Windows Server 2008, Windows Server 2008 R2 e Windows Server 7, anche se queste due funzionalità non sono supportate nella SKU Server Core.  
  
**Soluzione alternativa:** non installare o usare queste due funzionalità nella SKU Server Core di Windows Server 2008, Windows Server 2008 R2 e Windows Server 7.  
  
### <a name="52-sql-server-management-studio-depends-on-visual-studio-2010-sp1"></a>5.2. SQL Server Management Studio dipende da Visual Studio 2010 SP1  
**Problema**:  per un corretto funzionamento SQL Server 2012 Management Studio dipende da Visual Studio 2010 SP1. La disinstallazione di Visual Studio 2010 SP1 potrebbe causare la perdita di funzionalità in SQL Server Management Studio con conseguente stato non supportato di quest'ultimo. In questo caso potrebbero verificarsi i problemi seguenti:  
  
-   Funzionamento non corretto dei parametri della riga di comando relativi a ssms.exe.  
  
-   Visualizzazione di informazioni errate della Guida durante il tentativo di esecuzione di ssms.exe con l'opzione /? .  
  
-   Per ogni file aperto facendo doppio clic su di esso in Esplora risorse, avvio di una nuova istanza SSMS per aprire il file.  
  
-   Impossibilità di eseguire il debug delle query nella modalità utente normale.  
  
**Soluzione alternativa**: installare di nuovo Visual Studio 2010 SP1 e riavviare Management Studio.  
  
### <a name="53-x64-operating-systems-require-64-bit-powershell-20"></a>5.3 I sistemi operativi x64 richiedono PowerShell 2.0 a 64 bit  
**Problema:** le installazioni a 32 bit di Estensioni Windows PowerShell per SQL Server non sono supportate per istanze di SQL Server 2012 nei sistemi operativi a 64 bit.  
  
**Soluzioni alternative:**  
  
-   Installare SQL Server 2012 a 64 bit con gli strumenti di gestione a 64 bit ed Estensioni Windows PowerShell per SQL Server a 64 bit.  
  
-   In alternativa, importare il modulo SQLPS da un prompt di Windows PowerShell 2.0 a 32 bit.  
  
### <a name="54-an-error-might-occur-when-navigating-in-the-generate-script-wizard"></a>5.4 Potrebbe verificarsi un errore nel corso della Generazione guidata script  
**Problema:** dopo aver generato uno script nella Generazione guidata script facendo clic su **Salva o pubblica script**, su **Scegli opzioni** o **Imposta opzioni di generazione script**, quindi nuovamente su **Salva o pubblica script** potrebbe verificarsi l'errore seguente:  
  
```
An exception occurred while executing a Transact-SQL statement or batch. (Microsoft.SqlServer.ConnectionInfo)  
------------------------------  
ADDITIONAL INFORMATION:  
Invalid object name 'sys.federations'. (Microsoft SQL Server, Error: 208)
```
  
**Soluzione alternativa:** chiudere e riaprire la Generazione guidata script.  
  
### <a name="55-new-maintenance-plan-layout-not-compatible-with-earlier-sql-server-tools"></a>5.5 Layout del nuovo piano di manutenzione non compatibile con gli strumenti precedenti di SQL Server  
**Problema:** quando gli strumenti di gestione di SQL Server 2012 vengono usati per modificare un piano di manutenzione esistente creato in una versione precedente degli strumenti di gestione di SQL Server (SQL Server 2008 R2, SQL Server 2008 o SQL Server 2005), il piano di manutenzione viene salvato in un nuovo formato. Le precedenti versioni degli strumenti di gestione di SQL Server non supportano questo nuovo formato.  
  
**Soluzione alternativa**: nessuno  
  
### <a name="56-intellisense-has-limitations-when-logged-in-to-a-contained-database"></a>5.6 Limitazioni di Intellisense in caso di accesso a un database indipendente  
Problema: Intellisense in SQL Server Management Studio (SSMS) e SQL Server Data Tools (SSDT) non funziona come previsto quando utenti indipendenti accedono a database indipendenti. In questo caso potrebbero verificarsi i problemi seguenti:  
  
1.  Mancata visualizzazione della sottolineatura per oggetti non validi.  
  
2.  Mancata visualizzazione dell'elenco di completamento automatico.  
  
3.  Mancato funzionamento della descrizione comando per le funzioni predefinite.  
  
**Soluzione alternativa**:  nessuno  
  
### <a name="57-alwayson-availability-groups"></a>5.7 Gruppi di disponibilità AlwaysOn  
Prima di provare a creare un gruppo di disponibilità, vedere [Prerequisiti, restrizioni e consigli per i gruppi di disponibilità AlwaysOn (SQL Server)](https://go.microsoft.com/?linkid=9753168) nella documentazione online. Per un'introduzione ai gruppi di disponibilità AlwaysOn, vedere la pagina relativa ai [gruppi di disponibilità AlwaysOn (SQL Server)](https://go.microsoft.com/?linkid=9753166)nella documentazione online.  
  
#### <a name="571-client-connectivity-for-alwayson-availability-groups"></a>5.7.1 Connettività client per i gruppi di disponibilità AlwaysOn  
**Data aggiornamento:** 13 agosto 2012  
  
In questa sezione vengono descritti il supporto dei driver per i gruppi di disponibilità AlwaysOn e le soluzioni alternative per i driver non supportati.  
  
**Supporto driver**  
  
Nella seguente tabella viene riepilogato il supporto dei driver per i gruppi di disponibilità AlwaysOn:  
  
|Driver|Failover su più subnet|Finalità dell'applicazione|Routing di sola lettura|Failover su più subnet: Failover dell'endpoint su una sola subnet più rapido|Failover su più subnet: Risoluzione dell'istanza denominata per le istanze cluster SQL|  
|----------|--------------------------|----------------------|----------------------|------------------------------------------------------------------|---------------------------------------------------------------------------------|  
|SQL Native Client 11.0 ODBC|Sì|Sì|Sì|Sì|Sì|  
|SQL Native Client 11.0 OLEDB|No|Sì|Sì|No|No|  
|ADO.NET con .NET Framework 4.0 con patch di connettività **\&#42;**|Sì|Sì|Sì|Sì|Sì|  
|ADO.NET con.NET Framework 3.5 SP1 con patch di connettività **\&#42;\&#42;**|Sì|Sì|Sì|Sì|Sì|  
|Microsoft JDBC Driver 4.0 per SQL Server|Sì|Sì|Sì|Sì|Sì|  
  
**\&#42;** Scaricare la patch di connettività per ADO .NET con .NET Framework 4.0: [https://support.microsoft.com/kb/2600211](https://support.microsoft.com/kb/2600211).  
  
**\&#42;\&#42;** Scaricare la patch di connettività per ADO .NET con .NET Framework 3.5 SP1: [https://support.microsoft.com/kb/2654347](https://support.microsoft.com/kb/2654347).  
  
**Parola chiave MultiSubnetFailover e funzionalità associate**  
  
MultiSubnetFailover è una nuova parola chiave della stringa di connessione usata per accelerare il failover con i gruppi di disponibilità AlwaysOn e le istanze del cluster di failover AlwaysOn in SQL Server 2012. Le tre seguenti funzionalità secondarie vengono abilitate quando MultiSubnetFailover=True è impostato nella stringa di connessione:  
  
-   Failover multisubnet più rapido su un listener su più subnet per un gruppo di disponibilità AlwaysOn o istanze del cluster di failover.  
  
    -   Risoluzione dell'istanza denominata su un'istanza del cluster di failover AlwaysOn su più subnet.  
  
-   Failover a singola subnet più rapido su un listener su una sola subnet per un gruppo di disponibilità AlwaysOn o istanze del cluster di failover.  
  
    -   Questa funzionalità viene usata per la connessione a un listener in cui è disponibile un singolo indirizzo IP in una sola subnet. Vengono effettuati tentativi di connessione TCP più aggressivi per accelerare i failover su una sola subnet.  
  
-   Risoluzione dell'istanza denominata su un'istanza del cluster di failover AlwaysOn su più subnet.  
  
    -   Questo consente di aggiungere il supporto della risoluzione dell'istanza denominata per istanze del cluster di failover AlwaysOn con endpoint su più subnet.  
  
**MultiSubnetFailover=True non supportato da .NET Framework 3.5 o OLEDB**  
  
**Problema:** Se il gruppo di disponibilità o l'istanza del cluster di failover ha un nome di listener (noto come nome di rete o punto di accesso client in Gestione cluster WSFC) dipendente da più indirizzi IP di subnet diverse e si sta usando ADO.NET con .NET Framework 3.5SP1 o SQL Native Client 11.0 OLEDB, potenzialmente il 50% delle richieste di connessione client al listener del gruppo di disponibilità riscontrerà un timeout di connessione.  
  
**Soluzioni alternative:** è consigliabile eseguire una delle attività seguenti.  
  
-   Se non si dispone dell'autorizzazione per usare le risorse cluster, modificare il timeout di connessione in 30 secondi (questo valore corrisponde a un periodo di timeout TCP di 20 secondi più un buffer di 10).  
  
    **Vantaggi**: se si verifica un failover tra subnet, il tempo di recupero del client è breve.  
  
    **Svantaggi**: la metà delle connessioni client impiegherà più di 20 secondi  
  
-   Se si dispone dell'autorizzazione per usare le risorse cluster, l'approccio migliore consiste nell'impostare il nome di rete del listener del gruppo di disponibilità su **RegisterAllProvidersIP**=0. Per altre informazioni, vedere l'argomento "Script PowerShell di esempio per disabilitare RegisterAllProvidersIP e ridurre TTL", più avanti in questa sezione.  
  
    **Vantaggi:** non è necessario aumentare il valore di timeout della connessione client.  
  
    **Svantaggi:** se si verifica un failover tra subnet, il tempo di recupero del client potrebbe essere di almeno 15 minuti, a seconda dell'impostazione HostRecordTTL e della pianificazione della replica DNS/AD tra siti.  
  
**Script PowerShell di esempio per disabilitare RegisterAllProvidersIP e ridurre TTL**  
  
Tramite il seguente script PowerShell di esempio viene illustrato come disabilitare `RegisterAllProvidersIP` e ridurre TTL. Sostituire `yourListenerName` con il nome del listener che si sta modificando.  
  
```  
Import-Module FailoverClusters  
Get-ClusterResource yourListenerName|Set-ClusterParameter RegisterAllProvidersIP 0  
Get-ClusterResource yourListenerName|Set-ClusterParameter HostRecordTTL 300  
```  
  
#### <a name="572-upgrading-from-ctp3-with-availability-group-configured-is-not-supported"></a>5.7.2 L'aggiornamento da CTP3 con il gruppo di disponibilità configurato non è supportato  
Eliminare il gruppo di disponibilità e ricrearlo prima di procedere con l'aggiornamento. Si tratta di una limitazione della build CTP3. Le build future non avranno questa limitazione.  
  
#### <a name="573-side-by-side-installation-of-ctp3-with-later-versions-is-not-supported-if-you-have-an-availability-group-configured-in-your-instance"></a>5.7.3 L'installazione side-by-side di CTP3 con versioni successive non è supportata se nell'istanza è configurato un gruppo di disponibilità  
Si tratta di una limitazione della build CTP3. Le build future non avranno questa limitazione.  
  
#### <a name="574-side-by-side-installation-of-ctp3-with-later-versions-of-failover-cluster-instances-is-not-supported"></a>5.7.4 L'installazione side-by-side di CTP3 con versioni successive delle istanze del cluster di failover non è supportata.  
Si tratta di una limitazione della build CTP3. Le build future non avranno questa limitazione. Per aggiornare le istanze del cluster di failover da CTP3, assicurarsi di aggiornare tutte le istanze di un nodo allo stesso tempo.  
  
#### <a name="575--timeouts-may-occur-when-using-multi-ips-in-the-same-subnet-with-alwayson"></a>5.7.5 Possibile condizione di timeout in caso di utilizzo di più IP nella stessa subnet con AlwaysOn  
**Problema:** quando si usano più IP nella stessa subnet con AlwaysOn, i clienti potrebbero talvolta riscontrare un timeout. Questa condizione si verifica se il primo IP dell'elenco è errato.  
  
**Soluzione alternativa:** usare 'multisubnetfailover = true' nella stringa di connessione.  
  
#### <a name="576-failure-to-create-new-availability-group-listeners-because-of-active-directory-quotas"></a>5.7.6 Errore nella creazione di nuovi listener del gruppo di disponibilità a causa di quote di Active Directory  
**Problema:** La creazione di un nuovo listener del gruppo di disponibilità può avere esito negativo perché è stata raggiunta una quota di Active Directory per l'account del computer del nodo del cluster interessato. Per altre informazioni, vedere [Risoluzione dei problemi dell'account del Servizio cluster in relazione alla modifica di oggetti computer](https://support.microsoft.com/kb/307532) e la pagina relativa alle [quote di Active Directory](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc904295(v=ws.10)).  
  
#### <a name="577-netbios-conflicts-because-availability-group-listener-names-use-an-identical-15-character-prefix"></a>5.7.7 Conflitti NetBIOS dovuti all'utilizzo dello stesso prefisso a 15 caratteri nei nomi dei listener del gruppo di disponibilità  
Se si dispone di due cluster WSFC controllati dallo stesso dominio Active Directory e si tenta di creare listener del gruppo di disponibilità in entrambi i cluster usando nomi con più di 15 caratteri e un prefisso a 15 caratteri identico, verrà restituito un errore in cui si segnala che non è possibile portare online la risorsa del nome di rete virtuale. Per informazioni sulle regole di denominazione dei prefissi per i nomi DNS, vedere [Assegnare nomi ai domini](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731265(v=ws.10)).  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="60-integration-services"></a><a name="IS"></a>6.0 Integration Services  
  
### <a name="61-the-change-data-capture-service-for-oracle-and-the-change-data-capture-designer-console-for-oracle"></a>6.1 Servizio Change Data Capture per Oracle e Change Data Capture Designer Console per Oracle  
Il servizio CDC per Oracle è un servizio di Windows tramite cui vengono analizzati i log delle transazioni Oracle e acquisite le modifiche apportate alle tabelle Oracle di interesse nelle tabelle delle modifiche di SQL Server. Tramite CDC Designer Console è possibile sviluppare e gestire le istanze di Oracle CDC. CDC Designer Console è uno snap-in di Microsoft Management Console (MMC).  
  
#### <a name="611-install-the-cdc-service-for-oracle-and-the-cdc-designer-for-oracle"></a>6.1.1 Installare il servizio CDC per Oracle e CDC Designer per Oracle  
**Problema:** il servizio CDC e CDC Designer non vengono installati dal programma di installazione di SQL Server. È necessario installare manualmente il servizio CDC o CDD Designer in un computer che soddisfa i requisiti e i prerequisiti descritti nei file della Guida aggiornati.  
  
**Soluzione alternativa:** per installare il servizio CDC per Oracle, eseguire manualmente AttunityOracleCdcService.msi dai supporti di installazione di SQL Server. Per installare CDC Designer Console, eseguire manualmente AttunityOracleCdcDesigner.msi dai supporti di installazione di SQL Server.  I pacchetti di installazione per x86 e x64 sono presenti nel percorso .\Tools\AttunityCDCOracle\ sul supporto di installazione di SQL Server.  
  
#### <a name="612-f1-help-functionality-points-to-incorrect-documentation-files"></a>6.1.2 La funzionalità della Guida punta a file della documentazione non corretti  
**Problema:** non è possibile accedere alla documentazione della Guida corretta usando l'elenco a discesa con F1 o facendo clic su "?" nelle console di Attunity. Questi metodi puntano a file chm non corretti.  
  
**Soluzione alternativa:** i file con estensione chm corretti vengono installati insieme al servizio CDC per Oracle e CDC Designer per Oracle. Per visualizzare il contenuto corretto della Guida, avviare i file chm direttamente da questo percorso: `%Program Files%\Change Data Capture for Oracle by Attunity\*.chm`.  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="70-master-data-services"></a><a name="MDS"></a>7.0 Master Data Services  
  
### <a name="71-fixing-an-mds-installation-in-a-cluster"></a>7.1 Correzione di un'installazione di MDS in un cluster  
**Problema:** se si installa un'istanza in cluster della versione RTM di SQL Server 2012 con la casella di controllo **Master Data Services** selezionata, MDS verrà installato in un singolo nodo, ma non sarà disponibile né funzionerà in altri nodi aggiunti al cluster.  
  
**Soluzione alternativa**: per risolvere questo problema è necessario installare la versione cumulativa 1 (CU1) di SQL Server 2012 eseguendo la procedura seguente:  
  
1.  Assicurarsi che non sia presente alcuna installazione di SQL/MDS.  
  
2.  Scaricare SQL Server 2012 CU1 in una directory locale.  
  
3.  Installare SQL Server 2012 con la funzionalità MDS nel nodo del cluster principale, quindi in qualsiasi altro nodo del cluster.  
  
Per altre informazioni su questi problemi e su come effettuare i passaggi riportati in precedenza, vedere [https://support.microsoft.com/kb/2683467](https://support.microsoft.com/kb/2683467).  
  
### <a name="72-microsoft-silverlight-5-required"></a>7.2 Installazione di Microsoft Silverlight 5 necessaria  
Per funzionare nell'applicazione Web Gestione dati master, è necessario aver installato Silverlight 5.0 nel computer client. Se non si dispone della versione richiesta di Silverlight, sarà necessario installarla quando si passa a un'area dell'applicazione Web in cui è necessaria. Silverlight 5 può essere installato da [https://go.microsoft.com/fwlink/?LinkId=243096](https://go.microsoft.com/fwlink/?LinkId=243096).  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="80-reporting-services"></a><a name="RS"></a>8.0 Reporting Services  
  
### <a name="81-reporting-services-connectivity-to-sql-server-pdw-requires-updated-drivers"></a>8.1 Per la connettività di Reporting Services in SQL Server PDW sono richiesti driver aggiornati  
Per la connettività da SQL Server 2012 Reporting Services a Microsoft SQL Server PDW Appliance Update 2 e versioni successive è richiesto un aggiornamento ai driver di connettività di PDW. Per altre informazioni, è consigliabile che i clienti di SQL Server PDW contattino il supporto Microsoft.  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="90-streaminsight"></a><a name="SI"></a>9.0 StreamInsight  
SQL Server 2012 include StreamInsight 2.1. StreamInsight 2.1 richiede una licenza Microsoft SQL Server 2012 e .NET Framework 4.0. Oltre ad alcune correzioni di bug, in questa versione sono inclusi diversi miglioramenti delle prestazioni. Per altre informazioni, vedere le [note sulla versione di Microsoft StreamInsight 2.1](https://social.technet.microsoft.com/wiki/contents/articles/6539.aspx). Per scaricare separatamente StreamInsight 2.1, visitare la [pagina di download di Microsoft StreamInsight 2.1](https://www.microsoft.com/download/details.aspx?id=30149) nell'Area download Microsoft.  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="100-upgrade-advisor"></a><a name="UA"></a>10.0 Preparazione aggiornamento  
  
### <a name="101-link-to-install-upgrade-advisor-is-not-enabled-on-chinese-hk-operating-systems"></a>10.1 Il collegamento per installare Upgrade Advisor non è abilitato in sistemi operativi cinesi (HK)  
Problema: Quando si prova a installare Preparazione aggiornamento in qualsiasi versione di Windows supportata in sistemi operativi in lingua cinese (RAS di Hong Kong), è possibile che il collegamento per installare Preparazione aggiornamento non sia abilitato.  
  
**Soluzione alternativa**: individuare il file **SQLUA.msi** nel supporto SQL Server 2012 in `\1028_CHT_LP\x64\redist\Upgrade Advisor` o `\1028_CHT_LP\x86\redist\Upgrade Advisor`, a seconda dell'architettura del sistema operativo.  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")
