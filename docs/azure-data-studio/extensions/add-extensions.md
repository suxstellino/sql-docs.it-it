---
title: Aggiungere estensioni
description: Informazioni su come aggiungere funzionalità ad Azure Data Studio selezionando e installando le estensioni tra quelle fornite da Microsoft e da terze parti.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, maghan, sstein
ms.custom: ''
ms.date: 10/03/2019
ms.openlocfilehash: 3cc965ffd169d53e59706ca4ba68f2bcdddb5aa9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052121"
---
# <a name="extend-the-functionality-of-azure-data-studio"></a>Estendere la funzionalità di Azure Data Studio

Le estensioni di Azure Data Studio consentono di aggiungere facilmente nuove funzionalità all'installazione di base di questo strumento.

Le estensioni vengono fornite sia dal team di Azure Data Studio (Microsoft) sia dalla community di terze parti. Per altre informazioni sulla creazione di estensioni, vedere [Creazione di estensioni](./extension-authoring.md).

## <a name="add-azure-data-studio-extensions"></a>Aggiungere estensioni ad Azure Data Studio

1. Accedere alle estensioni disponibili selezionando l'icona Estensioni o scegliendo **Estensioni** dal menu **Visualizza**. È possibile usare il comando **Visualizza: Mostra estensioni**, disponibile nel **Riquadro comandi** (F1 o `Ctrl+Shift+P`).

    ![icona Gestione estensioni](media/add-extensions/extension-manager-icon.png)

    È anche possibile accedere rapidamente a Gestione estensioni premendo `Ctrl+Shift+X` (Windows/Linux) o `Command+Shift+X` (Mac).

2. Selezionare un'estensione disponibile per visualizzarne i dettagli.

    ![Dettagli dell'estensione](media/add-extensions/extension-details.png)

3. Selezionare l'estensione desiderata e **installarla**.

4. Dopo l'installazione, **ricaricare** per abilitare l'estensione in Azure Data Studio (necessario solo quando si installa un'estensione per la prima volta).

Se si verificano problemi durante l'accesso a Gestione estensioni in Azure Data Studio, è possibile scaricare l'estensione necessaria nella [Wiki di GitHub](https://github.com/microsoft/azuredatastudio/wiki/List-of-Extensions).

## <a name="manage-extensions"></a>Gestire le estensioni

### <a name="list-installed-extensions"></a>Elencare le estensioni installate

La visualizzazione predefinita Estensioni mostra le estensioni attualmente abilitate, tutte le estensioni consigliate per l'utente e un contenitore compresso di tutte le estensioni attualmente disabilitate. Il comando **Estensioni: Mostra estensioni installate**, disponibile nel **Riquadro comandi** o nel menu a discesa **Altre azioni** `(...)`, mostra un elenco di tutte le estensioni installate, incluse le estensioni disabilitate.

### <a name="uninstall-an-extension"></a>Disinstallare un'estensione

Per disinstallare un'estensione, fare clic sull'icona a forma di ingranaggio a destra della voce di un'estensione e scegliere **Disinstalla** dal menu a discesa. L'estensione selezionata verrà disinstallata e verrà richiesto di ricaricare Azure Data Studio.

 ![Menu a discesa dell'estensione](media/add-extensions/extension-gear-dropdown.png)

### <a name="disable-an-extension"></a>Disabilitare un'estensione

È possibile disabilitare temporaneamente un'estensione invece di rimuoverla in modo definitivo. È possibile disabilitare un'estensione in tutte le sessioni di Azure Data Studio (**Disabilita**) o solo per l'area di lavoro corrente (**Disabilita (area di lavoro)** ). È anche possibile disabilitare tutte le estensioni attualmente installate tramite il **Riquadro comandi** con i comandi **Estensioni: Disattiva tutte le estensioni** ed **Estensioni: Disable All Extensions (Workspace)** (Disabilita tutte le estensioni - Area di lavoro).

### <a name="enable-an-extension"></a>Abilitare un'estensione

Se un'estensione è stata disabilitata, sarà presente nella sezione **disabled** dell'elenco di estensioni e contrassegnata come **_disabled_*_. È possibile riabilitarla con i comandi _* Abilita** o **Abilita (area di lavoro)** nel menu a discesa. Il **Riquadro comandi** consente anche di abilitare tutte le estensioni con i comandi **Estensioni: Enable All Extensions** (Abilita tutte le estensioni) ed **Estensioni: Enable All Extensions (Workspace)** (Abilita tutte le estensioni - Area di lavoro).

![Abilitazione dell'estensione](media/add-extensions/extensions-enable.png)

### <a name="updating-an-extension"></a>Aggiornamento di un'estensione

Azure Data Studio cerca e installa automaticamente gli aggiornamenti per qualsiasi estensione installata. Per disattivare la funzionalità di aggiornamento automatico, è possibile disabilitarla con il comando **Estensioni: Disabilita l'aggiornamento automatico delle estensioni**.

Per aggiornare manualmente un'estensione, è possibile cercare gli aggiornamenti dell'estensione con il comando **Estensioni: Mostra estensioni obsolete** che esegue una ricerca nell'elenco di estensioni usando il filtro `@outdated`. Verranno visualizzati tutti gli aggiornamenti disponibili per tutte le estensioni attualmente installate. Fare clic sul pulsante **Aggiorna** per un'estensione obsoleta e l'aggiornamento verrà installato. Verrà quindi richiesto di ricaricare Azure Data Studio. È anche possibile aggiornare contemporaneamente tutte le estensioni obsolete con il comando **Estensioni: Aggiorna tutte le estensioni**.

Anche il comando **Estensioni: Check for Extensions Updates** (Cerca gli aggiornamenti delle estensioni) consente di verificare per quali estensioni sono disponibili aggiornamenti.

## <a name="install-from-a-vsix"></a>Installazione da un file VSIX

È possibile installare manualmente un'estensione di Azure Data Studio compressa in un file con estensione `.vsix` usando il comando **Installa da VSIX** nell'elenco a discesa dei comandi della visualizzazione Estensioni o il comando **Estensioni: Installa da VSIX** nel riquadro comandi e puntando al file `.vsix` dell'estensione.

## <a name="access-installed-azure-data-studio-extensions"></a>Accedere alle estensioni di Azure Data Studio installate

Ogni estensione migliora l'esperienza in Azure Data Studio in modo diverso. Di conseguenza, il punto di ingresso per le estensioni può variare. Vedere la documentazione specifica per l'estensione installata per informazioni sul modo in cui è possibile accedere alle relative funzionalità dopo l'installazione.

## <a name="extensions-view-filters"></a>Filtri della visualizzazione Estensioni

La casella di ricerca della visualizzazione Estensioni supporta filtri che consentono di trovare e gestire le estensioni. I comandi **Mostra estensioni installate** e **Mostra estensioni consigliate** usano filtri come `@installed` e `@recommended` nella casella di ricerca.

Per visualizzare un elenco completo di tutti i filtri e dei comandi di ordinamento, digitare @ nella casella di ricerca delle estensioni ed esplorare i suggerimenti:

![Ordinamento delle estensioni](media/add-extensions/extension-sort.png)

Ecco i filtri della visualizzazione Estensioni:

- `@builtin`: mostra le estensioni di Azure Data Studio raggruppate per tipo (linguaggi di programmazione, temi e così via).
- `@disabled`: mostra le estensioni installate disabilitate.
- `@enabled`: mostra le estensioni installate abilitate. Le estensioni possono essere abilitate/disabilitate singolarmente.
- `@installed`: mostra le estensioni installate.
- `@outdated`: mostra le estensioni installate obsolete. Una versione più recente è disponibile nel Marketplace.
- `@recommended`: mostra le estensioni consigliate raggruppate come specifiche dell'area di lavoro o per uso generale.
- `@category`: mostra le estensioni appartenenti a una categoria specificata. Di seguito sono elencate alcune categorie supportate. Per un elenco completo, digitare @category e seguire le opzioni nell'elenco dei suggerimenti:
    - `@category:themes`
    - `@category:formatters`
    - `@category:snippets` Questi filtri possono anche essere combinati. `@installed @category:themes`, ad esempio, visualizza tutti i temi installati.

Se non vengono specificati filtri, la visualizzazione Estensioni mostra le estensioni attualmente installate e consigliate.

### <a name="sorting"></a>Ordinamento

È possibile ordinare le estensioni con il filtro `@sort`, che può accettare i valori seguenti:

- `installs`: ordina per numero di installazioni della raccolta di estensioni, in ordine decrescente.
- `rating`: ordina per valutazione (da 1 a 5 stelle) della raccolta di estensioni, in ordine decrescente.
- `name`: ordina alfabeticamente in base al nome dell'estensione.

## <a name="common-questions"></a>Domande frequenti

### <a name="where-are-extensions-installed"></a>Dove sono le estensioni installate?

Le estensioni vengono installate in una cartella extensions per ogni utente. A seconda della piattaforma, il percorso della cartella è il seguente:

- Windows `%USERPROFILE%\.azuredatastudio\extensions`
- macOS `~/.azuredatastudio/extensions`
- Linux `~/.azuredatastudio/extensions`
