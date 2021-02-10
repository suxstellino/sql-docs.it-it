---
title: Estensione Kusto (KQL) per Azure Data Studio
description: Questo articolo descrive come connettersi ed eseguire query sui cluster di Esplora dati di Azure con Azure Data Studio.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: jukoesma
ms.custom: ''
ms.date: 10/29/2020
ms.openlocfilehash: 15b2dc331a1387039c79cbab164682f00981c591
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052032"
---
# <a name="kusto-kql-extension-for-azure-data-studio-preview"></a>Estensione Kusto (KQL) per Azure Data Studio (anteprima)

L'estensione Kusto (KQL) per [Azure Data Studio](../what-is-azure-data-studio.md) consente di connettersi ed eseguire query nei cluster di [Esplora dati di Azure](/azure/data-explorer/data-explorer-overview).

Gli utenti possono scrivere ed eseguire query KQL e creare notebook con il [kernel Kusto](../notebooks/notebooks-kusto-kernel.md) provvisto di IntelliSense.

L'abilitazione dell'esperienza Kusto (KQL) nativa in Azure Data Studio consente a data engineer, data scientist e analisti dei dati di osservare rapidamente tendenze e anomalie in grandi quantità di dati archiviati in Esplora dati di Azure.

Questa estensione è attualmente disponibile in anteprima.

## <a name="prerequisites"></a>Prerequisiti

Se non si ha una sottoscrizione di Azure, creare un [account Azure gratuito](https://azure.microsoft.com/free/) prima di iniziare.

Sono richiesti anche i prerequisiti seguenti:

- [Azure Data Studio installato](../download-azure-data-studio.md).
- [Un cluster e un database di Esplora dati di Azure](/azure/data-explorer/create-cluster-database-portal).

## <a name="install-the-kusto-kql-extension"></a>Installare l'estensione Kusto (KQL)

Per installare l'estensione Kusto (KQL) in Azure Data Studio, seguire questa procedura.

1. Aprire Gestione estensioni in Azure Data Studio. È possibile selezionare l'icona delle estensioni o scegliere **Estensioni** dal menu Visualizza.

2. Digitare *Kusto* nella barra di ricerca.

3. Selezionare l'estensione **Kusto (KQL)** e visualizzarne i dettagli.

4. Selezionare **Installa**.

:::image type="content" source="media/kusto-extension/kusto-extension-icon.png" alt-text="Estensione Kusto":::

## <a name="how-to-connect-to-an-azure-data-explorer-cluster"></a>Come connettersi a un cluster di Esplora dati di Azure

### <a name="find-your-azure-data-explorer-cluster"></a>Trovare il cluster di Esplora dati di Azure

Trovare il cluster di Esplora dati di Azure nel [portale di Azure](https://ms.portal.azure.com/#home), quindi trovare l'URI del cluster.

:::image type="content" source="media/kusto-extension/kusto-extension-adx-cluster-uri.png" alt-text="URI":::

È possibile iniziare immediatamente usando il cluster *help.kusto.windows.net*.

Gli esempi di questo articolo usano i dati del cluster help.kusto.windows.net.

### <a name="connection-details"></a>Dettagli connessione

Per configurare un cluster di Esplora dati di Azure per la connessione, seguire questa procedura.

1. Selezionare **Nuova connessione** nel riquadro **Connessioni**.

2. Immettere i dati necessari in **Dettagli connessione**.
    1. In **Tipo di connessione** selezionare *Kusto*.
    2. In **Cluster** immettere il cluster di Esplora dati di Azure.

        > [!Note]
        > Immettere il nome del cluster senza includere il prefisso `https://` o accodare `/`.

    3. In **Tipo di autenticazione** usare l'impostazione predefinita *Azure Active Directory - Universale con supporto MFA*.
    4. In **Account** usare le informazioni dell'account.
    5. In **Database** usare *Predefinito*.
    6. In **Gruppo di server** usare *Predefinito*.
        1. È possibile usare questo campo per organizzare i server in un gruppo specifico.
    7. Lasciare vuoto il campo **Nome (facoltativo)** .
        1. È possibile usare questo campo per assegnare un alias al server.

    :::image type="content" source="media/kusto-extension/kusto-extension-connection-details.png" alt-text="Dettagli connessione":::

## <a name="how-to-query-an-azure-data-explorer-database-in-azure-data-studio"></a>Come eseguire query in un database di Esplora dati in Azure Data Studio

Ora che è stata configurata una connessione al cluster Esplora dati di Azure, è possibile eseguire una query sui database usando Kusto (KQL).

Per creare una nuova scheda query è possibile selezionare **File > nuova query**, usare *CTRL+N* oppure fare clic con il pulsante destro del mouse sul database e selezionare **Nuova query**.

Quando viene aperta la scheda della nuova query, immettere la query Kusto.

Di seguito sono riportati alcuni esempi di query KQL:

```kusto
StormEvents
| limit 1000
```

```kusto
StormEvents
| where EventType == "Waterspout"
```

Per altre informazioni sulla scrittura di query KQL, vedere [Scrivere query per Esplora dati di Azure](/azure/data-explorer/write-queries#overview-of-the-query-language)

## <a name="view-extension-settings"></a>Visualizzare le impostazioni dell'estensione

Per modificare le impostazioni per l'estensione Kusto, seguire questa procedura.

1. Aprire Gestione estensioni in Azure Data Studio. È possibile selezionare l'icona delle estensioni o scegliere **Estensioni** dal menu Visualizza.

2. Trovare l'estensione **Kusto (KQL)** .

3. Selezionare l'icona **Gestisci**.

4. Selezionare l'icona **Impostazioni estensione**.

Le impostazioni delle estensioni hanno un aspetto simile al seguente:

:::image type="content" source="media/kusto-extension/kusto-extension-settings.png" alt-text="Impostazioni estensione Kusto (KQL)":::

## <a name="sanddance-visualization"></a>Visualizzazione SandDance

L'[estensione SandDance](sanddance-extension.md) e l'estensione Kusto (KQL) usate insieme in Azure Data Studio apportano una visualizzazione interattiva avanzata. Nel set di risultati della query KQL selezionare il pulsante **Visualizzatore** per avviare [SandDance](https://sanddance.js.org/).

:::image type="content" source="media/kusto-extension/kusto-extension-sanddance-demo.gif" alt-text="Visualizzazione SandDance":::

## <a name="known-issues"></a>Problemi noti

| Dettagli | Soluzione alternativa |
|---------|------------|
| [In un notebook Kusto la modifica di una connessione di database in una connessione alias salvata rimane bloccata dopo un errore nell'esecuzione della cella di codice](https://github.com/microsoft/azuredatastudio/issues/12384) | Chiudere e riaprire il notebook, quindi connettersi al cluster corretto con il database |
| [In un notebook Kusto la modifica di una connessione di database in una connessione alias non salvata non funziona](https://github.com/microsoft/azuredatastudio/issues/12843) |Creare una nuova connessione dal viewlet di connessione e salvarla con un alias. Creare quindi un nuovo notebook e connettersi alla nuova connessione salvata. | 
| [In un notebook Kusto l'elenco a discesa dei database non viene popolato quando si crea una nuova connessione ADX](https://github.com/microsoft/azuredatastudio/issues/12666) | Creare una nuova connessione dal viewlet di connessione e salvarla con un alias. Creare quindi un nuovo notebook e connettersi alla nuova connessione salvata. |

È possibile inviare una [richiesta di funzionalità](https://github.com/microsoft/azuredatastudio/issues/new?assignees=&labels=&template=feature_request.md&title=) per inviare commenti e suggerimenti al team del prodotto.  
È possibile registrare un [bug](https://github.com/microsoft/azuredatastudio/issues/new?assignees=&labels=&template=bug_report.md&title=) per fornire commenti e suggerimenti al team del prodotto.

## <a name="next-steps"></a>Passaggi successivi

- [Creare ed eseguire un notebook Kusto](../notebooks/notebooks-kusto-kernel.md)
- [Notebook Kqlmagic in Azure Data Studio](../notebooks/notebooks-kqlmagic.md)
- [Scheda di riferimento rapido sul passaggio da SQL a Kusto](/azure/data-explorer/kusto/query/sqlcheatsheet)
- [Informazioni su Esplora dati di Azure](/azure/data-explorer/data-explorer-overview)
- [Uso delle visualizzazioni SandDance](https://sanddance.js.org/)
