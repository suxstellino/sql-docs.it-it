---
title: Creare uno unit test di SQL Server vuoto
description: Informazioni su come creare unit test di SQL Server. Scoprire come usare gli stessi script TestInitialize e TestCleanup usati da altri test e come usare script diversi.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.unittesting.createtest
ms.assetid: b6f3cd5a-3389-42d6-a93f-97b3ddf31b95
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 510d669d780a7f7b6f71f5a84eab87e9b70bf4b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018321"
---
# <a name="how-to-create-an-empty-sql-server-unit-test"></a>Procedura: Creare uno unit test di SQL Server vuoto

Includere unit test nel progetto di database per verificare che le modifiche apportate agli oggetti di database non interrompano la funzionalità esistente. Le procedure seguenti illustrano come creare unit test di SQL Server per qualsiasi oggetto di database. SQL Server Data Tools include alcune funzionalità di supporto aggiuntive per trigger, stored procedure e funzioni di database. Per altre informazioni, vedere [Procedura: Creare unit test di SQL Server per funzioni, trigger e stored procedure](../ssdt/how-to-create-unit-tests-for-functions-triggers-stored-procedures.md).  
  
Quando si crea uno unit test di SQL Server usando la prima procedura, viene creato automaticamente un progetto di test, se non ve ne sono di disponibili. Se sono già disponibili progetti di test, è possibile scegliere di aggiungere il nuovo test a uno di tali progetti oppure è possibile crearne uno nuovo. Per altre informazioni sui progetti di test, vedere [Procedura: Creare un progetto di test per l'esecuzione di unit test del database di SQL Server](../ssdt/how-to-create-a-test-project-for-sql-server-database-unit-testing.md).  
  
Per creare uno unit test di SQL Server, sono disponibili due opzioni:  
  
-   Creare un nuovo unit test di SQL Server all'interno di una nuova classe di test.  
  
    In tutti gli unit test di SQL Server all'interno di una determinata classe di test verranno usati gli stessi script TestInitialize e TestCleanup. Creare una nuova classe di test se si desidera che nello unit test vengano utilizzati script TestInitialize e TestCleanup diversi rispetto ad altri unit test. Per altre informazioni, vedere [Script in unit test di SQL Server](../ssdt/scripts-in-sql-server-unit-tests.md).  
  
-   Creare un nuovo unit test di SQL Server all'interno di una classe di test esistente.  
  
    Scegliere questa opzione se nello unit test verranno utilizzati gli stessi script TestInitialize e TestCleanup di altri unit test all'interno della classe.  
  
### <a name="to-create-a-sql-server-unit-test-inside-a-new-test-class"></a>Per creare uno unit test di SQL Server all'interno di una nuova classe di test  
  
1.  Scegliere **Nuovo test** dal menu **Test**.  
  
    Verrà visualizzata la finestra di dialogo **Aggiungi nuovo test**.  
  
2.  In **Modelli** fare clic su **Unit test di SQL Server**.  
  
3.  In **Nome test** immettere un nome da assegnare al test.  
  
4.  In **Aggiungi a progetto di test** selezionare un progetto di test esistente a cui aggiungere il test. Se non sono presenti progetti di test o si vuole crearne uno nuovo, selezionare **Crea nuovo progetto di test <language>** .  
  
5.  Fare clic su **OK**.  
  
    Se il progetto di test è nuovo, verrà visualizzata la finestra di dialogo **Nuovo progetto di test**. Assegnare un nome al progetto e fare clic su **OK**.  
  
    Se il progetto di test è nuovo o non è stato configurato, verrà visualizzata la finestra di dialogo **Configurazione test di SQL Server<ProjectName>** . In questa finestra di dialogo è possibile configurare le seguenti informazioni per il progetto di test:  
  
    -   Connessione al database utilizzata per l'esecuzione di test.  
  
    -   Connessione al database utilizzata per convalidare i risultati del test, distribuire un database e generare dati.  
  
    -   Distribuzione automatica del progetto di database e delle eventuali modifiche dello schema associate a una configurazione del progetto specificata, prima dell'esecuzione degli unit test.  
  
    Per altre informazioni, vedere [Procedura: Configurare l'esecuzione di unit test di SQL Server](../ssdt/how-to-configure-sql-server-unit-test-execution.md).  
  
6.  Specificare le informazioni di configurazione del progetto, quindi scegliere **OK**.  
  
    \- - oppure -  
  
    Fare clic su **Annulla** per creare lo unit test senza configurare il progetto di test.  
  
    Il test vuoto viene visualizzato nella **finestra di progettazione unit test di SQL Server**. A seconda del linguaggio specificato per la creazione del progetto di test, viene aggiunto un file del codice sorgente Visual Basic o Visual C\# al progetto di test. In questo file è contenuta la classe di unit test di SQL Server generata da SQL Server Data Tools per lo unit test creato. In questa classe di test possono essere contenuti uno o più unit test che è possibile aggiungere tramite la finestra di progettazione unit test di SQL Server oppure tramite codice come nuovi metodi di test nella classe di test.  
  
    È inoltre possibile aggiungere ulteriori test effettuando le operazioni seguenti:  
  
    -   Fare clic con il pulsante destro del mouse su un progetto di test in **Esplora soluzioni**, selezionare **Aggiungi**, **Nuovo test**, quindi **Unit test di SQL Server**.  
  
    -   In Esplora oggetti di SQL Server selezionare Crea unit test.  
  
    Quando si seleziona questo file in **Esplora soluzioni**, per impostazione predefinita viene visualizzato nella finestra di progettazione unit test di SQL Server. Per visualizzare il codice o per personalizzarlo in modo da aggiungere ulteriori funzionalità agli unit test, selezionare il file, fare clic con il pulsante destro del mouse e scegliere **Visualizza codice**.  
  
### <a name="to-create-a-sql-server-unit-test-inside-an-existing-test-class"></a>Per creare un nuovo unit test di SQL Server all'interno di una classe di test esistente  
  
1.  Aprire una classe di unit test di SQL Server esistente nella **finestra di progettazione unit test di SQL Server**. Per accedere alla **finestra di progettazione unit test di SQL Server**, fare doppio clic sul file del codice sorgente dello unit test in **Esplora soluzioni**.  
  
2.  Fare clic sul segno più ( **+** ) nella barra di navigazione per visualizzare la finestra di dialogo **Specificare un nome di unit test**.  
  
3.  Digitare un nome e scegliere **OK**.  
  
    Il nuovo unit test di SQL Server sarà disponibile nell'elenco a discesa della barra di navigazione e verrà inoltre aggiunto come nuovo metodo di test nella classe di test. Per visualizzare il metodo di test nel codice, selezionare il file di classe, fare clic con il pulsante destro del mouse e scegliere **Visualizza codice**. Il nome del file di classe di test corrente viene visualizzato nella scheda presente nella parte superiore della **finestra di progettazione unit test di SQL Server**.  
  
Dopo aver configurato il progetto di test e aver creato il database, procedere con i passaggi successivi:  
  
-   Aggiungere uno script di test Transact\-SQL.  
  
-   Definire le azioni di pre-test e post-test.  
  
-   Aggiungere condizioni di test o un'altra istruzione di asserzione per verificare i risultati dello script.  
  
> [!NOTE]  
> La condizione di test Senza risultati è la condizione predefinita aggiunta a ogni test. Questa condizione di test viene inclusa per indicare che la verifica del test non è stata implementata. Eliminare tale condizione dal test dopo l'aggiunta di altre condizioni di test. Per altre informazioni, vedere [Procedura: Aggiungere condizioni di test a unit test del database](/previous-versions/visualstudio/visual-studio-2010/aa833242(v=vs.100)).  
  
## <a name="see-also"></a>Vedere anche  
[Procedura: Eseguire unit test di SQL Server](../ssdt/how-to-run-sql-server-unit-tests.md)  
[Creazione e definizione di unit test di SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Creazione di unit test](/previous-versions/visualstudio/visual-studio-2008/ms182523(v=vs.90))  
