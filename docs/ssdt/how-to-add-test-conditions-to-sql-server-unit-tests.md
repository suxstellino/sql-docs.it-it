---
title: Aggiungere condizioni di test a unit test di SQL Server
description: Informazioni su come aggiungere condizioni di test a un unit test di SQL Server. Scoprire come usare la finestra di progettazione unit test di SQL Server per questa attività.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 85ba2e56-a0b2-489c-aea2-fb135cce0cfc
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 9920430615d8391ef069078272ed8abbfcef6a00
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063596"
---
# <a name="how-to-add-test-conditions-to-sql-server-unit-tests"></a>Procedura: Aggiungere condizioni di test a unit test di SQL Server

È possibile aggiungere condizioni di test a uno unit test di SQL Server tramite la **finestra di progettazione unit test di SQL Server**. Quando si salva la classe di test, le condizioni di test vengono salvate automaticamente nel progetto di test come codice Visual C\# o Visual Basic nel file del codice sorgente contenente la classe di test. Dopo aver salvato una condizione di test, è possibile modificarla nella **finestra di progettazione unit test di SQL Server** o nel relativo file del codice sorgente.  
  
### <a name="to-add-test-conditions-to-a-sql-server-unit-test"></a>Per aggiungere condizioni di test a uno unit test di SQL Server  
  
1.  Aprire uno unit test di SQL Server nella **finestra di progettazione unit test di SQL Server**.  
  
    Il nome del test aperto viene visualizzato nella barra di navigazione nella parte superiore della finestra di progettazione unit test di SQL Server. Utilizzando la barra di navigazione è possibile selezionare i diversi metodi di test inclusi nella classe di test.  
  
2.  Nella barra di navigazione fare clic sul metodo di test a cui si vuole aggiungere le condizioni di test oppure fare clic su **Script comuni**.  
  
    > [!NOTE]  
    > Gli script comuni non appartengono a uno specifico unit test, ma precedono o seguono gli unit test nella classe di test. Per altre informazioni, vedere [Script in unit test di SQL Server](../ssdt/scripts-in-sql-server-unit-tests.md).  
  
3.  Nella barra di navigazione fare clic sullo script Transact\-SQL al quale aggiungere condizioni di test. È possibile aggiungere condizioni di test allo script di pre-test, di test o di post-test.  
  
    Lo script Transact\-SQL per il test in questione viene visualizzato nell'editor Transact\-SQL e le relative condizioni di test nel riquadro **Condizioni di test**.  
  
4.  Nell'elenco di selezione **Condizioni di test** fare clic su una condizione di test, quindi su **Aggiungi condizione di test** (+).  
  
    La condizione di test viene aggiunta al metodo di unit test.  
  
    > [!NOTE]  
    > È possibile riordinare le condizioni di test all'interno di un metodo di test facendo clic su una condizione di test, quindi sulle frecce SU e GIÙ nel riquadro **Condizioni di test**.  
  
5.  Selezionare la condizione di test appena aggiunta e visualizzare la finestra **Proprietà**.  
  
    Configurare la condizione di test nella finestra Proprietà. Ad esempio, è possibile modificare la proprietà **Ora di esecuzione** di una condizione di test Ora di esecuzione. Se si imposta questa proprietà, l'esito del test sarà negativo se lo script Transact\-SQL non viene eseguito entro il tempo specificato.  
  
## <a name="see-also"></a>Vedere anche  
[Creazione e definizione di unit test di SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Procedura: Creare uno unit test di SQL Server vuoto](../ssdt/how-to-create-an-empty-sql-server-unit-test.md)  
[Procedura: Creare unit test di SQL Server per funzioni, trigger e stored procedure](../ssdt/how-to-create-unit-tests-for-functions-triggers-stored-procedures.md)  
[Uso di condizioni di test in unit test di SQL Server](../ssdt/using-test-conditions-in-sql-server-unit-tests.md)  
[Script in unit test di SQL Server](../ssdt/scripts-in-sql-server-unit-tests.md)  
[Interpretazione dei risultati di unit test di SQL Server](../ssdt/interpreting-sql-server-unit-test-results.md)  
[Procedura: Eseguire unit test di SQL Server](../ssdt/how-to-run-sql-server-unit-tests.md)  
  
