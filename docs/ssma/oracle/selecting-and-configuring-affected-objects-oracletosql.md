---
description: Selezione e configurazione degli oggetti interessati (OracleToSQL)
title: Selezione e configurazione degli oggetti interessati (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Columns Comparison Settings
- Selection of Affected Objects
ms.assetid: 545eeda2-9829-4187-a858-619a96b4b71d
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: b8256909a2ebc460da2dd2db826e809dd6dfb50c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067626"
---
# <a name="selecting-and-configuring-affected-objects-oracletosql"></a>Selezione e configurazione degli oggetti interessati (OracleToSQL)
In questa pagina è possibile selezionare le tabelle e le chiavi esterne, che devono essere confrontate quando SSMA verifica i risultati dell'esecuzione per gli oggetti scelti nel passaggio precedente. Inoltre, è possibile personalizzare i parametri di verifica.  
  
## <a name="selection-of-affected-objects"></a>Selezione degli oggetti interessati  
Nell'albero degli oggetti Oracle situato sul lato sinistro della finestra, controllare le tabelle e le chiavi esterne, che devono essere confrontate per essere identiche.  
  
Se SSMA tester non è in grado di verificare questi oggetti, viene visualizzato il collegamento con l'etichetta **alcuni oggetti selezionati che contengono errori** nell'albero degli oggetti. Fare clic su questo collegamento per visualizzare i motivi per cui non è possibile confrontare questi oggetti e per deselezionare la selezione di oggetti non corretti.  
  
## <a name="table"></a>Tabella  
La scheda tabella contiene la visualizzazione griglia della tabella selezionata. La griglia contiene le informazioni seguenti sulla tabella selezionata:  
  
-   Nome colonna  
  
-   Tipo di dati  
  
-   Precision  
  
-   Scalabilità  
  
-   Regola  
  
-   Predefinito  
  
-   Identità  
  
-   Nullable  
  
## <a name="sql"></a>Sql  
Scheda SQL contiene il SQL "Create Table" della tabella selezionata.  
  
## <a name="data"></a>Data  
Scheda dati consente di visualizzare i dati presenti nella tabella selezionata.  
  
## <a name="properties"></a>Proprietà  
Scheda proprietà consente di visualizzare le proprietà della tabella selezionata. Nella scheda proprietà sono presenti i campi seguenti:  
  
-   Data di creazione o dell'Ultima modifica  
  
-   Nome oggetto  
  
## <a name="columns-comparison-settings"></a>Impostazioni di confronto colonne  
Definire le regole di confronto per le colonne della tabella nella pagina di **confronto colonne** . È possibile apportare le impostazioni seguenti.  
  
### <a name="use-during-test-comparisons"></a>Usare durante i confronti di test  
Determinare se questa colonna parteciperà alla verifica dei risultati del test.  
  
-   Se si sceglie **true**, SSMA consentirà di confrontare il contenuto di questa colonna dopo l'esecuzione del test in Oracle con il contenuto della colonna in SQL Server. 
  
-   Se si sceglie **false**, la colonna verrà esclusa dalla verifica dei risultati.  
  
### <a name="use-custom-scale"></a>Usare la scalabilità personalizzata  
Per le colonne con tipo di dati numerico, è possibile impostare una scala personalizzata per il confronto.  
  
-   Se si sceglie **true**, i valori numerici verranno arrotondati in base al valore della **scala di confronto** prima che vengano confrontati.  
  
-   Se si sceglie **false**, il confronto numerico sarà esatto.  
  
### <a name="comparing-scale"></a>Confronto tra scala  
  
-   Disponibile solo se l'opzione **Usa scala personalizzata** è impostata su **true**. Si tratta della precisione per il confronto numerico.  
  
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
  
-   Se si sceglie **false**, il confronto viene considerato per la lettera maiuscola.  
  
## <a name="comparing-sql"></a>Confronto di SQL  
È possibile visualizzare le istruzioni SELECT generate da SSMA tester nella pagina **confronto SQL** . Il tester Confronta i set di risultati di queste istruzioni riga per riga. Ogni riga successiva di un set di risultati Oracle deve essere uguale alla riga successiva del set di risultati generato in SQL Server.
  
È possibile modificare le istruzioni SELECT per fornire la verifica personalizzata. Per salvare le modifiche in Oracle e nelle istruzioni SQL Server, usare i pulsanti **applica** in origine e destinazione SQL, corrispondente.  
  
## <a name="next-step"></a>passaggio successivo  
[Personalizzazione dell'ordine delle chiamate &#40;OracleToSQL&#41;](../../ssma/oracle/customizing-calls-order-oracletosql.md)  
  
## <a name="see-also"></a>Vedere anche  
[Completamento della preparazione del test case &#40;OracleToSQL&#41;](../../ssma/oracle/finishing-test-case-preparation-oracletosql.md)  
[Esecuzione di test case &#40;OracleToSQL&#41;](../../ssma/oracle/running-test-cases-oracletosql.md)  
[Test di oggetti di database migrati &#40;OracleToSQL&#41;](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md)  
  
