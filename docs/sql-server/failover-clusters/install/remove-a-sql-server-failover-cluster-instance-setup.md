---
title: Rimuovere l'istanza del cluster di failover
description: Usare questa procedura per disinstallare un'istanza del cluster di failover Always On. Questo articolo include importanti considerazioni di cui tenere conto prima di procedere.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- clusters [SQL Server], removing failover cluster instance
- failover clustering [SQL Server], removing failover cluster instance
- uninstalling failover cluster instances
- removing failover cluster instances
ms.assetid: bf63353b-69cf-4c5c-98ea-7b151e36537f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 89e3cbbaeca927671a340e3fe4178e6eb5d807e8
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642880"
---
# <a name="remove-a-failover-cluster-instance-setup"></a>Rimuovere un'istanza del cluster di failover (programma di installazione)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Usare questa procedura per disinstallare un'istanza del cluster di failover Always On di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
> [!IMPORTANT]  
>  L'aggiornamento o la rimozione di un cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] è riservata agli amministratori locali con autorizzazione di accesso come servizio su tutti i nodi del cluster di failover di Windows Server.  
  
 **Prima di iniziare**  
  
 Si considerino gli aspetti seguenti prima di disinstallare un'istanza del cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]:  
  
-   Se [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client viene disinstallato per errore, l'avvio delle risorse di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] non riuscirà. Per reinstallare [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client, eseguire il programma di installazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per installare i prerequisiti di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   Se si disinstalla un cluster di failover con più risorse cluster IP SQL, è necessario rimuovere le risorse IP SQL aggiuntive usando Gestione cluster di failover o PowerShell.  
  
 Per informazioni sulla sintassi del prompt dei comandi, vedere [Installazione di SQL Server 2016 dal prompt dei comandi](../../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md).  
  
### <a name="to-uninstall-a-ssnoversion-failover-cluster-instance"></a>Per disinstallare un'istanza del cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]
  
1.  Per disinstallare un cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , utilizzare la funzionalità per la rimozione del nodo disponibile nel programma di installazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per rimuovere ogni nodo singolarmente. Per altre informazioni, vedere [Aggiungere o rimuovere nodi in un cluster di failover Always On &#40;programma di installazione&#41;](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Visualizzare e leggere i file di log del programma di installazione di SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)  
  
