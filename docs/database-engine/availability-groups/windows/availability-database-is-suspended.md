---
title: Il database di disponibilità è sospeso per un gruppo di disponibilità
description: Informazioni su come identificare i possibili motivi per cui un database all'interno di un gruppo di disponibilità Always On può essere sospeso.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: end-user-help
f1_keywords:
- sql13.swb.agdashboard.drp1notsuspended.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 6baee70f-848c-4e86-b20d-78875c0f82cb
author: cawrites
ms.author: chadam
ms.openlocfilehash: 09b8b9094e8ed3d860f342b30ace41c25826894b
ms.sourcegitcommit: 524a0f0cc9533188f4b14d2e78ba1cfe816b3b9a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/27/2021
ms.locfileid: "105633378"
---
# <a name="availability-database-is-suspended-for-an-availability-group"></a>Il database di disponibilità è sospeso per un gruppo di disponibilità
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Introduzione  
  
- **Nome criterio**: stato di sospensione del database di disponibilità
- **Problema**: il database di disponibilità è sospeso.
- **Categoria**: **avviso**
- **Facet**: database di disponibilità  
  
## <a name="description"></a>Descrizione  
 Questi criteri consentono di controllare lo stato di spostamento dei dati del database secondario, anche noto come "replica di database secondaria". I criteri sono in uno stato non integro se questo spostamento è sospeso. Altrimenti, sono in uno stato integro.  
  
## <a name="possible-causes"></a>Possibili cause  
 È probabile che la sincronizzazione dei dati in questo database di disponibilità sia stata sospesa per i motivi seguenti:  
  
-   È probabile che la sincronizzazione dei dati sia sospesa a causa di un errore.  
  
-   È probabile che l'amministratore del database abbia sospeso la sincronizzazione dei dati per scopi di manutenzione.  
  
## <a name="possible-solution"></a>Possibile soluzione  
 Riprendere la sincronizzazione dei dati. Se il problema persistente, controllare il gruppo di disponibilità nel Registro eventi, quindi diagnosticare il motivo per il quale lo spostamento dei dati è stato sospeso.  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Usare il dashboard Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
