---
title: Interpretazione dei risultati di unit test di SQL Server
description: Informazioni su come usare la finestra Risultati test o i file con estensione trx per visualizzare i risultati di unit test di SQL Server. Scoprire come ottenere informazioni dettagliate sui risultati.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: fde3c95b-2f68-483d-a197-0f7161b72fa3
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 88fc08dcc00e64c4b107a99608b891a43f236d66
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017911"
---
# <a name="interpreting-sql-server-unit-test-results"></a>Interpretazione dei risultati di unit test di SQL Server

Quando si esegue uno unit test di SQL Server, i risultati del test vengono generati automaticamente, salvati su disco e riepilogati nella finestra **Risultati del test**. Non appena si avvia un'esecuzione di test, viene visualizzata la finestra **Risultati test** con lo stato dell'esecuzione di test. Nella visualizzazione sono inclusi i test in esecuzione e quelli completati.  
  
Per visualizzare informazioni dettagliate su un risultato di test, fare doppio clic su nella finestra **Risultati test** per visualizzare la pagina **Visualizza dettagli risultati test**. Per ulteriori informazioni, fare doppio clic su un risultato di test.  
  
Per altre informazioni su come modificare la visualizzazione della finestra **Risultati del test**, vedere [Procedura: Aggiungere o rimuovere colonne nelle finestre dei test (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182508(v=vs.100)) o [Procedura: Aggiungere o rimuovere colonne nelle finestre dei test (Visual Studio 2012)](/previous-versions/visualstudio/visual-studio-2012/ms182508(v=vs.110)).  
  
## <a name="storing-test-results"></a>Archiviazione di risultati di test  
I risultati di unit test vengono archiviati automaticamente sul disco rigido in file con estensione trx. Un file trx è un file XML contenente i dettagli di un'esecuzione di test. È possibile caricare i file trx dalle esecuzioni di test precedenti per verificare i risultati di tali esecuzioni di test o per rieseguire i test precedenti. Per altre informazioni, vedere [Procedura: Eseguire nuovamente un test (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182472(v=vs.100)).  
  
> [!NOTE]  
> Non è possibile eseguire unit test in modalità remota.  
  
Se il team sta usando un progetto team di Visual Studio Team Foundation Server per semplificare la gestione del lavoro, è anche possibile pubblicare i dati di test in un database di SQL Server noto come archivio operativo.  
  
Per altre informazioni su come salvare i risultati di test, riutilizzarli e condividerli con il team, vedere [Procedura: Salvare e aprire risultati dei test in Visual Studio 2010](/previous-versions/visualstudio/visual-studio-2010/ms404662(v=vs.100)) o [Procedura: Salvare e aprire risultati dei test in Visual Studio 2012](/previous-versions/ms404662(v=vs.140)).  
  
## <a name="see-also"></a>Vedere anche  
[Esecuzione di unit test di SQL Server](../ssdt/running-sql-server-unit-tests.md)  
