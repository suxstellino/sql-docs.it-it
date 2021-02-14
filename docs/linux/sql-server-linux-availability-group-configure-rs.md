---
title: Configurare un gruppo di disponibilità con scalabilità in lettura (SQL Server in Linux)
description: Informazioni sulla configurazione di un gruppo di disponibilità Always On di SQL Server per i carichi di lavoro con scalabilità in lettura in Linux.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 01/09/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 5cad1c63817bf55231c065123b2456590dbfebb2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352738"
---
# <a name="configure-a-sql-server-availability-group-for-read-scale-on-linux"></a>Configurare un gruppo di disponibilità di SQL Server con scalabilità in lettura in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

È possibile configurare un gruppo di disponibilità Always On SQL Server per i carichi di lavoro con scalabilità in lettura in Linux. Esistono due tipi di architetture per i gruppi di disponibilità. L'architettura per la disponibilità elevata usa un modulo di gestione cluster per garantire la continuità operativa in modo più efficace. Questa architettura può includere anche repliche con scalabilità in lettura. Per creare un'architettura per la disponibilità elevata, vedere [Configurare un gruppo di disponibilità Always On di SQL Server per la disponibilità elevata in Linux](sql-server-linux-availability-group-configure-ha.md). L'altra architettura supporta solo i carichi di lavoro di scalabilità in lettura. Questo articolo illustra come creare un gruppo di disponibilità senza una Gestione cluster per i carichi di lavoro di scalabilità in lettura. Questa architettura fornisce solo la scalabilità in lettura. Non fornisce la disponibilità elevata.

> [!NOTE]
> Un gruppo di disponibilità con `CLUSTER_TYPE = NONE` può includere repliche ospitate in diverse piattaforme del sistema operativo. Non può tuttavia supportare la disponibilità elevata. 

[!INCLUDE [Create prerequisites](../includes/ss-linux-cluster-availability-group-create-prereq.md)]

## <a name="create-the-ag"></a>Creare il gruppo di disponibilità

Creare il gruppo di disponibilità. Impostare `CLUSTER_TYPE = NONE`. Impostare anche ogni replica con `FAILOVER_MODE = MANUAL`. Le applicazioni client che eseguono carichi di lavoro di analisi o esecuzione report possono connettersi direttamente ai database secondari. È anche possibile creare un elenco di routing di sola lettura. Le connessioni alla replica primaria inoltrano le richieste di connessione in lettura a ogni replica secondaria dell'elenco di routing in base a uno schema round-robin.

Lo script Transact-SQL seguente crea un gruppo di disponibilità con nome `ag1`. Lo script configura le repliche del gruppo di disponibilità con `SEEDING_MODE = AUTOMATIC`. In base a questa impostazione, SQL Server crea automaticamente il database in ciascun server secondario dopo l'aggiunta al gruppo di disponibilità. Aggiornare lo script seguente per il proprio ambiente. Sostituire i valori `<node1>` e `<node2>` con i nomi delle istanze di SQL Server che ospitano le repliche. Sostituire il valore `<5022>` con la porta impostata per l'endpoint. Nella replica primaria di SQL Server eseguire lo script Transact-SQL seguente:

```SQL
CREATE AVAILABILITY GROUP [ag1]
    WITH (CLUSTER_TYPE = NONE)
    FOR REPLICA ON
        N'<node1>' WITH (
            ENDPOINT_URL = N'tcp://<node1>:<5022>',
            AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
            FAILOVER_MODE = MANUAL,
            SEEDING_MODE = AUTOMATIC,
                    SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL)
            ),
        N'<node2>' WITH ( 
            ENDPOINT_URL = N'tcp://<node2>:<5022>', 
            AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
            FAILOVER_MODE = MANUAL,
            SEEDING_MODE = AUTOMATIC,
            SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL)
            );
        
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

### <a name="join-secondary-sql-servers-to-the-ag"></a>Aggiungere server SQL secondari al gruppo di disponibilità

Lo script Transact-SQL seguente aggiunge un server al gruppo di disponibilità con nome `ag1`. Aggiornare lo script per il proprio ambiente. In ogni replica secondaria di SQL Server eseguire lo script Transact-SQL seguente per l'aggiunta al gruppo di disponibilità:

```SQL
ALTER AVAILABILITY GROUP [ag1] JOIN WITH (CLUSTER_TYPE = NONE);
         
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

[!INCLUDE [Create post](../includes/ss-linux-cluster-availability-group-create-post.md)]

Questo gruppo di disponibilità non è una configurazione a disponibilità elevata. Se la disponibilità elevata è necessaria, seguire le istruzioni in [Configurare un gruppo di disponibilità Always On per SQL Server in Linux](sql-server-linux-availability-group-configure-ha.md). In particolare, creare il gruppo di disponibilità con `CLUSTER_TYPE=WSFC` (in Windows) o `CLUSTER_TYPE=EXTERNAL` (in Linux). Integrare quindi un modulo di gestione cluster (Windows Server Failover Clustering in Windows o Pacemaker in Linux).

## <a name="connect-to-read-only-secondary-replicas"></a>Eseguire la connessione a repliche secondarie di sola lettura

Esistono due modi per eseguire la connessione a repliche secondarie di sola lettura. Le applicazioni possono connettersi direttamente all'istanza di SQL Server che ospita la replica secondaria ed eseguire query sui database. Oppure possono usare il routing di sola lettura, per il quale è necessario un listener.

* [Repliche secondarie leggibili](../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)
* [Routing di sola lettura](../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md#ConnectToSecondary)

## <a name="fail-over-the-primary-replica-on-a-read-scale-availability-group"></a>Eseguire il failover della replica primaria in un gruppo di disponibilità con scalabilità in lettura

[!INCLUDE[Force failover](../includes/ss-force-failover-read-scale-out.md)]

## <a name="next-steps"></a>Passaggi successivi

* [Configurare gruppi di disponibilità distribuiti](../database-engine/availability-groups/windows/distributed-availability-groups.md)
* [Altre informazioni sui gruppi di disponibilità](../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)
* [Eseguire un failover manuale forzato](../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)