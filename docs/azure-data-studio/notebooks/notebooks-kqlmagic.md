---
title: Notebook con Kqlmagic (Kusto Query Language) in Azure Data Studio
description: Questa esercitazione illustra come è possibile creare ed eseguire Kqlmagic in Azure Data Studio.
ms.topic: how-to
ms.prod: azure-data-studio
ms.technology: azure-data-studio
author: markingmyname
ms.author: maghan
ms.reviewer: jukoesma
ms.custom: ''
ms.date: 10/29/2020
ms.openlocfilehash: cd9145bb01350a2019f1bfaa12c5280151c35f49
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091745"
---
# <a name="kqlmagic-in-azure-data-studio"></a>Kqlmagic in Azure Data Studio

**Kqlmagic** è un comando che estende le funzionalità del kernel Python in **[notebook di Azure Data Studio](./notebooks-guidance.md)** . È possibile combinare Python e il **[linguaggio di query Kusto (KQL)](/azure/data-explorer/kusto/query)** per eseguire query e visualizzare i dati tramite la libreria avanzata Plot.ly integrata con i comandi `render`. Kqlmagic riunisce i vantaggi di notebook, analisi dei dati e funzionalità avanzate di Python nella stessa posizione. Le origini dati supportate con Kqlmagic includono **[Esplora dati di Azure](/azure/data-explorer/data-explorer-overview)** , **[Application Insights](/azure/azure-monitor/app/app-insights-overview)** e i **[log di Monitoraggio di Azure](/azure/azure-monitor/platform/data-platform-logs)** .

Questo articolo illustra come creare ed eseguire un notebook in Azure Data Studio usando l'estensione Kqlmagic per un cluster di Esplora dati di Azure, un log di Application Insights e i log di Monitoraggio di Azure.

## <a name="prerequisites"></a>Prerequisiti

- [Azure Data Studio](../download-azure-data-studio.md)
- [Python](https://www.python.org/downloads/)

## <a name="install-and-set-up-kqlmagic-in-a-notebook"></a>Installare e configurare Kqlmagic in un notebook

I passaggi descritti in questa sezione vengono tutti eseguiti in un notebook di Azure Data Studio.

1. Creare un nuovo notebook e impostare **Kernel** su *Python 3*.

   ![Nuovo notebook](media/notebooks-kqlmagic/install-new-notebook.png)

2. Quando viene richiesto, selezionare **Sì** per aggiornare i pacchetti Python.

   ![Sì](media/notebooks-kqlmagic/install-python-yes.png)

3. Installare Kqlmagic:

   ```python
   !pip install Kqlmagic --no-cache-dir --upgrade
   ```

   Verificare che sia installato:

   ```python
   !pip list
   ```

   ![Elenco](media/notebooks-kqlmagic/install-list.png)

4. Caricare Kqlmagic:

   ```python
   %reload_ext Kqlmagic
   ```

   > [!Note]
   > Se questo passaggio ha esito negativo, chiudere il file e riaprirlo.

   ![Caricare l'estensione Kqlmagic](media/notebooks-kqlmagic/install-load-kql-magic-ext.png)

5. È possibile verificare se Kqlmagic è stato caricato correttamente visualizzando la documentazione della Guida o controllando la versione.

   ```python
   %kql --help "help"
   ```

   > [!Note]
   > Se `Samples@help` richiede una password, è possibile lasciarla vuota e premere **INVIO**.

   ![Guida](media/notebooks-kqlmagic/install-help.png)

   Per individuare la versione di Kqlmagic installata, eseguire il comando seguente.

   ```python
   %kql --version
   ```

## <a name="kqlmagic-with-an-azure-data-explorer-cluster"></a>Kqlmagic con un cluster di Esplora dati di Azure

Questa sezione illustra come eseguire un'analisi dei dati usando Kqlmagic con un cluster di Esplora dati di Azure.

### <a name="load-and-authenticate-kqlmagic-for-azure-data-explorer"></a><a name="ade-load-auth"></a> Caricare e autenticare Kqlmagic per Esplora dati di Azure

   > [!Note]
   > Ogni volta che si crea un nuovo notebook in Azure Data Studio è necessario caricare l'estensione Kqlmagic.

1. Verificare che **Kernel** sia impostato su *Python3*.

   ![Modifica del kernel](media/notebooks-kqlmagic/change-kernel.png)

2. Caricare Kqlmagic:

   ```python
   %reload_ext Kqlmagic
   ```

   ![Caricare l'estensione Kqlmagic](media/notebooks-kqlmagic/install-load-kql-magic-ext.png)

3. Connettersi al cluster ed eseguire l'autenticazione:

   ```python
   %kql azureDataExplorer://code;cluster='help';database='Samples'
   ```

    > [!Note]
    > Se si usa il proprio cluster ADX, è necessario includere l'area nella stringa di connessione, come indicato di seguito:   
    ```%kql azuredataexplorer://code;cluster='mycluster.westus';database='mykustodb'```

   Usare l'account di accesso del dispositivo per l'autenticazione. Copiare il codice dall'output e selezionare **authenticate** per aprire un browser in cui è necessario incollare il codice. Una volta eseguita l'autenticazione, è possibile tornare ad Azure Data Studio per continuare con il resto dello script.

   ![Autenticazione in Esplora dati di Azure](media/notebooks-kqlmagic/ade-auth.png)

### <a name="query-and-visualize-for-azure-data-explorer"></a>Eseguire query e visualizzare dati per Esplora dati di Azure

Eseguire la query dei dati usando l'[operatore di rendering](/azure/data-explorer/kusto/query/renderoperator) e visualizzare i dati tramite la libreria ploy.ly. La query e la visualizzazione forniscono un'esperienza integrata che fa uso di KQL (linguaggio di query Kusto) nativo.

1. Analizzare i primi 10 eventi tempesta (StormEvents) per stato e frequenza:

   ```python
   %kql StormEvents | summarize count() by State | sort by count_ | limit 10
   ```

   Se si ha familiarità con il linguaggio di query Kusto (KQL), è possibile digitare la query dopo `%kql`.

   ![Analizzare gli eventi tempesta](media/notebooks-kqlmagic/ade-analyze-storm-events.png)

2. Visualizzare un grafico della sequenza temporale:

   ```python
   %kql StormEvents \
   | summarize event_count=count() by bin(StartTime, 1d) \
   | render timechart title= 'Daily Storm Events'
   ```

   ![Visualizzare un diagramma temporale](media/notebooks-kqlmagic/ade-visualize-timechart.png)

3. Esempio di query su più righe con `%%kql`.

   ```python
   %%kql
   StormEvents
   | summarize count() by State
   | sort by count_
   | limit 10
   | render columnchart title='Top 10 States by Storm Event count'
   ```

   ![Esempio di query su più righe](media/notebooks-kqlmagic/ade-multiline-query-sample.png)

## <a name="kqlmagic-with-application-insights"></a>Kqlmagic con Application Insights

### <a name="load-and-authenticate-kqlmagic-for-application-insights"></a><a name="appin-load-auth"></a> Caricare e autenticare Kqlmagic per Application Insights

1. Verificare che **Kernel** sia impostato su *Python3*.

   ![Kernel](media/notebooks-kqlmagic/change-kernel.png)

2. Caricare Kqlmagic:

   ```python
   %reload_ext Kqlmagic
   ```

   ![Caricare l'estensione Kqlmagic](media/notebooks-kqlmagic/install-load-kql-magic-ext.png)

   > [!Note]
   > Ogni volta che si crea un nuovo notebook in Azure Data Studio è necessario caricare l'estensione Kqlmagic.

3. Connettersi ed eseguire l'autenticazione.

   Per prima cosa, è necessario generare una chiave API per la risorsa Application Insights. Usare quindi l'ID applicazione e la chiave API per connettersi ad Application Insights dal notebook:

   ```python
   %kql appinsights://appid='DEMO_APP';appkey='DEMO_KEY'
   ```

### <a name="query-and-visualize-for-application-insights"></a>Eseguire query e visualizzare dati per Application Insights

Eseguire la query dei dati usando l'[operatore di rendering](/azure/data-explorer/kusto/query/renderoperator) e visualizzare i dati tramite la libreria ploy.ly. La query e la visualizzazione forniscono un'esperienza integrata che fa uso di KQL (linguaggio di query Kusto) nativo.

1. Mostrare le visualizzazioni pagina:

   ```python
   %%kql
   pageViews
   | limit 10
   ```

   ![Visualizzazioni pagina](media/notebooks-kqlmagic/appin-page-views.png)

   > [!Note]
   > Usare il mouse per trascinare su un'area del grafico per eseguire lo zoom avanti fino alla data o alle date specifiche.

2. Mostrare le visualizzazioni pagina in un grafico con sequenza temporale:

   ```python
   %%kql
   pageViews
   | summarize event_count=count() by name, bin(timestamp, 1d)
   | render timechart title= 'Daily Page Views'
   ```

   ![Grafico con sequenza temporale](media/notebooks-kqlmagic/appin-timechart.png)

## <a name="kqlmagic-with-azure-monitor-logs"></a>Kqlmagic con i log di Monitoraggio di Azure

### <a name="load-and-authenticate-kqlmagic-for-azure-monitor-logs"></a><a name="aml-load-auth"></a> Caricare e autenticare Kqlmagic per i log di Monitoraggio di Azure

1. Verificare che **Kernel** sia impostato su *Python3*.

   ![Modifica](media/notebooks-kqlmagic/change-kernel.png)

2. Caricare Kqlmagic:

   ```python
   %reload_ext Kqlmagic
   ```

   ![Caricare l'estensione Kqlmagic](media/notebooks-kqlmagic/install-load-kql-magic-ext.png)

   > [!Note]
   > Ogni volta che si crea un nuovo notebook in Azure Data Studio è necessario caricare l'estensione Kqlmagic.

3. Connettersi ed eseguire l'autenticazione:

   ```python
   %kql loganalytics://workspace='DEMO_WORKSPACE';appkey='DEMO_KEY';alias='myworkspace'
   ```

   ![Autenticazione di Log Analytics](media/notebooks-kqlmagic/aml-auth.png)

### <a name="query-and-visualize-for-azure-monitor-logs"></a>Eseguire query e visualizzare dati per i log di Monitoraggio di Azure

Eseguire la query dei dati usando l'[operatore di rendering](/azure/data-explorer/kusto/query/renderoperator) e visualizzare i dati tramite la libreria ploy.ly. La query e la visualizzazione forniscono un'esperienza integrata che fa uso di KQL (linguaggio di query Kusto) nativo.

1. Visualizzare un grafico con sequenza temporale:

   ```python
   %%kql
   KubeNodeInventory
   | summarize event_count=count() by Status, bin(TimeGenerated, 1d)
   | render timechart title= 'Daily Kubernetes Nodes'
   ```

   ![Diagramma temporale dei nodi Kubernetes giornalieri di Log Analytics](media/notebooks-kqlmagic/aml-timechart-daily-kubernetes-nodes.png)

## <a name="next-steps"></a>Passaggi successivi

Altre informazioni sui notebook e Kqlmagic:

- [Estensione Kusto (KQL) per Azure Data Studio (anteprima)](../extensions/kusto-extension.md)
- [Creare ed eseguire un notebook Kusto (KQL) (anteprima)](./notebooks-kusto-kernel.md)
- [Usare Jupyter Notebook e l'estensione Kqlmagic per analizzare i dati in Esplora dati di Azure](/azure/data-explorer/Kqlmagic)
- [Extension (Magic) to Jupyter Notebook and Jupyter lab](https://github.com/Microsoft/jupyter-Kqlmagic) (Estensione Magic per Jupyter Notebook e lab Jupyter) che abilita l'esperienza dei notebook usando dati Kusto, Application Insights e Log Analytics.
- [Kqlmagic](https://pypi.org/project/Kqlmagic/)
- [Come usare i notebook in Azure Data Studio](./notebooks-guidance.md)