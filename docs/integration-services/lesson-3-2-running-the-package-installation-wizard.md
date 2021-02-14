---
description: Lezione 3-2 - Esecuzione dell'Installazione guidata pacchetti
title: "Passaggio 2: Esecuzione dell'Installazione guidata pacchetti | Microsoft Docs"
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: f91fbb89-4626-4c47-b96d-56052dc45861
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 66895007a304ee10878b54f1761b5e72792e1d57
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354947"
---
# <a name="lesson-3-2---running-the-package-installation-wizard"></a>Lezione 3-2 - Esecuzione dell'Installazione guidata pacchetti

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


In questa attività verrà eseguita l'Installazione guidata pacchetti per distribuire i pacchetti del progetto Deployment Tutorial in un'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Nella tabella sysssispackages del database msdb di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] possono essere installati solo i pacchetti. I file di supporto inclusi nel pacchetto di distribuzione verranno installati nel file system.  
  
L'Installazione guidata pacchetti consente di eseguire in modo semplificato i passaggi necessari per installare e configurare i pacchetti. I pacchetti verranno installati in un'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] nel computer di destinazione, ovvero il computer in cui si copia il pacchetto di distribuzione. Verrà inoltre creata la cartella C:\DeploymentTutorialInstall nella quale verranno installati i file non di pacchetto.  
  
In una lezione precedente sono stati modificati i pacchetti inclusi nell'esercitazione per l'utilizzo delle configurazioni. Mediante l'Installazione guidata pacchetti sarà possibile modificare tali configurazioni in modo da consentire l'esecuzione corretta dei pacchetti nell'ambiente di destinazione dell'installazione.  
  
### <a name="to-install-the-packages"></a>Per installare i pacchetti  
  
1.  Individuare il pacchetto di distribuzione nel computer di destinazione.  
  
    Se è stato usato il valore predefinito, ovvero bin\Deployment, come percorso dell'utilità di distribuzione, il pacchetto di distribuzione corrispondente si trova nella cartella Deployment del progetto Deployment Tutorial.  
  
2.  Nella cartella Deployment fare doppio clic sul file manifesto Deployment Tutorial.SSISDeploymentManifest.  
  
3.  Nella pagina iniziale dell'Installazione guidata pacchetti fare clic su **Avanti**.  
  
4.  Nella pagina Distribuzione pacchetti SSIS selezionare l'opzione **Distribuzione in SQL Server** selezionare la casella di controllo **Convalida pacchetti dopo l'installazione** e quindi fare clic su **Avanti**.  
  
5.  Nella pagina Impostazione istanza di SQL Server di destinazione specificare **(local**) nella casella **Nome server** .  
  
6.  Se l'istanza di SQL Server supporta l'autenticazione di Windows, selezionare **Usa autenticazione di Windows**. In caso contrario, selezionare **Usa autenticazione di SQL Server** e specificare nome utente e password.  
  
7.  Verificare che la casella di controllo **Usa l'archiviazione su server per la crittografia** sia deselezionata.  
  
8.  Fare clic su **Avanti.**  
  
9. Nella pagina Selezione cartella di installazione fare clic su **Sfoglia**.  
  
10. Nella finestra di dialogo **Sfoglia cartella** espandere **Risorse del computer** e quindi fare clic su **Disco locale (C:)**.  
  
11. Fare clic su **Crea nuova cartella** e sostituire il nome predefinito della nuova cartella, vale a dire **Nuova cartella**, con **DeploymentTutorialInstall**.  
  
    > [!IMPORTANT]  
    > A tale nome viene fatto riferimento nel valore delle variabili di ambiente utilizzate dalle configurazioni. Il nome della cartella e il riferimento devono corrispondere. In caso contrario non sarà possibile eseguire il pacchetto.  
  
12. Fare clic su **OK**.  
  
13. Nella pagina Selezione cartella di installazione verificare che nella casella Cartella sia indicato il percorso **C:\DeploymentTutorialInstall** e quindi fare clic su **Avanti**.  
  
14. Nella pagina Conferma installazione fare clic su **Avanti**.  
  
    I pacchetti verranno installati. Al termine dell'installazione, verrà aperta la pagina Configurazione pacchetti.  
  
15. Nella pagina Configurazione pacchetti verificare che nella casella **File di configurazione** siano elencati datatransferconfig.dtsconfig e loadxmldataconfig.dtsconfig.  
  
16. Nell'elenco **File di configurazione** fare clic su **datatransferconfig.dtsconfig**, espandere Property nella colonna **Percorso** della casella **Configurazioni** e aggiornare la colonna **Valore** con i valori seguenti:  
  
    |Proprietà|Valore|Valore aggiornato|  
    |------------|---------|-----------------|  
    |\Package.Connections[Deployment Tutorial Log].Properties[ConnectionString]|C:\Programmi\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deploying Packages\Completed Packages\Deployment Tutorial Log|C:\DeploymentTutorialInstall\Deployment Tutorial Log|  
    |\Package.Connections[NewCustomers].Properties[ConnectionString]|C:\Programmi\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deploying Packages\Sample Data\NewCustomers.txt|C:\DeploymentTutorialInstall\NewCustomers.txt|  
  
17. Nell'elenco **File di configurazione** fare clic su loadxmldataconfig.dtsconfig, espandere Property nella colonna **Percorso** della casella **Configurazioni** e aggiornare la colonna **Valore** con i valori seguenti:  
  
    |Proprietà|Valore|Valore aggiornato|  
    |------------|---------|-----------------|  
    |\Package.LoadXMLData.Properties[[XML Source].[XMLData]]|C:\Programmi\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deploying Packages\Sample Data\orders.xml|C:\DeploymentTutorialInstall\orders.xml|  
    |\Package.LoadXMLData.Properties[[XML Source].[XMLSchemaDefinition]]|C:\Programmi\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deploying Packages\Sample Data\orders.xsd|C:\DeploymentTutorialInstall\orders.xsd|  
  
18. Nella pagina di convalida dei pacchetti visualizzare i risultati della convalida di ogni pacchetto installato e quindi fare clic su **Avanti**.  
  
    Poiché i valori delle variabili di ambiente nel computer di destinazione differiscono da quelli delle variabili di ambiente nel computer di sviluppo, nella pagina di convalida dei pacchetti verranno visualizzati diversi avvisi. È probabile che vengano visualizzati quattro avvisi:  
  
    -   Il nome del file di configurazione "C:\DeploymentTutorial\DataTransferConfig.dtsConfig" non è valido. Controllare il nome del file di configurazione.  
  
    -   Impossibile caricare almeno una voce di configurazione per il pacchetto. Controllare le voci di configurazione e gli avvisi precedenti per visualizzare le descrizioni delle voci di configurazione con problemi.  
  
    -   Il nome del file di configurazione "C:\DeploymentTutorial\LoadXMLDataConfig.dtsConfig non è valido. Controllare il nome del file di configurazione.  
  
    -   Impossibile caricare almeno una voce di configurazione per il pacchetto. Controllare le voci di configurazione e gli avvisi precedenti per visualizzare le descrizioni delle voci di configurazione con problemi.  
  
    Questi avvisi non influiscono sull'installazione corretta dei pacchetti.  
  
    Se non si è selezionata l'opzione **Convalida pacchetti dopo l'installazione** nella pagina Distribuzione pacchetti SSIS, le pagine di convalida dei pacchetti non verranno aperte e al termine dell'installazione non verranno visualizzate informazioni relative alla convalida.  
  
19. Nella pagina Completamento Installazione guidata pacchetti leggere il riepilogo dell'installazione e quindi fare clic su **Fine**.  
  
    > [!NOTE]  
    > Verrà creato un file di log temporaneo da utilizzare nella convalida dei pacchetti. Il file non viene utilizzato quando si esegue il pacchetto.  
  
## <a name="next-task-in-lesson"></a>Attività successiva della lezione  
[Passaggio 3: Test dei pacchetti distribuiti](../integration-services/lesson-3-3-testing-the-deployed-packages.md)  
  
## <a name="see-also"></a>Vedere anche  
[Servizio Integration Services &#40;servizio SSIS&#41;](../integration-services/service/integration-services-service-ssis-service.md)  
