---
title: Creare un'estensione di tipo procedura guidata
description: Questa esercitazione illustra come creare un'estensione di tipo procedura guidata per aggiungere funzionalità personalizzate ad Azure Data Studio.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan
ms.custom: ''
ms.date: 08/28/2020
ms.openlocfilehash: ef9938d87de3963ab7757e0d4ec3af0e01d6f33a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048361"
---
# <a name="create-an-azure-data-studio-wizard-extension"></a>Creare un'estensione di tipo procedura guidata di Azure Data Studio

Questa esercitazione illustra come creare una nuova **estensione di tipo procedura guidata di Azure Data Studio**. L'estensione rende disponibile una procedura guidata per interagire con gli utenti in Azure Data Studio.

In questo articolo viene spiegato come:
> [!div class="checklist"]
> - Installare il generatore di estensioni
> - Creare l'estensione
> - Aggiungere una procedura guidata personalizzata all'estensione
> - Testare l'estensione
> - Creare il pacchetto dell'estensione
> - Pubblicare l'estensione nel marketplace

## <a name="prerequisites"></a>Prerequisiti

Azure Data Studio si basa sullo stesso framework di Visual Studio Code, quindi le estensioni per Azure Data Studio vengono compilate usando Visual Studio Code. Per iniziare, sono necessari i componenti seguenti:

- [Node.js](https://nodejs.org) installato e disponibile in `$PATH`. Node.js include [npm](https://www.npmjs.com/), lo strumento di gestione dei pacchetti di Node.js, che viene usato per installare il generatore di estensioni.
- [Visual Studio Code](https://code.visualstudio.com) per eseguire il debug dell'estensione.
- [Estensione di debug](https://marketplace.visualstudio.com/items?itemName=ms-mssql.sqlops-debug) di Azure Data Studio (facoltativo). In questo modo è possibile testare l'estensione senza dover creare il pacchetto e installarla in Azure Data Studio.
- Assicurarsi che `azuredatastudio` sia nel percorso. Per Windows, assicurarsi di scegliere l'opzione `Add to Path` in setup.exe. Per Mac o Linux, eseguire il **comando install ' azuredatastudio ' nel percorso** dal riquadro comandi in Azure Data Studio.

## <a name="install-the-extension-generator"></a>Installare il generatore di estensioni

Per semplificare il processo di creazione delle estensioni, abbiamo creato un [generatore di estensioni](https://code.visualstudio.com/docs/extensions/yocode) con Yeoman. Per installarlo, eseguire il comando seguente dal prompt dei comandi:

```console
npm install -g yo generator-azuredatastudio
```

## <a name="create-your-wizard-extension"></a>Creare l'estensione di tipo procedura guidata

### <a name="introduction-to-wizards"></a>Introduzione alle procedure guidate

Le procedure guidate sono un tipo di interfaccia utente che presenta pagine dettagliate che devono essere compilate dagli utenti per eseguire un'attività. Esempi comuni sono le procedure guidate per l'installazione del software e per la risoluzione dei problemi. Ad esempio:

:::image type="content" source="media/wizard-extension/dacpac-wizard.gif" alt-text="Procedura guidata dacpac":::

Dato che le procedure guidate sono spesso utili per l'uso dei dati e l'estensione delle funzionalità di Azure Data Studio, Azure Data Studio offre API per la creazione di procedure guidate personalizzate. Verrà illustrata la procedura per generare un modello di procedura guidata e modificarlo per creare una procedura guidata personalizzata.

### <a name="run-the-extension-generator"></a>Eseguire il generatore di estensioni

Per creare un'estensione:

1. Avviare il generatore di estensioni con il comando seguente:

   `yo azuredatastudio`

2. Scegliere **New Wizard or Dialog** (Nuova procedura guidata o finestra di dialogo) nell'elenco dei tipi di estensione. Selezionare quindi **Wizard** (Procedura guidata) e poi **Getting Started Template** (Modello introduttivo)

3. Seguire i passaggi per compilare il nome dell'estensione (per questa esercitazione, usare **My Test Extension**) e aggiungere una descrizione.

    :::image type="content" source="media/wizard-extension/extension-generator.png" alt-text="Generatore di estensioni":::

Il completamento dei passaggi precedenti consente di creare una nuova cartella. Aprire la cartella in Visual Studio Code ed è tutto pronto per creare l'estensione per una procedura guidata personalizzata.

### <a name="run-the-extension"></a>Eseguire l'estensione

Si vedrà ora cosa offre il modello di procedura guidata eseguendo l'estensione. Prima di eseguire l'estensione, assicurarsi che in Visual Studio Code sia installata l'**estensione di debug di Azure Data Studio**.

Selezionare **F5** in VS Code per avviare Azure Data Studio in modalità di debug con l'estensione in esecuzione. In Azure Data Studio eseguire quindi il comando **Launch Wizard** (Avvia procedura guidata) dal riquadro comandi (CTRL+MAIUSC+P) nella nuova finestra. Verrà avviata la procedura guidata predefinita fornita da questa estensione:

:::image type="content" source="media/wizard-extension/wizard-template.gif" alt-text="Modello di procedura guidata":::

Si vedrà ora come modificare questa procedura guidata predefinita.

### <a name="develop-the-wizard"></a>Sviluppare la procedura guidata

I file più importanti per iniziare con lo sviluppo dell'estensione sono `package.json`, `src/main.ts` e `vsc-extension-quickstart.md`:

- `package.json`: questo è il file manifesto, in cui viene registrato il comando **Launch Wizard** (Avvia procedura guidata). Questa è anche la posizione in cui `main.ts` viene dichiarato il punto di ingresso principale del programma.
- `main.ts`: contiene il codice per aggiungere elementi dell'interfaccia utente alla procedura guidata, ad esempio pagine, testo e pulsanti
- `vsc-extension-quickstart.md`: contiene la documentazione tecnica che può costituire un riferimento utile durante lo sviluppo

Si vedrà ora come apportare una modifica alla procedura guidata: verrà aggiunta una quarta pagina vuota. Modificare `src/main.ts` come illustrato di seguito. Verrà visualizzata una pagina aggiuntiva quando si avvia la procedura guidata.

:::image type="content" source="media/wizard-extension/wizard-main.png" alt-text="Procedura guidata principale":::
Quando si ha familiarità con il modello, di seguito sono riportate alcune idee aggiuntive da provare:

- Aggiungere un pulsante con larghezza 300 alla nuova pagina
- Aggiungere un componente flessibile in cui inserire il pulsante
- Aggiungere un'azione al pulsante. Ad esempio, quando si fa clic sul pulsante, avviare una finestra di dialogo di apertura file o aprire un editor di query.

## <a name="package-your-extension"></a>Creare il pacchetto dell'estensione

Per condividere l'estensione con altri utenti, è necessario inserirla in un pacchetto costituito da un unico file. Questo può essere pubblicato nel marketplace delle estensioni di Azure Data Studio o condiviso all'interno del team o della community. A tale scopo, è necessario installare un altro pacchetto npm dalla riga di comando:

```console
npm install -g vsce
```

Modificare il file `README.md` in base alla proprie esigenze e quindi passare alla directory di base dell'estensione ed eseguire `vsce package`. Facoltativamente, è possibile collegare un repository con l'estensione o continuare senza. Per aggiungerne uno, aggiungere una riga simile al file `package.json`.

```json
"repository": {
    "type": "git",
    "url": "https://github.com/anjalia/my-test-extension.git"
}
```

Dopo l'aggiunta di queste righe è stato creato un file my-test-extension-0.0.1.vsix pronto per l'installazione in Azure Data Studio.

:::image type="content" source="media/wizard-extension/install-vsix.png" alt-text="Installare VSIX":::

## <a name="publish-your-extension-to-the-marketplace"></a>Pubblicare l'estensione nel marketplace

Il marketplace delle estensioni di Azure Data Studio non è ancora completamente implementato, ma il processo corrente consiste nell'ospitare il pacchetto VSIX dell'estensione in una posizione qualsiasi (ad esempio, una pagina delle versioni di GitHub), quindi inviare una richiesta pull per aggiornare [questo file JSON](https://github.com/Microsoft/azuredatastudio/blob/release/extensions/extensionsGallery.json) con le informazioni sull'estensione.

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione sono state illustrate le procedure per:
> [!div class="checklist"]
> - Installare il generatore di estensioni
> - Creare l'estensione
> - Aggiungere una procedura guidata personalizzata all'estensione
> - Testare l'estensione
> - Creare il pacchetto dell'estensione
> - Pubblicare l'estensione nel marketplace

Ci auguriamo che la lettura di questo articolo sia di ispirazione per creare un'estensione personalizzata per Azure Data Studio. Sono disponibili il supporto per dashboard di informazioni dettagliate (utili grafici eseguiti in SQL Server), una serie di API specifiche di SQL e un enorme set di punti di estensione ereditati da Visual Studio Code.

Se si ha un'idea, ma non si è certi di come iniziare, aprire un problema o inviare un tweet al team: [azuredatastudio](https://twitter.com/azuredatastudio).

È sempre possibile fare riferimento alla [Guida delle estensioni di Visual Studio Code](https://code.visualstudio.com/docs/extensions/overview) perché copre tutte le API e i modelli esistenti.

Per informazioni su come usare T-SQL in Azure Data Studio, completare l'esercitazione sull'editor T-SQL:

> [!div class="nextstepaction"]
> [Usare l'editor Transact-SQL per creare oggetti di database](../tutorial-sql-editor.md)
