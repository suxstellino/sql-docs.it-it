---
title: 'Ripristinare il database: differenziale'
description: Questo articolo descrive come ripristinare un backup differenziale del database in SQL Server usando SQL Server Management Studio o Transact-SQL.
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- full differential backups [SQL Server]
- restoring databases [SQL Server], full differential backups
- database backups [SQL Server], full differential backups
- database restores [SQL Server], full differential backups
- backing up databases [SQL Server], full differential backups
ms.assetid: 0dd971a4-ee38-4dd3-9f30-ef77fc58dd11
author: cawrites
ms.author: chadam
ms.openlocfilehash: 028e5b2d1f26da698edae325b003fb402f721ee1
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99075843"
---
# <a name="restore-a-differential-database-backup-sql-server"></a>Ripristinare un backup differenziale di database (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In questo argomento viene descritto il ripristino di un backup differenziale del database in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Prerequisiti](#Prerequisites)  
  
     [Sicurezza](#Security)  
  
-   **Per ripristinare un backup differenziale di database utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   [Attività correlate](#RelatedTasks)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   Non è possibile utilizzare RESTORE in una transazione esplicita o implicita.  
  
-   I backup creati nella versione più recente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non possono essere ripristinati nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   In [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]è possibile ripristinare un database utente dal backup di un database creato tramite [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] o una versione successiva.  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Prerequisiti  
  
-   Nel modello di recupero con registrazione completa o con registrazione minima delle operazioni bulk, prima di poter ripristinare un database, è necessario effettuare il backup del log delle transazioni attivo, noto come parte finale del log. Per altre informazioni, vedere [Backup di un log delle transazioni &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md).  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Se il database da ripristinare non esiste, per eseguire un'operazione RESTORE l'utente deve disporre delle autorizzazioni CREATE DATABASE. Se il database esiste, le autorizzazioni per l'istruzione RESTORE vengono assegnate per impostazione predefinita ai membri dei ruoli predefiniti del server **sysadmin** e **dbcreator** e al proprietario (**dbo**) del database. Per l'opzione FROM DATABASE_SNAPSHOT, il database esiste sempre.  
  
 Le autorizzazioni per l'istruzione RESTORE vengono assegnate ai ruoli in cui le informazioni sull'appartenenza sono sempre disponibili per il server. Poiché è possibile controllare l'appartenenza ai ruoli predefiniti del database solo quando il database è accessibile e non è danneggiato, condizioni che non risultano sempre vere quando si esegue un'operazione RESTORE, i membri del ruolo predefinito del database **db_owner** non dispongono delle autorizzazioni per l'istruzione RESTORE.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-restore-a-differential-database-backup"></a>Per ripristinare un backup differenziale del database  
  
1.  Dopo aver stabilito la connessione all'istanza appropriata di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], in Esplora oggetti fare clic sul nome del server per espanderne l'albero.  
  
2.  Espandere **Database**. A seconda del database, selezionare un database utente oppure espandere **Database di sistema** e selezionare un database di sistema.  
  
3.  Fare clic con il pulsante destro del mouse sul database, scegliere **Attività**, selezionare **Ripristina**, quindi fare clic su **Database**.  
  
4.  Per specificare l'origine e il percorso dei set di backup da ripristinare, nella pagina **Generale** , utilizzare la sezione **Origine** . Selezionare una delle opzioni seguenti:  
  
    -   **Database**  
  
         Selezionare il database da ripristinare dall'elenco a discesa. Nell'elenco sono inclusi solo i database di cui è stato eseguito il backup in base alla cronologia dei backup di **msdb** .  
  
    > [!NOTE]  
    >  Se il backup viene eseguito da un server diverso, il server di destinazione non disporrà delle informazioni della cronologia di backup per il database specificato. In questo caso, selezionare **Dispositivo** per specificare manualmente il file o il dispositivo da ripristinare.  
  
    -   **Dispositivo**  
  
         Fare clic sul pulsante Sfoglia ( **...** ) per aprire la finestra di dialogo **Seleziona dispositivi di backup** . Nella casella **Tipi di supporti di backup** selezionare uno dei tipi di dispositivi elencati. Per selezionare uno o più dispositivi per la casella **Supporti di backup** , fare clic su **Aggiungi**.  
  
         Dopo avere aggiunto i dispositivi desiderati nella casella di riepilogo **Dispositivi di backup** , fare clic su **OK** per tornare alla pagina **Generale** .  
  
         Nella casella di riepilogo **Origine: Dispositivo: Database** selezionare il nome del database da ripristinare.  
  
         **Nota** Questo elenco è disponibile solo se si seleziona **Dispositivo** . Saranno disponibili solo i database che dispongono di backup sul dispositivo selezionato.  
  
5.  Nella sezione **Destinazione** , la casella **Database** viene popolata automaticamente con il nome del database da ripristinare. Per modificare il nome del database, immettere il nome nuovo nella casella **Database** .  
  
    > [!NOTE]  
    >  Per arrestare il ripristino in una data e un'ora diverse, fare clic su **Sequenza temporale** per accedere alla finestra di dialogo **Cronologia di backup** . Per informazioni sull'arresto del ripristino di un database in un momento specifico, vedere [Ripristino di un database di SQL Server fino a un punto specifico all'interno di un backup &#40;modello di recupero con registrazione completa&#41;](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md).  
  
6.  Nella griglia **Selezionare i set di backup da ripristinare** selezionare i backup che si desidera ripristinare dal backup differenziale.  
  
     Per informazioni sulle colonne nella griglia **Selezionare i set di backup da ripristinare** , vedere [Ripristina database &#40;pagina Generale&#41;](../../relational-databases/backup-restore/restore-database-general-page.md).  
  
7.  Nella pagina **Opzioni** del pannello **Opzioni di ripristino** è possibile selezionare una qualsiasi delle opzioni seguenti, in base alla situazione:  
  
    -   **Sovrascrivi il database esistente (WITH REPLACE)**  
  
    -   **Mantieni le impostazioni di replica (WITH KEEP_REPLICATION)**  
  
    -   **Chiedi conferma prima del ripristino di ogni backup**  
  
    -   **Limita accesso al database ripristinato (WITH RESTRICTED_USER)**  
  
     Per altre informazioni su queste opzioni, vedere [Ripristina database &#40;(pagina Opzioni)&#41;](../../relational-databases/backup-restore/restore-database-options-page.md).  
  
8.  Selezionare un'opzione per la casella **Stato di recupero** . Questa casella determina lo stato del database al termine dell'operazione di ripristino.  
  
    -   **RESTORE WITH RECOVERY** è il comportamento predefinito che lascia il database pronto per l'utilizzo mediante il rollback delle transazioni di cui non è stato eseguito il commit. I log delle transazioni aggiuntivi non possono essere ripristinati. Selezionare questa opzione se si desidera ripristinare subito tutti i backup necessari.  
  
    -   **RESTORE WITH NORECOVERY** lascia il database non operativo e non esegue il rollback delle transazioni di cui non è stato eseguito il commit. I log delle transazioni aggiuntivi possono essere ripristinati. Non è possibile utilizzare il database fino al completamento del recupero.  
  
    -   **RESTORE WITH STANDBY** lascia il database in modalità di sola lettura. Annulla le transazioni di cui non è stato eseguito il commit, ma salva le azioni di rollback in un file standby in modo che gli effetti del recupero possano essere annullati.  
  
     Per una descrizione delle opzioni, vedere [Ripristina database &#40;pagina Opzioni&#41;](../../relational-databases/backup-restore/restore-database-options-page.md).  
  
9. Le operazioni di ripristino non riescono in presenza di connessioni attive al database. Selezionare l'opzione **Chiudi connessioni esistenti** per garantire che tutte le connessioni attive tra [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] e il database vengano chiuse.  
  
10. Selezionare **Chiedi conferma prima del ripristino di ogni backup** se si desidera ricevere una richiesta di conferma prima di ciascuna operazione di ripristino. L'operazione non è normalmente necessaria, a meno che le dimensioni del database siano elevate e si desideri monitorare lo stato dell'operazione di ripristino.  
  
11. Facoltativamente, utilizzare la pagina **File** per ripristinare il database in un nuovo percorso. Per informazioni su come spostare un database, vedere [Ripristino di un database in una nuova posizione &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-database-to-a-new-location-sql-server.md).  
  
12. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-restore-a-differential-database-backup"></a>Per ripristinare un backup differenziale del database  
  
1.  Eseguire l'istruzione RESTORE DATABASE, specificando la clausola NORECOVERY, per ripristinare il backup completo del database precedente al backup differenziale del database. Per altre informazioni, vedere [Procedura: Ripristinare un backup completo](../../relational-databases/backup-restore/restore-a-database-backup-under-the-simple-recovery-model-transact-sql.md).  
  
2.  Eseguire l'istruzione RESTORE DATABASE per ripristinare il backup differenziale del database, specificando:  
  
    -   Il nome del database a cui si riferisce il backup differenziale del database.  
  
    -   Il dispositivo di backup da cui viene ripristinato il backup differenziale del database.  
  
    -   La clausola NORECOVERY, se sono presenti backup del log delle transazioni da applicare dopo il ripristino del backup differenziale del database. In caso contrario, specificare la clausola RECOVERY.  
  
3.  Con il modello di recupero con registrazione completa o con registrazione minima delle operazioni bulk, tramite il ripristino di un backup differenziale del database viene ripristinato il database fino al punto in cui è stato completato il backup differenziale del database. Per eseguire il recupero fino al momento dell'errore, applicare tutti i backup del log delle transazioni creati dopo la creazione dell'ultimo backup differenziale del database. Per altre informazioni, vedere [Applicazione dei backup di log delle transazioni &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md).  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Esempi (Transact-SQL)  
  
#### <a name="a-restoring-a-differential-database-backup"></a>R. Ripristino di un backup differenziale del database  
 Nell'esempio seguente vengono ripristinati un backup del database e un backup differenziale del database `MyAdvWorks` .  
  
```sql  
-- Assume the database is lost, and restore full database,   
-- specifying the original full database backup and NORECOVERY,   
-- which allows subsequent restore operations to proceed.  
RESTORE DATABASE MyAdvWorks  
   FROM MyAdvWorks_1  
   WITH NORECOVERY;  
GO  
-- Now restore the differential database backup, the second backup on   
-- the MyAdvWorks_1 backup device.  
RESTORE DATABASE MyAdvWorks  
   FROM MyAdvWorks_1  
   WITH FILE = 2,  
   RECOVERY;  
GO  
```  
  
#### <a name="b-restoring-a-database-differential-database-and-transaction-log-backup"></a>B. Ripristino di un backup del database, di un backup differenziale del database e di un backup del log delle transazioni  
 In questo esempio vengono ripristinati un backup, un backup differenziale e un backup del log delle transazioni del database `MyAdvWorks` .  
  
```sql  
-- Assume the database is lost at this point. Now restore the full   
-- database. Specify the original full database backup and NORECOVERY.  
-- NORECOVERY allows subsequent restore operations to proceed.  
RESTORE DATABASE MyAdvWorks  
   FROM MyAdvWorks_1  
   WITH NORECOVERY;  
GO  
-- Now restore the differential database backup, the second backup on   
-- the MyAdvWorks_1 backup device.  
RESTORE DATABASE MyAdvWorks  
   FROM MyAdvWorks_1  
   WITH FILE = 2,  
   NORECOVERY;  
GO  
-- Now restore each transaction log backup created after  
-- the differential database backup.  
RESTORE LOG MyAdvWorks  
   FROM MyAdvWorks_log1  
   WITH NORECOVERY;  
GO  
RESTORE LOG MyAdvWorks  
   FROM MyAdvWorks_log2  
   WITH RECOVERY;  
GO  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Creare un backup differenziale del database &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md)  
  
-   [Ripristinare un backup del log delle transazioni &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Backup differenziali &#40;SQL Server&#41;](../../relational-databases/backup-restore/differential-backups-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
  
