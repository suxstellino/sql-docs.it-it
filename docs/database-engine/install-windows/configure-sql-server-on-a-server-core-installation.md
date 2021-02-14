---
title: Configurare un'installazione Server Core
description: Questo articolo illustra in dettaglio la configurazione di SQL Server in un'installazione Server Core, inclusi gli strumenti di risoluzione dei problemi.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- IsHadrEnabled server property
- Server Core Installation [SQL Server]
ms.assetid: ed6e5e94-4b8d-422a-a17e-61b05a4df903
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 300e06b7eaab03af63685e495d8aa0de2ad83ed6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336390"
---
# <a name="configure-sql-server-on-a-server-core-installation"></a>Configurare SQL Server in un'installazione Server Core

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Questo articolo descrive i dettagli sulla configurazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un'installazione Server Core.  

##  <a name="configure-and-manage-server-core-on-windows-server"></a><a name="BKMK_ConfigureWindows"></a> Configurare e gestire Server Core in Windows Server  
Nella sezione vengono forniti i riferimenti agli articoli in cui sono illustrate la configurazione e la gestione di un'installazione Server Core.  
  
Non tutte le funzionalità di [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] sono supportate nella modalità Server Core.  Alcune di queste possono essere installate in un computer client o in un altro server in cui non viene eseguito Server Core e possono essere connesse ai servizi del motore di database installati in Server Core.  
  
Per altre informazioni sulla configurazione e sulla gestione di un'installazione Server Core in remoto, vedere gli articoli seguenti:  
  
- [Installazione Server Core](/windows-server/get-started/getting-started-with-server-core)  
  
- [Configurare un'installazione Server Core di Windows Server 2016 con Sconfig.cmd](/windows-server/get-started/sconfig-on-ws2016)  
  
- [Installare i ruoli e le funzionalità server in un server Server Core - Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574158(v=ws.11))
  
- [Gestione di un'installazione Server Core: Panoramica](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee441255(v=ws.10))  
  
- [Amministrazione di un'installazione Server Core](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee441258(v=ws.10))
  
##  <a name="install-ssnoversion-updates"></a><a name="BKMK_InstallSQLUpdates"></a> Installare gli aggiornamenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
In questa sezione vengono fornite informazioni sull'installazione di aggiornamenti per [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] in un computer con Windows Server Core. Si consiglia agli clienti di valutare e installare tempestivamente gli ultimi aggiornamenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per assicurarsi che i sistemi dispongano degli aggiornamenti di sicurezza più recenti. Per altre informazioni sulla procedura di installazione di [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] in un computer con Windows Server Core, vedere [Installare SQL Server in Server Core](../../database-engine/install-windows/install-sql-server-on-server-core.md).  
  
Di seguito sono disponibili due scenari per l'installazione di aggiornamenti del prodotto:  
  
- [Installazione di aggiornamenti per SQL Server durante una nuova installazione](../../database-engine/install-windows/configure-sql-server-on-a-server-core-installation.md#bkmk_NewInstall)  
  
- [Installazione di aggiornamenti per SQL Server dopo che è stato installato](../../database-engine/install-windows/configure-sql-server-on-a-server-core-installation.md#bkmk_alreadyInstall)  
  
###  <a name="installing-updates-for-ssnoversion-during-a-new-installation"></a><a name="bkmk_NewInstall"></a> Installazione di aggiornamenti per [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] durante una nuova installazione  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Il programma di installazione supporta solo installazioni dal prompt dei comandi in un sistema operativo Server Core. Per altre informazioni, vedere [Installazione di SQL Server dal prompt dei comandi](./install-sql-server-from-the-command-prompt.md).  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Il programma di installazione integra gli aggiornamenti più recenti del pacchetto con l'installazione del prodotto principale in modo che quest'ultimo e i relativi aggiornamenti applicabili vengano installati contemporaneamente.  
  
Dopo avere individuato le versioni più recenti degli aggiornamenti applicabili, questi vengono scaricati e integrati dal programma di installazione con il processo di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] corrente. Tramite l'aggiornamento del prodotto è possibile includere un aggiornamento cumulativo, un Service Pack o un Service Pack con aggiornamento cumulativo.  
  
Specificare i parametri UpdateSource e UpdateEnabled per includere gli aggiornamenti più recenti del prodotto con l'installazione del prodotto principale. Fare riferimento all'esempio seguente per abilitare gli aggiornamenti del prodotto durante l'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
```  
Setup.exe /qs /ACTION=Install /FEATURES=SQLEngine /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="<DomainName\UserName>" /SQLSVCPASSWORD="<StrongPassword>" /SQLSYSADMINACCOUNTS="<DomainName\UserName>" /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /UpdateEnabled=True /UpdateSource="<SourcePath>" /IACCEPTSQLSERVERLICENSETERMS  
```  
  
###  <a name="installing-updates-for-ssnoversion-after-it-has-been-installed"></a><a name="bkmk_alreadyInstall"></a> Installazione di aggiornamenti per [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] dopo che è stato installato  
In un'istanza installata di [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)]si consiglia di applicare gli aggiornamenti della sicurezza e gli aggiornamenti critici più recenti, inclusi General Distribution Release (GDR) e Service Pack (SP). I singoli aggiornamenti cumulativi e aggiornamenti della sicurezza devono essere adottati secondo le esigenze e caso per caso. Valutare l'aggiornamento; se è necessario, applicarlo.  
  
Applicare un aggiornamento dal prompt dei comandi sostituendo <package_name> con il nome del pacchetto di aggiornamento.  
  
- Aggiornare una singola istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e tutti i componenti condivisi. L'istanza può essere specificata utilizzando il parametro InstanceName o InstanceID.  
  
    ```  
    <package_name>.exe /qs /IAcceptSQLServerLicenseTerms /Action=Patch /InstanceName=MyInstance  
    ```  
  
- Aggiornare solo i componenti condivisi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
    ```  
    <package_name>.exe /qs /IAcceptSQLServerLicenseTerms /Action=Patch  
    ```  
  
- Aggiornare tutte le istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel computer e tutti i componenti condivisi:  
  
    ```  
    <package_name>.exe /qs /IAcceptSQLServerLicenseTerms /Action=Patch /AllInstances  
    ```  
  
## <a name="startstop-ssnoversion-service"></a><a name="BKMK_StartStopServices"></a> Avviare/arrestare il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
L'applicazione [sqlservr](../../tools/sqlservr-application.md) avvia, arresta, sospende e riprende un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dal prompt dei comandi.  
  
È inoltre possibile utilizzare i servizi .NET per avviare e arrestare i servizi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="enable-alwayson-availability-groups"></a><a name="BKMK_EnableAlwaysON"></a> Abilitare Gruppi di disponibilità AlwaysOn  
L'abilitazione per Gruppi di disponibilità AlwaysOn rappresenta un prerequisito per consentire a un'istanza del server di utilizzare i gruppi di disponibilità come soluzione a disponibilità elevata e di ripristino di emergenza. Per altre informazioni sulla gestione di Gruppi di disponibilità AlwaysOn, vedere [Abilitare e disabilitare la funzionalità Gruppi di disponibilità AlwaysOn SQL Server](../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md).  
  
### <a name="using-ssnoversion-configuration-manager-remotely"></a>Utilizzo di Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in remoto  
Questi passaggi devono essere effettuati in un PC in cui viene eseguita l'edizione client di Windows oppure Windows Server con la funzionalità Shell grafica server installata.  
  
1. Aprire **Gestione computer**. Per aprire **Gestione Computer**, fare clic su **Start**, digitare `compmgmt.msc`, quindi fare clic su **OK**.    
  
2. Nell'albero della console fare clic con il pulsante destro del mouse su **Gestione computer**, quindi scegliere **Connetti a un altro computer**.  
  
3. Nella finestra di dialogo **Seleziona computer** digitare il nome del computer con Server Core che si desidera gestire oppure fare clic su **Sfoglia** per individuarlo, quindi scegliere **OK**.  
  
4. In **Gestione computer** del computer con Server Core nell'albero della console fare clic su **Servizi e applicazioni**.  
  
5. Fare doppio clic su **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Gestione configurazione**.  
  
6. In **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Configuration Manager** fare clic su **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Servizi**, fare clic con il pulsante destro del mouse su **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** (\<instance name>), dove \<instance name> è il nome di un'istanza server locale per la quale si vuole abilitare Gruppi di disponibilità AlwaysOn, e fare clic su Proprietà.  
  
7. Selezionare la scheda **Disponibilità elevata AlwaysOn** .  
  
8. Verificare che nel campo Nome cluster di failover Windows sia incluso il nome del nodo del cluster di failover locale. Se il campo è vuoto, questa istanza del server non supporta attualmente Gruppi di disponibilità AlwaysOn. Il computer locale non è un nodo del cluster, il cluster WSFC è stato chiuso oppure questa edizione di [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] non supporta Gruppi di disponibilità AlwaysOn.  
  
9. Selezionare la casella di controllo Abilita gruppi di disponibilità AlwaysOn e scegliere OK.  
  
10. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Successivamente, è necessario riavviare manualmente il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . In questo modo è possibile scegliere un'ora per il riavvio che meglio soddisfa le esigenze aziendali. Al riavvio del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , AlwaysOn sarà abilitato e la proprietà del server IsHadrEnabled sarà impostata su 1.  
  
> [!NOTE]
>  -   È necessario disporre dei diritti utente adeguati o essere stato delegato dell'autorità appropriata nel computer di destinazione per connettersi al computer.  
> -   Il nome del computer che viene gestito viene visualizzato tra parentesi accanto a Gestione computer nell'albero della console.  
  
### <a name="using-powershell-cmdlets-to-enable-alwayson-availability-groups"></a>Utilizzo dei cmdlet di PowerShell per abilitare Gruppi di disponibilità AlwaysOn  
Il cmdlet di PowerShell, Enable-SqlAlwaysOn, viene utilizzato per abilitare Gruppi di disponibilità AlwaysOn in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se la funzionalità Gruppi di disponibilità AlwaysOn viene abilitata durante l'esecuzione del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è necessario riavviare il servizio Motore di database affinché la modifica venga completata. A meno che non sia specificato il parametro -Force, il cmdlet richiede se si desidera riavviare il servizio; se annullato, non viene eseguita alcuna operazione.  
  
Per la relativa esecuzione è necessario disporre delle autorizzazioni di amministratore.  
  
È possibile utilizzare una delle sintassi seguenti per abilitare Gruppi di disponibilità AlwaysOn per un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
```  
Enable-SqlAlwaysOn [-Path <string>] [-Credential <PSCredential>] [-Force] [-NoServiceRestart] [-Confirm] [-WhatIf] [<Commom Parameters>]  
```  
  
```  
Enable-SqlAlwaysOn -InputObject <Server> [-Credential <PSCredential>] [-Force] [-NoServiceRestart] [-Confirm] [-WhatIf] [<Commom Parameters>]  
```  
  
```  
Enable-SqlAlwaysOn [-ServerInstance <string>] [-Credential <PSCredential>] [-Force] [-NoServiceRestart] [-Confirm] [-WhatIf] [<Commom Parameters>]  
```  
  
Tramite il comando di PowerShell seguente viene abilitata la funzionalità Gruppi di disponibilità AlwaysOn di un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (Machine\Instance):  
  
```  
Enable-SqlAlwaysOn -Path SQLSERVER:\SQL\Machine\Instance  
```  
  
##  <a name="configuring-remote-access-of-ssnoversion-running-on-server-core"></a><a name="BKMK_ConfigureRemoteAccess"></a> Configurazione dell'accesso remoto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in esecuzione in Server Core  
 Eseguire le azioni descritte di seguito per configurare l'accesso remoto di un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] in esecuzione in Windows Server Core.  
  
### <a name="enable-remote-connections-on-the-instance-of-ssnoversion"></a>Abilitare connessioni remote nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Per abilitare connessioni remote, utilizzare in locale SQLCMD.exe ed eseguire le istruzioni seguenti nell'istanza Server Core:  
  
-   `EXEC sys.sp_configure N'remote access', N'1'`  
  
     `GO`  
  
-   `RECONFIGURE WITH OVERRIDE`  
  
     `GO`  
  
### <a name="enable-and-start-the-ssnoversion-browser-service"></a>Abilitare e avviare il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser  
 Per impostazione predefinita, il servizio Browser è disabilitato.  Se risulta disabilitato in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in esecuzione in Server Core, eseguire il comando riportato di seguito dal prompt dei comandi per abilitarlo:  
  
 `sc config SQLBROWSER start= auto`  
  
 Dopo averlo abilitato, eseguire il comando seguente dal prompt dei comandi per avviarlo:  
  
 `net start SQLBROWSER`  
  
### <a name="create-exceptions-in-windows-firewall"></a>Creazione di eccezioni in Windows Firewall  
 Per creare eccezioni per l'accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in Windows Firewall, seguire i passaggi specificati in [Configurare Windows Firewall per consentire l'accesso a SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
  
### <a name="enable-tcpip-on-the-instance-of-ssnoversion"></a>Abilitare TCP/IP nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Il protocollo TCP/IP può essere abilitato tramite Windows PowerShell per un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in Server Core. A tale scopo, seguire questa procedura:  
  
1.  Avviare **Gestione attività** nel computer in cui è in esecuzione Windows Server Core.  
  
2.  Nella scheda **Applicazioni** fare clic su **Nuova attività**.  
  
3.  Nella finestra di dialogo **Crea una nuova attività** digitare **sqlps.exe** nel campo **Apri** , quindi fare clic su **OK**. Verrà aperta la finestra di **Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Powershell**.  
  
4.  Nella finestra di **Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Powershell** eseguire lo script seguente per abilitare il protocollo TCP/IP:  
  
```powershell
$smo = 'Microsoft.SqlServer.Management.Smo.'  
$wmi = new-object ($smo + 'Wmi.ManagedComputer')  
# Enable the TCP protocol on the default instance.  If the instance is named, replace MSSQLSERVER with the instance name in the following line.  
$uri = "ManagedComputer[@Name='" + (get-item env:\computername).Value + "']/ServerInstance[@Name='MSSQLSERVER']/ServerProtocol[@Name='Tcp']"  
$Tcp = $wmi.GetSmoObject($uri)  
$Tcp.IsEnabled = $true  
$Tcp.Alter()  
$Tcp  
```  
  
##  <a name="ssnoversion-profiler"></a><a name="BKMK_Profiler"></a> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Profiler  
 In un computer remoto avviare [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] e selezionare Nuova traccia dal menu File. Tramite l'applicazione viene visualizzata la finestra di dialogo Connetti al server in cui è possibile specificare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , che risiede nel computer con Server Core, a cui si desidera effettuare la connessione. Per altre informazioni, vedere [Avviare SQL Server Profiler](../../tools/sql-server-profiler/start-sql-server-profiler.md).  
  
 Per altre informazioni sulle autorizzazioni richieste per eseguire [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], vedere [Autorizzazioni necessarie per l'esecuzione di SQL Server Profiler](../../tools/sql-server-profiler/permissions-required-to-run-sql-server-profiler.md).  
  
 Per altri dettagli su [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], vedere [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md).  
  
##  <a name="ssnoversion-auditing"></a><a name="BKMK_Auditing"></a> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Controllo  
 Per definire un controllo, è possibile utilizzare [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)] in remoto. In seguito alla creazione e all'abilitazione del controllo, la destinazione riceverà le voci. Per altre informazioni sulla creazione e sulla gestione dei controlli di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [SQL Server Audit &#40;Database Engine&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
##  <a name="command-prompt-utilities"></a><a name="BKMK_CMD"></a> Utilità del prompt dei comandi  
 È possibile utilizzare le utilità del prompt dei comandi seguenti tramite cui si possono generare script di operazioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un computer con Server Core. Nella tabella seguente è riportato un elenco delle utilità del prompt dei comandi fornite con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per Server Core:  
  
|**Utilità**|**Descrizione**|**Posizione di installazione**|  
|-----------------|---------------------|----------------------|  
|[Utilità bcp](../../tools/bcp-utility.md)|Usata per copiare i dati tra un'istanza di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e un file di dati in un formato specificato dall'utente.|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Utilità dtexec](../../integration-services/packages/dtexec-utility.md)|Utilizzata per configurare ed eseguire un pacchetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]DTS\Binn|  
|[Utilità dtutil](../../integration-services/dtutil-utility.md)|Consente di gestire i pacchetti SSIS.|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]DTS\Binn|  
|[Utilità osql](../../tools/osql-utility.md)|Consente di immettere istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] , procedure di sistema e file script al prompt dei comandi.|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Applicazione sqlagent90](../../tools/sqlagent90-application.md)|Utilizzata per avviare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent da un prompt dei comandi.|\<drive>:\Programmi\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\<*nome_istanza*>\MSSQL\Binn|  
|[Utilità sqlcmd](../../tools/sqlcmd-utility.md)|Consente di immettere istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] , procedure di sistema e file script al prompt dei comandi.|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Utilità SQLdiag](../../tools/sqldiag-utility.md)|Utilizzata per raccogliere informazioni di diagnostica per il Servizio Supporto Tecnico Clienti [!INCLUDE[msCoName](../../includes/msconame-md.md)] .|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Utilità sqlmaint](../../tools/sqlmaint-utility.md)|Utilizzata per eseguire i piani di manutenzione dei database creati nelle precedenti versioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|\<drive>:\Programmi\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL14.MSSQLSERVER\MSSQL\Binn|  
|[Utilità sqlps](../../tools/sqlps-utility.md)|Consente di eseguire comandi e script di PowerShell, nonché di caricare e registrare il provider e i cmdlet di PowerShell per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[sqlservr](../../tools/sqlservr-application.md)|Consente di avviare e arrestare un'istanza di [!INCLUDE[ssDE](../../includes/ssde-md.md)] dal prompt dei comandi per la risoluzione dei problemi.|\<drive>:\Programmi\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL14.MSSQLSERVER\MSSQL\Binn|  
  
##  <a name="use-troubleshooting-tools"></a><a name="BKMK_troubleshoot"></a> Utilizzare gli strumenti di risoluzione dei problemi  
 È possibile usare [SQLdiag Utility](../../tools/sqldiag-utility.md) per raccogliere i log e i file di dati da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e altri tipi di server, nonché per monitorare i server in un intervallo di tempo oppure risolvere problemi specifici dei server. SQLdiag è stata creata per velocizzare e semplificare la raccolta delle informazioni di diagnostica necessarie per il Servizio Supporto Tecnico Clienti Microsoft.  
  
 È possibile avviare l'utilità nel prompt dei comandi dell'amministratore in Server Core, usando la sintassi specificata nell'articolo: [Utilità di SQLdiag](../../tools/sqldiag-utility.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Installare SQL Server in Server Core](../../database-engine/install-windows/install-sql-server-on-server-core.md)   
 [Procedure per l'installazione](/previous-versions/sql/)  
  
