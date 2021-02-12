---
description: Selezione e configurazione degli oggetti interessati (SybaseToSQL)
title: Selezione e configurazione degli oggetti interessati (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Tester Component,Affected Objects
ms.assetid: a219df74-543a-4aec-aeeb-79f90ac3e2ee
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 129f33320e337847454f22c685eedc77931d1464
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077662"
---
# <a name="selecting-and-configuring-affected-objects-sybasetosql"></a>Selezione e configurazione degli oggetti interessati (SybaseToSQL)
In questa pagina è possibile selezionare le tabelle e le chiavi esterne, che devono essere confrontate quando SSMA verifica i risultati dell'esecuzione per gli oggetti scelti nel passaggio precedente. Inoltre, è possibile personalizzare i parametri di verifica.  
  
## <a name="selection-of-affected-objects"></a>Selezione degli oggetti interessati  
Nell'albero degli oggetti Sybase situato sul lato sinistro della finestra, controllare le tabelle e le chiavi esterne, che devono essere confrontate per essere identiche.  
  
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
  
## <a name="table-comparison-settings"></a>Impostazioni di confronto tabella  
Definire le regole di confronto per la pagina tabella in **confronto tabella** . È possibile apportare le impostazioni seguenti.  
  
### <a name="comparison-mode"></a>Modalità di confronto  
Definisce il contenuto della tabella in cui verrà eseguito il confronto.  
  
-   Se si seleziona **solo modifiche**, verrà eseguito il confronto completo delle righe della tabella.  
  
-   Se si seleziona **completo**, verrà eseguito il confronto solo per le righe modificate.  
  
## <a name="column-comparison-settings"></a>Impostazioni di confronto colonne  
Definire le regole di confronto per le colonne della tabella nella pagina **confronti** colonne. È possibile apportare le impostazioni seguenti.  
  
### <a name="use-during-test-comparisons"></a>Usare durante i confronti di test  
Determinare se questa colonna parteciperà alla verifica dei risultati del test.  
  
-   Se si sceglie **true**, SSMA consentirà di confrontare il contenuto di questa colonna dopo l'esecuzione del test in Sybase con il contenuto della colonna in SQL Server.
  
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
È possibile visualizzare le istruzioni SELECT generate da SSMA tester nella pagina **confronta SQL** . Il tester Confronta i set di risultati di queste istruzioni riga per riga. Ogni riga successiva di un set di risultati Sybase deve essere uguale alla riga successiva del set di risultati prodotto in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
È possibile modificare le istruzioni SELECT per fornire la verifica personalizzata. Per salvare le modifiche nelle istruzioni Sybase e in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , usare i pulsanti **applica** in origine e destinazione SQL, corrispondente.  
  
## <a name="next-step"></a>passaggio successivo  
[Personalizzazione dell'ordine delle chiamate &#40;SybaseToSQL&#41;](../../ssma/sybase/customizing-calls-order-sybasetosql.md)  
  
## <a name="see-also"></a>Vedere anche  
[Esecuzione di test case &#40;SybaseToSQL&#41;](../../ssma/sybase/running-test-cases-sybasetosql.md)  
[Test di oggetti di database migrati &#40;SybaseToSQL&#41;](../../ssma/sybase/testing-migrated-database-objects-sybasetosql.md)  
  
