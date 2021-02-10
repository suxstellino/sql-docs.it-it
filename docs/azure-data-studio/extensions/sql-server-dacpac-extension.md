---
title: Estensione SQL Server dacpac
description: Informazioni su come installare e avviare la procedura guidata dell'applicazione a livello di dati, che semplifica la distribuzione e l'estrazione dei file con estensione DACAPC, nonché l'importazione e l'esportazione di file con estensione BACPAC.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan, sstein
ms.custom: ''
ms.date: 11/04/2019
ms.openlocfilehash: e0cfe057767488eb0a7ddb741934a10410898ffa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040261"
---
# <a name="sql-server-dacpac-extension"></a>Estensione SQL Server dacpac

**La procedura guidata applicazione livello dati** offre un'esperienza di facile utilizzo della procedura guidata per distribuire ed estrarre file dacpac e importare ed esportare file BACPAC.

## <a name="features"></a>Funzionalità

* Distribuzione di un dacpac in un'istanza di SQL Server
* Estrazione di un'istanza di SQL Server in un dacpac
* Creazione di un database da un bacpac
* Esportazione dello schema e dei dati in un bacpac

![gif demo dell'estensione dacpac](media/sql-server-dacpac-extension/dacpac-extension-demo.gif)

## <a name="why-would-i-use-the-data-tier-application-wizard"></a>Perché usare la procedura guidata applicazione livello dati?

La procedura guidata semplifica la gestione dei file dacpac e bacpac e facilita quindi lo sviluppo e la distribuzione di elementi livello dati a supporto dell'applicazione. Per altre informazioni sull'uso di applicazioni livello dati, [vedere la documentazione.](../../relational-databases/data-tier-applications/data-tier-applications.md)

## <a name="install-the-extension"></a>Installare l'estensione

1. Selezionare l'icona Estensioni per visualizzare le estensioni disponibili.

    ![icona Gestione estensioni](media/add-extensions/extension-manager-icon.png)

2. Cercare l'estensione **dacpac di SQL Server** e selezionarla per visualizzarne i dettagli. Selezionare **Installa** per aggiungere l'estensione.

3. Dopo l'installazione, **ricaricare** per abilitare l'estensione in Azure Data Studio (necessario solo quando si installa un'estensione per la prima volta).

## <a name="launch-the-data-tier-application-wizard"></a>Avviare la procedura guidata applicazione livello dati

Per avviare la procedura guidata, fare clic con il pulsante destro del mouse sulla cartella Database o su un database specifico in Esplora oggetti e quindi selezionare **Data-tier Application Wizard** (Procedura guidata applicazione livello dati).

![menu di avvio dell'estensione dacpac](media/sql-server-dacpac-extension/dacpac-extension-launch.png)

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sui dacpac, [vedere la documentazione.](../../relational-databases/data-tier-applications/data-tier-applications.md)
Segnalare eventuali problemi e richieste di funzionalità [qui](https://github.com/microsoft/azuredatastudio/issues).