---
title: 'Esercitazione: Creare un report grafico rapido offline (Generatore report) | Microsoft Docs'
description: In questa esercitazione viene creato il grafico a torta in un report impaginato di Reporting Services in Generatore report. Si vedrà poi come aggiungere le percentuali e modificare il grafico a torta.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-builder
ms.topic: conceptual
helpviewer_keywords:
- reports, creating
- tutorials, getting started
- creating reports
ms.assetid: 6b1db67a-cf75-494c-b70c-09f1e6a8d414
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 2f7fb9781583a56a2765f066462e62717c1d32d6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070551"
---
# <a name="tutorial-create-a-quick-chart-report-offline-report-builder"></a>Esercitazione: Creare un report grafico rapido offline (Generatore report)

  In questa esercitazione viene usata una procedura guidata per creare un grafico a torta in un report impaginato [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] in [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]. Aggiungere le percentuali e modificare leggermente il grafico a torta. 
  
È possibile eseguire questa esercitazione in due modi diversi. Con entrambi i metodi si otterrà lo stesso risultato, ovvero un grafico a torta simile a quello riportato in questa illustrazione:  
  
 ![Grafico a torta rapido di Generatore report](../../reporting-services/report-builder/media/report-builder-quick-pie-chart.png "Grafico a torta rapido di Generatore report")  
  
## <a name="prerequisites"></a>Prerequisiti  
 Se si usano dati XML o una query [!INCLUDE[tsql](../../includes/tsql-md.md)], è necessario avere accesso a Generatore report. È possibile avviare [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] da un server di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] in modalità nativa o in modalità integrata SharePoint, oppure è possibile scaricare [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] dall'Area download Microsoft. Per altre informazioni, vedere [Install Report Builder](../../reporting-services/install-windows/install-report-builder.md).  
  
##  <a name="two-ways-to-do-this-tutorial"></a><a name="TwoWays"></a> Due modi per eseguire questa esercitazione  
  
-   [Creare il grafico a torta con dati XML](#CreatePieChartXML)  
  
-   [Creare il grafico a torta con una query Transact-SQL contenente dati](#CreatePieQueryData)  
  
### <a name="using-xml-data-for-this-tutorial"></a>Utilizzo di dati XML per l'esercitazione  
 È possibile utilizzare dati XML copiandoli da questo argomento e incollandoli nella procedura guidata. Non è necessario essere connessi a un server di report di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] in modalità nativa o in modalità integrata SharePoint e nemmeno accedere a un'istanza di SQL Server.  
  
 [Creare il grafico a torta con dati XML](#CreatePieChartXML)  
  
### <a name="using-a-tsql-query-that-contains-data-for-this-tutorial"></a>Uso di una query [!INCLUDE[tsql](../../includes/tsql-md.md)] contenente dati per l'esercitazione  
 È possibile copiare una query contenente dati da questo argomento e incollarla nella procedura guidata. Sarà necessario avere il nome di un'istanza di SQL Server e di credenziali sufficienti per l'accesso in sola lettura a qualsiasi database. Per la query del set di dati dell'esercitazione vengono usati dati letterali, ma è necessario elaborare la query da un'istanza di SQL Server per restituire i metadati richiesti per un set di dati del report.  
  
 Il vantaggio dell'uso della query [!INCLUDE[tsql](../../includes/tsql-md.md)] è dato dal fatto che in tutte le altre esercitazioni di [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] viene usato lo stesso metodo, pertanto durante le altre esercitazioni si conosceranno già le azioni da eseguire.  
  
 Per la query [!INCLUDE[tsql](../../includes/tsql-md.md)] sono necessari pochi altri prerequisiti. Per altre informazioni, vedere [Prerequisiti per le esercitazioni &#40;Generatore report&#41;](../../reporting-services/prerequisites-for-tutorials-report-builder.md).  
  
 [Creare il grafico a torta con una query Transact-SQL contenente dati](#CreatePieQueryData)  
  
##  <a name="creating-the-pie-chart-with-xml-data"></a><a name="CreatePieChartXML"></a> Creazione del grafico a torta con dati XML  
  
1.  [Avviare Generatore report](../../reporting-services/report-builder/start-report-builder.md) dal portale Web di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] o dal server di report in modalità integrata SharePoint oppure dal computer.  
  
     Verrà visualizzata la finestra di dialogo **Riquadro attività iniziale** .  
  
     ![Riquadro attività iniziale di Generatore report](../../reporting-services/media/rb-getstarted.png "Riquadro attività iniziale di Generatore report")  
  
     Se la finestra di dialogo **Riquadro attività iniziale** non viene visualizzata, fare clic su **File** >**Nuovo**. La finestra di dialogo **Nuovo report o set di dati** include all'incirca lo stesso contenuto della finestra di dialogo **Riquadro attività iniziale** .  
  
2.  Nel riquadro sinistro verificare che sia selezionata l'opzione **Nuovo report** .  
  
3.  Nel riquadro destro fare clic su **Creazione guidata grafico**, quindi scegliere **Crea**.  
  
4.  Nella pagina **Scegliere un set di dati** fare clic su **Crea un set di dati**, quindi scegliere **Avanti**.  
  
5.  Nella pagina **Scegliere una connessione a un'origine dei dati** fare clic su **Nuova**.  
  
     Verrà visualizzata la finestra di dialogo **Proprietà origine dati** .  
  
6.  È possibile assegnare qualsiasi nome a un'origine dati. Nella casella **Nome** digitare **Grafico a torta**.  
  
7.  Nella casella **Seleziona tipo di connessione** fare clic su **XML**.  
  
8.  Fare clic sulla scheda **Credenziali**, selezionare **Use current Windows user (Usa utente di Windows corrente). Può essere richiesta la delega Kerberos**, quindi fare clic su **OK**.  
  
9. Nella pagina **Scegliere una connessione a un'origine dei dati** fare clic su **Grafico a torta**, quindi su **Avanti**.  
  
10. Copiare il testo seguente e incollarlo nella casella grande nella parte superiore della pagina **Progetta query** .  
  
    ```  
    <Query>  
    <ElementPath>Root /S  {@Sales (Integer)} /C {@FullName} </ElementPath>  
    <XmlData>  
    <Root>  
    <S Sales="150">  
      <C FullName="Jae Pak" />  
    </S>  
    <S Sales="350">  
      <C FullName="Jillian  Carson" />  
    </S>  
    <S Sales="250">  
      <C FullName="Linda C Mitchell" />  
    </S>  
    <S Sales="500">  
      <C FullName="Michael Blythe" />  
    </S>  
    <S Sales="450">  
      <C FullName="Ranjit Varkey" />  
    </S>  
    </Root>  
    </XmlData>  
    </Query>  
    ```  
  
11. (Facoltativo) Fare clic sul pulsante **Esegui** ( **!** ) per visualizzare i dati su cui si baserà il grafico.  
  
     ![Progetta query in Generatore report](../../reporting-services/report-builder/media/rb-designquery.png "Progetta query in Generatore report")  
  
12. Fare clic su **Avanti**.  
  
13. Nella pagina **Scegliere un tipo di grafico** fare clic su **Torta**, quindi scegliere **Avanti**.  
  
14. Nella pagina relativa alla **disposizione dei campi del grafico** fare doppio clic sul campo **Vendite** nella casella **Campi disponibili**.  
  
     Il campo verrà spostato automaticamente nella casella **Valori** perché rappresenta un valore numerico.  
  
     ![Disposizione dei campi nella procedura guidata di Generatore report](../../reporting-services/report-builder/media/rb-wizarrangefields.png "Disposizione dei campi nella procedura guidata di Generatore report")  
  
15. Trascinare il campo **FullName** dalla casella **Campi disponibili** alla casella **Categorie**. In alternativa è possibile fare doppio clic sul campo per spostarlo nella casella **Categorie**. Al termine fare clic su **Avanti**.  
  
     La pagina di anteprima mostra il nuovo grafico a torta con dati rappresentativi. Nella legenda sono riportate le diciture Full Name 1, Full Name 2 e così via, anziché i nomi dei venditori, e le dimensioni delle sezioni della torta non sono precise. L'esempio serve solo per dare un'idea generale dell'aspetto del report.  
  
     ![Anteprima del nuovo grafico in Generatore report](../../reporting-services/report-builder/media/rb-newchartpreview.png "Anteprima del nuovo grafico in Generatore report")  
  
16. Fare clic su **Fine**.  
  
     È ora possibile visualizzare il nuovo report con grafico a torta nella visualizzazione Progettazione, sempre con dati rappresentativi.  
  
     ![Nuovo grafico a torta di Generatore report nella visualizzazione progettazione](../../reporting-services/report-builder/media/rb-newpiedesign.png "Nuovo grafico a torta di Generatore report nella visualizzazione progettazione")  
  
17. Per visualizzare il grafico a torta effettivo, fare clic su **Esegui** nella scheda **Home** della barra multifunzione.  
  
     ![Esecuzione del nuovo grafico in Generatore report](../../reporting-services/report-builder/media/rb-newchartrun.png "Esecuzione del nuovo grafico in Generatore report")  
  
18. Per continuare la modifica del grafico a torta, vedere [Al termine della procedura guidata](#AfterWizard) in questo articolo.  
  
##  <a name="creating-the-pie-chart-with-a-tsql-query"></a><a name="CreatePieQueryData"></a> Creazione del grafico a torta con una query [!INCLUDE[tsql](../../includes/tsql-md.md)]  
  
1.  [Avviare Generatore report](../../reporting-services/report-builder/start-report-builder.md) dal portale Web di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , dal server di report in modalità integrata SharePoint oppure dal computer.  
  
     Verrà visualizzata la finestra di dialogo **Riquadro attività iniziale** .  
  
    > [!NOTE]  
    >  Se la finestra di dialogo **Riquadro attività iniziale** non viene visualizzata, fare clic su **File** >**Nuovo**. La finestra di dialogo **Nuovo report o set di dati** include all'incirca lo stesso contenuto della finestra di dialogo **Riquadro attività iniziale** .  
  
2.  Nel riquadro sinistro verificare che sia selezionata l'opzione **Nuovo report** .  
  
3.  Nel riquadro destro fare clic su **Creazione guidata grafico**, quindi scegliere **Crea**.  
  
4.  Nella pagina **Scegliere un set di dati** fare clic su **Crea un set di dati**, quindi scegliere **Avanti**.  
  
5.  Nella pagina **Scegliere una connessione a un'origine dei dati** selezionare un'origine dati esistente o individuare il server di report, quindi selezionare un'origine dati e scegliere **Avanti**. Potrebbe essere necessario immettere un nome utente e una password.  
  
    > [!NOTE]  
    >  L'origine dati scelta non ha importanza purché si disponga delle autorizzazioni appropriate. Non verranno recuperati dati dall'origine dati. Per altre informazioni, vedere [Prerequisiti per le esercitazioni &#40;Generatore report&#41;](../../reporting-services/prerequisites-for-tutorials-report-builder.md).  
  
6.  Nella pagina **Progetta query** fare clic su **Modifica come testo**.  
  
7.  Incollare la query seguente nel relativo riquadro:  
  
    ```  
    SELECT 150 AS Sales, 'Jae Pak' AS FullName   
    UNION SELECT 350 AS Sales, 'Jillian Carson' AS FullName   
    UNION SELECT 250 AS Sales, 'Linda C Mitchell' AS FullName   
    UNION SELECT 500 AS Sales, 'Michael Blythe' AS FullName   
    UNION SELECT 450 AS Sales, 'Ranjit Varkey' AS FullName   
    ```  
  
8.  (Facoltativo) Fare clic sul pulsante Esegui ( **!** ) per visualizzare i dati su cui si baserà il grafico.  
  
9. Fare clic su **Avanti**.  
  
10. Nella pagina **Scegliere un tipo di grafico** fare clic su **Torta**, quindi scegliere **Avanti**.  
  
11. Nella pagina relativa alla **disposizione dei campi del grafico** fare doppio clic sul campo **Vendite** nella casella **Campi disponibili**.  
  
     Il campo verrà spostato automaticamente nella casella **Valori** perché rappresenta un valore numerico.  
  
12. Trascinare il campo **FullName** dalla casella **Campi disponibili** alla casella **Categorie**. In alternativa è possibile fare doppio clic sul campo per spostarlo nella casella **Categorie**. Al termine fare clic su **Avanti**.  
  
13. Fare clic su **Fine**.  
  
     Osservare ora il nuovo report grafico a torta nell'area di progettazione. Gli elementi visualizzati sono esemplificativi. Nella legenda sono riportate le diciture Full Name 1, Full Name 2 e così via, anziché i nomi dei venditori, e le dimensioni delle sezioni della torta non sono precise. L'esempio serve solo per dare un'idea generale dell'aspetto del report.  
  
15. Per visualizzare il grafico a torta effettivo, fare clic su **Esegui** nella scheda **Home** della barra multifunzione.  
 
##  <a name="after-you-run-the-wizard"></a><a name="AfterWizard"></a> Al termine della procedura guidata  
 Dopo aver terminato la creazione del report del grafico a torta, è possibile modificarlo nel modo desiderato. Nella scheda **Esegui** della barra multifunzione fare clic su **Progettazione** per continuare a modificarlo.  
  
## <a name="make-the-chart-bigger"></a>Ingrandire il grafico  
 Potrebbe essere necessario ingrandire il grafico a torta. 
 
*  Fare clic sul grafico, ma non sugli elementi in esso contenuti, per selezionarlo e trascinare l'angolo inferiore destro per ridimensionarlo.  

L'area di progettazione viene ingrandita man mano che viene trascinata.
  
## <a name="add-a-report-title"></a>Aggiungere un titolo al report  
1. Selezionare le parole **Titolo grafico** nella parte superiore del grafico, quindi digitare un titolo, ad esempio **Grafico a torta - Vendite**.  
2. Dopo aver selezionato il titolo, nel riquadro Proprietà modificare **Colore** in **Nero** e **FontSize**(Dimensione carattere) in **12 pt**.
  
## <a name="add-percentages"></a>Aggiungere percentuali  
 
1.  Fare clic con il pulsante destro del mouse sul grafico a torta e scegliere **Mostra etichette dati**. Le etichette dati vengono visualizzate in ogni sezione del grafico a torta.  
  
2.  Fare clic con il pulsante destro del mouse sulle etichette e scegliere **Proprietà etichetta serie**. Verrà visualizzata la finestra di dialogo **Proprietà etichetta serie** .  
  
3.  Nella casella **Dati etichetta** digitare **#PERCENT{P0}** .  
  
     Il testo **{P0}** specifica la percentuale senza cifre decimali. Se si digita solo **#PERCENT**, i numeri avranno due cifre decimali. **#PERCENT** è una delle tante parole chiave disponibili che eseguono un calcolo o una funzione.  
     
4. Fare clic su **Sì** per confermare che si vuole impostare **UseValueAsLabel** su **False**.

5. Nella scheda **Carattere** selezionare **Grassetto** e modificare **Colore** in **Bianco**.

6. Fare clic su **OK**.     
  
 Per altre informazioni sulla personalizzazione di etichette e legende dei grafici, vedere [Visualizzare i valori in percentuale in un grafico a torta &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/display-percentage-values-on-a-pie-chart-report-builder-and-ssrs.md) e [Modificare il testo di un elemento legenda &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/chart-legend-change-item-text-report-builder.md).  
  
##  <a name="whats-next"></a><a name="WhatsNext"></a> Operazioni successive  
 Al termine della creazione del primo report in [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)], provare a eseguire le altre esercitazioni e iniziare a creare report basati su dati personalizzati. Per eseguire [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] è necessario avere l'autorizzazione di accesso alle origini dati, ad esempio ai database, con una *stringa di connessione* che stabilisce l'effettiva connessione all'origine dati. L'amministratore di sistema disporrà di queste informazioni e potrà procedere alla configurazione.  
  
 Per eseguire le altre esercitazioni, è necessario avere il nome di un'istanza di SQL Server e di credenziali sufficienti per l'accesso in sola lettura a qualsiasi database. L'amministratore di sistema potrà fornire i dati necessari.  
  
 Per salvare infine i report in un server di report o in un sito di SharePoint integrato con un server di report, è necessario disporre dell'URL e delle autorizzazioni appropriate. Tutti i report creati possono essere eseguiti direttamente dal computer, tuttavia quando vengono eseguiti dal server di report o dal sito di SharePoint i report offrono maggiori funzionalità. Per eseguire i propri report o quelli presenti sul server di report o nel sito di SharePoint in cui vengono pubblicati è necessario disporre delle autorizzazioni appropriate. Per ottenere l'accesso è necessario rivolgersi all'amministratore di sistema.  
  
 Prima di iniziare può essere utile leggere le informazioni su alcuni concetti e termini. Vedere [Concetti relativi a Reporting Services (SSRS)](../reporting-services-concepts-ssrs.md). È inoltre consigliabile dedicarsi alla pianificazione prima di creare il primo report. Questa fase preliminare risulterà infatti molto utile. Vedere [Pianificazione di un report &#40;Generatore report&#41;](../../reporting-services/report-design/planning-a-report-report-builder.md).  

## <a name="next-steps"></a>Passaggi successivi

[Esercitazioni di Generatore report](../../reporting-services/report-builder-tutorials.md)   
[Generatore report in SQL Server](../../reporting-services/report-builder/report-builder-in-sql-server-2016.md)  

Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
