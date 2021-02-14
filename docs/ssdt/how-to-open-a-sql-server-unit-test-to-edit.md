---
title: Aprire uno unit test di SQL Server per la modifica
description: Informazioni su come modificare un unit test di SQL Server in modo che sia possibile aggiungere funzionalità o personalizzare le condizioni. Vedere diversi modi per aprire il file del codice sorgente del test.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: c6af1b12-54cd-42f9-b2ef-7164f8078323
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 0cd06acbd8817c62e728ee054024f570682450fc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018171"
---
# <a name="how-to-open-a-sql-server-unit-test-to-edit"></a>Procedura: Aprire uno unit test di SQL Server per la modifica

Dopo aver creato uno unit test di SQL Server, è possibile usare la **finestra di progettazione unit test di SQL Server** per aggiungere condizioni di test e istruzioni Transact\-SQL. Per i test creati tramite la finestra di progettazione viene generato codice Visual C# o Visual Basic, che verrà eseguito quando si esegue il test.  
  
Se si è soddisfatti del test, è possibile eseguirlo senza modifiche. Se si desidera aggiungere maggiori funzionalità a questo unit test, è possibile modificarne il codice. Questo codice è contenuto in un file con estensione cs o vb nel progetto di test. Per altre informazioni, vedere [File di unit test di SQL Server](../ssdt/sql-server-unit-test-files.md). È inoltre possibile personalizzare i test creando nuove condizioni di test. Per altre informazioni, vedere [Procedura: Creare condizioni di test per la finestra di progettazione degli unit test del database (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/aa833409(v=vs.100)).  
  
> [!NOTE]  
> Se si elimina un metodo di test modificando il file con estensione cs o vb, il metodo di test viene comunque visualizzato nella **finestra di progettazione unit test di SQL Server**. Questa situazione si verifica perché il metodo InitializeComponent della classe di test contiene ancora le variabili membro per tale test. Sebbene il test sia visualizzato nella finestra di progettazione, non sarà possibile eseguirlo perché il codice non è più presente. Per rigenerare il metodo di test per questo test, modificare Transact\-SQL nell'editor, quindi salvare il file di test con estensione cs o vb o ricompilare il progetto di test.  
  
### <a name="to-open-the-source-code-file-of-a-sql-server-unit-test-from-solution-explorer"></a>Per aprire il file del codice sorgente di uno unit test di SQL Server da Esplora soluzioni  
  
-   In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul file del codice sorgente contenente lo unit test di SQL Server, quindi scegliere **Visualizza codice**.  
  
    Il metodo di test dello unit test viene visualizzato nella finestra di modifica principale di Visual Studio al momento dell'apertura del file.  
  
### <a name="to-open-the-source-code-file-of-a-sql-server-unit-test-from-the-test-view-window-visual-studio-2010"></a>Per aprire il file del codice sorgente di uno unit test di SQL Server nella finestra Visualizzazione test (Visual Studio 2010)  
  
1.  Eseguire uno unit test. Per altre informazioni, vedere la sezione "Eseguire unit test di SQL Server" in [Procedura dettagliata: Creazione ed esecuzione di uno unit test di SQL Server](../ssdt/walkthrough-creating-and-running-a-sql-server-unit-test.md).  
  
2.  Nella finestra Visualizzazione test fare clic con il pulsante destro del mouse sul test, quindi scegliere **Apri test**.  
  
    Il metodo di test dello unit test viene visualizzato nella finestra di modifica principale di Visual Studio al momento dell'apertura del file.  
  
### <a name="to-open-the-source-code-file-of-a-sql-server-unit-test-from-the-test-view-window-visual-studio-2012"></a>Per aprire il file del codice sorgente di uno unit test di SQL Server nella finestra Visualizzazione test (Visual Studio 2012)  
  
1.  Eseguire uno unit test. Per altre informazioni, vedere la sezione "Eseguire unit test di SQL Server" in [Procedura dettagliata: Creazione ed esecuzione di uno unit test di SQL Server](../ssdt/walkthrough-creating-and-running-a-sql-server-unit-test.md).  
  
2.  Nella finestra Esplora test fare clic sul nome del file del codice sorgente dello unit test.  
  
    Il metodo di test dello unit test viene visualizzato nella finestra di modifica principale di Visual Studio al momento dell'apertura del file.  
  
## <a name="see-also"></a>Vedere anche  
[Creazione e definizione di unit test di SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Verifica del codice di database tramite unit test di SQL Server](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  
