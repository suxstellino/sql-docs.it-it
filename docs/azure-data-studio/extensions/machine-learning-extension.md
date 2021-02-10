---
title: Estensione Machine Learning
description: L'estensione Machine Learning per Azure Data Studio consente di gestire pacchetti, importare modelli di Machine Learning, eseguire stime e creare notebook per eseguire esperimenti per i database SQL.
ms.prod: azure-data-studio
ms.technology: machine-learning
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: sstein
ms.custom: ''
ms.date: 05/19/2020
ms.openlocfilehash: 8eb7dd5f5a11c508af0d85e7559349c14e558adb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051952"
---
# <a name="machine-learning-extension-for-azure-data-studio-preview"></a>Estensione Machine Learning per Azure Data Studio (anteprima)

L'estensione Machine Learning per [Azure Data Studio](../what-is-azure-data-studio.md) consente di gestire pacchetti, importare modelli di Machine Learning, eseguire stime e creare notebook per eseguire esperimenti per i database SQL. Questa estensione è attualmente disponibile in anteprima.

## <a name="prerequisites"></a>Prerequisiti

Nel computer in cui si esegue Azure Data Studio devono essere installati i prerequisiti seguenti.

- [Python 3](https://www.python.org/downloads/). Dopo aver installato Python, è necessario specificare il percorso locale di un'installazione di Python in [Impostazioni estensione](#settings). Se è stato usato il [notebook di un kernel Python ](../notebooks/notebooks-python-kernel.md) in Azure Data Studio, per impostazione predefinita l'estensione usa il percorso del notebook.

- [Microsoft ODBC Driver 17 for SQL Server](../../connect/odbc/download-odbc-driver-for-sql-server.md) per Windows, macOS o Linux.

- [R 3.5](https://www.r-project.org/) (facoltativo). Versioni diverse dalla 3.5 non sono attualmente supportate. Dopo aver installato R 3.5, è necessario abilitare R e specificare il percorso locale di un'installazione di R in [Impostazioni estensione](#settings). Questa operazione è necessaria solo se si vogliono gestire pacchetti R nel database.

### <a name="trouble-installing-python-3-from-within-ads"></a>Problemi di installazione di Python 3 dall'interno di ADS?

Se si tenta di installare Python 3, ma si riceve un errore relativo a TLS/SSL, aggiungere questi due componenti facoltativi:

_errore di esempio:_
```
$: ~/0.0.1/bin/python3 -m pip install --user "jupyter>=1.0.0" --extra-index-url https://prose-python-packages.azurewebsites.net
WARNING: pip is configured with locations that require TLS/SSL, however the ssl module in Python is not available.
Looking in indexes: https://pypi.org/simple, https://prose-python-packages.azurewebsites.net
Requirement already satisfied: jupyter
```

_installare questi:_

- [Homebrew](https://brew.sh) (facoltativo). Installare homebrew e quindi eseguire `brew update` dalla riga di comando.

- *openssl* (facoltativo). Eseguire poi `brew install openssl`.

## <a name="install-the-extension"></a>Installare l'estensione

Per installare l'estensione Machine Learning in Azure Data Studio, attenersi alla procedura riportata di seguito.

1. Aprire Gestione estensioni in Azure Data Studio. È possibile selezionare l'icona delle estensioni o scegliere Estensioni dal menu Visualizza.

1. Selezionare l'estensione **Machine Learning** estensione e visualizzarne i dettagli.

1. Selezionare **Installa**.

1. Selezionare **Ricarica** per abilitare l'estensione. Questa operazione è necessaria solo la prima volta che si installa un'estensione.

<a name="settings"></a>

## <a name="extension-settings"></a>Impostazioni estensione

Per modificare le impostazioni per l'estensione Machine Learning, attenersi alla procedura riportata di seguito.

1. Aprire Gestione estensioni in Azure Data Studio. È possibile selezionare l'icona delle estensioni o scegliere Estensioni dal menu Visualizza.

1. Individuare l'estensione **Machine Learning** tra le estensioni **abilitate**.

1. Selezionare l'icona **Gestisci**.

1. Selezionare l'icona **Impostazioni estensione**.

Le impostazioni delle estensioni hanno un aspetto simile al seguente:

:::image type="content" source="media/machine-learning-extension/ml-extension-settings.png" alt-text="Impostazioni dell'estensione Machine Learning":::

### <a name="enable-python"></a>Abilitare Python

Per usare l'estensione Machine Learning e la gestione dei pacchetti Python nel database, eseguire la procedura seguente.

> [!IMPORTANT]
> Per il funzionamento dell'estensione Machine Learning è necessario che Python sia abilitato e configurato per la maggior parte delle funzionalità, anche se non si vuole usare la gestione dei pacchetti Python nella funzionalità del database.

1. Verificare che l'impostazione **Machine Learning: Enable Python** (Machine Learning: abilita Python) sia abilitata. Per impostazione predefinita, questa impostazione è abilitata.

1. Specificare il percorso dell'installazione di Python preesistente in **Machine Learning: Percorso Python**. Può trattarsi del percorso completo del file eseguibile di Python o della cartella in cui si trova il file eseguibile. Se è stato usato il [notebook di un kernel Python ](../notebooks/notebooks-python-kernel.md) in Azure Data Studio, per impostazione predefinita l'estensione usa il percorso del notebook.

### <a name="enable-r"></a>Abilitare R

Per usare l'estensione Machine Learning per la gestione dei pacchetti R nel database, eseguire la procedura seguente.

1. Verificare che l'impostazione **Machine Learning: Enable R** (Machine Learning: abilita R) sia abilitata. Per impostazione predefinita, questa opzione è disabilitata.

1. Specificare il percorso dell'installazione di R preesistente in **Machine Learning: Percorso R**. Deve corrispondere al percorso completo del file eseguibile di R. 

## <a name="use-the-machine-learning-extension"></a>Usare l'estensione Machine Learning

Per usare l'estensione Machine Learning in Azure Data Studio, attenersi alla procedura riportata di seguito.

1. Aprire il viewlet **Connessioni** in Azure Data Studio.

1. Fare clic con il pulsante destro del mouse sul server e scegliere **Gestisci**.

1. Selezionare **Machine Learning** nel menu a sinistra in **Generale**.

Seguire i collegamenti in **Passaggi successivi** per vedere come usare l'estensione Machine Learning per gestire i pacchetti, eseguire stime e importare modelli nel database.

## <a name="next-steps"></a>Passaggi successivi

- [Gestire i pacchetti nel database](machine-learning-extension-manage-packages.md)
- [Eseguire stime](machine-learning-extension-predictions.md)
- [Importare o visualizzare modelli](machine-learning-extension-import-view-models.md)
- [Notebook in Azure Data Studio](../notebooks/notebooks-guidance.md)
- [Documentazione per Machine Learning in SQL](../../machine-learning/index.yml)
- [Machine Learning e intelligenza artificiale con ONNX in SQL Edge (anteprima)](/azure/azure-sql-edge/onnx-overview)