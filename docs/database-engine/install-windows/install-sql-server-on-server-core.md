---
title: Installare SQL Server in Server Core | Microsoft Docs
description: È possibile installare SQL Server in un'installazione Server Core. L'opzione di installazione Server Core offre un ambiente minimo per l'esecuzione di ruoli del server specifici.
ms.custom: ''
ms.date: 06/29/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 1dd294cc-5b69-4d0c-9005-3e307b75678b
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: ab5a2352358fdb678241425e0ca5e8f999d8a2f2
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170173"
---
# <a name="install-sql-server-on-server-core"></a>Installare SQL Server in Server Core

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

È possibile installare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un'installazione Server Core.   
  
L'opzione di installazione Server Core offre un ambiente minimo per l'esecuzione di ruoli del server specifici. Ciò consente di ridurre i requisiti di manutenzione e gestione e la superficie di attacco per i ruoli del server in questione. Per altre informazioni sull'installazione in Server Core, vedere [Installare Server Core](/windows-server/get-started/getting-started-with-server-core). Per altre informazioni sull'implementazione di Server Core in [!INCLUDE[win8srv](../../includes/win8srv-md.md)], vedere [Server Core for Windows Server 2012](https://msdn.microsoft.com/library/hh846323\(VS.85\).aspx) (https://msdn.microsoft.com/library/hh846323(VS.85).aspx) (Server Core per Windows Server 2012).  
  
 Per un elenco dei sistemi operativi attulamente supportati, vedere [Requisiti hardware e software per l'installazione di SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md).

## <a name="prerequisites"></a>Prerequisiti  
  
|Requisito|Modalità di installazione|  
|-----------------|--------------------|  
|[!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 4.6.1 |Per tutte le edizioni di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], ad eccezione di [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)], l'installazione richiede [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 4.6.1 Server Core Profile. Il programma di installazione di SQL Server lo installerà automaticamente se non è già installato. L'installazione richiede un riavvio. È possibile installare [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] prima di eseguire il programma di installazione per evitare un riavvio.|  
|Windows Installer 4.5|Fornito con installazione Server Core.|  
|Windows PowerShell|Fornito con installazione Server Core.|  
|Java Runtime |Per usare PolyBase, è necessario installare il Java Runtime appropriato. Per altre informazioni, vedere [Installazione di PolyBase](../../relational-databases/polybase/polybase-installation.md).|
  
##  <a name="supported-features"></a><a name="BK_SupportedFeatures"></a> Funzionalità supportate  
 Usare la tabella seguente per identificare le funzionalità supportate in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] in un'installazione Server Core.  
  
|Funzionalità|Supportato|Informazioni aggiuntive|  
|-------------|---------------|----------------------------|  
|Servizi[!INCLUDE[ssDE](../../includes/ssde-md.md)]|Sì||  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Replica|Sì||  
|Ricerca full-text|Sì||  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|Sì||  
|[!INCLUDE[rsql_productname_md](../../includes/rsql-productname-md.md)]|Sì||  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|No||  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Data Tools (SSDT)|No||  
|Connettività strumenti client|Sì||  
|Server Integration Services|Sì||  
|Compatibilità con le versioni precedenti di strumenti client.|No||  
|SDK di strumenti client|No||  
|Documentazione online di[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|No||  
|Strumenti di gestione - Di base|Solo remoto|L'installazione di queste funzionalità in Server Core non è supportata. Questi componenti possono essere installati in un server diverso da Server Core e connessi ai servizi del [!INCLUDE[ssDE](../../includes/ssde-md.md)] installati in Server Core.|  
|Strumenti di gestione - Completa|Solo remoto|L'installazione di queste funzionalità in Server Core non è supportata. Questi componenti possono essere installati in un server diverso da Server Core e connessi ai servizi del [!INCLUDE[ssDE](../../includes/ssde-md.md)] installati in Server Core.|  
|Controller di Riesecuzione distribuita|No||  
|Client Riesecuzione distribuita|Solo remoto|L'installazione di queste funzionalità in Server Core non è supportata. Questi componenti possono essere installati in un server diverso da Server Core e connessi ai servizi del [!INCLUDE[ssDE](../../includes/ssde-md.md)] installati in Server Core.|  
|SDK di Connettività SQL Client|Sì||  
|Microsoft Sync Framework|Sì|Microsoft Sync Framework non è incluso nel pacchetto di installazione di [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] . È possibile scaricare la versione appropriata di Sync Framework dall'[Area download Microsoft](https://go.microsoft.com/fwlink/?LinkId=221788) (https://go.microsoft.com/fwlink/?LinkId=221788) e installarla in un computer che esegue Server Core.|  
|[!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]|No||  
|[!INCLUDE[ssDQSnoversion](../../includes/ssdqsnoversion-md.md)]|No||  
  
## <a name="supported-scenarios"></a>Scenari supportati  
 Nella tabella seguente viene illustrata la matrice di scenario supportata per l'installazione di [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] in un'installazione Server Core.  
  
| Installazione | Destinazione valida |  
|-|-|  
|Edizioni di[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Tutte le edizioni a 64 bit di [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] |  
|Lingua di[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Tutte le lingue|  
|Lingua di[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nella combinazione lingua/impostazioni locali del sistema operativo|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in lingua inglese nel sistema operativo Windows il lingua giapponese<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in lingua inglese nel sistema operativo Windows in lingua tedesca<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in lingua inglese nel sistema operativo Windows in lingua cinese (Cina)<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in lingua inglese nel sistema operativo Windows in lingua araba (Arabia Saudita)<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in lingua inglese nel sistema operativo Windows in lingua tailandese (Thai)<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in lingua inglese nel sistema operativo Windows in lingua turca<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in lingua inglese nel sistema operativo Windows in lingua portoghese (Portogallo)<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in lingua inglese nel sistema operativo Windows in lingua inglese|  
|Edizione di Windows|Windows Server 2019 Datacenter <br/><br/> Windows Server 2019 Standard <br /><br />  [!INCLUDE[winserver2016_datacenter_md](../../includes/winserver2016-datacenter-md.md)]<br/><br/>[!INCLUDE[winserver2016_standard_md](../../includes/winserver2016-standard-md.md)]<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Datacenter<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Standard<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Essentials<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Foundation<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Data Center<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Standard<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Essentials<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Foundation|  
  
## <a name="upgrade"></a>Aggiornamento 
 Nelle installazioni Server Core è supportato l'aggiornamento da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] a [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] .  
  
## <a name="install"></a>Installazione  
 [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] non supporta l'installazione tramite apposita procedura guidata nel sistema operativo Server Core. In caso di installazione in Server Core, il programma di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prevede il supporto della modalità non interattiva completa tramite il parametro /Q o della modalità non interattiva semplice tramite il parametro /QS. Per altre informazioni, vedere [Installazione di SQL Server dal prompt dei comandi](./install-sql-server-from-the-command-prompt.md).  
  
 Indipendentemente dal metodo di installazione, è necessario confermare l'accettazione delle condizioni di licenza del software come utente singolo o per conto di un'entità, a meno che l'utilizzo del software non sia disciplinato da un contratto separato, ad esempio un contratto multilicenza [!INCLUDE[msCoName](../../includes/msconame-md.md)] o un contratto di terze parti con un fornitore di software indipendente o un OEM.  
  
 Le condizioni di licenza vengono visualizzate per la revisione e l'accettazione nell'interfaccia utente del programma di installazione. Le installazioni automatiche che utilizzano i parametri /Q o /QS devono includere il parametro /IACCEPTSQLSERVERLICENSETERMS. È possibile esaminare separatamente le condizioni di licenza alla pagina relativa alle [condizioni di licenza software Microsoft](https://go.microsoft.com/fwlink/?LinkId=148209).  
  
> [!NOTE]  
>  A seconda della modalità di ricezione del software, ad esempio attraverso un contratto multilicenza [!INCLUDE[msCoName](../../includes/msconame-md.md)] , l'utilizzo del software potrebbe essere soggetto a condizioni aggiuntive.  
  
 Per installare funzionalità specifiche, utilizzare il parametro /FEATURES e specificare la funzionalità padre oppure i valori delle funzionalità. Per ulteriori informazioni sui parametri delle funzionalità e sul relativo utilizzo, vedere le sezioni seguenti.  
  
### <a name="feature-parameters"></a>Parametri delle funzionalità  
  
|Parametro della funzionalità|Descrizione|  
|-----------------------|-----------------|  
|SQLENGINE|Viene installato solo il [!INCLUDE[ssDE](../../includes/ssde-md.md)].|  
|REPLICA|Viene installato il componente di replica insieme al [!INCLUDE[ssDE](../../includes/ssde-md.md)].|  
|FULLTEXT|Viene installato il componente FullText insieme al [!INCLUDE[ssDE](../../includes/ssde-md.md)].|  
|AS|Vengono installati tutti i componenti di [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .|  
|IS|Vengono installati tutti i componenti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .|  
|CONN|Vengono installati i componenti di connettività.| 
|ADVANCEDANALYTICS |Installa R Services e richiede il motore di database. Le installazioni automatiche richiedono il parametro /IACCEPTROPENLICENSETERMS.  |


 Vedere l'esempio seguente relativo all'utilizzo di parametri delle funzionalità:  
  
|Parametro e valori|Descrizione|  
|--------------------------|-----------------|  
|/FEATURES=SQLEngine|Viene installato solo il [!INCLUDE[ssDE](../../includes/ssde-md.md)].|  
|/FEATURES=SQLEngine,FullText|Installa il [!INCLUDE[ssDE](../../includes/ssde-md.md)] e il componente full-text.|  
|/FEATURES=SQLEngine,Conn|Vengono installati il [!INCLUDE[ssDE](../../includes/ssde-md.md)] e i componenti di connettività.|  
|/FEATURES=SQLEngine,AS,IS,Conn|Vengono installati il [!INCLUDE[ssDE](../../includes/ssde-md.md)], [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]e i componenti di connettività.|  
|/FEATURES=SQLENGINE,ADVANCEDANALYTICS /IACCEPTROPENLICENSETERMS |Installa il  [!INCLUDE[ssDE](../../includes/ssde-md.md)] e [!INCLUDE[rsql_productname_md](../../includes/rsql-productname-md.md)].|  

  
### <a name="installation-options"></a>Opzione di installazione  
 Il programma di installazione supporta le seguenti opzioni di installazione durante l'installazione di [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] in un sistema operativo Server Core:  
  
1.  **Installazione dalla riga di comando**  
  
     Per installare funzionalità specifiche tramite l'opzione di installazione del prompt dei comandi, utilizzare il parametro /FEATURES e specificare la funzionalità padre o i valori delle funzionalità. Di seguito è riportato un esempio di utilizzo dei parametri dalla riga di comando.  
  
    ```console
    Setup.exe /qs /ACTION=Install /FEATURES=SQLEngine,Replication /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="<DomainName\UserName>" /SQLSVCPASSWORD="<StrongPassword>" /SQLSYSADMINACCOUNTS="<DomainName\UserName>" /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /TCPENABLED=1 /IACCEPTSQLSERVERLICENSETERMS  
    ```  
  
2.  **Installazione tramite il file di configurazione**  
  
     Il programma di installazione supporta l'utilizzo del file di configurazione solo tramite il prompt dei comandi. Il file di configurazione è un file di testo con la struttura di base di un parametro (coppia nome/valore) e un commento descrittivo. L'estensione del file di configurazione specificato al prompt dei comandi deve essere INI. Vedere gli esempi di ConfigurationFile.INI seguenti:  
  
    - Installazione del [!INCLUDE[ssDE](../../includes/ssde-md.md)]. 
    
    L'esempio seguente illustra come installare una nuova istanza autonoma che include il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssDE](../../includes/ssde-md.md)]:  

    ```console
    ; SQL Server Configuration File  
    [OPTIONS]  
    
    ; Specifies a Setup work flow, like INSTALL, UNINSTALL, or UPGRADE. This is a required parameter.   
    
    ACTION="Install"  
    
    ; Specifies features to install, uninstall, or upgrade. The lists of features include SQLEngine, FullText, Replication, AS, IS, and Conn.   
    
    FEATURES=SQLENGINE  
    
    ; Specify a default or named instance. MSSQLSERVER is the default instance for non-Express editions and SQLExpress for Express editions. This parameter is required when installing the ssNoVersion Database Engine, and Analysis Services (AS).  
    
    INSTANCENAME="MSSQLSERVER"  
    
    ; Specify the Instance ID for the ssNoVersion features you have specified. ssNoVersion directory structure, registry structure, and service names will incorporate the instance ID of the ssNoVersion instance.   
    
    INSTANCEID="MSSQLSERVER"  
    
    ; Account for ssNoVersion service: Domain\User or system account.   
    
    SQLSVCACCOUNT="NT Service\MSSQLSERVER"  
    
    ; Windows account(s) to provision as ssNoVersion system administrators.   
    
    SQLSYSADMINACCOUNTS="\<DomainName\UserName>"  
    
    ; Accept the License agreement to continue with Installation  
    
    IAcceptSQLServerLicenseTerms="True"  
    
    ```

    -   Installazione dei componenti di connettività. Nell'esempio seguente viene illustrato come installare i componenti di connettività.  
  
        ```console
        ; SQL Server Configuration File  
        [OPTIONS]  
  
        ; Specifies a Setup work flow, like INSTALL, UNINSTALL, or UPGRADE. This is a required parameter.   
  
        ACTION="Install"  
  
        ; Specifies features to install, uninstall, or upgrade. The lists of features include SQLEngine, FullText, Replication, AS, IS, and Conn.   
  
        FEATURES=Conn  
  
        ; Specifies acceptance of License Terms  
  
        IAcceptSQLServerLicenseTerms="True  
  
        ```  
  
    -   Installazione di tutte le funzionalità supportate  
  
        Nell'esempio seguente viene illustrato come installare tutte le funzionalità di [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] supportate in Server Core.  
  
        ```console
        ; SQL Server Configuration File  
        [OPTIONS]  
        ; Specifies a Setup work flow, like INSTALL, UNINSTALL, or UPGRADE. This is a required parameter.   
  
        ACTION="Install"  
  
        ; Specifies features to install, uninstall, or upgrade. The lists of features include SQLEngine, FullText, Replication, AS, IS, and Conn.   
  
        FEATURES=SQLENGINE,FullText,Replication,AS,IS,Conn  
  
        ; Specify a default or named instance. MSSQLSERVER is the default instance for non-Express editions and SQLExpress for Express editions. This parameter is required when installing the ssNoVersion Database Engine (SQL), or Analysis Services (AS).  
  
        INSTANCENAME="MSSQLSERVER"  
  
        ; Specify the Instance ID for the ssNoVersion features you have specified. ssNoVersion directory structure, registry structure, and service names will incorporate the instance ID of the ssNoVersion instance.   
  
        INSTANCEID="MSSQLSERVER"  
  
        ; Account for ssNoVersion service: Domain\User or system account.   
  
        SQLSVCACCOUNT="NT Service\MSSQLSERVER"  
  
        ; Windows account(s) to provision as ssNoVersion system administrators.   
  
        SQLSYSADMINACCOUNTS="\<DomainName\UserName>"  
  
        ; The name of the account that the Analysis Services service runs under.   
  
        ASSVCACCOUNT= "NT Service\MSSQLServerOLAPService"  
  
        ; Specifies the list of administrator accounts that need to be provisioned.   
  
        ASSYSADMINACCOUNTS="\<DomainName\UserName>"  
  
        ; Specifies the server mode of the Analysis Services instance. Valid values are MULTIDIMENSIONAL, POWERPIVOT or TABULAR. ASSERVERMODE is case-sensitive. All values must be expressed in upper case.   
  
        ASSERVERMODE="MULTIDIMENSIONAL"  
  
        ; Optional value, which specifies the state of the TCP protocol for the ssNoVersion service. Supported values are: 0 to disable the TCP protocol, and 1 to enable the TCP protocol.  
  
        TCPENABLED=1  
  
        ;Specifies acceptance of License Terms  
  
        IAcceptSQLServerLicenseTerms="True"  
        ```  
  
     Di seguito viene descritto come avviare il programma di installazione con un file di configurazione personalizzato o predefinito:  
  
    -   Avviare il programma di installazione con un file di configurazione personalizzato:  
  
         Per specificare il file di configurazione al prompt dei comandi:  
  
        ```console
        Setup.exe /QS /ConfigurationFile=MyConfigurationFile.INI  
        ```  
  
         Per specificare le password al prompt dei comandi anziché nel file di configurazione:  
  
        ```console
        Setup.exe /QS /SQLSVCPASSWORD="************" /ASSVCPASSWORD="************"  /ConfigurationFile=MyConfigurationFile.INI  
        ```  
  
    -   Avviare il programma di installazione con DefaultSetup.ini:  
  
         Se il file DefaultSetup.ini si trova nelle cartelle \x86 e \x64, al livello radice dei supporti di origine di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , aprirlo e aggiungere il parametro *Features* al file.  
  
         Se il file DefaultSetup.ini non esiste, è possibile crearlo e copiarlo nelle cartelle \x86 e \x64, al livello radice dei supporti di origine di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="configure-remote-access-of-ssnoversion-on-server-core"></a>Configurare l'accesso remoto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in Server Core  
 Eseguire le azioni descritte di seguito per configurare l'accesso remoto di un'istanza di [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] in esecuzione in Server Core.  
  
### <a name="enable-remote-connections-on-the-instance-of-ssnoversion"></a>Abilitare connessioni remote nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  

Per abilitare connessioni remote, utilizzare in locale SQLCMD.exe ed eseguire le istruzioni seguenti nell'istanza Server Core:  

```sql
EXEC sys.sp_configure N'remote access', N'1'  
GO
RECONFIGURE WITH OVERRIDE
GO
```  
  
### <a name="enable-and-start-the-ssnoversion-browser-service"></a>Abilitare e avviare il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] browser service  
 Per impostazione predefinita, il servizio Browser è disabilitato.  Se risulta disabilitato in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in esecuzione in Server Core, eseguire il comando riportato di seguito dal prompt dei comandi per abilitarlo:  
  
 `sc config SQLBROWSER start= auto`  
  
 Dopo averlo abilitato, eseguire il comando seguente dal prompt dei comandi per avviarlo:  
  
 `net start SQLBROWSER`  
  
### <a name="create-exceptions-in-windows-firewall"></a>Creazione di eccezioni in Windows Firewall  
 Per creare eccezioni per l'accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in Windows Firewall, seguire i passaggi specificati in [Configurare Windows Firewall per consentire l'accesso a SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
  
### <a name="enable-tcpip-on-the-instance-of-ssnoversion"></a>Abilitare TCP/IP nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Il protocollo TCP/IP può essere abilitato tramite Windows PowerShell per un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in Server Core. A tale scopo, seguire questa procedura:  
  
1.  In PowerShell: Import-Module SQLPS.  
  
2.  Nella finestra di **Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Powershell** eseguire lo script seguente per abilitare il protocollo TCP/IP:  
  
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
  
## <a name="uninstall"></a>Disinstallare

 Dopo aver eseguito l'accesso a un computer in cui è in esecuzione Server Core, si dispone di un ambiente desktop limitato con un prompt dei comandi di amministratore. È possibile usare questo prompt dei comandi per avviare la disinstallazione di [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)]. Per disinstallare un'istanza di [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)], avviare l'operazione dal prompt dei comandi in modalità non interattiva completa tramite il parametro /Q o in modalità non interattiva semplice tramite il parametro /QS. Il parametro /QS consente di tenere traccia dello stato di avanzamento tramite l'interfaccia utente, ma non supporta input. /Q viene eseguito in modalità non interattiva senza alcuna interfaccia utente.  
  
 Per disinstallare un'istanza esistente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
```console
Setup.exe /Q /Action=Uninstall /FEATURES=SQLEngine,AS,IS /INSTANCENAME=MSSQLSERVER  
```  
  
 Per rimuovere un'istanza denominata, specificare il nome dell'istanza anziché il nome `MSSQLSERVER` dell'esempio precedente.  
  
## <a name="start-a-new-command-prompt"></a>Aprire una nuova finestra del prompt dei comandi

Se si chiude il prompt dei comandi inavvertitamente, è possibile avviare un nuovo prompt dei comandi eseguendo la procedura descritta di seguito.  
 
1.  Premere Ctrl+Maiusc+Esc per visualizzare Gestione attività.  
2.  Nella scheda **Applicazioni** fare clic su **Nuova attività**.  
3.  Nella finestra di dialogo **Crea una nuova attività** digitare **cmd** nel campo **Apri** e quindi [!INCLUDE[clickOK](../../includes/clickok-md.md)].  
  
## <a name="see-also"></a>Vedere anche  
 [Installare SQL Server tramite un file di configurazione](./install-sql-server-using-a-configuration-file.md)   
 [Installare SQL Server dal prompt dei comandi](./install-sql-server-from-the-command-prompt.md)   
 [Edizioni e funzionalità supportate di SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md)   
 [Installare Server Core](/windows-server/get-started/getting-started-with-server-core)   
 [Configurare un'installazione Server Core di Windows Server 2016 con Sconfig.cmd](/windows-server/get-started/sconfig-on-ws2016)   
 [Cmdlet del cluster di failover in Windows PowerShell](/powershell/module/failoverclusters/)

  
