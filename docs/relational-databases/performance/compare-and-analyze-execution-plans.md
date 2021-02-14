---
title: Confrontare e analizzare i piani di esecuzione | Microsoft Docs
description: Informazioni su come confrontare e analizzare i piani di esecuzione usando SQL Server Management Studio. I piani di esecuzione visualizzano i metodi di recupero dati di Query Optimizer.
ms.custom: ''
ms.date: 11/21/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Showplan results
- execution plans [SQL Server]
- queries [SQL Server], tuning
- execution plans [SQL Server], how-to topics
- SQL Server Management Studio [SQL Server], execution plans
- tuning queries [SQL Server]
ms.assetid: bcd6f094-c613-4835-ae19-4caaadb4bb17
author: pmasl
ms.author: pelopes
manager: amitban
ms.openlocfilehash: 8b0eed72c447f2f5414d7a87a8b8a9ae08f0b415
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344722"
---
# <a name="compare-and-analyze-execution-plans"></a>Confrontare e analizzare i piani di esecuzione
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
Questa sezione illustra come confrontare e analizzare i piani di esecuzione usando Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Questa funzionalità è disponibile a partire da [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] v17.4.  
  
Nei piani di esecuzione sono visualizzate graficamente le modalità di recupero dei dati selezionate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Query Optimizer. I piani di esecuzione rappresentano il costo di esecuzione di istruzioni e query specifiche in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite icone anziché tramite la rappresentazione di tabella generata dall'istruzione [SET SHOWPLAN_ALL](../../t-sql/statements/set-showplan-all-transact-sql.md) o [SET SHOWPLAN_TEXT](../../t-sql/statements/set-showplan-text-transact-sql.md). Questo approccio grafico è particolarmente utile per la comprensione delle caratteristiche relative alle prestazioni di una query. 

[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] include una funzionalità che consente agli utenti di confrontare due piani di esecuzione, ad esempio un piano considerato corretto e uno errato per la stessa query, e di eseguire l'analisi della causa radice. È inclusa anche una funzionalità per l'analisi di un singolo piano di query che offre informazioni dettagliate su scenari che possono avere effetto sulle prestazioni di una query attraverso l'analisi del piano di esecuzione.

Per altre informazioni sui piani di esecuzione query, vedere il [piano di esecuzione stimato](../../relational-databases/performance/display-the-estimated-execution-plan.md), il [piano di esecuzione effettivo](../../relational-databases/performance/display-an-actual-execution-plan.md) e la [Guida sull'architettura di elaborazione delle query](../../relational-databases/query-processing-architecture-guide.md).
  
## <a name="in-this-section"></a>Contenuto della sezione  
[Confrontare i piani di esecuzione](../../relational-databases/performance/display-the-estimated-execution-plan.md)     
[Analizzare un piano di esecuzione effettivo](../../relational-databases/performance/display-an-actual-execution-plan.md)      
  
