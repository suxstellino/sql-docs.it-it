---
description: Installare Data Quality Services
title: Installare Data Quality Services
ms.date: 09/11/2017
ms.prod: sql
ms.prod_service: data-quality-services
ms.reviewer: ''
ms.technology: data-quality-services
ms.topic: conceptual
ms.assetid: 486e4216-a946-4c6e-828c-61bc905f7ec1
author: swinarko
ms.author: sawinark
ms.openlocfilehash: c7a91729430078f5913d5f4d83ca75015c50ac15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339907"
---
# <a name="install-data-quality-services"></a>Installare Data Quality Services

[!INCLUDE [SQL Server - Windows only ](../../includes/applies-to-version/sql-windows-only.md)]

  [!INCLUDE[ssDQSnoversionLong](../../includes/ssdqsnoversionlong-md.md)] (DQS) contiene i due componenti seguenti: **[!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)]** e **[!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)]** .  
  
|Componente DQS|Descrizione|  
|-------------------|-----------------|  
|[!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)]|[!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] viene installato nel motore di database di [!INCLUDE[ssNoversion](../../includes/ssNoVersion-md.md)] e include tre database, DQS_MAIN, DQS_PROJECTS e DQS_STAGING_DATA. In DQS_MAIN sono contenuti il motore DQS, le stored procedure DQS e gli articoli della Knowledge Base pubblicati. In DQS_PROJECTS sono contenute le informazioni sul progetto Data Quality. DQS_STAGING_DATA è l'area di gestione temporanea in cui è possibile copiare i dati di origine per eseguire operazioni DQS e, successivamente, esportare i dati elaborati.|  
|[!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)]|[!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)] è un'applicazione autonoma che consente di connettersi a [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)]e offre un'interfaccia utente grafica estremamente intuitiva per effettuare operazioni di qualità dei dati e altre attività amministrative correlate a DQS.|  
  
> [!IMPORTANT]  
>  Oltre ai due componenti DQS sopra indicati, è anche possibile:  
>   
>  Utilizzare la trasformazione DQS Cleansing in Integration Services tramite cui viene eseguita la pulizia dei dati durante il processo di creazione dei pacchetti di Integration Services. Questo componente viene installato automaticamente quando si installa Integration Services. Per informazioni sull'installazione di Integration Services, vedere [Installazione di Integration Services](../../integration-services/install-windows/install-integration-services.md).  
>   
>  Abilitare l'integrazione DQS in Master Data Services per utilizzare la funzionalità DQS corrispondente nel componente aggiuntivo Master Data Services per Excel. Per altre informazioni, vedere [Abilitare l'integrazione di Data Quality Services con Master Data Services](../../master-data-services/install-windows/enable-data-quality-services-integration-with-master-data-services.md).  
  
 L'installazione di DQS è un processo in tre fasi:  
  
-   [Attività di preinstallazione](#PreInstallationTasks): verificare i requisiti di sistema prima di installare DQS.  
  
-   [Attività di installazione di Data Quality Services](#DQSInstallation): installare DQS tramite il programma di installazione di SQL Server.  
  
-   [Attività di post-installazione](#PostInstallationTasks): eseguire queste attività dopo il completamento dell'installazione di SQL Server per completare l'installazione di DQS.  
  
> [!NOTE]  
>  In questo argomento non sono incluse le istruzioni per l'esecuzione dell'installazione dalla riga di comando. Per informazioni sulle opzioni della riga di comando per l'installazione di [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] e del client, vedere [Parametri delle funzionalità](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md#Feature) in [Installazione di SQL Server dal prompt dei comandi](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md).  
  
##  <a name="pre-installation-tasks"></a><a name="PreInstallationTasks"></a> Attività di preinstallazione  
 Prima di installare DQS, assicurarsi che il computer in uso soddisfi i requisiti minimi di sistema. Nella tabella seguente sono fornite informazioni sui requisiti minimi di sistema per i componenti DQS:  
  
|Componente DQS|Requisiti minimi di sistema|  
|-------------------|---------------------------------|  
|[!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)]|Memoria (RAM) minima: 2 GB/Consigliata: almeno 4 GB<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] Motore di database. Per altre informazioni, vedere [Informazioni sul Motore di database di SQL Server](../../database-engine/install-windows/install-sql-server-database-engine.md).|  
|[!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)]|.NET Framework 4.0 (installato durante l'installazione di [!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)] , se non è già presente)<br /><br /> Internet Explorer 6.0 SP1 o versione successiva|  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] e [!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)] possono essere installati nello stesso computer o in computer diversi. Entrambi i componenti inoltre possono essere installati indipendentemente l'uno dall'altro e in qualsiasi sequenza. Per utilizzare [!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)], tuttavia, sarà necessario aver installato [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] a cui eseguire la connessione.  
>   
>  È possibile connettersi alla versione [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] di [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] utilizzando la versione corrente o una precedente di [!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)] e trasformazione DQS Cleansing. Per informazioni sull'aggiornamento della versione esistente di DQS a [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)], vedere [Aggiornare Data Quality Services](../../database-engine/install-windows/upgrade-data-quality-services.md).  
>   
>  Sebbene Microsoft Excel non sia un prerequisito per l'installazione di Client Data Quality, Microsoft Excel 2003 o versione successiva deve essere installato nel computer Client Data Quality per eseguire diverse operazioni nell'applicazione client, ad esempio l'importazione di valori di dominio da un file di Excel o il mapping ai dati di origine in un foglio di Excel per attività di individuazione delle informazioni, pulizia o corrispondenza.  
  
 Per informazioni dettagliate sui requisiti minimi di sistema per l'installazione di [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)], vedere [Requisiti hardware e software per l'installazione di SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md).  
  
##  <a name="data-quality-services-installation-tasks"></a><a name="DQSInstallation"></a> Attività di installazione di Data Quality Services  
 Per installare i componenti di DQS, è necessario utilizzare il programma di installazione di [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] . Quando si esegue il programma di installazione di SQL Server, è necessario scorrere una serie di pagine dell'Installazione guidata per selezionare le opzioni appropriate in base ai requisiti di cui si dispone. Nella tabella seguente sono elencate solo le pagine dell'Installazione guidata le cui opzioni selezionate influiscono sull'installazione di DQS:  
  
|Pagina|Azione|  
|----------|------------|  
|Selezione caratteristiche|Selezionare:<br /><br /> **Data Quality Services** in **Servizi motore di database** per installare [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)]. <br />Se si seleziona la casella di controllo **Data Quality Services** , il programma di installazione di SQL Server copierà il file di installazione DQSInstaller.exe nella directory dell'istanza di SQL Server del computer. È necessario eseguire questo file dopo aver completato l'installazione di SQL Server per *completare* l'installazione di [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] . Inoltre, è necessario eseguire alcuni passaggi aggiuntivi per configurare [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] prima che sia possibile utilizzarlo. Per altre informazioni, vedere [Attività post-installazione](#PostInstallationTasks).<br /><br /> **Client Data Quality** per installare [!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)].<br /><br /> Consigliabile **Strumenti di gestione-completa** in **strumenti di gestione-di base** per installare [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] . Fornisce un'interfaccia utente grafica per gestire l'istanza di SQL Server e supporta l'utente nell'esecuzione di attività aggiuntive di postinstallazione elencate nella sezione successiva.|  
|Configurazione del motore di database|Per aggiungere l'account utente di Windows al ruolo predefinito del server sysadmin, fare clic su **Aggiungi utente corrente** . Questa operazione è richiesta per poter eseguire il file DQSInstaller.exe in un secondo momento per completare l'installazione di [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] .|  
  
##  <a name="post-installation-tasks"></a><a name="PostInstallationTasks"></a> Attività post-installazione  
 Dopo aver completato l'Installazione guidata di SQL Server, è necessario effettuare i passaggi aggiuntivi indicati in questa sezione per completare l'installazione di [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] , configurarlo e infine utilizzarlo.  
  
1.  Per completare l'installazione di [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] , eseguire il file DQSInstaller.exe. Durante l'esecuzione del file DQSInstaller.exe vengono effettuate le attività seguenti:  
  
    -   Creazione dei database DQS_MAIN, DQS_PROJECTS e DQS_STAGING_DATA.  
  
    -   Creazione degli account di accesso ##MS_dqs_db_owner_login## e ##MS_dqs_service_login##.  
  
    -   Creazione dei ruoli dqs_administrator, dqs_kb_editor e dqs_kb_operator nel database DQS_MAIN.  
  
    -   Creazione della stored procedure DQInitDQS_MAIN nel database master.  
  
    -   Il file DQS_install.log viene creato in genere nella cartella C:\Programmi\Microsoft SQL Server\MSSQL13.*<nome_istanza>* \MSSQL\Log. Nel file sono contenute informazioni sulle azioni effettuate durante l'esecuzione del file DQSInstaller.exe.  
  
    -   Se un database Master Data Services è presente nella stessa istanza di SQL Server come [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)], viene creato un utente di cui è stato eseguito il mapping all'account di accesso a Master Data Services e a cui viene concesso il ruolo dqs_administrator nel database DQS_MAIN.  
  
     In questo modo viene completata l'installazione di [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] .  
  
     Per ulteriori informazioni, vedere [eseguire DQSInstaller.exe per completare l'installazione del server Data Quality](../../data-quality-services/install-windows/run-dqsinstaller-exe-to-complete-data-quality-server-installation.md).  
  
2.  Concedere i ruoli DQS agli utenti:  
  
     Per accedere a [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] con [!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)], un utente deve avere uno dei tre ruoli seguenti nel database DQS_MAIN:  
  
    -   **dqs_administrator**  
  
    -   **dqs_kb_editor**  
  
    -   **dqs_kb_operator**  
  
     Per impostazione predefinita, se l'account utente è un membro del ruolo predefinito del server sysadmin, è possibile accedere a [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] utilizzando [!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)] anche se nessuno dei ruoli DQS viene concesso all'account utente. Per informazioni su questi tre ruoli DQS, vedere [DQS Sicurezza](../../data-quality-services/dqs-security.md).  
  
     Per altre informazioni, vedere [Concedere ruoli DQS agli utenti](../../data-quality-services/install-windows/grant-dqs-roles-to-users.md).  
  
    > [!NOTE]  
    >  I tre ruoli DQS non sono disponibili per i database DQS_PROJECTS e DQS_STAGING_DATA.  
  
3.  Rendere disponibili i dati per le operazioni DQS. Assicurarsi che sia possibile accedere ai dati di origine per le operazioni DQS e di poter esportare i dati elaborati in una tabella di un database.  
  
     Per altre informazioni, vedere  
                    [Accedere ai dati per le operazioni DQS](../../data-quality-services/install-windows/access-data-for-the-dqs-operations.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Video: installare e configurare DQS](/previous-versions/dn912438(v=msdn.10))   
 [Aggiornare gli assembly SQLCLR dopo l'aggiornamento di .NET Framework](../../data-quality-services/install-windows/upgrade-sqlclr-assemblies-after-net-framework-update.md)   
 [Esportare e importare le Knowledge base di DQS utilizzando DQSInstaller.exe](../../data-quality-services/install-windows/export-and-import-dqs-knowledge-bases-using-dqsinstaller-exe.md)   
 [Aggiornare Data Quality Services](../../database-engine/install-windows/upgrade-data-quality-services.md)   
 [Rimuovere oggetti server Data Quality Services](../../sql-server/install/remove-data-quality-server-objects.md)   
 [Installare SQL Server funzionalità di Business Intelligence](../../sql-server/install/install-sql-server-business-intelligence-features.md)   
 [Disinstalla SQL Server](../../sql-server/install/uninstall-sql-server.md)   
 [Data Quality Services](../../data-quality-services/data-quality-services.md)   
 [Risolvere i problemi di installazione e configurazione in DQS](https://social.technet.microsoft.com/wiki/contents/articles/3776.aspx)  
  
