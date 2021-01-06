---
title: Modalità quorum WSFC e configurazione del voto
description: Informazioni sulle modalità quorum e sulla votazione usate da Windows Server Failover Clustering con un'istanza di failover di SQL Server o un gruppo di disponibilità Always On.
ms.custom: seo-lt-2019
ms.date: 10/03/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], WSFC clusters
- quorum [SQL Server], AlwaysOn and WSFC quorum
- failover clustering [SQL Server], AlwaysOn Availability Groups
ms.assetid: ca0d59ef-25f0-4047-9130-e2282d058283
author: cawrites
ms.author: chadam
ms.openlocfilehash: 51e48eeead20cce35d4e4c58d767384bab1a8a65
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642629"
---
# <a name="wsfc-quorum-modes-and-voting-configuration-sql-server"></a>Modalità quorum WSFC e configurazione del voto (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Sia i [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)][!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] , sia le istanze del cluster di failover Always On usano Windows Server Failover Clustering (WSFC) come tecnologia della piattaforma.  WSFC utilizza un approccio basato sul quorum per monitorare l'integrità complessiva del cluster e aumentare al massimo la tolleranza di errore a livello di nodo. Una comprensione di base delle modalità quorum WSFC e della configurazione di voto dei nodi è molto importante per progettare ed eseguire la soluzione di ripristino di emergenza e a disponibilità elevata Always On, nonché per risolverne i problemi.  
  
  
##  <a name="cluster-health-detection-by-quorum"></a><a name="ClusterHealthDetectionbyQuorum"></a> Rilevamento dell'integrità del cluster in base al quorum  
 Ogni nodo in un cluster WSFC partecipa alla comunicazione heartbeat periodica per condividere il proprio stato di integrità con gli altri nodi. I nodi che non rispondono sono considerati in stato di errore.  
  
 Un set di nodi *quorum* rappresenta una maggioranza dei nodi votanti e degli elementi di controllo nel cluster WSFC. L'integrità e lo stato complessivi di un cluster WSFC sono determinati da un *voto quorum* periodico.  La presenza di un quorum indica che il cluster è integro e in grado di fornire tolleranza di errore a livello di nodo.  
  
 L'assenza di un quorum indica che il cluster non è integro.  È necessario gestire l'integrità complessiva del cluster WSFC per garantire che siano disponibili nodi secondari integri in cui eseguire il failover dei nodi primari.  Se il voto quorum non riesce, il cluster WSFC verrà impostato offline come misura precauzionale.  Ciò provocherà inoltre l'arresto di tutte le istanze di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] registrate con il cluster.  
  
> [!IMPORTANT]  
>  Se un cluster WSFC viene impostato offline a causa di un errore del quorum, è necessario l'intervento manuale per reimpostare online il cluster.  
>   
>  Per altre informazioni, vedere: [Ripristino di emergenza WSFC tramite quorum forzato &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-disaster-recovery-through-forced-quorum-sql-server.md).  
  
##  <a name="quorum-modes"></a><a name="QuorumModes"></a> Modalità quorum  
 Viene impostata una *modalità quorum* al livello del cluster WSFC che specifica la metodologia utilizzata per i voti quorum.  L'utilità Gestione cluster di failover specifica una modalità quorum consigliata basata sul numero di nodi nel cluster.  
  
 È possibile utilizzare le modalità quorum seguenti per determinare gli elementi che costituiscono un quorum di voti:  
  
-   **Maggioranza dei nodi.** Oltre la metà dei nodi votanti nel cluster deve votare affermativamente perché il cluster sia integro.  
  
-   **Maggioranza dei nodi e delle condivisioni file.** Simile alla modalità quorum Maggioranza dei nodi, ad eccezione del fatto che viene configurata anche una condivisione file remota come elemento di controllo del voto e la connettività da qualsiasi nodo a tale condivisione viene anch'essa conteggiata come voto affermativo.  Oltre la metà dei possibili voti deve essere affermativa perché il cluster sia integro.  
  
     Come procedura consigliata, la condivisione file di controllo non deve trovarsi in alcun nodo nel cluster e deve essere visibile a tutti i nodi nel cluster.  
  
-   **Maggioranza dei nodi e dei dischi.** Simile alla modalità quorum Maggioranza dei nodi, ad eccezione del fatto che viene designata come elemento di controllo di voto anche una risorsa cluster di tipo disco condiviso e la connettività da qualsiasi nodo a tale disco condiviso viene conteggiata anch'essa come voto affermativo.  Oltre la metà dei possibili voti deve essere affermativa perché il cluster sia integro.  
  
-   **Solo disco.** Viene designata come elemento di controllo anche una risorsa cluster di tipo disco condiviso e la connettività da qualsiasi nodo a tale disco condiviso viene conteggiata anch'essa come voto affermativo.  
  
> [!TIP]  
>  Quando si utilizza una configurazione di archiviazione asimmetrica per [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], è in genere consigliabile utilizzare la modalità quorum Maggioranza dei nodi se è presente un numero dispari di nodi votanti oppure la modalità Maggioranza dei nodi e delle condivisioni file quando è presente un numero pari di nodi votanti.  
  
##  <a name="voting-and-non-voting-nodes"></a><a name="VotingandNonVotingNodes"></a> Nodi votanti e non votanti  
 Per impostazione predefinita, ogni nodo nel cluster WSFC è incluso come un membro del quorum del cluster. Ogni nodo dispone di un singolo voto per determinare l'integrità complessiva del cluster e ogni nodo tenterà continuamente di stabilire un quorum.  La discussione del quorum a questo punto ha qualificato attentamente il set di nodi del cluster WSFC che votano sull'integrità del cluster come *nodi votanti*.  
  
 Nessun singolo nodo in un cluster WSFC può determinare in modo definitivo che il cluster è integro o non integro complessivamente.  In qualsiasi momento specificato, dal punto di vista di ciascun nodo, alcuni degli altri nodi possono apparire come offline, con un failover in corso oppure possono non rispondere a causa di un errore di comunicazione di rete.  Una funzione principale del voto quorum consiste nel determinare se lo stato apparente di ogni nodo nel cluster WSFC è davvero l'effettivo stato di tali nodi.  
  
 Per tutti i modelli di quorum ad eccezione di "Solo disco", l'efficacia di un voto quorum dipende da comunicazioni affidabili tra tutti i nodi votanti nel cluster. Le comunicazioni di rete tra nodi nella stessa subnet fisica devono essere considerate affidabili e il voto quorum deve essere considerato attendibile.  
  
 Se, tuttavia, viene rilevato che un nodo in un'altra subnet non risponde in un voto quorum, ma è in realtà online e altrimenti integro, la causa è molto probabilmente un errore di comunicazioni di rete tra subnet.  Poiché dipende dalla topologia del cluster, dalla modalità quorum e dalla configurazione dei criteri di failover, l'errore di comunicazione di rete può effettivamente comportare la creazione di più di un set (o subset) di nodi votanti.  
  
 Quando più di un subset di nodi votanti è in grado di stabilire singolarmente un quorum, si verifica uno *scenario split brain*.  In tale scenario i nodi nei quorum separati possono comportarsi in modo diverso ed essere in conflitto gli uni con gli altri.  
  
> [!NOTE]  
>  Lo scenario split brain è possibile solo quando un amministratore di sistema esegue manualmente un'operazione di quorum forzata o, in casi molto rari, un failover forzato, suddividendo in modo esplicito il set di nodi quorum.  
  
 Per semplificare la configurazione del quorum e aumentare i tempi di attività, può essere necessario modificare l'impostazione *NodeWeight* di ogni nodo in modo che il voto del nodo non venga conteggiato a fini del quorum.  
  
> [!IMPORTANT]  
>  Per utilizzare le impostazioni NodeWeight, è necessario applicare l'aggiornamento rapido seguente a tutti i server del cluster WSFC:  
>   
>  [KB2494036](https://support.microsoft.com/kb/2494036): è disponibile un aggiornamento rapido per configurare un nodo del cluster che non presenta voti quorum in [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] e in [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)]  
  
##  <a name="recommended-adjustments-to-quorum-voting"></a><a name="RecommendedAdjustmentstoQuorumVoting"></a> Modifiche ai voti quorum consigliate  
 Per abilitare o disabilitare il voto di un nodo WSFC specifico, attenersi alle linee guida seguenti:  
  
-   **Nessun voto per impostazione predefinita.** Supporre che ogni nodo sia escluso dal voto in assenza di una giustificazione esplicita.  
  
-   **Includere tutti le repliche primarie.** Ogni nodo WSFC che ospita una replica primaria del gruppo disponibilità o rappresenta il proprietario preferito di un'istanza del cluster di failover deve disporre di un voto.  
  
-   **Includere i possibili proprietari del failover automatico.** Ogni nodo che può ospitare una replica primaria in seguito a un failover automatico del gruppo di disponibilità o al failover dell'istanza del cluster di failover deve disporre di un voto. Se è presente un solo gruppo di disponibilità nel cluster WSFC e le repliche di disponibilità sono ospitate solo da istanze autonome, questa regola include solo la replica secondaria che rappresenta la destinazione del failover automatico.  
  
-   **Escludere nodi del sito secondari.** In generale, non assegnare voti a nodi WSFC che si trovano in un sito di ripristino di emergenza secondario.  Non è consigliabile fare in modo che i nodi nel sito secondario contribuiscano a una decisione che comporti l'impostazione offline del cluster quando non vi sono problemi con il sito primario.  
  
-   **Numero dispari di voti.** Se necessario, aggiungere una condivisione file di controllo, un nodo di controllo o un disco di controllo al cluster e modificare la modalità quorum per impedire possibili valori equivalenti nel voto quorum.  
  
-   **Valutare nuovamente le assegnazioni dei voti dopo il failover.** Non è consigliabile eseguire il failover in una configurazione del cluster che non supporta un quorum integro.  
  
> [!IMPORTANT]
>  Quando si convalida la configurazione dei voti quorum di WSFC, nella Creazione guidata Gruppo di disponibilità Always On viene visualizzato un avviso se una o più delle condizioni seguenti è vera:  
> 
>  -   Il nodo del cluster che ospita la replica primaria non dispone di un voto.  
> -   Una replica secondaria è configurata per il failover automatico e il relativo nodo del cluster non dispone di un voto.  
> -   [KB2494036](https://support.microsoft.com/kb/2494036) non è installato in tutti i nodi del cluster che ospitano repliche di disponibilità. Questa patch è necessaria per aggiungere o rimuovere voti per i nodi del cluster in distribuzioni multisito. Tuttavia, in distribuzioni a singolo sito, non è in genere necessaria e l'avviso può essere ignorato senza rischi.  
> 
> [!TIP]
>  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] espone diverse DMW (viste a gestione dinamica) di sistema che semplificano la gestione delle impostazioni correlate alla configurazione del cluster WSFC e dei voti quorum dei nodi.  
> 
>  Per altre informazioni, vedere  [sys.dm_hadr_cluster](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-transact-sql.md), [sys.dm_hadr_cluster_members](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md), [sys.dm_os_cluster_nodes](../../../relational-databases/system-dynamic-management-views/sys-dm-os-cluster-nodes-transact-sql.md), [sys.dm_hadr_cluster_networks](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-networks-transact-sql.md)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Visualizzare le impostazioni NodeWeight per il quorum del cluster](../../../sql-server/failover-clusters/windows/view-cluster-quorum-nodeweight-settings.md)  
  
-   [Configurare le impostazioni NodeWeight per il quorum del cluster](../../../sql-server/failover-clusters/windows/configure-cluster-quorum-nodeweight-settings.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenuto correlato  
  
-   [Microsoft SQL Server Always On Solutions Guide for High Availability and Disaster Recovery (Guida alle soluzioni AlwaysOn di Microsoft SQL Server per la disponibilità elevata e il ripristino di emergenza)](https://go.microsoft.com/fwlink/?LinkId=227600)  
  
-   [Quorum vote configuration check in Always On Availability Group Wizards (Controllo della configurazione di voto quorum nelle procedure guidate dei gruppi di disponibilità Always On)](https://blogs.msdn.microsoft.com/sqlalwayson/2012/03/13/quorum-vote-configuration-check-in-alwayson-availability-group-wizards-andy-jing/)  
  
-   [Tecnologie di Windows Server:  cluster di failover](https://technet.microsoft.com/library/cc732488\(v=WS.10\).aspx)  
  
-   [Guida dettagliata al cluster di failover: configurazione del quorum in un cluster di failover](https://technet.microsoft.com/library/cc770620\(WS.10\).aspx)  
  
## <a name="see-also"></a>Vedere anche  
 [Ripristino di emergenza WSFC tramite quorum forzato &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-disaster-recovery-through-forced-quorum-sql-server.md)   
 [Windows Server Failover Clustering &#40;WSFC&#41; con SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)  
  
  
