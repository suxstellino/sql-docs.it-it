---
title: Configurare un gruppo di disponibilità distribuito
description: Informazioni su come configurare un gruppo di disponibilità distribuito usando un esempio Transact-SQL. Scoprire anche dove trovare informazioni sui gruppi di disponibilità distribuiti.
ms.custom: seodec18
ms.date: 01/28/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: f7c7acc5-a350-4a17-95e1-e689c78a0900
author: cawrites
ms.author: chadam
ms.openlocfilehash: 94eec1c8490a1827b27953adef2e79c45ff5efd8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342447"
---
# <a name="configure-an-always-on-distributed-availability-group"></a>Configurare un gruppo di disponibilità distribuito Always On  
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Per creare un gruppo di disponibilità distribuito, è necessario creare due gruppi di disponibilità ognuno con il proprio listener. È quindi possibile combinare questi gruppi di disponibilità in un gruppo di disponibilità distribuito. La procedura seguente illustra un esempio di base in Transact-SQL. Questo esempio non descrive in dettaglio la creazione di gruppi di disponibilità e listener, ma mette in rilievo i requisiti principali.

Per una panoramica tecnica dei gruppi di disponibilità distribuiti, vedere [Gruppi di disponibilità distribuiti](distributed-availability-groups.md).

## <a name="prerequisites"></a>Prerequisiti

### <a name="set-the-endpoint-listeners-to-listen-to-all-ip-addresses"></a>Impostare i listener di endpoint in ascolto per tutti gli indirizzi IP

Assicurarsi che gli endpoint possano comunicare tra i vari gruppi di disponibilità nel gruppo di disponibilità distribuito. Se un gruppo di disponibilità è impostato su una rete specifica dell'endpoint, il gruppo di disponibilità distribuito non funzionerà correttamente. In ogni server che ospita una replica nel gruppo di disponibilità distribuito, configurare il listener per l'ascolto su tutti gli indirizzi IP (`LISTENER_IP = ALL`).

#### <a name="create-a-listener-to-listen-to-all-ip-addresses"></a>Creare un listener per l'ascolto di tutti gli indirizzi IP

Ad esempio, lo script seguente crea un endpoint del listener sulla porta TCP 5022 in ascolto su tutti gli indirizzi IP.  

```sql
CREATE ENDPOINT [aodns-hadr] 
    STATE=STARTED
    AS TCP (LISTENER_PORT = 5022, LISTENER_IP = ALL)
FOR DATA_MIRRORING (
   ROLE = ALL, 
   AUTHENTICATION = WINDOWS NEGOTIATE,
   ENCRYPTION = REQUIRED ALGORITHM AES
)
GO
```

#### <a name="alter-a-listener-to-listen-to-all-ip-addresses"></a>Modificare un listener per l'ascolto di tutti gli indirizzi IP

Ad esempio, lo script seguente modifica un endpoint del listener per l'ascolto di tutti gli indirizzi IP.  

```sql
ALTER ENDPOINT [aodns-hadr] 
    AS TCP (LISTENER_IP = ALL)
GO
```

## <a name="create-first-availability-group"></a>Creare un primo gruppo di disponibilità

### <a name="create-the-primary-availability-group-on-the-first-cluster"></a>Creare il gruppo di disponibilità primario nel primo cluster  
Creare un gruppo di disponibilità nel primo cluster di failover di Windows Server (WSFC).   In questo esempio il gruppo di disponibilità è denominato `ag1` per il database `db1`. La replica primaria del gruppo di disponibilità primario è nota come **server primario globale** in un gruppo di disponibilità distribuito. Server1 è il server primario globale in questo esempio.        
  
```sql  
CREATE AVAILABILITY GROUP [ag1]   
FOR DATABASE db1   
REPLICA ON N'server1' WITH (ENDPOINT_URL = N'TCP://server1.contoso.com:5022',  
    FAILOVER_MODE = AUTOMATIC,  
    AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,   
    BACKUP_PRIORITY = 50,   
    SECONDARY_ROLE(ALLOW_CONNECTIONS = NO),   
    SEEDING_MODE = AUTOMATIC),   
N'server2' WITH (ENDPOINT_URL = N'TCP://server2.contoso.com:5022',   
    FAILOVER_MODE = AUTOMATIC,   
    AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,   
    BACKUP_PRIORITY = 50,   
    SECONDARY_ROLE(ALLOW_CONNECTIONS = NO),   
    SEEDING_MODE = AUTOMATIC);   
GO  
  
```  
  
>[!NOTE]
>L'esempio precedente usa il seeding automatico, dove **SEEDING_MODE** è impostato su **AUTOMATIC** per le repliche e il gruppo di disponibilità distribuito. Questa configurazione imposta le repliche secondarie e il gruppo di disponibilità secondario in modo che vengano popolati automaticamente senza richiedere alcun backup e ripristino manuale del database primario.  
  
### <a name="join-the-secondary-replicas-to-the-primary-availability-group"></a>Creare un join delle repliche secondarie al gruppo di disponibilità primario  
È necessario creare un join tra tutte le repliche secondarie al gruppo di disponibilità tramite **ALTER AVAILABILITY GROUP** con l'opzione **JOIN** . Dal momento che in questo esempio viene usato il seeding automatico, è necessario chiamare anche **ALTER AVAILABILITY GROUP** con l'opzione **GRANT CREATE ANY DATABASE**. Questa impostazione consente al gruppo di disponibilità di creare il database e avviare il seeding automatico dalla replica primaria.  
  
In questo esempio i seguenti comandi vengono eseguiti sulla replica secondaria, `server2`, per creare un join del gruppo di disponibilità `ag1` . Il gruppo di disponibilità può quindi creare database sulla replica secondaria.  
  
```sql  
ALTER AVAILABILITY GROUP [ag1] JOIN   
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE  
GO  
```  

>[!NOTE]
>Quando il gruppo di disponibilità crea un database in una replica secondaria, imposta il proprietario del database come l'account che ha eseguito l'istruzione `ALTER AVAILABILITY GROUP` per concedere l'autorizzazione per la creazione di qualsiasi database. Per informazioni complete, vedere [Concedere le autorizzazioni di creazione di database sulla replica secondaria al gruppo di disponibilità](automatic-seeding-secondary-replicas.md#grantCreate).
  
### <a name="create-a-listener-for-the-primary-availability-group"></a>Creare un listener per il gruppo di disponibilità primario  

Successivamente aggiungere un listener per il gruppo di disponibilità primario sul primo WSFC. In questo esempio il listener viene denominato `ag1-listener`. Per istruzioni dettagliate sulla creazione di un listener, vedere [Creare o configurare un listener del gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).  
  
```sql
ALTER AVAILABILITY GROUP [ag1]    
    ADD LISTENER 'ag1-listener' ( 
        WITH IP ( ('2001:db88:f0:f00f::cf3c'),('2001:4898:e0:f213::4ce2') ) , 
        PORT = 60173);    
GO  
```  
  

## <a name="create-second-availability-group"></a>Creare un secondo gruppo di disponibilità  
 Creare quindi un secondo gruppo di disponibilità, `ag2`, nel secondo WSFC. In questo caso il database non è specificato, perché viene eseguito il seeding automatico del database stesso dal gruppo di disponibilità primario.  La replica primaria del gruppo di disponibilità secondario è nota come **server di inoltro** in un gruppo di disponibilità distribuito. In questo esempio server3 è il server d'inoltro. 
  
```sql  
CREATE AVAILABILITY GROUP [ag2]   
FOR   
REPLICA ON N'server3' WITH (ENDPOINT_URL = N'TCP://server3.contoso.com:5022',   
    FAILOVER_MODE = MANUAL,   
    AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,   
    BACKUP_PRIORITY = 50,   
    SECONDARY_ROLE(ALLOW_CONNECTIONS = NO),   
    SEEDING_MODE = AUTOMATIC),   
N'server4' WITH (ENDPOINT_URL = N'TCP://server4.contoso.com:5022',   
    FAILOVER_MODE = MANUAL,   
    AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,   
    BACKUP_PRIORITY = 50,   
    SECONDARY_ROLE(ALLOW_CONNECTIONS = NO),   
    SEEDING_MODE = AUTOMATIC);   
GO  
```  
  
> [!NOTE]  
> Il gruppo di disponibilità secondario deve usare lo stesso endpoint del mirroring del database (in questo esempio la porta 5022). In caso contrario, la replica verrà interrotta dopo un failover locale.  
  
### <a name="join-the-secondary-replicas-to-the-secondary-availability-group"></a>Creare un join delle repliche secondarie al gruppo di disponibilità secondario  
 In questo esempio i seguenti comandi vengono eseguiti sulla replica secondaria, `server4`, per creare un join del gruppo di disponibilità `ag2` . Il gruppo di disponibilità può quindi creare database sulla replica secondaria per supportare il seeding automatico.  
  
```sql  
ALTER AVAILABILITY GROUP [ag2] JOIN   
ALTER AVAILABILITY GROUP [ag2] GRANT CREATE ANY DATABASE  
GO  
```  
  
### <a name="create-a-listener-for--the-secondary-availability-group"></a>Creare un listener per il gruppo di disponibilità secondario  
 Successivamente aggiungere un listener per il gruppo di disponibilità secondario sul secondo WSFC. In questo esempio il listener viene denominato `ag2-listener`. Per istruzioni dettagliate sulla creazione di un listener, vedere [Creare o configurare un listener del gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).  
  
```sql  
ALTER AVAILABILITY GROUP [ag2]    
    ADD LISTENER 'ag2-listener' ( WITH IP ( ('2001:db88:f0:f00f::cf3c'),('2001:4898:e0:f213::4ce2') ) , PORT = 60173);    
GO  
```  
  
## <a name="create-distributed-availability-group-on-first-cluster"></a>Creare un gruppo di disponibilità distribuito nel primo cluster  
 Creare un gruppo di disponibilità distribuito nel primo WSFC (in questo esempio denominato `distributedag` ). Usare il comando **CREATE AVAILABILITY GROUP** con l'opzione **DISTRIBUTED** . Il parametro **AVAILABILITY GROUP ON** specifica i gruppi di disponibilità membri, `ag1` e `ag2`.  
  
# <a name="automatic-seeding"></a>[Seeding automatico](#tab/automatic)

Per creare il gruppo di disponibilità distribuito usando il seeding automatico, usare il seguente codice Transact-SQL: 

```sql  
CREATE AVAILABILITY GROUP [distributedag]  
   WITH (DISTRIBUTED)   
   AVAILABILITY GROUP ON  
      'ag1' WITH    
      (   
         LISTENER_URL = 'tcp://ag1-listener.contoso.com:5022',    
         AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,   
         FAILOVER_MODE = MANUAL,   
         SEEDING_MODE = AUTOMATIC   
      ),   
      'ag2' WITH    
      (   
         LISTENER_URL = 'tcp://ag2-listener.contoso.com:5022',   
         AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,   
         FAILOVER_MODE = MANUAL,   
         SEEDING_MODE = AUTOMATIC   
      );    
GO   
```  
  

# <a name="manual-seeding"></a>[Seeding manuale](#tab/manual)

Per creare il gruppo di disponibilità distribuito usando il seeding manuale, usare il seguente codice Transact-SQL: 

```sql
CREATE AVAILABILITY GROUP [distributedag]  
   WITH (DISTRIBUTED)   
   AVAILABILITY GROUP ON  
      'ag1' WITH    
      (   
         LISTENER_URL = 'tcp://ag1-listener.contoso.com:5022',    
         AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,   
         FAILOVER_MODE = MANUAL,   
         SEEDING_MODE = MANUAL   
      ),   
      'ag2' WITH    
      (   
         LISTENER_URL = 'tcp://ag2-listener.contoso.com:5022',   
         AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,   
         FAILOVER_MODE = MANUAL,   
         SEEDING_MODE = MANUAL   
      );    
GO   

```

---

> [!NOTE]  
>  Il parametro **LISTENER_URL** specifica il listener per ogni gruppo di disponibilità insieme all'endpoint del mirroring del database del gruppo di disponibilità. Questo esempio riguarda la porta `5022` (non la porta `60173` usata per creare il listener). Se si usa un bilanciamento del carico, ad esempio in Azure, [aggiungere una regola per il bilanciamento del carico per la porta del gruppo di disponibilità distribuito](/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener#add-load-balancing-rule-for-distributed-availability-group). Aggiungere la regola per la porta del listener, oltre che alla porta dell'istanza di SQL Server. 

### <a name="cancel-automatic-seeding-to-forwarder"></a>Annullare il seeding automatico al server d'inoltro

Se per qualsiasi motivo è necessario annullare l'inizializzazione del server d'inoltro _prima_ che i due gruppi di disponibilità siano sincronizzati, modificare (ALTER) il gruppo di disponibilità distribuito impostando il parametro SEEDING_MODE del server di inoltro su MANUAL e annullare immediatamente il seeding. Eseguire il comando nel database primario globale: 

```sql
-- Cancel automatic seeding.  Connect to global primary but specify DAG AG2
ALTER AVAILABILITY GROUP [distributedag]   
   MODIFY  
   AVAILABILITY GROUP ON  
   'ag2' WITH  
   (  SEEDING_MODE = MANUAL  );   
```

  
## <a name="join-distributed-availability-group-on-second-cluster"></a>Aggiungere un gruppo di disponibilità distribuito nel secondo cluster  
 Creare quindi un join del gruppo di disponibilità distribuito nel secondo WSFC.  
  
# <a name="automatic-seeding"></a>[Seeding automatico](#tab/automatic)

Per creare un join con il gruppo di disponibilità distribuito usando il seeding automatico, usare il seguente codice Transact-SQL: 

```sql  
ALTER AVAILABILITY GROUP [distributedag]   
   JOIN   
   AVAILABILITY GROUP ON  
      'ag1' WITH    
      (   
         LISTENER_URL = 'tcp://ag1-listener.contoso.com:5022',    
         AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,   
         FAILOVER_MODE = MANUAL,   
         SEEDING_MODE = AUTOMATIC   
      ),   
      'ag2' WITH    
      (   
         LISTENER_URL = 'tcp://ag2-listener.contoso.com:5022',   
         AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,   
         FAILOVER_MODE = MANUAL,   
         SEEDING_MODE = AUTOMATIC   
      );    
GO  
```  



# <a name="manual-seeding"></a>[Seeding manuale](#tab/manual)

Per creare un join con il gruppo di disponibilità distribuito usando il seeding manuale, usare il seguente codice Transact-SQL: 

```sql
ALTER AVAILABILITY GROUP [distributedag]   
   JOIN   
   AVAILABILITY GROUP ON  
      'ag1' WITH    
      (   
         LISTENER_URL = 'tcp://ag1-listener.contoso.com:5022',    
         AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,   
         FAILOVER_MODE = MANUAL,   
         SEEDING_MODE = MANUAL   
      ),   
      'ag2' WITH    
      (   
         LISTENER_URL = 'tcp://ag2-listener.contoso.com:5022',   
         AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,   
         FAILOVER_MODE = MANUAL,   
         SEEDING_MODE = MANUAL  
      );    
GO  
```

Se il seeding manuale viene usato per creare il database nel server d'inoltro, eseguire un backup completo e un backup del log delle transazioni dal database primario globale e ripristinarli nel server d'inoltro con l'opzione NONRECOVERY. Esempio:

Per eseguire il backup nel database primario globale: 

```sql
BACKUP DATABASE [db1] 
TO DISK = '<full backup location>' WITH FORMAT
BACKUP LOG [db1] 
TO DISK = '<log backup location>' WITH FORMAT
```

Per eseguire il ripristino nel server d'inoltro: 

```sql
RESTORE DATABASE [db1] 
FROM DISK = '<full backup location>' WITH NORECOVERY
RESTORE LOG [db1] FROM DISK = '<log backup location>' WITH NORECOVERY
```

Successivamente, eseguire il comando seguente nel server d'inoltro

```sql
ALTER DATABASE [db1] SET HADR AVAILABILITY GROUP = [distributedag]
```

---


## <a name="join-the-database-on-the-secondary-of-the-second-availability-group"></a><a name="failover"></a> Aggiungere il database nella replica secondaria del secondo gruppo di disponibilità
Quando il database nella replica secondaria del secondo gruppo di disponibilità è in uno stato di ripristino, è necessario aggiungerlo manualmente al gruppo di disponibilità.

```sql  
ALTER DATABASE [db1] SET HADR AVAILABILITY GROUP = [ag2];   
```
  
## <a name="fail-over-to-a-secondary-availability-group"></a><a name="failover"></a> Failover su un gruppo di disponibilità secondario  

In questo momento è supportato solo il failover manuale. Per eseguire il failover manuale di un gruppo di disponibilità distribuito:

1. Per assicurarsi che non si verifichino perdite di dati, arrestare tutte le transazioni nei database primari globali, ovvero i database del gruppo di disponibilità primario, quindi impostare il gruppo di disponibilità distribuito per il commit sincrono.
1. Attendere che il gruppo di disponibilità distribuito sia sincronizzato e che abbia lo stesso last_hardened_lsn per ogni database. 
1. Nella replica primaria globale impostare il ruolo del gruppo di disponibilità distribuito su `SECONDARY`.
1. Verificare la conformità del failover.
1. Eseguire il failover del gruppo di disponibilità primario.

Gli esempi Transact-SQL seguenti illustrano i passaggi dettagliati per il failover del gruppo di disponibilità distribuito denominato `distributedag`:

1. Per assicurarsi che non si verifichino perdite di dati, arrestare tutte le transazioni nei database primari globali, ovvero i database del gruppo di disponibilità primario. Impostare quindi il gruppo di disponibilità distribuito per il commit sincrono eseguendo il codice seguente *sia* nel server primario globale che nel server d'inoltro.   
    
      ```sql  
      -- sets the distributed availability group to synchronous commit 
       ALTER AVAILABILITY GROUP [distributedag] 
       MODIFY 
       AVAILABILITY GROUP ON
       'ag1' WITH 
        ( 
        AVAILABILITY_MODE = SYNCHRONOUS_COMMIT 
        ), 
        'ag2' WITH  
        ( 
        AVAILABILITY_MODE = SYNCHRONOUS_COMMIT 
        );
       
       -- verifies the commit state of the distributed availability group
       select ag.name, ag.is_distributed, ar.replica_server_name, ar.availability_mode_desc, ars.connected_state_desc, ars.role_desc, 
       ars.operational_state_desc, ars.synchronization_health_desc from sys.availability_groups ag  
       join sys.availability_replicas ar on ag.group_id=ar.group_id
       left join sys.dm_hadr_availability_replica_states ars
       on ars.replica_id=ar.replica_id
       where ag.is_distributed=1
       GO

      ```  
   > [!NOTE]
   > In un gruppo di disponibilità distribuito, lo stato di sincronizzazione tra i due gruppi di disponibilità dipende dalla modalità di disponibilità di entrambe le repliche. Per la modalità con commit sincrono, il gruppo di disponibilità primario corrente e quello secondario corrente devono essere entrambi configurati con la modalità di disponibilità `SYNCHRONOUS_COMMIT`. Per questo motivo, è necessario eseguire lo script precedente nella replica primaria globale e nel server di inoltro.


1. Attendere che lo stato del gruppo di disponibilità distribuito diventi `SYNCHRONIZED` e che tutte le repliche abbiano lo stesso last_hardened_lsn (per ogni database). Eseguire la query seguente sia nel database primario globale, che è la replica primaria del gruppo di disponibilità primario, che nel server di inoltro per controllare i valori synchronization_state_desc e last_hardened_lsn: 
    
      ```sql  
      -- Run this query on the Global Primary and the forwarder
      -- Check the results to see if synchronization_state_desc is SYNCHRONIZED, and the last_hardened_lsn is the same per database on both the global primary and       forwarder 
      -- If not rerun the query on both side every 5 seconds until it is the case
      --
      SELECT ag.name
             , drs.database_id
             , db_name(drs.database_id) as database_name
             , drs.group_id
             , drs.replica_id
             , drs.synchronization_state_desc
             , drs.last_hardened_lsn  
      FROM sys.dm_hadr_database_replica_states drs 
      INNER JOIN sys.availability_groups ag on drs.group_id = ag.group_id;
      ```  

    Proseguire quando il valore **synchronization_state_desc** del gruppo di disponibilità è `SYNCHRONIZED` e il valore last_hardened_lsn è uguale per ogni database sia nel database primario globale che nel server di inoltro.  Se **synchronization_state_desc** non è `SYNCHRONIZED` o last_hardened_lsn è diverso, eseguire il comando ogni cinque secondi fino a quando questi valori non vengono modificati. Non continuare fino a quando i valori **synchronization_state_desc** = `SYNCHRONIZED` e last_hardened_lsn non sono uguali per ogni database. 

1. Nel server primario globale impostare il ruolo del gruppo di disponibilità distribuito su `SECONDARY`. 

    ```sql
    ALTER AVAILABILITY GROUP distributedag SET (ROLE = SECONDARY); 
    ```  

    A questo punto, il gruppo di disponibilità distribuito non è disponibile.

1. Verificare la conformità del failover. Eseguire la query seguente sia nel database primario globale che nel server di inoltro:

    ```sql
     -- Run this query on the Global Primary and the forwarder
     -- Check the results to see if the last_hardened_lsn is the same per database on both the global primary and forwarder 
     -- The availability group is ready to fail over when the last_hardened_lsn is the same for both availability groups per database
     --
     SELECT ag.name, 
         drs.database_id, 
         db_name(drs.database_id) as database_name,
         drs.group_id, 
         drs.replica_id,
         drs.last_hardened_lsn
     FROM sys.dm_hadr_database_replica_states drs
     INNER JOIN sys.availability_groups ag ON drs.group_id = ag.group_id;
    ```  

    Il gruppo di disponibilità è pronto per il failover quando il valore **last_hardened_lsn** è uguale per entrambi i gruppi di disponibilità per ogni database. Se last_hardened_lsn non diventa uguale dopo un periodo di tempo, per evitare la perdita di dati, eseguire il failback nel database primario globale eseguendo questo comando nel database primario globale e quindi ricominciare dal secondo passaggio: 

    ```sql
    -- If the last_hardened_lsn is not the same after a period of time, to avoid data loss, 
    -- we need to fail back to the global primary by running this command on the global primary 
    -- and then start over from the second step:

    ALTER AVAILABILITY GROUP distributedag FORCE_FAILOVER_ALLOW_DATA_LOSS; 
    ```


1. Failover dal gruppo di disponibilità primaria al gruppo di disponibilità secondaria. Eseguire il comando seguente nel server di inoltro, ovvero l'istanza di SQL Server che ospita la replica primaria del gruppo di disponibilità secondario. 

    ```sql
    -- Once the last_hardened_lsn is the same per database on both sides
    -- We can Fail over from the primary availability group to the secondary availability group. 
    -- Run the following command on the forwarder, the SQL Server instance that hosts the primary replica of the secondary availability group.

    ALTER AVAILABILITY GROUP distributedag FORCE_FAILOVER_ALLOW_DATA_LOSS; 
    ```  

    Dopo questo passaggio, il gruppo di disponibilità distribuito sarà disponibile.
      
Dopo aver completato i passaggi precedenti, si verifica il failover del gruppo di disponibilità distribuito senza perdita di dati. Se i gruppi di disponibilità si trovano a una distanza geografica che provoca latenza, reimpostare la modalità di disponibilità su ASYNCHRONOUS_COMMIT. 
  
## <a name="remove-a-distributed-availability-group"></a>Rimuovere un gruppo di disponibilità distribuito  
 L'istruzione Transact-SQL seguente rimuove un gruppo di disponibilità distribuito denominato `distributedag`:  
  
```sql  
DROP AVAILABILITY GROUP [distributedag]  
```  

## <a name="create-distributed-availability-group-on-failover-cluster-instances"></a>Creare un gruppo di disponibilità distribuito in istanze del cluster di failover

È possibile creare un gruppo di disponibilità distribuito usando un gruppo di disponibilità in un'istanza del cluster di failover. In questo caso, non è necessario un listener del gruppo di disponibilità. Usare il nome di rete virtuale (VNN) per la replica primaria dell'istanza FCI. L'esempio seguente illustra un gruppo di disponibilità distribuito denominato SQLFCIDAG. Un gruppo di disponibilità è SQLFCIAG. SQLFCIAG ha due repliche FCI. Il nome di rete virtuale per la replica primaria FCI è SQLFCIAG-1 e il nome di rete virtuale per la replica FCI secondaria è SQLFCIAG-2. Il gruppo di disponibilità distribuito include anche SQLAG-DR, per il ripristino di emergenza.

![Gruppo di disponibilità AlwaysOn distribuito](../../../database-engine/availability-groups/windows/media/always-on-availability-group-distributed.png)

 
 
 Il DDL seguente crea questo gruppo di disponibilità distribuito. 

```sql  
CREATE AVAILABILITY GROUP [SQLFCIDAG]  
   WITH (DISTRIBUTED)   
   AVAILABILITY GROUP ON  
  'SQLFCIAG' WITH    
    (   
        LISTENER_URL = 'tcp://SQLFCIAG-1.contoso.com:5022',    
         AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,   
         FAILOVER_MODE = MANUAL,   
         SEEDING_MODE = AUTOMATIC
      ),   
  'SQLAG-DR' WITH    
       (   
         LISTENER_URL = 'tcp://SQLAG-DR.contoso.com:5022',   
         AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,   
         FAILOVER_MODE = MANUAL,   
         SEEDING_MODE = AUTOMATIC
      );   
```  

L'URL del listener è il nome di rete virtuale dell'istanza FCI primaria.

## <a name="manually-fail-over-fci-in-distributed-availability-group"></a>Eseguire il failover manuale di FCI in un gruppo di disponibilità distribuito

Per eseguire manualmente il failover del gruppo di disponibilità FCI, aggiornare il gruppo di disponibilità distribuito in modo da riflettere la modifica dell'URL del listener. Ad esempio, eseguire il DDL seguente sul gruppo di disponibilità primaria e il gruppo di disponibilità secondaria di SQLFCIAG:

```sql  
ALTER AVAILABILITY GROUP [SQLFCIDAG]  
   MODIFY AVAILABILITY GROUP ON  
 'SQLFCIAG' WITH    
    (   
        LISTENER_URL = 'tcp://SQLFCIAG-2.contoso.com:5022'
    )
```  
  
## <a name="next-steps"></a>Passaggi successivi

 [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/create-availability-group-transact-sql.md)   
 [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md)  
  
