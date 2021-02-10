---
title: Impostazioni utente e dell'area di lavoro
description: Informazioni su come usare le impostazioni per personalizzare l'editor, l'interfaccia utente e il comportamento funzionale di Azure Data Studio in base alle proprie preferenze.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: how-to
author: yualan
ms.author: alayu
ms.reviewer: maghan, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: 76f6cedaa26165d14ce96b234b0d6cd8129806e6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048201"
---
# <a name="modify-user-and-workspace-settings"></a>Modificare le impostazioni utente e dell'area di lavoro

Le impostazioni consentono di configurare facilmente Azure Data Studio in base alle preferenze personali. Sono disponibili opzioni per la modifica di quasi tutti i componenti dell'editor, dell'interfaccia utente e del comportamento funzionale di Azure Data Studio.

In Azure Data Studio sono disponibili due ambiti di impostazioni diversi:

* **Utente** - Queste impostazioni vengono applicate globalmente a qualsiasi istanza di Azure Data Studio aperta.
* **Area di lavoro** - Le impostazioni dell'area di lavoro sono specifiche di una cartella del computer e sono disponibili solo se la cartella è aperta nella barra laterale Explorer. Le impostazioni definite in questo ambito sostituiscono quelle specificate nell'ambito dell'utente.

## <a name="creating-user-and-workspace-settings"></a>Creazione di impostazioni utente e dell'area di lavoro

Il comando di menu **File** > **Preferenze** > **Impostazioni** (**Codice** > **Preferenze** > **Impostazioni** in Mac) costituisce il punto di ingresso per la configurazione delle impostazioni utente e dell'area di lavoro. Viene visualizzato un elenco di impostazioni predefinite. Copiare le impostazioni che si vuole modificare nel file `settings.json` appropriato. Le schede a destra consentono di spostarsi rapidamente tra i file delle impostazioni utente e quelli delle impostazioni dell'area di lavoro.

È possibile aprire le impostazioni utente e quelle dell'area di lavoro da **Riquadro comandi** (**CTRL+MAIUSC+P**) con **Preferenze: Apri impostazioni utente** e **Preferenze: Apri impostazioni area di lavoro** oppure usare la scelta rapida da tastiera (**CTRL +** ).

Nell'esempio seguente vengono disabilitati i numeri di riga nell'editor e vengono configurate righe di codice di cui aumentare automaticamente il rientro.

![Impostazioni di esempio](media/settings/sample-settings.png)

Le modifiche alle impostazioni vengono ricaricate da Azure Data Studio dopo il salvataggio del file `settings.json` modificato.

> [!NOTE]
> Le impostazioni dell'area di lavoro sono particolarmente utili per condividere le impostazioni specifiche di un progetto tra i membri del team.

## <a name="settings-file-locations"></a>Percorsi del file delle installazioni

A seconda della piattaforma, il file delle impostazioni utente si trova qui:

* **Windows** `%APPDATA%\azuredatastudio\User\settings.json`
* **Mac** `$HOME/Library/Application Support/azuredatastudio/User/settings.json`
* **Linux** `$HOME/.config/azuredatastudio/User/settings.json`

Il file di impostazioni dell'area di lavoro si trova nella cartella `.Azure Data Studio` del progetto.

## <a name="hot-exit"></a>Hot Exit

Per impostazione predefinita, Azure Data Studio ricorda le modifiche non salvate apportate ai file anche quando si chiude il programma. In Visual Studio Code è una funzionalità analoga a Hot Exit.

Per impostazione predefinita, la funzionalità Hot Exit è disattivata. Per attivare la funzionalità Hot Exit, modificare l'impostazione `files.hotExit`. Per informazioni dettagliate, vedere [Hot Exit (nella documentazione di Visual Studio Code)](https://code.visualstudio.com/docs/editor/codebasics#_hot-exit).

## <a name="tab-color"></a>Colore delle schede

Per semplificare l'identificazione delle connessioni in uso, è possibile impostare i colori delle schede aperte nell'editor in modo che corrispondano al colore del gruppo di server a cui appartiene la connessione. Per impostazione predefinita, i colori delle schede sono disattivati. Per attivare i colori delle schede, modificare l'impostazione `sql.tabColorMode`.

## <a name="additional-resources"></a>Risorse aggiuntive

Azure Data Studio ha ereditato le funzionalità delle impostazioni utente e dell'area di lavoro da Visual Studio Code. Per informazioni dettagliate sulle impostazioni, è quindi possibile vedere l'articolo [Impostazioni per Visual Studio Code](https://code.visualstudio.com/docs/getstarted/settings).
