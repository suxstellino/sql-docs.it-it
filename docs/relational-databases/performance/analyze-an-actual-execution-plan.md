---
title: Analizzare un piano di esecuzione effettivo | Microsoft Docs
description: Informazioni su come analizzare i piani di esecuzione grafici effettivi, che contengono informazioni di runtime, usando la funzionalità di analisi del piano di SQL Server Management Studio.
ms.custom: ''
ms.date: 10/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- analyzing execution plans
- analyzing actual execution plans
- execution plans [SQL Server], analyzing
ms.assetid: 9e583a18-5f4a-4054-bfe1-4b2a76630db6
author: pmasl
ms.author: pelopes
manager: amitban
ms.openlocfilehash: 33ed1ed03b9e340e295c14481a6432fca0d3a385
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350061"
---
# <a name="analyze-an-actual-execution-plan"></a>Analizzare un piano di esecuzione effettivo

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Questo argomento descrive come analizzare piani di esecuzione grafici effettivi usando la funzionalità di analisi dei piani di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Questa funzionalità è disponibile a partire da [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] v17.4. In genere è consigliabile [installare la versione di SSMS più recente](../../ssms/download-sql-server-management-studio-ssms.md).

> [!NOTE]
> I piani di esecuzione effettivi vengono generati dopo l'esecuzione di query [!INCLUDE[tsql](../../includes/tsql-md.md)] o batch. Un piano di esecuzione effettivo include quindi informazioni di runtime, ad esempio il numero di righe effettivo, le metriche relative all'utilizzo delle risorse e gli avvisi sul runtime, se disponibili. Per altre informazioni, vedere [Visualizzazione di un piano di esecuzione effettivo](../../relational-databases/performance/display-an-actual-execution-plan.md).
  
La risoluzione dei problemi relativi alle prestazioni delle query richiede una notevole esperienza in materia di elaborazione delle query e piani di esecuzione, senza la quale non è possibile risalire alle cause principali dei problemi ed eliminarle.

[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] include una funzionalità che implementa un certo livello di automazione dell'attività di analisi del piano di esecuzione effettivo, in particolare per quanto riguarda i piani grandi e complessi. L'obiettivo è facilitare l'individuazione degli scenari che presentano una [stima di cardinalità](../../relational-databases/performance/cardinality-estimation-sql-server.md) imprecisa e ottenere consigli sulle possibili soluzioni di attenuazione.

> [!IMPORTANT]
> Prima di applicare le soluzioni di attenuazione proposte in ambienti di produzione, assicurarsi di svolgere sufficienti e adeguati test.
  
## <a name="to-analyze-an-execution-plan-for-a-query"></a>Per analizzare un piano di esecuzione per una query  
  
1.  Aprire un file di piano di esecuzione query salvato in precedenza (file con estensione sqlplan) scegliendo **Apri file** dal menu **File** o trascinando un file di piano nella finestra [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)]. In alternativa, se è stata appena eseguita una query e si è scelto di visualizzare il piano di esecuzione, passare alla scheda **Piano di esecuzione** nel riquadro dei risultati. 

2.  Fare clic con il pulsante destro del mouse su un'area vuota del piano di esecuzione e fare clic su **Analizza piano di esecuzione effettivo**. 

    ![Fare clic con il pulsante destro del mouse su Analizza piano di esecuzione effettivo](../../relational-databases/performance/media/plananalysismenuoption.png "Fare clic con il pulsante destro del mouse su Analizza piano di esecuzione effettivo")   

3.  La finestra **Analisi showplan** viene visualizzata nella parte inferiore. La scheda **Istruzione multipla** è utile durante l'analisi di piani con più istruzioni poiché consente l'analisi dell'istruzione corretta.

4.  Selezionare la scheda Scenari per visualizzare i dettagli sui problemi individuati per il piano di esecuzione effettivo. Per ogni operatore elencato nel riquadro sinistro, il riquadro destro mostra i dettagli sullo scenario tramite il collegamento *Fare clic qui per altre informazioni su questo scenario* e le possibili ragioni che spiegano lo scenario.

    ![Risultati dell'analisi del piano di esecuzione](../../relational-databases/performance/media/plananalysis-scenarios.png "Risultati dell'analisi del piano di esecuzione") 
