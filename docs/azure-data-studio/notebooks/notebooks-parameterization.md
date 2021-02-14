---
title: Parametrizzazione dei notebook in Azure Data Studio
description: Questa esercitazione illustra come creare un notebook con parametri in ADS.
ms.topic: how-to
ms.prod: azure-data-studio
ms.technology: azure-data-studio
author: vasubhog
ms.author: vasubhog
ms.reviewer: mikeray, alayu, maghan
ms.custom: ''
ms.date: 01/25/2021
ms.openlocfilehash: dc0558d660143de35340010735c74e9abc0cdd56
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343459"
---
# <a name="create-a-parameterized-notebook"></a>Creare un notebook con parametri

La **parametrizzazione** è la possibilità di eseguire lo stesso notebook con parametri diversi.

Questo articolo illustra come creare ed eseguire un notebook con parametri in Azure Data Studio usando il kernel Python.

## <a name="prerequisites"></a>Prerequisiti

- [Azure Data Studio](../download-azure-data-studio.md)
- [Python](https://www.python.org/downloads/)

## <a name="install-and-set-up-papermill-in-azure-data-studio"></a>Installare e configurare Papermill in Azure Data Studio

I passaggi descritti in questa sezione vengono tutti eseguiti in un notebook di Azure Data Studio.

1. Creare un nuovo notebook e impostare **Kernel** su *Python 3*.

   ![Nuovo notebook](media/notebooks-kqlmagic/install-new-notebook.png)

2. È possibile che venga richiesto di aggiornare i pacchetti Python quando è necessario aggiornare i pacchetti.

   ![Sì](media/notebooks-kqlmagic/install-python-yes.png)

3. Installare Papermill:

   ```python
   import sys
   !{sys.executable} -m pip install papermill --no-cache-dir --upgrade
   ```

   Verificare che sia installato:

   ```python
   import sys
   !{sys.executable} -m pip list
   ```

   :::image type="content" source="media/notebooks-parameterization/install-list-papermill.png" alt-text="Elenco":::

5. È possibile verificare se Papermill è stato caricato correttamente controllando la versione di Papermill.

   ```python
   import papermill
   papermill
   ```

   :::image type="content" source="media/notebooks-parameterization/install-validation-papermill.png" alt-text="Convalida":::

## <a name="set-up-a-parameterized-notebook"></a>Configurare un notebook con parametri

1. Verificare che **Kernel** sia impostato su *Python3*.

   ![Modifica del kernel](media/notebooks-kqlmagic/change-kernel.png)

2. Crea una nuova cella di codice e un tag come **cella parametri**.

   ```python
   x = 2.0
   y = 5.0
   ```

   :::image type="content" source="media/notebooks-parameterization/make-parameter-cell.png" alt-text="Notebook cella parametro":::

3. Aggiungere altre celle per testare parametri diversi.

   ```python
   addition = x + y
   multiply = x * y
   ```

   ```python
   print("Addition: " + str(addition))
   print("Multiplication: " + str(multiply))
   ```

   Celle nel notebook di input di esempio: :::image type="content" source="media/notebooks-parameterization/test-cells.png" alt-text="celle del notebook di input aggiuntive":::

4. Salvare notebook come **input. ipynb**.
   :::image type="content" source="media/notebooks-parameterization/save-notebook.png" alt-text="Salva notebook":::

## <a name="how-to-execute-papermill-notebook"></a>Come eseguire Papermill notebook

Papermill può essere eseguito in due modi:

- Interfaccia della riga di comando
- API Python

### <a name="parameterized-cli-execution"></a>Esecuzione CLI con parametri

Per eseguire un notebook usando l'interfaccia della riga di comando, immettere il comando Papermill nel terminale con il notebook di input, il percorso per il notebook di output e le opzioni.

> [!Note]
   > La documentazione dell'interfaccia della riga di comando Papermill è disponibile [qui](https://papermill.readthedocs.io/en/latest/usage-execute.html#execute-via-cli).

1. Eseguire il notebook di input con nuovi parametri.

   ```shell
   papermill Input.ipynb Output.ipynb -p x 10 -p y 20
   ```

   Verrà eseguito il notebook di input con nuovi valori per i parametri **x** e **y**.

2. Dopo l'esecuzione, visualizzare nuovo notebook con parametri di output.
   È possibile notare che è presente una nuova cella con etichetta **# injected-Parameters** contenente i nuovi valori di parametro passati tramite l'interfaccia della riga di comando.

   :::image type="content" source="media/notebooks-parameterization/output-notebook.png" alt-text="Notebook di output":::

### <a name="parameterized-python-api-execution"></a>Esecuzione di API Python con parametri

> [!Note]
   > La documentazione dell'API Python di Papermill è disponibile [qui](https://papermill.readthedocs.io/en/latest/usage-execute.html#execute-via-the-python-api).

1. Creare un nuovo notebook e impostare **Kernel** su *Python 3*.
   ![Nuovo notebook](media/notebooks-kqlmagic/install-new-notebook.png)

2. Aggiungere una nuova cella di codice e usare Papermill per usare il metodo Execute.

   ```python
   import papermill as pm

   pm.execute_notebook(
   '/Users/vasubhog/GitProjects/AzureDataStudio-Notebooks/Demo_Parameterization/Input.ipynb',
   '/Users/vasubhog/GitProjects/AzureDataStudio-Notebooks/Demo_Parameterization/Output.ipynb',
   parameters = dict(x = 10, y = 20)
   )
   ```

   ![Esecuzione dell'API Python di Papermill](media/notebooks-parameterization/python-api-execute.png)

3. Dopo l'esecuzione, visualizzare nuovo notebook di parametrizzazione output.

   È possibile notare che è presente una nuova cella con etichetta **# injected-Parameters** contenente i nuovi valori di parametro passati tramite l'interfaccia della riga di comando.

   :::image type="content" source="media/notebooks-parameterization/output-notebook.png" alt-text="Notebook di output":::

## <a name="next-steps"></a>Passaggi successivi

Altre informazioni sui notebook e sulla parametrizzazione:

- [Come usare i notebook in Azure Data Studio](./notebooks-guidance.md)
- [Documentazione di parametrizzazione Papermill](https://papermill.readthedocs.io/en/latest/index.html)