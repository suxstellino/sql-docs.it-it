---
description: Installare il primo server di report in modalità SharePoint
title: Installare il primo server di report in modalità SharePoint | Microsoft Docs
ms.date: 10/05/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: bf701e1ccdff887f7861c934d59bff9c09be66b6
ms.sourcegitcommit: 295b9dfc758471ef7d238a2b0f92f93e34acbb1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/31/2021
ms.locfileid: "106054642"
---
# <a name="install-the-first-report-server-in-sharepoint-mode"></a>Installare il primo server di report in modalità SharePoint

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

  Nelle procedure contenute in questo argomento viene illustrata l'installazione a server singolo di Reporting Services in modalità SharePoint. Nei passaggi è inclusa l'esecuzione dell'Installazione guidata di SQL Server, nonché di attività di configurazione in cui viene utilizzata Amministrazione centrale SharePoint. L'argomento può anche essere usato per singole procedure di aggiornamento di un'installazione esistente, ad esempio per creare un'applicazione di servizio di Reporting Services.  
  
> [!NOTE]
> L'integrazione di Reporting Services con SharePoint non è più disponibile nelle versioni successive a SQL Server 2016.
  
 Per informazioni sull'aggiunta di altri server di Reporting Services a una farm esistente, vedere quanto riportato di seguito:  
  
-   [Aggiungere un ulteriore server di report a una farm &#40;con scalabilità orizzontale SSRS&#41;](../../reporting-services/install-windows/add-an-additional-report-server-to-a-farm-ssrs-scale-out.md)  
  
-   [Aggiungere un ulteriore front-end Web di Reporting Services a una farm](../../reporting-services/install-windows/add-an-additional-reporting-services-web-front-end-to-a-farm.md)  
  
 L'installazione a server singolo è utile per scenari di sviluppo e di test, ma non è consigliata per ambienti di produzione.  
  
##  <a name="example-single-server-deployment"></a><a name="bkmk_singleserver"></a> Distribuzione a server singolo di esempio

 L'installazione di un unico server è utile per scenari di sviluppo e di test ma l'utilizzo di un server singolo non è consigliato per un ambiente di produzione. Con il termine "ambiente a server singolo" ci si riferisce a un computer singolo con SharePoint e componenti di Reporting Services installati nello stesso computer. In questo argomento non viene illustrata la scalabilità orizzontale con più server di Reporting Services.  
  
 Nel diagramma seguente sono illustrati i componenti che fanno parte di una distribuzione di Reporting Services a server singolo.  
 
 > [!NOTE]
 > Per SharePoint 2016, Excel Services è stato spostato in Office Online Server e non può essere usato in una distribuzione a server singolo. Office Online Server deve essere distribuito in un altro server. Per altre informazioni, vedere [Office Online Server overview](https://technet.microsoft.com/library/jj219437\(v=office.16\).aspx) (Panoramica di Office Online Server) e [Configure Excel Online administrative settings](https://technet.microsoft.com/library/jj219698\(v=office.16\).aspx)(Configurare le impostazioni amministrative di Excel Online).
  
|Componente|Descrizione|  
|-|-|  
|**(1)**|Servizio SharePoint installato dal programma di installazione di SQL Server. È possibile creare una o più applicazioni di servizio di Reporting Services.|  
|**(2)**|Il componente aggiuntivo di Reporting Services per prodotti SharePoint offre i componenti dell'interfaccia utente nei server SharePoint.|  
|**(3)**|Applicazione Excel Services usata da Power View e [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]. Non è disponibile in una distribuzione a server singolo per SharePoint 2016. È necessaria un'istanza di [Office Online Server](https://technet.microsoft.com/library/jj219437\(v=office.16\).aspx) .|  
|**(4)**|Applicazione di servizio [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)].|  
  
 ![Distribuzione a server singolo in modalità SharePoint per SSRS](../../reporting-services/install-windows/media/rs-sharepoint-1server-deployment.gif "Distribuzione a server singolo in modalità SharePoint per SSRS")  
  
> [!TIP]  
>  Per esempi di distribuzione più complessi, vedere [Installazione della modalità SharePoint di Reporting Services (SharePoint 2010 e SharePoint 2013)](/previous-versions/sql/sql-server-2016/hh231674(v=sql.130)).  
  
##  <a name="setup-accounts"></a><a name="bkmk_setupaccounts"></a> Account di configurazione

 In questa sezione vengono descritti gli account e le autorizzazioni usati per i passaggi relativi alla distribuzione principale di Reporting Services in modalità SharePoint.  
  
 **Installazione e registrazione del servizio Reporting Services:**  
  
-   L'account corrente durante l'installazione, definito account di "configurazione", di Reporting Services in modalità SharePoint deve avere i diritti amministrativi nel computer locale. Se si installa Reporting Services dopo SharePoint e l'account di "configurazione" è anch'esso membro del gruppo di amministratori della farm di SharePoint, tramite l'installazione di Reporting Services verrà registrato automaticamente il servizio Reporting Services. Se si installa Reporting Services prima di SharePoint o l'account di "configurazione" non è un membro del gruppo di amministratori della farm, il servizio deve essere registrato manualmente. Vedere la sezione [Passaggio 2: Registrare e avviare il servizio SharePoint di Reporting Services](#bkmk_install_SSRS_sharedservice).  
  
 **Creazione di un'applicazione di servizio Reporting Services**  
  
-   Dopo l'installazione e la registrazione del servizio Reporting Services, creare una o più applicazioni di servizio Reporting Services. L'account di servizio della farm di SharePoint deve essere temporaneamente membro del gruppo di amministratori locali affinché l'applicazione di servizio Reporting Services possa essere creata. Per altre informazioni sulle autorizzazioni per gli account di SharePoint 2013, vedere [Autorizzazioni e impostazioni di sicurezza per gli account in SharePoint Server 2013](/SharePoint/install/account-permissions-and-security-settings-in-sharepoint-server-2016) oppure, per https://technet.microsoft.com/library/cc678863.aspx) SharePoint 2016, vedere [Autorizzazioni e impostazioni di sicurezza per gli account in SharePoint Server 2016](https://technet.microsoft.com/library/cc678863\(v=office.16\).aspx).  
  
     Per motivi di sicurezza è consigliabile che gli account amministratori di farm di SharePoint non siano anche account amministratori del sistema operativo locale. Se si aggiunge un account amministratore di farm al gruppo di amministratori locali durante il processo di installazione, è consigliabile rimuoverlo al termine dell'installazione.  
  
##  <a name="step-1-install-reporting-services-report-server-in-sharepoint-mode"></a><a name="bkmk_install_SSRS"></a> Passaggio 1: Installare il server di report Reporting Services in modalità SharePoint

 Durante questo passaggio vengono installati un server di report di Reporting Services in modalità SharePoint e il componente aggiuntivo Reporting Services per prodotti SharePoint. A seconda degli elementi già installati nel computer, alcune delle pagine di installazione descritte nei passaggi seguenti potrebbero non essere visualizzate.  
 
 > [!IMPORTANT]
 > Per SharePoint 2016, il server SharePoint in cui verrà installato Reporting Services deve avere il ruolo del server **Custom**. La distribuzione di Reporting Services verrà eseguita correttamente in un server SharePoint che non ha il ruolo del server **Custom**, ma durante la finestra di manutenzione di SharePoint successiva, MinRole arresterà il servizio Reporting Services perché rileverà che la modalità integrata di SharePoint Reporting Services non indica il supporto per nessun altro ruolo del server SharePoint. L'applicazione di servizio Reporting Services supporta solo il ruolo **Custom**.
 
 > [!NOTE]
 > Se si prevede di installare anche il servizio Power Pivot, in SharePoint 2016 installarlo prima di installare Reporting Services. Il servizio Power Pivot può essere installato solo in un server SharePoint nel ruolo **Custom**.
 
 ### <a name="apply-the-custom-server-role-to-a-sharepoint-2016-server"></a>Applicare il ruolo del server Custom a un server SharePoint 2016
 
 > [!NOTE]
 > Non si applica a SharePoint 2013.
 
 1. Accedere al server SharePoint in cui si prevede di installare [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)](Configurare le impostazioni amministrative di Excel Online).
 
 2. Avviare la **Shell di gestione SharePoint 2016** come amministratore. 
  
    È possibile fare clic con il pulsante destro del mouse su **Shell di gestione SharePoint 2016** e scegliere **Esegui come amministratore**.

3. Al prompt dei comandi di PowerShell eseguire il comando seguente.

    > [!NOTE]
    > Verificare di avere specificato il nome corretto del server SharePoint.
    
    ```powershell
    Set-SPServer SERVERNAME -Role Custom
    ```

4. Verrà visualizzata una risposta che informa che è stato pianificato un processo timer. Sarà necessario attendere l'esecuzione del processo.

5. Usare il comando seguente per verificare il ruolo assegnato del server.

    ```powershell
    Get-SPServer SERVERNAME 
    ```
 
 6. Il **Ruolo** dovrà essere **Custom**.
 
 ### <a name="install-reporting-services"></a>Installare Reporting Services
  
1.  Eseguire l'Installazione guidata di SQL Server (Setup.exe).  
  
2.  Selezionare **Installazione** nella parte sinistra della procedura guidata, quindi scegliere **Nuova installazione autonoma di SQL Server o aggiunta di funzionalità a un'installazione esistente**.  

3.  Se viene visualizzata la pagina **Codice Product Key**, digitare il proprio codice o accettare l'impostazione predefinita dell'edizione "Enterprise Evaluation".  
  
     Selezionare **Avanti**.  
  
4.  Se viene visualizzata la pagina relativa alle condizioni di licenza, rivederle e accettarle. Microsoft ringrazia gli utenti per la loro disponibilità a inviare dati di utilizzo delle funzionalità al fine di migliorare le funzionalità dei prodotti e il Servizio Supporto Tecnico Clienti.  
  
     Selezionare **Avanti**.  

5.  È consigliabile selezionare **Usa Microsoft Update per verificare la disponibilità di aggiornamenti (scelta consigliata)** . Operazione facoltativa.
  
     Selezionare **Avanti**.   
  
6.  Nella pagina **Installazione dei file di installazione** , a seconda degli elementi già installati nel computer, potrebbe essere visualizzato il messaggio seguente:  
  
    -   "Operazioni in sospeso in uno o più file interessati. Al termine del processo, è necessario riavviare il computer".  
  
    -   Selezionare **Avanti**.  
  
7.  Se viene visualizzata la pagina **Regole di installazione** , esaminare eventuali avvisi o problemi che comportano blocchi. Fare quindi clic su **Avanti**.
 
8. Nella pagina **Selezione funzionalità** selezionare le opzioni seguenti:  
  
    -   **Reporting Services – SharePoint**  
  
    -   **Componente aggiuntivo Reporting Services per prodotti SharePoint**.  
  
    -   Facoltativamente, è anche possibile selezionare **Servizi motore di database** per un ambiente completo, ma è consigliabile avere un'istanza del motore di database di SQL Server che ospita i database di SharePoint.  
  
     Selezionare **Avanti**.  
  
     ![Screenshot della pagina di selezione delle funzionalità con le opzioni Reporting Services - SharePoint e Componente aggiuntivo Reporting Services per prodotti SharePoint selezionate.](../../reporting-services/install-windows/media/rs-setupfeatureselection-sharepoint-with-circles.png)
  
9. Se si seleziona Servizi motore di database, accettare l'istanza predefinita di **MSSQLSERVER** nella pagina **Configurazione dell'istanza** e fare clic su **Avanti**.  
  
     ![nota](/analysis-services/analysis-services/instances/install-windows/media/ssrs-fyi-note.png "nota")L'architettura del servizio SharePoint di Reporting Services non è basata su un'istanza di SQL Server come l'architettura di Reporting Services precedente.  
  
10. Se viene visualizzata la pagina **Configurazione server** , digitare le credenziali appropriate. Se si vuole usare le funzionalità di avvisi dati o sottoscrizioni di Reporting Services, è necessario modificare il **Tipo di avvio** per SQL Server Agent in **Automatico**. La pagina **Configurazione server** potrebbe non essere visualizzata a seconda degli elementi già installati nel computer.  
  
     Selezionare **Avanti**.  
  
11. Se si seleziona Servizi motore di database, verrà visualizzata la pagina **Configurazione del motore di database** . Aggiungere gli account appropriati all'elenco degli amministratori SQL e fare clic su **Avanti**.  
  
12. Nella pagina **Configurazione di Reporting Services** , l'opzione **Solo installazione** dovrebbe apparire come selezionata. Tramite questa opzione vengono installati i file del server di report e non viene configurato l'ambiente SharePoint per Reporting Services.  
  
    > [!NOTE]
    > Una volta completata l'installazione di SQL Server, attenersi alle altre sezioni di questo argomento per configurare l'ambiente SharePoint. Questo include l'installazione del servizio condiviso Reporting Services e la creazione delle applicazioni di servizio Reporting Services.  
  
     ![Screenshot della sezione Modalità integrata SharePoint di Reporting Services con l'opzione Solo installazione selezionata ed evidenziata.](../../reporting-services/install-windows/media/ssrs-2016-setup-configuration.png)
  
13. Esaminare eventuali avvisi e quindi fare clic su **Avanti** nella pagina **Regole di configurazione della funzionalità** se ci si ferma in questa pagina.  
  
14. Nella pagina **Inizio installazione** consultare il riepilogo sull'installazione. Nel riepilogo verrà incluso un nodo figlio **Modalità SharePoint di Reporting Services** per cui verrà visualizzato un valore **SharePointFilesOnlyMode**. Selezionare **Installa**.  
  
15. Per l'installazione saranno richiesti diversi minuti. Verrà visualizzata la pagina **Operazione completata** con le funzionalità elencate e il relativo stato. Potrebbe essere visualizzata una finestra di dialogo in cui viene indicata la necessità di riavviare il computer.  
  
##  <a name="step-2-register-and-start-the-reporting-services-sharepoint-service"></a><a name="bkmk_install_SSRS_sharedservice"></a> Passaggio 2: Registrare e avviare il servizio SharePoint di Reporting Services  
 ![Contenuto correlato di PowerShell](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "Contenuto correlato di PowerShell")  
  
> [!NOTE]
> Se si esegue l'installazione in una farm di SharePoint esistente, non è necessario completare i passaggi in questa sezione. Il servizio SharePoint di Reporting Services viene installato e avviato durante l'esecuzione dell'Installazione guidata di SQL Server inclusa nella sezione precedente di questo documento.  
  
 Di seguito sono riportati i motivi comuni per cui è necessario registrare manualmente il servizio Reporting Services.  
  
-   È stata installata la modalità SharePoint di Reporting Services prima di SharePoint.  
  
-   L'account usato per installare la modalità SharePoint di Reporting Services non era membro del gruppo di amministratori di farm di SharePoint. Per altre informazioni, vedere la sezione [Setup accounts](#bkmk_setupaccounts).  
  
 I file necessari sono stati installati come parte dell'Installazione guidata di SQL Server, ma i servizi devono essere registrati nella farm SharePoint.  
  
 I passaggi seguenti illustrano l'apertura della shell di gestione SharePoint e l'esecuzione dei cmdlet di PowerShell:  
  
1.  Selezionare il pulsante **Start** .  
  
2.  Selezionare il gruppo **Microsoft SharePoint 2016 Products** (Prodotti Microsoft SharePoint 2016) o **Microsoft SharePoint 2013 Products** (Prodotti Microsoft SharePoint 2013).  
  
3.  Fare clic con il pulsante destro del mouse su **Shell di gestione SharePoint 2016** o su **Shell di gestione SharePoint 2013** e scegliere **Esegui come amministratore**. 

    > [!NOTE]
    > I comandi di SharePoint non sono riconosciuti nella finestra standard di Windows PowerShell. Usare la **shell di gestione di SharePoint**.  
  
4.  Eseguire il comando PowerShell seguente per installare il servizio SharePoint Reporting Services. Se il comando viene correttamente eseguito, nella shell di gestione viene visualizzata una nuova riga. In questo caso,**non viene restituito alcun messaggio** :  
  
    ```  
    Install-SPRSService  
    ```  
  
5.  Eseguire il comando PowerShell seguente per installare il proxy del servizio Reporting Services. Se il comando viene correttamente eseguito, nella shell di gestione viene visualizzata una nuova riga. In questo caso,**non viene restituito alcun messaggio** :  
  
    ```  
    Install-SPRSServiceProxy  
    ```  
  
6.  Eseguire il seguente comando PowerShell per avviare il servizio o visualizzare le note seguenti per le istruzioni su come avviare il servizio da Amministrazione centrale SharePoint:  
  
    ```  
    get-spserviceinstance -all |where {$_.TypeName -like "SQL Server Reporting*"} | Start-SPServiceInstance  
    ```  
  
    > [!IMPORTANT]
    > Se viene visualizzato un messaggio di errore simile al seguente,  
    >   
    ```powershell
    >     Install-SPRSService : The term 'Install-SPRSService' **is not recognized** as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.  
    ```
    >
    > Si è in Windows PowerShell anziché nella shell di gestione SharePoint oppure la modalità SharePoint di Reporting Services non è installata. Per altre informazioni su Reporting Services e PowerShell, vedere [Cmdlet di PowerShell per la modalità SharePoint di Reporting Services](../../reporting-services/report-server-sharepoint/powershell-cmdlets-for-reporting-services-sharepoint-mode.md).  
  
 È anche possibile avviare il servizio da Amministrazione centrale SharePoint anziché eseguire il terzo comando PowerShell. I passaggi seguenti sono anche utili per verificare che il servizio è in esecuzione.  
  
1.  Nel gruppo **Impostazioni sistema** di Amministrazione centrale SharePoint fare clic su **Gestisci servizi nel server** .  
  
2.  Individuare il **servizio SQL Server Reporting Services** e fare clic su **Avvio** nella colonna Azione.  
  
3.  Lo stato del servizio Reporting Services cambia da **Arrestato** in **Avviato**. Se il servizio Reporting Services non è presente nell'elenco, utilizzare PowerShell per installare il servizio.  
  
    > [!NOTE]  
    >  Se il servizio Reporting Services rimane nello stato **Avvio in corso** e non cambia in **Avviato**, verificare che il servizio "Amministrazione SharePoint 2013" venga avviato in Server Manager di Windows.  
  
##  <a name="step-3-create-a-reporting-services-service-application"></a><a name="bkmk_create_serrviceapplication"></a> Passaggio 3: Creare un'applicazione di servizio Reporting Services  
 In questa sezione vengono presentati i passaggi per creare un'applicazione di servizio e una descrizione delle proprietà, se è in corso la revisione di un'applicazione di servizio esistente.  
  
1.  Nel gruppo **Gestione applicazioni** di Amministrazione centrale SharePoint selezionare **Gestisci applicazioni di servizio**.  
  
2.  Nella barra multifunzione di SharePoint selezionare il pulsante **Nuovo** .  
  
3.  Scegliere **Applicazione di servizio SQL Server Reporting Services** dal menu Nuovo.  
  
    > [!IMPORTANT]  
    >  Se l'opzione Reporting Services non viene visualizzata nell'elenco, **significa che il servizio condiviso Reporting Services non è installato**. Rivedere la sezione precedente sull'uso dei cmdlet di PowerShell per installare il servizio Reporting Services.  
  
4.  Nella pagina **Creazione di un'applicazione di servizio di SQL Server Reporting Services** immettere un nome per l'applicazione. Se si creano più applicazioni di servizio Reporting Services, tramite un nome descrittivo o una convenzione di denominazione sarà possibile organizzare le operazioni di amministrazione e gestione.  
  
5.  Nella sezione **Pool di applicazioni** creare un nuovo pool di applicazioni per l'applicazione (consigliato). Se si utilizza lo stesso nome sia per il pool di applicazioni sia per l'applicazione di servizio, l'amministrazione può risultare più semplice. Questa situazione può essere influenzata anche dal numero di applicazioni di servizio create e dall'eventuale necessità di utilizzarne diverse in un unico pool di applicazioni. Vedere la documentazione del server SharePoint relativa alle indicazioni e alle procedure consigliate per la gestione del pool di applicazioni.  
  
     Selezionare o creare un account di sicurezza per il pool di applicazioni. Assicurarsi di specificare un account utente di dominio. Un account utente di dominio permette di utilizzare la funzionalità dell'account gestito di SharePoint, che consente di aggiornare password e informazioni sull'account da un'unica posizione. Gli account di dominio sono inoltre obbligatori se si prevede di ridimensionare la distribuzione per includere istanze del servizio aggiuntive da eseguire con la stessa identità.  
  
6.  In **Server di database** è possibile utilizzare il server corrente o scegliere un SQL Server diverso.  
  
7.  In **Nome database** il valore predefinito è `ReportingService_<guid>`, ovvero un nome di database univoco. Se si immette un nuovo valore, accertarsi che sia univoco. Si tratta del nuovo database da creare in modo specifico per l'applicazione di servizio.  
  
8.  In **Autenticazione database** l'impostazione predefinita è Autenticazione di Windows. Se si sceglie **Autenticazione di SQL Server**, fare riferimento alla documentazione di SharePoint per le procedure consigliate sull'utilizzo di questo tipo di autenticazione in una distribuzione di SharePoint.  
  
9. Nella sezione **Associazione applicazione Web** , selezionare l'applicazione Web di cui effettuare il provisioning per l'accesso dall'applicazione di servizio Reporting Services corrente. È possibile associare un'applicazione di servizio Reporting Services a una sola applicazione Web. Se tutte le applicazioni Web correnti sono già associate con un'applicazione di servizio Reporting Services, viene visualizzato un messaggio di avviso.  
  
10. Selezionare **OK**.  
  
11. Il completamento del processo di creazione dell'applicazione di servizio potrebbe richiedere diversi minuti. Al termine, verrà visualizzato un messaggio di conferma e un collegamento a una pagina di **provisioning di sottoscrizioni e avvisi** . Se si vuole usare la funzionalità di Reporting Services relativa alle sottoscrizioni o quella relativa agli avvisi dati, completare il passaggio di provisioning. Per altre informazioni, vedere [Eseguire il provisioning di sottoscrizioni e avvisi per le applicazioni di servizio SSRS](../../reporting-services/install-windows/provision-subscriptions-and-alerts-for-ssrs-service-applications.md).  
  
 ![Contenuto correlato di PowerShell](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "Contenuto correlato di PowerShell") Per informazioni sull'uso di PowerShell per creare un'applicazione di servizio Reporting Services, vedere:  
  
-   Vedere la sezione seguente [Script di Windows PowerShell per i passaggi da 1 a 4](#bkmk_full_script).  
  
-   Argomento [Per creare un'applicazione di servizio Reporting Services con PowerShell](../../reporting-services/report-server-sharepoint/reporting-services-sharepoint-service-and-service-applications.md).  

##  <a name="step-4-activate-the-power-view-site-collection-feature"></a><a name="bkmk_powerview"></a>Passaggio 4: Attivare la funzionalità per la raccolta di siti di Power View.

 [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)], una funzionalità del componente aggiuntivo di SQL Server 2016 Reporting Services per i prodotti [!INCLUDE[msCoName](../../includes/msconame-md.md)] SharePoint, è una funzionalità di raccolta siti. La funzionalità viene attivata automaticamente per raccolte siti radice e raccolte siti create dopo l'installazione del componente aggiuntivo di Reporting Services. Se si intende utilizzare [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)], verificare che la funzionalità sia attivata.  
  
 Se si installa il componente aggiuntivo di Reporting Services per prodotti SharePoint dopo l'installazione del server SharePoint, la funzionalità di integrazione del server di report e la funzionalità di integrazione di Power View verranno attivate solo per le raccolte siti radice. Per le altre raccolte siti, attivare manualmente le funzionalità.  
  
#### <a name="to-activate-or-verify-the-power-view-site-collection-feature"></a>Per attivare o verificare la funzionalità per la raccolta di siti di Power View  
  
1.  Nei passaggi seguenti si presuppone che il sito di SharePoint sia configurato per la **versione esperienza** 2013, per SharePoint 2013.  
  
     Aprire il browser al sito di SharePoint desiderato. Ad esempio https://\<servername>/sites/bi  
  
2.  Selezionare **Impostazioni**![Impostazioni di SharePoint](/analysis-services/analysis-services/media/as-sharepoint2013-settings-gear.gif "Impostazioni di SharePoint").  
  
3.  Selezionare **Impostazioni sito**.  
  
4.  Nel gruppo **Amministrazione raccolta siti** selezionare **Funzionalità raccolta siti**.  
  
5.  Individuare **Funzionalità di integrazione Power View** nell'elenco.  
  
6.  Selezionare **Attiva**. Lo stato della funzionalità verrà impostato su **Attiva**.  
  
 Questa procedura viene completata per ogni raccolta siti. Per altre informazioni, vedere [Attivare le funzionalità di integrazione per Power View e server di report in SharePoint](../../reporting-services/report-server-sharepoint/site-collection-features-report-server-and-power-view.md).  
  
##  <a name="windows-powershell-script-for-steps-1-4"></a><a name="bkmk_full_script"></a> Script di Windows PowerShell per i passaggi da 1 a 4  
 Lo script di PowerShell in questa sezione è l'equivalente del completamento dei passaggi da 1 a 4 nelle sezioni precedenti. Lo script consente di completare quanto indicato di seguito:  
  
-   Viene installato il servizio Reporting Services e il proxy del servizio e viene avviato il servizio.  
  
-   Viene creato un proxy del servizio denominato "Reporting Services".  
  
-   Viene creata un'applicazione di servizio Reporting Services denominata "Reporting Services Application".  
  
-   Viene abilitata la funzionalità di [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)] per una raccolta siti.  
  
 Parametri  
  
-   Aggiornare **-Account** per il proxy del servizio. È necessario che l'account sia un account del servizio gestito nella farm di SharePoint. Per ulteriori informazioni, vedere l'argomento di SharePoint [Pianificare gli account amministrativi e di servizio in SharePoint 2013](/SharePoint/security-for-sharepoint-server/plan-for-administrative-and-service-accounts).  
  
-   Aggiornare il parametro **–DatabaseServer** per l'applicazione di servizio. Questo parametro è l'istanza del motore di database  
  
-   Aggiornare il parametro **–url** del sito per il quale si desidera abilitare la funzionalità [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)].  
  
 **Per utilizzare lo script:**  
  
1.  Aprire Windows PowerShell con privilegi amministrativi.  
  
2.  Copiare il codice seguente nella finestra dello script.  
  
3.  Aggiornare i tre parametri descritti nella sezione precedente e quindi eseguire lo script.  
  
```  
#This script Configures SQL Server Reporting Services SharePoint mode  
  
$starttime=Get-Date  
write-host -foregroundcolor DarkGray StartTime>> $starttime   
  
Write-Host -ForegroundColor Green "Import the SharePoint PowerShell snappin"  
Add-PSSnapin Microsoft.Sharepoint.Powershell -EA 0  
  
Write-Host -ForegroundColor Green "Install SSRS Service and Service Proxy, and start the service"  
Write-Host -ForegroundColor Green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"  
  
    Write-Host -ForegroundColor Green "Install the Reporting Services Shared Service"  
    Install-SPRSService  
  
    Write-Host -ForegroundColor Green " Install the Reporting Services Service Proxy"  
    Install-SPRSServiceProxy  
  
    # Get the ID of the RS Service Instance and start the service   
    Write-Host -ForegroundColor Green "Start the Reporting Services Service"  
    $RS = Get-SPServiceInstance | Where {$_.TypeName -eq "SQL Server Reporting Services Service"}  
    Start-SPServiceInstance -Identity $RS.Id.ToString()  
  
    # Wait for the Reporting Services Service to start...  
    $Status = Get-SPServiceInstance $RS.Id.ToString()  
    While ($Status.Status -ne "Online")  
    {  
        Write-Host -ForegroundColor Green "SSRS Service Not Online...Current Status = " $Status.Status  
        Start-Sleep -Seconds 2  
        $Status = Get-SPServiceInstance $RS.Id.ToString()  
    }  
  
$time=Get-Date  
write-host -foregroundcolor DarkGray StartTime>> $starttime   
write-host -foregroundcolor DarkGray $time  
  
Write-Host -ForegroundColor Green "Create a new application pool and Reporting Services service application"  
Write-Host -ForegroundColor Green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"  
Write-Host -ForegroundColor Green "Create a new application pool"  
#!!!! update "-Account" with an existing Managed Service Account  
New-SPServiceApplicationPool -Name "Reporting Services" -Account "<domain>\User name>"  
$appPool = Get-SPServiceApplicationPool "Reporting Services"  
  
Write-Host -ForegroundColor Green " Create the Reporting Services Service Application"  
#!!!! Update "-DatabaseServer", an instance of the SQL Server database engine   
$rsService = New-SPRSServiceApplication -Name "Reporting Services Application" -ApplicationPool $appPool -DatabaseName "Reporting_Services_Application" -DatabaseServer "<server name>"  
  
Write-Host -ForegroundColor Green "Create the Reporting Services Service Application Proxy"  
$rsServiceProxy = New-SPRSServiceApplicationProxy -Name "Reporting Services Application Proxy" -ServiceApplication $rsService  
  
Write-Host -ForegroundColor Green "Associate service application proxy to default web site and grant web applications rights to SSRS application pool"  
Write-Host -ForegroundColor Green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"     
# Associate the Reporting Services Service Applicatoin Proxy to the default web site...  
Get-SPServiceApplicationProxyGroup -default | Add-SPServiceApplicationProxyGroupMember -Member $rsServiceProxy  
  
$time=Get-Date  
write-host -foregroundcolor DarkGray StartTime>> $starttime   
write-host -foregroundcolor DarkGray $time  
  
Write-Host -ForegroundColor Green "Enable the PowerView and reportserver site features"  
Write-Host -ForegroundColor Green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"  
#!!!! update "-url"  of the site where you want the features enabled  
Enable-SPfeature -identity "powerview" -Url https://server/sites/bi  
Enable-SPfeature -identity "reportserver" -Url https://server/sites/bi  
  
####To Verify, you can run the following:  
#Get-SPRSServiceApplication  
#Get-SPServiceApplicationPool | where {$_.name -like "reporting*"}  
#Get-SPRSServiceApplicationProxy  
  
```  
  
##  <a name="additional-configuration"></a><a name="bkmk_additional_config"></a> Configurazione aggiuntiva  
 In questa sezione vengono descritti i passaggi di configurazione aggiuntivi che sono importanti nella maggior parte delle distribuzioni SharePoint.  
  
###  <a name="configure-excel-services-and-power-pivot"></a><a name="bkmk_configure_ECS"></a> Configurare Excel Services e Power Pivot  
 Per visualizzare i report [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)] Power View in una cartella di lavoro di Excel 2016 o Excel 2013 in SharePoint, è necessario configurare Excel Services per l'uso di un server [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] in modalità Power Pivot. 
 
 Per SharePoint 2016, è necessario configurare [Office Online Server](https://technet.microsoft.com/library/jj219456\(v=office.16\).aspx) per usare Excel Services. Per informazioni dettagliate, vedere i white paper seguenti.
 
 - [Distribuzione di SQL Server 2016 PowerPivot e Power View in SharePoint 2016](/analysis-services/instances/install-windows/deploying-sql-server-2016-powerpivot-and-power-view-in-sharepoint-2016)
 
 - [Distribuzione di SQL Server 2016 PowerPivot e Power View in una farm di SharePoint 2016 a più livelli](/analysis-services/instances/install-windows/deploy-powerpivot-and-power-view-multi-tier-sharepoint-2016-farm)
 
 Per SharePoint 2016, sarà necessario creare e configurare un'applicazione Excel Services. Per altre informazioni, vedere gli argomenti seguenti:  
  
-   Sezione "Configurare Excel Services per l'integrazione di Analysis Services" in [Installazione di Analysis Services in modalità Power Pivot](/analysis-services/instances/install-windows/install-analysis-services-in-power-pivot-mode).  
  
-   [Gestire le impostazioni del modello di dati di Excel Services (SharePoint Server 2013)](/SharePoint/administration/manage-excel-services-data-model-settings).  

L'account di sicurezza del pool di applicazioni usato dall'applicazione di servizio Reporting Services deve anche essere un amministratore del server di Analysis Services.
  
###  <a name="provision-subscriptions-and-alerts"></a><a name="bkmk_provision_agent"></a> Provisioning per sottoscrizioni e avvisi  
 Le funzionalità di sottoscrizione e avviso dati di Reporting Services possono richiedere la configurazione di autorizzazioni di SQL Server Agent. Se viene visualizzato un messaggio di errore che indica che SQL Server Agent è richiesto sebbene sia in esecuzione, aggiornare le autorizzazioni. È possibile fare clic sul collegamento **Provisioning di sottoscrizioni e avvisi** nella pagina di creazione dell'applicazione di servizio per passare a un'altra pagina di provisioning di SQL Server Agent. Il passaggio del provisioning è necessario se la distribuzione interessa più computer, ad esempio quando l'istanza di database di SQL Server si trova in un computer diverso. Per altre informazioni, vedere [Eseguire il provisioning di sottoscrizioni e avvisi per le applicazioni di servizio SSRS](../../reporting-services/install-windows/provision-subscriptions-and-alerts-for-ssrs-service-applications.md).  
  
### <a name="configure-e-mail-for-ssrs-service-applications"></a>Configurare la posta elettronica per le applicazioni di servizio SSRS  
 La funzionalità relativa agli avvisi dati di Reporting Services consente di inviare avvisi come messaggi di posta elettronica. Per inviare messaggi di posta elettronica potrebbe essere necessario configurare l'applicazione di servizio Reporting Services, nonché modificare l'estensione per il recapito tramite posta elettronica per l'applicazione di servizio. Se si pensa di usare l'estensione per il recapito dei messaggi di posta elettronica per la funzionalità di sottoscrizione di Reporting Services, è necessario configurare la posta elettronica. Per altre informazioni, vedere [Configurare le impostazioni di posta elettronica per l'applicazione di servizio Reporting Services &#40;SharePoint 2013 e SharePoint 2016&#41;](./configure-e-mail-for-a-reporting-services-service-application.md). 
  
### <a name="add-reporting-services-content-types-to-content-libraries"></a>Aggiungere i tipi di contenuto di Reporting Services alle raccolte contenuto  
 Reporting Services specifica tipi di contenuto predefiniti usati per gestire file di origini dati condivise con estensione rsds e file di definizione di report di Generatore report con estensione rdl. Se si aggiungono i tipi di contenuto **Report di Generatore report** e **Origine dati report** a una raccolta, sarà possibile utilizzare il comando **Nuovo** per creare nuovi documenti del tipo desiderato. Per altre informazioni, vedere [Aggiungere i tipi di contenuto di Reporting Services a una raccolta di SharePoint](../../reporting-services/report-server-sharepoint/add-reporting-services-content-types-to-a-sharepoint-library.md).  
  
### <a name="activate-the-report-server-file-sync-feature"></a>Attivare la funzionalità Sincronizzazione file server di report  
 La funzionalità a livello di sito **Sincronizzazione file server di report** è utile quando gli utenti caricano di frequente elementi di report pubblicati direttamente nelle raccolte documenti di SharePoint. La funzionalità di sincronizzazione file consente di sincronizzare il catalogo del server di report con gli elementi nelle raccolte documenti con maggiore frequenza. Per altre informazioni, vedere [Attivare la funzionalità Sincronizzazione file server di report in Amministrazione centrale SharePoint](../../reporting-services/report-server-sharepoint/activate-the-report-server-file-sync-feature-in-sharepoint-ca.md).  
  
##  <a name="verify-the-installation"></a><a name="bkmk_verify_installation"></a> Verificare l'installazione  
 Di seguito sono riportati i passaggi e le procedure consigliati per verificare la distribuzione in modalità SharePoint di Reporting Services.  
  
-   Vedere la sezione di SharePoint nell'argomento di verifica [Verify a Reporting Services Installation](../../reporting-services/install-windows/verify-a-reporting-services-installation.md).  
  
-   In una raccolta documenti di SharePoint creare un report di base di Reporting Services contenente solo una casella di testo, ad esempio un titolo. Il report non contiene origini dati e set di dati. L'obiettivo consiste nel verificare che sia possibile aprire Generatore report, compilare un report semplice e visualizzare il report in anteprima.  
  
     Salvare il report nella raccolta documenti e quindi eseguire il report dalla raccolta. Per altre informazioni sulla creazione di report con Generatore report, vedere [Avviare Generatore report (Generatore report)](../report-builder/start-report-builder.md).  
  
## <a name="next-steps"></a>Passaggi successivi

[PowerShell cmdlets for Reporting Services SharePoint Mode](../../reporting-services/report-server-sharepoint/powershell-cmdlets-for-reporting-services-sharepoint-mode.md)   
[Eseguire l'aggiornamento e la migrazione di Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)   
[Edizioni e funzionalità supportate per SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md)   
[Servizio SharePoint di Reporting Services e applicazioni di servizio](../../reporting-services/report-server-sharepoint/reporting-services-sharepoint-service-and-service-applications.md)  

Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)