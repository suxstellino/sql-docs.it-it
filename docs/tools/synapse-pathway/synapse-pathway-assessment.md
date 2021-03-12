---
title: Valutazione dell'anteprima del percorso della sinapsi di Azure.
description: Eseguire una conversione del codice data warehouse con il percorso delle sinapsi di Azure
author: anshul82-ms
ms.author: anrampal
ms.prod: sql
ms.technology: tools-other
ms.topic: tutorial
ms.date: 03/02/2021
monikerRange: =azure-sqldw-latest
ms.custom: template-tutorial
ms.openlocfilehash: 3e5e8536c135244288d022879764d021c53e67ca
ms.sourcegitcommit: f10f0d604be1dce6c600a92aec4c095e7b52e19c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/11/2021
ms.locfileid: "102770501"
---
# <a name="tutorial-to-perform-your-first-code-translation-with-azure-synapse-pathway-preview"></a>Esercitazione per eseguire la prima traduzione del codice con l'anteprima del percorso sinapsi di Azure
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

Azure sinapsi pathway Preview introduce il supporto per la conversione di schemi, tabelle, viste, funzioni e così via da **Netezza**, **fiocco di neve** e **Microsoft SQL Server** nel codice di reclamo T-SQL che automatizza la migrazione ad Azure sinapsi Analytics.

Per altre informazioni, vedere [Panoramica dell'anteprima del percorso delle sinapsi di Azure](azure-synapse-pathway-overview.md).

In questa esercitazione verranno illustrate le procedure per:

> [!div class="checklist"]
> * Eseguire la prima traduzione degli script SQL in dal data warehouse esistente a script T-SQL per Azure Synpase SQL 
> * Scegliere una delle origini disponibili
> * Visualizzare gli errori e gli avvisi sugli oggetti che non sono stati tradotti

## <a name="prerequisites"></a>Prerequisiti

Per completare questa esercitazione, verificare di aver installato il [percorso sinapsi di Azure](synapse-pathway-download.md). Se è necessaria un'introduzione, vedere [Panoramica del percorso delle sinapsi di Azure](azure-synapse-pathway-overview.md).

## <a name="run-the-translation"></a>Eseguire la traduzione

1. Avviare l'MSI del percorso delle sinapsi di Azure. 

1. Selezionare una delle origini disponibili, quelle che verranno aggiunte a breve sono visualizzate in grigio.
1. Nella cartella directory di input selezionare Sfoglia e puntare lo strumento al percorso della cartella degli script **DDL** e **DML** che devono essere tradotti.

    > [!Note]
    > Solo i file con estensione SQL possono essere forniti come origine di input. Se l'utente fornisce DDL, script DML nel file con estensione txt, lo strumento non effettuerà alcuna conversione.

1. Quando si converte il codice Netezza in Azure sinapsi Analytics, scegliere IBM Netezza nell'elenco a discesa tipo di traduzione.
  ![Input di valutazione della sinapsi di Azure.](./media/synapse-pathway-assessment/assessment-input.png)

1. Per selezionare la directory di output, selezionare Sfoglia per specificare il percorso in cui verrà generato l'output.
 ![Directory di output della sinapsi di Azure.](./media/synapse-pathway-assessment/output-directory.png)

1. Selezionare **translate (Converti** ) per avviare la traduzione

## <a name="view-results"></a>Visualizzare i risultati

1. La durata della valutazione dipende dal numero di database aggiunti e dalle dimensioni dello schema di ogni database. I risultati vengono visualizzati per ogni database non appena sono disponibili.
 ![Report di valutazione della sinapsi di Azure.](./media/synapse-pathway-assessment/assessment-report-rendering.png)

1. Quando si seleziona Visualizza risultati, verrà visualizzata la directory di output specificata nel passaggio precedente e i file script tradotti verranno visualizzati in base alla struttura della directory di input.

1. Include la struttura del progetto di cui è possibile eseguire facilmente il commit nel repository GitHub.
  
1. Un file di risultati, che avrà un elenco di errori e avvisi, verrà caricato nella stessa directory di output.

## <a name="run-the-translation-using-command-line"></a>Eseguire la traduzione usando la riga di comando
1. Durante l'installazione, AspCmd.exe sarà disponibile nel percorso C:\Programmi (x86) \Caching sinapsi (anteprima)
1. Avviare il prompt dei comandi e passare al percorso del file 
1. Digitare aspcmd.exe--Help per un elenco di comandi

  ![Comandi della guida della riga di comando di Azure sinapsi assessment.](./media/synapse-pathway-assessment/command-line-help.png)


4. È possibile iniziare a eseguire le traduzioni utilizzando la riga di comando

 ![Valutazione delle sinapsi di Azure tramite la riga di comando.](./media/synapse-pathway-assessment/command-line-assessment.png)

## <a name="next-steps"></a>Passaggi successivi

[Informazioni su come salvare e caricare la valutazione](tutorial-save-load-assessment.md)
