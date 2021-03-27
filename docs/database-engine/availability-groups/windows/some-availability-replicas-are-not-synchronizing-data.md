---
title: Le repliche di disponibilità non sincronizzano i dati
description: Possibili cause e risoluzioni per i casi in cui una o più repliche di disponibilità in un gruppo di disponibilità Always On non sincronizzano i dati con la replica primaria.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.agdashboard.agp4synchronizing.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 3db6a569-e942-4321-a0dd-c4ab002087c8
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6acc37f3976ed9a728aae899eb6178dc29d4de54
ms.sourcegitcommit: 524a0f0cc9533188f4b14d2e78ba1cfe816b3b9a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/27/2021
ms.locfileid: "105633365"
---
# <a name="some-availability-replicas-are-not-synchronizing-data"></a>Alcune repliche di disponibilità non stanno sincronizzando i dati.
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Introduzione  
  
- **Nome criterio**: stato di sincronizzazione dei dati delle repliche di disponibilità
- **Problema**: alcune repliche di disponibilità non eseguono la sincronizzazione dei dati.
- **Categoria**: **avviso**
- **Facet**: gruppo di disponibilità  
  
## <a name="description"></a>Descrizione  
 Questi criteri consentono di eseguire il rollup dello stato di sincronizzazione dei dati di tutte le repliche di disponibilità nel gruppo di disponibilità e di verificare se la sincronizzazione di una qualsiasi replica di disponibilità non funzioni. I criteri si trovano in uno stato non integro se uno degli stati di sincronizzazione dei dati della replica di disponibilità è NON IN SINCRONIZZAZIONE.  
  
 Questi criteri sono in uno stato integro se nessuno degli stati di sincronizzazione dei dati della replica di disponibilità è NON IN SINCRONIZZAZIONE.  
 
## <a name="possible-causes"></a>Possibili cause  
 In questo gruppo di disponibilità, almeno una replica secondaria non si trova nello stato di sincronizzazione NON IN SINCRONIZZAZIONE e non riceve dati dalla replica primaria.  
  
## <a name="possible-solution"></a>Possibile soluzione  
 Utilizzare lo stato dei criteri della replica di disponibilità per trovare la replica di disponibilità con uno stato NON IN SINCRONIZZAZIONE e risolvere il relativo problema.  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Usare il dashboard Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
