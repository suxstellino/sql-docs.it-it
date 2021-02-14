---
title: Failover forzato di SQL Server per i gruppi di disponibilità
description: Failover forzato per i gruppi di disponibilità con tipo di cluster NONE
services: ''
author: MikeRayMSFT
ms.topic: include
ms.date: 02/05/2018
ms.author: mikeray
ms.custom: include file
ms.openlocfilehash: 6e4eb6f9cf7b58a18f42d0b99531084d7fb2cd59
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343361"
---
Ogni gruppo di disponibilità include solo una replica primaria, che consente operazioni di lettura e scrittura. Per modificare la replica primaria, è possibile effettuare il failover. In un gruppo di disponibilità tipico, gestione cluster automatizza il processo di failover. In un gruppo di disponibilità con tipo di cluster NONE, il processo di failover è manuale.

Esistono due modi per effettuare il failover della replica primaria in un gruppo di disponibilità con tipo di cluster NONE:

- Failover manuale senza perdita di dati
- Failover manuale forzato con perdita di dati


### <a name="manual-failover-without-data-loss"></a>Failover manuale senza perdita di dati

Utilizzare questo metodo quando la replica primaria è disponibile, ma è necessario modificare temporaneamente o in modo permanente l'istanza che ospita la replica primaria.
Per evitare una potenziale perdita di dati, prima di effettuare il failover manuale, verificare che la replica secondaria di destinazione sia aggiornata.

Per effettuare il failover manuale senza perdita di dati:

1. Impostare la replica primaria corrente e la replica secondaria di destinazione come `SYNCHRONOUS_COMMIT`.

   ```SQL
   ALTER AVAILABILITY GROUP [AGRScale] 
        MODIFY REPLICA ON N'<node2>' 
        WITH (AVAILABILITY_MODE = SYNCHRONOUS_COMMIT);
   ```

1. Per verificare che per le transazioni attive venga eseguito il commit della replica primaria e di almeno una replica secondaria sincrona, eseguire la query seguente:

   ```SQL
   SELECT ag.name, 
      drs.database_id, 
      drs.group_id, 
      drs.replica_id, 
      drs.synchronization_state_desc, 
      ag.sequence_number
   FROM sys.dm_hadr_database_replica_states drs, sys.availability_groups ag
   WHERE drs.group_id = ag.group_id; 
   ```

   La replica secondaria è sincronizzata quando `synchronization_state_desc` è `SYNCHRONIZED`.

1. Aggiornare `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` su 1.

   Lo script seguente imposta `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` su 1 in un gruppo di disponibilità denominato `ag1`. Prima di eseguire lo script seguente, sostituire `ag1` con il nome del gruppo di disponibilità:

   ```SQL
   ALTER AVAILABILITY GROUP [AGRScale] 
        SET (REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT = 1);
   ```

   Questa impostazione assicura che per ogni transazione attiva venga eseguito il commit della replica primaria e di almeno una replica secondaria sincrona.
   >[!NOTE]
   >Questa impostazione non è specifica del failover e deve essere impostata in base ai requisiti dell'ambiente.

1. Impostare la replica primaria offline per preparare la modifica del ruolo: 

   ```SQL
   ALTER AVAILABILITY GROUP [AGRScale] OFFLINE
   ```

1. Alzare il livello della replica secondaria di destinazione a replica primaria.

   ```SQL
   ALTER AVAILABILITY GROUP AGRScale FORCE_FAILOVER_ALLOW_DATA_LOSS; 
   ```

1. Aggiornare il ruolo della replica primaria precedente in `SECONDARY`, quindi eseguire il comando seguente nell'istanza di SQL Server che ospita la replica primaria precedente:

   ```SQL
   ALTER AVAILABILITY GROUP [AGRScale] 
        SET (ROLE = SECONDARY); 
   ```

   > [!NOTE]
   > Per eliminare un gruppo di disponibilità, usare [DROP AVAILABILITY GROUP](../t-sql/statements/drop-availability-group-transact-sql.md). Per un gruppo di disponibilità creato con il tipo di cluster NONE o EXTERNAL, eseguire il comando su tutte le repliche che fanno parte del gruppo di disponibilità.

1. Riprendere lo spostamento dati, eseguire il comando seguente per ogni database nel gruppo di disponibilità nell'istanza di SQL Server che ospita la replica primaria:

   ```SQL
   ALTER DATABASE [db1]
        SET HADR RESUME
   ```

1. Ricreare ogni listener creato a scopo di scalabilità in lettura e che non rientra nella gestione cluster. Se il listener originale punta alla replica primaria precedente, rimuoverlo e ricrearlo in modo che punti a quella nuova.

### <a name="forced-manual-failover-with-data-loss"></a>Failover manuale forzato con perdita di dati

Se la replica primaria non è disponibile e non può essere recuperata immediatamente, è necessario forzare un failover sulla replica secondaria con perdita di dati. Tuttavia, se la replica primaria originale viene ripristinata dopo il failover, assumerà il ruolo primario. Per evitare che ogni replica sia in uno stato diverso, rimuovere la replica primaria originale dal gruppo di disponibilità dopo un failover forzato con perdita di dati. Quando la replica primaria originale torna online, rimuovere completamente il gruppo di disponibilità. 

Per forzare un failover manuale con perdita di dati dalla replica primaria N1 alla replica secondaria N2, attenersi alla procedura seguente: 

1. Nella replica secondaria (N2), avviare il failover forzato: 

    ```SQL
    ALTER AVAILABILITY GROUP [AGRScale] FORCE_FAILOVER_ALLOW_DATA_LOSS;
    ```
    
1. Nella nuova replica primaria (N2) rimuovere il database primario originale (N1): 

    ```SQL
    ALTER AVAILABILITY GROUP [AGRScale]
    REMOVE REPLICA ON N'N1';
    ```
    
1. Verificare che tutto il traffico dell'applicazione punti al listener e/o alla nuova replica primaria. 
1. Se il database primario originale (N1) è online, portare immediatamente il gruppo di disponibilità AGRScale offline nel database primario originale (N1):

   ```SQL
   ALTER AVAILABILITY GROUP [AGRScale] OFFLINE
   ```
1. Se sono presenti dati o modifiche non sincronizzate, conservare questi dati tramite backup o altre opzioni di replica dei dati in base alle esigenze aziendali.     
1. Rimuovere quindi il gruppo di disponibilità dal database primario originale (N1):

    ```SQL
    DROP AVAILABILITY GROUP [AGRScale];
    ```
1. Eliminare il database del gruppo di disponibilità nella replica primaria originale (N1): 

    ```SQL
    USE [master]
    GO
    DROP DATABASE [AGDBRScale]
    GO
    ```
    
 1. Opzionale Se lo si desidera, è ora possibile aggiungere di nuovo N1 come nuova replica secondaria al gruppo di disponibilità AGRScale.
