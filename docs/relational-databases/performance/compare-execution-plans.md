---
title: Confrontare i piani di esecuzione | Microsoft Docs
description: Informazioni su come confrontare le analogie e le differenze tra i piani di esecuzione grafici effettivi usando la funzionalità di confronto tra i piani di SQL Server Management Studio.
ms.custom: ''
ms.date: 11/21/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- comparing execution plans
- compare execution plans
- plan comparison
- execution plans [SQL Server], comparing
- Query Store, comparing plans
ms.assetid: 9e583a18-5f4a-4054-bfe1-4b2a76630db6
author: pmasl
ms.author: pelopes
manager: amitban
ms.openlocfilehash: 03b6ec22a3eadf7281f0513997cfcc28d40eb1b6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344728"
---
# <a name="compare-execution-plans"></a>Confrontare i piani di esecuzione
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
Questo argomento illustra come confrontare le analogie e le differenze tra i piani di esecuzione grafici effettivi usando la funzionalità di confronto tra i piani di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Questa funzionalità è disponibile a partire da [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] v16.
  
> [!NOTE]
> I piani di esecuzione effettivi vengono generati dopo l'esecuzione di query [!INCLUDE[tsql](../../includes/tsql-md.md)] o batch. Un piano di esecuzione effettivo include quindi informazioni di runtime, ad esempio il numero di righe effettivo, le metriche relative all'utilizzo delle risorse e gli avvisi sul runtime, se disponibili. Per altre informazioni, vedere [Visualizzazione di un piano di esecuzione effettivo](../../relational-databases/performance/display-an-actual-execution-plan.md).
  
Per risolvere i problemi è possibile che i professionisti di database abbiano la necessità di confrontare i piani:
-   Individuare il motivo per cui una query o un batch ha rallentato improvvisamente.
-   Individuare l'impatto della riscrittura di una query.
-   Osservare come una modifica per il miglioramento delle prestazioni introdotta nella progettazione dello schema, come un nuovo indice, ha effettivamente modificato il piano di esecuzione.  
 
L'opzione di menu **Confronto tra i piani** consente di eseguire un confronto side-by-side tra due diversi piani di esecuzione per facilitare l'identificazione delle analogie e delle modifiche che sono alla base dei diversi comportamenti per tutte le ragioni elencate precedentemente. L'opzione consente di eseguire un confronto tra:
- Due file di piani di esecuzione salvati in precedenza (estensione *sqlplan*).
- Un piano di esecuzione attivo e un piano di esecuzione delle query salvato in precedenza.
- Due piani di query selezionati in [Query Store](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md).

> [!TIP]
> L'opzione Confronto tra i piani può essere usata con tutti i file con estensione *sqlplan*, anche da versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Poiché l'opzione abilita anche un confronto offline, non è necessario essere connessi a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 

Quando viene eseguito il confronto di due piani di esecuzione, le aree del piano che **eseguono essenzialmente le stesse operazioni** sono rappresentate con lo stesso colore e motivo. Se si fa clic su un'area di un colore in uno dei piani, l'altro piano verrà centrato sul nodo corrispondente nel piano. È comunque possibile confrontare operatori e nodi senza corrispondenza dei piani di esecuzione, ma in questo caso è necessario selezionare manualmente gli operatori da confrontare.

> [!IMPORTANT]
> Per la ricerca delle analogie vengono usati solo i nodi che si prevede modifichino la forma del piano. Per questa ragione, può essere presente un nodo senza colore al centro tra due nodi che si trovano nella stessa sottosezione del piano. In questo caso la mancanza di colore implica che i nodi sono stati ignorati quando è stata verificata l'uguaglianza delle sezioni.
  
## <a name="to-compare-execution-plans"></a>Per confrontare i piani di esecuzione
  
1.  Aprire un file di piano di esecuzione query salvato in precedenza (file con estensione sqlplan) scegliendo **Apri file** dal menu **File** o trascinando un file di piano nella finestra [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)]. In alternativa, se è stata appena eseguita una query e si è scelto di visualizzare il piano di esecuzione, passare alla scheda **Piano di esecuzione** nel riquadro dei risultati. 

2.  Fare clic con il pulsante destro del mouse su un'area vuota del piano di esecuzione e fare clic su **Confronta showplan**. 

    ![Fare clic con il pulsante destro del mouse su Confronta showplan](../../relational-databases/performance/media/plancomparisonmenuoption.png "Fare clic con il pulsante destro del mouse su Confronta showplan")   

3.  Scegliere il secondo file di piano di query con cui si vuole eseguire il confronto. Viene aperto il secondo file per consentire il confronto dei piani.

4.  I piani confrontati vengono aperti in una nuova finestra, uno nella parte superiore e uno nella parte inferiore per impostazione predefinita. La selezione predefinita corrisponderà alla prima occorrenza di un operatore o un nodo comune nei piani confrontati ma con differenze tra i due piani. Tutti gli operatori e i nodi evidenziati sono presenti in entrambi i piani confrontati. Se si seleziona un operatore evidenziato nei piani nella parte superiore o sinistra, viene selezionato automaticamente l'operatore corrispondente nei piani nella parte inferiore o destra. Se si seleziona l'operatore del nodo radice in uno dei piani confrontati (il nodo SELECT nell'immagine seguente), viene selezionato anche l'operatore del nodo radice corrispondente nell'altro piano confrontato.

    ![Confronto dei piani di due file di piano salvati](../../relational-databases/performance/media/plancomparison-plans.png "Confronto dei piani di due file di piano salvati")  

     > [!TIP]
     > È possibile attivare o disattivare la visualizzazione del confronto tra i piani di esecuzione side-by-side facendo clic con il pulsante destro del mouse su un'area vuota del piano di esecuzione e selezionando **Attiva/Disattiva orientamento separatore**.

     > [!TIP]
     > Tutte le opzioni di zoom e navigazione disponibili per i piani di esecuzione possono essere usate nella modalità di confronto dei piani. Per altre informazioni, vedere [Visualizzazione di un piano di esecuzione effettivo](../../relational-databases/performance/display-an-actual-execution-plan.md).

5.  Sul lato destro viene aperta anche una finestra delle proprietà doppie, nell'ambito della selezione predefinita. Le proprietà che sono presenti in entrambi gli operatori confrontati ma presentano differenze verranno precedute dal segno *diverso da* (&ne;) per una più facile identificazione.

    ![Finestra delle proprietà doppie](../../relational-databases/performance/media/plancomparison-properties.png "Finestra delle proprietà doppie")  

6.  Nella parte inferiore viene aperta anche la finestra di navigazione di confronto **Analisi showplan**. Sono disponibili tre schede:

    1.  Nella scheda **Opzioni dell'istruzione** la selezione predefinita è *Evidenzia le operazioni simili* e gli stessi operatori o nodi evidenziati nei piani confrontati hanno lo stesso colore e motivo di linea. Per spostarsi tra aree simili nei piani confrontati, fare clic su un motivo di linea. È anche possibile scegliere di evidenziare le differenze nei piani anziché le analogie selezionando *Evidenzia gli operatori che non corrispondono a segmenti simili*. 
    
       > [!NOTE]
       > Per impostazione predefinita, i nomi di database vengono ignorati durante il confronto dei piani per consentire il confronto di piani acquisiti per i database con nomi diversi ma con lo stesso schema. Ad esempio, i nomi di database vengono ignorati quando vengono confrontati piani dei database *ProdDB* e *TestDB*. Questo comportamento può essere modificato con l'opzione *Ignora il nome del database per il confronto degli operatori*.

       ![Finestra Analisi showplan](../../relational-databases/performance/media/plancomparison-analysis.png "Finestra Analisi showplan") 

    2.  La scheda **Istruzione multipla** è utile durante il confronto di piani con più istruzioni poiché consente il confronto della coppia di istruzioni corretta.

        ![Istruzioni multiple nel piano confrontato](../../relational-databases/performance/media/plancomparison-multiple.png "Istruzioni multiple nel piano confrontato")  

    3.  Nella scheda **Scenari** è disponibile un'analisi automatizzata di alcuni degli aspetti più rilevanti da considerare relativi alle differenze della [stima della cardinalità](../../relational-databases/performance/cardinality-estimation-sql-server.md) nei piani confrontati. Per ogni operatore elencato nel riquadro sinistro, il riquadro destro mostra i dettagli sullo scenario tramite il collegamento *Fare clic qui per altre informazioni su questo scenario* e le possibili ragioni che spiegano lo scenario. 

        ![Righe stimate diverse](../../relational-databases/performance/media/plancomparison-scenarios.png "Righe stimate diverse")  

    Se questa finestra è chiusa, fare clic con il pulsante destro del mouse su un'area vuota di un piano confrontato e selezionare **Opzioni di confronto showplan** per aprirla nuovamente.

    ![Opzioni di confronto del piano](../../relational-databases/performance/media/plancomparison-options.png "Opzioni di confronto del piano")  

## <a name="to-compare-execution-plans-in-query-store"></a>Per confrontare i piani di esecuzione in Query Store

1.  In Query Store identificare una query con più di un piano di esecuzione. Per altre informazioni sugli scenari di Query Store, vedere [Scenari di utilizzo di Query Store](../../relational-databases/performance/query-store-usage-scenarios.md#identify-and-tune-top-resource-consuming-queries).

2.  Usare il tasto MAIUSC e il mouse per selezionare due piani per la stessa query. 

    ![Selezionare due piani in Query Store](../../relational-databases/performance/media/plancomparison-querystore.png "Selezionare due piani in Query Store")   

3.  Usare il pulsante **Confronta i piani per la query selezionata in una finestra separata** per avviare il confronto tra i piani. Sarà quindi possibile eseguire i passaggi da 4 a 6 della procedura *Per confrontare i piani di esecuzione*. 

    ![Confrontare showplan in Query Store](../../relational-databases/performance/media/plancomparison-querystoreoption.png "Confrontare showplan in Query Store") 
