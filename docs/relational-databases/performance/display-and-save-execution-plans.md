---
title: Visualizzare e salvare piani di esecuzione | Microsoft Docs
description: Informazioni su come visualizzare e salvare i piani di esecuzione in un file in formato XML usando SQL Server Management Studio.
ms.custom: ''
ms.date: 08/21/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
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
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f06128a425040fbd6542b12d0275276efa66d480
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505263"
---
# <a name="display-and-save-execution-plans"></a>Visualizzare e salvare piani di esecuzione
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
In questa sezione viene illustrata la procedura di visualizzazione e di salvataggio dei piani di esecuzione in un file in formato XML utilizzando Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
Nei piani di esecuzione sono visualizzate graficamente le modalità di recupero dei dati selezionate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Query Optimizer. I piani di esecuzione rappresentano il costo di esecuzione di istruzioni e query specifiche in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite icone anziché tramite la rappresentazione di tabella generata dall'istruzione [SET SHOWPLAN_ALL](../../t-sql/statements/set-showplan-all-transact-sql.md) o [SET SHOWPLAN_TEXT](../../t-sql/statements/set-showplan-text-transact-sql.md). Questo approccio grafico è utile per la comprensione delle caratteristiche relative alle prestazioni di una query.  

Benché [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Query Optimizer produca solo un piano di esecuzione, sono disponibili i concetti di piano di esecuzione **stimato** e piano di esecuzione **effettivo**.
-  Un [piano di esecuzione stimato](../../relational-databases/performance/display-the-estimated-execution-plan.md) restituisce il piano di esecuzione prodotto da Query Optimizer in fase di compilazione. La produzione del piano di esecuzione stimato non comporta l'esecuzione effettiva della query o del batch e quindi non include alcuna informazione di runtime, ad esempio le metriche relative all'utilizzo effettivo delle risorse o avvisi sul runtime. 
-  Un [piano di esecuzione effettivo](../../relational-databases/performance/display-an-actual-execution-plan.md) restituisce il piano di esecuzione prodotto da Query Optimizer e successivo al completamento dell'esecuzione di query o batch. Il piano include informazioni di runtime sulle metriche relative all'utilizzo effettivo delle risorse e avvisi sul runtime.  

Per altre informazioni sui piani di esecuzione delle query, vedere la [Guida sull'architettura di elaborazione delle query](../../relational-databases/query-processing-architecture-guide.md).
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Visualizzare il piano di esecuzione stimato](../../relational-databases/performance/display-the-estimated-execution-plan.md)  
  
-   [Visualizzazione di un piano di esecuzione effettivo](../../relational-databases/performance/display-an-actual-execution-plan.md)  
  
-   [Salvare un piano di esecuzione in formato XML](../../relational-databases/performance/save-an-execution-plan-in-xml-format.md)  
  
  
