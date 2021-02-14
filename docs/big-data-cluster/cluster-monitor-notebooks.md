---
title: Monitorare il cluster con i notebook Jupyter e Azure Data Studio
titleSuffix: SQL Server Big Data Clusters
description: Monitoraggio del cluster con i notebook Jupyter e Azure Data Studio in un cluster Big Data di SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 10/01/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: bc370b42f247c4b8e57c5628feaae7385ba1502d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039374"
---
# <a name="monitoring-cluster-with-notebooks"></a>Monitoraggio del cluster con i notebook

Questa pagina è un indice dei notebook per i cluster Big Data di SQL Server. I notebook eseguibili (con estensione ipynb) sono progettati per facilitare il monitoraggio dei cluster Big Data in SQL Server 2019.

Ogni notebook è progettato per cercare le proprie dipendenze. Un comando **Run all cells** (Esegui tutte le celle) viene eseguito correttamente oppure genera un'eccezione con un hint con collegamento ipertestuale a un altro notebook per risolvere la dipendenza mancante. Seguire il collegamento ipertestuale dell'hint al notebook successivo, scegliere **Run all cells** (Esegui tutte le celle) e, al termine, tornare al notebook originale e scegliere di nuovo **Run all cells** (Esegui tutte le celle).

Se vengono installate tutte le dipendenze, ma il comando **Run all cells** (Esegui tutte le celle) ha esito negativo, ciascun notebook analizzerà i risultati e, laddove possibile, produrrà un suggerimento di collegamento ipertestuale a un altro notebook da cui ottenere un aiuto nella risoluzione del problema.


## <a name="monitoring-kubernetes"></a>Monitoraggio di Kubernetes

Questa sezione contiene un set di notebook utili per ottenere informazioni e stato relativi a un cluster Big Data di SQL Server usando l'interfaccia della riga di comando di `azdata`.

|Nome |Descrizione |
|---|---|---|---|
|TSG006 - Ottenere lo stato dei pod di sistema|Visualizzare lo stato di tutti i pod di sistema. |
|TSG007 - Ottenere lo stato dei pod BDC|Visualizzare lo stato dei pod del cluster Big Data.|
|TSG008 - Ottenere informazioni sulla versione (Kubernetes)|Ottenere informazioni sul cluster Kubernetes.|
|TSG009 - Ottenere i nodi (Kubernetes)|Ottenere i contesti di Kubernetes. |
|TSG010 - Ottenere i contesti di configurazione|Uso della query DMV per ottenere altre informazioni sull'errore interno di Query Processor|
|TSG015 - Visualizzare i servizi BDC (Kubernetes)|Ottenere lo stato dei servizi del cluster Big Data (BDC) distribuito nel cluster Kubernetes. |
|TSG016 - Descrizione dei Pod BDC|Ottenere lo stato dei pod del cluster Big Data (BDC) distribuito nel cluster Kubernetes. |
|TSG020 - Descrivere i nodi (Kubernetes)|Ottenere le informazioni sui nodi per il cluster Big Data (BDC) usando l'interfaccia della riga di comando kubectl. |
|TSG021 - Ottenere informazioni sul cluster (Kubernetes)|Ottenere informazioni sul cluster Kubernetes. |
|TSG022 - Ottenere l'indirizzo IP esterno per l'host di kubeadm|Ottenere l'indirizzo IP esterno dell'host di kubeadm. |
|TSG023 - Ottenere tutti gli oggetti di BDC (Kubernetes)|Ottenere un riepilogo di tutte le risorse Kubernetes per lo spazio dei nomi di sistema e lo spazio dei nomi del cluster Big Data. |
|TSG042 - Ottenere il nome del nodo e i montaggi esterni per le PVC dei dati e dei log|Ottenere il nome del nodo che ospita il pod insieme ai montaggi esterni di dati e log. |
|TSG063 - Ottenere le classi di archiviazione (Kubernetes)|Usare questo notebook per ottenere le classi di archiviazione Kubernetes. |
|TSG064 - Ottenere attestazioni di volume permanente per il cluster Big Data (BDC)|Visualizzare le attestazioni di volume permanente (PVC) per il cluster Big Data. |
|TSG065 - Ottenere i segreti di BDC (Kubernetes)|Visualizzare i segreti del cluster Big Data. |
|TSG066 - Ottenere l'evento di BDC (Kubernetes)|Visualizzare gli eventi del cluster Big Data.|
|TSG072 - Ottenere i volumi permanenti (Kubernetes)|Visualizzare i volumi permanenti (PV) per il cluster Kubernetes. I volumi permanenti sono oggetti non definiti negli spazi dei nomi. |
|TSG081 - Ottenere gli spazi dei nomi (Kubernetes)|Ottenere gli spazi dei nomi Kubernetes. |
|TSG089 - Descrivere i pod BDC non in esecuzione|Visualizzare i pod BDC non in esecuzione per il cluster Kubernetes. |
|TSG097 - Ottenere i set con stato di BDC (Kubernetes)|Visualizzare i set con stato di BDC per il cluster Kubernetes. |
|TSG098 - Ottenere i set di repliche di BDC (Kubernetes)|Visualizzare i set di repliche di BDC per il cluster Kubernetes. |
|TSG099 - Ottenere i DaemonSet di BDC (Kubernetes)|Visualizzare i DaemonSet di BDC per il cluster Kubernetes. |


## <a name="monitor-big-data-cluster-bdc"></a>Monitoraggio del cluster Big Data (BDC)

Questa sezione contiene un set di notebook utili per ottenere informazioni e stato relativi al cluster Kubernetes che ospita un cluster Big Data (BDC) di SQL Server.

|Nome |Descrizione |
|---|---|---|---|
|TSG003 - Visualizzare le sessioni Spark di BDC|Visualizzare le sessioni Spark di BDC. |
|TSG004 - Visualizzare le app BDC|Visualizzare le app in esecuzione in un cluster Big Data (BDC).|
|TSG012 - Visualizzare lo stato di BDC|Ottenere lo stato corrente dei diversi componenti del cluster Big Data (BDC).|
|TSG013 - Visualizzare l'elenco dei file nel pool di archiviazione (HDFS)|Ottenere l'elenco dei file nel pool di archiviazione (HDFS). |
|TSG014 - Visualizzare gli endpoint BDC|Ottenere tutti gli endpoint disponibili per il cluster Big Data (BDC).|
|TSG017 - Visualizzare la configurazione di BDC|Ottenere la configurazione del cluster Big Data (BDC). |
|TSG033 - Visualizzare lo stato SQL di BDC|Ottenere lo stato SQL Server del cluster Big Data (BDC) distribuito nel cluster Kubernetes. |
|TSG049 - Visualizzare lo stato del controller di BDC|Ottenere lo stato del controller del cluster Big Data (BDC) distribuito nel cluster Kubernetes. |
|TSG068 - Visualizzare lo stato HDFS di BDC|Ottenere lo stato HDFS del cluster Big Data (BDC) distribuito nel cluster Kubernetes. |
|TSG069 - Visualizzare lo stato del gateway del cluster Big Data|Ottenere lo stato del gateway BDC del cluster Big Data (BDC) distribuito nel cluster Kubernetes. |
|TSG070 - Eseguire query sul pool master SQL| Eseguire una query SQL sull'istanza master. |

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vedere [Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).
