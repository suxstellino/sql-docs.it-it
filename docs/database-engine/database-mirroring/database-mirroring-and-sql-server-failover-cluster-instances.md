---
title: Mirroring del database e istanze del cluster di failover
description: Informazioni su come combinare il mirroring del database con le istanze del cluster di failover di SQL Server. Informazioni sulla modalità a prestazioni elevate e la modalità a sicurezza elevata con e senza failover.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- failover clustering [SQL Server], database mirroring
- database mirroring [SQL Server], failover
- high-availability mode [SQL Server]
ms.assetid: f1dd6a79-698b-4e31-b923-6bfc3ea0b617
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 0adc43373642a0f4e79f2f7af32e96f96ef7e7de
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642077"
---
# <a name="database-mirroring-and-sql-server-failover-cluster-instances"></a>Mirroring del database e istanze del cluster di failover di SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Per cluster di failover si intende una combinazione di uno o più dischi fisici inclusi in un gruppo cluster di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Cluster Services (MSCS), noto come gruppo di risorse, che partecipano ai nodi del cluster. Il gruppo di risorse viene configurato come istanza cluster di failover che ospita un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un'istanza cluster di failover di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene visualizzata nella rete come singolo computer, ma include funzionalità che consentono il failover tra nodi nel caso in cui un nodo non sia più disponibile. Per altre informazioni, vedere [Istanze del cluster di failover Always On &#40;SQL Server&#41;](../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md).  
  
 I cluster di failover offrono un supporto a disponibilità elevata per un'intera istanza di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], mentre il mirroring del database offre un supporto a disponibilità elevata per un database singolo. Il mirroring del database viene utilizzato tra cluster di failover, nonché tra un cluster di failover e un host non cluster.  
  
> [!NOTE]  
>  Per un'introduzione al mirroring del database, vedere [Mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md).  
  
## <a name="mirroring-and-clustering"></a>Mirroring e clustering  
 In genere, quando il mirroring viene utilizzato in combinazione con il clustering, il server principale e il server mirror risiedono entrambi nei cluster. Il server principale viene eseguito nell'istanza cluster di failover di un cluster mentre il server mirror viene eseguito nell'istanza cluster di failover di un cluster diverso. È tuttavia possibile stabilire una sessione di mirroring in cui un partner risiede nell'istanza cluster di failover di un cluster e l'altro partner risiede in un computer non cluster separato.  
  
 Se il server principale non è temporaneamente disponibile a causa del failover di un cluster, i client vengono disconnessi dal database. Al termine del failover di un cluster, è possibile riconnettere i client al server principale nello stesso cluster, in un cluster diverso o in un computer non cluster, a seconda della [modalità operativa](../../database-engine/database-mirroring/database-mirroring-operating-modes.md). Pertanto, quando si sceglie la configurazione per il mirroring del database in un ambiente cluster, la modalità operativa utilizzata per il mirroring riveste notevole importanza.  
  
### <a name="high-safety-mode-session-with-automatic-failover"></a>Sessione in modalità a sicurezza elevata con failover automatico  
 Se si desidera eseguire il mirroring di un database in modalità a sicurezza elevata con failover automatico, per i partner è consigliabile una configurazione a due cluster. Questa configurazione consente di ottenere la disponibilità massima. Il server di controllo del mirroring può risiedere in un terzo cluster o in un computer non cluster.  
  
 Se si verifica un problema nel nodo in cui è in esecuzione il server principale corrente, entro alcuni secondi viene avviato il failover automatico del database, mentre è ancora in corso il failover del cluster a un altro nodo. Viene eseguito il failover della sessione di mirroring del database al server mirror nell'altro cluster o nel computer non cluster, mentre il server mirror precedente diventa il server principale. Il nuovo server principale esegue il rollforward della rispettiva copia del database nel modo più rapido possibile e la porta online come database principale. Al termine del failover del cluster, che in genere richiede diversi minuti, l'istanza cluster di failover che in precedenza svolgeva la funzione di server principale diventa il server mirror.  
  
 Nella figura seguente viene illustrato un failover automatico tra cluster in una sessione di mirroring in esecuzione in modalità a sicurezza elevata con un server di controllo del mirroring, che supporta il failover automatico.  
  
 ![Failover su un cluster](../../database-engine/database-mirroring/media/dbm-and-failover-clustering.gif "Failover su un cluster")  
  
 Le tre istanze del server nella sessione di mirroring risiedono in tre cluster distinti: **Cluster_A**, **Cluster_B** e **Cluster_C**. In ogni cluster è in esecuzione un'istanza predefinita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] come istanza cluster di failover di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . All'avvio della sessione di mirroring, l'istanza cluster di failover in **Cluster_A** è il server principale, l'istanza cluster di failover in **Cluster_B** è il server mirror e l'istanza cluster di failover in **Cluster_C** è il server di controllo del mirroring nella sessione. Nella fase finale, si verifica un problema nel nodo attivo del **Cluster_A** e, di conseguenza, il server principale non è più disponibile.  
  
 Prima che si verifichi il failover del cluster, il server mirror, con la collaborazione del server di controllo del mirroring, rileva la perdita del server principale. Il server mirror esegue il rollforward del rispettivo database, portandolo online come nuovo database principale nel più breve tempo possibile. Al termine del failover nel **Cluster_A** , il server principale precedente è diventato il server mirror, che sincronizza il rispettivo database con il database principale corrente del **Cluster_B**.  
  
### <a name="high-safety-mode-session-without-automatic-failover"></a>Sessione in modalità a sicurezza elevata senza failover automatico  
 Se si esegue il mirroring di un database in modalità a sicurezza elevata senza failover automatico, un altro nodo del cluster assumerà il ruolo di server principale se nel nodo in cui è eseguito il server principale corrente si verifica un errore. Si noti che, quando il cluster non è disponibile, non è disponibile nemmeno il database.  
  
### <a name="high-performance-mode-session"></a>Sessione in modalità a prestazioni elevate  
 Se si desidera eseguire il mirroring di un database in modalità a prestazioni elevate, è consigliabile posizionare il server principale nell'istanza cluster di failover di un cluster e il server mirror in un server non cluster in una posizione remota. Se viene eseguito il failover del cluster su un nodo diverso, l'istanza cluster di failover continuerà a svolgere la funzione di server principale nella sessione di mirroring. Se si verificano problemi nell'intero cluster, è possibile forzare il servizio nel server mirror.  
  
 **Per impostare un nuovo cluster di failover di SQL Server**  
  
-   [Creare un nuovo cluster di failover di SQL Server &#40;programma di installazione&#41;](../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md)  
  
 **Per impostare il mirroring del database**  
  
-   [Impostazione del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)  
  
-   [Stabilire una sessione di mirroring del database tramite autenticazione di Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Modalità di funzionamento del mirroring del database](../../database-engine/database-mirroring/database-mirroring-operating-modes.md)   
 [Istanze del cluster di failover Always On &#40;SQL Server&#41;](../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)  
  
  
