---
title: Creare un'estensione di tipo dashboard
description: Questa esercitazione illustra come creare un'estensione di tipo dashboard per aggiungere funzionalità personalizzate ad Azure Data Studio.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: alayu, maghan
ms.custom: ''
ms.date: 08/28/2020
ms.openlocfilehash: d933f9bdff4279b841522f8a0e09f66e01364630
ms.sourcegitcommit: fa63019cbde76dd981b0c5a97c8e4d57e8d5ca4e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2021
ms.locfileid: "99495756"
---
# <a name="create-an-azure-data-studio-dashboard-extension"></a>Creare un'estensione di tipo dashboard di Azure Data Studio

Questa esercitazione illustra come creare una nuova estensione di tipo dashboard di Azure Data Studio. L'estensione consente di aggiungere contributi al dashboard di connessione di Azure Data Studio, in modo che sia possibile estendere la funzionalità di Azure Data Studio in modo facilmente visibile per gli utenti.

In questo articolo viene spiegato come:

> [!div class="checklist"]
> - Installare il generatore di estensioni.
> - Creare l'estensione.
> - Aggiungere contributi al dashboard nell'estensione.
> - Testare l'estensione.
> - Creare il pacchetto dell'estensione.
> - Pubblicare l'estensione nel marketplace.

## <a name="prerequisites"></a>Prerequisiti

Azure Data Studio si basa sullo stesso framework di Visual Studio Code, quindi le estensioni per Azure Data Studio vengono compilate usando Visual Studio Code. Per iniziare, sono necessari i componenti seguenti:

- [Node.js](https://nodejs.org) installato e disponibile in `$PATH`. Node.js include [npm](https://www.npmjs.com/), lo strumento di gestione dei pacchetti di Node.js, che viene usato per installare il generatore di estensioni.
- [Visual Studio Code](https://code.visualstudio.com) per eseguire il debug dell'estensione.
- [Estensione di debug](https://marketplace.visualstudio.com/items?itemName=ms-mssql.sqlops-debug) di Azure Data Studio (facoltativo). L'estensione di debug consente di testare l'estensione senza dover creare il pacchetto e installarla in Azure Data Studio.
- Assicurarsi che `azuredatastudio` sia nel percorso. Per Windows, assicurarsi di scegliere l'opzione **Aggiungi a PATH** in setup.exe. Per Mac o Linux, eseguire il **comando install ' azuredatastudio ' nel percorso** dal riquadro comandi in Azure Data Studio.

## <a name="install-the-extension-generator"></a>Installare il generatore di estensioni

Per semplificare il processo di creazione delle estensioni, abbiamo creato un [generatore di estensioni](https://code.visualstudio.com/docs/extensions/yocode) con Yeoman. Per installarlo, eseguire il comando seguente dal prompt dei comandi:

```console
npm install -g yo generator-azuredatastudio
```

## <a name="create-your-dashboard-extension"></a>Creare l'estensione di tipo dashboard

### <a name="introduction-to-the-dashboard"></a>Introduzione al dashboard

Il dashboard di connessione di Azure Data Studio è un potente strumento che riepiloga le connessioni di un utente fornendo anche informazioni dettagliate.

Esistono due varianti del dashboard. Il *dashboard server* riepiloga l'intero server e il *dashboard database* riepiloga un singolo database. Per accedere, è possibile fare clic con il pulsante destro del mouse su un server o un database nel viewlet **Connections** (Connessioni) di Azure Data Studio e scegliere **Manage** (Gestisci).

:::image type="content" source="media/dashboard-extension/dashboard-summary.gif" alt-text="Screenshot che illustra l'introduzione ai dashboard.":::

Sono disponibili tre punti di aggiunta contributi fondamentali per l'estensione, che consentono di aggiungere funzionalità al dashboard:

1. **Scheda completa nel dashboard**: scheda separata nel dashboard per l'estensione. Può essere aggiunta a un dashboard server o database. Personalizzabile con widget, una barra degli strumenti e una sezione di spostamento.
2. **Azioni nella home page**: pulsanti di azione nella parte superiore della barra degli strumenti della connessione.
3. **Widget**: grafi eseguiti sull'istanza di SQL Server.

   :::image type="content" source="media/dashboard-extension/dashboard-contrib-points.png" alt-text="Screenshot che mostra i punti di aggiunta contributi.":::

### <a name="run-the-extension-generator"></a>Eseguire il generatore di estensioni

Per creare un'estensione:

1. Avviare il generatore di estensioni con il comando seguente:

   `yo azuredatastudio`

1. Scegliere **New Dashboard** (Nuovo dashboard) nell'elenco dei tipi di estensione.

1. Rispondere alle domande, come illustrato, per creare un'estensione che aggiunge una scheda al dashboard server.

   :::image type="content" source="media/dashboard-extension/dashboard-generator.png" alt-text="Screenshot che mostra il generatore di estensioni.":::

   Le domande sono numerose e di seguito vengono fornite altre informazioni sul significato di ognuna:

   :::image type="content" source="media/dashboard-extension/dashboard-flowchart.png" alt-text="Screenshot che mostra un diagramma di flusso del dashboard.":::

Il completamento dei passaggi precedenti consente di creare una nuova cartella. Aprire la cartella in Visual Studio Code ed è tutto pronto per creare un'estensione per un dashboard personalizzato.

### <a name="run-the-extension"></a>Eseguire l'estensione

Si vedrà ora cosa offre il modello di dashboard eseguendo l'estensione. Prima di eseguire l'estensione, assicurarsi che in Visual Studio Code sia installata l'**estensione di debug di Azure Data Studio**.

Selezionare **F5** in Visual Studio Code per avviare Azure Data Studio in modalità di debug con l'estensione in esecuzione. È quindi possibile vedere i contributi offerti dal modello predefinito per il dashboard.

Si vedrà ora come modificare questo dashboard predefinito.

### <a name="develop-the-dashboard"></a>Sviluppare il dashboard

Il file più importante per iniziare con lo sviluppo dell'estensione è `package.json`. Si tratta del file manifesto, in cui vengono registrati i contributi del dashboard. Si notino le sezioni `dashboard.tabs`, `dashboard.insights` e `dashboard.containers`.

Di seguito vengono descritte alcune modifiche da provare:

- Provare a usare i vari tipi di informazioni dettagliate, tra cui bar, horizontalBar e timeSeries.
- Scrivere query personalizzate da eseguire sulla connessione a SQL Server.
- Vedere questa [esercitazione di esempio sulle informazioni dettagliate](../tutorial-qds-sql-server.md) o [questa esercitazione](../tutorial-table-space-sql-server.md) per esercitazioni specifiche sulle informazioni dettagliate.

## <a name="package-your-extension"></a>Creare il pacchetto dell'estensione

Per condividere l'estensione con altri utenti, è necessario inserirla in un pacchetto costituito da un unico file. L'estensione può essere pubblicata nel marketplace delle estensioni di Azure Data Studio o condivisa all'interno del team o della community. A tale scopo, è necessario installare un altro pacchetto npm dalla riga di comando.

```console
npm install -g vsce
```

Modificare il file `README.md` nel modo desiderato. Passare quindi alla directory di base dell'estensione ed eseguire `vsce package`. Facoltativamente, è possibile collegare un repository con l'estensione o continuare senza. Per aggiungerne uno, aggiungere una riga simile al file `package.json`.

```json
"repository": {
    "type": "git",
    "url": "https://github.com/anjalia/my-test-extension.git"
}
```

Dopo aver aggiunto queste righe, viene creato un file `my-test-extension-0.0.1.vsix` e si è pronti per l'installazione in Azure Data Studio.

:::image type="content" source="media/dashboard-extension/install-vsix.png" alt-text="Screenshot che mostra l'installazione di VSIX.":::

## <a name="publish-your-extension-to-the-marketplace"></a>Pubblicare l'estensione nel marketplace

Il marketplace delle estensioni di Azure Data Studio è in costruzione. Il processo corrente prevede di ospitare l'estensione VSIX in una posizione qualsiasi, ad esempio, in una pagina di rilascio di GitHub. Inviare quindi una richiesta pull che aggiorna [questo file JSON](https://github.com/Microsoft/azuredatastudio/blob/release/extensions/extensionsGallery.json) con le informazioni sull'estensione.

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione sono state illustrate le procedure per:
> [!div class="checklist"]
> - Installare il generatore di estensioni.
> - Creare l'estensione.
> - Aggiungere contributi al dashboard nell'estensione.
> - Testare l'estensione.
> - Creare il pacchetto dell'estensione.
> - Pubblicare l'estensione nel marketplace.

Ci auguriamo che la lettura di questo articolo sia di ispirazione per creare un'estensione personalizzata per Azure Data Studio. Sono disponibili il supporto per dashboard di informazioni dettagliate (utili grafici eseguiti in SQL Server), una serie di API specifiche di SQL e un enorme set di punti di estensione ereditati da Visual Studio Code.

Se si ha un'idea, ma non si è certi di come iniziare, aprire un problema o inviare un tweet al team: [azuredatastudio](https://twitter.com/azuredatastudio).

Per altre informazioni, vedere la [Guida delle estensioni di Visual Studio Code](https://code.visualstudio.com/docs/extensions/overview) che documenta tutte le API e i modelli esistenti.

Per informazioni su come usare T-SQL in Azure Data Studio, completare l'esercitazione sull'editor T-SQL:

> [!div class="nextstepaction"]
> [Usare l'editor Transact-SQL per creare oggetti di database](../tutorial-sql-editor.md)
