---
description: Uso di repository test (OracleToSQL)
title: Uso di repository di test (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Test Cases Repository
- Test Results Repository
ms.assetid: f941cce4-d3e3-4aeb-a88a-4f101a97a9f4
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: f73dd1aabecee8625ace5cc4af0d342124306815
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063456"
---
# <a name="using-test-repositories-oracletosql"></a>Uso di repository test (OracleToSQL)
Il repository di test di SSMA archivia i test case e i risultati dei test di SSMA tester per un uso successivo. I dati del repository vengono salvati nelle tabelle SQL Server **TestCaseRepository** e **RunTestCaseResultRepository** nella **ssma_oracle_utilities** dello schema del database **ssmatesterdb** .  
  
Nella finestra di dialogo repository dei test case sono disponibili i pulsanti seguenti:  
  
-   Fare clic sul pulsante **Aggiorna** per aggiornare l'elenco di test case o risultati test.  
  
-   Fare clic sul pulsante **Chiudi** per chiudere la finestra di dialogo repository dei test case.  
  
## <a name="test-cases-repository"></a>Repository test case  
È possibile visualizzare il repository dei test case facendo clic su **test case...** dal menu **tester** . SSMA Visualizza quindi la finestra di dialogo **repository di test case** con un elenco di test case salvati nella pagina **test case** .  
  
Nella griglia vengono visualizzate le informazioni seguenti su ogni test case:  
  
-   Nome: nome del test case.  
  
-   Created: data di creazione test case.  
  
-   Modificato: la data dell'Ultima modifica del test case.  
  
-   Descrizione: le descrizioni del test case.  
  
Nella pagina test case sono disponibili i pulsanti seguenti:  
  
-   Fare clic sul pulsante **Aggiungi** per eseguire la procedura guidata test case e creare un nuovo test.  
  
-   Fare clic sul pulsante **Rimuovi** per eliminare il test selezionato dal repository. Quando un test case viene eliminato, vengono eliminati anche tutti i Risultati test correlati.  
  
-   Fare clic sul pulsante **modifica** per eseguire la procedura guidata test case e modificare il test selezionato.  
  
-   Fare clic sul pulsante **Run (Esegui** ) per aprire la finestra di dialogo [esecuzione di test case (OracleToSQL)](./running-test-cases-oracletosql.md) ed eseguire il test selezionato.  
  
## <a name="test-results-repository"></a>Repository Risultati test  
È possibile visualizzare il repository Risultati test nella pagina **risultati test** della finestra **repository della finestra test case** . Per aprirlo, fare clic su **risultati test...** dal menu **tester** .  
  
È possibile usare due filtri nella pagina **risultati test** :  
  
-   Filtro del nome del test case: consente di scegliere i risultati dei test per nome test case. Il valore di **tutti i test case** di questo filtro consente la visualizzazione dei risultati dei test per tutti i test case.  
  
-   Filtro data esecuzione test case: filtra i risultati dei test in base alla data di salvataggio. Il valore di **tutto il periodo** del filtro consente di visualizzare i risultati dei test per qualsiasi data di salvataggio.  
  
Nella griglia vengono visualizzate le informazioni seguenti relative ai risultati dei test.  
  
-   Nome: nome del test case.  
  
-   Saved: data del test case del salvataggio.  
  
-   Risultati: un breve riepilogo dell'esecuzione dei test (la descrizione comando di questa cella Visualizza un riepilogo completo dell'esecuzione dei test).  
  
Nella pagina dei risultati del test sono disponibili i pulsanti seguenti:  
  
-   Fare clic sul pulsante **Visualizza** per aprire la [visualizzazione dei report del test case &#40;OracleToSQL&#41;](../../ssma/oracle/viewing-test-case-reports-oracletosql.md) del risultato del test case corrente.  
  
-   Fare clic sul pulsante **Elimina** per eliminare il risultato del test selezionato  
  
## <a name="see-also"></a>Vedere anche  
[Esecuzione di test case &#40;OracleToSQL&#41;](../../ssma/oracle/running-test-cases-oracletosql.md)  
[Test di oggetti di database migrati &#40;OracleToSQL&#41;](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md)  
