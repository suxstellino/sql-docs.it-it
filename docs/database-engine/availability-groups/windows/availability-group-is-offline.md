---
title: Il gruppo di disponibilità è offline
description: Informazioni su come identificare i momenti e i possibili motivi per cui un gruppo di disponibilità Always On può passare offline.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: end-user-help
f1_keywords:
- sql13.swb.agdashboard.agp2online.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 093c5208-bf7a-49f4-a546-72b48197cadf
author: cawrites
ms.author: chadam
ms.openlocfilehash: 56d79fedca8a5cdd0d91be2728430f5edbdab9ab
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641214"
---
# <a name="always-on-availability-group-is-offline"></a>Un gruppo di disponibilità Always On è offline
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Introduzione  
  
|||  
|-|-|  
|**Nome criteri**|Stato online del gruppo di disponibilità|  
|**Problema**|Il gruppo di disponibilità è offline.|  
|**Categoria**|**Critico**|  
|**Facet**|gruppo di disponibilità|  
  
## <a name="description"></a>Descrizione  
 Questi criteri consentono di controllare lo stato online o offline del gruppo di disponibilità. I criteri si trovano in uno stato non integro e viene generato un avviso quando la risorsa cluster del gruppo di disponibilità è offline o nel gruppo di disponibilità non è disponibile una replica primaria.  
  
 I criteri si trovano in uno stato integro quando la risorsa cluster del gruppo di disponibilità è online e nel gruppo di disponibilità è disponibile una replica primaria.  
  
> [!NOTE]  
>  Per questa versione di [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]le informazioni sulle possibili cause e le possibili soluzioni sono disponibili nella pagina [Availability group is offline](https://go.microsoft.com/fwlink/p/?LinkId=220850) (Il gruppo di disponibilità è offline) in TechNet Wiki.  
  
## <a name="possible-causes"></a>Possibili cause  
 Questo problema può essere causato da un errore nell'istanza del server in cui è ospitata la replica primaria o dal passaggio alla modalità offline della risorsa del gruppo di disponibilità WSCF (Windows Server Failover Clustering). Di seguito sono riportate le possibili cause del passaggio alla modalità offline del gruppo di disponibilità:  
  
-   Il gruppo di disponibilità non è configurato con la modalità di failover automatico. La replica primaria non è più disponibile e il ruolo di tutte le repliche nel gruppo di disponibilità diventa RISOLUZIONE.  
  
    -   Il servizio dell'istanza della replica primaria non funziona o non risponde.  
  
    -   Si è verificato un problema di connettività tra il gruppo di disponibilità e il cluster.  
  
-   Il gruppo di disponibilità è configurato con la modalità di failover automatico, ma non viene completato correttamente.  
  
    -   Durante il failover automatico, il controllo di conformità della replica primaria nella replica di destinazione non viene eseguito correttamente e nessuna replica è disponibile a diventare la nuova primaria.  
  
-   La risorsa del gruppo di disponibilità nel cluster passa alla modalità offline.  
  
    -   Si verifica un problema grave in qualsiasi risorsa cluster dipendente che ne determina il passaggio alla modalità offline. La risorsa del gruppo di disponibilità è offline finché la risorsa dipendente non passa alla modalità online.  
  
    -   Un problema grave nel cluster determina la disabilitazione della risorsa del gruppo di disponibilità.  
  
-   È in corso un failover automatico, manuale o forzato per il gruppo di disponibilità.  
  
## <a name="possible-solutions"></a>Possibili soluzioni  
 Di seguito sono riportate le possibili soluzioni a questo problema:  
  
-   Se l'istanza di SQL Server della replica primaria non è attiva, riavviare il server, quindi verificare che venga recuperato uno stato integro per il gruppo di disponibilità.  
  
-   Se sembra che il failover automatico non sia stato completato correttamente, verificare che i database della replica siano sincronizzati con la replica primaria nota in precedenza, quindi eseguire il failover alla replica primaria. Se i database non sono sincronizzati, selezionare una replica con una perdita minima di dati, quindi recuperare alla modalità failover.  
  
-   Se la risorsa nel cluster è offline mentre le istanze di SQL Server sembrano essere integre, utilizzare Gestione cluster di failover per controllare l'integrità del cluster o altri problemi del cluster nel server. Gestione cluster di failover può essere utilizzato anche per tentare di portare online la risorsa del gruppo di disponibilità.  
  
-   Se è in corso un failover, attenderne il completamento.  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Usare il dashboard Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
