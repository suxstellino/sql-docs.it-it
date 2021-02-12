---
title: Gestire i cluster Big Data nel cluster privato del servizio Azure Kubernetes
titleSuffix: SQL Server Big Data Cluster
description: Informazioni sulla gestione dei cluster Big Data di SQL Server nel cluster privato del servizio Azure Kubernetes.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 08/20/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 768818a9e034961aa0460515e3db84ce3a4a8092
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043871"
---
# <a name="manage-big-data-cluster-in-aks-private-cluster"></a>Gestire cluster Big Data nel cluster privato del servizio Azure Kubernetes

Questo articolo illustra come gestire un cluster privato del servizio Azure Kubernetes con i cluster Big Data di SQL Server distribuiti in Azure.

Come descritto in [Creare un cluster privato](/azure/aks/private-clusters/), l'endpoint del server API del cluster privato del servizio Azure Kubernetes non ha un indirizzo IP pubblico. Per gestire il server API, usare una macchina virtuale che abbia accesso alla rete virtuale di Azure (VNet) dei cluster del servizio Azure Kubernetes.

## <a name="azure-vm---same-vnet"></a>Macchina virtuale di Azure - stessa VNet

Il metodo più semplice consiste nel distribuire una macchina virtuale di Azure nella stessa VNet del cluster del servizio Azure Kubernetes.

1. Distribuire una macchina virtuale di Azure nella stessa VNet con il cluster del servizio Azure Kubernetes. Questa operazione viene talvolta chiamata *jumpbox*.
1. Connettersi a tale macchina virtuale e [installare gli strumenti per Big Data di SQL Server 2019](deployment-guidance.md#install-sql-server-2019-big-data-tools).

Per motivi di sicurezza, è possibile usare le funzionalità del servizio Azure Kubernetes per gli intervalli IP autorizzati del server API per limitare l'accesso al server API (sul piano di controllo del servizio Azure Kubernetes). L'accesso limitato consente indirizzi IP specifici, ad esempio una macchina virtuale jumpbox o di gestione, oppure un intervallo di indirizzi IP per un gruppo di sviluppatori e l'indirizzo IP front-end pubblico del firewall.

## <a name="other-options"></a>Altre opzioni

Le alternative all'uso di un jumpbox includono:

* Usare una macchina virtuale in una rete separata e configurare il [Peering di rete virtuale](/azure/virtual-network/virtual-network-peering-overview) per la VNet.

* Connessione con [Azure ExpressRoute](/azure/expressroute/expressroute-introduction) o [Gateway VPN](/azure/vpn-gateway/vpn-gateway-about-vpngateways).

   Tutti questi metodi sono illustrati in [Opzioni per la connessione al cluster privato](/azure/aks/private-clusters#options-for-connecting-to-the-private-cluster).

* Se il servizio è in esecuzione dietro [Azure Load Balancer Standard](/azure/aks/load-balancer-standard) può essere abilitato per il [collegamento privato di Azure](/azure/private-link/private-link-service-overview#limitations). Con il collegamento privato di Azure è possibile abilitare l'accesso privato da altri reti virtuali di Azure.

* In uno scenario ibrido è anche possibile configurare la connessione con [Azure ExpressRoute](/azure/expressroute/expressroute-introduction) o [Gateway VPN](/azure/vpn-gateway/vpn-gateway-about-vpngateways).

## <a name="next-steps"></a>Passaggi successivi

[Connettersi a un cluster Big Data di SQL Server con Azure Data Studio](connect-to-big-data-cluster.md)