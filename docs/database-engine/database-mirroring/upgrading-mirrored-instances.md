---
title: Aggiornamento di istanze con mirroring | Microsoft Docs
description: Informazioni su come ridurre i tempi di inattività durante l'aggiornamento di un'istanza con mirroring di SQL Server tramite un aggiornamento in sequenza. Questo articolo include le procedure consigliate.
ms.custom: ''
ms.date: 02/01/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- upgrading SQL Server, rolling upgrade of mirrored databases
- database mirroring [SQL Server], upgrading system
- rolling upgrades [SQL Server]
ms.assetid: 0e73bd23-497d-42f1-9e81-8d5314bcd597
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 36d92a6dcbe8fa1bb2af06a042a281ef5648a5df
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641936"
---
# <a name="upgrading-mirrored-instances"></a>Aggiornamento di istanze con mirroring
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Quando si aggiorna un'istanza con mirroring di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a una nuova versione di [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] , a un nuovo Service Pack o aggiornamento cumulativo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]oppure a un nuovo Service Pack o aggiornamento cumulativo di Windows, è possibile ridurre i tempi di inattività per ogni database con mirroring a un singolo failover manuale eseguendo un aggiornamento in sequenza o due failover manuali in caso di failback all'istanza primaria originale. L'aggiornamento in sequenza è un processo in più fasi che, nella forma più semplice, implica l'aggiornamento dell'istanza di [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] che in quel momento funge da server mirror in una sessione di mirroring, seguito dal failover manuale del database con mirroring, dall'aggiornamento della prima istanza principale di [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] e dalla ripresa del mirroring. Nella pratica, il processo esatto dipende dalla modalità operativa, nonché dal numero e dal layout della sessione di mirroring in esecuzione nelle istanze di [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] da aggiornare.  
  
> [!NOTE]  
>  Per informazioni sull'uso del mirroring del database con log shipping durante una migrazione, scaricare il [white paper Database Mirroring and Log Shipping](https://t.co/RmO6ruCT4J)(Mirroring del database e log shipping).  
  
## <a name="prerequisites"></a>Prerequisiti  
 Prima di iniziare, esaminare le informazioni seguenti:  
  
-   [Aggiornamenti di versione ed edizione supportati](../../database-engine/install-windows/supported-version-and-edition-upgrades.md): verificare che sia possibile eseguire l'aggiornamento a SQL Server 2016 dalla versione del sistema operativo Windows e di SQL Server in uso. Ad esempio, non è possibile eseguire l'aggiornamento diretto da un'istanza di SQL Server 2005 a [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
-   [Scegliere un metodo di aggiornamento del motore di database](../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md): selezionare il metodo e la procedura di aggiornamento appropriati in base alla verifica degli aggiornamenti della versione e dell'edizione supportate e anche agli altri componenti installati nell'ambiente interessato per aggiornare i componenti nell'ordine corretto.  
  
-   [Pianificare e testare il piano di aggiornamento del motore di database](../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md): esaminare le note sulla versione, i problemi di aggiornamento noti, l'elenco di controllo pre-aggiornamento e sviluppare e testare il piano di aggiornamento.  
  
-   [Requisiti hardware e software per l'installazione di SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md):  esaminare i requisiti software per l'installazione di [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Se è necessario software aggiuntivo, installarlo in ogni nodo prima di iniziare il processo di aggiornamento per ridurre al minimo eventuali tempi di inattività.  
  
## <a name="recommended-preparation-best-practices"></a>Preparazione consigliata (procedure consigliate)  
 Prima di avviare un aggiornamento in sequenza, si consiglia di effettuare le operazioni seguenti:  
  
1.  Eseguire un failover manuale di prova su almeno una delle sessioni di mirroring:  
  
    -   [Failover manuale di una sessione di mirroring del database &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/manually-fail-over-a-database-mirroring-session-sql-server-management-studio.md)  
  
    -   [Failover manuale in una sessione di mirroring del database &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/manually-fail-over-a-database-mirroring-session-transact-sql.md).  
  
    > [!NOTE]  
    >  Per informazioni sul funzionamento del failover manuale, vedere [Cambio di ruolo durante una sessione di mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md).  
  
2.  Proteggere i dati:  
  
    1.  Eseguire un backup completo di ogni database principale:  
  
         [Creazione di un backup completo del database &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md).  
  
    2.  Eseguire il comando [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) in ogni database principale.  
  
## <a name="stages-of-a-rolling-upgrade"></a>Fasi di un aggiornamento in sequenza  
 I passaggi specifici di un aggiornamento in sequenza dipendono dalla modalità operativa della configurazione del mirroring. Tuttavia, le fasi di base sono identiche.  
  
> [!NOTE]  
>  Per altre informazioni sulle modalità di funzionamento, vedere [Modalità di funzionamento del mirroring del database](../../database-engine/database-mirroring/database-mirroring-operating-modes.md).  
  
 Nel diagramma di flusso della figura seguente vengono illustrate le fasi di base di un aggiornamento in sequenza per ogni modalità operativa. Le procedure corrispondenti vengono descritte dopo l'illustrazione.  
  
 ![Diagramma di flusso che illustra la procedura di un aggiornamento in sequenza](../../database-engine/database-mirroring/media/dbm-rolling-upgrade.gif "Diagramma di flusso che illustra la procedura di un aggiornamento in sequenza")  
  
> [!IMPORTANT]  
>  Un'istanza del server potrebbe ricoprire ruoli di mirroring diversi (server principale, server mirror o server di controllo del mirroring) in sessioni di mirroring simultanee. In questo caso, è necessario adattare di conseguenza il processo di aggiornamento in sequenza di base. Per altre informazioni, vedere [Cambio di ruolo durante una sessione di mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)(Mirroring del database e log shipping).  
  
> [!NOTE]  
>  In molti casi, dopo aver completato l'aggiornamento in sequenza, sarà possibile eseguire il failback al server principale originale.  
  
### <a name="to-change-a-session-from-high-performance-mode-to-high-safety-mode"></a>Per modificare la modalità di una sessione dalla modalità a elevate prestazioni alla modalità a protezione elevata  
  
1.  Se una sessione di mirroring viene eseguita in modalità a elevate prestazioni, prima di eseguire un aggiornamento in sequenza impostare la modalità operativa su protezione elevata senza failover automatico.  
  
    > [!IMPORTANT]  
    >  Se il server mirror è geograficamente distante dal server principale, un aggiornamento in sequenza potrebbe non essere appropriato.  
  
    -   In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]: modificare l'opzione **Modalità operativa** e impostarla su **Protezione elevata senza failover automatico (sincrona)** usando la [pagina Mirroring](../../relational-databases/databases/database-properties-mirroring-page.md) della finestra di dialogo **Proprietà database**. Per informazioni su come accedere a questa pagina, vedere [Avvio della Configurazione guidata sicurezza mirroring del database &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md).  
  
    -   In [!INCLUDE[tsql](../../includes/tsql-md.md)]: impostare la sicurezza della transazione su FULL. Per altre informazioni, vedere [Modifica della protezione delle transazioni in una sessione di mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/change-transaction-safety-in-a-database-mirroring-session-transact-sql.md).  
  
### <a name="to-remove-a-witness-from-a-session"></a>Per rimuovere un server di controllo del mirroring da una sessione  
  
1.  Se una sessione di mirroring prevede un server di controllo del mirroring, si consiglia di rimuoverlo prima di eseguire un aggiornamento in sequenza. In caso contrario, quando l'istanza del server mirror viene aggiornata, la disponibilità del database dipende dal server di controllo del mirroring che rimane connesso all'istanza del server principale. Dopo avere rimosso un server di controllo del mirroring, è possibile aggiornarlo in qualsiasi momento durante il processo di aggiornamento in sequenza senza rischiare alcun tempo di inattività del database.  
  
    > [!NOTE]  
    >  Per altre informazioni, vedere [Quorum: Impatto di un server di controllo del mirroring sulla disponibilità del database &#40;mirroring del database&#41;](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md).  
  
    -   [Rimuovere il server di controllo del mirroring da una sessione di mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/remove-the-witness-from-a-database-mirroring-session-sql-server.md)  
  
### <a name="to-perform-the-rolling-upgrade"></a>Per eseguire l'aggiornamento in sequenza  
  
1.  Per ridurre al minimo i tempi di inattività, si consiglia di: Avviare l'aggiornamento in sequenza aggiornando qualsiasi server partner di mirroring che è attualmente il server mirror in tutte le relative sessioni di mirroring. In questa fase potrebbe essere necessario aggiornare più istanze del server.  
  
    > [!NOTE]  
    >  Un server di controllo del mirroring può essere aggiornato in qualsiasi punto nel processo di aggiornamento in sequenza. Ad esempio, se un'istanza del server è un server mirror nella sessione 1 e un server di controllo del mirroring nella sessione 2, è possibile aggiornare l'istanza del server.  
  
     L'istanza del server da aggiornare per prima dipende dalla configurazione corrente delle sessioni di mirroring, ovvero:  
  
    -   Se un'istanza server è già il server mirror in tutte le sue sessioni di mirroring, aggiornare l'istanza server alla nuova versione.  
  
    -   Se tutte le istanze server sono attualmente il server principale in tutte le sessioni di mirroring, selezionare un'istanza server da aggiornare per prima. Quindi, eseguire manualmente il failover su ciascuno dei database principali e aggiornare tale istanza server.  
  
     Dopo aver eseguito l'aggiornamento, un'istanza del server reintegra automaticamente ciascuna delle sue sessioni di mirroring.  
  
2.  Attendere la sincronizzazione di ciascuna sessione di mirroring la cui istanza del server mirror è stata appena aggiornata. Quindi, connettersi all'istanza del server principale ed eseguire manualmente il failover della sessione. Con il failover, l'istanza del server aggiornata diventa il server principale per la sessione e il server principale precedente diventa il server mirror.  
  
     L'obiettivo di questo passaggio è consentire a un'altra istanza del server di diventare il server mirror in tutte le sessione di mirroring in cui è un server partner.  
  
     **Restrizioni dopo l'esecuzione del failover in un'istanza server aggiornata.**  
  
     Dopo il failover da un'istanza del server precedente a un'istanza del server [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] , la sessione del database viene sospesa. Non può essere ripristinata finché l'altro server partner non sarà stato aggiornato. Tuttavia, il server principale continua ad accettare connessioni e a consentire l'accesso ai dati e le modifiche sul database principale.  
  
    > [!NOTE]  
    >  L'avvio di una nuova sessione di mirroring richiede che tutte le istanze server eseguano la stessa versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
3.  Dopo aver eseguito il failover, è consigliabile eseguire il comando [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) sul database principale.  
  
4.  Aggiornare ciascuna istanza server che ora è il server mirror in tutte le sessioni di mirroring nelle quali è un server partner. In questa fase potrebbe essere necessario aggiornare più server.  
  
    > [!IMPORTANT]  
    >  In una configurazione di mirroring complessa, alcune istanze del server potrebbero essere ancora il server principale originale in una o più sessioni di mirroring. Ripetere i passaggi da 2 a 4 per tali istanze del server finché tutte le istanze coinvolte non sono state aggiornate.  
  
5.  Riprendere la sessione di mirroring.  
  
    > [!NOTE]  
    >  Il failover automatico funziona solo dopo che il server di controllo è stato aggiornato e aggiunto nuovamente nella sessione di mirroring.  
  
6.  Aggiornare le istanze server rimanenti che sono il server di controllo del mirroring in tutte le sue sessioni di mirroring. Dopo che un server di controllo del mirroring aggiornato integra una sessione di mirroring, il failover automatico diventa nuovamente possibile. In questa fase potrebbe essere necessario aggiornare più server.  
  
### <a name="to-return-a-session-to-high-performance-mode"></a>Per ripristinare una sessione alla modalità a elevate prestazioni  
  
1.  Facoltativamente, tornare alla modalità a elevate prestazioni utilizzando uno dei metodi seguenti:  
  
    -   In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]: modificare l'opzione **Modalità operativa** e impostarla su **Prestazioni elevate (asincrona)** usando la [pagina Mirroring](../../relational-databases/databases/database-properties-mirroring-page.md) della finestra di dialogo **Proprietà database**.  
  
    -   In [!INCLUDE[tsql](../../includes/tsql-md.md)]: usare [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md) per impostare la protezione della transazione su OFF.  
  
### <a name="to-add-a-witness-back-into-a-mirroring-session"></a>Per aggiungere un server di controllo nuovamente in una sessione di mirroring  
  
1.  Facoltativamente, nella modalità a protezione elevata, ristabilire il server di controllo del mirroring su ciascuna sessione di mirroring.  
  
     **Per integrare un server di controllo del mirroring**  
  
    -   [Aggiunta o sostituzione di un server di controllo del mirroring del database &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/add-or-replace-a-database-mirroring-witness-sql-server-management-studio.md)  
  
    -   [Aggiungere un server di controllo del mirroring del database tramite l'autenticazione di Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Eseguire l'aggiornamento a SQL Server 2016 usando l'Installazione guidata &#40;programma di installazione&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)   
 [Installare SQL Server 2016 dal prompt dei comandi](../install-windows/install-sql-server-from-the-command-prompt.md)   
 [Mirroring del database di ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [Visualizzazione dello stato di un database con mirroring &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/view-the-state-of-a-mirrored-database-sql-server-management-studio.md)   
 [Mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Cambio di ruolo durante una sessione di mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)   
 [Utilizzo forzato del servizio in una sessione di mirroring del database &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/force-service-in-a-database-mirroring-session-transact-sql.md)   
 [Avviare il monitoraggio mirroring del database &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)   
 [Database Mirroring Operating Modes](../../database-engine/database-mirroring/database-mirroring-operating-modes.md)  
  
