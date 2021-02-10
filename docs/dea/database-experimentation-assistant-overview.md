---
title: Panoramica dell'Database Experimentation Assistant
description: Informazioni dettagliate su Database Experimentation Assistant (DEA), ad esempio come valutare una versione di destinazione SQL Server per un carico di lavoro specifico.
ms.date: 12/12/2019
ms.prod: sql
ms.suite: sql
ms.technology: dea
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: pochiraju
ms.author: rajpo
ms.reviewer: mathoma
ms.custom: seo-lt-2019
ms.openlocfilehash: 09d334d7cfcc313cd36d3b59a6b47c64d441f3ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017071"
---
# <a name="overview-of-database-experimentation-assistant"></a>Panoramica di Database Experimentation Assistant

Database Experimentation Assistant (DEA) è una soluzione di sperimentazione per gli aggiornamenti di SQL Server. DEA può aiutarti a valutare una versione di destinazione di SQL Server per un carico di lavoro specifico. I clienti che eseguono l'aggiornamento da versioni precedenti di SQL Server (a partire da 2005) a versioni più recenti di SQL Server possono usare la metrica di analisi fornita dallo strumento.

Le metriche di analisi di DEA includono:

- Query con errori di compatibilità.
- Query degradate e piani di query.
- Altri dati di confronto del carico di lavoro.

I dati di confronto possono determinare una maggiore confidenza e garantire una corretta esperienza di aggiornamento.

## <a name="get-dea"></a>Ottenere DEA

Per installare DEA, [scaricare](https://www.microsoft.com/download/details.aspx?id=54090) la versione più recente dello strumento. Eseguire quindi il file di **DatabaseExperimentationAssistant.exe** .

## <a name="solution-architecture-for-comparing-workloads"></a>Architettura della soluzione per il confronto dei carichi di lavoro

Il diagramma seguente illustra l'architettura della soluzione per un confronto del carico di lavoro. Il confronto del carico di lavoro USA DEA e Riesecuzione distribuita durante un aggiornamento da SQL Server 2008 a SQL Server 2016.

![Architettura della soluzione di confronto del carico di lavoro](./media/database-experimentation-assistant-overview/dea-overview-compare-solution-architecture.png)

## <a name="dea-prerequisites"></a>Prerequisiti di DEA

Di seguito sono riportati alcuni prerequisiti per l'esecuzione di DEA:

- Requisito hardware minimo: un computer a core singolo con 3,5 GB di RAM.
- Requisito hardware ideale: CPU con 8 core (con 3,5 GB di RAM o superiore). I processori con più di otto core non migliorano i tempi di esecuzione di DEA.
- Per archiviare A, B e database di analisi dei report è necessario un ulteriore 33% delle dimensioni della traccia delle prestazioni.

## <a name="configure-dea"></a>Configura DEA

Nell'architettura dell'ambiente prerequisiti è consigliabile installare DEA nello *stesso computer del controller di riesecuzione distribuita*. Questa pratica evita le chiamate tra computer e semplifica la configurazione.

### <a name="required-configuration-for-workload-comparison-using-dea"></a>Configurazione richiesta per il confronto dei carichi di lavoro con DEA

DEA si connette ai server di database tramite l'autenticazione di Windows. Assicurarsi che l'utente che esegue DEA possa connettersi ai server di database (origine, destinazione e analisi) usando l'autenticazione di Windows.

**Acquisire i requisiti di configurazione**

Per acquisire una traccia, è necessario che l'utente esegua DEA:

- Consente di connettersi al server di database di origine utilizzando l'autenticazione di Windows.
- Dispone dei diritti sysadmin nel server di database di origine.

Inoltre, l'account del servizio che esegue il server di database di origine richiede l'accesso in scrittura al percorso della cartella di traccia.

Per ulteriori informazioni, vedere [domande frequenti sull'acquisizione di tracce](database-experimentation-assistant-capture-trace.md#frequently-asked-questions-about-trace-capture).

**Requisiti di configurazione della riproduzione**

Per riprodurre una traccia, è necessario che l'utente esegua DEA:

- Consente di connettersi al server di database di destinazione tramite l'autenticazione di Windows.
- Dispone dei diritti sysadmin nel server di database di destinazione.

Inoltre, la riproduzione di una traccia richiede che:

- L'account del servizio che esegue i server del database di destinazione ha accesso in scrittura al percorso della cartella di traccia.
- L'account del servizio che esegue Riesecuzione distribuita client può connettersi al server di database di destinazione tramite l'autenticazione di Windows.
- Le porte TCP vengono aperte per le richieste in ingresso nel controller di Riesecuzione distribuita. DEA comunica con il controller di Riesecuzione distribuita usando le interfacce COM.

Per ulteriori informazioni, vedere [domande frequenti sulla riproduzione della traccia](database-experimentation-assistant-replay-trace.md#frequently-asked-questions-about-trace-replay).

**Requisiti di configurazione dell'analisi**

Per eseguire l'analisi, è necessario che l'utente esegua DEA:

- Consente di connettersi al server di database di analisi utilizzando l'autenticazione di Windows.
- Dispone dei diritti sysadmin nel server di database di origine.

Per ulteriori informazioni, vedere [domande frequenti sui report di analisi](database-experimentation-assistant-create-report.md#frequently-asked-questions-about-analysis-reports).

## <a name="set-up-telemetry"></a>Configurare la telemetria

DEA dispone di una funzionalità abilitata per Internet in grado di inviare informazioni di telemetria a Microsoft da utilizzare per migliorare l'esperienza del prodotto. Anche le informazioni raccolte vengono salvate nel computer per il controllo locale, in modo da poter visualizzare sempre gli elementi raccolti. Tutti i file di log di DEA vengono salvati nella cartella% Temp% \\ dea.

I dati di telemetria possono essere raccolti in quattro tipi di eventi:

- **TraceEvent**: eventi di utilizzo per l'applicazione (ad esempio, "attivato arresta acquisizione").
- **Eccezione**: eccezione generata durante l'utilizzo dell'applicazione.
- **DiagnosticEvent**: registro eventi per facilitare la diagnosi quando si verificano problemi (*non* inviati a Microsoft).
- **FeedbackEvent**: Commenti utente inviati tramite l'applicazione.

La raccolta e l'invio di dati di telemetria sono facoltativi. Per specificare quali eventi vengono raccolti e se gli eventi raccolti vengono inviati a Microsoft, attenersi alla procedura seguente:

1. Passare al percorso in cui è installato DEA (ad esempio, C: \\ Program Files (x86) \\ Microsoft Corporation \\ database Experimentation Assistant).
2. Aprire e modificare i file con estensione config **DEA.exe.config** (per l'applicazione) e **DEACmd.exe.config** (per l'interfaccia della riga di comando) per risolvere lo scenario in base alle esigenze:
    - Per interrompere la raccolta di un tipo di evento, impostare il valore dell' *evento* (ad esempio, **TraceEvent**) su **false**. Per iniziare a raccogliere nuovamente l'evento, impostare il valore su **true**.
    - Per arrestare il salvataggio delle copie locali degli eventi, impostare il valore di **TraceLoggerEnabled** su **false**. Per iniziare a salvare di nuovo le copie locali, impostare il valore su **true**.
    - Per arrestare l'invio di eventi a Microsoft, impostare il valore di **AppInsightsLoggerEnabled** su **false**. Per iniziare a inviare nuovamente gli eventi a Microsoft, impostare il valore su **true**.

DEA è disciplinato dall' [informativa sulla privacy Microsoft](https://aka.ms/dea-privacy).

## <a name="see-also"></a>Vedi anche

- L'articolo [Panoramica del processo di confronto del carico di lavoro](database-experimentation-assistant-get-started.md), che illustra il processo di confronto dei carichi di lavoro in due ambienti.
