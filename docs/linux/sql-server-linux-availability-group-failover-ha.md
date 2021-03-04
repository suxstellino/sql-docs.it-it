---
title: Gestire il failover di un gruppo di disponibilità - SQL Server in Linux
description: 'Questo articolo descrive i tipi di failover: failover manuale pianificato e automatico e failover manuale forzato. Con il failover automatico e il failover manuale pianificato vengono conservati tutti i dati.'
author: tejasaks
ms.author: tejasaks
ms.reviewer: vanto
ms.date: 03/01/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: 863278eacebc4b405a4a44e72c4318e950d0cc1d
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837672"
---
# <a name="always-on-availability-group-failover-on-linux"></a>Failover di un gruppo di disponibilità Always On in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Nel contesto di un gruppo di disponibilità, il ruolo primario e il ruolo secondario delle repliche di disponibilità sono generalmente intercambiabili in un processo noto come failover. Sono disponibili tre tipi di failover: failover automatico (senza perdita di dati), failover manuale pianificato (senza perdita di dati) e failover manuale forzato (con possibile perdita di dati), in genere chiamato *failover forzato*. Il failover automatico e il failover manuale pianificato consentono di conservare tutti i dati. Per un gruppo di disponibilità, il failover avviene a livello di replica di disponibilità. In pratica, il failover di un gruppo di disponibilità viene eseguito in una delle relative repliche secondarie, ovvero la destinazione di failover corrente. 

Per informazioni di base sul failover, vedere [Failover e modalità di failover](../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md).

## <a name="manual-failover"></a><a name="failover"></a>Failover manuale

Usare gli strumenti per la gestione di cluster per eseguire il failover di un gruppo di disponibilità gestito tramite un modulo di gestione cluster esterno. Se, ad esempio, una soluzione usa Pacemaker per gestire un cluster Linux, usare `pcs` per eseguire failover manuali in RHEL o Ubuntu. In SLES usare `crm`. 

> [!IMPORTANT]
> In condizioni normali, non eseguire il failover con Transact-SQL o con gli strumenti di gestione di SQL Server come SSMS o PowerShell. Quando `CLUSTER_TYPE = EXTERNAL`, l'unico valore accettabile per `FAILOVER_MODE` è `EXTERNAL`. Con queste impostazioni, tutte le azioni di failover manuale o automatico vengono eseguite dal modulo di gestione cluster esterno. Per istruzioni su come forzare il failover con potenziale perdita di dati, vedere [Forzare il failover](#forceFailover).

### <a name="a-namemanualfailovermanual-failover-steps"></a><a name="manualFailover">Passaggi per il failover manuale

Per eseguire il failover, la replica secondaria che diventerà primaria deve essere sincrona. Se una replica secondaria è asincrona, [modificare la modalità di disponibilità](../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md).

Il failover manuale prevede due passaggi.

   Prima di tutto, [si esegue manualmente il failover spostando la risorsa del gruppo di disponibilità](#manualMove) dal nodo del cluster proprietario delle risorse in un nuovo nodo.

   Il cluster esegue il failover della risorsa del gruppo di disponibilità e aggiunge un vincolo di posizione. Con questo vincolo, la risorsa viene configurata per l'esecuzione nel nuovo nodo. Il vincolo deve essere rimosso per una corretta esecuzione del failover in futuro.

   Successivamente, [si rimuove il vincolo di posizione](#removeLocConstraint).

#### <a name="step-1-manually-fail-over-by-moving-availability-group-resource"></a><a name="manualMove"></a> Passaggio 1. Eseguire manualmente il failover spostando la risorsa del gruppo di disponibilità

Per eseguire manualmente il failover di una risorsa del gruppo di disponibilità denominata *ag_cluster* in un nodo del cluster denominato *nodeName2*, eseguire il comando appropriato per la distribuzione:

- **Esempio di RHEL/Ubuntu**

   ```bash
   sudo pcs resource move ag_cluster-master nodeName2 --master --lifetime=30S
   ```

- **Esempio di SLES**

   ```bash
   crm resource migrate ag_cluster nodeName2 --lifetime=30S
   ```

>[!IMPORTANT]
>Quando si usa l'opzione--Lifetime, il vincolo location creato per spostare la risorsa è di natura temporanea ed è valido per 30 secondi nell'esempio precedente.
>Si noti che il vincolo temporaneo non viene cancellato automaticamente e può essere visualizzato nell'elenco di vincoli, ma come vincolo scaduto. I vincoli scaduti non influiscono sul comportamento di failover del cluster pacemaker. Se non si usa l'opzione--Lifetime durante lo spostamento della risorsa, è necessario rimuovere un vincolo location aggiunto automaticamente come indicato di seguito.

#### <a name="step-2-remove-the-location-constraint"></a><a name="removeLocConstraint"> </a> Passaggio 2. Rimuovere il vincolo di posizione

Durante un failover manuale, il comando `pcs``move` o il comando `crm``migrate` aggiunge un vincolo di posizione per consentire il posizionamento della risorsa nel nuovo nodo di destinazione. Per visualizzare il nuovo vincolo, eseguire il comando seguente dopo aver spostato manualmente la risorsa:

- **Esempio di RHEL/Ubuntu**

   ```bash
   sudo pcs constraint list --full
   ```

- **Esempio di SLES**

   ```bash
   crm config show
   ```

Ecco un esempio di vincolo creato per effetto di un failover manuale. 
 `Enabled on: Node1 (score:INFINITY) (role: Master) (id:cli-prefer-ag_cluster-master)`

   > [!NOTE]
   > Il nome della risorsa del gruppo di disponibilità nei cluster Pacemaker in Red Hat Enterprise Linux 8. x e Ubuntu 18.04 potrebbe essere simile *ag_cluster-clone* poiché la nomenclatura relativa alle risorse è passata all'uso di *risorse clone promotable*. 

- **Esempio di RHEL/Ubuntu**

   Nel comando seguente `cli-prefer-ag_cluster-master` è l'ID del vincolo che deve essere rimosso. `sudo pcs constraint list --full` restituisce questo ID. 
   
   ```bash
   sudo pcs resource clear ag_cluster-master  
   ```
   Oppure
   
   ```bash
   sudo pcs constraint remove cli-prefer-ag_cluster-master  
   ```
  
   In alternativa, è possibile eseguire lo spostamento e la cancellazione dei vincoli generati automaticamente in un'unica riga come descritto di seguito. L'esempio seguente usa la terminologia delle *risorse clone* in base a Red Hat Enterprise Linux 8.x. 
  
   ```bash
   sudo pcs resource move ag_cluster-clone --master nodeName2 && sleep 30 && sudo pcs resource clear ag_cluster-clone

   ```
  
- **Esempio di SLES**

   Nel comando seguente `cli-prefer-ms-ag_cluster` è l'ID del vincolo. `crm config show` restituisce questo ID. 
   
   ```bash
   crm configure
   delete cli-prefer-ms-ag_cluster 
   commit
   ```

>[!NOTE]
>Il failover automatico non comporta l'aggiunta di un vincolo di posizione, quindi non è necessaria alcuna operazione di pulizia. 

Per altre informazioni:
- [Red Hat - Managing Cluster Resources](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Configuring_the_Red_Hat_High_Availability_Add-On_with_Pacemaker/ch-manageresource-HAAR.html) (Red Hat - Gestione di risorse cluster)
- [Pacemaker - Move Resources Manually](https://clusterlabs.org/pacemaker/doc/en-US/Pacemaker/1.1/html/Clusters_from_Scratch/_move_resources_manually.html)
  (Pacemaker - Spostare manualmente le risorse) [SLES Administration Guide - Resources](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#sec.ha.troubleshooting.resource) (Guida all'amministrazione di SLES - Risorse) 
 
## <a name="force-failover"></a><a name="forceFailover"></a> Forzare il failover 

Il failover forzato è destinato essenzialmente al ripristino di emergenza. In questo caso non è possibile eseguire il failover con gli strumenti per la gestione di cluster perché il data center principale non è attivo. Se si forza il failover in una replica secondaria non sincronizzata, è possibile che vengano persi alcuni dati. Forzare il failover solo se è necessario ripristinare immediatamente il servizio nel gruppo di disponibilità e si è disposti a rischiare la perdita di dati.

Se non è possibile usare gli strumenti per la gestione di cluster per interagire con il cluster, ad esempio se il cluster non risponde a causa di un evento di emergenza nel data center primario, potrebbe essere necessario forzare il failover in modo da ignorare il modulo di gestione cluster esterno. Questa procedura è sconsigliata per le normali operazioni perché comporta il rischio di perdita di dati. Adottarla quando gli strumenti per la gestione di cluster non riescono a eseguire l'azione di failover. Dal punto di vista funzionale, questa procedura è simile all'[esecuzione di un failover manuale forzato](../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md) su un gruppo di disponibilità in Windows.
 
La procedura per forzare il failover illustrata di seguito è specifica di SQL Server in Linux.

1. Verificare che la risorsa del gruppo di disponibilità non sia più gestita dal cluster. 

      - Impostare la risorsa in modalità non gestita nel nodo del cluster di destinazione. Questo comando segnala all'agente di arrestare il monitoraggio e la gestione della risorsa. Ad esempio: 
      
      ```bash
      sudo pcs resource unmanage <resourceName>
      ```

      - Se il tentativo di impostare la risorsa in modalità non gestita non riesce, eliminare la risorsa. Ad esempio:

      ```bash
      sudo pcs resource delete <resourceName>
      ```

      >[!NOTE]
      >Quando si elimina una risorsa, vengono eliminati anche tutti i vincoli associati. 

1. Nell'istanza di SQL Server che ospita la replica secondaria, impostare la variabile del contesto della sessione `external_cluster`.

   ```Transact-SQL
   EXEC sp_set_session_context @key = N'external_cluster', @value = N'yes';
   ```

1. Eseguire il failover del gruppo di disponibilità con Transact-SQL. Nell'esempio seguente sostituire `<MyAg>` con il nome del gruppo di disponibilità. Connettersi all'istanza di SQL Server che ospita la replica secondaria di destinazione ed eseguire il comando seguente:

   ```Transact-SQL
   ALTER AVAILABILITY GROUP <MyAg> FORCE_FAILOVER_ALLOW_DATA_LOSS;
   ```

1.  Dopo un failover forzato, ripristinare lo stato integro del gruppo di disponibilità prima di riavviare il monitoraggio e la gestione delle risorse cluster o di creare nuovamente la risorsa del gruppo di disponibilità. Rivedere [Attività essenziali dopo un failover forzato](../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md#FollowUp).

1.  Riavviare il monitoraggio e la gestione delle risorse cluster:

   Per riavviare il monitoraggio e la gestione delle risorse cluster, eseguire il comando seguente:

   ```bash
   sudo pcs resource manage <resourceName>
   sudo pcs resource cleanup <resourceName>
   ```

   Se la risorsa cluster è stata eliminata, crearla di nuovo. Per creare nuovamente la risorsa cluster, seguire le istruzioni in [Creare una risorsa del gruppo di disponibilità](sql-server-linux-availability-group-cluster-rhel.md#create-availability-group-resource).

>[!Important]
>Non usare i passaggi precedenti per le esercitazioni sul ripristino di emergenza perché comportano il rischio di perdita di dati. In alternativa, modificare la replica asincrona in sincrona e seguire le istruzioni per il [failover manuale normale](#manualFailover).

## <a name="database-level-monitoring-and-failover-trigger"></a>Monitoraggio a livello di database e trigger di failover

Per `CLUSTER_TYPE=EXTERNAL`, la semantica del trigger di failover è diversa rispetto al cluster WSFC. Quando il gruppo di disponibilità si trova in un'istanza di SQL Server in un cluster WSFC, la disattivazione dello stato `ONLINE` del database ha l'effetto di generare un errore di integrità del gruppo di disponibilità. In risposta a questa situazione, il modulo di gestione cluster attiva un'azione di failover. In Linux, l'istanza di SQL Server non è in grado di comunicare con il cluster. Il monitoraggio dell'integrità del database viene eseguito *dall'esterno verso l'interno*. Se l'utente ha acconsentito esplicitamente al monitoraggio e al failover a livello di database (impostando l'opzione `DB_FAILOVER=ON` durante la creazione del gruppo di disponibilità), il cluster verificherà se lo stato del database è `ONLINE` ogni volta che eseguirà un'azione di monitoraggio. Il cluster esegue una query sullo stato in `sys.databases`. Per qualsiasi stato diverso da `ONLINE`, verrà attivato automaticamente un failover (se le condizioni di failover automatico sono soddisfatte). Il tempo effettivo del failover dipende dalla frequenza dell'azione di monitoraggio e dallo stato di aggiornamento del database in sys.databases.

Il failover automatico richiede almeno una replica sincrona.

## <a name="next-steps"></a>Passaggi successivi

[Configurare un cluster Red Hat Enterprise Linux per le risorse cluster di un gruppo di disponibilità di SQL Server](sql-server-linux-availability-group-cluster-rhel.md)

[Configurare un cluster SUSE Linux Enterprise Server per le risorse cluster di un gruppo di disponibilità di SQL Server](sql-server-linux-availability-group-cluster-sles.md)

[Configurare un cluster Ubuntu per le risorse cluster di un gruppo di disponibilità di SQL Server](sql-server-linux-availability-group-cluster-ubuntu.md)
