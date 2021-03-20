---
title: Abilitare o disabilitare un protocollo di rete del server | Microsoft Docs
description: Informazioni su come abilitare e disabilitare un protocollo di rete del server. Scoprire come usare Gestione configurazione SQL Server o PowerShell per questa attività.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- network protocols [SQL Server], disabling
- remote connections [SQL Server], enabling using Configuration Manager
- protocols [SQL Server], enabling using Configuration Manager
- protocols [SQL Server], disabling using Configuration Manager
- disabling network protocols, Configuration Manager
- network protocols [SQL Server], enabling
- enabling network protocols, Configuration Manager
- surface area configuration [SQL Server], connection protocols
- connections [SQL Server], enabling remote using Configuration Manager
ms.assetid: ec5ccb69-61c9-4576-8843-014b976fd46e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 84058cee37f25877d00a8088e3683fa55fbf256c
ms.sourcegitcommit: 00af0b6448ba58e3685530f40bc622453d3545ac
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2021
ms.locfileid: "104673155"
---
# <a name="enable-or-disable-a-server-network-protocol"></a>Abilitare o disabilitare un protocollo di rete del server
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Tutti i protocolli di rete vengono installati dal programma di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ma è possibile abilitarli o meno. In questo argomento viene descritto come abilitare o disabilitare un protocollo di rete del server in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o PowerShell. Per rendere effettive le modifiche, è necessario arrestare e riavviare il [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
> [!IMPORTANT]  
>  Durante l'installazione di [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] viene aggiunto un account di accesso per il gruppo BUILTIN\Users. In questo modo, tutti gli utenti autenticati del computer possono accedere all'istanza di [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] come un membro del ruolo pubblico. L'account di accesso BUILTIN\Users può essere rimosso in sicurezza per limitare l'accesso del [!INCLUDE[ssDE](../../includes/ssde-md.md)] agli utenti di computer che dispongono di account di accesso singoli o sono membri di altri gruppi di Windows con account di accesso.  
  
> [!WARNING]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e i provider di dati [!INCLUDE[msCoName](../../includes/msconame-md.md)] per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fino alla versione [!INCLUDE[sssql14](../../includes/sssql14-md.md)] supportano solo TLS 1.0 e SSL 3.0 per impostazione predefinita. Se si applica un protocollo diverso, ad esempio TLS 1.1 o TLS 1.2, apportando modifiche nel livello SChannel del sistema operativo, le connessioni a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] potrebbe avere esito negativo se non è stato installato l'aggiornamento appropriato per aggiungere il supporto per TLS 1.1 e 1.2 in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] come descritto <a href="https://support.microsoft.com/help/3135244/tls-1-2-support-for-microsoft-sql-server">qui</a>. A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], tutte le versioni di rilascio di SQL Server includono il supporto di TLS 1.2 e non richiedono aggiornamenti aggiuntivi.
  
 **Contenuto dell'articolo**  
  
-   **Per abilitare e disabilitare un protocollo di rete del server utilizzando:**  
  
     [Gestione configurazione SQL Server](#SSMSProcedure)  
  
     [PowerShell](#PowerShellProcedure)  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> Utilizzo di Gestione configurazione SQL Server  
  
#### <a name="to-enable-a-server-network-protocol"></a>Per abilitare un protocollo di rete del server  
  
1.  Nel riquadro della console di Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] espandere **Configurazione di rete SQL Server**.  
  
2.  Nel riquadro della console fare clic su **Protocolli per**  _\<instance name>_ .  
  
3.  Nel riquadro dei dettagli fare clic con il pulsante destro del mouse sul protocollo da modificare e quindi scegliere **Abilita** o **Disabilita**.  
  
4.  Nel riquadro della console fare clic su **Servizi di SQL Server**.  
  
5.  Nel riquadro dei dettagli fare clic con il pulsante destro del mouse su **SQL Server (** _\<instance name>_ **)** e quindi scegliere **Riavvia** per arrestare e riavviare il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
##  <a name="using-sql-server-powershell"></a><a name="PowerShellProcedure"></a> Utilizzo di SQL Server PowerShell  
  
#### <a name="to-enable-a-server-network-protocol-using-powershell"></a>Per abilitare un protocollo di rete del server utilizzando PowerShell  
  
1.  Aprire un prompt dei comandi con autorizzazioni di amministratore.  
  
2.  Avviare Windows PowerShell dalla barra delle applicazioni oppure fare clic su Start, Tutti i programmi, Accessori, Windows PowerShell e quindi Windows PowerShell.  
  
3.  Importare il modulo **sqlps** immettendo **Import-Module "sqlps"**  
  
4.  Eseguire le seguenti istruzioni per abilitare i protocolli TCP e Named Pipes. Sostituire `<computer_name>` con il nome del computer in cui è in esecuzione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se si configura un'istanza denominata, sostituire `MSSQLSERVER` con il nome dell'istanza.  
  
     Per disabilitare i protocolli, impostare le proprietà `IsEnabled` su `$false`.  
  
    ```  
    $smo = 'Microsoft.SqlServer.Management.Smo.'  
    $wmi = new-object ($smo + 'Wmi.ManagedComputer').  
  
    # List the object properties, including the instance names.  
    $Wmi  
  
    # Enable the TCP protocol on the default instance.  
    $uri = "ManagedComputer[@Name='<computer_name>']/ ServerInstance[@Name='MSSQLSERVER']/ServerProtocol[@Name='Tcp']"  
    $Tcp = $wmi.GetSmoObject($uri)  
    $Tcp.IsEnabled = $true  
    $Tcp.Alter()  
    $Tcp  
  
    # Enable the named pipes protocol for the default instance.  
    $uri = "ManagedComputer[@Name='<computer_name>']/ ServerInstance[@Name='MSSQLSERVER']/ServerProtocol[@Name='Np']"  
    $Np = $wmi.GetSmoObject($uri)  
    $Np.IsEnabled = $true  
    $Np.Alter()  
    $Np  
    ```  
  
#### <a name="to-configure-the-protocols-for-the-local-computer"></a>Per configurare i protocolli per il computer locale  
  
-   Quando lo script viene eseguito localmente e configura il computer locale, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell offre una maggiore flessibilità per lo script determinando dinamicamente il nome del computer locale. Per recuperare il nome del computer locale, sostituire la riga che imposta la variabile `$uri` con la riga seguente.  
  
    ```  
    $uri = "ManagedComputer[@Name='" + (get-item env:\computername).Value + "']/ServerInstance[@Name='MSSQLSERVER']/ServerProtocol[@Name='Tcp']"  
    ```  
  
#### <a name="to-restart-the-database-engine-by-using-sql-server-powershell"></a>Per riavviare il Motore di database tramite SQL Server PowerShell  
  
-   Dopo aver disabilitato o abilitato i protocolli, è necessario arrestare e riavviare il [!INCLUDE[ssDE](../../includes/ssde-md.md)] per rendere effettiva la modifica. Eseguire le istruzioni seguenti per arrestare e avviare l'istanza predefinita tramite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell. Per arrestare e avviare un'istanza denominata, sostituire `'MSSQLSERVER'` con `'MSSQL$<instance_name>'`.  
  
    ```  
    # Get a reference to the ManagedComputer class.  
    CD SQLSERVER:\SQL\<computer_name>  
    $Wmi = (get-item .).ManagedComputer  
    # Get a reference to the default instance of the Database Engine.  
    $DfltInstance = $Wmi.Services['MSSQLSERVER']  
    # Display the state of the service.  
    $DfltInstance  
    # Stop the service.  
    $DfltInstance.Stop();  
    # Wait until the service has time to stop.  
    # Refresh the cache.  
    $DfltInstance.Refresh();   
    # Display the state of the service.  
    $DfltInstance  
    # Start the service again.  
    $DfltInstance.Start();  
    # Wait until the service has time to start.  
    # Refresh the cache and display the state of the service.  
    $DfltInstance.Refresh(); $DfltInstance  
    ```  
  
  
