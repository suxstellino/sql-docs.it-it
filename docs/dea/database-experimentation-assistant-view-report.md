---
title: Visualizzare i report di analisi per gli aggiornamenti di SQL Server
description: Informazioni su come visualizzare e comprendere i report di analisi per ottenere informazioni dettagliate sulle prestazioni in Database Experimentation Assistant (DEA).
ms.custom: seo-lt-2019
ms.date: 02/04/2020
ms.prod: sql
ms.prod_service: dea
ms.suite: sql
ms.technology: dea
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: pochiraju
ms.author: rajpo
ms.reviewer: mathoma
ms.openlocfilehash: d1666d4ba2279623065dd3c0d8faf5e0653de82e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065946"
---
# <a name="view-analysis-reports-in-database-experimentation-assistant"></a>Visualizzare i report di analisi in Database Experimentation Assistant

Dopo aver usato Database Experimentation Assistant (DEA) per [creare un report di analisi](database-experimentation-assistant-create-report.md), è possibile esaminare il report per ottenere informazioni dettagliate sulle prestazioni in base al test a/B eseguito.

## <a name="open-an-existing-analysis-report"></a>Aprire un report di analisi esistente

1. In DEA selezionare l'icona dell'elenco, specificare il nome del server e il tipo di autenticazione, selezionare o deselezionare le caselle di controllo **Crittografa connessione** e **attendibilità del server certificato** nel modo appropriato per lo scenario, quindi selezionare **Connetti**.

   ![Connetti al server con il report](./media/database-experimentation-assistant-view-report/dea-connect-to-server-with-report-files.png)

2. Nella schermata **report di analisi** , sul lato sinistro, selezionare la voce per il report che si desidera visualizzare.

   ![Apre un file di report esistente](./media/database-experimentation-assistant-view-report/dea-select-report-to-view.png)

## <a name="view-and-understand-the-analysis-report"></a>Visualizzare e comprendere il report di analisi

In questa sezione viene illustrato il report di analisi.

Nella prima pagina del report vengono visualizzate le informazioni sulla versione e le informazioni di compilazione per i server di destinazione in cui è stato eseguito l'esperimento. La soglia consente di regolare la sensibilità o la tolleranza dell'analisi di test A/B. Per impostazione predefinita, la soglia è impostata su 5%; qualsiasi miglioramento delle prestazioni >= 5% è classificato come "migliorato".  L'elenco a discesa consente di valutare il report con diverse soglie di prestazioni.

È possibile esportare i dati del report in un file CSV selezionando il pulsante **Esporta** .  In qualsiasi pagina del report di analisi è possibile selezionare **stampa** per stampare gli elementi visibili sullo schermo in quel momento.

### <a name="query-distribution"></a>Distribuzione di query

- Selezionare sezioni diverse dei grafici a torta per visualizzare solo le query che appartengono a tale categoria.

   ![Categorie di report come sezioni della torta](./media/database-experimentation-assistant-view-report/dea-view-report-pie-slices.png)

  - **Danneggiato**: query con prestazioni peggiori nella destinazione 2 rispetto alla destinazione 1.
  - **Errori**: query che hanno mostrato errori almeno una volta in almeno una delle destinazioni.
  - **Miglioramento**: query con prestazioni migliori sulla destinazione 2 rispetto alla destinazione 1.
  - **Non è possibile valutare**: le query con dimensioni di esempio troppo ridotte per l'analisi statistica. Per l'analisi di test A/B, DEA richiede che le stesse query dispongano di almeno 15 esecuzioni in ogni destinazione.
  - **Stesso**: query senza differenza statistica tra target 1 e target 2.

  Eventuali query di errore vengono visualizzate in grafici distinti; un grafico a barre che classifica gli errori per tipo e un grafico a torta classificazione degli errori in base all'ID errore.

   ![Grafici delle query di errore](./media/database-experimentation-assistant-view-report/dea-error-query-charts.png)

  Esistono quattro possibili tipi di errori:

  - **Errori esistenti**: errori presenti sia nella destinazione 1 che nella destinazione 2.
  - **Nuovi errori**: errori nuovi nella destinazione 2.
  - **Errori risolti**: errori presenti nella destinazione 1 ma risolti nella destinazione 2.
  - **Blocchi di aggiornamento**: errori che bloccano l'aggiornamento al server di destinazione.

  Facendo clic su qualsiasi barra o sezione a torta nei grafici si esegue il drill-down nella categoria e vengono visualizzate le metriche delle prestazioni, anche per la categoria **non è possibile valutare** .

  Il dashboard Mostra inoltre le prime cinque query migliorate e ridotte per offrire una panoramica delle prestazioni rapide.

### <a name="individual-query-drill-down"></a>Drill-down per query singole

È possibile selezionare i collegamenti al modello di query per informazioni più dettagliate su query specifiche.

![Eseguire il drill-down in una query specifica](./media/database-experimentation-assistant-view-report/dea-query-drill-down-report.png)

- Selezionare una query specifica per aprire il relativo riepilogo di confronto.

   ![Riepilogo del confronto](./media/database-experimentation-assistant-view-report/dea-view-report-comparison-summary.png)

   È possibile trovare statistiche di riepilogo per la query, ad esempio il numero di esecuzioni, la durata media, la CPU media, le letture/scritture e il numero di errori.  Se la query è una query di errore, nella scheda **informazioni sull'errore** vengono visualizzati maggiori dettagli sull'errore.  Nella scheda **informazioni sul piano di query** è possibile trovare informazioni sui piani di query usati per la query nella destinazione 1 e nella destinazione 2.

   > [!NOTE]
   > Se si sta analizzando l'evento esteso (. XEL), le informazioni sul piano di query non vengono raccolte per limitare le richieste di memoria nel computer dell'utente.

## <a name="see-also"></a>Vedi anche

- Per informazioni su come generare un report di analisi da un prompt dei comandi, vedere [Run al prompt dei comandi](database-experimentation-assistant-run-command-prompt.md).
