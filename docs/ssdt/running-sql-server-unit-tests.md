---
title: Esecuzione di unit test di SQL Server
description: Acquisire familiarità con gli unit test di SQL Server. Visualizzare risorse per la creazione di test, la creazione di condizioni di test personalizzate, l'esecuzione di test e l'interpretazione dei risultati.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.unittesting.testconfig
ms.assetid: febcc87f-eb18-4c12-ba30-82ef0d49aaa3
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 5983d00806f895d8112ac111312309cf93aa48ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063496"
---
# <a name="running-sql-server-unit-tests"></a>Esecuzione di unit test di SQL Server

Per migliorare e gestire la qualità del codice, è possibile creare ed eseguire unit test di SQL Server per verificare il comportamento di qualsiasi oggetto di database e archiviare quindi questi test nel controllo delle versioni. Quando l'utente o qualsiasi membro del team modifica lo schema del database, eseguire sia unit test di SQL Server sia unit test del software per verificare che le modifiche non abbiano interrotto le funzionalità esistenti. È possibile eseguire singoli test o gruppi di test, detti elenchi di test. Per altre informazioni, vedere [Utilizzo di elenchi di test (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182461(v=vs.100)).  
  
## <a name="ways-to-run-sql-server-unit-tests"></a>Modi per eseguire unit test di SQL Server  
È possibile eseguire gli unit test di SQL Server in diversi modi, che variano in base al software installato, come illustrato di seguito:  
  
-   Eseguire test tramite la finestra **Visualizzazione test** di Visual Studio 2010. Per altre informazioni, vedere [Procedura: Eseguire unit test di SQL Server](../ssdt/how-to-run-sql-server-unit-tests.md) e [Procedura: Eseguire test automatizzati da Microsoft Visual Studio 2010](/previous-versions/visualstudio/visual-studio-2010/ms182470(v=vs.100)). Per Visual Studio 2012, vedere [Procedura: Eseguire test automatizzati da Microsoft Visual Studio 2012](/previous-versions/ms182470(v=vs.140)).  
  
-   Eseguire test tramite il comando MSTest.exe al prompt dei comandi. Per altre informazioni, vedere [Procedura: Eseguire test automatizzati dalla riga di comando tramite MSTest (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182487(v=vs.100)) o [Procedura: Eseguire test automatizzati dalla riga di comando tramite MSTest (Visual Studio 2012)](/previous-versions/ms182487(v=vs.140)).  
  
-   Eseguire test da **Esplora soluzioni** tramite l'esecuzione di un progetto di test. Per altre informazioni, vedere [Procedura: Eseguire test automatizzati da Microsoft Visual Studio 2010](/previous-versions/visualstudio/visual-studio-2010/ms182470(v=vs.100)) o [Procedura: Eseguire test automatizzati da Microsoft Visual Studio 2012](/previous-versions/ms182470(v=vs.140)).  
  
-   Eseguire di nuovo i test dalla finestra **Risultati test**. Per altre informazioni, vedere [Procedura: Eseguire nuovamente un test (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182472(v=vs.100)).  
  
-   Eseguire singoli test o elenchi di test dalla finestra **Editor elenco dei test** (Visual Studio 2010). Per altre informazioni, vedere [Procedura: Eseguire test automatizzati da Microsoft Visual Studio 2010](/previous-versions/visualstudio/visual-studio-2010/ms182470(v=vs.100)) o [Procedura: Eseguire test automatizzati da Microsoft Visual Studio 2012](/previous-versions/ms182470(v=vs.140)).  
  
-   Eseguire test durante la compilazione di un progetto in Team Foundation Build. Per altre informazioni, vedere [Procedura: Configurare ed eseguire test pianificati dopo avere compilato l'applicazione (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182465(v=vs.100)) o [Procedura: Configurare ed eseguire test pianificati dopo avere compilato l'applicazione (Visual Studio 2012)](/previous-versions/visualstudio/visual-studio-2012/ms182465(v=vs.110)).  
  
È possibile eseguire unit test di SQL Server in un ordine specifico tramite un test ordinato. Per altre informazioni, vedere [Procedura: Creare un test ordinato (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182631(v=vs.100)) o [Procedura: Creare un test ordinato (Visual Studio 2012)](/previous-versions/ms182631(v=vs.140)).  
  
## <a name="interpreting-tests-results"></a>Interpretazione dei risultati dei test  
Dopo l'esecuzione dei test, nella finestra **Risultati test** vengono visualizzati i test con esito positivo o negativo. Per altre informazioni, vedere [Interpretazione dei risultati di unit test di SQL Server](../ssdt/interpreting-sql-server-unit-test-results.md). Per altre informazioni sulla diagnosi di un errore imprevisto, vedere [Procedura: Eseguire il debug di oggetti di database](../ssdt/how-to-debug-database-objects.md).  
  
## <a name="topics-in-this-section"></a>Argomenti in questa sezione  
Questa sezione contiene i seguenti argomenti:  
  
-   [Procedura: Eseguire il debug di oggetti di database](../ssdt/how-to-debug-database-objects.md)  
  
-   [Procedura: Eseguire unit test di SQL Server da Team Foundation Build](../ssdt/how-to-run-sql-server-unit-tests-from-team-foundation-build.md)  
  
-   [Procedura: Eseguire unit test di SQL Server](../ssdt/how-to-run-sql-server-unit-tests.md)  
  
-   [Interpretazione dei risultati di unit test di SQL Server](../ssdt/interpreting-sql-server-unit-test-results.md)  
  
## <a name="related-scenarios"></a>Scenari correlati  
[Creazione e definizione di unit test di SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
È possibile definire unit test per verificare il comportamento degli oggetti di database e per associare ogni progetto di test a un piano di generazione dati, una configurazione di distribuzione e una stringa di connessione diversi.  
  
[Condizioni di test personalizzate per unit test di SQL Server](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md)  
È possibile creare una condizione di test personalizzata per testare qualsiasi condizione non verificabile tramite le condizioni di test predefinite.  
  
## <a name="see-also"></a>Vedere anche  
[Verifica del codice di database tramite unit test di SQL Server](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  
