---
title: SQL Server Distributed Replay
titleSuffix: SQL Server Distributed Replay
description: La funzionalità Riesecuzione distribuita di SQL Server consente di valutare l'effetto degli aggiornamenti futuri per SQL Server, l'hardware e il sistema operativo e l'ottimizzazione di SQL Server.
ms.prod: sql
ms.technology: tools-other
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: mikeray
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: ac77b9ead15eb4171dbcaee38235a9f934aaa172
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837783"
---
# <a name="sql-server-distributed-replay"></a>SQL Server Distributed Replay

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

La funzionalità Riesecuzione distribuita di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consente di valutare l'impatto dei futuri aggiornamenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. È possibile usarla anche per valutare l'impatto degli aggiornamenti hardware e del sistema operativo e dell'ottimizzazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

## <a name="benefits-of-distributed-replay"></a>Vantaggi di Distributed Replay

Analogamente a SQL Server Profiler, è possibile usare Riesecuzione distribuita per riprodurre una traccia acquisita su un ambiente di testing aggiornato. Diversamente da SQL Server Profiler, Riesecuzione distribuita non si limita alla riproduzione del carico di lavoro da un singolo computer,

ma offre una soluzione più scalabile rispetto a SQL Server Profiler. Con Distributed Replay è possibile riprodurre un carico di lavoro da più computer e simulare in modo migliore un carico di lavoro di importanza critica.

La funzionalità Riesecuzione distribuita di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può usare più computer per riprodurre dati di traccia e simulare un carico di lavoro di importanza critica. Utilizzare Distributed Replay per testare la compatibilità delle applicazioni e le prestazioni o per pianificare la capacità.

## <a name="when-to-use-distributed-replay"></a>Quando utilizzare Distributed Replay

SQL Server Profiler e Riesecuzione distribuita offrono alcune funzionalità sovrapposte.

È possibile usare SQL Server Profiler per riprodurre una traccia acquisita su un ambiente di testing aggiornato. È inoltre possibile analizzare i risultati di riproduzione per cercare possibili incompatibilità funzionali e di prestazioni. Tuttavia, SQL Server Profiler può riprodurre solo un carico di lavoro da un singolo computer. Quando si riproduce un'applicazione OLTP intensiva che ha molte connessioni simultanee attive o una velocità effettiva elevata, SQL Server Profiler può costituire un collo di bottiglia per le risorse.

Riesecuzione distribuita offre una soluzione più scalabile rispetto a SQL Server Profiler. Utilizzarlo per riprodurre un carico di lavoro da più computer e simulare in modo migliore un carico di lavoro di importanza critica.

Nella tabella seguente viene descritto quando utilizzare ciascuno strumento.

|Strumento|Usare se...|
|----------|---------------|
| SQL Server Profiler | Si desidera utilizzare il meccanismo di riproduzione convenzionale in un singolo computer. In particolare, sono necessarie funzionalità di debug riga per riga, ad esempio i comandi **Passaggio**, **Esegui fino al cursore** e **Imposta/Rimuovi punto di interruzione** .<br /><br /> Si desidera riprodurre una traccia per [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . |
| Distributed Replay |Si desidera valutare la compatibilità delle applicazioni. Si desidera, ad esempio, testare scenari di aggiornamento di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e del sistema operativo, gli aggiornamenti hardware o l'ottimizzazione degli indici.<br /><br /> La concorrenza nella traccia acquisita è talmente elevata che un singolo client di riproduzione non è in grado di simularla in modo appropriato.|  

## <a name="distributed-replay-concepts"></a>Concetti di base di Distributed Replay

I componenti seguenti costituiscono l'ambiente di Distributed Replay:  

- **Strumento di amministrazione Riesecuzione distribuita**: applicazione console, **DReplay.exe**, usata per comunicare con il servizio controller di Riesecuzione distribuita. Utilizzare lo strumento di amministrazione per controllare la riproduzione distribuita.  

- **Controller di Riesecuzione distribuita**: computer che esegue il servizio di Windows denominato controller di Riesecuzione distribuita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Con il controller di Riesecuzione distribuita è possibile orchestrare le azioni dei client Riesecuzione distribuita. In ogni ambiente di Riesecuzione distribuita può essere presente una sola istanza del controller.  

- **Client Riesecuzione distribuita**: uno o più computer (fisici o virtuali) che eseguono il servizio di Windows denominati client Riesecuzione distribuita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. I client Riesecuzione distribuita vengono usati insieme per simulare carichi di lavoro in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. In ogni ambiente di Riesecuzione distribuita possono essere presenti uno o più client.  

- **Server di destinazione**: istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che i client Riesecuzione distribuita possono usare per riprodurre i dati di traccia. È consigliabile posizionare il server di destinazione in un ambiente di testing.

Distributed Replay Administration Tool, Controller e Client possono essere installati in computer diversi o sullo stesso computer. Sullo stesso computer può essere in esecuzione una sola istanza del servizio Distributed Replay Controller o Client.

Nella figura seguente viene mostrata l'architettura fisica di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay:  

![Architettura di Riesecuzione distribuita](../../tools/distributed-replay/media/distributedreplayarch.gif "Architettura di Riesecuzione distribuita")  

## <a name="distributed-replay-tasks"></a>Attività Distributed Replay

|Descrizione dell'attività|Argomento|  
|----------------------|-----------|  
| Viene descritto come configurare Distributed Replay. | [Configurare Riesecuzione distribuita](../../tools/distributed-replay/configure-distributed-replay.md) |
| Viene descritto come preparare i dati di traccia di input. | [Preparare i dati di traccia di input](../../tools/distributed-replay/prepare-the-input-trace-data.md) |
| Viene descritto come riprodurre i dati di traccia. |[Rieseguire i dati di traccia](../../tools/distributed-replay/replay-trace-data.md) | | Viene descritto come rivedere i risultati dei dati di traccia di Distributed Replay. |[Esaminare i risultati della riesecuzione](../../tools/distributed-replay/review-the-replay-results.md)|
| Viene descritto come usare lo strumento di amministrazione per avviare, monitorare e annullare operazioni nel controller. | [Opzioni della riga di comando dello strumento di amministrazione &#40;Utilità Riesecuzione distribuita&#41;](../../tools/distributed-replay/administration-tool-command-line-options-distributed-replay-utility.md) |

## <a name="see-also"></a>Vedere anche

[Forum di SQL Server per Riesecuzione distribuita](https://social.technet.microsoft.com/Forums/sl/sqldru/)