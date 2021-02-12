---
title: Creare un nuovo progetto di database
description: Scoprire come creare un nuovo progetto di database. Informazioni su come importare lo schema da un database esistente nel nuovo progetto.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.dbprojectwizard.importschema
- sql.data.tools.SqlProjectImportDatabaseDialog.dialog
- sql.data.tools.importscriptwizard.welcome
- sql.data.tools.importscriptwizard.summary
- sql.data.tools.SqlProjectImportDatabaseSummaryDialog.dialog
- sql.data.tools.importscriptwizard.fileselection
ms.assetid: 0b7883fa-b6e1-4ccf-b1d8-f522fd03a59d
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 6039138eda38f67aae2a1f4770ebc93380087607
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018371"
---
# <a name="how-to-create-a-new-database-project"></a>Procedura: Creare un nuovo progetto di database

È possibile creare un nuovo progetto di database e importare lo schema del database da un database esistente, un file di script con estensione sql o un'applicazione di livello dati (con estensione dacpac). È quindi possibile richiamare gli stessi strumenti visivi della finestra di progettazione (Editor Transact\-SQL, Progettazione tabelle) disponibili per lo sviluppo del database connesso per apportare modifiche al progetto di database offline e pubblicare di nuovo le modifiche nel database di produzione. Inoltre, è possibile salvare le modifiche come script da pubblicare in un secondo momento. Se si usa il riquadro **Proprietà progetto**, è possibile impostare la piattaforma di destinazione su versioni differenti di SQL Server (incluso SQL Azure).  
  
Nelle due procedure seguenti si ottiene sostanzialmente lo stesso risultato creando un nuovo progetto di database e importando lo schema da un database esistente. Ogni oggetto di database sarà rappresentato come file di script SQL (con estensione sql) in **Esplora soluzioni**. Per altre informazioni sull'importazione dello schema del database da uno snapshot, vedere [Procedura: Creare uno snapshot di un progetto](../ssdt/how-to-create-a-snapshot-of-a-project.md).  
  
> [!WARNING]  
> Nelle procedure seguenti vengono usate entità create nelle procedure precedenti nella sezione [Sviluppo del database connesso](../ssdt/connected-database-development.md).  
  
### <a name="to-create-a-new-database-project-off-a-connected-database"></a>Per creare un nuovo progetto di database da un database connesso  
  
1.  Fare clic con il pulsante destro del mouse sul nodo **TradeDev** in **Esplora oggetti di SQL Server** e selezionare **Crea nuovo progetto**.  
  
2.  Nella finestra di dialogo **Importa database** si noti che le impostazioni di **Connessione database di origine** sono state scelte in modo predefinito dal database selezionato in **Esplora oggetti di SQL Server**. Nell'impostazione **Progetto di destinazione** impostare il nome del progetto su **TradeDev**.  
  
3.  Nella sezione **Impostazioni di importazione** si notino le opzioni per importare impostazioni e oggetti specifici e per creare cartelle per ogni schema e/o tipo di oggetto. Per una gerarchia organizzata di tutti gli oggetti di database in uso, accettare tutte le impostazioni predefinite e fare clic su **Avvia**.  
  
4.  Nella finestra di dialogo **Importa database** vengono visualizzati un indicatore di stato e un elenco di oggetti in corso di importazione tramite SSDT. Al termine dell'operazione di importazione, fare clic su **Fine** per uscire dalla schermata finale.  
  
5.  Esaminare la gerarchia in **Esplora soluzioni**. Espandere la cartella **dbo** per individuare le cartelle **Funzioni**, **Tabelle** e **Viste**. Si noti che le tabelle e la funzione sono raggruppate nelle relative cartelle dello schema.  
  
6.  Fare doppio clic su **Products.sql** in **Tabelle**. Verrà aperto **Progettazione tabelle**, in cui sono visualizzate l'interpretazione visiva della tabella nella Griglia colonne e la definizione dello script della tabella nel riquadro di script. Tale rappresentazione è identica a quella mostrata nella sezione [Sviluppo del database connesso](../ssdt/connected-database-development.md).  
  
7.  Deselezionare la casella **Consenti valori Null** per la colonna **CustomerId**. Premere CTRL+S per salvare il file.  
  
8.  Fare clic con il pulsante destro del mouse sul progetto **TradeDev** in **Esplora soluzioni** e selezionare **Compila** per compilare il progetto di database.  
  
    I risultati dell'operazione di compilazione possono essere visualizzati nella Finestra di output  
  
### <a name="to-create-a-new-project-and-import-existing-database-schema"></a>Per creare un nuovo progetto e importare uno schema del database esistente  
  
1.  Fare clic su **File**, **Nuovo**, quindi su **Progetto**. Nella finestra di dialogo **Nuovo progetto** selezionare **SQL Server** nel riquadro sinistro. Si noti che è disponibile un unico tipo di progetto di database, ovvero **Progetto di database di SQL Server**. Non esiste alcun progetto specifico della piattaforma come nelle versioni precedenti di Visual Studio. Sarà possibile impostare la piattaforma di destinazione nella finestra di dialogo **Impostazioni progetto** al termine della creazione del progetto. Tale attività verrà illustrata nell'argomento [Procedura: Modificare la piattaforma di destinazione e pubblicare un progetto di database](../ssdt/how-to-change-target-platform-and-publish-a-database-project.md).  
  
2.  Impostare il nome del progetto su **TradeDev** e fare clic su **OK** per creare il nuovo progetto.  
  
3.  Fare clic con il pulsante destro del mouse sul progetto **TradeDev** appena creato in **Esplora soluzioni**, selezionare **Importa**, quindi **Database**.  
  
    Verrà visualizzata la finestra di dialogo **Importa database**. Nella sezione **Connessione database di origine** fare clic su **Scegli un database** e selezionare **TradeDev**. Se **TradeDev** non è indicato nell'elenco a discesa, usare il pulsante **Nuova connessione** per modificare le proprietà della connessione.  
  
4.  Nella sezione **Impostazioni di importazione** si notino le opzioni per importare impostazioni e oggetti specifici e per creare cartelle per ogni schema e/o tipo di oggetto. Per una gerarchia organizzata di tutti gli oggetti di database in uso, accettare tutte le impostazioni predefinite e fare clic su **Avvia**.  
  
5.  Nella finestra di dialogo **Importa database** vengono visualizzati un indicatore di stato e un elenco di oggetti in corso di importazione tramite SSDT. Al termine dell'operazione di importazione, fare clic su **Fine** per uscire dalla schermata finale.  
  
6.  Esaminare la gerarchia in **Esplora soluzioni**. Espandere la cartella **dbo** per individuare le cartelle **Funzioni**, **Tabelle** e **Viste**. Si noti che le tabelle e la funzione sono raggruppate nelle relative cartelle dello schema.  
  
7.  Fare doppio clic su **Products.sql** in **Tabelle**. Verrà aperto **Progettazione tabelle**, in cui sono visualizzate l'interpretazione visiva della tabella nella Griglia colonne e la definizione dello script della tabella nel riquadro di script. Tale rappresentazione è identica a quella mostrata nella sezione [Sviluppo del database connesso](../ssdt/connected-database-development.md).  
  
8.  Deselezionare la casella **Consenti valori Null** per la colonna **CustomerId**. Premere CTRL+S per salvare il file.  
  
9. Fare clic con il pulsante destro del mouse sul progetto **TradeDev** in **Esplora soluzioni** e selezionare **Compila** per compilare il progetto di database.  
  
## <a name="see-also"></a>Vedere anche  
[Procedura: Modificare la piattaforma di destinazione e pubblicare un progetto di database](../ssdt/how-to-change-target-platform-and-publish-a-database-project.md)  
  
