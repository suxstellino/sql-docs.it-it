---
title: Monitorare gli script con report personalizzati
description: Usare report personalizzati in SQL Server Management Studio (SSMS) per monitorare l'esecuzione di script esterni (Python ed R) e le risorse usate, diagnosticare i problemi e ottimizzare le prestazioni in Machine Learning Services per SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/14/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: e0c3874177691832bd6fa66f81c3fe94c726ff76
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337072"
---
# <a name="monitor-python-and-r-script-execution-using-custom-reports-in-sql-server-management-studio"></a>Monitorare l'esecuzione di script Python ed R tramite report personalizzati in SQL Server Management Studio
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Usare report personalizzati in [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) per monitorare l'esecuzione di script esterni (Python ed R) e le risorse usate, diagnosticare i problemi e ottimizzare le prestazioni in [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md).

In questi report è possibile visualizzare dettagli quali:

- Sessioni Python o R attive
- Impostazioni di configurazione per l'istanza
- Statistiche di esecuzione per i processi di Machine Learning
- Eventi estesi per R Services
- Pacchetti di Python o R installati nell'istanza corrente

Questo articolo spiega come installare e usare i report personalizzati disponibili per Machine Learning Services per SQL Server.

Per altre informazioni sui report in SQL Server Management Studio, vedere [Report personalizzati in Management Studio](../../ssms/object/custom-reports-in-management-studio.md).

## <a name="how-to-install-the-reports"></a>Come installare i report

I report vengono progettati usando SQL Server Reporting Services, ma possono essere usati direttamente da SQL Server Management Studio. Non è necessario che Reporting Services sia installato nell'istanza di SQL Server.

Per usare questi report, seguire questa procedura:

1. Scaricare i [report personalizzati di SSMS](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/machine-learning-services/ssms-custom-reports) per Machine Learning Services per SQL Server da GitHub.

   ::: moniker range="=azuresqldb-mi-current"
   >[!NOTE]
   > Il report personalizzato **Machine Learning Services - Configure Instance** (Configura istanza) non è supportato in Istanza gestita di SQL di Azure.
   ::: moniker-end

2. Copiare i report in Management Studio

    1. Individuare la cartella dei report personalizzati usata da SQL Server Management Studio. Per impostazione predefinita, i report personalizzati vengono archiviati in questa cartella (dove **user_name** corrisponde al nome utente Windows):

        `C:\Users\user_name\Documents\SQL Server Management Studio\Custom Reports`

       È anche possibile specificare una cartella diversa oppure creare sottocartelle.

    2. Copiare i file con estensione RDL scaricati in precedenza nella cartella dei report personalizzati.

3. Eseguire i report in Management Studio

    1. In Management Studio fare doppio clic sul nodo **Database** per l'istanza in cui si vogliono eseguire i report.

    2. Fare clic su **Report** e quindi su **Report personalizzati**.

    3. Nella finestra di dialogo **Apri file** individuare la cartella dei report personalizzati.

    4. Selezionare uno dei file RDL scaricati e fare clic su **Apri**.

## <a name="reports"></a>Report

Il [repository dei report di SSMS personalizzati in GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/machine-learning-services/ssms-custom-reports) include i report seguenti:

| Report | Descrizione |
|-|-|
| Sessioni attive | Gli utenti connessi all'istanza di SQL Server che stanno eseguendo uno script Python o R. |
| Configurazione | Impostazioni di installazione di Machine Learning Services e proprietà del runtime Python o R. |
| Configure Instance (Configura istanza) | Configurazione di Machine Learning Services. |
| Execution Statistics (Statistiche di esecuzione) | Statistiche di esecuzione di Machine Learning Services. È ad esempio possibile ottenere il numero totale di esecuzioni di script esterni e il numero di esecuzioni parallele. |
| Eventi estesi | Eventi estesi disponibili per ottenere altre informazioni dettagliate sull'esecuzione di script esterni. |
| Pacchetti | Creazione dell'elenco dei pacchetti R o Python installati nell'istanza di SQL Server e delle relative proprietà, ad esempio la versione e il nome. |
| Resource Usage | Visualizzazione della CPU, della memoria, del consumo di I/O di SQL Server e dell'esecuzione di script esterni. È anche possibile visualizzare l'impostazione relativa alla memoria per i pool di risorse esterni. |

## <a name="next-steps"></a>Passaggi successivi

- [Monitorare Machine Learning Services per SQL Server tramite DMV](monitor-sql-server-machine-learning-services-using-dynamic-management-views.md)
- [Monitorare gli script Python e R con eventi estesi in Machine Learning Services per SQL Server](extended-events.md)
