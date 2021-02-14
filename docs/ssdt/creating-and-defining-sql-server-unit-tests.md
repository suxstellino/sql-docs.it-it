---
title: Creazione e definizione di unit test di SQL Server
description: Informazioni sugli unit test di SQL Server. Visualizzare fonti di informazioni su come creare ed eseguire unit test, risolvere i problemi ed eseguire altre attività correlate.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 3c082177-a2b1-4fde-8833-b49b2a351815
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: a6aab4b3ce10b59633d13ccadf3fb8ead67e375a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079612"
---
# <a name="creating-and-defining-sql-server-unit-tests"></a>Creazione e definizione di unit test di SQL Server

È possibile eseguire unit test di SQL Server per verificare se le modifiche apportate a uno o più oggetti di database in uno schema abbiano interferito con le funzionalità esistenti in un'applicazione di database. Questi test costituiscono un'integrazione degli unit test creati dagli sviluppatori del software. È necessario eseguire entrambi i tipi di test per verificare il comportamento dell'applicazione.  
  
È possibile verificare il comportamento di qualsiasi oggetto nello schema aggiungendo uno unit test di SQL Server e uno script Transact\-SQL per testare l'oggetto. In alternativa, si può generare automaticamente uno stub di uno script Transact\-SQL se si vuole verificare il comportamento di una funzione, di un trigger o di una stored procedure particolare. Dopo aver generato lo stub, è necessario personalizzarlo per ottenere risultati significativi.  
  
> [!NOTE]  
> È possibile creare un test vuoto, aggiungervi il codice ed eseguirlo senza che un progetto di database di SQL Server sia aperto. Tuttavia, non si può generare automaticamente uno stub Transact\-SQL tramite cui testare una funzione, un trigger o una stored procedure senza aprire il progetto contenente l'oggetto che si vuole verificare.  
  
## <a name="common-tasks"></a>Attività comuni  
Nella tabella seguente sono incluse le descrizioni di attività comuni che supportano questo scenario e vengono forniti i collegamenti a ulteriori informazioni su come completare queste attività.  
  
|Attività comuni|Contenuto di supporto|  
|----------------|----------------------|  
|**Fare pratica**: è possibile seguire una procedura dettagliata introduttiva per acquisire familiarità con le modalità di creazione ed esecuzione di uno unit test di SQL Server semplice.|-   [Procedura dettagliata: Creazione ed esecuzione di uno unit test di SQL Server](../ssdt/walkthrough-creating-and-running-a-sql-server-unit-test.md)|  
|**Altre informazioni sugli unit test di SQL Server**: è possibile acquisire maggiori informazioni sui file e sugli script tramite cui viene costituito uno unit test di SQL Server. Inoltre, è possibile ottenere altre informazioni sull'uso delle condizioni di test e sulle asserzioni Transact\-SQL negli unit test.|-   [Script in unit test di SQL Server](../ssdt/scripts-in-sql-server-unit-tests.md)<br />-   [File di unit test di SQL Server](../ssdt/sql-server-unit-test-files.md)<br />-   [Uso di condizioni di test in unit test di SQL Server](../ssdt/using-test-conditions-in-sql-server-unit-tests.md)<br />-   [Uso di asserzioni Transact-SQL in unit test di SQL Server](../ssdt/using-transact-sql-assertions-in-sql-server-unit-tests.md)|  
|**Creare uno o più progetti di test**: è necessario creare unit test di SQL Server in un progetto di test. Se si crea uno unit test di SQL Server che usa Esplora oggetti di SQL Server prima di un progetto di prova, quest'ultimo viene creato automaticamente. È possibile creare più di un progetto di test se, ad esempio, si desidera utilizzare piani di generazione dati diversi o configurazioni di distribuzione differenti in vari set di test. Quando si crea il progetto di test, è possibile configurare le impostazioni relativa al test, ad esempio la stringa di connessione, le impostazioni di distribuzione e un piano di generazione dati da utilizzare per il progetto in questione.|-   [Procedura: Creare un progetto di test per l'esecuzione di unit test del database di SQL Server](../ssdt/how-to-create-a-test-project-for-sql-server-database-unit-testing.md)<br />-|  
|**Configurare la modalità di esecuzione dello unit test**: è possibile specificare la stringa di connessione al database in cui si eseguono i test, il piano di generazione dati e le impostazioni di distribuzione. Configurare queste impostazioni quando si aggiunge per la prima volta uno unit test di SQL Server al progetto, tuttavia esse possono essere modificate anche in un secondo momento.|-   [Procedura: Configurare l'esecuzione di unit test di SQL Server](../ssdt/how-to-configure-sql-server-unit-test-execution.md)<br />-   [Panoramica delle stringhe di connessione e delle autorizzazioni](../ssdt/overview-of-connection-strings-and-permissions.md)|  
|**Creare uno unit test di SQL Server**: è possibile creare automaticamente stub di codice Transact\-SQL di unit test SQL Server per verificare il comportamento di una funzione, di un trigger o di una stored procedure. È anche possibile creare uno unit test di SQL Server vuoto e aggiungere successivamente il codice Transact\-SQL per testare gli altri tipi di oggetti di database.|-   [Procedura: Creare unit test di SQL Server per funzioni, trigger e stored procedure](../ssdt/how-to-create-unit-tests-for-functions-triggers-stored-procedures.md)<br />-   [Procedura: Creare uno unit test di SQL Server vuoto](../ssdt/how-to-create-an-empty-sql-server-unit-test.md)|  
|**Scrivere il codice per uno unit test di SQL Server**: dopo aver creato uno unit test, modificare o scrivere il codice Transact\-SQL per testare un oggetto di database. Per ogni test, definire una o più condizioni di test tramite cui viene determinato l'esito positivo o negativo del test. Per scenari più complessi, è possibile modificare il codice di Visual Basic o Visual C\# nel progetto di database. Ad esempio, è possibile scrivere uno unit test in esecuzione nell'ambito di un'unica transazione.|-   [Procedura: Aprire uno unit test di SQL Server per la modifica](../ssdt/how-to-open-a-sql-server-unit-test-to-edit.md)<br />-   [Procedura: Aggiungere condizioni di test a unit test di SQL Server](../ssdt/how-to-add-test-conditions-to-sql-server-unit-tests.md)<br />-   [Procedura: Scrivere uno unit test di SQL Server in esecuzione nell'ambito di una singola transazione](../ssdt/how-to-write-sql-server-unit-test-that-runs-in-single-transaction-scope.md)<br />-   [Tasti di scelta rapida per la finestra di progettazione unit test di SQL Server](../ssdt/keyboard-shortcuts-for-sql-server-unit-test-designer.md)|  
|**Risolvere i problemi**: è possibile acquisire altre informazioni sulla risoluzione dei problemi comuni relativi a SQL Server.|-   [Risoluzione dei problemi relativi a unit test del database di SQL Server](../ssdt/troubleshooting-sql-server-database-unit-testing-issues.md)|  
  
## <a name="related-scenarios"></a>Scenari correlati  
[Esecuzione di unit test di SQL Server](../ssdt/running-sql-server-unit-tests.md)  
Dopo aver creato gli unit test di SQL Server, è possibile eseguirli nella finestra Visualizzazione test, cioè la finestra di progettazione unit test di SQL Server, o usando Team Foundation Build.  
  
[Scenario: Definire condizioni di test personalizzate per gli unit test del database](/previous-versions/visualstudio/visual-studio-2010/dd193282(v=vs.100))  
È possibile creare una condizione di test personalizzata per testare un comportamento non verificabile dalle condizioni di test predefinite.  
  
## <a name="see-also"></a>Vedere anche  
[Verifica del codice di database tramite unit test di SQL Server](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  
