---
description: Selezione e configurazione degli oggetti da testare (OracleToSQL)
title: Selezione e configurazione degli oggetti da testare (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Selection of Objects to Test,Parameter Comparison Settings
ms.assetid: 29fb6542-5c1f-4b14-a822-87700beb7623
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 2cf0aab82f56740dfe73476af89899fd9fd135f6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067613"
---
# <a name="selecting-and-configuring-objects-to-test-oracletosql"></a>Selezione e configurazione degli oggetti da testare (OracleToSQL)
In questo passaggio si selezionano gli oggetti da testare e si configurano le impostazioni per il confronto dei parametri di output delle procedure e delle funzioni, oltre ai valori restituiti dalle funzioni.  
  
## <a name="selection-of-objects-to-test"></a>Selezione degli oggetti da testare  
Nell'albero degli oggetti Oracle situato sul lato sinistro della finestra, controllare gli oggetti che si desidera richiamare durante il processo di test. Vedere l'elenco completo di oggetti [testabili nell'argomento testing migrated database objects &#40;OracleToSQL&#41;](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md) .  
  
Se SSMA tester non supporta alcuno degli oggetti selezionati per il test, si noterà che il collegamento con l'etichetta **alcuni oggetti selezionati contiene errori** nell'albero degli oggetti. Fare clic su questo collegamento per visualizzare i motivi per cui non è possibile testare questi oggetti e per cancellare la selezione di oggetti non corretti.  
  
Sul lato destro è possibile visualizzare diverse pagine in cui la pagina **SQL** Mostra la definizione dell'oggetto corrente. Nella pagina **parametri** sono elencati i parametri se l'oggetto è un stored procedure o una funzione. Nella pagina delle **Proprietà** vengono visualizzate le caratteristiche aggiuntive dell'oggetto. Vedere la descrizione delle pagine **confronti di parametri** e **valori di chiamata** di seguito.  
  
## <a name="parameter-comparison-settings"></a>Impostazioni di confronto parametri  
Definire le regole di confronto per i parametri di output e i valori restituiti nella pagina **confronti parametri** . È possibile apportare le impostazioni seguenti.  
  
### <a name="use-during-test-comparisons"></a>Usare durante i confronti di test  
Consente di abilitare l'utilizzo del parametro selezionato nel confronto dei risultati del test.  
  
-   Se si sceglie **true**, SSMA consentirà di confrontare il valore di output di questo parametro dopo l'esecuzione della procedura in Oracle con il valore corrispondente in SQL Server.
  
-   Se si sceglie **false**, il parametro verrà escluso dalla verifica dei risultati.  
  
### <a name="use-custom-scale"></a>Usare la scalabilità personalizzata  
Per i parametri di tipo di dati numeric, è possibile impostare una scala personalizzata per il confronto.  
  
-   Se si sceglie **true**, i valori numerici verranno arrotondati in base al valore della **scala di confronto** prima che vengano confrontati.  
  
-   Se si sceglie **false**, il confronto numerico sarà esatto.  
  
### <a name="comparing-scale"></a>Confronto tra scala  
Disponibile solo se l'opzione **Usa scala personalizzata** è impostata su **true**. Si tratta della precisione per il confronto numerico.  
  
### <a name="date-time-comparing"></a>Confronto data e ora  
Definisce il modo in cui vengono confrontati i valori di data/ora.  
  
-   Se si seleziona **Confronta data intera**, verrà eseguito il confronto completo dei valori di entrambe le piattaforme.  
  
-   Se si seleziona **Confronta solo data**, la parte relativa all'ora verrà ignorata.  
  
-   Se si seleziona **Confronta solo l'ora**, la parte della data verrà ignorata.  
  
-   Se si seleziona **Ignora millisecondi**, i risultati verranno confrontati fino a secondi.  
  
-   Se si seleziona **Ignora data e millisecondi**, il risultato verrà confrontato solo per parte dell'ora e ignorerà le parti frazionarie di un secondo.  
  
### <a name="ignore-strings-case"></a>Ignora case stringhe  
Controlla la distinzione tra maiuscole e minuscole del confronto.  
  
-   Se si sceglie **true**, il confronto non fa distinzione tra maiuscole e minuscole.  
  
-   Se si sceglie **false**, per il confronto verrà fatta distinzione tra maiuscole e minuscole.  
  
### <a name="ignore-trailing-spaces"></a>Ignora spazi finali  
Controlla il modo in cui gli spazi finali vengono considerati durante il confronto.  
  
-   Se si sceglie **true**, le stringhe confrontate verranno tagliate a destra prima del confronto.  
  
-   Se si sceglie **false**, le stringhe confrontate manterranno gli spazi vuoti finali.  
  
## <a name="specify-input-values-for-procedures-and-functions-call-values"></a>Specificare i valori di input per le procedure e le funzioni (valori di chiamata)  
È possibile specificare i valori dei parametri di input nella pagina **valori di chiamata** . Il pulsante **Aggiungi chiamata** aggiunge una nuova chiamata con valori di parametro vuoti. Il pulsante **Rimuovi chiamate** rimuove la chiamata corrente.  
  
## <a name="next-step"></a>passaggio successivo  
[Selezione e configurazione degli oggetti interessati &#40;OracleToSQL&#41;](../../ssma/oracle/selecting-and-configuring-affected-objects-oracletosql.md)  
  
## <a name="see-also"></a>Vedere anche  
[Test di oggetti di database migrati &#40;OracleToSQL&#41;](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md)  
  
