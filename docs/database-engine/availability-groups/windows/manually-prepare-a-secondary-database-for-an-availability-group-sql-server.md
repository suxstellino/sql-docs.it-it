---
title: Preparare un database secondario per un gruppo di disponibilità
description: Descrizione di come si prepara manualmente un database secondario per creare un join con un gruppo di disponibilità Always On.
ms.custom: seodec18
ms.date: 07/25/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.preparedbs.f1
- sql13.swb.availabilitygroup.configsecondarydbs.f1
helpviewer_keywords:
- secondary databases [SQL Server], in availability group
- secondary databases [SQL Server]
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], databases
ms.assetid: 9f2feb3c-ea9b-4992-8202-2aeed4f9a6dd
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3a87c7b4dc37a7b1a70b31b9d7b9191a3570db98
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98764753"
---
# <a name="prepare-a-secondary-database-for-an-always-on-availability-group"></a>Preparare un database secondario per un gruppo di disponibilità Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
Questo argomento illustra come preparare un database per un gruppo di disponibilità Always On in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] usando [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)] o PowerShell. La preparazione di un database richiede due passaggi: 

1. Ripristinare un back recente del database primario e backup di log successivi in ogni istanza del server in cui viene ospitata la replica secondaria, usando RESTORE WITH NORECOVERY
2. Creare un join del database ripristinato al gruppo di disponibilità.  
  
> [!TIP]  
>  Se si dispone di una configurazione per il log shipping esistente, è possibile convertire il database primario per il log shipping insieme a uno o più dei relativi database secondari in una replica primaria del gruppo di disponibilità e una o più delle relative repliche secondarie. Per altre informazioni, vedere [Prerequisiti per la migrazione dal log shipping ai gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-migrating-log-shipping-to-always-on-availability-groups.md).  

##  <a name="prerequisites-and-restrictions"></a><a name="Prerequisites"></a> Prerequisiti e restrizioni  
  
-   Verificare che nel sistema in cui si desidera collocare il database sia presente un'unità disco con spazio sufficiente per i database secondari.  
  
-   Il nome del database secondario deve essere lo stesso del database primario.  
  
-   Utilizzare RESTORE WITH NORECOVERY per ogni operazione di ripristino.  
  
-   Se il database secondario deve risiedere in un percorso di file diverso (inclusa la lettera dell'unità) dal database primario, è inoltre necessario utilizzare l'opzione WITH MOVE nel comando Restore per ognuno dei file di database per specificare il percorso del database secondario.  
  
-   Se si ripristina il database un filegroup alla volta, prestare attenzione a ripristinare l'intero database.  
  
-   Dopo il ripristino del database, è necessario ripristinare (WITH NORECOVERY) ogni backup del log creato dall'ultimo backup dei dati ripristinato.  
  
##  <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni  
  
-   Nelle istanze autonome di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]è consigliabile che il percorso del file di un determinato database secondario, inclusa la lettera di unità, sia se possibile identico a quello del database primario corrispondente. Se durante la creazione di un database secondario i file del database vengono spostati, infatti, potrebbe essere impossibile aggiungere successivamente file al database secondario senza sospendere il database secondario.  
  
-   Prima di preparare i database secondari, si consiglia di sospendere i backup del log pianificati sui database nel gruppo di disponibilità finché non viene completata l'inizializzazione delle repliche secondarie.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
 Quando viene eseguito il backup di un database, la [proprietà TRUSTWORTHY del database](../../../relational-databases/security/trustworthy-database-property.md) viene impostata su OFF. Di conseguenza, la proprietà TRUSTWORTHY è sempre impostata su OFF in un database appena ripristinato.  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Le autorizzazioni BACKUP DATABASE e BACKUP LOG vengono assegnate per impostazione predefinita ai membri del ruolo predefinito del server **sysadmin** e dei ruoli predefiniti del database **db_owner** e **db_backupoperator** . Per altre informazioni, vedere [BACKUP &#40;Transact-SQL&#41;](../../../t-sql/statements/backup-transact-sql.md).  
  
 Se il database da ripristinare non esiste nell'istanza del server, l'istruzione RESTORE richiede autorizzazioni CREATE DATABASE. Per altre informazioni, vedere [RESTORE &#40;Transact-SQL&#41;](../../../t-sql/statements/restore-statements-transact-sql.md).  
  
##  <a name="use-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
> [!NOTE]  
>  Se i percorsi dei file di backup e ripristino sono identici nell'istanza del server che ospita una replica primaria e in ogni istanza che ospita una replica secondaria, è possibile creare database di replica seocondari usando la [Creazione guidata Gruppo di disponibilità](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md), la [procedura guidata Aggiungi replica a gruppo di disponibilità](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)o la [procedura guidata Aggiungi database a gruppo di disponibilità](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md).  
  
 **Per preparare un database secondario**  
  
1.  A meno che non si disponga già di un backup recente del database primario, creare un nuovo backup del database completo o differenziale. Secondo la procedura consigliata, collocare il backup ed eventuali backup del log successivi nella condivisione di rete consigliata.  
  
2.  Creare almeno un nuovo backup del log del database primario.

   >[!NOTE]
   >Un backup del log delle transazioni potrebbe non essere necessario se non è stato acquisito in precedenza nel database nella replica primaria. È consigliabile effettuare un backup del log delle transazioni ogni volta che un nuovo database viene unito in join al gruppo di disponibilità. 
  
3.  Nell'istanza del server che ospita la replica secondaria, ripristinare il backup completo del database primario (e facoltativamente un backup differenziale) seguito da eventuali backup del log successivi.  
  
     Nella pagina **Opzioni di RESTORE DATABASE**, selezionare **Lascia il database non operativo e non eseguire il rollback delle transazioni di cui non è stato eseguito il commit. I log delle transazioni aggiuntivi possono essere ripristinati. (RESTORE WITH NORECOVERY)** .  
  
     Se i percorsi dei file del database primario e del database secondario sono diversi, ad esempio se il database primario si trova nell'unità "F:" ma nell'istanza del server che ospita la replica secondaria non è disponibile un'unità "F:", includere l'opzione MOVE nella clausola WITH.  
  
4.  Per completare la configurazione del database secondario, è necessario creare un join del database secondario al gruppo di disponibilità. Per altre informazioni, vedere [Creare un join di un database secondario a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md).  
  
> [!NOTE]  
>  Per informazioni sull'esecuzione di queste operazioni di backup e ripristino, vedere [Attività correlate a backup e ripristino](#RelatedTasks), più avanti in questa sezione.  
  
###  <a name="related-backup-and-restore-tasks"></a><a name="RelatedTasks"></a> Attività correlate a backup e ripristino  
 **Per creare un backup del database**  
  
-   [Creazione di un backup completo del database &#40;SQL Server&#41;](../../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [Creare un backup differenziale del database &#40;SQL Server&#41;](../../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md)  
  
 **Per creare un backup del log**  
  
-   [Eseguire il backup di un log delle transazioni &#40;SQL Server&#41;](../../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
 **Per ripristinare i backup**  
  
-   [Ripristinare un backup del database con SSMS](../../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
-   [Ripristinare un backup differenziale del database &#40;SQL Server&#41;](../../../relational-databases/backup-restore/restore-a-differential-database-backup-sql-server.md)  
  
-   [Ripristinare un backup del log delle transazioni &#40;SQL Server&#41;](../../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)  
  
-   [Ripristinare un database in una nuova posizione &#40;SQL Server&#41;](../../../relational-databases/backup-restore/restore-a-database-to-a-new-location-sql-server.md)  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
 **Per preparare un database secondario**  
  
> [!NOTE]  
>  Per un esempio di questa procedura, vedere [Esempio (Transact-SQL)](#ExampleTsql), più indietro in questo argomento.  
  
1.  A meno che si disponga di un backup completo recente del database primario, connettersi all'istanza del server che ospita la replica primaria e creare un backup completo del database. Secondo la procedura consigliata, collocare il backup ed eventuali backup del log successivi nella condivisione di rete consigliata.  
  
2.  Nell'istanza del server che ospita la replica secondaria, ripristinare il backup completo del database primario (e facoltativamente un backup differenziale) seguito da tutti i backup del log successivi. Utilizzare WITH NORECOVERY per ogni operazione di ripristino.  
  
     Se i percorsi dei file del database primario e del database secondario sono diversi, ad esempio se il database primario si trova nell'unità "F:" ma nell'istanza del server che ospita la replica secondaria non è disponibile un'unità "F:", includere l'opzione MOVE nella clausola WITH.  
  
3.  Se sono stati eseguiti altri backup del log sul database primario dopo il backup del log richiesto, è inoltre necessario copiarli nell'istanza del server che ospita la replica secondaria e applicare ognuno di questi backup del log al database secondario, a partire dal meno recente e utilizzando sempre RESTORE WITH NORECOVERY.  
  
    > [!NOTE]  
    >  Il backup del log non esiste se il database primario è stato appena creato e non è ancora stato eseguito alcun backup del log oppure se il modello di recupero è stato appena modificato da SIMPLE a FULL.  
  
4.  Per completare la configurazione del database secondario, è necessario creare un join del database secondario al gruppo di disponibilità. Per altre informazioni, vedere [Creare un join di un database secondario a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md).  
  
> [!NOTE]  
>  Per informazioni sull'esecuzione di queste operazioni di backup e ripristino, vedere [Attività correlate a backup e ripristino](#RelatedTasks), più avanti in questo argomento.  
  
###  <a name="transact-sql-example"></a><a name="ExampleTsql"></a> Esempio Transact-SQL  
 Nell'esempio seguente viene preparato un database secondario. Nell'esempio viene utilizzato il database di esempio [!INCLUDE[ssSampleDBobject](../../../includes/sssampledbobject-md.md)] in cui, per impostazione predefinita, viene utilizzato il modello di recupero con registrazione minima.  
  
1.  Per utilizzare il database [!INCLUDE[ssSampleDBobject](../../../includes/sssampledbobject-md.md)] , modificarlo in modo da utilizzare il modello di recupero con registrazione completa:  
  
    ```  
    USE master;  
    GO  
    ALTER DATABASE MyDB1   
    SET RECOVERY FULL;  
    GO  
    ```  
  
2.  Dopo aver modificato il modello di recupero del database da SIMPLE a FULL, creare un backup completo da utilizzare per la creazione del database secondario. Dopo la modifica del modello di recupero, è consigliabile selezionare l'opzione WITH FORMAT per creare un nuovo set di supporti. L'operazione risulta utile per separare i backup eseguiti durante l'utilizzo del modello di recupero con registrazione completa dai backup precedenti eseguiti durante l'utilizzo del modello di recupero con registrazione semplice. Ai fini di questo esempio, il file di backup (C:\\[!INCLUDE[ssSampleDBobject](../../../includes/sssampledbobject-md.md)].bak) verrà creato nella stessa unità del database.  
  
    > [!NOTE]  
    >  Nel caso di un database di produzione, è consigliabile eseguire sempre il backup in un dispositivo distinto.  
  
     Nell'istanza del server che ospita la replica primaria (`INSTANCE01`), creare un backup completo del database primario, nel modo seguente:  
  
    ```  
    BACKUP DATABASE MyDB1   
        TO DISK = 'C:\MyDB1.bak'   
        WITH FORMAT  
    GO  
    ```  
  
3.  Copiare il backup completo nell'istanza del server in cui è ospitata la replica secondaria.  
  
4.  Ripristinare il backup completo nell'istanza del server che ospita la replica secondaria, utilizzando RESTORE WITH NORECOVERY. Il comando di ripristino dipende dal fatto che i percorsi del database primario e di quelli secondari siano identici.  
  
    -   **Se i percorsi sono identici:**  
  
         Nel computer che ospita la replica secondaria, ripristinare il backup completo nel modo seguente:  
  
        ```  
        RESTORE DATABASE MyDB1   
            FROM DISK = 'C:\MyDB1.bak'   
            WITH NORECOVERY  
        GO  
        ```  
  
    -   **Se i percorsi sono diversi:**  
  
         Se il percorso del database secondario è diverso dal percorso del database primario, ad esempio perché le lettere di unità non corrispondono, per creare il database secondario è necessario che l'operazione di ripristino includa una clausola MOVE.  
  
        > [!IMPORTANT]  
        >  Se il nome di percorso del database primario è diverso dal nome di percorso dei database secondari, non è possibile aggiungere un file. Alla ricezione del log relativo all'operazione di aggiunta del file, l'istanza del server della replica secondaria tenta infatti di salvare il nuovo file nello stesso percorso utilizzato dal database primario.  
  
         Ad esempio, tramite il comando seguente viene ripristinato un backup di un database primario che risiede nella directory di dati dell'istanza predefinita di [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)], C:\Programmi\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\DATA. Con l'operazione di ripristino il database verrà spostato nella directory dei dati di un'istanza remota di [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] denominata (*Always On1*) che ospita la replica secondaria su un altro nodo del cluster. In questo percorso, i file di dati e di log vengono ripristinati nella directory *C:\Programmi\Microsoft SQL Server\MSSQL13.On1\MSSQL\DATA* . Per l'operazione di ripristino viene utilizzata l'opzione WITH NORECOVERY per lasciare il database secondario nel database di ripristino.  
  
        ```  
        RESTORE DATABASE MyDB1  
          FROM DISK='C:\MyDB1.bak'  
         WITH NORECOVERY,   
            MOVE 'MyDB1_Data' TO   
             'C:\Program Files\Microsoft SQL Server\MSSQL13.Always On1\MSSQL\DATA\MyDB1_Data.mdf',   
            MOVE 'MyDB1_Log' TO  
             'C:\Program Files\Microsoft SQL Server\MSSQL13.Always On1\MSSQL\DATA\MyDB1_Data.ldf';  
        GO  
        ```  
  
5.  Dopo il ripristino del backup completo, è necessario creare un backup del log nel database primario. Ad esempio, l'istruzione [!INCLUDE[tsql](../../../includes/tsql-md.md)] seguente esegue il backup del log in un file di backup denominato *E:\MyDB1_log.trn*:  
  
    ```  
    BACKUP LOG MyDB1   
      TO DISK = 'E:\MyDB1_log.trn'   
    GO  
    ```  
  
6.  Prima di creare il join del database alla replica secondaria, è necessario applicare il backup del log richiesto ed eventuali backup del log successivi.  
  
     Ad esempio, l'istruzione [!INCLUDE[tsql](../../../includes/tsql-md.md)] seguente ripristina il primo log da *C:\MyDB1.trn*:  
  
    ```  
    RESTORE LOG MyDB1   
      FROM DISK = 'E:\MyDB1_log.trn'   
        WITH FILE=1, NORECOVERY  
    GO  
    ```  
  
7.  Se vengono eseguiti altri backup del log prima del join del database alla replica secondaria, è inoltre necessario ripristinare tutti questi backup, in sequenza, nell'istanza del server che ospita la replica secondaria utilizzando RESTORE WITH NORECOVERY.  
  
     Ad esempio, l'istruzione [!INCLUDE[tsql](../../../includes/tsql-md.md)] seguente ripristina altri due log da *E:\MyDB1_log.trn*:  
  
    ```  
    RESTORE LOG MyDB1   
      FROM DISK = 'E:\MyDB1_log.trn'   
        WITH FILE=2, NORECOVERY  
    GO  
    RESTORE LOG MyDB1   
      FROM DISK = 'E:\MyDB1_log.trn'   
        WITH FILE=3, NORECOVERY  
    GO  
    ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Utilizzo di PowerShell  
 **Per preparare un database secondario**  
  
1.  Se è necessario creare un backup recente del database primario, spostarsi nella directory (**cd**) dell'istanza del server che ospita la replica primaria.  
  
2.  Usare il cmdlet **Backup-SqlDatabase** per creare i singoli backup.  
  
3.  Spostarsi nella directory (**cd**) dell'istanza del server che ospita la replica secondaria.  
  
4.  Per ripristinare il database e i backup del log di ogni database primario, usare il cmdlet **restore-SqlDatabase** , specificando il parametro di ripristino **NoRecovery** . Se i percorsi di file differiscono tra i computer in cui sono ospitate la replica primaria e la replica secondaria di destinazione, usare anche il parametro di ripristino **RelocateFile** .  
  
    > [!NOTE]  
    >  Per visualizzare la sintassi di un cmdlet, usare il cmdlet **Get-Help** nell'ambiente PowerShell di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Per altre informazioni, vedere [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
5.  Per completare la configurazione del database secondario, è necessario creare un join dello stesso al gruppo di disponibilità. Per altre informazioni, vedere [Creare un join di un database secondario a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md).  
  
 **Per impostare e utilizzare il provider PowerShell per SQL Server**  
  
-   [Provider PowerShell per SQL Server](../../../powershell/sql-server-powershell-provider.md)  
  
###  <a name="sample-backup-and-restore-script-and-command"></a><a name="ExamplePSscript"></a> Script e comando di backup e ripristino di esempio  
 Tramite i comandi di PowerShell riportati di seguito viene eseguito il backup completo di un database e del log delle transazioni in una condivisione di rete e vengono ripristinati i backup dalla condivisione. In questo esempio si presuppone che il percorso del file in cui viene ripristinato il database corrisponda al percorso del file nel quale è stato eseguito il backup del database.  
  
```  
# Create database backup  
Backup-SqlDatabase -Database "MyDB1" -BackupFile "\\share\backups\MyDB1.bak" -ServerInstance "SourceMachine\Instance"  
# Create log backup  
Backup-SqlDatabase -Database "MyDB1" -BackupAction "Log" -BackupFile "\\share\backups\MyDB1.trn" -ServerInstance "SourceMachine\Instance"  
# Restore database backup   
Restore-SqlDatabase -Database "MyDB1" -BackupFile "\\share\backups\MyDB1.bak" -NoRecovery -ServerInstance "DestinationMachine\Instance"  
# Restore log backup   
Restore-SqlDatabase -Database "MyDB1" -BackupFile "\\share\backups\MyDB1.trn" -RestoreAction "Log" -NoRecovery -ServerInstance "DestinationMachine\Instance"  
  
```  
  
##  <a name="next-steps"></a><a name="FollowUp"></a> Passaggi successivi  
 Per completare la configurazione del database secondario, creare un join del database appena ripristinato al gruppo di disponibilità. Per altre informazioni, vedere [Creare un join di un database secondario a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../../t-sql/statements/backup-transact-sql.md)   
 [Argomenti dell'istruzione RESTORE &#40;Transact-SQL&#41;](../../../t-sql/statements/restore-statements-arguments-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../../t-sql/statements/restore-statements-transact-sql.md)   
 [Risolvere i problemi relativi a una operazione di aggiunta file non riuscita &#40;Gruppi di disponibilità Always On&#41;](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)  
  
