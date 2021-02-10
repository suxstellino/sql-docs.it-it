---
title: Eseguire stime con l'estensione Machine Learning
description: Informazioni su come usare l'estensione Machine Learning per Azure Data Studio per eseguire stime con un modello ONNX nel database.
ms.prod: azure-data-studio
ms.technology: machine-learning
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: sstein
ms.custom: ''
ms.date: 06/09/2020
ms.openlocfilehash: 6da9918b4ebb3ed571d82d23c123c8e477741c0b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048541"
---
# <a name="make-predictions-with-machine-learning-extension-for-azure-data-studio-preview"></a>Eseguire stime con l'estensione Machine Learning per Azure Data Studio (anteprima)

Informazioni su come usare l'[estensione Machine Learning per Azure Data Studio](machine-learning-extension.md) per eseguire stime con un modello ONNX nel database. L'estensione genera uno script T-SQL tramite l'istruzione [PREDICT](../../t-sql/queries/predict-transact-sql.md) per eseguire stime sul set di dati archiviato nella tabella con un modello importato in precedenza, che risiede in un file locale o proveniente da Azure Machine Learning.

> [!IMPORTANT]
> L'esecuzione di stime con l'estensione Machine Learning supporta attualmente solo [Machine Learning Services in Istanza gestita di SQL di Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview) e [SQL Edge di Azure con ONNX](/azure/azure-sql-edge/onnx-overview).

## <a name="prerequisites"></a>Prerequisiti

- Installare e configurare l'[estensione Machine Learning per Azure Data Studio](machine-learning-extension.md). È necessario specificare i [percorsi di installazione di Python in Impostazioni estensione](machine-learning-extension.md#settings).

- I pacchetti Python **onnxruntime**, **mlflow** e **mlflow-dbstore**. Se i pacchetti non sono ancora installati, l'estensione Machine Learning richiederà di installarli.

## <a name="make-predictions-from-onnx-model"></a>Eseguire stime dal modello ONNX

Attenersi alla procedura seguente per usare un modello ONNX per eseguire stime.

1. Selezionare **Make predictions** (Esegui stime).

1. Se viene richiesto di installare **onnxruntime**, **mlflow** e **mlflow-dbstore**, selezionare **Sì**.

1. Scegliere la posizione in cui si trova il modello e selezionare **Avanti**. È possibile usare:
    - **Modelli importati**. Scegliere questa opzione per usare un modello già archiviato nel database. Scegliere il **database** e la **tabella** in cui si trova il modello, selezionare il modello che si vuole usare e selezionare **Avanti**.
    - **Caricamento file**. Scegliere questa opzione per usare un modello da un file. Selezionare il file di modello in **File di origine** e selezionare **Avanti**.
    - **Azure Machine Learning**. Scegliere questa opzione per usare un modello proveniente da Azure Machine Learning. Per prima cosa, **accedere ad Azure**. Selezionare quindi l'**account**, la **sottoscrizione**, il **gruppo di risorse** e l'**area di lavoro ML** di Azure. Selezionare il modello che si vuole usare e selezionare **Avanti**.

1. Eseguire il mapping dei dati di origine al modello.
    - Selezionare il **database di origine** e la **tabella di origine** contenenti il set di dati a cui si vuole applicare la stima.
    - Eseguire il mapping delle colonne in **Model Input mapping** (Mapping input modello) e **Model output** (Output modello). L'estensione eseguirà automaticamente il mapping delle colonne con lo stesso nome e lo stesso tipo di dati.

1. Selezionare **Stima**.

Azure Data Studio creerà una nuova query T-SQL con l'istruzione [PREDICT](../../t-sql/queries/predict-transact-sql.md), utilizzabile per eseguire stime sui dati.

## <a name="next-steps"></a>Passaggi successivi

- [Estensione Machine Learning in Azure Data Studio](machine-learning-extension.md)
- [Gestire i pacchetti nel database](machine-learning-extension-manage-packages.md)
- [Importare o visualizzare modelli](machine-learning-extension-import-view-models.md)
- [Notebook in Azure Data Studio](../notebooks/notebooks-guidance.md)
- [Documentazione per Machine Learning in SQL](../../machine-learning/index.yml)
- [Machine Learning Services in Istanza gestita di SQL di Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview)
- [Machine Learning e intelligenza artificiale con ONNX in SQL Edge (anteprima)](/azure/azure-sql-edge/onnx-overview)