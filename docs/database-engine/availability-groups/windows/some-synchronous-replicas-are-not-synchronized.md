---
title: Alcune repliche sincrone non sono sincronizzate
description: Descrive alcune possibili cause e soluzioni per i casi in cui una replica sincrona non è sincronizzata per un gruppo di disponibilità Always On.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.agdashboard.agp5synchronized.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: e58ed56e-4c30-42e6-a9fc-a8c401620e02
author: cawrites
ms.author: chadam
ms.openlocfilehash: b9070e88373e689cd753e1d378d55e1501715c5c
ms.sourcegitcommit: 524a0f0cc9533188f4b14d2e78ba1cfe816b3b9a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/27/2021
ms.locfileid: "105633337"
---
# <a name="some-synchronous-replicas-are-not-synchronized"></a>Alcune repliche sincrone non sono sincronizzate
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Introduzione  
  
- **Nome criterio**: stato di sincronizzazione dei dati delle repliche sincrone
- **Problema**: alcune repliche sincrone non sono sincronizzate.
- **Categoria**: **avviso**
- **Facet**: gruppo di disponibilità  
  
## <a name="description"></a>Descrizione  
 Questi criteri consentono di eseguire il rollup dello stato di sincronizzazione dei dati di tutte le repliche di disponibilità e di verificare tutte le repliche di disponibilità che non sono nello stato di sincronizzazione previsto. I criteri sono in uno stato non integro se una replica asincrona è in uno stato NON IN SINCRONIZZAZIONE e una replica sincrona è in uno stato NON SINCRONIZZATO. Altrimenti, i criteri sono in uno stato integro.  

## <a name="possible-causes"></a>Possibili cause  
 In questo gruppo di disponibilità, almeno una replica sincrona non è attualmente sincronizzata. Lo stato di sincronizzazione della replica può essere SINCRONIZZAZIONE IN CORSO o NON IN SINCRONIZZAZIONE.  
  
## <a name="possible-solution"></a>Possibile soluzione  
 Utilizzare lo stato dei criteri della replica di disponibilità per trovare quella con lo stato di sincronizzazione non corretto e risolvere il problema.  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Usare il dashboard Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
