---
description: Aggiornare Power Pivot per SharePoint
title: Aggiornare Power Pivot per SharePoint | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
ms.assetid: 80ba9e43-f3f0-4730-9fb1-2afd2dd3e6fc
author: Minewiskan
ms.author: owend
monikerRange: '>=sql-server-2016'
manager: erikre
ms.openlocfilehash: 971c23cfe9b49ad052c3808deca6e6221acb38e5
ms.sourcegitcommit: bf7577b3448b7cb0e336808f1112c44fa18c6f33
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2021
ms.locfileid: "104610939"
---
# <a name="upgrade-power-pivot-for-sharepoint"></a>Aggiornare Power Pivot per SharePoint

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
  Questo articolo riepiloga i passaggi necessari per aggiornare una distribuzione di [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] a [!INCLUDE[ssGeminiLong](../../includes/ssgeminilong-md.md)]. I passaggi specifici dipendono dalla versione di SharePoint in esecuzione nell'ambiente e includono il componente aggiuntivo [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] per SharePoint (**spPowerPivot.msi**).  
  
 **[!INCLUDE[applies](../../includes/applies-md.md)]**  SharePoint 2010 | SharePoint 2013  
  
 Per le note sulla versione, vedere [Note sulla versione di SQL Server 2016](../../sql-server/sql-server-2016-release-notes.md).  
  
 **Contenuto dell'articolo:**  
  
 [Prerequisiti](#bkmk_prereq)  
  
 [Aggiornare una farm SharePoint 2013 esistente](#bkmk_uprgade_sharepoint2013)  
  
 [Aggiornare una farm SharePoint 2010 esistente](#bkmk_uprgade_sharepoint2010)  
  
 [Cartelle di lavoro](#bkmk_workbooks)  
  
 [Aggiornamento dati](#bkmk_datarefresh)  
  
 [Verificare le versioni dei componenti e servizi di Power Pivot](#bkmk_verify_versions)  
  
 [Aggiornamento di più server Power Pivot per SharePoint in una farm SharePoint](#geminifarm)  
  
 [Applicazione di una correzione QFE a un'istanza Power Pivot nella farm](#qfe)  
  
 [Attività di verifica post-aggiornamento](#verify)  
  
## <a name="background"></a>Background  
  
-   Se viene eseguito l'aggiornamento di una farm SharePoint 2010 multiserver in cui sono incluse due o più istanze di [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] , è necessario eseguire l'aggiornamento completo di ciascun server **prima** di procedere con il server successivo. Un aggiornamento completo include l'esecuzione del programma di installazione di SQL Server per aggiornare i file di programma di [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] , quindi azioni di aggiornamento di SharePoint per la configurazione dei servizi aggiornati. La disponibilità dei server sarà limitata finché non verranno eseguite le azioni di aggiornamento nello strumento di configurazione [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] appropriato o in Windows PowerShell.  
  
-   Le versioni di tutte le istanze del Servizio di sistema [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] e Analysis Services in una farm SharePoint 2010 devono essere uguali. Per informazioni su come verificare la versione, vedere la sezione [Verificare le versioni dei componenti e servizi di Power Pivot](#bkmk_verify_versions) in questo articolo.  
  
-   Gli strumenti di configurazione [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] sono una delle funzionalità condivise di SQL Server. Tutte le funzionalità condivise vengono aggiornate contemporaneamente. Se durante un processo di aggiornamento si selezionano altre istanze o funzionalità di SQL Server per le quali è richiesto un aggiornamento della funzionalità condivisa, verrà aggiornato anche lo strumento di configurazione [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . È possibile che si riscontrino problemi se viene aggiornato lo strumento di configurazione [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] ma non l'istanza di [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Per altre informazioni sulle funzionalità condivise di SQL Server, vedere [Eseguire l'aggiornamento a SQL Server 2016 usando l'Installazione guidata &#40;programma di installazione&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md).  
  
-   Il componente aggiuntivo [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] per SharePoint (**spPowerPivot.msi**) viene installato side-by-side con le versioni precedenti. Ad esempio, il componente aggiuntivo viene installato nella cartella `c:\Program Files\Microsoft SQL Server\nnn\Tools\PowerPivotTools` . Per informazioni sui file di installazione SQL Server, vedere [percorsi dei file](../../sql-server/install/file-locations-for-default-and-named-instances-of-sql-server.md#shared-files-for-all-instances-of-) .
  
##  <a name="prerequisites"></a><a name="bkmk_prereq"></a> Prerequisiti  
 **Autorizzazioni**  
  
-   Per aggiornare un'installazione di [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] per SharePoint, è necessario essere un amministratore di farm. Per eseguire il programma di installazione di SQL Server, è necessario essere un amministratore locale.  
  
-   È necessario avere autorizzazioni **db_owner** per il database di configurazione della farm.  
  
 **SQL Server:**  
  
-   Se l'installazione esistente di [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] è [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], per l'aggiornamento a [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] è necessario [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] Service Pack 2 (SP2).  
  
-   Se l'installazione esistente di [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] è [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], per l'aggiornamento a [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] è necessario [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Service Pack 1 (SP1).  
  
 **SharePoint 2010:**  
  
-   Se l'installazione esistente esegue SharePoint 2010, installare SharePoint 2010 Service Pack 2 prima di effettuare l'aggiornamento a [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)][!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)]. Per ulteriori informazioni, vedere [Service Pack 2 per Microsoft SharePoint 2010](https://www.microsoft.com/download/details.aspx?id=39672). Utilizzare il comando `(Get-SPfarm).BuildVersion.ToString()` di PowerShell per verificare la versione. Per correlare la versione di build alla data di rilascio, vedere [Numeri di build di SharePoint 2010](https://www.toddklindt.com/blog/Lists/Posts/Post.aspx?ID=224).  
  
##  <a name="upgrade-an-existing-sharepoint-2013-farm"></a><a name="bkmk_uprgade_sharepoint2013"></a> Aggiornare una farm SharePoint 2013 esistente  
 Per aggiornare [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] distribuito in SharePoint 2013, effettuare le operazioni seguenti:  
  
 ![aggiornamento di PowerPivot per SharePoint 2013](../../database-engine/install-windows/media/as-powepivot-upgrade-flow-sharepoint2013.png "aggiornamento di PowerPivot per SharePoint 2013")  
  
1.  Eseguire il programma di installazione di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] nei server back-end che eseguono [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] in modalità SharePoint. Se il server ospita più istanze di [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], aggiornare almeno l'istanza di **POWERPIVOT** . Nell'elenco seguente vengono riepilogati i passaggi dell'Installazione guidata correlati a un aggiornamento di [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] :  
  
    1.  Nell'Installazione guidata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fare clic su **Installazione**.  
  
    2.  Fare clic su **Aggiorna da SQL Server**.  
  
    3.  Nella pagina **Seleziona istanza** selezionare il nome dell'istanza di **POWERPIVOT** , quindi fare clic su **Avanti**.  
  
    4.  Per altre informazioni, vedere [Eseguire l'aggiornamento a SQL Server 2016 usando l'Installazione guidata &#40;programma di installazione&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)  
  
2.  Riavviare il server.  
  
3.  Eseguire il componente aggiuntivo [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] per SharePoint (**spPowerPivot.msi**) in ogni server nella farm SharePoint 2013 per installare i provider di dati. Fanno eccezione i server in cui è stata eseguita l'Installazione guidata di SQL Server, che aggiorna anche i provider di dati. Per altre informazioni, vedere [Scaricare Microsoft SQL Server 2014 Power Pivot per Microsoft SharePoint 2013](https://www.microsoft.com/download/details.aspx?id=42300) e [Installare o disinstallare il componente aggiuntivo Power Pivot per SharePoint &#40;SharePoint 2013&#41;](/analysis-services/instances/install-windows/install-or-uninstall-the-power-pivot-for-sharepoint-add-in-sharepoint-2013).  
  
4.  **Eseguire il componente aggiuntivo [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] per SharePoint 2013** in uno dei server applicazioni di SharePoint per configurare la farm SharePoint con i file della soluzione aggiornati, installati dal componente aggiuntivo. Non è possibile utilizzare Amministrazione centrale SharePoint per questo passaggio. Per altre informazioni, vedere gli argomenti seguenti:  
  
    1.  Nella pagina iniziale di Windows digitare **[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]** e, nei risultati di ricerca, fare clic su **[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] per SharePoint 2013**. Si noti che la ricerca può restituire entrambe le versioni dello strumento di configurazione.  
  
         ![due strumenti di configurazione PowerPivot](/analysis-services/analysis-services/instances/install-windows/media/as-powerpivot-configtools-bothicons.gif "due strumenti di configurazione PowerPivot")  
  
         Oppure  
  
         Dal menu **Start** scegliere **Tutti i programmi**, fare clic su [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], **Strumenti di configurazione** e successivamente selezionare **[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] per SharePoint 2013**. Si noti che lo strumento è elencato solo se [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] è installato nel server locale.  
  
    2.  All'avvio, lo strumento di configurazione controlla lo stato dell'aggiornamento della soluzione farm [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] e delle soluzioni delle applicazioni Web [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Se vengono rilevate versioni precedenti di queste soluzioni, verrà visualizzato il messaggio "**Sono state rilevate versioni più recenti dei file di soluzione [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]. Selezionare l'opzione di aggiornamento per aggiornare la farm**". Fare clic su **OK** per chiudere il messaggio di convalida del sistema.  
  
    3.  Fare clic su **Aggiorna funzionalità, servizi, applicazioni e soluzioni**, quindi fare clic su **OK**.  
  
    4.  Rivedere le azioni nell'elenco attività del riquadro sinistro ed escludere quelle che non si desidera vengano eseguite dallo strumento. Tutte le azioni sono incluse per impostazione predefinita. Per rimuovere un'azione, selezionarla nell'elenco attività di sinistra, quindi deselezionare la casella di controllo **Includere l'azione nell'elenco attività** nella pagina **Parametri** .  
  
    5.  È possibile esaminare le informazioni dettagliate nella scheda **Script** o **Output** .  
  
         Nella scheda Output viene fornito un riepilogo delle azioni che saranno eseguite dallo strumento. Queste informazioni vengono salvate in file di log nel percorso: `C:\Program Files\Microsoft SQL Server\130\Tools\PowerPivotTools\SPAddinConfiguration\Log`.  
  
         Nella scheda Script sono visualizzati i cmdlet di PowerShell o i riferimenti ai file di script di PowerShell che verranno eseguiti dallo strumento.  
  
    6.  Fare clic su **Convalida** per controllare la validità di ogni azione. Se **Convalida** non è disponibile, significa che tutte le azioni sono valide per il sistema. Se **Convalida** è disponibile, potrebbe essere stato modificato un valore di input (ad esempio il nome applicazione del servizio di Excel) o lo strumento potrebbe aver determinato che non è possibile eseguire una determinata azione. Se non è possibile eseguire un'azione, è necessario escluderla o correggere le condizioni sottostanti per le quali l'azione viene contrassegnata come non valida.  
  
        > [!IMPORTANT]  
        >  È necessario elaborare sempre per prima la prima azione **Aggiornare la soluzione farm**. Con questa azione verranno registrati i cmdlet di PowerShell utilizzati per configurare il server. Se viene generato un errore per questa azione, non continuare. Utilizzare invece le informazioni fornite dall'errore per diagnosticare e risolvere il problema prima di elaborare ulteriori azioni nell'elenco attività.  
  
    7.  Fare clic su **Esegui** per eseguire tutte le azioni valide per questa attività. L'opzione **Esegui** è disponibile solo dopo il superamento del controllo di convalida. Quando fa clic su **Esegui** viene visualizzato il seguente messaggio di avviso, in cui si ricorda che le azioni vengono elaborate in modalità batch: "**Tutte le impostazioni di configurazione che sono contrassegnate come valide nello strumento verranno applicate alla farm di SharePoint. Continuare?** ".  
  
    8.  Per continuare, scegliere **Sì** .  
  
    9. Il completamento dell'aggiornamento delle soluzioni e delle funzionalità nella farm può richiedere diversi minuti. Durante questo periodo di tempo, le richieste di connessione ai dati [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]**avranno esito negativo** con errori di tipo "**Impossibile aggiornare i dati**" o "**Errore durante il tentativo di eseguire l'azione richiesta. Riprovare**". Al termine dell'aggiornamento, il server diventerà disponibile e questi errori non si verificheranno più.  
  
     Per altre informazioni, vedere gli argomenti seguenti:  
  
    -   [Strumenti di configurazione di Power Pivot](/analysis-services/power-pivot-sharepoint/power-pivot-configuration-tools)  
  
    -   [Configurare o ripristinare Power Pivot per SharePoint 2013 &#40;strumento di configurazione Power Pivot&#41;](/analysis-services/power-pivot-sharepoint/configure-or-repair-power-pivot-for-sharepoint-2013)  
  
    -   [Configurazione di Power Pivot con Windows PowerShell](/analysis-services/power-pivot-sharepoint/power-pivot-configuration-using-windows-powershell)  
  
    -   [Informazioni di riferimento su PowerShell per Power Pivot per SharePoint](/analysis-services/powershell/powershell-reference-for-power-pivot-for-sharepoint)  
  
5.  Verificare che l'aggiornamento sia stato completato correttamente eseguendo i passaggi di post-aggiornamento e controllando la versione dei server [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] nella farm. Per altre informazioni, vedere [Attività di verifica post-aggiornamento](#verify) in questo articolo e la sezione seguente.  
  
##  <a name="upgrade-an-existing-sharepoint-2010-farm"></a><a name="bkmk_uprgade_sharepoint2010"></a> Aggiornare una farm SharePoint 2010 esistente  
 Per aggiornare [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] distribuito in SharePoint 2010, effettuare le operazioni seguenti:  
  
 ![aggiornamento di PowerPivot per SharePoint 2010](../../database-engine/install-windows/media/as-powepivot-upgrade-flow-sharepoint2010.png "aggiornamento di PowerPivot per SharePoint 2010")  
  
1.  Scaricare [Service Pack 2 per Microsoft SharePoint 2010](https://www.microsoft.com/download/details.aspx?id=39672) e applicarlo a tutti i server nella farm. Verificare che l'installazione di SharePoint SP2 sia completata. Nella pagina Aggiornamento e migrazione di Amministrazione centrale aprire la pagina Controlla stato installazione prodotti e patch per visualizzare i messaggi di stato correlati a SP2.  
  
2.  Verificare che il servizio Windows Amministrazione SharePoint 2010 sia in esecuzione.  
  
    ```  
    Get-Service | where {$_.displayname -like "*SharePoint*"}  
    ```  
  
3.  Verificare che i servizi **SharePoint** **SQL Server Analysis Services** e che il **Servizio di sistema [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] per SQL Server** vengano avviati in Amministrazione centrale SharePoint o usare il comando PowerShell seguente:  
  
    ```  
    get-SPserviceinstance | where {$_.typename -like "*sql*"}  
    ```  
  
4.  Verificare che il servizio **Windows** **SQL Server Analysis Services ([!INCLUDE[ssGemini](../../includes/ssgemini-md.md)])** sia in esecuzione.  
  
    ```  
    Get-Service | where {$_.displayname -like "*powerpivot*"}  
    ```  
  
5.  **Eseguire il programma di installazione di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]** nel primo server applicazioni di SharePoint in cui viene eseguito il servizio Windows **SQL Server Analysis Services ([!INCLUDE[ssGemini](../../includes/ssgemini-md.md)])** per aggiornare l'istanza POWERPIVOT. Nella pagina Installazione dell'Installazione guidata di SQL Server scegliere l'opzione di aggiornamento. Per altre informazioni, vedere [Eseguire l'aggiornamento a SQL Server 2016 usando l'Installazione guidata &#40;programma di installazione&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md).  
  
6.  **Riavviare il server** prima di eseguire lo strumento di configurazione. Ciò garantisce che qualsiasi aggiornamento o prerequisito installato dal programma di installazione di SQL Server sia configurato completamente nel sistema.  
  
7.  **Eseguire lo strumento di configurazione[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]** nel primo server applicazioni di SharePoint in cui viene eseguito il servizio SQL Server Analysis Services ([!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]) per aggiornare le soluzioni e i servizi Web in SharePoint. Non è possibile utilizzare Amministrazione centrale per questo passaggio.  
  
    1.  Dal menu **Start** scegliere **Tutti i programmi**, fare clic su [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], **Strumenti di configurazione**, quindi su **[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] Strumento di configurazione**. Si noti che lo strumento è elencato solo se [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] è installato nel server locale.  
  
    2.  All'avvio, lo strumento di configurazione controlla lo stato dell'aggiornamento della soluzione farm [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] e delle soluzioni delle applicazioni Web [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Se vengono rilevate versioni precedenti di queste soluzioni, viene visualizzato il messaggio "Sono state rilevate versioni più recenti dei file di soluzione [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]. Selezionare l'opzione di aggiornamento per aggiornare la farm". Fare clic su **OK** per chiudere la finestra di messaggio.  
  
    3.  Fare clic su **Aggiorna funzionalità, servizi, applicazioni e soluzioni**, quindi su **OK** per continuare.  
  
    4.  Viene visualizzato l'avviso seguente: "Le cartelle di lavoro nel dashboard di gestione [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] stanno per essere aggiornate all'ultima versione. Tutte le personalizzazioni apportate alle cartelle di lavoro esistenti andranno perse. Continuare?"  
  
         Questo avviso fa riferimento alle cartelle di lavoro nel dashboard di gestione [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] che generano report relativi all'attività di aggiornamento dati. Se le cartelle di lavoro sono state personalizzate, qualsiasi modifica apportata a tali cartelle di lavoro verrà persa quando i file esistenti vengono sostituiti con le versioni più recenti.  
  
         Fare clic su **Sì** per sovrascrivere le cartelle di lavoro con versioni più recenti. In alternativa, fare clic su **No** per tornare alla home page. Salvare le cartelle di lavoro in un percorso diverso in modo da conservarne una copia, quindi tornare a questo passaggio quando è possibile continuare.  
  
         Per ulteriori informazioni sulla personalizzazione delle cartelle di lavoro utilizzate nel dashboard, vedere l'argomento relativo alla [personalizzazione del dashboard di gestione Power Pivot](/previous-versions/sql/sql-server-2008-r2/ff718155(v=sql.105)).  
  
    5.  Rivedere le azioni nell'elenco attività ed escludere quelle che non si desidera vengano eseguite dallo strumento. Tutte le azioni sono incluse per impostazione predefinita. Per rimuovere un'azione, selezionarla nell'elenco attività, quindi deselezionare la casella di controllo **Includere l'azione nell'elenco attività** nella pagina Parametri.  
  
    6.  È possibile esaminare le informazioni dettagliate nella scheda **Output** o **Script** .  
  
         Nella scheda Output viene fornito un riepilogo delle azioni che saranno eseguite dallo strumento. Queste informazioni vengono salvate in file di log nel percorso: `c:\Program Files\Microsoft SQL Server\130\Tools\PowerPivotTools\ConfigurationTool\Log`.  
  
         Nella scheda Script sono visualizzati i cmdlet di PowerShell o i riferimenti ai file di script di PowerShell che verranno eseguiti dallo strumento.  
  
    7.  Fare clic su **Convalida** per controllare la validità di ogni azione. Se **Convalida** non è disponibile, significa che tutte le azioni sono valide per il sistema. Se **Convalida** è disponibile, potrebbe essere stato modificato un valore di input (ad esempio il nome applicazione del servizio di Excel) o lo strumento potrebbe aver determinato che non è possibile eseguire una determinata azione. Se non è possibile eseguire un'azione, è necessario escluderla o correggere le condizioni sottostanti per le quali l'azione viene contrassegnata come non valida.  
  
        > [!IMPORTANT]  
        >  È necessario elaborare sempre per prima la prima azione **Aggiornare la soluzione farm**. Con questa azione verranno registrati i cmdlet di PowerShell utilizzati per configurare il server. Se viene generato un errore per questa azione, non continuare. Utilizzare invece le informazioni fornite dall'errore per diagnosticare e risolvere il problema prima di elaborare ulteriori azioni nell'elenco attività.  
  
    8.  Fare clic su **Esegui** per eseguire tutte le azioni valide per questa attività. L'opzione **Esegui** è disponibile solo dopo il superamento del controllo di convalida. Quando fa clic su **Esegui** viene visualizzato il seguente messaggio di avviso, in cui si ricorda che le azioni vengono elaborate in modalità batch: "Tutte le impostazioni di configurazione che sono contrassegnate come valide nello strumento verranno applicate alla farm di SharePoint. Continuare?"  
  
    9. Per continuare, scegliere **Sì** .  
  
    10. Il completamento dell'aggiornamento delle soluzioni e delle funzionalità nella farm può richiedere diversi minuti. Durante questo periodo di tempo le richieste di connessione ai dati [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] avranno esito negativo con errori di tipo "Impossibile aggiornare i dati" o "Errore durante il tentativo di eseguire l'azione richiesta. Riprovare". Al termine dell'aggiornamento, il server diventerà disponibile e questi errori non si verificheranno più.  
  
8.  **Ripetere il processo** per ogni servizio SQL Server Analysis Services ([!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]) nella farm: 1) Eseguire il programma di installazione di SQL Server 2) Eseguire lo strumento di configurazione [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)].  
  
9. Verificare che l'aggiornamento sia stato completato correttamente eseguendo i passaggi di post-aggiornamento e controllando la versione dei server [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] nella farm. Per altre informazioni, vedere [Attività di verifica post-aggiornamento](#verify) in questo articolo e la sezione seguente.  
  
10. **Risoluzione dei problemi**  
  
     È possibile visualizzare informazioni sull'errore nel riquadro Parametri per ciascuna azione.  
  
     Per i problemi correlati alla distribuzione o al ritiro della soluzione, verificare che il servizio SharePoint 2010 Administrator sia avviato. Tramite questo servizio vengono eseguiti i processi timer che attivano le modifiche della configurazione in una farm. Se il servizio non è in esecuzione, la distribuzione o il ritiro della soluzione avrà esito negativo. Gli errori persistenti indicano che un processo di distribuzione o ritiro esistente è già in coda e blocca ulteriori azioni da parte dello strumento di configurazione.  
  
    1.  Avviare la shell di gestione SharePoint 2010 come amministratore, quindi eseguire il comando indicato di seguito per visualizzare i processi in coda:  
  
        ```  
        Stsadm -o enumdeployments  
        ```  
  
    2.  Esaminare le distribuzioni esistenti per le informazioni seguenti: **Tipo** è Retraction o Deployment, **File** è powerpivotwebapp.wsp o powerpivotfarm.wsp.  
  
    3.  Per le distribuzioni o i ritiri correlati alle soluzioni [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)], copiare il valore GUID per **JobId** e incollarlo nel comando seguente. Per copiare il GUID, usare i comandi Contrassegna, Copia e Incolla del menu Modifica della shell:  
  
        ```  
        Stsadm -o canceldeployment -id "<GUID>"  
        ```  
  
    4.  Riprovare l'attività nello strumento di configurazione facendo clic su **Convalida** , quindi su **Esegui**.  
  
     Per tutti gli altri errori, controllare i log ULS. Per altre informazioni, vedere [Configurare e visualizzare i file di log di SharePoint e la registrazione diagnostica &#40;Power Pivot per SharePoint&#41;](/analysis-services/power-pivot-sharepoint/configure-and-view-sharepoint-and-diagnostic-logging).  
  
##  <a name="workbooks"></a><a name="bkmk_workbooks"></a> Cartelle di lavoro  
 L'aggiornamento di un server non comporta necessariamente l'aggiornamento delle cartelle di lavoro di [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] in esso eseguite, ma le cartelle di lavoro meno recenti create con la versione precedente di [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] per Excel continueranno a funzionare come prima, usando le funzionalità disponibili in questa versione. Le cartelle di lavoro rimangono funzionali perché in un server aggiornato è disponibile la versione del provider OLE DB di Analysis Services che faceva parte dell'installazione precedente.  
  
##  <a name="data-refresh"></a><a name="bkmk_datarefresh"></a> Aggiornamento dati  
 L'aggiornamento interesserà le operazioni di aggiornamento dati. L'aggiornamento dati pianificato nel server è disponibile solo per le cartelle di lavoro corrispondenti alla versione del server. Se vengono ospitate cartelle di lavoro della versione precedente, l'aggiornamento dati potrebbe non funzionare più per tali cartelle di lavoro. Per abilitare di nuovo l'aggiornamento dati, è necessario aggiornare le cartelle di lavoro. È possibile aggiornare ogni cartella di lavoro manualmente in [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] per Excel o abilitare la funzionalità di aggiornamento automatico dei dati in SharePoint 2010. Con l'aggiornamento automatico sarà possibile aggiornare una cartella di lavoro alla versione corrente prima dell'esecuzione dell'aggiornamento dati, consentendo alle operazioni di aggiornamento dati di rimanere pianificate.  
  
##  <a name="verify-the-versions-of-power-pivot-components-and-services"></a><a name="bkmk_verify_versions"></a> Verificare le versioni dei componenti e servizi di Power Pivot  
 La versione di tutte le istanze del servizio di sistema [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] e di Analysis Services deve essere uguale. Per verificare che la versione di tutti i componenti server sia uguale, controllare le informazioni sulla versione per gli elementi seguenti:  
  
### <a name="verify-the-version-of-power-pivot-solutions-and-the-power-pivot-system-service"></a>Verificare la versione delle soluzioni Power Pivot e del Servizio di sistema Power Pivot  
 Eseguire il comando PowerShell seguente:  
  
```  
Get-PowerPivotSystemService  
```  
  
 Verificare **CurrentSolutionVersion**. La versione di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]è la 13.0.\<major build>.\<minor build>  
  
### <a name="verify-the-version-of-the-analysis-services-windows-service"></a>Verificare la versione del servizio Windows Analysis Services  
 Se sono stati aggiornati solo alcuni dei server [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] di una farm SharePoint 2010, l'istanza di [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] nei server non aggiornati sarà meno recente della versione prevista nella farm. Sarà necessario effettuare l'aggiornamento di tutti i server alla stessa versione affinché possano essere utilizzati. Usare uno dei metodi seguenti per verificare la versione del servizio Windows SQL Server Analysis Services ([!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]) in ogni computer.  
  
 **Esplora file di Windows**:  
  
1.  Passare alla cartella **Bin** per l'istanza di [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Ad esempio `C:\Program Files\Microsoft SQL Server\MSAS13.POWERPIVOT\OLAP\bin`.  
  
2.  Fare clic con il pulsante destro del mouse su `msmdsrv.exe`e selezionare **Proprietà**.  
  
3.  Fare clic su **Dettagli**.  
  
4.  La versione file di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] deve essere la 13.00.\<major build>.\<minor build>.  
  
5.  Verificare che il numero sia identico alla versione della soluzione e del Servizio di sistema [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
 **Informazioni sull'avvio del servizio:**  
  
 All'avvio, il servizio [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] scrive le informazioni sulla versione nel registro eventi di Windows.  
  
1.  Eseguire il `eventvwr`  
  
2.  Creare un filtro per l'elemento `MSOLAP$POWERPIVOT`di origine.  
  
3.  Cercare un evento del livello informazioni simile al seguente  
  
     Servizio   avviato. Microsoft SQL Server Analysis Services 64 Bit Evaluation (x64) RTM **13.0.2000.8**.  
  
 **Utilizzare PowerShell per verificare la versione del file.**  
  
 È possibile utilizzare PowerShell per verificare la versione del prodotto. PowerShell è un'ottima scelta se si desidera eseguire lo script o automatizzare la verifica della versione.  
  
```  
(get-childitem "C:\Program Files\Microsoft SQL Server\MSAS13.POWERPIVOT2000\OLAP\bin\msmdsrv.exe").VersionInfo  
```  
  
 Il comando PowerShell riportato sopra restituisce informazioni simili alle seguenti:  
  
 VersioneProdotto   VersioneFile           NomeFile  
  
 **13.0.2000.8** 2016.0130.200    C:\Programmi\Microsoft SQL Server\MSAS13.POWERPIVOT2000\OLAP\bin\msmdsrv.exe  
  
### <a name="verify-the-msolap-data-provider-version-on-sharepoint"></a>Verificare la versione del provider di dati MSOLAP in SharePoint  
 Utilizzare le istruzioni seguenti per controllare quali versioni dei provider OLE DB di Analysis Services sono considerate attendibili da Excel Services. È necessario essere amministratore di una farm o di un'applicazione di servizio per controllare le impostazioni del provider di dati attendibile di Excel Services.  
  
1.  In Gestione applicazioni di Amministrazione centrale fare clic su **Gestisci applicazioni di servizio**.  
  
2.  Fare clic sul nome dell'applicazione di servizio Excel Services, ad esempio **ExcelServiceApp1**.  
  
3.  Fare clic su **Provider di dati attendibili**. Dovrebbe essere visualizzato MSOLAP.5 (Provider OLE DB Microsoft per OLAP Services 11.0). Se è stata aggiornata l'installazione di [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] , sarà anche visualizzato MSOLAP.4 dalla versione precedente.  
  
4.  Per ulteriori informazioni, vedere [Aggiungere MSOLAP.5 come provider di dati attendibile in Excel Services](/analysis-services/power-pivot-sharepoint/add-msolap-5-as-a-trusted-data-provider-in-excel-services).  
  
 MSOLAP.4 viene descritto come provider Microsoft OLE DB per OLAP Services 10.0. Questa versione potrebbe essere quella predefinita per [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] installata con Excel Services o la versione per [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] . La versione predefinita installata da SharePoint non supporta l'accesso ai dati [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . È necessario disporre della versione per [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] o successiva per connettersi alle cartelle di lavoro di [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] su SharePoint. Per verificare la disponibilità della versione per [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] , utilizzare le istruzioni nella sezione precedente in cui viene illustrato come verificare la versione visualizzando le proprietà del file.  
  
### <a name="verify-the-adomdnet-data-provider-version"></a>Verificare la versione del provider di dati ADOMD.NET  
 Utilizzare le istruzioni seguenti per controllare quale versione di ADOMD.NET è installata. È necessario essere amministratore di una farm o di un'applicazione di servizio per controllare le impostazioni del provider di dati attendibile di Excel Services.  
  
1.  Nel server applicazioni SharePoint, passare a `c:\Windows\Assembly`.  
  
2.  Ordinare per nome di assembly e individuare **Microsoft.Analysis Services.Adomd.Client**.  
  
3.  Verificare di avere la versione 13.0.\<build number>.  
  
##  <a name="upgrading-multiple-power-pivot-for-sharepoint-servers-in-a-sharepoint-farm"></a><a name="geminifarm"></a> Aggiornamento di più server Power Pivot per SharePoint in una farm SharePoint  
 In una topologia multiserver in cui sono inclusi più server [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] , la versione di tutti i componenti e di tutte le istanze del server deve essere uguale. Il server che esegue la versione più recente del software imposta il livello per tutti i server nella farm. Se vengono aggiornati solo alcuni server, quelli che eseguono versioni meno recenti del software non saranno più disponibili finché non verranno aggiornati.  
  
 Dopo l'aggiornamento del primo server, gli altri server che non sono ancora stati aggiornati **non saranno più disponibili**. La disponibilità viene ripristinata quando tutti i server presentano lo stesso livello funzionale.  
  
 Il programma di installazione di SQL Server aggiorna i file di soluzione [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] nel computer fisico. Per aggiornare le soluzioni in uso nella farm, è necessario usare lo strumento di configurazione [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] descritto in una sezione precedente di questo articolo.  
  
##  <a name="applying-a-qfe-to-a-power-pivot-instance-in-the-farm"></a><a name="qfe"></a> Applicazione di una correzione QFE a un'istanza Power Pivot nella farm  
 L'applicazione della patch a un server [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] per SharePoint consente di effettuare l'aggiornamento dei file di programma esistenti a una versione più recente in cui è inclusa una correzione per un problema specifico. In caso di applicazione di una correzione QFE a una topologia multiserver, non esiste un server primario da cui iniziare. È possibile iniziare con qualsiasi server finché si applica la stessa correzione QFE agli altri server [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] nella farm.  
  
 Quando si applica la correzione QFE, è necessario eseguire anche un passaggio di configurazione per aggiornare le informazioni sulla versione del server nel database di configurazione della farm. La versione del server per cui è stata installata la patch diventa la nuova versione prevista per la farm. Fino a quando la correzione QFE non viene applicata e configurata in tutti i computer, le istanze di [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] per SharePoint che non contengono la correzione QFE non saranno disponibili per gestire le richieste per i dati [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
 Per verificare che la correzione QFE sia stata applicata e configurata correttamente, seguire queste istruzioni:  
  
1.  Installare la patch attenendosi alle istruzioni fornite con la correzione QFE.  
  
2.  Avviare lo strumento di configurazione di [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
3.  Fare clic su **Aggiorna funzionalità, servizi, applicazioni e soluzioni**, quindi fare clic su **OK**.  
  
4.  Rivedere le azioni incluse nell'attività di aggiornamento, quindi fare clic su **Convalida**.  
  
5.  Fare clic su **Esegui** per applicare le azioni.  
  
6.  Ripetere per le istanze aggiuntive di [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] per SharePoint nella farm.  
  
    > [!IMPORTANT]  
    >  In una distribuzione multiserver assicurarsi di applicare la patch a ogni istanza e di configurarla prima di continuare con il computer successivo. È necessario completare l'attività di aggiornamento con lo strumento di configurazione [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] per l'istanza corrente prima di passare all'istanza successiva.  
  
 Per controllare le informazioni sulla versione per i servizi della farm, utilizzare la pagina **Controlla stato installazione prodotti e patch** nella sezione Gestione aggiornamento e patch di Amministrazione centrale.  
  
##  <a name="post-upgrade-verification-tasks"></a><a name="verify"></a> Attività di verifica post-aggiornamento  
 Al termine dell'aggiornamento, eseguire i passaggi seguenti per verificare che il server sia operativo.  
  
|Attività|Collegamento|  
|----------|----------|  
|Verificare che il servizio sia in esecuzione in tutti i computer in cui è eseguito [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] per SharePoint.|[Avviare o arrestare un server Power Pivot per SharePoint](/analysis-services/power-pivot-sharepoint/start-or-stop-a-power-pivot-for-sharepoint-server)|  
|Verificare l'attivazione della funzionalità a livello di raccolta siti.|[Attivare l'integrazione delle funzionalità di Power Pivot per le raccolte siti in Amministrazione centrale](/analysis-services/power-pivot-sharepoint/activate-power-pivot-integration-for-site-collections-in-ca)|  
|Verificare che le singole cartelle di lavoro di [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] vengano caricate correttamente aprendo una cartella di lavoro e facendo clic sui filtri e sul filtro dei dati per avviare una query.|Verificare la presenza di file memorizzati nella cache sul disco rigido. Un file memorizzato nella cache conferma che il file di dati è stato caricato in quel server fisico. Cercare i file memorizzati nella cache nella cartella c:\Programmi\Microsoft SQL Server\MSAS13.POWERPIVOT\OLAP\Backup.|  
|Testare l'aggiornamento dei dati nelle cartelle di lavoro selezionate configurate per l'aggiornamento dei dati.|Il modo più semplice per testare l'aggiornamento dei dati consiste nel modificare una pianificazione di aggiornamento dei dati, scegliendo la casella di controllo **Aggiorna anche appena possibile** in modo che l'aggiornamento dei dati venga eseguito immediatamente. Con questo passaggio si determinerà se l'aggiornamento dei dati viene completato correttamente per la cartella di lavoro corrente. Ripetere questi passaggi per le altre cartelle di lavoro utilizzate frequentemente per assicurare che l'aggiornamento dei dati sia operativo. Per altre informazioni sulla pianificazione dell'aggiornamento dei dati, vedere [Pianificare un aggiornamento dei dati (Power Pivot per SharePoint)](/sharepoint/administration/data-refresh-using-the-unattended-data-refresh-account).|  
|Nel tempo, monitorare i report dell'aggiornamento dati in Dashboard di gestione [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] per verificare che non ci siano errori di aggiornamento dati.|[Dati di utilizzo e dashboard di gestione PowerPivot](/analysis-services/power-pivot-sharepoint/power-pivot-management-dashboard-and-usage-data)|  
  
 Per altre informazioni su come configurare le impostazioni e funzionalità di [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , vedere [Amministrazione e configurazione del server Power Pivot in Amministrazione centrale](/analysis-services/power-pivot-sharepoint/power-pivot-server-administration-and-configuration-in-central-administration).  
  
 Per istruzioni dettagliate su tutte le attività di configurazione post-installazione, vedere [Configurazione iniziale (Power Pivot per SharePoint)](/sharepoint/administration/configure-power-pivot-for-sharepoint-2013).  
  
## <a name="see-also"></a>Vedere anche  
 [Edizioni e funzionalità supportate per SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md)   
 [Installazione di Power Pivot per SharePoint 2010](https://sharepointgeorge.com/2012/installing-sql-server-powerpivot-sharepointstep-step-guide/)  
  
