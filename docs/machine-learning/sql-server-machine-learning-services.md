---
title: Che cos'è Machine Learning Services per SQL Server (Python e R)?
titleSuffix: ''
description: Machine Learning Services è una funzionalità di SQL Server che offre la possibilità di eseguire script Python e R con dati relazionali. È possibile usare pacchetti e framework open source, oltre ai pacchetti Python e R di Microsoft per l'analisi predittiva e le funzioni di Machine Learning. Gli script vengono eseguiti nel database senza trasferire i dati all'esterno di SQL Server o in rete. Questo articolo illustra le nozioni di base di Machine Learning Services per SQL Server e spiega come iniziare a usare questa funzionalità.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/10/2020
ms.topic: overview
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 98e9dd928f2639bfd1df3e6782bcc9aaed60f31e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102869"
---
# <a name="what-is-sql-server-machine-learning-services-with-python-and-r"></a>Che cos'è Machine Learning Services per SQL Server con Python e R?
[!INCLUDE [SQL Server 2017 SQL MI](../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Machine Learning Services è una funzionalità di SQL Server che offre la possibilità di eseguire script Python e R con dati relazionali. È possibile usare pacchetti e framework open source, oltre ai [pacchetti Python e R di Microsoft](#packages), per l'analisi predittiva e le funzioni di Machine Learning. Gli script vengono eseguiti nel database senza trasferire i dati all'esterno di SQL Server o in rete. Questo articolo illustra le nozioni di base di Machine Learning Services per SQL Server e spiega come iniziare a usare questa funzionalità.

Per le funzioni di Machine Learning su altre piattaforme SQL, vedere la [documentazione per Machine Learning in SQL](index.yml).

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!NOTE]
> Per l'esecuzione di Java in SQL Server, vedere la [documentazione dell'estensione del linguaggio Java](../language-extensions/java-overview.md).
::: moniker-end

## <a name="execute-python-and-r-scripts-in-sql-server"></a>Eseguire script Python e R in SQL Server

Machine Learning Services per SQL Server consente di eseguire script Python e R nel database. È possibile usare questa funzionalità per preparare e pulire i dati, eseguire la progettazione delle caratteristiche e il training, la valutazione e la distribuzione di modelli di Machine Learning all'interno di un database. La funzionalità esegue gli script nella posizione in cui i dati risiedono, eliminando la necessità di trasferire i dati in rete in un altro server.

È possibile eseguire script Python e R in un'istanza di SQL Server con la stored procedure [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

Le distribuzioni di base di Python e R sono incluse in Machine Learning Services. È possibile installare e usare pacchetti e framework open source, come PyTorch, TensorFlow e scikit-learn, oltre ai pacchetti Microsoft.

Machine Learning Services usa un framework di estendibilità per eseguire gli script Python e R in SQL Server. Altre informazioni sono disponibili in:

+ [Framework di estendibilità](concepts/extensibility-framework.md)
+ [Estensione di Python](concepts/extension-python.md)
+ [Estensione di R](concepts/extension-r.md)

## <a name="get-started-with-machine-learning-services"></a>Introduzione a Machine Learning Services

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
1. [Installare Machine Learning Services per SQL Server in Windows](install/sql-machine-learning-services-windows-install.md) o [in Linux](../linux/sql-server-linux-setup-machine-learning.md?toc=/sql/machine-learning/toc.json). È anche possibile usare [Machine Learning Services nei cluster Big Data](../big-data-cluster/machine-learning-services.md) e [Machine Learning Services in Istanza gestita di SQL di Azure ](/azure/azure-sql/managed-instance/machine-learning-services-overview).

1. Configurare gli strumenti di sviluppo. È possibile [eseguire script Python e R nei notebook di Azure Data Studio](install/sql-machine-learning-azure-data-studio.md). È anche possibile eseguire T-SQL in [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md).

1. Scrivere il primo script Python o R.

   + [Esercitazioni di Python per Machine Learning in SQL](tutorials/python-tutorials.md)
   + [Esercitazioni di R per Machine Learning in SQL](tutorials/r-tutorials.md)
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
+ Scrivere il primo script Python o R.

   + [Esercitazioni di Python per Machine Learning in SQL](tutorials/python-tutorials.md)
   + [Esercitazioni di R per Machine Learning in SQL](tutorials/r-tutorials.md)
::: moniker-end

::: moniker range="=sql-server-2017"
1. [Installare Machine Learning Services per SQL Server in Windows](install/sql-machine-learning-services-windows-install.md).

1. Configurare gli strumenti di sviluppo. È possibile [eseguire script Python e R nei notebook di Azure Data Studio](install/sql-machine-learning-azure-data-studio.md). È anche possibile usare T-SQL in [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md).

1. Scrivere il primo script Python o R.

   + [Esercitazioni di Python per Machine Learning in SQL](tutorials/python-tutorials.md)
   + [Esercitazioni di R per Machine Learning in SQL](tutorials/r-tutorials.md)
::: moniker-end

<a name="versions"></a>

## <a name="python-and-r-versions"></a>Versioni di Python e R

Di seguito sono elencate le versioni di Python e R incluse in Machine Learning Services.

| Versione di SQL Server | Versione Python | Versione di R |
|-|-|-|
| SQL Server 2017 | 3.5.2 | 3.3.3 |
| SQL Server 2019 | 3.7.3 | 3.5.2 |

Per la versione di R in SQL Server 2016, vedere la sezione [Versione di R in Che cos'è R Services?](r/sql-server-r-services.md?view=sql-server-2016&preserve-view=true#version)

<a name="packages"></a>

## <a name="python-and-r-packages"></a>Pacchetti Python e R

È possibile usare pacchetti e framework open source, oltre ai pacchetti aziendali Microsoft. I pacchetti Python e R open source più comuni sono preinstallati in Machine Learning Services. Sono inclusi anche i pacchetti Python e R di Microsoft seguenti:

| Linguaggio | Pacchetto | Descrizione |
|-|-|-|
| Python | [revoscalepy](python/ref-py-revoscalepy.md) | Pacchetto principale per Python scalabile. Trasformazioni e manipolazione di dati, riepilogo statistico, visualizzazione e molte forme di modellazione. Inoltre, le funzioni di questo pacchetto distribuiscono automaticamente i carichi di lavoro tra i core disponibili per l'elaborazione parallela. |
| Python | [microsoftml](python/ref-py-microsoftml.md) | Aggiunge algoritmi di Machine Learning per creare modelli personalizzati per l'analisi del testo, l'analisi delle immagini e l'analisi del sentiment. | 
| R | [RevoScaleR](r/ref-r-revoscaler.md) | Pacchetto principale per R scalabile. Trasformazioni e manipolazione di dati, riepilogo statistico, visualizzazione e molte forme di modellazione. Inoltre, le funzioni di questo pacchetto distribuiscono automaticamente i carichi di lavoro tra i core disponibili per l'elaborazione parallela. |
| R | [MicrosoftML (R)](r/ref-r-microsoftml.md) | Aggiunge algoritmi di Machine Learning per creare modelli personalizzati per l'analisi del testo, l'analisi delle immagini e l'analisi del sentiment. |
| R | [olapR](r/ref-r-olapr.md) | Funzioni R usate per le query MDX su un cubo OLAP di SQL Server Analysis Services. |
| R | [sqlrutils](r/ref-r-sqlrutils.md) | Meccanismo per usare gli script R in una stored procedure T-SQL, registrare la stored procedure con un database ed eseguirla da un [ambiente di sviluppo R](r/set-up-a-data-science-client.md). |
| R | [Microsoft R Open](https://mran.microsoft.com/rro) | Microsoft R Open (MRO) è la distribuzione avanzata di R di Microsoft. Si tratta di una piattaforma open source completa per analisi statistica e data science. Basata su R e compatibile al 100% con questo linguaggio, include funzionalità aggiuntive per migliorare le prestazioni e la riproducibilità. |

Per altre informazioni sui pacchetti installati con Machine Learning Services e su come installare altri pacchetti, vedere:

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
+ [Ottenere informazioni sui pacchetti Python](package-management/python-package-information.md)
+ [Installare pacchetti Python con sqlmlutils](package-management/install-additional-python-packages-on-sql-server.md)
+ [Ottenere informazioni sui pacchetti R](package-management/r-package-information.md)
+ [Installare nuovi pacchetti R con sqlmlutils](package-management/install-additional-r-packages-on-sql-server.md)
::: moniker-end
::: moniker range="=sql-server-2017"
+ [Ottenere informazioni sui pacchetti Python](package-management/python-package-information.md)
+ [Installare i pacchetti con gli strumenti Python in SQL Server](package-management/install-python-packages-standard-tools.md)
+ [Ottenere informazioni sui pacchetti R](package-management/r-package-information.md)
+ [Usare T-SQL (CREATE EXTERNAL LIBRARY) per installare i pacchetti R in SQL Server](package-management/install-r-packages-with-tsql.md)
::: moniker-end

## <a name="next-steps"></a>Passaggi successivi

+ [Installare Machine Learning Services per SQL Server in Windows](install/sql-machine-learning-services-windows-install.md) o [in Linux](../linux/sql-server-linux-setup-machine-learning.md?toc=/sql/machine-learning/toc.json)
+ [Esercitazioni di Python per Machine Learning in SQL](tutorials/python-tutorials.md)
+ [Esercitazioni di R per Machine Learning in SQL](tutorials/r-tutorials.md)