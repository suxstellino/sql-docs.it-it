---
title: Alcune repliche di disponibilità non presentano un ruolo integro | Microsoft Docs
description: Il monitoraggio Stato del ruolo delle repliche di disponibilità controlla se esistono repliche di disponibilità che non presentano un ruolo integro.
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
f1_keywords:
- sql13.swb.agdashboard.agp6allroleshealthy.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 7ec5b337-7201-4a66-a541-7560f8b18784
author: cawrites
ms.author: chadam
ms.openlocfilehash: 514de23cf905f7344f76a3597658f8a9d40d1013
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/13/2020
ms.locfileid: "94583896"
---
# <a name="some-availability-replicas-do-not-have-a-healthy-role"></a>Alcune repliche di disponibilità non dispongono di un ruolo integro
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Introduzione  
  
|||  
|-|-|  
|**Nome criteri**|Stato del ruolo delle repliche di disponibilità|  
|**Problema**|Alcune repliche di disponibilità non presentano un ruolo integro.|  
|**Categoria**|**Warning**|  
|**Facet**|gruppo di disponibilità|  
  
## <a name="description"></a>Descrizione  
 Questi criteri consentono di eseguire il rollup dello stato di connessione di tutte le repliche di disponibilità e di verificare l'eventuale esistenza di repliche di disponibilità che non presentano un ruolo integro. I criteri sono in uno stato non integro quando una replica di disponibilità non è né primaria né secondaria. Altrimenti, sono in uno stato integro.  
  
> [!NOTE]  
>  Per questa versione di [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)], le informazioni sulle possibili cause e le soluzioni sono disponibili nella pagina relativa ad [alcune repliche di disponibilità che non presentano un ruolo integro](https://go.microsoft.com/fwlink/p/?LinkId=220854) su Wiki di TechNet.  
  
## <a name="possible-causes"></a>Possibili cause  
 Nel gruppo di disponibilità è presente almeno una replica di disponibilità il cui ruolo attuale non è primario né secondario.  
  
## <a name="possible-solution"></a>Possibile soluzione  
 Utilizzare lo stato dei criteri della replica di disponibilità per trovare quella il cui ruolo non è primario o secondario e risolvere il problema.  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Usare il dashboard Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
