---
title: Aggiornare un database connesso con Power Buffer
description: Informazioni su come usare Power Buffer per aggiornare un database. Scoprire come verificare le modifiche prima di applicarle e come salvare le modifiche in uno script per una distribuzione successiva.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.commitpreview.dialog
ms.assetid: 4048b7f8-71a9-47ad-b812-3fc1e8066240
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 2ecad870723402e33026668055368dac5dfd02c2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018091"
---
# <a name="how-to-update-a-connected-database-with-power-buffer"></a>Procedura: Aggiornare un database connesso con Power Buffer

La tecnologia Power Buffer di SQL Server Data Tools consente di applicare facilmente le modifiche al database connesso archiviandole tutte nella sessione corrente. Tutti gli errori causati dalla modifica della finestra Power Buffer (nell'Editor Transact\-SQL o in Progettazione tabelle) vengono visualizzati immediatamente nel riquadro **Elenco errori**, che consente di seguire gli errori identificati per un'ulteriore risoluzione dei problemi. È possibile verificare le modifiche in sospeso finché non si è pronti ad applicarle al database. Durante il processo di aggiornamento, tramite SSDT viene creato automaticamente uno script ALTER in base alle modifiche e vengono visualizzati gli avvisi di tutti i potenziali problemi. È possibile applicare tutte le modifiche accumulate in tutte le finestre Power Buffer aperte allo stesso database o salvare lo script ALTER da distribuire in un secondo momento.  
  
SSDT consente inoltre di tenere conto di tutte le modifiche apportate allo schema del database esternamente a Visual Studio. Ad esempio, se si aggiunge una nuova tabella a un database esistente in SQL Server Management Studio, tale modifica verrà visualizzata immediatamente in Esplora oggetti di SQL Server in Visual Studio senza aggiornare manualmente la tabella. Questa funzionalità di rilevamento dello sfasamento assicura che venga visualizzata sempre la definizione dello schema più recente di un database in Esplora oggetti di SQL Server. Si noti che tutti gli oggetti di database aperti in Progettazione tabelle o nell'Editor Transact\-SQL per la modifica non verranno aggiornati per mostrare le modifiche esternamente a Visual Studio.  
  
Nelle procedure seguenti vengono usate entità create nelle procedure precedenti nella sezione [Sviluppo del database connesso](../ssdt/connected-database-development.md).  
  
### <a name="to-apply-the-changes-made-in-the-previous-procedures"></a>Per applicare le modifiche apportate nelle procedure precedenti  
  
1.  Fare clic sul pulsante verde **Aggiorna** sulla barra degli strumenti. Se si passa il mouse sul pulsante, viene visualizzata la descrizione comando "Aggiorna database". La barra degli strumenti si trova sopra la Griglia colonne di Progettazione tabelle.  
  
2.  Verrà visualizzata la finestra di dialogo **Anteprima aggiornamenti database** e verrà generato in background uno script di distribuzione basato sulle modifiche. Nella finestra di dialogo viene quindi visualizzato un riepilogo delle azioni che verranno intraprese tramite SSDT (ad esempio la creazione o l'eliminazione di entità di database), insieme ai potenziali problemi identificati. Questa situazione non è applicabile alla procedura, ma risulterà utile nel caso in cui nella definizione di database siano presenti errori che impediscono un'operazione di aggiornamento finché non vengono risolti.  
  
3.  Se non si vuole aggiornare il database in questo momento, fare clic sul pulsante **Annulla** per uscire dalla finestra di dialogo **Anteprima aggiornamenti database**.  
  
4.  Se le modifiche apportate sono corrette, fare clic sul pulsante **Aggiorna database** nella finestra di dialogo **Anteprima aggiornamenti database**. Lo script di distribuzione viene eseguito automaticamente e le modifiche accumulate vengono a questo punto applicate al database.  
  
5.  Se si vuole visualizzare lo script di distribuzione per eseguire la verifica o per apportare modifiche prima dell'aggiornamento, fare clic sul pulsante **Genera script** nella finestra di dialogo **Anteprima aggiornamenti database**. Lo script generato verrà aperto in una nuova finestra dell'Editor Transact\-SQL. È possibile fare clic sul pulsante **Esegui query** nella barra degli strumenti dell'Editor Transact\-SQL per eseguire questa query. La funzione di questo pulsante è simile a quella di **Aggiorna database** nel passaggio 4.  
  
    > [!WARNING]  
    > Se si apportano modifiche allo script di distribuzione e tale script viene eseguito, le modifiche non verranno visualizzate nelle entità di database aperte. Ad esempio, se si rinomina una colonna della tabella `Customers` nello script di distribuzione e quest'ultimo viene eseguito per aggiornare il database e se la tabella `Customers` viene aperta in Progettazione tabelle, il nome della colonna sarà ancora quello visualizzato al momento della selezione del pulsante **Aggiorna database**. È necessario chiudere manualmente Progettazione tabelle senza salvarla in locale come script. Quando si riapre la tabella da **Esplora oggetti di SQL Server**, si noterà che il database è stato effettivamente aggiornato con le modifiche apportate nello script di distribuzione.  
  
6.  Nel riquadro **Output** dell'Editor Transact\-SQL (o nel riquadro **Messaggio** se si esegue lo script di distribuzione autonomamente) si noti quanto riportato di seguito che indica il completamento dell'aggiornamento.  
  
**Creazione di [dbo].[Customers]...Creazione di [dbo].[Products]...Creazione di [dbo].[Suppliers]...Creazione di FK_Products_SupplierId...Creazione di FK_Products_CustomerId...Creazione di CK_Products_ShelfLife Aggiornamento della parte del database sottoposta a transazione completato. Verifica dei dati esistenti rispetto ai vincoli appena creati Aggiornamento completato.**  
  
7.  In **Esplora oggetti di SQL Server** si noti che le nuove tabelle vengono visualizzate nel nodo **Tabelle** del database **Trade**.  
  
### <a name="to-view-changes-made-to-a-database-outside-visual-studio"></a>Per visualizzare le modifiche apportate a un database esternamente a Visual Studio  
  
1.  Aprire SQL Server Management Studio. Nella finestra di dialogo **Connetti al server** immettere lo stesso server di database a cui è stata eseguita la connessione in Visual Studio e fare clic su **Connetti**.  
  
2.  In **Esplora oggetti di SQL Server** espandere **Database**, quindi spostarsi sul database **Trade**.  
  
3.  Fare clic con il pulsante destro del mouse su **Tabelle** in **Trade** e selezionare **Nuova tabella**. In Progettazione tabelle immettere **id** come Nome colonna e **int** come Tipo di dati.  
  
4.  Fare clic sull'icona **Salva** nella barra degli strumenti per salvare la tabella. Accettare il nome predefinito e fare clic su **OK**.  
  
    Tornare a Visual Studio. Esaminare il nodo **Tabelle** nel database **Trade** in **Esplora oggetti di SQL Server**. Si noti l'aspetto della tabella **Table_1** appena creata.  
  
5.  Fare clic con il pulsante destro del mouse su **Table_1** e selezionare **Elimina**. Nella finestra di dialogo **Anteprima aggiornamenti database** fare clic su **Aggiorna database**.  
  
## <a name="see-also"></a>Vedere anche  
[Procedura: Correggere gli errori](../ssdt/how-to-fix-errors.md)  
  
