---
title: Salvare e caricare valutazioni con l'anteprima del percorso sinapsi di Azure
description: Salvare e caricare data warehouse valutazioni con l'anteprima del percorso sinapsi di Azure
author: anshul82-ms
ms.author: anrampal
ms.prod: sql
ms.technology: Azure Synapse Pathway
ms.topic: tutorial
ms.date: 03/02/2021
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: c2893272c3bc2cebf21b85378565af8ba0383bc8
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839195"
---
# <a name="save-and-load-assessments-with-azure-synapse-pathway-preview"></a>Salvare e caricare valutazioni con l'anteprima del percorso sinapsi di Azure
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

Le istruzioni dettagliate seguenti illustrano come salvare e caricare una valutazione data warehouse da un file usando il percorso delle sinapsi di Azure.

In questa esercitazione verranno illustrate le procedure per:

> [!div class="checklist"]
> * Salva una valutazione in un file
> * Caricare la valutazione da un file

## <a name="prerequisites"></a>Prerequisiti

Per completare questa esercitazione, verificare di aver installato [Azure sinapsi pathway](synapse-pathway-download.md) vedere [Panoramica del percorso delle sinapsi di Azure](azure-synapse-pathway-overview.md) per altre informazioni sullo strumento.

## <a name="saving-an-assessment-to-a-file"></a>Salvataggio di una valutazione in un file
 
1. Dopo aver eseguito la traduzione, dovrebbe essere visualizzato il report che riepiloga la valutazione del percorso delle sinapsi di Azure per la conversione del codice ![ .](./media/save-load-assessment/report-overview.png)
3. Selezionare il pulsante **Salva valutazione** , specificare il nome del file e quindi fare clic su **Salva**.
![Valutazione del percorso della sinapsi di Azure.](./media/save-load-assessment/save-assessment.png)

4. Viene creato un file con estensione asmprj nella destinazione specificata

## <a name="loading-an-assessment-from-a-file"></a>Caricamento di una valutazione da un file

1. Per aprire la stessa valutazione, selezionare **valutazione del carico** e specificare il nome del file. asmprj di ![ Azure sinapsi pathway assessment.](./media/save-load-assessment/browse-location.png)

1. Le cartelle di origine, di input e di output verranno popolate in base alla valutazione selezionata.
![Valutazione del percorso della sinapsi di Azure.](./media/save-load-assessment/load-assessment.png)
1. Selezionare **translate** per rieseguire la conversione del codice

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Generazione di report](report-generation.md)
