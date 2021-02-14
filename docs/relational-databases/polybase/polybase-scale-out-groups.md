---
title: Gruppi con scalabilità orizzontale di PolyBase | Microsoft Docs
description: Usare la funzionalità per i gruppi di PolyBase per creare un cluster di istanze di SQL Server. In questo modo è possibile migliorare le prestazioni delle query per set di dati di grandi dimensioni da origini esterne.
ms.date: 04/23/2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
f1_keywords:
- sql13.swb.polybasescaleoutcluster.page.f1
helpviewer_keywords:
- PolyBase
- PolyBase, scale-out groups
- scale-out PolyBase
ms.assetid: c7810135-4d63-4161-93ab-0e75e9d10ab5
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: c7c2594dfd40ff952c28cf203e05f37c789b3d06
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076662"
---
# <a name="polybase-scale-out-groups"></a>Gruppi con scalabilità orizzontale di PolyBase

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Un'istanza di SQL Server autonomo con PolyBase può diventare un collo di bottiglia in termini di prestazioni quando si lavora con grandi set di dati in Hadoop o in archiviazione BLOB di Azure. La funzionalità Gruppo di PolyBase consente di creare un cluster di istanze di SQL Server per elaborare grandi quantità di set di dati da origini dati esterne, come ad esempio Hadoop o archiviazione BLOB di Azure, in un meccanismo di scalabilità orizzontale che consente di migliorare le prestazioni delle query. È ora possibile ridimensionare le risorse di calcolo di SQL Server per soddisfare le esigenze di prestazioni del carico di lavoro. I gruppi con scalabilità orizzontale PolyBase, ovvero un gruppo di istanze di SQL Server, consentono di elaborare grandi set di dati esterni in un'architettura di elaborazione parallela. Le prestazioni di caricamento dei dati e query possono aumentare in modo lineare con l'aggiunta di altre istanze di SQL Server al gruppo. 
  
Vedere [Get started with PolyBase](./polybase-guide.md) (Introduzione a PolyBase) e [PolyBase Guide](../../relational-databases/polybase/polybase-guide.md)(Guida di Polybase).
  
![Diagramma che mostra i gruppi con scalabilità orizzontale di PolyBase.](../../relational-databases/polybase/media/polybase-scale-out-groups.png "Gruppi con scalabilità orizzontale di PolyBase")  
  
## <a name="head-node"></a>Nodo head  

Il nodo head contiene l'istanza di SQL Server alla quale vengono inviate le query PolyBase. Ogni gruppo di PolyBase può avere un solo nodo head. Un nodo head è un gruppo logico costituito dal motore di database SQL Server, dal motore PolyBase e da PolyBase Data Movement Service nell'istanza di SQL Server. Con SQL Server 2017 e SQL Server 2016, il nodo head deve essere un'edizione Enterprise. A partire da SQL Server 2019 il nodo head di PolyBase può essere un'edizione Enterprise o Standard.
  
## <a name="compute-node"></a>Nodo di calcolo

Un nodo di calcolo contiene l'istanza di SQL Server che assiste nell'elaborazione delle query di scalabilità orizzontale sui dati esterni. Un nodo di calcolo è un gruppo logico costituito da SQL Server e da PolyBase Data Movement Service nell'istanza di SQL Server. Un gruppo di PolyBase può presentare più nodi di calcolo. Il nodo head e tutti i nodi di calcolo devono eseguire la stessa versione di SQL Server. La versione iniziale di SQL Server 2016 consentiva ai nodi di calcolo di essere un'edizione Enterprise o Standard. A partire da SQL Server 2016 SP1, tutte le edizioni di SQL Server possono essere un nodo di calcolo.

## <a name="scale-out-reads"></a>Letture con scalabilità orizzontale

Quando si eseguono query su istanze esterne di SQL Server, Oracle o Teradata, le tabelle partizionate trarranno vantaggio dalle letture con scalabilità orizzontale. Ogni nodo in un gruppo con scalabilità orizzontale PolyBase può alternare fino a 8 lettori per la lettura dei dati esterni. E a ogni lettore viene assegnata una partizione per la lettura nella tabella esterna. 

Ad esempio, si supponga di avere una tabella esterna di SQL Server con 12 partizioni mensili e un gruppo con scalabilità orizzontale PolyBase da 3 nodi. Ogni nodo userà 4 lettori di PolyBase per l'elaborazione di ciascuna delle 12 partizioni, come illustrato nella figura seguente. 

> [!NOTE]
>  Questa funzionalità è diversa dalle letture con scalabilità orizzontale su Hadoop. 

![Letture con scalabilità orizzontale di PolyBase](../../relational-databases/polybase/media/polybase-scale-out-groups2.png "Gruppi con scalabilità orizzontale di PolyBase")
  
## <a name="distributed-query-processing"></a>Elaborazione delle query distribuite  

Le query PolyBase vengono inviate a SQL Server nel nodo head. La parte della query che fa riferimento a tabelle esterne viene passata al motore PolyBase.
  
Il motore PolyBase è il componente principale alla base delle query PolyBase. Il motore, infatti, analizza la query su dati esterni, genera il piano di query e distribuisce il lavoro al servizio di spostamento dati nei nodi di calcolo ai fini dell'esecuzione. Dopo il completamento del lavoro, riceve i risultati dei nodi di calcolo e li invia a SQL Server per l'elaborazione e la restituzione al client.
  
Il servizio di spostamento dati di PolyBase riceve istruzioni dal motore PolyBase e trasferisce i dati tra HDFS e SQL Server e tra istanze di SQL Server nei nodi head e di calcolo.
  
## <a name="next-steps"></a>Passaggi successivi

Per configurare un gruppo con scalabilità orizzontale PolyBase, vedere la guida seguente:

[Migliorare i gruppi con scalabilità orizzontale PolyBase in Windows](configure-scale-out-groups-windows.md)

## <a name="see-also"></a>Vedere anche

 [sys-dm-exec-compute-nodes](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md)   
 [sys-dm-exec-compute-node-status](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-node-status-transact-sql.md)   
 [sys.dm_exec_compute_node_errors](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-node-errors-transact-sql.md)
