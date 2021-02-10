---
title: Valutare il livello di accesso ai dati di un'applicazione con Data Migration Assistant
description: Informazioni su come usare Data Migration Assistant per valutare il livello di accesso ai dati di un'applicazione.
ms.date: 01/23/2020
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, Assess
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.custom: seo-lt-2019
ms.openlocfilehash: f56976c9140cb4ac18c048d7d57d6949fc4ae74d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065160"
---
# <a name="assess-an-apps-data-access-layer-with-data-migration-assistant"></a>Valutare il livello di accesso ai dati di un'app con Data Migration Assistant

Le applicazioni in genere si connettono e mantengono i dati in un database. Il livello di accesso ai dati dell'applicazione fornisce l'accesso semplificato a questi dati. Data Migration Assistant (DMA) ha consentito agli utenti di valutare i database e gli oggetti correlati. La versione più recente di DMA (v 5.0) introduce il supporto per l'analisi della connettività del database e delle query SQL incorporate nel codice dell'applicazione.

Si consideri il segmento di codice C# seguente:

![Segmento di codice C# di esempio](../dma/media/dma-assess-app-data-layer/dma-sample-c-sharp-code-segment.png)

In questo caso, è possibile osservare che l'applicazione usa una query SQL per ottenere il nome di un dipendente.

![Dettagli del segmento di codice C# di esempio](../dma/media/dma-assess-app-data-layer/dma-sample-c-sharp-code-detail.png)

Il proprietario di un'applicazione deve essere in grado di identificare i diversi database a cui l'applicazione può connettersi e le query incorporate nel livello di accesso ai dati dell'applicazione. Inoltre, è necessario identificare le modifiche necessarie per modernizzare l'applicazione in servizi dati di Azure.

## <a name="assess-an-app-with-data-access-migration-toolkit"></a>Valutare un'app con Data Access Migration Toolkit

Per abilitare questa valutazione, è stata introdotta di recente il Data Access Migration Toolkit (DEMT), un'estensione Visual Studio Code. La versione più recente di questa estensione (v 0,2) aggiunge il supporto per le applicazioni .NET e il dialetto T-SQL.

1. Scaricare e installare VS Code da [qui](https://code.visualstudio.com/download).
2. Abilitare l' [estensione Data Access Migration Toolkit](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit) dal Marketplace delle estensioni.

   ![Pagina di estensione di Data Access Migration Toolkit](../dma/media/dma-assess-app-data-layer/dma-damt-extension-page.png)

3. Aprire il progetto dell'applicazione in Visual Studio Code.

   ![Progetto di applicazione in Visual Studio Code](../dma/media/dma-assess-app-data-layer/dma-app-project-in-vscode.png)

4. Avviare la console di estensione (CTRL-MAIUSC-P), quindi eseguire il comando di **accesso ai dati: analizza area di lavoro** .

   ![Console di estensione in Visual Studio Code](../dma/media/dma-assess-app-data-layer/dma-vscode-extension-console.png)

5. Selezionare il dialetto SQL Server.

   ![Selezione dialetto SQL Server](../dma/media/dma-assess-app-data-layer/dma-sql-server-dialect.png)

   Al termine dell'analisi, il comando genera un report di query e comandi di connettività SQL.

   ![Report di accesso ai dati](../dma/media/dma-assess-app-data-layer/dma-data-access-report.png)

6. Esaminare il report per i componenti di connettività dei dati e per le query SQL incorporate nel codice dell'applicazione, che vengono visualizzate evidenziate.

   ![Query SQL nel codice dell'applicazione](../dma/media/dma-assess-app-data-layer/dma-sql-queries-in-app-code.png)

   Queste query possono essere analizzate tramite DMA per la compatibilità e i problemi di parità delle funzionalità in base alla piattaforma SQL di destinazione.

7. Per valutare il livello dati dell'applicazione, esportare il report come file JSON.

   ![esportazione di file con estensione JSON](../dma/media/dma-assess-app-data-layer/dma-json-file-export.png)

   In questo caso, il file generato è:

   ![Visualizza il contenuto del file JSON](../dma/media/dma-assess-app-data-layer/dma-json-file-contents.png)

   Data Migration Assistant consente la valutazione delle query identificate nell'applicazione nel contesto della modernizzazione del database alla piattaforma dati di Azure.

8. Avviare Data Migration Assistant, quindi creare un progetto di valutazione.

   ![Data Migration Assistant nuovo progetto di valutazione](../dma/media/dma-assess-app-data-layer/dma-new-assessment-project.png)

9. Selezionare l'istanza di SQL Server di origine.

   ![Data Migration Assistant selezionare SQL Server origine](../dma/media/dma-assess-app-data-layer/dma-select-sql-source.png)

10. Consente di selezionare il database a cui è connessa l'applicazione.

    ![Migrazione di accesso ai dati selezionare il database dell'applicazione](../dma/media/dma-assess-app-data-layer/dma-select-app-database.png)

    Per semplificare la valutazione dell'accesso ai dati, DMA introduce la possibilità di includere file JSON con query dell'applicazione. Si includerà quindi il file JSON creato in precedenza con le query dell'applicazione.

11. Selezionare il database e passare al file JSON esportato da Data Access Migration Toolkit per includere le query dell'applicazione per la valutazione.

    ![Data Migration Assistant aprire il file JSON DMAT](../dma/media/dma-assess-app-data-layer/dma-open-damt-json-file.png)

12. Avviare la valutazione.

    ![Data Migration Assistant Avvia valutazione](../dma/media/dma-assess-app-data-layer/dma-start-assessment.png)

13. Esaminare il report di valutazione. Il report generato includerà eventuali problemi di compatibilità o di parità delle funzionalità rilevati nelle query dell'applicazione come illustrato di seguito.

    ![Report di valutazione Data Migration Assistant](../dma/media/dma-assess-app-data-layer/dma-assessment-report.png)

Ora, oltre a avere la prospettiva del database della migrazione, gli utenti hanno anche una visualizzazione dal punto di vista dell'applicazione.

## <a name="see-also"></a>Vedi anche

* [Panoramica di Data Migration Assistant](../dma/dma-overview.md)
* [Data Migration Assistant: impostazioni di configurazione](../dma/dma-configurationsettings.md)
* [Data Migration Assistant: procedure consigliate](../dma/dma-bestpractices.md)
* [Toolkit per la migrazione dell'accesso ai dati](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)