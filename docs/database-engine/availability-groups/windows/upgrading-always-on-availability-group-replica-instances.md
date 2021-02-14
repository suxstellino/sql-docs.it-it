---
title: Aggiornare repliche del gruppo di disponibilità
description: Informazioni su come ridurre i tempi di inattività per la replica primaria durante gli aggiornamenti di SQL Server eseguendo un aggiornamento in sequenza.
ms.custom: seo-lt-2019
ms.date: 01/10/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
ms.assetid: f670af56-dbcc-4309-9119-f919dcad8a65
author: cawrites
ms.author: chadam
ms.openlocfilehash: f0a1e63760b0a584a4e89cb20253b3df21bef9c9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340859"
---
# <a name="upgrading-always-on-availability-group-replica-instances"></a>Aggiornamento delle istanze di replica dei gruppi di disponibilità AlwaysOn
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Quando si aggiorna un'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] che ospita un gruppo di disponibilità AlwaysOn a una nuova versione di [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)], a un nuovo Service Pack o aggiornamento cumulativo di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] oppure quando la si installa su un nuovo Service Pack o aggiornamento cumulativo di Windows, è possibile ridurre i tempi di inattività per la replica primaria a un singolo failover manuale eseguendo un aggiornamento in sequenza (o a due failover manuali in caso di failback sulla replica primaria originale). Durante il processo di aggiornamento, non sarà disponibile una replica secondaria per il failover o le operazioni di sola lettura e, dopo l'aggiornamento, a seconda del volume di attività nel nodo della replica primaria, l'aggiornamento della replica secondaria al nodo della replica primaria potrebbe richiedere un po' di tempo (è dunque prevedibile un traffico di rete elevato). Occorre sapere anche che dopo il failover iniziale in una replica secondaria che esegue una versione più recente di SQL Server, i database di tale gruppo di disponibilità saranno sottoposti a un processo di aggiornamento affinché la versione adottata sia la più recente. Durante questa operazione le repliche non saranno leggibili per nessuno di questi database. Il tempo di inattività dopo il failover iniziale dipenderà dal numero di database contenuti nel gruppo di disponibilità. Se si prevede di eseguire il failback sulla replica primaria originale, questo passaggio non sarà ripetuto durante il failback.
  
>[!NOTE]  
>Questo articolo si limita a illustrare l'aggiornamento di SQL Server. Non viene descritto l'aggiornamento del sistema operativo che contiene WSFC (Windows Server Failover Cluster). L'aggiornamento del sistema operativo Windows che ospita il cluster di failover non è supportato per i sistemi operativi precedenti a Windows Server 2012 R2. Per aggiornare un nodo del cluster in esecuzione in Windows Server 2012 R2, vedere [Aggiornamento in sequenza del sistema operativo del cluster](/windows-server/failover-clustering/cluster-operating-system-rolling-upgrade).  
  
## <a name="prerequisites"></a>Prerequisiti  
Prima di iniziare, esaminare le informazioni seguenti:  
  
- [Aggiornamenti di versione ed edizione supportati](../../../database-engine/install-windows/supported-version-and-edition-upgrades.md): verificare che sia possibile eseguire l'aggiornamento a SQL Server 2016 dalla versione del sistema operativo Windows e di SQL Server in uso. Ad esempio, non è possibile eseguire l'aggiornamento diretto da un'istanza di SQL Server 2005 a [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)].  
  
- [Scegliere un metodo di aggiornamento del motore di database](../../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md): per l'aggiornamento nell'ordine corretto selezionare il metodo e la procedura di aggiornamento appropriati in base alla verifica degli aggiornamenti di versione ed edizione supportati e anche agli altri componenti installati nell'ambiente.  
  
- [Pianificare e testare il piano di aggiornamento del motore di database](../../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md): esaminare le note sulla versione, i problemi di aggiornamento noti, l'elenco di controllo pre-aggiornamento e sviluppare e testare il piano di aggiornamento.  
  
- [Requisiti hardware e software per l'installazione di SQL Server](../../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md):  esaminare i requisiti software per l'installazione di [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]. Se è necessario software aggiuntivo, installarlo in ogni nodo prima di iniziare il processo di aggiornamento per ridurre al minimo eventuali tempi di inattività.  

- [Verificare se per i database del gruppo di disponibilità siano in uso Change Data Capture o la replica](#special-steps-for-change-data-capture-or-replication): se i database nel gruppo di disponibilità sono abilitati per Change Data Capture (CDC), seguire queste [istruzioni](#special-steps-for-change-data-capture-or-replication).

>[!NOTE]  
>L'uso di più versioni delle istanze di SQL Server nello stesso gruppo di disponibilità è supportato solo nel contesto di un aggiornamento in sequenza e non deve rimanere in tale stato per periodi di tempo prolungati perché l'aggiornamento deve essere eseguito rapidamente. L'altra opzione per eseguire l'aggiornamento di SQL Server 2016 e versioni successive prevede l'uso di un gruppo di disponibilità distribuito.

## <a name="rolling-upgrade-basics-for-always-on-ags"></a>Informazioni di base sull'aggiornamento in sequenza per i gruppi di disponibilità AlwaysOn  
Per ridurre al minimo i tempi di inattività e la perdita di dati per i gruppi di disponibilità, osservare le linee guida seguenti quando si eseguono gli aggiornamenti dei server:  
  
- Prima di avviare l'aggiornamento in sequenza:  
  
    - Eseguire un failover manuale di prova su almeno una delle istanze di replica con commit sincrono  
  
    - Proteggere i dati eseguendo un backup completo di ogni database di disponibilità  
  
    - Eseguire il comando DBCC CHECKDB su ogni database di disponibilità  
  
-   Aggiornare sempre prima le istanze di replica secondaria remota, quindi quelle di replica secondaria locale e, infine, l'istanza di replica primaria.  
  
-   Non è possibile eseguire backup su un database in corso di aggiornamento.  Prima di eseguire l'aggiornamento delle repliche secondarie, configurare la preferenza per i backup automatici in modo che vengano eseguiti solo sulla replica primaria.  Durante un aggiornamento di versione, le repliche non sono né leggibili né disponibili per i backup. Durante un aggiornamento non di versione, è possibile configurare l'esecuzione di backup automatici sulle repliche secondarie prima di aggiornare la replica primaria.  
  
-   Durante un aggiornamento di versione, non è possibile leggere repliche secondarie leggibili dopo il relativo aggiornamento e prima che venga eseguito il failover della replica primaria su una replica secondaria aggiornata o che venga aggiornata la replica primaria.  
  
-   Per evitare failover accidentali del gruppo di disponibilità durante il processo di aggiornamento, rimuovere il failover da tutte le repliche con commit sincrono prima di iniziare.  
  
-   Non aggiornare l'istanza di replica primaria prima di aver eseguito il failover del gruppo di disponibilità su un'istanza aggiornata con una replica secondaria. In caso contrario, le applicazioni client potrebbero subire tempi di inattività prolungati durante l'aggiornamento sull'istanza di replica primaria.  
  
-   Eseguire sempre il failover del gruppo di disponibilità su un'istanza di replica secondaria con commit sincrono. Se si esegue il failover su un'istanza di replica secondaria con commit asincrono, i database sono soggetti alla perdita di dati e lo spostamento dei dati viene automaticamente sospeso fino a quando non viene ripreso manualmente.  
  
-   Non aggiornare l'istanza di replica primaria prima di aver aggiornato tutte le altre istanze di replica secondaria. Non è più possibile recapitare log tramite una replica primaria aggiornata alle repliche secondarie la cui istanza di [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] non è ancora stata aggiornata alla stessa versione. Se viene sospeso lo spostamento dei dati in una replica secondaria, il failover automatico per quest'ultima non può essere eseguito e nei database di disponibilità possono verificarsi perdite di dati. Questo vale anche per un aggiornamento in sequenza in cui si esegue manualmente il failover da una replica primaria precedente a una nuova replica primaria. In quanto tale, dopo aver aggiornato la replica primaria precedente, potrebbe essere necessario riprendere la sincronizzazione.
  
-   Prima di eseguire il failover di un gruppo di disponibilità, verificare che lo stato di sincronizzazione della destinazione di failover sia SINCRONIZZATO.  

  > [!WARNING]
  > L'installazione di una nuova istanza o nuova versione di SQL Server in un server in cui è installata una versione precedente di SQL Server può inavvertitamente **causare un'interruzione per i gruppi di disponibilità ospitati dalla versione precedente di SQL Server.** Ciò accade perché durante l'installazione dell'istanza o della versione di SQL Server, il modulo di disponibilità elevata di SQL Server (RHS.EXE) viene aggiornato. Questo aggiornamento determina un'interruzione temporanea dei gruppi di disponibilità esistenti nel ruolo primario del server. Quando si installa una versione più recente di SQL Server in un sistema che ospita già una versione precedente di SQL Server con un gruppo di disponibilità è pertanto consigliabile seguire una delle procedure seguenti:
  > - Installare la nuova versione di SQL Server durante una finestra di manutenzione. 
  > - Eseguire il failover del gruppo di disponibilità nella replica secondaria in modo che non risulti primaria durante l'installazione della nuova istanza di SQL Server. 
  
## <a name="rolling-upgrade-process"></a>Processo di aggiornamento in sequenza  
 Il processo effettivo dipende da fattori quali la topologia di distribuzione dei gruppi di disponibilità e la modalità di commit di ogni replica. Nello scenario più semplice, l'aggiornamento in sequenza è articolato in un processo a più fasi che nella sua forma essenziale prevede i passaggi seguenti:  
  
 ![Aggiornamento del gruppo di disponibilità in uno scenario di disponibilità elevata e ripristino di emergenza](../../../database-engine/availability-groups/windows/media/alwaysonupgrade-ag-hadr.gif "Aggiornamento del gruppo di disponibilità in uno scenario di disponibilità elevata e ripristino di emergenza")  
  
1.  Rimuovere il failover automatico in tutte le repliche con commit sincrono  
  
2.  Aggiornare tutte le istanze di replica secondaria con commit asincrono. 
  
3.  Aggiornare tutte le istanze di replica secondaria con commit sincrono remote. 

4.  Aggiornare tutte le istanze di replica secondaria con commit sincrono locali. 
  
4.  Eseguire manualmente il failover del gruppo di disponibilità su una replica secondaria con commit sincrono locale appena aggiornata.  
  
5.  Aggiornare l'istanza di replica locale in cui, in precedenza, era ospitata la replica primaria.  
  
6.  Configurare i partner di failover automatico in base alle proprie preferenze.
  
 Se necessario, è possibile eseguire un ulteriore failover manuale per ripristinare la configurazione originale del gruppo di disponibilità.  
 
   > [!NOTE]
   > - L'aggiornamento di una replica con commit sincrono e la relativa impostazione offline non comporteranno un ritardo delle transazioni nella replica primaria. Dopo che la replica secondaria è stata disconnessa, il commit delle transazioni avviene nella replica primaria senza attendere la finalizzazione dei log nella replica secondaria. 
   > - Se `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` è impostato su `1` o `2`, la replica primaria può non risultare disponibile per le operazioni di lettura/scrittura quando non è disponibile un numero corrispondente di repliche secondarie di sincronizzazione durante il processo di aggiornamento. 
  
## <a name="ag-with-one-remote-secondary-replica"></a>Gruppo di disponibilità con una replica secondaria remota  
 Se è stato distribuito un gruppo di disponibilità solo a fini di ripristino di emergenza potrebbe essere necessario eseguirne il failover su una replica secondaria con commit asincrono. Questo tipo di configurazione è illustrato nella figura seguente:  
  
 ![Aggiornamento del gruppo di disponibilità in uno scenario di ripristino di emergenza](../../../database-engine/availability-groups/windows/media/agupgrade-ag-dr.gif "Aggiornamento del gruppo di disponibilità in uno scenario di ripristino di emergenza")  
  
 In questo caso è necessario eseguire il failover del gruppo di disponibilità su una replica secondaria con commit asincrono durante l'aggiornamento in sequenza. Per evitare perdite di dati, modificare la modalità di commit impostando il commit sincrono e attendere che venga completata la sincronizzazione della replica secondaria prima di eseguire il failover del gruppo di disponibilità. Il processo di aggiornamento in sequenza può quindi avvenire come segue:  
  
1.  Aggiornare l'istanza di replica secondaria sul sito remoto  
  
2.  Impostare la modalità di commit sincrono  
  
3.  Attendere che lo stato di sincronizzazione sia SINCRONIZZATO  
  
4.  Eseguire il failover del gruppo di disponibilità sulla replica secondaria nel sito remoto  
  
5.  Aggiornare l'istanza di replica (sito primario) locale  
  
6.  Eseguire di nuovo il failover del gruppo di disponibilità sul sito primario  
  
7.  Impostare la modalità di commit asincrono  
  
 Poiché la modalità di commit sincrono non è consigliata per la sincronizzazione dei dati con un sito remoto, tramite le applicazioni client potrebbe essere rilevato un aumento immediato della latenza del database in seguito alla modifica dell'impostazione. L'esecuzione di un failover causa inoltre la rimozione di tutti i messaggi di log non riconosciuti. Il numero di messaggi di log rimossi può essere notevole a causa dell'elevata latenza di rete tra i due siti determinando un numero elevato di errori delle transazioni nei client. È possibile ridurre l'impatto sulle applicazioni client con le azioni seguenti:  
  
-   Scegliere con attenzione una finestra di manutenzione nei periodi di minore traffico client  
  
-   Durante l'aggiornamento di [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] nel sito primario, impostare di nuovo la modalità di commit asincrono, quindi ripristinare il commit sincrono una volta pronti a effettuare nuovamente il failover sul sito primario  
  
## <a name="ag-with-failover-cluster-instance-nodes"></a>Gruppo di disponibilità con nodi di istanze del cluster di failover  
 Se in un gruppo di disponibilità sono contenuti nodi di istanze del cluster di failover, è consigliabile aggiornare i nodi inattivi prima di quelli attivi. Nella figura seguente è illustrato uno scenario comune di gruppi di disponibilità con istanze del cluster di failover per la disponibilità elevata locale e il commit asincrono tra le istanze stesse ai fini del ripristino di emergenza remoto ed è indicata la relativa sequenza di aggiornamento.  
  
 ![Aggiornamento del gruppo di disponibilità con FCI](../../../database-engine/availability-groups/windows/media/agupgrade-ag-fci-dr.gif "Aggiornamento del gruppo di disponibilità con FCI")  
  
1.  Aggiornare REMOTE2  
  
2.  Effettuare il failover dell'istanza FCI2 a REMOTE2  
  
3.  Aggiornare REMOTE1  
  
4.  Aggiornare PRIMARY2  
  
5.  Effettuare il failover dell'istanza FCI1 a PRIMARY2  
  
6.  Aggiornare PRIMARY1  
  
## <a name="upgrade-or-update-sql-server-instances-with-multiple-ags"></a>Aggiornare le istanze di SQL Server con più gruppi di disponibilità  
 Se si eseguono più gruppi di disponibilità con repliche primarie su nodi server distinti (configurazione Attiva/Attiva), il percorso di aggiornamento prevede altri passaggi di failover per preservare la disponibilità elevata durante il processo. Si supponga di avere tre gruppi di disponibilità in tre nodi server con tutte le repliche in modalità commit sincrono come illustrato nella tabella seguente:  
  
|AG|Nodo1|Nodo2|Nodo3|  
|------------------------|-----------|-----------|-----------|  
|AG1|Primaria|||  
|AG2||Primaria||  
|AG3|||Primaria|  
  
 In determinate situazioni potrebbe essere opportuno eseguire un aggiornamento in sequenza con bilanciamento del carico articolato come segue:  
  
1.  Effettuare il failover di AG2 a Nodo3 (per liberare Nodo2)  
  
2.  Aggiornare Nodo2  
  
3.  Effettuare il failover di AG1 a Nodo2 (per liberare Nodo1)  
  
4.  Aggiornare Nodo1  
  
5.  Effettuare il failover di AG2 e AG3 a Nodo1 (per liberare Nodo3)  
  
6.  Aggiornare Nodo3  
  
7.  Effettuare il failover di AG3 a Nodo3  
  
 Questa sequenza di aggiornamento implica un tempo di inattività medio inferiore alla durata di due failover per gruppo di disponibilità. La configurazione risultante è illustrata nella tabella seguente.  
  
|AG|Nodo1|Nodo2|Nodo3|  
|------------------------|-----------|-----------|-----------|  
|AG1||Primaria||  
|AG2|Primaria|||  
|AG3|||Primaria|  
  
 Il percorso di aggiornamento e i tempi di inattività per le applicazioni client possono variare a seconda della specifica implementazione in uso.  
  
> [!NOTE]  
>  In molti casi, dopo aver completato l'aggiornamento in sequenza, sarà possibile eseguire il failback sulla replica primaria originale. 

## <a name="rolling-upgrade-of-a-distributed-availability-group"></a>Aggiornamento in sequenza di un gruppo di disponibilità distribuito
Per eseguire un aggiornamento in sequenza di un gruppo di disponibilità distribuito, aggiornare prima tutte le repliche secondarie. Quindi eseguire il failover dell'istanza di inoltro e aggiornare l'ultima istanza rimanente del secondo gruppo di disponibilità. Dopo che tutte le altre repliche sono state aggiornate, eseguire il failover della replica primaria globale e quindi aggiornare l'ultima istanza rimanente del primo gruppo di disponibilità. Di seguito è riportato un diagramma dettagliato con questa procedura. 

 Il percorso di aggiornamento e i tempi di inattività per le applicazioni client possono variare a seconda della specifica implementazione in uso.  
  
> [!NOTE]  
>  In molti casi, dopo aver completato l'aggiornamento in sequenza sarà possibile eseguire il failback sulla replica primaria originale. 

### <a name="general-steps-to-upgrade-a-distributed-availability-group"></a>Procedura generica per l'aggiornamento di un gruppo di disponibilità distribuito
1. Eseguire il backup tutti i database, inclusi i database di sistema e quelli che fanno parte del gruppo di disponibilità. 
2. Eseguire l'aggiornamento e riavviare tutte le repliche secondarie del secondo gruppo di disponibilità (downstream). 
3. Eseguire l'aggiornamento e riavviare tutte le repliche secondarie del primo gruppo di disponibilità (upstream). 
4. Eseguire il failover dell'istanza di inoltro primaria a una replica secondaria aggiornata del gruppo di disponibilità secondario.
5. Attendere la sincronizzazione dei dati. I database devono risultare sincronizzati in tutte le repliche con commit sincrono e l'istanza primaria globale deve essere sincronizzata con l'istanza di inoltro.  
6. Eseguire l'aggiornamento e riavviare l'ultima istanza rimanente del gruppo di disponibilità secondario. 
7. Effettuare il failover dell'istanza primaria globale a un'istanza secondaria aggiornata del primo gruppo di disponibilità.  
8. Aggiornare l'ultima istanza rimanente del gruppo di disponibilità primario.
9. Riavviare il server appena aggiornato. 
10. (Facoltativo) Eseguire il failback di entrambi i gruppi di disponibilità alle repliche primarie originali.  

>[!IMPORTANT]
>- Verificare sempre il completamento della sincronizzazione tra i singoli passaggi. Prima di procedere con il passaggio seguente, verificare che all'interno del gruppo di disponibilità le repliche con commit sincrono siano sincronizzate e che l'istanza primaria globale sia sincronizzata con l'istanza di inoltro nel gruppo di disponibilità distribuito. 
>- **Raccomandazione**: ogni volta che si verifica la sincronizzazione, aggiornare sia il nodo del database sia il nodo del gruppo di disponibilità distribuito in SQL Server Management Studio. Dopo il completamento della sincronizzazione, salvare uno screenshot degli stati di ogni replica. In tal modo è possibile tenere traccia della fase in corso, garantire che tutto funzioni correttamente prima del passaggio successivo e facilitare la risoluzione dei problemi in caso di errori. 


### <a name="diagram-example-for-a-rolling-upgrade-of-a-distributed-availability-group"></a>Diagramma di esempio per l'aggiornamento in sequenza di un gruppo di disponibilità distribuito

| gruppo di disponibilità | Replica primaria | Replica secondaria|
| :------ | :----------------------------- |  :------ |
| AG1 | NODE1\SQLAG | NODE2\SQLAG|
| AG2 | NODE3\SQLAG | NODE4\SQLAG|
| DISTRIBUTEDAG| AG1 (globale) | AG2 (istanza di inoltro) |
| &nbsp; | &nbsp; | &nbsp; |

![Diagramma di esempio per il gruppo di disponibilità distribuito](media/upgrading-always-on-availability-group-replica-instances/rolling-upgrade-dag-diagram.png)


Procedura per aggiornare le istanze in questo diagramma: 

1. Eseguire il backup tutti i database, inclusi i database di sistema e quelli che fanno parte del gruppo di disponibilità. 
2. Aggiornare NODE4\SQLAG (replica secondaria di AG2) e riavviare il server. 
3. Aggiornare NODE2\SQLAG (replica secondaria di AG1) e riavviare il server. 
4. Eseguire il failover di AG2 da NODE3\SQLAG a NODE4\SQLAG. 
5. Aggiornare NODE3\SQLAG e riavviare il server. 
6. Eseguire il failover di AG1 da NODE1\SQLAG to NODE2\SQLAG. 
7. Aggiornare NODE1\SQLAG e riavviare il server. 
8. (Facoltativo) Eseguire il failback alle repliche primarie originali.
    1. Eseguire il failback di AG2 da NODE4\SQLAG a NODE3\SQLAG.  
    2. Eseguire il failback di AG1 da NODE2\SQLAG a NODE1\SQLAG. 

Se in ogni gruppo di disponibilità esisteva una terza replica, questa verrà aggiornata prima di NODE3\SQLAG e NODE1\SQLAG. 

>[!IMPORTANT]
>- Verificare sempre il completamento della sincronizzazione tra i singoli passaggi. Prima di procedere con il passaggio seguente, verificare che all'interno del gruppo di disponibilità le repliche con commit sincrono siano sincronizzate e che l'istanza primaria globale sia sincronizzata con l'istanza di inoltro nel gruppo di disponibilità distribuito. 
>- Raccomandazione: ogni volta che si verifica la sincronizzazione, aggiornare sia il nodo del database sia il nodo del gruppo di disponibilità distribuito in SQL Server Management Studio. Dopo il completamento della sincronizzazione, salvare uno screenshot dello stato complessivo. In tal modo è possibile tenere traccia della fase in corso, garantire che tutto funzioni correttamente prima del passaggio successivo e facilitare la risoluzione dei problemi in caso di errori. 


## <a name="special-steps-for-change-data-capture-or-replication"></a>Passaggi speciali per Change Data Capture o la replica

A seconda dell'aggiornamento che si sta applicando, possono essere necessari passaggi aggiuntivi per i database di replica del gruppo di disponibilità abilitati per Change Data Capture o la replica. Fare riferimento alle note sulla versione relative all'aggiornamento per stabilire se i passaggi seguenti sono necessari:

1. Aggiornare ogni replica secondaria.

1. Al termine dell'aggiornamento di tutte le repliche secondarie, eseguire il failover del gruppo di disponibilità su un'istanza aggiornata. 

1. Eseguire l'istruzione Transact-SQL seguente sull'istanza che ospita la replica primaria:

   ```sql
   EXECUTE [master].[sys].[sp_vupgrade_replication];
   ```

   >[!NOTE]
   >L'esecuzione di questo comando potrebbe richiedere alcuni minuti. 

1. Aggiornare l'istanza che era in origine la replica primaria.

Per informazioni di carattere generale, vedere l'articolo sulla [possibilità che la funzionalità CDC non funzioni dopo che è stato applicato l'aggiornamento cumulativo più recente](/archive/blogs/sql_server_team/cdc-functionality-may-break-after-upgrading-to-the-latest-cu-for-sql-server-2012-2014-and-2016).

  
## <a name="see-also"></a>Vedere anche  
 [Eseguire l'aggiornamento a SQL Server 2016 usando l'Installazione guidata &#40;programma di installazione&#41;](../../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)   

 [Installazione di SQL Server 2016 dal prompt dei comandi](../../install-windows/install-sql-server-from-the-command-prompt.md)  
  
