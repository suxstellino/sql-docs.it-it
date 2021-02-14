---
title: Riconciliazione del DNS di Active Directory e Kubernetes nelle distribuzioni di cluster Big Data
description: Configurare la riconciliazione DNS per un cluster Big Data di SQL Server in modalità Active Directory
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 09/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a20afc81637eba30427b7b1062738b2d81ae67a0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047881"
---
# <a name="active-directory-and-kubernetes-dns-reconciliation-in-big-data-clusters-deployments"></a>Riconciliazione del DNS di Active Directory e Kubernetes nelle distribuzioni di cluster Big Data

Questo articolo descrive alcuni dei problemi relativi alla gestione dell'integrazione di Active Directory durante la distribuzione di cluster Big Data (BDC) e illustra le possibili soluzioni.

## <a name="overview"></a>Panoramica

Quando il cluster Big Data non viene distribuito con l'integrazione di Active Directory, per le risoluzioni DNS interne ci si basa sul servizio [CoreDNS di Kubernetes](https://kubernetes.io/docs/tasks/administer-cluster/coredns/). Kubernetes usa un dominio interno, ad esempio `<namespace>.svc.cluster.local`. Crea record A (ricerca diretta) e PTR (ricerca inversa) nel server DNS con nomi in questo dominio.

Tuttavia, quando è abilitata la modalità Active Directory, viene introdotto un nuovo dominio con uno specifico set di server DNS. Durante la risoluzione dei nomi interna, questo può generare confusione riguardo al set di server DNS da usare per le ricerche dirette e inverse.

## <a name="challenges"></a>Problematiche

* Quando vengono distribuiti nuovi pod Kubernetes, è necessario aggiungere voci DNS in entrambi i set di server DNS. Kubernetes esegue la registrazione delle voci nel relativo servizio CoreDNS, ma il flusso di lavoro di distribuzione del cluster BDC è responsabile dell'aggiunta delle voci richieste nei server DNS del controller di dominio di Active Directory. Analogamente, quando un cluster BDC viene eliminato, il flusso di lavoro deve verificare che tali voci vengano rimosse.
* I server DNS di Active Directory sono esterni al cluster Kubernetes. Il cluster BDC ha tuttavia uno specifico spazio IP all'interno di Kubernetes e non è in grado di creare record per questo spazio IP in un server DNS situato esternamente, perché lo spazio IP non è visibile all'esterno dei limiti del cluster.
* Quando si verificano eventi di failover all'interno del cluster Kubernetes, anche i record nei server DNS di Active Directory devono essere aggiornati.
* Oltre ai nomi dei pod, è necessario che anche i nomi dei servizi Kubernetes siano indirizzabili tramite le ricerche dei nomi di dominio di Active Directory. Ciò costituisce un ulteriore problema per il servizio DNS di Active Directory perché un nome di servizio può essere mappato a più indirizzi IP di pod.
* La registrazione della propagazione degli aggiornamenti e dei ritardi di replica nei server DNS di Active Directory dell'organizzazione può avere un impatto significativo, al di fuori del controllo dei flussi di lavoro di gestione del cluster BDC. Ciò può influire immediatamente sulle funzionalità del cluster BDC in fase di distribuzione e failover. Al contrario, il servizio CoreDNS di Kubernetes è più veloce ed efficiente grazie alla sua posizione locale.

## <a name="solution"></a>Soluzione

Per aggirare i problemi sopra illustrati, la soluzione implementata nel cluster BDC prevede un nuovo servizio CoreDNS interno gestito all'interno dello spazio dei nomi del cluster BDC. Questo è l'unico servizio DNS che i pod nello spazio dei nomi del cluster BDC raggiungeranno per le risoluzioni dei nomi. La complessità di più domini rimane nascosta dietro il nuovo servizio CoreDNS.

Nel diagramma seguente, ad esempio, i pod usano il server CoreDNS del cluster BDC per risolvere i nomi. I pod non interagiscono direttamente con il server CoreDNS di Kubernetes o il server DNS di Active Directory. 

:::image type="content" source="media/active-directory-dns-reconciliation/bdc-ad-dns-reconciliation.png" alt-text="I pod si connettono al server CoreDNS nel proprio spazio dei nomi":::

Di seguito sono riportati alcuni dettagli relativi all'implementazione che chiariscono il funzionamento di questa progettazione nel cluster BDC:

### <a name="centralized-management-of-multiple-domains"></a>Gestione centralizzata di più domini

La complessità di ciò che accade nelle ricerche dei nomi rimane nascosta dietro il servizio DNS interno in modo centralizzato. Questo evita di dover gestire più domini su singoli pod e semplifica l'attività di progettazione.

### <a name="no-records-for-internal-pods-in-external-dns-servers"></a>Nessun record per i pod interni nei server DNS esterni

Come risultato di questo principio di progettazione, il cluster BDC non dovrà creare e gestire record A e PTR per i pod nello spazio IP di Kubernetes nei server DNS esterni.

### <a name="no-duplication-of-records"></a>Nessuna duplicazione di record

I record DNS interni si trovano in più posizioni. L'unica risorsa di archiviazione per questi record è il servizio CoreDNS di Kubernetes. Il servizio CoreDNS interno del cluster BDC esegue un'attività di riscrittura computazionale e di inoltro di query DNS al servizio CoreDNS di Kubernetes.

### <a name="computational-rewriting"></a>Riscrittura computazionale

Poiché il cluster BDC non archivia alcun record, è responsabile della conversione delle query di ricerca diretta in ingresso con nomi di dominio di Active Directory nei nomi di dominio di Kubernetes e dell'inoltro di tali query al servizio CoreDNS di Kubernetes.
Ad esempio, una query in ingresso per `compute-0-0.contoso.local` verrebbe riscritta in `compute-0-0.compute-0-svc.contoso.svc.cluster.local` e questa richiesta verrebbe inoltrata al servizio CoreDNS di Kubernetes.
In caso di ricerche inverse, la richiesta viene inoltrata con indirizzi IP interni, come per il servizio CoreDNS di Kubernetes, e la risposta viene riscritta da tale servizio al nome di dominio di Active Directory prima di essere inoltrata al client.

### <a name="simplicity-in-pod-configurations"></a>Semplicità nelle configurazioni dei pod

Poiché nel file /etc/resolv.conf di tutti i pod del cluster BDC viene fatto riferimento solo al servizio CoreDNS di tale cluster, questo semplifica la visualizzazione della rete dai pod. La complessità rimane invece nascosta nel servizio CoreDNS interno.

### <a name="static-and-reliable-ip-address-for-dns-service"></a>Indirizzo IP statico e affidabile per il servizio DNS

Per il servizio CoreDNS distribuito dal cluster BDC viene registrato un indirizzo IP interno statico accessibile da tutti i pod. Ciò consente di assicurarsi che i valori in /etc/resolv.conf non devono essere aggiornati.

### <a name="service-load-balance-management-is-retained-by-kubernetes"></a>Bilanciamento del carico dei servizi gestito da Kubernetes

Quando si eseguono ricerche di servizi invece che di singoli pod, queste vengono comunque indirizzate al servizio CoreDNS di Kubernetes e quindi il cluster BDC non è responsabile dell'implementazione del bilanciamento del carico in modo specifico per il dominio di Active Directory.

Se, ad esempio, viene ricevuta una richiesta di ricerca diretta per `compute-0-svc.contoso.local`, questa verrà riscritta in ` compute-0-svc.contoso.svc.cluster`.local. La richiesta verrà inoltrata al servizio CoreDNS di Kubernetes e il bilanciamento del carico verrà eseguito nell'ambito di tale servizio. Come risposta verrà generato un indirizzo IP per una delle molteplici istanze del pool di calcolo (repliche di pod).

### <a name="scalability"></a>Scalabilità

Poiché nel cluster BDC non viene archiviato alcun record, è possibile ridimensionare il servizio CoreDNS del cluster BDC senza conservazione dello stato e replica dei record su più repliche. Se i record DNS verranno archiviati nel cluster BDC, sarà necessario eseguire la replica dello stato su tutti i pod anche per il cluster BDC.

### <a name="externally-visible-service-entries-stay-in-ad-dns"></a>Voci dei servizi visibili esternamente mantenute nel DNS di Active Directory

Per gli endpoint dei servizi che devono essere accessibili ai client all'esterno del cluster Kubernetes, le voci DNS verranno create nel server DNS di Active Directory durante la distribuzione del cluster BDC. L'utente immetterà i nomi DNS da cui eseguire la registrazione tramite i profili di configurazione della distribuzione.

### <a name="self-deprovisioning"></a>Deprovisioning automatico

Dopo l'eliminazione del cluster BDC, non sono previste altre operazioni dinamiche per eliminare le voci DNS durante il deprovisioning del cluster. Le uniche voci del DNS di Active Directory remoto che devono essere pulite riguardano i servizi esterni e sono statiche dal punto di vista numerico. Le voci del servizio DNS interno vengono rimosse automaticamente con il cluster.

## <a name="next-steps"></a>Passaggi successivi

- [Distribuire un cluster Big Data di SQL Server in modalità Active Directory](active-directory-deploy.md)
- [Gestire l'accesso al cluster Big Data in modalità Active Directory](active-directory-objects.md)
- [Distribuire più cluster Big Data di SQL Server nello stesso dominio di Active Directory](active-directory-deployment-background.md)
