---
title: Uso di Ottimizzazione guidata motore di database
description: Informazioni sul modo in cui l'interfaccia utente grafica di Ottimizzazione guidata motore di database di SQL Server ottimizza i database, gestisce le sessioni di ottimizzazione e visualizza le indicazioni di ottimizzazione.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Database Engine [SQL Server], tutorials
ms.assetid: 3317d4f8-ed9e-4f2e-b5f1-a6bf3a9d6c8d
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 10d32bb3267e79f4f97ce7a4832c1a999751904c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353517"
---
# <a name="lesson-2-using-database-engine-tuning-advisor"></a>Lezione 2: Uso di Ottimizzazione guidata motore di database

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Lo strumento Ottimizzazione guidata motore di database consente di ottimizzare i database, gestire le sessioni di ottimizzazione e visualizzare le indicazioni di ottimizzazione. Gli utenti esperti di strutture di progettazione fisica possono utilizzare questo strumento per analisi esplorative di ottimizzazione dei database. Gli utenti inesperti di ottimizzazione di database possono utilizzare lo strumento per individuare la migliore configurazione delle strutture di progettazione fisica per i carichi di lavoro che desiderano ottimizzare. Questa lezione illustra le tecniche di base per gli amministratori di database che non conoscono l'interfaccia utente grafica dello strumento Ottimizzazione guidata motore di database e per gli amministratori di sistema con una conoscenza limitata delle strutture di progettazione fisica.  

## <a name="prerequisites"></a>Prerequisiti 

Per completare questa esercitazione, sono necessari SQL Server Management Studio, l'accesso a un server che esegue SQL Server e un database AdventureWorks.

- Installare [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md).
- Installare [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Scaricare il [database di esempio AdventureWorks2017](../../samples/adventureworks-install-configure.md).


Le istruzioni per il ripristino dei database in SSMS sono disponibili in [Ripristinare un database](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md).

  >[!NOTE]
  > Questa esercitazione è destinata agli utenti che hanno familiarità con l'uso di SQL Server Management Studio e con attività semplici di amministrazione del database. 
  
## <a name="tuning-a-workload"></a>Ottimizzazione di un carico di lavoro
Per individuare la migliore struttura fisica di database per l'esecuzione di query sulle tabelle e i database selezionati per l'ottimizzazione, è possibile utilizzare Ottimizzazione guidata motore di database.  

1.  Copiare un'istruzione [SELECT](../../t-sql/queries/select-examples-transact-sql.md) di esempio e incollare l'istruzione nell'editor di query di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Salvare il file con il nome **MyScript.sql** in una directory in cui sia possibile individuarlo facilmente. Di seguito è riportato un esempio che funziona con il database AdventureWorks2017.  

 ```sql
 Use [Adventureworks2017]; -- may need to modify database name to match database
 GO
 SELECT DISTINCT pp.LastName, pp.FirstName 
 FROM Person.Person pp JOIN HumanResources.Employee e
 ON e.BusinessEntityID = pp.BusinessEntityID WHERE pp.BusinessEntityID IN 
 (SELECT SalesPersonID 
 FROM Sales.SalesOrderHeader
 WHERE SalesOrderID IN 
 (SELECT SalesOrderID 
 FROM Sales.SalesOrderDetail
 WHERE ProductID IN 
 (SELECT ProductID 
 FROM Production.Product p 
 WHERE ProductNumber = 'BK-M68B-42')));
 GO
 ```

  ![Salvare la query SQL](media/dta-tutorials/dta-save-query.png)
  
2.  Avviare Ottimizzazione guidata motore di database. Selezionare **Ottimizzazione guidata di database** dal menu **Strumenti** in SQL Server Management Studio (SSMS).  Per altre informazioni, vedere [Avviare Ottimizzazione guidata motore di database](lesson-1-basic-navigation-in-database-engine-tuning-advisor.md#launch-database-tuning-advisor). Connettersi a SQL Server nella finestra di dialogo **Connetti a server**.  
  
3.  Nella scheda **Generale** del riquadro destro dell'interfaccia utente grafica di Ottimizzazione guidata motore di database digitare **MySession** in **Nome sessione**. 
  
4.  Selezionare **File** per il **carico di lavoro** e selezionare l'icona a forma di binocolo che **consente di cercare un file di carico di lavoro**. Individuare il file **MyScript.sql** salvato nel passaggio 1.  

   ![Trovare lo script salvato in precedenza](media/dta-tutorials/dta-script.png)
  
5.  Selezionare AdventureWorks2017 nell'elenco **Database per l'analisi del carico di lavoro**, selezionare AdventureWorks2017 nella griglia **Selezionare i database e le tabelle da ottimizzare** e selezionare **Salva log di ottimizzazione**. **Database per l'analisi del carico di lavoro** specifica il primo database al quale Ottimizzazione guidata motore di database si connette durante l'ottimizzazione di un carico di lavoro. Dopo l'inizio dell'ottimizzazione, Ottimizzazione guidata motore di database si connette ai database specificati dalle istruzioni `USE DATABASE` contenute nel carico di lavoro.  

  ![Opzioni di ottimizzazione guidata di database](media/dta-tutorials/dta-select-db.png)
  
6.  Selezionare la scheda **Opzioni di ottimizzazione** . In questa esercitazione non verranno impostate le opzioni di ottimizzazione, tuttavia è utile analizzare brevemente le opzioni di ottimizzazione predefinite. Premere F1 per visualizzare la Guida relativa a questa pagina a schede. Fare clic su **Opzioni avanzate** per visualizzare le opzioni di ottimizzazione aggiuntive. Fare clic su **?** nella finestra di dialogo **Opzioni di ottimizzazione avanzate** per ottenere informazioni sulle opzioni di ottimizzazione visualizzate. Fare clic su **Annulla** per chiudere la finestra di dialogo **Opzioni di ottimizzazione avanzate** lasciando selezionate le opzioni predefinite.  

  ![Opzioni di ottimizzazione di Ottimizzazione guidata motore di database](media/dta-tutorials/dta-tuning-options.png)
  
7.  Fare clic sul pulsante **Avvia analisi** sulla barra degli strumenti. Durante l'esecuzione dell'analisi del carico di lavoro da parte di Ottimizzazione guidata motore di database, è possibile monitorarne lo stato nella scheda **Stato** . Dopo aver completato l'ottimizzazione, verrà visualizzata la scheda **Indicazioni** .  
  
    Se viene visualizzato un errore relativo alla data e ora di arresto dell'ottimizzazione, controllare l'impostazione **Data e ora arresto** nella scheda principale **Opzioni di ottimizzazione** . Verificare che i valori **Data e ora arresto** siano successivi alla data e all'ora correnti e, se necessario, modificarli.  

  ![Avviare l'analisi di ottimizzazione guidata di database](media/dta-tutorials/dta-start-analysis.png)

  
8.  Dopo aver completato l'analisi, salvare le indicazioni come script [!INCLUDE[tsql](../../includes/tsql-md.md)] scegliendo **Salva indicazioni** dal menu **Azioni** . Nella finestra di dialogo **Salva con nome** trovare la directory in cui si vuole salvare lo script delle indicazioni e digitare il nome file **MyRecommendations**.  

  ![Salvare le raccomandazioni di ottimizzazione guidata di database](media/dta-tutorials/dta-save-recommendations.png)

## <a name="view-tuning-recommendations"></a>Visualizzazione delle indicazioni di ottimizzazione
  
1.  Nella scheda **Indicazioni** usare la barra di scorrimento disponibile nella parte inferiore della pagina a schede per visualizzare le colonne **Indicazioni relative agli indici** . Ogni riga rappresenta un oggetto di database (ovvero indici o viste indicizzate) che Ottimizzazione guidata [!INCLUDE[ssDE](../../includes/ssde-md.md)] consiglia di eliminare o creare. Scorrere fino alla colonna all'estrema destra e fare clic su **Definizione**. [!INCLUDE[ssDE](../../includes/ssde-md.md)] Ottimizzazione guidata visualizza una finestra **Anteprima script SQL** nella quale è possibile visualizzare lo script [!INCLUDE[tsql](../../includes/tsql-md.md)] che crea o elimina l'oggetto di database della riga. Fare clic su **Chiudi** per chiudere la finestra di anteprima.  
  
    In caso di difficoltà nell'individuazione di una **Definizione** contenente un collegamento, deselezionare la casella di controllo **Mostra oggetti esistenti** nella parte inferiore della pagina a schede. In questo modo verrà ridotto il numero di righe visualizzate. Quando viene deselezionata questa casella di controllo, in Ottimizzazione guidata [!INCLUDE[ssDE](../../includes/ssde-md.md)] vengono visualizzati solo gli oggetti per i quali è stata generata un'indicazione. Selezionare la casella di controllo **Mostra oggetti esistenti** per visualizzare tutti gli oggetti di database attualmente esistenti nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . Utilizzare la barra di scorrimento sul lato sinistro della pagina a schede per visualizzare tutti gli oggetti.

  ![Raccomandazione sull'indice di ottimizzazione guidata di database](media/dta-tutorials/dta-recommendation.png)  
  
2.  Fare clic con il pulsante destro del mouse sulla griglia nel riquadro **Indicazioni relative agli indici** . Il menu di scelta rapida consente di selezionare e deselezionare le indicazioni. Consente inoltre di modificare il carattere del testo utilizzato nella griglia.  
 
   ![Menu di selezione per la raccomandazione sull'indice](media/dta-tutorials/dta-index-recommendation-options.png)
  
3.  Scegliere **Salva indicazioni** dal menu **Azioni** per salvare tutte le indicazioni in uno script [!INCLUDE[tsql](../../includes/tsql-md.md)] . Denominare lo script **MySessionRecommendations.sql**.  
  
    Aprire lo script MySessionRecommendations.sql nell'editor di query di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] per visualizzarlo. Sebbene sia possibile applicare le indicazioni al database di esempio [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] eseguendo lo script nell'editor di query, ciò non è consigliabile. Chiudere lo script nell'editor di query senza eseguirlo.  
  
    In alternativa, è possibile applicare le indicazioni scegliendo **Applica indicazioni** dal menu **Azioni** di Ottimizzazione guidata [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Si consiglia tuttavia di non applicare le indicazioni in questa esercitazione.  
  
4.  Se nella scheda **Indicazioni** sono disponibili più indicazioni, deselezionare alcune righe relative agli oggetti di database nella griglia **Indicazioni relative agli indici** .  
  
5.  Scegliere **Valuta indicazioni** dal menu **Azioni**. [!INCLUDE[ssDE](../../includes/ssde-md.md)] Ottimizzazione guidata crea una nuova sessione di ottimizzazione nella quale è possibile valutare un subset delle indicazioni originali di MySession.  
  
6.  Digitare **EvaluateMySession** come nuovo **Nome sessione** e fare clic sul pulsante **Avvia analisi** sulla barra degli strumenti. È possibile ripetere i passaggi 2 e 3 per questa nuova sessione di ottimizzazione in modo da visualizzare le indicazioni.  
  
### <a name="summary"></a>Summary  
La valutazione di un subset di indicazioni può essere necessaria se le opzioni di ottimizzazione devono essere modificate dopo aver eseguito una sessione. Si supponga ad esempio di richiedere a Ottimizzazione guidata [!INCLUDE[ssDE](../../includes/ssde-md.md)] di considerare le viste indicizzate quando si specificano le opzioni di ottimizzazione per una sessione, ma che dopo la generazione dell'indicazione si decida di non utilizzare le viste indicizzate. In questo caso è possibile scegliere **Valuta indicazioni** dal menu **Azioni** per valutare nuovamente la sessione in Ottimizzazione guidata [!INCLUDE[ssDE](../../includes/ssde-md.md)] senza tenere in considerazione le viste indicizzate. Quando viene utilizzata l'opzione **Valuta indicazioni** , le indicazioni generate precedentemente vengono applicate in maniera ipotetica alla progettazione fisica corrente per poi arrivare alla progettazione fisica per la seconda sessione di ottimizzazione.  
  
Altre informazioni sui risultati delle ottimizzazioni sono disponibili nella scheda **Report** che verrà illustrata nell'attività successiva di questa lezione.  

## <a name="view-tuning-reports"></a>Visualizzazione dei report di ottimizzazione
Sebbene sia utile visualizzare gli script che è possibile usare per implementare i risultati dell'ottimizzazione, in Ottimizzazione guidata motore di database sono disponibili anche numerosi report utili. Tali report offrono informazioni sulle strutture di progettazione fisica esistenti nel database che si sta ottimizzando e sulle strutture consigliate. È possibile visualizzare i report di ottimizzazione facendo clic sulla scheda **Report** descritta nell'attività seguente.


1. Selezionare la scheda **Report** nell'ottimizzazione guidata di database. 
2. Nel riquadro **Riepilogo ottimizzazione** è possibile visualizzare le informazioni sulla sessione di ottimizzazione. Utilizzare la barra di scorrimento per visualizzare tutto il contenuto del riquadro. Si notino i valori di **Miglioramento percentuale previsto** e **Spazio utilizzato seguendo le indicazioni**. È possibile limitare lo spazio utilizzato per le indicazioni quando si impostano le opzioni di ottimizzazione. Nella scheda **Opzioni di ottimizzazione** selezionare **Opzioni avanzate**. Selezionare **Spazio massimo per le indicazioni** e specificare lo spazio massimo in megabyte che può essere usato da una configurazione consigliata. Usare il pulsante **Indietro** nel visualizzatore della Guida per tornare a questa esercitazione. 

    ![Riepilogo ottimizzazione di Ottimizzazione guidata motore di database](media/dta-tutorials/dta-tuning-summary.png)
  
3.  Nell'elenco **Selezionare il report** del riquadro **Report ottimizzazione** fare clic su **Report costo istruzioni** . Se è necessario più spazio per la visualizzazione del report, trascinare il bordo del riquadro **Monitoraggio sessione** verso sinistra. Ad ogni istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] eseguita su una tabella del database è associato un costo delle prestazioni. Tale costo può essere ridotto creando indici efficaci sulle colonne di una tabella alle quali si accede di frequente. In questo report viene illustrato il miglioramento percentuale previsto tra il costo originale dell'esecuzione di un'istruzione del carico di lavoro e il costo previsto se viene implementata l'indicazione di ottimizzazione. Si noti che la quantità di informazioni contenute nel report dipende dalla lunghezza e dalla complessità del carico di lavoro.  

    ![Report di ottimizzazione guidata di database - Costo delle istruzioni](media/dta-tutorials/dta-statement-cost.png)
  
4.  Fare clic con il pulsante destro del mouse sul riquadro **Report costo istruzioni** nell'area della griglia e quindi scegliere **Esporta in un file**. Salvare il report con il nome **MyReport**. L'estensione xml verrà aggiunta al nome del file automaticamente. Per visualizzare il contenuto del file MyReport.xml, è possibile aprire il report nell'editor XML preferito o in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
5.  Tornare alla scheda **Report** di Ottimizzazione guidata motore di database e fare nuovamente clic con il pulsante destro del mouse su **Report costo istruzioni** . Controllare le altre opzioni disponibili. Si noti che è possibile modificare il carattere del report visualizzato. Se la modifica del carattere avviene in questo punto, verranno modificate anche le altre pagine a schede.  
  
6.  Fare clic sugli altri report disponibili nell'elenco **Selezionare report** in modo da acquisire maggiore familiarità.  
  
### <a name="summary"></a>Summary  
A questo punto è stata esaminata la scheda **Report** dell'interfaccia utente grafica di Ottimizzazione guidata motore di database per la sessione di ottimizzazione di MySession. È possibile eseguire gli stessi passaggi per esplorare i report creati per la sessione di ottimizzazione di EvaluateMySession. Nel riquadro **Monitoraggio sessione** fare doppio clic su **EvaluateMySession** per iniziare.  


 ## <a name="next-lesson"></a>Lezione successiva  
[Lezione 3: Uso dell'utilità del prompt dei comandi di ottimizzazione guidata di database](../../tools/dta/lesson-3-using-the-dta-command-prompt-utility.md)  
   
  
