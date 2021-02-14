---
title: Gestire i gruppi di disponibilità - SQL Server in Linux
description: Questo articolo descrive come eseguire un aggiornamento in sequenza relativo a istanze di SQL Server in Linux con gruppi di disponibilità. Prima di eseguire l'aggiornamento, esaminare le procedure consigliate.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 03/01/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: 00e576b6fd2fc92cca591a5ffa3d6e97556bc1ad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352567"
---
# <a name="operate-always-on-availability-groups-on-linux"></a>Gestire i gruppi di disponibilità Always On in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

## <a name="upgrade-availability-group"></a>Aggiornare un gruppo di disponibilità

Prima di aggiornare un gruppo di disponibilità, rivedere i modelli e le procedure in [Aggiornamento delle istanze di replica dei gruppi di disponibilità AlwaysOn](../database-engine/availability-groups/windows/upgrading-always-on-availability-group-replica-instances.md).

Le sezioni seguenti illustrano come eseguire un aggiornamento in sequenza relativo a istanze di SQL Server in Linux con gruppi di disponibilità. 

### <a name="upgrade-steps-on-linux"></a>Procedura di aggiornamento in Linux

Quando le repliche del gruppo di disponibilità si trovano su istanze di SQL Server in Linux, il tipo di cluster del gruppo di disponibilità è `EXTERNAL` o `NONE`. Un gruppo di disponibilità controllato da un modulo di gestione cluster diverso da WSFC (Windows Server Failover Cluster) è di tipo `EXTERNAL`. Pacemaker con Corosync è un esempio di modulo di gestione cluster esterno. Il tipo di cluster di un gruppo di disponibilità senza modulo di gestione cluster è `NONE`. La procedura di aggiornamento descritta di seguito riguarda specificamente i gruppi di disponibilità con tipo di cluster `EXTERNAL` o `NONE`.

L'ordine in cui si aggiornano le istanze dipende dal fatto che il loro ruolo sia secondario e che ospitino o meno repliche sincrone o asincrone. Si aggiornano prima le istanze di SQL Server che ospitano repliche secondarie asincrone e dopo quelle che ospitano repliche secondarie sincrone. 

   >[!NOTE]
   >Se un gruppo di disponibilità ha solo repliche asincrone, per evitare perdite di dati impostarne una come sincrona e attendere che venga sincronizzata. Aggiornare quindi questa replica.
   
Prima di iniziare, eseguire il backup di ogni database.

1. Arrestare la risorsa nel nodo che ospita la replica secondaria a cui è destinato l'aggiornamento.
   
   Prima di eseguire il comando di aggiornamento, arrestare la risorsa per evitare che venga monitorata dal cluster e che ne venga eseguito inutilmente il failover. L'esempio seguente aggiunge al nodo un vincolo di posizione che determina l'arresto della risorsa. Aggiornare `ag_cluster-master` con il nome della risorsa e `nodeName1` con il nodo che ospita la replica a cui è destinato l'aggiornamento.

   ```bash
   pcs constraint location ag_cluster-master avoids nodeName1
   ```

1. Aggiornare SQL Server sulla replica secondaria.

   L'esempio seguente aggiorna i pacchetti `mssql-server` e `mssql-server-ha`.

   ```bash
   sudo yum update mssql-server
   sudo yum update mssql-server-ha
   ```
1. Rimuovere il vincolo di posizione.

   Prima di eseguire il comando di aggiornamento, arrestare la risorsa per evitare che venga monitorata dal cluster e che ne venga eseguito inutilmente il failover. L'esempio seguente aggiunge al nodo un vincolo di posizione che determina l'arresto della risorsa. Aggiornare `ag_cluster-master` con il nome della risorsa e `nodeName1` con il nodo che ospita la replica a cui è destinato l'aggiornamento.

   ```bash
   pcs constraint remove location-ag_cluster-master-rhel1--INFINITY
   ```
   Come procedura consigliata, assicurarsi che la risorsa sia avviata (tramite il comando `pcs status`) e che la replica secondaria sia connessa e sincronizzata dopo l'aggiornamento.

1. Al termine dell'aggiornamento di tutte le repliche secondarie, eseguire manualmente il failover in una delle repliche secondarie sincrone.

   Per i gruppi di disponibilità con tipo di cluster `EXTERNAL`, eseguire il failover usando gli strumenti per la gestione di cluster. Per il failover dei gruppi di disponibilità con tipo di cluster `NONE`, è necessario usare Transact-SQL. 
   L'esempio seguente esegue il failover di un gruppo di disponibilità con gli strumenti per la gestione di cluster. Sostituire `<targetReplicaName>` con il nome della replica secondaria sincrona che diventerà primaria:

   ```bash
   sudo pcs resource move ag_cluster-master <targetReplicaName> --master  
   ``` 
   
   >[!IMPORTANT]
   >I passaggi seguenti si applicano solo ai gruppi di disponibilità che non dispongono di un modulo di gestione cluster.

   Se il tipo di cluster per il gruppo di disponibilità è `NONE`, eseguire il failover manualmente. Completare i passaggi seguenti nell'ordine indicato:

      a. Il comando seguente imposta la replica primaria come secondaria. Sostituire `AG1` con il nome del gruppo di disponibilità. Eseguire il comando Transact-SQL nell'istanza di SQL Server che ospita la replica primaria.

      ```transact-sql
      ALTER AVAILABILITY GROUP [ag1] SET (ROLE = SECONDARY);
      ```

      b. Il comando seguente imposta una replica secondaria sincrona come primaria. Eseguire il comando Transact-SQL seguente nell'istanza di destinazione di SQL Server, ovvero l'istanza che ospita la replica secondaria sincrona.

      ```transact-sql
      ALTER AVAILABILITY GROUP [ag1] FAILOVER;
      ```

1. Dopo il failover, eseguire l'aggiornamento di SQL Server nella replica primaria precedente ripetendo la procedura riportata sopra.

   L'esempio seguente aggiorna i pacchetti `mssql-server` e `mssql-server-ha`.

   ```bash
   # add constraint for the resource to stop on the upgraded node
   # replace 'nodename2' with the name of the cluster node targeted for upgrade
   pcs constraint location ag_cluster-master avoids nodeName2
   sudo yum update mssql-server
   sudo yum update mssql-server-ha
   ```
   
   ```bash
   # upgrade mssql-server and mssql-server-ha packages
   sudo yum update mssql-server
   sudo yum update mssql-server-ha
   ```

   ```bash
   # remove the constraint; make sure the resource is started and replica is connected and synchronized
   pcs constraint remove location-ag_cluster-master-rhel1--INFINITY
   ```

1. Per i gruppi di disponibilità con un modulo di gestione cluster esterno, dove il tipo di cluster è EXTERNAL, rimuovere il vincolo di posizione causato dal failover manuale. 

   ```bash
   sudo pcs constraint remove cli-prefer-ag_cluster-master  
   ```

1. Riprendere lo spostamento dei dati per la replica secondaria appena aggiornata, ovvero la replica primaria precedente. Questo passaggio è necessario quando un'istanza di SQL Server con una versione più recente trasferisce i blocchi di log a un'istanza con una versione precedente in un gruppo di disponibilità. Eseguire il comando seguente sulla nuova replica secondaria (la replica primaria precedente).

   ```transact-sql
   ALTER DATABASE database_name SET HADR RESUME;
   ```

Dopo l'aggiornamento di tutti i server, è possibile eseguire il failback. Eseguire nuovamente il failover nel database primario originale, se necessario. 

## <a name="drop-an-availability-group"></a>Eliminare un gruppo di disponibilità

Per eliminare un gruppo di disponibilità, eseguire [DROP AVAILABILITY GROUP](../t-sql/statements/drop-availability-group-transact-sql.md). Se il tipo di cluster è `EXTERNAL` o `NONE`, eseguire il comando su ogni istanza di SQL Server che ospita una replica. Ad esempio, per eliminare un gruppo di disponibilità denominato `group_name`, eseguire il comando seguente:

   ```transact-sql
   DROP AVAILABILITY GROUP group_name
   ```
 

## <a name="next-steps"></a>Passaggi successivi

[Configurare un cluster Red Hat Enterprise Linux per le risorse cluster di un gruppo di disponibilità di SQL Server](sql-server-linux-availability-group-cluster-rhel.md)

[Configurare un cluster SUSE Linux Enterprise Server per le risorse cluster di un gruppo di disponibilità di SQL Server](sql-server-linux-availability-group-cluster-sles.md)

[Configurare un cluster Ubuntu per le risorse cluster di un gruppo di disponibilità di SQL Server](sql-server-linux-availability-group-cluster-ubuntu.md)
