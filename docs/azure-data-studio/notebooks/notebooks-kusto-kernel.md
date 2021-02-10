---
title: Notebook con kernel Kusto in Azure Data Studio
description: Questa esercitazione illustra come è possibile creare ed eseguire un notebook Kusto.
ms.topic: how-to
ms.prod: azure-data-studio
ms.technology: azure-data-studio
author: markingmyname
ms.author: maghan
ms.reviewer: jukoesma
ms.custom: ''
ms.date: 09/22/2020
ms.openlocfilehash: 1362e6a5ccd6152a1cb2597076d25c2da4149fd0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048231"
---
# <a name="create-and-run-a-kusto-kql-notebook-preview"></a>Creare ed eseguire un notebook Kusto (KQL) (anteprima)

Questo articolo illustra come creare ed eseguire un [notebook Azure Data Studio ](./notebooks-guidance.md) usando l'estensione [Kusto (KQL)](../extensions/kusto-extension.md), connettendosi a un cluster di Esplora dati di Azure.

Con l'estensione Kusto (KQL), è possibile modificare l'opzione kernel in **Kusto**.

Questa funzionalità è attualmente in anteprima.

## <a name="prerequisites"></a>Prerequisiti

Se non si ha una sottoscrizione di Azure, creare un [account Azure gratuito](https://azure.microsoft.com/free/) prima di iniziare.

- [Un cluster Esplora dati di Azure con un database a cui è possibile connettersi](/azure/data-explorer/create-cluster-database-portal).
- [Azure Data Studio](../download-azure-data-studio.md).
- [Estensione Kusto (KQL) per Azure Data Studio](../extensions/kusto-extension.md).

## <a name="create-a-kusto-kql-notebook"></a>Creare un notebook Kusto (KQL)

Questa procedura illustra come creare un file di notebook in Azure Data Studio:

1. In Azure Data Studio, connettersi al cluster di Esplora dati di Azure.

2. Passare al riquadro **Connessioni** e nella finestra **Server** fare clic con il pulsante destro del mouse sul database Kusto e selezionare *Nuovo notebook*. È anche possibile passare a **File** > **Nuovo notebook**.

   :::image type="content" source="media/notebooks-kusto-kernel/kusto-new-notebook.png" alt-text="Apri notebook":::

3. Selezionare *Kusto* per il **Kernel**. Verificare che il menu **Collega a** sia impostato sul nome del cluster e sul database. Per questo articolo viene usato il cluster help.kusto.windows.net con i dati del database di esempio.

   :::image type="content" source="media/notebooks-kusto-kernel/set-kusto-kernel.png" alt-text="Imposta Kernel e Connetti a":::

È possibile salvare il notebook usando il comando **Salva** o **Salva con nome** del menu **File**.

Per aprire un notebook, è possibile usare il comando **Apri file** nel menu **File**, selezionare **Apri file** nella pagina di **benvenuto** o usare il comando **File: Apri** nel riquadro comandi.

## <a name="change-the-connection"></a>Modificare la connessione

Per modificare la connessione Kusto per un notebook:

1. Selezionare il menu **Collega a** dalla barra degli strumenti del notebook e quindi selezionare **Cambia connessione**.

   :::image type="content" source="media/notebooks-kusto-kernel/kusto-select-attach-to-change-connections.png" alt-text="modificare le connessioni":::

   > [!Note]
   > Verificare che sia inserito il valore del database. Per i notebook di Kusto è necessario specificare il database.

2. A questo punto è possibile selezionare un server di connessione recente oppure immettere i dettagli della nuova connessione per la connessione.

   :::image type="content" source="media/notebooks-kusto-kernel/kusto-change-connection-cluster.png" alt-text="Selezionare un cluster diverso":::

   > [!Note]
   > Specificare il nome del cluster senza `https://`.

## <a name="run-a-code-cell"></a>Eseguire una cella di codice

È possibile creare celle contenenti query KQL che è possibile eseguire sul posto selezionando il pulsante **Esegui cella** sulla sinistra della cella. I risultati verranno visualizzati nel notebook al termine dell'esecuzione della cella.

Ad esempio:

1. Aggiungere una nuova cella di codice selezionando il comando **+Codice** sulla barra degli strumenti.

   :::image type="content" source="media/notebooks-kusto-kernel/kusto-kernel-code.png" alt-text="Blocco di codice kernel Kusto":::

2. Copiare e incollare l'esempio seguente nella cella e selezionare **Esegui cella**. In questo esempio vengono eseguite query sui dati StormEvents per un tipo di evento specifico.

   ```kusto
    StormEvents
    | where EventType == "Waterspout"
   ```

   :::image type="content" source="media/notebooks-kusto-kernel/run-kusto-notebook-cell.png" alt-text="Esegui cella":::

## <a name="save-the-result-or-show-chart"></a>Salvare il risultato o visualizzare il grafico

Se si esegue uno script che restituisce un risultato, è possibile salvare il risultato in formati diversi usando la barra degli strumenti visualizzata sopra il risultato.

- Salva in formato CSV
- Salva in formato Excel
- Salva in formato JSON
- Salva in formato XML
- Mostra il grafico

```kusto
    StormEvents
    | limit 10
```

:::image type="content" source="media/notebooks-kusto-kernel/run-notebook-save-results.png" alt-text="Salva risultato":::

## <a name="known-issues"></a>Problemi noti

| Dettagli | Soluzione alternativa |
|---------|------------|
| [Il risultato della query mostra solo le intestazioni di colonna](https://github.com/microsoft/azuredatastudio/issues/12565). | N/D |

È possibile inviare una [richiesta di funzionalità](https://github.com/microsoft/azuredatastudio/issues/new?assignees=&labels=&template=feature_request.md&title=) per inviare commenti e suggerimenti al team del prodotto.  
È possibile registrare un [bug](https://github.com/microsoft/azuredatastudio/issues/new?assignees=&labels=&template=bug_report.md&title=) per fornire commenti e suggerimenti al team del prodotto.

## <a name="next-steps"></a>Passaggi successivi

Altre informazioni sui notebook:

- [Estensione Kusto (KQL) per Azure Data Studio](../extensions/kusto-extension.md)
- [Come usare i notebook in Azure Data Studio](./notebooks-guidance.md)
- [Creare ed eseguire un notebook Python](./notebooks-python-kernel.md)
- [Creare ed eseguire un notebook di SQL Server](./notebooks-sql-kernel.md)