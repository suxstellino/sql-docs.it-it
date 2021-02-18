---
title: Forzare l'avvio di un cluster WSFC senza un quorum
description: Forzare l'avvio di un nodo di un cluster Windows Server Failover Clustering senza un quorum, operazione che potrebbe essere necessaria per recuperare i dati e ristabilire la disponibilità elevata.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], WSFC clusters
- quorum [SQL Server], AlwaysOn and WSFC quorum
ms.assetid: 4a121375-7424-4444-b876-baefa8fe9015
author: cawrites
ms.author: chadam
ms.openlocfilehash: dd232e6b5908ff26c47a4b7a945a68f919175418
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353549"
---
# <a name="force-a-wsfc-cluster-to-start-without-a-quorum"></a>Forzare l'avvio di un cluster WSFC senza un quorum
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene illustrato come forzare l'avvio senza un quorum di un nodo del cluster Windows Server Failover Clustering (WSFC).  Questa operazione potrebbe rivelarsi necessaria negli scenari multi-subnet e in caso di ripristino di emergenza per recuperare i dati e ristabilire completamente la disponibilità elevata per le istanze del cluster di failover di [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] e [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   **Prima di iniziare:**  [Indicazioni](#Recommendations), [Sicurezza](#Security)  
  
-   **Per forzare l'avvio di un cluster senza un quorum usando:**  [Uso di Gestione cluster di failover](#FailoverClusterManagerProcedure), [Uso di PowerShell](#PowerShellProcedure), [Uso di Net.exe](#CommandPromptProcedure)  
  
-   **Completamento:**  [Follow Up: Dopo aver forzato l'avvio senza un quorum di un cluster](#FollowUp)  
  
##  <a name="before-you-start"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni  
 Salvo esplicita istruzione, le procedure illustrate in questo argomento funzionano se eseguite da qualsiasi nodo del cluster WSFC.  Tuttavia, è possibile ottenere risultati migliori ed evitare problemi di rete eseguendo questi passaggi dal nodo di cui si desidera forzare l'avvio senza un quorum.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
 L'utente deve disporre di un account di dominio che sia membro del gruppo Administrators locale su ogni nodo del cluster WSFC.  
  
##  <a name="using-failover-cluster-manager"></a><a name="FailoverClusterManagerProcedure"></a> Utilizzo di Gestione cluster di failover  
  
##### <a name="to-force-a-cluster-to-start-without-a-quorum"></a>Per forzare l'avvio di un cluster senza un quorum  
  
1.  Aprire Gestione cluster di failover e connettersi al nodo del cluster desiderato per forzare la modalità online.  
  
2.  Nel riquadro **Azioni** fare clic su **Forza avvio del cluster** e quindi su **Sì - Forza l'avvio del cluster**.  
  
3.  Nel riquadro sinistro, nell'albero **Gestione cluster di failover** fare clic sul nome del cluster.  
  
4.  Nel riquadro di riepilogo, verificare che il valore corrente per **Configurazione quorum** sia:  **Avviso: il cluster è in esecuzione in stato ForceQuorum**.  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Utilizzo di Powershell  
  
##### <a name="to-force-a-cluster-to-start-without-a-quorum"></a>Per forzare l'avvio di un cluster senza un quorum  
  
1.  Avviare Windows PowerShell con privilegi elevati tramite **Esegui come amministratore**.  
  
2.  Importare il modulo `FailoverClusters` per abilitare i commandlet del cluster.  
  
3.  Utilizzare `Stop-ClusterNode` per assicurarsi che il servizio cluster sia stato arrestato.  
  
4.  Utilizzare `Start-ClusterNode` con `-FixQuorum` per forzare l'avvio del servizio cluster.  
  
5.  Utilizzare `Get-ClusterNode` con `-Property NodeWieght = 1` per impostare il valore che garantisca che il nodo è un membro votante del quorum.  
  
6.  Restituire le proprietà del nodo del cluster in un formato leggibile.  
  
### <a name="example-powershell"></a>Esempio (Powershell)  
 L'esempio seguente forza l'avvio senza un quorum del servizio del cluster del nodo Always OnSrv02, imposta il valore `NodeWeight = 1`ed enumera lo stato del nodo del cluster dal nodo appena forzato.  
  
```powershell  
Import-Module FailoverClusters  
  
$node = "Always OnSrv02"  
Stop-ClusterNode -Name $node  
Start-ClusterNode -Name $node -FixQuorum  
  
(Get-ClusterNode $node).NodeWeight = 1  
  
$nodes = Get-ClusterNode -Cluster $node  
$nodes | Format-Table -property NodeName, State, NodeWeight  
  
```  
  
##  <a name="using-netexe"></a><a name="CommandPromptProcedure"></a> Utilizzo di Net.exe  
  
##### <a name="to-force-a-cluster-to-start-without-a-quorum"></a>Per forzare l'avvio di un cluster senza un quorum  
  
1.  Utilizzare Desktop remoto per connettersi al nodo del cluster desiderato per attivare la modalità online.  
  
2.  Avviare un prompt dei comandi con privilegi elevati tramite **Esegui come amministratore**.  
  
3.  Utilizzare **net.exe** per assicurarsi che il servizio cluster locale sia stato arrestato.  
  
4.  Utilizzo **net.exe** con `/forcequorum` per forzare l'avvio del servizio cluster locale.  
  
### <a name="example-netexe"></a>Esempio (Net.exe)  
 Nell'esempio seguente viene forzato l'avvio senza un quorum del servizio cluster del nodo, viene impostato il valore `NodeWeight = 1`e viene enumerato lo stato del nodo del cluster dal nodo appena forzato.  
  
```ms-dos  
net.exe stop clussvc  
net.exe start clussvc /forcequorum  
```  
  
##  <a name="follow-up-after-forcing-cluster-to-start-without-a-quorum"></a><a name="FollowUp"></a> Completamento: Dopo aver forzato l'avvio senza un quorum di un cluster  
  
-   È necessario rivalutare e riconfigurare i valori NodeWeight per costruire correttamente un nuovo quorum prima di riportare online altri nodi. In caso contrario, è possibile che il cluster torni nuovamente offline.  
  
     Per altre informazioni, vedere [Modalità quorum WSFC e configurazione del voto &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-quorum-modes-and-voting-configuration-sql-server.md).  
  
-   Le procedure illustrate in questo argomento rappresentano unicamente un passaggio per riportare online il cluster WSFC in caso di un errore non previsto del quorum.  Potrebbe anche essere necessario effettuare passaggi aggiuntivi per impedire che altri nodi del cluster WSFC interferiscano con la nuova configurazione del quorum.  
  
-   Anche altre funzionalità di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] quali [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], mirroring del database e log shipping potrebbero richiedere azioni per recuperare i dati e ristabilire completamente la disponibilità elevata.  
  
     **Per altre informazioni:**  
  
     [Eseguire un failover manuale forzato di un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)  
  
     [Utilizzo forzato del servizio in una sessione di mirroring del database &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/force-service-in-a-database-mirroring-session-transact-sql.md)  
  
     [Failover su un database secondario per il log shipping &#40;SQL Server&#41;](../../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenuto correlato  
  
-   [Visualizzare eventi e log per un cluster di failover](https://technet.microsoft.com/library/cc772342\(WS.10\).aspx)  
  
-   [Pagina relativa al cluster di failover Get-ClusterLog](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee461045(v=technet.10))  
  
## <a name="see-also"></a>Vedere anche  
 [Ripristino di emergenza WSFC tramite quorum forzato &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-disaster-recovery-through-forced-quorum-sql-server.md)   
 [Configurare le impostazioni NodeWeight per il quorum del cluster](../../../sql-server/failover-clusters/windows/configure-cluster-quorum-nodeweight-settings.md)   
 [Cmdlet del cluster di failover in Windows PowerShell elencati per attività](https://technet.microsoft.com/library/ee619761\(WS.10\).aspx)  
  
