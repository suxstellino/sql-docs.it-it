---
title: Log shipping e replica (SQL Server) | Microsoft Docs
description: Informazioni sul modo in cui il processo di log shipping applica al database secondario il log delle transazioni relativo a ogni operazione di inserimento, aggiornamento o eliminazione eseguita sul database primario.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], log shipping and
- log shipping [SQL Server], replication and
ms.assetid: 132bebfd-0206-4d23-829a-b38e5ed17bc9
author: cawrites
ms.author: chadam
ms.openlocfilehash: cb1a0e9950e272a7c05024f0b12c48d5d6ae0838
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643907"
---
# <a name="log-shipping-and-replication-sql-server"></a>Log shipping e replica (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Il log shipping coinvolge due copie di un unico database che in genere risiedono in computer diversi. In un momento dato solo una copia del database risulta disponibile per i client. Questa copia è nota come database primario. Gli aggiornamenti al database primario apportati dai client vengono propagati attraverso il log shipping all'altra copia del database, nota come database secondario. Il processo di log shipping prevede l'applicazione nel database secondario del log delle transazioni relativo a ogni operazione di inserimento, aggiornamento o eliminazione eseguita sul database primario.  
  
 Quando il log shipping viene utilizzato in combinazione con la replica, si noti quanto segue:  
  
-   La replica non continua in caso di failover del server del log shipping. Se si verifica il failover, gli agenti di replica non si connettono al server secondario. In questo modo le transazioni non vengono replicate nei Sottoscrittori. Se si verifica il failback nel server primario, la replica viene ripresa. Tutte le transazioni copiate durante il log shipping dal server secondario a quello primario vengono replicate nei Sottoscrittori.  
  
-   Se il database primario viene perso definitivamente, è possibile rinominare il database secondario affinché la replica possa continuare. Nella parte restante dell'argomento vengono descritti i requisiti e le procedure necessari in questo caso. Nell'esempio seguente viene utilizzato il database di pubblicazione, ovvero il database più comune per cui eseguire il log shipping. Le operazioni possono tuttavia essere eseguite nei database di sottoscrizione e di distribuzione.  
  
 Per informazioni sul recupero di database oggetto di replica senza dover riconfigurare la replica, vedere [Backup e ripristino di database replicati](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md).  
  
> [!NOTE]  
>  Per garantire la disponibilità del database di pubblicazione è consigliabile utilizzare il mirroring del database invece del log shipping. Per altre informazioni, vedere [Mirroring e replica del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md).  
  
## <a name="requirements-and-procedures-for-replicating-from-the-secondary-if-the-primary-is-lost"></a>Requisiti e procedure per la replica dal database secondario se quello primario viene perso  
 Tenere presenti i requisiti e le considerazioni seguenti:  
  
-   Se nel server primario sono inclusi più database di pubblicazione, distribuire i log per tutti i database di pubblicazione allo stesso server secondario.  
  
-   Il percorso di installazione dell'istanza del server secondario deve corrispondere a quello dell'istanza del server primario. I percorsi dei database utente nel server secondario devono corrispondere a quelli nel server primario.  
  
-   Eseguire il backup della chiave master del servizio nel server primario. La chiave verrà ripristinata nel server secondario. Per altre informazioni, vedere [BACKUP SERVICE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/backup-service-master-key-transact-sql.md).  
  
-   Il log shipping non costituisce una garanzia completa contro la perdita di dati. Un errore nel database primario può provocare la perdita di dati di cui non è ancora stato eseguito il backup o di copie di backup andate perse quando si è verificato l'errore.  
  
### <a name="log-shipping-with-transactional-replication"></a>Log shipping con replica transazionale  
 Nel caso della replica transazionale, il funzionamento del log shipping dipende dall'opzione **sync with backup** . Questa opzione può essere impostata nel database di pubblicazione e nel database di distribuzione. Nel caso del log shipping per il server di pubblicazione, è importante che l'opzione sia impostata nel database di pubblicazione.  
  
 L'impostazione di questa opzione nel database di pubblicazione garantisce che le transazioni vengano recapitate al database di distribuzione solo dopo che è stato eseguito il backup nel database di pubblicazione. L'ultimo backup del database di pubblicazione può quindi essere ripristinato nel server secondario. In questo modo il database di distribuzione avrà le stesse transazioni del database di pubblicazione ripristinato. Questa opzione garantisce la consistenza tra server di pubblicazione, server di distribuzione e Sottoscrittori in caso di failover del server di pubblicazione in un server secondario. L'impostazione di questa opzione ha effetto sulla latenza e sulla velocità effettiva in quanto le transazioni non possono essere recapitate al database di distribuzione finché non ne viene eseguito il backup nel server di pubblicazione. Se l'applicazione in uso consente questa latenza, è consigliabile impostare l'opzione nel database di pubblicazione. Se l'opzione **sync with backup** non è impostata, i Sottoscrittori potrebbero ricevere modifiche che non sono più incluse nel database recuperato nel server secondario. Per altre informazioni, vedere [Strategie per il backup e il ripristino della replica snapshot e della replica transazionale](../../relational-databases/replication/administration/strategies-for-backing-up-and-restoring-snapshot-and-transactional-replication.md).  
  
 **Per configurare la replica transazionale e il log shipping con l'opzione sync with backup**  
  
1.  Se l'opzione sync with backup non è impostata nel database di pubblicazione, eseguire `sp_replicationdboption '<publicationdatabasename>', 'sync with backup', 'true'`. Per altre informazioni, vedere [sp_replicationdboption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md).  
  
2.  Configurare il log shipping per il database di pubblicazione. Per altre informazioni, vedere [Configurare il log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md).  
  
3.  Se si verifica un errore nel server di pubblicazione, ripristinare l'ultimo log del database nel server secondario mediante l'opzione KEEP_REPLICATION di RESTORE LOG. In questo modo vengono mantenute tutte le impostazioni di replica per il database. Per altre informazioni, vedere [Failover su un database secondario per il log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md) e [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
4.  Ripristinare i database **msdb** e **master** dal server primario al server secondario. Per altre informazioni, vedere [Backup e ripristino di Database di sistema &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md). Se il server primario è anche un server di distribuzione, ripristinare il database di distribuzione dal server primario a quello secondario.  
  
     Le impostazioni e la configurazione di replica di questi database devono essere consistenti con quelle del database di pubblicazione nel server primario.  
  
5.  Nel server secondario rinominare il computer e quindi l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in modo che i nuovi nomi corrispondano al nome del server primario. Per informazioni sulla ridenominazione di un computer, vedere la documentazione di Windows. Per informazioni sulla ridenominazione del server, vedere [Rinominare un computer che ospita un'istanza autonoma di SQL Server](../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md) e [Ridenominare un'istanza del cluster di failover di SQL Server](../../sql-server/failover-clusters/install/rename-a-sql-server-failover-cluster-instance.md).  
  
6.  Nel server secondario ripristinare la chiave master del servizio di cui è stato eseguito il backup dal server primario. Per altre informazioni, vedere [RESTORE SERVICE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-service-master-key-transact-sql.md).  
  
 **Per configurare la replica transazionale e il log shipping senza l'opzione sync with backup**  
  
1.  Configurare il log shipping per il database di pubblicazione. Per altre informazioni, vedere [Configurare il log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md).  
  
2.  Se si verifica un errore nel server di pubblicazione, ripristinare l'ultimo log del database nel server secondario mediante l'opzione KEEP_REPLICATION di RESTORE LOG. In questo modo vengono mantenute tutte le impostazioni di replica per il database. Per altre informazioni, vedere [Failover su un database secondario per il log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md) e [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
3.  Ripristinare i database **msdb** e **master** dal server primario al server secondario. Per altre informazioni, vedere [Backup e ripristino di Database di sistema &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md). Se il server primario è anche un server di distribuzione, ripristinare il database di distribuzione dal server primario a quello secondario.  
  
     Le impostazioni e la configurazione di replica di questi database devono essere consistenti con quelle del database di pubblicazione nel server primario.  
  
4.  Nel server secondario rinominare il computer e quindi l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in modo che i nuovi nomi corrispondano al nome del server primario. Per informazioni sulla ridenominazione di un computer, vedere la documentazione di Windows. Per informazioni sulla ridenominazione del server, vedere [Rinominare un computer che ospita un'istanza autonoma di SQL Server](../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md) e [Ridenominare un'istanza del cluster di failover di SQL Server](../../sql-server/failover-clusters/install/rename-a-sql-server-failover-cluster-instance.md).  
  
     È possibile che venga visualizzato un messaggio di errore dell'agente di lettura log che informa l'utente che il database di pubblicazione e il database di distribuzione non sono sincronizzati.  
  
5.  Nel server secondario ripristinare la chiave master del servizio di cui è stato eseguito il backup dal server primario. Per altre informazioni, vedere [RESTORE SERVICE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-service-master-key-transact-sql.md).  
  
6.  Eseguire **sp_replrestart**. Questa stored procedure consente di forzare l'agente di lettura log in modo che le transazioni già replicate nel log del database di pubblicazione vengano ignorate. Le transazioni applicate dopo il completamento della stored procedure vengono elaborate dall'agente di lettura log. Per altre informazioni, vedere [sp_replrestart &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replrestart-transact-sql.md).  
  
7.  Dopo aver eseguito la stored procedure, riavviare l'agente di lettura log. Per altre informazioni, vedere [Avviare e arrestare un agente di replica &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md).  
  
8.  Le transazioni che sono già state distribuite al Sottoscrittore possono essere applicate al server di pubblicazione. Affinché non si verifichi un errore dell'agente di distribuzione durante il tentativo di riapplicazione di tali transazioni al Sottoscrittore, specificare il profilo agente **Continua in caso di errori di coerenza dei dati**.  
  
### <a name="log-shipping-with-merge-replication"></a>Log shipping con replica di tipo merge  
 Eseguire la procedura seguente per configurare la replica di tipo merge e il log shipping.  
  
 **Per configurare la replica di tipo merge e il log shipping**  
  
1.  Configurare il log shipping per il database di pubblicazione. Per altre informazioni, vedere [Configurare il log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md).  
  
2.  In caso di errore del server di pubblicazione, nel server secondario rinominare il computer e quindi l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in modo che i nuovi nomi corrispondano al nome del server primario. Per informazioni sulla ridenominazione di un computer, vedere la documentazione di Windows. Per informazioni sulla ridenominazione del server, vedere [Rinominare un computer che ospita un'istanza autonoma di SQL Server](../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md) e [Ridenominare un'istanza del cluster di failover di SQL Server](../../sql-server/failover-clusters/install/rename-a-sql-server-failover-cluster-instance.md).  
  
3.  Ripristinare l'ultimo log del database nel server secondario mediante l'opzione KEEP_REPLICATION di RESTORE LOG. In questo modo vengono mantenute tutte le impostazioni di replica per il database. Per altre informazioni, vedere [Failover su un database secondario per il log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md) e [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
4.  Ripristinare i database **msdb** e **master** dal server primario al server secondario. Per altre informazioni, vedere [Backup e ripristino di Database di sistema &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md). Se il server primario è anche un server di distribuzione, ripristinare il database di distribuzione dal server primario a quello secondario.  
  
     Le impostazioni e la configurazione di replica di questi database devono essere consistenti con quelle del database di pubblicazione nel server primario.  
  
5.  Nel server secondario ripristinare la chiave master del servizio di cui è stato eseguito il backup dal server primario. Per altre informazioni, vedere [RESTORE SERVICE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-service-master-key-transact-sql.md).  
  
6.  Sincronizzare il database di pubblicazione con uno o più database di sottoscrizione. In questo modo è possibile caricare le modifiche già apportate nel database di pubblicazione, ma non ancora incluse nella copia di backup ripristinata. I dati che è possibile caricare dipendono dal modo in cui una pubblicazione è filtrata:  
  
    -   Se la pubblicazione non è filtrata, sarà possibile aggiornare il database di pubblicazione eseguendo la sincronizzazione con il Sottoscrittore più aggiornato.  
  
    -   Se la pubblicazione è filtrata, l'aggiornamento del database di pubblicazione potrebbe non essere possibile. Considerare una tabella partizionata in modo che ogni sottoscrizione riceva i dati relativi ai clienti solo per una singola area: Nord, Est, Sud e Ovest. Se per ogni partizione di dati è disponibile almeno un Sottoscrittore, la sincronizzazione con un Sottoscrittore per ogni partizione dovrebbe consentire di aggiornare il database di pubblicazione. Tuttavia, se ad esempio i dati nella partizione Ovest non sono stati replicati in alcun Sottoscrittore, questi dati nel server di pubblicazione non potranno essere aggiornati. In questo caso è consigliabile reinizializzare tutte le sottoscrizioni in modo da garantire la convergenza dei dati nel server di pubblicazione e nei Sottoscrittori. Per altre informazioni, vedere [Reinizializzare le sottoscrizioni](../../relational-databases/replication/reinitialize-subscriptions.md).  
  
     Se si esegue la sincronizzazione con un Sottoscrittore che esegue una versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] precedente a [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], la sottoscrizione non può essere anonima, ma deve essere una sottoscrizione client o una sottoscrizione server (chiamate sottoscrizioni locali e sottoscrizioni globali nelle versioni precedenti del prodotto). Per altre informazioni, vedere [Sincronizzare i dati](../../relational-databases/replication/synchronize-data.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Replica di SQL Server](../../relational-databases/replication/sql-server-replication.md)   
 [Informazioni sul log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Mirroring e replica del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md)  
  
  
