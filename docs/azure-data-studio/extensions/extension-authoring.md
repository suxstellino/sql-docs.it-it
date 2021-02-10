---
title: Creare estensioni
description: È possibile aggiungere funzionalità ad Azure Data Studio con un'estensione. Informazioni su come creare un'estensione e pubblicarla nella raccolta delle estensioni.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: alayu
ms.custom: ''
ms.date: 08/26/2020
ms.openlocfilehash: a7cb4ede1c8ef11d55a40b5b72d825ba02c5a550
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048571"
---
# <a name="extend-functionality-by-creating-azure-data-studio-extensions"></a>Estendere le funzionalità creando estensioni per Azure Data Studio

Le estensioni di Azure Data Studio consentono di aggiungere facilmente nuove funzionalità all'installazione di base di questo strumento.

Le estensioni vengono fornite sia dal team di Azure Data Studio (Microsoft) sia dalla community di terze parti.

## <a name="author-an-extension"></a>Creare un'estensione

Se si è interessati a estendere Azure Data Studio, è possibile creare un'estensione personalizzata e pubblicarla nella raccolta di estensioni.

### <a name="write-an-extension"></a>Scrivere un'estensione

#### <a name="prerequisites"></a>Prerequisiti

Per sviluppare un'estensione, è necessario che [Node.js](https://nodejs.org/) sia installato e disponibile in `$PATH`. Node.js include npm, lo strumento di gestione dei pacchetti di Node.js, che viene usato per installare il generatore di estensioni.

Per creare la nuova estensione, è possibile usare il generatore di estensioni di Azure Data Studio. Il [generatore di estensioni](https://www.npmjs.com/package/generator-azuredatastudio) Yeoman è un utile punto di partenza per i progetti di estensioni. Per avviare il generatore, immettere il comando seguente in un prompt dei comandi:

```console
npm install -g yo generator-azuredatastudio
yo azuredatastudio
```

Per informazioni approfondite su come iniziare a usare il modello di estensione, vedere [Estensione della mappatura tastiera](keymap-extension.md), che illustra la creazione di un'estensione.

### <a name="extensibility-references"></a>Informazioni di riferimento sull'estendibilità

Per informazioni sull'estendibilità di Azure Data Studio, vedere [Panoramica dell'estendibilità](../extensibility.md). È anche possibile vedere esempi di come usare l'API negli [esempi](https://github.com/Microsoft/azuredatastudio/tree/main/samples) esistenti.

## <a name="debug-an-extension"></a>Eseguire il debug di un'estensione

È possibile eseguire il debug della nuova estensione usando l'estensione [Azure Data Studio Debug](https://github.com/kevcunnane/sqlops-debug) di Visual Studio Code.

Per eseguire il debug dell'estensione:

1. Aprire l'estensione con [Visual Studio Code](https://code.visualstudio.com/).
2. Installare l'estensione Azure Data Studio Debug.
3. Premere **F5** oppure selezionare l'icona **Debug** e quindi selezionare **Avvia**.
4. Una nuova istanza di Azure Data Studio viene avviata in una modalità speciale (host di sviluppo estensione). Questa nuova istanza è ora in grado di riconoscere l'estensione.

## <a name="create-an-extension-package"></a>Creare un pacchetto di estensioni

Dopo aver scritto l'estensione, è necessario creare un pacchetto VSIX installabile in Azure Data Studio. È possibile usare [vscode-vsce](https://github.com/Microsoft/vscode-vsce) (Visual Studio Code Extension) per creare il pacchetto VSIX.

```console
npm install -g vsce
cd myExtensionName
vsce package
# The myExtensionName.vsix file has now been generated
```

Con un pacchetto VSIX, è possibile condividere l'estensione in locale e in privato condividendo il file con estensione vsix e usando il comando **Extensions: Install From VSIX File** (Estensioni: Installa da file VSIX) dal riquadro comandi per installare l'estensione in Azure Data Studio.

## <a name="publish-an-extension"></a>Pubblicare un'estensione

Per pubblicare la nuova estensione in Azure Data Studio:

1. Aggiungere l'estensione alla [raccolta di estensioni](https://github.com/Microsoft/azuredatastudio/blob/release/extensions/extensionsGallery.json).
2. Attualmente non è disponibile il supporto per ospitare estensioni di terze parti. Anziché scaricare l'estensione, Azure Data Studio consente di passare a una pagina di download. Per impostare una pagina di download per l'estensione, impostare il valore di asset **Microsoft.AzureDataStudio.DownloadPage**.
3. Creare una richiesta pull per il ramo Release/Extensions.
4. Inviare una richiesta di revisione al team.

L'estensione verrà verificata e aggiunta alla raccolta di estensioni.

### <a name="publish-extension-updates"></a>Pubblicare gli aggiornamenti delle estensioni

Il processo di pubblicazione degli aggiornamenti è simile alla pubblicazione dell'estensione. Assicurarsi che la versione sia aggiornata in package.json.

## <a name="next-steps"></a>Passaggi successivi

Per istruzioni dettagliate su come iniziare, vedere una delle esercitazioni seguenti sulla creazione di estensioni:

- [Esercitazione per l'estensione di tipo mappatura della tastiera](keymap-extension.md)
- [Esercitazione per l'estensione di tipo dashboard](dashboard-extension.md)
- [Esercitazione per l'estensione di tipo notebook](notebook-extension.md)
- [Esercitazione per l'estensione di tipo book Jupyter](jupyter-book-extension.md)
- [Esercitazione per l'estensione di tipo procedura guidata](wizard-extension.md)
