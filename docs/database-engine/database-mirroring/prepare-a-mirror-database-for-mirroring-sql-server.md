---
title: Preparare un database per il mirroring
description: Informazioni su come preparare un database di SQL Server per il mirroring del database usando SQL Server Management Studio o Transact-SQL in SQL Server.
ms.custom: seo-lt-2019
ms.date: 11/10/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], preparing for mirroring
- logins [SQL Server], database mirroring
- mirror database [SQL Server]
ms.assetid: 8676f9d8-c451-419b-b934-786997d46c2b
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 1fa476693286b32cb2910526bb0ea7431faa5120
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642160"
---
# <a name="prepare-a-mirror-database-for-mirroring-sql-server"></a>Preparazione di un database mirror per il mirroring (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Prima di avviare una sessione di mirroring del database, è necessario che il proprietario del database o l'amministratore del sistema verifichi che il database mirror sia stato creato e sia pronto per il mirroring. La creazione di un nuovo database mirror richiede l'esecuzione di un backup completo del database principale e di un backup del log successivo. Entrambi i backup devono quindi essere ripristinati sull'istanza del server mirror tramite WITH NORECOVERY.  
  
 In questo argomento viene descritto come preparare un database mirror in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
-   **Prima di iniziare:**  
  
     [Requisiti](#Requirements)  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Indicazioni](#Recommendations)  
  
     [Sicurezza](#Security)  
  
-   [Per preparare un database mirror esistente per il riavvio del mirroring](#PrepareToRestartMirroring)  
  
-   [Per preparare un nuovo database mirror](#CombinedProcedure)  
  
-   **Completamento:**  [Dopo la preparazione di un database mirror](#FollowUp)  
  
-   [Attività correlate](#RelatedTasks)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="requirements"></a><a name="Requirements"></a> Requisiti  
  
-   Le istanze del server principale e del server mirror devono essere eseguite nella stessa versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Sebbene sia possibile che la versione di SQL Server del server mirror sia successiva, questa configurazione è consigliata solo per processi di aggiornamento accuratamente pianificati. In questo tipo di configurazione si corre il rischio che venga effettuato un failover automatico durante il quale lo spostamento dei dati viene automaticamente sospeso in quanto non è possibile spostare i dati in una versione precedente di SQL Server. Per altre informazioni, vedere [Aggiornamento di istanze con mirroring](../../database-engine/database-mirroring/upgrading-mirrored-instances.md).  
  
-   Le istanze del server principale e del server mirror devono essere eseguite nella stessa edizione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per informazioni sul supporto del mirroring del database in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], vedere [Edizioni e funzionalità supportate di SQL Server 2017](~/sql-server/editions-and-components-of-sql-server-2017.md).  
  
-   Il database deve utilizzare il modello di recupero con registrazione completa.  
  
     Per altre informazioni, vedere [Visualizzare o modificare il modello di recupero di un database &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md) o [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) e [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
-   Il nome del database mirror deve essere identico a quello del database principale.  
  
-   Per poter eseguire correttamente il mirroring, è necessario che lo stato del database mirror sia RESTORING. Quando si prepara un database mirror, è necessario utilizzare l'opzione RESTORE WITH NORECOVERY per tutte le operazioni di ripristino. Per ripristinare un backup completo del database principale, seguito da tutti i backup del log successivi sarà necessaria almeno l'opzione WITH NORECOVERY.  
  
-   Nel sistema in cui si desidera creare il database mirror deve essere disponibile un'unità disco con spazio sufficiente per contenere il database in questione.  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   Non è possibile eseguire il mirroring del database di sistema **master**, **msdb**, **temp** o **model** .  
  
-   Non è possibile eseguire il mirroring di un database appartenente a un [gruppo di disponibilità Always On](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md).  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni  
  
-   Utilizzare un backup completo molto recente o uno differenziale recente del database principale.  
  
-   Se un processo di backup del log è stato pianificato in modo da essere eseguito sul database principale con una frequenza elevata, potrebbe essere necessario disabilitare tale processo fino all'avvio del mirroring.  
  
-   Se possibile, è consigliabile che il percorso del database mirror, inclusa la lettera di unità, sia identico a quello del database principale.  
  
     Se i percorsi dei file sono diversi, ad esempio il database principale è disponibile nell'unità F: e tale unità non è presente nel sistema mirror, è necessario includere l'opzione MOVE nell'istruzione RESTORE.  
  
    > [!IMPORTANT]  
    >  Per aggiungere un file durante una sessione di mirroring senza conseguenze per la sessione, è necessario che il percorso del file esista in entrambi i server. Pertanto, se durante la creazione del database mirror i file del database vengono spostati, potrebbe essere impossibile aggiungere successivamente file al database mirror senza sospendere il mirroring. Per informazioni sulla gestione di un'operazione di creazione file non riuscita, vedere [Risolvere i problemi relativi alla configurazione del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md).  
  
-   Se il database principale ha cataloghi full-text, è consigliabile vedere [Mirroring di database e cataloghi full-text &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-full-text-catalogs-sql-server.md).  
  
-   Nel caso di un database di produzione, eseguire sempre il backup in un dispositivo distinto.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
 La proprietà TRUSTWORTHY è impostata su OFF quando viene eseguito il backup di un database. Di conseguenza, la proprietà TRUSTWORTHY è sempre impostata su OFF in un nuovo database mirror. Se il database deve risultare attendibile dopo un failover, è necessario eseguire passaggi di configurazione aggiuntivi. Per altre informazioni, vedere [Impostare un database mirror per l'uso della proprietà Trustworthy &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md).  
  
 Per informazioni sull'abilitazione della decrittografia automatica della chiave master del database di un database mirror, vedere [Impostazione di un database mirror crittografato](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md).  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Proprietario del database o amministratore di sistema.  
  
##  <a name="to-prepare-an-existing-mirror-database-to-restart-mirroring"></a><a name="PrepareToRestartMirroring"></a> Per preparare un database mirror esistente per il riavvio del mirroring  
 Se il mirroring è stato rimosso e lo stato del database mirror è ancora RECOVERING, è possibile riavviare il mirroring.  
  
1.  Eseguire almeno un backup del log sul database principale. Per altre informazioni, vedere [Backup di un log delle transazioni &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md).  
  
2.  Nel database mirror utilizzare RESTORE WITH NORECOVERY per il ripristino di tutti i backup del log eseguiti sul database principale dopo la rimozione del mirroring. Per altre informazioni, vedere [Ripristinare un backup del log delle transazioni &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md).  
  
##  <a name="to-prepare-a-new-mirror-database"></a><a name="CombinedProcedure"></a> Per preparare un nuovo database mirror  
 **Per preparare un database mirror**  
  
> [!NOTE]  
>  Per un esempio [!INCLUDE[tsql](../../includes/tsql-md.md)] di questa procedura, vedere [Esempio (Transact-SQL)](#TsqlExample)più avanti in questa sezione.  
  
1.  Connettersi all'istanza del server principale.  
  
2.  Creare un backup completo o differenziale del database principale.  
  
    -   [Creazione di un backup completo del database &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
    -   [Creare un backup differenziale del database &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md).  
  
3.  In genere, è necessario eseguire almeno un backup del log sul database principale. È tuttavia possibile evitare il backup del log se il database è stato appena creato e non è ancora stato eseguito alcun backup del log oppure se il modello di recupero è stato appena modificato da SIMPLE a FULL.  
  
    -   [Eseguire il backup di un log delle transazioni &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
4.  Se i backup non sono posizionati in un'unità di rete accessibile da entrambi i sistemi, copiare i backup del database e del log nel sistema in cui verrà ospitata l'istanza del server mirror.  
  
5.  Connettersi all'istanza del server mirror.  
  
6.  Se si utilizza RESTORE WITH NORECOVERY, creare il database mirror ripristinando il backup di database completo e, facoltativamente, il backup di database differenziale più recente, nell'istanza del server mirror.  
  
    > [!NOTE]  
    >  Se si ripristina il database un filegroup alla volta, prestare attenzione a ripristinare l'intero database.  
  
    -   [Ripristinare un backup del database con SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
    -   [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md) e [RESTORE Arguments &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md).  
  
7.  Se si utilizza RESTORE WITH NORECOVERY, applicare tutti i backup del log in sospeso al database mirror.  
  
    -   [Ripristinare un backup del log delle transazioni &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Esempio (Transact-SQL)  
 Prima di iniziare una sessione di mirroring del database, è necessario creare il database mirror. Questa operazione deve essere eseguita poco prima di iniziare la sessione di mirroring.  
  
 Nell'esempio viene utilizzato il database di esempio [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] in cui, per impostazione predefinita, viene utilizzato il modello di recupero con registrazione minima.  
  
1.  Per utilizzare il mirroring con il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] , è necessario modificare il database in modo da utilizzare il modello di recupero con registrazione completa:  
  
    ```  
    USE master;  
    GO  
    ALTER DATABASE AdventureWorks   
    SET RECOVERY FULL;  
    GO  
    ```  
  
2.  Dopo aver modificato il modello di recupero del database da SIMPLE a FULL, creare un backup completo da utilizzare per la creazione del database mirror. Dopo la modifica del modello di recupero, è consigliabile selezionare l'opzione WITH FORMAT per creare un nuovo set di supporti. L'operazione risulta utile per separare i backup eseguiti durante l'utilizzo del modello di recupero con registrazione completa dai backup precedenti eseguiti durante l'utilizzo del modello di recupero con registrazione semplice. Ai fini di questo esempio, il file di backup (`C:\AdventureWorks.bak`) verrà creato nella stessa unità del database.  
  
    > [!NOTE]  
    >  Nel caso di un database di produzione, è consigliabile eseguire sempre il backup in un dispositivo distinto.  
  
     Nell'istanza del server principale, ovvero in `PARTNERHOST1`, creare un backup completo del database principale nel modo seguente:  
  
    ```  
    BACKUP DATABASE AdventureWorks   
        TO DISK = 'C:\AdventureWorks.bak'   
        WITH FORMAT  
    GO  
    ```  
  
3.  Copiare il backup completo nel server mirror.  
  
4.  Se si utilizza RESTORE WITH NORECOVERY, ripristinare il backup completo nell'istanza del server mirror. Il comando di ripristino dipende dal fatto che i percorsi del database mirror e di quello principale siano identici o meno.  
  
    -   **Se i percorsi sono identici:**  
  
         Nell'istanza del server mirror, ovvero in `PARTNERHOST5`, ripristinare il backup completo nel modo seguente:  
  
        ```  
        RESTORE DATABASE AdventureWorks   
            FROM DISK = 'C:\AdventureWorks.bak'   
            WITH NORECOVERY  
        GO  
        ```  
  
    -   **Se i percorsi sono diversi:**  
  
         Se il percorso del database mirror è diverso dal percorso del database principale, ad esempio perché le lettere di unità non corrispondono, per creare il database mirror è necessario che l'operazione di ripristino includa una clausola MOVE.  
  
        > [!IMPORTANT]  
        >  Se il nome di percorso del database principale è diverso dal nome di percorso del database mirror, non è possibile aggiungere un file. Alla ricezione del log relativo all'operazione di aggiunta del file, l'istanza del server mirror tenta infatti di salvare il nuovo file nella posizione utilizzata dal database principale.  
  
         Ad esempio, il comando seguente ripristina un backup di un database principale che si trova in C:\Programmi\Microsoft SQL Server\MSSQL..*n*\MSSQL\Data\ in un percorso diverso, D:\Programmi\Microsoft SQL Server\MSSQL.*n*\MSSQL\Dat`a\`, in cui deve trovarsi il database mirror.  
  
        ```  
        RESTORE DATABASE AdventureWorks  
           FROM DISK='C:\AdventureWorks.bak'  
           WITH NORECOVERY,   
              MOVE 'AdventureWorks_Data' TO   
                 'D:\Program Files\Microsoft SQL Server\MSSQL.n\MSSQL\Data\AdventureWorks_Data.mdf',   
              MOVE 'AdventureWorks_Log' TO  
                 'D:\Program Files\Microsoft SQL Server\MSSQL.n\MSSQL\Data\AdventureWorks_Log.ldf';  
        GO  
        ```  
  
5.  Dopo aver creato il backup completo, è necessario creare un backup del log nel database principale. Ad esempio, l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente esegue il backup del log nello stesso file utilizzato dal precedente backup completo:  
  
    ```  
    BACKUP LOG AdventureWorks   
        TO DISK = 'C:\AdventureWorks.bak'   
    GO  
    ```  
  
6.  Prima di avviare il mirroring, è necessario applicare il backup del log richiesto ed eventuali backup del log successivi.  
  
     Ad esempio, l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente ripristina il primo log da `C:\AdventureWorks.bak`:  
  
    ```  
    RESTORE LOG AdventureWorks   
        FROM DISK = 'C:\AdventureWorks.bak'   
        WITH FILE=1, NORECOVERY  
    GO  
    ```  
  
7.  Se vengono eseguiti altri backup del log prima dell'avvio del mirroring, è necessario ripristinare anche tali backup, in sequenza, nel server mirror tramite WITH NORECOVERY.  
  
     Ad esempio, l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente ripristina due log aggiuntivi da `C:\AdventureWorks.bak`:  
  
    ```  
    RESTORE LOG AdventureWorks   
        FROM DISK = 'C:\AdventureWorks.bak'   
        WITH FILE=2, NORECOVERY  
    GO  
    RESTORE LOG AdventureWorks   
        FROM DISK = 'C:\AdventureWorks.bak'   
        WITH FILE=3, NORECOVERY  
    GO  
    ```  
  
 Per un esempio completo di impostazione del mirroring del database, con le impostazioni relative alla sicurezza e ai partner, nonché l'aggiunta di un server di controllo del mirroring, vedere [Impostazione del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md).  
  
##  <a name="follow-up-after-preparing-a-mirror-database"></a><a name="FollowUp"></a> Completamento: Dopo la preparazione di un database mirror  
  
1.  Se è stato eseguito un backup del log aggiuntivo dopo l'ultima operazione RESTORE LOG, è necessario applicare manualmente ogni backup del log aggiuntivo, utilizzando RESTORE WITH NORECOVERY.  
  
2.  Avviare la sessione di mirroring. Per altre informazioni, vedere [Stabilire una sessione di mirroring del database tramite autenticazione di Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md) o [Stabilire una sessione di mirroring del database tramite autenticazione di Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md).  
  
3.  Se è stato disabilitato il processo di backup sul database principale, riabilitare il processo.  
  
4.  Se il database deve risultare attendibile dopo un failover, è necessario eseguire passaggi di configurazione aggiuntivi dopo l'avvio del mirroring. Per altre informazioni, vedere [Impostare un database mirror per l'uso della proprietà Trustworthy &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Creazione di un backup completo del database &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [Ripristinare un backup del log delle transazioni &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)  
  
-   [Stabilire una sessione di mirroring del database tramite autenticazione di Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
-   [Stabilire una sessione di mirroring del database tramite autenticazione di Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)  
  
-   [Impostazione di un database mirror crittografato](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)  
  
-   [Impostazione di un database mirror per l'utilizzo della proprietà Trustworthy &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Sicurezza trasporto per il mirroring del database e i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Impostazione del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [Backup e ripristino di indici e cataloghi full-text](../../relational-databases/search/back-up-and-restore-full-text-catalogs-and-indexes.md)   
 [Mirroring di database e cataloghi full-text &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-full-text-catalogs-sql-server.md)   
 [Mirroring e replica del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Argomenti RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md)  
  
  

