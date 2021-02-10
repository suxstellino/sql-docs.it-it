---
title: Importare o visualizzare modelli con l'estensione Machine Learning
description: Informazioni su come usare l'estensione Machine Learning per Azure Data Studio per importare un modello ONNX o visualizzare modelli già importati nel database.
ms.prod: azure-data-studio
ms.technology: machine-learning
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: sstein
ms.custom: ''
ms.date: 06/09/2020
ms.openlocfilehash: 3f55f793391e408d96b8c6d07a31c7e790d54853
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048531"
---
# <a name="import-or-view-models-with-machine-learning-extension-for-azure-data-studio-preview"></a>Importare o visualizzare modelli con l'estensione Machine Learning per Azure Data Studio (anteprima)

Informazioni su come usare l'[estensione Machine Learning per Azure Data Studio](machine-learning-extension.md) per importare un modello ONNX o visualizzare modelli già importati nel database.

> [!IMPORTANT]
> L'importazione e la visualizzazione in un database con l'estensione Machine Learning supporta attualmente solo [Machine Learning Services in Istanza gestita di SQL di Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview) e [SQL Edge di Azure con ONNX](/azure/azure-sql-edge/onnx-overview).

## <a name="prerequisites"></a>Prerequisiti

- Installare e configurare l'[estensione Machine Learning per Azure Data Studio](machine-learning-extension.md). È necessario specificare i [percorsi di installazione di Python in Impostazioni estensione](machine-learning-extension.md#settings).

- I pacchetti Python **onnxruntime**, **mlflow** e **mlflow-dbstore**. Se i pacchetti non sono ancora installati, l'estensione Machine Learning richiederà di installarli.

## <a name="view-models"></a>Visualizzazione di modelli

Attenersi alla procedura seguente per visualizzare i modelli ONNX archiviati nel database.

1. Selezionare **Import or view models** (Importa o visualizza modelli).

1. Se viene richiesto di installare **onnxruntime**, **mlflow** e **mlflow-dbstore**, selezionare **Sì**.

1. Selezionare il **database**  e la **tabella** in cui sono archiviati i modelli.

Verrà visualizzato l'elenco dei modelli. È possibile modificare il nome e la descrizione di ogni modello o eliminare un modello dall'elenco.

## <a name="import-a-new-model"></a>Importare un nuovo modello

Attenersi alla procedura seguente per importare un modello ONNX nel database.

1. Selezionare **Import or view models** (Importa o visualizza modelli).

1. Se viene richiesto di installare **onnxruntime**, **mlflow** e **mlflow-dbstore**, selezionare **Sì**.

1. Selezionare **Import models** (Importa modelli).

1. Selezionare il **database di modelli**  in cui si vuole archiviare il modello importato.

1. Selezionare la **tabella di modelli** in cui si vuole archiviare il modello importato. È possibile scegliere una **tabella esistente** o creare una **nuova tabella**. Selezionare **Avanti**.

1. Selezionare la posizione in cui si trova il modello e selezionare **Avanti**. È possibile usare:
    - **Caricamento file**. Scegliere questa opzione per usare un modello da un file. Selezionare il file di modello in **File di origine** e selezionare **Avanti**.
    - **Azure Machine Learning**. Scegliere questa opzione per usare un modello proveniente da Azure Machine Learning. Per prima cosa, **accedere ad Azure**. Selezionare quindi l'**account**, la **sottoscrizione**, il **gruppo di risorse** e l'**area di lavoro ML** di Azure. Selezionare il modello che si vuole usare e selezionare **Avanti**.

1. Immettere **Nome** e **Descrizione** del modello e selezionare **Distribuisci** per archiviare il modello nel database.

> [!NOTE]
> L'estensione Machine Learning è attualmente in anteprima. Lo schema della tabella in cui vengono archiviati i modelli, quindi, potrebbe cambiare in futuro.

## <a name="next-steps"></a>Passaggi successivi

- [Estensione Machine Learning in Azure Data Studio](machine-learning-extension.md)
- [Gestire i pacchetti nel database](machine-learning-extension-manage-packages.md)
- [Eseguire stime](machine-learning-extension-predictions.md)
- [Documentazione per Machine Learning in SQL](../../machine-learning/index.yml)
- [Machine Learning Services in Istanza gestita di SQL di Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview)
- [Machine Learning e intelligenza artificiale con ONNX in SQL Edge (anteprima)](/azure/azure-sql-edge/onnx-overview)
