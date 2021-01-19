---
description: Servizio Integration Services (servizio SSIS)
title: Servizio Integration Services (servizio SSIS) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssiseditserverregistration.connectionproperties.f1
- sql13.swb.connecttodts.connectionproperties.f1
- sql13.swb.connection.login.dtsserver.f1
- sql13.swb.connecttodts.login.f1
- sql13.swb.connecttodtsserver.login.f1
helpviewer_keywords:
- Integration Services service, about Integration Services service
- SQL Server Integration Services service
- service [Integration Services],about Integration Services service
- service [Integration Services]
- SQL Server Integration Services, service
ms.assetid: 2c785b3b-4a0c-4df7-b5cd-23756dc87842
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a665d78d180d87fe0d412b025bcce75d509285d4
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171203"
---
# <a name="integration-services-service-ssis-service"></a>Servizio Integration Services (servizio SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Negli argomenti contenuti in questa sezione viene illustrato il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , un servizio Windows per la gestione dei pacchetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Questo servizio non è necessario per creare, salvare ed eseguire i pacchetti di Integration Services. [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] supporta il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] per la compatibilità con le versioni precedenti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 A partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] archivia oggetti, impostazioni e dati operativi nel database **SSISDB** per i progetti distribuiti nel server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] usando il modello di distribuzione del progetto. Nel server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , che è un'istanza del motore di database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è ospitato il database. Per altre informazioni sul database, vedere [Catalogo SSIS](../../integration-services/catalog/ssis-catalog.md). Per altre informazioni sulla distribuzione di progetti nel server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], vedere [Distribuire progetti e pacchetti di Integration Services (SSIS)](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).  
  
## <a name="management-capabilities"></a>Funzionalità di gestione  
 Il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] è un servizio Windows per la gestione di pacchetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] è disponibile solo in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 Il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] consente di eseguire le attività di gestione seguenti:  
  
-   Avvio di pacchetti locali o archiviati in una posizione remota  
  
-   Arresto di pacchetti eseguiti localmente o in modalità remota  
  
-   Monitoraggio di pacchetti eseguiti localmente o in modalità remota  
  
-   Importazione ed esportazione di pacchetti  
  
-   Gestione dell'archiviazione di pacchetti  
  
-   Personalizzazione delle cartelle di archiviazione  
  
-   Arresto dei pacchetti in esecuzione quando il servizio viene arrestato  
  
-   Visualizzazione del registro eventi di Windows  
  
-   Connessione a più server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]  
  
## <a name="startup-type"></a>Tipo di avvio
 Il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] viene installato quando si installa il componente [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per impostazione predefinita, il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] viene avviato in base al tipo di avvio automatico. È necessario che sia in esecuzione se si desidera eseguire il monitoraggio dei pacchetti archiviati nell'archivio pacchetti [!INCLUDE[ssIS](../../includes/ssis-md.md)] . L'archivio pacchetti [!INCLUDE[ssIS](../../includes/ssis-md.md)] può essere il database msdb di un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o le cartelle designate del file system.  
  
 Il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] non è necessario se si desidera semplicemente progettare ed eseguire pacchetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . È invece necessario per elencare e monitorare i pacchetti tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  

## <a name="manage-the-service"></a>Gestire il servizio
  
 Quando viene installato il componente [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], viene installato anche il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Per impostazione predefinita, il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] viene avviato in base al tipo di avvio automatico. È tuttavia necessario installare [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] per utilizzare il servizio per la gestione di pacchetti [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] archiviati e in esecuzione.  
  
> [!NOTE]
> Per connettersi direttamente a un'istanza del servizio Integration Services legacy, è necessario usare la versione di SQL Server Management Studio (SSMS) allineata alla versione di SQL Server in cui è in esecuzione il servizio Integration Services. Ad esempio, per connettersi al servizio Integration Services legacy in esecuzione in un'istanza di SQL Server 2016, è necessario usare la versione di SQL Server Management Studio rilasciata per SQL Server 2016. [Scaricare SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).
>
>   Nella finestra di dialogo **Connetti al server** di SQL Server Management Studio non è possibile specificare il nome di un server sul quale è in esecuzione una versione precedente del servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Per gestire i pacchetti archiviati in un server remoto, tuttavia, non è necessario connettersi all'istanza del servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] nel server remoto in questione. Modificare, invece, il file di configurazione per il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] in modo che in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] vengano visualizzati i pacchetti archiviati nel server remoto.   
  
 È possibile installare solo una singola istanza del servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] in un computer. Il servizio non è specifico di una particolare istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Connettersi al servizio utilizzando il nome del computer sul quale è in esecuzione il servizio.  
  
 Per la gestione del servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] è possibile usare uno dei seguenti snap-in di Microsoft Management Console (MMC): Gestione configurazione SQL Server o Servizi di SQL Server. Per gestire pacchetti in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], è prima di tutti necessario assicurarsi che il servizio sia avviato.  
  
 Per impostazione predefinita, il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] è configurato per gestire i pacchetti archiviati nel database msdb dell'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] installata in contemporanea con [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Se contemporaneamente non viene installata alcuna istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] , il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] è configurato per gestire i pacchetti contenuti nel database msdb dell'istanza predefinita locale del [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Per gestire pacchetti archiviati in un'istanza denominata o un'istanza remota del [!INCLUDE[ssDE](../../includes/ssde-md.md)]o in più istanze del [!INCLUDE[ssDE](../../includes/ssde-md.md)], è necessario modificare il file di configurazione per il servizio.
  
 Per impostazione predefinita, il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] è configurato in modo che i pacchetti in esecuzione vengano arrestati all'arresto del servizio. Il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , tuttavia, non attende che i pacchetti vengano arrestati e l'esecuzione di alcuni pacchetti potrebbe continuare dopo l'arresto del servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 Se il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] viene arrestato, è possibile continuare a eseguire pacchetti tramite l'Importazione/Esportazione guidata [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] , l'Utilità di esecuzione pacchetti e l'utilità del prompt dei comandi **dtexec** (dtexec.exe). Non è tuttavia possibile eseguire il monitoraggio dei pacchetti in esecuzione.  
  
 Per impostazione predefinita, il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] viene eseguito nel contesto dell'account NETWORK SERVICE. È consigliabile eseguire il servizio SQL Server Integration Services con un account con autorizzazioni limitate, ad esempio l'account NETWORK SERVICE. L'esecuzione del servizio SQL Server Integration Services con un account con privilegi elevati rappresenta un potenziale rischio per la sicurezza. 
  
 Il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] esegue registrazioni nel registro eventi di Windows. È possibile visualizzare gli eventi del servizio in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. È anche possibile visualizzare gli eventi del servizio utilizzando il Visualizzatore eventi di Windows.  
  
## <a name="set-the-properties-of-the-service"></a>Impostare le proprietà del servizio
  
 Il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] consente di gestire e monitorare i pacchetti in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Alla prima installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] viene avviato e impostato per l'avvio automatico.  
  
 Dopo l'installazione del servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , è possibile impostare le proprietà del servizio usando Gestione configurazione SQL Server o lo snap-in MMC Servizi.  
  
 Per configurare le altre importanti funzionalità del servizio, inclusi i percorsi in cui vengono memorizzati e gestiti i pacchetti, è necessario modificare il file di configurazione del servizio.
  
### <a name="to-set-properties-of-the-integration-services-service-by-using-sql-server-configuration-manager"></a>Per impostare le proprietà del servizio Integration Services tramite Gestione configurazione SQL Server  
  
1.  Fare clic sul menu **Start** , scegliere **Tutti i programmi**, **Microsoft SQL Server**, **Strumenti di configurazione** e quindi fare clic su **Gestione configurazione SQL Server**.  
  
2.  Nello snap-in **Gestione configurazione SQL Server** individuare **SQL Server Integration Services** nell'elenco dei servizi, fare clic con il pulsante destro del mouse su **SQL Server Integration Services** e quindi scegliere **Proprietà**.  
  
3.  Nella finestra di dialogo **Proprietà di SQL Server Integration Services** è possibile eseguire una delle operazioni seguenti:  
  
    -   Fare clic sulla scheda **Connessione** per visualizzare le informazioni di accesso, come il nome dell'account.  
  
    -   Fare clic sulla scheda **Servizio** per visualizzare le informazioni relative al servizio, ad esempio il nome del computer host e per specificare la modalità di avvio del servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
        > [!NOTE]  
        >  Nella scheda **Avanzate** non sono disponibili informazioni relative al servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
4.  Fare clic su **OK**.  
  
5.  Scegliere **Esci** dal menu **File** per chiudere lo snap-in **Gestione configurazione SQL Server** .  
  
### <a name="to-set-properties-of-the-integration-services-service-by-using-services"></a>Per impostare le proprietà del servizio Integration Services tramite i Servizi  
  
1.  Nel **Pannello di controllo** fare clic su **Strumenti di amministrazione** nella visualizzazione classica oppure su **Prestazioni e manutenzione** e quindi su **Strumenti di amministrazione** nella visualizzazione per categorie.  
  
2.  Fare clic su **Servizi**.  
  
3.  Nello snap-in **Servizi** individuare **SQL Server Integration Services** nell'elenco dei servizi, fare clic con il pulsante destro del mouse su **SQL Server Integration Services** e quindi scegliere **Proprietà**.  
  
4.  Nella finestra di dialogo **Proprietà di SQL Server Integration Services** è possibile eseguire una delle operazioni seguenti:  
  
    -   Fare clic sulla scheda **General** (Generale). Per abilitare il servizio, selezionare il tipo di avvio manuale o automatico. Per disabilitarlo, nella casella **Tipo di avvio** selezionare Disabilita. Quando si seleziona Disabilita, il servizio non viene arrestato se è in esecuzione.  
  
         Se il servizio è già abilitato, è possibile fare clic su **Arresta** per arrestarlo oppure su **Avvia** per avviarlo.  
  
    -   Fare clic sulla scheda **Accedi** per visualizzare o modificare le informazioni relative all'accesso.  
  
    -   Fare clic sulla scheda **Recupero** per visualizzare le risposte predefinite del computer agli errori del servizio. È possibile modificare queste opzioni in base all'ambiente specifico.  
  
    -   Fare clic sulla scheda **Dipendenze** per visualizzare un elenco dei servizi dipendenti. Il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] non ha alcuna dipendenza.  
  
5.  Fare clic su **OK**.  
  
6.  Facoltativamente, se è stato impostato il tipo di avvio manuale o automatico, è possibile fare clic con il pulsante destro del mouse su **SQL Server Integration Services** e quindi scegliere **Avvia, Arresta o Riavvia**.  
  
7.  Scegliere **Esci** dal menu **File** per chiudere lo snap-in **Servizi** .  

## <a name="grant-permissions-to-the-service"></a>Concedere autorizzazioni al servizio
  Nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], per impostazione predefinita quando si installa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tutti gli utenti nel gruppo Utenti dispongono di accesso al servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Quando si installa la versione corrente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], gli utenti non dispongono di accesso al servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Il servizio è protetto per impostazione predefinita. Dopo avere installato [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , l'amministratore deve concedere l'accesso al servizio.  
  
### <a name="to-grant-access-to-the-integration-services-service"></a>Per concedere l'accesso al servizio Integration Services  
  
1.  Eseguire Dcomcnfg.exe. Dcomcnfg.exe fornisce un'interfaccia utente per la modifica di determinate impostazioni del Registro di sistema.  
  
2.  Nella finestra di dialogo **Servizi componenti** espandere il nodo > Computer > Risorse del computer > Config DCOM.  
  
3.  Fare clic con il pulsante destro del mouse su **Microsoft SQL Server Integration Services 13.0** e quindi scegliere **Proprietà**.  
  
4.  Nella scheda **Sicurezza** della finestra **Autorizzazioni di esecuzione e attivazione** fare clic su **Modifica** .  
  
5.  Aggiungere utenti e assegnare le autorizzazioni appropriate, quindi scegliere OK.  
  
6.  Ripetere i passaggi 4 e 5 per le autorizzazioni di accesso.  
  
7.  Riavviare SQL Server Management Studio.  
  
8.  Riavviare il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  

### <a name="event-logged-when-permissions-are-missing"></a>Evento registrato quando mancano le autorizzazioni

Se l'account del servizio di SQL Server Agent non dispone delle **[autorizzazioni di esecuzione e attivazione]** DCOM per Integration Services, l'evento seguente viene aggiunto al log eventi del sistema quando SQL Server Agent esegue i processi del pacchetto SSIS:

```
Log Name: System
Source: **Microsoft-Windows-DistributedCOM**
Date: 1/9/2019 5:42:13 PM
Event ID: **10016**
Task Category: None
Level: Error
Keywords: Classic
User: NT SERVICE\SQLSERVERAGENT
Computer: testmachine
Description:
The application-specific permission settings do not grant Local Activation permission for the COM Server application with CLSID
{xxxxxxxxxxxxxxxxxxxxxxxxxxxxx}
and APPID
{xxxxxxxxxxxxxxxxxxxxxxxxxxxxx}
to the user NT SERVICE\SQLSERVERAGENT SID (S-1-5-80-344959196-2060754871-2302487193-2804545603-1466107430) from address LocalHost (Using LRPC) running in the application container Unavailable SID (Unavailable). This security permission can be modified using the Component Services administrative tool.
```

## <a name="configure-the-service"></a>Configurare il servizio
 
Durante l'installazione di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]viene creato e installato il file di configurazione per il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , che contiene le impostazioni seguenti:  
  
-   All'arresto del servizio ai pacchetti viene inviato un comando di arresto.  
  
-   Le cartelle radice da visualizzare per [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] nella finestra Esplora oggetti di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] sono le cartelle MSDB e File System.  
  
-   I pacchetti nel file system gestiti dal servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] si trovano in %Programmi%\Microsoft SQL Server\130\DTS\Packages.  
  
 In questo file di configurazione è inoltre specificato quale database msdb contiene i pacchetti che verranno gestiti dal servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Per impostazione predefinita, il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] è configurato per gestire i pacchetti archiviati nel database msdb dell'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] installata in contemporanea con [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Se contemporaneamente non viene installata alcuna istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] , il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] è configurato per gestire i pacchetti contenuti nel database msdb dell'istanza predefinita locale del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
### <a name="default-configuration-file-example"></a>Esempio di file di configurazione predefinito  
 Nell'esempio seguente è riportato un file di configurazione predefinito in cui sono specificate le impostazioni seguenti:  
  
-   Arresto dei pacchetti in esecuzione quando viene arrestato il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
-   Le cartelle radice per l'archiviazione dei pacchetti in [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sono le cartelle MSDB e File system.  
  
-   Il servizio gestisce i pacchetti archiviati nel database msdb dell'istanza predefinita locale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   I pacchetti archiviati nel file system nella cartella Pacchetti sono gestiti dal servizio.  
  
 **Esempio di un file di configurazione predefinito**  
  
```xml
\<?xml version="1.0" encoding="utf-8"?>  
\<DtsServiceConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  <StopExecutingPackagesOnShutdown>true</StopExecutingPackagesOnShutdown>  
  <TopLevelFolders>  
    \<Folder xsi:type="SqlServerFolder">  
      <Name>MSDB</Name>  
      <ServerName>.</ServerName>  
    </Folder>  
    \<Folder xsi:type="FileSystemFolder">  
      <Name>File System</Name>  
      <StorePath>..\Packages</StorePath>  
    </Folder>  
  </TopLevelFolders>    
</DtsServiceConfiguration>  
```  
  
### <a name="modify-the-configuration-file"></a>Modificare il file di configurazione  
 È possibile modificare il file di configurazione in modo da consentire l'esecuzione dei pacchetti anche quando il servizio viene arrestato, visualizzare cartelle radice aggiuntive in Esplora oggetti oppure specificare un'altra cartella o cartelle aggiuntive nel file system da gestire tramite il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . È possibile, ad esempio, creare cartelle radice aggiuntive di tipo **SqlServerFolder** per gestire i pacchetti nei database msdb di istanze aggiuntive del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
> [!NOTE]  
>  Alcuni caratteri non sono validi per i nomi delle cartelle. I caratteri validi per i nomi delle cartelle sono determinati dalla classe [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]**System.IO.Path** e dal campo **GetInvalidFilenameChars** . Il campo **GetInvalidFilenameChars** fornisce una matrice specifica della piattaforma che contiene i caratteri che non è possibile specificare negli argomenti delle stringhe dei percorsi passati ai membri della classe **Path** . Il set di caratteri non validi può variare in base al file system. Caratteri non validi sono in genere le virgolette ("), il carattere minore di (<) e la barra verticale (|).  
  
 Per gestire i pacchetti archiviati in un'istanza denominata o remota del [!INCLUDE[ssDE](../../includes/ssde-md.md)], è tuttavia necessario modificare il file di configurazione. Se non si aggiorna il file di configurazione, non sarà possibile usare **Esplora oggetti** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] per visualizzare i pacchetti archiviati nel database msdb nell'istanza denominata o remota. Se si tenta di utilizzare **Esplora oggetti** per visualizzare questi pacchetti, verrà visualizzato il messaggio di errore seguente:  
  
 `Failed to retrieve data for this request. (Microsoft.SqlServer.SmoEnum)`  
  
 `The SQL Server specified in Integration Services service configuration is not present or is not available. This might occur when there is no default instance of SQL Server on the computer. For more information, see the topic "Configuring the Integration Services Service" in SQL Server 2008 Books Online.`  
  
 `Login Timeout Expired`  
  
 `An error has occurred while establishing a connection to the server. When connecting to SQL Server 2008, this failure may be caused by the fact that under the default settings SQL Server does not allow remote connections.`  
  
 `Named Pipes Provider: Could not open a connection to SQL Server [2]. (MsDtsSvr).`  
  
 Per modificare il file di configurazione per il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , usare un editor di testo.  
  
> [!IMPORTANT]  
>  Al termine della modifica del file di configurazione del servizio, è necessario riavviare il servizio in modo che utilizzi la configurazione aggiornata.  
  
### <a name="modified-configuration-file-example"></a>Esempio di file di configurazione modificato  
 Nell'esempio seguente viene illustrato un file di configurazione modificato per [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Questo file è per un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] denominata `InstanceName` su un server denominato `ServerName`.  
  
 **Esempio di un file di configurazione modificato per un'istanza denominata di SQL Server**  
  
```xml
\<?xml version="1.0" encoding="utf-8"?>  
\<DtsServiceConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  <StopExecutingPackagesOnShutdown>true</StopExecutingPackagesOnShutdown>  
  <TopLevelFolders>  
    \<Folder xsi:type="SqlServerFolder">  
      <Name>MSDB</Name>  
      <ServerName>ServerName\InstanceName</ServerName>  
    </Folder>  
    \<Folder xsi:type="FileSystemFolder">  
      <Name>File System</Name>  
      <StorePath>..\Packages</StorePath>  
    </Folder>  
  </TopLevelFolders>    
</DtsServiceConfiguration>  
```  
  
### <a name="modify-the-configuration-file-location"></a>Modificare il percorso del file di configurazione  
 La chiave del Registro di sistema **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\SSIS\ServiceConfigFile** specifica il percorso e il nome del file di configurazione usato dal servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Il valore predefinito della chiave del Registro di sistema è **C:\Programmi\Microsoft SQL Server\130\DTS\Binn\ MsDtsSrvr.ini.xml**. È possibile aggiornare il valore della chiave del Registro di sistema per utilizzare un nome e un percorso diversi per il file di configurazione. Si noti che il numero di versione nel percorso (120 per SQL Server [!INCLUDE[ssSQL14_md](../../includes/sssql14-md.md)], 130 per [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e così via) varia in base alla versione di SQL Server.
  
> [!CAUTION]  
>  La modifica non corretta del Registro di sistema può causare seri problemi che potrebbero richiedere la reinstallazione del sistema operativo. [!INCLUDE[msCoName](../../includes/msconame-md.md)] non può garantire che i problemi causati dalla modifica non corretta del Registro di sistema possano essere risolti. Prima di modificare il Registro di sistema, eseguire il backup dei dati importanti. Per informazioni sul backup, sul ripristino e sulla modifica del Registro di sistema, vedere l'articolo di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Knowledge Base relativo alla [descrizione del Registro di sistema di Microsoft Windows](https://support.microsoft.com/kb/256986).  
  
 Il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] carica il file di configurazione al momento dell'avvio. Qualsiasi modifica alla voce del Registro di sistema richiede il riavvio del servizio.  

## <a name="connect-to-the-local-service"></a>Connettersi al servizio locale
  Prima di connettersi al servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , l'amministratore deve concedere l'accesso al servizio. 
  
### <a name="to-connect-to-the-integration-services-service"></a>Per connettersi al servizio Integration Services  
  
1.  Aprire [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
2.  Scegliere **Esplora oggetti** dal menu **Visualizza** .  
  
3.  Sulla barra degli strumenti di Esplora oggetti fare clic su **Connetti** e quindi su **Integration Services**.  
  
4.  Nella finestra di dialogo **Connetti al server** specificare un nome di server. Per specificare il server locale, è possibile digitare un punto (.), (locale) o **localhost** .  
  
5.  Fare clic su **Connetti**.  

## <a name="connect-to-a-remote-ssis-server"></a>Connettersi a un server SSIS remoto
  
 Per la connessione a un'istanza di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] in un server remoto da [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o da un'altra applicazione di gestione, è necessario un set specifico di diritti nel server per gli utenti dell'applicazione.  
  
> [!IMPORTANT]
> Per connettersi direttamente a un'istanza del servizio Integration Services legacy, è necessario usare la versione di SQL Server Management Studio (SSMS) allineata alla versione di SQL Server in cui è in esecuzione il servizio Integration Services. Ad esempio, per connettersi al servizio Integration Services legacy in esecuzione in un'istanza di SQL Server 2016, è necessario usare la versione di SQL Server Management Studio rilasciata per SQL Server 2016. [Scaricare SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).
>
>  Per gestire i pacchetti archiviati in un server remoto, non è necessario connettersi all'istanza del servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sul server remoto in questione. Modificare, invece, il file di configurazione per il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] in modo che in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] vengano visualizzati i pacchetti archiviati nel server remoto.
  
### <a name="connecting-to-integration-services-on-a-remote-server"></a>Connessione a Integration Services in un server remoto  
  
#### <a name="to-connect-to-integration-services-on-a-remote-server"></a>Per connettersi a Integration Services in un server remoto  
  
1.  Aprire [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
2.  Scegliere **Connetti Esplora oggetti** dal menu **File** per visualizzare la finestra di dialogo **Connetti al server** .  
  
3.  Nell'elenco **Tipo server** selezionare **Integration Services** .  
  
4.  Nella casella di testo **Nome server** digitare il nome di un server [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
    > [!NOTE]  
    >  Il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] non è specifico dell'istanza. Connettersi al servizio utilizzando il nome del computer sul quale è in esecuzione Integration Services.  
  
5.  Fare clic su **Connetti**.  
  
> [!NOTE]  
>  Nella finestra di dialogo **Cerca server** non vengono visualizzate le istanze remote di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Le opzioni disponibili nella scheda **Opzioni di connessione** della finestra di dialogo **Connetti al server** , visualizzata facendo clic sul pulsante **Opzioni** , non sono inoltre applicabili alle connessioni a [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
### <a name="eliminating-the-access-is-denied-error"></a>Eliminazione dell'errore "Accesso negato"  
 Quando un utente che non dispone di diritti sufficienti tenta di connettersi a un'istanza di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] in un server remoto, viene visualizzato un errore di accesso negato. È possibile evitare la visualizzazione di questo messaggio di errore verificando che gli utenti dispongano delle autorizzazioni DCOM necessarie.  
  
#### <a name="to-configure-rights-for-remote-users-on-windows-server-2003-or-windows-xp"></a>Per configurare i diritti per gli utenti remoti in Windows Server 2003 o Windows XP  
  
1.  Se l'utente non è membro del gruppo Administrators locale, aggiungerlo al gruppo di utenti DCOM. A questo scopo, è possibile usare lo snap-in MMC Gestione computer, a cui è possibile accedere dal menu **Strumenti di amministrazione** .  
  
2.  Aprire il Pannello di controllo, fare clic su **Strumenti di amministrazione** e quindi fare doppio clic su **Servizi componenti** per avviare lo snap-in MMC Servizi componenti.  
  
3.  Espandere il nodo **Servizi componenti** nel riquadro a sinistra della console. Espandere il nodo **Computer** , espandere il nodo **Risorse del computer**, quindi fare clic sul nodo **Config DCOM** .  
  
4.  Selezionare il nodo **Config DCOM** , quindi selezionare SQL Server Integration Services 11.0 nell'elenco di applicazioni che è possibile configurare.  
  
5.  Fare clic con il pulsante destro del mouse su SQL Server Integration Services 11.0 e scegliere **Proprietà**.  
  
6.  Nella finestra di dialogo **Proprietà - SQL Server Integration Services 11.0** selezionare la scheda **Sicurezza** .  
  
7.  In **Autorizzazioni di esecuzione e attivazione** selezionare **Personalizza**, quindi fare clic su **Modifica** per aprire la finestra di dialogo **Autorizzazione di avvio** .  
  
8.  Nella finestra di dialogo **Autorizzazione di avvio** aggiungere o eliminare utenti e assegnare le autorizzazioni appropriate agli utenti e ai gruppi desiderati. Le autorizzazioni disponibili sono Avvio locale, Avvio remoto, Attivazione locale e Attivazione remota. I diritti di avvio consentono di concedere o negare l'autorizzazione per l'avvio e l'arresto del servizio, mentre quelli di attivazione consentono di concedere o negare l'autorizzazione per la connessione al servizio.  
  
9. Fare clic su OK per chiudere la finestra di dialogo.  
  
10. In **Autorizzazioni di accesso**, ripetere i passaggi 7 e 8 per assegnare le autorizzazioni appropriate a utenti e gruppi.  
  
11. Chiudere lo snap-in MMC.  
  
12. Riavviare il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
#### <a name="to-configure-rights-for-remote-users-on-windows-2000-with-the-latest-service-packs"></a>Per configurare i diritti per gli utenti remoti in Windows 2000 con il Service Pack più recente  
  
1.  Eseguire **dcomcnfg.exe** al prompt dei comandi.  
  
2.  Nella pagina **Applicazioni** della finestra di dialogo **Proprietà - Configurazione Distributed COM** selezionare SQL Server Integration Services 11.0, quindi fare clic su **Proprietà**.  
  
3.  Selezionare la pagina **Sicurezza** .  
  
4.  Utilizzare le due finestre di dialogo separate per la configurazione delle **Autorizzazioni di accesso** e delle **Autorizzazioni di avvio**. Non è possibile fare distinzione tra l'accesso remoto e quello locale. Le autorizzazioni di accesso includono l'accesso remoto e locale e quelle di avvio includono l'avvio remoto e locale.  
  
5.  Chiudere le finestre di dialogo e **dcomcnfg.exe**.  
  
6.  Riavviare il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
### <a name="connecting-by-using-a-local-account"></a>Connessione tramite un account locale

Se si utilizza un account di Windows locale in un computer client, è possibile connettersi al servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] in un computer remoto solo se nel computer remoto è presente un account locale con lo stesso nome, la stessa password e i diritti appropriati.  
  
### <a name="ssis-windows-service-doesnt-support-delegation"></a>Il servizio di Windows SSIS non supporta la delega

SSIS non supporta la delega delle credenziali, a volte definita doppio hop. In questo scenario si usa un computer client, SSIS è installato in un secondo computer e SQL Server è installato in un terzo computer. Sebbene tramite SSMS le credenziali vengano passate correttamente dal computer client al secondo computer in cui è in esecuzione SSIS, tramite SSIS non è possibile passare le credenziali dal secondo computer al terzo computer in cui è in esecuzione SQL Server.

## <a name="configure-the-firewall"></a>Configurare il firewall
  
 Il sistema Windows Firewall impedisce a utenti non autorizzati di accedere alle risorse del computer tramite una connessione di rete. Per accedere a un'istanza di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] tramite questo firewall, è necessario configurarlo in modo da consentire l'accesso.  
  
> [!IMPORTANT]  
>  Per gestire i pacchetti archiviati in un server remoto, non è necessario connettersi all'istanza del servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sul server remoto in questione. Modificare, invece, il file di configurazione per il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] in modo che in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] vengano visualizzati i pacchetti archiviati nel server remoto.
  
 Il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] usa il protocollo DCOM.
  
 Sono disponibili numerosi sistemi firewall. Se si esegue un firewall diverso da Windows Firewall, consultare la documentazione del firewall per informazioni specifiche sul sistema in uso.  
  
 Se il firewall supporta filtri a livello di applicazione, è possibile utilizzare l'interfaccia utente disponibile in Windows per specificare le eccezioni consentite, ovvero i programmi e i servizi che non verranno bloccati dal firewall. In caso contrario, sarà necessario configurare DCOM per l'utilizzo di un set limitato di porte TCP. Nel sito Web Microsoft per cui è disponibile un collegamento più indietro in questo argomento sono disponibili informazioni sulla procedura per l'impostazione delle porte TCP da utilizzare.  
  
 Il servizio Integration Services utilizza la porta 135 e tale porta non è modificabile. È necessario aprire la porta TCP 135 per l'accesso a Gestione controllo servizi. Gestione controllo servizi esegue operazioni come l'avvio e l'arresto dei servizi [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] e la trasmissione di richieste di controllo al servizio in esecuzione.  
  
 Le informazioni nella sezione seguente sono specifiche per Windows Firewall. È possibile configurare il sistema Windows Firewall tramite l'esecuzione di comandi dal prompt dei comandi oppure tramite l'impostazione delle proprietà desiderate nella finestra di dialogo Windows Firewall.  
  
 Per altre informazioni sulle impostazioni predefinite di Windows Firewall e per una descrizione delle porte TCP che interessano il motore di database, Analysis Services, Reporting Services e Integration Services, vedere [Configurare Windows Firewall per consentire l'accesso a SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
  
### <a name="configuring-a-windows-firewall"></a>Configurazione di Windows Firewall  
 È possibile utilizzare i comandi seguenti per aprire la porta TCP 135, aggiungere MsDtsSrvr.exe all'elenco delle eccezioni e specificare l'ambito di sblocco del firewall.  
  
#### <a name="to-configure-a-windows-firewall-using-the-command-prompt-window"></a>Per configurare Windows Firewall da una finestra del prompt dei comandi  
  
1.  Eseguire il comando seguente:

    ```dos
    netsh firewall add portopening protocol=TCP port=135 name="RPC (TCP/135)" mode=ENABLE scope=SUBNET
    ```
  
2.  Eseguire il comando seguente:

    ```dos
    netsh firewall add allowedprogram program="%ProgramFiles%\Microsoft SQL Server\100\DTS\Binn\MsDtsSrvr.exe" name="SSIS Service" scope=SUBNET
    ```
  
    > [!NOTE]  
    >  Per aprire il firewall per tutti i computer, inclusi quelli su Internet, sostituire scope=SUBNET con scope=ALL.  
  
 Nell'argomento seguente viene descritta la procedura per aprire la porta TCP 135, aggiungere MsDtsSrvr.exe all'elenco delle eccezioni e specificare l'ambito di sblocco del firewall tramite l'interfaccia utente di Windows.  
  
#### <a name="to-configure-a-firewall-using-the-windows-firewall-dialog-box"></a>Per configurare il firewall nella finestra di dialogo Windows Firewall  
  
1.  Nel Pannello di controllo fare doppio clic su **Windows Firewall**.  
  
2.  Nella finestra di dialogo **Windows Firewall** fare clic sulla scheda **Eccezioni** e quindi su **Aggiungi programma**.  
  
3.  Nella finestra di dialogo **Aggiungi programma** fare clic su **Sfoglia**, passare alla cartella Programmi\Microsoft SQL Server\100\DTS\Binn, fare clic su MsDtsSrvr.exe e quindi scegliere **Apri**. Fare clic su **OK** per chiudere la finestra di dialogo **Aggiungi programma** .  
  
4.  Nella scheda **Eccezioni** fare clic su **Aggiungi porta**.  
  
5.  Nella finestra di dialogo **Aggiungi porta** digitare **RPC(TCP/135)** o un altro nome descrittivo nella casella **Nome**, digitare **135** nella casella **Numero porta** e quindi selezionare **TCP**.  
  
    > [!IMPORTANT]  
    >  [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] Il servizio usa sempre la porta 135. Non è possibile specificare una porta diversa.  
  
6.  Nella finestra di dialogo **Aggiungi porta** è possibile scegliere facoltativamente **Cambia ambito** per modificare l'ambito predefinito.  
  
7.  Nella finestra di dialogo **Cambia ambito** selezionare **Solo la rete (subnet) locale** oppure digitare un elenco personalizzato e quindi fare clic su **OK**.  
  
8.  Per chiudere la finestra di dialogo **Aggiungi porta** , fare clic su **OK**.  
  
9. Per chiudere la finestra di dialogo **Windows Firewall** , fare clic su **OK**.  
  
    > [!NOTE]  
    >  Per configurare Windows Firewall, questa procedura usa l'elemento **Windows Firewall** nel Pannello di controllo. L'elemento **Windows Firewall** configura il firewall solo per il profilo del percorso di rete corrente. È tuttavia possibile configurare Windows Firewall tramite lo strumento della riga di comando **netsh** o lo snap-in [!INCLUDE[msCoName](../../includes/msconame-md.md)] Management Console (MMC) denominato Windows Firewall con sicurezza avanzata. Per altre informazioni sull'esecuzione di questa operazione, vedere [Configurare Windows Firewall per consentire l'accesso a SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
