---
title: Notebook con kernel Python in Azure Data Studio
description: Questa esercitazione illustra come è possibile creare ed eseguire un notebook Python.
ms.topic: how-to
ms.prod: azure-data-studio
ms.technology: azure-data-studio
author: garyericson
ms.author: garye
ms.reviewer: mikeray, alayu, maghan
ms.custom: ''
ms.date: 07/01/2020
ms.openlocfilehash: e1859c5e1af823e067210c5b69247702e8c78f89
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040341"
---
# <a name="create-and-run-a-python-notebook"></a>Creare ed eseguire un notebook Python

[!INCLUDE[SQL Server 2019](../../includes/applies-to-version/sqlserver2019.md)]

Questa esercitazione illustra come creare ed eseguire un notebook in Azure Data Studio usando il kernel Python.

## <a name="prerequisites"></a>Prerequisiti

- [Azure Data Studio installato](../download-azure-data-studio.md)

## <a name="create-a-notebook"></a>Creare un notebook

Questa procedura illustra come creare un file di notebook in Azure Data Studio:

1. Aprire Azure Data Studio. Se viene richiesto di connettersi a un'istanza di SQL Server è possibile connettersi o fare clic su **Annulla**.

1. Scegliere **Nuovo notebook** dal menu **File**.

1. Selezionare **Python 3** per **Kernel**. **Collega a** è impostato su "localhost".

   :::image type="content" source="media/notebooks-python-kernel/set-kernel-and-attach-to-python.png" alt-text="Impostare il kernel":::

È possibile salvare il notebook usando il comando **Salva** o **Salva con nome** del menu **File**.

Per aprire un notebook, è possibile usare il comando **Apri file** nel menu **File**, selezionare **Apri file** nella pagina di **benvenuto** o usare il comando **File: Apri** nel riquadro comandi.

## <a name="change-the-python-kernel"></a>Modificare il kernel Python

La prima volta che ci si connette al kernel Python in un notebook, viene visualizzata la pagina **Configura Python per Notebooks**. È possibile selezionare:

- **Nuova installazione di Python** per installare una nuova copia di Python per Azure Data Studio o
- **Usa l'installazione esistente di Python** per specificare il percorso di un'installazione esistente di Python da usare in Azure Data Studio

Per visualizzare il percorso e la versione del kernel Python attivo, creare una cella di codice ed eseguire i comandi Python seguenti:

```python
import os
import sys
print(sys.version_info)
print(os.path.dirname(sys.executable))
```

Per connettersi a un'installazione diversa di Python:

1. Scegliere **Preferenze** dal menu **File** e quindi **Impostazioni**.
1. Scorrere fino a **Configurazione di Notebook** in **Estensioni**.
1. In **Usa l'installazione esistente di Python** deselezionare l'opzione "Percorso locale di un'installazione preesistente di Python usata da Notebooks."
1. Riavviare Azure Data Studio.

Quando Azure Data Studio viene avviato e ci si connette al kernel Python, viene visualizzata la pagina **Configura Python per Notebooks**. È possibile scegliere di creare una nuova installazione di Python o specificare il percorso di un'installazione esistente.

## <a name="run-a-code-cell"></a>Eseguire una cella di codice

È possibile creare celle contenenti codice SQL che è possibile eseguire sul posto facendo clic sul pulsante **Esegui cella** (la freccia rotonda nera) a sinistra della cella. I risultati verranno visualizzati nel notebook al termine dell'esecuzione della cella.

Ad esempio:

1. Aggiungere una nuova cella di codice Python selezionando il comando **+Codice** sulla barra degli strumenti.

   :::image type="content" source="media/notebooks-python-kernel/notebook-toolbar-python.png" alt-text="Barra degli strumenti del notebook":::

1. Copiare e incollare l'esempio seguente nella cella e fare clic su **Esegui cella**. Questo esempio esegue un'operazione matematica semplice il cui risultato viene visualizzato sotto.

   ```python
   a = 1
   b = 2
   c = a/b
   print(c)
   ```

   :::image type="content" source="media/notebooks-python-kernel/run-notebook-cell-python.png" alt-text="Eseguire la cella del notebook":::

## <a name="next-steps"></a>Passaggi successivi

Altre informazioni sui notebook:

- [Estendere Python con Kqlmagic](./notebooks-kqlmagic.md)
- [Come usare i notebook in Azure Data Studio](./notebooks-guidance.md)
- [Creare ed eseguire un notebook di SQL Server](./notebooks-sql-kernel.md)
