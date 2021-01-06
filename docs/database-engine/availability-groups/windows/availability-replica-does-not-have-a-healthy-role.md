---
title: La replica non ha un ruolo integro per un gruppo di disponibilità
description: Identificare i possibili motivi per cui una replica di disponibilità non ha un ruolo integro all'interno di un gruppo di disponibilità Always On.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: troubleshooting
f1_keywords:
- sql13.swb.agdashboard.arp1rolehealthy.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: ebb2c9f4-2097-4688-b4fb-8f0571047317
author: cawrites
ms.author: chadam
ms.openlocfilehash: e5ef8c8a912854c48489cb5860bb3813bdc6fd10
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643177"
---
# <a name="availability-replica-does-not-have-a-healthy-role-for-an-always-on-availability-group"></a>La replica di disponibilità non presenta un ruolo integro per un gruppo di disponibilità Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Introduzione  
  
|||  
|-|-|  
|**Nome criteri**|Stato del ruolo della replica di disponibilità|  
|**Problema**|La replica di disponibilità non presenta un ruolo integro.|  
|**Categoria**|**Critico**|  
|**Facet**|replica di disponibilità|  
  
## <a name="description"></a>Descrizione  
 Tramite questi criteri viene controllato lo stato del ruolo della replica di disponibilità. I criteri sono in uno stato non integro quando il ruolo della replica di disponibilità non è né primario né secondario. Altrimenti, sono in uno stato integro.  
  
> [!NOTE]  
>  Per questa versione di [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)], le informazioni sulle possibili cause e le soluzioni sono disponibili nella pagina relativa alla [replica di disponibilità che non presenta un ruolo integro](https://go.microsoft.com/fwlink/p/?LinkId=220856) su Wiki di TechNet.  
  
## <a name="possible-causes"></a>Possibili cause  
 Il ruolo di questa replica di disponibilità non è integro. La replica non presenta il ruolo primario o quello secondario.  
  
## <a name="possible-solution-information_still_to_come"></a>Possibile soluzione: Information_still_to_come  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Usare il dashboard Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
