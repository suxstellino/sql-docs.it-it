---
description: Lezione 3-3 - Test dei pacchetti distribuiti
title: 'Passaggio 3: Test dei pacchetti distribuiti | Microsoft Docs'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 9159da3f-c9ca-4015-9e85-3bf4373a1349
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 96a06d721c1ccbb4ce254705ceb676c1c338b68a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354936"
---
# <a name="lesson-3-3---testing-the-deployed-packages"></a>Lezione 3-3 - Test dei pacchetti distribuiti

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


In questa attività si procederà al test dei pacchetti distribuiti in un'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
In altre esercitazioni di [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] vengono eseguiti i pacchetti in [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)], l'ambiente di sviluppo per [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)], usando l'opzione **Avvia debug** del menu **Debug** . In questa esercitazione i pacchetti verranno eseguiti in modo diverso.  
  
[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] offre diversi strumenti che è possibile usare per eseguire pacchetti nell'ambiente di test e in quello di produzione, ovvero l'utilità del prompt dei comandi **dtexec** e l'Utilità di esecuzione pacchetti. Quest'ultima è uno strumento grafico compilato in base a **dtexec**. Entrambi gli strumenti eseguono il pacchetto immediatamente. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] include inoltre un sottosistema di SQL Server Agent progettato appositamente per pianificare l'esecuzione di pacchetti nell'ambito di un processo di SQL Server Agent.  
  
Per eseguire i pacchetti distribuiti verrà utilizzata l'Utilità di esecuzione pacchetti. I pacchetti verranno utilizzati nello stato in cui sono e non sarà pertanto necessario aggiornare informazioni in nessuna delle pagine della finestra di dialogo. I pacchetti verranno eseguiti dalla pagina Generale, ovvero la prima dell'Utilità di esecuzione pacchetti. Se si desidera, è possibile fare clic sulle altre pagine per esaminare le informazioni in esse contenute relative a ogni pacchetto.  
  
> [!NOTE]  
> Per accertarsi che i pacchetti vengano eseguiti correttamente nel contesto di questa esercitazione, non è necessario modificare alcuna opzione.  
  
Prima di eseguire i pacchetti in [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] mediante l'Utilità di esecuzione pacchetti, verificare che il servizio Integration Services sia in esecuzione. Tale servizio offre il supporto necessario per l'archiviazione e l'esecuzione dei pacchetti. Se il servizio è arrestato, non è possibile connettersi a [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] e in [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] non verranno elencati i pacchetti da eseguire. È inoltre necessario disporre delle autorizzazioni necessarie per eseguire il pacchetto nell'istanza in cui è stato distribuito. Per altre informazioni, vedere [Ruoli Integration Services &#40;servizio SSIS&#41;](../integration-services/security/integration-services-roles-ssis-service.md).  
  
Le cartelle di livello superiore all'interno della cartella Pacchetti archiviati rappresentano le cartelle definite dall'utente e monitorate da Integration Services. È possibile specificare qualsiasi numero di cartelle desiderato nel file MsDtsSrvr.ini.xml. Ai fini di questa esercitazione si presuppone che venga utilizzato il file MsDtsSrvr.ini.xml e che i nomi delle cartelle di livello superiore all'interno di Pacchetti archiviati siano File system e MSDB.  
  
### <a name="to-connect-to-integration-services-in-sql-server-management-studio"></a>Per connettersi a Integration Services in SQL Server Management Studio  
  
1.  Fare clic sul pulsante **Start**, scegliere **Tutti i programmi**, **Microsoft SQL Server**, quindi **SQL Server Management Studio**.  
  
2.  Nella finestra di dialogo **Connetti al server** selezionare **Integration Services** nell'elenco **Tipo server** , specificare il nome del server nella casella **Nome server** e fare clic su **Connetti**.  
  
    > [!IMPORTANT]  
    > Se non è possibile stabilire la connessione con [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)], è probabile che il servizio [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] non sia in esecuzione. Per informazioni sullo stato del servizio, fare clic sul pulsante **Start**, scegliere **Tutti i programmi**, **Microsoft SQL Server**, **Strumenti di configurazione**, quindi fare clic su **Gestione configurazione SQL Server**. Nel riquadro di sinistra fare clic su **Servizi di SQL Server**. Nel riquadro di destra cercare il servizio [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] . Se non è già in esecuzione, avviare il servizio.  
  
    [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] si apre. Per impostazione predefinita, la finestra di Esplora oggetti viene aperta e collocata nell'angolo superiore destro dell'applicazione. Se Esplora oggetti non viene visualizzato, scegliere **Esplora oggetti** dal menu **Visualizza** .  
  
### <a name="to-run-the-packages-using-the-execute-package-utility"></a>Per eseguire i pacchetti mediante l'Utilità di esecuzione pacchetti  
  
1.  In Esplora oggetti espandere la cartella Pacchetti archiviati.  
  
2.  Espandere la cartella MSDB. Poiché sono stati distribuiti in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], tutti i pacchetti vengono archiviati nel database msdb di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] e visualizzati nella cartella MSDB. La cartella File system è vuota, a meno che i pacchetti non siano stati distribuiti nel file system all'esterno di Deployment Tutorial.  
  
3.  A partire dall'inizio dell'elenco dei pacchetti, fare clic con il pulsante destro del mouse su DataTransfer e scegliere **Esegui pacchetto**.  
  
4.  Nella finestra di dialogo **Utilità di esecuzione pacchetti** fare clic su **Esegui**.  
  
5.  Nella finestra di dialogo **Utilità di esecuzione pacchetti** visualizzare lo stato e i risultati dell'esecuzione del pacchetto. Quando il pulsante **Arresta** non è più disponibile, ovvero il pacchetto è stato completato, fare clic su **Chiudi**.  
  
    > [!IMPORTANT]  
    > Se si fa clic su **Arresta** durante l'esecuzione del pacchetto, quest'ultimo non verrà completato.  
  
6.  Nella finestra di dialogo **Utilità di esecuzione pacchetti** fare clic su **Chiudi**.  
  
7.  Ripetere i passaggi da 3 a 5 per il pacchetto LoadXML.  
  
8.  Scegliere **Esci** dal menu **File**.  
  
### <a name="to-verify-the-results-of-the-datatransfer-package"></a>Per verificare i risultati del pacchetto DataTransfer  
  
1.  Sulla barra degli strumenti di [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]fare clic su **Nuova query**.  
  
2.  Nella finestra di dialogo **Connetti al server** selezionare **Motore di database** nell'elenco **Tipo server** , specificare il nome del server in cui sono installati i pacchetti dell'esercitazione oppure digitare (local) nella casella **Nome server** e selezionare una modalità di autenticazione. Se si utilizza l'autenticazione di SQL Server, specificare un nome utente e una password.  
  
3.  Fare clic su **Connetti**.  
  
4.  Nella finestra della query digitare o incollare l'istruzione SQL seguente:  
  
    `USE AdventureWorks`  
  
    `SELECT * FROM HighIncomeCustomers`  
  
5.  Premere **F5** o fare clic sull'icona Esegui sulla barra degli strumenti.  
  
    La query restituirà 31 righe di dati. Il risultato restituito contiene tutte le righe del file di testo Customers.txt con valori superiori a 100000 nella colonna YearlyIncome.  
  
6.  Individuare la cartella DeploymentTutorial , fare clic con il pulsante destro del mouse sul file XML di log denominato Deployment Tutorial Log e quindi fare clic su **Apri**. È possibile aprire il file in Blocco note oppure in un altro editor di testo/XML.  
  
### <a name="to-verify-the-results-of-the-loadxmldata-package"></a>Per verificare i risultati del pacchetto LoadXMLData  
  
1.  Sulla barra degli strumenti di [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]fare clic su **Nuova query**.  
  
2.  Se viene richiesto di connettersi di nuovo, nella finestra di dialogo **Connetti al server** selezionare **Motore di database** nell'elenco **Tipo server** , specificare il nome del server in cui sono installati i pacchetti dell'esercitazione oppure digitare (local) nella casella **Nome server** e selezionare una modalità di autenticazione. Se si utilizza l'autenticazione di SQL Server, specificare un nome utente e una password.  
  
3.  Fare clic su **Connetti**.  
  
4.  Nella finestra della query digitare o incollare l'istruzione SQL seguente:  
  
    `USE AdventureWorks`  
  
    `SELECT * FROM OrderDatesByCountryRegion`  
  
5.  Premere **F5** o fare clic sull'icona Esegui sulla barra degli strumenti.  
  
    La query restituirà 21 righe di dati. I risultati sono costituiti dalle righe del file di dati XML, orders.xml. Ogni riga è un riepilogo per paese/area geografica, ovvero la riga elenca il nome di un paese/area geografica, il numero di ordine per ogni paese/area geografica e le date degli ordini più recenti e meno recenti.  
  
## <a name="see-also"></a>Vedere anche  
[Utilità dtexec](../integration-services/packages/dtexec-utility.md)  
  
  
  
