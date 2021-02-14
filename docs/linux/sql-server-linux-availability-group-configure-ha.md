---
title: Configurare un gruppo di disponibilità per SQL Server in Linux
description: Informazioni su come creare un gruppo di disponibilità Always On di SQL Server per la disponibilità elevata in Linux.
author: VanMSFT
ms.custom: seo-lt-2019
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/26/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: 7313fbc789b37cf326d55566c1f087f80b4929df
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "100351943"
---
# <a name="configure-sql-server-always-on-availability-group-for-high-availability-on-linux"></a>Configurare un gruppo di disponibilità Always On di SQL Server per la disponibilità elevata in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo articolo illustra come creare un gruppo di disponibilità Always On di SQL Server per la disponibilità elevata in Linux. Per i gruppi di disponibilità esistono due tipi di configurazione. Una configurazione per la *disponibilità elevata* usa un modulo di gestione cluster per garantire la continuità operativa. Questa configurazione può includere anche le repliche con scalabilità in lettura. Questo documento illustra come creare il gruppo di disponibilità per la disponibilità elevata.

È anche possibile creare un gruppo di disponibilità senza un modulo di gestione cluster per la *scalabilità in lettura*. Il gruppo di disponibilità per la sola scalabilità in lettura fornisce repliche di sola lettura per il miglioramento delle prestazioni. Non fornisce disponibilità elevata. Per creare un gruppo di disponibilità per la scalabilità in lettura, vedere [Configurare un gruppo di disponibilità di SQL Server con scalabilità in lettura per Linux](sql-server-linux-availability-group-configure-rs.md).

Le configurazioni che garantiscono disponibilità elevata e protezione dei dati richiedono due o tre repliche con commit sincrono. Con tre repliche sincrone, il gruppo di disponibilità può essere ripristinato automaticamente anche se un server non è disponibile. Per altre informazioni, vedere [Disponibilità elevata e protezione dei dati per le configurazioni del gruppo di disponibilità](sql-server-linux-availability-group-ha.md). 

Tutti i server devono essere fisici o virtuali e i server virtuali devono trovarsi nella stessa piattaforma di virtualizzazione. Questo requisito è determinato dal fatto che gli agenti di isolamento sono specifici della piattaforma. Vedere [Policies for Guest Clusters](https://access.redhat.com/articles/29440#guest_policies) (Criteri per i cluster guest).

## <a name="roadmap"></a>Roadmap

I passaggi da seguire per creare un gruppo di disponibilità sui server Linux per la disponibilità elevata sono diversi da quelli relativi a un cluster di failover di Windows Server. Nell'elenco seguente sono descritti i passaggi principali: 

1. [Configurare SQL Server nei tre server del cluster](sql-server-linux-setup.md).

   >[!IMPORTANT]
   >Tutti e tre i server del gruppo di disponibilità devono trovarsi nella stessa piattaforma, fisica o virtuale, perché la disponibilità elevata di Linux usa gli agenti di isolamento per isolare le risorse nei server. Gli agenti di isolamento sono specifici per ogni piattaforma.

2. Creare il gruppo di disponibilità. Questo passaggio è illustrato in questo articolo. 

3. Configurare un modulo per la gestione di risorse cluster, ad esempio Pacemaker.
   
   La modalità di configurazione di un modulo per la gestione di risorse cluster dipende dalla specifica distribuzione Linux. Per istruzioni specifiche per le singole distribuzioni, vedere i collegamenti seguenti: 

   * [RHEL](sql-server-linux-availability-group-cluster-rhel.md)
   * [SUSE](sql-server-linux-availability-group-cluster-sles.md)
   * [Ubuntu](sql-server-linux-availability-group-cluster-ubuntu.md)

   >[!IMPORTANT]
   >Per la disponibilità elevata, negli ambienti di produzione è necessario un agente di isolamento, ad esempio STONITH. Nelle dimostrazioni di questa documentazione non vengono usati agenti di isolamento. Le dimostrazioni sono riportate solo a scopo di test e convalida. 
   
   >Un cluster Linux usa l'isolamento per ripristinare uno stato noto del cluster. La modalità di configurazione dell'isolamento dipende dalla distribuzione e dall'ambiente. Attualmente l'isolamento non è disponibile in alcuni ambienti cloud. Per altre informazioni, vedere [Support Policies for RHEL High Availability Clusters - Virtualization Platforms](https://access.redhat.com/articles/29440) (Criteri di supporto per il cluster RHEL a disponibilità elevata - Piattaforme di virtualizzazione).
   
   >Per SLES, vedere [SUSE Linux Enterprise High Availability Extension](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#cha.ha.fencing).

5. Aggiungere il gruppo di disponibilità come risorsa nel cluster.  

   La procedura per aggiungere il gruppo di disponibilità come risorsa nel cluster dipende dalla distribuzione Linux. Per istruzioni specifiche per le singole distribuzioni, vedere i collegamenti seguenti: 

   * [RHEL](sql-server-linux-availability-group-cluster-rhel.md#create-availability-group-resource)
   * [SLES](sql-server-linux-availability-group-cluster-sles.md#configure-the-cluster-resources-for-sql-server)
   * [Ubuntu](sql-server-linux-availability-group-cluster-ubuntu.md#create-availability-group-resource)

[!INCLUDE [Create Prerequisites](../includes/ss-linux-cluster-availability-group-create-prereq.md)]

## <a name="create-the-ag"></a>Creare il gruppo di disponibilità

Gli esempi riportati in questa sezione illustrano come creare il gruppo di disponibilità con Transact-SQL. È anche possibile usare la Creazione guidata Gruppo di disponibilità di SQL Server Management Studio. Quando si crea un gruppo di disponibilità con la procedura guidata e si aggiungono le repliche al gruppo di disponibilità, viene restituito un errore. Per risolvere questo problema, concedere le autorizzazioni `ALTER``CONTROL` e `VIEW DEFINITIONS` a Pacemaker sul gruppo di disponibilità in tutte le repliche. Una volta concesse le autorizzazioni nella replica primaria, aggiungere i nodi al gruppo di disponibilità tramite la procedura guidata, ma per il corretto funzionamento della disponibilità elevata, concedere le autorizzazioni su tutte le repliche.

Per una configurazione a disponibilità elevata che garantisca il failover automatico, il gruppo di disponibilità richiede almeno tre repliche. Per il supporto della disponibilità elevata è necessaria una delle configurazioni seguenti:

- [Tre repliche sincrone](sql-server-linux-availability-group-ha.md#threeSynch)

- [Due repliche sincrone e una replica di configurazione](sql-server-linux-availability-group-ha.md#twoSynch)

Per informazioni, vedere [Disponibilità elevata e protezione dei dati per le configurazioni del gruppo di disponibilità](sql-server-linux-availability-group-ha.md).

>[!NOTE]
>I gruppi di disponibilità possono includere repliche aggiuntive, sincrone o asincrone. 

Creare il gruppo di disponibilità per la disponibilità elevata in Linux. Usare [CREATE AVAILABILITY GROUP](../t-sql/statements/create-availability-group-transact-sql.md) con `CLUSTER_TYPE = EXTERNAL`. 

* Impostare `CLUSTER_TYPE = EXTERNAL` per specificare che il gruppo di disponibilità viene gestito da un'entità cluster esterna. Un esempio di entità cluster esterna è costituito da Pacemaker. Quando il tipo di cluster del gruppo di disponibilità è esterno, 

* Impostare `FAILOVER_MODE = EXTERNAL` per le repliche primarie e secondarie. 
   In questo modo, la replica interagisce con un modulo di gestione cluster esterno, ad esempio Pacemaker. 

Gli script Transact-SQL seguenti creano un gruppo di disponibilità per la disponibilità elevata denominato `ag1`. Lo script configura le repliche del gruppo di disponibilità con `SEEDING_MODE = AUTOMATIC`. In base a questa impostazione, SQL Server crea automaticamente il database in ogni server secondario. Aggiornare lo script seguente per il proprio ambiente. Sostituire il valori di `<node1>`, `<node2>` o `<node3>` con i nomi delle istanze di SQL Server che ospitano le repliche. Sostituire `<5022>` con il numero della porta impostata per l'endpoint del mirroring dei dati. Per creare il gruppo di disponibilità, eseguire gli script Transact-SQL seguenti nell'istanza di SQL Server che ospita la replica primaria.

Eseguire **solo uno** degli script seguenti: 

- [Creare un gruppo di disponibilità con tre repliche sincrone](#threeSynch)
- [Creare un gruppo di disponibilità con due repliche sincrone e una replica di configurazione](#configOnly)
- [Creare un gruppo di disponibilità con due repliche sincrone](#readScale)

<a name="threeSynch"></a>

- Creare un gruppo di disponibilità con tre repliche sincrone

   ```SQL
   CREATE AVAILABILITY GROUP [ag1]
       WITH (DB_FAILOVER = ON, CLUSTER_TYPE = EXTERNAL)
       FOR REPLICA ON
           N'<node1>' 
            WITH (
               ENDPOINT_URL = N'tcp://<node1>:<5022>',
               AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
               FAILOVER_MODE = EXTERNAL,
               SEEDING_MODE = AUTOMATIC
               ),
           N'<node2>' 
            WITH ( 
               ENDPOINT_URL = N'tcp://<node2>:<5022>', 
               AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
               FAILOVER_MODE = EXTERNAL,
               SEEDING_MODE = AUTOMATIC
               ),
           N'<node3>'
           WITH( 
              ENDPOINT_URL = N'tcp://<node3>:<5022>', 
              AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
              FAILOVER_MODE = EXTERNAL,
              SEEDING_MODE = AUTOMATIC
              );
        
   ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
   ```

   >[!IMPORTANT]
   >Dopo aver eseguito lo script precedente per creare un gruppo di disponibilità con tre repliche sincrone, non eseguire lo script seguente:

<a name="configOnly"></a>
- Creare un gruppo di disponibilità con due repliche sincrone e una replica di configurazione:

   >[!IMPORTANT]
   >Questa architettura consente a qualsiasi edizione di SQL Server di ospitare la terza replica. La terza replica, ad esempio, può essere ospitata in SQL Server Express Edition. In Express Edition l'unico tipo di endpoint valido è `WITNESS`. 

   ```SQL
   CREATE AVAILABILITY GROUP [ag1] 
      WITH (CLUSTER_TYPE = EXTERNAL) 
      FOR REPLICA ON 
       N'<node1>' WITH ( 
          ENDPOINT_URL = N'tcp://<node1>:<5022>', 
          AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, 
          FAILOVER_MODE = EXTERNAL, 
          SEEDING_MODE = AUTOMATIC 
          ), 
       N'<node2>' WITH (  
          ENDPOINT_URL = N'tcp://<node2>:<5022>',  
          AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, 
          FAILOVER_MODE = EXTERNAL, 
          SEEDING_MODE = AUTOMATIC 
          ), 
       N'<node3>' WITH ( 
          ENDPOINT_URL = N'tcp://<node3>:<5022>', 
          AVAILABILITY_MODE = CONFIGURATION_ONLY  
          );
   ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
   ```
<a name="readScale"></a>

- Creare un gruppo di disponibilità con due repliche sincrone

   Includere due repliche con modalità di disponibilità sincrona. Ad esempio, lo script seguente crea un gruppo di disponibilità denominato `ag1`. `node1` e `node2` ospitano le repliche in modalità sincrona con seeding e failover automatici.

   >[!IMPORTANT]
   >Per creare un gruppo di disponibilità con due repliche sincrone eseguire solo lo script seguente. Non eseguire lo script seguente se è stato eseguito uno degli script precedenti. 

   ```SQL
   CREATE AVAILABILITY GROUP [ag1]
      WITH (CLUSTER_TYPE = EXTERNAL)
      FOR REPLICA ON
      N'node1' WITH (
         ENDPOINT_URL = N'tcp://node1:5022',
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
         FAILOVER_MODE = EXTERNAL,
         SEEDING_MODE = AUTOMATIC
      ),
      N'node2' WITH ( 
         ENDPOINT_URL = N'tcp://node2:5022', 
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
         FAILOVER_MODE = EXTERNAL,
         SEEDING_MODE = AUTOMATIC
      );
        
   ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
   ```


È anche possibile configurare un gruppo di disponibilità con `CLUSTER_TYPE=EXTERNAL` usando SQL Server Management Studio o PowerShell. 

### <a name="join-secondary-replicas-to-the-ag"></a>Aggiungere le repliche secondarie al gruppo di disponibilità

L'utente di Pacemaker richiede le autorizzazioni `ALTER`, `CONTROL` e `VIEW DEFINITION` sul gruppo di disponibilità in tutte le repliche. Per concedere le autorizzazioni, eseguire lo script Transact-SQL seguente dopo che il gruppo di disponibilità è stato creato nella replica primaria e in ogni replica secondaria immediatamente dopo l'aggiunta al gruppo di disponibilità. Prima di eseguire lo script, sostituire `<pacemakerLogin>` con il nome dell'account utente di Pacemaker. Se non è disponibile un account di accesso per Pacemaker, [creare un account di accesso di SQL Server per Pacemaker](sql-server-linux-availability-group-cluster-ubuntu.md#create-a-sql-server-login-for-pacemaker).

```sql
GRANT ALTER, CONTROL, VIEW DEFINITION ON AVAILABILITY GROUP::ag1 TO <pacemakerLogin>
GRANT VIEW SERVER STATE TO <pacemakerLogin>
```

Lo script Transact-SQL seguente aggiunge un'istanza di SQL Server a un gruppo di disponibilità denominato `ag1`. Aggiornare lo script per il proprio ambiente. In ogni istanza di SQL Server che ospita una replica secondaria eseguire lo script Transact-SQL seguente per aggiungerla al gruppo di disponibilità.

```sql
ALTER AVAILABILITY GROUP [ag1] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
         
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

[!INCLUDE [Create Post](../includes/ss-linux-cluster-availability-group-create-post.md)]

>[!IMPORTANT]
>Dopo aver creato il gruppo di disponibilità, è necessario configurare l'integrazione con uno strumento per la gestione di cluster come Pacemaker per la disponibilità elevata. Per una configurazione di scalabilità in lettura con gruppi di disponibilità, a partire da [!INCLUDE [SQL Server version](../includes/sssql17-md.md)], non è necessario configurare un cluster.

Se si è seguita la procedura descritta in questo documento, si ha un gruppo di disponibilità non ancora configurato in cluster. Il passaggio successivo consiste nell'aggiungere il cluster. Questa configurazione è valida per gli scenari di scalabilità in lettura/bilanciamento del carico, ma non è completa per la disponibilità elevata. Per ottenere la disponibilità elevata, è necessario aggiungere il gruppo di disponibilità come risorsa cluster. Vedere [Passaggi successivi](#next-steps) per le istruzioni. 

## <a name="notes"></a>Note

>[!IMPORTANT]
>Dopo aver configurato il cluster e aggiunto il gruppo di disponibilità come risorsa cluster, non è possibile usare Transact-SQL per eseguire il failover delle risorse del gruppo di disponibilità. Le risorse cluster di SQL Server in Linux non sono strettamente associate al sistema operativo come in un cluster WSFC (Windows Server Failover Cluster). Il servizio SQL Server non è a conoscenza della presenza del cluster. Tutta l'orchestrazione viene eseguita tramite gli strumenti per la gestione di cluster. In RHEL o Ubuntu usare `pcs`. In SLES usare `crm`. 

>[!IMPORTANT]
>Se il gruppo di disponibilità è una risorsa cluster, nella versione corrente è stato rilevato un problema per cui il failover forzato con perdita di dati in una replica asincrona non funziona. Il problema verrà risolto nella prossima versione. Il failover manuale o automatico in una replica sincrona viene eseguito correttamente.


## <a name="next-steps"></a>Passaggi successivi

[Configurare un cluster Red Hat Enterprise Linux per le risorse cluster di un gruppo di disponibilità di SQL Server](sql-server-linux-availability-group-cluster-rhel.md)

[Configurare un cluster SUSE Linux Enterprise Server per le risorse cluster di un gruppo di disponibilità di SQL Server](sql-server-linux-availability-group-cluster-sles.md)

[Configurare un cluster Ubuntu per le risorse cluster di un gruppo di disponibilità di SQL Server](sql-server-linux-availability-group-cluster-ubuntu.md)