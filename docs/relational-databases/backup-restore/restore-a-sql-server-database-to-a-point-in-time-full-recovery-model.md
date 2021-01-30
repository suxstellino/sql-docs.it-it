---
description: Ripristino di un database di SQL Server fino a un punto specifico all'interno di un backup (modello di recupero con registrazione completa)
title: Ripristinare un database di SQL Server fino a un punto specifico (modello di recupero con registrazione completa) | Microsoft Docs
decription: Learn how to restore a database to a point in time in SQL Server using SQL Server Management Studio or Transact-SQL in the full or bulk-logged recovery models.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- STOPAT clause [RESTORE LOG statement]
- point in time recovery [SQL Server]
- restoring databases [SQL Server], point in time
ms.assetid: 3a5daefd-08a8-4565-b54f-28ad01a47d32
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4038ba678af8635f3ac2bd12f68c818a61e2c87f
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99075634"
---
# <a name="restore-a-sql-server-database-to-a-point-in-time-full-recovery-model"></a>Ripristino di un database di SQL Server fino a un punto specifico all'interno di un backup (modello di recupero con registrazione completa)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In questo argomento viene descritto il ripristino temporizzato di un database [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante l'utilizzo di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Le informazioni contenute in questo argomento sono rilevanti solo per i database [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui viene utilizzato il modello di recupero con registrazione completa o con registrazione minima delle operazioni bulk.  
  
> [!IMPORTANT]  
>  Nel modello di recupero con registrazione minima delle operazioni bulk se un backup del log contiene modifiche con registrazione minima delle operazioni bulk, il recupero temporizzato non è possibile rispetto a un punto all'interno di tale backup. È necessario recuperare il database fino alla fine del backup del log delle transazioni.  
  
-   **Prima di iniziare:**  
  
     [Indicazioni](#Recommendations)  
  
     [Sicurezza](#Security)  
  
-   **Per ripristinare un database di SQL Server in un punto nel tempo mediante:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni  
  
-   Utilizzo dell'opzione STANDBY per la ricerca di un punto sconosciuto nel tempo  
  
-   Impostazione del punto nel tempo all'inizio di una sequenza di ripristino  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Se il database da ripristinare non esiste, per eseguire un'operazione RESTORE l'utente deve disporre delle autorizzazioni CREATE DATABASE. Se il database esiste, le autorizzazioni per l'istruzione RESTORE vengono assegnate per impostazione predefinita ai membri dei ruoli predefiniti del server **sysadmin** e **dbcreator** e al proprietario (**dbo**) del database. Per l'opzione FROM DATABASE_SNAPSHOT, il database esiste sempre.  
  
 Le autorizzazioni per l'istruzione RESTORE vengono assegnate ai ruoli in cui le informazioni sull'appartenenza sono sempre disponibili per il server. Poiché è possibile controllare l'appartenenza ai ruoli predefiniti del database solo quando il database è accessibile e non è danneggiato, condizioni che non risultano sempre vere quando si esegue un'operazione RESTORE, i membri del ruolo predefinito del database **db_owner** non dispongono delle autorizzazioni per l'istruzione RESTORE.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
 **Per ripristinare un database fino a un punto nel tempo**  
  
1.  Stabilire una connessione all'istanza appropriata di [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], quindi espandere l'albero di server in Esplora oggetti.  
  
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
  
6.  Fare clic su **Sequenza temporale** per accedere alla finestra di dialogo **Sequenza temporale backup** .  
  
7.  Nella sezione **Ripristina fino a** fare clic su **Data e ora specifiche**.  
  
8.  Utilizzare le caselle **Data** e **Ora** o la barra del dispositivo di scorrimento per specificare una data e ora specifiche per l'arresto del ripristino. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
    > [!NOTE]  
    >  Usare la casella **Intervallo sequenza temporale** per modificare la quantità di tempo visualizzata nella sequenza temporale.  
  
9. Dopo aver specificato una determinata temporizzazione, tramite Database Recovery Advisor viene assicurato che nella colonna **Ripristina** della griglia **Set di backup da ripristinare** vengano selezionati solo i backup necessari per il ripristino fino a quella temporizzazione. Questi backup selezionati costituiscono il piano di ripristino consigliato per il ripristino temporizzato. Per l'operazione di ripristino temporizzato è consigliabile utilizzare solo i backup selezionati.  
  
     Per informazioni sulle colonne nella griglia **Selezionare i set di backup da ripristinare** , vedere [Ripristina database &#40;pagina Generale&#41;](../../relational-databases/backup-restore/restore-database-general-page.md). Per informazioni su Database Recovery Advisor, vedere [Panoramica del ripristino e del recupero &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md).  
  
10. Nella pagina **Opzioni** del pannello **Opzioni di ripristino** è possibile selezionare una qualsiasi delle opzioni seguenti, in base alla situazione:  
  
    -   **Sovrascrivi il database esistente (WITH REPLACE)**  
  
    -   **Mantieni le impostazioni di replica (WITH KEEP_REPLICATION)**  
  
    -   **Limita accesso al database ripristinato (WITH RESTRICTED_USER)**  
  
     Per altre informazioni su queste opzioni, vedere [Ripristina database &#40;(pagina Opzioni)&#41;](../../relational-databases/backup-restore/restore-database-options-page.md).  
  
11. Selezionare un'opzione per la casella **Stato di recupero** . Questa casella determina lo stato del database al termine dell'operazione di ripristino.  
  
    -   **RESTORE WITH RECOVERY** è il comportamento predefinito che lascia il database pronto per l'utilizzo mediante il rollback delle transazioni di cui non è stato eseguito il commit. I log delle transazioni aggiuntivi non possono essere ripristinati. Selezionare questa opzione se si desidera ripristinare subito tutti i backup necessari.  
  
    -   **RESTORE WITH NORECOVERY** lascia il database non operativo e non esegue il rollback delle transazioni di cui non è stato eseguito il commit. I log delle transazioni aggiuntivi possono essere ripristinati. Non è possibile utilizzare il database fino al completamento del recupero.  
  
    -   **RESTORE WITH STANDBY** lascia il database in modalità di sola lettura. Annulla le transazioni di cui non è stato eseguito il commit, ma salva le azioni di rollback in un file standby in modo che gli effetti del recupero possano essere annullati.  
  
     Per una descrizione delle opzioni, vedere [Ripristina database &#40;pagina Opzioni&#41;](../../relational-databases/backup-restore/restore-database-options-page.md).  
  
12. **Esegui il backup della parte finale del log prima del ripristino** verrà selezionato se necessario per il punto nel tempo selezionato. Non è necessario modificare questa impostazione, ma è possibile scegliere di eseguire il backup della parte finale del log, anche se non è richiesto.  
  
13. Le operazioni di ripristino potrebbero non riuscire in presenza di connessioni attive al database. Selezionare l'opzione **Chiudi connessioni esistenti** per garantire che tutte le connessioni attive tra [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] e il database vengano chiuse. Questa casella di controllo imposta il database sulla modalità utente singolo prima di effettuare qualsiasi operazione di ripristino e imposta il database sulla modalità multiutente al termine.  
  
14. Selezionare **Chiedi conferma prima del ripristino di ogni backup** se si desidera ricevere una richiesta di conferma prima di ciascuna operazione di ripristino. L'operazione non è normalmente necessaria, a meno che le dimensioni del database siano elevate e si desideri monitorare lo stato dell'operazione di ripristino.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Con Transact-SQL  
 **Prima di iniziare**  
  
 Un momento specifico viene sempre ripristinato da un backup del log. In ogni istruzione RESTORE LOG della sequenza di ripristino, è necessario specificare l'ora o la transazione di destinazione in una clausola STOPAT identica. Come prerequisito per un ripristino temporizzato, è necessario innanzitutto ripristinare un backup completo del database il cui endpoint sia precedente rispetto al momento di ripristino di destinazione. Il backup completo del database può essere precedente rispetto al backup completo del database più recente purché vengano ripristinati tutti i backup del log successivi, fino al backup del log contenente la data e ora specifica di destinazione compreso.  
  
 Per consentire l'identificazione del backup del database da ripristinare, è possibile specificare la clausola WITH STOPAT nell'istruzione RESTORE DATABASE per generare un errore se un backup dei dati è troppo recente rispetto alla data e ora specifica di destinazione. Il backup completo dei dati viene sempre ripristinato, anche se contiene la data e ora specifica di destinazione.  
  
 **Sintassi [!INCLUDE[tsql](../../includes/tsql-md.md)] di base**  
  
 RESTORE LOG *nome_database* FROM <dispostivo_backup> WITH STOPAT **=** _ora_ **,** RECOVERY...  
  
 Il punto di recupero è l'ultimo commit delle transazioni eseguito in corrispondenza o prima del valore **datetime** specificato da *time*.  
  
 Per ripristinare solo le modifiche apportate prima di determinato momento, specificare WITH STOPAT **=** _ora_ per ogni backup da ripristinare. Questo garantisce che il momento nel tempo desiderato non venga superato.  
  
 **Per ripristinare un database fino a un punto nel tempo**  
  
> [!NOTE]  
>  Per un esempio di questa procedura, vedere [Esempio (Transact-SQL)](#TsqlExample)più avanti in questa sezione.  
  
1.  Connettersi all'istanza del server in cui si desidera ripristinare il database.  
  
2.  Eseguire l'istruzione RESTORE DATABASE utilizzando l'opzione NORECOVERY.  
  
    > [!NOTE]  
    >  Se una sequenza di ripristino parziale esclude qualsiasi filegroup [FILESTREAM](../../relational-databases/blob/filestream-sql-server.md) , il ripristino temporizzato non è supportato. È possibile forzare la continuazione della sequenza di ripristino. Tuttavia i filegroup FILESTREAM omessi dall'istruzione RESTORE non potranno mai più essere ripristinati. Per forzare un ripristino temporizzato, specificare l'opzione CONTINUE_AFTER_ERROR insieme all'opzione STOPAT, STOPATMARK o STOPBEFOREMARK che è necessario specificare anche nelle istruzioni RESTORE LOG successive. Se si specifica CONTINUE_AFTER_ERROR, la sequenza di ripristino parziale ha esito positivo e il filegroup FILESTREAM non può più essere recuperato.  
  
3.  Ripristinare l'ultimo backup del database differenziale, se presente, senza recuperare il database (RESTORE DATABASE *database_name* FROM *backup_device* WITH NORECOVERY).  
  
4.  Applicare ogni backup del log delle transazioni nella stessa sequenza di creazione, specificando l'ora in cui si intende arrestare il ripristino del log (RESTORE DATABASE *database_name* FROM <backup_device> WITH STOPAT **=**_time_**,** RECOVERY).  
  
    > [!NOTE]  
    >  Le opzioni RECOVERY e STOPAT. Se il backup del log delle transazioni non contiene i dati corrispondenti all'ora richiesta, ad esempio se l'ora specificata è successiva al periodo di tempo gestito dal log delle transazioni, viene generato un messaggio di avviso e il database non viene recuperato.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Esempio (Transact-SQL)  
 Nell'esempio seguente viene ripristinato lo stato del database corrispondente alle ore `12:00 AM` del giorno `April 15, 2020` e viene illustrata un'operazione di ripristino di più backup del log. Nel dispositivo di backup, `AdventureWorksBackups`, il backup completo del database da ripristinare è il terzo set di backup (`FILE = 3`), il primo backup del log è il quarto set di backup (`FILE = 4`) e il secondo backup del log è il quinto set di backup (`FILE = 5`).  
  
> [!IMPORTANT]  
>  Il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] utilizza il modello di recupero con registrazione minima. Per consentire i backup del log, prima di eseguire un backup completo del database, il database viene impostato per l'utilizzo del modello di recupero con registrazione completa tramite `ALTER DATABASE AdventureWorks SET RECOVERY FULL`.  
  
```  
RESTORE DATABASE AdventureWorks  
   FROM AdventureWorksBackups  
   WITH FILE=3, NORECOVERY;  
  
RESTORE LOG AdventureWorks  
   FROM AdventureWorksBackups  
   WITH FILE=4, NORECOVERY, STOPAT = 'Apr 15, 2020 12:00 AM';  
  
RESTORE LOG AdventureWorks  
   FROM AdventureWorksBackups  
   WITH FILE=5, NORECOVERY, STOPAT = 'Apr 15, 2020 12:00 AM';  
RESTORE DATABASE AdventureWorks WITH RECOVERY;   
GO  
  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Ripristinare un backup del database con SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
-   [Eseguire il backup di un log delle transazioni &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
-   [Ripristinare un database fino al punto di errore nel modello di recupero con registrazione completa &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/restore-database-to-point-of-failure-full-recovery.md)  
  
-   [Ripristinare un database fino a una transazione contrassegnata &#40;SQL Server Management Studio&#41;](../../relational-databases/backup-restore/restore-a-database-to-a-marked-transaction-sql-server-management-studio.md)  
  
-   [Recupero fino a un numero di sequenza del file di log &#40;SQL Server&#41;](../../relational-databases/backup-restore/recover-to-a-log-sequence-number-sql-server.md)  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore.ToPointInTime%2A> (SMO)  
  
## <a name="see-also"></a>Vedere anche  
 [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)  
  
  
