---
title: Il gruppo di disponibilità non è pronto per il failover automatico
description: Informazioni su come identificare i possibili motivi per cui un gruppo di disponibilità Always On non è pronto per il failover.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: end-user-help
f1_keywords:
- sql13.swb.agdashboard.agp3autofailover.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 28261014-342c-442a-bd89-6d04b8d4e8b7
author: cawrites
ms.author: chadam
ms.openlocfilehash: b19e7f11a60ac21b191c524c0444414f1c5231e8
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766035"
---
# <a name="always-on-availability-group-is-not-ready-for-automatic-failover"></a>Il gruppo di disponibilità Always On non è pronto per il failover automatico
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Introduzione  
  
|||  
|-|-|  
|**Nome criteri**|Conformità del gruppo di disponibilità al failover automatico|  
|**Problema**|Il gruppo di disponibilità non è pronto per il failover automatico.|  
|**Categoria**|**Critico**|  
|**Facet**|gruppo di disponibilità|  
  
## <a name="description"></a>Descrizione  
 Con questi criteri è possibile verificare che nel gruppo di disponibilità sia disponibile almeno una replica secondaria pronta per il failover. I criteri si trovano in uno stato non integro e viene generato un avviso quando il failover della replica primaria è automatico, ma nessuna delle repliche secondarie nel gruppo di disponibilità è pronta per il failover.  
  
 I criteri sono in uno stato integro quando almeno una replica secondaria è pronta per il failover automatico.
  
## <a name="possible-causes"></a>Possibili cause  
 Il gruppo di disponibilità non è pronto per il failover automatico. La replica primaria viene configurata per il failover automatico; tuttavia, la replica secondaria non è pronta per il failover automatico. La replica secondaria configurata per il failover automatico potrebbe non essere disponibile o il relativo stato della sincronizzazione dei dati è attualmente NON SINCRONIZZATO.  
  
## <a name="possible-solutions"></a>Possibili soluzioni  
 Di seguito sono riportate le possibili soluzioni a questo problema:  
  
-   Verificare che almeno una replica secondaria venga configurata come failover automatico. Se non è disponibile alcuna replica secondaria configurata come failover automatico, aggiornare la configurazione di una replica secondaria affinché rappresenti la destinazione del failover automatico con commit sincrono.  
  
-   Utilizzare i criteri per verificare che i dati siano in uno stato di sincronizzazione e che la destinazione del failover automatico sia SINCRONIZZATO, quindi risolvere il problema della replica di disponibilità.  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Usare il dashboard Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
