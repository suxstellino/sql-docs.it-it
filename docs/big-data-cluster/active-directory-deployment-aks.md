---
title: Eseguire la distribuzione in Active Directory nei servizi Azure Kubernetes
titleSuffix: SQL Server Big Data Cluster
description: Presentazioni dei concetti e informazioni sulla pianificazione della distribuzione di cluster Big Data di SQL Server in modalità AD nei servizi Azure Kubernetes (AKS).
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 11/12/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a0407589eefab4d5e997d582fee900a20dfcb0a3
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837012"
---
# <a name="deploy-sql-server-big-data-clusters-in-ad-mode-on-azure-kubernetes-services-aks"></a>Distribuire cluster Big Data di SQL Server in modalità AD nei servizi Azure Kubernetes (AKS)

I cluster Big Data di SQL Server supportano la [modalità di distribuzione Active Directory (AD)](./active-directory-prerequisites.md) per la **gestione delle identità e degli accessi (IAM)** . La gestione delle identità e degli accessi per il **servizio Azure Kubernetes** si è rilevata complicata perché i protocolli standard del settore come OAuth 2.0 e OpenID Connect, ampiamente supportati dalla piattaforma per la gestione delle identità Microsoft, non sono supportati da SQL Server.  

Questo articolo illustra come distribuire un cluster Big Data in modalità AD durante la distribuzione nel [servizio Azure Kubernetes](/azure/aks/intro-kubernetes). 

## <a name="architecture-topologies"></a>Topologie di architettura

Il servizio **Active Directory Domain Services (AD DS)** viene eseguito in una macchina virtuale di Azure [nello stesso modo](/windows-server/identity/ad-ds/deploy/virtual-dc/adds-on-azure-vm) in cui viene eseguito in molte istanze locali.  Dopo aver alzato di livello i nuovi controller di dominio in Azure, impostare i server DNS primario e secondario per la rete virtuale. Gli eventuali server DNS locali verranno abbassati al livello terziario o inferiore. L'autenticazione di AD consente ai client aggiunti a un dominio in [Linux di eseguire l'autenticazione per SQL Server](../linux/sql-server-linux-active-directory-auth-overview.md) usando le proprie credenziali di dominio e il protocollo Kerberos.

È possibile procedere in vari modi per distribuire un cluster Big Data in modalità AD nel servizio Azure Kubernetes.  Questo articolo presenta due metodi, che sono più facili da implementare e integrare con le architetture di livello aziendale esistenti:

* **Estendere il dominio di Active Directory locale ad Azure.** Questo metodo [consente all'ambiente Active Directory](/azure/architecture/reference-architectures/identity/adds-extend-domain) di fornire servizi di autenticazione distribuiti usando Active Directory Domain Services (AD DS) in Azure. È possibile replicare l'istanza locale del servizio Active Directory Domain Services (AD DS) per ridurre la latenza causata dall'invio di richieste di autenticazione dal cloud al servizio AD DS locale. Un caso d'uso tipico per questa soluzione è quando l'applicazione è ospitata in parte in locale e in parte in Azure e le richieste di autenticazione devono essere trasferite avanti e indietro.

   Indicazioni dettagliate su come distribuire questa soluzione sono disponibili [in questa architettura di riferimento](https://github.com/mspnp/identity-reference-architectures/tree/master/adds-extend-domain).

* **Estendere la foresta di risorse di Active Directory Domain Services (AD DS) ad Azure.** In questa architettura viene creato un nuovo dominio in Azure considerato attendibile dalla foresta di Active Directory locale. Questa architettura mostra una [relazione di trust unidirezionale dal dominio in Azure alla foresta locale](/azure/architecture/reference-architectures/identity/adds-forest).

   Il trust consente agli utenti locali di accedere alle risorse nel dominio in Azure. Indicazioni dettagliate su come distribuire questa soluzione sono disponibili [in questa architettura di riferimento](https://github.com/mspnp/identity-reference-architectures/tree/master/adds-forest).

Le architetture di riferimento descritte in precedenza consentono di creare una zona di destinazione che dispone di tutte le risorse da distribuire da zero o di eventuali soluzioni aggiuntive basate sull'architettura esistente. Oltre a queste architetture di riferimento, è necessario distribuire il cluster Big Data in un cluster AKS in una subnet separata che rimanga nella VNet di destinazione o in una VNet con peering.

L'immagine seguente rappresenta un'architettura tipica:

:::image type="content" source="media/active-directory-deployment-aks/ad-in-aks-diagram.png" alt-text="Cluster AKS con AD e cluster Big Data di SQL Server":::

## <a name="recommendations"></a>Consigli

Le indicazioni seguenti sono valide per la maggior parte delle distribuzioni di cluster Big Data in modalità AD nel servizio Azure Kubernetes. Le opzioni disponibili verranno indicate in ogni componente per fornire indicazioni per una migliore integrazione con l'architettura di livello aziendale.

### <a name="networking-recommendations"></a>Raccomandazioni di rete

È possibile usare alcuni componenti chiave per connettere l'ambiente locale ad Azure:

* **Gateway VPN di Azure**: un gateway VPN è un tipo specifico di gateway di rete virtuale, usato per inviare traffico crittografato tra una rete virtuale di Azure e una posizione locale attraverso la rete Internet pubblica. Verranno usati sia il gateway di rete virtuale di Azure che il gateway di rete virtuale locale. Per informazioni su come configurarli, vedere [Che cos'è un Gateway VPN?](/azure/vpn-gateway/vpn-gateway-about-vpngateways).
* **Azure ExpressRoute**: le connessioni ExpressRoute non sfruttano la rete Internet pubblica e offrono un livello di sicurezza superiore, maggiore affidabilità, velocità più elevate e minori latenze rispetto alle connessioni Internet tradizionali. La scelta dell'opzione di connettività influirà sulla latenza, sulle prestazioni e sul livello di SLA della soluzione a seconda degli SKU. Per informazioni specifiche, vedere [Informazioni sui gateway di rete virtuale per ExpressRoute](/azure/expressroute/expressroute-about-virtual-network-gateways).

Per accedere ad altre infrastrutture di Azure, la maggior parte dei clienti usa una JumpBox o [Azure Bastion](/azure/bastion/bastion-overview). Il **collegamento privato di Azure** consente di accedere in modo sicuro ai servizi PaaS, di Azure, incluso il servizio Azure Kubernetes in questo scenario, oltre ad altri servizi ospitati di Azure tramite un endpoint privato nella rete virtuale. Il traffico tra la rete virtuale e il servizio attraversa la rete backbone Microsoft, impedendone l'esposizione alla rete Internet pubblica. È anche possibile creare un servizio collegamento privato personale nella rete virtuale e distribuirlo privatamente ai clienti.

### <a name="active-directory-and-azure-recommendation"></a>Consigli per Active Directory e Azure

Il servizio AD DS locale archivia le informazioni sugli account utente e consente ad altri utenti autorizzati nella stessa rete di accedere a tali informazioni autenticando le identità associate a utenti, computer, applicazioni o altre risorse incluse in un limite di sicurezza. Nella maggior parte degli scenari ibridi, l'autenticazione utente viene eseguita tramite un gateway VPN o una connessione ExpressRoute all'ambiente AD DS locale.  

Per la distribuzione di un cluster Big Data in modalità AD, la soluzione per [integrare Active Directory locale con Azure](/azure/architecture/reference-architectures/identity/) deve soddisfare i prerequisiti seguenti:

* Un [account di AD con autorizzazioni specifiche](active-directory-prerequisites.md) per creare account utente, di gruppo e computer all'interno dell'unità organizzativa (OU) specificata nell'istanza di Active Directory locale.
* Un server DNS per [risolvere le voci DNS interne](active-directory-dns-reconciliation.md). Deve contenere sia **record A (ricerca diretta)** che **record PTR (ricerca inversa)** nel server DNS con nomi in questo dominio. Specificare le impostazioni DNS del dominio nel profilo di distribuzione del cluster Big Data.  

## <a name="next-steps"></a>Passaggi successivi

[Esercitazione: Distribuire cluster Big Data di SQL Server in modalità AD nei servizi Azure Kubernetes (AKS)](active-directory-deployment-aks-tutorial.md)