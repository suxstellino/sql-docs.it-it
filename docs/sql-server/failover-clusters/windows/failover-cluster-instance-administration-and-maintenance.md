---
title: Gestione e manutenzione delle istanze del cluster di failover
description: Informazioni sulla manutenzione e l'amministrazione delle istanze dei cluster di failover di SQL Server con il programma di installazione di SQL Server o lo snap-in Gestione cluster di failover.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- user accounts [SQL Server], failover clustering
- clusters [SQL Server], maintaining
- nodes [Faillover Clustering]
- failover clustering [SQL Server], maintaining
- adding nodes
- virtual servers [SQL Server], removing nodes
- clustered instance of SQL Server
- nodes [Faillover Clustering], removing
- nodes [Faillover Clustering], adding
- service accounts [SQL Server]
- removing nodes
- virtual servers [SQL Server], adding nodes
ms.assetid: 2d5c63e9-8061-45c3-94db-8dd3100b8a91
author: cawrites
ms.author: chadam
ms.openlocfilehash: fb546ec95946e2dc534dfa16a51807b86077b322
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642711"
---
# <a name="failover-cluster-instance-administration-and-maintenance"></a>Gestione e manutenzione dell'istanza del cluster di failover
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Le attività di manutenzione quali l'aggiunta o la rimozione di nodi da un'istanza del cluster di failover (FCI) AlwaysOn esistente vengono portate a termine usando il programma di installazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Le altre attività dell'amministrazione, quali la modifica delle risorse indirizzo IP, il recupero da determinati scenari di FCI, vengono eseguite mediante lo snap-in Gestione cluster di failover che è lo snap-in di gestione per il servizio Windows Server Failover Clustering (WSFC).  
  
## <a name="maintaining-a-failover-cluster-instance"></a>Gestione un'istanza del cluster di failover  
 Dopo avere installato un'istanza FCI, è possibile modificarlo o ripristinarlo tramite il programma di installazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . È possibile, ad esempio, aggiungere ulteriori nodi a un'istanza FCI, eseguire un'istanza FCI come istanza autonoma o rimuovere un nodo da una configurazione di un'istanza FCI.  
  
### <a name="adding-a-node-to-an-existing-failover-cluster-instance"></a>Aggiunta di un nodo a un'istanza del cluster di failover esistente  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] è possibile scegliere l'opzione di manutenzione di un'istanza FCI esistente. Se si sceglie questa opzione, è possibile aggiungere altri nodi all'istanza di FCI eseguendo l'installazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nel computer che si vuole aggiungere all'istanza di FCI. Per altre informazioni, vedere [Creare un nuovo cluster di failover di SQL Server &#40;programma di installazione&#41;](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md) e [Aggiungere o rimuovere nodi in un cluster di failover di SQL Server &#40;programma di installazione&#41;](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md).  
  
### <a name="removing-a-node-from-an-existing-failover-cluster-instance"></a>Rimozione di un nodo da un'istanza del cluster di failover esistente  
 È possibile rimuovere un nodo da una configurazione dell'istanza FCI eseguendo l'installazione [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nel computer che si desidera rimuovere dall'istanza FCI. Ogni nodo di un'istanza FCI è considerato come peer senza dipendenze da altri nodi nell'istanza FCI e può essere rimosso. Per poter essere rimosso, un nodo danneggiato non deve necessariamente essere disponibile. Se il nodo non è disponibile, non verranno disinstallati i file binari di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Un nodo rimosso può essere nuovamente aggiunto a 'istanza FCI in qualsiasi momento. Per altre informazioni, vedere [Aggiungere o rimuovere nodi in un cluster di failover di SQL Server &#40;programma di installazione&#41;](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md).  
  
### <a name="changing-service-accounts"></a>Modifica degli account del servizio  
 È consigliabile evitare di modificare le password degli account del servizio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] quando un nodo dell'istanza FCI è inattivo oppure offline. Se, tuttavia, non è possibile evitare tale modifica, sarà necessario reimpostare la password utilizzando Gestione configurazione [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] quando tutti i nodi saranno di nuovo online.  
  
 Se l'account del servizio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] non è un amministratore nel cluster, non sarà possibile eliminare le condivisioni amministrative in nessun nodo del cluster. Per il corretto funzionamento di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , è necessario che le condivisioni amministrative siano disponibili in un cluster.  
  
> [!IMPORTANT]  
>  Non utilizzare lo stesso account come account del servizio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e account del servizio WSFC. Se viene modificata la password dell'account del servizio WSFC, non sarà possibile completare l'installazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
 In [!INCLUDE[nextref_longhorn](../../../includes/nextref-longhorn-md.md)], i SID del servizio vengono utilizzati per gli account del servizio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Per altre informazioni, vedere [Configurare account di servizio e autorizzazioni di Windows](../../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
## <a name="administering-a-failover-cluster-instance"></a>Amministrazione di un'istanza del cluster di failover  
  
|Descrizione dell'attività|Collegamento all'argomento|  
|----------------------|----------------|  
|Viene descritto come aggiungere dipendenze a una risorsa di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|[Aggiungere dipendenze a una risorsa di SQL Server](../../../sql-server/failover-clusters/windows/add-dependencies-to-a-sql-server-resource.md)|  
|Kerberos è un protocollo di autenticazione di rete progettato per fornire un'autenticazione avanzata alle applicazioni client/server. Oltre a offrire un'infrastruttura di interoperabilità, il protocollo Kerberos contribuisce ad aumentare la sicurezza dell'autenticazione estesa all'intera azienda. È possibile usare l'autenticazione Kerberos con le istanze autonome di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] o con le istanze FCI AlwaysOn.|[Registrazione di un nome dell'entità servizio per le connessioni Kerberos](../../../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md).|  
|Vengono forniti i collegamenti a contenuti che descrivono come abilitare l'autenticazione Kerberos||  
|Viene descritta la procedura utilizzata per recuperare da un errore di un cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|[Recuperare da un errore dell'istanza del cluster di failover](../../../sql-server/failover-clusters/windows/recover-from-failover-cluster-instance-failure.md)|  
|Descrivere la procedura utilizzata per modificare la risorsa indirizzo IP per un'istanza del cluster di failover [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|[Modificare l'indirizzo IP di un'istanza del cluster di failover](../../../sql-server/failover-clusters/windows/change-the-ip-address-of-a-failover-cluster-instance.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Configurazione delle impostazioni HealthCheckTimeout](../../../sql-server/failover-clusters/windows/configure-healthchecktimeout-property-settings.md)   
 [Configurare le impostazioni della proprietà FailureConditionLevel](../../../sql-server/failover-clusters/windows/configure-failureconditionlevel-property-settings.md)   
 [Visualizzare e leggere il log di diagnostica dell'istanza del cluster di failover](../../../sql-server/failover-clusters/windows/view-and-read-failover-cluster-instance-diagnostics-log.md)  
  
  
