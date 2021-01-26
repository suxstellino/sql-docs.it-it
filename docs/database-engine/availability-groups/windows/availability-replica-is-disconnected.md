---
title: La replica di disponibilità è disconnessa in un gruppo di disponibilità
description: Informazioni su come identificare i possibili motivi per cui una replica è disconnessa all'interno di un gruppo di disponibilità Always On.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: troubleshooting
f1_keywords:
- sql13.swb.agdashboard.arp2connected.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 1a2162d3-54fb-4356-b349-effbdc15a5a4
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0f62cc4bb263701fc71fceb663a1391a7012f514
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766029"
---
# <a name="availability-replica-is-disconnected-within-an-always-on-availability-group"></a>La replica di disponibilità è disconnessa in un gruppo di disponibilità Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Introduzione  
  
|||  
|-|-|  
|**Nome criteri**|Stato di connessione della replica di disponibilità|  
|**Problema**|La replica di disponibilità è disconnessa.|  
|**Categoria**|**Critico**|  
|**Facet**|replica di disponibilità|  
  
## <a name="description"></a>Descrizione  
 Tramite questi criteri è possibile controllare lo stato di connessione tra le repliche di disponibilità. I criteri sono in uno stato non integro quando lo stato di connessione della replica di disponibilità è DISCONNESSO. Altrimenti, sono in uno stato integro.  
  
## <a name="possible-causes"></a>Possibili cause  
 La replica secondaria non è connessa alla replica primaria. Lo stato è DISCONNESSO. Il problema può essere causato da uno dei motivi seguenti:  
  
-   La porta di connessione potrebbe essere in conflitto con un'altra applicazione.  
  
-   Il tipo o l'algoritmo di crittografia non è corrispondente.  
  
-   L'endpoint di connessione è stato eliminato o non è stato avviato.  
  
-   Il trasporto è disconnesso.  
  
## <a name="possible-solutions"></a>Possibili soluzioni  
 Di seguito sono riportate le possibili soluzioni a questo problema:  
  
-   Verificare la configurazione dell'endpoint del mirroring di database per le istanze della replica primaria e secondaria e aggiornare la configurazione non corrispondente.  
  
-   Controllare se la porta è in conflitto, e in questo caso, modificare il numero di porta.  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Usare il dashboard Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
