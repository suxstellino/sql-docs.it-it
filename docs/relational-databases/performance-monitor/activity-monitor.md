---
title: Monitoraggio attività | Microsoft Docs
description: Informazioni su come usare Monitoraggio attività per visualizzare informazioni sui processi di SQL Server e su come questi processi influiscono sull'istanza corrente di SQL Server.
ms.custom: ''
ms.date: 04/07/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Activity Monitor [SQL Server]
ms.assetid: 1e6c430d-3a2a-468e-a3d5-ef5459c36c15
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 747cad801170aa9ef2e4907e96dde1b20ddc268e
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/02/2020
ms.locfileid: "96506048"
---
# <a name="activity-monitor"></a>Monitoraggio attività
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
Monitoraggio attività consente di visualizzare informazioni sui processi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e sul modo in cui questi processi influiscono sull'istanza corrente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
Monitoraggio attività è una finestra del documento a schede con i riquadri espandibili e comprimibili seguenti: **Panoramica**, **Processi**, **Attese risorse**, **I/O file di dati**, **Query recenti con costo elevato** e **Query attive con costo elevato**. Quando un riquadro è espanso, Monitoraggio attività esegue una query sull'istanza per ottenere informazioni. Quando un riquadro è compresso, significa che tutte le relative attività di query sono arrestate. È possibile espandere uno o più riquadri contemporaneamente per visualizzare diversi tipi di attività sull'istanza.  
 
## <a name="customize-columns"></a>Personalizzare colonne 
Per le colonne incluse nei riquadri **Processi**, **Attese risorse**, **I/O file di dati**, e **Query recenti con costo elevato** e **Query attive con costo elevato**, personalizzare la visualizzazione come indicato di seguito:  
  
1.  Per ridisporre l'ordine delle colonne, fare clic sull'intestazione di colonna e trascinarla in un altro punto della barra multifunzione delle intestazioni.  
  
2.  Per ordinare una colonna, fare clic sul relativo nome.  
  
3.  Per applicare un filtro in base a una o più colonne, fare clic sulla freccia a discesa nell'intestazione della colonna, quindi selezionare un valore.  

## <a name="more-information"></a>Ulteriori informazioni  
   
|Descrizione|Argomento|  
|-|-|  
|Viene descritto come aprire Monitoraggio attività e come impostare l'intervallo di aggiornamento di Monitoraggio attività.|[Aprire Monitoraggio attività &#40;SQL Server Management Studio&#41;](../../relational-databases/performance-monitor/open-activity-monitor-sql-server-management-studio.md)|  
|Collegamenti agli argomenti relativi alle prestazioni del server e al monitoraggio delle attività.|[Monitoraggio delle prestazioni e dell'attività del server](../../relational-databases/performance/server-performance-and-activity-monitoring.md)|  
  
  
