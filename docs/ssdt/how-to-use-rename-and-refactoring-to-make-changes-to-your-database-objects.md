---
title: Ridenominazione e refactoring per apportare modifiche agli oggetti di database
description: Informazioni sui menu Rinomina e Refactoring nell'editor Transact-SQL. Scoprire come rinominare i tipi, modificare gli schemi degli oggetti, espandere i caratteri jolly e specificare i nomi completi.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.dbrefactoring.previewdialog
- sql.data.tools.editor.howto.refactoring
- sql.data.tools.dbrefactoring.renamedialog
- sql.data.tools.dbrefactoring.moveschemadialog
- sql.data.tools.dbrefactoring.renameserverdatabasedialog
ms.assetid: f35520e6-8e6e-47b1-87a3-22c0cf2cabdb
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 8fd846301467dbbcbf08aa6bf4a02e52fb467c4b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018021"
---
# <a name="how-to-use-rename-and-refactoring-to-make-changes-to-your-database-objects"></a>Procedura: Usare la ridenominazione e il refactoring per apportare modifiche agli oggetti di database

Il menu contestuale Refactoring nell'Editor Transact\-SQL consente di rinominare o spostare un oggetto in uno schema differente e di generare un'anteprima di tutte le aree interessate prima di eseguire il commit della modifica. È inoltre possibile usare il menu Refactoring per indicare il nome completo di tutti i riferimenti agli oggetti di database o per espandere alcuni caratteri jolly nelle istruzioni `SELECT` del progetto di database in uso.  
  
> [!NOTE]  
> Nelle procedure seguenti vengono usate entità create nelle procedure precedenti nelle sezioni [Sviluppo del database connesso](../ssdt/connected-database-development.md) e [Sviluppo di database offline orientato ai progetti](../ssdt/project-oriented-offline-database-development.md).  
  
### <a name="to-rename-a-type"></a>Per rinominare un tipo  
  
1.  Fare clic con il pulsante destro del mouse sulla tabella **Products** (Products.sql) in **Esplora soluzioni** e selezionare **Visualizza codice** per aprire lo script nell'Editor Transact\-SQL.  
  
2.  Fare clic con il pulsante destro del mouse su `[Products]` nello script, selezionare **Refactoring**, quindi **Rinomina**.  
  
3.  Nel campo **Nuovo nome** impostare il nome su **Product**. Lasciare l'opzione **Anteprima modifiche** selezionata e scegliere **OK**.  
  
4.  Nell schermata successiva sarà possibile visualizzare un'anteprima di un elenco di script che saranno interessati dall'operazione di ridenominazione. In particolare, verranno evidenziate tutte le parti che fanno riferimento a `Products`. Questa operazione è molto simile all'attività Trova tutti i riferimenti illustrata nella procedura precedente. Fare clic su un elemento nel riquadro superiore e visualizzare la modifica effettiva negli script (evidenziati in verde) nel riquadro inferiore.  
  
5.  Fare clic su **Applica**.  
  
6.  Per i file di script già aperti in Progettazione tabelle o nell'Editor Transact\-SQL, si noti che le posizioni in cui sono state eseguite modifiche sono evidenziate con una barra verde a sinistra nell'Editor Transact\-SQL.  
  
7.  Si noti l'aggiunta di **TradeDev.refactorlog** in **Esplora soluzioni**. Fare doppio clic per aprirlo. In esso è contenuta una rappresentazione XML di tutte le modifiche in questa sessione.  
  
8.  Premere F5 per compilare e distribuire il progetto nel database locale.  
  
9. Fare clic con il pulsante destro del mouse sul database **TradeDev** in **Locale** in **Esplora oggetti di SQL Server** e selezionare **Aggiorna**.  
  
10. Espandere **Tabelle**. Si noti che la tabella **Products** è stata rinominata.  
  
11. Fare clic con il pulsante destro del mouse su **Product** e selezionare **Visualizza dati**. Si noti che i dati esistenti non sono stati modificati nonostante l'operazione di ridenominazione.  
  
### <a name="to-expand-wildcards"></a>Per espandere i caratteri jolly  
  
1.  Espandere il nodo **Funzioni** in **Esplora soluzioni** e fare doppio clic su **GetProductsBySupplier.sql**.  
  
2.  Posizionare il cursore sull'asterisco in questa riga e fare clic con il pulsante destro del mouse. Selezionare **Refactoring** ed **Espandi caratteri jolly**.  
  
    ```  
    SELECT * from Product p  
    ```  
  
3.  Nella finestra di dialogo **Anteprima modifiche** fare clic su `SELECT * from Product p` nel riquadro superiore per evidenziare tale elemento.  
  
4.  Nel riquadro **Anteprima modifiche** sottostante si noti che al carattere `*` è stato aggiunto quanto riportato di seguito nello script.  
  
    ```  
    [Id], [Name], [ShelfLife], [SupplierId], [CustomerId]  
    ```  
  
5.  Fare clic sul pulsante **Applica**.  Si noti che la riga contenente le modifiche prodotte dall'operazione di espansione è di nuovo evidenziata con una barra verde a sinistra.  
  
### <a name="to-fully-qualify-database-object-names"></a>Per indicare i nomi completi di oggetti di database  
  
1.  Assicurarsi che **GetProductsBySupplier.sql** sia ancora aperto nell'Editor Transact\-SQL.  
  
2.  Posizionare il cursore su `Product` in questa riga e fare clic con il pulsante destro del mouse. Selezionare **Refactoring** e **Utilizza nomi completi**.  
  
    ```  
    SELECT [Id], [Name], [ShelfLife], [SupplierId], [CustomerId] from Product p  
    ```  
  
3.  Fare clic sul pulsante **Applica** nella finestra di dialogo **Anteprima modifiche**.  Si noti che tutti i riferimenti agli oggetti sono stati aggiornati per includere il nome dello schema dell'oggetto e, se l'oggetto dispone di un elemento padre, il nome dell'elemento padre.  
  
    ```  
    SELECT [p].[Id], [p].[Name], [p].[ShelfLife], [p].[SupplierId], [p].[CustomerId] from [dbo].[Product] p  
    ```  
  
### <a name="to-move-schema"></a>Per spostare lo schema  
  
1.  Fare clic con il pulsante destro del mouse sull'oggetto da spostare. Selezionare **Refactoring** e **Sposta schema**.  
  
2.  Nell'elenco **Nuovo schema** fare clic sul nome dello schema in cui spostare l'oggetto. Fare clic su OK.  
  
    Se è stata selezionata la casella di controllo **Anteprima modifiche**, verrà visualizzata la finestra di dialogo **Anteprima modifiche**. In caso contrario, il nome dell'oggetto viene aggiornato e l'oggetto viene spostato nel nuovo schema.  
  
