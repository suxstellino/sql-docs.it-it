---
title: Istanza del cluster di failover con gruppi di disponibilità
description: Informazioni su come migliorare la disponibilità elevata e il ripristino di emergenza combinando le funzionalità di un'istanza di cluster di failover di SQL Server e di un gruppo di disponibilità Always On.
ms.custom: seo-lt-2019
ms.date: 07/02/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- clustering [SQL Server]
- Availability Groups [SQL Server], WSFC clusters
- Failover Cluster Instances [SQL Server], see failover clustering [SQL Server]
- quorum [SQL Server]
- failover clustering [SQL Server], AlwaysOn Availability Groups
- Availability Groups [SQL Server], Failover Cluster Instances
ms.assetid: 613bfbf1-9958-477b-a6be-c6d4f18785c3
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 1cb8d5776ffa69832b001b7f3594f743cf4c3d39
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644144"
---
# <a name="failover-clustering-and-always-on-availability-groups-sql-server"></a>Clustering di failover e gruppi di disponibilità Always On (SQL Server)

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

   [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], la soluzione di ripristino di emergenza a disponibilità elevata introdotta in [!INCLUDE[sssql11](../../../includes/sssql11-md.md)], richiede WSFC (Windows Server Failover Clustering). Sebbene inoltre [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] non dipenda dal clustering di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , è possibile utilizzare un'istanza di clustering di failover per ospitare una replica di disponibilità per un gruppo di disponibilità. È importante conoscere la funzione di ogni tecnologia di clustering e sapere quali sono le considerazioni necessarie per la progettazione di un ambiente di [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] .  
  
> [!NOTE]  
>  Per informazioni sui concetti di [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] , vedere [Panoramica di Gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
  
  
##  <a name="windows-server-failover-clustering-and-availability-groups"></a><a name="WSFC"></a> Clustering di failover di Windows Server e gruppi di disponibilità  
 La distribuzione di [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] richiede un cluster WSCF (Windows Server Failover Clustering). Per essere abilitata per [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], un'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] deve trovarsi in un nodo WSFC e il nodo e il cluster WSFC devono essere online. Inoltre, ogni replica di disponibilità di un gruppo di disponibilità deve risiedere in un nodo diverso da quello del cluster WSFC. L'unica eccezione è che quando viene eseguita la migrazione a un altro cluster WSFC, un gruppo di disponibilità può risiedere temporaneamente in due cluster.  
  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] si basa sul cluster WSFC (Windows Server Failover Clustering) per monitorare e gestire i ruoli correnti delle repliche di disponibilità che appartengono a un determinato gruppo di disponibilità e per determinare in che modo un evento di failover influisce sulle repliche di disponibilità. Il gruppo di risorse WSFC viene creato per ogni gruppo di disponibilità che viene creato. Il cluster WSFC esegue il monitoraggio del gruppo di risorse per valutare lo stato di integrità della replica primaria.  
  
 Il quorum di [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] si basa su tutti i nodi del cluster WSFC, indipendentemente dal fatto che un nodo del cluster ospiti una replica di disponibilità. A differenza del mirroring del database, non esiste alcun ruolo del server di controllo in [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)].  
  
 L'integrità complessiva di un cluster WSFC è determinata dai voti di un quorum di nodi nel cluster. Se il cluster WSFC viene portato offline a causa di un'emergenza non pianificata o di un errore persistente dell'hardware o di comunicazione, è necessario un intervento amministrativo manuale. Un amministratore di cluster Windows Server or WSFC dovrà forzare un quorum e, successivamente, riportare online i nodi del cluster ancora esistenti in una configurazione non a tolleranza d'errore.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] sono sottochiavi del cluster WSFC. Se si elimina e si ricrea un cluster WSFC, è necessario disabilitare e riabilitare la funzionalità [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] in ogni istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in cui è ospitata una replica di disponibilità nel cluster WSFC originale.  
  
 Per informazioni sull'esecuzione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nei nodi WSFC e sul quorum WSFC, vedere [WSFC &#40;Windows Server Failover Clustering&#41; con SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md).  
  
##  <a name="ssnoversion-failover-cluster-instances-fcis-and-availability-groups"></a><a name="SQLServerFC"></a> [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] - Istanze del cluster di failover (FCI) e gruppi di disponibilità  
 È possibile configurare un secondo livello di failover a livello di istanza del server implementando un'istanza FCI di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] insieme al cluster WSFC. Una replica di disponibilità può essere ospitata da un'istanza autonoma di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] o da un'istanza FCI. Solo un partner di un'istanza del cluster di failover può ospitare una replica per un gruppo di disponibilità. Quando una replica di disponibilità viene eseguita in un'istanza del cluster di failover, l'elenco dei possibili proprietari per il gruppo di disponibilità conterrà solo il nodo FCI attivo.  
  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] non dipende da alcun mezzo di archiviazione condivisa. Tuttavia, se si utilizza un'istanza del cluster di failover (FCI) di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per ospitare una o più repliche di disponibilità, per ognuna di queste FCI sarà richiesta l'archiviazione condivisa in base all'installazione dell'istanza del cluster di failover di SQL Server standard.  
  
 Per altre informazioni sui prerequisiti aggiuntivi, vedere la sezione "Prerequisiti e restrizioni per l'uso di un'istanza del cluster di failover di SQL Server per ospitare una replica di disponibilità" di [Prerequisiti, restrizioni e consigli per i gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
### <a name="comparison-of-failover-cluster-instances-and-availability-groups"></a>Confronto tra le istanze del cluster di failover e i gruppi di disponibilità  
 Indipendentemente dal numero di nodi nell'istanza FCI, in un'intera FCI viene ospitata una sola replica all'interno di un gruppo di disponibilità. Nella tabella seguente sono descritte le differenze dei concetti tra i nodi di un'istanza FCI e le repliche all'interno di un gruppo di disponibilità.  
  
||Nodi all'interno di un'istanza FCI|Repliche all'interno di un gruppo di disponibilità|  
|-|-------------------------|-------------------------------------------|  
|**Usa cluster WSFC**|Sì|Sì|  
|**Livello di protezione**|Istanza|Database|  
|**Tipo di archiviazione**|Condiviso|Non condivisi<br /><br /> Mentre le repliche di un gruppo di disponibilità non condividono le risorse di archiviazione, una replica ospitata da un'istanza del cluster di failover usa una soluzione di archiviazione condivisa come richiesto da tale istanza. La soluzione di archiviazione è condivisa solo dai nodi all'interno dell'istanza FCI e non tra le repliche del gruppo di disponibilità.|  
|**Soluzioni di archiviazione**|Collegamento diretto, rete SAN, punti di montaggio, SMB|Dipende dal tipo di nodo|  
|**Secondarie leggibili**|No*|Sì|  
|**Impostazioni dei criteri di failover applicabili**|Quorum WSFC<br /><br /> Specifiche per FCI<br /><br /> Impostazioni dei gruppi di disponibilità**|Quorum WSFC<br /><br /> Impostazioni gruppo di disponibilità|  
|**Risorse di cui è stato eseguito il failover**|Server, istanza e database|Solo database|  
  
 \* Mentre le repliche secondarie sincrone di un gruppo di disponibilità sono sempre in esecuzione nelle rispettive istanze di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , i nodi secondari in un'istanza del cluster di failover non hanno avviato le rispettive istanze di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e quindi non sono leggibili. In un'istanza FCI, un nodo secondario consente di avviare l'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] solo quando la proprietà del gruppo di risorse viene trasferita a questo nodo durante un failover dell'istanza FCI. Tuttavia, nel nodo FCI attivo, se un database ospitato da FCI appartiene a un gruppo di disponibilità e la replica di disponibilità locale è in esecuzione come replica secondaria leggibile, il database è leggibile.  
  
 ** Le impostazioni dei criteri di failover per il gruppo di disponibilità si applicano a tutte le repliche, indipendentemente dal fatto che siano ospitate in un'istanza autonoma o in un'istanza del cluster di failover.  
  
> [!NOTE]  
>  Per altre informazioni sul **numero di nodi** all'interno delle istanze FCI e sui **Gruppi di disponibilità Always On** per edizioni diverse di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vedere [Funzionalità supportate dalle edizioni di SQL Server 2012](/previous-versions/sql/sql-server-2012/cc645993(v=sql.110)) (https://go.microsoft.com/fwlink/?linkid=232473).  
  
### <a name="considerations-for-hosting-an-availability-replica-on-an-fci"></a>Considerazioni sull'hosting di una replica di disponibilità in un'istanza FCI  
  
> [!IMPORTANT]  
>  Se si intende ospitare una replica di disponibilità in un'istanza del cluster di failover di SQL Server (FCI), assicurarsi che i nodi host di Windows Server 2008 soddisfino i prerequisiti e le restrizioni di Always On per le istanze del cluster di failover (FCI). Per altre informazioni, vedere [Prerequisiti, restrizioni e consigli per i gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ha istanze del cluster di failover che non supportano il failover automatico da gruppi di disponibilità, quindi le repliche di disponibilità ospitate da un'istanza del cluster di failover possono essere configurate solo per il failover manuale.  
  
 Può essere necessario configurare un cluster WSCF per includere i dischi condivisi che non sono disponibili in tutti i nodi. Ad esempio, si consideri un cluster WSFC in due data center con tre nodi. Due dei nodi ospitano un'istanza FCI di SQL nel data center principale e hanno accesso agli stessi dischi condivisi. Al terzo nodo è permesso ospitare un'istanza autonoma di SQL Server in un data center diverso, ma non è consentito accedere ai dischi condivisi dal data center principale. Questa configurazione WSFC supporta la distribuzione di un gruppo di disponibilità se nell'istanza FCI è ospitata la replica primaria e in quella autonoma la replica secondaria.  
  
 Se si decide che in un'istanza FCI venga ospitata una replica di disponibilità per un determinato gruppo di disponibilità, assicurarsi che un failover dell'istanza FCI non possa comportare il tentativo da parte di un unico nodo WSFC di ospitare due repliche di disponibilità per lo stesso gruppo di disponibilità.  
  
 Nello scenario di esempio seguente si descrivono i potenziali problemi causati da questa configurazione:  
  
 Marcel configura un cluster WSFC con due nodi, `NODE01` e `NODE02`. Installa un'istanza del cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , `fciInstance1`, sia in `NODE01` sia in `NODE02` dove `NODE01` è il proprietario corrente di `fciInstance1`.  
 In `NODE02`Marcel installa un'altra istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], `Instance3`, che è un'istanza autonoma.  
 In `NODE01`Marcel abilita fciInstance1 per [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. In `NODE02`abilita `Instance3` per [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Successivamente configura un gruppo di disponibilità per il quale in `fciInstance1` è ospitata la replica primaria e in `Instance3` quella secondaria.  
 A un certo punto `fciInstance1` non è più disponibile in `NODE01` e il cluster WSFC causa il failover di `fciInstance1` in `NODE02`. Dopo il failover, `fciInstance1` è un'istanza abilitata per [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]in esecuzione nel ruolo primario di `NODE02`. Tuttavia, `Instance3` si trova ora nello stesso nodo WSFC di `fciInstance1`. Viene pertanto violato il vincolo [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] .  
 Per risolvere il problema presentato in questo scenario, l'istanza autonoma, `Instance3`, deve trovarsi in un altro nodo dello stesso cluster WSFC come `NODE01` e `NODE02`.  
  
 Per altre informazioni sulle istanze FCI di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vedere [Istanze del cluster di failover Always On &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md).  
  
##  <a name="restrictions-on-using-the-wsfc-failover-cluster-manager-with-availability-groups"></a><a name="FCMrestrictions"></a> Le restrizioni sull'utilizzo di Gestione cluster di failover WSFC con i gruppi di disponibilità  
 Non utilizzare Gestione cluster di failover per modificare i gruppi di disponibilità, ad esempio:  
  
-   Non aggiungere o rimuovere risorse nel servizio del cluster (gruppo di risorse) per il gruppo di disponibilità.  
  
-   Non modificare le proprietà del gruppo di disponibilità, ad esempio i possibili proprietari e i proprietari preferiti. Queste proprietà vengono impostate automaticamente dal gruppo di disponibilità.  
  
-   **Non usare Gestione cluster di failover per spostare i gruppi di disponibilità in altri nodi o per effettuarne il failover.** Gestione cluster di failover non è in grado di rilevare lo stato di sincronizzazione delle repliche di disponibilità, pertanto possono verificarsi tempi di inattività prolungati. È necessario utilizzare [!INCLUDE[tsql](../../../includes/tsql-md.md)] o [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  

  >[!WARNING]
  > L'uso di Gestione cluster di failover per lo spostamento di un'*istanza di cluster di failover* che ospita un gruppo di disponibilità in un nodo che *ospita già* una replica dello stesso gruppo di disponibilità può causare la perdita della replica del gruppo di disponibilità, impedendo che venga portato online sul nodo di destinazione. Un singolo nodo di un cluster di failover non può ospitare più di una replica dello stesso gruppo di disponibilità. Per altre informazioni su come si verifica questa situazione e sulle misure da adottare, vedere il blog [Replica unexpectedly dropped in availability group](/archive/blogs/alwaysonpro/issue-replica-unexpectedly-dropped-in-availability-group) (Replica rilasciata in modo inatteso nel gruppo di disponibilità). 
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenuto correlato  
  
-   **Blog:**  
  
     [Pagina relativa alla configurazione del clustering di failover di Windows per SQL Server (gruppo di disponibilità o FCI) con sicurezza limitata](/archive/blogs/sqlalwayson/configure-windows-failover-clustering-for-sql-server-availability-group-or-fci-with-limited-security)  
  
     [SQL Server AlwaysOn Team Blog: blog ufficiale del team di SQL Server AlwaysOn](/archive/blogs/sqlalwayson/)  
  
     [Pagina relativa ai blog del Servizio Supporto Tecnico Clienti per gli ingegneri di SQL Server](/archive/blogs/psssql/)  
  
-   **White paper:**  
  
     [Pagina relativa alla guida all'architettura di AlwaysOn in cui viene illustrata la compilazione di una soluzione a disponibilità elevata e di ripristino di emergenza usando istanze del cluster di failover e gruppi di disponibilità](/previous-versions/sql/sql-server-2012/jj215886(v=msdn.10))  
  
     [Microsoft SQL Server Always On Solutions Guide for High Availability and Disaster Recovery (Guida alle soluzioni AlwaysOn di Microsoft SQL Server per la disponibilità elevata e il ripristino di emergenza)](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
     [Pagina relativa ai white paper Microsoft per SQL Server 2012](https://msdn.microsoft.com/library/hh403491.aspx)  
  
     [Pagina relativa ai white paper del team di consulenza clienti di SQL Server](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Abilitare e disabilitare gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)   
 [Monitorare Gruppi di disponibilità &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Istanze del cluster di failover Always On &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)  
  
