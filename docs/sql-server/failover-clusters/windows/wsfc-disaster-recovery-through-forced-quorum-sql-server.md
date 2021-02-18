---
title: Ripristino di emergenza tramite quorum forzato
description: Il ripristino da un errore del quorum richiede un intervento manuale. Informazioni su come forzare il quorum in caso di emergenza di un'istanza del cluster di failover di SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], WSFC clusters
- quorum [SQL Server], AlwaysOn and WSFC quorum
- failover clustering [SQL Server], AlwaysOn Availability Groups
ms.assetid: 6cefdc18-899e-410c-9ae4-d6080f724046
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2358f1f57a7d6b72f97daf4c27963436a40adfca
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355228"
---
# <a name="wsfc-disaster-recovery-through-forced-quorum-sql-server"></a>Ripristino di emergenza WSFC tramite quorum forzato (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Un errore del quorum è causato generalmente da una situazione di emergenza a livello di sistema, da un errore di comunicazione persistente o da una configurazione errata che interessa diversi nodi del cluster WSFC.  Per il recupero da un errore del quorum è necessario intervenire manualmente.  
  
-   **Prima di iniziare:**  [Prerequisiti](#Prerequisites), [Sicurezza](#Security)  
  
-   **Ripristino di emergenza WSFC tramite la procedura relativa al quorum forzato** [Ripristino di emergenza WSFC tramite la procedura relativa al quorum forzato](#Main)  
  
-   [Attività correlate](#RelatedTasks)  
  
-   [Contenuto correlato](#RelatedContent)  
  
##  <a name="before-you-start"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Prerequisiti  
 Nella procedura relativa al quorum forzato si presuppone che il quorum fosse integro prima dell'errore.  
  
> [!WARNING]  
>  L'utente deve conoscere a fondo i concetti e le interazioni di Windows Server Failover Clustering, dei modelli del quorum WSFC, di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]e della configurazione di distribuzione specifica dell'ambiente.  
>   
>  Per altre informazioni, vedere:  [WSFC (Windows Server Failover Clustering) con SQL Server](https://msdn.microsoft.com/library/hh270278\(v=SQL.110\).aspx), [Modalità quorum WSFC e configurazione del voto (SQL Server)](https://msdn.microsoft.com/library/hh270280\(v=SQL.110\).aspx)  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
 L'utente deve disporre di un account di dominio che sia membro del gruppo Administrators locale su ogni nodo del cluster WSFC.  
  
##  <a name="wsfc-disaster-recovery-through-the-forced-quorum-procedure"></a><a name="Main"></a> Ripristino di emergenza WSFC tramite la procedura relativa al quorum forzato  
 Si tenga presente che l'errore del quorum imposterà offline tutti i servizi del cluster, le istanze di SQL Server e [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]nel cluster WSFC, poiché il cluster, in base alla configurazione, non è in grado di assicurare la tolleranza di errore a livello di nodo.  Un errore del quorum significa che i nodi votanti integri del cluster WSFC non soddisfano più il modello di quorum. È possibile che alcuni nodi abbiano avuto esito completamente negativo e alcuni abbiano solo arrestato il servizio WSFC e siano altrimenti integri, a eccezione della perdita della capacità di comunicare con un quorum.  
  
 Per riportare online il cluster WSFC è necessario correggere la causa principale dell'errore del quorum con la configurazione esistente, recuperare i database interessati in base alle esigenze ed eventualmente riconfigurare i nodi restanti nel cluster WSFC per riflettere la topologia di cluster esistente.  
  
 È possibile utilizzare la procedura relativa al *quorum forzato* su un nodo del cluster WSFC per ignorare i controlli di sicurezza che hanno portato il cluster offline.  L'esecuzione della procedura comporta la sospensione dei controlli di voto del quorum all'interno del cluster e consente di riportare online le risorse del cluster WSFC e SQL Server su tutti i nodi nel cluster.  
  
 È opportuno che in questo tipo di processo di ripristino di emergenza siano inclusi i passaggi indicati di seguito.  
  
#### <a name="to-recover-from-quorum-failure"></a>Per correggere un errore del quorum:  
  
1.  **Determinare l'ambito dell'errore.** Identificare quali gruppi di disponibilità o istanze di SQL Server non rispondono, quali nodi del cluster sono online e disponibili per l'utilizzo dopo la condizione di emergenza ed esaminare i registri eventi di Windows e i log di sistema di SQL Server.  Dove possibile, è consigliabile mantenere dati e registri di sistema per un'analisi successiva.  
  
    > [!TIP]  
    >  In un'istanza di [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]che risponde, è possibile ottenere le informazioni sull'integrità dei gruppi di disponibilità che hanno una replica di disponibilità nell'istanza del server locale eseguendo una query sulla vista a gestione dinamica (DMV) [sys.dm_hadr_availability_group_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md) .  
  
2.  **Avviare il cluster WSFC tramite quorum forzato su un singolo nodo.** Identificare un nodo con un numero minimo di errori dei componenti, che non sia l'arresto del servizio cluster WSFC.  Verificare che questo nodo possa comunicare con la maggior parte degli altri nodi.  
  
     Su questo nodo riportare manualmente il cluster online utilizzando la procedura relativa al quorum forzato.  Per ridurre al minimo la possibile perdita di dati, selezionare un nodo che nell'ultima operazione ospitava una replica primaria del gruppo di disponibilità.  
  
     Per altre informazioni, vedere:  [Forzare l'avvio di un cluster WSFC senza un quorum](https://msdn.microsoft.com/library/hh270275\(v=SQL.110\).aspx)  
  
    > [!NOTE]  
    >  L'impostazione del quorum forzato comporta il blocco dei controlli del quorum a livello di cluster, finché il cluster WSFC logico non otterrà una maggioranza di voti e passerà automaticamente alla modalità operativa di un quorum normale.  
  
3.  **Avviare il servizio WSFC normalmente su tutti i nodi diversamente integri, uno alla volta.** Non è necessario specificare l'opzione per il quorum forzato quando si avvia il servizio cluster sugli altri nodi.  
  
     Man mano che il servizio WSFC ritorna online su ogni nodo, viene avviata la negoziazione con gli altri nodi integri per sincronizzare il nuovo stato di configurazione del cluster.  Questa operazione deve essere effettuata un nodo alla volta per evitare possibili race condition nella risoluzione dell'ultimo stato noto del cluster.  
  
    > [!WARNING]  
    >  Assicurarsi che ogni nodo che si avvia possa comunicare con gli altri nodi appena riportati online.  Considerare la possibilità di disabilitare il servizio WSFC sugli altri nodi.  In caso contrario, si corre il rischio di creare di più di un set di nodi del quorum, ottenendo uno scenario "split brain". Se i risultati nel passaggio 1 sono accurati, questa situazione non dovrebbe verificarsi.  
  
4.  **Applicare la nuova modalità quorum e la configurazione di voto dei nodi.** Se tramite la forzatura del quorum vengono riavviati tutti i nodi del cluster e la causa principale dell'errore del quorum è stata corretta, non sarà necessario apportare modifiche alla modalità quorum originale e alla configurazione di voto dei nodi.  
  
     In caso contrario, è necessario valutare il nodo del cluster appena recuperato e la topologia della replica di disponibilità e modificare la modalità quorum e le assegnazioni dei voti per ogni nodo, a seconda dei casi. Sarà necessario impostare offline i nodi non recuperati oppure impostare su zero i relativi voti.  
  
    > [!TIP]  
    >  A questo punto è possibile che i nodi e le istanze di SQL Server risultino apparentemente ripristinati al normale funzionamento.  È tuttavia possibile che non sia ancora disponibile un quorum integro.  Usando Gestione cluster di failover o Dashboard Always On all'interno di SQL Server Management Studio o le DMV appropriate, verificare che sia stato ripristinato un quorum.  
  
5.  **Recuperare le repliche di database del gruppo di disponibilità in base alle esigenze.** I database del gruppo non di disponibilità dovrebbero essere recuperati e ritornare online autonomamente come parte del normale processo di avvio di SQL Server.  
  
     È possibile ridurre al minimo la possibile perdita di dati e il tempo di recupero per le repliche del gruppo di disponibilità riportandoli online in questa sequenza: replica primaria, repliche secondarie sincrone, repliche secondarie asincrone.  
  
6.  **Ripristinare o sostituire componenti con errori e convalidare di nuovo il cluster.** Dopo aver ripristinato la situazione di emergenza iniziale e l'errore del quorum, è necessario ripristinare o sostituire i nodi con errori e modificare di conseguenza le configurazioni WSFC e Always On correlate.  Questa operazione può includere l'eliminazione di repliche del gruppo di disponibilità, la rimozione di nodi dal cluster o l'eliminazione e la reinstallazione del software in un nodo.  
  
     È necessario ripristinare o rimuovere tutte le repliche di disponibilità con errori.  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] il log delle transazioni non verrà troncato oltre l'ultimo punto noto della replica di disponibilità meno aggiornata.   Se una replica con errori non viene ripristinata o rimossa dal gruppo di disponibilità, le dimensioni dei log delle transazioni aumenteranno e si correrà il rischio di esaurire lo spazio dei log delle transazioni delle altre repliche.  
  
    > [!NOTE]  
    >  Se si esegue la Convalida guidata configurazione di WSFC quando nel cluster WSFC è presente un listener del gruppo di disponibilità, tramite la procedura guidata verrà generato il seguente messaggio di avviso non corretto:  
    >   
    >  "La proprietà RegisterAllProviderIP per il nome di rete 'Nome:<nome_rete>' è impostata su 1. Per la configurazione corrente del cluster tale valore dovrebbe essere impostato su 0."  
    >   
    >  Ignorare tale messaggio.  
  
7.  **Ripetere il passaggio 4 in base alle esigenze.** L'obiettivo è ristabilire il livello appropriato di tolleranza di errore e disponibilità elevata per le operazioni integre.  
  
8.  **Eseguire un'analisi RPO/RTO.** È consigliabile analizzare i log di sistema di SQL Server, i timestamp del database e i registri eventi di Windows per determinare la causa principale dell'errore e documentare il punto e il tempo di recupero effettivi.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Forzare l'avvio di un cluster WSFC senza un quorum](../../../sql-server/failover-clusters/windows/force-a-wsfc-cluster-to-start-without-a-quorum.md)  
  
-   [Eseguire un failover manuale forzato di un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)  
  
-   [Visualizzare le impostazioni NodeWeight per il quorum del cluster](../../../sql-server/failover-clusters/windows/view-cluster-quorum-nodeweight-settings.md)  
  
-   [Configurare le impostazioni NodeWeight per il quorum del cluster](../../../sql-server/failover-clusters/windows/configure-cluster-quorum-nodeweight-settings.md)  
  
-   [Usare il Dashboard Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenuto correlato  
  
-   [Visualizzare eventi e log per un cluster di failover](https://technet.microsoft.com/library/cc772342\(WS.10\).aspx)  
  
-   [Pagina relativa al cluster di failover Get-ClusterLog](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee461045(v=technet.10))  
  
## <a name="see-also"></a>Vedere anche  
 [Windows Server Failover Clustering &#40;WSFC&#41; con SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)  
  
