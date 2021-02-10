---
title: Estensione Confronto schema
description: Informazioni su come installare e usare l'estensione di confronto schemi di Azure Data Studio per confrontare facilmente due database e modificarli in modo selettivo in modo che corrispondano.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan, sstein
ms.custom: ''
ms.date: 11/04/2019
ms.openlocfilehash: 88f96d75150fc1b3e69d9cf158a50e02110dbae0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048478"
---
# <a name="schema-compare-extension"></a>Estensione Confronto schema

L'estensione Confronto schema consente di confrontare facilmente due definizioni di database e di applicare le differenze dall'origine alla destinazione.

## <a name="features"></a>Funzionalità

* Confrontare gli schemi di due file con estensione dacpac o database
* Visualizzare i risultati come un set di azioni che devono essere eseguite sulla destinazione affinché corrisponda all'origine
* Escludere selettivamente le azioni elencate nei risultati
* Impostare le opzioni che controllano l'ambito del confronto
* Applicare le modifiche alla destinazione o generare uno script con lo stesso effetto
* Salvare il confronto

![Confronto schema: Confronto di esempio](media/schema-compare-extension/schema-compare.png)

## <a name="why-would-i-use-the-schema-compare-extension"></a>Perché usare l'estensione Confronto schema?

Può essere noioso gestire e sincronizzare manualmente versioni di database diverse. L'estensione Confronto schema semplifica il processo di confronto dei database e offre il controllo completo durante la loro sincronizzazione. È possibile filtrare in modo selettivo differenze e categorie di differenze specifiche prima di applicare le modifiche. L'estensione Confronto schema è uno strumento affidabile che consente di risparmiare tempo e codice.

![Confronto schema: Finestra di dialogo Opzioni](media/schema-compare-extension/schema-compare-options.png)

## <a name="install-the-extension"></a>Installare l'estensione

1. Selezionare l'icona Estensioni per visualizzare le estensioni disponibili.

    ![icona Gestione estensioni](media/add-extensions/extension-manager-icon.png)

2. Cercare l'estensione **Confronto schema** e selezionarla per visualizzarne i dettagli. Selezionare **Installa** per aggiungere l'estensione.

3. Dopo l'installazione, **ricaricare** per abilitare l'estensione in Azure Data Studio (necessario solo quando si installa un'estensione per la prima volta).

## <a name="launch-a-schema-compare"></a>Avviare Confronto di schema

1. Per aprire la finestra di dialogo Confronto schema, **fare clic con il pulsante destro del mouse** su un database in Esplora oggetti e quindi scegliere **Confronto schema**. Il database selezionato viene impostato come database di origine nel confronto.

    ![menu di avvio confronto schema](media/schema-compare-extension/schema-compare-launch.png)

2. Selezionare uno dei puntini di sospensione (...) per modificare l'origine e la destinazione del confronto dello schema e selezionare OK.

    ![confronto schema - selezione di origine/destinazione](media/schema-compare-extension/schema-compare-select-source-target.png)

3. Per personalizzare il confronto, selezionare il pulsante **Opzioni** sulla barra degli strumenti.

4. Selezionare **Confronta** per visualizzare i risultati del confronto.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su Confronto schema, vedere [Usare il confronto schema per confrontare definizioni di database diverse](../../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md).
Segnalare eventuali problemi e richieste di funzionalità [qui](https://github.com/microsoft/azuredatastudio/issues).