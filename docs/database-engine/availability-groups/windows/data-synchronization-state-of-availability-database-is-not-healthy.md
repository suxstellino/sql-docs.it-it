---
title: Lo stato della sincronizzazione dei dati del database di disponibilità non è integro
description: Informazioni su come identificare i possibili motivi per cui lo stato della sincronizzazione dei dati del database in un gruppo di disponibilità Always On non è integro.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: end-user-help
f1_keywords:
- sql13.swb.agdashboard.arp3datasynchealthy.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 4fd003e7-808e-4b0e-b28a-47d9f2616f06
author: cawrites
ms.author: chadam
manager: erikre
ms.openlocfilehash: 0e470b200e31e047c7bd9cdd15cc74277f4b0b1e
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765703"
---
# <a name="data-synchronization-state-of-availability-database-is-not-healthy-for-an-always-on-availability-group"></a>Lo stato di sincronizzazione dei dati del database di disponibilità non è integro per un gruppo di disponibilità Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Introduzione  
  
|||  
|-|-|  
|**Nome criteri**|Stato di sincronizzazione dei dati del database di disponibilità|  
|**Problema**|Lo stato di sincronizzazione dei dati del database di disponibilità non è integro.|  
|**Categoria**|**Warning**|  
|**Facet**|Database di disponibilità|  
  
## <a name="description"></a>Descrizione  
 Questi criteri consentono di eseguire il rollup dello stato di sincronizzazione dei dati di tutti i database di disponibilità, anche noti come "repliche di disponibilità", nella replica di disponibilità. I criteri sono in uno stato non integro se una qualsiasi replica del database non è nello stato di sincronizzazione dei dati previsto. Altrimenti, sono in uno stato integro.  
  
## <a name="possible-causes"></a>Possibili cause  
 Lo stato di sincronizzazione dei dati di questo database di disponibilità non è integro. In una replica di disponibilità con commit asincrono, ogni database di disponibilità deve trovarsi nello stato SINCRONIZZAZIONE IN CORSO. In una replica con commit sincrono, ogni database di disponibilità deve trovarsi nello stato SINCRONIZZATO.  
  
## <a name="possible-solution"></a>Possibile soluzione  
 Utilizzare i criteri della replica del database per trovare la replica del database con uno stato di sincronizzazione dei dati non integro e risolvere il relativo problema.  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](~/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Usare il dashboard Always On &#40;SQL Server Management Studio&#41;](~/database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  


