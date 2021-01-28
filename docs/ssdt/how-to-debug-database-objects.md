---
title: Eseguire il debug di oggetti di database
description: Informazioni su come eseguire il debug di stored procedure, funzioni e trigger. Scoprire come attivare il debug, impostare i punti di interruzione ed eseguire un unit test di SQL Server in modalità debug.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: f5d4584f-e85f-4558-b056-83681c365978
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: b77373adb76921ee526aea70528399970bda9246
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597020"
---
# <a name="how-to--debug-database-objects"></a>Procedura:  Eseguire il debug di oggetti di database

Uno unit test di SQL Server è costituito dagli elementi seguenti:  
  
-   Codice dello unit test scritto in Visual C\# o Visual Basic. Questo codice, generato dalla finestra di progettazione unit test di SQL Server, è responsabile dell'invio dello script Transact\-SQL che forma il corpo del test.  
  
-   Una o più condizioni di test, scritte in Visual C\# o Visual Basic. Per eseguire il debug delle condizioni di test, attenersi alla procedura per eseguire il debug di uno unit test come descritto in [Procedura: eseguire il debug durante l'esecuzione di un test (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182484(v=vs.100)) o [Procedura: eseguire il debug durante l'esecuzione di un test (Visual Studio 2012)](/previous-versions/ms182484(v=vs.140)).  
  
-   Uno o più script Transact\-SQL eseguiti sugli oggetti nel database sottoposto a test. Non è possibile eseguire il debug di questi script Transact\-SQL.  
  
Nelle procedure descritte in questo argomento viene descritto come eseguire il debug di oggetti di database specifici, ad esempio stored procedure, funzioni e trigger, nel database sottoposto a test. Per eseguire il debug di un oggetto di database, attenersi alle procedure in questo ordine:  
  
1.  Abilitare il debug di SQL Server nel progetto di test.  
  
2.  Abilitare il debug dell'applicazione nell'istanza di SQL Server che ospita il database sottoposto a test.  
  
3.  Impostare i punti di interruzione nello script Transact\-SQL degli oggetti di database di cui viene eseguito il debug.  
  
4.  Eseguire il debug dello unit test. In questa procedura il test viene eseguito in modalità di debug.  
  
### <a name="to-enable-sql-debugging-on-your-test-project"></a>Per abilitare il debug di SQL Server nel progetto di test  
  
1.  Aprire **Esplora soluzioni**.  
  
2.  In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto di test e scegliere **Proprietà**.  
  
    Verrà visualizzata una pagina delle proprietà con lo stesso nome del progetto di test.  
  
3.  Nella pagina delle proprietà fare clic su **Debug**.  
  
4.  In **Attiva debugger** fare clic su **Attiva debug SQL Server**.  
  
5.  Salvare le modifiche.  
  
### <a name="to-set-an-increased-execution-context-timeout-to-enable-debugging-for-your-test-project"></a>Per impostare un timeout del contesto di esecuzione maggiore per abilitare il debug del progetto di test  
  
1.  Scegliere **Apri** dal menu **File**, quindi fare clic su **File**.  
  
2.  Passare alla cartella contenente il progetto di test e fare doppio clic sul file app.config.  
  
    Il file app.config verrà aperto nell'editor.  
  
3.  Modificare il nodo ExecutionContext per aggiungere un timeout del comando, come nel seguente esempio:  
  
    ```  
    <ExecutionContext CommandTimeout ="300" Provider="System.Data.SqlClient" ConnectionString="Data Source=TargetServerName\TargetInstanceName;Initial Catalog=TargetDatabaseName;Integrated Security=True;Pooling=False" />  
    ```  
  
4.  Salvare le modifiche.  
  
5.  Ricompilare il progetto di unit test.  
  
> [!IMPORTANT]  
> Se non si ricompila il progetto, le modifiche apportate al file app.config non verranno applicate quando si eseguono gli unit test e il debug avrà esito negativo.  
  
### <a name="to-add-breakpoints-to-your-transact-sql-script"></a>Per aggiungere punti di interruzione allo script Transact\-SQL  
  
1.  Aprire **Esplora oggetti di SQL Server** dal menu **Visualizza**.  
  
2.  In **Connessioni dati** espandere il nodo del database da testare.  
  
3.  Se accanto all'icona del database viene visualizzata una piccola "x" rossa, la connessione al database viene chiusa. In tal caso, fare clic con il pulsante destro del mouse sul database e scegliere **Aggiorna**. Potrebbe essere necessario specificare le credenziali per aprire la connessione al database.  
  
4.  Espandere il nodo **Viste**, **Stored procedure** o **Funzioni** per trovare l'oggetto di cui eseguire il debug.  
  
5.  Fare doppio clic sull'oggetto di cui si desidera eseguire il debug.  
  
6.  Per impostare un punto di interruzione, fare clic sulla barra laterale grigia.  
  
### <a name="to-debug-your-sql-server-unit-test"></a>Per eseguire il debug dello unit test di SQL Server  
  
1.  In Visual Studio 2010 aprire la finestra **Visualizzazione test** (Test -> Finestre). In Visual Studio 2012 aprire la finestra **Esplora test**.  
  
2.  Fare clic con il pulsante destro del mouse sul test tramite il cui script Transact\-SQL è possibile verificare il comportamento dell'oggetto di database in cui sono impostati i punti di interruzione e selezionare **Esegui debug selezione**.  
  
    Il test viene eseguito in modalità di debug finché non viene rilevato un punto di interruzione nell'oggetto di database.  
  
3.  (Facoltativo) Per aprire un'altra finestra di debug, scegliere **Finestre** dal menu **Debug**, quindi fare clic su **Punti di interruzione**, **Output** o **Controllo immediato**.  
  
## <a name="see-also"></a>Vedere anche  
[Esecuzione di unit test di SQL Server](../ssdt/running-sql-server-unit-tests.md)  
[Debug di Transact-SQL (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/zefbf0t6(v=vs.100))  
