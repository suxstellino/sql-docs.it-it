---
title: Estensione SQL Server Agent
description: Informazioni su come installare e usare l'estensione SQL Server Agent per Azure Data Studio, un'estensione per la gestione dei processi e delle configurazioni di SQL Agent.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: 6bec78ab2c79af6f6a4dca70b25345f567def48b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040271"
---
# <a name="sql-server-agent-extension-preview"></a>Estensione SQL Server Agent (anteprima)

L'estensione SQL Server Agent (anteprima) è un'estensione per la gestione e la risoluzione di problemi dei processi e della configurazione di SQL Agent. Questa estensione è attualmente disponibile in anteprima.

Le azioni principali comprendono:

- Elencare i processi di SQL Server Agent configurati in SQL Server
- Visualizzare la cronologia dei processi con i risultati della loro esecuzione
- Avviare e arrestare i processi tramite il controllo di base dei processi

## <a name="install-the-sql-server-agent-extension"></a>Installare l'estensione SQL Server Agent

1. Per aprire Gestione estensioni e accedere alle estensioni disponibili, selezionare l'icona delle estensioni oppure l'opzione **Estensioni** dal menu **Visualizza**.
2. Selezionare un'estensione disponibile per visualizzarne i dettagli.

   ![Installare Agent](media/sql-server-agent-extension/install-sql-agent.png)

3. Selezionare l'estensione desiderata e **installarla**.
4. Selezionare **Ricarica** per abilitare l'estensione (necessario solo la prima volta che si installa un'estensione).
5. Passare al dashboard di gestione facendo clic con il pulsante destro del mouse sul server o sul database in uso e scegliendo quindi **Gestisci**.
6. Le estensioni installate vengono visualizzate come schede nel dashboard di gestione:

   ![Visualizzare Agent](media/sql-server-agent-extension/view-sql-agent.png)

## <a name="view-jobs"></a>Visualizzare i processi

Quando ci si connette all'estensione SQL Server Agent, il primo elemento visualizzato è un elenco di tutti i processi di Agent.

   ![Visualizzare i processi](media/sql-server-agent-extension/job-view.png)

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su SQL Server Agent, [consultare la documentazione di riferimento](../../ssms/agent/sql-server-agent.md).
