---
title: Aggiornare i database replicati o applicare patch | Microsoft Docs
description: SQL Server supporta l'aggiornamento di database replicati da versioni precedenti di SQL Server senza interrompere l'attività in altri nodi.
ms.custom: ''
ms.date: 07/24/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- merge replication database upgrades [SQL Server replication]
- replication [SQL Server], upgrading
- transactional replication, upgrading databases
- snapshot replication [SQL Server], upgrading databases
- upgrading replicated databases
ms.assetid: 9926a4f7-bcd8-4b9b-9dcf-5426a5857116
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 72b1d22d14cf83d44743d2081ee051bd02faf342
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351018"
---
# <a name="upgrade-or-patch-replicated-databases"></a>Aggiornare i database replicati o applicare patch

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
  [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)] supporta l'aggiornamento di database replicati da versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] senza che sia necessario, durante l'aggiornamento di un nodo, arrestare le attività negli altri nodi. Verificare che vengano osservate le regole relative alle versioni supportate in una topologia:  
  
-   La versione del server di distribuzione è indifferente, purché superiore o uguale alla versione del server di pubblicazione (in molti casi l'istanza del server di distribuzione è la stessa del server di pubblicazione).    
-   La versione del server di pubblicazione è indifferente, purché inferiore o uguale alla versione del server di distribuzione.    
-   La versione del Sottoscrittore dipende dal tipo di pubblicazione:    
    - La versione di un Sottoscrittore per una pubblicazione transazionale può essere una versione qualsiasi all'interno di un intervallo di due versioni, in base alla versione del server di pubblicazione. Ad esempio: un server di pubblicazione SQL Server 2012 (11.x) può avere sottoscrittori SQL Server 2014 (12.x) e SQL Server 2016 (13.x). Un server di pubblicazione SQL Server 2016 (13.x) può avere sottoscrittori SQL Server 2014 (12.x) e SQL Server 2012 (11.x).     
    - Un sottoscrittore per una pubblicazione di tipo merge può avere una versione inferiore o uguale alla versione del server di pubblicazione supportata nel ciclo di supporto del ciclo di vita delle versioni.  
 
Il percorso di aggiornamento a SQL Server cambia a seconda del modello di distribuzione. SQL Server offre due percorsi di aggiornamento generali:
- Affiancato: distribuisce un ambiente parallelo e sposta nel nuovo ambiente i database insieme agli oggetti associati a livello di istanza, ad esempio gli account di accesso, i processi e così via. 
- Aggiornamento sul posto: consente al supporto di installazione di SQL Server di aggiornare l'installazione di SQL Server esistente sostituendo i bit di SQL Server e aggiornando gli oggetti di database. Per ridurre al minimo i tempi di inattività, negli ambienti che eseguono i gruppi di disponibilità AlwaysOn o le istanze del cluster di failover l'aggiornamento sul posto è combinato a un [aggiornamento in sequenza](choose-a-database-engine-upgrade-method.md#rolling-upgrade). 

Un approccio comune che è stato adottato per gli aggiornamenti affiancati delle topologie di replica consiste nello spostare nel nuovo ambiente affiancato solo le coppie di server di pubblicazione/sottoscrittore invece dell'intera topologia. Questo approccio graduale consente di controllare i tempi di inattività e di ridurre, in una certa misura, l'impatto sull'azienda che dipende dalla replica.  

La maggior parte di questo articolo riguarda l'aggiornamento della versione di SQL Server. Il processo di aggiornamento sul posto, tuttavia, deve essere usato anche per l'applicazione di patch a SQL Server con un Service Pack o un aggiornamento cumulativo. 

 >[!WARNING]
 > L'aggiornamento di una topologia di replica è un processo che si compone di più passaggi. È consigliabile provare ad aggiornare una replica della topologia di replica in un ambiente di test prima di eseguire l'aggiornamento nell'ambiente di produzione reale. Ciò consentirà di procurarsi le conoscenze operativa necessarie per gestire senza problemi l'aggiornamento evitando di incorrere in lunghi e costosi tempi di inattività durante il processo di aggiornamento. Alcuni clienti sono riusciti a ridurre i tempi di inattività in modo significativo usando i gruppi di disponibilità AlwaysOn e/o le istanze del cluster di failover di SQL Server nei propri ambienti di produzione durante l'aggiornamento della topologia di replica. Inoltre, prima di tentare l'aggiornamento, è consigliabile eseguire un backup di tutti i database, tra cui i database di distribuzione, MSDB, master, i database utente che partecipano alla replica.


## <a name="replication-matrix"></a>Matrice di replica

[!INCLUDE[repl matrix](../../includes/replication-compat-matrix.md)]
  
## <a name="run-the-log-reader-agent-for-transactional-replication-before-upgrade"></a>Esecuzione dell'agente di lettura log per la replica transazionale prima dell'aggiornamento  
 Prima di eseguire l'aggiornamento di [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)], è necessario assicurarsi che tutte le transazioni completate dalle tabelle pubblicate siano state elaborate dall'agente di lettura log. Per assicurarsi che tutte le transazioni siano state elaborate, eseguire i passaggi seguenti per ogni database che contiene pubblicazioni transazionali:  
  
1.  Verificare che l'agente di lettura log sia in esecuzione per il database. Per impostazione predefinita, l'agente viene eseguito in modo continuativo.    
2.  Arrestare l'attività dell'utente nelle tabelle pubblicate.  
3.  Concedere tempo all'agente di lettura log per copiare transazioni nel database di distribuzione, quindi arrestare l'agente.  
4.  Eseguire [sp_replcmds](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md) per verificare che siano state elaborate tutte le transazioni. Il set di risultati restituito da questa procedura deve essere vuoto.   
5.  Eseguire [sp_replflush](../../relational-databases/system-stored-procedures/sp-replflush-transact-sql.md) per chiudere la connessione da sp_replcmds.   
6.  Eseguire l'aggiornamento del server alla versione più recente di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].   
7.  Riavviare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent e l'agente di lettura log se non vengono avviati automaticamente dopo l'aggiornamento.  
  
## <a name="run-agents-for-merge-replication-after-upgrade"></a>Esecuzione degli agenti per una replica di tipo merge dopo l'aggiornamento  
 Al termine dell'aggiornamento, eseguire l'agente snapshot per ogni pubblicazione di tipo merge e l'agente di merge per ogni sottoscrizione in modo da aggiornare i metadati della replica. Non occorre applicare il nuovo snapshot in quanto non è necessario reinizializzare le sottoscrizioni. I metadati delle sottoscrizioni vengono aggiornati alla prima esecuzione dell'agente di merge successiva all'aggiornamento. Ciò significa che il database di sottoscrizione può rimanere online e attivo durante l'aggiornamento del server di pubblicazione.  
  
 La replica di tipo merge archivia i metadati delle pubblicazioni e delle sottoscrizioni in alcune tabelle di sistema nei database di pubblicazione e sottoscrizione. L'esecuzione dell'agente snapshot aggiorna i metadati delle pubblicazioni e l'esecuzione dell'agente di merge aggiorna i metadati delle sottoscrizioni. È semplicemente richiesta la generazione di uno snapshot di pubblicazione. Se una pubblicazione di tipo merge utilizza filtri con parametri, ogni partizione includerà uno snapshot. Non è necessario aggiornare gli snapshot partizionati.  
  
 Eseguire gli agenti da [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], da Monitoraggio replica o dalla riga di comando. Per altre informazioni sull'esecuzione dell'agente di snapshot, vedere gli articoli seguenti:  
  
-   [Creazione e applicazione dello snapshot iniziale](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)
-   [Avviare e arrestare un agente di replica &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md)
-   [Creazione e applicazione dello snapshot iniziale](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)
-   [Replication Agent Executables Concepts](../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)  
  

Per altre informazioni sull'esecuzione dell'agente di merge, vedere gli articoli seguenti:
-   [Sincronizzazione di una sottoscrizione pull](../../relational-databases/replication/synchronize-a-pull-subscription.md)
-   [Sincronizzazione di una sottoscrizione push](../../relational-databases/replication/synchronize-a-push-subscription.md)  
  

Al termine dell'aggiornamento di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in una topologia in cui viene utilizzata una replica di tipo merge, modificare il livello di compatibilità di tutte le pubblicazioni se si desidera utilizzare le nuove funzionalità.  
  
## <a name="upgrading-to-standard-workgroup-or-express-editions"></a>Aggiornamento alle versioni Standard, Workgroup o Express  
Prima di eseguire l'aggiornamento da un'edizione all'altra di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] , verificare che le funzionalità utilizzate siano supportate nell'edizione a cui si esegue l'aggiornamento. Per altre informazioni, vedere la sezione sulla replica in [Edizioni e funzionalità supportate di SQL Server](../../sql-server/editions-and-components-of-sql-server-2017.md).  

## <a name="steps-to-upgrade-a-replication-topology"></a>Procedura di aggiornamento della topologia di replica
Questi passaggi illustrano l'ordine in cui i server di una topologia di replica devono essere aggiornati. La stessa procedura si applica alle repliche transazionali e di tipo merge. Tuttavia, questi passaggi non coprono la replica peer-to-peer, le sottoscrizioni ad aggiornamento in coda, né le sottoscrizioni ad aggiornamento immediato. 

### <a name="in-place-upgrade"></a>Aggiornamento sul posto 
1. Aggiornare il database di distribuzione. 
2. Aggiornare il server di pubblicazione e il sottoscrittore. Questi componenti possono essere aggiornati in qualsiasi ordine. 

 >[!NOTE]
 > Per SQL 2008 e 2008 R2, l'aggiornamento del server di pubblicazione e del sottoscrittore deve essere contestuale per permettere l'allineamento con la matrice della topologia di replica. Un server di pubblicazione o sottoscrittore SQL 2008 o 2008 R2 non può avere un server di pubblicazione né un sottoscrittore SQL 2016 o versione successiva. Se l'aggiornamento contestuale non è possibile, usare un aggiornamento intermedio per portare le istanze di SQL a SQL 2014 e quindi ripetere l'aggiornamento per portarle a SQL 2016 o versione successiva.  

### <a name="side-by-side-upgrade"></a>Aggiornamento affiancato
1. Aggiornare il database di distribuzione.
1. Riconfigurare la [distribuzione](../../relational-databases/replication/configure-distribution.md) nella nuova istanza di SQL Server.
1. Aggiornare il server di pubblicazione.
1. Aggiornare il sottoscrittore.
1. Riconfigurare tutte le coppie di server di pubblicazione/sottoscrittore, compresa la reinizializzazione del sottoscrittore. 


## <a name="steps-for-side-by-side-migration-of-the-distributor-to-windows-server-2012-r2"></a>Procedura per la migrazione affiancata del server di distribuzione a Windows Server 2012 R2
Se si prevede di aggiornare l'istanza di SQL Server a SQL Server 2016 o versione successiva e il sistema operativo corrente è Windows 2008 o 2008 R2, sarà necessario eseguire un aggiornamento affiancato del sistema operativo per portarlo a Windows Server R2 o versione successiva. Il motivo di questo aggiornamento intermedio del sistema operativo è che non è possibile installare SQL Server 2016 in un computer Windows Server 2008 o 2008 R2 e che Windows Server 2008 o 2008 R2 non supporta gli aggiornamenti sul posto direttamente in Windows Server 2016. Sebbene sia possibile eseguire un aggiornamento sul posto da Windows Server 2008 o 2008 R2 a Windows Server 2012 e quindi a Windows Server 2016, questa operazione non è in genere consigliata a causa del tempo di inattività e della complessità che impedisce un percorso di rollback semplice. Un aggiornamento affiancato è l'unico percorso di aggiornamento disponibile per le istanze SQL Server che fanno parte di un cluster di failover.  Eseguire i passaggi seguenti in un'istanza di SQL Server autonoma o in un'istanza del cluster di failover AlwaysOn.

1. Configurare una nuova edizione e versione dell'istanza di SQL Server, autonoma o del cluster di failover AlwaysOn, come server di distribuzione in Windows Server 2012 R2 o 2016 specificando un nome diverso per il cluster Windows e per l'istanza del cluster di failover di SQL Server o per l'host autonomo. Mantenere la struttura di directory identica a quella del vecchio server di distribuzione per garantire che i file eseguibili degli agenti di replica, le cartelle di replica e i percorsi file del database si trovino nello stesso percorso nel nuovo ambiente. Ciò riduce la necessità di eseguire interventi successivi alla migrazione o all'aggiornamento.
1. Verificare che la replica sia sincronizzata, quindi arrestare tutti gli agenti di replica. 
1. Arrestare l'istanza del server di distribuzione di SQL Server corrente. Se si tratta di un'istanza autonoma, arrestare il server. Se si tratta di un'istanza del cluster di failover di SQL, portare offline l'intero ruolo di SQL Server in Gestione cluster, incluso il nome di rete. 
1. Rimuovere le voci di oggetto computer AD e DNS per i vecchio ambiente (istanza del server di distribuzione corrente). 
1. Modificare il nome host del nuovo server in modo che corrisponda a quelle del vecchio server.
    1. Se si tratta di un'istanza del cluster di failover di SQL, assegnare alla nuova istanza del cluster di failover di SQL lo stesso nome di server virtuale dell'istanza precedente. 
1. Copiare i file di database dall'istanza precedente usando il reindirizzamento, la copia di archiviazione o la copia di file SAN. 
1. Portare online la nuova istanza di SQL Server. 
1. Riavviare tutti gli agenti di replica e verificare che vengano eseguiti correttamente.
1. Verificare che la replica funzioni come previsto. 
1. Usare i supporti di installazione di SQL Server per eseguire un aggiornamento sul posto dell'istanza di SQL Server e portarla alla nuova versione di SQL Server. 


  >[!NOTE]
  > Per ridurre i tempi di inattività, si consiglia di eseguire la *migrazione affiancata* del server di distribuzione e *l'aggiornamento sul posto a SQL Server 2016* come attività separate. Questo consente di adottare un approccio graduale, ridurre i rischi e contenere i tempi di inattività.

## <a name="web-synchronization-for-merge-replication"></a>Sincronizzazione Web per la replica di tipo merge  
 L'opzione di sincronizzazione Web per la replica di tipo merge richiede che il listener per la replica di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (replisapi.dll) venga copiato nella directory virtuale nel server Internet Information Services (IIS) utilizzato per la sincronizzazione. Quando si configura la sincronizzazione Web, il file viene copiato nella directory virtuale dalla procedura di configurazione guidata della sincronizzazione Web. Se si aggiornano i componenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installati nel server IIS, è necessario copiare manualmente replisapi.dll dalla directory COM alla directory virtuale nel server IIS. Per altre informazioni sulla configurazione della sincronizzazione Web, vedere [Configurazione della sincronizzazione Web](../../relational-databases/replication/configure-web-synchronization.md).  
  
## <a name="restoring-a-replicated-database-from-an-earlier-version"></a>Ripristino di un database replicato da una versione precedente  
 Per verificare che in seguito al ripristino del backup di un database replicato vengano mantenute le impostazioni di replica di una versione precedente, eseguire il ripristino in un server e un database con gli stessi nomi del server e del database utilizzati per la creazione della copia di backup.  
  
## <a name="see-also"></a>Vedere anche  
 [Replica di SQL Server](../../relational-databases/replication/sql-server-replication.md)  
 [Domande frequenti sull'amministrazione della replica](../../relational-databases/replication/administration/frequently-asked-questions-for-replication-administrators.md)   
 [Compatibilità con le versioni precedenti della replica](../../relational-databases/replication/replication-backward-compatibility.md)   
 [Aggiornamenti di versione ed edizione supportati](../../database-engine/install-windows/supported-version-and-edition-upgrades.md)   
 [Eseguire l'aggiornamento di SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)  
 [Upgrading a Replication Topology to SQL Server 2016](/archive/blogs/sql_server_team/upgrading-a-replication-topology-to-sql-server-2016) (Aggiornamento di una topologia di replica a SQL Server 2016)