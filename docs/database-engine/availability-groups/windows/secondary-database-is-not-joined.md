---
title: Un database secondario non è associato | Microsoft Docs
description: Il monitoraggio Stato di join del database di disponibilità controlla lo stato di join del database secondario come parte della gestione basata su criteri per i gruppi di disponibilità Always On.
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.agdashboard.drp2joined.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 10817e5e-75fa-42dd-baa2-359bea3ad051
author: cawrites
ms.author: chadam
ms.openlocfilehash: 39cbbe7a884d5e8fe45d0877b7a581016e1e95c2
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97640547"
---
# <a name="secondary-database-is-not-joined"></a>Un database secondario non è associato
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Introduzione  
  
|||  
|-|-|  
|**Nome criteri**|Stato di join del database di disponibilità|  
|**Problema**|Il database secondario non è unito in join.|  
|**Categoria**|**Warning**|  
|**Facet**|Database di disponibilità|  
  
## <a name="description"></a>Descrizione  
 Questi criteri consentono di controllare lo stato di join del database secondario, anche noto come "replica di database secondaria". I criteri sono in uno stato non integro quando la replica del set di dati non è unita in join. Altrimenti, sono in uno stato integro.  
  
> [!NOTE]  
>  Per questa versione di [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]le informazioni sulle cause e sulle soluzioni possibili sono disponibili in [Secondary database is not joined](https://go.microsoft.com/fwlink/p/?LinkId=220862) (Database secondario non unito in join) in TechNet Wiki.  
  
## <a name="possible-causes"></a>Possibili cause  
 Questo database secondario non è unito in join al gruppo di disponibilità. La configurazione di questo database secondario è incompleta.  
  
## <a name="possible-solution"></a>Possibile soluzione  
 Utilizzare Transact-SQL, PowerShell o SQL Server Management Studio per creare un join della replica secondaria al gruppo di disponibilità. Per altre informazioni sul join delle repliche secondarie ai gruppi di disponibilità, vedere [Creare un join di una replica secondaria a un gruppo di disponibilità (SQL Server)](https://msdn.microsoft.com/library/ff878473\(SQL.110\).aspx).  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Usare il dashboard Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
