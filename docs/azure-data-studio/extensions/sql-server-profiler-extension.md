---
title: Estensione SQL Server Profiler
description: Informazioni su come installare e usare l'estensione SQL Server Profiler. È una soluzione di analisi SQL Server facile da usare e simile a SSMS Profiler.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: f2a6672f3b73a5be8077229019473758b2bf1e95
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040251"
---
# <a name="sql-server-profiler-extension-preview"></a>Estensione SQL Server Profiler (anteprima)

L'estensione SQL Server Profiler (anteprima) offre una semplice soluzione di analisi per SQL Server con funzionalità analoghe al profiler di SQL Server Management Studio (SSMS), ma realizzate con eventi estesi. SQL Server Profiler è molto facile da usare e offre valori predefiniti per le più comuni configurazioni di analisi. L'esperienza utente è ottimizzata per la ricerca di eventi e la visualizzazione del testo Transact-SQL (T-SQL) associato. SQL Server Profiler per Azure Data Studio prevede anche valori predefiniti per la raccolta di attività di esecuzione di T-SQL tramite un'esperienza utente di facile utilizzo. Questa estensione è attualmente disponibile in anteprima.

**Casi d'uso comuni di SQL Profiler:**

- Esecuzione dei singoli passaggi di query problematiche allo scopo di individuare la causa del problema.
- Individuazione e diagnosi di query con esecuzione rallentata.
- Acquisizione della serie di istruzioni Transact-SQL che ha generato un problema.
- Monitoraggio delle prestazioni di SQL Server per l'ottimizzazione dei carichi di lavoro.
- Correlazione dei contatori delle prestazioni per la diagnosi dei problemi.

## <a name="install-the-sql-server-profiler-extension"></a>Installare l'estensione SQL Server Profiler

1. Per aprire Gestione estensioni e accedere alle estensioni disponibili, selezionare l'icona delle estensioni oppure l'opzione **Estensioni** dal menu **Visualizza**.
2. Selezionare un'estensione disponibile per visualizzarne i dettagli.

    ![Gestione estensioni Profiler](media/sql-server-profiler-extension/profiler-extension.png)

3. Selezionare l'estensione desiderata e **installarla**.
4. Selezionare **Ricarica** per abilitare l'estensione (necessario solo la prima volta che si installa un'estensione).

## <a name="start-profiler"></a>Avvio Profiler

1. Per avviare Profiler, creare prima una connessione a un server nella scheda Server.
2. Dopo aver creato una connessione, digitare **ALT + P** per avviare Profiler.
3. Per avviare Profiler, digitare **ALT + S**. È ora possibile iniziare a visualizzare gli eventi estesi.

    ![Visualizzare Profiler](media/sql-server-profiler-extension/view-profiler.png)

4. Per arrestare Profiler, digitare **Alt + S**. Questo tasto di scelta rapida è un elemento Toggle.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su Profiler e sugli eventi estesi, vedere [Eventi estesi](../../relational-databases/extended-events/extended-events.md).