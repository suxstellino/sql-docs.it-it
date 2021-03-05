---
title: Generazione di report anteprima del percorso sinapsi di Azure
description: Il percorso delle sinapsi di Azure fornisce report completi sugli script tradotti.
author: anshul82-ms
ms.author: anrampal
ms.topic: tutorial
ms.prod: sql
ms.technology: tools-other
ms.date: 03/02/2021
monikerRange: =azure-sqldw-latest
ms.custom: template-tutorial
ms.openlocfilehash: 26701c33f713a1b82e3bab604a03e7dca054a36b
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186353"
---
# <a name="report-generation-for-azure-synapse-pathway-preview"></a>Generazione di report per l'anteprima del percorso sinapsi di Azure
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

Il percorso delle sinapsi di Azure fornisce un report completo sul numero di script tradotti correttamente. Il report Mostra anche il numero di errori e avvisi per le istruzioni che non sono state convertite.

## <a name="generate-report"></a>Genera report

Quando si seleziona **Traduci**, viene visualizzato un report simile al seguente:

![Report percorso sinapsi di Azure.](./media/report-generaration/report-overview.png)

## <a name="report-summary"></a>Riepilogo report

La conversione riuscita mostrerà la percentuale di script tradotti correttamente.

![Percorso delle sinapsi di Azure.](./media/report-generaration/conversion-success.png)

Nella sezione relativa alla distribuzione dell'istruzione viene dettagliato il numero di istruzioni DDL e DML che sono state convertite, incluso il numero di schemi, tabelle e database creati.

![Report1 di sinapsi di Azure.](./media/report-generaration/statement-distribution.png)

Il risparmio di migrazione viene calcolato in base al numero e alla complessità, semplice, alto o medio, degli oggetti tradotti. Il percorso sinapsi presuppone 30 minuti di lavoro manuale per ogni conversione di oggetti.

![Report2 di sinapsi di Azure.](./media/report-generaration/migration-savings.png)

## <a name="next-steps"></a>Passaggi successivi

[Scarica il percorso delle sinapsi di Azure](synapse-pathway-download.md)