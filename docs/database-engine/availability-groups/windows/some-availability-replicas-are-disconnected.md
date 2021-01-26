---
title: Alcune repliche di disponibilità sono disconnesse
description: Possibili cause e soluzioni nei casi in cui la replica del gruppo di disponibilità sia disconnessa per un gruppo di disponibilità Always On di SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.agdashboard.agp7allconnected.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: aea808be-6f0f-40c2-9aa2-a2a435ec6443
author: cawrites
ms.author: chadam
ms.openlocfilehash: f4641417055b4a237dbed4459f165dc66f7febc8
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765889"
---
# <a name="some-availability-replicas-are-disconnected"></a>Alcune repliche di disponibilità sono disconnesse
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Introduzione  
  
|||  
|-|-|  
|**Nome criteri**|Stato di connessione delle repliche di disponibilità|  
|**Problema**|Alcune repliche di disponibilità sono disconnesse.|  
|**Categoria**|**Warning**|  
|**Facet**|gruppo di disponibilità|  
  
## <a name="description"></a>Descrizione  
 Questi criteri consentono di eseguire il rollup dello stato di connessione di tutte le repliche di disponibilità e di verificare tutte le repliche di disponibilità che sono disconnesse. I criteri sono in uno stato non integro quando una replica di disponibilità è disconnessa. Altrimenti, sono in uno stato integro.  
 
## <a name="possible-causes"></a>Possibili cause  
 In questo gruppo di disponibilità, almeno una replica secondaria non è connessa alla replica primaria. Lo stato è DISCONNESSO.  
  
## <a name="possible-solution"></a>Possibile soluzione  
 Utilizzare lo stato dei criteri della replica di disponibilità per trovare quella disconnessa e risolvere il problema.  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Usare il dashboard Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
