---
title: Esercitazioni di R
titleSuffix: SQL machine learning
description: Questo articolo descrive le esercitazioni di R per Machine Learning in SQL. Informazioni su come eseguire gli script e creare modelli di Machine Learning.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 8b463ded43f7906627e08d86eb1809b169e6125d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346242"
---
# <a name="r-tutorials-for-sql-machine-learning"></a>Esercitazioni di R per Machine Learning in SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Questo articolo descrive le esercitazioni e gli argomenti di avvio rapido di R per [Machine Learning Services in SQL Server](../sql-server-machine-learning-services.md) e in [cluster Big Data](../../big-data-cluster/machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2017"
Questo articolo descrive le esercitazioni e gli argomenti di avvio rapido di R per [Machine Learning Services per SQL Server](../sql-server-machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2016"
Questo articolo descrive le esercitazioni e gli argomenti di avvio rapido di R per [R Services per SQL Server 2016](../r/sql-server-r-services.md).
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Questo articolo descrive le esercitazioni e gli argomenti di avvio rapido di Python per [Machine Learning Services per Istanza gestita di SQL di Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview).
::: moniker-end

<a name="bkmk_sqltutorials"></a>

## <a name="r-tutorials"></a>Esercitazioni di R

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"
| Esercitazione | Descrizione |
|------|-------------|
| [Stimare il noleggio di sci con l'albero delle decisioni](r-predictive-model-introduction.md) | Usare R e un modello di albero delle decisioni per stimare il numero di noleggi di sci in futuro. Usare notebook in Azure Data Studio per preparare i dati ed eseguire il training del modello e T-SQL per distribuire il modello. |
| [Categorizzazione dei clienti tramite clustering K-means](r-clustering-model-introduction.md) | Usare R per sviluppare e distribuire un modello di clustering K-Means per categorizzare i clienti. Usare notebook in Azure Data Studio per preparare i dati ed eseguire il training del modello e T-SQL per distribuire il modello. |
| [Analisi R nel database per data scientist](../tutorials/walkthrough-data-science-end-to-end-walkthrough.md) | Per gli sviluppatori R che non hanno familiarità con l'uso di Machine Learning in SQL, questa esercitazione illustra come eseguire le attività comuni di data science in SQL. Caricare e visualizzare dati, eseguire il training e il salvataggio di un modello in un database e usare il modello per l'analisi predittiva. |
| [Analisi R nel database per sviluppatori SQL](../tutorials/r-taxi-classification-introduction.md) | Creare e distribuire una soluzione R completa usando solo strumenti SQL. Descrive nel dettaglio il passaggio di una soluzione all'ambiente di produzione. Si apprenderà come eseguire il wrapping del codice R in una stored procedure, come salvare un modello R in un database e come effettuare chiamate con parametri al modello R per l'esecuzione di stime. |
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
| Esercitazione | Descrizione |
|------|-------------|
| [Stimare il noleggio di sci con l'albero delle decisioni](r-predictive-model-introduction.md) | Usare R e un modello di albero delle decisioni per stimare il numero di noleggi di sci in futuro. Usare notebook in Azure Data Studio per preparare i dati ed eseguire il training del modello e T-SQL per distribuire il modello. |
| [Categorizzazione dei clienti tramite clustering K-means](r-clustering-model-introduction.md) | Usare R per sviluppare e distribuire un modello di clustering K-Means per categorizzare i clienti. Usare notebook in Azure Data Studio per preparare i dati ed eseguire il training del modello e T-SQL per distribuire il modello. |
::: moniker-end

## <a name="r-quickstarts"></a>Argomenti di avvio rapido di R

Se non si ha familiarità con Machine Learning in SQL, è possibile provare anche gli argomenti di avvio rapido di R.

| Guida introduttiva | Descrizione |
|-|-|
| [Eseguire script R semplici](quickstart-r-create-script.md) | Informazioni di base su come chiamare R in T-SQL usando [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). |
| [Strutture di dati e oggetti con R](quickstart-r-data-types-and-objects.md) | Descrive come SQL usa il linguaggio R per gestire strutture di dati. |
| [Creare e assegnare i punteggi a un modello predittivo in R](quickstart-r-data-types-and-objects.md) | Illustra come creare, sottoporre a training e usare un modello R per eseguire stime in base ai nuovi dati. |

## <a name="next-steps"></a>Passaggi successivi

+ [Estensione Python in SQL Server](../concepts/extension-r.md)
